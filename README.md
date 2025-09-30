[boom room.txt](https://github.com/user-attachments/files/22608294/boom.room.txt)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>BOOM ROOM (Smooth Scroll)</title>
  <style>
    body { font-family: sans-serif; text-align: center; background: #222; color: #fff; transition: background 0.3s; }
    #signup, #login, #chat, #adminPanel { display: none; margin-top: 20px; }
    #messages { border: 1px solid #555; width: 80%; margin: 10px auto; height: 300px; overflow-y: auto; background: #111; padding: 5px; scroll-behavior: smooth; }
    input { padding: 5px; margin: 5px; border-radius: 4px; border: 1px solid #555; }
    button { padding: 8px 15px; margin: 5px; border-radius: 6px; cursor: pointer; }
  </style>
</head>
<body>
  <h1>🔥 BOOM ROOM 🔥</h1>

  <div id="home">
    <button onclick="showSignup()">Sign Up</button>
    <button onclick="showLogin()">Log In</button>
  </div>

  <div id="signup">
    <h2>Sign Up</h2>
    <input id="signupName" placeholder="Name"><br>
    <input id="signupPass" type="password" placeholder="Password"><br>
    <button onclick="signup()">Submit</button>
  </div>

  <div id="login">
    <h2>Log In</h2>
    <input id="loginName" placeholder="Name"><br>
    <input id="loginPass" type="password" placeholder="Password"><br>
    <button onclick="login()">Enter</button>
  </div>

  <div id="chat">
    <div id="messages"></div>
    <input id="chatInput" placeholder="Type a message..." onkeydown="if(event.key==='Enter') sendMessage()">
    <button onclick="sendMessage()">Send</button>
    <button onclick="showAdmin()">Admin Panel</button>
  </div>

  <div id="adminPanel">
    <h3>Admin Panel</h3>
    <button onclick="clearChat()">Clear Chat</button><br>
    <button onclick="changeBackground('red')">Red</button>
    <button onclick="changeBackground('blue')">Blue</button>
    <button onclick="changeBackground('green')">Green</button>
    <button onclick="changeBackground('purple')">Purple</button>
    <button onclick="changeBackground('black')">Black</button>
  </div>

<script>
  let users = JSON.parse(localStorage.getItem('users') || '{}');
  let messages = JSON.parse(localStorage.getItem('messages') || '[]');
  let currentUser = null;

  function showSignup() { hideAll(); document.getElementById('signup').style.display = 'block'; }
  function showLogin() { hideAll(); document.getElementById('login').style.display = 'block'; }
  function showChat() { hideAll(); document.getElementById('chat').style.display = 'block'; renderMessages(); }
  function hideAll() { 
    document.getElementById('signup').style.display = 'none';
    document.getElementById('login').style.display = 'none';
    document.getElementById('chat').style.display = 'none';
    document.getElementById('adminPanel').style.display = 'none';
  }

  function signup() {
    const name = document.getElementById('signupName').value.trim();
    const pass = document.getElementById('signupPass').value.trim();
    if (!name || !pass) return alert('Enter name & password');
    if (users[name]) return alert('Name already taken');
    users[name] = pass;
    localStorage.setItem('users', JSON.stringify(users));
    alert('Remember this, you’ll need it to log in!');
    showLogin();
  }

  function login() {
    const name = document.getElementById('loginName').value.trim();
    const pass = document.getElementById('loginPass').value.trim();
    if (users[name] && users[name] === pass) {
      currentUser = name;
      showChat();
    } else {
      alert('Wrong log in');
    }
  }

  function sendMessage() {
    const input = document.getElementById('chatInput');
    const msg = input.value.trim();
    if (!msg || !currentUser) return;
    messages.push({ name: currentUser, msg });
    localStorage.setItem('messages', JSON.stringify(messages));
    input.value = '';
    renderMessages();
  }

  function renderMessages() {
    const msgDiv = document.getElementById('messages');
    msgDiv.innerHTML = '';
    messages.forEach(m => {
      const div = document.createElement('div');
      div.textContent = `${m.name}: ${m.msg}`;
      msgDiv.appendChild(div);
    });
    // Smooth auto-scroll to the latest message
    msgDiv.scrollTo({ top: msgDiv.scrollHeight, behavior: 'smooth' });
  }

  function showAdmin() {
    const pwd = prompt('Enter admin password:');
    if (pwd === 'kalesia') document.getElementById('adminPanel').style.display = 'block';
    else alert('Wrong password');
  }

  function clearChat() {
    messages = [];
    localStorage.setItem('messages', JSON.stringify(messages));
    renderMessages();
  }

  function changeBackground(color) {
    document.body.style.background = color;
    localStorage.setItem('bg', color);
  }

  // Load background from localStorage
  const savedBg = localStorage.getItem('bg');
  if (savedBg) document.body.style.background = savedBg;

  // Auto-update for multi-tab simulation
  setInterval(() => {
    messages = JSON.parse(localStorage.getItem('messages') || '[]');
    renderMessages();
    const bg = localStorage.getItem('bg');
    if (bg) document.body.style.background = bg;
  }, 500);
</script>
</body>
</html>
