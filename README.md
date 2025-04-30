# superfastcouriers1<!DOCTYPE html>
<html>
<head>
  <title>Manage AWB</title>
</head>
<body>
  <h2>Enter AWB Number</h2>
  <form id="awbForm">
    <input type="text" id="awbNumber" placeholder="AWB Number" required />
    <button type="submit">Save AWB</button>
  </form>
  <p id="message"></p>

  <h2>Track AWB</h2>
  <input type="text" id="trackInput" placeholder="Enter AWB to Track" />
  <button onclick="trackAWB()">Track</button>
  <div id="trackResult"></div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
    import { getDatabase, ref, set, get } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBAMd2GT9GtHEZ-Sf_RuUkUaNghq14bEw",
      authDomain: "superfastpanel-da000.firebaseapp.com",
      databaseURL: "https://superfastpanel-da000-default-rtdb.firebaseio.com",
      projectId: "superfastpanel-da000",
      storageBucket: "superfastpanel-da000.appspot.com",
      messagingSenderId: "405590662257",
      appId: "1:405590662257:web:69809813dce14b09e8cfd0"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    const form = document.getElementById('awbForm');
    const messageDiv = document.getElementById('message');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      const awb = document.getElementById('awbNumber').value.trim();
      if (!awb) return;
      await set(ref(db, 'awbs/' + awb), {
        status: "In Transit",
        updated: new Date().toISOString()
      });
      messageDiv.textContent = "AWB saved successfully!";
      form.reset();
    });

    window.trackAWB = async () => {
      const input = document.getElementById('trackInput').value.trim();
      const resultDiv = document.getElementById('trackResult');
      if (!input) {
        resultDiv.textContent = "Please enter an AWB.";
        return;
      }
      const snapshot = await get(ref(db, 'awbs/' + input));
      if (snapshot.exists()) {
        const data = snapshot.val();
        resultDiv.innerHTML = `Status: ${data.status}<br>Updated: ${data.updated}`;
      } else {
        resultDiv.textContent = "AWB not found.";
      }
    };
  </script>
</body>
</html>
