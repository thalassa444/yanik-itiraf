# yanik-itiraf 🔥
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <title>Yanık İtiraf Sitesi</title>
  <style>
    body {
      background: #1e1e2f;
      color: #eee;
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      min-height: 100vh;
      margin: 0;
    }
    h1 {
      color: #ff6f91;
      margin-bottom: 5px;
      text-shadow: 1px 1px 3px #aa5577;
    }
    #welcome {
      font-size: 1.1rem;
      margin-bottom: 20px;
      color: #ffadc6;
      font-style: italic;
      text-align: center;
      max-width: 500px;
    }
    form {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      width: 100%;
      max-width: 500px;
    }
    input[type="text"] {
      flex: 1;
      padding: 10px;
      border-radius: 6px;
      border: none;
      font-size: 1rem;
      box-shadow: inset 0 0 5px #ff6f91;
      background: #2a2a40;
      color: #eee;
      transition: box-shadow 0.3s ease;
    }
    input[type="text"]:focus {
      outline: none;
      box-shadow: 0 0 10px #ff6f91;
    }
    button {
      background: #ff6f91;
      border: none;
      color: white;
      padding: 10px 20px;
      font-size: 1rem;
      border-radius: 6px;
      cursor: pointer;
      transition: background 0.3s ease;
      box-shadow: 0 4px 6px rgba(255, 111, 145, 0.6);
    }
    button:hover {
      background: #ff3b67;
    }
    ul {
      list-style: none;
      padding: 0;
      width: 100%;
      max-width: 500px;
      max-height: 300px;
      overflow-y: auto;
      border: 1px solid #444;
      border-radius: 8px;
      background: #2a2a40;
      box-shadow: inset 0 0 10px #ff6f91;
    }
    ul li {
      padding: 10px;
      border-bottom: 1px solid #444;
      word-wrap: break-word;
    }
    ul li:last-child {
      border-bottom: none;
    }
  </style>
</head>
<body>
  <h1>Yanık İtiraf Sitesi 🔥</h1>
  <div id="welcome">İtirafını yaz, yükle, başkalarınınkini oku. Tamamen anonim!</div>
  <form id="confessionForm">
    <input id="confessionInput" type="text" placeholder="İtirafını yaz, rahat ol..." required />
    <button type="submit">Gönder</button>
  </form>
  <ul id="confessionList"></ul>

  <!-- Firebase UMD (modülsüz) sürüm -->
  <script src="https://www.gstatic.com/firebasejs/10.1.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.1.0/firebase-firestore.js"></script>

  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyAgk2qtVwOsqcfroBULqGrGmC-LYWev21U",
      authDomain: "yanik-itiraf.firebaseapp.com",
      projectId: "yanik-itiraf",
      storageBucket: "yanik-itiraf.firebasestorage.app",
      messagingSenderId: "1002967804957",
      appId: "1:1002967804957:web:4dc985d919ca3b83e8eaf4"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    const form = document.getElementById("confessionForm");
    const input = document.getElementById("confessionInput");
    const list = document.getElementById("confessionList");

    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      const val = input.value.trim();
      if (!val) return;
      await db.collection("confessions").add({
        text: val,
        timestamp: firebase.firestore.FieldValue.serverTimestamp()
      });
      input.value = "";
    });

    db.collection("confessions")
      .orderBy("timestamp", "desc")
      .onSnapshot((snapshot) => {
        list.innerHTML = "";
        snapshot.forEach((doc) => {
          const li = document.createElement("li");
          li.textContent = doc.data().text;
          list.appendChild(li);
        });
      });
  </script>
</body>
</html>
