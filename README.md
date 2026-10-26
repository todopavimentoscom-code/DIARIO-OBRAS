<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Control de Obra - Pavimentos Industriales</title>
  <style>
    :root {
      --bg: #0f172a;
      --card-bg: #1e293b;
      --accent: #2563eb;
      --accent-hover: #1d4ed8;
      --success: #16a34a;
      --warning: #ca8a04;
      --danger: #dc2626;
      --text: #f8fafc;
      --text-muted: #94a3b8;
      --border: #334155;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      touch-action: manipulation;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      padding-bottom: 40px;
    }

    header {
      background: #090d16;
      padding: 16px;
      text-align: center;
      border-bottom: 2px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    header h1 {
      font-size: 1.2rem;
      color: #38bdf8;
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 16px;
    }

    .card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 16px;
      border: 1px solid var(--border);
    }

    .card h2 {
      font-size: 1.1rem;
      margin-bottom: 14px;
      color: #38bdf8;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    label {
      display: block;
      font-size: 0.9rem;
      color: var(--text-muted);
      margin-bottom: 6px;
      font-weight: bold;
    }

    select, input, textarea {
      width: 100%;
      padding: 14px;
      border-radius: 8px;
      border: 1px solid var(--border);
      background: #0f172a;
      color: var(--text);
      font-size: 1rem;
      margin-bottom: 14px;
    }

    select:focus, input:focus, textarea:focus {
      outline: 2px solid var(--accent);
    }

    .grid-2 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }

    .btn {
      width: 100%;
      padding: 16px;
      border: none;
      border-radius: 10px;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: background 0.2s;
    }

    .btn-primary { background: var(--accent); color: white; }
    .btn-success { background: var(--success); color: white; }
    .btn-warning { background: var(--warning); color: white; }
    .btn-danger { background: var(--danger); color: white; }

    .btn-option {
      background: #0f172a;
      border: 2px solid var(--border);
      color: var(--text);
      padding: 14px;
      text-align: left;
      border-radius: 8px;
      margin-bottom: 8px;
      font-size: 0.95rem;
    }

    .btn-option.selected {
      border-color: var(--accent);
      background: #1e3a8a;
    }

    .obra-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #0f172a;
      padding: 10px 14px;
      border-radius: 8px;
      margin-bottom: 8px;
      border: 1px solid var(--border);
    }

    .obra-item button {
      background: var(--danger);
      color: white;
      border: none;
      padding: 6px 12px;
      border-radius: 6px;
      cursor: pointer;
    }

    .badge {
      background: #38bdf8;
      color: #0f172a;
      padding: 4px 8px;
      border-radius: 4px;
      font-weight: bold;
      font-size: 0.8rem;
    }

    #screen-lock {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: var(--bg);
      z-index: 1000;
      padding: 24px;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .hidden { display: none !important; }
  </style>
</head>
<body>

  <!-- PANTALLA INICIAL DE IDENTIFICACIÓN -->
  <div id="screen-lock">
    <div style="text-align: center; margin-bottom: 24px;">
      <h1 style="color: #38bdf8; font-size: 1.6rem;">PAVIMENTOS INDUSTRIALES</h1>
      <p style="color: var(--text-muted); margin-top: 8px;">Selecciona tu nombre para comenzar</p>
    </div>
    
    <label for="select-worker-init">¿Quién eres?</label>
    <select id="select-worker-init">
      <option value="">-- Elige tu nombre --</option>
      <option value="DAVID">DAVID</option>
      <option value="EVGENII">EVGENII</option>
      <option value="JORGE">JORGE</option>
      <option value="ALVARO">ALVARO</option>
      <option value="GONZALO">GONZALO</option>
      <option value="FIDEL">FIDEL</option>
      <option value="JAVIER">JAVIER</option>
      <option value="FERNANDO">FERNANDO</option>
      <option value="MYKHAILO">MYKHAILO</option>
    </select>

    <button class="btn btn-primary" onclick="confirmIdentity()">ENTRAR AL PARTE</button>
  </div>

  <!-- CABECERA PRINCIPAL -->
  <header>
    <h1>PARTE DE OBRA DIARIO</h1>
    <span id="display-user" class="badge" style="margin-top: 4px; display: inline-block;"></span>
  </header>

  <div class="container">
    
    <!-- PASO 1: SELECCIÓN DE OBRA Y FECHA -->
    <div class="card">
      <h2>1. Fecha y Obra</h2>
      
      <label>Fecha del Parte</label>
      <input type="date" id="input-fecha">

      <label>Obra Asignada</label>
      <select id="select-obra"></select>

      <button class="btn-option" style="background: transparent; border: 1px dashed var(--accent); color: #38bdf8;" onclick="toggleGestionObras()">
        ⚙️ Añadir / Eliminar Obras de la Lista
      </button>

      <!-- GESTIÓN DINÁMICA DE OBRAS -->
      <div id="section-obras-manager" class="hidden" style="margin-top: 14px; padding-top: 14px; border-top: 1px solid var(--border);">
        <label>Nueva Obra</label>
        <div style="display: flex; gap: 8px;">
          <input type="text" id="nueva-obra-nombre" placeholder="Nombre / Código de Obra" style="margin: 0;">
          <button class="btn btn-primary" style="width: auto; white-space: nowrap;" onclick="agregarObra()">Añadir</button>
        </div>
        <div id="lista-obras-container" style="margin-top: 14px;"></div>
      </div>
    </div>

    <!-- PASO 2: PREGUNTA CLAVE DE ASISTENCIA A LA OBRA -->
    <div class="card">
      <h2>2. Asistencia a la Obra</h2>
      <p style="margin-bottom: 12px; font-size: 0.95rem;">¿Has estado en esta obra todo el tiempo hoy?</p>
      
      <div class="grid-2">
        <button id="btn-obra-si" class="btn btn-option" onclick="setEstuvoEnObra(true)">SI, he estado</button>
        <button id="btn-obra-no" class="btn btn-option" onclick="setEstuvoEnObra(false)">NO he estado</button>
      </div>

      <!-- SI NO HA ESTADO EN LA OBRA -->
      <div id="motivo-no-obra" class="hidden" style="margin-top: 14px;">
        <label>Motivo por el que NO has estado:</label>
        <button class="btn-option motivo-btn" onclick="setMotivoNoObra('Vacaciones')">🏖️ Vacaciones</button>
        <button class="btn-option motivo-btn" onclick="setMotivoNoObra('Baja médica')">🏥 Baja Médica</button>
        <button class="btn-option motivo-btn" onclick="setMotivoNoObra('En nave / almacén')">🏭 En nave / Almacén</button>
        <button class="btn-option motivo-btn" onclick="setMotivoNoObra('Mantenimiento / Taller')">🛠️ Taller / Vehículo</button>
        <button class="btn-option motivo-btn" onclick="setMotivoNoObra('Festivo / Permiso')">📅 Festivo / Permiso</button>
        <textarea id="motivo-no-obra-otro" placeholder="Otro motivo explicativo..." rows="2" style="margin-top: 6px;"></textarea>
      </div>
    </div>

    <!-- PASO 3: TRABAJO REALIZADO -->
    <div id="paso-cumplimiento" class="card hidden">
      <h2>3. Cumplimiento de lo Previsto</h2>
      <p style="margin-bottom: 12px; font-size: 0.95rem;">¿Se ha realizado todo lo planificado para hoy?</p>
      
      <div class="grid-2">
        <button id="btn-plan-si" class="btn btn-option" onclick="setCumplioPlan(true)">SI, todo hecho</button>
        <button id="btn-plan-no" class="btn btn-option" onclick="setCumplioPlan(false)">NO se terminó</button>
      </div>

      <div id="motivo-no-plan" class="hidden" style="margin-top: 14px;">
        <label>Explica por qué no se ha podido realizar todo lo previsto:</label>
        <textarea id="texto-no-plan" rows="3" placeholder="Falta de soporte preparado, fallo de maquinaria, corte de luz..."></textarea>
      </div>

      <div style="margin-top: 14px;">
        <label>Detalle de lo que SE HA HECHO hoy (m2, capas, etc.):</label>
        <textarea id="texto-trabajo-hecho" rows="3" placeholder="Ejemplo: Granallado y aplicación de imprimación en sector 1 (450 m2)..."></textarea>
      </div>
    </div>

    <!-- PASO 4: MATERIAL GASTADO -->
    <div id="paso-materiales" class="card hidden">
      <h2>4. Material Gastado en Obra</h2>
      <p style="font-size: 0.85rem; color: var(--text-muted); margin-bottom: 12px;">Indica resinas, catalizadores, áridos o poliuretanos consumidos hoy:</p>
      
      <textarea id="texto-materiales" rows="4" placeholder="Ejemplo:&#10;- 5 Botes Comp A + 5 Botes Comp B&#10;- 10 Sacos Árido Cuarzo 0.4&#10;- 2 Botes Topcoat Poliuretano"></textarea>
    </div>

    <!-- PASO 5: ENVÍO FINAL DE REPORTE -->
    <div class="card">
      <h2>5. Enviar Reporte</h2>
      <p style="font-size: 0.85rem; color: var(--text-muted); margin-bottom: 14px;">El reporte se enviará por WhatsApp directamente a Vanessa y Javier.</p>
      
      <button class="btn btn-success" style="font-size: 1.1rem; padding: 18px;" onclick="enviarWhatsApp()">
        📲 ENVIAR PARTE A SUPERVISORES
      </button>
    </div>

  </div>

  <script>
    const SUPERVISORES = [
      { nombre: "Vanessa", tel: "34621011991" },
      { nombre: "Javier", tel: "34653637676" }
    ];

    let usuarioActual = "";
    let estuvoEnObra = null;
    let motivoNoObraSel = "";
    let cumplioPlan = null;
    let obrasList = ["Obra Nave Polígono Central", "Obra Nave Logística", "Obra Parking Centro"];

    window.onload = function() {
      document.getElementById('input-fecha').value = new Date().toISOString().split('T')[0];
      const obrasGuardadas = localStorage.getItem('pav_obras');
      if (obrasGuardadas) {
        obrasList = JSON.parse(obrasGuardadas);
      }
      renderObrasSelect();
      renderObrasManager();
    };

    function confirmIdentity() {
      const val = document.getElementById('select-worker-init').value;
      if (!val) {
        alert("Por favor, selecciona tu nombre.");
        return;
      }
      usuarioActual = val;
      document.getElementById('display-user').innerText = "TRABAJADOR: " + usuarioActual;
      document.getElementById('screen-lock').classList.add('hidden');
    }

    function toggleGestionObras() {
      const sec = document.getElementById('section-obras-manager');
      sec.classList.toggle('hidden');
    }

    function renderObrasSelect() {
      const sel = document.getElementById('select-obra');
      sel.innerHTML = "";
      obrasList.forEach((o) => {
        const opt = document.createElement('option');
        opt.value = o;
        opt.innerText = o;
        sel.appendChild(opt);
      });
    }

    function renderObrasManager() {
      const container = document.getElementById('lista-obras-container');
      container.innerHTML = "";
      obrasList.forEach((o, index) => {
        const div = document.createElement('div');
        div.className = "obra-item";
        div.innerHTML = `
          <span>${o}</span>
          <button onclick="eliminarObra(${index})">Eliminar</button>
        `;
        container.appendChild(div);
      });
    }

    function agregarObra() {
      const input = document.getElementById('nueva-obra-nombre');
      const nombre = input.value.trim();
      if (!nombre) return;
      obrasList.push(nombre);
      input.value = "";
      guardarObras();
    }

    function eliminarObra(index) {
      obrasList.splice(index, 1);
      guardarObras();
    }

    function guardarObras() {
      localStorage.setItem('pav_obras', JSON.stringify(obrasList));
      renderObrasSelect();
      renderObrasManager();
    }

    function setEstuvoEnObra(val) {
      estuvoEnObra = val;
      document.getElementById('btn-obra-si').classList.toggle('selected', val === true);
      document.getElementById('btn-obra-no').classList.toggle('selected', val === false);

      const divMotivoNoObra = document.getElementById('motivo-no-obra');
      const pasoCumplimiento = document.getElementById('paso-cumplimiento');
      const pasoMateriales = document.getElementById('paso-materiales');

      if (val === false) {
        divMotivoNoObra.classList.remove('hidden');
        pasoCumplimiento.classList.add('hidden');
        pasoMateriales.classList.add('hidden');
      } else {
        divMotivoNoObra.classList.add('hidden');
        pasoCumplimiento.classList.remove('hidden');
        pasoMateriales.classList.remove('hidden');
      }
    }

    function setMotivoNoObra(motivo) {
      motivoNoObraSel = motivo;
      document.querySelectorAll('.motivo-btn').forEach(b => {
        b.classList.remove('selected');
        if (b.innerText.includes(motivo)) b.classList.add('selected');
      });
    }

    function setCumplioPlan(val) {
      cumplioPlan = val;
      document.getElementById('btn-plan-si').classList.toggle('selected', val === true);
      document.getElementById('btn-plan-no').classList.toggle('selected', val === false);

      const divMotivoPlan = document.getElementById('motivo-no-plan');
      if (val === false) {
        divMotivoPlan.classList.remove('hidden');
      } else {
        divMotivoPlan.classList.add('hidden');
      }
    }

    function enviarWhatsApp() {
      if (!usuarioActual) {
        alert("Debes seleccionar tu nombre primero.");
        return;
      }
      if (estuvoEnObra === null) {
        alert("Por favor responde si has estado en la obra todo el tiempo.");
        return;
      }

      const fecha = document.getElementById('input-fecha').value;
      const obra = document.getElementById('select-obra').value;

      let msg = `*PARTE DIARIO DE OBRA*\n`;
      msg += `👤 *Trabajador:* ${usuarioActual}\n`;
      msg += `📅 *Fecha:* ${fecha}\n`;
      msg += `🏗️ *Obra:* ${obra}\n`;
      msg += `----------------------------------------\n`;

      if (estuvoEnObra === false) {
        let motivoFinal = motivoNoObraSel;
        const otroText = document.getElementById('motivo-no-obra-otro').value.trim();
        if (otroText) motivoFinal += ` (${otroText})`;

        msg += `❌ *¿Estuvo en la obra?:* NO\n`;
        msg += `📌 *Motivo:* ${motivoFinal || 'No especificado'}\n`;
      } else {
        msg += `✅ *¿Estuvo en la obra?:* SI (Todo el tiempo)\n\n`;

        if (cumplioPlan === true) {
          msg += `🎯 *¿Cumplió lo previsto?:* SI\n`;
        } else if (cumplioPlan === false) {
          const causa = document.getElementById('texto-no-plan').value.trim();
          msg += `⚠️ *¿Cumplió lo previsto?:* NO\n`;
          msg += `❓ *Causa:* ${causa || 'Sin especificar'}\n`;
        }

        const trabajoHecho = document.getElementById('texto-trabajo-hecho').value.trim();
        if (trabajoHecho) {
          msg += `🔨 *Trabajo Realizado:* ${trabajoHecho}\n`;
        }

        const material = document.getElementById('texto-materiales').value.trim();
        if (material) {
          msg += `\n📦 *Material Gastado:*\n${material}\n`;
        }
      }

      const textoEncoded = encodeURIComponent(msg);
      
      SUPERVISORES.forEach((sup, idx) => {
        setTimeout(() => {
          window.open(`https://api.whatsapp.com/send?phone=${sup.tel}&text=${textoEncoded}`, '_blank');
        }, idx * 600);
      });
    }
  </script>
</body>
</html>
