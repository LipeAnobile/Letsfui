[Uploading aa.html…]()
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <!-- Meta tag crucial para a sensação de app mobile -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Let's Fui</title>
    
    <!-- Fontes (Poppins) -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <!-- Ícones (Phosphor Icons) -->
    <script src="https://unpkg.com/@phosphor-icons/web"></script>

    <style>
        /* --- 1. Configurações Globais e Variáveis --- */
        :root {
            --primary: #FFD60A; /* Amarelo */
            --secondary: #0E0E0E; /* Preto */
            --white: #FFFFFF;
            --grey-light: #f5f5f5;
            --grey-mid: #b0b0b0;
            --grey-dark: #757575;
            --font-family: 'Poppins', sans-serif;
            --nav-height: 70px; /* Altura da navbar */
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent; /* Remove flash azul no clique mobile */
        }

        html, body {
            height: 100%;
            width: 100%;
            overflow: hidden; /* Previne "pulo" no body */
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        body {
            font-family: var(--font-family);
            background: var(--secondary); /* Fundo "fora" do app */
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* --- 2. Estrutura Principal do App --- */

        #app-container {
            width: 100%;
            height: 100%;
            /* Limita a largura no desktop para simular um celular */
            max-width: 450px; 
            margin: 0 auto;
            position: relative;
            background: var(--white);
            overflow: hidden; /* Container principal não deve scrollar */
            display: flex;
            flex-direction: column;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            border-radius: 2px; /* Pequeno detalhe para desktop */
        }

        /* O #app-content é o "viewfinder" das telas */
        #app-content {
            flex: 1; /* Ocupa todo o espaço menos a navbar (que é absoluta) */
            position: relative;
            overflow: hidden; /* Controla as transições de página */
        }

        /* --- 3. Telas (Páginas) e Transições --- */

        .page {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            padding: 20px 20px var(--nav-height) 20px;
            overflow-y: auto;
            background: var(--white);
            
            /* Configuração inicial da transição (invisível e fora da tela) */
            opacity: 0;
            transform: translateX(30px);
            transition: opacity 0.3s ease-out, transform 0.3s ease-out;
            z-index: 1;
            display: none; /* JS controlará isso */
        }
        
        .page.active {
            opacity: 1;
            transform: translateX(0);
            z-index: 10;
            display: block;
        }

        /* Transição de saída */
        .page.exit {
            opacity: 0;
            transform: translateX(-30px);
            z-index: 1;
        }

        /* --- 4. Componentes Reutilizáveis --- */

        .app-header {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 24px;
            font-size: 22px;
            font-weight: 700;
            color: var(--secondary);
            gap: 5px;
        }
        .app-header i {
            font-size: 28px;
            color: var(--primary);
        }

        .search-bar {
            width: 100%;
            padding: 14px 18px;
            font-size: 14px;
            font-family: var(--font-family);
            font-weight: 500;
            background: var(--grey-light);
            border: none;
            border-radius: 12px;
            margin-bottom: 20px;
        }
        .search-bar::placeholder {
            color: var(--grey-dark);
            font-weight: 500;
        }

        .btn {
            display: inline-block;
            padding: 12px 20px;
            font-size: 14px;
            font-weight: 600;
            font-family: var(--font-family);
            border-radius: 10px;
            border: none;
            cursor: pointer;
            text-align: center;
            text-decoration: none;
            transition: all 0.2s ease;
        }
        .btn:active {
            transform: scale(0.97);
            filter: brightness(0.95);
        }

        .btn-primary {
            background: var(--primary);
            color: var(--secondary);
        }
        .btn-secondary {
            background: var(--secondary);
            color: var(--white);
        }
        .btn-block {
            display: block;
            width: 100%;
        }

        .section-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            color: var(--secondary);
        }

        /* --- 5. Splash Screen --- */

        #splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1000;
            background: var(--primary);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 10px;
            transition: opacity 0.5s ease-out, visibility 0.5s ease-out;
        }
        #splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }
        .splash-logo {
            font-size: 40px;
            font-weight: 800;
            color: var(--secondary);
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .splash-logo i {
            font-size: 44px;
        }
        #splash-screen .spinner {
            width: 24px;
            height: 24px;
            border: 3px solid rgba(14, 14, 14, 0.3);
            border-top-color: var(--secondary);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            margin-top: 20px;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* --- 6. Navbar Inferior --- */

        #bottom-navbar {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: var(--nav-height);
            background: var(--white);
            border-top: 1px solid var(--grey-light);
            display: flex;
            justify-content: space-around;
            align-items: flex-start; /* Alinha no topo da navbar */
            padding-top: 10px;
            z-index: 100;
            box-shadow: 0 -5px 20px rgba(0,0,0,0.03);
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: pointer;
            gap: 4px;
            color: var(--grey-dark);
            transition: color 0.2s ease;
        }
        .nav-item i {
            font-size: 24px; /* Reduzido */
            transition: all 0.2s ease;
            /* Removido padding e border-radius */
        }
        .nav-item span {
            font-size: 10px;
            font-weight: 600;
        }

        /* Estilo do item ATIVO (Modificado) */
        .nav-item.active {
            color: var(--primary); /* Cor primária para ícone e texto */
        }
        /* Removido .nav-item.active i (fundo amarelo) */


        /* --- 7. Tela 1: Explorar --- */

        /* --- 7b. Banner Carousel --- */
        .banner-carousel {
            width: 100%;
            height: 140px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            margin-bottom: 20px;
            box-shadow: 0 6px 20px rgba(0,0,0,0.1);
        }
        .carousel-wrapper {
            display: flex;
            width: 300%; /* 3 slides */
            height: 100%;
            transition: transform 0.5s ease-in-out;
        }
        .carousel-slide {
            width: 33.333%; /* 1/3 */
            height: 100%;
            background-size: cover;
            background-position: center;
            position: relative;
        }
        /* Sombra interna para legibilidade do texto */
        .carousel-slide::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 60%;
            background: linear-gradient(to top, rgba(0,0,0,0.6), transparent);
        }
        .slide-content {
            position: absolute;
            bottom: 15px;
            left: 15px;
            color: var(--white);
            z-index: 2;
        }
        .slide-content h3 {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 2px;
        }
        .slide-content p {
            font-size: 12px;
            font-weight: 400;
        }
        .carousel-pagination {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 6px;
            z-index: 3;
        }
        .carousel-pagination .dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;
        }
        .carousel-pagination .dot.active {
            background: var(--white);
            width: 12px;
            border-radius: 5px;
        }

        .filter-bar {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding-bottom: 10px; /* Espaço para sombra */
            margin-bottom: 20px;
            /* Esconde a scrollbar */
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }
        .filter-bar::-webkit-scrollbar {
            display: none; /* Chrome, Safari, Opera */
        }
        .filter-item {
            display: flex;
            align-items: center;
            gap: 6px;
            padding: 8px 15px;
            background: var(--grey-light);
            color: var(--grey-dark);
            border-radius: 20px;
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            white-space: nowrap; /* Impede quebra de linha */
        }
        .filter-item i {
            font-size: 16px;
        }
        .filter-item.active, .filter-item:hover {
            background: var(--primary);
            color: var(--secondary);
            font-weight: 600;
            box-shadow: 0 4px 10px rgba(255, 214, 10, 0.3);
        }

        .establishment-list {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        .establishment-card {
            width: 100%;
            border-radius: 16px;
            background: var(--white);
            box-shadow: 0 6px 25px rgba(0,0,0,0.06);
            overflow: hidden;
            transition: transform 0.2s ease;
            /* Adicionado para lógica de filtro */
            transition: opacity 0.3s ease, transform 0.3s ease, max-height 0.3s ease, margin 0.3s ease;
            max-height: 500px;
            opacity: 1;
        }
        /* Classe para esconder item filtrado */
         .establishment-card.hidden {
            opacity: 0;
            max-height: 0;
            margin-bottom: 0;
            padding-top: 0;
            padding-bottom: 0;
            overflow: hidden;
            border: none;
            box-shadow: none;
        }

        .establishment-card:active {
            transform: scale(0.98);
        }
        .card-image {
            width: 100%;
            height: 160px;
            background-size: cover;
            background-position: center;
        }
        .card-content {
            padding: 15px;
        }
        .card-tags {
            display: flex;
            gap: 8px;
            margin-bottom: 8px;
        }
        .card-tag {
            font-size: 11px;
            font-weight: 600;
            padding: 4px 8px;
            border-radius: 6px;
        }
        .tag-category {
            background: var(--primary);
            color: var(--secondary);
        }
        .tag-distance {
            background: var(--grey-light);
            color: var(--grey-dark);
        }
        .card-title {
            font-size: 18px;
            font-weight: 700;
            color: var(--secondary);
            margin-bottom: 10px;
        }
        .card-description {
            font-size: 13px;
            color: var(--grey-dark);
            margin-bottom: 15px;
            /* Limita a 2 linhas */
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;  
            overflow: hidden;
        }


        /* --- 8. Tela 2: Mapa --- */

        /* Header customizado para o Mapa */
        #page-mapa .app-header {
            justify-content: space-between;
        }
        .location-display {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 14px;
            font-weight: 500;
            padding: 8px 12px;
            background: var(--grey-light);
            border-radius: 10px;
        }
        .location-display i {
            color: var(--primary);
            font-size: 18px;
        }
        .filter-button {
            padding: 8px;
            background: var(--grey-light);
            border-radius: 10px;
            font-size: 20px;
            cursor: pointer;
        }

        #map-container {
            width: 100%;
            /* Altura baseada na proporção da tela */
            height: calc(100% - 100px); /* 100% menos header e padding */
            background: #e8f8ec; /* Cor de mapa simulada */
            border-radius: 16px;
            position: relative;
            overflow: hidden;
            background-image: url('https://placehold.co/600x800/e8f8ec/a0c0a8?text=Mapa+Campinas');
            background-size: cover;
            background-position: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .map-pin {
            position: absolute;
            font-size: 36px;
            color: var(--primary);
            cursor: pointer;
            transition: transform 0.2s ease;
            filter: drop-shadow(0 2px 3px rgba(0,0,0,0.3));
            z-index: 10; /* Z-index para ficar acima do heatmap */
        }
        .map-pin:hover {
            transform: scale(1.1);
        }
        /* Posições simuladas dos pins */
        .pin-1 { top: 20%; left: 30%; }
        .pin-2 { top: 45%; left: 55%; }
        .pin-3 { top: 60%; left: 25%; }
        .pin-4 { top: 75%; left: 70%; }

        /* --- NOVOS ESTILOS: Heatmap (Mancha de Calor) --- */
        .heatmap-spot {
            position: absolute;
            width: 120px;
            height: 120px;
            border-radius: 50%;
            cursor: pointer;
            z-index: 5; /* Abaixo dos pins, acima do mapa */
            animation: pulse 2.5s infinite ease-out;
            pointer-events: auto;
        }
        /* Amarelo mais escuro (superlotado) */
        .spot-busy {
            background: radial-gradient(circle, rgba(255, 200, 0, 0.7), rgba(255, 200, 0, 0.0) 70%);
            width: 150px;
            height: 150px;
        }
        /* Amarelo fraco (movimentado) */
        .spot-medium {
            background: radial-gradient(circle, rgba(255, 214, 10, 0.5), rgba(255, 214, 10, 0.0) 70%);
        }
        @keyframes pulse {
            0% { transform: scale(0.95); opacity: 0.7; }
            50% { transform: scale(1.05); opacity: 1; }
            100% { transform: scale(0.95); opacity: 0.7; }
        }

        #heatmap-popup {
            position: absolute;
            background: var(--white);
            border-radius: 10px;
            padding: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            z-index: 25; /* Acima de tudo */
            display: none; /* Escondido por padrão */
            width: 200px;
            border: 2px solid var(--primary);
            transition: opacity 0.2s ease;
        }
        #heatmap-popup h4 {
            font-size: 14px;
            font-weight: 700;
            color: var(--secondary);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        #heatmap-popup h4 i { color: #f97316; } /* Laranja para "fogo" */
        #heatmap-popup-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        #heatmap-popup-list li {
            font-size: 13px;
            color: var(--grey-dark);
            padding-bottom: 5px;
            font-weight: 500;
        }
        #heatmap-popup-list li strong {
            color: var(--secondary);
        }
        /* --- FIM DOS NOVOS ESTILOS --- */


        .pin-popup {
            position: absolute;
            bottom: -100%; /* Começa escondido */
            left: 10px;
            right: 10px;
            background: var(--white);
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.15);
            display: flex;
            align-items: center;
            gap: 12px;
            transition: bottom 0.3s ease-out;
            z-index: 20;
        }
        .pin-popup.show {
            bottom: 10px;
        }
        .popup-image {
            width: 50px;
            height: 50px;
            border-radius: 8px;
            background-size: cover;
            background-position: center;
            flex-shrink: 0;
        }
        .popup-info h4 {
            font-size: 14px;
            font-weight: 600;
        }
        .popup-info p {
            font-size: 12px;
            color: var(--grey-dark);
        }
        .popup-action {
            margin-left: auto;
            flex-shrink: 0;
        }
        .btn-popup {
            padding: 8px 12px;
            font-size: 12px;
        }

        /* --- 9. Tela 3: Clube Let's Fui --- */
        
        .points-display {
            text-align: center;
            margin-bottom: 25px;
        }
        .points-display h1 {
            font-size: 48px;
            font-weight: 800;
            color: var(--primary);
            line-height: 1.1;
        }
        .points-display p {
            font-size: 16px;
            font-weight: 500;
            color: var(--grey-dark);
        }

        .progress-bar-container {
            width: 100%;
            margin-bottom: 10px;
        }
        .progress-bar {
            width: 100%;
            height: 12px;
            background: var(--grey-light);
            border-radius: 10px;
            overflow: hidden;
        }
        .progress-fill {
            width: 0%; /* JS vai animar isso */
            height: 100%;
            background: var(--primary);
            border-radius: 10px;
            transition: width 1s cubic-bezier(0.25, 1, 0.5, 1); /* Animação suave */
        }
        .progress-labels {
            display: flex;
            justify-content: space-between;
            font-size: 11px;
            font-weight: 500;
            color: var(--grey-dark);
        }

        .rewards-list {
            margin-top: 30px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .reward-card {
            display: flex;
            align-items: center;
            gap: 15px;
            background: var(--white);
            border: 1px solid var(--grey-light);
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }
        .reward-icon {
            width: 50px;
            height: 50px;
            flex-shrink: 0;
            background: var(--primary);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 28px;
            color: var(--secondary);
        }
        .reward-info h4 {
            font-size: 15px;
            font-weight: 600;
        }
        .reward-info p {
            font-size: 13px;
            color: var(--grey-dark);
        }
        .reward-card .btn {
            margin-left: auto;
            flex-shrink: 0;
            padding: 8px 12px;
            font-size: 12px;
        }

        .clube-footer {
            text-align: center;
            font-size: 12px;
            color: var(--grey-mid);
            margin-top: 30px;
        }


        /* --- 10. Tela 4: Feed (Social) --- */
        
        /* Fundo amarelo para a página do feed */
        #page-feed {
            background-color: var(--white); /* MUDADO DE VOLTA */
        }

        /* Adicionando Stories de volta */
        .stories-bar {
            display: flex;
            gap: 15px;
            overflow-x: auto;
            padding: 5px 0 15px 0;
            margin-bottom: 20px;
            /* Esconde a scrollbar */
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        .stories-bar::-webkit-scrollbar { display: none; }

        .story-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            cursor: pointer;
        }
        .story-avatar {
            width: 65px;
            height: 65px;
            border-radius: 50%;
            border: 3px solid var(--primary); /* Borda amarela */
            padding: 2px; /* Espaço entre borda e imagem */
            background-clip: content-box; /* Borda não cobre a imagem */
            background-size: cover;
            background-position: center;
            background-color: var(--white); /* Fundo branco caso a imagem falhe */
        }
        .story-item span {
            font-size: 11px;
            font-weight: 600; /* Mais forte */
            color: var(--secondary); /* Texto preto */
        }
        /* Fim Stories */


        .feed-list {
            display: flex;
            flex-direction: column;
            gap: 15px; /* Espaço entre os balões */
        }

        .tweet-post {
            display: flex;
            gap: 15px;
            background-color: var(--white); /* MUDADO (Balão branco) */
            border-radius: 16px;
            padding: 15px;
            border: 1px solid var(--grey-light); /* ADICIONADO */
            box-shadow: none; /* REMOVIDO */
        }
        .tweet-avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background-size: cover;
            background-position: center;
            flex-shrink: 0;
            border: 2px solid var(--white);
        }
        .tweet-content {
            flex: 1;
        }
        .tweet-header {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
        }
        .tweet-header h4 {
            font-size: 15px;
            font-weight: 700;
            color: var(--secondary);
        }
        .tweet-header span {
            font-size: 13px;
            color: var(--secondary);
            opacity: 0.7;
        }
        .tweet-text {
            font-size: 14px;
            line-height: 1.5;
            color: var(--secondary);
            margin-bottom: 15px;
            word-wrap: break-word; /* Garante quebra de linha */
        }
        .tweet-text strong { /* for @mentions */
            color: #000;
        }
        .tweet-actions {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: var(--secondary);
            opacity: 0.8;
            max-width: 250px; /* Impede que se espalhe demais */
        }
        .tweet-actions .action-btn { /* Classe genérica para botões de ação */
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            color: var(--secondary);
        }
        .tweet-actions .action-btn i {
            font-size: 20px;
        }
        .tweet-actions .action-like.liked { /* Específico para like */
            color: #ff3b30;
        }
        .tweet-actions .action-like.liked i {
            color: #ff3b30;
        }


        /* --- 11. Tela 5: Perfil --- */

        .profile-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 30px;
        }
        .avatar {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            background-size: cover;
            background-position: center;
            border: 3px solid var(--primary);
        }
        .profile-info h2 {
            font-size: 20px;
            font-weight: 700;
        }
        .profile-info p {
            font-size: 14px;
            color: var(--grey-dark);
        }
        .profile-stats {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            background: var(--grey-light);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 30px;
        }
        .stat-item {
            text-align: center;
        }
        .stat-item h3 {
            font-size: 20px;
            font-weight: 700;
            color: var(--secondary);
        }
        .stat-item p {
            font-size: 12px;
            font-weight: 500;
            color: var(--grey-dark);
        }

        .profile-menu {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .menu-button {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background: var(--grey-light);
            border-radius: 12px;
            font-size: 15px;
            font-weight: 600;
            color: var(--secondary);
            cursor: pointer;
            transition: background 0.2s ease;
        }
        .menu-button:active {
            background: #e0e0e0;
        }
        .menu-button i {
            font-size: 22px;
            color: var(--grey-dark);
        }
        .menu-button.danger {
            color: #ff3b30;
        }
        .menu-button.danger i {
            color: #ff3b30;
        }

        /* --- 12. Notificação Inteligente (Toast) --- */

        #notification-toast {
            position: fixed;
            /* Posicionado acima da navbar */
            bottom: calc(var(--nav-height) + 15px);
            left: 20px;
            right: 20px;
            
            /* Ajuste para o container do app */
            max-width: calc(450px - 40px);
            margin: 0 auto;
            
            background: var(--secondary);
            color: var(--white);
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.2);
            font-size: 13px;
            font-weight: 500;
            line-height: 1.4;
            
            /* Animação */
            transform: translateY(200%);
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 500;
        }
        #notification-toast.show {
            transform: translateY(0);
            opacity: 1;
        }

        /* --- 13. Modal Genérico --- */

        #modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            z-index: 900;
            
            display: flex;
            justify-content: center;
            align-items: center;
            
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        #modal-overlay.visible {
            opacity: 1;
            visibility: visible;
        }

        #modal-content {
            background: var(--white);
            /* Ajuste para o container do app */
            width: calc(100% - 40px);
            max-width: calc(450px - 40px);
            
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
            
            transform: scale(0.9);
            opacity: 0;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        #modal-overlay.visible #modal-content {
            transform: scale(1);
            opacity: 1;
        }

        #modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        #modal-title {
            font-size: 20px;
            font-weight: 700;
            width: 90%; /* Evita quebra de linha feia */
        }
        #modal-close {
            font-size: 28px;
            color: var(--grey-mid);
            cursor: pointer;
        }
        #modal-body {
            font-size: 14px;
            line-height: 1.6;
            color: var(--grey-dark);
            max-height: 60vh; /* Limita altura em telas pequenas */
            overflow-y: auto;
        }
        #modal-body p {
            margin-bottom: 15px;
        }
        #modal-body h5 {
            font-size: 15px;
            font-weight: 600;
            color: var(--secondary);
            margin-top: 20px;
            margin-bottom: 10px;
        }
        .modal-image {
            width: 100%;
            height: 180px;
            border-radius: 12px;
            background-size: cover;
            background-position: center;
            margin-bottom: 15px;
        }
        #modal-footer {
            margin-top: 20px;
        }
        
        /* Estilo específico para QR Code no Modal */
        .qr-code-simulator {
            width: 100%;
            max-width: 200px;
            height: 200px;
            margin: 10px auto 20px auto;
            border-radius: 10px;
            background-image:
                linear-gradient(45deg, #0E0E0E 25%, transparent 25%), 
                linear-gradient(-45deg, #0E0E0E 25%, transparent 25%),
                linear-gradient(45deg, transparent 75%, #0E0E0E 75%),
                linear-gradient(-45deg, transparent 75%, #0E0E0E 75%);
            background-size: 20px 20px;
            background-color: var(--white);
            border: 5px solid var(--secondary);
            animation: qr-pulse 1.5s infinite;
        }

        @keyframes qr-pulse {
            0% { box-shadow: 0 0 0 0 rgba(14, 14, 14, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(14, 14, 14, 0); }
            100% { box-shadow: 0 0 0 0 rgba(14, 14, 14, 0); }
        }

    </style>
</head>
<body>
    <!-- Simulação da moldura do celular -->
    <div id="app-container">
        
        <!-- Tela de Splash (Inicialização) -->
        <div id="splash-screen">
            <div class="splash-logo">
                <i class="ph-fill ph-map-pin"></i>
                <span>Let's Fui</span>
            </div>
            <div class="spinner"></div>
        </div>

        <!-- Conteúdo principal do App (Telas roláveis) -->
        <main id="app-content">

            <!-- ============================================= -->
            <!-- == Tela 1: EXPLORAR (Padrão) ================ -->
            <!-- ============================================= -->
            <section class="page active" id="page-explorar">
                <header class="app-header">
                    <i class="ph-fill ph-map-pin"></i>
                    <span>Let's Fui</span>
                </header>
                
                <input type="search" class="search-bar" placeholder="O que você quer fazer hoje?">

                <!-- Banners Rotativos -->
                <div class="banner-carousel" id="banner-carousel">
                    <div class="carousel-wrapper" id="carousel-wrapper">
                        <div class="carousel-slide" style="background-image: url('https://placehold.co/600x300/0E0E0E/FFD60A?text=Promo+Bar+Vileta')">
                            <div class="slide-content">
                                <h3>Happy Hour em Dobro</h3>
                                <p>No Bar Vileta, de seg. a qua.</p>
                            </div>
                        </div>
                        <div class="carousel-slide" style="background-image: url('https://placehold.co/600x300/16a34a/FFFFFF?text=Festival+de+Inverno')">
                            <div class="slide-content">
                                <h3>Festival de Inverno</h3>
                                <p>Música ao vivo no Taquaral.</p>
                            </div>
                        </div>
                        <div class="carousel-slide" style="background-image: url('https://placehold.co/600x300/c2410c/FFFFFF?text=Noite+Italiana')">
                             <div class="slide-content">
                                <h3>Noite Italiana</h3>
                                <p>Menu especial no Bellini.</p>
                            </div>
                        </div>
                    </div>
                    <div class="carousel-pagination" id="carousel-pagination">
                        <span class="dot active"></span>
                        <span class="dot"></span>
                        <span class="dot"></span>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-item active" data-filter="all">
                        <i class="ph-fill ph-sparkle"></i> Todos
                    </div>
                    <div class="filter-item" data-filter="comer">
                        <i class="ph-fill ph-pizza"></i> Comer
                    </div>
                    <div class="filter-item" data-filter="beber">
                        <i class="ph-fill ph-martini"></i> Beber
                    </div>
                    <div class="filter-item" data-filter="eventos">
                        <i class="ph-fill ph-music-notes"></i> Eventos
                    </div>
                    <div class="filter-item" data-filter="pet">
                        <i class="ph-fill ph-dog"></i> Pet Friendly
                    </div>
                    <div class="filter-item" data-filter="ar-livre">
                        <i class="ph-fill ph-tree"></i> Ar Livre
                    </div>
                </div>

                <div class="establishment-list">
                    <!-- Card 1 -->
                    <article class="establishment-card" data-category="beber" 
                             data-title="Bar Vileta" 
                             data-img="https://placehold.co/600x400/0E0E0E/FFD60A?text=Bar+Vileta"
                             data-desc="Um clássico do Cambuí. Chopp gelado, porções generosas e o melhor da gastronomia de boteco.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/0E0E0E/FFD60A?text=Bar+Vileta')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🍸 Beber</span>
                                <span class="card-tag tag-distance">600 m</span>
                            </div>
                            <h3 class="card-title">Bar Vileta</h3>
                            <p class="card-description">Um clássico do Cambuí. Chopp gelado, porções generosas e o melhor da gastronomia de boteco.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>

                    <!-- Card 2 -->
                    <article class="establishment-card" data-category="comer"
                             data-title="Bellini Ristorante"
                             data-img="https://placehold.co/600x400/c2410c/FFFFFF?text=Bellini"
                             data-desc="Alta gastronomia italiana dentro do Hotel Vitória. Ambiente sofisticado e pratos premiados.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/c2410c/FFFFFF?text=Bellini')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🍔 Comer</span>
                                <span class="card-tag tag-distance">1.8 km</span>
                            </div>
                            <h3 class="card-title">Bellini Ristorante</h3>
                            <p class="card-description">Alta gastronomia italiana dentro do Hotel Vitória. Ambiente sofisticado e pratos premiados.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>
                    
                    <!-- Card 3 -->
                    <article class="establishment-card" data-category="ar-livre"
                             data-title="Parque Taquaral"
                             data-img="https://placehold.co/600x400/16a34a/FFFFFF?text=Taquaral"
                             data-desc="Lagoa, bondinho, planetário e muito verde. O ponto de encontro de Campinas para esportes e lazer.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/16a34a/FFFFFF?text=Taquaral')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🌳 Ar Livre</span>
                                <span class="card-tag tag-distance">3.2 km</span>
                            </div>
                            <h3 class="card-title">Parque Taquaral</h3>
                            <p class="card-description">Lagoa, bondinho, planetário e muito verde. O ponto de encontro de Campinas para esportes e lazer.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>

                    <!-- Card 4 (Novo) -->
                    <article class="establishment-card" data-category="beber" 
                             data-title="Seo Rosa" 
                             data-img="https://placehold.co/600x400/a21caf/FFFFFF?text=Seo+Rosa"
                             data-desc="Bar e restaurante badalado no Gramado. Famoso pela feijoada aos sábados e pelo ambiente descontraído.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/a21caf/FFFFFF?text=Seo+Rosa')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🍸 Beber</span>
                                <span class="card-tag tag-distance">1.1 km</span>
                            </div>
                            <h3 class="card-title">Seo Rosa</h3>
                            <p class="card-description">Bar e restaurante badalado no Gramado. Famoso pela feijoada aos sábados e pelo ambiente descontraído.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>

                    <!-- Card 5 (Novo) -->
                    <article class="establishment-card" data-category="comer" 
                             data-title="Padaria Alemã" 
                             data-img="https://placehold.co/600x400/f59e0b/0E0E0E?text=Padaria+Alemã"
                             data-desc="Tradicional em Campinas, oferece pães artesanais, doces incríveis e um café colonial completo.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/f59e0b/0E0E0E?text=Padaria+Alemã')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🍔 Comer</span>
                                <span class="card-tag tag-distance">2.1 km</span>
                            </div>
                            <h3 class="card-title">Padaria Alemã</h3>
                            <p class="card-description">Tradicional em Campinas, oferece pães artesanais, doces incríveis e um café colonial completo.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>
                    
                    <!-- Card 6 (Novo) -->
                    <article class="establishment-card" data-category="eventos" 
                             data-title="Teatro Castro Mendes" 
                             data-img="https://placehold.co/600x400/4f46e5/FFFFFF?text=Teatro"
                             data-desc="O principal palco cultural da cidade, recebendo peças de teatro, shows de música e espetáculos de dança.">
                        <div class="card-image" style="background-image: url('https://placehold.co/600x400/4f46e5/FFFFFF?text=Teatro')"></div>
                        <div class="card-content">
                            <div class="card-tags">
                                <span class="card-tag tag-category">🎶 Eventos</span>
                                <span class="card-tag tag-distance">4.0 km</span>
                            </div>
                            <h3 class="card-title">Teatro Castro Mendes</h3>
                            <p class="card-description">O principal palco cultural da cidade, recebendo peças de teatro, shows de música e espetáculos de dança.</p>
                            <button class="btn btn-secondary btn-block btn-view-more">Ver mais</button>
                        </div>
                    </article>
                </div>
            </section>

            <!-- ============================================= -->
            <!-- == Tela 2: MAPA ============================= -->
            <!-- ============================================= -->
            <section class="page" id="page-mapa">
                <header class="app-header">
                    <div class="location-display">
                        <i class="ph-fill ph-map-pin-line"></i>
                        <span>Cambuí</span>
                    </div>
                    <div class="filter-button">
                        <i class="ph-fill ph-sliders-horizontal"></i>
                    </div>
                </header>
                
                <div id="map-container">
                    <!-- Pins simulados -->
                    <i class="ph-fill ph-map-pin map-pin pin-1" data-title="Bar Vileta" data-img="https://placehold.co/600x400/0E0E0E/FFD60A?text=Bar+Vileta" data-desc="🍸 Beber - 600m"></i>
                    <i class="ph-fill ph-map-pin map-pin pin-2" data-title="Bellini Ristorante" data-img="https://placehold.co/600x400/c2410c/FFFFFF?text=Bellini" data-desc="🍔 Comer - 1.8km"></i>
                    <i class="ph-fill ph-map-pin map-pin pin-3" data-title="Parque Taquaral" data-img="https://placehold.co/600x400/16a34a/FFFFFF?text=Taquaral" data-desc="🌳 Ar Livre - 3.2km"></i>
                    <i class="ph-fill ph-map-pin map-pin pin-4" data-title="Seo Rosa" data-img="https://placehold.co/600x400/a21caf/FFFFFF?text=Seo+Rosa" data-desc="🍸 Beber - 1.1km"></i>
                    
                    <!-- NOVAS MANCHAS DE CALOR (HEATMAPS) -->
                    <div class="heatmap-spot spot-busy" 
                         style="top: 35%; left: 45%;"
                         data-bars='[{"name": "Bar Vileta", "checkins": 28}, {"name": "Bellini", "checkins": 19}]'>
                    </div>
                    <div class="heatmap-spot spot-medium" 
                         style="top: 65%; left: 60%;"
                         data-bars='[{"name": "Seo Rosa", "checkins": 12}]'>
                    </div>
                    
                    
                    <!-- Popup do Pin (escondido) -->
                    <div class="pin-popup" id="pin-popup">
                        <div class="popup-image" id="popup-image"></div>
                        <div class="popup-info">
                            <h4 id="popup-title">Nome do Local</h4>
                            <p id="popup-desc">Categoria e distância</p>
                        </div>
                        <div class="popup-action">
                            <button class="btn btn-primary btn-popup btn-view-more">Ver</button>
                        </div>
                    </div>
                    
                    <!-- NOVO POPUP DO HEATMAP (escondido) -->
                    <div id="heatmap-popup">
                        <h4><i class="ph-fill ph-fire"></i> Em Alta Agora</h4>
                        <ul id="heatmap-popup-list">
                            <!-- JS vai popular aqui -->
                        </ul>
                    </div>
                </div>
            </section>

            <!-- ============================================= -->
            <!-- == Tela 3: CLUBE LET'S FUI ================== -->
            <!-- ============================================= -->
            <section class="page" id="page-clube">
                <header class="app-header">
                    <span>Clube Let's Fui 💛</span>
                </header>
                
                <div class="points-display">
                    <h1 id="user-points">420</h1>
                    <p>Pontos acumulados</p>
                </div>

                <div class="progress-bar-container">
                    <div class="progress-bar">
                        <div class="progress-fill" id="progress-fill"></div>
                    </div>
                    <div class="progress-labels">
                        <span>Nível Bronze</span>
                        <span>Próximo Nível: 1000 pts</span>
                    </div>
                </div>

                <div class="rewards-list">
                    <h3 class="section-title">Recompensas Disponíveis</h3>
                    
                    <article class="reward-card">
                        <div class="reward-icon"><i class="ph-fill ph-percent"></i></div>
                        <div class="reward-info">
                            <h4>Desconto 10%</h4>
                            <p>Válido no Bar Vileta</p>
                        </div>
                        <button class="btn btn-primary btn-resgatar-recompensa" data-cost="100">100 pts</button>
                    </article>
                    
                    <article class="reward-card">
                        <div class="reward-icon"><i class="ph-fill ph-gift"></i></div>
                        <div class="reward-info">
                            <h4>Brinde Exclusivo</h4>
                            <p>Sobremesa no Bellini</p>
                        </div>
                        <button class="btn btn-primary btn-resgatar-recompensa" data-cost="250">250 pts</button>
                    </article>

                    <article class="reward-card">
                        <div class="reward-icon"><i class="ph-fill ph-ticket"></i></div>
                        <div class="reward-info">
                            <h4>Passeio de Bondinho</h4>
                            <p>Grátis no Parque Taquaral</p>
                        </div>
                        <button class="btn btn-primary btn-resgatar-recompensa" data-cost="500">500 pts</button>
                    </article>
                </div>

                <p class="clube-footer">
                    Cada experiência vale pontos. Cada ponto, novas descobertas.
                </p>
            </section>

            <!-- ============================================= -->
            <!-- == Tela 4: FEED (SOCIAL) ==================== -->
            <!-- ============================================= -->
            <section class="page" id="page-feed">
                <header class="app-header">
                    <span>Feed</span>
                </header>
                
                <!-- Stories dos Estabelecimentos -->
                <h3 class="section-title">Stories</h3>
                <div class="stories-bar">
                    <div class="story-item">
                        <div class="story-avatar" style="background-image: url('https://placehold.co/100x100/0E0E0E/FFD60A?text=BV')"></div>
                        <span>Bar Vileta</span>
                    </div>
                    <div class="story-item">
                        <div class="story-avatar" style="background-image: url('https://placehold.co/100x100/a21caf/FFFFFF?text=SR')"></div>
                        <span>Seo Rosa</span>
                    </div>
                    <div class="story-item">
                        <div class="story-avatar" style="background-image: url('https://placehold.co/100x100/16a34a/FFFFFF?text=T')"></div>
                        <span>Taquaral</span>
                    </div>
                    <div class="story-item">
                        <div class="story-avatar" style="background-image: url('https://placehold.co/100x100/c2410c/FFFFFF?text=B')"></div>
                        <span>Bellini</span>
                    </div>
                    <div class="story-item">
                        <div class="story-avatar" style="background-image: url('https://placehold.co/100x100/f59e0b/0E0E0E?text=PA')"></div>
                        <span>P. Alemã</span>
                    </div>
                </div>


                <h3 class="section-title">Bombando agora 🔥</h3>
                
                <div class="feed-list">
                    <!-- Post 1 (Estabelecimento) -->
                    <article class="tweet-post">
                        <div class="tweet-avatar" style="background-image: url('https://placehold.co/100x100/a21caf/FFFFFF?text=SR')"></div>
                        <div class="tweet-content">
                            <div class="tweet-header">
                                <h4>Seo Rosa</h4>
                                <span>@SeoRosaCps · 15m</span>
                            </div>
                            <p class="tweet-text">
                                🔥 PROMOÇÃO RELÂMPAGO! 🔥 Aqui no <strong>@SeoRosa</strong> (Gramado), pedindo uma feijoada, a primeira caipirinha é por nossa conta! Só hoje!
                                #LetsFui #Feijoada #Promoção
                            </p>
                            <div class="tweet-actions">
                                <span class="action-btn action-reply"><i class="ph ph-chat-circle"></i> <span>21</span></span>
                                <span class="action-btn action-retweet"><i class="ph ph-arrows-counter-clockwise"></i> <span>40</span></span>
                                <span class="action-btn action-like"><i class="ph ph-heart"></i> <span>112</span></span>
                                <span class="action-btn action-share"><i class="ph ph-paper-plane-tilt"></i></span>
                            </div>
                        </div>
                    </article>
                    
                    <!-- Post 2 (Estabelecimento) -->
                    <article class="tweet-post">
                        <div class="tweet-avatar" style="background-image: url('https://placehold.co/100x100/16a34a/FFFFFF?text=T')"></div>
                        <div class="tweet-content">
                            <div class="tweet-header">
                                <h4>Parque Taquaral</h4>
                                <span>@TaquaralOficial · 1h</span>
                            </div>
                            <p class="tweet-text">
                                Dia lindo por aqui! ☀️ Perfeito para um passeio na Lagoa. Não esqueça de resgatar seu passeio de bondinho no app!
                            </p>
                            <div class="tweet-actions">
                                <span class="action-btn action-reply"><i class="ph ph-chat-circle"></i> <span>12</span></span>
                                <span class="action-btn action-retweet"><i class="ph ph-arrows-counter-clockwise"></i> <span>30</span></span>
                                <span class="action-btn action-like liked"><i class="ph-fill ph-heart"></i> <span>102</span></span>
                                <span class="action-btn action-share"><i class="ph ph-paper-plane-tilt"></i></span>
                            </div>
                        </div>
                    </article>
                    
                    <!-- Post 3 (Estabelecimento) -->
                    <article class="tweet-post">
                        <div class="tweet-avatar" style="background-image: url('https://placehold.co/100x100/0E0E0E/FFD60A?text=BV')"></div>
                        <div class="tweet-content">
                            <div class="tweet-header">
                                <h4>Bar Vileta</h4>
                                <span>@BarVileta · 3h</span>
                            </div>
                            <p class="tweet-text">
                                Quinta é quase sexta! 🍻 Happy hour rolando solto aqui no Cambuí. Marque os amigos que vão te pagar uma rodada hoje!
                            </p>
                            <div class="tweet-actions">
                                <span class="action-btn action-reply"><i class="ph ph-chat-circle"></i> <span>19</span></span>
                                <span class="action-btn action-retweet"><i class="ph ph-arrows-counter-clockwise"></i> <span>22</span></span>
                                <span class="action-btn action-like"><i class="ph ph-heart"></i> <span>89</span></span>
                                <span class="action-btn action-share"><i class="ph ph-paper-plane-tilt"></i></span>
                            </div>
                        </div>
                    </article>
                    
                </div>
            </section>

            <!-- ============================================= -->
            <!-- == Tela 5: PERFIL =========================== -->
            <!-- ============================================= -->
            <section class="page" id="page-perfil">
                <div class="profile-header">
                    <div class="avatar" style="background-image: url('https://placehold.co/100x100/eab308/0E0E0E?text=LF')"></div>
                    <div class="profile-info">
                        <h2>Olá, Luis Felipe 👋</h2>
                        <p>Campinas, SP</p>
                    </div>
                </div>

                <div class="profile-stats">
                    <div class="stat-item">
                        <h3 id="stat-checkins">17</h3>
                        <p>Check-ins</p>
                    </div>
                    <div class="stat-item">
                        <h3 id="stat-pontos">420</h3>
                        <p>Pontos Totais</p>
                    </div>
                </div>

                <div class="profile-menu">
                    <h3 class="section-title">Minha Conta</h3>
                    <div class="menu-button">
                        <i class="ph-fill ph-bookmarks-simple"></i>
                        <span>Minhas Experiências</span>
                    </div>
                    <div class="menu-button">
                        <i class="ph-fill ph-gear-six"></i>
                        <span>Configurações</span>
                    </div>
                    <div class="menu-button">
                        <i class="ph-fill ph-bell"></i>
                        <span>Notificações</span>
                    </div>
                    <div classs="menu-button danger">
                        <i class="ph-fill ph-sign-out"></i>
                        <span>Sair</span>
                    </div>
                </div>

            </section>

        </main>

        <!-- Navbar Fixa na Base -->
        <nav id="bottom-navbar">
            <div class="nav-item active" data-page="page-explorar">
                <i class="ph-fill ph-house"></i>
                <span>Explorar</span>
            </div>
            <div class="nav-item" data-page="page-mapa">
                <i class="ph-fill ph-map-trifold"></i>
                <span>Mapa</span>
            </div>
            <div class="nav-item" data-page="page-clube">
                <i class="ph-fill ph-medal"></i>
                <span>Clube</span>
            </div>
            <div class="nav-item" data-page="page-feed">
                <i class="ph-fill ph-chats-circle"></i>
                <span>Feed</span>
            </div>
            <div class="nav-item" data-page="page-perfil">
                <i class="ph-fill ph-user-circle"></i>
                <span>Perfil</span>
            </div>
        </nav>

        <!-- Notificação "Toast" (escondida) -->
        <div id="notification-toast">
            👋 Ei, Luis Felipe! Vimos que você está no <strong>Cambuí</strong>. Descubra experiências incríveis por aqui!
        </div>

    </div> <!-- Fim #app-container -->

    
    <!-- ============================================= -->
    <!-- == Modal Genérico (Detalhes, QR Code, etc) == -->
    <!-- ============================================= -->
    <div id="modal-overlay">
        <div id="modal-content">
            <div id="modal-header">
                <h3 id="modal-title">Título do Modal</h3>
                <i class="ph-fill ph-x-circle" id="modal-close"></i>
            </div>
            <div id="modal-body">
                <!-- Conteúdo dinâmico aqui -->
                <p>Conteúdo padrão do modal.</p>
            </div>
            <div id="modal-footer">
                <!-- Botões dinâmicos aqui -->
            </div>
        </div>
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', () => {

            // --- Variáveis Globais ---
            const navItems = document.querySelectorAll('.nav-item');
            const pages = document.querySelectorAll('.page');
            const appContent = document.getElementById('app-content');
            
            // Modal
            const modalOverlay = document.getElementById('modal-overlay');
            const modalCloseBtn = document.getElementById('modal-close');
            const modalTitle = document.getElementById('modal-title');
            const modalBody = document.getElementById('modal-body');
            const modalFooter = document.getElementById('modal-footer');
            
            // Elementos
            const userPointsEl = document.getElementById('user-points');
            const statPontosEl = document.getElementById('stat-pontos');
            const statCheckinsEl = document.getElementById('stat-checkins');

            // Carousel
            const carouselWrapper = document.getElementById('carousel-wrapper');
            const carouselDots = document.querySelectorAll('#carousel-pagination .dot');
            let currentSlide = 0;
            const totalSlides = 3;
            let carouselInterval; // Será definido em startCarousel()

            // Filtros (NOVO)
            const filterItems = document.querySelectorAll('.filter-item');
            const establishmentCards = document.querySelectorAll('.establishment-card');
            
            let currentPageId = 'page-explorar';
            let notificationTimer;
            let userPoints = 420; // Pontos iniciais
            let userCheckins = 17; // Check-ins iniciais

            // --- 1. Lógica da Splash Screen ---
            
            setTimeout(() => {
                document.getElementById('splash-screen').classList.add('hidden');
            }, 2000); // 2 segundos de splash

            // --- 1.5. Lógica do Carousel ---
            function startCarousel() {
                // Limpa intervalo anterior para evitar duplicação
                if(carouselInterval) clearInterval(carouselInterval);
                
                carouselInterval = setInterval(() => {
                    currentSlide = (currentSlide + 1) % totalSlides;
                    updateCarousel();
                }, 4000); // Muda a cada 4 segundos
            }

            function updateCarousel() {
                if (carouselWrapper) {
                    carouselWrapper.style.transform = `translateX(-${currentSlide * 100 / totalSlides}%)`;
                }
                if (carouselDots && carouselDots.length > 0) {
                    carouselDots.forEach((dot, index) => {
                        dot.classList.toggle('active', index === currentSlide);
                    });
                }
            }
            
            // Inicia o carousel na primeira carga
            if (carouselWrapper) {
                startCarousel();
            }

            // --- 1.7. Lógica dos Filtros (NOVO) ---
            filterItems.forEach(item => {
                item.addEventListener('click', () => {
                    // 1. Atualiza o estado 'active' no botão
                    filterItems.forEach(i => i.classList.remove('active'));
                    item.classList.add('active');
                    
                    const filter = item.dataset.filter;

                    // 2. Filtra os cards
                    establishmentCards.forEach(card => {
                        const category = card.dataset.category;
                        
                        // Mostra o card se o filtro for 'all' OU se a categoria do card bate com o filtro
                        if (filter === 'all' || category === filter) {
                            card.classList.remove('hidden');
                        } else {
                            card.classList.add('hidden'); // Esconde
                        }
                    });
                });
            });


            // --- 2. Lógica de Navegação SPA ---
            
            navItems.forEach(item => {
                item.addEventListener('click', () => {
                    const pageId = item.dataset.page;
                    navigateTo(pageId);
                });
            });

            function navigateTo(pageId) {
                if (pageId === currentPageId) return; // Não faz nada se já está na página

                // Desativa a página e item antigos
                const oldPage = document.getElementById(currentPageId);
                const oldNavItem = document.querySelector(`.nav-item[data-page="${currentPageId}"]`);
                
                if (oldPage) oldPage.classList.add('exit');
                if (oldNavItem) oldNavItem.classList.remove('active');

                // Ativa a nova página e item
                const newPage = document.getElementById(pageId);
                const newNavItem = document.querySelector(`.nav-item[data-page="${pageId}"]`);
                
                if (newPage) {
                    newPage.classList.remove('exit'); // Garante que não tenha a classe de saída
                    newPage.classList.add('active');
                    // Reseta o scroll da nova página ao entrar
                    newPage.scrollTop = 0;
                }
                if (newNavItem) newNavItem.classList.add('active');

                // Limpa a classe 'exit' da página antiga após a transição
                if(oldPage) {
                    setTimeout(() => {
                        oldPage.classList.remove('active');
                        oldPage.classList.remove('exit');
                    }, 300); // Mesmo tempo da transição CSS
                }

                currentPageId = pageId;

                // --- Lógicas Específicas de cada Página ---
                
                // Limpa o timer de notificação
                if (notificationTimer) clearTimeout(notificationTimer);
                
                // Pausa/Inicia o carousel
                if (pageId === 'page-explorar') {
                    if(carouselWrapper) startCarousel(); // Reinicia
                } else {
                    if(carouselInterval) clearInterval(carouselInterval);
                    carouselInterval = null; // Limpa o ID do intervalo
                }


                // Anima a barra de progresso ao entrar no Clube
                if (pageId === 'page-clube') {
                    updatePointsDisplay(); // Garante que os pontos estão corretos
                    setTimeout(() => {
                        const progress = (userPoints / 1000) * 100; // Nível em 1000 pts
                        const progressFill = document.getElementById('progress-fill');
                        if (progressFill) {
                            progressFill.style.width = `${Math.min(progress, 100)}%`;
                        }
                    }, 200); // Pequeno delay para a animação
                } else {
                    // Reseta a barra ao sair para animar na próxima vez
                    const progressFill = document.getElementById('progress-fill');
                    if(progressFill) {
                        progressFill.style.width = '0%';
                    }
                }

                // Dispara notificação inteligente
                if (pageId === 'page-perfil' || pageId === 'page-mapa') {
                    notificationTimer = setTimeout(showNotification, 5000); // 5 segundos
                }
            }


            // --- 3. Lógica do Modal (Detalhes do Estabelecimento) ---
            
            // Precisa usar delegação de evento, pois os botões do mapa são criados dinamicamente
            document.getElementById('app-content').addEventListener('click', (e) => {
                if (e.target.classList.contains('btn-view-more')) {
                    // Pega dados do card pai
                    const card = e.target.closest('.establishment-card, .pin-popup');
                    
                    let title, img, desc, category;
                    
                    if(card && card.classList.contains('establishment-card')) {
                         title = card.dataset.title;
                         img = card.dataset.img;
                         desc = card.dataset.desc;
                         category = card.dataset.category;
                    } else if (card && card.classList.contains('pin-popup')) {
                         // Lógica para o popup do mapa
                         const pinTitle = card.querySelector('#popup-title').textContent;
                         const pin = document.querySelector(`.map-pin[data-title="${pinTitle}"]`);
                         
                         if (!pin) return; // Proteção
                         
                         title = pin.dataset.title;
                         img = pin.dataset.img;
                         desc = `Descrição detalhada do ${title} vinda do banco de dados... Lorem ipsum dolor sit amet, consectetur adipiscing elit. Perfeito para curtir no Cambuí.`;
                         category = pin.dataset.desc.split(' - ')[0]; // Pega a categoria (ícone)
                    } else {
                        return; // Não é um botão que nos interessa
                    }

                    // Preenche o modal
                    modalTitle.textContent = title;
                    modalBody.innerHTML = `
                        <div class="modal-image" style="background-image: url('${img}')"></div>
                        <p>${desc}</p>
                        <h5>Horários</h5>
                        <p>
                            Seg - Sex: 18:00 - 00:00<br>
                            Sáb - Dom: 12:00 - 01:00
                        </p>
                    `;
                    modalFooter.innerHTML = `<button class="btn btn-primary btn-block" id="modal-btn-resgatar-cupom">Resgatar Cupom (+20 pts)</button>`;
                    
                    // Adiciona listener ao novo botão do footer
                    document.getElementById('modal-btn-resgatar-cupom').addEventListener('click', () => {
                        addPoints(20);
                        userCheckins++;
                        if (statCheckinsEl) {
                            statCheckinsEl.textContent = userCheckins;
                        }
                        
                        // Simula feedback
                        showToast(`+20 pontos ganhos no ${title}!`, 'success');
                        modalOverlay.classList.remove('visible');
                    }, { once: true }); // Garante que o listener seja adicionado apenas uma vez

                    // Exibe o modal
                    modalOverlay.classList.add('visible');
                }
            });

            // Fechar Modal
            if (modalCloseBtn) {
                modalCloseBtn.addEventListener('click', () => modalOverlay.classList.remove('visible'));
            }
            if (modalOverlay) {
                modalOverlay.addEventListener('click', (e) => {
                    // Fecha ao clicar fora do conteúdo
                    if (e.target === modalOverlay) {
                        modalOverlay.classList.remove('visible');
                    }
                });
            }


            // --- 4. Lógica da Tela "Clube" (Resgatar Recompensa) ---

            document.querySelectorAll('.btn-resgatar-recompensa').forEach(button => {
                button.addEventListener('click', (e) => {
                    const cost = parseInt(e.target.dataset.cost);
                    const rewardCard = e.target.closest('.reward-card');
                    if (!rewardCard) return;
                    
                    const rewardTitleEl = rewardCard.querySelector('h4');
                    if (!rewardTitleEl) return;
                    
                    const rewardTitle = rewardTitleEl.textContent;
                    
                    if (userPoints >= cost) {
                        // Tem pontos
                        userPoints -= cost;
                        updatePointsDisplay();
                        showQrCodeModal(rewardTitle, cost);
                    } else {
                        // Não tem pontos
                        showToast("Ops! Pontos insuficientes.", 'error');
                    }
                });
            });
            
            function showQrCodeModal(title, cost) {
                modalTitle.textContent = "Resgate Efetuado!";
                modalBody.innerHTML = `
                    <p>Apresente este QR Code no estabelecimento para resgatar seu <strong>${title}</strong>.</p>
                    <div class="qr-code-simulator"></div>
                    <p style="text-align: center; font-weight: 600; color: var(--secondary); margin-top: 15px;">
                        -${cost} pontos debitados.
                    </p>
                `;
                modalFooter.innerHTML = `<button class="btn btn-secondary btn-block" id="modal-btn-fechar-qr">Fechar</button>`;
                
                document.getElementById('modal-btn-fechar-qr').addEventListener('click', () => {
                     modalOverlay.classList.remove('visible');
                }, { once: true });
                
                modalOverlay.classList.add('visible');
            }


            // --- 5. Lógica da Tela "Mapa" (Pop-up dos Pins) ---
            
            const pins = document.querySelectorAll('.map-pin');
            const pinPopup = document.getElementById('pin-popup');
            const popupImage = document.getElementById('popup-image');
            const popupTitle = document.getElementById('popup-title');
            const popupDesc = document.getElementById('popup-desc');
            
            // --- NOVAS VARIÁVEIS: Heatmap ---
            const heatmapSpots = document.querySelectorAll('.heatmap-spot');
            const heatmapPopup = document.getElementById('heatmap-popup');
            const heatmapPopupList = document.getElementById('heatmap-popup-list');
            const mapContainer = document.getElementById('map-container'); // Já deve existir, mas garantindo

            pins.forEach(pin => {
                pin.addEventListener('click', (e) => {
                    e.stopPropagation(); // Impede que o clique no pin feche ele mesmo
                    if (!pinPopup || !popupTitle || !popupDesc || !popupImage) return;
                    
                    // Esconde o popup do heatmap se estiver aberto
                    if (heatmapPopup) heatmapPopup.style.display = 'none';
                    
                    // Preenche o popup
                    popupTitle.textContent = pin.dataset.title;
                    popupImage.style.backgroundImage = `url('${pin.dataset.img}')`;
                    
                    // Exibe o popup
                    pinPopup.classList.add('show');
                });
            });
            
            // --- 5.5. NOVA LÓGICA: Heatmap Popup ---
            heatmapSpots.forEach(spot => {
                spot.addEventListener('click', (e) => {
                    e.stopPropagation(); // Impede que o clique no mapa feche o popup
                    
                    // 1. Esconde o popup de pin
                    if (pinPopup) pinPopup.classList.remove('show');
                    
                    // 2. Pega os dados
                    const barsData = JSON.parse(spot.dataset.bars);
                    
                    // 3. Popula a lista
                    heatmapPopupList.innerHTML = ''; // Limpa
                    barsData.forEach(bar => {
                        const li = document.createElement('li');
                        li.innerHTML = `<strong>${bar.name}</strong> - ${bar.checkins} check-ins`;
                        heatmapPopupList.appendChild(li);
                    });

                    // 4. Posiciona o popup
                    // Pega a posição do spot
                    const spotRect = spot.getBoundingClientRect();
                    const containerRect = mapContainer.getBoundingClientRect();
                    
                    // Calcula a posição relativa ao map-container
                    const spotTopInContainer = spotRect.top - containerRect.top;
                    const spotLeftInContainer = spotRect.left - containerRect.left;

                    // Posiciona o popup centralizado acima do spot
                    let popupTop = spotTopInContainer - 100; // 100 é altura estimada do popup + espaço
                    let popupLeft = spotLeftInContainer + (spotRect.width / 2) - 100; // 100 é metade da largura (200px)

                    // Ajusta para não sair da tela (dentro do mapContainer)
                    if (popupTop < 10) popupTop = 10;
                    if (popupLeft < 10) popupLeft = 10;
                    if (popupLeft + 200 > containerRect.width - 10) popupLeft = containerRect.width - 210;

                    heatmapPopup.style.left = `${popupLeft}px`; 
                    heatmapPopup.style.top = `${popupTop}px`;
                    
                    // 5. Exibe
                    heatmapPopup.style.display = 'block';
                });
            });


            // Esconde os popups se clicar fora deles (no mapa)
            if (mapContainer) {
                mapContainer.addEventListener('click', (e) => {
                    // Se o clique for DIRETAMENTE no map-container (não num pin ou spot)
                    if (e.target.id === 'map-container') {
                        if (pinPopup) pinPopup.classList.remove('show');
                        if (heatmapPopup) heatmapPopup.style.display = 'none';
                    }
                });
            }
            // O botão "Ver" dentro do popup usa a lógica de modal (.btn-view-more)


            // --- 6. Lógica da Tela "Feed" (Like) --- (MODIFICADO)
            
            // Usa delegação de evento na página do feed
            const pageFeed = document.getElementById('page-feed');
            if (pageFeed) {
                pageFeed.addEventListener('click', (e) => {
                    const likeBtn = e.target.closest('.action-like');
                    if (likeBtn) {
                        toggleLike(likeBtn);
                    }
                });
            }

            function toggleLike(btn) {
                btn.classList.toggle('liked');
                const icon = btn.querySelector('i');
                const countEl = btn.querySelector('span'); // Pega o <span> dentro do botão
                if (!icon || !countEl) return;
                
                let count = parseInt(countEl.textContent);
                
                if (btn.classList.contains('liked')) {
                    icon.className = 'ph-fill ph-heart';
                    count++;
                } else {
                    icon.className = 'ph ph-heart';
                    count--;
                }
                countEl.textContent = `${count}`; // Atualiza o <span>
            }
            

            // --- 7. Lógica da "Notificação Inteligente" (Toast) ---
            
            const toast = document.getElementById('notification-toast');
            function showNotification() {
                if (toast) {
                    toast.classList.add('show');
                    // Esconde após 4 segundos
                    setTimeout(() => {
                        toast.classList.remove('show');
                    }, 4000);
                }
            }
            
            // Função genérica de Toast (para feedbacks)
            function showToast(message, type = 'default') {
                const genericToast = document.createElement('div');
                genericToast.id = 'notification-toast'; // Reusa o estilo
                genericToast.textContent = message;
                
                if(type === 'error') {
                    genericToast.style.background = '#ff3b30'; // Vermelho
                }
                if(type === 'success') {
                    genericToast.style.background = '#34c759'; // Verde
                }
                
                const appContainer = document.getElementById('app-container');
                if (!appContainer) return;
                
                appContainer.appendChild(genericToast);
                
                // Força o 'reflow' para a animação funcionar
                requestAnimationFrame(() => {
                    genericToast.classList.add('show');
                });
                
                setTimeout(() => {
                    genericToast.classList.remove('show');
                    setTimeout(() => {
                        genericToast.remove();
                    }, 500); // Remove do DOM após a animação de saída
                }, 3000);
            }
            

            // --- 8. Funções Utilitárias ---
            
            function updatePointsDisplay() {
                if (!userPointsEl || !statPontosEl) return;
                
                // Anima os pontos mudando
                const startPoints = parseInt(userPointsEl.textContent) || 0;
                const diff = userPoints - startPoints;
                
                if(diff === 0) return;

                let duration = 500; // 0.5s
                let startTime = null;

                function animate(timestamp) {
                    if (!startTime) startTime = timestamp;
                    const elapsed = timestamp - startTime;
                    const progress = Math.min(elapsed / duration, 1);
                    
                    const currentAnimatedPoints = Math.floor(startPoints + (diff * progress));
                    
                    userPointsEl.textContent = currentAnimatedPoints;
                    statPontosEl.textContent = currentAnimatedPoints;
                    
                    if (progress < 1) {
                        requestAnimationFrame(animate);
                    } else {
                        userPointsEl.textContent = userPoints; // Garante valor final
                        statPontosEl.textContent = userPoints;
                    }
                }
                requestAnimationFrame(animate);
            }

            function addPoints(pointsToAdd) {
                userPoints += pointsToAdd;
                updatePointsDisplay();
            }

        });
    </script>
</body>
</html>
