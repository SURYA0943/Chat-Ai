# Chat-Ai
site link to open:- https://surya0943.github.io/Chat-Ai/

Before using it create your API_KEY and past it in the js code in the 1st line (const API_KEY = 'API_KEY';)
so that you can get access to the server.

==>How to create API_KEY:-         
1.Go to https://ai.google.dev/gemini-api/docs/api-key.     
2.Click on "Get a Gemini API key in Google AI Studio".     
3.Click on Create API KEY.      
4.Now copy your API_KEY and past it in the 1st line of the Javascript code      

==>Code Explanation<==

1. API Configuration
      const API_KEY = 'YOUR_GEMINI_API_KEY';
      const API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent';
      Stores the API key (used to authenticate requests).
      Defines the API endpoint to send user messages and get AI responses.
2. Getting HTML Elements
    
      const chatMessages = document.getElementById('chat-messages');
      const userInput = document.getElementById('user-input');
      const sendButton = document.getElementById('send-button');
      Gets references to the chat display, input box, and send button.
3. Sending User Input to the API

      async function generateResponse(prompt) {
            const response = await fetch(`${API_URL}?key=${API_KEY}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                contents: [{ parts: [{ text: prompt }] }]
                })
            });

   if (!response.ok) throw new Error('Failed to generate response');
   
   const data = await response.json();
    return data.candidates[0].content.parts[0].text;
}

explanation:-
Sends a POST request to the Gemini API with the user’s message (prompt).
Receives a response and extracts the AI-generated text.
Handles errors if the API request fails.


5. Cleaning Markdown Formatting

      function cleanMarkdown(text) {
          return text.replace(/#{1,6}\s?/g, '') // Removes Markdown headers (#, ##, ###)
                     .replace(/\*\*/g, '') // Removes bold text (**bold**)
                     .replace(/\n{3,}/g, '\n\n') // Limits extra newlines
                     .trim(); // Removes extra spaces
      }

explanation:-
Cleans up Markdown formatting from the AI response before displaying it.


6. Displaying Messages in the Chat

    function addMessage(message, isUser) {
          const messageElement = document.createElement('div');
          messageElement.classList.add('message', isUser ? 'user-message' : 'bot-message');

      const profileImage = document.createElement('img');
      profileImage.classList.add('profile-image');
      profileImage.src = isUser ? 'user.png' : 'bot.png';
      profileImage.alt = isUser ? 'User' : 'Bot';

      const messageContent = document.createElement('div');
      messageContent.classList.add('message-content');
      messageContent.textContent = message;

      messageElement.appendChild(profileImage);
      messageElement.appendChild(messageContent);
      chatMessages.appendChild(messageElement);
      chatMessages.scrollTop = chatMessages.scrollHeight; // Auto-scroll to latest message

explanation:-
  Creates a message box with text + profile image.
  Adds the message to the chat window.
  Auto-scrolls to keep the latest message visible.

  
6. Handling User Input

async function handleUserInput() {
          const userMessage = userInput.value.trim();
          if (userMessage) {
          addMessage(userMessage, true); // Display user's message
          userInput.value = ''; // Clear input field

   sendButton.disabled = true;
        userInput.disabled = true; // Prevent multiple messages while waiting for AI

   try {
            const botMessage = await generateResponse(userMessage); // Get AI response
            addMessage(cleanMarkdown(botMessage), false); // Display AI's message
        } catch (error) {
            console.error('Error:', error);
            addMessage('Sorry, I encountered an error. Please try again.', false);
        } finally {
            sendButton.disabled = false;
            userInput.disabled = false;
            userInput.focus(); // Re-enable input field
        }
    }
}

explanation:-
  Gets user input and sends it to the API.
  Disables input while waiting for AI response.
  Handles errors and re-enables input after response.

  
7. Event Listeners for User Interaction
    sendButton.addEventListener('click', handleUserInput);
    Calls handleUserInput() when the send button is clicked.

    userInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault(); // Prevents new line
            handleUserInput(); // Sends message
        }
    });

explanation:-
    Listens for the "Enter" key to send messages (without Shift+Enter for new lines).
    🔹 Summary
    ✅ Uses Google Gemini AI API to generate responses.
    ✅ Formats & displays messages in a chat-like interface.
    ✅ Handles user input, API requests, and errors properly.
    ✅ Auto-scrolls chat and improves user experience.
