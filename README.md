<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tabla desde Google Sheets</title>
</head>
<body>

  <label for="opciones">Selecciona una opción:</label>
  <select id="opciones"></select>

  <table id="miTabla"></table>

  <script>
 function getDataJSONP(callback) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Ingredientes_Public');
  var data = sheet.getRange('A:E').getValues(); // Obtener datos de las columnas A a E
  var jsonData = JSON.stringify(data);

  // Retorna el resultado como una llamada a la función de retorno (callback)
  return ContentService.createTextOutput(callback + "(" + jsonData + ");")
      .setMimeType(ContentService.MimeType.JAVASCRIPT);
}
    function init() {
      // Reemplaza 'CLAVE_DE_TU_HOJA' con la clave real de tu hoja de cálculo
      var public_spreadsheet_url = 'https://script.google.com/macros/s/AKfycbxowfxTyD3W_uUm_wKtlPrmI0x0kGcJo_ebRRibo1TAiSKiqTjTEBonLVkNplLX7SQ4/exec?callback=nombreDeTuFuncion';

      // Genera una función de retorno única
      var callbackName = 'jsonpCallback' + new Date().getTime();
      window[callbackName] = function(data) {
        mostrarDatos(data);
        delete window[callbackName]; // Limpia la función de retorno después de usarla
      };

      // Agrega el script al DOM para realizar la solicitud JSONP
      var script = document.createElement('script');
      script.src = public_spreadsheet_url + '?callback=' + callbackName;
      document.head.appendChild(script);
    }

    function mostrarDatos(data) {
      // 'data' contiene tus datos de hoja de cálculo
      console.log(data);

      // Ahora puedes manipular los datos como desees, por ejemplo, construir una tabla HTML
      var table = document.getElementById('miTabla');
      data.forEach(function (item) {
        var row = table.insertRow();
        var cell1 = row.insertCell(0);
        var cell2 = row.insertCell(1);
        cell1.innerText = item.Nombre;
        cell2.innerText = item.Precio;
        // Agrega más celdas según tus columnas
      });

      // Rellenar el desplegable con las opciones de la lista
      var dropdown = document.getElementById('opciones');
      data.forEach(function (item) {
        var option = document.createElement('option');
        option.value = item.Nombre;
        option.innerText = item.Nombre;
        dropdown.appendChild(option);
      });
    }

    window.onload = function () {
      init();
    };
  </script>

</body>
</html>
