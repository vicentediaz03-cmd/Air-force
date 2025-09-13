<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AIR FORCE</title>
  <style>
    :root {
      --primary: #0078d4;
      --bg: #f7f7f7;
      --card-bg: #ffffff;
      --text: #222;
      --border: #eaeaea;
      --shadow: 0 2px 12px rgba(0,0,0,0.07);
    }
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 0;
    }
    header {
      padding: 40px 0 20px 0;
      text-align: center;
    }
    h1 {
      font-weight: 700;
      font-size: 2.5em;
      margin: 0;
      letter-spacing: 1px;
    }
    .buscador {
      display: block;
      margin: 30px auto 40px auto;
      padding: 12px 18px;
      width: 320px;
      border-radius: 6px;
      border: 1px solid var(--border);
      font-size: 1em;
      background: #fff;
      box-shadow: var(--shadow);
    }
    main {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 20px;
    }
    .avion {
      background: var(--card-bg);
      border-radius: 12px;
      box-shadow: var(--shadow);
      display: flex;
      flex-wrap: wrap;
      align-items: flex-start;
      gap: 24px;
      margin-bottom: 38px;
      padding: 20px 28px;
      border: 1px solid var(--border);
      transition: box-shadow 0.2s, transform 0.2s;
    }
    .avion:hover {
      box-shadow: 0 6px 24px rgba(0,0,0,0.13);
      transform: translateY(-4px) scale(1.01);
    }
    .galeria {
      position: relative;
      width: 340px;
      min-width: 220px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 12px;
    }
    .galeria img[data-galeria] {
      width: 320px;
      height: 230px;
      object-fit: cover;
      border-radius: 6px;
      box-shadow: var(--shadow);
      background: #ffffff;
      transition: transform 0.3s;
      cursor: pointer;
      display: block;
    }
    .galeria img[data-galeria]:hover {
      transform: scale(1.04);
    }
    .galeria .flecha {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: #fff;
      border: none;
      color: var(--primary);
      font-size: 2em;
      cursor: pointer;
      border-radius: 50%;
      width: 38px;
      height: 38px;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 2;
      transition: background 0.2s;
      box-shadow: var(--shadow);
    }
    .galeria .flecha.izq { left: 8px; }
    .galeria .flecha.der { right: 8px; }
    .logo-fabricante {
      width: 100px;
      height: auto;
      object-fit: contain;
      border-radius: 8px;
      background: #fff;
      box-shadow: var(--shadow);
      border: 1px solid var(--border);
      cursor: pointer;
    }
    .avion-info {
      flex: 1;
      min-width: 220px;
    }
    .avion-info h2 {
      font-size: 1.4em;
      margin: 0 0 10px 0;
      font-weight: 600;
    }
    .avion-info p {
      margin: 7px 0;
      font-size: 1.05em;
    }
    .mostrar-btn {
      background: var(--primary);
      color: #fff;
      border: none;
      padding: 7px 16px;
      border-radius: 6px;
      cursor: pointer;
      margin-top: 12px;
      font-size: 1em;
      box-shadow: var(--shadow);
      transition: background 0.2s;
    }
    .mostrar-btn:hover {
      background: #03131f;
    }
    .detalles {
      display: none;
      margin-top: 12px;
      font-size: 0.98em;
      color: #444;
      animation: fadeIn 0.5s;
      background: #f3f3f3;
      border-radius: 8px;
      padding: 12px 18px;
    }
    .video-container {
      text-align: center;
      margin: -20px auto 40px auto;
      background: var(--card-bg);
      border-radius: 12px;
      box-shadow: var(--shadow);
      padding: 32px 0 18px 0;
      max-width: 900px;
    }
    .video-container h2 {
      font-size: 1.3em;
      font-weight: 500;
      margin-bottom: 18px;
    }
    video {
      width: 95%;
      max-width: 830px;
      border-radius: 10px;
      box-shadow: var(--shadow);
      background: #eeeeee;
    }
    footer {
      text-align: center;
      margin: 60px 0 20px 0;
      color: #666;
      font-size: 1em;
      letter-spacing: 1px;
    }
    @media (max-width: 900px) {
      main { padding: 0 10px; }
      .avion { flex-direction: column; gap: 18px; padding: 22px 10px;}
      .galeria { width: 100%; }
      .galeria img[data-galeria] { width: 98%; max-width: 340px; height: auto;}
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px);}
      to { opacity: 1; transform: translateY(0);}
    }
    .oculto { display: none !important; }
  </style>
  <script>
    function toggleDetalles(id) {
      const detalles = document.getElementById(id);
      if (detalles.style.display === 'none' || detalles.style.display === '') {
        detalles.style.display = 'block';
        detalles.style.animation = 'fadeIn 0.5s';
      } else {
        detalles.style.display = 'none';
      }
    }
    function filtrarAviones() {
      const filtro = document.getElementById('buscador').value.toLowerCase();
      const aviones = document.getElementsByClassName('avion');
      for (let avion of aviones) {
        const nombre = avion.querySelector('h2').textContent.toLowerCase();
        avion.style.display = nombre.includes(filtro) ? 'flex' : 'none';
      }
    }
    function cambiarImagen(galeriaId, dir) {
      const galeria = document.getElementById(galeriaId);
      const imagenes = galeria.querySelectorAll('img[data-galeria]');
      let actual = -1;
      imagenes.forEach((img, i) => {
        if (!img.classList.contains('oculto')) actual = i;
      });
      imagenes[actual].classList.add('oculto');
      let nuevo = actual + dir;
      if (nuevo < 0) nuevo = imagenes.length - 1;
      if (nuevo >= imagenes.length) nuevo = 0;
      imagenes[nuevo].classList.remove('oculto');
    }
    function mostrarImagenGrande(src) {
      const modal = document.createElement('div');
      modal.style.position = 'fixed';
      modal.style.top = '0';
      modal.style.left = '0';
      modal.style.width = '100vw';
      modal.style.height = '100vh';
      modal.style.background = 'rgba(0,0,0,0.8)';
      modal.style.display = 'flex';
      modal.style.alignItems = 'center';
      modal.style.justifyContent = 'center';
      modal.style.zIndex = '9999';
      modal.onclick = () => document.body.removeChild(modal);
       const img = document.createElement('img');
            img.src = src;
            img.style.maxWidth = '80vw';
            img.style.maxHeight = '80vh';
            img.style.borderRadius = '14px';
            modal.appendChild(img);

            document.body.appendChild(modal);
        }
  
        window.addEventListener('DOMContentLoaded', () => {
            document.querySelectorAll('.galeria').forEach(galeria => {
                const imgs = galeria.querySelectorAll('img[data-galeria]');
                imgs.forEach((img, i) => {
                    if (i !== 0) img.classList.add('oculto');
                    else img.classList.remove('oculto');
                });
            });
        });
    </script>
  </script>
</head>
<body>
  <header>
    <header>
        <h1>FIGHTERS AIR FORCE</h1>
        <input class="buscador" id="buscador" type="text" placeholder="Buscar avión..." oninput="filtrarAviones()">
    </header>
    <div class="video-container">>
        <video width="830" controls>
            <source src="ssstik.io_1757720937426.mp4" type="video/mp4">
            Tu navegador no soporta el video.
        </video>
    </div>
    <main>
        <div class="avion">
            <div class="galeria" id="galeria-su57">
                <button class="flecha izq" onclick="cambiarImagen('galeria-su57', -1)">&lt;</button>
                <img src="Su-57-Felon-Stealth-Fighter-1.jpg" alt="Su-57 Felon" data-galeria onclick="mostrarImagenGrande(this.src)">
                <img src="Su-57.jpg" alt="Su-57 Felon 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
                <img src="Flag_of_Russia.svg.png" alt="Logo Sukhoi" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
                <button class="flecha der" onclick="cambiarImagen('galeria-su57', 1)">&gt;</button>
            </div>
            <div class="avion-info">
                <h2>Su-57 Felon</h2>
                <p><strong>Tipo:</strong> Caza furtivo de quinta generación</p>
                <p><strong>Fabricante:</strong> Sukhoi (Rusia)</p>
                <p><strong>Velocidad Máxima:</strong> Mach 2.0</p>
                <p><strong>Armamento:</strong> Misiles aire-aire, aire-tierra, cañón interno</p>
                <p><strong>Descripción:</strong> El Su-57 es el primer caza furtivo ruso, diseñado para competir con los cazas occidentales. Incorpora tecnología stealth, supercrucero y alta maniobrabilidad.</p>
                <button class="mostrar-btn" onclick="toggleDetalles('detalles-su57')">Mostrar detalles</button>
                <div class="detalles" id="detalles-su57">
                    <ul>
                        <li>Primer vuelo: 2010</li>
                        <li>Tripulación: 1</li>
                        <li>Radio de combate: 1,500 km</li>
                        <li>Motor: Saturn Izdeliye 30</li>
                        <li><a href="https://es.wikipedia.org/wiki/Sukhoi_Su-57" target="_blank">Wikipedia</a></li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="avion">
            <div class="galeria" id="galeria-f22">
                <button class="flecha izq" onclick="cambiarImagen('galeria-f22', -1)">&lt;</button>
                <img src="EE.UU-despliega-sus-poderosos-aviones-de-combate-F-22-Raptor-en-Oriente-Medio-ante-tension-con-Rusia.jpg" alt="F-22 Raptor" data-galeria onclick="mostrarImagenGrande(this.src)">
                <img src="avion.jpg" alt="F-22 Raptor 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
                <img src="Lockheed_Martin-Logo.wine.png" alt="Logo Lockheed Martin" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
                <button class="flecha der" onclick="cambiarImagen('galeria-f22', 1)">&gt;</button>
            </div>
            <div class="avion-info">
                <h2>F-22 Raptor</h2>
                <p><strong>Tipo:</strong> Caza furtivo de superioridad aérea</p>
                <p><strong>Fabricante:</strong> Lockheed Martin (EE. UU.)</p>
                <p><strong>Velocidad Máxima:</strong> Mach 2.25</p>
                <p><strong>Armamento:</strong> Misiles aire-aire, bombas guiadas, cañón M61A2</p>
                <p><strong>Descripción:</strong> El F-22 es considerado uno de los cazas más avanzados del mundo, con capacidades stealth, supercrucero y sistemas electrónicos de última generación.</p>
                <button class="mostrar-btn" onclick="toggleDetalles('detalles-f22')">Mostrar detalles</button>
                <div class="detalles" id="detalles-f22">
                    <ul>
                        <li>Primer vuelo: 1997</li>
                        <li>Tripulación: 1</li>
                        <li>Radio de combate: 760 km</li>
                        <li>Motor: Pratt & Whitney F119</li>
                        <li><a href="https://es.wikipedia.org/wiki/Lockheed_Martin_F-22_Raptor" target="_blank">Wikipedia</a></li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="avion">
            <div class="galeria" id="galeria-j20">
                <button class="flecha izq" onclick="cambiarImagen('galeria-j20', -1)">&lt;</button>
                <img src="J-20-new.jpg"Chengdu J-20" data-galeria onclick="mostrarImagenGrande(this.src)">
                <img src="j-20-4.webp" alt="Chengdu J-20 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
                <img src="logo-chengdu.jpg" alt="Logo Chengdu" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
                <button class="flecha der" onclick="cambiarImagen('galeria-j20', 1)">&gt;</button>
            </div>
            <div class="avion-info">
                <h2>Chengdu J-20</h2>
                <p><strong>Tipo:</strong> Caza furtivo de largo alcance</p>
                <p><strong>Fabricante:</strong> Chengdu Aerospace Corporation (China)</p>
                <p><strong>Velocidad Máxima:</strong> Mach 2.0</p>
                <p><strong>Armamento:</strong> Misiles aire-aire, bombas guiadas</p>
                <p><strong>Descripción:</strong> El J-20 es el primer caza furtivo chino, diseñado para misiones de penetración profunda y superioridad aérea, con tecnología stealth y avanzada aviónica.</p>
                <button class="mostrar-btn" onclick="toggleDetalles('detalles-j20')">Mostrar detalles</button>
                <div class="detalles" id="detalles-j20">
                    <ul>
                        <li>Primer vuelo: 2011</li>
                        <li>Tripulación: 1</li>
                        <li>Radio de combate: 1,100 km</li>
                        <li>Motor: WS-10B</li>
                        <li><a href="https://es.wikipedia.org/wiki/Chengdu_J-20" target="_blank">Wikipedia</a></li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="avion">
            <div class="galeria" id="galeria-f15">
                <button class="flecha izq" onclick="cambiarImagen('galeria-f15', -1)">&lt;</button>
                <img src="images.jpeg" alt="F-15 Eagle / F-15EX" data-galeria onclick="mostrarImagenGrande(this.src)">
                <img src="images (1).jpeg" alt="F-15 Eagle 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
                <img src="McDonnell_Douglas.svg.png" alt="Logo Boeing" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
                <button class="flecha der" onclick="cambiarImagen('galeria-f15', 1)">&gt;</button>
            </div>
            <div class="avion-info">
                <h2>F-15 Eagle / F-15EX</h2>
                <p><strong>Tipo:</strong> Caza de superioridad aérea / multirrol</p>
                <p><strong>Fabricante:</strong> McDonnell Douglas / Boeing (EE. UU.)</p>
                <p><strong>Velocidad Máxima:</strong> Mach 2.5</p>
                <p><strong>Armamento:</strong> Misiles aire-aire, bombas, cañón M61 Vulcan</p>
                <p><strong>Descripción:</strong> El F-15 es uno de los cazas más exitosos de la historia, con una tasa de victorias excepcional y versiones modernizadas como el F-15EX.</p>
                <button class="mostrar-btn" onclick="toggleDetalles('detalles-f15')">Mostrar detalles</button>
                <div class="detalles" id="detalles-f15">
                    <ul>
                        <li>Primer vuelo: 1972</li>
                        <li>Tripulación: 1 o 2</li>
                        <li>Radio de combate: 1,967 km</li>
                        <li>Motor: Pratt & Whitney F100</li>
                        <li><a href="https://es.wikipedia.org/wiki/McDonnell_Douglas_F-15_Eagle" target="_blank">Wikipedia</a></li>
                    </ul>
                </div>
            </div>
        </div>
    </main>
    <footer>
        © 2025 | Página creada por Vicente | Proyecto Universidad
    </footer>
</body>
</html>
