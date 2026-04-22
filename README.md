# Clasificaci-nDeSVS-naturaleza-

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Mi colección de insectos 🐛</title>

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
  width: 180px;
}

.tarjeta img {
  width: 100%;
  border-radius: 8px;
}

.categoria {
  font-weight: bold;
  color: green;
}
</style>
</head>

<body>

<h1>🐛 Mi colección de insectos</h1>

<div class="formulario">
  <input type="file" id="foto" accept="image/*">
  
  <input type="text" id="nombre" placeholder="Nombre del insecto">

  <select id="categoria">
    <option value="Arañas">Arañas</option>
    <option value="Escarabajos">Escarabajos</option>
    <option value="Mariposas">Mariposas</option>
    <option value="Otros">Otros</option>
  </select>

  <button onclick="guardar()">Guardar</button>
</div>

<div id="galeria"></div>

<script>
let datos = JSON.parse(localStorage.getItem("insectos")) || [];

function guardar() {
  let archivo = document.getElementById("foto").files[0];
  let nombre = document.getElementById("nombre").value;
  let categoria = document.getElementById("categoria").value;

  if (!archivo) return alert("Subí una imagen");

  let reader = new FileReader();

  reader.onload = function(e) {
    let nuevo = {
      imagen: e.target.result,
      nombre: nombre,
      categoria: categoria
    };

    datos.push(nuevo);
    localStorage.setItem("insectos", JSON.stringify(datos));

    mostrar();
  };

  reader.readAsDataURL(archivo);
}

function mostrar() {
  let galeria = document.getElementById("galeria");
  galeria.innerHTML = "";

  datos.forEach(insecto => {
    let div = document.createElement("div");
    div.className = "tarjeta";

    div.innerHTML = `
      <img src="${insecto.imagen}">
      <p>${insecto.nombre}</p>
      <p class="categoria">${insecto.categoria}</p>
    `;

    galeria.appendChild(div);
  });
}

mostrar();
</script>

</body>
</html>
