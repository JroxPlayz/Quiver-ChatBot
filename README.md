<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiver - Chatbot</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      padding: 20px;
    }
    .chat-container {
      max-width: 600px;
      margin: auto;
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .chat-box {
      height: 400px;
      overflow-y: scroll;
      border-bottom: 2px solid #ddd;
      padding-right: 10px;
    }
    .message {
      display: flex;
      margin-bottom: 10px;
      padding: 5px;
      border-radius: 10px;
    }
    .bot-message {
      background-color: #f1f1f1;
      color: #333;
      margin-left: auto;
      padding: 10px;
      border-radius: 10px;
      max-width: 80%;
    }
    .user-message {
      background-color: #0084ff;
      color: white;
      margin-right: auto;
      padding: 10px;
      border-radius: 10px;
      max-width: 80%;
    }
    .input-container {
      display: flex;
      justify-content: space-between;
      margin-top: 10px;
    }
    input[type="text"] {
      width: 80%;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      padding: 10px 20px;
      background-color: #0084ff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0073e6;
    }
    .typing-indicator {
      color: #0073e6;
      font-style: italic;
    }
    .emoji-button {
      font-size: 24px;
      cursor: pointer;
    }
    .quick-replies {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      margin-top: 10px;
    }
    .quick-reply {
      padding: 10px;
      background-color: #f1f1f1;
      border-radius: 20px;
      cursor: pointer;
    }
    .quick-reply:hover {
      background-color: #ddd;
    }
  </style>
</head>
<body>

  <div class="chat-container">
    <div class="chat-box" id="chatBox"></div>
    <div class="input-container">
      <input type="text" id="userInput" placeholder="Type a message..." />
      <button onclick="sendMessage()">Send</button>
    </div>
    <div class="quick-replies" id="quickReplies"></div>
  </div>

  <script>
    // Basic chatbot data and settings
    let userMessages = [];
    let botMessages = [];

    const chatBox = document.getElementById('chatBox');
    const userInput = document.getElementById('userInput');
    const quickReplies = document.getElementById('quickReplies');

    // Simulate the bot typing indicator
    function showTypingIndicator() {
      const typingIndicator = document.createElement('div');
      typingIndicator.classList.add('typing-indicator');
      typingIndicator.textContent = 'Bot is typing...';
      chatBox.appendChild(typingIndicator);
    }

    // Remove typing indicator
    function removeTypingIndicator() {
      const typingIndicator = document.querySelector('.typing-indicator');
      if (typingIndicator) {
        typingIndicator.remove();
      }
    }

    // Function to display messages
    function displayMessages() {
      chatBox.innerHTML = '';
      userMessages.forEach((msg, index) => {
        const userDiv = document.createElement('div');
        userDiv.classList.add('message');
        userDiv.innerHTML = `<div class="user-message">${msg}</div>`;
        chatBox.appendChild(userDiv);
        
        const botDiv = document.createElement('div');
        botDiv.classList.add('message');
        botDiv.innerHTML = `<div class="bot-message">${botMessages[index]}</div>`;
        chatBox.appendChild(botDiv);
      });
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    // Bot response logic
    function getBotResponse(userText) {
      let response = '';
      
      // Basic command checks
      if (userText.includes("hello")) {
        response = "Hi there! How can I assist you today?";
      } else if (userText.includes("weather")) {
        response = "Sure! Let me get the weather for you.";
      } else if (userText.includes("joke")) {
        response = "Why don't skeletons fight each other? They don't have the guts!";
      } else if (userText.includes("goodbye")) {
        response = "Goodbye! Hope to talk again soon!";
      } else {
        response = "Sorry, I don't understand that.";
      }

      return response;
    }

    // Send message
    function sendMessage() {
      const message = userInput.value.trim();
      if (!message) return;

      // Display user message
      userMessages.push(message);
      userInput.value = '';
      
      // Show bot typing indicator
      showTypingIndicator();
      
      // Get and display bot response after a delay
      setTimeout(() => {
        const botResponse = getBotResponse(message);
        botMessages.push(botResponse);
        displayMessages();
        removeTypingIndicator();
      }, 1500);
    }

    // Quick replies
    function displayQuickReplies() {
      const replies = ["Hi", "What's the weather?", "Tell me a joke", "Goodbye"];
      quickReplies.innerHTML = '';
      replies.forEach(reply => {
        const button = document.createElement('div');
        button.classList.add('quick-reply');
        button.textContent = reply;
        button.onclick = () => {
          userInput.value = reply;
          sendMessage();
        };
        quickReplies.appendChild(button);
      });
    }

    // Add emojis
    function addEmoji(emoji) {
      userInput.value += emoji;
    }

    // Emoji button functionality
    const emojiButton = document.createElement('button');
    emojiButton.classList.add('emoji-button');
    emojiButton.textContent = '😊';
    emojiButton.onclick = () => addEmoji('😊');
    document.body.appendChild(emojiButton);

    // Call displayQuickReplies on page load
    window.onload = displayQuickReplies;
  </script>

</body>
</html>
