<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>MateVida Digital - Nivel Compras en Palí</title>
    <style>
        :root {
            --verde: #4CAF50;
            --azul: #2196F3;
            --amarillo: #FFC107;
            --naranja: #FF9800;
            --rojo: #e74c3c;
            --fondo: #e3f2fd;
        }

        body {
            margin: 0;
            background-color: var(--fondo);
            font-family: 'Arial Rounded MT Bold', sans-serif;
            text-align: center;
            -webkit-user-select: none;
            user-select: none;
        }

        /* PANTALLA DE INICIO CONSTANTE */
        #pantalla-inicio {
            position: fixed;
            top: 0; left: 0; 
            width: 100%; height: 100%;
            background: var(--fondo);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 99999 !important;
        }

        .contenedor-menu {
            background: white;
            padding: 30px;
            border-radius: 30px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 90%;
            max-width: 450px;
        }

        .btn-principal {
            background: var(--verde);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 22px;
            font-weight: bold;
            border-radius: 25px;
            cursor: pointer;
            width: 100%;
            margin-top: 15px;
            box-shadow: 0 5px 0px #388E3C;
            transition: 0.1s;
        }

        .btn-principal:active {
            transform: translateY(4px);
            box-shadow: 0 1px 0px #388E3C;
        }

        /* CONTENEDOR DEL JUEGO */
        .game-container {
            max-width: 600px;
            margin: 20px auto;
            background: white;
            padding: 20px;
            border-radius: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            width: calc(100% - 30px);
            box-sizing: border-box;
        }

        .header-status {
            background: var(--azul);
            color: white;
            padding: 12px;
            border-radius: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
            font-size: 15px;
            margin-bottom: 20px;
        }

        .avatar-box {
            background: #f5f5f5;
            border-radius: 20px;
            padding: 15px;
            margin-bottom: 20px;
            min-height: 110px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .avatar-emoticon { font-size: 50px; }
        .avatar-mensaje { margin-top: 8px; font-size: 16px; font-weight: bold; color: #333; }

        .pregunta-box {
            background: #eef7fe;
            border: 2px solid #b3e5fc;
            border-radius: 20px;
            padding: 20px;
            font-size: 18px;
            color: #1a237e;
            line-height: 1.5;
            margin-bottom: 20px;
            text-align: left;
        }

        .grid-opciones {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .btn-opcion {
            border: none;
            padding: 18px 10px;
            border-radius: 20px;
            color: #333;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 5px 0px rgba(0,0,0,0.15);
        }

        .opc-0 { background: #e8f5e9; border: 2px solid var(--verde); }
        .opc-1 { background: #e3f2fd; border: 2px solid var(--azul); }
        .opc-2 { background: #fffde7; border: 2px solid var(--amarillo); }
        .opc-3 { background: #fff3e0; border: 2px solid var(--naranja); }

        #btn-siguiente {
            display: none;
            background: var(--naranja);
            box-shadow: 0 5px 0px #E65100;
            margin-top: 20px;
        }

        #pantalla-final { display: none; }

        .tabla-resultados {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
            border-radius: 15px;
            overflow: hidden;
            background: #f9f9f9;
        }

        .tabla-resultados th, .tabla-resultados td { padding: 15px; text-align: center; font-size: 18px; }
        .tabla-resultados th { background: var(--azul); color: white; }
        .fila-dato { border-bottom: 1px solid #eee; }

        @media (max-width: 480px) {
            .grid-opciones { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

    <div id="pantalla-inicio">
        <div class="contenedor-menu">
            <span style="font-size: 65px;">🛒</span>
            <h1 style="color: var(--azul); margin: 10px 0 5px 0;">Nivel: Compras en Palí</h1>
            <p style="color: #666; margin-bottom: 25px; font-size: 16px;">¡Resuelve los desafíos del supermercado!</p>
            <button class="btn-principal" id="btn-comenzar-juego">¡Empezar Juego! 🚀</button>
        </div>
    </div>

    <div class="game-container">
        <div class="header-status">
            <span id="txt-progreso">Pregunta: 1 / 7</span>
            <span id="txt-tiempo">⏱️ Tiempo: 00:00</span>
            <span id="txt-puntos">⭐ Aciertos: 0</span>
        </div>

        <div class="avatar-box" id="caja-avatar">
            <div class="avatar-emoticon" id="avatar-cara">🤖</div>
            <div class="avatar-mensaje" id="avatar-texto">¡Hola! Analiza con cuidado cada problema.</div>
        </div>

        <div id="bloqueo-juego">
            <div class="pregunta-box" id="campo-pregunta">Cargando desafío matemático...</div>
            <div class="grid-opciones" id="contenedor-opciones"></div>
            <button id="btn-siguiente" class="btn-principal" onclick="avanzarPregunta()">Siguiente Reto ➔</button>
        </div>

        <div id="pantalla-final">
            <span style="font-size: 60px;">🏆</span>
            <h2 style="color: var(--azul);">¡Desafío Completado!</h2>
            <table class="tabla-resultados">
                <thead>
                    <tr><th colspan="2">Tus Resultados Finales</th></tr>
                </thead>
                <tbody>
                    <tr class="fila-dato">
                        <td>⏱️ <b>Tiempo Total:</b></td>
                        <td id="td-tiempo-final" style="font-weight: bold;">00:00</td>
                    </tr>
                    <tr class="fila-dato">
                        <td>📊 <b>Rendimiento:</b></td>
                        <td id="td-porcentaje-final" style="font-weight: bold; font-size: 22px; color: var(--verde);">0%</td>
                    </tr>
                </tbody>
            </table>
            <button class="btn-principal" style="margin-top:30px;" onclick="reiniciarJuego()">Volver a Intentar 🔄</button>
        </div>
    </div>

<script>
    // Audio Context seguro
    const AudioGame = {
        ctx: null,
        init() {
            if (!this.ctx) this.ctx = new (window.AudioContext || window.webkitAudioContext)();
        },
        playCorrecto() {
            this.init();
            try {
                const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
                osc.connect(gain); gain.connect(this.ctx.destination);
                osc.frequency.setValueAtTime(523.25, this.ctx.currentTime);
                osc.frequency.setValueAtTime(659.25, this.ctx.currentTime + 0.1);
                gain.gain.setValueAtTime(0.1, this.ctx.currentTime);
                osc.start(); osc.stop(this.ctx.currentTime + 0.3);
            } catch(e) {}
        },
        playError() {
            this.init();
            try {
                const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
                osc.connect(gain); gain.connect(this.ctx.destination);
                osc.frequency.setValueAtTime(150, this.ctx.currentTime);
                gain.gain.setValueAtTime(0.1, this.ctx.currentTime);
                osc.start(); osc.stop(this.ctx.currentTime + 0.2);
            } catch(e) {}
        }
    };

    const bancoProblemas = [
        {
            pregunta: "🛒 <b>Problema 1:</b> Compras una barra de mantequilla por C$35 y un paquete de galletas por C$15. ¿Cuánto debes pagar en total?",
            opciones: ["C$40", "C$45", "C$50", "C$55"],
            correcta: "C$50"
        },
        {
            pregunta: "🍫 <b>Problema 2:</b> Un chocolate cuesta C$12. Si decides comprar 4 chocolates iguales para compartir, ¿cuánto gastarás en total?",
            opciones: ["C$36", "C$48", "C$44", "C$52"],
            correcta: "C$48"
        },
        {
            pregunta: "🪙 <b>Problema 3:</b> Pagas una bolsa de arroz de C$22 con un billete de C$50. ¿Cuánto vuelto te deben entregar en la caja?",
            opciones: ["C$28", "C$38", "C$26", "C$32"],
            correcta: "C$28"
        },
        {
            pregunta: "🥛 <b>Problema 4:</b> Una caja grande con 6 litros de leche cuesta C$120. ¿Cuánto cuesta cada litro de leche individualmente?",
            opciones: ["C$15", "C$20", "C$25", "C$18"],
            correcta: "C$20"
        },
        {
            pregunta: "🍎 <b>Problema 5:</b> Una bolsa de manzanas pesa 3 libras. Si subes al carrito 4 bolsas iguales, ¿cuántas libras de manzana llevas en total?",
            opciones: ["7 libras", "10 libras", "12 libras", "14 libras"],
            correcta: "12 libras"
        },
        {
            pregunta: "🧼 <b>Problema 6:</b> El jabón líquido cuesta C$45, pero hoy tiene una rebaja especial de C$15. ¿Cuál es el precio final del jabón con el descuento?",
            opciones: ["C$30", "C$35", "C$25", "C$40"],
            correcta: "C$30"
        },
        {
            pregunta: "📦 <b>Problema 7:</b> En el estante quedan 3 filas de jugos, y cada fila tiene exactamente 8 jugos. ¿Cuántos jugos quedan disponibles en total?",
            opciones: ["21 jugos", "24 jugos", "28 jugos", "18 jugos"],
            correcta: "24 jugos"
        }
    ];

    let indiceActual = 0;
    let aciertosDirectos = 0; 
    let erroresCometidos = 0; 
    let haFalladoEnPreguntaActual = false;
    let haRespondidoCorrectamente = false;
    let tiempoSegundos = 0;
    let intervaloCronometro = null;

    // Vinculación segura del botón de inicio cuando el documento esté totalmente cargado
    document.addEventListener("DOMContentLoaded", function() {
        actualizarMarcadores();
        mostrarProblema();
        
        const botonInicio = document.getElementById('btn-comenzar-juego');
        if(botonInicio) {
            botonInicio.addEventListener('click', function() {
                // Forzar desaparición pase lo que pase
                document.getElementById('pantalla-inicio').style.display = 'none';
                iniciarJuego();
            });
        }
    });

    function iniciarJuego() {
        indiceActual = 0;
        aciertosDirectos = 0;
        erroresCometidos = 0;
        haRespondidoCorrectamente = false;
        haFalladoEnPreguntaActual = false;
        tiempoSegundos = 0;
        
        if(intervaloCronometro) clearInterval(intervaloCronometro);
        
        document.getElementById('bloqueo-juego').style.display = 'block';
        document.getElementById('pantalla-final').style.display = 'none';
        document.getElementById('btn-siguiente').style.display = 'none';
        
        actualizarMarcadores();
        mostrarProblema();
        iniciarCronometro();
    }

    function iniciarCronometro() {
        intervaloCronometro = setInterval(() => {
            tiempoSegundos++;
            let mins = Math.floor(tiempoSegundos / 60).toString().padStart(2, '0');
            let segs = (tiempoSegundos % 60).toString().padStart(2, '0');
            document.getElementById('txt-tiempo').innerText = `⏱️ Tiempo: ${mins}:${segs}`;
        }, 1000);
    }

    function mostrarProblema() {
        haRespondidoCorrectamente = false;
        haFalladoEnPreguntaActual = false;
        document.getElementById('btn-siguiente').style.display = 'none';
        restablecerAvatar();
        
        let problema = bancoProblemas[indiceActual];
        if(!problema) return;
        
        document.getElementById('campo-pregunta').innerHTML = problema.pregunta;
        
        let contenedor = document.getElementById('contenedor-opciones');
        contenedor.innerHTML = "";
        
        problema.opciones.forEach((opcion, i) => {
            let boton = document.createElement('button');
            boton.className = `btn-opcion opc-${i}`;
            boton.innerText = opcion;
            boton.onclick = () => verificarRespuesta(opcion, boton);
            contenedor.appendChild(boton);
        });
    }

    function verificarRespuesta(opcionSeleccionada, botonPresionado) {
        if (haRespondidoCorrectamente) return;

        let problema = bancoProblemas[indiceActual];
        if(opcionSeleccionada.trim() === problema.correcta.trim()) {
            AudioGame.playCorrecto();
            if (!haFalladoEnPreguntaActual) aciertosDirectos++;
            haRespondidoCorrectamente = true;
            actualizarMarcadores();
            
            document.getElementById('avatar-cara').innerText = "🥳";
            document.getElementById('avatar-texto').innerText = "¡CORRECTO! 🌟 ¡Excelente trabajo analizando los datos!";
            document.getElementById('caja-avatar').style.backgroundColor = "#e8f5e9";
            
            document.querySelectorAll('.btn-opcion').forEach(b => b.disabled = true);
            botonPresionado.style.background = "#a5d6a7";
            document.getElementById('btn-siguiente').style.display = 'block';
        } else {
            AudioGame.playError();
            haFalladoEnPreguntaActual = true;
            erroresCometidos++; 
            
            document.getElementById('avatar-cara').innerText = "😥";
            document.getElementById('avatar-texto').innerText = "¡INCORRECTO! ❌ Intenta otra vez.";
            document.getElementById('caja-avatar').style.backgroundColor = "#ffebee";
            
            botonPresionado.style.background = "#ffcdd2";
            botonPresionado.disabled = true;
        }
    }

    function avanzarPregunta() {
        indiceActual++;
        if (indiceActual < bancoProblemas.length) {
            mostrarProblema();
            actualizarMarcadores();
        } else {
            finalizarJuego();
        }
    }

    function restablecerAvatar() {
        document.getElementById('avatar-cara').innerText = "🤖";
        document.getElementById('avatar-texto').innerText = "¡Lee con atención el problema de compras!";
        document.getElementById('caja-avatar').style.backgroundColor = "#f5f5f5";
    }

    function actualizarMarcadores() {
        document.getElementById('txt-progreso').innerText = `Pregunta: ${indiceActual + 1} / ${bancoProblemas.length}`;
        document.getElementById('txt-puntos').innerText = `⭐ Aciertos: ${aciertosDirectos}`;
    }

    function finalizarJuego() {
        if(intervaloCronometro) clearInterval(intervaloCronometro);
        document.getElementById('bloqueo-juego').style.display = 'none';
        document.getElementById('pantalla-final').style.display = 'block';
        
        let mins = Math.floor(tiempoSegundos / 60).toString().padStart(2, '0');
        let segs = (tiempoSegundos % 60).toString().padStart(2, '0');
        document.getElementById('td-tiempo-final').innerText = `${mins}:${segs}`;
        
        let penalizacion = erroresCometidos * 5;
        let porcentajeFinal = Math.max(0, 100 - penalizacion);
        document.getElementById('td-porcentaje-final').innerText = `${porcentajeFinal}%`;
    }

    function reiniciarJuego() {
        document.getElementById('pantalla-inicio').style.display = 'flex';
    }
</script>
</body>
</html>
