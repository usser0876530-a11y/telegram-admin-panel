<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel - Bot Telegram</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        
        .header {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .header h1 {
            color: #667eea;
            margin-bottom: 10px;
            font-size: 28px;
        }
        
        .header p {
            color: #666;
            font-size: 14px;
        }
        
        .card {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 15px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.08);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
        }
        
        .card h2 {
            color: #333;
            font-size: 20px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .card p {
            color: #666;
            font-size: 14px;
            margin-bottom: 15px;
            line-height: 1.6;
        }
        
        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 14px 28px;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }
        
        .btn:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
        }
        
        .btn:active {
            transform: scale(0.98);
        }
        
        .btn-secondary {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            box-shadow: 0 4px 15px rgba(245, 87, 108, 0.4);
        }
        
        .btn-secondary:hover {
            box-shadow: 0 6px 20px rgba(245, 87, 108, 0.6);
        }
        
        .stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 15px;
        }
        
        .stat-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }
        
        .stat-value {
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
        }
        
        .stat-label {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
        }
        
        .alert {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 14px;
            color: #856404;
        }
        
        .footer {
            text-align: center;
            color: rgba(255,255,255,0.8);
            font-size: 12px;
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🛡️ Panel Administration</h1>
            <p>Interface de gestion avancée</p>
        </div>

        <div class="alert">
            ⚠️ <strong>Important :</strong> Utilisez les outils de tracking IP de manière légale et éthique, avec le consentement des utilisateurs.
        </div>

        <div class="card">
            <h2>🔍 IP Tracker Légal</h2>
            <p>Générez des liens de tracking avec Grabify ou IPLogger. N'oubliez pas d'informer l'utilisateur de la collecte de données.</p>
            <button class="btn" onclick="openGrabify()">🌐 Ouvrir Grabify</button>
            <button class="btn btn-secondary" onclick="openIPLogger()" style="margin-top: 10px;">📊 Ouvrir IPLogger</button>
        </div>

        <div class="card">
            <h2>👥 Gestion des Utilisateurs</h2>
            <p>Statistiques en temps réel de votre communauté</p>
            <div class="stats">
                <div class="stat-item">
                    <div class="stat-value" id="totalUsers">0</div>
                    <div class="stat-label">Utilisateurs</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="verifiedUsers">0</div>
                    <div class="stat-label">Vérifiés</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="bannedUsers">0</div>
                    <div class="stat-label">Bannis</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="activeUsers">0</div>
                    <div class="stat-label">Actifs</div>
                </div>
            </div>
            <button class="btn" onclick="refreshStats()" style="margin-top: 15px;">🔄 Actualiser</button>
        </div>

        <div class="card">
            <h2>📢 Diffusion Rapide</h2>
            <p>Envoyez un message à tous vos utilisateurs</p>
            <button class="btn" onclick="sendBroadcast()">📤 Envoyer un Broadcast</button>
        </div>

        <div class="card">
            <h2>📊 Analytics</h2>
            <p>Consultez les statistiques détaillées de votre bot</p>
            <button class="btn" onclick="viewAnalytics()">📈 Voir les Analytics</button>
        </div>

        <div class="footer">
            Développé avec ❤️ pour Telegram<br>
            © 2026 - Panel Admin Bot
        </div>
    </div>

    <script>
        // Initialiser Telegram Web App
        let tg = window.Telegram.WebApp;
        tg.expand();
        tg.ready();

        // Fonction pour ouvrir Grabify
        function openGrabify() {
            tg.openLink('https://grabify.link');
        }

        // Fonction pour ouvrir IPLogger
        function openIPLogger() {
            tg.openLink('https://iplogger.org');
        }

        // Simuler des statistiques (remplacez par de vraies données de votre bot)
        function refreshStats() {
            document.getElementById('totalUsers').textContent = Math.floor(Math.random() * 1000) + 500;
            document.getElementById('verifiedUsers').textContent = Math.floor(Math.random() * 500) + 200;
            document.getElementById('bannedUsers').textContent = Math.floor(Math.random() * 50);
            document.getElementById('activeUsers').textContent = Math.floor(Math.random() * 300) + 100;
            
            tg.showAlert('✅ Statistiques actualisées !');
        }

        // Fonction broadcast
        function sendBroadcast() {
            tg.showAlert('Cette fonctionnalité sera bientôt disponible. Pour l\'instant, utilisez le bot directement avec /admin');
        }

        // Fonction analytics
        function viewAnalytics() {
            tg.showAlert('Dashboard analytics en développement. Utilisez /admin dans le bot pour voir les stats complètes.');
        }

        // Charger les stats au démarrage
        refreshStats();

        // Configurer le bouton principal
        tg.MainButton.text = "Fermer";
        tg.MainButton.show();
        tg.MainButton.onClick(function() {
            tg.close();
        });
    </script>
</body>
</html>
