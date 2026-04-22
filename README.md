
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
  <input
