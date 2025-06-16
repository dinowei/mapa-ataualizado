<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapa Mundi Interativo</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://d3js.org/d3.v7.min.js"></script>
    <script src="https://d3js.org/topojson.v3.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #0a192f;
            color: #e6f1ff;
            overflow-x: hidden;
        }
        
        .container {
            display: flex;
            min-height: 100vh;
            padding: 20px;
            gap: 20px;
        }
        
        .header {
            text-align: center;
            padding: 20px;
            background: linear-gradient(90deg, #0a192f, #112240, #0a192f);
            border-bottom: 1px solid #64ffda;
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #64ffda, #a8ff78);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .header p {
            font-size: 1.1rem;
            opacity: 0.8;
        }
        
        .map-container {
            flex: 1;
            position: relative;
            background-color: #112240;
            border-radius: 15px;
            padding: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }
        
        .map-controls {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
        }
        
        .map-btn {
            background-color: #0a192f;
            border: 1px solid #64ffda;
            color: #64ffda;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 18px;
        }
        
        .map-btn:hover {
            background-color: #64ffda;
            color: #0a192f;
            transform: scale(1.1);
        }
        
        #world-map {
            width: 100%;
            height: 100%;
            min-height: 70vh;
        }
        
        .info-panel {
            width: 350px;
            background-color: #112240;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
            gap: 20px;
            overflow-y: auto;
        }
        
        .country-details {
            background: linear-gradient(145deg, #112240, #0a192f);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #233554;
        }
        
        .country-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #233554;
        }
        
        .country-flag {
            width: 60px;
            height: 40px;
            border: 1px solid #233554;
            border-radius: 4px;
            object-fit: cover;
        }
        
        .country-name {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(90deg, #64ffda, #a8ff78);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .country-info-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        
        .info-item {
            display: flex;
            flex-direction: column;
        }
        
        .info-label {
            font-size: 0.85rem;
            color: #64ffda;
            opacity: 0.7;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 5px;
        }
        
        .info-value {
            font-size: 1.1rem;
            font-weight: 500;
        }
        
        .news-section {
            background: linear-gradient(145deg, #112240, #0a192f);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #233554;
        }
        
        .section-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #233554;
            color: #64ffda;
        }
        
        .news-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .news-item {
            background-color: #0a192f;
            border-radius: 8px;
            padding: 15px;
            transition: all 0.3s ease;
            border: 1px solid #233554;
        }
        
        .news-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border-color: #64ffda;
        }
        
        .news-title {
            font-size: 1.1rem;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        .news-title a {
            color: #e6f1ff;
            text-decoration: none;
        }
        
        .news-title a:hover {
            color: #64ffda;
            text-decoration: underline;
        }
        
        .news-desc {
            font-size: 0.9rem;
            color: #a8b2d1;
            margin-bottom: 10px;
        }
        
        .news-meta {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
            color: #64ffda;
        }
        
        .ai-section {
            background: linear-gradient(145deg, #112240, #0a192f);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #233554;
        }
        
        .ai-content {
            background-color: #0a192f;
            border-radius: 8px;
            padding: 15px;
            min-height: 100px;
            margin-bottom: 15px;
            font-size: 0.95rem;
            line-height: 1.6;
            border: 1px solid #233554;
        }
        
        .ai-btn {
            background: linear-gradient(90deg, #64ffda, #a8ff78);
            color: #0a192f;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
        }
        
        .ai-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(100, 255, 218, 0.3);
        }
        
        .ai-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .map-hover-info {
            position: absolute;
            background-color: rgba(10, 25, 47, 0.9);
            border: 1px solid #64ffda;
            border-radius: 8px;
            padding: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.3s ease;
            backdrop-filter: blur(5px);
            z-index: 1000;
        }
        
        .map-hover-flag {
            width: 30px;
            height: 20px;
            border-radius: 2px;
            object-fit: cover;
        }
        
        .map-hover-name {
            font-weight: 600;
            font-size: 1.1rem;
        }
        
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(100, 255, 218, 0.3);
            border-radius: 50%;
            border-top-color: #64ffda;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .country.selected {
            fill: #64ffda !important;
            stroke: #64ffda !important;
            stroke-width: 1.5px;
            filter: drop-shadow(0 0 8px rgba(100, 255, 218, 0.7));
        }
        
        .country.hovered {
            fill: #a8ff78 !important;
            stroke: #a8ff78 !important;
            stroke-width: 1px;
            filter: drop-shadow(0 0 5px rgba(168, 255, 120, 0.5));
        }
        
        .country {
            fill: #1e3a8a;
            stroke: #1d4ed8;
            stroke-width: 0.5px;
            transition: all 0.3s ease;
        }
        
        .map-svg {
            background-color: #0f2a5c;
        }
        
        @media (max-width: 900px) {
            .container {
                flex-direction: column;
            }
            
            .info-panel {
                width: 100%;
                max-height: 50vh;
            }
            
            #world-map {
                min-height: 50vh;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Mapa Mundi Interativo</h1>
        <p>Explore países, descubra informações detalhadas e acompanhe as últimas notícias</p>
    </div>
    
    <div class="container">
        <div class="map-container">
            <div class="map-controls">
                <button class="map-btn" id="zoom-in" title="Zoom In"><i class="fas fa-plus"></i></button>
                <button class="map-btn" id="zoom-out" title="Zoom Out"><i class="fas fa-minus"></i></button>
                <button class="map-btn" id="reset-map" title="Redefinir Mapa"><i class="fas fa-globe-americas"></i></button>
                <button class="map-btn" id="rotate-map" title="Rotacionar Mapa"><i class="fas fa-sync-alt"></i></button>
            </div>
            
            <div id="world-map"></div>
            
            <div class="map-hover-info" id="map-country-hover-info">
                <img class="map-hover-flag" id="map-hover-flag" src="" alt="">
                <div class="map-hover-name" id="map-hover-name"></div>
            </div>
        </div>
        
        <div class="info-panel">
            <div class="country-details">
                <div class="country-header">
                    <img id="details-flag" class="country-flag" src="" alt="Bandeira">
                    <h2 id="details-country-name" class="country-name">Selecione um país</h2>
                </div>
                
                <div class="country-info-grid">
                    <div class="info-item">
                        <div class="info-label">Capital</div>
                        <div id="details-capital" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">População</div>
                        <div id="details-population" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Língua</div>
                        <div id="details-language" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Moeda</div>
                        <div id="details-currency" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Coordenadas</div>
                        <div id="details-coords" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Fuso Horário</div>
                        <div id="details-timezone" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Continente</div>
                        <div id="details-continent" class="info-value">--</div>
                    </div>
                    <div class="info-item">
                        <div class="info-label">Área</div>
                        <div id="details-area" class="info-value">--</div>
                    </div>
                </div>
            </div>
            
            <div class="news-section">
                <h3 class="section-title">Últimas Notícias</h3>
                <button id="refresh-details-news" class="ai-btn"><i class="fas fa-sync-alt"></i> Atualizar Notícias</button>
                <div id="details-news-content" class="news-list">
                    <div class="news-item">
                        <div class="news-title">Selecione um país para ver as notícias</div>
                        <div class="news-desc">As notícias mais recentes serão exibidas aqui</div>
                    </div>
                </div>
            </div>
            
            <div class="ai-section">
                <h3 class="section-title">Curiosidades</h3>
                <div id="space-fact-output" class="ai-content">
                    Clique no botão para gerar uma curiosidade interessante!
                </div>
                <button id="generate-space-fact-btn" class="ai-btn"><i class="fas fa-star"></i> Gerar Curiosidade</button>
            </div>
        </div>
    </div>

    <script>
        // Dados dos países
        const countriesData = {
            "BR": { 
                name: "Brasil", capital: "Brasília", lat: -15.78, lon: -47.93, timezone: "UTC-3",
                population: "214 milhões", language: "Português", currency: "Real (BRL)",
                continent: "América do Sul", area: "8.516 milhões km²", gdp: "US$ 1.92 trilhões"
            },
            "US": { 
                name: "Estados Unidos", capital: "Washington, D.C.", lat: 38.90, lon: -77.04, timezone: "UTC-4 a UTC-10",
                population: "331 milhões", language: "Inglês", currency: "Dólar (USD)",
                continent: "América do Norte", area: "9.834 milhões km²", gdp: "US$ 23.32 trilhões"
            },
            "GB": { 
                name: "Reino Unido", capital: "Londres", lat: 51.50, lon: -0.12, timezone: "UTC+1",
                population: "67 milhões", language: "Inglês", currency: "Libra Esterlina (GBP)",
                continent: "Europa", area: "242.495 km²", gdp: "US$ 3.19 trilhões"
            },
            "JP": {
                name: "Japão", capital: "Tóquio", lat: 35.68, lon: 139.69, timezone: "UTC+9",
                population: "125 milhões", language: "Japonês", currency: "Iene (JPY)",
                continent: "Ásia", area: "377.975 km²", gdp: "US$ 4.91 trilhões"
            },
            "AU": {
                name: "Austrália", capital: "Camberra", lat: -35.28, lon: 149.13, timezone: "UTC+10",
                population: "26 milhões", language: "Inglês", currency: "Dólar Australiano (AUD)",
                continent: "Oceania", area: "7.692 milhões km²", gdp: "US$ 1.55 trilhões"
            },
            "CA": {
                name: "Canadá", capital: "Ottawa", lat: 45.42, lon: -75.69, timezone: "UTC-5 a UTC-8",
                population: "38 milhões", language: "Inglês, Francês", currency: "Dólar Canadense (CAD)",
                continent: "América do Norte", area: "9.985 milhões km²", gdp: "US$ 2.02 trilhões"
            },
            "DE": {
                name: "Alemanha", capital: "Berlim", lat: 52.52, lon: 13.40, timezone: "UTC+2",
                population: "83 milhões", language: "Alemão", currency: "Euro (EUR)",
                continent: "Europa", area: "357.022 km²", gdp: "US$ 4.26 trilhões"
            },
            "FR": {
                name: "França", capital: "Paris", lat: 48.85, lon: 2.35, timezone: "UTC+2",
                population: "65 milhões", language: "Francês", currency: "Euro (EUR)",
                continent: "Europa", area: "643.801 km²", gdp: "US$ 2.94 trilhões"
            },
            "IN": {
                name: "Índia", capital: "Nova Delhi", lat: 28.61, lon: 77.20, timezone: "UTC+5:30",
                population: "1.4 bilhões", language: "Hindi, Inglês", currency: "Rúpia Indiana (INR)",
                continent: "Ásia", area: "3.287 milhões km²", gdp: "US$ 3.18 trilhões"
            },
            "CN": {
                name: "China", capital: "Pequim", lat: 39.90, lon: 116.40, timezone: "UTC+8",
                population: "1.4 bilhões", language: "Mandarim", currency: "Yuan (CNY)",
                continent: "Ásia", area: "9.597 milhões km²", gdp: "US$ 17.73 trilhões"
            },
            "RU": {
                name: "Rússia", capital: "Moscou", lat: 55.75, lon: 37.61, timezone: "UTC+2 a UTC+12",
                population: "144 milhões", language: "Russo", currency: "Rublo Russo (RUB)",
                continent: "Europa/Ásia", area: "17.125 milhões km²", gdp: "US$ 1.78 trilhões"
            },
            "MX": {
                name: "México", capital: "Cidade do México", lat: 19.43, lon: -99.13, timezone: "UTC-5 a UTC-8",
                population: "126 milhões", language: "Espanhol", currency: "Peso Mexicano (MXN)",
                continent: "América do Norte", area: "1.964 milhões km²", gdp: "US$ 1.29 trilhões"
            },
            "ZA": {
                name: "África do Sul", capital: "Pretória", lat: -25.74, lon: 28.22, timezone: "UTC+2",
                population: "60 milhões", language: "Africâner, Inglês, Zulu, Xhosa...", currency: "Rand (ZAR)",
                continent: "África", area: "1.221 milhões km²", gdp: "US$ 415 bilhões"
            },
            "EG": {
                name: "Egito", capital: "Cairo", lat: 30.04, lon: 31.23, timezone: "UTC+2",
                population: "109 milhões", language: "Árabe", currency: "Libra Egípcia (EGP)",
                continent: "África", area: "1.002 milhões km²", gdp: "US$ 404 bilhões"
            },
            "IT": {
                name: "Itália", capital: "Roma", lat: 41.90, lon: 12.49, timezone: "UTC+2",
                population: "59 milhões", language: "Italiano", currency: "Euro (EUR)",
                continent: "Europa", area: "301.340 km²", gdp: "US$ 2.11 trilhões"
            },
            "ES": {
                name: "Espanha", capital: "Madri", lat: 40.41, lon: -3.70, timezone: "UTC+2",
                population: "47 milhões", language: "Espanhol", currency: "Euro (EUR)",
                continent: "Europa", area: "505.990 km²", gdp: "US$ 1.45 trilhões"
            },
            "NG": {
                name: "Nigéria", capital: "Abuja", lat: 9.07, lon: 7.49, timezone: "UTC+1",
                population: "218 milhões", language: "Inglês, Hauçá, Iorubá, Igbo", currency: "Naira (NGN)",
                continent: "África", area: "923.768 km²", gdp: "US$ 441 bilhões"
            },
            "KE": {
                name: "Quênia", capital: "Nairóbi", lat: -1.28, lon: 36.82, timezone: "UTC+3",
                population: "55 milhões", language: "Suaíli, Inglês", currency: "Xelim Queniano (KES)",
                continent: "África", area: "580.367 km²", gdp: "US$ 110 bilhões"
            }
        };

        // Mapeamento de IDs do TopoJSON para códigos de país
        const topoJsonIdToCountryCodeMap = {
            "076": "BR", "840": "US", "826": "GB", "392": "JP", "036": "AU",
            "124": "CA", "276": "DE", "250": "FR", "356": "IN", "156": "CN",
            "643": "RU", "484": "MX", "710": "ZA", "818": "EG", "380": "IT",
            "724": "ES", "566": "NG", "404": "KE", "504": "MA", "012": "DZ",
            "288": "GH", "024": "AO", "508": "MZ", "180": "CD"
        };

        // Estado da aplicação
        const state = {
            svgMap: null,
            projection: null,
            pathGenerator: null,
            zoomBehavior: null,
            rotationInterval: null,
            currentRotation: 0,
            activeCountryCode: null,
            currentTransform: d3.zoomIdentity,
            currentHoveredCountry: null,
            newsData: null,
            theme: 'dark'
        };

        // Inicializar elementos DOM
        function initializeDOMElements() {
            // Mapear todos os elementos necessários
            return {
                countryDetailsPanel: document.getElementById('country-details-panel'),
                detailsFlag: document.getElementById('details-flag'),
                detailsCountryName: document.getElementById('details-country-name'),
                detailsCapital: document.getElementById('details-capital'),
                detailsPopulation: document.getElementById('details-population'),
                detailsLanguage: document.getElementById('details-language'),
                detailsCurrency: document.getElementById('details-currency'),
                detailsCoords: document.getElementById('details-coords'),
                detailsTimezone: document.getElementById('details-timezone'),
                detailsContinent: document.getElementById('details-continent'),
                detailsArea: document.getElementById('details-area'),
                detailsNewsContent: document.getElementById('details-news-content'),
                refreshDetailsNewsBtn: document.getElementById('refresh-details-news'),
                mapCountryHoverInfo: document.getElementById('map-country-hover-info'),
                mapHoverFlag: document.getElementById('map-hover-flag'),
                mapHoverName: document.getElementById('map-hover-name'),
                spaceFactOutput: document.getElementById('space-fact-output'),
                generateSpaceFactBtn: document.getElementById('generate-space-fact-btn'),
                newsSummaryOutput: document.getElementById('news-summary-output'),
                summarizeNewsBtn: document.getElementById('summarize-news-btn'),
                zoomInBtn: document.getElementById('zoom-in'),
                zoomOutBtn: document.getElementById('zoom-out'),
                resetMapBtn: document.getElementById('reset-map'),
                rotateMapBtn: document.getElementById('rotate-map')
            };
        }

        // Variável global para elementos DOM
        let DOM = initializeDOMElements();

        // Função para renderizar o mapa
        async function renderMap() {
            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            if (width === 0 || height === 0) return;

            // Remover SVG existente
            d3.select("#world-map svg").remove();

            // Criar novo SVG
            state.svgMap = d3.select("#world-map")
                .append("svg")
                .attr("width", "100%")
                .attr("height", "100%")
                .attr("viewBox", `0 0 ${width} ${height}`)
                .attr("class", "map-svg");

            // Configurar projeção do mapa
            state.projection = d3.geoNaturalEarth1()
                .scale(width / 5.5)
                .translate([width / 2, height / 2]);

            state.pathGenerator = d3.geoPath().projection(state.projection);

            try {
                // Carregar dados do mundo
                const world = await d3.json("https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json");
                
                // Adicionar países ao mapa
                state.svgMap.append("g")
                    .attr("class", "countries")
                    .selectAll("path")
                    .data(topojson.feature(world, world.objects.countries).features)
                    .enter()
                    .append("path")
                    .attr("d", state.pathGenerator)
                    .attr("class", "country")
                    .attr("id", (d) => `country-${d.id}`)
                    .on("mouseover", handleCountryMouseOver)
                    .on("mouseout", handleCountryMouseOut)
                    .on("click", handleCountryClick);

                // Configurar zoom
                state.zoomBehavior = d3.zoom()
                    .scaleExtent([1, 8])
                    .on("zoom", handleZoom);

                state.svgMap.call(state.zoomBehavior)
                    .on("mousedown", () => state.svgMap.classed("grabbing", true))
                    .on("mouseup", () => state.svgMap.classed("grabbing", false));

                // Selecionar Brasil por padrão
                setTimeout(() => {
                    const brazilPath = state.svgMap.select('#country-076');
                    if (!brazilPath.empty()) {
                        const node = brazilPath.node();
                        if (node) node.dispatchEvent(new MouseEvent('click', { bubbles: true }));
                    }
                }, 500);

            } catch (error) {
                console.error("Erro ao carregar mapa:", error);
            }
        }

        // Handlers de eventos do mapa
        function handleCountryMouseOver(event, d) {
            state.currentHoveredCountry = d;
            const topoId = d.id;
            const countryCode = topoJsonIdToCountryCodeMap[topoId];
            const countryPath = d3.select(this);
            
            if (countryCode && countryCode !== state.activeCountryCode) {
                countryPath.classed("hovered", true);
            }
            showMapCountryHoverInfo(countryCode, d.properties.name, event, d);
        }

        function handleCountryMouseOut(event, d) {
            state.currentHoveredCountry = null;
            const topoId = d.id;
            const countryCode = topoJsonIdToCountryCodeMap[topoId];
            const countryPath = d3.select(this);

            if (countryCode && countryCode !== state.activeCountryCode) {
                countryPath.classed("hovered", false);
            }
            hideMapCountryHoverInfo();
        }

        function handleCountryClick(event, d) {
            const topoId = d.id;
            const countryCode = topoJsonIdToCountryCodeMap[topoId];
            
            d3.selectAll(".country").classed("selected", false);
            d3.select(this).classed("selected", true);
            
            state.activeCountryCode = countryCode;
            updateCountryInfo(countryCode);
            displayCountryDetails(countryCode, d.properties.name);
            zoomToCountry(d);
        }

        function handleZoom(event) {
            state.currentTransform = event.transform;
            state.svgMap.selectAll("g.countries").attr("transform", event.transform);
            state.svgMap.selectAll("path.country").attr("stroke-width", 0.5 / event.transform.k);

            if (DOM.mapCountryHoverInfo.classList.contains('show') && state.currentHoveredCountry) {
                showMapCountryHoverInfo(
                    topoJsonIdToCountryCodeMap[state.currentHoveredCountry.id], 
                    state.currentHoveredCountry.properties.name, 
                    event, 
                    state.currentHoveredCountry
                );
            }
        }

        // Funções de controle do mapa
        function zoomToCountry(d) {
            if (!state.svgMap || !state.pathGenerator || !state.projection || !state.zoomBehavior) return;

            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            const bounds = state.pathGenerator.bounds(d);
            const dx = bounds[1][0] - bounds[0][0];
            const dy = bounds[1][1] - bounds[0][1];
            const x_coord = (bounds[0][0] + bounds[1][0]) / 2;
            const y_coord = (bounds[0][1] + bounds[1][1]) / 2;

            const scale = Math.max(1, Math.min(8, 0.9 / Math.max(dx / width, dy / height)));
            const translate = [width / 2 - scale * x_coord, height / 2 - scale * y_coord];

            state.svgMap.transition()
                .duration(750)
                .call(state.zoomBehavior.transform, d3.zoomIdentity.translate(translate[0], translate[1]).scale(scale));
        }

        function zoomIn() {
            if (state.svgMap && state.zoomBehavior) {
                state.svgMap.transition().duration(300).call(state.zoomBehavior.scaleBy, 1.5);
            }
        }

        function zoomOut() {
            if (state.svgMap && state.zoomBehavior) {
                state.svgMap.transition().duration(300).call(state.zoomBehavior.scaleBy, 0.75);
            }
        }

        function resetMap() {
            if (state.svgMap && state.zoomBehavior) {
                state.svgMap.transition().duration(750).call(state.zoomBehavior.transform, d3.zoomIdentity);
            }
            
            state.currentRotation = 0;
            if (state.projection) state.projection.rotate([state.currentRotation, 0]);
            if (state.svgMap) state.svgMap.selectAll("path.country").attr("d", state.pathGenerator);
            
            d3.selectAll(".country").classed("selected", false);
            state.activeCountryCode = null;
            updateCountryInfo(null);
            
            if (state.rotationInterval) {
                clearInterval(state.rotationInterval);
                state.rotationInterval = null;
                DOM.rotateMapBtn.innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            }
            
            DOM.detailsCountryName.textContent = "Selecione um país";
            DOM.detailsFlag.src = "";
            DOM.detailsNewsContent.innerHTML = '<div class="news-item"><div class="news-title">Selecione um país para ver as notícias</div><div class="news-desc">As notícias mais recentes serão exibidas aqui</div></div>';
            DOM.spaceFactOutput.textContent = "Clique no botão para gerar uma curiosidade interessante!";
        }

        function rotateMap() {
            if (!DOM.rotateMapBtn) return;

            if (state.rotationInterval) {
                clearInterval(state.rotationInterval);
                state.rotationInterval = null;
                DOM.rotateMapBtn.innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            } else {
                state.rotationInterval = setInterval(() => {
                    state.currentRotation += 0.5;
                    if (state.currentRotation > 360) state.currentRotation -= 360;
                    if (state.projection) state.projection.rotate([state.currentRotation, 0]);
                    if (state.svgMap) state.svgMap.selectAll("path.country").attr("d", state.pathGenerator);
                }, 50);
                DOM.rotateMapBtn.innerHTML = '<i class="fas fa-pause"></i> Pausar Rotação';
            }
        }

        // Funções de UI
        function showMapCountryHoverInfo(countryCode, countryNameFromTopoJson, d3Event, d3Datum) {
            if (!DOM.mapHoverFlag || !DOM.mapHoverName || !DOM.mapCountryHoverInfo || !state.pathGenerator) return;
            
            const displayCountryName = countryNameFromTopoJson || "País Desconhecido";
            const flagSrc = countryCode ? `https://flagcdn.com/w20/${countryCode.toLowerCase()}.png` : 'https://placehold.co/24x18/cccccc/333333?text=N/A';

            DOM.mapHoverFlag.src = flagSrc;
            DOM.mapHoverName.textContent = displayCountryName;
            
            let xPos, yPos;
            if (d3Event && typeof d3Event.pageX === 'number' && typeof d3Event.pageY === 'number') {
                xPos = d3Event.pageX + 20;
                yPos = d3Event.pageY - (DOM.mapCountryHoverInfo.offsetHeight / 2);
            } else {
                const centroid = state.pathGenerator.centroid(d3Datum);
                const mapContainer = document.getElementById('world-map');
                if (!mapContainer) return;
                
                const mapRect = mapContainer.getBoundingClientRect();
                const transformedCentroid = state.currentTransform.apply(centroid);

                xPos = transformedCentroid[0] + mapRect.left + window.scrollX + 20;
                yPos = transformedCentroid[1] + mapRect.top + window.scrollY - DOM.mapCountryHoverInfo.offsetHeight / 2;
            }

            DOM.mapCountryHoverInfo.style.left = xPos + 'px';
            DOM.mapCountryHoverInfo.style.top = yPos + 'px';
            DOM.mapCountryHoverInfo.style.opacity = 1;
        }

        function hideMapCountryHoverInfo() {
            if (DOM.mapCountryHoverInfo) DOM.mapCountryHoverInfo.style.opacity = 0;
        }

        function displayCountryDetails(countryCode, countryNameFromTopoJson) {
            if (!DOM.detailsFlag || !DOM.detailsCountryName) return;

            const country = countriesData[countryCode];
            const displayCountryName = countryNameFromTopoJson || (country ? country.name : "País Desconhecido");

            // Atualizar informações básicas
            DOM.detailsFlag.src = countryCode ? `https://flagcdn.com/w40/${countryCode.toLowerCase()}.png` : 'https://placehold.co/40x30/cccccc/333333?text=N/A';
            DOM.detailsCountryName.textContent = displayCountryName;
            
            // Atualizar demais detalhes se disponíveis
            if (country) {
                if (DOM.detailsCapital) DOM.detailsCapital.textContent = country.capital;
                if (DOM.detailsPopulation) DOM.detailsPopulation.textContent = country.population;
                if (DOM.detailsLanguage) DOM.detailsLanguage.textContent = country.language;
                if (DOM.detailsCurrency) DOM.detailsCurrency.textContent = country.currency;
                if (DOM.detailsCoords) DOM.detailsCoords.textContent = `${country.lat}°N, ${Math.abs(country.lon)}°${country.lon < 0 ? 'W' : 'E'}`;
                if (DOM.detailsTimezone) DOM.detailsTimezone.textContent = country.timezone;
                if (DOM.detailsContinent) DOM.detailsContinent.textContent = country.continent || '--';
                if (DOM.detailsArea) DOM.detailsArea.textContent = country.area || '--';
                
                fetchNews(country.name);
            } else {
                DOM.detailsNewsContent.innerHTML = '<div class="news-item"><div class="news-title">Detalhes do país não disponíveis</div></div>';
            }
            
            DOM.spaceFactOutput.textContent = "Clique no botão para gerar uma curiosidade interessante!";
        }

        function updateCountryInfo(countryCode) {
            const country = countryCode ? countriesData[countryCode] : null;
            
            if (!country) {
                return;
            }
        }

        // Funções de dados
        async function fetchNews(query) {
            if (!DOM.detailsNewsContent) return;
            
            // Exemplo de notícias (em uma implementação real, você usaria uma API como NewsAPI)
            const sampleNews = [
                {
                    title: `${query} anuncia novo plano econômico`,
                    description: `O governo de ${query} apresentou hoje um novo plano para estimular a economia do país.`,
                    url: "#",
                    source: "Notícias Internacionais",
                    publishedAt: new Date()
                },
                {
                    title: `Crescimento recorde no turismo em ${query}`,
                    description: `O setor de turismo em ${query} registrou crescimento recorde neste trimestre.`,
                    url: "#",
                    source: "Portal do Turismo",
                    publishedAt: new Date(Date.now() - 2 * 24 * 60 * 60 * 1000)
                },
                {
                    title: `${query} lança iniciativa de sustentabilidade ambiental`,
                    description: `O país anunciou uma nova iniciativa para reduzir emissões de carbono até 2030.`,
                    url: "#",
                    source: "Green News",
                    publishedAt: new Date(Date.now() - 3 * 24 * 60 * 60 * 1000)
                },
                {
                    title: `Acordo comercial entre ${query} e países vizinhos`,
                    description: `Novo acordo comercial promete fortalecer a economia regional.`,
                    url: "#",
                    source: "Economia Global",
                    publishedAt: new Date(Date.now() - 4 * 24 * 60 * 60 * 1000)
                }
            ];

            renderNews(sampleNews);
        }

        function renderNews(articles) {
            if (!DOM.detailsNewsContent) return;
            
            DOM.detailsNewsContent.innerHTML = '';
            articles.forEach(article => {
                const newsItem = document.createElement('div');
                newsItem.classList.add('news-item');
                
                const date = new Date(article.publishedAt);
                const formattedDate = date.toLocaleDateString('pt-BR');
                
                newsItem.innerHTML = `
                    <h3 class="news-title"><a href="${article.url}" target="_blank">${article.title}</a></h3>
                    <div class="news-desc">${article.description}</div>
                    <div class="news-meta">
                        <span>${article.source}</span>
                        <span>${formattedDate}</span>
                    </div>
                `;
                DOM.detailsNewsContent.appendChild(newsItem);
            });
        }

        // Funções de IA (simuladas)
        function generateSpaceFact() {
            if (!DOM.spaceFactOutput || !DOM.generateSpaceFactBtn) return;

            // Simular carregamento
            DOM.spaceFactOutput.classList.add('loading');
            DOM.spaceFactOutput.innerHTML = '<span class="loading"></span> Gerando curiosidade...';
            DOM.generateSpaceFactBtn.disabled = true;

            // Dados de curiosidades espaciais
            const spaceFacts = [
                "A Estação Espacial Internacional viaja a 28.000 km/h, completando 16 órbitas terrestres por dia.",
                "Existem mais estrelas no universo do que grãos de areia em todas as praias da Terra.",
                "Vênus é o planeta mais quente do sistema solar, com temperaturas que podem chegar a 470°C.",
                "Uma colher de chá de uma estrela de nêutrons pesaria cerca de 6 bilhões de toneladas.",
                "A Lua está se afastando da Terra a uma taxa de aproximadamente 3,8 cm por ano.",
                "Júpiter é tão grande que poderia conter todos os outros planetas do sistema solar dentro dele.",
                "O sol contém 99,86% de toda a massa do sistema solar.",
                "Um dia em Mercúrio equivale a 176 dias na Terra.",
                "Há mais árvores na Terra do que estrelas na Via Láctea - cerca de 3 trilhões contra 100-400 bilhões.",
                "A luz solar que vemos agora partiu do Sol há 8 minutos e 20 segundos."
            ];

            // Simular tempo de resposta da IA
            setTimeout(() => {
                const randomFact = spaceFacts[Math.floor(Math.random() * spaceFacts.length)];
                DOM.spaceFactOutput.classList.remove('loading');
                DOM.spaceFactOutput.textContent = randomFact;
                DOM.generateSpaceFactBtn.disabled = false;
            }, 1500);
        }

        // Configurar eventos
        function setupEventListeners() {
            // Botões de controle do mapa
            if (DOM.zoomInBtn) DOM.zoomInBtn.addEventListener('click', zoomIn);
            if (DOM.zoomOutBtn) DOM.zoomOutBtn.addEventListener('click', zoomOut);
            if (DOM.resetMapBtn) DOM.resetMapBtn.addEventListener('click', resetMap);
            if (DOM.rotateMapBtn) DOM.rotateMapBtn.addEventListener('click', rotateMap);
            
            // Botão de atualizar notícias
            if (DOM.refreshDetailsNewsBtn) {
                DOM.refreshDetailsNewsBtn.addEventListener('click', () => {
                    if (state.activeCountryCode) {
                        const country = countriesData[state.activeCountryCode];
                        if (country) fetchNews(country.name);
                    }
                });
            }
            
            // Botão de curiosidades
            if (DOM.generateSpaceFactBtn) {
                DOM.generateSpaceFactBtn.addEventListener('click', generateSpaceFact);
            }
            
            // Redimensionamento responsivo
            const mapContainer = document.getElementById('world-map');
            if (mapContainer) {
                window.addEventListener('resize', () => {
                    clearTimeout(resizeTimeout);
                    resizeTimeout = setTimeout(renderMap, 300);
                });
            }
        }

        // Inicializar aplicação
        document.addEventListener('DOMContentLoaded', () => {
            DOM = initializeDOMElements();
            setupEventListeners();
            renderMap();
            
            // Atualizar relógio a cada segundo
            setInterval(() => {
                const now = new Date();
                document.getElementById('current-date').textContent = now.toLocaleDateString('pt-BR', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });
            }, 1000);
        });

        // Variável para timeout de redimensionamento
        let resizeTimeout;
    </script>
</body>
</html>
