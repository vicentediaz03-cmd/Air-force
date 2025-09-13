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
</head>
<body>
  <header>
    <h1>FIGHTERS AIR FORCE</h1>
    <input class="buscador" id="buscador" type="text" placeholder="Buscar avión..." oninput="filtrarAviones()">
  </header>

  <div class="video-container">
    <video controls>
      <source src="ssstik.io_1757720937426.mp4" type="video/mp4">
      Tu navegador no soporta el video.
    </video>
  </div>

  <main>
    <!-- Aquí tus bloques de aviones (Su-57, F-22, J-20, F-15) con los cambios aplicados -->
  </main>

  <footer>
    © 2025 | Página creada por Vicente | Proyecto Universidad
  </footer>
</body>
</html>
