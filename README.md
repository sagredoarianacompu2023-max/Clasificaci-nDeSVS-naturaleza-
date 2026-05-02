<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Diario Visual Retro</title>
  <style>
    :root {
      --bg: #060b04;
      --panel: rgba(7, 18, 8, 0.95);
      --panel-strong: rgba(2, 8, 3, 0.98);
      --text: #a3ff8f;
      --text-soft: #7edc8f;
      --border: rgba(134, 255, 132, 0.18);
      --accent: #d0ffad;
      --shadow: 0 0 20px rgba(51, 255, 116, 0.08);
    }

    * { box-sizing: border-box; }
    html, body { margin: 0; min-height: 100%; background: radial-gradient(circle at top, rgba(20, 80, 20, 0.2), transparent 35%), linear-gradient(180deg, #0b1408 0%, #050703 100%); color: var(--text); font-family: "Courier New", Courier, monospace; }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      background-image: linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px);
      background-size: 100% 4px, 4px 100%;
      opacity: 0.16;
      z-index: 0;
    }

    .app-shell { position: relative; z-index: 1; max-width: 1200px; margin: 0 auto; padding: 24px; }
    header { display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 16px; margin-bottom: 24px; padding: 22px; border: 1px solid var(--border); border-radius: 20px; background: rgba(0,0,0,0.55); box-shadow: var(--shadow); }
    h1 { margin: 0; font-size: clamp(1.6rem, 2vw, 2.3rem); letter-spacing: 0.08em; text-transform: uppercase; color: var(--accent); }
    .user-bar { display: flex; flex-wrap: wrap; align-items: center; gap: 10px; font-size: 0.95rem; }
    .user-bar span { display: inline-flex; align-items: center; gap: 6px; }

    .button, .secondary { border: 1px solid var(--border); border-radius: 999px; background: rgba(8, 18, 10, 0.86); color: var(--text); padding: 10px 18px; cursor: pointer; transition: background 0.2s ease, transform 0.15s ease; text-decoration: none; font-family: inherit; }
    .button:hover, .secondary:hover { background: rgba(24, 55, 25, 0.96); transform: translateY(-1px); }
    .button:active, .secondary:active { transform: translateY(0); }

    .panel { margin-bottom: 28px; padding: 20px; border: 1px solid var(--border); border-radius: 18px; background: var(--panel); box-shadow: var(--shadow); }
    .hidden { display: none !important; }
    .panel h2 { margin-top: 0; color: var(--accent); letter-spacing: 0.06em; text-transform: uppercase; font-size: 1.05rem; }
    .field-group { display: grid; gap: 14px; margin-top: 18px; }
    .field-group label { display: grid; gap: 8px; font-size: 0.95rem; color: var(--text-soft); }
    input[type="text"], textarea, input[type="file"] { width: 100%; background: rgba(3, 10, 4, 0.92); border: 1px solid rgba(140, 255, 128, 0.18); border-radius: 12px; color: var(--text); font-family: inherit; font-size: 0.95rem; padding: 12px 14px; }
    textarea { min-height: 140px; resize: vertical; white-space: pre-wrap; line-height: 1.6; }

    .action-row { display: flex; flex-wrap: wrap; gap: 12px; margin-top: 18px; }
    .hint { margin-top: 12px; color: rgba(158, 255, 142, 0.68); font-size: 0.92rem; line-height: 1.5; }

    .feed-title { margin: 0 0 18px; font-size: 1.05rem; color: var(--accent); letter-spacing: 0.08em; text-transform: uppercase; }
    .gallery { display: grid; gap: 18px; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); }

    .card { position: relative; overflow: hidden; border: 1px solid rgba(146, 255, 132, 0.16); border-radius: 20px; background: rgba(5, 13, 6, 0.96); box-shadow: inset 0 0 40px rgba(0, 255, 120, 0.04), var(--shadow); }
    .card-image { display: block; width: 100%; aspect-ratio: 4 / 3; object-fit: cover; filter: sepia(0.22) saturate(0.38) hue-rotate(80deg) contrast(0.96); transition: filter 0.35s ease, transform 0.35s ease; cursor: pointer; }
    .card:hover .card-image { filter: none; transform: scale(1.02); }
    .card-body { padding: 16px; display: grid; gap: 12px; }
    .card-title { margin: 0; font-size: 1rem; text-transform: uppercase; letter-spacing: 0.06em; color: var(--accent); }
    .card-date { margin: 0; font-size: 0.82rem; color: rgba(158, 255, 143, 0.72); }
    .card-text { margin: 0; white-space: pre-wrap; line-height: 1.55; color: var(--text-soft); min-height: 3.4rem; }
    .card-actions { display: flex; gap: 10px; flex-wrap: wrap; justify-content: flex-end; }
    .card-actions button { border: 1px solid rgba(146, 255, 132, 0.22); background: rgba(7, 18, 7, 0.92); color: var(--text); padding: 8px 12px; border-radius: 999px; cursor: pointer; font-size: 0.88rem; transition: background 0.2s ease; }
    .card-actions button:hover { background: rgba(24, 60, 24, 0.94); }
    .card-actions button.delete { border-color: rgba(255, 110, 110, 0.24); color: #ff9c9c; }
    .status { margin-top: 10px; color: rgba(150, 255, 140, 0.8); font-size: 0.9rem; }

    @media (max-width: 700px) {
      header { flex-direction: column; align-items: stretch; }
      .action-row { justify-content: stretch; }
      .user-bar { justify-content: space-between; }
    }
  </style>
</head>
<body>
  <div class="app-shell">
    <header>
      <div>
        <h1>Diario Visual</h1>
        <p class="status" id="modeLabel">Estado: Modo lectura</p>
      </div>
      <div class="user-bar">
        <span id="userInfo">No autenticado</span>
        <button class="button" id="signInBtn">Iniciar sesión</button>
        <button class="secondary hidden" id="signOutBtn">Cerrar sesión</button>
      </div>
    </header>

    <section class="panel hidden" id="adminPanel">
      <h2>Panel de administración</h2>
      <div class="field-group">
        <label>
          Imagen
          <input type="file" id="imageInput" accept="image/*" />
        </label>
        <label>
          Título corto
          <input type="text" id="titleInput" placeholder="Ej. Caminata en el bosque" />
        </label>
        <label>
          Texto largo
          <textarea id="textInput" placeholder="Escribe tu entrada..." spellcheck="true"></textarea>
        </label>
      </div>
      <div class="action-row">
        <button class="button" id="publishBtn">Publicar</button>
        <button class="secondary hidden" id="cancelEditBtn">Cancelar edición</button>
      </div>
      <p class="hint">Solo el administrador puede subir, editar y borrar publicaciones. Las demás cuentas solo tienen modo lectura.</p>
    </section>

    <section>
      <h2 class="feed-title">Feed público</h2>
      <div class="gallery" id="entries"></div>
      <p class="status" id="feedStatus">Cargando publicaciones...</p>
    </section>
  </div>

  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-firestore-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-storage-compat.js"></script>
  <script>
    // Correo que tiene privilegios de administración.
    const adminEmail = "sagredoarianacompu2023@gmail.com";

    // Rellenar con tu configuración real de Firebase.
    const firebaseConfig = {
      apiKey: "TU_API_KEY",
      authDomain: "TU_AUTH_DOMAIN",
      projectId: "TU_PROJECT_ID",
      storageBucket: "TU_STORAGE_BUCKET",
      messagingSenderId: "TU_MESSAGING_SENDER_ID",
      appId: "TU_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.firestore();
    const storage = firebase.storage();

    const signInBtn = document.getElementById("signInBtn");
    const signOutBtn = document.getElementById("signOutBtn");
    const userInfo = document.getElementById("userInfo");
    const modeLabel = document.getElementById("modeLabel");
    const adminPanel = document.getElementById("adminPanel");

    const imageInput = document.getElementById("imageInput");
    const titleInput = document.getElementById("titleInput");
    const textInput = document.getElementById("textInput");
    const publishBtn = document.getElementById("publishBtn");
    const cancelEditBtn = document.getElementById("cancelEditBtn");
    const entriesContainer = document.getElementById("entries");
    const feedStatus = document.getElementById("feedStatus");

    let currentUser = null;
    let currentEditId = null;
    let currentImageUrl = "";
    let currentImagePath = "";

    signInBtn.addEventListener("click", async () => {
      const provider = new firebase.auth.GoogleAuthProvider();
      try {
        await auth.signInWithRedirect(provider);
      } catch (error) {
        alert("Error de inicio de sesión: " + error.message);
      }
    });

    signOutBtn.addEventListener("click", async () => {
      await auth.signOut();
    });

    publishBtn.addEventListener("click", handlePublish);
    cancelEditBtn.addEventListener("click", resetForm);

    auth.onAuthStateChanged((user) => {
      currentUser = user;
      updateInterface();
    });

    function updateInterface() {
      const isAdmin = currentUser?.email === adminEmail;
      if (currentUser) {
        userInfo.textContent = `Usuario: ${currentUser.displayName || currentUser.email}`;
        signInBtn.classList.add("hidden");
        signOutBtn.classList.remove("hidden");
      } else {
        userInfo.textContent = "No autenticado";
        signInBtn.classList.remove("hidden");
        signOutBtn.classList.add("hidden");
      }

      if (isAdmin) {
        adminPanel.classList.remove("hidden");
        modeLabel.textContent = "Estado: Administrador activo";
      } else {
        adminPanel.classList.add("hidden");
        modeLabel.textContent = "Estado: Modo lectura";
      }
    }

    function showStatus(message) {
      feedStatus.textContent = message;
    }

    function formatTimestamp(timestamp) {
      if (!timestamp) return "Fecha desconocida";
      const date = timestamp.toDate ? timestamp.toDate() : new Date(timestamp);
      return date.toLocaleDateString("es-ES", { weekday: "short", year: "numeric", month: "short", day: "numeric" });
    }

    async function handlePublish() {
      const title = titleInput.value.trim();
      const text = textInput.value.trim();
      const file = imageInput.files[0];
      const isAdmin = currentUser?.email === adminEmail;

      if (!isAdmin) {
        alert("Solo el administrador puede publicar.");
        return;
      }

      if (!title || !text) {
        alert("Debes completar el título y el texto.");
        return;
      }

      if (!currentImageUrl && !file) {
        alert("Selecciona una imagen para publicar.");
        return;
      }

      publishBtn.disabled = true;
      publishBtn.textContent = currentEditId ? "Guardando..." : "Publicando...";

      try {
        let imageUrl = currentImageUrl;
        let imagePath = currentImagePath;

        if (file) {
          if (currentImagePath) {
            await storage.ref(currentImagePath).delete().catch(() => {});
          }
          const timestamp = Date.now();
          imagePath = `entries/${timestamp}_${file.name.replace(/\s+/g, "_")}`;
          await storage.ref(imagePath).put(file);
          imageUrl = await storage.ref(imagePath).getDownloadURL();
        }

        const payload = {
          title,
          text,
          imageUrl,
          imagePath,
          updatedAt: firebase.firestore.FieldValue.serverTimestamp()
        };

        if (currentEditId) {
          await db.collection("Diario Visuala").doc(currentEditId).update(payload);
        } else {
          await db.collection("Diario Visual").add({ ...payload, createdAt: firebase.firestore.FieldValue.serverTimestamp() });
        }

        resetForm();
      } catch (error) {
        alert("Error al guardar la publicación: " + error.message);
      } finally {
        publishBtn.disabled = false;
        publishBtn.textContent = currentEditId ? "Guardar cambios" : "Publicar";
      }
    }

    function resetForm() {
      currentEditId = null;
      currentImageUrl = "";
      currentImagePath = "";
      imageInput.value = "";
      titleInput.value = "";
      textInput.value = "";
      publishBtn.textContent = "Publicar";
      cancelEditBtn.classList.add("hidden");
    }

    function renderEntry(doc) {
      const item = doc.data();
      const card = document.createElement("article");
      card.className = "card";

      const image = document.createElement("img");
      image.className = "card-image";
      image.src = item.imageUrl || "";
      image.alt = item.title || "Publicación";
      image.addEventListener("click", () => { if (item.imageUrl) window.open(item.imageUrl, "_blank"); });

      const body = document.createElement("div");
      body.className = "card-body";

      const title = document.createElement("h3");
      title.className = "card-title";
      title.textContent = item.title || "Sin título";

      const date = document.createElement("p");
      date.className = "card-date";
      date.textContent = formatTimestamp(item.createdAt || item.updatedAt);

      const text = document.createElement("p");
      text.className = "card-text";
      text.textContent = item.text || "";

      body.append(title, date, text);

      const isAdmin = currentUser?.email === adminEmail;
      if (isAdmin) {
        const actions = document.createElement("div");
        actions.className = "card-actions";

        const editButton = document.createElement("button");
        editButton.type = "button";
        editButton.className = "edit";
        editButton.textContent = "Editar";
        editButton.addEventListener("click", () => loadEntryForEdit(doc.id, item));

        const deleteButton = document.createElement("button");
        deleteButton.type = "button";
        deleteButton.className = "delete";
        deleteButton.textContent = "Eliminar";
        deleteButton.addEventListener("click", () => deleteEntry(doc.id, item));

        actions.append(editButton, deleteButton);
        body.appendChild(actions);
      }

      card.append(image, body);
      return card;
    }

    function loadEntryForEdit(id, item) {
      currentEditId = id;
      titleInput.value = item.title || "";
      textInput.value = item.text || "";
      currentImageUrl = item.imageUrl || "";
      currentImagePath = item.imagePath || "";
      publishBtn.textContent = "Guardar cambios";
      cancelEditBtn.classList.remove("hidden");
      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    async function deleteEntry(id, item) {
      const isAdmin = currentUser?.email === adminEmail;
      if (!isAdmin) {
        alert("Solo el administrador puede borrar publicaciones.");
        return;
      }
      const confirmed = confirm("¿Eliminar esta publicación? Esta acción no se puede deshacer.");
      if (!confirmed) return;

      try {
        if (item.imagePath) {
          await storage.ref(item.imagePath).delete().catch(() => {});
        }
        await db.collection("Diario Visual").doc(id).delete();
      } catch (error) {
        alert("Error al eliminar: " + error.message);
      }
    }

    function subscribeFeed() {
      db.collection("Diario Visual").orderBy("createdAt", "desc").onSnapshot((snapshot) => {
        entriesContainer.innerHTML = "";
        if (snapshot.empty) {
          showStatus("Sin publicaciones aún. El administrador puede agregar entradas.");
          return;
        }
        snapshot.forEach((doc) => {
          entriesContainer.appendChild(renderEntry(doc));
        });
        showStatus(`${snapshot.size} publicación(es) visibles.`);
      }, (error) => {
        showStatus("No se pudo cargar el feed: " + error.message);
      });
    }

    subscribeFeed();
  </script>

</body>
</html>


<rules_version = '2';
  service cloud.firestore {
        match /databases/{database}/documents {
          match /Diario Visual/{docId} {
            allow read: if true;
            allow create, update, delete: if request.auth != null
              && request.auth.token.email == "sagredoarianacompu2023@gmail.com";
          }
        }
      }

    Storage:
      service firebase.storage {
        match /b/{bucket}/o {
          match /entries/{allPaths=**} {
            allow read: if true;
            allow write: if request.auth != null
              && request.auth.token.email == "sagredoarianacompu2023@gmail.com";
          }
        }
      }
  >
