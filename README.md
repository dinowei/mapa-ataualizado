<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CosmoNews - Mapa Estelar Interativo</title>
    <script src="https://cdn.jsdelivr.net/npm/astronomy-engine@2.0/astronomy-engine.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
    <script src="https://cdn.jsdelivr.net/npm/topojson-client@3"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* ... (mantido o estilo original) ... */
    </style>
</head>
<body>
    <div class="container">
        <!-- ... (mantido o HTML original) ... -->
    </div>

    <script>
        // ... (dados dos países, constelações, etc. mantidos) ...

        // Variáveis globais para o mapa D3
        let svgMap, projection, pathGenerator, zoomBehavior, rotationInterval;
        let currentRotation = 0;
        let activeCountryCode = null; // Para rastrear o país atualmente selecionado (clicado)
        let currentTransform = d3.zoomIdentity; // Inicializa a transformação do zoom
        let currentHoveredCountryD3Datum = null; // Armazena o datum do país atualmente hovered

        // Variáveis para otimização
        let lastWidth = 0, lastHeight = 0, resizeTimeout;
        let lastAnimationFrame;

        // Função para inicializar e renderizar/atualizar o mapa
        async function renderMap() {
            const mapLoading = document.getElementById('map-loading');
            mapLoading.classList.add('show'); // Mostra o loading

            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            if (width === 0 || height === 0) {
                console.warn("Contêiner do mapa tem largura ou altura zero. O mapa não será renderizado até que as dimensões sejam válidas.");
                mapLoading.innerHTML = `<p style="color: var(--news-red);">Aguardando dimensões do mapa...</p>`;
                mapLoading.classList.remove('show');
                return; 
            }

            // Remove SVG existente para evitar duplicação em redesenho
            d3.select("#world-map svg").remove();

            svgMap = d3.select("#world-map")
                .append("svg")
                .attr("width", "100%")
                .attr("height", "100%")
                .attr("viewBox", `0 0 ${width} ${height}`); // Usa viewBox para responsividade

            // Define a projeção
            projection = d3.geoNaturalEarth1()
                .scale(width / 5.5) // Ajusta a escala inicial para o tamanho do contêiner
                .translate([width / 2, height / 2]);

            pathGenerator = d3.geoPath().projection(projection);

            try {
                // CORREÇÃO: URL válida para os dados do mapa
                const world = await d3.json("https://unpkg.com/world-atlas@2.0.2/countries-110m.json");

                svgMap.append("g")
                    .attr("class", "countries")
                    .selectAll("path")
                    .data(topojson.feature(world, world.objects.countries).features)
                    .enter()
                    .append("path")
                    .attr("d", pathGenerator)
                    .attr("fill", "rgba(100, 149, 237, 0.4)")
                    .attr("stroke", "rgba(255, 255, 255, 0.2)")
                    .attr("stroke-width", 0.5)
                    .attr("class", "country")
                    .attr("id", d => `country-${d.id}`) // Prefixo para IDs válidos
                    .on("mouseover", function(event, d) {
                        currentHoveredCountryD3Datum = d; // Armazena o datum do país hovered
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        const countryPath = d3.select(this);
                        
                        // Aplica escala no hover, exceto se for o país selecionado
                        if (countryCode && countryCode !== activeCountryCode) {
                            countryPath.classed("hovered", true).transition().duration(100).attr("transform", "scale(1.025)"); // Ajustado para 1.025
                        }
                        // Chamar a nova função para exibir detalhes no mapa (no hover)
                        showMapCountryHoverInfo(countryCode, d.properties.name, event, d);
                    })
                    .on("mouseout", function(event, d) {
                        currentHoveredCountryD3Datum = null; // Limpa o datum ao sair do hover
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        const countryPath = d3.select(this);

                        // Reseta a escala apenas se não for o país selecionado
                        if (countryCode && countryCode !== activeCountryCode) {
                            countryPath.classed("hovered", false).transition().duration(100).attr("transform", "");
                        }
                        // Esconder a caixa de detalhes no mapa (no hover)
                        hideMapCountryHoverInfo();
                    })
                    .on("click", function(event, d) {
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        
                        // Remover seleção anterior
                        d3.selectAll(".country").classed("selected", false)
                            .attr("fill", "rgba(100, 149, 237, 0.4)")
                            .attr("stroke", "rgba(255, 255, 255, 0.2)")
                            .attr("stroke-width", 0.5);
                        
                        // Destacar país selecionado
                        d3.select(this).classed("selected", true)
                            .attr("fill", "var(--neon-blue)")
                            .attr("stroke", "white")
                            .attr("stroke-width", 1.5);
                        
                        activeCountryCode = countryCode; // Atualiza o país ativo
                        updateCountryInfo(countryCode); // Atualiza o painel principal de informações e notícias
                        displayCountryDetails(countryCode, d.properties.name); // Atualiza o painel lateral com detalhes (no clique)
                        zoomToCountry(d); // NOVO: Zoom no país clicado
                    });

                // Configura o comportamento de zoom e arrastar (apenas uma vez)
                zoomBehavior = d3.zoom()
                    .scaleExtent([1, 8]) // Limites de zoom
                    .on("zoom", (event) => {
                        currentTransform = event.transform; // Atualiza a transformação atual
                        svgMap.selectAll("path").attr("transform", event.transform);
                        svgMap.select(".countries").attr("stroke-width", 0.5 / event.transform.k); // Escala a largura do traço
                        // Reposiciona a caixa de informações no mapa durante o zoom/pan
                        if (mapCountryHoverInfo.classList.contains('show') && currentHoveredCountryD3Datum) {
                            showMapCountryHoverInfo(
                                topoJsonIdToCountryCodeMap[currentHoveredCountryD3Datum.id], 
                                currentHoveredCountryD3Datum.properties.name, 
                                event, 
                                currentHoveredCountryD3Datum
                            );
                        }
                    });
                svgMap.call(zoomBehavior);

                // Adiciona funcionalidade de arrastar (pan)
                svgMap.on("mousedown", function() {
                    d3.select(this).classed("grabbing", true);
                }).on("mouseup", function() {
                    d3.select(this).classed("grabbing", false);
                });

                // Seleciona o Brasil por padrão (ID TopoJSON para Brasil é "076")
                const defaultCountryTopoId = "076";
                const brazilPath = svgMap.select(`#country-${defaultCountryTopoId}`);
                if (!brazilPath.empty()) {
                    // Pequeno delay para garantir que o elemento esteja pronto para receber o clique
                    setTimeout(() => brazilPath.dispatch("click"), 100);
                } else {
                    console.warn(`País com ID TopoJSON ${defaultCountryTopoId} (Brasil) não encontrado para seleção padrão.`);
                    // Fallback para o primeiro país disponível
                    const firstPath = svgMap.select(".country").node();
                    if (firstPath) {
                        setTimeout(() => d3.select(firstPath).dispatch("click"), 100);
                    }
                }

            } catch (error) {
                console.error("Erro fatal ao carregar ou renderizar o mapa:", error);
                mapLoading.innerHTML = `<p style="color: var(--news-red);">Erro ao carregar mapa: ${error.message}. Tente recarregar a página.</p>`;
            } finally {
                mapLoading.classList.remove('show'); // Esconde o loading
            }
        }

        // Função para dar zoom em um país específico
        function zoomToCountry(d) {
            if (!svgMap || !pathGenerator || !projection) return;

            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            // Calcula o "bounding box" do país selecionado
            const bounds = pathGenerator.bounds(d);
            const dx = bounds[1][0] - bounds[0][0];
            const dy = bounds[1][1] - bounds[0][1];
            const x = (bounds[0][0] + bounds[1][0]) / 2;
            const y = (bounds[0][1] + bounds[1][1]) / 2;

            // Calcula a nova escala e valores de translação
            const scale = Math.max(1, Math.min(8, 0.9 / Math.max(dx / width, dy / height)));
            const translate = [width / 2 - scale * x, height / 2 - scale * y];

            // Aplica a transformação de zoom e pan
            svgMap.transition()
                .duration(750) // Transição suave
                .call(zoomBehavior.transform, d3.zoomIdentity.translate(translate[0], translate[1]).scale(scale));
        }

        // ... (outras funções mantidas) ...

        // Inicializar a aplicação ao carregar a página
        window.onload = function() {
            const mapLoading = document.getElementById('map-loading');
            mapLoading.classList.add('show');

            // Inicializa ResizeObserver com otimizações
            const resizeObserver = new ResizeObserver(entries => {
                clearTimeout(resizeTimeout);
                resizeTimeout = setTimeout(() => {
                    const mapContainer = document.getElementById('world-map');
                    const newWidth = mapContainer.offsetWidth;
                    const newHeight = mapContainer.offsetHeight;
                    
                    // Redesenha apenas se as dimensões mudaram significativamente
                    if (Math.abs(newWidth - lastWidth) > 10 || Math.abs(newHeight - lastHeight) > 10) {
                        lastWidth = newWidth;
                        lastHeight = newHeight;
                        renderMap();
                    }
                }, 300); // Debounce de 300ms
            });
            resizeObserver.observe(document.getElementById('world-map'));

            // Renderiza o mapa inicial
            setTimeout(() => {
                renderMap();
            }, 200);

            // Atualiza o relógio e dados astronômicos a cada segundo
            setInterval(() => {
                if (activeCountryCode) {
                    updateCountryInfo(activeCountryCode);
                } else {
                    // Atualiza apenas a data e hora local se nenhum país estiver selecionado
                    document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { day: 'numeric', month: 'long', year: 'numeric' });
                    document.getElementById('stellar-time').textContent = new Date().toLocaleTimeString('pt-BR', { hour12: false });
                }
            }, 1000);
        };

        // Movimentar a caixa de detalhes no mapa com o mouse (otimizado)
        document.addEventListener('mousemove', (e) => {
            // Apenas atualiza a posição se a caixa de detalhes no mapa estiver visível E houver um país hovered
            if (!mapCountryHoverInfo.classList.contains('show') || !currentHoveredCountryD3Datum) return;

            // Otimização com requestAnimationFrame
            if (lastAnimationFrame) cancelAnimationFrame(lastAnimationFrame);
            
            lastAnimationFrame = requestAnimationFrame(() => {
                // Recalcula a posição com base no centro do país e no offset
                const centroid = pathGenerator.centroid(currentHoveredCountryD3Datum);
                const mapContainer = document.getElementById('world-map');
                const mapRect = mapContainer.getBoundingClientRect();
                const transformedCentroid = currentTransform.apply(centroid);

                mapCountryHoverInfo.style.left = (transformedCentroid[0] + mapRect.left + window.scrollX + 20) + 'px';
                mapCountryHoverInfo.style.top = (transformedCentroid[1] + mapRect.top + window.scrollY - mapCountryHoverInfo.offsetHeight / 2) + 'px';
            });
        });

        // ... (restante do código mantido) ...
    </script>
</body>
</html>
