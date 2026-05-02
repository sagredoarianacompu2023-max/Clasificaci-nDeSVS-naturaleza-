<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA DE ARCHIVO C.N</title>
    <style>
        :root {
            --bg: #050805;
            --terminal: #4af626;
            --dark-green: #142514;
            --text-dim: #2a4d27;
        }

        body {
            background-color: var(--bg);
            color: var(--terminal);
            font-family: 'Courier New', Courier, monospace;
            margin: 0;
            padding: 20px;
            line-height: 1.4;
        }

        /* Cabecera */
        header {
            border-bottom: 2px solid var(--terminal);
            margin-bottom: 30px;
            padding-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Panel de Administración (Oculto por defecto) */
        #adminPanel {
            background: var(--dark-green);
            padding: 25px;
            border: 1px solid var(--terminal);
            margin-bottom: 40px;
            display: none;
            box-shadow: 0 0 15px rgba(74, 246, 38, 0.2);
        }

        input[type="file"], input[type="text"], textarea {
            background: #000;
            border: 1px solid var(--terminal);
            color: var(--terminal);
            padding: 12px;
            margin: 10px 0;
            width: 100%;
            box-sizing: border-box;
            font-family: inherit;
        }

        button {
            background: var(--terminal);
            color: #000;
            border: none;
            padding: 12px 24px;
            font-weight: bold;
            cursor: pointer;
            text-transform: uppercase;
        }

        button:hover { background: #fff; }

        /* Galería */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 40px;
        }

        .card {
            border: 1px solid var(--text-dim);
            padding: 15px;
            background: rgba(20, 37, 20, 0.3);
            transition: 0.3s;
        }

        .card:hover { border-color: var(--terminal); }

        .card img {
            width: 100%;
            height: auto;
            max-height: 400px;
            object-fit: cover;
            border: 1px solid var(--text-dim);
            filter: grayscale(100%) brightness(0.8) sepia(100%) hue-rotate(60deg);
            cursor: pointer;
        }

        .card img:hover { filter: none; brightness(1); }

        .card-date {
            font-size: 0.7rem;
            color: var(--text-dim);
            margin-top: 10px;
            text-align: right;
        }

        .card-text {
            margin-top: 15px;
            white-space: pre-wrap; /* Mantiene los saltos de línea del texto */
        }

        .admin-controls {
            margin-top: 20px;
            border-top: 1px dashed var(--text-dim);
            padding-top: 10px;
            display: none;
        }

        .btn-action {
            background: none;
            border: 1px solid var(--terminal);
            color: var(--terminal);
            padding: 5px 10px;
            font-size: 0.7rem;
            margin-right: 5px;
        }

        .btn-delete { border-color: #f00; color: #f00; }

        /* Efecto CRT opcional */
        .scanline {
            width: 100%; height: 100%;
            position: fixed; top: 0; left: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.1) 50%);
            background-size: 100% 4px;
            z-index: 100; pointer-events: none;
        }
    </style>
</head>
<body>

<div class="scanline"></div>

<header>
    <div>
        <h1 style="margin:0">> ARCHIVO_PERSONAL_CN</h1>
        <small id="status">MODO: LECTURA_PUBLICA</small>
    </div>
    <button id="loginBtn" style="font-size: 0.7rem; background:none; border:1px solid var(--terminal); color:var(--terminal)">ACCESO ADMIN</button>
</header>

<div id="adminPanel">
    <h3>[+] NUEVA ENTRADA DE TEXTO Y FOTO</h3>
    <input type="file" id="fotoFile" accept="image/*">
    <input type="text" id="titulo" placeholder="TÍTULO O ETIQUETA RÁPIDA">
    <textarea id="descripcion" rows="5" placeholder="ESCRIBE AQUÍ TU TEXTO PERSONALIZADO..."></textarea>
    <button onclick="publicar()">GUARDAR EN ARCHIVO</button>
</div>

<div class="grid" id="galeria">
    </div>

<script>
    let esAdmin = false;

    // Login Simple (Simulado)
    document.getElementById('loginBtn').onclick = () => {
        const pass = prompt("IDENTIFICACIÓN REQUERIDA:");
        if(pass === "CN2026") { 
            esAdmin = true;
            document.getElementById('adminPanel').style.display = 'block';
            document.getElementById('status').innerText = "MODO: EDICIÓN_TOTAL";
            document.querySelectorAll('.admin-controls').forEach(el => el.style.display = 'block');
            alert("SISTEMA DESBLOQUEADO");
        }
    };

    function publicar() {
        const titulo = document.getElementById('titulo').value;
        const texto = document.getElementById('descripcion').value;
        const galeria = document.getElementById('galeria');
        const fecha = new Date().toLocaleDateString();

        // Estructura de la publicación
        const nuevaEntrada = `
            <div class="card">
                <img src="https://via.placeholder.com/400x300/142514/4af626?text=FOTO_REGISTRADA" onclick="window.open(this.src)">
                <div class="card-date">${fecha}</div>
                <h3 style="margin: 5px 0">${titulo.toUpperCase()}</h3>
                <div class="card-text">${texto}</div>
                <div class="admin-controls" style="${esAdmin ? 'display:block' : 'display:none'}">
                    <button class="btn-action" onclick="editar(this)">MODIFICAR</button>
                    <button class="btn-action btn-delete" onclick="borrar(this)">ELIMINAR</button>
                </div>
            </div>
        `;
        
        galeria.insertAdjacentHTML('afterbegin', nuevaEntrada);
        
        // Limpiar campos
        document.getElementById('titulo').value = "";
        document.getElementById('descripcion').value = "";
    }

    function borrar(btn) {
        if(confirm("¿CONFIRMAR ELIMINACIÓN?")) {
            btn.closest('.card').remove();
        }
    }

    function editar(btn) {
        alert("Función para abrir el cuadro de edición con el texto actual.");
    }
</script>

</body>
</html>


