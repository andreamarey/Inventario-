<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Inventario — EMBARQUES</title>
<!-- Librerías -->
<script src="https://unpkg.com/html5-qrcode"></script>
<script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<style>
/* Tipografía (fallback si no agregas el <link> en <head>) */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');

/* =====================  Variables de diseño  ===================== */
:root{
  /* Paleta base (del usuario) */
  --teal-600:#468B83;   /* principal */
  --blue-600:#46718B;   /* acento 1 */
  --indigo-600:#464E8B; /* acento 2 */

  /* Derivados y neutros */
  --primary: var(--teal-600);
  --primary-700: #3b756f;
  --primary-50:  #ecf7f5;

  --bg:#f5f7fb;
  --card:#ffffff;
  --text:#111827;
  --muted:#6b7280;
  --border:#e5e7eb;
  --shadow:0 6px 20px rgba(0,0,0,.06);
  --ring: rgba(70,139,131,.25);

  /* NUEVO: Colores de estado */
  --success:#10b981;
  --success-50:#ecfdf5;
  --warning:#f59e0b;
  --warning-50:#fffbeb;
  --danger:#ef4444;
  --danger-600:#dc2626;
  --danger-50:#fef2f2;

  /* Hover fila tabla */
  --row-hover:#f1f5f9;
}

/* =====================  Reset básico  ===================== */
*{ box-sizing:border-box }
html,body{ height:100% }
body{
  margin:0;
  background: var(--bg);
  color: var(--text);
  font-family: "Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
  font-weight: 400;
  letter-spacing:.2px;
}

/* =====================  Header  ===================== */
header{
  background: linear-gradient(135deg,var(--teal-600), var(--blue-600));
  color:#fff;
  padding:28px 20px;
  text-align:center;
  font-weight:700;
  letter-spacing:.5px;
  box-shadow: var(--shadow);
  border-bottom:4px solid var(--indigo-600);
  position: relative;
}

header h1{
  margin:0;
  font-size:2rem;
  font-family:"Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
  text-transform: uppercase;
  text-shadow: 0 2px 6px rgba(0,0,0,.25);
  letter-spacing:1px;
}

header span.sub{
  display:block;
  margin-top:6px;
  font-size:0.9rem;
  font-weight:400;
  letter-spacing:.5px;
  color: rgba(255,255,255,.85);
}

/* =====================  Contenedor  ===================== */
.container{
  max-width:1200px;
  margin:28px auto;
  padding: 0 20px;
}

/* =====================  Tabs (inactivas texto negro / activa blanca)  ===================== */
.tabs{
  display:flex; gap:10px; flex-wrap:wrap; margin-bottom:14px;
}
.tab-btn{
  flex:1; min-width:160px;
  padding:12px 14px;
  background:#f3f4f6;       /* inactiva */
  color:#111;               /* texto negro en inactiva */
  border:1px solid var(--border);
  border-radius:10px;
  font-weight:600;
  cursor:pointer;
  transition: all .18s ease;
}
.tab-btn:hover{
  background:#fff;
  box-shadow: 0 2px 10px rgba(0,0,0,.05);
}
.tab-btn.active{
  background:#fff;          /* activa blanca */
  color: var(--primary-700);/* texto con matiz del primario */
  border-color: var(--primary);
  box-shadow: 0 6px 16px rgba(70,139,131,.15);
}

/* =====================  Tarjetas / secciones  ===================== */
.card{
  background: var(--card);
  border: 1px solid var(--border);
  border-radius:14px;
  padding: 24px;
  margin-bottom: 22px;
  box-shadow: var(--shadow);
}

/* =====================  Tipografía / espaciados  ===================== */
h2{
  margin:0 0 12px 0;
  font-size: 1.25rem;
  font-weight:700;
  letter-spacing:.2px;
}
h3{
  margin:18px 0 10px 0;
  font-size: 1rem;
  font-weight:600;
}
.muted{ color: var(--muted); font-size:.92rem; }

/* =====================  Layout de formularios  ===================== */
.row{ display:flex; gap:14px; flex-wrap:wrap; margin-bottom: 12px; }

input,select,button{
  width:100%;
  padding:12px 14px;
  margin:6px 0;
  border-radius:10px;
  border:1px solid var(--border);
  background:#fff;
  color:var(--text);
  font-size:15px;
}
input:focus,select:focus{
  outline:none;
  border-color: var(--primary);
  box-shadow: 0 0 0 4px var(--ring);
}

/* =====================  Botones  ===================== */
button{
  background: var(--primary);
  color:#fff;
  border:none;
  font-weight:700;
  cursor:pointer;
  border-radius:10px;
  padding:12px 14px;
  transition:
    transform .06s ease,
    box-shadow .2s ease,
    background .2s ease,
    filter .2s ease;
}
button:hover{
  filter: brightness(0.97);
  box-shadow:0 6px 16px rgba(0,0,0,.08);
}
button:active{ transform: translateY(1px); }
button:focus-visible{
  outline: none;
  box-shadow: 0 0 0 4px var(--ring);
}

/* Variantes */
.btn-secondary{ background: var(--blue-600); }
.btn-secondary:hover{ filter: brightness(0.98); }

.btn-danger{ background: var(--danger); }
.btn-danger:hover{ background: var(--danger-600); }

/* Botones de borrar (mantengo tus clases) */
.btn-del, .btn-del-mov{
  background: var(--danger);
  color:#fff; border:none;
  padding:8px 12px;
  border-radius:10px;
  font-size:12.5px;
  font-weight:700;
}
.btn-del:hover, .btn-del-mov:hover{ background: var(--danger-600); }

/* Iconos dentro de botones */
button i { margin-right: 6px; }

/* =====================  Tablas  ===================== */
.inv-table, .mov-table{
  width:100%;
  border-collapse: collapse;
  margin-top:10px;
  font-size:14px;
  background:#fff;
  border:1px solid var(--border);
  border-radius:12px;
  overflow:hidden;
  box-shadow: var(--shadow);
}
.inv-table th, .inv-table td,
.mov-table th, .mov-table td{
  border-bottom:1px solid var(--border);
  padding:10px 12px;
  text-align:left;
}
.inv-table th, .mov-table th{
  background: var(--primary-50);
  color:#0f172a;
  font-weight:700;
}
.inv-table th, .mov-table th{
  position: sticky; /* opcional, requiere contenedor con overflow */
  top: 0;
  z-index: 1;
}
.inv-table tbody tr:nth-child(even),
.mov-table tbody tr:nth-child(even){
  background:#fafafa;
}
.inv-table td:last-child, .mov-table td:last-child{ text-align:center; }
.mov-table td:nth-last-child(3){ text-align:right; } /* columna Δ */
/* =====================  Chips para "Tipo" en historial  ===================== */
.chip{
  display:inline-block;
  padding:2px 8px;
  border-radius:999px;
  font-size:12px;
  line-height:1.6;
  font-weight:700;
  border:1px solid transparent;
}

.chip-register{
  background: var(--success-50);
  color: var(--success);
  border-color: rgba(16,185,129,.25);
}
.chip-import{
  background: var(--warning-50);
  color: var(--warning);
  border-color: rgba(245,158,11,.25);
}
.chip-scan{
  background: var(--primary-50);
  color: var(--primary-700);
  border-color: rgba(70,139,131,.25);
}

/* Hover de filas en tablas (más feedback visual) */
.inv-table tbody tr:hover,
.mov-table tbody tr:hover{
  background: var(--row-hover);
}

/* =====================  Etiqueta / vista previa (SIN LOGO) ===================== */
.label-preview{
  border:1.5px solid #111;
  padding:16px 16px 12px;
  width:380px;                /* ancho de preview en pantalla */
  background:#fff;
  border-radius:10px;
  box-shadow: 0 4px 14px rgba(0,0,0,.08);
  font-family: Arial, sans-serif;
  margin: 0 auto;             /* centrada en el contenedor */
}

.label-title{
  font-size:18px;
  font-weight:800;
  text-align:center;
  letter-spacing:.5px;
  margin:0 0 6px;
}
.label-sub{
  font-size:14px;
  font-weight:600;
  margin:0 0 8px;
  text-align:center;
}
.label-divider{
  height:2px;
  background:#111;
  margin:6px 0 10px;
  border-radius:2px;
}

.label-row{
  display:flex;
  gap:8px;
  font-size:14px;
  margin:6px 0;
  align-items:flex-end;
}
.label-row .k{
  width:88px;
  font-weight:800;
}
.label-row .v{
  flex:1;
  min-height:18px;            /* deja “hueco” visible si está vacío */
  border-bottom:1px dotted #999;
  padding-bottom:2px;
  word-break: break-word;
}

.barcode-wrap{
  margin-top:12px;
  text-align:center;
}
.code-text{
  font-size:12px;
  margin-top:6px;
  letter-spacing:1px;
}
.label-title{
  font-size:18px;
  font-weight:800;
}
/* =====================  Tarjetas de historial  ===================== */
.group{ margin:12px 0 }
.group h3{ margin:0 0 8px 0; font-size:1rem }
.mov{ padding:8px 10px; border:1px solid var(--border); border-radius:12px; margin:6px 0; background:#fff }
.kv{ font-family: ui-monospace, Menlo, Consolas, monospace; font-size:.92rem }

/* =====================  Flash de escaneo  ===================== */
#scanFlash{
  position: fixed; top:16px; right:16px; z-index:9999;
  padding:12px 14px; border-radius:12px; font-weight:700; color:#fff;
  display:none; opacity:0; transition:opacity .25s ease;
  box-shadow: 0 10px 24px rgba(0,0,0,.18);
}
#scanFlash.ok{ background:#10b981; }
#scanFlash.err{ background:#ef4444; }
/* Visibilidad para tabs */
.hidden { display: none !important; }
button i {
  margin-right: 6px;
}
</style>
</head>
<body>
<header>
  <h1>Inventario — Embarques</h1>
  <span class="sub">Control y seguimiento de muebles</span>
</header>
<div class="container">

<!-- Tabs -->
<div class="tabs">
  <button class="tab-btn active" data-tab="registrar">Registrar</button>
  <button class="tab-btn" data-tab="escanear">Escanear</button>
  <button class="tab-btn" data-tab="inventario">Inventario</button>
  <button class="tab-btn" data-tab="conteo">Conteo mensual</button>
  <button class="tab-btn" data-tab="etiquetas">Etiquetas</button>
  <button class="tab-btn" data-tab="config">Configuración y Herramientas</button>
</div>

  <!-- Registrar -->
  <section id="registrar" class="card">
    <h2>Registrar producto</h2>
    <div class="row">
      <input id="newName" placeholder="Modelo (ej. Sillón Roma)">
      <input id="newFabric" placeholder="Tela (ej. Lino Beige)">
      <input id="newSKU" placeholder="SKU (opcional, ej. SR-LB)">
    </div>
<button id="btnGenerate"><i class="fa fa-plus"></i> Guardar / Sumar</button>
<div class="muted">
  Si el <b>SKU</b> está vacío, se busca por <b>Modelo + Tela</b> (sin mezclar con los que sí tienen SKU).
  Si el <b>SKU</b> tiene valor, se usa <b>SKU + Modelo + Tela</b>.
</div>
  </section>

  <!-- Escanear -->
  <section id="escanear" class="card hidden">
    <h2>Escanear (QR + Code128)</h2>
    <div class="row">
      <select id="cameraSelect" title="Selecciona cámara"></select>
<button id="btnStart" class="btn-secondary"><i class="fa fa-camera"></i> Iniciar cámara</button>
<button id="btnStop" class="btn-secondary"><i class="fa fa-stop"></i> Detener cámara</button>
    </div>
    <div id="reader" style="width:100%; max-width:420px; margin:8px auto;"></div>
    <div class="row">
      <input id="scanCode" placeholder="Código detectado (KLL o SKU)" readonly>
      <input id="scanName" placeholder="Modelo" readonly>
      <input id="scanFabric" placeholder="Tela" readonly>
      <input id="scanSKU" placeholder="SKU" readonly>
      <input id="scanQty" placeholder="Cantidad total" readonly>
    </div>
    <div class="row">
    <button id="btnPlus1" class="btn-secondary"><i class="fa fa-plus"></i> +1</button>
      <input id="plusN" type="number" min="1" value="5" style="max-width:140px">
<button id="btnPlusN" class="btn-secondary"><i class="fa fa-layer-group"></i> +N</button>
    </div>
    <div class="muted">Tras detectar, se pausa 1.2s para evitar dobles lecturas.</div>
  </section>

  <!-- Inventario -->
  <section id="inventario" class="card hidden">
    <h2>Inventario</h2>

    <div class="row">
      <input id="searchInput" placeholder="Buscar por modelo, tela, SKU o código" oninput="renderInventory(); renderHistory();">
    </div>

    <div class="row" style="margin-top:6px">
      <label style="flex:1">Desde: <input type="date" id="dateFrom" onchange="renderInventory(); renderHistory();"></label>
      <label style="flex:1">Hasta: <input type="date" id="dateTo" onchange="renderInventory(); renderHistory();"></label>
    </div>

    <div class="row" style="margin-top:6px">
  <button onclick="exportFilteredTotals()">
    <i class="fa fa-file-export"></i> Exportar CSV (Totales Filtrados)
  </button>
  <button onclick="exportFilteredHistory()" class="btn-secondary">
    <i class="fa fa-file-export"></i> Exportar CSV (Historial Filtrado)
  </button>
</div>

  <h3 style="margin-top:14px">Totales (filtrados)</h3>

<table class="inv-table">
  <thead>
    <tr>
      <th>Modelo</th>
      <th>Tela</th>
      <th>SKU</th>
      <th>Código</th>
      <th style="text-align:right">Cantidad</th>
      <th>Fecha</th>
      <th style="text-align:center">Acciones</th>
    </tr>
  </thead>
  <tbody id="inventoryTableBody">
    <!-- Se llena dinámicamente -->
  </tbody>
</table>

   <h3 style="margin-top:14px">Historial (movimientos filtrados)</h3>
<table class="mov-table">
  <thead>
    <tr>
      <th>Fecha</th>
      <th>Hora</th>
      <th>Código</th>
      <th>SKU</th>
      <th>Modelo</th>
      <th>Tela</th>
      <th style="text-align:right">Δ</th>
      <th>Tipo</th>
      <th style="text-align:center">Acciones</th>
    </tr>
  </thead>
  <tbody id="historyTableBody">
    <!-- Se llena dinámicamente -->
  </tbody>
</table>
  </section>

  <!-- Conteo mensual -->
  <section id="conteo" class="card hidden">
    <h2>Conteo mensual</h2>
 <div class="row">
  <label style="flex:1">
    Mes a reportar:
    <input id="countMonth" type="month">
  </label>
  <input id="countLabel" placeholder="Nombre del corte (ej. Septiembre 2025)">
  <button id="btnStartCount" class="btn-secondary">Iniciar conteo</button>
  <button id="btnSaveCount">Guardar corte</button>
</div>
<div class="muted">
  Al iniciar, se calculará: <b>Stock inicial</b> (del snapshot anterior), <b>Entradas del mes</b> (de tus movimientos) y podrás editar el <b>Stock final</b> para ese mes.
  Las <b>salidas</b> se infieren como: (Inicial + Entradas) − Final.
</div>
    <div class="muted">Ajusta las cantidades observadas en piso antes de guardar.</div>
    <div id="countEditor" style="margin-top:10px"></div>
    <hr style="margin:14px 0">
    <h3>Cortes guardados</h3>
    <div id="snapshotsList" class="muted">No hay cortes guardados.</div>
  </section>

<!-- Etiquetas (BUSCADOR + SUGERENCIAS) -->
<section id="etiquetas" class="card hidden">
  <h2>Etiquetas</h2>

  <!-- fila de búsqueda + imprimir -->
  <div class="row">
    <input id="labelSearch" placeholder="Buscar por modelo, tela, SKU o código" oninput="renderLabelSuggestions()">
    <button id="btnPrint">Imprimir etiqueta</button>
  </div>

  <!-- NUEVO: campos editables -->
  <div class="row">
    <input id="labelCliente" placeholder="CLIENTE (opcional)">
    <input id="labelDestino" placeholder="DESTINO (opcional)">
  </div>

  <div id="labelSuggestions" class="muted" style="margin-top:6px">Escribe para ver sugerencias…</div>
  <div id="previewWrap" style="margin-top:12px"></div>
  <div class="muted" style="margin-top:6px">
    Formato: Encabezado fijo · Cliente (editable) · Línea · Modelo · Tela · SKU · Destino (editable) · Fecha (impresión) · Código de barras con descripción.
  </div>
</section>
  <!-- Configuración y Herramientas -->
  <section id="config" class="card hidden">
    <h2>Configuración y Herramientas</h2>

    <h3>Preferencias de escaneo</h3>
    <label><input type="checkbox" id="prefBeep" checked> Sonido (beep al detectar)</label><br>
    <label><input type="checkbox" id="prefVibrate" checked> Vibración (si el dispositivo lo soporta)</label>

    <hr style="margin:14px 0">

    <h3>Importar / Exportar (CSV)</h3>
    <div class="row">
      <input type="file" id="csvFile" accept=".csv">
      <button id="btnImport" class="btn-secondary">Importar CSV (requiere columna “Código”)</button>
    </div>
    <div class="row">
      <button id="btnExportTotals">Exportar CSV (Totales)</button>
      <button id="btnExportMovs" class="btn-secondary">Exportar CSV (Movimientos)</button>
    </div>

    <hr style="margin:14px 0">

    <h3>Sesión completa (JSON)</h3>
    <div class="row">
      <button id="btnExportSession">Exportar sesión completa (JSON)</button>
      <input type="file" id="jsonSessionFile" accept=".json">
      <button id="btnImportSession" class="btn-secondary">Importar sesión completa (JSON)</button>
    </div>
    <div class="muted" style="margin-top:8px">
      Exporta/Importa <b>inventario + movimientos + contador KLL + preferencias + cortes mensuales</b> en un solo archivo.<br>
      Al <b>importar</b> se reemplaza todo el contenido actual por el del archivo.
    </div>
  </section>

</div>

<!-- Indicador visual de escaneo -->
<div id="scanFlash" aria-live="polite"></div>

<script>
/* ===== Estado & Persistencia ===== */
let items = [];     // [{code, sku, nombre, tela, cantidad, createdISO}]
let moves = [];     // [{dateISO, code, sku, nombre, tela, delta, type}]
let kllCounter = 1;
let prefs = { beep:true, vibrate:true };
let snapshots = []; // [{id,label,createdISO,totals:[...]}]
let countBuffer = [];
let selectedLabelCode = null;

/* ===== Cargar ===== */
(function load(){
  try{
    items      = JSON.parse(localStorage.getItem('items_simple')||'[]');
    moves      = JSON.parse(localStorage.getItem('moves_simple')||'[]');
    kllCounter = parseInt(localStorage.getItem('kllCounter_simple')||'1',10);
    const p    = JSON.parse(localStorage.getItem('prefs_simple')||'{}');
    prefs.beep     = p.beep!==undefined ? p.beep : true;
    prefs.vibrate  = p.vibrate!==undefined ? p.vibrate : true;
    snapshots  = JSON.parse(localStorage.getItem('snapshots_simple')||'[]');
  }catch(e){
    items=[]; moves=[]; kllCounter=1; prefs={beep:true,vibrate:true}; snapshots=[];
  }

  const chkBeep = document.getElementById('prefBeep');
  const chkVib  = document.getElementById('prefVibrate');
  if (chkBeep) chkBeep.checked = !!prefs.beep;
  if (chkVib)  chkVib.checked  = !!prefs.vibrate;

  // Asegura IDs de movimientos ya guardados (definición de ensureMoveIds más abajo en tu archivo)
  try { if (typeof ensureMoveIds === 'function') ensureMoveIds(); } catch(_){}

  // Pinta vistas iniciales
  try { renderInventory(); } catch(_){}
  try { refreshLabelSelect(); } catch(_){}
})();

/* ===== Exportaciones desde Config (botones "Totales" y "Movimientos") ===== */
(function bindConfigExports(){
  const btnTot = document.getElementById('btnExportTotals');
  const btnMov = document.getElementById('btnExportMovs');
  if (btnTot) btnTot.addEventListener('click', exportAllTotals);
  if (btnMov) btnMov.addEventListener('click', exportAllMoves);
})();

/* ===== Exportar TODO (sin filtros) ===== */
function exportAllTotals(){
  if (!Array.isArray(items) || items.length===0){
    alert("No hay datos en Totales para exportar.");
    return;
  }
  const headers = ["Modelo","Tela","SKU","Código","Cantidad","Fecha Alta"];
  const rows = [...items]
    .sort((a,b)=>(b.createdISO||"").localeCompare(a.createdISO||""))
    .map(i=>[
      i.nombre,
      i.tela,
      i.sku || "",
      i.code,
      Number(i.cantidad)||0,
      i.createdISO ? new Date(i.createdISO).toLocaleDateString() : ""
    ]);
  // downloadCSV está definido más abajo en tu archivo
  downloadCSV("inventario_totales_COMPLETO.csv", headers, rows);
}

function exportAllMoves(){
  if (!Array.isArray(moves) || moves.length===0){
    alert("No hay movimientos para exportar.");
    return;
  }
  const headers = ["Fecha","Hora","Código","SKU","Modelo","Tela","Delta","Tipo"];
  const rows = [...moves]
    .sort((a,b)=>(b.dateISO||"").localeCompare(a.dateISO||""))
    .map(m=>{
      const d = m.dateISO ? new Date(m.dateISO) : new Date();
      return [
        d.toLocaleDateString(),
        d.toLocaleTimeString(),
        m.code,
        m.sku || "",
        m.nombre,
        m.tela,
        Number(m.delta)||0,
        m.type
      ];
    });
  downloadCSV("inventario_movimientos_COMPLETO.csv", headers, rows);
}

function saveAll(){
  localStorage.setItem('items_simple', JSON.stringify(items));
  localStorage.setItem('moves_simple', JSON.stringify(moves));
  localStorage.setItem('kllCounter_simple', String(kllCounter));
  localStorage.setItem('prefs_simple', JSON.stringify(prefs));
  localStorage.setItem('snapshots_simple', JSON.stringify(snapshots));
}

/* ===== Tabs (única implementación) ===== */
window.showTab = function(id){
  // Ocultar todas las secciones
  document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
  // Mostrar sección seleccionada
  const sec = document.getElementById(id);
  if (sec) sec.classList.remove('hidden');

  // Marcar pestaña activa
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  const btn = document.querySelector(`.tab-btn[data-tab="${id}"]`);
  if (btn) btn.classList.add('active');

  // Hooks seguros por pestaña
  if (id === 'escanear'){
    try { if (typeof initCameras === 'function') initCameras(); } catch(_){}
  } else {
    try { if (typeof stopScanner === 'function') stopScanner(); } catch(_){}
  }
  if (id === 'etiquetas'){
    try { if (typeof refreshLabelSelect === 'function') refreshLabelSelect(); } catch(_){}
  }
  if (id === 'conteo'){
    try { if (typeof renderCountEditor === 'function') renderCountEditor(); } catch(_){}
    try { if (typeof renderSnapshotsList === 'function') renderSnapshotsList(); } catch(_){}
  }
};

// Inicializa pestaña y listener global (delegación)
(function initTabs(){
  // Mostrar la pestaña marcada como activa, o 'registrar' por defecto
  const initial = document.querySelector('.tab-btn.active')?.dataset.tab || 'registrar';
  showTab(initial);

  // Un solo listener para todos los .tab-btn
  document.addEventListener('click', function(ev){
    const btn = ev.target.closest('.tab-btn');
    if (!btn) return;
    const id = btn.getAttribute('data-tab');
    if (!id) return;
    ev.preventDefault();
    showTab(id);
  }, true);

  // (Opcional) Exponer función para cambiar pestañas desde HTML/JS
  window.switchTab = showTab;
})();
/* ===== Registrar ===== */
document.getElementById('btnGenerate').addEventListener('click', ()=>{
  const nombre = document.getElementById('newName').value.trim();
  const tela   = document.getElementById('newFabric').value.trim();
  const sku    = document.getElementById('newSKU').value.trim(); // ahora opcional

  // SKU ya no es obligatorio
  if(!nombre || !tela){
    alert("Completa Modelo y Tela");
    return;
  }

  // Helper seguro para comparar en minúsculas
  const lc = s => (s||'').trim().toLowerCase();
  const hasSku = sku.trim().length > 0;

  // Regla de coincidencia:
  // - Si hay SKU: match por (nombre + tela + sku)
  // - Si NO hay SKU: match por (nombre + tela) y además que ese item NO tenga SKU
  let existing = null;
  if (hasSku){
    existing = items.find(i =>
      lc(i.nombre) === lc(nombre) &&
      lc(i.tela)   === lc(tela)   &&
      lc(i.sku)    === lc(sku)
    );
  } else {
    existing = items.find(i =>
      lc(i.nombre) === lc(nombre) &&
      lc(i.tela)   === lc(tela)   &&
      lc(i.sku)    === ''         // no mezclar con los que sí tienen SKU
    );
  }

  const nowISO = new Date().toISOString();

  if(existing){
    existing.cantidad += 1;
    moves.push({
      id: genId(),
      dateISO: nowISO,
      code: existing.code,
      sku: existing.sku,           // puede venir vacío
      nombre: existing.nombre,
      tela: existing.tela,
      delta: +1,
      type: 'register'
    });
    alert(`Se sumó +1 a ${existing.nombre} (${existing.tela}) · SKU ${existing.sku||'(sin SKU)'}\nCódigo: ${existing.code}`);
  }else{
    const code = "KLL-" + String(kllCounter).padStart(4,"0");
    kllCounter++;
    items.push({ code, sku, nombre, tela, cantidad:1, createdISO:nowISO });
    moves.push({ id: genId(), dateISO: nowISO, code, sku, nombre, tela, delta:+1, type:'register' });
    alert(`Nuevo producto creado con código ${code}`);
  }

  saveAll();
  document.getElementById('newName').value = "";
  document.getElementById('newFabric').value = "";
  document.getElementById('newSKU').value = "";
  renderInventory();
  refreshLabelSelect();
});

/* ===== Inventario: filtros + export ===== */
function parseInputDate(id){
  const el = document.getElementById(id);
  if(!el || !el.value) return null;
  return new Date(el.value + "T00:00:00");
}
function inRange(iso){
  if(!iso) return true;
  const d = new Date(iso);
  const from = parseInputDate('dateFrom');
  const to   = parseInputDate('dateTo');
  if(from && d < from) return false;
  if(to){
    const end = new Date(to.getTime()); end.setHours(23,59,59,999);
    if(d > end) return false;
  }
  return true;
}
function renderInventory(){
  const tbody = document.getElementById('inventoryTableBody');
  if(!tbody) return;
  const q = (document.getElementById('searchInput')?.value || "").toLowerCase();
  tbody.innerHTML = "";

  const data = items
    .filter(i=>{
      const okQ = !q || i.nombre.toLowerCase().includes(q) || i.tela.toLowerCase().includes(q) ||
                        (i.sku||"").toLowerCase().includes(q) || i.code.toLowerCase().includes(q);
      const okD = inRange(i.createdISO);
      return okQ && okD;
    })
    .sort((a,b)=>(b.createdISO||"").localeCompare(a.createdISO||""));

  if(data.length===0){
    const tr = document.createElement('tr');
    tr.innerHTML = `<td colspan="7" class="muted" style="text-align:center">No hay resultados con el filtro actual.</td>`;
    tbody.appendChild(tr);
    // Aun así refrescamos el historial filtrado
    renderHistory();
    return;
  }

  data.forEach(i=>{
    const dateTxt = i.createdISO ? new Date(i.createdISO).toLocaleDateString() : "";
  const tr = document.createElement('tr');
tr.innerHTML = `
  <td>${escapeHtml(i.nombre)}</td>
  <td>${escapeHtml(i.tela)}</td>
  <td>${escapeHtml(i.sku||'')}</td>
  <td>${escapeHtml(i.code)}</td>
  <td style="text-align:right">${i.cantidad}</td>
  <td>${escapeHtml(dateTxt)}</td>
  <td>
    <button class="btn-del" onclick="deleteItem('${escapeJs(i.code)}')" title="Eliminar producto">
      <i class="fa fa-trash"></i> Eliminar
    </button>
  </td>
`;
tbody.appendChild(tr);
  });

  // Mantén el historial sincronizado con los mismos filtros
  renderHistory();
}
function renderHistory(){
  const tbody = document.getElementById('historyTableBody');
  if(!tbody) return;

  const q = (document.getElementById('searchInput')?.value || "").toLowerCase();
  const rows = moves
    .filter(m=>{
      const okQ = !q || m.nombre.toLowerCase().includes(q) || m.tela.toLowerCase().includes(q) ||
                        (m.sku||"").toLowerCase().includes(q) || m.code.toLowerCase().includes(q);
      const okD = inRange(m.dateISO);
      return okQ && okD;
    })
    .sort((a,b)=>(b.dateISO||"").localeCompare(a.dateISO||""));

  tbody.innerHTML = "";

  if(rows.length===0){
    const tr = document.createElement('tr');
    tr.innerHTML = `<td colspan="9" class="muted" style="text-align:center">No hay movimientos para este filtro.</td>`;
    tbody.appendChild(tr);
    return;
  }

  // Asegura IDs (por si faltó ensureMoveIds en la carga)
  let needSave = false;
  rows.forEach(m=>{
    if(!m.id){
      m.id = (typeof genId === 'function' ? genId() : ('mv_'+Date.now()+Math.random().toString(36).slice(2,7)));
      needSave = true;
    }
  });
  if(needSave) saveAll();

  rows.forEach(m=>{
    const d = m.dateISO ? new Date(m.dateISO) : new Date();
const tr = document.createElement('tr');
tr.innerHTML = `
  <td>${d.toLocaleDateString()}</td>
  <td>${d.toLocaleTimeString()}</td>
  <td>${escapeHtml(m.code)}</td>
  <td>${escapeHtml(m.sku||'')}</td>
  <td>${escapeHtml(m.nombre)}</td>
  <td>${escapeHtml(m.tela)}</td>
  <td style="text-align:right">${m.delta>0? '+'+m.delta : m.delta}</td>
<td>
  <span class="chip chip-${escapeHtml(m.type)}">
    ${escapeHtml(m.type)}
  </span>
</td>
  <td>
    <button class="btn-del-mov" onclick="deleteMove('${escapeJs(m.id)}')" title="Eliminar movimiento">
      <i class="fa fa-trash"></i> Eliminar
    </button>
  </td>
`;
tbody.appendChild(tr);
  });
}function exportFilteredTotals(){
  const q = (document.getElementById('searchInput')?.value || "").toLowerCase();
  const rows = items
    .filter(i=>{
      const okQ = !q || i.nombre.toLowerCase().includes(q) || i.tela.toLowerCase().includes(q) ||
                        (i.sku||"").toLowerCase().includes(q) || i.code.toLowerCase().includes(q);
      const okD = inRange(i.createdISO);
      return okQ && okD;
    })
    .sort((a,b)=>(b.createdISO||"").localeCompare(a.createdISO||""));
  if(rows.length===0){ alert("No hay datos para exportar con el filtro actual."); return; }

  const headers = ["Modelo","Tela","SKU","Código","Cantidad","Fecha Alta"];
  const data = rows.map(i=>[
    i.nombre, i.tela, i.sku||"", i.code, i.cantidad,
    i.createdISO ? new Date(i.createdISO).toLocaleDateString() : ""
  ]);
  downloadCSV("inventario_totales_filtrado.csv", headers, data);
}
function exportFilteredHistory(){
  const q = (document.getElementById('searchInput')?.value || "").toLowerCase();
  const rows = moves
    .filter(m=>{
      const okQ = !q || m.nombre.toLowerCase().includes(q) || m.tela.toLowerCase().includes(q) ||
                        (m.sku||"").toLowerCase().includes(q) || m.code.toLowerCase().includes(q);
      const okD = inRange(m.dateISO);
      return okQ && okD;
    })
    .sort((a,b)=>(b.dateISO||"").localeCompare(a.dateISO||""));
  if(rows.length===0){ alert("No hay movimientos para exportar con el filtro actual."); return; }

  const headers = ["Fecha","Hora","Código","SKU","Modelo","Tela","Delta","Tipo"];
  const data = rows.map(m=>{
    const d = new Date(m.dateISO||Date.now());
    return [ d.toLocaleDateString(), d.toLocaleTimeString(), m.code, m.sku||"", m.nombre, m.tela, m.delta, m.type ];
  });
  downloadCSV("inventario_historial_filtrado.csv", headers, data);
}

/* ===== Conteo mensual ===== */
document.getElementById('btnStartCount').addEventListener('click', ()=>{
  // 1) Mes objetivo
  const monthEl = document.getElementById('countMonth');
  const monthVal = (monthEl && monthEl.value) ? monthEl.value : getCurrentMonthValue(); // "YYYY-MM"
  const {start, end, label: monthLabel} = getMonthRange(monthVal);

  // 2) Snapshot anterior (stock inicial por código)
  const prevSnap = findPreviousSnapshotForMonth(monthVal);
  const inicioMap = new Map(); // code -> cantidad inicial
  if (prevSnap && Array.isArray(prevSnap.totals)) {
    prevSnap.totals.forEach(t=>{
      inicioMap.set(t.code, Number(t.cantidad)||0);
    });
  }

  // 3) Entradas del mes (solo deltas positivos) agrupadas por código
  const entradasMap = new Map(); // code -> cantidad (+)
  moves.forEach(m=>{
    if(!m.dateISO) return;
    const d = new Date(m.dateISO);
    if (d >= start && d <= end && (Number(m.delta)||0) > 0) {
      const cur = entradasMap.get(m.code)||0;
      entradasMap.set(m.code, cur + (Number(m.delta)||0));
    }
  });

  // 4) Construir buffer de conteo: unión de códigos (inicio ∪ entradas)
  const unionCodes = new Set([...inicioMap.keys(), ...entradasMap.keys()]);
  countBuffer = [];
  unionCodes.forEach(code=>{
    const it = items.find(i=>i.code===code) || moves.find(m=>m.code===code); // para datos descriptivos
    const nombre = it?.nombre || '';
    const tela   = it?.tela   || '';
    const sku    = it?.sku    || '';
    const ini    = inicioMap.get(code)   || 0;
    const ent    = entradasMap.get(code) || 0;
    const fin    = ini + ent; // valor inicial sugerido, editable
    countBuffer.push({ code, sku, nombre, tela, inicial: ini, entradas: ent, final: fin });
  });

  // Guarda meta para el guardado
  countBuffer._monthVal   = monthVal;
  countBuffer._monthStart = start.toISOString();
  countBuffer._monthEnd   = end.toISOString();
  countBuffer._monthLabel = monthLabel;

  // Si no hay registros, igual iniciamos vacío (para que puedas cargar manualmente)
  if (countBuffer.length===0) {
    // opcional: incluir productos actuales con final inicializado en 0
  }

  renderCountEditor(); // mostrará columnas: Inicial / Entradas / Final (editable) / Salidas inf.
});
document.getElementById('btnSaveCount').addEventListener('click', ()=>{
  if(!countBuffer || countBuffer.length===0){
    alert("Primero inicia un conteo del mes."); return;
  }
  // Etiqueta del corte
  let label = (document.getElementById('countLabel').value || "").trim();
  const monthLbl = countBuffer._monthLabel || '';
  if(!label) label = monthLbl ? `Corte ${monthLbl}` : `Corte ${new Date().toLocaleDateString()}`;

  // Estructura a guardar:
  // - totals: FINAL por producto (lo que contaste físicamente al cierre)
  // - meta por producto: inicial, entradas, final, salidas inf.
  const details = countBuffer.map(r=>{
    const inicial = Number(r.inicial)||0;
    const entradas = Number(r.entradas)||0;
    const final = Number(r.final)||0;
    const salidasInf = inicial + entradas - final;
    return {
      code: r.code, sku: r.sku, nombre: r.nombre, tela: r.tela,
      inicial, entradas, final, salidasInf
    };
  });

  const snap = {
    id: "snap_" + Date.now(),
    label,
    monthVal: countBuffer._monthVal || null,       // "YYYY-MM"
    monthStartISO: countBuffer._monthStart || null,
    monthEndISO: countBuffer._monthEnd || null,
    createdISO: new Date().toISOString(),
    // Para compatibilidad con lo que ya tenías:
    totals: details.map(d=>({ code:d.code, sku:d.sku, nombre:d.nombre, tela:d.tela, cantidad:d.final })),
    // Nuevo: guardamos también el desglose mensual para reportes
    monthly: details
  };

  snapshots.push(snap);
  saveAll();
  renderSnapshotsList();
  alert("Corte mensual guardado.");
});
function renderCountEditor(){
  const box = document.getElementById('countEditor');
  if(!box) return;

  if(!countBuffer || countBuffer.length===0){
    box.innerHTML = '<div class="muted">No hay datos para este mes. Inicia un conteo.</div>';
    return;
  }

  const monthLbl = countBuffer._monthLabel || '';
  let html = `
    <div class="muted">Mes: <b>${monthLbl}</b>. Edita la columna <b>Final</b> según tu conteo físico. Las <i>salidas inf.</i> se calculan como (Inicial + Entradas − Final).</div>
    <div style="overflow:auto">
    <table style="width:100%; border-collapse:collapse; margin-top:8px">
      <thead>
        <tr>
          <th style="text-align:left;  border-bottom:1px solid #e5e7eb; padding:6px">Modelo</th>
          <th style="text-align:left;  border-bottom:1px solid #e5e7eb; padding:6px">Tela</th>
          <th style="text-align:left;  border-bottom:1px solid #e5e7eb; padding:6px">SKU</th>
          <th style="text-align:left;  border-bottom:1px solid #e5e7eb; padding:6px">Código</th>
          <th style="text-align:right; border-bottom:1px solid #e5e7eb; padding:6px">Inicial</th>
          <th style="text-align:right; border-bottom:1px solid #e5e7eb; padding:6px">Entradas</th>
          <th style="text-align:right; border-bottom:1px solid #e5e7eb; padding:6px">Final</th>
          <th style="text-align:right; border-bottom:1px solid #e5e7eb; padding:6px">Salidas inf.</th>
        </tr>
      </thead>
      <tbody>
  `;

  countBuffer.forEach((r, idx)=>{
    const salInf = (Number(r.inicial)||0) + (Number(r.entradas)||0) - (Number(r.final)||0);
    html += `
      <tr>
        <td style="padding:6px">${escapeHtml(r.nombre)}</td>
        <td style="padding:6px">${escapeHtml(r.tela)}</td>
        <td style="padding:6px">${escapeHtml(r.sku||'')}</td>
        <td style="padding:6px">${escapeHtml(r.code)}</td>
        <td style="padding:6px; text-align:right">${Number(r.inicial)||0}</td>
        <td style="padding:6px; text-align:right">${Number(r.entradas)||0}</td>
        <td style="padding:6px; text-align:right">
          <input type="number" min="0" value="${Number(r.final)||0}" data-idx="${idx}" style="width:100px; text-align:right">
        </td>
        <td style="padding:6px; text-align:right" id="sal_${idx}">${salInf}</td>
      </tr>
    `;
  });

  html += `</tbody></table></div>`;
  box.innerHTML = html;

  // Listeners para recalcular salidas inferidas al editar "final"
  box.querySelectorAll('input[type="number"]').forEach(inp=>{
    inp.addEventListener('input', ()=>{
      const i = parseInt(inp.getAttribute('data-idx'));
      let v = parseInt(inp.value||"0");
      if(Number.isNaN(v) || v<0) v = 0;
      countBuffer[i].final = v;
      const sInf = (Number(countBuffer[i].inicial)||0) + (Number(countBuffer[i].entradas)||0) - v;
      const cell = document.getElementById('sal_'+i);
      if(cell) cell.textContent = sInf;
    });
  });
}
function renderSnapshotsList(){
  const box = document.getElementById('snapshotsList');
  if(!box) return;
  if(!snapshots || snapshots.length===0){
    box.innerHTML = '<div class="muted">No hay cortes guardados.</div>';
    return;
  }

  let html = "";
  const arr = [...snapshots].sort((a,b)=>(b.createdISO||"").localeCompare(a.createdISO||""));
  arr.forEach(s=>{
    const dateTxt = new Date(s.createdISO).toLocaleString();

    // Total piezas (final) — compatible con tu formato anterior
    const totalPzasFinal = Array.isArray(s.totals)
      ? s.totals.reduce((acc,t)=>acc+(parseInt(t.cantidad)||0),0)
      : 0;

    // >>> NUEVO: totales de entradas y salidas inferidas (si el snapshot trae "monthly")
    const entradasTot = Array.isArray(s.monthly)
      ? s.monthly.reduce((a,x)=>a+(Number(x.entradas)||0),0)
      : 0;
    const salidasInfTot = Array.isArray(s.monthly)
      ? s.monthly.reduce((a,x)=>a+(Number(x.salidasInf)||0),0)
      : 0;

    // Etiqueta de mes (si existe)
    const monthLabel = s.monthVal
      ? new Date(s.monthVal + "-01T00:00:00Z").toLocaleDateString(undefined, {year:'numeric', month:'long'})
      : null;

    html += `
      <div class="mov">
        <div>
          <b>${escapeHtml(s.label)}</b>
          ${monthLabel ? ` · <span class="muted">${escapeHtml(monthLabel)}</span>` : ""}
          · ${dateTxt}
          · <b>Total piezas (final): ${totalPzasFinal}</b>
          ${Array.isArray(s.monthly)
            ? ` · Entradas mes: <b>${entradasTot}</b> · Salidas inf.: <b>${salidasInfTot}</b>`
            : ""
          }
        </div>
        <div style="margin-top:6px" class="row">
          <button class="btn-secondary" data-exp="${s.id}">Exportar CSV</button>
          <button class="btn-secondary" data-view="${s.id}">Ver detalle</button>
          <button class="btn-danger" data-del="${s.id}">Eliminar</button>
        </div>
      </div>
    `;
  });
  box.innerHTML = html;

  // Listeners
  box.querySelectorAll('[data-exp]').forEach(btn=>{
    btn.addEventListener('click', ()=> exportSnapshotCSV(btn.getAttribute('data-exp')));
  });
  box.querySelectorAll('[data-view]').forEach(btn=>{
    btn.addEventListener('click', ()=> viewSnapshot(btn.getAttribute('data-view')));
  });
  box.querySelectorAll('[data-del]').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const id = btn.getAttribute('data-del');
      const s = snapshots.find(x=>x.id===id);
      if(!s) return;
      if(!confirm(`¿Eliminar el corte "${s.label}"?`)) return;
      snapshots = snapshots.filter(x=>x.id!==id);
      saveAll(); renderSnapshotsList();
    });
  });
}
function exportSnapshotCSV(id){
  const s = snapshots.find(x=>x.id===id);
  if(!s){ alert("Corte no encontrado"); return; }

  // Si el snapshot tiene desglose mensual, exportamos el formato nuevo:
  if (Array.isArray(s.monthly) && s.monthly.length > 0){
    const headers = ["Modelo","Tela","SKU","Código","Inicial","Entradas","Final","Salidas inferidas","Corte","Mes","Fecha de corte"];
    const monthLabel = s.monthVal
      ? new Date(s.monthVal + "-01T00:00:00Z").toLocaleDateString(undefined, {year:'numeric', month:'long'})
      : "";
    const rows = s.monthly.map(d=>[
      d.nombre,
      d.tela,
      d.sku || "",
      d.code,
      Number(d.inicial)||0,
      Number(d.entradas)||0,
      Number(d.final)||0,
      Number(d.salidasInf)||0,
      s.label,
      monthLabel,
      new Date(s.createdISO).toLocaleString()
    ]);
    downloadCSV(
      `corte_${(s.label||'').replace(/\s+/g,'_')}_mensual.csv`,
      headers,
      rows
    );
    return;
  }

  // Si no hay 'monthly', usamos el formato antiguo (compatibilidad):
  const headers = ["Modelo","Tela","SKU","Código","Cantidad","Corte","Fecha de corte"];
  const rows = (Array.isArray(s.totals)? s.totals : []).map(t=>[
    t.nombre,
    t.tela,
    t.sku || "",
    t.code,
    Number(t.cantidad)||0,
    s.label,
    new Date(s.createdISO).toLocaleString()
  ]);
  downloadCSV(
    `corte_${(s.label||'').replace(/\s+/g,'_')}.csv`,
    headers,
    rows
  );
}
function viewSnapshot(id){
  const s = snapshots.find(x=>x.id===id);
  if(!s){ alert("Corte no encontrado"); return; }
  const top = `${s.label} — ${new Date(s.createdISO).toLocaleString()}`;
  let txt = top + "\n\n";
  s.totals.slice(0,60).forEach(t=>{
    txt += `${t.nombre} · ${t.tela} · SKU ${t.sku||''} · ${t.code} · Cant: ${t.cantidad}\n`;
  });
  if(s.totals.length>60) txt += `\n... (${s.totals.length-60} más)`;
  alert(txt);
}

/* ===== Escáner (Html5Qrcode - cámara) ===== */
let h5 = null;
let currentCamId = null;
let lastText = "";
let resumeTimer = null;

document.getElementById('btnStart').addEventListener('click', startScanner);
document.getElementById('btnStop').addEventListener('click', stopScanner);

async function initCameras(){
  const select  = document.getElementById('cameraSelect');
  const btnStart = document.getElementById('btnStart');
  if (!select) return;

  // Limpia estado
  select.innerHTML = "";
  btnStart?.setAttribute('disabled','');

  try{
    const cams = await Html5Qrcode.getCameras();

    if (!cams || cams.length === 0){
      // Sin cámaras
      const opt = document.createElement('option');
      opt.value = ""; opt.textContent = "Sin cámaras detectadas";
      select.appendChild(opt);
      currentCamId = null;
      return;
    }

    // Poblar lista
    cams.forEach(c=>{
      const opt = document.createElement('option');
      opt.value = c.id; opt.textContent = c.label || c.id;
      select.appendChild(opt);
    });

    // Elegir trasera si existe
    const rear = cams.find(c=>/back|rear|trase|environment/i.test(c.label||""));
    currentCamId = (rear ? rear.id : cams[0].id);
    select.value = currentCamId;

    // Habilitar botón iniciar
    btnStart?.removeAttribute('disabled');

    // Cambio de cámara
    select.onchange = ()=>{
      currentCamId = select.value || null;
      if (h5){ stopScanner(); startScanner(); }
    };
  }catch(e){
    console.warn(e);
    const opt = document.createElement('option');
    opt.value = ""; opt.textContent = "No se pudo acceder a cámaras";
    select.appendChild(opt);
    currentCamId = null;
  }
}
async function startScanner(){
  if(h5) return;
  const camId = currentCamId || (await Html5Qrcode.getCameras())[0]?.id;
  if(!camId){ alert("No hay cámaras disponibles."); return; }
  h5 = new Html5Qrcode("reader");
  const formats = [
    Html5QrcodeSupportedFormats.QR_CODE,
    Html5QrcodeSupportedFormats.CODE_128,
    Html5QrcodeSupportedFormats.CODE_39,
    Html5QrcodeSupportedFormats.EAN_13
  ];
  try{
    await h5.start({deviceId:{exact:camId}}, {fps:10, qrbox:250, formatsToSupport:formats}, onScanSuccess, onScanFailure);
  }catch(e){
    console.error(e);
    alert("No pude iniciar la cámara. Revisa permisos o intenta con otro navegador.");
    h5 = null;
  }
}
function stopScanner(){
  if(!h5) return;
  try{ h5.stop().then(()=>h5.clear()).catch(()=>{}); }catch(e){}
  h5 = null; lastText = ""; clearTimeout(resumeTimer);
}
function onScanFailure(){ /* silencioso */ }
function onScanSuccess(text){
  // Evita dobles lecturas por cámara
  if(text===lastText) return;
  lastText = text;
  if(h5 && h5.pause) h5.pause(true);
  clearTimeout(resumeTimer);
  resumeTimer = setTimeout(()=>{ if(h5 && h5.resume) h5.resume(); lastText=""; }, 1200);

  // Reutiliza la misma lógica que el escáner físico
  processScan(text);
}

/* ===== Lógica común para cámara + escáner físico ===== */
function processScan(text){
  const codeTextRaw = String(text || "");
  const codeText = normalizeScan(codeTextRaw);
  if(!codeText) return;

  // Reflejar el texto normalizado en la UI
  const codeInput = document.getElementById('scanCode');
  if(codeInput) codeInput.value = codeText;

  // Buscar por Código KLL o por SKU usando comparación normalizada
  const item = items.find(i =>
    normalizeScan(i.code) === codeText ||
    normalizeScan(i.sku || "") === codeText
  );

  if(item){
    item.cantidad += 1;
    const nowISO = new Date().toISOString();
    moves.push({
      id: genId(),
      dateISO: nowISO,
      code: item.code,
      sku: item.sku,
      nombre: item.nombre,
      tela: item.tela,
      delta: +1,
      type: 'scan'
    });

    if(prefs.beep) beep();
    if(prefs.vibrate && navigator.vibrate) navigator.vibrate(80);

    flashScan(true, `+1 ${item.nombre} (${item.tela})`);

    const n = document.getElementById('scanName');
    const f = document.getElementById('scanFabric');
    const s = document.getElementById('scanSKU');
    const q = document.getElementById('scanQty');
    if(n) n.value = item.nombre;
    if(f) f.value = item.tela;
    if(s) s.value = item.sku || "";
    if(q) q.value = item.cantidad;

    saveAll();
    renderInventory();
  }else{
    flashScan(false, 'Código no registrado');

    const n = document.getElementById('scanName');
    const f = document.getElementById('scanFabric');
    const s = document.getElementById('scanSKU');
    const q = document.getElementById('scanQty');
    if(n) n.value = "No registrado";
    if(f) f.value = "";
    if(s) s.value = "";
    if(q) q.value = "";
  }
}

/* ===== Preferencias ===== */
document.getElementById('prefBeep').addEventListener('change', (e)=>{ prefs.beep = e.target.checked; saveAll(); });
document.getElementById('prefVibrate').addEventListener('change', (e)=>{ prefs.vibrate = e.target.checked; saveAll(); });

/* ===== Etiquetas — Buscador con sugerencias ===== */
function refreshLabelSelect(){ // resetea buscador al entrar a la pestaña
  selectedLabelCode = null;
  labelCliente = "";
  labelDestino = "";

  const s = document.getElementById('labelSearch');
  const box = document.getElementById('labelSuggestions');
  const prev = document.getElementById('previewWrap');
  const c = document.getElementById('labelCliente');
  const d = document.getElementById('labelDestino');

  if (s) s.value = "";
  if (box) box.innerHTML = 'Escribe para ver sugerencias…';
  if (prev) prev.innerHTML = '';
  if (c) c.value = "";
  if (d) d.value = "";
}
// Mantiene CLIENTE y DESTINO sincronizados con la vista previa
document.getElementById('labelCliente')?.addEventListener('input', (e)=>{
  labelCliente = e.target.value;
  if (selectedLabelCode) showPreview();
});
document.getElementById('labelDestino')?.addEventListener('input', (e)=>{
  labelDestino = e.target.value;
  if (selectedLabelCode) showPreview();
});
function renderLabelSuggestions(){
  const box = document.getElementById('labelSuggestions');
  const q = (document.getElementById('labelSearch')?.value || "").toLowerCase().trim();
  box.innerHTML = "";
  if(!q){
    box.innerHTML = 'Escribe para ver sugerencias…';
    return;
  }
  const matches = items.filter(i=>{
    return i.nombre.toLowerCase().includes(q) ||
           i.tela.toLowerCase().includes(q) ||
           (i.sku||"").toLowerCase().includes(q) ||
           i.code.toLowerCase().includes(q);
  }).slice(0,20);

  if(matches.length===0){
    box.innerHTML = '<span class="muted">Sin resultados.</span>';
    return;
  }
  const ul = document.createElement('ul');
  ul.style.paddingLeft = '16px';
  matches.forEach(i=>{
    const li = document.createElement('li');
    li.style.cursor = 'pointer';
    li.innerHTML = `<b>${escapeHtml(i.nombre)}</b> · ${escapeHtml(i.tela)} · SKU ${escapeHtml(i.sku||'')} · <span class="muted">${escapeHtml(i.code)}</span>`;
    li.onclick = ()=>{
      selectedLabelCode = i.code;
      document.getElementById('labelSearch').value = `${i.nombre} · ${i.tela} · SKU ${i.sku||''} · ${i.code}`;
      showPreview();
    };
    ul.appendChild(li);
  });
  box.innerHTML = "";
  box.appendChild(ul);
}
function showPreview(){
  const wrap = document.getElementById('previewWrap');
  wrap.innerHTML = "";
  if(!selectedLabelCode) return;

  const it = items.find(i=>i.code===selectedLabelCode);
  if(!it){ wrap.innerHTML = '<div class="muted">Producto no encontrado.</div>'; return; }

  // Toma los campos editables (si existen)
  const cliente = (document.getElementById('labelCliente')?.value || "").trim();
  const destino = (document.getElementById('labelDestino')?.value || "").trim();
  const fechaImp = new Date().toLocaleDateString('es-MX', {day:'2-digit', month:'2-digit', year:'numeric'});

  const div = document.createElement('div');
  div.className="label-preview";
  div.innerHTML = `
    <div class="label-title">MUEBLES KRILL S.A. DE C.V</div>
    <div class="label-sub">CLIENTE: ${cliente ? escapeHtml(cliente) : ''}</div>
    <div class="label-divider"></div>

    <div class="label-row"><span class="k">MODELO:</span><span class="v">${escapeHtml(it.nombre)}</span></div>
    <div class="label-row"><span class="k">TELA:</span><span class="v">${escapeHtml(it.tela)}</span></div>
    <div class="label-row"><span class="k">SKU:</span><span class="v">${escapeHtml(it.sku||'')}</span></div>
    <div class="label-row"><span class="k">DESTINO:</span><span class="v">${destino ? escapeHtml(destino) : ''}</span></div>
    <div class="label-row"><span class="k">FECHA:</span><span class="v">${escapeHtml(fechaImp)}</span></div>

    <div class="barcode-wrap">
      <svg id="barcodePrev"></svg>
      <div class="code-text">${escapeHtml(it.code)}</div>
    </div>
  `;
  wrap.appendChild(div);

  try{
    JsBarcode("#barcodePrev", it.code, { format:"CODE128", width:2, height:60, displayValue:true });
  }catch(e){}
}
// === (Opcional) Vista previa en vivo al escribir CLIENTE/DESTINO ===
(function bindLabelLivePreview(){
  const cli = document.getElementById('labelCliente');
  const des = document.getElementById('labelDestino');
  if (cli) cli.addEventListener('input', showPreview);
  if (des) des.addEventListener('input', showPreview);
})();

// === BLOQUE DE IMPRESIÓN (REEMPLAZO COMPLETO) ===
document.getElementById('btnPrint').addEventListener('click', ()=>{
  if(!selectedLabelCode){
    alert("Primero busca y selecciona un producto de las sugerencias.");
    return;
  }
  const it = items.find(i=>i.code===selectedLabelCode);
  if(!it){ alert("Producto no encontrado"); return; }

  const cliente = (document.getElementById('labelCliente')?.value || "").trim();
  const destino = (document.getElementById('labelDestino')?.value || "").trim();
  const fechaImp = new Date().toLocaleDateString('es-MX', {day:'2-digit', month:'2-digit', year:'numeric'});

  const w = window.open('', '', 'width=900,height=650');

  const html = `
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Etiqueta ${escapeHtml(it.code)}</title>
  <style>
    @page { size: 152.4mm 101.6mm; margin: 0; }
    html, body { margin:0; padding:0; }
    body {
      width: 152.4mm; height: 101.6mm;
      font-family: Arial, sans-serif;
      -webkit-print-color-adjust: exact !important;
      print-color-adjust: exact !important;
    }
    .page { position: relative; width: 152.4mm; height: 101.6mm; box-sizing: border-box; padding: 6mm 8mm; }
    .header { text-align: center; }
    .brand { font-weight: 700; font-size: 24pt; line-height: 1; white-space: nowrap; }
    .cliente { font-weight: 700; font-size: 24pt; line-height: 1.05; margin-top: 1mm; word-break: break-word; }
    .divider { height: 2px; background: #000; margin: 3mm 0 4mm; border-radius: 2px; }
    .rows { width: calc(100% - 16mm); margin: 0 auto; }
    .row{ display:flex; justify-content:center; gap:3mm; align-items:baseline; margin:1mm 0; flex-wrap:wrap; }
    .k{ font: 700 24pt Arial, sans-serif; letter-spacing:.2pt; }
    .v{ font: 400 22pt Arial, sans-serif; min-width:40mm; text-align:left; word-break:break-word; }
    .barcode { margin-top: 4mm; text-align: center; }
    .barcode svg { width: 100%; height: 45mm; }
    .code-text { font-size: 14pt; margin-top: 1mm; letter-spacing: 1px; }
    @media print { .page { padding: 6mm 8mm; } }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"><\/script>
</head>
<body>
  <div class="page">
    <div class="header">
      <div class="brand">MUEBLES KRILL S.A. DE C.V</div>
      <div class="cliente">CLIENTE: ${escapeHtml(cliente)}</div>
    </div>

    <div class="divider"></div>

    <div class="rows">
      <div class="row"><span class="k">MODELO:</span><span class="v">${escapeHtml(it.nombre)}</span></div>
      <div class="row"><span class="k">TELA:</span><span class="v">${escapeHtml(it.tela)}</span></div>
      <div class="row"><span class="k">SKU:</span><span class="v">${escapeHtml(it.sku||'')}</span></div>
      <div class="row"><span class="k">DESTINO:</span><span class="v">${escapeHtml(destino)}</span></div>
      <div class="row"><span class="k">FECHA:</span><span class="v">${escapeHtml(fechaImp)}</span></div>
    </div>

    <div class="barcode">
      <svg id="b"></svg>
      <div class="code-text">${escapeHtml(it.code)}</div>
    </div>
  </div>

  <script>
    window.onload = function(){
      try{
        JsBarcode("#b","${escapeJs(it.code)}",{
          format:"CODE128",
          width: 2,
          displayValue: true,
          font: "Arial",
          fontSize: 12
        });
      }catch(e){}
      window.print();
    };
  <\/script>
</body>
</html>
  `;

  w.document.write(html);
  w.document.close();
  w.focus();
});
/* ===== Botones de +1 / +N en pestaña Escanear ===== */
document.getElementById('btnPlus1').addEventListener('click', ()=>{
  const code = document.getElementById('scanCode').value.trim();
  if(!code) return;
  const item = items.find(i=>i.code===code || (i.sku||"").toLowerCase()===code.toLowerCase());
  if(!item) return;
  item.cantidad += 1;
  const nowISO = new Date().toISOString();
  moves.push({id: genId(),dateISO:nowISO, code:item.code, sku:item.sku, nombre:item.nombre, tela:item.tela, delta:+1, type:'scan'});
  if(prefs.beep) beep();
  if(prefs.vibrate && navigator.vibrate) navigator.vibrate(80);
  flashScan(true, `+1 ${item.nombre} (${item.tela})`);
  saveAll(); renderInventory();
  document.getElementById('scanQty').value = item.cantidad;
});
document.getElementById('btnPlusN').addEventListener('click', ()=>{
  const n = parseInt(document.getElementById('plusN').value||"0");
  const code = document.getElementById('scanCode').value.trim();
  if(!n || !code) return;
  const item = items.find(i=>i.code===code || (i.sku||"").toLowerCase()===code.toLowerCase());
  if(!item) return;
  item.cantidad += n;
  const nowISO = new Date().toISOString();
  moves.push({id: genId(),dateISO:nowISO, code:item.code, sku:item.sku, nombre:item.nombre, tela:item.tela, delta:+n, type:'scan'});
  if(prefs.beep) beep();
  if(prefs.vibrate && navigator.vibrate) navigator.vibrate([60,30,60]);
  flashScan(true, `+${n} ${item.nombre} (${item.tela})`);
  saveAll(); renderInventory();
  document.getElementById('scanQty').value = item.cantidad;
});

/* ===== Sesión completa (JSON) ===== */
// Permitir re-seleccionar el mismo archivo sin recargar
const jsonInput = document.getElementById('jsonSessionFile');
if(jsonInput){
  jsonInput.addEventListener('click', ()=>{ jsonInput.value = ''; });
}

// (opcional si luego usas el CSV): hace lo mismo para el CSV
const csvInput = document.getElementById('csvFile');
if(csvInput){
  csvInput.addEventListener('click', ()=>{ csvInput.value = ''; });
}
document.getElementById('btnExportSession').addEventListener('click', ()=>{
  const payload = {
    version: 2,
    exportedAt: new Date().toISOString(),
    items,
    moves,
    kllCounter,
    prefs,
    snapshots
  };
  const blob = new Blob([JSON.stringify(payload,null,2)], {type:'application/json;charset=utf-8'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'inventario_sesion_completa.json'; a.click();
  URL.revokeObjectURL(url);
});
document.getElementById('btnImportSession').addEventListener('click', ()=>{
  const input = document.getElementById('jsonSessionFile');
  if(!input.files || !input.files[0]){ 
    alert("Selecciona un archivo .json de sesión completa."); 
    return; 
  }

  const file = input.files[0];
  const reader = new FileReader();
  reader.onload = (e)=>{
    try{
      const data = JSON.parse(e.target.result);
      if(!data || !Array.isArray(data.items) || !Array.isArray(data.moves)){
        alert("Archivo inválido: faltan 'items' o 'moves'."); return;
      }
      items = data.items;
      moves = data.moves;
      kllCounter = Number.isInteger(data.kllCounter) ? data.kllCounter : 1;
      prefs = data.prefs && typeof data.prefs === 'object'
        ? { beep: !!data.prefs.beep, vibrate: !!data.prefs.vibrate }
        : { beep: true, vibrate: true };
      if(Array.isArray(data.snapshots)) snapshots = data.snapshots;

      saveAll();
      renderInventory();
      refreshLabelSelect();
      const activeTab = document.querySelector('.tab-btn.active')?.dataset.tab;
      if(activeTab==='conteo'){ renderSnapshotsList(); renderCountEditor(); }

      alert("Sesión completa importada correctamente.");
    }catch(err){
      console.error(err);
      alert("No se pudo leer el JSON. Verifica el archivo.");
    }
  };
  reader.readAsText(file);
}); // ←← Asegúrate de que este cierre exista
// === Importar CSV (Totales; requiere columna "Código") ===
(function bindCsvImport(){
  const btn = document.getElementById('btnImport');
  const input = document.getElementById('csvFile');
  if(!btn || !input) return;

  // Permitir re-seleccionar el mismo archivo sin recargar
  input.addEventListener('click', ()=>{ input.value = ''; });

  btn.addEventListener('click', ()=>{
    if(!input.files || !input.files[0]){
      alert("Selecciona un archivo .csv primero.");
      return;
    }
    const file = input.files[0];
    const reader = new FileReader();

    reader.onload = (e)=>{
      try{
        const rows = parseCSV(e.target.result);
        if(!rows.length){
          alert("El CSV está vacío."); return;
        }

        // Validación mínima: necesitamos la columna "Código" o "Codigo"
        const hasCodigo = Object.keys(rows[0]).some(h => h.trim().toLowerCase() === 'código' || h.trim().toLowerCase() === 'codigo');
        if(!hasCodigo){
          alert('El CSV debe incluir la columna "Código".'); 
          return;
        }

        let creados = 0, actualizados = 0, movs = 0;

        rows.forEach(r=>{
          const get = (k) => {
            const key = Object.keys(r).find(h => h.trim().toLowerCase() === k.toLowerCase());
            return key ? (r[key]||'').trim() : '';
          };

          const code      = get('código') || get('codigo');
          if(!code) return;

          const sku       = get('sku');
          const nombre    = get('modelo') || get('nombre') || '';
          const tela      = get('tela') || '';
          const cantStr   = get('cantidad');
          const qty       = Number.parseInt(cantStr, 10);
          const cantidad  = Number.isFinite(qty) && qty > 0 ? qty : 1;

          const nowISO = new Date().toISOString();
          const it = items.find(i => i.code === code);

          if(it){
            it.nombre   = nombre || it.nombre;
            it.tela     = tela   || it.tela;
            it.sku      = sku    || it.sku;
            it.cantidad = (Number(it.cantidad)||0) + cantidad;
            actualizados++;

            moves.push({
              id: genId(),
              dateISO: nowISO,
              code: it.code,
              sku: it.sku,
              nombre: it.nombre,
              tela: it.tela,
              delta: +cantidad,
              type: 'import'
            });
            movs++;
          }else{
            items.push({
              code,
              sku,
              nombre,
              tela,
              cantidad,
              createdISO: nowISO
            });
            creados++;

            moves.push({
              id: genId(),
              dateISO: nowISO,
              code,
              sku,
              nombre,
              tela,
              delta: +cantidad,
              type: 'import'
            });
            movs++;
          }
        });

        saveAll();
        renderInventory();
        refreshLabelSelect();

        alert(`Importación terminada.\nCreados: ${creados}\nActualizados: ${actualizados}\nMovimientos: ${movs}`);
      }catch(err){
        console.error(err);
        alert("No se pudo leer el CSV. Verifica el formato.");
      }
    };

    reader.readAsText(file);
  });
})();
/* ===== Utilidades ===== */
// === Helper para resetear <input type="file"> de forma segura ===
function resetFileInput(el){
  try{
    el.value = '';
  }catch(e){
    // fallback raro de algunos navegadores: clonar y reemplazar
    const nuevo = el.cloneNode();
    el.parentNode.replaceChild(nuevo, el);
  }
}
// Normaliza lo leído por el escáner para comparar códigos y SKUs
function normalizeScan(s){
  if(!s) return "";
  let t = String(s).normalize('NFKC').trim();

  // Reemplaza apóstrofos/acento por guion
  t = t.replace(/[\u00B4\u2019\u0027\u0060\u02CA\u02B9\u2018]/g, '-');

  // Reemplaza dashes Unicode por guion simple
  t = t.replace(/[\u2013\u2014\u2212\u2010\u2011]/g, '-');

  // Quita espacios
  t = t.replace(/\s+/g, '');

  return t.toUpperCase();
}
function beep(){
  try{
    const ac = new (window.AudioContext||window.webkitAudioContext)();
    const o = ac.createOscillator(); const g = ac.createGain();
    o.type='sine'; o.frequency.value=880; o.connect(g); g.connect(ac.destination);
    g.gain.setValueAtTime(0.0001, ac.currentTime);
    g.gain.exponentialRampToValueAtTime(0.3, ac.currentTime+0.01);
    o.start();
    setTimeout(()=>{ g.gain.exponentialRampToValueAtTime(0.0001, ac.currentTime+0.02); o.stop(ac.currentTime+0.03); }, 120);
  }catch(e){}
}
function flashScan(ok, msg){
  const el = document.getElementById('scanFlash');
  if(!el) return;
  el.textContent = msg || (ok ? '¡Leído!' : 'No registrado');
  el.className = ok ? 'ok' : 'err';
  el.style.display = 'block';
  el.offsetHeight; // forzar reflujo
  el.style.opacity = '1';
  clearTimeout(el._hide1);
  clearTimeout(el._hide2);
  el._hide1 = setTimeout(()=>{ el.style.opacity = '0'; }, 700);
  el._hide2 = setTimeout(()=>{ el.style.display = 'none'; }, 1000);
}
function formatDateDDMMYYYY(d){
  const dd = String(d.getDate()).padStart(2,'0');
  const mm = String(d.getMonth()+1).padStart(2,'0');
  const yyyy = d.getFullYear();
  return `${dd}/${mm}/${yyyy}`;
}
function downloadCSV(filename, headers, rows){
  const csv = headers.join(",")+"\n"+rows.map(r=>r.map(v=>`"${String(v).replace(/"/g,'""')}"`).join(",")).join("\n");
  const blob = new Blob([csv],{type:"text/csv;charset=utf-8"});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download=filename; a.click();
  URL.revokeObjectURL(url);
}
function parseCSV(text){
  const lines = text.replace(/\r\n/g,"\n").replace(/\r/g,"\n").split("\n").filter(l=>l.trim().length>0);
  if(lines.length===0) return [];
  const headers = splitCSVLine(lines[0]);
  const out = [];
  for(let i=1;i<lines.length;i++){
    const cols = splitCSVLine(lines[i]);
    const obj = {};
    headers.forEach((h,idx)=> obj[h.trim()] = (cols[idx]||"").trim());
    out.push(obj);
  }
  return out;
}
function splitCSVLine(line){
  const res=[]; let cur=""; let inQ=false;
  for(let i=0;i<line.length;i++){
    const ch=line[i];
    if(ch==='"'){ if(inQ && line[i+1]==='"'){ cur+='"'; i++; } else inQ=!inQ; }
    else if(ch===',' && !inQ){ res.push(cur); cur=""; }
    else{ cur+=ch; }
  }
  res.push(cur); return res;
}
// Devuelve {start, end, label} para un "YYYY-MM"
function getMonthRange(yyyyMM){
  try{
    const [Y,M] = yyyyMM.split('-').map(x=>parseInt(x,10));
    const start = new Date(Date.UTC(Y, M-1, 1, 0,0,0,0));
    const end   = new Date(Date.UTC(Y, M, 0, 23,59,59,999)); // último día del mes
    const label = new Date(Date.UTC(Y, M-1, 1)).toLocaleDateString(undefined, {year:'numeric', month:'long'});
    return {start, end, label};
  }catch(e){
    const now = new Date();
    const yyyy = now.getFullYear();
    const mm = String(now.getMonth()+1).padStart(2,'0');
    return getMonthRange(`${yyyy}-${mm}`);
  }
}

// Obtiene el valor "YYYY-MM" del mes actual
function getCurrentMonthValue(){
  const now = new Date();
  const yyyy = now.getFullYear();
  const mm = String(now.getMonth()+1).padStart(2,'0');
  return `${yyyy}-${mm}`;
}

// Busca el snapshot más cercano anterior a "YYYY-MM"
function findPreviousSnapshotForMonth(yyyyMM){
  if(!snapshots || snapshots.length===0) return null;
  // Selecciona entre snapshots que tengan monthVal y sean < yyyyMM
  const prevs = snapshots.filter(s=> s.monthVal && s.monthVal < yyyyMM);
  if(prevs.length===0) return null;
  // El más reciente de los anteriores
  prevs.sort((a,b)=> (b.monthVal).localeCompare(a.monthVal));
  return prevs[0];
}
function escapeHtml(s){ return String(s).replace(/[&<>"']/g, m=>({"&":"&amp;","<":"&lt;",">":"&gt;","\"":"&quot;","'":"&#39;"}[m])); }
function escapeJs(s){ return String(s).replace(/\\/g,'\\\\').replace(/`/g,'\\`').replace(/\$/g,'\\$').replace(/"/g,'\\"'); }
function genId(prefix='mv_'){
  return prefix + Date.now().toString(36) + Math.random().toString(36).slice(2,7);
}
function ensureMoveIds(){
  let changed = false;
  moves.forEach(m=>{
    if(!m.id){ m.id = genId(); changed = true; }
  });
  if(changed) saveAll();
}
// --- ELIMINAR UN PRODUCTO COMPLETO (y todos sus movimientos asociados) ---
function deleteItem(code){
  const it = items.find(x => x.code === code);
  if(!it){ 
    alert("Producto no encontrado."); 
    return; 
  }

  // Confirmación antes de borrar
  if(!confirm(
    `¿Eliminar "${it.nombre}" (${it.tela}) · SKU ${it.sku||''} · Código ${it.code} del inventario?\n` +
    `También se eliminarán todos sus movimientos asociados.`
  )) return;

  // Quitar el producto del inventario
  items = items.filter(x => x.code !== code);

  // Quitar todos los movimientos de ese producto
  const antes = moves.length;
  moves = moves.filter(m => m.code !== code);
  const quitados = antes - moves.length;

  // Guardar cambios
  saveAll();

  // Refrescar vistas
  renderInventory();
  refreshLabelSelect();

  // Mensaje final
  alert(`Eliminado.\nCódigo: ${code}\nMovimientos eliminados: ${quitados}`);
}
function deleteMove(moveId){
  const idx = moves.findIndex(m=>m.id===moveId);
  if(idx===-1){ alert("Movimiento no encontrado."); return; }

  const m = moves[idx];
  const it = items.find(i=>i.code===m.code);

  // Confirmación
  const d = m.dateISO ? new Date(m.dateISO) : new Date();
  const txt = `¿Eliminar este movimiento?\n` +
              `Fecha: ${d.toLocaleDateString()} ${d.toLocaleTimeString()}\n` +
              `Código: ${m.code} · SKU: ${m.sku||''}\n` +
              `Modelo: ${m.nombre} · Tela: ${m.tela}\n` +
              `Δ: ${m.delta} · Tipo: ${m.type}\n\n` +
              `Se revertirá la cantidad en inventario.`;
  if(!confirm(txt)) return;

  // Revertir cantidad del producto
  if(it){
    const newQty = (parseInt(it.cantidad)||0) - (parseInt(m.delta)||0);
    it.cantidad = Math.max(0, newQty);
  }

  // Quitar movimiento
  moves.splice(idx,1);

  saveAll();
  renderInventory(); // esto a su vez llama renderHistory()
}

/* ===== Escáner físico (USB/Bluetooth) como teclado ===== */
(function enableKeyboardScanner(){
  let buffer = "";
  let lastTime = 0;
  const MAX_GAP_MS = 40;            // si pasan >40ms entre teclas, no es escáner
  const MIN_LEN   = 4;
  const END_KEYS  = new Set(["Enter", "Tab"]);

  window.addEventListener('keydown', (ev)=>{
    const now = Date.now();

    // si el intervalo es grande, empezamos un nuevo buffer
    if(now - lastTime > MAX_GAP_MS){
      buffer = "";
    }
    lastTime = now;

    // Enter/Tab marca fin del escaneo
    if(END_KEYS.has(ev.key)){
      if(buffer.length >= MIN_LEN){
        ev.preventDefault(); // evita submits accidentales
        ev.stopPropagation();
        processScan(buffer);
      }
      buffer = "";
      return;
    }

    // Ignorar combinaciones especiales
    if(ev.ctrlKey || ev.altKey || ev.metaKey) return;
    // Solo acumular caracteres imprimibles
    if(ev.key.length !== 1) return;

    buffer += ev.key;
  }, true);
})();
</script>
</body>
</html>