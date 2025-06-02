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
        html, body {
            height: 100%; /* Garante que HTML e Body ocupem a altura total da viewport */
            margin: 0;
            padding: 0;
            display: flex; /* Torna o body um contêiner flexível */
            flex-direction: column; /* Organiza os filhos verticalmente */
        }

        :root {
            --space-dark: #0B0D17;
            --space-blue: #1A1B41;
            --neon-blue: #4DEDFF;
            --neon-purple: #845AFF;
            --star-yellow: #FFE66D;
            --news-red: #FF4D4D;
            --day-bg: #f0f8ff;
            --day-text: #333;
            --day-card: #ffffff;
            --day-border: #d0e8ff;
        }
        
        /* Modo dia/noite */
        body.day-mode {
            background-color: var(--day-bg);
            color: var(--day-text);
            background-image: none; /* Remove o gradiente no modo dia */
        }
        
        body.day-mode .map-container, 
        body.day-mode .stellar-clock,
        body.day-mode .info-box,
        body.day-mode .country-details-panel, /* Adicionado para o novo painel */
        body.day-mode .controls {
            background: rgba(255, 255, 255, 0.7);
            border: 1px solid var(--day-border);
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
        }

        body.day-mode .country-details-panel .news-section { /* Estilo específico para notícias no painel de detalhes */
            background: rgba(255, 255, 255, 0.7);
            border: 1px solid var(--day-border);
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
        }
        
        body.day-mode .info-title, 
        body.day-mode .news-title,
        body.day-mode .info-label {
            color: #1a73e8;
        }
        
        body.day-mode .country-details-panel .news-section {
            border-color: #ff6b6b;
            box-shadow: 0 0 15px rgba(255, 107, 107, 0.1);
        }
        
        body.day-mode .theme-toggle {
            background: var(--day-card);
            color: #333;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--space-dark);
            color: white;
            min-height: 100vh;
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(255, 230, 109, 0.1) 0%, transparent 20%),
                radial-gradient(circle at 90% 30%, rgba(77, 237, 255, 0.1) 0%, transparent 25%),
                radial-gradient(circle at 50% 80%, rgba(132, 90, 255, 0.1) 0%, transparent 30%);
            transition: background-color 0.5s ease;
        }

        .container {
            max-width: 1600px;
            margin: 0 auto;
            padding: 1rem;
            flex-grow: 1; /* Permite que o contêiner principal ocupe o espaço restante */
            display: flex;
            flex-direction: column; /* Organiza os filhos verticalmente dentro do contêiner */
        }

        header {
            text-align: center;
            margin-bottom: 1.5rem;
            position: relative;
            padding-top: 1rem;
        }

        h1 {
            font-size: 2.8rem;
            margin: 0;
            background: linear-gradient(90deg, var(--neon-blue), var(--neon-purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(77, 237, 255, 0.3);
            position: relative;
            z-index: 2;
            letter-spacing: 1px;
        }

        .subtitle {
            font-size: 1.2rem;
            color: rgba(255, 255, 255, 0.7);
            margin-top: 0.5rem;
        }

        .planet-decoration {
            position: absolute;
            border-radius: 50%;
            filter: blur(10px);
            opacity: 0.7;
            z-index: 1;
        }

        .planet-1 {
            width: 100px;
            height: 100px;
            background: radial-gradient(circle at 30% 30%, var(--neon-blue), var(--space-blue));
            top: -30px;
            left: 10%;
            animation: float 8s ease-in-out infinite;
        }

        .planet-2 {
            width: 150px;
            height: 150px;
            background: radial-gradient(circle at 30% 30%, var(--neon-purple), #3A1B6E);
            bottom: -50px;
            right: 10%;
            animation: float 10s ease-in-out infinite 2s;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(5deg); }
        }

        .theme-toggle {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: rgba(26, 27, 65, 0.7);
            border: 1px solid var(--neon-blue);
            border-radius: 50px;
            padding: 0.5rem 1rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            z-index: 10;
            transition: all 0.3s ease;
        }

        .theme-toggle:hover {
            background: rgba(77, 237, 255, 0.2);
            transform: translateY(-2px);
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
            flex-grow: 1; /* Permite que o conteúdo principal ocupe o espaço restante */
        }

        .map-section {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            position: relative; /* Para posicionar o painel de detalhes do país */
            flex-grow: 1; /* Permite que a seção do mapa cresça */
        }

        .map-container {
            background: rgba(26, 27, 65, 0.5);
            border-radius: 15px;
            padding: 1rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(77, 237, 255, 0.2);
            box-shadow: 0 0 20px rgba(77, 237, 255, 0.1);
            position: relative;
            flex-grow: 1; /* Permite que o contêiner do mapa ocupe o espaço disponível */
            min-height: 400px; /* Adiciona uma altura mínima para garantir visibilidade */
        }

        #world-map {
            width: 100%;
            height: 100%; /* Garante que o elemento SVG ocupe 100% da altura do seu pai */
            cursor: grab; /* Cursor para indicar que pode ser arrastado */
        }
        #world-map.grabbing {
            cursor: grabbing;
        }

        .country { /* Estilo padrão do país */
            transition: fill 0.2s ease, transform 0.1s ease-out, stroke 0.2s ease;
            transform-origin: center center; /* Garante que a escala seja do centro */
        }
        .country:hover:not(.selected) { /* Apenas para países não selecionados */
            transform: scale(1.025); /* Ajustado para 1.025 (metade do efeito anterior) */
        }
        .country.selected { /* Estilo para o país selecionado */
            fill: var(--neon-blue) !important; /* Força a cor de seleção */
            stroke: white !important;
            stroke-width: 1.5 !important;
        }
        
        /* Estilo para a caixa de informações SIMPLIFICADAS com bandeira no mapa (aparece no hover) */
        .map-country-hover-info {
            position: absolute;
            background: rgba(0, 0, 0, 0.85); /* Mais escuro para destaque */
            border: 1px solid var(--neon-blue);
            border-radius: 8px;
            padding: 8px 12px; /* Reduzido para menos conteúdo */
            pointer-events: none; /* Crucial para não bloquear interações do mapa */
            z-index: 101; /* Acima de tudo no mapa */
            display: flex;
            align-items: center; /* Alinha bandeira e nome horizontalmente */
            gap: 10px;
            opacity: 0;
            transition: opacity 0.2s ease-out, transform 0.2s ease-out;
            transform: translate(0px, -50%); /* Ajuste para centralizar verticalmente em relação ao mouse/país */
            min-width: 120px; /* Largura mínima para os detalhes */
            max-width: 200px; /* Largura máxima */
            box-shadow: 0 4px 15px rgba(77, 237, 255, 0.2);
        }

        .map-country-hover-info.show {
            opacity: 1;
            transform: translate(0px, -50%); /* Mantém a posição ajustada */
        }

        .map-country-hover-info img {
            width: 24px; /* Tamanho menor para a bandeira no hover */
            height: 18px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 2px;
        }

        .map-country-hover-info span {
            font-size: 1rem; /* Tamanho da fonte ajustado */
            font-weight: bold;
            color: var(--star-yellow); /* Destaque o nome do país */
            white-space: nowrap; /* Evita quebra de linha no nome */
            overflow: hidden;
            text-overflow: ellipsis; /* Adiciona reticências se o nome for muito longo */
        }


        /* Novo painel de informações detalhadas do país (AGORA SÓ NO CLIQUE) */
        .country-details-panel {
            position: absolute; /* Posicionamento absoluto dentro de map-section */
            top: 0;
            right: -320px; /* Escondido à direita por padrão */
            width: 300px; /* Largura do painel */
            height: 100%;
            background: rgba(26, 27, 65, 0.9);
            border-radius: 15px;
            padding: 1.5rem;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(77, 237, 255, 0.3);
            box-shadow: 0 0 30px rgba(77, 237, 255, 0.2);
            z-index: 90; /* Abaixo do hover-tooltip, mas acima do mapa */
            transition: right 0.4s ease-out, opacity 0.4s ease-out; /* Animação de entrada/saída */
            opacity: 0; /* Escondido por padrão */
            pointer-events: none; /* Não interfere com cliques no mapa quando escondido */
            display: flex;
            flex-direction: column;
            gap: 1rem;
            overflow-y: auto; /* Para conteúdo que excede a altura */
        }

        .country-details-panel.show {
            right: 0; /* Ajuste para a posição desejada quando visível */
            opacity: 1;
            pointer-events: auto; /* Permite interação quando visível */
        }
        /* Ajuste para que o painel apareça ao lado do mapa quando há espaço */
        @media (min-width: 1600px) { /* Aumentar o max-width do container para 1600px */
            .main-content {
                grid-template-columns: 1fr 300px 1fr; /* Mapa | Painel Detalhes | Info Panel */
                gap: 2rem;
            }
            .map-section {
                grid-column: 1 / 2; /* Mapa na primeira coluna */
            }
            .country-details-panel {
                position: relative; /* Torna-o parte do grid */
                right: auto;
                top: auto;
                opacity: 1; /* Sempre visível se o grid permitir */
                pointer-events: auto;
                grid-column: 2 / 3; /* Painel de detalhes na segunda coluna */
                width: auto; /* Ajusta à coluna do grid */
                height: auto;
            }
            .info-panel {
                grid-column: 3 / 4; /* Painel de informações na terceira coluna */
            }
        }
        /* Ajuste para que o painel apareça abaixo do mapa em telas menores */
        @media (max-width: 1599px) and (min-width: 768px) {
            .main-content {
                grid-template-columns: 1fr; /* Em telas menores, o mapa e o painel de detalhes ficam em uma única coluna */
            }
            .map-section {
                width: 100%; /* Ocupa toda a largura da coluna */
            }
            .country-details-panel {
                position: relative; /* Volta a ser parte do fluxo normal */
                right: auto;
                top: auto;
                width: 100%;
                height: auto;
                opacity: 1; /* Sempre visível se o grid permitir */
                pointer-events: auto;
                margin-top: 1.5rem; /* Espaço entre o mapa e o painel */
            }
        }


        .country-details-panel .flag-and-name {
            display: flex;
            align-items: center;
            gap: 1rem;
            margin-bottom: 1rem;
        }
        .country-details-panel .flag-and-name img {
            width: 40px;
            height: 30px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 4px;
        }
        .country-details-panel .flag-and-name h3 {
            margin: 0;
            color: var(--neon-blue);
            font-size: 1.6rem;
        }
        .country-details-panel p {
            margin: 0.5rem 0;
        }


        .info-label {
            color: var(--neon-blue);
            font-weight: bold;
        }

        .controls {
            display: flex;
            gap: 1rem;
            background: rgba(26, 27, 65, 0.5);
            border-radius: 15px;
            padding: 1rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(77, 237, 255, 0.2);
            box-shadow: 0 0 20px rgba(77, 237, 255, 0.1);
        }

        .control-btn {
            flex: 1;
            background: rgba(0, 0, 0, 0.3);
            border: none;
            border-radius: 8px;
            padding: 0.8rem;
            color: white;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            transition: all 0.3s ease;
        }

        .control-btn:hover {
            background: rgba(77, 237, 255, 0.2);
            transform: translateY(-2px);
        }

        .info-panel {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            flex-grow: 1; /* Permite que o painel de informações cresça */
        }

        .stellar-clock {
            background: rgba(26, 27, 65, 0.5);
            border-radius: 15px;
            padding: 1.5rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(77, 237, 255, 0.2);
            box-shadow: 0 0 20px rgba(77, 237, 255, 0.1);
        }

        .stellar-time {
            font-size: 3rem;
            font-family: 'Courier New', monospace;
            background: linear-gradient(90deg, var(--neon-blue), var(--neon-purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin: 0.5rem 0;
        }

        .stellar-date {
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.1rem;
            margin-bottom: 1rem;
        }

        .info-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.5rem;
        }

        .info-box {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 1.2rem;
            border-left: 3px solid var(--neon-blue);
        }

        .info-title {
            color: var(--neon-blue);
            margin-top: 0;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .planet-list {
            list-style: none;
            padding: 0;
            margin: 0.5rem 0;
        }

        .planet-list li {
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .planet-icon {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: inline-block;
        }

        /* Estilo para a seção de notícias DENTRO do painel de detalhes do país */
        .country-details-panel .news-section {
            background: rgba(0, 0, 0, 0.3); /* Um pouco mais escuro que o painel principal */
            border-radius: 10px;
            padding: 1.2rem;
            border-left: 3px solid var(--news-red);
            margin-top: 1.5rem; /* Espaçamento do conteúdo acima */
            flex-grow: 1; /* Permite que ocupe o espaço restante */
            display: flex;
            flex-direction: column;
        }

        .country-details-panel .news-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }

        .country-details-panel .news-title {
            color: var(--news-red);
            margin: 0;
            font-size: 1.4rem;
        }

        .country-details-panel .refresh-btn {
            background: rgba(255, 77, 77, 0.2);
            border: none;
            border-radius: 5px;
            padding: 0.5rem 1rem;
            color: white;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: all 0.3s ease;
        }

        .country-details-panel .refresh-btn:hover {
            background: rgba(255, 77, 77, 0.3);
            transform: translateY(-2px);
        }

        .country-details-panel .news-content {
            flex-grow: 1;
            overflow-y: auto; /* Notícias roláveis dentro do painel */
        }

        .country-details-panel .news-item {
            margin-bottom: 1.2rem;
            padding-bottom: 1.2rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .country-details-panel .news-item h3 {
            margin: 0 0 0.5rem 0;
            color: white;
            font-size: 1.1rem;
        }

        .country-details-panel .news-item p {
            margin: 0.3rem 0;
            color: rgba(255, 255, 255, 0.8);
            font-size: 0.95rem;
        }

        .country-details-panel .news-source {
            font-size: 0.85rem;
            color: var(--news-red);
            margin-top: 0.5rem;
            display: flex;
            justify-content: space-between;
        }

        .loading {
            display: none; /* Controlado por JS */
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 10;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 15px;
        }

        .loading.show {
            display: flex;
        }

        .loading-spinner {
            width: 50px;
            height: 50px;
            border: 5px solid rgba(77, 237, 255, 0.2);
            border-top-color: var(--neon-blue);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 1rem;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        footer {
            text-align: center;
            margin-top: 2rem;
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.9rem;
            padding: 1rem;
        }

        .api-note {
            margin-top: 0.5rem;
            font-size: 0.8rem;
        }

        /* Estilos para o LLM */
        .llm-feature {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 1.2rem;
            border-left: 3px solid var(--neon-purple); /* Cor diferente para LLM */
            margin-top: 1.5rem; /* Espaçamento */
        }

        .llm-feature .info-title {
            color: var(--neon-purple);
        }

        .llm-output {
            margin-top: 1rem;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 8px;
            font-style: italic;
            color: rgba(255, 255, 255, 0.9);
            min-height: 50px; /* Garante altura mínima */
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
        }

        .llm-output.loading-text {
            color: rgba(255, 255, 255, 0.6);
            font-style: normal;
        }

        .llm-button {
            background: linear-gradient(90deg, var(--neon-purple), var(--neon-blue));
            border: none;
            border-radius: 8px;
            padding: 0.8rem 1.2rem;
            color: white;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            transition: all 0.3s ease;
            margin-top: 1rem;
            width: 100%; /* Ocupa a largura total */
        }

        .llm-button:hover {
            opacity: 0.9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(132, 90, 255, 0.3);
        }

        .llm-button:disabled {
            background: #555;
            cursor: not-allowed;
            opacity: 0.6;
        }


        @media (max-width: 1200px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .info-grid {
                grid-template-columns: 1fr;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            /* Em telas menores, o map-container pode ter uma altura fixa para garantir visibilidade */
            .map-container {
                height: 500px; /* Altura fixa para telas menores */
                flex-grow: 0; /* Desabilita flex-grow quando a altura é fixa */
            }
            .country-details-panel { /* Ajuste para telas menores */
                position: relative;
                right: auto;
                top: auto;
                width: 100%;
                height: auto;
                opacity: 1;
                pointer-events: auto;
                margin-top: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="planet-decoration planet-1"></div>
            <div class="planet-decoration planet-2"></div>
            <h1><i class="fas fa-star"></i> CosmoNews - Mapa Estelar</h1>
            <p class="subtitle">Explore o universo, notícias em tempo real e o céu noturno</p>
            <div class="theme-toggle" id="theme-toggle">
                <i class="fas fa-moon"></i> Modo Noturno
            </div>
        </header>

        <div class="main-content">
            <div class="map-section">
                <div class="controls">
                    <button class="control-btn" id="zoom-in"><i class="fas fa-search-plus"></i> Zoom In</button>
                    <button class="control-btn" id="zoom-out"><i class="fas fa-search-minus"></i> Zoom Out</button>
                    <button class="control-btn" id="reset-map"><i class="fas fa-globe-americas"></i> Resetar Mapa</button>
                    <button class="control-btn" id="rotate-map"><i class="fas fa-sync-alt"></i> Rotacionar</button>
                </div>
                
                <div class="map-container">
                    <div id="world-map"></div>
                    <div class="map-country-hover-info" id="map-country-hover-info">
                        <img id="map-hover-flag" src="" alt="Bandeira">
                        <span id="map-hover-name"></span>
                    </div>
                    <div class="loading" id="map-loading">
                        <div class="loading-spinner"></div>
                        <p>Carregando mapa estelar...</p>
                    </div>
                </div>
                <div class="country-details-panel" id="country-details-panel">
                    <div class="flag-and-name">
                        <img id="details-flag" src="" alt="Bandeira do País">
                        <h3 id="details-country-name">Selecione um País</h3>
                    </div>
                    <p><span class="info-label">Capital:</span> <span id="details-capital">--</span></p>
                    <p><span class="info-label">População:</span> <span id="details-population">--</span></p>
                    <p><span class="info-label">Idioma:</span> <span id="details-language">--</span></p>
                    <p><span class="info-label">Moeda:</span> <span id="details-currency">--</span></p>
                    <p><span class="info-label">Coordenadas:</span> <span id="details-coords">--</span></p>
                    <p><span class="info-label">Fuso Horário:</span> <span id="details-timezone">--</span></p>

                    <div class="news-section">
                        <div class="news-header">
                            <h2 class="news-title"><i class="fas fa-newspaper"></i> Notícias em Tempo Real</h2>
                            <button class="refresh-btn" id="refresh-details-news">
                                <i class="fas fa-sync-alt"></i> Atualizar
                            </button>
                        </div>
                        <div class="news-content" id="details-news-content">
                            <p>Notícias do país aparecerão aqui.</p>
                        </div>
                        <button class="llm-button" id="summarize-news-btn" disabled>
                            <i class="fas fa-magic"></i> ✨ Resumir Notícias
                        </button>
                        <div class="llm-output" id="news-summary-output">
                            O resumo das notícias aparecerá aqui.
                        </div>
                    </div>
                    </div>
            </div>

            <div class="info-panel">
                <div class="stellar-clock">
                    <div class="stellar-date" id="current-date">2 de junho de 2025</div>
                    <div class="stellar-time" id="stellar-time">--:--:--</div>
                    <div id="location-info">Clique em um país no mapa para começar</div>
                </div>

                <div class="info-grid">
                    <div class="info-box">
                        <h3 class="info-title"><i class="fas fa-star"></i> Astronomia</h3>
                        <p><span class="info-label">Hora Sideral:</span> <span id="sidereal-time">--:--:--</span></p>
                        <p><span class="info-label">Constelação:</span> <span id="constellation">--</span></p>
                        <p><span class="info-label">Altitude Solar:</span> <span id="sun-altitude">--°</span></p>
                        <p><span class="info-label">Fase Lunar:</span> <span id="moon-phase">--</span></p>
                        <button class="llm-button" id="explore-constellation-btn" disabled>
                            <i class="fas fa-brain"></i> ✨ Explorar Constelação
                        </button>
                        <div class="llm-output" id="constellation-details">
                            Detalhes da constelação aparecerão aqui.
                        </div>
                    </div>

                    <div class="info-box">
                        <h3 class="info-title"><i class="fas fa-cloud-moon"></i> Planetas Visíveis</h3>
                        <ul class="planet-list" id="visible-planets">
                            <li>Carregando...</li>
                        </ul>
                    </div>

                    <div class="info-box">
                        <h3 class="info-title"><i class="fas fa-meteor"></i> Chuvas de Meteoros</h3>
                        <div id="meteor-showers">
                            <p>Nenhuma chuva de meteoros ativa</p>
                        </div>
                    </div>

                    <div class="info-box">
                        <h3 class="info-title"><i class="fas fa-info-circle"></i> Informações do País</h3>
                        <p><span class="info-label">Fuso Horário:</span> <span id="timezone">--</span></p>
                        <p><span class="info-label">População:</span> <span id="population">--</span></p>
                        <p><span class="info-label">Idioma:</span> <span id="language">--</span></p>
                        <p><span class="info-label">Moeda:</span> <span id="currency">--</span></p>
                    </div>
                </div>

                <div class="llm-feature">
                    <h3 class="info-title"><i class="fas fa-lightbulb"></i> Curiosidade Espacial do Dia</h3>
                    <div class="llm-output" id="space-fact-output">
                        Clique no botão para gerar uma curiosidade!
                    </div>
                    <button class="llm-button" id="generate-space-fact-btn">
                        <i class="fas fa-rocket"></i> ✨ Gerar Curiosidade
                    </button>
                </div>

                </div>
        </div>

        <footer>
            <p>CosmoNews © 2025 | Dados astronômicos por Astronomy Engine | Notícias por NewsAPI</p>
            <p class="api-note">Nota: Esta demonstração usa uma chave de API de teste. Para funcionar, você precisa de uma chave da NewsAPI e o aplicativo deve ser executado em 'localhost' ou em um plano pago da NewsAPI.</p>
        </footer>
    </div>

    <script>
        // Dados dos países
        // IMPORTANTE: Para que a bandeira e as informações detalhadas apareçam para mais países,
        // você precisa adicionar os dados correspondentes aqui.
        // O 'countryCode' (ex: "BR", "US") deve ser o código ISO 3166-1 alpha-2.
        // As constelações são apenas exemplos e podem não corresponder à visibilidade real de cada país.
        const countriesData = {
            "BR": { 
                name: "Brasil", 
                capital: "Brasília", 
                lat: -15.78, 
                lon: -47.93, 
                timezone: "UTC-3",
                population: "214 milhões",
                language: "Português",
                currency: "Real (BRL)",
                constellations: ["Cruzeiro do Sul", "Escorpião", "Centauro"]
            },
            "US": { 
                name: "Estados Unidos", 
                capital: "Washington, D.C.", 
                lat: 38.90, 
                lon: -77.04, 
                timezone: "UTC-4 a UTC-10",
                population: "331 milhões",
                language: "Inglês",
                currency: "Dólar (USD)",
                constellations: ["Órion", "Ursa Maior", "Touro"]
            },
            "GB": { 
                name: "Reino Unido", 
                capital: "Londres", 
                lat: 51.50, 
                lon: -0.12, 
                timezone: "UTC+1",
                population: "67 milhões",
                language: "Inglês",
                currency: "Libra Esterlina (GBP)",
                constellations: ["Cassiopeia", "Ursa Menor", "Pégaso"]
            },
            "JP": {
                name: "Japão",
                capital: "Tóquio",
                lat: 35.68,
                lon: 139.69,
                timezone: "UTC+9",
                population: "125 milhões",
                language: "Japonês",
                currency: "Iene (JPY)",
                constellations: ["Ursa Maior", "Órion", "Touro"]
            },
            "AU": {
                name: "Austrália",
                capital: "Camberra",
                lat: -35.28,
                lon: 149.13,
                timezone: "UTC+10",
                population: "26 milhões",
                language: "Inglês",
                currency: "Dólar Australiano (AUD)",
                constellations: ["Cruzeiro do Sul", "Centauro", "Escorpião"]
            },
            "CA": {
                name: "Canadá",
                capital: "Ottawa",
                lat: 45.42,
                lon: -75.69,
                timezone: "UTC-5 a UTC-8",
                population: "38 milhões",
                language: "Inglês, Francês",
                currency: "Dólar Canadense (CAD)",
                constellations: ["Ursa Menor", "Cassiopeia", "Cefeu"]
            },
            "DE": {
                name: "Alemanha",
                capital: "Berlim",
                lat: 52.52,
                lon: 13.40,
                timezone: "UTC+2",
                population: "83 milhões",
                language: "Alemão",
                currency: "Euro (EUR)",
                constellations: ["Ursa Maior", "Cefeu", "Lira"]
            },
            "FR": {
                name: "França",
                capital: "Paris",
                lat: 48.85,
                lon: 2.35,
                timezone: "UTC+2",
                population: "65 milhões",
                language: "Francês",
                currency: "Euro (EUR)",
                constellations: ["Órion", "Cão Maior", "Gêmeos"]
            },
            "IN": { // Índia
                name: "Índia",
                capital: "Nova Delhi",
                lat: 28.61,
                lon: 77.20,
                timezone: "UTC+5:30",
                population: "1.4 bilhões",
                language: "Hindi, Inglês",
                currency: "Rúpia Indiana (INR)",
                constellations: ["Ursa Maior", "Órion", "Escorpião"]
            },
            "CN": { // China
                name: "China",
                capital: "Pequim",
                lat: 39.90,
                lon: 116.40,
                timezone: "UTC+8",
                population: "1.4 bilhões",
                language: "Mandarim",
                currency: "Yuan (CNY)",
                constellations: ["Ursa Maior", "Dragão", "Cisne"]
            },
            "RU": { // Rússia
                name: "Rússia",
                capital: "Moscou",
                lat: 55.75,
                lon: 37.61,
                timezone: "UTC+2 a UTC+12",
                population: "144 milhões",
                language: "Russo",
                currency: "Rublo Russo (RUB)",
                constellations: ["Ursa Maior", "Ursa Menor", "Cefeu"]
            },
            "MX": { // México
                name: "México",
                capital: "Cidade do México",
                lat: 19.43,
                lon: -99.13,
                timezone: "UTC-5 a UTC-8",
                population: "126 milhões",
                language: "Espanhol",
                currency: "Peso Mexicano (MXN)",
                constellations: ["Órion", "Touro", "Escorpião"]
            },
            "ZA": { // África do Sul (já estava, mas bom para referência)
                name: "África do Sul",
                capital: "Pretória",
                lat: -25.74,
                lon: 28.22,
                timezone: "UTC+2",
                population: "60 milhões",
                language: "Africâner, Inglês, Zulu, Xhosa...",
                currency: "Rand (ZAR)",
                constellations: ["Cruzeiro do Sul", "Centauro", "Carina"]
            },
            "EG": { // Egito (já estava, mas bom para referência)
                name: "Egito",
                capital: "Cairo",
                lat: 30.04,
                lon: 31.23,
                timezone: "UTC+2",
                population: "109 milhões",
                language: "Árabe",
                currency: "Libra Egípcia (EGP)",
                constellations: ["Órion", "Touro", "Leão"]
            },
            "IT": { // Itália
                name: "Itália",
                capital: "Roma",
                lat: 41.90,
                lon: 12.49,
                timezone: "UTC+2",
                population: "59 milhões",
                language: "Italiano",
                currency: "Euro (EUR)",
                constellations: ["Ursa Maior", "Órion", "Touro"]
            },
            "ES": { // Espanha
                name: "Espanha",
                capital: "Madri",
                lat: 40.41,
                lon: -3.70,
                timezone: "UTC+2",
                population: "47 milhões",
                language: "Espanhol",
                currency: "Euro (EUR)",
                constellations: ["Ursa Maior", "Órion", "Touro"]
            },
            "NG": { // Nigéria
                name: "Nigéria",
                capital: "Abuja",
                lat: 9.07,
                lon: 7.49,
                timezone: "UTC+1",
                population: "218 milhões",
                language: "Inglês, Hauçá, Iorubá, Igbo",
                currency: "Naira (NGN)",
                constellations: ["Órion", "Cão Maior", "Leão"]
            },
            "KE": { // Quênia
                name: "Quênia",
                capital: "Nairóbi",
                lat: -1.28,
                lon: 36.82,
                timezone: "UTC+3",
                population: "55 milhões",
                language: "Suaíli, Inglês",
                currency: "Xelim Queniano (KES)",
                constellations: ["Cruzeiro do Sul", "Centauro", "Órion"]
            },
            "MA": { // Marrocos
                name: "Marrocos",
                capital: "Rabat",
                lat: 34.02,
                lon: -6.83,
                timezone: "UTC+1",
                population: "37 milhões",
                language: "Árabe, Berber",
                currency: "Dirham Marroquino (MAD)",
                constellations: ["Ursa Maior", "Órion", "Touro"]
            },
            "DZ": { // Argélia
                name: "Argélia",
                capital: "Argel",
                lat: 36.75,
                lon: 3.04,
                timezone: "UTC+1",
                population: "45 milhões",
                language: "Árabe, Berber",
                currency: "Dinar Argelino (DZD)",
                constellations: ["Órion", "Touro", "Leão"]
            },
            "GH": { // Gana
                name: "Gana",
                capital: "Acra",
                lat: 5.55,
                lon: -0.20,
                timezone: "UTC+0",
                population: "32 milhões",
                language: "Inglês",
                currency: "Cedi Ganês (GHS)",
                constellations: ["Órion", "Cão Maior"]
            },
            "AO": { // Angola
                name: "Angola",
                capital: "Luanda",
                lat: -8.83,
                lon: 13.23,
                timezone: "UTC+1",
                population: "34 milhões",
                language: "Português",
                currency: "Kwanza (AOA)",
                constellations: ["Cruzeiro do Sul", "Centauro"]
            },
            "MZ": { // Moçambique
                name: "Moçambique",
                capital: "Maputo",
                lat: -25.96,
                lon: 32.57,
                timezone: "UTC+2",
                population: "32 milhões",
                language: "Português",
                currency: "Metical Moçambicano (MZN)",
                constellations: ["Cruzeiro do Sul", "Centauro"]
            },
            "CD": { // República Democrática do Congo
                name: "República Democrática do Congo",
                capital: "Kinshasa",
                lat: -4.44,
                lon: 15.26,
                timezone: "UTC+1 a UTC+2",
                population: "95 milhões",
                language: "Francês, Lingala, Quicongo, Suaíli, Tshiluba",
                currency: "Franco Congolês (CDF)",
                constellations: ["Cruzeiro do Sul", "Órion"]
            }
        };

        // Mapeamento de IDs do TopoJSON para os códigos de país em countriesData
        // IMPORTANTE: Você precisa adicionar o mapeamento aqui para cada país que adicionar em `countriesData`.
        // Os IDs numéricos podem ser encontrados nos dados do world-atlas (ou inspecionando o 'd.id' no console).
        const topoJsonIdToCountryCodeMap = {
            "076": "BR", // Brasil
            "840": "US", // Estados Unidos
            "826": "GB", // Reino Unido
            "392": "JP", // Japão
            "036": "AU", // Austrália
            "124": "CA", // Canadá
            "276": "DE", // Alemanha
            "250": "FR",  // França
            "356": "IN",  // Índia
            "156": "CN",  // China
            "643": "RU",  // Rússia
            "484": "MX",  // México
            "710": "ZA",  // África do Sul
            "818": "EG",  // Egito
            "380": "IT",  // Itália
            "724": "ES",   // Espanha
            "566": "NG",  // Nigéria
            "404": "KE",  // Quênia
            "504": "MA",  // Marrocos
            "012": "DZ",   // Argélia
            "288": "GH",  // Gana
            "024": "AO",  // Angola
            "508": "MZ",  // Moçambique
            "180": "CD"   // República Democrática do Congo
            // EXEMPO: Adicione mais mapeamentos aqui (você precisará do ID TopoJSON para cada país)
            // "032": "AR" // Argentina
        };

        // Dados das constelações (mantidos)
        const constellationsData = {
            "Cruzeiro do Sul": {
                stars: ["Acrux", "Mimosa", "Gacrux"],
                color: "#FFD700"
            },
            "Órion": {
                stars: ["Betelgeuse", "Rigel", "Bellatrix"],
                color: "#ADD8E6"
            },
            "Ursa Maior": {
                stars: ["Dubhe", "Merak", "Alioth"],
                color: "#90EE90"
            },
            "Escorpião": {
                stars: ["Antares", "Shaula", "Sargas"],
                color: "#FF6347"
            },
            "Centauro": {
                stars: ["Alpha Centauri", "Hadar", "Menkent"],
                color: "#DA70D6"
            },
            "Touro": {
                stars: ["Aldebaran", "El Nath", "Alcyone"],
                color: "#FFA07A"
            },
            "Ursa Menor": {
                stars: ["Polaris", "Kochab", "Pherkad"],
                color: "#B0E0E6"
            },
            "Cassiopeia": {
                stars: ["Shedar", "Caph", "Ruchbah"],
                color: "#FFC0CB"
            },
            "Cefeu": {
                stars: ["Alderamin", "Alfirk", "Errai"],
                color: "#87CEFA"
            },
            "Cão Maior": {
                stars: ["Sirius", "Adhara", "Wezen"],
                color: "#F0E68C"
            },
            "Pégaso": {
                stars: ["Markab", "Scheat", "Algenib"],
                color: "#DDA0DD"
            },
            "Lira": {
                stars: ["Vega", "Sheliak", "Sulafat"],
                color: "#AFEEEE"
            },
            "Gêmeos": {
                stars: ["Castor", "Pollux", "Alhena"],
                color: "#FFDAB9"
            },
            "Dragão": {
                stars: ["Thuban", "Etamin", "Rastaban"],
                color: "#C0C0C0"
            },
            "Cisne": {
                stars: ["Deneb", "Albireo", "Sadr"],
                color: "#ADD8E6"
            },
            "Carina": {
                stars: ["Canopus", "Miaplacidus", "Avior"],
                color: "#FFFACD"
            },
            "Leão": {
                stars: ["Regulus", "Denebola", "Algieba"],
                color: "#F0E68C"
            }
        };

        // Dados de planetas visíveis (simulados)
        const visiblePlanetsData = [
            { name: "Vênus", color: "#FFD700" },
            { name: "Marte", color: "#FF4500" },
            { name: "Júpiter", color: "#DAA520" },
            { name: "Saturno", color: "#BDB76B" }
        ];

        // Dados de chuvas de meteoros (simulados)
        const meteorShowersData = [
            { name: "Líridas", active: false, peak: "22 de Abr" },
            { name: "Eta Aquáridas", active: true, peak: "6 de Mai" },
            { name: "Perseidas", active: false, peak: "12 de Ago" }
        ];

        // Variáveis globais para o mapa D3
        let svgMap, projection, pathGenerator, zoomBehavior, rotationInterval;
        let currentRotation = 0;
        let activeCountryCode = null; // Para rastrear o país atualmente selecionado (clicado)
        let currentTransform = d3.zoomIdentity; // Inicializa a transformação do zoom
        let currentHoveredCountryD3Datum = null; // Armazena o datum do país atualmente hovered

        // Elementos do DOM para o painel de detalhes do país (no clique)
        const countryDetailsPanel = document.getElementById('country-details-panel');
        const detailsFlag = document.getElementById('details-flag');
        const detailsCountryName = document.getElementById('details-country-name');
        const detailsCapital = document.getElementById('details-capital');
        const detailsPopulation = document.getElementById('details-population');
        const detailsLanguage = document.getElementById('details-language');
        const detailsCurrency = document.getElementById('details-currency');
        const detailsCoords = document.getElementById('details-coords');
        const detailsTimezone = document.getElementById('details-timezone');
        const detailsNewsContent = document.getElementById('details-news-content'); // Novo elemento para notícias no painel de detalhes
        const refreshDetailsNewsBtn = document.getElementById('refresh-details-news'); // Novo botão de atualização de notícias

        // Elementos do DOM para a caixa de informações SIMPLIFICADAS com bandeira no mapa (no hover)
        const mapCountryHoverInfo = document.getElementById('map-country-hover-info');
        const mapHoverFlag = document.getElementById('map-hover-flag');
        const mapHoverName = document.getElementById('map-hover-name');


        // Elementos do DOM para as novas funcionalidades LLM
        const spaceFactOutput = document.getElementById('space-fact-output');
        const generateSpaceFactBtn = document.getElementById('generate-space-fact-btn');
        const constellationDetails = document.getElementById('constellation-details');
        const exploreConstellationBtn = document.getElementById('explore-constellation-btn');
        const summarizeNewsBtn = document.getElementById('summarize-news-btn'); // Novo botão para resumir notícias
        const newsSummaryOutput = document.getElementById('news-summary-output'); // Novo elemento para o resumo das notícias

        // URL de imagem de placeholder para bandeiras ausentes
        const PLACEHOLDER_FLAG_URL = 'https://placehold.co/24x18/cccccc/333333?text=N/A'; // Ajustado para o novo tamanho do hover


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
                // Não retorna imediatamente para que o ResizeObserver possa tentar novamente
                mapLoading.classList.remove('show'); // Esconde o loading se as dimensões ainda forem zero
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
                // Tenta carregar o mapa da CDN
                const world = await d3.json("https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json");

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
                // Se o svgMap for recriado, o zoomBehavior precisa ser reaplicado.
                // Resetar o zoomBehavior para o novo SVG
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
                const defaultPath = d3.select(`#country-${defaultCountryTopoId}`);
                if (!defaultPath.empty()) {
                    defaultPath.dispatch("click");
                } else {
                    console.warn(`País com ID TopoJSON ${defaultCountryTopoId} (Brasil) não encontrado para seleção padrão.`);
                    // Fallback para o primeiro país disponível se o padrão não for encontrado
                    const firstAvailableTopoFeature = topojson.feature(world, world.objects.countries).features[0];
                    if (firstAvailableTopoFeature) {
                        d3.select(`#country-${firstAvailableTopoFeature.id}`).dispatch("click");
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
            // Adiciona um preenchimento (padding) para que o país não ocupe 100% da tela
            const scale = Math.max(1, Math.min(8, 0.9 / Math.max(dx / width, dy / height))); // Limita o zoom máximo a 8
            const translate = [width / 2 - scale * x, height / 2 - scale * y];

            // Aplica a transformação de zoom e pan
            svgMap.transition()
                .duration(750) // Transição suave
                .call(zoomBehavior.transform, d3.zoomIdentity.translate(translate[0], translate[1]).scale(scale));
        }


        // Funções de controle do mapa
        function zoomIn() {
            if (svgMap) svgMap.transition().duration(750).call(zoomBehavior.scaleBy, 2);
        }

        function zoomOut() {
            if (svgMap) svgMap.transition().duration(750).call(zoomBehavior.scaleBy, 0.5);
        }

        function resetMap() {
            if (svgMap) svgMap.transition().duration(750).call(zoomBehavior.transform, d3.zoomIdentity);
            // Redefine a rotação
            currentRotation = 0;
            if (projection) projection.rotate([currentRotation, 0]);
            if (svgMap) svgMap.selectAll("path").attr("d", pathGenerator);
            // Remove seleção de país
            d3.selectAll(".country").classed("selected", false)
                .attr("fill", "rgba(100, 149, 237, 0.4)")
                .attr("stroke", "rgba(255, 255, 255, 0.2)")
                .attr("stroke-width", 0.5);
            activeCountryCode = null;
            updateCountryInfo(null); // Limpa as informações do painel principal
            if (rotationInterval) { // Pausa a rotação se estiver ativa
                clearInterval(rotationInterval);
                rotationInterval = null;
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            }
            hideCountryDetails(); // Esconde o painel de detalhes lateral
            hideMapCountryHoverInfo(); // Esconde a caixa de detalhes no mapa
            currentHoveredCountryD3Datum = null; // Limpa o datum hovered
            // Limpa os outputs do LLM
            spaceFactOutput.textContent = 'Clique no botão para gerar uma curiosidade!';
            constellationDetails.textContent = 'Detalhes da constelação aparecerão aqui.';
            exploreConstellationBtn.disabled = true;
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.'; // Limpa o resumo
            summarizeNewsBtn.disabled = true; // Desabilita o botão de resumo
        }

        function rotateMap() {
            if (rotationInterval) {
                clearInterval(rotationInterval);
                rotationInterval = null;
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            } else {
                rotationInterval = setInterval(() => {
                    currentRotation += 0.5; // Velocidade de rotação
                    if (currentRotation > 360) currentRotation -= 360;
                    if (projection) projection.rotate([currentRotation, 0]);
                    if (svgMap) svgMap.selectAll("path").attr("d", pathGenerator);
                }, 50); // Intervalo de rotação
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-pause"></i> Pausar Rotação';
            }
        }

        // Função para mostrar a caixa de informações SIMPLIFICADAS no mapa ao lado do país (no hover)
        function showMapCountryHoverInfo(countryCode, countryNameFromTopoJson, d3Event, d3Datum) {
            // Não precisamos de countryData completo aqui, apenas o nome e o código para a bandeira
            const displayCountryName = countryNameFromTopoJson || "País Desconhecido";
            const flagSrc = countryCode ? `https://flagcdn.com/w20/${countryCode.toLowerCase()}.png` : PLACEHOLDER_FLAG_URL;

            mapHoverFlag.src = flagSrc;
            mapHoverFlag.onerror = () => { mapHoverFlag.src = PLACEHOLDER_FLAG_URL; }; // Fallback para bandeira
            mapHoverName.textContent = displayCountryName;
            
            // Calcula a posição do centro do país no SVG
            const centroid = pathGenerator.centroid(d3Datum);
            
            // Obtém a posição do container do mapa na tela
            const mapContainer = document.getElementById('world-map');
            const mapRect = mapContainer.getBoundingClientRect();

            // Ajusta o centro do país pela transformação atual do zoom/pan
            const transformedCentroid = currentTransform.apply(centroid);

            // Posiciona o elemento div (map-country-hover-info) relativo ao container do mapa
            // Adiciona um pequeno offset para não ficar exatamente em cima do país
            mapCountryHoverInfo.style.left = (transformedCentroid[0] + mapRect.left + window.scrollX + 20) + 'px'; // Offset à direita
            mapCountryHoverInfo.style.top = (transformedCentroid[1] + mapRect.top + window.scrollY - mapCountryHoverInfo.offsetHeight / 2) + 'px'; // Centraliza verticalmente

            mapCountryHoverInfo.classList.add('show');
        }

        // Função para esconder a caixa de informações SIMPLIFICADAS no mapa (no hover)
        function hideMapCountryHoverInfo() {
            mapCountryHoverInfo.classList.remove('show');
        }

        // Função para exibir o painel de detalhes do país (AGORA SÓ NO CLIQUE)
        function displayCountryDetails(countryCode, countryNameFromTopoJson) {
            const country = countriesData[countryCode];
            const displayCountryName = countryNameFromTopoJson || "País Desconhecido";

            detailsFlag.src = countryCode ? `https://flagcdn.com/w40/${countryCode.toLowerCase()}.png` : PLACEHOLDER_FLAG_URL;
            detailsFlag.alt = `${displayCountryName} Flag`;
            detailsFlag.onerror = () => { detailsFlag.src = PLACEHOLDER_FLAG_URL; }; // Fallback para bandeira
            detailsCountryName.textContent = displayCountryName;
            detailsCapital.textContent = country ? country.capital : "--";
            detailsPopulation.textContent = country ? country.population : "--";
            detailsLanguage.textContent = country ? country.language : "--";
            detailsCurrency.textContent = country ? country.currency : "--";
            detailsCoords.textContent = country ? `${country.lat}°N, ${Math.abs(country.lon)}°${country.lon < 0 ? 'W' : 'E'}` : "--";
            detailsTimezone.textContent = country ? country.timezone : "--";
            countryDetailsPanel.classList.add('show');

            // Chamar a função de busca de notícias para o painel de detalhes
            fetchNews(country.name, detailsNewsContent);
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.'; // Limpa o resumo ao selecionar novo país
            summarizeNewsBtn.disabled = true; // Desabilita o botão até que as notícias sejam carregadas
        }

        // Função para esconder o painel de detalhes do país
        function hideCountryDetails() {
            countryDetailsPanel.classList.remove('show');
        }


        // Atualizar informações do país selecionado (painel principal)
        function updateCountryInfo(countryCode) {
            const country = countriesData[countryCode];
            if (!country) {
                // Se nenhum país mapeado ou um país não mapeado foi clicado
                document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { day: 'numeric', month: 'long', year: 'numeric' });
                document.getElementById('stellar-time').textContent = '--:--:--';
                document.getElementById('location-info').textContent = 'Clique em um país no mapa para começar';
                document.getElementById('sidereal-time').textContent = '--:--:--';
                document.getElementById('constellation').textContent = '--';
                document.getElementById('sun-altitude').textContent = '--°';
                document.getElementById('moon-phase').textContent = '--';
                document.getElementById('timezone').textContent = '--';
                document.getElementById('population').textContent = '--';
                document.getElementById('language').textContent = '--';
                document.getElementById('currency').textContent = '--';
                detailsNewsContent.innerHTML = '<p>Notícias do país aparecerão aqui.</p>'; // Limpa notícias no painel de detalhes
                document.getElementById('visible-planets').innerHTML = '<li>Carregando...</li>'; // Reseta planetas
                document.getElementById('meteor-showers').innerHTML = '<p>Nenhuma chuva de meteoros ativa</p>'; // Reseta chuvas de meteoros
                
                exploreConstellationBtn.disabled = true; // Desabilita o botão se não houver país
                constellationDetails.textContent = 'Detalhes da constelação aparecerão aqui.'; // Limpa detalhes
                newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.'; // Limpa o resumo
                summarizeNewsBtn.disabled = true; // Desabilita o botão de resumo
                return;
            }
            
            // Atualizar informações básicas
            document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { 
                day: 'numeric', 
                month: 'long', 
                year: 'numeric' 
            });
            
            document.getElementById('location-info').textContent = `${country.capital}, ${country.name} (${country.timezone})`;
            
            // Calcular hora estelar com Astronomy Engine
            const now = new Date();
            // Verifica se window.Astronomy está disponível
            if (typeof window.Astronomy !== 'undefined') {
                const observer = new window.Astronomy.Observer(country.lat, country.lon, 0);
                const siderealTime = window.Astronomy.SiderealTime(observer);
                
                const siderealHours = Math.floor(siderealTime);
                const siderealMinutes = Math.floor((siderealTime - siderealHours) * 60);
                const siderealSeconds = Math.floor((((siderealTime - siderealHours) * 60) - siderealMinutes) * 60);
                
                document.getElementById('sidereal-time').textContent = 
                    `${siderealHours.toString().padStart(2, '0')}:${siderealMinutes.toString().padStart(2, '0')}:${siderealSeconds.toString().padStart(2, '0')}`;
                
                // Posição do Sol e Lua
                const sunPos = window.Astronomy.Equator('Sun', now, observer, true, true);
                const moonPhase = window.Astronomy.MoonPhase(now);
                
                let phaseText = "";
                if (moonPhase < 0.125 || moonPhase >= 0.875) phaseText = "Lua Nova";
                else if (moonPhase >= 0.125 && moonPhase < 0.375) phaseText = "Quarto Crescente";
                else if (moonPhase >= 0.375 && moonPhase < 0.625) phaseText = "Lua Cheia";
                else if (moonPhase >= 0.625 && moonPhase < 0.875) phaseText = "Quarto Minguante";
                
                document.getElementById('sun-altitude').textContent = `${sunPos.altitude.toFixed(1)}°`;
                document.getElementById('moon-phase').textContent = `${phaseText} (${(moonPhase * 100).toFixed(1)}%)`;

            } else {
                console.warn("Astronomy Engine não carregada ou disponível globalmente.");
                document.getElementById('sidereal-time').textContent = '--:--:--';
                document.getElementById('sun-altitude').textContent = '--°';
                document.getElementById('moon-phase').textContent = '--';
            }

            // Hora local (ajustada pelo fuso horário do país)
            const localTime = new Date();
            // Para simular o fuso horário do país, precisamos ajustar a hora UTC
            // Exemplo simples: assume que o timezone é um offset em horas de UTC.
            // Para fusos horários complexos, uma biblioteca como moment-timezone seria necessária.
            const offsetHours = parseInt(country.timezone.replace('UTC', '').replace('+', ''));
            localTime.setUTCHours(localTime.getUTCHours() + offsetHours);
            document.getElementById('stellar-time').textContent = localTime.toLocaleTimeString('pt-BR', { hour12: false });
            
            // Constelação principal
            document.getElementById('constellation').textContent = country.constellations[0];
            exploreConstellationBtn.disabled = false; // Habilita o botão

            // Informações adicionais do país
            document.getElementById('timezone').textContent = country.timezone;
            document.getElementById('population').textContent = country.population;
            document.getElementById('language').textContent = country.language;
            document.getElementById('currency').textContent = country.currency;

            // Atualizar planetas visíveis (simulação)
            updateVisiblePlanets();
            // Atualizar chuvas de meteoros (simulação)
            updateMeteorShowers();

            // A busca de notícias agora é feita em displayCountryDetails
            // fetchNews(country.name); // Removido daqui
        }

        // Função para atualizar planetas visíveis
        function updateVisiblePlanets() {
            const visiblePlanetsList = document.getElementById('visible-planets');
            visiblePlanetsList.innerHTML = ''; // Limpa a lista

            visiblePlanetsData.forEach(planet => {
                const li = document.createElement('li');
                li.innerHTML = `<span class="planet-icon" style="background-color: ${planet.color};"></span> ${planet.name}`;
                visiblePlanetsList.appendChild(li);
            });
        }

        // Função para atualizar chuvas de meteoros
        function updateMeteorShowers() {
            const meteorShowersDiv = document.getElementById('meteor-showers');
            meteorShowersDiv.innerHTML = ''; // Limpa o conteúdo

            const activeShowers = meteorShowersData.filter(shower => shower.active);

            if (activeShowers.length > 0) {
                activeShowers.forEach(shower => {
                    const p = document.createElement('p');
                    p.innerHTML = `<span class="info-label">${shower.name}:</span> Pico em ${shower.peak}`;
                    meteorShowersDiv.appendChild(p);
                });
            } else {
                meteorShowersDiv.innerHTML = '<p>Nenhuma chuva de meteoros ativa</p>';
            }
        }

        // Função para buscar notícias da NewsAPI
        // Agora aceita um elemento de destino para as notícias
        async function fetchNews(query, targetElement) {
            targetElement.innerHTML = '<p>Carregando notícias...</p>'; // Mensagem de carregamento
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.'; // Limpa o resumo anterior
            summarizeNewsBtn.disabled = true; // Desabilita o botão enquanto carrega

            // **IMPORTANTE: Substitua 'SUA_API_KEY_DA_NEWSAPI' pela sua chave real da NewsAPI.**
            // Você pode obter uma chave gratuita em: https://newsapi.org/
            const NEWS_API_KEY = 'SUA_API_KEY_DA_NEWSAPI'; 

            // Verifica se a chave da API é a placeholder
            if (NEWS_API_KEY === 'SUA_API_KEY_DA_NEWSAPI' || NEWS_API_KEY === '') {
                targetElement.innerHTML = `
                    <p style="color: var(--news-red);">
                        As notícias não podem ser carregadas.
                        <br><br>
                        **Causas comuns e soluções:**
                        <br>
                        1.  **Chave de API ausente ou de demonstração:** Você precisa obter sua própria chave de API gratuita em <a href="https://newsapi.org/" target="_blank" style="color: var(--neon-blue); text-decoration: underline;">newsapi.org</a> e substituir 'SUA_API_KEY_DA_NEWSAPI' no código.
                        <br>
                        2.  **Executando fora do localhost:** O plano de desenvolvedor da NewsAPI só permite requisições diretas do navegador se o aplicativo estiver sendo executado em 'localhost'. Por favor, execute o aplicativo em um servidor local (ex: \`python -m http.server\` no terminal e acesse \`http://localhost:8000\`).
                        <br>
                        3.  **Limite de requisições:** Você pode ter atingido o limite de requisições diárias da NewsAPI.
                    </p>
                `;
                console.error("Erro NewsAPI: Chave de API de demonstração ou ausente/inválida, ou aplicativo não está em localhost. Verifique as instruções no código.");
                return; // Sai da função para não tentar fazer a requisição
            }

            // Usando encodeURIComponent para garantir que a query seja formatada corretamente para a URL
            const url = `https://newsapi.org/v2/everything?q=${encodeURIComponent(query)}&language=pt&sortBy=relevancy&apiKey=${NEWS_API_KEY}`;

            try {
                const response = await fetch(url);
                const data = await response.json();

                if (data.status === 'ok' && data.articles.length > 0) {
                    targetElement.innerHTML = ''; // Limpa o conteúdo anterior
                    data.articles.slice(0, 5).forEach(article => { // Limita a 5 notícias
                        const newsItem = document.createElement('div');
                        newsItem.classList.add('news-item');
                        newsItem.innerHTML = `
                            <h3><a href="${article.url}" target="_blank">${article.title}</a></h3>
                            <p>${article.description || 'Sem descrição.'}</p>
                            <div class="news-source">
                                <span>${article.source.name}</span>
                                <span>${new Date(article.publishedAt).toLocaleDateString('pt-BR')}</span>
                            </div>
                        `;
                        targetElement.appendChild(newsItem);
                    });
                    summarizeNewsBtn.disabled = false; // Habilita o botão de resumo se houver notícias
                } else {
                    targetElement.innerHTML = `<p>Nenhuma notícia encontrada para "${query}".</p>`;
                    if (data.message) {
                        console.error("Erro na NewsAPI:", data.message);
                        // Mensagem mais específica para o erro de localhost
                        if (data.message.includes("Requests from the browser are not allowed on the Developer plan, except from localhost")) {
                             targetElement.innerHTML += `<p style="color: var(--news-red); font-size: 0.8rem;">Erro da API: ${data.message}. Por favor, execute o aplicativo em 'localhost' ou obtenha um plano pago da NewsAPI.</p>`;
                        } else {
                            targetElement.innerHTML += `<p style="color: var(--news-red); font-size: 0.8rem;">Erro da API: ${data.message}</p>`;
                        }
                    }
                    summarizeNewsBtn.disabled = true; // Desabilita o botão se não houver notícias
                }
            } catch (error) {
                console.error("Erro ao buscar notícias:", error);
                targetElement.innerHTML = `<p style="color: var(--news-red);">Erro ao carregar notícias. Verifique sua conexão ou chave da API.</p>`;
                summarizeNewsBtn.disabled = true; // Desabilita o botão em caso de erro
            }
        }

        // Função para gerar uma curiosidade espacial usando a API Gemini
        async function generateSpaceFact() {
            spaceFactOutput.classList.add('loading-text');
            spaceFactOutput.textContent = 'Gerando curiosidade...';
            generateSpaceFactBtn.disabled = true;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: "Gere uma curiosidade interessante e concisa sobre o espaço ou astronomia. Não inclua títulos ou formatação Markdown." }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas injetará a chave da API em tempo de execução
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    spaceFactOutput.textContent = text;
                } else {
                    spaceFactOutput.textContent = 'Não foi possível gerar a curiosidade. Tente novamente.';
                    console.error("Erro na resposta da API Gemini:", result);
                }
            } catch (error) {
                spaceFactOutput.textContent = 'Erro ao conectar com a IA. Verifique sua conexão.';
                console.error("Erro ao chamar a API Gemini para curiosidade:", error);
            } finally {
                spaceFactOutput.classList.remove('loading-text');
                generateSpaceFactBtn.disabled = false;
            }
        }

        // Função para explorar detalhes da constelação usando a API Gemini
        async function exploreConstellation() {
            const currentConstellation = document.getElementById('constellation').textContent;
            if (currentConstellation === '--' || !activeCountryCode) {
                constellationDetails.textContent = 'Selecione um país para explorar sua constelação principal.';
                return;
            }

            constellationDetails.classList.add('loading-text');
            constellationDetails.textContent = `Explorando ${currentConstellation}...`;
            exploreConstellationBtn.disabled = true;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: `Forneça informações interessantes e concisas sobre a constelação ${currentConstellation}, incluindo sua mitologia e como encontrá-la no céu. Não inclua títulos ou formatação Markdown.` }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas injetará a chave da API em tempo de execução
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    constellationDetails.textContent = text;
                } else {
                    constellationDetails.textContent = 'Não foi possível obter detalhes da constelação. Tente novamente.';
                    console.error("Erro na resposta da API Gemini para constelação:", result);
                }
            } catch (error) {
                constellationDetails.textContent = 'Erro ao conectar com a IA. Verifique sua conexão.';
                console.error("Erro ao chamar a API Gemini para constelação:", error);
            } finally {
                constellationDetails.classList.remove('loading-text');
                exploreConstellationBtn.disabled = false;
            }
        }

        // Função para resumir notícias usando a API Gemini
        async function summarizeNews(countryName) {
            newsSummaryOutput.classList.add('loading-text');
            newsSummaryOutput.textContent = 'Gerando resumo...';
            summarizeNewsBtn.disabled = true;

            try {
                const newsItems = detailsNewsContent.querySelectorAll('.news-item');
                let newsText = "";
                newsItems.forEach(item => {
                    const title = item.querySelector('h3 a').textContent;
                    const description = item.querySelector('p').textContent;
                    newsText += `Título: ${title}\nDescrição: ${description}\n\n`;
                });

                if (!newsText.trim()) {
                    newsSummaryOutput.textContent = 'Nenhuma notícia para resumir.';
                    return;
                }

                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: `Resuma as seguintes notícias sobre ${countryName} de forma concisa, destacando os pontos mais importantes. Não inclua títulos ou formatação Markdown:\n\n${newsText}` }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas injetará a chave da API em tempo de execução
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    newsSummaryOutput.textContent = text;
                } else {
                    newsSummaryOutput.textContent = 'Não foi possível gerar o resumo. Tente novamente.';
                    console.error("Erro na resposta da API Gemini para resumo de notícias:", result);
                }
            } catch (error) {
                newsSummaryOutput.textContent = 'Erro ao conectar com a IA para resumir notícias. Verifique sua conexão.';
                console.error("Erro ao chamar a API Gemini para resumo de notícias:", error);
            } finally {
                newsSummaryOutput.classList.remove('loading-text');
                // O botão de resumo deve ser reabilitado apenas se houver notícias para resumir
                const newsCount = detailsNewsContent.querySelectorAll('.news-item').length;
                if (newsCount > 0) {
                    summarizeNewsBtn.disabled = false;
                }
            }
        }


        // Alternar modo dia/noite
        document.getElementById('theme-toggle').addEventListener('click', () => {
            document.body.classList.toggle('day-mode');
            const toggleButton = document.getElementById('theme-toggle');
            if (document.body.classList.contains('day-mode')) {
                toggleButton.innerHTML = '<i class="fas fa-sun"></i> Modo Diurno';
            } else {
                toggleButton.innerHTML = '<i class="fas fa-moon"></i> Modo Noturno';
            }
        });

        // Adicionar listeners para os botões de controle do mapa
        document.getElementById('zoom-in').addEventListener('click', zoomIn);
        document.getElementById('zoom-out').addEventListener('click', zoomOut);
        document.getElementById('reset-map').addEventListener('click', resetMap);
        document.getElementById('rotate-map').addEventListener('click', rotateMap);
        
        // Listener para o novo botão de atualização de notícias no painel de detalhes
        refreshDetailsNewsBtn.addEventListener('click', () => {
            if (activeCountryCode) {
                const selectedCountryName = countriesData[activeCountryCode] ? countriesData[activeCountryCode].name : null;
                if (selectedCountryName) {
                    fetchNews(selectedCountryName, detailsNewsContent); // Passa o elemento de destino correto
                } else {
                    detailsNewsContent.innerHTML = '<p style="color: var(--news-red);">Não foi possível atualizar notícias para este país (dados detalhados ausentes).</p>';
                }
            } else {
                detailsNewsContent.innerHTML = '<p style="color: var(--news-red);">Selecione um país no mapa para ver as últimas notícias.</p>';
            }
        });

        // Adicionar listeners para os botões LLM
        generateSpaceFactBtn.addEventListener('click', generateSpaceFact);
        exploreConstellationBtn.addEventListener('click', exploreConstellation);
        summarizeNewsBtn.addEventListener('click', () => summarizeNews(countriesData[activeCountryCode].name)); // Listener para o novo botão de resumo

        // Inicializa ResizeObserver para o mapa
        const resizeObserver = new ResizeObserver(entries => {
            for (let entry of entries) {
                if (entry.target.id === 'world-map') {
                    // Adiciona um pequeno atraso para garantir que o layout esteja estável
                    setTimeout(() => {
                        renderMap(); // Redesenha o mapa quando seu contêiner é redimensionado
                    }, 100); 
                }
            }
        });

        // Inicializar a aplicação ao carregar a página
        window.onload = function() {
            const mapLoading = document.getElementById('map-loading');
            mapLoading.classList.add('show');

            // Começa a observar o contêiner do mapa imediatamente
            resizeObserver.observe(document.getElementById('world-map'));

            // Chamada inicial para renderizar o mapa. Um pequeno atraso pode ajudar com o layout inicial.
            setTimeout(() => {
                renderMap();
            }, 200); // Atraso de 200ms para a renderização inicial do mapa

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
            // Mensagem de console para a NewsAPI
            console.warn("Lembrete: Para que as notícias funcionem, você precisa de uma chave de API válida da NewsAPI e o aplicativo deve ser executado em 'localhost' ou em um plano pago da NewsAPI.");
        };

        // Movimentar a caixa de detalhes no mapa com o mouse
        document.addEventListener('mousemove', (e) => {
            // Apenas atualiza a posição se a caixa de detalhes no mapa estiver visível E houver um país hovered
            if (mapCountryHoverInfo.classList.contains('show') && currentHoveredCountryD3Datum) {
                // Recalcula a posição com base no centro do país e no offset
                const centroid = pathGenerator.centroid(currentHoveredCountryD3Datum);
                const mapContainer = document.getElementById('world-map');
                const mapRect = mapContainer.getBoundingClientRect();
                const transformedCentroid = currentTransform.apply(centroid);

                mapCountryHoverInfo.style.left = (transformedCentroid[0] + mapRect.left + window.scrollX + 20) + 'px';
                mapCountryHoverInfo.style.top = (transformedCentroid[1] + mapRect.top + window.scrollY - mapCountryHoverInfo.offsetHeight / 2) + 'px';
            }
        });

    </script>
</body>
</html>
# mapa-ataualizado
