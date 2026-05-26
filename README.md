<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>MateVida Digital - Portal Educativo</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <style>
        :root {
            --verde: #4CAF50; --azul: #2196F3; --amarillo: #FFC107; --naranja: #FF9800;
            --fondo: #e3f2fd; --azul-tech: #1a237e;
        }
        body { 
            margin: 0; background-color: var(--fondo); 
            font-family: 'Arial Rounded MT Bold', sans-serif; text-align: center; 
        }
        
        .top-nav {
            background: white; padding: 15px; display: flex;
            justify-content: space-between; box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            align-items: center; flex-wrap: wrap; gap: 15px; /* Más espacio entre elementos */
        }
        .top-nav input { padding: 10px 15px; border-radius: 20px; border: 1px solid #ccc; width: 100%; max-width: 250px; }
        .menu-usuario-top { display: flex; align-items: center; gap: 15px; font-size: 14px; font-weight: bold; }

        .hero { padding: 20px 10px; }
        .robot { font-size: 60px; display: block; }
        .titulo { font-size: clamp(30px, 8vw, 45px); color: var(--verde); margin: 5px 0; }
        .titulo span { color: var(--naranja); }
        .slogan { font-weight: bold; color: var(--azul); margin-bottom: 20px; display: block; font-size: 18px; }

        .menu { display: flex; justify-content: center; gap: 12px; flex-wrap: wrap; padding: 10px; max-width: 1000px; margin: 0 auto; }
        .card {
            width: calc(50% - 20px); /* Dos por fila en móviles */
            max-width: 140px; padding: 18px 10px; border-radius: 20px; color: white;
            font-weight: bold; cursor: pointer; box-shadow: 0 5px 0px rgba(0,0,0,0.2);
            transition: 0.2s; font-size: 16px; box-sizing: border-box;
        }
        .card:active { transform: translateY(4px); box-shadow: 0 1px 0px rgba(0,0,0,0.2); }
        .c-verde { background: var(--verde); }
        .c-azul { background: var(--azul); }
        .c-amarillo { background: var(--amarillo); }
        .c-naranja { background: var(--naranja); }

        .main-stage {
            max-width: 900px; margin: 20px 15px; background: white;
            padding: 25px; border-radius: 30px; min-height: 400px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            display: inline-block; width: calc(100% - 30px); box-sizing: border-box;
        }

        .grid-contenido {
            display: grid; 
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); /* Columnas más flexibles */
            gap: 15px; margin-top: 20px;
        }
        .item-lista {
            border: 2px solid #f0f0f0; padding: 15px; border-radius: 15px;
            cursor: pointer; transition: 0.3s; background: white;
            text-decoration: none; color: black; display: flex; flex-direction: column; align-items: center;
            justify-content: center;
        }
        .item-lista h3 { font-size: 1rem; margin: 10px 0 0 0; line-height: 1.2; }
        .item-lista:hover { border-color: var(--azul); transform: scale(1.03); }
        
        /* Visor Mejorado */
        #visor-pro {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: #1a1a2e; z-index: 2000;
        }
        .barra-visor {
            background: #16213e; color: white; padding: 10px 15px; 
            display: flex; justify-content: space-between; align-items: center;
            gap: 10px;
        }
        #titulo-juego-actual {
            font-size: 1rem; flex: 1; text-align: left;
            white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
        }
        .grupo-botones { display: flex; gap: 6px; flex-shrink: 0; }
        .btn-accion {
            border: none; padding: 8px 12px; border-radius: 8px; cursor: pointer; 
            color: white; font-weight: bold; font-size: 12px;
            display: flex; align-items: center; gap: 5px;
        }

        /* Modal QR */
        #modal-qr {
            display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
            background: white; padding: 20px; border-radius: 20px; box-shadow: 0 0 20px rgba(0,0,0,0.5);
            z-index: 2002; text-align: center; width: 85%; max-width: 280px;
        }
        #overlay {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 2001;
        }

        /* AJUSTES RESPONSIVOS */
        @media (min-width: 601px) {
            .card { width: 120px; }
            .top-nav { padding: 15px 40px; }
            .main-stage { margin: 30px auto; }
        }

        @media (max-width: 600px) {
            .top-nav { justify-content: center; }
            .menu-usuario-top { width: 100%; justify-content: center; border-top: 1px solid #eee; pt: 10px; }
            .barra-visor { padding: 8px; }
            #titulo-juego-actual { font-size: 0.85rem; }
            .btn-accion span:not(:first-child) { display: none; } /* En móvil muy pequeño solo queda el emoji */
            .btn-accion { padding: 8px; }
        }

        /* Estilos para el Bloqueo de Bienvenida */
        #bloqueo-inicio {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--fondo); z-index: 3000; display: flex;
            justify-content: center; align-items: center; padding: 20px; box-sizing: border-box;
        }
        .contenedor-login {
            background: white; padding: 30px; border-radius: 30px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1); width: 100%; max-width: 400px;
        }
        .contenedor-login input {
            width: 100%; padding: 12px 20px; margin: 15px 0;
            border-radius: 25px; border: 2px solid #ddd; font-size: 16px;
            box-sizing: border-box; text-align: center;
        }
        .contenedor-login button {
            background: var(--verde); color: white; border: none;
            padding: 12px 30px; font-size: 18px; font-weight: bold;
            border-radius: 25px; cursor: pointer; width: 100%;
            box-shadow: 0 4px 0px #388E3C; transition: 0.2s;
        }
        .contenedor-login button:active { transform: translateY(4px); box-shadow: 0 0px 0px #388E3C; }
    </style>
</head>
<body>

<div id="bloqueo-inicio">
    <div class="contenedor-login">
        <span style="font-size: 50px;">🤖</span>
        <h2 style="color: var(--azul); margin: 10px 0;">¡Bienvenido a MateVida!</h2>
        <p style="color: #666; font-size: 14px;">Ingresa tu nombre o usuario para comenzar a jugar</p>
        <input type="text" id="nombreUsuarioInput" placeholder="Tu Nombre o Usuario...">
        <button onclick="ingresarAApp()">¡Comenzar! 🚀</button>
    </div>
</div>

<div class="top-nav">
    <input type="text" id="inputBusqueda" onkeyup="buscarContenido()" placeholder="🔍 Buscar actividad...">
    <div class="menu-usuario-top">
        <span onclick="irInicio()" style="cursor:pointer">🏠 Inicio</span> | 
        <span onclick="accederTrafico()" style="cursor:pointer; color: var(--naranja);">📊 Tráfico</span>
    </div>
</div>

<div class="hero">
    <span class="robot">🤖</span>
    <h1 class="titulo">MateVida <span>Digital</span></h1>
    <span class="slogan">“Aprende jugando”</span>
</div>

<div class="menu">
    <div class="card c-verde" onclick="cargarSeccion('juegos')">🎮<br>Juegos</div>
    <div class="card c-azul" onclick="cargarSeccion('videos')">📺<br>Videos</div>
    <div class="card c-amarillo" onclick="cargarSeccion('fichas')">📚<br>Fichas</div>
    <div class="card c-naranja" onclick="cargarSeccion('recursos')">👥<br>Recursos</div>
</div>

<div id="pantalla" class="main-stage">
    <h2>Entrena tu Mente, Domina los Números</h2>
    <p>Selecciona una categoría arriba para comenzar a aprender.</p>
    <img src="https://raw.githubusercontent.com/Brimar26/portada/main/portadamate.png" alt="Portada MateVida" style="max-width:100%; border-radius:20px; display: block; margin: 20px auto;">   
</div>

<div id="visor-pro">
    <div class="barra-visor" translate="no">
        <strong id="titulo-juego-actual">Cargando...</strong>
        <div class="grupo-botones">
            <button onclick="compartirJuego()" class="btn-accion" style="background: var(--azul);"><span>🔗</span> <span>COMPARTIR</span></button>
            <button onclick="mostrarQR()" class="btn-accion" style="background: var(--amarillo); color: black;"><span>📱</span> <span>QR</span></button>
            <button onclick="cerrarSoftware()" class="btn-accion" style="background:#e74c3c;"><span>✖</span> <span>CERRAR</span></button>
        </div>
    </div>
    <iframe id="iframe-software" src="" style="width:100%; height:calc(100% - 55px); border:none; background: white;"></iframe>
</div>

<div id="overlay" onclick="cerrarQR()"></div>
<div id="modal-qr">
    <h3>Escanea para jugar</h3>
    <div id="qrcode" style="display: flex; justify-content: center; margin: 15px;"></div>
    <button onclick="cerrarQR()" class="btn-accion" style="background: #333; margin: 0 auto; display: block; width: 100px; justify-content: center;">Cerrar</button>
</div>

<script>
let urlActual = "";
let nombreActual = "";

// LÓGICA DE CONTROL DE TRÁFICO LOCAL Y ACCESO
const CONTRASEÑA_ADMIN = "admin123"; // Cambia esta contraseña si lo deseas

function ingresarAApp() {
    let nombre = document.getElementById('nombreUsuarioInput').value.trim();
    if (nombre === "") {
        alert("Por favor, introduce un nombre o usuario válido.");
        return;
    }
    
    // Guardar el usuario en el historial de la sesión actual
    let historico = JSON.parse(sessionStorage.getItem('trafico_usuarios')) || [];
    historico.push({ nombre: nombre, hora: new Date().toLocaleTimeString() });
    sessionStorage.setItem('trafico_usuarios', JSON.stringify(historico));
    
    // Ocultar la pantalla de bienvenida
    document.getElementById('bloqueo-inicio').style.display = 'none';
}

function accederTrafico() {
    let psw = prompt("SÓLO ADMINISTRADOR: Introduce la contraseña para ver el tráfico:");
    if (psw === CONTRASEÑA_ADMIN) {
        mostrarPanelTrafico();
    } else if (psw !== null) {
        alert("Contraseña incorrecta. Acceso denegado.");
    }
}

function mostrarPanelTrafico() {
    let historico = JSON.parse(sessionStorage.getItem('trafico_usuarios')) || [];
    let listaHTML = historico.length === 0 
        ? "<p>No hay registros de ingresos todavía.</p>" 
        : `<ul style="text-align:left; display:inline-block; margin:10px auto;">` + 
          historico.map(u => `<li><strong>${u.nombre}</strong> ingresó a las ${u.hora}</li>`).join('') + 
          `</ul>`;

    document.getElementById('pantalla').innerHTML = `
        <h2 style="color:var(--azul)">📊 Panel de Tráfico (Privado)</h2>
        <div style="background:#f9f9f9; padding:20px; border-radius:15px; display:inline-block; width: 100%; max-width: 500px; box-sizing:border-box;">
            <p><strong>Total de accesos registrados en esta sesión:</strong> ${historico.length}</p>
            <hr style="border:1px solid #ddd;">
            <h4>Usuarios que han entrado:</h4>
            ${listaHTML}
        </div>`;
}

function lanzarSoftware(url, nombre) {
    urlActual = url;
    nombreActual = nombre;
    document.getElementById('titulo-juego-actual').innerText = nombre;
    const iframe = document.getElementById('iframe-software');
    iframe.src = url;
    document.getElementById('visor-pro').style.display = 'block';
    document.body.style.overflow = 'hidden'; 
}

async function compartirJuego() {
    const shareData = {
        title: nombreActual,
        text: `¡Mira este juego en MateVida Digital: ${nombreActual}!`,
        url: urlActual
    };
    try {
        if (navigator.share) {
            await navigator.share(shareData);
        } else {
            await navigator.clipboard.writeText(urlActual);
            alert("Enlace copiado al portapapeles");
        }
    } catch (err) { console.log("Error al compartir", err); }
}

function mostrarQR() {
    document.getElementById('qrcode').innerHTML = ""; 
    new QRCode(document.getElementById("qrcode"), {
        text: urlActual,
        width: 180,
        height: 180
    });
    document.getElementById('modal-qr').style.display = 'block';
    document.getElementById('overlay').style.display = 'block';
}

function cerrarQR() {
    document.getElementById('modal-qr').style.display = 'none';
    document.getElementById('overlay').style.display = 'none';
}

function cerrarSoftware() {
    document.getElementById('visor-pro').style.display = 'none';
    document.getElementById('iframe-software').src = ""; 
    document.body.style.overflow = 'auto';
    urlActual = "";
}

function buscarContenido() {
    let input = document.getElementById('inputBusqueda').value.toLowerCase();
    let items = document.getElementsByClassName('item-lista');
    for (let i = 0; i < items.length; i++) {
        let texto = items[i].innerText.toLowerCase();
        items[i].style.display = texto.includes(input) ? "flex" : "none";
    }
}

function irInicio() { 
    // Evitamos el reload completo para no perder temporalmente el sessionStorage del tráfico en la demo rápida
    document.getElementById('pantalla').innerHTML = `
        <h2>Entrena tu Mente, Domina los Números</h2>
        <p>Selecciona una categoría arriba para comenzar a aprender.</p>
        <img src="https://raw.githubusercontent.com/Brimar26/portada/main/portadamate.png" alt="Portada MateVida" style="max-width:100%; border-radius:20px; display: block; margin: 20px auto;">
    `;
}

function cargarSeccion(tipo) {
    const pantalla = document.getElementById('pantalla');
    let html = "";

    if (tipo === 'juegos') {
        html = `
            <h2 style="color:var(--verde)">🎮 Panel de Actividades</h2>
            <div class="grid-contenido">
                <div class="item-lista" onclick="lanzarSoftware('https://view.genially.com/69ec0dd6dc5b8be996902a64', 'Carrera Operaciones')">
                    <div style="font-size:40px">🏎️</div><h3>Carrera Operaciones</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://view.genially.com/69ec0094c6e29311f5854247', 'Operaciones Matemáticas')">
                    <div style="font-size:40px">➕</div><h3>Operaciones Matemáticas</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://es.educaplay.com/juego/28831395-operaciones_fundamentales_si_no.html', 'Si o No')">
                    <div style="font-size:40px">✅</div><h3>Si o No</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://es.educaplay.com/juego/28831338-operaciones_en_la_vida_diaria.html', 'Vida Diaria')">
                    <div style="font-size:40px">📋</div><h3>Vida Diaria</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://es.educaplay.com/juego/28918221-desafios_cotidianos_con_operaciones.html', 'Desafíos Cotidianos')">
                    <div style="font-size:40px">🏆</div><h3>Desafíos Cotidianos</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://view.genially.com/69f7e18ce15a962730ed9a10', 'Compras')">
                    <div style="font-size:40px">🛒</div><h3>Compras</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://brimar26.github.io/DADOS-M-GICOS/', 'DADOS')">
                    <div style="font-size:40px">🎮</div><h3>DADOS</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://brimar26.github.io/Matecarrera/', 'Matecarrera')">
                    <div style="font-size:40px">🏎</div><h3>Matecarrera</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://github.com/Brimar26/Nivel-Pali', 'NIVEL PALI')">
                    <div style="font-size:40px">🅿️</div><h3>NIVEL PALI</h3>
                </div>
                
                <div class="item-lista" onclick="lanzarSoftware('https://brimar26.github.io/tablas-de-multiplicar/', 'MATEBLAS')">
                    <div style="font-size:40px">🤖✖️</div><h3>MATEBLAS</h3>
                </div>
            </div>`;
    } 
    else if (tipo === 'videos') {
        html = `
            <h2 style="color:var(--azul)">📺 Videoteca Educativa</h2>
            <div class="grid-contenido">
                <div class="item-lista" onclick="lanzarSoftware('https://www.youtube.com/embed/aEh9WnqiyAg', 'Operaciones Vida Diaria')">
                    <img src="https://img.youtube.com/vi/aEh9WnqiyAg/mqdefault.jpg" width="100%" style="border-radius:10px; margin-bottom:10px;">
                    <h3>Vida Diaria</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://www.youtube.com/embed/otatgqU8o0w', 'Matemática divertida')">
                    <img src="https://img.youtube.com/vi/otatgqU8o0w/mqdefault.jpg" width="100%" style="border-radius:10px; margin-bottom:10px;">
                    <h3>Matemática divertida</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://www.youtube.com/embed/yyKkL0R59g0', 'Aprende a dividir')">
                    <img src="https://img.youtube.com/vi/yyKkL0R59g0/mqdefault.jpg" width="100%" style="border-radius:10px; margin-bottom:10px;">
                    <h3>Aprende a dividir</h3>
                </div>
                <div class="item-lista" onclick="lanzarSoftware('https://www.youtube.com/embed/WBqXpj1_96g', 'Operaciones')">
                    <img src="https://img.youtube.com/vi/WBqXpj1_96g/mqdefault.jpg" width="100%" style="border-radius:10px; margin-bottom:10px;">
                    <h3>Operaciones</h3>
                </div>
            </div>`;
    }
    else if (tipo === 'fichas') {
        html = `
            <h2 style="color:var(--amarillo)">📚 Fichas e Imprimibles</h2>
            <div class="grid-contenido">
                <a href="https://web.seducoahuila.gob.mx/biblioweb/upload/operaciones-y-problemas-3c2ba-de-primaria%20(1).pdf" target="_blank" class="item-lista">
                    <div style="font-size:40px">🧮</div><h3>Operaciones y Problemas</h3>
                </a>
                <a href="https://www.jica.go.jp/Resource/project/elsalvador/004/materials/ku57pq00003u6zom-att/cuaderno_ejercicios_primaria_05.pdf" target="_blank" class="item-lista">
                    <div style="font-size:40px">📓</div><h3>Cuaderno de Ejercicios</h3>
                </a>
                <a href="https://www.mamutmatematicas.com/ejercicios/tabla-orden-operaciones.php" target="_blank" class="item-lista">
                    <div style="font-size:40px">⚖️</div><h3>Orden de Operaciones</h3>
                </a>
                <a href="https://arbolabc.com/juegos-tablas-de-multiplicar/tablas-imprimibles/operaciones-tabla-del-7" target="_blank" class="item-lista">
                    <div style="font-size:40px">✖️</div><h3>Tablas de Multiplicar</h3>
                </a>
            </div>`;
    }
    else if (tipo === 'recursos') {
        html = `
            <h2 style="color:var(--naranja)">👥 Sección de Recursos</h2>
            <div class="grid-contenido">
                <div class="item-lista">
                    <div style="font-size:40px">🏠</div>
                    <h3>Guía para Padres</h3>
                    <a href='https://gu-a-para-padres.tiiny.site/' target="_blank" style="margin-top:10px; color:var(--azul);">Entrar al Menú</a>
                </div>
                <div class="item-lista">
                    <div style="font-size:40px">👤</div>
                    <h3>Guía para Estudiantes</h3>
                    <a href='https://www.pdffiller.com/s/1LObffqZo7' target="_blank" style="margin-top:10px; color:var(--azul);">Entrar al Menú</a>
                </div>
                <div class="item-lista">
                    <div style="font-size:40px">🎖️</div>
                    <h3>Guía para Docentes</h3>
                    <a href='https://www.pdffiller.com/s/ULtGifon' target="_blank" style="margin-top:10px; color:var(--azul);">Entrar al Menú</a>
                </div>
            </div>`;
    }
    pantalla.innerHTML = html;
}
</script>
</body>
</html>
