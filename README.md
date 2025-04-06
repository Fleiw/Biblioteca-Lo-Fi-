<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Leitura do Livro</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body class="p-4 font-sans">
  <div id="conteudo" class="max-w-3xl mx-auto">
    <p>Carregando livro...</p>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
    import { getFirestore, doc, getDoc } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "SUA_API_KEY",
      authDomain: "SEU_AUTH_DOMAIN",
      projectId: "SEU_PROJECT_ID",
      storageBucket: "SEU_BUCKET",
      messagingSenderId: "SEU_SENDER_ID",
      appId: "SEU_APP_ID"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);

    const params = new URLSearchParams(window.location.search);
    const id = params.get("id");

    const conteudo = document.getElementById("conteudo");

    async function carregarLivro() {
      const ref = doc(db, "livros", id);
      const snap = await getDoc(ref);
      if (!snap.exists()) {
        conteudo.innerHTML = "<p>Livro não encontrado.</p>";
        return;
      }

      const livro = snap.data();
      conteudo.innerHTML = `
        <h1 class="text-3xl font-bold mb-2">${livro.titulo}</h1>
        <p class="text-gray-600 mb-4">por ${livro.autor}</p>
        <img src="${livro.capaUrl}" class="w-full max-h-96 object-contain rounded mb-4"/>
        <p class="mb-6">${livro.descricao}</p>
        ${livro.pdfUrl ? `<iframe src="${livro.pdfUrl}" class="w-full h-[600px] border rounded"></iframe>` : '<p>PDF não disponível</p>'}
      `;
    }

    carregarLivro();
  </script>
</body>
</html>
