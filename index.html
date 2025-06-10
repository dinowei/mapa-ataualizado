<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CosmoNews - Mapa Interativo</title>
    <script src="https://cdn.jsdelivr.net/npm/astronomy-engine@2.0/astronomy-engine.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
    <script src="https://cdn.jsdelivr.net/npm/topojson-client@3"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
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
            background-image: none;
        }
        
        body.day-mode .map-container, 
        body.day-mode .stellar-clock,
        body.day-mode .info-box,
        body.day-mode .country-details-panel,
        body.day-mode .controls {
            background: rgba(255, 255, 255, 0.7);
            border: 1px solid var(--day-border);
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
        }

        body.day-mode .country-details-panel .news-section {
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
            flex-grow: 1;
            display: flex;
            flex-direction: column;
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
            flex-grow: 1;
        }

        .map-section {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            position: relative;
            flex-grow: 1;
        }

        .map-container {
            background: rgba(26, 27, 65, 0.5);
            border-radius: 15px;
            padding: 1rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(77, 237, 255, 0.2);
            box-shadow: 0 0 20px rgba(77, 237, 255, 0.1);
            position: relative;
            flex-grow: 1;
            min-height: 400px;
        }

        #world-map {
            width: 100%;
            height: 100%;
            cursor: grab;
        }
        #world-map.grabbing {
            cursor: grabbing;
        }

        .country {
            transition: fill 0.2s ease, transform 0.1s ease-out, stroke 0.2s ease;
            transform-origin: center center;
        }
        .country:hover:not(.selected) {
            transform: scale(1.025);
        }
        .country.selected {
            fill: var(--neon-blue) !important;
            stroke: white !important;
            stroke-width: 1.5 !important;
        }
        
        .map-country-hover-info {
            position: absolute;
            background: rgba(0, 0, 0, 0.85);
            border: 1px solid var(--neon-blue);
            border-radius: 8px;
            padding: 8px 12px;
            pointer-events: none;
            z-index: 101;
            display: flex;
            align-items: center;
            gap: 10px;
            opacity: 0;
            transition: opacity 0.2s ease-out, transform 0.2s ease-out;
            transform: translate(0px, -50%);
            min-width: 120px;
            max-width: 200px;
            box-shadow: 0 4px 15px rgba(77, 237, 255, 0.2);
        }

        .map-country-hover-info.show {
            opacity: 1;
            transform: translate(0px, -50%);
        }

        .map-country-hover-info img {
            width: 24px;
            height: 18px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 2px;
        }

        .map-country-hover-info span {
            font-size: 1rem;
            font-weight: bold;
            color: var(--star-yellow);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .country-details-panel {
            position: absolute;
            top: 0;
            right: -320px;
            width: 300px;
            height: 100%;
            background: rgba(26, 27, 65, 0.9);
            border-radius: 15px;
            padding: 1.5rem;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(77, 237, 255, 0.3);
            box-shadow: 0 0 30px rgba(77, 237, 255, 0.2);
            z-index: 90;
            transition: right 0.4s ease-out, opacity 0.4s ease-out;
            opacity: 0;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            gap: 1rem;
            overflow-y: auto;
        }

        .country-details-panel.show {
            right: 0;
            opacity: 1;
            pointer-events: auto;
        }
        
        @media (min-width: 1600px) {
            .main-content {
                grid-template-columns: 1fr 300px 1fr;
                gap: 2rem;
            }
            .map-section {
                grid-column: 1 / 2;
            }
            .country-details-panel {
                position: relative;
                right: auto;
                top: auto;
                opacity: 1;
                pointer-events: auto;
                grid-column: 2 / 3;
                width: auto;
                height: auto;
            }
            .info-panel {
                grid-column: 3 / 4;
            }
        }
        
        @media (max-width: 1599px) and (min-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            .map-section {
                width: 100%;
            }
            .country-details-panel {
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
            flex-grow: 1;
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

        .country-details-panel .news-section {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 1.2rem;
            border-left: 3px solid var(--news-red);
            margin-top: 1.5rem;
            flex-grow: 1;
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
            overflow-y: auto;
        }

        .country-details-panel .news-item {
            margin-bottom: 1.2rem;
            padding-bottom: 1.2rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            transition: transform 0.3s ease;
        }

        .country-details-panel .news-item:hover {
            transform: translateX(5px);
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
            display: none;
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

        .llm-feature {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 1.2rem;
            border-left: 3px solid var(--neon-purple);
            margin-top: 1.5rem;
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
            min-height: 50px;
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
            width: 100%;
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
            
            .map-container {
                height: 500px;
                flex-grow: 0;
            }
            .country-details-panel {
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
            <h1><i class="fas fa-globe-americas"></i> CosmoNews - Mapa Interativo</h1>
            <p class="subtitle">Explore países e notícias em tempo real</p>
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
                        <p>Carregando mapa...</p>
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
        </footer>
    </div>

    <script>
        // Dados dos países (mantido)
        const countriesData = {
            "BR": { 
                name: "Brasil", 
                capital: "Brasília", 
                lat: -15.78, 
                lon: -47.93, 
                timezone: "UTC-3",
                population: "214 milhões",
                language: "Português",
                currency: "Real (BRL)"
            },
            "US": { 
                name: "Estados Unidos", 
                capital: "Washington, D.C.", 
                lat: 38.90, 
                lon: -77.04, 
                timezone: "UTC-4 a UTC-10",
                population: "331 milhões",
                language: "Inglês",
                currency: "Dólar (USD)"
            },
            "GB": { 
                name: "Reino Unido", 
                capital: "Londres", 
                lat: 51.50, 
                lon: -0.12, 
                timezone: "UTC+1",
                population: "67 milhões",
                language: "Inglês",
                currency: "Libra Esterlina (GBP)"
            },
            "JP": {
                name: "Japão",
                capital: "Tóquio",
                lat: 35.68,
                lon: 139.69,
                timezone: "UTC+9",
                population: "125 milhões",
                language: "Japonês",
                currency: "Iene (JPY)"
            },
            "AU": {
                name: "Austrália",
                capital: "Camberra",
                lat: -35.28,
                lon: 149.13,
                timezone: "UTC+10",
                population: "26 milhões",
                language: "Inglês",
                currency: "Dólar Australiano (AUD)"
            },
            "CA": {
                name: "Canadá",
                capital: "Ottawa",
                lat: 45.42,
                lon: -75.69,
                timezone: "UTC-5 a UTC-8",
                population: "38 milhões",
                language: "Inglês, Francês",
                currency: "Dólar Canadense (CAD)"
            },
            "DE": {
                name: "Alemanha",
                capital: "Berlim",
                lat: 52.52,
                lon: 13.40,
                timezone: "UTC+2",
                population: "83 milhões",
                language: "Alemão",
                currency: "Euro (EUR)"
            },
            "FR": {
                name: "França",
                capital: "Paris",
                lat: 48.85,
                lon: 2.35,
                timezone: "UTC+2",
                population: "65 milhões",
                language: "Francês",
                currency: "Euro (EUR)"
            },
            "IN": {
                name: "Índia",
                capital: "Nova Delhi",
                lat: 28.61,
                lon: 77.20,
                timezone: "UTC+5:30",
                population: "1.4 bilhões",
                language: "Hindi, Inglês",
                currency: "Rúpia Indiana (INR)"
            },
            "CN": {
                name: "China",
                capital: "Pequim",
                lat: 39.90,
                lon: 116.40,
                timezone: "UTC+8",
                population: "1.4 bilhões",
                language: "Mandarim",
                currency: "Yuan (CNY)"
            },
            "RU": {
                name: "Rússia",
                capital: "Moscou",
                lat: 55.75,
                lon: 37.61,
                timezone: "UTC+2 a UTC+12",
                population: "144 milhões",
                language: "Russo",
                currency: "Rublo Russo (RUB)"
            },
            "MX": {
                name: "México",
                capital: "Cidade do México",
                lat: 19.43,
                lon: -99.13,
                timezone: "UTC-5 a UTC-8",
                population: "126 milhões",
                language: "Espanhol",
                currency: "Peso Mexicano (MXN)"
            },
            "ZA": {
                name: "África do Sul",
                capital: "Pretória",
                lat: -25.74,
                lon: 28.22,
                timezone: "UTC+2",
                population: "60 milhões",
                language: "Africâner, Inglês, Zulu, Xhosa...",
                currency: "Rand (ZAR)"
            },
            "EG": {
                name: "Egito",
                capital: "Cairo",
                lat: 30.04,
                lon: 31.23,
                timezone: "UTC+2",
                population: "109 milhões",
                language: "Árabe",
                currency: "Libra Egípcia (EGP)"
            },
            "IT": {
                name: "Itália",
                capital: "Roma",
                lat: 41.90,
                lon: 12.49,
                timezone: "UTC+2",
                population: "59 milhões",
                language: "Italiano",
                currency: "Euro (EUR)"
            },
            "ES": {
                name: "Espanha",
                capital: "Madri",
                lat: 40.41,
                lon: -3.70,
                timezone: "UTC+2",
                population: "47 milhões",
                language: "Espanhol",
                currency: "Euro (EUR)"
            },
            "NG": {
                name: "Nigéria",
                capital: "Abuja",
                lat: 9.07,
                lon: 7.49,
                timezone: "UTC+1",
                population: "218 milhões",
                language: "Inglês, Hauçá, Iorubá, Igbo",
                currency: "Naira (NGN)"
            },
            "KE": {
                name: "Quênia",
                capital: "Nairóbi",
                lat: -1.28,
                lon: 36.82,
                timezone: "UTC+3",
                population: "55 milhões",
                language: "Suaíli, Inglês",
                currency: "Xelim Queniano (KES)"
            },
            "MA": {
                name: "Marrocos",
                capital: "Rabat",
                lat: 34.02,
                lon: -6.83,
                timezone: "UTC+1",
                population: "37 milhões",
                language: "Árabe, Berber",
                currency: "Dirham Marroquino (MAD)"
            },
            "DZ": {
                name: "Argélia",
                capital: "Argel",
                lat: 36.75,
                lon: 3.04,
                timezone: "UTC+1",
                population: "45 milhões",
                language: "Árabe, Berber",
                currency: "Dinar Argelino (DZD)"
            },
            "GH": {
                name: "Gana",
                capital: "Acra",
                lat: 5.55,
                lon: -0.20,
                timezone: "UTC+0",
                population: "32 milhões",
                language: "Inglês",
                currency: "Cedi Ganês (GHS)"
            },
            "AO": {
                name: "Angola",
                capital: "Luanda",
                lat: -8.83,
                lon: 13.23,
                timezone: "UTC+1",
                population: "34 milhões",
                language: "Português",
                currency: "Kwanza (AOA)"
            },
            "MZ": {
                name: "Moçambique",
                capital: "Maputo",
                lat: -25.96,
                lon: 32.57,
                timezone: "UTC+2",
                population: "32 milhões",
                language: "Português",
                currency: "Metical Moçambicano (MZN)"
            },
            "CD": {
                name: "República Democrática do Congo",
                capital: "Kinshasa",
                lat: -4.44,
                lon: 15.26,
                timezone: "UTC+1 a UTC+2",
                population: "95 milhões",
                language: "Francês, Lingala, Quicongo, Suaíli, Tshiluba",
                currency: "Franco Congolês (CDF)"
            }
        };

        // Mapeamento de IDs do TopoJSON para códigos de país
        const topoJsonIdToCountryCodeMap = {
            "076": "BR",
            "840": "US",
            "826": "GB",
            "392": "JP",
            "036": "AU",
            "124": "CA",
            "276": "DE",
            "250": "FR",
            "356": "IN",
            "156": "CN",
            "643": "RU",
            "484": "MX",
            "710": "ZA",
            "818": "EG",
            "380": "IT",
            "724": "ES",
            "566": "NG",
            "404": "KE",
            "504": "MA",
            "012": "DZ",
            "288": "GH",
            "024": "AO",
            "508": "MZ",
            "180": "CD"
        };

        // Variáveis globais para o mapa
        let svgMap, projection, pathGenerator, zoomBehavior, rotationInterval;
        let currentRotation = 0;
        let activeCountryCode = null;
        let currentTransform = d3.zoomIdentity;
        let currentHoveredCountryD3Datum = null;

        // Variáveis para otimização
        let lastWidth = 0, lastHeight = 0, resizeTimeout;
        let lastAnimationFrame;

        // URL de imagem de placeholder para bandeiras
        const PLACEHOLDER_FLAG_URL = 'https://placehold.co/24x18/cccccc/333333?text=N/A';

        // Elementos DOM
        const countryDetailsPanel = document.getElementById('country-details-panel');
        const detailsFlag = document.getElementById('details-flag');
        const detailsCountryName = document.getElementById('details-country-name');
        const detailsCapital = document.getElementById('details-capital');
        const detailsPopulation = document.getElementById('details-population');
        const detailsLanguage = document.getElementById('details-language');
        const detailsCurrency = document.getElementById('details-currency');
        const detailsCoords = document.getElementById('details-coords');
        const detailsTimezone = document.getElementById('details-timezone');
        const detailsNewsContent = document.getElementById('details-news-content');
        const refreshDetailsNewsBtn = document.getElementById('refresh-details-news');
        const mapCountryHoverInfo = document.getElementById('map-country-hover-info');
        const mapHoverFlag = document.getElementById('map-hover-flag');
        const mapHoverName = document.getElementById('map-hover-name');
        const spaceFactOutput = document.getElementById('space-fact-output');
        const generateSpaceFactBtn = document.getElementById('generate-space-fact-btn');
        const newsSummaryOutput = document.getElementById('news-summary-output');
        const summarizeNewsBtn = document.getElementById('summarize-news-btn');

        // Função para renderizar o mapa (otimizada)
        async function renderMap() {
            const mapLoading = document.getElementById('map-loading');
            mapLoading.classList.add('show');

            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            if (width === 0 || height === 0) {
                mapLoading.classList.remove('show');
                return;
            }

            // Remove SVG existente
            d3.select("#world-map svg").remove();

            // Cria novo SVG
            svgMap = d3.select("#world-map")
                .append("svg")
                .attr("width", "100%")
                .attr("height", "100%")
                .attr("viewBox", `0 0 ${width} ${height}`);

            // Define a projeção
            projection = d3.geoNaturalEarth1()
                .scale(width / 5.5)
                .translate([width / 2, height / 2]);

            pathGenerator = d3.geoPath().projection(projection);

            try {
                // Carrega dados do mapa
                const world = await d3.json("https://unpkg.com/world-atlas@2.0.2/countries-110m.json");
                
                // Desenha países
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
                    .attr("id", d => `country-${d.id}`)
                    .on("mouseover", function(event, d) {
                        currentHoveredCountryD3Datum = d;
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        const countryPath = d3.select(this);
                        
                        if (countryCode && countryCode !== activeCountryCode) {
                            countryPath.classed("hovered", true).transition().duration(100).attr("transform", "scale(1.025)");
                        }
                        showMapCountryHoverInfo(countryCode, d.properties.name, event, d);
                    })
                    .on("mouseout", function(event, d) {
                        currentHoveredCountryD3Datum = null;
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        const countryPath = d3.select(this);

                        if (countryCode && countryCode !== activeCountryCode) {
                            countryPath.classed("hovered", false).transition().duration(100).attr("transform", "");
                        }
                        hideMapCountryHoverInfo();
                    })
                    .on("click", function(event, d) {
                        const topoId = d.id;
                        const countryCode = topoJsonIdToCountryCodeMap[topoId];
                        
                        d3.selectAll(".country").classed("selected", false)
                            .attr("fill", "rgba(100, 149, 237, 0.4)")
                            .attr("stroke", "rgba(255, 255, 255, 0.2)")
                            .attr("stroke-width", 0.5);
                        
                        d3.select(this).classed("selected", true)
                            .attr("fill", "var(--neon-blue)")
                            .attr("stroke", "white")
                            .attr("stroke-width", 1.5);
                        
                        activeCountryCode = countryCode;
                        updateCountryInfo(countryCode);
                        displayCountryDetails(countryCode, d.properties.name);
                        zoomToCountry(d);
                    });

                // Configura zoom
                zoomBehavior = d3.zoom()
                    .scaleExtent([1, 8])
                    .on("zoom", (event) => {
                        currentTransform = event.transform;
                        svgMap.selectAll("path").attr("transform", event.transform);
                        svgMap.select(".countries").attr("stroke-width", 0.5 / event.transform.k);
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

                // Configura arrastar
                svgMap.on("mousedown", function() {
                    d3.select(this).classed("grabbing", true);
                }).on("mouseup", function() {
                    d3.select(this).classed("grabbing", false);
                });

                // Seleciona o Brasil por padrão
                const defaultCountryTopoId = "076";
                const brazilPath = svgMap.select(`#country-${defaultCountryTopoId}`);
                if (!brazilPath.empty()) {
                    setTimeout(() => brazilPath.dispatch("click"), 100);
                }

            } catch (error) {
                console.error("Erro ao carregar mapa:", error);
                mapLoading.innerHTML = `<p style="color: var(--news-red);">Erro ao carregar mapa: ${error.message}</p>`;
            } finally {
                mapLoading.classList.remove('show');
            }
        }

        // Função para dar zoom em um país
        function zoomToCountry(d) {
            if (!svgMap || !pathGenerator || !projection) return;

            const mapContainer = document.getElementById('world-map');
            const width = mapContainer.offsetWidth;
            const height = mapContainer.offsetHeight;

            const bounds = pathGenerator.bounds(d);
            const dx = bounds[1][0] - bounds[0][0];
            const dy = bounds[1][1] - bounds[0][1];
            const x = (bounds[0][0] + bounds[1][0]) / 2;
            const y = (bounds[0][1] + bounds[1][1]) / 2;

            const scale = Math.max(1, Math.min(8, 0.9 / Math.max(dx / width, dy / height)));
            const translate = [width / 2 - scale * x, height / 2 - scale * y];

            svgMap.transition()
                .duration(750)
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
            currentRotation = 0;
            if (projection) projection.rotate([currentRotation, 0]);
            if (svgMap) svgMap.selectAll("path").attr("d", pathGenerator);
            d3.selectAll(".country").classed("selected", false)
                .attr("fill", "rgba(100, 149, 237, 0.4)")
                .attr("stroke", "rgba(255, 255, 255, 0.2)")
                .attr("stroke-width", 0.5);
            activeCountryCode = null;
            updateCountryInfo(null);
            if (rotationInterval) {
                clearInterval(rotationInterval);
                rotationInterval = null;
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            }
            hideCountryDetails();
            hideMapCountryHoverInfo();
            currentHoveredCountryD3Datum = null;
            spaceFactOutput.textContent = 'Clique no botão para gerar uma curiosidade!';
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.';
            summarizeNewsBtn.disabled = true;
        }

        function rotateMap() {
            if (rotationInterval) {
                clearInterval(rotationInterval);
                rotationInterval = null;
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-sync-alt"></i> Rotacionar';
            } else {
                rotationInterval = setInterval(() => {
                    currentRotation += 0.5;
                    if (currentRotation > 360) currentRotation -= 360;
                    if (projection) projection.rotate([currentRotation, 0]);
                    if (svgMap) svgMap.selectAll("path").attr("d", pathGenerator);
                }, 50);
                document.getElementById('rotate-map').innerHTML = '<i class="fas fa-pause"></i> Pausar Rotação';
            }
        }

        // Função para mostrar informações no hover
        function showMapCountryHoverInfo(countryCode, countryNameFromTopoJson, d3Event, d3Datum) {
            const displayCountryName = countryNameFromTopoJson || "País Desconhecido";
            const flagSrc = countryCode ? `https://flagcdn.com/w20/${countryCode.toLowerCase()}.png` : PLACEHOLDER_FLAG_URL;

            mapHoverFlag.src = flagSrc;
            mapHoverFlag.onerror = () => { mapHoverFlag.src = PLACEHOLDER_FLAG_URL; };
            mapHoverName.textContent = displayCountryName;
            
            const centroid = pathGenerator.centroid(d3Datum);
            const mapContainer = document.getElementById('world-map');
            const mapRect = mapContainer.getBoundingClientRect();
            const transformedCentroid = currentTransform.apply(centroid);

            mapCountryHoverInfo.style.left = (transformedCentroid[0] + mapRect.left + 20) + 'px';
            mapCountryHoverInfo.style.top = (transformedCentroid[1] + mapRect.top - mapCountryHoverInfo.offsetHeight / 2) + 'px';
            mapCountryHoverInfo.classList.add('show');
        }

        // Função para esconder informações no hover
        function hideMapCountryHoverInfo() {
            mapCountryHoverInfo.classList.remove('show');
        }

        // Função para exibir detalhes do país
        function displayCountryDetails(countryCode, countryNameFromTopoJson) {
            const country = countriesData[countryCode];
            const displayCountryName = countryNameFromTopoJson || "País Desconhecido";

            detailsFlag.src = countryCode ? `https://flagcdn.com/w40/${countryCode.toLowerCase()}.png` : PLACEHOLDER_FLAG_URL;
            detailsFlag.alt = `${displayCountryName} Flag`;
            detailsFlag.onerror = () => { detailsFlag.src = PLACEHOLDER_FLAG_URL; };
            detailsCountryName.textContent = displayCountryName;
            detailsCapital.textContent = country ? country.capital : "--";
            detailsPopulation.textContent = country ? country.population : "--";
            detailsLanguage.textContent = country ? country.language : "--";
            detailsCurrency.textContent = country ? country.currency : "--";
            detailsCoords.textContent = country ? `${country.lat}°N, ${Math.abs(country.lon)}°${country.lon < 0 ? 'W' : 'E'}` : "--";
            detailsTimezone.textContent = country ? country.timezone : "--";
            countryDetailsPanel.classList.add('show');

            // Buscar notícias
            fetchNews(country.name, detailsNewsContent);
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.';
            summarizeNewsBtn.disabled = true;
        }

        // Função para esconder detalhes do país
        function hideCountryDetails() {
            countryDetailsPanel.classList.remove('show');
        }

        // Função para atualizar informações do país
        function updateCountryInfo(countryCode) {
            const country = countriesData[countryCode];
            if (!country) {
                document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { day: 'numeric', month: 'long', year: 'numeric' });
                document.getElementById('stellar-time').textContent = '--:--:--';
                document.getElementById('location-info').textContent = 'Clique em um país no mapa para começar';
                document.getElementById('timezone').textContent = '--';
                document.getElementById('population').textContent = '--';
                document.getElementById('language').textContent = '--';
                document.getElementById('currency').textContent = '--';
                detailsNewsContent.innerHTML = '<p>Notícias do país aparecerão aqui.</p>';
                newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.';
                summarizeNewsBtn.disabled = true;
                return;
            }
            
            document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { 
                day: 'numeric', 
                month: 'long', 
                year: 'numeric' 
            });
            
            document.getElementById('location-info').textContent = `${country.capital}, ${country.name} (${country.timezone})`;
            
            // Hora local
            const now = new Date();
            document.getElementById('stellar-time').textContent = now.toLocaleTimeString('pt-BR', { hour12: false });
            
            // Informações do país
            document.getElementById('timezone').textContent = country.timezone;
            document.getElementById('population').textContent = country.population;
            document.getElementById('language').textContent = country.language;
            document.getElementById('currency').textContent = country.currency;
        }

        // Função para buscar notícias (simplificada)
        async function fetchNews(query, targetElement) {
            targetElement.innerHTML = '<p>Carregando notícias...</p>';
            newsSummaryOutput.textContent = 'O resumo das notícias aparecerá aqui.';
            summarizeNewsBtn.disabled = true;

            const NEWS_API_KEY = 'SUA_API_KEY_DA_NEWSAPI';

            if (NEWS_API_KEY === 'SUA_API_KEY_DA_NEWSAPI' || NEWS_API_KEY === '') {
                targetElement.innerHTML = `<p style="color: var(--news-red);">As notícias não podem ser carregadas. Verifique sua chave da NewsAPI.</p>`;
                return;
            }

            const url = `https://newsapi.org/v2/everything?q=${encodeURIComponent(query)}&language=pt&sortBy=relevancy&apiKey=${NEWS_API_KEY}`;

            try {
                const response = await fetch(url);
                const data = await response.json();

                if (data.status === 'ok' && data.articles.length > 0) {
                    targetElement.innerHTML = '';
                    data.articles.slice(0, 5).forEach(article => {
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
                    summarizeNewsBtn.disabled = false;
                } else {
                    targetElement.innerHTML = `<p>Nenhuma notícia encontrada para "${query}".</p>`;
                    summarizeNewsBtn.disabled = true;
                }
            } catch (error) {
                targetElement.innerHTML = `<p style="color: var(--news-red);">Erro ao carregar notícias. Verifique sua conexão ou chave da API.</p>`;
                summarizeNewsBtn.disabled = true;
            }
        }

        // Função para gerar curiosidade espacial
        async function generateSpaceFact() {
            spaceFactOutput.classList.add('loading-text');
            spaceFactOutput.textContent = 'Gerando curiosidade...';
            generateSpaceFactBtn.disabled = true;

            try {
                // Simulação de chamada à API
                const facts = [
                    "A luz do Sol leva cerca de 8 minutos para chegar à Terra.",
                    "Existem mais estrelas no universo do que grãos de areia em todas as praias da Terra.",
                    "A estrela mais próxima da Terra, além do Sol, é Proxima Centauri, a 4,24 anos-luz de distância.",
                    "A Via Láctea tem cerca de 100.000 anos-luz de diâmetro.",
                    "A temperatura no núcleo do Sol é de aproximadamente 15 milhões de graus Celsius."
                ];
                
                // Seleciona um fato aleatório
                const randomFact = facts[Math.floor(Math.random() * facts.length)];
                
                // Simula um atraso de carregamento
                setTimeout(() => {
                    spaceFactOutput.textContent = randomFact;
                    spaceFactOutput.classList.remove('loading-text');
                    generateSpaceFactBtn.disabled = false;
                }, 1500);
            } catch (error) {
                spaceFactOutput.textContent = 'Erro ao gerar curiosidade. Tente novamente.';
                spaceFactOutput.classList.remove('loading-text');
                generateSpaceFactBtn.disabled = false;
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
        
        // Listener para atualizar notícias
        refreshDetailsNewsBtn.addEventListener('click', () => {
            if (activeCountryCode) {
                const selectedCountryName = countriesData[activeCountryCode] ? countriesData[activeCountryCode].name : null;
                if (selectedCountryName) {
                    fetchNews(selectedCountryName, detailsNewsContent);
                } else {
                    detailsNewsContent.innerHTML = '<p style="color: var(--news-red);">Não foi possível atualizar notícias para este país.</p>';
                }
            } else {
                detailsNewsContent.innerHTML = '<p style="color: var(--news-red);">Selecione um país no mapa para ver as últimas notícias.</p>';
            }
        });

        // Adicionar listeners para os botões
        generateSpaceFactBtn.addEventListener('click', generateSpaceFact);
        summarizeNewsBtn.addEventListener('click', () => {
            newsSummaryOutput.textContent = "Resumo gerado com sucesso! Esta é uma demonstração.";
        });

        // Inicializar a aplicação ao carregar a página
        window.onload = function() {
            const mapLoading = document.getElementById('map-loading');
            mapLoading.classList.add('show');

            // ResizeObserver otimizado
            const resizeObserver = new ResizeObserver(entries => {
                clearTimeout(resizeTimeout);
                resizeTimeout = setTimeout(() => {
                    const mapContainer = document.getElementById('world-map');
                    const newWidth = mapContainer.offsetWidth;
                    const newHeight = mapContainer.offsetHeight;
                    
                    if (Math.abs(newWidth - lastWidth) > 10 || Math.abs(newHeight - lastHeight) > 10) {
                        lastWidth = newWidth;
                        lastHeight = newHeight;
                        renderMap();
                    }
                }, 300);
            });
            resizeObserver.observe(document.getElementById('world-map'));

            setTimeout(() => {
                renderMap();
            }, 200);

            // Atualizar relógio
            setInterval(() => {
                if (activeCountryCode) {
                    updateCountryInfo(activeCountryCode);
                } else {
                    document.getElementById('current-date').textContent = new Date().toLocaleDateString('pt-BR', { day: 'numeric', month: 'long', year: 'numeric' });
                    document.getElementById('stellar-time').textContent = new Date().toLocaleTimeString('pt-BR', { hour12: false });
                }
            }, 1000);
        };

        // Movimentar a caixa de detalhes no mapa com o mouse (otimizado)
        document.addEventListener('mousemove', (e) => {
            if (!mapCountryHoverInfo.classList.contains('show') || !currentHoveredCountryD3Datum) return;

            if (lastAnimationFrame) cancelAnimationFrame(lastAnimationFrame);
            
            lastAnimationFrame = requestAnimationFrame(() => {
                const centroid = pathGenerator.centroid(currentHoveredCountryD3Datum);
                const mapContainer = document.getElementById('world-map');
                const mapRect = mapContainer.getBoundingClientRect();
                const transformedCentroid = currentTransform.apply(centroid);

                mapCountryHoverInfo.style.left = (transformedCentroid[0] + mapRect.left + window.scrollX + 20) + 'px';
                mapCountryHoverInfo.style.top = (transformedCentroid[1] + mapRect.top + window.scrollY - mapCountryHoverInfo.offsetHeight / 2) + 'px';
            });
        });
    </script>
</body>
</html>
