
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Receta</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    table {
      width: 100%;
      margin-bottom: 20px;
    }
    table, th, td {
      border: 1px solid #ddd;
    }
    th, td {
      padding: 10px;
      text-align: left;
    }
    th {
      font-weight: bold;
      border: none;
      background: none;
    }
    textarea, input {
      font-family: Arial, sans-serif;
      border: none;
      background: none;
      outline: none;
      font-size: 1em;
      margin-top: 10px;
    }
    textarea {
      resize: none;
      font-size: 1.5em;
      font-weight: bold;
      width: 100%;
      
    }
    .icon-pencil, .icon-trash {
      width: 16px;
      height: 20px;
      cursor: pointer;
      display: inline-block;
      vertical-align: middle;
    }
    .icon-pencil {
      background: url('https://www.svgrepo.com/show/359598/pencil.svg') center/contain no-repeat;
    }
    .icon-trash {
      background: url('https://www.svgrepo.com/show/359065/remove.svg') center/contain no-repeat;
    }
    button {
      border: none;
      padding: 10px;
      background-color: #3498db;
      color: #fff;
      cursor: pointer;
      font-size:14px;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>Detalles de la Receta</h1>

  <!-- Tabla con la información de la receta -->
  <table id="tablaReceta">
    <tr>
      <th>Nombre de la receta</th>
      <th>Índice de proteína</th>
      <th>Proteínas</th>
      <th>Índice de grasas</th>
      <th>Grasas</th>
      <th>Índice de Carbh.</th>
      <th>Carbh</th>
      <th>Kcal</th>
      <th>Ingredientes</th>
    </tr>
    <tr>
      <td id="tituloReceta" contenteditable="false">Ensalada de espinacas y queso de cabra</td>
      <td contenteditable="false">1,0</td>
      <td contenteditable="false">29,3</td>
      <td contenteditable="false">1,1</td>
      <td contenteditable="false">27,5</td>
      <td contenteditable="false">0,9</td>
      <td contenteditable="false">42,2</td>
      <td contenteditable="false">522,1</td>
      <td id="ingredientesReceta" contenteditable="false">150g Queso de cabra, 30g espinacas, 100g patatas cocidas, 10g nueces, 5g Aceite de Oliva Virgen Extra, 1 Tomate, 0,5 Manzana, 10g Vinagre Balsámico de Módena</td>
    </tr>
  </table>

  <!-- Área de edición -->
 <div id="edicionTitulo">
  <textarea id="tituloEditable" style="width: 90%; overflow: hidden;text-overflow: ellipsis; box-sizing: border-box; resize: none; font-size: 1.5em; font-weight: bold;" oninput="ajustarAltura(this)"></textarea>
</div>

  <!-- Cantidades e Ingredientes -->
  <div id="edicionIngredientes">
    <!-- Aquí se añadirán los campos de edición dinámicamente -->
  </div>

  <!-- Botón de agregar campo de edición -->
  <button onclick="agregarCampoEdicion()">Agregar Ingrediente</button>

  <!-- Botón de guardar -->
  <button onclick="guardarEdicion()">Guardar</button>
  
  
  <!-- Botón de guardar nueva receta -->
<button onclick="limpiarDatosTituloIngredientes()">Limpiar Ingredientes</button>

  <script>
    function ajustarAltura(elemento) {
  elemento.style.height = 'auto';
  elemento.style.height = (elemento.scrollHeight) + 'px';
}
  
    // Llenar el textarea con el valor inicial de la tabla
    const tituloReceta = document.getElementById('tituloReceta');
    const tituloEditable = document.getElementById('tituloEditable');
    tituloEditable.value = tituloReceta.innerText;

    // Obtener las cantidades e ingredientes de la columna 9
    const ingredientesReceta = document.getElementById('ingredientesReceta');
    const cantidadesIngredientes = ingredientesReceta.innerText.split(', ');

    // Llenar la sección de edición con las cantidades e ingredientes
  const seccionIngredientes = document.getElementById('edicionIngredientes');
  cantidadesIngredientes.forEach((item, index) => {
    const [cantidad, ...resto] = item.split(' ');
    const ingrediente = resto.join(' '); // Unir las palabras del ingrediente
    agregarCampoEdicion(cantidad, ingrediente);
    });

    function hacerEditable(id) {
      const elemento = document.getElementById(id);
      elemento.removeAttribute('readonly');
      elemento.focus();
    }

    

    function guardar(elemento, columna) {
      // Aquí puedes escribir el código para guardar el dato editado.
      if (columna === 'titulo') {
        const tituloReceta = document.getElementById('tituloReceta');
        tituloReceta.innerText = elemento.value;
      } else if (columna === 'ingredientes') {
        alert(`Dato "${elemento.value}" guardado correctamente.`);
        const ingredientesReceta = document.getElementById('ingredientesReceta');
        ingredientesReceta.innerText = elemento.value;
      }
    }

    function agregarCampoEdicion(cantidad = '', ingrediente = '') {
      const nuevoCampo = document.createElement('div');
      nuevoCampo.className = 'editable';

      const inputCantidad = document.createElement('input');
      inputCantidad.type = 'text';
      inputCantidad.value = cantidad;
      inputCantidad.style.width = '15%'; // Estilo específico para cantidades

      const inputIngrediente = document.createElement('input');
      inputIngrediente.type = 'text';
      inputIngrediente.value = ingrediente;
      inputIngrediente.style.width = '75%'; // Estilo específico para ingredientes

      const iconPencil = document.createElement('div');
      iconPencil.className = 'icon-pencil';
      iconPencil.onclick = function () {
        hacerEditable(inputCantidad.id);
        hacerEditable(inputIngrediente.id);
      };

      const iconTrash = document.createElement('div');
      iconTrash.className = 'icon-trash';
      iconTrash.onclick = function () {
        seccionIngredientes.removeChild(nuevoCampo);
      };

      nuevoCampo.appendChild(iconTrash);
      nuevoCampo.appendChild(inputCantidad);
      nuevoCampo.appendChild(inputIngrediente);
      
      

      seccionIngredientes.appendChild(nuevoCampo);

      
    }

   function guardarEdicion() {
  // Obtener la tabla
  const tablaReceta = document.getElementById('tablaReceta');

  // Obtener el título editable y la cadena de cantidades e ingredientes
  const tituloEditable = document.getElementById('tituloEditable').value.trim();
  let cantidadesIngredientes = '';

  const camposEdicion = document.querySelectorAll('.editable');
  camposEdicion.forEach(campo => {
    const inputCantidad = campo.querySelector('input:nth-child(2)');
    const inputIngrediente = campo.querySelector('input:nth-child(3)');

    if (inputCantidad.value.trim() !== '' && inputIngrediente.value.trim() !== '') {
      cantidadesIngredientes += `${inputCantidad.value.trim()} ${inputIngrediente.value.trim()}, `;
    }
  });

  // Eliminar la última coma y espacio
  cantidadesIngredientes = cantidadesIngredientes.slice(0, -2);

  // Buscar la fila correspondiente al título editable
  let filaEncontrada = null;
  for (let i = 1; i < tablaReceta.rows.length; i++) {
    const tituloFila = tablaReceta.rows[i].cells[0].innerText.trim();
    if (tituloFila === tituloEditable) {
      filaEncontrada = tablaReceta.rows[i];
      break;
    }
  }

  // Si se encuentra la fila, actualizar los datos
  if (filaEncontrada) {
    filaEncontrada.cells[0].innerText = tituloEditable; // Actualizar título

    // Actualizar la cadena de cantidades e ingredientes en la última celda
    filaEncontrada.cells[8].innerText = cantidadesIngredientes;
  } else {
    // Si no se encuentra la fila, mostrar un cuadro de diálogo
if (!filaEncontrada) {
  const confirmacion = window.confirm('¿Desea guardar una nueva receta?');
  if (confirmacion) {
    // Si el usuario elige guardar nueva receta, llamar a la función correspondiente
    guardarNuevaReceta();
    return; // Salir de la función para evitar la limpieza del área de edición
  } else {
    // Si el usuario elige cancelar, puedes realizar alguna acción adicional o simplemente salir
    return;
  }
}
  }

  // Si se encuentra la fila, preguntar si se desea sobrescribir la receta
if (filaEncontrada) {
  const confirmacion = window.confirm('¿Desea sobrescribir la receta existente con este título?');
  if (confirmacion) {
    // Si el usuario elige sobrescribir, actualizar los datos
    filaEncontrada.cells[0].innerText = tituloEditable; // Actualizar título
    filaEncontrada.cells[8].innerText = cantidadesIngredientes; // Actualizar cantidades e ingredientes
  } else {
    // Si el usuario elige no sobrescribir, abortar la función
    return;
  }
}
}

function limpiarDatosTituloIngredientes() {
   // Limpiar el área de edición del título y los ingredientes
  document.getElementById('tituloEditable').value = 'Escribe aquí el nombre de tu receta...';
  document.getElementById('ingredientesReceta').value = '';

  // Limpiar los campos editables individuales
  const camposEdicion = document.querySelectorAll('.editable');
  camposEdicion.forEach(campo => {
    const inputCantidad = campo.querySelector('input:nth-child(2)');
    const inputIngrediente = campo.querySelector('input:nth-child(3)');

    inputCantidad.value = '';
    inputIngrediente.value = '';
  });
}

function guardarNuevaReceta() {
  // Obtener la tabla
  const tablaReceta = document.getElementById('tablaReceta');

  // Crear una nueva fila
  const nuevaFila = tablaReceta.insertRow(tablaReceta.rows.length);

  // Insertar celdas en la nueva fila
  for (let i = 0; i < 9; i++) {
    const nuevaCelda = nuevaFila.insertCell(i);

    if (i === 0) {
      // Si es la primera celda (título), tomar el valor del campo de edición
      nuevaCelda.innerText = document.getElementById('tituloEditable').value;
    } else if (i === 8) {
      // Si es la última celda (ingredientes), construir la cadena de cantidades e ingredientes
      let cantidadesIngredientes = '';
      const camposEdicion = document.querySelectorAll('.editable');

      camposEdicion.forEach(campo => {
        const inputCantidad = campo.querySelector('input:nth-child(2)');
        const inputIngrediente = campo.querySelector('input:nth-child(3)');

        if (inputCantidad.value.trim() !== '' && inputIngrediente.value.trim() !== '') {
          cantidadesIngredientes += `${inputCantidad.value.trim()} ${inputIngrediente.value.trim()}, `;
        }
      });

      // Eliminar la última coma y espacio
      cantidadesIngredientes = cantidadesIngredientes.slice(0, -2);

      // Asignar la cadena de cantidades e ingredientes a la nueva celda
      nuevaCelda.innerText = cantidadesIngredientes;
    } 
  }

 

 
 
}



  </script>
</body>
</html>
