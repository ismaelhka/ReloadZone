# ReloadZone
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

    .stats-container {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
    }

    .stat-card {
      background-color: #34495e;
      width: 48%;
      margin-bottom: 20px;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
      text-align: center;
    }

    footer {
      margin-top: 30px;
      text-align: center;
      font-size: 0.9em;
    }
  </style>
</head>
<body>

  <header>
    <h1>Plataforma de Recarga UC</h1>
    <p>Consulta estadísticas y recarga UC con facilidad.</p>
  </header>

  <div class="container">
    <!-- Sección de estadísticas -->
    <div class="section">
      <h2>Consulta las estadísticas de un jugador</h2>
      <input type="text" id="playerId" class="input-field" placeholder="Ingresa el ID del jugador">
      <button class="button" onclick="getPlayerStats()">Consultar Estadísticas</button>
      <div id="playerStats" class="stats-container"></div>
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
  </div>

  <footer>
    <p>&copy; 2025 Plataforma de Recarga UC. Todos los derechos reservados.</p>
  </footer>

  <script>
    const apiKey = 'TU_API_KEY_AQUI';  // Sustituye con tu API Key real de PUBG

    // Función para obtener estadísticas del jugador
    function getPlayerStats() {
      const playerId = document.getElementById('playerId').value;
      if (!playerId) {
        alert("Por favor, ingresa un ID de jugador.");
        return;
      }

      fetch(`https://api.pubg.com/shards/mobile/players/${playerId}/seasons`, {
        method: 'GET',
        headers: {
          'Authorization': `Bearer ${apiKey}`,
          'Accept': 'application/vnd.api+json',
        }
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('No se encontraron estadísticas para este jugador.');
        }
        return response.json();
      })
      .then(data => {
        console.log(data);  // Imprime los datos para depuración
        if (data && data.data && data.data[0]) {
          const stats = data.data[0].attributes.stats;
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
          `;
          document.getElementById('playerStats').innerHTML = statsHtml;
        } else {
          document.getElementById('playerStats').innerHTML = '<p>No se encontraron estadísticas.</p>';
        }
      })
      .catch(error => {
        document.getElementById('playerStats').innerHTML = `<p>Error: ${error.message}</p>`;
      });
    }

    // Función para recargar UC
    function rechargeUC() {
      const ucAmount = document.getElementById('ucAmount').value;
      if (!ucAmount || ucAmount <= 0) {
        alert("Por favor, ingresa una cantidad válida de UC.");
        return;
      }

      // Aquí simula el proceso de recarga y muestra un mensaje
      // Si tienes un backend que maneje las recargas, este es el lugar para integrarlo.
      document.getElementById('rechargeStatus').innerHTML = `<p>Recarga exitosa. Has recibido ${ucAmount} UC.</p>`;
      
      // Simulando el pago con PayPal (puedes agregar lógica de pago real más tarde)
      console.log(`Recarga de ${ucAmount} UC completada.`); // Solo para depuración
    }
  </script>

</body>
</html>
