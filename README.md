// Define una variable para almacenar la selección
    let selectedUC = {
        uc: null,
        precio: null
    };

    // LÓGICA PARA SELECCIONAR LA TARJETA DE UC
    document.querySelectorAll('.uc-card').forEach(card => {
        card.addEventListener('click', function() {
            // Quitar la clase 'seleccionado' de todas las tarjetas
            document.querySelectorAll('.uc-card').forEach(c => c.classList.remove('seleccionado'));
            
            // Añadir la clase 'seleccionado' a la tarjeta clickeada
            this.classList.add('seleccionado');

            // Actualizar la variable de selección
            selectedUC.uc = this.getAttribute('data-uc');
            selectedUC.precio = this.querySelector('.precio').textContent;

            console.log(`Seleccionaste ${selectedUC.uc} UC por ${selectedUC.precio}`);
        });
    });

    // FUNCIÓN SIMULADA PARA BUSCAR JUGADOR (Mantenemos el token de ejemplo)
    const PUBG_API_TOKEN = "eyJ0eXAi0iJKV1QiLCJhbGciOiJlUzI1NiJ9.eyJqdGkiOiJl NJRINGJIMC04ZDk5LTAxM2UtM2Y2ZS01MjQxZDA2Y2U 0NDgiLCJpc3MiOiJnYW1lbG9ja2VyliwiaWF0ljoxNzYwN zE0MDY4LCJwdWliOiJibHVlaG9sZSIsInRpdGxlIjoi cHVicylslmFwcCI6InB1YmctcG93ZXJodWlifQ.5bCTMcKacNm QbyuaPX-8GyXQx2y-uyfUt40JuR";
    
    function mostrarDatosJugador() {
        const playerId = document.getElementById('player-id').value.trim();
        const resultadoDiv = document.getElementById('resultado-jugador');
        const playerName = document.getElementById('player-name');
        const playerLevel = document.getElementById('player-level');

        if (playerId.length < 5) {
            alert("Por favor, ingresa un ID de jugador válido.");
            resultadoDiv.style.display = 'none';
            return;
        }

        // SIMULACIÓN DEL RESULTADO
        const nombreSimulado = "ProGamer" + playerId.substring(playerId.length - 4);
        const nivelSimulado = Math.floor(Math.random() * 80) + 10; 
        
        playerName.textContent = nombreSimulado;
        playerLevel.textContent = nivelSimulado;
        resultadoDiv.style.display = 'block';

        document.getElementById('recarga-opciones').scrollIntoView({ behavior: 'smooth' });
    }

    // --- NUEVA FUNCIÓN PARA PROCESAR EL PAGO (MÉTODO POST SIMULADO) ---
    function procesarPago() {
        const playerId = document.getElementById('player-id').value.trim();
        const playerName = document.getElementById('player-name').textContent;

        if (!playerId || document.getElementById('resultado-jugador').style.display === 'none') {
            alert("⚠️ Primero debes verificar tu ID de jugador.");
            document.getElementById('jugador-info').scrollIntoView({ behavior: 'smooth' });
            return;
        }

        if (!selectedUC.uc) {
            alert("⚠️ Debes seleccionar un paquete de UC para continuar.");
            document.getElementById('recarga-opciones').scrollIntoView({ behavior: 'smooth' });
            return;
        }

        // 1. PREPARAR EL CUERPO (BODY) DE LA PETICIÓN POST
        const bodyData = {
            player_id: playerId,
            player_name: playerName,
            uc_amount: selectedUC.uc,
            price: selectedUC.precio,
            currency: 'USD',
            payment_method: 'PENDING'
        };

        // 2. SIMULAR LA LLAMADA POST
        console.log("--- SIMULANDO PETICIÓN POST DE PAGO ---");
        console.log("URL de Pago Simulada: https://paymentgateway.com/process");
        console.log("Cuerpo (Body) Enviado:");
        console.log(JSON.stringify(bodyData, null, 2));

        // Aquí iría el código real de 'fetch' con el método POST
        /*
        fetch('https://paymentgateway.com/process', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                // 'Authorization': `Bearer ${PUBG_API_TOKEN}` // O un token de pago
            },
            body: JSON.stringify(bodyData)
        })
        .then(response => response.json())
        .then(data => {
            // Si el backend responde, lo redirigimos a la página de éxito/pago
            window.location.href = `pagina_de_pago.html?order_id=${data.orderId}`;
        })
        .catch(error => console.error('Error en el pago:', error));
        */


        // 3. SIMULAR LA REDIRECCIÓN A LA SECCIÓN DE PAGO FINAL
        alert(`✅ Datos listos. Redirigiendo a la pasarela de pago para ${selectedUC.uc} UC a ${playerName}.`);
        
        // Redirigir la página a una URL de ejemplo (Usando GET para la redirección, como es lo estándar)
        window.location.href = `pago_final.html?player=${playerId}&uc=${selectedUC.uc}&amount=${selectedUC.precio}`;
    }
