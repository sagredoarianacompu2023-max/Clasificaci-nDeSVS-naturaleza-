<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Biblioteca Científica Natural C.N</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root { --primary: #2e7d32; --secondary: #f1f8e9; --accent: #a5d6a7; }
        body { font-family: 'Segoe UI', sans-serif; background: #fafafa; margin: 0; }
        
        /* Navbar */
        nav { background: var(--primary); color: white; padding: 1rem 5%; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        .logo { font-weight: bold; font-size: 1.5rem; letter-spacing: 1px; }
        .btn-google { background: white; color: #444; border: none; padding: 8px 15px; border-radius: 5px; cursor: pointer; font-weight: bold; }

        /* Feed de Publicaciones */
        .container { max-width: 1000px; margin: 2rem auto; padding: 0 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 25px; }
        
        .card { background: white; border-radius: 15px; overflow: hidden; box-shadow: 0 4px 15px rgba(0,0,0,0.1); transition: 0.3s; }
        .card img { width: 100%; height: 200px; object-fit: cover; cursor: zoom-in; }
        .card-body { padding: 15px; }
        .card-tag { background: var(--secondary); color: var(--primary); padding: 4px 10px; border-radius: 20px; font-size: 0.8rem; font-weight: bold; }
        
        /* Modales y Formularios */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 1000; align-items: center; justify-content: center; }
        .form-upload { background: white; padding: 30px; border-radius: 20px; width: 90%; max-width: 500px; }
        
        input, select, textarea { width: 100%; padding: 10px; margin: 10px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; }
        .btn-main { background: var(--primary); color: white; border: none; width: 100%; padding: 12px; border-radius: 8px; cursor: pointer; font-size: 1rem; }
    </style>
</head>
<body>

<nav>
    <div class="logo"><i class="fas fa-leaf"></i> Biblioteca C.N</div>
    <button class="btn-google" id="loginBtn"><i class="fab fa-google"></i> Conectar con Gmail</button>
</nav>

<div class="container">
    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem;">
        <h2>Exploraciones Naturales</h2>
        <button onclick="toggleModal('uploadModal')" style="background: var(--primary); color: white; border: none; padding: 10px 20px; border-radius: 50px; cursor: pointer;">
            <i class="fas fa-plus"></i> Nueva Entrada
        </button>
    </div>

    <div class="grid" id="mainGrid">
        <div class="card">
            <img src="https://images.unsplash.com/photo-1518531933037-91b2f5f229cc" onclick="zoomImage(this.src)">
            <div class="card-body">
                <span class="card-tag">Fungi</span>
                <h3 style="margin: 10px 0 5px;">Amanita Muscaria</h3>
                <p style="color: #666; font-size: 0.9rem;"><i class="far fa-calendar"></i> 23/04/2026</p>
                <button onclick="editEntry()" style="border: none; background: none; color: #2e7d32; cursor: pointer; font-weight: bold; padding: 0;">Editar datos</button>
            </div>
        </div>
    </div>
</div>

<div id="uploadModal" class="modal">
    <div class="form-upload">
        <h3>Identificar Hallazgo</h3>
        <input type="file" id="photoInput" accept="image/*">
        <input type="text" placeholder="Nombre científico o común">
        <select>
            <option>Categoría</option>
            <option>Flora</option>
            <option>Fauna (Insectos/Spiders)</option>
            <option>Fungi</option>
            <option>Geología</option>
        </select>
        <input type="date">
        <button class="btn-main">Publicar en mi Biblioteca</button>
        <button onclick="toggleModal('uploadModal')" style="background:none; border:none; color:red; margin-top:10px; width:100%; cursor:pointer;">Cancelar</button>
    </div>
</div>

<script>
    function toggleModal(id) {
        const m = document.getElementById(id);
        m.style.display = (m.style.display === 'flex') ? 'none' : 'flex';
    }

    function zoomImage(src) {
        // Crear un visualizador rápido
        const viewer = document.createElement('div');
        viewer.style = "position:fixed; inset:0; background:rgba(0,0,0,0.9); z-index:2000; display:flex; align-items:center; justify-content:center; cursor:zoom-out;";
        viewer.innerHTML = `<img src="${src}" style="max-width:95%; max-height:95%; border-radius:10px; box-shadow:0 0 20px rgba(0,0,0,0.5)">`;
        viewer.onclick = () => viewer.remove();
        document.body.appendChild(viewer);
    }

    function editEntry() {
        alert("Aquí se abrirá el formulario para editar nombre, fecha y categoría.");
    }
</script>

</body>
</html>
