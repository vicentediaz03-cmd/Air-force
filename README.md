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
      padding: 20px 10px;
      text-align: center;
    }
    h1 {
      font-weight: 700;
      font-size: 1.8em;
      margin: 0;
    }
    .buscador {
      display: block;
      margin: 20px auto;
      padding: 12px;
      width: 90%;
      max-width: 400px;
      border-radius: 6px;
      border: 1px solid var(--border);
      font-size: 1em;
      background: #fff;
      box-shadow: var(--shadow);
    }
    main {
      padding: 0 10px;
    }
    .avion {
      background: var(--card-bg);
      border-radius: 12px;
      box-shadow: var(--shadow);
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 28px;
      padding: 16px;
      border: 1px solid var(--border);
    }
    .galeria {
      position: relative;
      width: 100%;
      max-width: 500px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .galeria img[data-galeria] {
      width: 100%;
      height: auto;
      border-radius: 8px;
      box-shadow: var(--shadow);
      cursor: pointer;
    }
    .galeria .flecha {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(255,255,255,0.8);
      border: none;
      color: var(--primary);
      font-size: 1.6em;
      cursor: pointer;
      border-radius: 50%;
      width: 36px;
      height: 36px;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 2;
    }
    .galeria .flecha.izq { left: 10px; }
    .galeria .flecha.der { right: 10px; }
    .logo-fabricante {
      margin-top: 12px;
      width: 80px;
      height: auto;
      border-radius: 6px;
      border: 1px solid var(--border);
      box-shadow: var(--shadow);
      cursor: pointer;
    }
    .avion-info {
      width: 100%;
      margin-top: 14px;
    }
    .avion-info h2 {
      font-size: 1.3em;
      margin: 0 0 8px 0;
      font-weight: 600;
      text-align: center;
    }
    .avion-info p {
      margin: 6px 0;
      font-size: 1em;
    }
    .mostrar-btn {
      display: block;
      margin: 12px auto 0 auto;
      background: var(--primary);
      color: #fff;
      border: none;
      padding: 10px 20px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 1em;
      box-shadow: var(--shadow);
    }
    .mostrar-btn:hover {
      background: #005fa3;
    }
    .detalles {
      display: none;
      margin-top: 12px;
      font-size: 0.95em;
      color: #444;
      background: #f3f3f3;
      border-radius: 8px;
      padding: 12px;
    }
    .video-container {
      text-align: center;
      margin: 10px auto 30px auto;
      background: var(--card-bg);
      border-radius: 12px;
      box-shadow: var(--shadow);
      padding: 18px;
      max-width: 100%;
    }
    video {
      width: 100%;
      max-width: 500px;
      border-radius: 10px;
      box-shadow: var(--shadow);
    }
    footer {
      text-align: center;
      margin: 30px 0 20px 0;
      color: #666;
      font-size: 0.9em;
    }
    .oculto { display: none !important; }
  </style>
  <script>
    function toggleDetalles(id) {
      const detalles = document.getElementById(id);
      detalles.style.display = (detalles.style.display === 'none' || detalles.style.display === '') ? 'block' : 'none';
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
      img.style.maxWidth = '90vw';
      img.style.maxHeight = '90vh';
      img.style.borderRadius = '10px';
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
</head>
<body>
  <header>
    <h1>FIGHTERS AIR FORCE</h1>
    <input class="buscador" id="buscador" type="text" placeholder="Buscar avión..." oninput="filtrarAviones()">
  </header>

  <div class="video-container">
    <video controls>
      <source src="ssstik.io_1757720937426.mp4" type="video/mp4">
    </video>
  </div>

  <main>
    <div class="avion">
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

  <div class="galeria" id="galeria-su57">
    <button class="flecha izq" onclick="cambiarImagen('galeria-su57', -1)">&lt;</button>
    <img src="Su-57-Felon-Stealth-Fighter-1.jpg" alt="Su-57 Felon" data-galeria onclick="mostrarImagenGrande(this.src)">
    <img src="Su-57.jpg" alt="Su-57 Felon 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
    <img src="Flag_of_Russia.svg.png" alt="Logo Sukhoi" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
    <button class="flecha der" onclick="cambiarImagen('galeria-su57', 1)">&gt;</button>
  </div>
</div>
   <div class="avion">
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
  <div class="galeria" id="galeria-f22">
    <button class="flecha izq" onclick="cambiarImagen('galeria-f22', -1)">&lt;</button>
    <img src="EE.UU-despliega-sus-poderosos-aviones-de-combate-F-22-Raptor-en-Oriente-Medio-ante-tension-con-Rusia.jpg" alt="F-22 Raptor" data-galeria onclick="mostrarImagenGrande(this.src)">
    <img src="avion.jpg" alt="F-22 Raptor 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
    <img src="Lockheed_Martin-Logo.wine.png" alt="Logo Lockheed Martin" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
    <button class="flecha der" onclick="cambiarImagen('galeria-f22', 1)">&gt;</button>
  </div>
</div>
    <div class="avion">
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
  <div class="galeria" id="galeria-j20">
    <button class="flecha izq" onclick="cambiarImagen('galeria-j20', -1)">&lt;</button>
    <img src="J-20-new.jpg" alt="Chengdu J-20" data-galeria onclick="mostrarImagenGrande(this.src)">
    <img src="j-20-4.webp" alt="Chengdu J-20 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
    <img src="logo-chengdu.jpg" alt="Logo Chengdu" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
    <button class="flecha der" onclick="cambiarImagen('galeria-j20', 1)">&gt;</button>
  </div>
</div>
    <div class="avion">
  <div class="avion-info">
    <h2>F-15 Eagle / F-15EX</h2>
    <p><strong>Tipo:</strong> Caza de superioridad aérea / multirrol</p>
    <p><strong>Fabricante:</strong> McDonnell Douglas / Boeing (EE. UU.)</p>
    <p><strong>Velocidad Máxima:</strong> Mach 2.5</p>
    <p><strong>Armamento:</strong> Misiles aire-aire, bombas, cañón M61 Vulcan</p>
    <p><strong>Descripción:</strong> El F-15 es uno de los cazas más exitosos de la historia, con una tasa de victorias excepcional y versiones modernizadas como el F-15EX, que incorpora aviónica avanzada, radar AESA, arquitectura digital abierta y capacidad para transportar hasta 22 misiles aire-aire.</p>
    <button class="mostrar-btn" onclick="toggleDetalles('detalles-f15')">Mostrar detalles</button>
    <div class="detalles" id="detalles-f15">
      <ul>
        <li>Primer vuelo: 1972 (F-15 Eagle) / 2021 (F-15EX)</li>
        <li>Tripulación: 1 o 2</li>
        <li>Radio de combate: 1,967 km</li>
        <li>Motor: Pratt & Whitney F100 / F110 (según variante)</li>
        <li><a href="https://es.wikipedia.org/wiki/Boeing_F-15EX_Eagle_II" target="_blank">Wikipedia</a></li>
      </ul>
    </div>
  </div>
  <div class="galeria" id="galeria-f15">
    <button class="flecha izq" onclick="cambiarImagen('galeria-f15', -1)">&lt;</button>
    <img src="images.jpeg" alt="F-15 Eagle / F-15EX" data-galeria onclick="mostrarImagenGrande(this.src)">
    <img src="images (1).jpeg" alt="F-15 Eagle 2" data-galeria class="oculto" onclick="mostrarImagenGrande(this.src)">
    <img src="McDonnell_Douglas.svg.png" alt="Logo Boeing" class="logo-fabricante" onclick="mostrarImagenGrande(this.src)">
    <button class="flecha der" onclick="cambiarImagen('galeria-f15', 1)">&gt;</button>
  </div>
</div>
  </main>

  <footer>
    © 2025 | Página creada por Vicente | Proyecto Universidad
  </footer>
</body>
</html>
