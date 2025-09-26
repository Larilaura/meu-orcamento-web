<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Organizador de Orçamentos</title>
  <style>
    body { font-family: Arial; margin: 20px; }
    input, select { margin: 5px; padding: 5px; }
    table { border-collapse: collapse; width: 100%; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: left; }
    th { background-color: #f4f4f4; }
    .finalizado { background-color: #d4fcd4; }
    button.acao { margin-right: 5px; }
  </style>
</head>
<body>
  <h2>Organizador de Orçamentos</h2>

  <label>Nome do Cliente:</label>
  <input type="text" id="cliente"><br>

  <label>Número do Orçamento:</label>
  <input type="text" id="numeroOrcamento"><br>

  <label>Responsável:</label>
  <input type="text" id="responsavel"><br>

  <label>Status:</label>
  <select id="status">
    <option value="Em andamento">Em andamento</option>
    <option value="Finalizado">Finalizado</option>
  </select><br>

  <button onclick="adicionarOrcamento()">Adicionar Orçamento</button>

  <table id="tabelaOrcamentos">
    <thead>
      <tr>
        <th>Cliente</th>
        <th>Nº Orçamento</th>
        <th>Responsável</th>
        <th>Status</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    let editandoLinha = null;

    function adicionarOrcamento() {
      const cliente = document.getElementById("cliente").value;
      const numero = document.getElementById("numeroOrcamento").value;
      const responsavel = document.getElementById("responsavel").value;
      const status = document.getElementById("status").value;

      if (!cliente || !numero || !responsavel) {
        alert("Preencha todos os campos!");
        return;
      }

      if (editandoLinha) {
        // Modo edição: atualiza todos os campos, inclusive o número do orçamento
        atualizarLinha(editandoLinha, cliente, numero, responsavel, status);
        editandoLinha = null;
        document.querySelector('button[onclick="adicionarOrcamento()"]').textContent = "Adicionar Orçamento";
      } else {
        // Novo orçamento
        inserirLinhaTabela(cliente, numero, responsavel, status);
      }

      // Limpa campos
      document.getElementById("cliente").value = "";
      document.getElementById("numeroOrcamento").value = "";
      document.getElementById("responsavel").value = "";
      document.getElementById("status").value = "Em andamento";
    }

    function inserirLinhaTabela(cliente, numero, responsavel, status) {
      const tabela = document.getElementById("tabelaOrcamentos").getElementsByTagName("tbody")[0];
      const novaLinha = tabela.insertRow();

      if (status === "Finalizado") {
        novaLinha.classList.add("finalizado");
      }

      novaLinha.insertCell(0).textContent = cliente;
      novaLinha.insertCell(1).textContent = numero;
      novaLinha.insertCell(2).textContent = responsavel;
      novaLinha.insertCell(3).textContent = status;

      // Célula de ações
      const celulaAcoes = novaLinha.insertCell(4);
      const btnEditar = document.createElement("button");
      btnEditar.textContent = "Editar";
      btnEditar.className = "acao";
      btnEditar.onclick = function () { editarLinha(novaLinha); };

      const btnRemover = document.createElement("button");
      btnRemover.textContent = "Remover";
      btnRemover.className = "acao";
      btnRemover.onclick = function () { removerLinha(novaLinha); };

      celulaAcoes.appendChild(btnEditar);
      celulaAcoes.appendChild(btnRemover);
    }

    function editarLinha(linha) {
      // Preenche os campos com os valores da linha, inclusive o número do orçamento
      document.getElementById("cliente").value = linha.cells[0].textContent;
      document.getElementById("numeroOrcamento").value = linha.cells[1].textContent;
      document.getElementById("responsavel").value = linha.cells[2].textContent;
      document.getElementById("status").value = linha.cells[3].textContent;

      editandoLinha = linha;
      document.querySelector('button[onclick="adicionarOrcamento()"]').textContent = "Salvar Alterações";
    }

    function atualizarLinha(linha, cliente, numero, responsavel, status) {
      linha.cells[0].textContent = cliente;
      linha.cells[1].textContent = numero; // Agora atualiza o número do orçamento também!
      linha.cells[2].textContent = responsavel;
      linha.cells[3].textContent = status;

      if (status === "Finalizado") {
        linha.classList.add("finalizado");
      } else {
        linha.classList.remove("finalizado");
      }
    }

    function removerLinha(linha) {
      if (confirm("Tem certeza que deseja remover este orçamento?")) {
        linha.remove();
        // Se estava editando esta linha, limpa os campos
        if (linha === editandoLinha) {
          editandoLinha = null;
          document.getElementById("cliente").value = "";
          document.getElementById("numeroOrcamento").value = "";
          document.getElementById("responsavel").value = "";
          document.getElementById("status").value = "Em andamento";
          document.querySelector('button[onclick="adicionarOrcamento()"]').textContent = "Adicionar Orçamento";
        }
      }
    }
  </script>
</body>
</html>
