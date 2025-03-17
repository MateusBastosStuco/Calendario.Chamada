<!DOCTYPE html>
<html lang="pt-br">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciamento de Alunos - Sistema de Chamada</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <style>
        body {
            background: linear-gradient(135deg, #4e54c8, #8f94fb);
            color: #333;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Arial', sans-serif;
            padding: 20px;
        }

        .container-admin {
            background: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 1200px;
        }

        .btn-remove,
        .btn-add-calendar,
        .btn-presenca,
        .btn-falta,
        .btn-justificado {
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 12px;
        }

        .btn-remove {
            background: red;
            color: white;
        }

        .btn-presenca {
            background: green;
            color: white;
        }

        .btn-falta {
            background: red;
            color: white;
        }

        .btn-justificado {
            background: #f7c500;
            color: white;
        }

        .aluno-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 10px;
            padding: 10px;
            background: white;
            border-radius: 5px;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
        }

        .btn-clear-all {
            background-color: #dc3545;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 20px;
            display: block;
            width: 100%;
        }

        .status-container {
            display: flex;
            justify-content: flex-start;
            align-items: center;
        }

        .status-container .btn-presenca,
        .status-container .btn-falta,
        .status-container .btn-justificado {
            margin-left: 10px;
        }

        .justificativa-falta {
            margin-top: 5px;
            display: none;
        }

        .justificativa-falta textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
        }

        .justificativa-falta button {
            margin-top: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
        }

        .alert-message {
            margin-top: 10px;
            padding: 10px;
            border-radius: 5px;
            display: none;
        }

        .alert-success {
            background-color: #28a745;
            color: white;
        }

        .alert-error {
            background-color: #dc3545;
            color: white;
        }

        .historico-container {
            margin-top: 10px;
        }

        .historico-item {
            margin-bottom: 5px;
            background-color: #f8f9fa;
            padding: 5px;
            border-radius: 5px;
        }

        .filter-buttons {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }

        .filter-buttons button {
            flex: 1;
            margin: 0 5px;
            padding: 10px;
            font-size: 14px;
        }

        .filter-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }

        .filter-container select {
            margin: 5px;
            padding: 8px;
            width: 200px;
        }
    </style>
</head>

<body>

    <div class="container-admin">
        <h2>Gerenciar Alunos - Sistema de Chamada</h2>

        <div class="alert-message" id="alertMessage"></div>

        <!-- Filtros -->
        <div class="filter-container">
            <input type="text" id="filtroNome" class="form-control" placeholder="Filtrar por nome do aluno" onkeyup="aplicarFiltros()">
            <select id="filtroOficina" class="form-control" onchange="aplicarFiltros()">
                <option value="">Filtrar por Oficina</option>
                <option value="Street Dance">Street Dance</option>
                <option value="Futebol">Futebol</option>
                <option value="Música">Música</option>
                <option value="Ludoteca">Ludoteca</option>
                <option value="Contação de Histórias">Contação de Histórias</option>
                <option value="Ballet">Ballet</option>
                <option value="Vôlei">Vôlei</option>
                <option value="Teatro">Teatro</option>
                <option value="Programa Eureka">Programa Eureka</option>
                <option value="Sucatoteca">Sucatoteca</option>
                <option value="Adolescer">Adolescer</option>
            </select>
            <select id="filtroDiaSemana" class="form-control" onchange="aplicarFiltros()">
                <option value="">Filtrar por Dia da Semana</option>
                <option value="segunda">Segunda-feira</option>
                <option value="terca">Terça-feira</option>
                <option value="quarta">Quarta-feira</option>
                <option value="quinta">Quinta-feira</option>
                <option value="sexta">Sexta-feira</option>
            </select>
        </div>

        <div class="mb-3">
            <input type="text" id="nomeAluno" class="form-control" placeholder="Nome do Aluno">

            <!-- Oficina 1 -->
            <h5>Selecione a Oficina 1:</h5>
            <select id="oficina1" class="form-control mt-2">
                <option value="Street Dance">Street Dance</option>
                <option value="Futebol">Futebol</option>
                <option value="Música">Música</option>
                <option value="Ludoteca">Ludoteca</option>
                <option value="Contação de Histórias">Contação de Histórias</option>
                <option value="Ballet">Ballet</option>
                <option value="Vôlei">Vôlei</option>
                <option value="Teatro">Teatro</option>
                <option value="Programa Eureka">Programa Eureka</option>
                <option value="Sucatoteca">Sucatoteca</option>
                <option value="Adolescer">Adolescer</option>
            </select>
            <input type="text" id="nomeProfessor1" class="form-control mt-2" placeholder="Nome do Professor da Oficina 1">
            <select id="periodo1" class="form-control mt-2">
                <option value="Manha 1">Manhã 1</option>
                <option value="Manha 2">Manhã 2</option>
                <option value="Tarde 1">Tarde 1</option>
                <option value="Tarde 2">Tarde 2</option>
            </select>
            <select id="diaSemana1" class="form-control mt-2">
                <option value="segunda">Segunda-feira</option>
                <option value="terca">Terça-feira</option>
                <option value="quarta">Quarta-feira</option>
                <option value="quinta">Quinta-feira</option>
                <option value="sexta">Sexta-feira</option>
            </select>

            <!-- Oficina 2 -->
            <h5 class="mt-4">Selecione a Oficina 2:</h5>
            <select id="oficina2" class="form-control mt-2">
                <option value="Street Dance">Street Dance</option>
                <option value="Futebol">Futebol</option>
                <option value="Música">Música</option>
                <option value="Ludoteca">Ludoteca</option>
                <option value="Contação de Histórias">Contação de Histórias</option>
                <option value="Ballet">Ballet</option>
                <option value="Vôlei">Vôlei</option>
                <option value="Teatro">Teatro</option>
                <option value="Programa Eureka">Programa Eureka</option>
                <option value="Sucatoteca">Sucatoteca</option>
                <option value="Adolescer">Adolescer</option>
            </select>
            <input type="text" id="nomeProfessor2" class="form-control mt-2" placeholder="Nome do Professor da Oficina 2">
            <select id="periodo2" class="form-control mt-2">
                <option value="Manha 1">Manhã 1</option>
                <option value="Manha 2">Manhã 2</option>
                <option value="Tarde 1">Tarde 1</option>
                <option value="Tarde 2">Tarde 2</option>
            </select>
            <select id="diaSemana2" class="form-control mt-2">
                <option value="segunda">Segunda-feira</option>
                <option value="terca">Terça-feira</option>
                <option value="quarta">Quarta-feira</option>
                <option value="quinta">Quinta-feira</option>
                <option value="sexta">Sexta-feira</option>
            </select>
        </div>

        <button class="btn btn-primary w-100" onclick="adicionarAluno()">Adicionar Aluno</button>

        <!-- Botão para limpar campos -->
        <button class="btn btn-secondary w-100 mt-4" onclick="limparCampos()">Limpar Campos</button>

        <h4 class="mt-4">Lista de Alunos</h4>
        <div id="listaAlunos"></div>

        <button class="btn-clear-all" onclick="limparTodosDados()">Limpar Todos os Alunos</button>
        <button class="btn btn-success w-100 mt-4" onclick="gerarPdf()">Gerar PDF</button>
    </div>

    <script>
        // Array de alunos
        let alunos = [];
        let alunosFiltrados = [];

        function adicionarAluno() {
            const nome = document.getElementById('nomeAluno').value;
            const oficina1 = document.getElementById('oficina1').value;
            const nomeProfessor1 = document.getElementById('nomeProfessor1').value;
            const periodo1 = document.getElementById('periodo1').value;
            const diaSemana1 = document.getElementById('diaSemana1').value;
            const oficina2 = document.getElementById('oficina2').value;
            const nomeProfessor2 = document.getElementById('nomeProfessor2').value;
            const periodo2 = document.getElementById('periodo2').value;
            const diaSemana2 = document.getElementById('diaSemana2').value;

            if (!nome || !oficina1 || !nomeProfessor1 || !periodo1 || !diaSemana1 || !oficina2 || !nomeProfessor2 || !periodo2 || !diaSemana2) {
                showAlertMessage("Preencha todos os campos!", "error");
                return;
            }

            const aluno = {
                nome,
                oficina1,
                nomeProfessor1,
                periodo1,
                diaSemana1,
                oficina2,
                nomeProfessor2,
                periodo2,
                diaSemana2,
                status: [],
                justificativas: []
            };

            alunos.push(aluno);
            alunosFiltrados.push(aluno);

            showAlertMessage("Aluno adicionado com sucesso!", "success");
            renderizarListaAlunos();
            limparCampos();
        }

        function aplicarFiltros() {
            const nomeFiltro = document.getElementById('filtroNome').value.toLowerCase();
            const oficinaFiltro = document.getElementById('filtroOficina').value;
            const diaSemanaFiltro = document.getElementById('filtroDiaSemana').value;

            alunosFiltrados = alunos.filter(aluno => {
                const nomeMatch = aluno.nome.toLowerCase().includes(nomeFiltro);
                const oficinaMatch = oficinaFiltro === "" || aluno.oficina1 === oficinaFiltro || aluno.oficina2 === oficinaFiltro;
                const diaSemanaMatch = diaSemanaFiltro === "" || aluno.diaSemana1 === diaSemanaFiltro || aluno.diaSemana2 === diaSemanaFiltro;
                return nomeMatch && oficinaMatch && diaSemanaMatch;
            });

            renderizarListaAlunos();
        }

        function renderizarListaAlunos() {
            const listaAlunosDiv = document.getElementById("listaAlunos");
            listaAlunosDiv.innerHTML = "";

            if (alunosFiltrados.length === 0) {
                listaAlunosDiv.innerHTML = "<p>Nenhum aluno encontrado com os filtros aplicados.</p>";
                return;
            }

            alunosFiltrados.forEach((aluno, index) => {
                const alunoDiv = document.createElement("div");
                alunoDiv.classList.add("aluno-item");

                alunoDiv.innerHTML = `
                    <span>${aluno.nome}</span>
                    <div class="status-container">
                        <button class="btn-presenca" onclick="marcarPresenca(${index})">Presença</button>
                        <button class="btn-falta" onclick="marcarFalta(${index})">Falta</button>
                        <button class="btn-justificado" onclick="marcarJustificativa(${index})">Justificativa</button>
                    </div>
                `;

                listaAlunosDiv.appendChild(alunoDiv);
            });
        }

        function limparCampos() {
            document.getElementById('nomeAluno').value = '';
            document.getElementById('oficina1').value = 'Street Dance';
            document.getElementById('nomeProfessor1').value = '';
            document.getElementById('periodo1').value = 'Manha 1';
            document.getElementById('diaSemana1').value = 'segunda';
            document.getElementById('oficina2').value = 'Street Dance';
            document.getElementById('nomeProfessor2').value = '';
            document.getElementById('periodo2').value = 'Manha 1';
            document.getElementById('diaSemana2').value = 'segunda';
        }

        function limparTodosDados() {
            alunos = [];
            alunosFiltrados = [];
            renderizarListaAlunos();
        }

        function showAlertMessage(message, type) {
            const alertMessage = document.getElementById('alertMessage');
            alertMessage.textContent = message;
            alertMessage.classList.remove('alert-success', 'alert-error');
            alertMessage.classList.add(type === 'success' ? 'alert-success' : 'alert-error');
            alertMessage.style.display = 'block';
        }

        function gerarPdf() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            doc.text('Relatório de Alunos', 10, 10);
            alunosFiltrados.forEach((aluno, index) => {
                doc.text(`${index + 1}. ${aluno.nome}`, 10, 20 + (index * 10));
            });

            doc.save('relatorio_alunos.pdf');
        }

    </script>

</body>

</html>
