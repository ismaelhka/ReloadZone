<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plataforma de Estadísticas PUBG y Recarga UC</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #1e2a3b;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      height: 100vh;
      margin: 0;
    }
    .container {
      text-align: center;
      margin-top: 20px;
    }
    input, button, select {
      padding: 10px;
      font-size: 1rem;
      margin-top: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
      background-color: #333;
      color: white;
    }
    button:hover {
      background-color: #555;
    }
    #playerStats {
      margin-top: 20px;
      display: none;
    }
    #ucMessage {
      margin-top: 20px;
      font-size: 1.2rem;
      color: #f1c40f;
    }
    #ucContainer {
      margin-top: 30px;
      text-align: center;
    }
    #ucMessage {
      margin-top: 20px;
      font-size: 1.2rem;
      color: #f1c40f;
    }
  </style>
</head>
<body>

  <h1>Consulta de Estadísticas PUBG y Recarga UC</h1>

  <div class="container">
    <input type="text" id="playerId" placeholder="Ingresa el ID del Jugador">
    <button onclick="getPlayerStats()">Ver Estadísticas</button>
  </div>

  <div id="playerStats">
    <h2>Estadísticas del Jugador</h2>
    <div id="statsContainer"></div>
  </div>

  <div id="ucContainer">
    <h2>Recargar UC</h2>
    <select id="ucOptions">
      <option value="60">60 UC</option>
      <option value="150">150 UC</option>
      <option value="300">300 UC</option>
      <option value="600">600 UC</option>
    </select>
    <button onclick="addUC()">Recargar UC</button>
    <p id="ucMessage"></p>
  </div>

  <script>
    // API Key actualizada
    const apiKey = 'eyJ0eXAi0iJKV1QiLCJhbGciOiJlUzI1NiJ9.eyJqdGkiOiJlNJRINGJIMC04ZDk5LTAxM2UtM2Y2ZS01MjQxZDA2Y2U0NDgiLCJpc3MiOiJnYW1lbG9ja2VyliwiaWF0ljoxNzYwNzE0MDY4LCJwdWliOiJibHVlaG9sZSIsInRpdGxlljoicHViZylslmFwcCI6InB1YmctcG93ZXJodWlifQ.5bCTMcKacNmQbyuaPX-8GyXQx2y-uyfUt40JuRgEVQ';

    // Función para obtener las estadísticas del jugador
    function getPlayerStats() {
      const playerId = document.getElementById('playerId').value;
      if (!playerId) {
        alert('Por favor, ingresa un ID de jugador válido.');
        return;
      }

      fetch(`https://api.pubg.com/shards/mobile/players/${playerId}/seasons`, {
        method: 'GET',
        headers: {
          'Authorization': `Bearer ${apiKey}`,  // Usando la nueva API Key
          'Accept': 'application/vnd.api+json'
        }
      })
      .then(response => response.json())
      .then(data => {
        if (data && data.data && data.data.length > 0) {
          const stats = data.data[0].attributes.stats;
          // Mostrar estadísticas del jugador
          document.getElementById('statsContainer').innerHTML = `
            <p>Kills: ${stats.kills}</p>
            <p>Victorias: ${stats.wins}</p>
            <p>Partidas Jugadas: ${stats.matches}</p>
            <p>Daño Infligido: ${stats.damageDealt}</p>
          `;
          // Mostrar la sección de estadísticas
          document.getElementById('playerStats').style.display = 'block';
          // Desplazarse a la sección de estadísticas
          window.location.hash = "playerStats";
        } else {
          alert('No se encontraron estadísticas para este jugador.');
        }
      })
      .catch(error => {
        console.error('Error al obtener estadísticas:', error);
        alert('Hubo un error al obtener las estadísticas.');
      });
    }

    // Función para simular la recarga de UC
    function addUC() {
      const ucAmount = document.getElementById('ucOptions').value; // Obtener la cantidad de UC seleccionada
      document.getElementById('ucMessage').innerText = `¡Has recibido ${ucAmount} UC!`;
    }
  </script>

</body>
</html>
