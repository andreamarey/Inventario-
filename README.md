# Inventario-
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Inventario — KLL + SKU</title>
<!-- Librerías -->
<script src="https://unpkg.com/html5-qrcode"></script>
<script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
<style>
  :root{
    --primary:#2563eb; --primary-600:#1d4ed8;
    --bg:#f3f4f6; --card:#fff; --text:#111827; --muted:#6b7280;
    --ring:rgba(37,99,235,.15)
  }
  *{box-sizing:border-box}
  body { font-family: Arial, sans-serif; margin:0; background:var(--bg); color:var(--text); }
  header { background:var(--primary); color:#fff; padding:16px; text-align:center; font-weight:700; }
  .container { max-width:1100px; margin:20px auto; padding:10px; }
  .tabs { display:flex; gap:6px; margin-bottom:10px; flex-wrap:wrap; }
  .tab-btn { flex:1; min-width:150px; padding:10px; cursor:pointer; border:1px solid #e5e7eb; background:#fff; border-radius:8px; font-weight:700; }
  .tab-btn.active { background:var(--primary); color:#fff; border-color:var(--primary); }
  .card { background:#fff; border-radius:10px; padding:15px; margin-bottom:15px; box-shadow:0 2px 8px rgba(0,0,0,.06); }
  .hidden { display:none; }
  input,button,select { width:100%; padding:10px; margin:6px 0; border-radius:8px; border:1px solid #e5e7eb; background:#fff; color:var(--text); }
  input:focus,select:focus { outline:none; box-shadow:0 0 0 3px var(--ring); border-color:var(--primary); }
  button { background:var(--primary); color:#fff; border:none; cursor:pointer; font-weight:700; }
  button:hover { background:var(--primary-600); }
  .btn-secondary{ background:#6b7280; }
  .btn-secondary:hover{ background:#4b5563; }
  .btn-danger{ background:#ef4444; }
  .btn-danger:hover{ background:#dc2626; }
  .row{display:flex; gap:10px; flex-wrap:wrap}
  .muted { color:var(--muted); font-size:.9rem; }
  ul{padding-left:18px; margin:6px 0}

  /* Etiqueta preview (pantalla) */
  .label-preview { border:1px solid #000; padding:12px; width:320px; background:#fff; border-radius:6px; }
  .model { font-size:20px; font-weight:700; }
  .fabric, .sku, .date { font-size:14px; margin-top:2px; }

  /* Historial / Conteo */
  .group{margin:10px 0}
  .group h3{margin:0 0 6px 0; font-size:1rem}
  .mov{padding:6px 8px; border:1px solid #e5e7eb; border-radius:8px; margin:4px 0; background:#fff}
  .kv{font-family:ui-monospace, Menlo, Consolas, monospace; font-size:.9rem}
  table th, table td { font-size:14px; }

  /* ===== Indicador de escaneo (flash) ===== */
  #scanFlash{
    position: fixed;
    top: 16px;
    right: 16px;
    z-index: 9999;
    padding: 12px 14px;
    border-radius: 10px;
    font-weight: 700;
    color: #fff;
    display: none;
    opacity: 0;
    transition: opacity .25s ease;
    box-shadow: 0 8px 20px rgba(0,0,0,.15);
  }
  #scanFlash.ok { background: #10b981; }  /* verde */
  #scanFlash.err{ background: #ef4444; }  /* rojo  */
  
  /* Inventario como tabla */
.inv-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  font-size: 14px;
  background: #fff;
}
.inv-table th, .inv-table td {
  border: 1px solid #e5e7eb;
  padding: 8px;
  text-align: left;
}
.inv-table th {
  background: #f9fafb;
  font-weight: 700;
}
.inv-table tbody tr:nth-child(even) {
  background: #f3f4f6;
}
.inv-table td:last-child {
  text-align: center;
}

/* Botón eliminar */
.btn-del {
  background: #ef4444;
  color: #fff;
  border: none;
  padding: 6px 10px;
  border-radius: 8px;
  font-size: 12px;
  cursor: pointer;
}
.btn-del:hover { background: #dc2626; }

/* Movimientos como tabla */
.mov-table{
  width:100%;
  border-collapse:collapse;
  margin-top:10px;
  font-size:14px;
  background:#fff;
}
.mov-table th, .mov-table td{
  border:1px solid #e5e7eb;
  padding:8px;
  text-align:left;
}
.mov-table th{
  background:#f9fafb;
  font-weight:700;
}
.mov-table tbody tr:nth-child(even){
  background:#f3f4f6;
}
.mov-table td:nth-last-child(3){ text-align:right; } /* columna Δ */
.mov-table td:last-child{ text-align:center; }

/* Botón eliminar movimiento (reutiliza .btn-del) */
.btn-del-mov{ /* opcional si quieres un tono distinto */
  background:#ef4444;
  color:#fff;
  border:none;
  padding:6px 10px;
  border-radius:8px;
  font-size:12px;
  cursor:pointer;
}
.btn-del-mov:hover{ background:#dc2626; }

/* Dashboard */
.dashboard {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  margin: 16px auto;
  max-width: 1100px;
}
.dash-card {
  flex: 1;
  min-width: 200px;
  text-align: center;
  padding: 16px;
  border-radius: 12px;
  background: #fff;
  box-shadow: 0 2px 8px rgba(0,0,0,.08);
}
.dash-icon {
  font-size: 24px;
  margin-bottom: 8px;
}
.dash-value {
  font-size: 28px;
  font-weight: 700;
  color: var(--primary);
}
.dash-label {
  font-size: 14px;
  color: var(--muted);
}
</style>
</head>
<body>
<header>Inventario — KLL + SKU</header>
<!-- Dashboard -->
<div id="dashboard" class="dashboard">
  <div class="card dash-card">
    <div class="dash-icon">📦</div>
    <div class="dash-value" id="dashProducts">0</div>
    <div class="dash-label">Productos distintos</div>
  </div>
  <div class="card dash-card">
    <div class="dash-icon">🪑</div>
    <div class="dash-value" id="dashPieces">0</div>
    <div class="dash-label">Piezas en inventario</div>
  </div>
  <div class="card dash-card">
    <div class="dash-icon">📅</div>
    <div class="dash-value" id="dashMoves">0</div>
    <div class="dash-label">Movimientos este mes</div>
  </div>
  <div class="card dash-card">
    <div class="dash-icon">📊</div>
    <div class="dash-value" id="dashCut">–</div>
    <div class="dash-label">Último corte guardado</div>
  </div>
</div>
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
      <input id="newSKU" placeholder="SKU (ej. SR-LB)">
    </div>
    <button id="btnGenerate">Guardar / Sumar</button>
    <div class="muted">Si ya existe el mismo <b>SKU + Modelo + Tela</b>, se suma cantidad y se reutiliza el mismo KLL.</div>
  </section>

  <!-- Escanear -->
  <section id="escanear" class="card hidden">
    <h2>Escanear (QR + Code128)</h2>
    <div class="row">
      <select id="cameraSelect" title="Selecciona cámara"></select>
      <button id="btnStart" class="btn-secondary">Iniciar cámara</button>
      <button id="btnStop" class="btn-secondary">Detener cámara</button>
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
      <button id="btnPlus1" class="btn-secondary">+1</button>
      <input id="plusN" type="number" min="1" value="5" style="max-width:140px">
      <button id="btnPlusN" class="btn-secondary">+N</button>
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
      <button onclick="exportFilteredTotals()">Exportar CSV (Totales Filtrados)</button>
      <button onclick="exportFilteredHistory()" class="btn-secondary">Exportar CSV (Historial Filtrado)</button>
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
    <div class="row">
      <input id="labelSearch" placeholder="Buscar por modelo, tela, SKU o código" oninput="renderLabelSuggestions()">
      <button id="btnPrint">Imprimir etiqueta</button>
    </div>
    <div id="labelSuggestions" class="muted" style="margin-top:6px">Escribe para ver sugerencias…</div>
    <div id="previewWrap" style="margin-top:12px"></div>
    <div class="muted" style="margin-top:6px">
      Formato: Modelo (grande) · Tela · <b>SKU</b> · Fecha · Código de barras (KLL)
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
let moves = [];     // [{dateISO, code, sku, nombre, tela, delta, type}] // type: 'register'|'import'|'scan'
let kllCounter = 1;
let prefs = { beep:true, vibrate:true };
let snapshots = []; // [{id,label,createdISO,totals:[{code,sku,nombre,tela,cantidad}],notes?}]
let countBuffer = []; // buffer de conteo
let selectedLabelCode = null; // Etiquetas (buscador)

/* ===== Cargar ===== */
(function load(){
  try{
    items = JSON.parse(localStorage.getItem('items_simple')||'[]');
    moves = JSON.parse(localStorage.getItem('moves_simple')||'[]');
    kllCounter = parseInt(localStorage.getItem('kllCounter_simple')||'1');
    const p = JSON.parse(localStorage.getItem('prefs_simple')||'{}');
    prefs.beep = p.beep!==undefined ? p.beep : true;
    prefs.vibrate = p.vibrate!==undefined ? p.vibrate : true;
    snapshots = JSON.parse(localStorage.getItem('snapshots_simple')||'[]');
  }catch(e){
    items=[]; moves=[]; kllCounter=1; prefs={beep:true, vibrate:true}; snapshots=[];
  }

  document.getElementById('prefBeep').checked = !!prefs.beep;
  document.getElementById('prefVibrate').checked = !!prefs.vibrate;

  // Asegura IDs en movimientos ya existentes
  ensureMoveIds();

  // Pinta tablas iniciales
  renderInventory();
  refreshLabelSelect(); // buscador de etiquetas
  updateDashboard();
})();
function updateDashboard(){
  // 1) Productos distintos
  document.getElementById('dashProducts').textContent = items.length;

  // 2) Piezas totales
  const totalPzas = items.reduce((acc,i)=>acc+(parseInt(i.cantidad)||0),0);
  document.getElementById('dashPieces').textContent = totalPzas;

  // 3) Movimientos del mes actual
  const monthVal = getCurrentMonthValue(); // ya tienes esta función
  const {start, end} = getMonthRange(monthVal);
  const movesMonth = moves.filter(m=>{
    const d = new Date(m.dateISO||Date.now());
    return d >= start && d <= end;
  });
  document.getElementById('dashMoves').textContent = movesMonth.length;

  // 4) Último corte mensual guardado
  if(snapshots.length>0){
    const last = [...snapshots].sort((a,b)=>(b.createdISO||"").localeCompare(a.createdISO||""))[0];
    const lbl = last.label || "Sin nombre";
    const fecha = new Date(last.createdISO).toLocaleDateString();
    document.getElementById('dashCut').textContent = `${lbl} (${fecha})`;
  }else{
    document.getElementById('dashCut').textContent = "–";
  }
}
function saveAll(){
  localStorage.setItem('items_simple', JSON.stringify(items));
  localStorage.setItem('moves_simple', JSON.stringify(moves));
  localStorage.setItem('kllCounter_simple', String(kllCounter));
  localStorage.setItem('prefs_simple', JSON.stringify(prefs));
  localStorage.setItem('snapshots_simple', JSON.stringify(snapshots));
}

/* ===== Cambio de pestañas (protegido) ===== */
document.querySelectorAll('.tab-btn').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('section').forEach(s=>s.classList.add('hidden'));
    const sec = document.getElementById(btn.dataset.tab);
    if (sec) sec.classList.remove('hidden');

    if (btn.dataset.tab==='etiquetas') {
      if (typeof refreshLabelSelect === 'function') refreshLabelSelect();
    }
    if (btn.dataset.tab==='escanear'){
      if (typeof initCameras === 'function') initCameras();
    } else {
      if (typeof stopScanner === 'function') stopScanner();
    }
    if (btn.dataset.tab==='conteo'){
      if (typeof renderCountEditor==='function') renderCountEditor();
      if (typeof renderSnapshotsList==='function') renderSnapshotsList();
    }
  });
});

/* ===== Registrar ===== */
document.getElementById('btnGenerate').addEventListener('click', ()=>{
  const nombre = document.getElementById('newName').value.trim();
  const tela   = document.getElementById('newFabric').value.trim();
  const sku    = document.getElementById('newSKU').value.trim();
  if(!nombre || !tela || !sku){ alert("Completa Modelo, Tela y SKU"); return; }

  const existing = items.find(i=>i.sku.toLowerCase()===sku.toLowerCase() &&
                                 i.nombre.toLowerCase()===nombre.toLowerCase() &&
                                 i.tela.toLowerCase()===tela.toLowerCase());
  const nowISO = new Date().toISOString();

  if(existing){
    existing.cantidad += 1;
    moves.push({id: genId(),dateISO: nowISO, code: existing.code, sku: existing.sku, nombre: existing.nombre, tela: existing.tela, delta:+1, type:'register'});
    alert(`Se sumó +1 a ${existing.nombre} (${existing.tela}) · SKU ${existing.sku}\nCódigo: ${existing.code}`);
  }else{
    const code = "KLL-" + String(kllCounter).padStart(4,"0");
    kllCounter++;
    items.push({code, sku, nombre, tela, cantidad:1, createdISO:nowISO});
    moves.push({id: genId(),dateISO: nowISO, code, sku, nombre, tela, delta:+1, type:'register'});
    alert(`Nuevo producto creado con código ${code}`);
  }
  saveAll();
  document.getElementById('newName').value = "";
  document.getElementById('newFabric').value = "";
  document.getElementById('newSKU').value = "";
  renderInventory();
  refreshLabelSelect();
  updateDashboard();
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
      <td><button class="btn-del" onclick="deleteItem('${escapeJs(i.code)}')">Eliminar</button></td>
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
      <td>${escapeHtml(m.type)}</td>
      <td>
        <button class="btn-del-mov" onclick="deleteMove('${escapeJs(m.id)}')">Eliminar</button>
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
  const select = document.getElementById('cameraSelect');
  select.innerHTML = "";
  try{
    const cams = await Html5Qrcode.getCameras();
    cams.forEach(c=>{
      const opt = document.createElement('option');
      opt.value = c.id; opt.textContent = c.label || c.id;
      select.appendChild(opt);
    });
    const rear = cams.find(c=>/back|rear|trase|environment/gi.test(c.label||""));
    currentCamId = (rear ? rear.id : cams[0]?.id);
    if(currentCamId) select.value = currentCamId;
    select.onchange = ()=>{ currentCamId = select.value; if(h5){ stopScanner(); startScanner(); } };
  }catch(e){
    console.error(e);
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
  const codeText = String(text || "").trim();
  if(!codeText) return;

  // Reflejar el texto detectado en la pestaña Escanear (si está abierta)
  const codeInput = document.getElementById('scanCode');
  if(codeInput) codeInput.value = codeText;

  // Buscar por Código KLL exacto o por SKU (insensible a mayúsculas)
  const item = items.find(i=> i.code === codeText ||
                              (i.sku||"").toLowerCase() === codeText.toLowerCase());

  if(item){
    item.cantidad += 1;
    const nowISO = new Date().toISOString();
    moves.push({id: genId(),dateISO:nowISO, code:item.code, sku:item.sku, nombre:item.nombre, tela:item.tela, delta:+1, type:'scan'});

    if(prefs.beep) beep();
    if(prefs.vibrate && navigator.vibrate) navigator.vibrate(80);

    // Indicador visual de éxito
    flashScan(true, `+1 ${item.nombre} (${item.tela})`);

    // Actualiza campos de la pestaña Escanear (si visible)
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
    // Indicador visual de error / no encontrado
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
  const s = document.getElementById('labelSearch');
  const box = document.getElementById('labelSuggestions');
  const prev = document.getElementById('previewWrap');
  if (s) s.value = "";
  if (box) box.innerHTML = 'Escribe para ver sugerencias…';
  if (prev) prev.innerHTML = '';
}
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

  const dateTxt = it.createdISO ? new Date(it.createdISO).toLocaleDateString() : "";
  const div = document.createElement('div');
  div.className="label-preview";
  div.innerHTML = `
    <div class="model">${escapeHtml(it.nombre)}</div>
    <div class="fabric">Tela: ${escapeHtml(it.tela)}</div>
    <div class="sku">SKU: ${escapeHtml(it.sku||'')}</div>
    <div class="date">${escapeHtml(dateTxt)}</div>
    <svg id="barcodePrev"></svg>
  `;
  wrap.appendChild(div);
  try{ JsBarcode("#barcodePrev", it.code, {format:"CODE128", width:2, height:60, displayValue:true}); }catch(e){}
}

/* ===== Impresión de etiqueta ===== */
document.getElementById('btnPrint').addEventListener('click', ()=>{
  if(!selectedLabelCode){ alert("Primero busca y selecciona un producto de las sugerencias."); return; }
  const it = items.find(i=>i.code===selectedLabelCode);
  if(!it){ alert("Producto no encontrado"); return; }
const dateTxt = new Date().toLocaleDateString();
  const w = window.open('', '', 'width=420,height=360');
  const html = `
    <html><head><title>Etiqueta ${escapeHtml(it.code)}</title>
    <style>
      body{font-family:Arial, sans-serif; margin:0; padding:16px; text-align:center}
      .model{font-size:20px; font-weight:700}
      .fabric{font-size:14px; margin-top:2px}
      .sku{font-size:14px; margin-top:2px}
      .date{font-size:14px; margin-top:2px}
      svg{margin-top:10px}
    </style>
    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"><\/script>
    </head><body>
      <div class="model">${escapeHtml(it.nombre)}</div>
      <div class="fabric">Tela: ${escapeHtml(it.tela)}</div>
      <div class="sku">SKU: ${escapeHtml(it.sku||'')}</div>
      <div class="date">${escapeHtml(dateTxt)}</div>
      <svg id="b"></svg>
      <script>
        try{
          JsBarcode("#b","${escapeJs(it.code)}",{format:"CODE128",width:2,height:60,displayValue:true});
        }catch(e){}
        window.onload=function(){window.print();}
      <\/script>
    </body></html>
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

/* ===== Preferencias ===== */
document.getElementById('prefBeep').addEventListener('change', (e)=>{ prefs.beep = e.target.checked; saveAll(); });
document.getElementById('prefVibrate').addEventListener('change', (e)=>{ prefs.vibrate = e.target.checked; saveAll(); });

/* ===== Sesión completa (JSON) ===== */
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
  if(!input.files || !input.files[0]){ alert("Selecciona un archivo .json de sesión completa."); return; }
  if(!confirm("Esto reemplazará TODO el inventario actual, movimientos, contador KLL, preferencias y (si vienen) los cortes mensuales. ¿Continuar?")) return;

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
});

/* ===== Utilidades ===== */
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
