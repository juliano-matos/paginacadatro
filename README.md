<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro Escolar</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background-color: #b3d9ff;
            margin: 0;
            padding: 0;
            color: #333;
        }

        .header {
            background-color: #7a4b8f;
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        }

        .header .nav-buttons {
            display: flex;
            gap: 15px;
        }

        .header button {
            background: linear-gradient(45deg, #7a4b8f, #b3d9ff);
            color: white;
            border: none;
            padding: 12px 16px;
            cursor: pointer;
            border-radius: 5px;
            font-size: 16px;
            transition: background 0.3s ease;
        }

        .header button:hover {
            background: linear-gradient(45deg, #b3d9ff, #7a4b8f);
        }

        .header .title {
            flex-grow: 1;
            text-align: center;
            font-size: 30px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .container {
            max-width: 700px;
            margin: 60px auto;
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 12px 18px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        button:hover {
            background-color: #218838;
        }

        table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
        }

        table, th, td {
            border: 1px solid #ddd;
        }

        th, td {
            padding: 12px;
            text-align: left;
            font-size: 14px;
        }

        th {
            background-color: #f8f9fa;
            font-weight: 500;
        }

        input, select {
            width: 100%;
            margin: 10px 0;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .hidden {
            display: none;
        }

        .error {
            color: #e74c3c;
            font-size: 14px;
        }

        .success {
            color: #2ecc71;
            font-size: 16px;
        }

    </style>
</head>
<body>
    <div class="header">
        <div class="nav-buttons">
            <button onclick="mostrarHome()">Home</button>
            <button onclick="mostrarInfo()">Info</button>
        </div>
        <span class="title">Portal de Cursos</span>
        <div class="nav-buttons">
            <button onclick="login()">Login</button>
            <button onclick="logout()">Logout</button>
        </div>
    </div>

    <div id="home" class="container">
        <h2>Bem-vindo ao portal de cadastro de alunos e professores!</h2>
        <p>Clique em "Home" para acessar a página inicial.</p>
    </div>

    <div id="info" class="container hidden">
        <h2>Escolha uma opção:</h2>
        <button onclick="mostrarSecao('cadastro')">Cadastro</button>
        <button onclick="mostrarSecao('pesquisa')">Pesquisa</button>
    </div>

    <div id="cadastro" class="container hidden">
        <h2>Cadastro Escolar</h2>
        <select id="tipo">
            <option value="Aluno">Aluno</option>
            <option value="Professor">Professor</option>
        </select>
        <input type="text" id="nome" placeholder="Nome" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="password" id="senha" placeholder="Senha" required>
        <input type="text" id="turma" placeholder="Turma">
        <input type="text" id="sala" placeholder="Sala">
        <button onclick="adicionarCadastro()">Cadastrar</button>
        <p id="mensagem" class="hidden success">Cadastro realizado com sucesso! Para verificar, acesse a pesquisa.</p>
        <p id="erro" class="error hidden">Já existe um cadastro com esse nome!</p>
    </div>

    <div id="pesquisa" class="container hidden">
        <h2>Buscar Cadastro</h2>
        <input type="text" id="search" placeholder="Digite um nome..." onkeydown="buscarCadastros(event)">
        <table>
            <thead>
                <tr>
                    <th>Tipo</th>
                    <th>Nome</th>
                    <th>Email</th>
                    <th>Turma</th>
                    <th>Sala</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody id="tabela-cadastros"></tbody>
        </table>
    </div>

    <script>
        let usuarioLogado = null;

        function mostrarHome() {
            document.getElementById("home").classList.remove("hidden");
            document.getElementById("info").classList.add("hidden");
            document.getElementById("cadastro").classList.add("hidden");
            document.getElementById("pesquisa").classList.add("hidden");
            window.scrollTo(0, 0);
        }

        function mostrarInfo() {
            document.getElementById("home").classList.add("hidden");
            document.getElementById("info").classList.remove("hidden");
            document.getElementById("cadastro").classList.add("hidden");
            document.getElementById("pesquisa").classList.add("hidden");
            window.scrollTo(0, 0);
        }

        function mostrarSecao(secao) {
            document.getElementById("home").classList.add("hidden");
            document.getElementById("info").classList.add("hidden");
            document.getElementById("cadastro").classList.add("hidden");
            document.getElementById("pesquisa").classList.add("hidden");
            document.getElementById(secao).classList.remove("hidden");
            window.scrollTo(0, 0);
        }

        function adicionarCadastro() {
            const tipo = document.getElementById("tipo").value;
            const nome = document.getElementById("nome").value;
            const email = document.getElementById("email").value;
            const senha = document.getElementById("senha").value;
            const turma = document.getElementById("turma").value;
            const sala = document.getElementById("sala").value;

            let cadastros = JSON.parse(localStorage.getItem("cadastros")) || [];
            const cadastroExistente = cadastros.find(cadastro => cadastro.nome.toLowerCase() === nome.toLowerCase());

            if (cadastroExistente) {
                document.getElementById("erro").classList.remove("hidden");
                return;
            } else {
                document.getElementById("erro").classList.add("hidden");
            }

            const cadastro = { tipo, nome, email, senha, turma, sala };
            cadastros.push(cadastro);
            localStorage.setItem("cadastros", JSON.stringify(cadastros));

            document.getElementById("nome").value = "";
            document.getElementById("email").value = "";
            document.getElementById("senha").value = "";
            document.getElementById("turma").value = "";
            document.getElementById("sala").value = "";

            let mensagem = document.getElementById("mensagem");
            mensagem.classList.remove("hidden");
            setTimeout(() => {
                mensagem.classList.add("hidden");
            }, 3000);
        }

        function buscarCadastros(event) {
            if (event.key !== "Enter") return;

            const searchTerm = document.getElementById("search").value.toLowerCase();
            const cadastros = JSON.parse(localStorage.getItem("cadastros")) || [];
            const tabela = document.getElementById("tabela-cadastros");
            tabela.innerHTML = "";

            cadastros.filter(cadastro => cadastro.nome.toLowerCase().includes(searchTerm))
                .forEach(cadastro => {
                    let row = tabela.insertRow();
                    row.innerHTML = `
                        <td>${cadastro.tipo}</td>
                        <td>${cadastro.nome}</td>
                        <td>${cadastro.email}</td>
                        <td>${cadastro.turma}</td>
                        <td>${cadastro.sala}</td>
                        <td>
                            ${usuarioLogado ? 
                            `<button onclick="editarCadastro('${cadastro.nome}')">Editar</button>
                             <button onclick="removerCadastro('${cadastro.nome}')">Remover</button>` : 
                            ""} 
                        </td>
                    `;
                });
        }

        function editarCadastro(nome) {
            if (!usuarioLogado) {
                alert("Você precisa estar logado para editar o cadastro.");
                return;
            }

            let cadastros = JSON.parse(localStorage.getItem("cadastros")) || [];
            const cadastro = cadastros.find(cadastro => cadastro.nome === nome);

            if (cadastro) {
                document.getElementById("tipo").value = cadastro.tipo;
                document.getElementById("nome").value = cadastro.nome;
                document.getElementById("email").value = cadastro.email;
                document.getElementById("senha").value = cadastro.senha;
                document.getElementById("turma").value = cadastro.turma;
                document.getElementById("sala").value = cadastro.sala;

                cadastros = cadastros.filter(c => c.nome !== nome);
                localStorage.setItem("cadastros", JSON.stringify(cadastros));
            }
        }

        function removerCadastro(nome) {
            if (!usuarioLogado) {
                alert("Você precisa estar logado para remover o cadastro.");
                return;
            }

            let cadastros = JSON.parse(localStorage.getItem("cadastros")) || [];
            cadastros = cadastros.filter(cadastro => cadastro.nome !== nome);
            localStorage.setItem("cadastros", JSON.stringify(cadastros));

            alert("Cadastro removido com sucesso!");
            buscarCadastros(new KeyboardEvent("keydown", { key: "Enter" }));
        }

        function login() {
            const email = prompt("Digite seu email:");
            const senha = prompt("Digite sua senha:");

            let cadastros = JSON.parse(localStorage.getItem("cadastros")) || [];
            const cadastro = cadastros.find(c => c.email.toLowerCase() === email.toLowerCase() && c.senha === senha);

            if (cadastro) {
                usuarioLogado = cadastro;
                alert(`Login realizado com sucesso! Bem-vindo, ${cadastro.nome}!`);
            } else {
                alert("Email ou senha incorretos. Por favor, tente novamente.");
            }
        }

        function logout() {
            usuarioLogado = null;  // Apenas remove o usuário logado, não afeta os cadastros
            alert("Logout realizado!");
            mostrarHome(); // Volta para a página inicial
        }
    </script>
</body>
</html>
