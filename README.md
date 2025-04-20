<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Charity Recommender Chatbot</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(to right, #f9f9f9, #e0f7fa);
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    #chat-container {
      width: 450px;
      background-color: #ffffff;
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    }
    #chat-box {
      height: 320px;
      overflow-y: auto;
      border: 1px solid #cfd8dc;
      padding: 12px;
      border-radius: 8px;
      margin-bottom: 12px;
      background-color: #fafafa;
      font-size: 14px;
    }
    .message {
      margin: 8px 0;
      padding: 6px 10px;
      border-radius: 10px;
      display: inline-block;
      max-width: 80%;
      word-wrap: break-word;
    }
    .user {
      background-color: #e1f5fe;
      align-self: flex-end;
      float: right;
      clear: both;
    }
    .bot {
      background-color: #c8e6c9;
      align-self: flex-start;
      float: left;
      clear: both;
    }
    #input-area {
      display: flex;
      align-items: center;
    }
    #user-input {
      flex: 1;
      padding: 10px;
      border: 1px solid #90a4ae;
      border-radius: 6px;
    }
    #send-btn, #mic-btn {
      padding: 10px 14px;
      margin-left: 8px;
      border: none;
      background-color: #00796b;
      color: white;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
    }
    #send-btn:hover, #mic-btn:hover {
      background-color: #004d40;
    }
  </style>
</head>
<body>
  <div id="chat-container">
    <h3>🤖 Charity Recommender</h3>
    <div id="chat-box"></div>
    <div id="input-area">
      <input type="text" id="user-input" placeholder="Ask about charities...">
      <button id="mic-btn">🎤</button>
      <button id="send-btn">Send</button>
    </div>
  </div>

  <script>
    const chatBox = document.getElementById('chat-box');
    const userInput = document.getElementById('user-input');
    const sendBtn = document.getElementById('send-btn');
    const micBtn = document.getElementById('mic-btn');

    const recommendations = {
      "health": "You might consider donating to Partners In Health or Doctors Without Borders.",
      "education": "Consider Room to Read or Khan Academy for educational causes.",
      "climate": "You can support charities like Cool Earth or The Clean Air Task Force.",
      "animals": "World Wildlife Fund and Best Friends Animal Society are great choices.",
      "poverty": "GiveDirectly and Oxfam work effectively in poverty alleviation."
    };

    function addMessage(message, sender) {
      const messageDiv = document.createElement('div');
      messageDiv.className = `message ${sender}`;
      messageDiv.innerText = message;
      chatBox.appendChild(messageDiv);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function getResponse(input) {
      input = input.toLowerCase();
      for (let topic in recommendations) {
        if (input.includes(topic)) {
          return recommendations[topic];
        }
      }
      return "I'm sorry, I couldn't find a charity for that. Try asking about health, education, climate, animals, or poverty.";
    }

    function handleUserInput() {
      const input = userInput.value.trim();
      if (input) {
        addMessage(input, 'user');
        const response = getResponse(input);
        setTimeout(() => addMessage(response, 'bot'), 500);
        userInput.value = '';
      }
    }

    sendBtn.addEventListener('click', handleUserInput);
    userInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') handleUserInput();
    });

    micBtn.addEventListener('click', () => {
      if ('SpeechRecognition' in window || 'webkitSpeechRecognition' in window) {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        recognition.lang = 'en-US';
        recognition.start();

        recognition.onresult = function(event) {
          const voiceInput = event.results[0][0].transcript;
          userInput.value = voiceInput;
          handleUserInput();
        };
      } else {
        alert('Speech recognition is not supported in this browser. Try using Google Chrome.');
      }
    });

    addMessage("Hi! I'm here to help you find charities based on your interests. Ask me about health, education, climate, animals, or poverty.", 'bot');
  </script>
</body>
</html>
