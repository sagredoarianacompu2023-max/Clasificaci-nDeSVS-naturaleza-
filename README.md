
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Mi colección natural 🌿</title>

<style>
body {
  font-family: Arial;
  background: #eef5ec;
  text-align: center;
}

h1 {
  margin-top: 20px;
}

/* Formulario */
.formulario {
  background: white;
  padding: 15px;
  margin: 20px auto;
  width: 300px;
  border-radius: 10px;
}

input, select, button {
  margin: 5px;
  padding: 8px;
  width: 90%;
}

/* Galería */
#galeria {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.tarjeta {
  background: white;
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
  width: 200px;
}

.tarjeta img {
  width: 100%;
  border-radius: 8px;
}

.categoria {
  font-weight: bold;
  color: green;
}

.botones {
  display: flex;
  justify-content: space-around;
  margin-top: 5px;
}
</style>
</head>

<body>

<h1>🌿 Mi colección natural</h1>

<div class="formulario">
  <input type="file" id="foto" accept="image/*">
  <input type="text" id="nombre" placeholder="Nombre">

  <select id="categoria">
    <option>Hongos</option>
    <option>Plantas</option>
    <option>Insectos</option>
    <option>Reptiles</option>
    <option>Aves</option>
    <option>Mamíferos</option>
    <option>Anfibios</option>
    <option>Peces</option>
    <option>Arácnidos</option>
    <option>Moluscos</option>
    <option>Otros</option>
  </select>

  <input type="text" id="lugar" placeholder="Lugar encontrado">
  <input type="date" id="fecha">

  <button onclick="guardar()">Guardar</button>
</div>

<!-- FILTRO -->
<select onchange="filtrar(this.value)">
  <option value="Todos">Mostrar todos</option>
  <option>Hongos</option>
  <option>Plantas</option>
  <option>Insectos</option>
  <option>Reptiles</option>
  <option>Aves</option>
  <option>Mamíferos</option>
  <option>Anfibios</option>
  <option>Peces</option>
  <option>Arácnidos</option>
  <option>Moluscos</option>
</select>

<div id="galeria"></div>

<script>
let datos = JSON.parse(localStorage.getItem("coleccion")) || [];
let editandoIndex = null;

function guardar() {
  let archivo = document.getElementById("foto").files[0];
  let nombre = document.getElementById("nombre").value;
  let categoria = document.getElementById("categoria").value;
  let lugar = document.getElementById("lugar").value;
  let fecha = document.getElementById("fecha").value;

  if (editandoIndex !== null) {
    datos[editandoIndex].nombre = nombre;
    datos[editandoIndex].categoria = categoria;
    datos[editandoIndex].lugar = lugar;
    datos[editandoIndex].fecha = fecha;
    editandoIndex = null;
    actualizar();
    return;
  }

  if (!archivo) return alert("Subí una imagen");

  let reader = new FileReader();

  reader.onload = function(e) {
    let nuevo = {
      imagen: e.target.result,
      nombre,
      categoria,
      lugar,
      fecha
    };

    datos.push(nuevo);
    actualizar();
  };

  reader.readAsDataURL(archivo);
}

function actualizar() {
  localStorage.setItem("coleccion", JSON.stringify(datos));
  mostrar(datos);
}

function mostrar(lista) {
  let galeria = document.getElementById("galeria");
  galeria.innerHTML = "";

  lista.forEach((item, index) => {
    let div = document.createElement("div");
    div.className = "tarjeta";

    div.innerHTML = `
      <img src="${item.imagen}">
      <p><b>${item.nombre}</b></p>
      <p class="categoria">${item.categoria}</p>
      <p>📍 ${item.lugar}</p>
      <p>📅 ${item.fecha}</p>

      <div class="botones">
        <button onclick="editar(${index})">✏️</button>
        <button onclick="borrar(${index})">🗑️</button>
      </div>
    `;

    galeria.appendChild(div);
  });
}

function borrar(index) {
  if (confirm("¿Eliminar esta foto?")) {
    datos.splice(index, 1);
    actualizar();
  }
}

function editar(index) {
  let item = datos[index];

  document.getElementById("nombre").value = item.nombre;
  document.getElementById("categoria").value = item.categoria;
  document.getElementById("lugar").value = item.lugar;
  document.getElementById("fecha").value = item.fecha;

  editandoIndex = index;
}

function filtrar(cat) {
  if (cat === "Todos") {
    mostrar(datos);
  } else {
    let filtrados = datos.filter(d => d.categoria === cat);
    mostrar(filtrados);
  }
}

mostrar(datos);
</script>

</body>
</html>
