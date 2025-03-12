<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>By Mulia</title>
    <link rel="stylesheet" href="tailwand.css">
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-storage.js"></script>
    <script>
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
            appId: "YOUR_APP_ID"
        };

        firebase.initializeApp(firebaseConfig);
        const storage = firebase.storage();

        async function fetchPhotos() {
            const storageRef = storage.ref("uploads");
            const result = await storageRef.listAll();
            const urls = await Promise.all(result.items.map(item => item.getDownloadURL()));
            
            const photoGrid = document.getElementById("photo-grid");
            photoGrid.innerHTML = "";
            urls.forEach(url => {
                const card = document.createElement("div");
                card.className = "photo-card";
                card.innerHTML = `<img src="${url}" class="photo-image" />
                                 <a href="${url}" download class="download-button">Download</a>`;
                photoGrid.appendChild(card);
            });
        }

        async function handleUpload() {
            const fileInput = document.getElementById("file-input");
            if (!fileInput.files.length) return;
            
            const file = fileInput.files[0];
            const storageRef = storage.ref(`uploads/${file.name}`);
            await storageRef.put(file);
            fetchPhotos();
        }

        window.onload = fetchPhotos;
    </script>
</head>
<body>
    <div class="container">
        <h1 class="title">By Mulia</h1>
        <div class="upload-section">
            <input type="file" id="file-input">
            <button onclick="handleUpload()" class="upload-button">Upload Photo</button>
        </div>
        <div class="education-history">
          <h2>Riwayat Pendidikan</h2>
          <p>Pernah bersekolah di SDN 1 Citapen dari tahun 2012-2018, beranjak ke SMPN 3 Ciawi dari tahun 2018-2021, kemudian naik ke jenjang SMA tetapnya bersekolah di SMAN 1 Caringin.</p>
      </div>
        <div id="photo-grid" class="photo-grid"></div>
    </div>
</body>
</html>
