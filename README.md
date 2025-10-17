 ReloadZone
 <!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plataforma de Recarga UC</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    /* Estilos generales */
    body {
      font-family: 'Roboto', sans-serif;
      background-color: #0a192f;
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }

    header {
      text-align: center;
      padding: 20px;
      margin-bottom: 40px;
    }

    h1 {
      font-size: 2.5em;
      color: #1abc9c;
    }

    .container {
      width: 80%;
      max-width: 1100px;
      background-color: #1d2a40;
      border-radius: 12px;
      padding: 30px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
    }

    .section {
      margin-bottom: 40px;
    }

    .section h2 {
      font-size: 1.8em;
      margin-bottom: 20px;
    }

    .input-field, .button {
      background-color: #34495e;
      border-radius: 8px;
      padding: 12px;
      width: 100%;
      margin-bottom: 20px;
      border: none;
      color: #fff;
      font-size: 1em;
      outline: none;
    }

    .button {
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .button:hover {
      background-color: #16a085;
    }

    .card {
      background-color: #2c3e50;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 15px rgba(0, 0, 0, 0.3);
    }

    footer {
      margin-top: 30px;
      text-align: center;
      font-size: 0.9em;
    }

    .message {
      margin-top: 20px;
      color: #2ecc71;
      font-size: 1.2em;
    }

    /* Nueva sección para mostrar las estadísticas */
    #statsSection {
      display: none;
      margin-top: 40px;
    }

    .stat-card {
      background-color: #34495e;
      padding: 20px;
      border-radius: 8px;
      margin-bottom: 20px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    }

    .stat-card h3 {
      font-size: 1.2em;
      margin-bottom: 10px;
    }

    .stat-card p {
      font-size: 1.4em;
    }

  </style>
</head>
<body>

  <header>
    <h1>Plataforma de Recarga UC</h1>
    <p>Consulta estadísticas y recarga UC con facilidad.</p>
  </header>

  <div class="container">
    <!-- Sección de consulta de estadísticas -->
    <div class="section">
      <h2>Consulta las estadísticas de un jugador</h2>
      <input type="text" id="playerId" class="input-field" placeholder="Ingresa el ID del jugador">
      <button class="button" onclick="getPlayerStats()">Consultar Estadísticas</button>
    </div>

    <!-- Sección de recarga de UC -->
    <div class="section">
      <h2>Recarga UC</h2>
      <div class="card">
        <input type="number" id="ucAmount" class="input-field" placeholder="Cantidad de UC" min="1">
        <button class="button" onclick="rechargeUC()">Recargar UC</button>
        <div id="rechargeStatus"></div>
      </div>
    </div>

    <!-- Sección donde se mostrarán las estadísticas del jugador -->
    <div id="statsSection">
      <h2>Estadísticas del Jugador</h2>
      <div id="playerStats"></div>
      <button class="button" onclick="goBack()">Volver</button>
    </div>
  </div>

  <footer>
    <p>&copy; 2025 Plataforma de Recarga UC. Todos los derechos reservados.</p>
  </footer>

  <script>
    const apiKey = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJlNjRlNGJlMC04ZDk5LTAxM2UtM2Y2ZS01MjQxZDA2Y2U0NDgiLCJpc3MiOiJnYW1lbG9ja2VyIiwiaWF0IjoxNzYwNzE0MDY4LCJwdWIiOiJibHVlaG9sZSIsInRpdGxlIjoicHViZyIsImFwcCI6InB1YmctcG93ZXJodWIifQ.5bCTMcKacNmQbyuaPX-8GyXQHx2y-uyfUt40JuRgEVQ';  // Tu clave API de PUBG

    // Función para obtener estadísticas del jugador
    function getPlayerStats() {
      const playerId = document.getElementById('playerId').value;
      if (!playerId) {
        alert("Por favor, ingresa un ID de jugador.");
        return;
      }

      // Realiza la solicitud a la API de PUBG
      fetch(`https://api.pubg.com/shards/mobile/players/${playerId}/seasons`, {
        method: 'GET',
        headers: {
          'Authorization': `Bearer ${apiKey}`,
          'Accept': 'application/vnd.api+json',
        }
      })
      .then(response => {
        // Verifica si la respuesta es exitosa
        if (!response.ok) {
          throw new Error('No se encontraron estadísticas para este jugador.');
        }
        return response.json();
      })
      .then(data => {
        console.log('Datos de la API:', data);  // Ver la respuesta completa en la consola

        // Verifica si los datos están correctamente estructurados
        if (data && data.data && data.data[0]) {
          const stats = data.data[0].attributes.stats;

          // Genera el HTML para mostrar las estadísticas
          const statsHtml = `
            <div class="stat-card">
              <h3>Kills</h3>
              <p>${stats.kills}</p>
            </div>
            <div class="stat-card">
              <h3>Victorias</h3>
              <p>${stats.wins}</p>
            </div>
            <div class="stat-card">
              <h3>Partidas Jugadas</h3>
              <p>${stats.matches}</p>
            </div>
            <div class="stat-card">
              <h3>Daño Infligido</h3>
              <p>${stats.damageDealt}</p>
            </div>
          `;
          // Muestra la sección de estadísticas
          document.getElementById('playerStats').innerHTML = statsHtml;
          document.getElementById('statsSection').style.display = 'block';
          window.scrollTo(0, document.body.scrollHeight);
        } else {
          document.getElementById('playerStats').innerHTML = '<p>No se encontraron estadísticas para este jugador.</p>';
        }
      })
      .catch(error => {
        console.error('Error al obtener las estadísticas:', error);
        document.getElementById('playerStats').innerHTML = `<p>Error: ${error.message}</p>`;
      });
    }

    // Función para simular la recarga de UC
    function rechargeUC() {
      const ucAmount = document.getElementById('ucAmount').value;
      if (!ucAmount || ucAmount <= 0) {
        alert("Por favor, ingresa una cantidad válida de UC.");
        return;
      }

      // Simula la recarga y muestra un mensaje
