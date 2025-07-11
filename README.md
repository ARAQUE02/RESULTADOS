<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Consulta de Resultados</title>
  <style>
    /* ... Tu CSS original, no se modifica ... */
    body { font-family: 'Segoe UI', Arial, sans-serif; padding: 30px; background: #f7fafc; }
    .container { background: #fff; padding: 24px; border-radius: 14px; max-width: 1000px; margin: auto; box-shadow: 0 4px 24px 0 rgba(48,84,150,0.08);}
    h2 { color: #22406e; margin-bottom: 16px; }
    input, button { padding: 10px; margin-top: 10px; width: 100%; box-sizing: border-box; }
    .resultados { margin-top: 24px; }
    table { border-collapse: collapse; width: 100%; margin-bottom: 32px; background: #fff; color: #234; border-radius: 10px; overflow: hidden; box-shadow: 0 2px 14px 0 rgba(34,64,110,0.08); page-break-inside: avoid; font-size: 1em; }
    th, td { padding: 9px 12px; text-align: left; }
    .tituloTabla { background: #325c80 !important; font-size: 1.22em; color: #fff !important; letter-spacing: 0.04em; border-bottom: 3px solid #b2d9f7; text-align: left; }
    .subtitulo { background: #b2d9f7 !important; color: #234 !important; font-weight: 600; letter-spacing: 0.02em; border-bottom: 2px solid #e6f1fa; }
    th:not(.tituloTabla):not(.subtitulo) { background: #e6f1fa; color: #222 !important; font-weight: 600; border-bottom: 2px solid #b2d9f7; letter-spacing: 0.01em; width: 33%; }
    td { background: #fff; border-bottom: 1px solid #e4e8f0; color: #222; }
    tr:last-child td { border-bottom: none; }
    tr:nth-child(even) td { background: #f6fafd; }
    input, button { border-radius: 5px; border: 1px solid #b0bec5; font-size: 1em;}
    .boton-azul { background: #325c80; color: #fff; cursor: pointer; font-weight: 500; transition: background 0.2s; border: 1px solid #325c80; margin-bottom: 10px; }
    .boton-azul:hover { background: #203354; }
    .boton-imprimir { margin-top: 20px; width: 100%; }
    .password-container { position: relative; width: 100%; margin-top: 10px; }
    .password-toggle { position: absolute; right: 12px; top: 50%; transform: translateY(-50%); cursor: pointer; user-select: none; display: flex; align-items: center; height: 100%; }
    .status-cell { flex: 1; padding: 9px 12px; color: #fff; font-weight: bold; border-radius: 0 0 10px 0; text-align: center; font-size: 0.98em; display: flex; align-items: center; justify-content: center; }
    .value-cell { flex: 1; padding: 9px 12px; }
    .split-cell { display: flex; width: 100%; }
    @media (max-width: 700px) {
      .container { padding: 10px; }
      th, td { font-size: 0.97em; }
      .split-cell { flex-direction: column; }
      .status-cell, .value-cell { border-radius: 0; }
    }
    @media print {
      html, body { width: 100%; background: #fff !important; padding: 0 !important; margin: 0 !important; box-shadow: none !important; }
      .container { box-shadow: none !important; max-width: 100vw !important; width: 100vw !important; padding: 0 !important; margin: 0 !important; }
      h2, input, .boton-azul:not(.boton-imprimir) { display: none !important; }
      .boton-imprimir { display: none !important; }
      .resultados { margin-top: 0; }
      table { box-shadow: none !important; page-break-inside: avoid; width: 100vw !important; max-width: 100vw !important; font-size: 0.85em !important; margin: 0 !important; }
      th, .tituloTabla, .subtitulo, td { color: inherit !important; background: inherit !important; -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; padding: 5px 7px !important; }
      th.tituloTabla { background: #325c80 !important; color: #fff !important; }
      .subtitulo { background: #b2d9f7 !important; color: #234 !important; }
      th:not(.tituloTabla):not(.subtitulo) { background: #e6f1fa !important; color: #222 !important; }
      td { background: #fff !important; }
      tr:nth-child(even) td { background: #f6fafd !important; }
      @page { size: A4 landscape; margin: 12mm; }
    }
    .password-toggle svg { vertical-align: middle; }
  </style>
</head>
<body>
<div class="container">
    <h2>Consulta tus resultados</h2>
    <input type="text" id="nombre" placeholder="Escribe tu Nombre o Parte del Nombre" onkeydown="if(event.key==='Enter'){buscar();}">
     <div class="password-container">
      <input type="password" id="no" placeholder="Escribe tu Matricula (contraseña)" onkeydown="if(event.key==='Enter'){buscar();}">
      <span id="togglePwd" class="password-toggle" onclick="togglePassword()" aria-label="Mostrar/Ocultar contraseña">
        <!-- Ojo abierto SVG (mostrar) -->
        <svg id="eyeIcon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24" stroke="#325c80" stroke-width="2">
          <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 12s3.75-7.5 9.75-7.5 9.75 7.5 9.75 7.5-3.75 7.5-9.75 7.5S2.25 12 2.25 12z"/>
          <circle cx="12" cy="12" r="3.75"/>
        </svg>
        <!-- Ojo con línea SVG (ocultar), oculto por defecto -->
        <svg id="eyeSlashIcon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24" stroke="#325c80" stroke-width="2" style="display:none">
          <path stroke-linecap="round" stroke-linejoin="round" d="M3 3l18 18"/>
          <path stroke-linecap="round" stroke-linejoin="round" d="M10.477 10.489A3.75 3.75 0 0015.51 15.522"/>
          <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 12s3.75-7.5 9.75-7.5c2.083 0 3.952.48 5.514 1.227"/>
          <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 12s-3.75 7.5-9.75 7.5a11.21 11.21 0 01-5.514-1.227"/>
        </svg>
      </span>
    </div>
    <button class="boton-azul" onclick="buscar()">Buscar</button>
    <div class="resultados" id="resultados"></div>
    <button class="boton-azul boton-imprimir" onclick="imprimir()">Imprimir</button>
  </div>
  <!-- PapaParse para leer el CSV de Google Sheets -->
  <script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
  <script>
    let datos = [];

    function cargarDatosGoogleSheet(callback) {
      Papa.parse(
        "https://docs.google.com/spreadsheets/d/e/2PACX-1vROtR0P_TfsFJUbmUFhCV5_FOpB9knMvgtw9biFMhrhqoOtnZk9hYwJ5VXkVqJWgoHyLb3EGYoCb8-1/pub?output=csv",
        {
          download: true,
          header: true,
          dynamicTyping: false,
          complete: function(results) {
            datos = results.data;
            if (typeof callback === "function") callback();
          }
        }
      );
    }

    // Actualiza los datos cada 60 segundos automáticamente
    setInterval(() => cargarDatosGoogleSheet(), 60000);

    function getIMCStatus(imc) { const n = parseFloat(imc); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 18.5) return { color: '#29b6f6', text: 'Bajo peso' }; if (n < 25) return { color: '#66bb6a', text: 'Normal' }; if (n < 30) return { color: '#ffd600', text: 'Elevado' }; if (n < 40) return { color: '#ffb300', text: 'Poco elevado' }; return { color: '#e53935', text: 'Obesidad severa' };}
    function getICCStatus(icc, gender) { const n = parseFloat(icc); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (gender === 'Hombre' || gender === 'hombre') { if (n < 0.95) return { color: '#ffd600', text: 'Bajo' }; if (n >= 0.96 && n <= 1) return { color: '#66bb6a', text: 'Normal' }; if (n > 1) return { color: '#29b6f6', text: 'Alto' }; return { color: '#b0bec5', text: 'Sin clasificación' }; } else if (gender === 'Mujer' || gender === 'mujer') { if (n < 0.80) return { color: '#ffd600', text: 'Bajo' }; if (n >= 0.80 && n < 0.81) return { color: '#ffd600', text: 'Bajo' }; if (n >= 0.81 && n <= 0.85) return { color: '#66bb6a', text: 'Normal' }; if (n > 0.85) return { color: '#29b6f6', text: 'Alto' }; return { color: '#b0bec5', text: 'Sin clasificación' }; } else { return { color: '#b0bec5', text: 'Género desconocido' }; } }
    function getAguaStatus(agua) { const n = parseFloat(String(agua).replace('%','')); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 45) return { color: '#ffd600', text: 'Bajo' }; if (n < 55) return { color: '#66bb6a', text: 'Normal' }; return { color: '#29b6f6', text: 'Alto' }; }
    function getGrasaStatus(grasa) { const n = parseFloat(String(grasa).replace('%','')); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 18) return { color: '#29b6f6', text: 'Muy bajo' }; if (n < 25) return { color: '#66bb6a', text: 'Normal' }; if (n < 30) return { color: '#ffd600', text: 'Elevado' }; if (n < 40) return { color: '#ffb300', text: 'Alto' }; return { color: '#e53935', text: 'Muy alto' }; }
    function getMusculoStatus(musculo) { const n = parseFloat(String(musculo).replace('%','')); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 30) return { color: '#e53935', text: 'Muy bajo' }; if (n < 40) return { color: '#ffd600', text: 'Bajo' }; if (n < 50) return { color: '#66bb6a', text: 'Normal' }; return { color: '#29b6f6', text: 'Alto' }; }
    function getTASStatus(tas) { const n = parseFloat(tas); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 90) return { color: '#29b6f6', text: 'Baja' }; if (n < 120) return { color: '#66bb6a', text: 'Normal' }; if (n < 140) return { color: '#ffd600', text: 'Elevada' }; return { color: '#e53935', text: 'Muy elevada' }; }
    function getTADStatus(tad) { const n = parseFloat(tad); if (isNaN(n)) return { color: '#b0bec5', text: 'N/A' }; if (n < 60) return { color: '#29b6f6', text: 'Baja' }; if (n < 80) return { color: '#66bb6a', text: 'Normal' }; if (n < 90) return { color: '#ffd600', text: 'Elevada' }; return { color: '#e53935', text: 'Muy elevada' }; }
    function normalizar(texto) { return texto.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').replace(/\s+/g, ' ').trim(); }

    function buscar() {
      cargarDatosGoogleSheet(function() {
        const nombreInput = normalizar(document.getElementById('nombre').value);
        const noInput = document.getElementById('no').value.trim();
        const resultadosDiv = document.getElementById('resultados');
        resultadosDiv.innerHTML = '';
        if (!nombreInput || !noInput) {
          resultadosDiv.innerHTML = '<p>Por favor escribe tu Nombre y tu Matricula de empleado.</p>';
          return;
        }
        const resultado = datos.find(d => 
          normalizar(d.NOMBRE).includes(nombreInput) && String(d.No) === noInput
        );
        if (resultado) {
          const imcStatus = getIMCStatus(resultado["IMC"]);
          const iccStatus = getICCStatus(resultado["ICC"], resultado["SEXO"]);
          const aguaStatus = getAguaStatus(resultado["% AGUA CORPORAL"]);
          const grasaStatus = getGrasaStatus(resultado["% GRASA CORPORAL (TOTAL)"]);
          const musculoStatus = getMusculoStatus(resultado["% MASA MUSCULAR (TOTAL)"]);
          const tasStatus = getTASStatus(resultado["TAS"]);
          const tadStatus = getTADStatus(resultado["TAD"]);
          let html = `<table>
            <thead>
              <tr><th colspan="2" class="tituloTabla">${resultado.NOMBRE}</th></tr>
            </thead>
            <tbody>
              <tr><th>Sexo</th><td>${resultado.SEXO ?? ''}</td></tr>
              <tr><th>Cintura</th><td>${resultado["CINTURA"] ?? ''}</td></tr>
              <tr><th>Cadera</th><td>${resultado["CADERA"] ?? ''}</td></tr>
              <tr><th>Muñeca</th><td>${resultado["MUÑECA"] ?? ''}</td></tr>
              <tr><th>Tobillo</th><td>${resultado["TOBILLO"] ?? ''}</td></tr>
              <tr>
                <th>TAS (Tensión Arterial Sistólica)</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["TAS"] ?? ''}</div>
                    <div class="status-cell" style="background:${tasStatus.color};">${tasStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr>
                <th>TAD (Tensión Arterial Diastólica)</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["TAD"] ?? ''}</div>
                    <div class="status-cell" style="background:${tadStatus.color};">${tadStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr><th>Talla</th><td>${resultado["TALLA"] ?? ''}</td></tr>
              <tr><th>Peso</th><td>${resultado["PESO"] ?? ''}</td></tr>
              <tr><th>DCI / BMR (Calorías)</th><td>${resultado["DCI / BMR"] ?? ''}</td></tr>
              <tr><th>Edad Metabólica</th><td>${resultado["EDAD METABOLICA"] ?? ''}</td></tr>
              <tr>
                <th>% Agua Corporal</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["% AGUA CORPORAL"] ?? ''}</div>
                    <div class="status-cell" style="background:${aguaStatus.color};">${aguaStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr class="subtitulo"><th colspan="2"><b>% Grasa Corporal</b></th></tr>
              <tr>
                <th>Total</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["% GRASA CORPORAL (TOTAL)"] ?? ''}</div>
                    <div class="status-cell" style="background:${grasaStatus.color};">${grasaStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr><th>Brazo Izquierdo</th><td>${resultado["BRAZO IZQ."] ?? ''}</td></tr>
              <tr><th>Brazo Derecho</th><td>${resultado["BRAZO DER."] ?? ''}</td></tr>
              <tr><th>Pierna Derecha</th><td>${resultado["PIERNA DER."] ?? ''}</td></tr>
              <tr><th>Pierna Izquierda</th><td>${resultado["PIERNA IZQ."] ?? ''}</td></tr>
              <tr><th>Tronco</th><td>${resultado["TRONCO"] ?? ''}</td></tr>
              <tr class="subtitulo"><th colspan="2"><b>% Masa Muscular</b></th></tr>
              <tr>
                <th>Total</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["% MASA MUSCULAR (TOTAL)"] ?? ''}</div>
                    <div class="status-cell" style="background:${musculoStatus.color};">${musculoStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr><th>Brazo Izquierdo</th><td>${resultado["BRAZO IZQ_1."] ?? ''}</td></tr>
              <tr><th>Brazo Derecho</th><td>${resultado["BRAZO DER_1."] ?? ''}</td></tr>
              <tr><th>Pierna Derecha</th><td>${resultado["PIERNA DER_1."] ?? ''}</td></tr>
              <tr><th>Pierna Izquierda</th><td>${resultado["PIERNA IZQ_1."] ?? ''}</td></tr>
              <tr><th>Tronco</th><td>${resultado["TRONCO_1"] ?? ''}</td></tr>
              <tr class="subtitulo"><th colspan="2"><b>Índices corporales</b></th></tr>
              <tr>
                <th>IMC (Índice de Masa Corporal)</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["IMC"] ?? ''}</div>
                    <div class="status-cell" style="background:${imcStatus.color};">${imcStatus.text}</div>
                  </div>
                </td>
              </tr>
              <tr>
                <th>ICC (Índice cintura-cadera)</th>
                <td style="padding:0;">
                  <div class="split-cell">
                    <div class="value-cell">${resultado["ICC"] ?? ''}</div>
                    <div class="status-cell" style="background:${iccStatus.color};">${iccStatus.text}</div>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>`;
          resultadosDiv.innerHTML = html;
        } else {
          resultadosDiv.innerHTML = '<p>No se encontraron resultados para ese nombre y No.</p>';
        }
      });
    }
    function imprimir() { window.print(); }
    function togglePassword() {
      const pwdInput = document.getElementById('no');
      const eyeIcon = document.getElementById('eyeIcon');
      const eyeSlashIcon = document.getElementById('eyeSlashIcon');
      if (pwdInput.type === "password") {
        pwdInput.type = "text";
        eyeIcon.style.display = "none";
        eyeSlashIcon.style.display = "inline";
      } else {
        pwdInput.type = "password";
        eyeIcon.style.display = "inline";
        eyeSlashIcon.style.display = "none";
      }
    }
    window.onload = function() {
      cargarDatosGoogleSheet();
      document.getElementById('nombre').value = '';
      document.getElementById('no').value = '';
      document.getElementById('resultados').innerHTML = '';
    };
  </script>
</body>
</html>
