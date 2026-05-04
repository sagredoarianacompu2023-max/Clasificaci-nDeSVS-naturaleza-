



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
    header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; padding: 16px 0; }
    h1 { margin: 0; font-size: clamp(1.8rem, 3vw, 2.5rem); letter-spacing: 0.08em; text-transform: uppercase; color: var(--accent); text-shadow: 0 0 10px rgba(208, 255, 173, 0.5); }
    .access-btn { background: none; border: 1px solid var(--border); border-radius: 8px; color: var(--text-soft); padding: 8px 16px; cursor: pointer; font-family: inherit; font-size: 0.9rem; transition: all 0.2s ease; }
    .access-btn:hover { background: rgba(24, 55, 25, 0.5); color: var(--text); }

    .user-info { display: flex; align-items: center; gap: 12px; font-size: 0.9rem; }
    .user-info span { color: var(--text-soft); }
    .logout-btn { background: none; border: 1px solid rgba(255, 110, 110, 0.3); border-radius: 8px; color: #ff9c9c; padding: 6px 12px; cursor: pointer; font-family: inherit; font-size: 0.85rem; transition: all 0.2s ease; }
    .logout-btn:hover { background: rgba(255, 110, 110, 0.1); }

    .modal-overlay { position: fixed; inset: 0; background: rgba(0, 0, 0, 0.8); display: flex; align-items: center; justify-content: center; z-index: 1000; opacity: 0; visibility: hidden; transition: all 0.3s ease; }
    .modal-overlay.show { opacity: 1; visibility: visible; }
    .modal-content { background: var(--panel); border: 1px solid var(--border); border-radius: 12px; padding: 24px; max-width: 400px; width: 90%; box-shadow: var(--shadow); }
    .modal-content h2 { margin-top: 0; color: var(--accent); font-size: 1.2rem; text-align: center; }
    .modal-content .field-group { margin-top: 20px; }
    .modal-content .field-group label { font-size: 0.9rem; }
    .modal-content input { margin-bottom: 8px; }
    .modal-content .action-row { display: flex; gap: 12px; justify-content: center; margin-top: 20px; }
    .modal-content .button { min-width: 100px; }

    .panel { margin-bottom: 32px; padding: 20px; background: var(--panel); border-radius: 12px; box-shadow: var(--shadow); }
    .hidden { display: none !important; }
    .panel h2 { margin-top: 0; color: var(--accent); letter-spacing: 0.06em; text-transform: uppercase; font-size: 1.1rem; }

    .field-group { display: grid; gap: 12px; margin-top: 16px; }
    .field-group label { display: grid; gap: 6px; font-size: 0.9rem; color: var(--text-soft); }
    input[type="text"], input[type="email"], input[type="password"], textarea, input[type="file"] { width: 100%; background: rgba(3, 10, 4, 0.92); border: 1px solid rgba(140, 255, 128, 0.18); border-radius: 8px; color: var(--text); font-family: inherit; font-size: 0.9rem; padding: 10px 12px; }
    textarea { min-height: 120px; resize: vertical; white-space: pre-wrap; line-height: 1.6; }

    .action-row { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 16px; }
    .button, .secondary { border: 1px solid var(--border); border-radius: 8px; background: rgba(8, 18, 10, 0.86); color: var(--text); padding: 8px 16px; cursor: pointer; transition: all 0.2s ease; text-decoration: none; font-family: inherit; font-size: 0.9rem; }
    .button:hover, .secondary:hover { background: rgba(24, 55, 25, 0.96); transform: translateY(-1px); }
    .button:active, .secondary:active { transform: translateY(0); }

    .feed-title { margin: 0 0 16px; font-size: 1.1rem; color: var(--accent); letter-spacing: 0.08em; text-transform: uppercase; }
    .gallery { display: grid; gap: 16px; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); }

    .card { position: relative; overflow: hidden; border-radius: 12px; background: rgba(5, 13, 6, 0.96); box-shadow: inset 0 0 40px rgba(0, 255, 120, 0.04), var(--shadow); }
    .card-image { display: block; width: 100%; aspect-ratio: 4 / 3; object-fit: cover; filter: sepia(0.22) saturate(0.38) hue-rotate(80deg) contrast(0.96); transition: filter 0.35s ease, transform 0.35s ease; cursor: pointer; }
    .card:hover .card-image { filter: none; transform: scale(1.02); }
    .card-body { padding: 14px; display: grid; gap: 10px; }
    .card-title { margin: 0; font-size: 1rem; text-transform: uppercase; letter-spacing: 0.06em; color: var(--accent); }
    .card-date { margin: 0; font-size: 0.8rem; color: rgba(158, 255, 143, 0.72); }
    .card-text { margin: 0; white-space: pre-wrap; line-height: 1.55; color: var(--text-soft); min-height: 3rem; }
    .card-actions { display: flex; gap: 8px; flex-wrap: wrap; justify-content: flex-end; }
    .card-actions button { border: 1px solid rgba(146, 255, 132, 0.22); background: rgba(7, 18, 7, 0.92); color: var(--text); padding: 6px 10px; border-radius: 6px; cursor: pointer; font-size: 0.85rem; transition: background 0.2s ease; }
    .card-actions button:hover { background: rgba(24, 60, 24, 0.94); }
    .card-actions button.delete { border-color: rgba(255, 110, 110, 0.24); color: #ff9c9c; }

    .status { margin-top: 12px; color: rgba(150, 255, 140, 0.8); font-size: 0.9rem; }

    @media (max-width: 700px) {
      .app-shell { padding: 16px; }
      header { flex-direction: column; align-items: center; gap: 16px; }
      .modal-content { padding: 20px; }
      .gallery { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <div class="app-shell">
    <header>
      <h1>DIARIO VISUAL</h1>
      <div id="userBar">
        <button class="access-btn" id="accessBtn">Acceso Editor</button>
      </div>
    </header>

    <!-- Modal de Login -->
    <div class="modal-overlay" id="loginModal">
      <div class="modal-content">
        <h2>Acceso Editor</h2>
        <button class="button" id="showLoginBtn">Iniciar sesión</button>
        <div id="loginFields" class="hidden">
          <div class="field-group">
            <label>
              Gmail
              <input type="email" id="emailInput" placeholder="tu@email.com" />
            </label>
            <label>
              Contraseña
              <input type="password" id="passwordInput" placeholder="Tu contraseña" />
            </label>
          </div>
          <div class="action-row">
            <button class="button" onclick="iniciarSesion()">Entrar</button>
            <button class="secondary" onclick="cancelLogin()">Cancelar</button>
          </div>
        </div>
      </div>
    </div>

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

  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2.35.0/dist/umd/supabase.min.js"></script>
  <script>
    // Correo que tiene privilegios de administración.
    const adminEmail = "sagredoarianacompu2023@gmail.com";

    const SUPABASE_URL = "https://bgcbavxgvhezxsjgkqeb.supabase.co";
    const SUPABASE_KEY = "sb_publishable_MNdq_3YCOGmIbQZ67JY5Sw_oZ4JJBBc";
    const SUPABASE_BUCKET = "Entries"; 
    // Debe ser idéntico al nombre en el Dashboard
    const SUPABASE_BUCKET = "Entries";

    const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

    const signInBtn = document.getElementById("signInBtn");
    const createEditorBtn = document.getElementById("createEditorBtn");
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

    // Modal elements
    const accessBtn = document.getElementById("accessBtn");
    const loginModal = document.getElementById("loginModal");
    const showLoginBtn = document.getElementById("showLoginBtn");
    const loginFields = document.getElementById("loginFields");
    const emailInput = document.getElementById("emailInput");
    const passwordInput = document.getElementById("passwordInput");

    let currentUser = null;
    let currentEditId = null;
    let currentImageUrl = "";
    let currentImagePath = "";

    // Modal event listeners
    document.getElementById("userBar").addEventListener("click", (e) => {
      if (e.target.id === "accessBtn") {
        loginModal.classList.add("show");
      }
    });

    showLoginBtn.addEventListener("click", () => {
      loginFields.classList.remove("hidden");
      showLoginBtn.classList.add("hidden");
    });

    loginModal.addEventListener("click", (e) => {
      if (e.target === loginModal) {
        cancelLogin();
      }
    });

    publishBtn.addEventListener("click", handlePublish);
    cancelEditBtn.addEventListener("click", resetForm);

    async function initAuth() {
      const {
        data: { session },
        error
      } = await supabaseClient.auth.getSession();
      if (error) console.warn("Supabase Auth error:", error.message);
      currentUser = session?.user || null;
      updateInterface();

      supabaseClient.auth.onAuthStateChange((event, session) => {
        currentUser = session?.user || null;
        updateInterface();
      });
    }

    function updateInterface() {
      const isAdmin = currentUser?.email === adminEmail;
      const userBar = document.getElementById("userBar");
      const accessBtn = document.getElementById("accessBtn");

      if (currentUser) {
        userBar.innerHTML = `
          <div class="user-info">
            <span>${currentUser.user_metadata?.full_name || currentUser.email}</span>
            <button class="logout-btn" onclick="cerrarSesion()">Cerrar sesión</button>
          </div>
        `;
      } else {
        userBar.innerHTML = '<button class="access-btn" id="accessBtn">Acceso Editor</button>';
      }

      if (isAdmin) {
        adminPanel.classList.remove("hidden");
      } else {
        adminPanel.classList.add("hidden");
      }
    }

    function cancelLogin() {
      loginModal.classList.remove("show");
      loginFields.classList.add("hidden");
      showLoginBtn.classList.remove("hidden");
      emailInput.value = "";
      passwordInput.value = "";
    }

    async function cerrarSesion() {
      await supabaseClient.auth.signOut();
      currentUser = null;
      updateInterface();
    }

    function showStatus(message) {
      feedStatus.textContent = message;
    }

    function formatTimestamp(value) {
      if (!value) return "Fecha desconocida";
      const date = new Date(value);
      return date.toLocaleDateString("es-ES", { weekday: "short", year: "numeric", month: "short", day: "numeric" });
    }

    async function uploadImage(file) {
      const imagePath = `${Date.now()}_${file.name.replace(/\s+/g, "_")}`;
      const { data, error } = await supabaseClient.storage.from(SUPABASE_BUCKET).upload(imagePath, file, {
        cacheControl: "3600",
        upsert: false
      });
      if (error) throw new Error(error.message);

      const { data: publicUrlData, error: publicUrlError } = supabaseClient.storage.from(SUPABASE_BUCKET).getPublicUrl(imagePath);
      if (publicUrlError) throw new Error(publicUrlError.message);
      return { imageUrl: publicUrlData.publicUrl, imagePath };
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
            await supabaseClient.storage.from(SUPABASE_BUCKET).remove([currentImagePath]).catch(() => {});
          }
          const uploaded = await uploadImage(file);
          imageUrl = uploaded.imageUrl;
          imagePath = uploaded.imagePath;
        }

        const payload = {
          title,
          text,
          imageUrl,
          imagePath,
          updatedAt: new Date().toISOString()
        };

        if (currentEditId) {
          const { error } = await supabaseClient.from("diario").update(payload).eq("id", currentEditId);
          if (error) throw new Error(error.message);
        } else {
          const { error } = await supabaseClient.from("diario").insert([{ ...payload, createdAt: new Date().toISOString() }]);
          if (error) throw new Error(error.message);
        }

        resetForm();
        await loadEntries();
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

    // Para Registrarse (Sign Up)
    async function registrarUsuario() {
      const { data, error } = await supabaseClient.auth.signUp({
        email: 'sagredoarianacompu2023@gmail.com',
        password: 'mnbvcxz2005',
      })
      if (error) console.error("Error al registrarse:", error.message);
      else console.log("Registro exitoso", data.user);
    }

    // Función para Iniciar Sesión pidiendo datos
    async function iniciarSesion() {
        // Obtenemos los valores que escribiste en la pantalla
        const email = document.getElementById('emailInput').value.trim();
        const password = document.getElementById('passwordInput').value.trim();

        // Validación: comprobar que no estén vacíos
        if (!email || !password) {
            alert("Por favor completa el email y la contraseña.");
            return;
        }

        const { data, error } = await supabaseClient.auth.signInWithPassword({
            email: email,
            password: password,
        });

        if (error) {
            console.error("Error al entrar:", error.message);
            alert("Error al entrar: " + error.message);
        } else {
            console.log("¡Sesión iniciada!", data.user);
            currentUser = data.user;
            
            // Verificar si el usuario es administrador
            if (data.user.email === adminEmail) {
                console.log("✓ Acceso de administrador detectado");
                alert("¡Bienvenido, Editor!");
            } else {
                alert("¡Bienvenido!");
            }
            
            // Limpiar campos y cerrar modal
            cancelLogin();
            updateInterface();
        }
    }

    // Función para Crear Editor pidiendo datos
    async function crearEditor() {
        // Obtenemos los valores que escribiste en la pantalla
        const email = document.getElementById('emailInput').value.trim();
        const password = document.getElementById('passwordInput').value.trim();

        // Validación: comprobar que no estén vacíos
        if (!email || !password) {
            alert("Por favor completa el email y la contraseña.");
            return;
        }

        // Validación: contraseña mínima de 6 caracteres
        if (password.length < 6) {
            alert("La contraseña debe tener al menos 6 caracteres.");
            return;
        }

        const { data, error } = await supabaseClient.auth.signUp({
            email: email,
            password: password,
        });

        if (error) {
            console.error("Error al crear usuario:", error.message);
            alert("Error al crear: " + error.message);
        } else {
            console.log("Usuario creado con éxito:", data.user);
            alert("Usuario creado con éxito. Ahora puedes iniciar sesión con estos datos.");
            
            // Limpiar campos después de crear el usuario
            document.getElementById('emailInput').value = "";
            document.getElementById('passwordInput').value = "";
        }
    }

    function renderEntry(item) {
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
        editButton.addEventListener("click", () => loadEntryForEdit(item.id, item));

        const deleteButton = document.createElement("button");
        deleteButton.type = "button";
        deleteButton.className = "delete";
        deleteButton.textContent = "Eliminar";
        deleteButton.addEventListener("click", () => deleteEntry(item.id, item));

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
          await supabaseClient.storage.from(SUPABASE_BUCKET).remove([item.imagePath]).catch(() => {});
        }
        const { error } = await supabaseClient.from("diario").delete().eq("id", id);
        if (error) throw new Error(error.message);
        await loadEntries();
      } catch (error) {
        alert("Error al eliminar: " + error.message);
      }
    }

    async function loadEntries() {
      const { data, error } = await supabaseClient.from("diario").select("*").order("createdAt", { ascending: false });
      if (error) {
        showStatus("No se pudo cargar el feed: " + error.message);
        return;
      }

      entriesContainer.innerHTML = "";
      if (!data || data.length === 0) {
        showStatus("Sin publicaciones aún. El administrador puede agregar entradas.");
        return;
      }

      data.forEach((item) => {
        entriesContainer.appendChild(renderEntry(item));
      });
      showStatus(`${data.length} publicación(es) visibles.`);
    }

    initAuth();
    loadEntries();
  </script>
</body>
</html>
