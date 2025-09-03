<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IMOB - Sistema de Gestão Imobiliária</title>
    <!-- Font Awesome para Ícones -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* CSS Variables para fácil customização */
        :root {
            --primary-color: #3498db;
            --primary-color-dark: #2980b9;
            --secondary-color: #2c3e50;
            --light-gray: #f8f9fa;
            --dark-gray: #34495e;
            --border-color: #ddd;
            --success-bg: #d4edda;
            --success-color: #155724;
            --error-bg: #f8d7da;
            --error-color: #721c24;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        /* Reset e Estilos Globais */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: var(--font-family);
        }
        
        body {
            display: flex;
            min-height: 100vh;
            color: #333;
            background-color: var(--light-gray);
            overflow: hidden; /* Evita scrollbars desnecessários */
        }
        
        /* Layout Dividido */
        .image-section {
            flex: 1;
            background-image: url('https://www.papoimobiliario.com/wp-content/uploads/2022/04/Por-dentro-de-uma-imobiliaria-online-como-funciona-esse-modelo-de-negocio-800x533.jpeg');
            background-size: cover;
            background-position: center;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .image-section::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(45deg, rgba(30, 60, 114, 0.8), rgba(52, 152, 219, 0.7));
        }
        
        .image-content {
            position: relative;
            z-index: 1;
            color: white;
            padding: 40px;
            text-align: center;
        }
        
        .image-content h1 {
            font-size: 2.8rem;
            margin-bottom: 20px;
            font-weight: 700;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            cursor: pointer; /* Indica que é clicável */
        }
        
        .image-content p {
            font-size: 1.2rem;
            line-height: 1.6;
            max-width: 600px;
            opacity: 0.9;
        }
        
        .auth-section {
            width: 100%;
            max-width: 500px;
            padding: 40px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            background: white;
            transition: transform 0.5s ease-in-out;
        }

        /* Container Principal com Animação */
        .page {
            width: 100%;
            display: none;
            animation: fadeIn 0.5s ease-in-out;
        }
        .page.active {
            display: block;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* Box de Autenticação */
        .auth-box {
            background: white;
            border-radius: 10px;
        }
        
        .auth-logo {
            text-align: center;
            margin-bottom: 25px;
        }
        
        .auth-logo img {
            max-width: 180px;
            margin-bottom: 10px;
        }
        
        .auth-title {
            color: var(--secondary-color);
            font-size: 1.8rem;
            font-weight: 600;
        }
        
        /* Formulários */
        .form-group {
            margin-bottom: 20px;
            position: relative;
        }
        
        .form-group .fa-solid {
            position: absolute;
            left: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: #aaa;
        }
        
        .form-group .password-toggle {
            position: absolute;
            right: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: #aaa;
            cursor: pointer;
        }

        .auth-form label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--dark-gray);
            font-size: 0.9rem;
        }
        
        .auth-form input {
            width: 100%;
            padding: 12px 15px 12px 45px; /* Espaço para o ícone */
            border: 1px solid var(--border-color);
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s;
        }

        .auth-form input[type="password"] {
            padding-right: 45px; /* Espaço para o ícone de visibilidade */
        }
        
        .auth-form input:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
        }
        
        .form-options {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            font-size: 0.9rem;
        }
        
        .remember-me { display: flex; align-items: center; }
        .remember-me input { width: auto; margin-right: 8px; }
        .forgot-password { color: var(--primary-color); text-decoration: none; }
        .forgot-password:hover { text-decoration: underline; }
        
        /* Botões */
        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            padding: 14px;
            background: var(--primary-color);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }
        
        .btn:hover { background: var(--primary-color-dark); }
        .btn .spinner {
            display: none;
            width: 20px;
            height: 20px;
            border: 2px solid rgba(255, 255, 255, 0.5);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        .btn.loading .btn-text { display: none; }
        .btn.loading .spinner { display: block; }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* Rodapé do Auth */
        .auth-footer {
            text-align: center;
            margin-top: 25px;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        .auth-footer a { color: var(--primary-color); text-decoration: none; font-weight: 600; }
        .auth-footer a:hover { text-decoration: underline; }
        
        /* Mensagens */
        .message {
            padding: 12px;
            margin-bottom: 20px;
            border-radius: 8px;
            text-align: center;
            display: none;
            animation: fadeIn 0.3s;
        }
        .message.error { background: var(--error-bg); color: var(--error-color); display: block; }
        .message.success { background: var(--success-bg); color: var(--success-color); display: block; }

        /* ---------- PAINEL DE ADMIN ---------- */
        .admin-modal-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(5px);
            z-index: 1000;
            display: none;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.3s;
        }
        .admin-modal {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 700px;
            max-height: 90vh;
            overflow-y: auto;
        }
        #adminManagementPanel { display: none; }
        .admin-section { margin-top: 20px; }
        .admin-section h3 {
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 10px;
            margin-bottom: 15px;
        }
        .request-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 10px;
        }
        .request-item:nth-child(odd) { background: var(--light-gray); }
        .request-item .actions button {
            margin-left: 10px;
            padding: 5px 10px;
            border-radius: 5px;
            border: none;
            color: white;
            cursor: pointer;
        }
        .btn-approve { background: #27ae60; }
        .btn-reject { background: #c0392b; }

        /* ---------- MENSAGEM DE BEM-VINDO ---------- */
        .welcome-toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(45deg, var(--primary-color), var(--primary-color-dark));
            color: white;
            padding: 15px 30px;
            border-radius: 50px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            z-index: 2000;
            display: none;
            animation: slideInDown 0.5s, slideOutUp 0.5s 2.5s forwards;
            font-size: 1.1rem;
        }
        @keyframes slideInDown { from { top: -100px; opacity: 0; } to { top: 20px; opacity: 1; } }
        @keyframes slideOutUp { from { top: 20px; opacity: 1; } to { top: -100px; opacity: 0; } }

        /* Responsividade */
        @media (max-width: 900px) {
            .image-section { display: none; }
            .auth-section { max-width: 100%; padding: 30px 20px; }
        }
    </style>
</head>
<body>
    <!-- Mensagem de Boas Vindas -->
    <div id="welcomeToast" class="welcome-toast"></div>

    <!-- Seção da Imagem -->
    <div class="image-section">
        <div class="image-content">
            <h1 id="adminTrigger">IMOB - Sistema de Gestão Imobiliária</h1>
            <p>A plataforma definitiva para gerenciar suas propriedades, clientes e negócios com eficiência e segurança.</p>
        </div>
    </div>
    
    <!-- Seção de Autenticação -->
    <div class="auth-section">
        <!-- Página de Login -->
        <div id="loginPage" class="page active">
            <div class="auth-box">
                <div class="auth-logo">
                    <h2 class="auth-title">Acesse sua Conta</h2>
                </div>
                
                <form id="loginForm" class="auth-form">
                    <div id="loginMessage" class="message"></div>
                    
                    <div class="form-group">
                        <i class="fa-solid fa-envelope"></i>
                        <input type="email" id="loginEmail" required placeholder="seu@email.com">
                    </div>
                    
                    <div class="form-group">
                        <i class="fa-solid fa-lock"></i>
                        <input type="password" id="loginPassword" required placeholder="••••••••">
                        <i class="fa-solid fa-eye password-toggle" onclick="togglePasswordVisibility('loginPassword')"></i>
                    </div>
                    
                    <div class="form-options">
                        <label class="remember-me">
                            <input type="checkbox" id="rememberMe">
                            <span>Lembrar-me</span>
                        </label>
                        <a href="#" class="forgot-password" onclick="requestPasswordReset()">Esqueceu a senha?</a>
                    </div>
                    
                    <button type="submit" class="btn">
                        <span class="btn-text">Entrar</span>
                        <div class="spinner"></div>
                    </button>
                    
                    <div class="auth-footer">
                        Não tem conta? <a href="#" onclick="showPage('registerPage')">Cadastre-se</a>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Página de Cadastro -->
        <div id="registerPage" class="page">
            <div class="auth-box">
                <div class="auth-logo">
                    <h2 class="auth-title">Crie sua Conta</h2>
                </div>
                
                <form id="registerForm" class="auth-form">
                    <div id="registerMessage" class="message"></div>
                    
                    <div class="form-group">
                        <i class="fa-solid fa-user"></i>
                        <input type="text" id="registerName" required placeholder="Nome Completo">
                    </div>
                    
                    <div class="form-group">
                        <i class="fa-solid fa-envelope"></i>
                        <input type="email" id="registerEmail" required placeholder="Seu melhor e-mail">
                    </div>
                                        
                    <div class="form-group">
                        <i class="fa-solid fa-lock"></i>
                        <input type="password" id="registerPassword" required placeholder="Crie uma senha forte">
                        <i class="fa-solid fa-eye password-toggle" onclick="togglePasswordVisibility('registerPassword')"></i>
                    </div>
                    
                    <div class="form-group">
                        <i class="fa-solid fa-lock"></i>
                        <input type="password" id="registerConfirmPassword" required placeholder="Confirme sua senha">
                    </div>
                    
                    <button type="submit" class="btn">
                        <span class="btn-text">Enviar para Aprovação</span>
                        <div class="spinner"></div>
                    </button>
                    
                    <div class="auth-footer">
                        Já tem conta? <a href="#" onclick="showPage('loginPage')">Faça login</a>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Modal de Admin -->
    <div id="adminModal" class="admin-modal-overlay">
        <div class="admin-modal">
            <!-- Login do Admin -->
            <div id="adminLoginPanel">
                <h2 class="auth-title">Acesso Restrito</h2>
                <form id="adminLoginForm" class="auth-form" style="margin-top: 20px;">
                    <div id="adminMessage" class="message"></div>
                    <div class="form-group">
                        <i class="fa-solid fa-user-shield"></i>
                        <input type="text" id="adminUser" required placeholder="Usuário Admin">
                    </div>
                    <div class="form-group">
                        <i class="fa-solid fa-key"></i>
                        <input type="password" id="adminPassword" required placeholder="Senha Admin">
                    </div>
                    <button type="submit" class="btn">Acessar Painel</button>
                    <button type="button" class="btn" style="background: #7f8c8d; margin-top: 10px;" onclick="closeAdminModal()">Fechar</button>
                </form>
            </div>
            <!-- Painel de Gerenciamento -->
            <div id="adminManagementPanel">
                <h2 class="auth-title">Painel do Administrador</h2>
                <div class="admin-section">
                    <h3><i class="fa-solid fa-user-plus"></i> Cadastros Pendentes</h3>
                    <div id="pendingUsersList"></div>
                </div>
                <div class="admin-section">
                    <h3><i class="fa-solid fa-key"></i> Solicitações de Nova Senha</h3>
                    <div id="pendingResetsList"></div>
                </div>
                 <button type="button" class="btn" style="background: var(--primary-color-dark); margin-top: 20px;" onclick="closeAdminModal()">Sair do Painel</button>
            </div>
        </div>
    </div>


    <script>
        // =================================================================
        // SIMULAÇÃO DE BACKEND COM LOCALSTORAGE
        // Em uma aplicação real, estas funções fariam chamadas a uma API.
        // =================================================================
        function getStorage(key) {
            return JSON.parse(localStorage.getItem(key)) || [];
        }

        function setStorage(key, data) {
            localStorage.setItem(key, JSON.stringify(data));
        }

        // Inicializa dados de exemplo se não existirem
        if (!localStorage.getItem('imob_users')) {
            const exampleUser = {
                name: 'Usuário Pendente Exemplo',
                email: 'exemplo@pendente.com',
                password: '123', // Senhas devem ser hasheadas no backend
                status: 'pending'
            };
            setStorage('imob_users', [exampleUser]);
        }
        if (!localStorage.getItem('imob_reset_requests')) {
            const exampleReset = {
                email: 'usuario@esquecido.com',
                date: new Date().toISOString()
            };
            setStorage('imob_reset_requests', [exampleReset]);
        }
        
        // =================================================================
        // FUNÇÕES DE UI E INTERAÇÃO
        // =================================================================
        
        // Função para alternar entre páginas (Login, Cadastro)
        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            document.querySelectorAll('.message').forEach(msg => msg.className = 'message');
        }

        // Função para mostrar/esconder senha
        function togglePasswordVisibility(fieldId) {
            const input = document.getElementById(fieldId);
            const icon = input.nextElementSibling;
            if (input.type === 'password') {
                input.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }
        
        // Função para exibir mensagem
        function showMessage(elementId, text, type = 'error') {
            const messageEl = document.getElementById(elementId);
            messageEl.textContent = text;
            messageEl.className = `message ${type}`;
        }
        
        // Controla o estado de 'loading' dos botões
        function toggleButtonLoading(button, isLoading) {
            button.classList.toggle('loading', isLoading);
            button.disabled = isLoading;
        }

        // =================================================================
        // LÓGICA DOS FORMULÁRIOS
        // =================================================================

        // Formulário de Login
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const button = this.querySelector('.btn');
            toggleButtonLoading(button, true);

            setTimeout(() => { // Simula a latência da rede
                const email = document.getElementById('loginEmail').value;
                const password = document.getElementById('loginPassword').value;
                const users = getStorage('imob_users');
                const user = users.find(u => u.email === email);

                if (!user) {
                    showMessage('loginMessage', 'E-mail não encontrado.');
                } else if (user.status !== 'approved') {
                    showMessage('loginMessage', 'Seu cadastro ainda está pendente de aprovação.');
                } else if (user.password !== password) { // Em produção, compare hashes!
                    showMessage('loginMessage', 'Senha incorreta.');
                } else {
                    showMessage('loginMessage', 'Login bem-sucedido!', 'success');
                    
                    // --- INÍCIO DA MODIFICAÇÃO ---
                    // 1. Cria um token de sessão simples para autenticação.
                    const sessionToken = `imob_auth_token_${Date.now()}`;
                    sessionStorage.setItem('sessionToken', sessionToken);

                    // 2. Salva os dados do usuário para serem usados na página principal.
                    const currentUser = { name: user.name, email: user.email };
                    sessionStorage.setItem('currentUser', JSON.stringify(currentUser));
                    // --- FIM DA MODIFICAÇÃO ---

                    const welcomeToast = document.getElementById('welcomeToast');
                    welcomeToast.textContent = `Bem-vindo(a), ${user.name}! Tenha um ótimo trabalho.`;
                    welcomeToast.style.display = 'block';

                    setTimeout(() => {
                        window.location.href = "índice.html"; // Redireciona para a página correta
                    }, 3000);
                    return; // Evita que o loading seja desativado abaixo
                }
                toggleButtonLoading(button, false);
            }, 1000);
        });

        // Formulário de Cadastro
        document.getElementById('registerForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const button = this.querySelector('.btn');
            toggleButtonLoading(button, true);

            setTimeout(() => {
                const name = document.getElementById('registerName').value;
                const email = document.getElementById('registerEmail').value;
                const password = document.getElementById('registerPassword').value;
                const confirmPassword = document.getElementById('registerConfirmPassword').value;
                
                if (password !== confirmPassword) {
                    showMessage('registerMessage', 'As senhas não coincidem.');
                    toggleButtonLoading(button, false);
                    return;
                }

                const users = getStorage('imob_users');
                if (users.some(u => u.email === email)) {
                    showMessage('registerMessage', 'Este e-mail já foi cadastrado.');
                    toggleButtonLoading(button, false);
                    return;
                }

                const newUser = { name, email, password, status: 'pending' };
                users.push(newUser);
                setStorage('imob_users', users);

                showMessage('registerMessage', 'Cadastro enviado! Aguarde a aprovação do administrador.', 'success');
                this.reset();
                setTimeout(() => showPage('loginPage'), 3000);
                
                toggleButtonLoading(button, false);
            }, 1000);
        });

        // Solicitação de Nova Senha
        function requestPasswordReset() {
            const email = prompt("Por favor, digite seu e-mail para solicitar uma nova senha:");
            if (email) {
                const users = getStorage('imob_users');
                if (!users.some(u => u.email === email)) {
                    alert("E-mail não encontrado em nosso sistema.");
                    return;
                }

                const requests = getStorage('imob_reset_requests');
                if (requests.some(r => r.email === email)) {
                     alert("Você já possui uma solicitação de nova senha pendente.");
                     return;
                }

                requests.push({ email, date: new Date().toISOString() });
                setStorage('imob_reset_requests', requests);
                alert("Sua solicitação foi enviada. Um administrador entrará em contato em breve.");
            }
        }

        // =================================================================
        // LÓGICA DO PAINEL DE ADMIN
        // =================================================================
        const adminModal = document.getElementById('adminModal');
        
        // Gatilho para abrir o painel
        document.getElementById('adminTrigger').addEventListener('dblclick', () => {
            adminModal.style.display = 'flex';
        });

        function closeAdminModal() {
            adminModal.style.display = 'none';
            document.getElementById('adminLoginPanel').style.display = 'block';
            document.getElementById('adminManagementPanel').style.display = 'none';
            document.getElementById('adminLoginForm').reset();
            showMessage('adminMessage', '', ''); // Limpa mensagem
        }

        // Login do Admin
        document.getElementById('adminLoginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const user = document.getElementById('adminUser').value;
            const pass = document.getElementById('adminPassword').value;

            if (user === 'admin' && pass === 'admin') {
                document.getElementById('adminLoginPanel').style.display = 'none';
                document.getElementById('adminManagementPanel').style.display = 'block';
                renderAdminPanel();
            } else {
                showMessage('adminMessage', 'Credenciais de admin incorretas.');
            }
        });
        
        // Renderiza o conteúdo do painel de admin
        function renderAdminPanel() {
            // Renderiza usuários pendentes
            const users = getStorage('imob_users');
            const pendingUsers = users.filter(u => u.status === 'pending');
            const usersList = document.getElementById('pendingUsersList');
            usersList.innerHTML = pendingUsers.length === 0 ? '<p>Nenhum cadastro pendente.</p>' : '';
            pendingUsers.forEach(user => {
                usersList.innerHTML += `
                    <div class="request-item">
                        <span><strong>${user.name}</strong> (${user.email})</span>
                        <div class="actions">
                            <button class="btn-approve" onclick="approveUser('${user.email}')">Aprovar</button>
                            <button class="btn-reject" onclick="rejectUser('${user.email}')">Rejeitar</button>
                        </div>
                    </div>
                `;
            });

            // Renderiza solicitações de senha
            const requests = getStorage('imob_reset_requests');
            const resetsList = document.getElementById('pendingResetsList');
            resetsList.innerHTML = requests.length === 0 ? '<p>Nenhuma solicitação pendente.</p>' : '';
            requests.forEach(req => {
                resetsList.innerHTML += `
                     <div class="request-item">
                        <span>E-mail: <strong>${req.email}</strong></span>
                        <div class="actions">
                            <button class="btn-approve" onclick="approveReset('${req.email}')">Concluir</button>
                        </div>
                    </div>
                `;
            });
        }

        // Funções de ação do Admin
        function approveUser(email) {
            let users = getStorage('imob_users');
            const userIndex = users.findIndex(u => u.email === email);
            if (userIndex > -1) {
                users[userIndex].status = 'approved';
                setStorage('imob_users', users);
                renderAdminPanel(); // Atualiza a UI
                alert(`Usuário ${email} aprovado com sucesso!`);
            }
        }

        function rejectUser(email) {
            let users = getStorage('imob_users');
            users = users.filter(u => u.email !== email);
            setStorage('imob_users', users);
            renderAdminPanel(); // Atualiza a UI
            alert(`Usuário ${email} rejeitado e removido.`);
        }

        function approveReset(email) {
            let requests = getStorage('imob_reset_requests');
            requests = requests.filter(r => r.email !== email);
            setStorage('imob_reset_requests', requests);
            renderAdminPanel(); // Atualiza a UI
            alert(`Solicitação de ${email} concluída. Lembre-se de contatar o usuário com a nova senha.`);
        }

    </script>
</body>
</html>
