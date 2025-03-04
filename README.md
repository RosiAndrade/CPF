# CPF
Validador cpf
//Rosilaine
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário com Validação</title>
    <style>
        .error {
            color: red;
            font-weight: bold;
        }
        .result {
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <h2>Formulário de Produto</h2>
    <form id="formProduto">
        <label for="nomeProduto">Nome do Produto:</label>
        <input type="text" id="nomeProduto" name="nomeProduto"><br><br>

        <label for="quantidade">Quantidade:</label>
        <input type="number" id="quantidade" name="quantidade"><br><br>

        <label for="valor">Valor (R$):</label>
        <input type="number" id="valor" name="valor" step="0.01"><br><br>

        <button type="submit">Enviar</button>
    </form>

    <div id="error" class="error"></div>

    <div id="resultado" class="result"></div>

    <script>
        // Função para validar o formulário e exibir os resultados
        document.getElementById('formProduto').addEventListener('submit', function(event) {
            event.preventDefault(); // Evita o envio do formulário

            // Limpa mensagens de erro
            document.getElementById('error').textContent = '';
            document.getElementById('resultado').textContent = '';

            // Pega os valores dos campos
            const nomeProduto = document.getElementById('nomeProduto').value;
            const quantidade = document.getElementById('quantidade').value;
            const valor = document.getElementById('valor').value;

            // Validação para garantir que o nome do produto não esteja vazio
            if (!nomeProduto.trim()) {
                document.getElementById('error').textContent = 'O nome do produto é obrigatório!';
                return;
            }

            // Criação do objeto de produto
            const produto = {
                nome: nomeProduto,
                quantidade: parseInt(quantidade, 10),
                valor: parseFloat(valor)
            };

            // Exibindo os dados do produto
            const total = produto.quantidade * produto.valor;
            document.getElementById('resultado').innerHTML = `
                <h3>Detalhes do Produto</h3>
                <p><strong>Nome:</strong> ${produto.nome}</p>
                <p><strong>Quantidade:</strong> ${produto.quantidade}</p>
                <p><strong>Valor unitário:</strong> R$ ${produto.valor.toFixed(2)}</p>
                <p><strong>Total:</strong> R$ ${total.toFixed(2)}</p>
            `;
        });
    </script>

</body>
</html>


<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Validação de CPF</title>
    <style>
        .error {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h2>Validação de CPF</h2>
    <form id="formCpf">
        <label for="cpf">Digite o CPF (somente números):</label>
        <input type="text" id="cpf" name="cpf" maxlength="11">
        <button type="submit">Validar</button>
    </form>

    <div id="error" class="error"></div>
    <div id="resultado"></div>

    <script>
        // Função para validar o CPF
        function validarCPF(cpf) {
            // Remove caracteres não numéricos
            cpf = cpf.replace(/[^\d]+/g, '');

            // Verifica se o CPF tem 11 caracteres
            if (cpf.length !== 11) {
                return false;
            }

            // Verifica se todos os dígitos são iguais (ex: 111.111.111-11)
            if (/^(\d)\1{10}$/.test(cpf)) {
                return false;
            }

            // Validação do primeiro dígito verificador
            let soma = 0;
            let multiplicador = 10;
            for (let i = 0; i < 9; i++) {
                soma += parseInt(cpf.charAt(i)) * multiplicador--;
            }
            let resto = soma % 11;
            let digito1 = (resto < 2) ? 0 : 11 - resto;

            // Validação do segundo dígito verificador
            soma = 0;
            multiplicador = 11;
            for (let i = 0; i < 10; i++) {
                soma += parseInt(cpf.charAt(i)) * multiplicador--;
            }
            resto = soma % 11;
            let digito2 = (resto < 2) ? 0 : 11 - resto;

            // Verifica se os dígitos verificadores calculados são iguais aos fornecidos
            if (parseInt(cpf.charAt(9)) === digito1 && parseInt(cpf.charAt(10)) === digito2) {
                return true;
            }

            return false;
        }

        // Função para manipular o envio do formulário
        document.getElementById('formCpf').addEventListener('submit', function(event) {
            event.preventDefault(); // Evita o envio do formulário

            const cpf = document.getElementById('cpf').value;
            const errorElement = document.getElementById('error');
            const resultadoElement = document.getElementById('resultado');

            // Limpa as mensagens anteriores
            errorElement.textContent = '';
            resultadoElement.textContent = '';

            // Valida o CPF
            if (validarCPF(cpf)) {
                resultadoElement.textContent = 'CPF válido!';
                resultadoElement.style.color = 'green';
            } else {
                errorElement.textContent = 'CPF inválido! Verifique e tente novamente.';
            }
        });
    </script>

</body>
</html>
