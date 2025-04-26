<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot Groq</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chatbox {
            width: 400px;
            height: 500px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            padding: 10px;
        }
        .messages {
            flex: 1;
            overflow-y: scroll;
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
        .input-area {
            display: flex;
        }
        .input-area input {
            width: 80%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .input-area button {
            width: 20%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="chatbox">
        <div class="messages" id="messages"></div>
        <div class="input-area">
            <input type="text" id="userInput" placeholder="Digite sua mensagem...">
            <button onclick="sendMessage()">Enviar</button>
        </div>
    </div>

    <script>
        const apiKey = "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"; // Sua chave da API Groq
        const apiUrl = "https://api.groq.com/v1/chat"; // Substitua pela URL correta da API Groq

        function sendMessage() {
            const userInput = document.getElementById('userInput').value;
            if (userInput.trim() === '') return;

            // Exibe a mensagem do usuário no chat
            addMessage('Você: ' + userInput);

            // Chama a API do Groq
            fetch(apiUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + apiKey // Autenticação com a chave da API
                },
                body: JSON.stringify({ query: userInput })
            })
            .then(response => response.json())
            .then(data => {
                const botResponse = data.response || "Desculpe, não entendi sua pergunta.";
                addMessage('Groq: ' + botResponse);
            })
            .catch(error => {
                addMessage('Erro: ' + error.message);
            });

            document.getElementById('userInput').value = ''; // Limpa a caixa de entrada
        }

        function addMessage(message) {
            const messagesDiv = document.getElementById('messages');
            const messageDiv = document.createElement('div');
            messageDiv.textContent = message;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight; // Rola para a última mensagem
        }
    </script>
</body>
</html>
