<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ChatBot DNI → Contraseña</title>
  <style>
    body{font-family:system-ui,Arial;margin:0;display:flex;align-items:center;justify-content:center;height:100vh;background:#f3f4f6}
    .card{background:#fff;padding:20px;border-radius:10px;box-shadow:0 6px 20px rgba(0,0,0,.08);width:360px}
    .chat{height:260px;overflow:auto;border:1px solid #e5e7eb;padding:10px;border-radius:6px;margin-bottom:10px;background:#fcfcfd}
    .msg{margin:6px 0;padding:8px 10px;border-radius:8px;display:inline-block}
    .user{background:#e0f2fe}
    .bot{background:#eef2ff}
    .controls{display:flex;gap:8px}
    input[type="text"]{flex:1;padding:10px;border-radius:6px;border:1px solid #d1d5db}
    button{padding:10px 12px;border-radius:6px;border:none;background:#2563eb;color:white;cursor:pointer}
    small.note{color:#6b7280}
  </style>
</head>
<body>
  <div class="card" role="main">
    <h3>Chatbot: DNI → Contraseña</h3>
    <div class="chat" id="chat" aria-live="polite"></div>

    <div class="controls">
      <input id="dniInput" type="text" placeholder="Ingresa DNI (solo números)" />
      <button id="sendBtn">Enviar</button>
    </div>
    <p><small class="note">Demo con DNIs y contraseñas predefinidos.</small></p>
  </div>

  <script>
    // MAPA DNI → Contraseña (modifica estos valores)
    const mapa = {
      "12345678": "clave2025",
      "87654321": "miPass123",
      "55555555": "superclave"
    };

    const chat = document.getElementById('chat');
    const dniInput = document.getElementById('dniInput');
    const sendBtn = document.getElementById('sendBtn');

    function pushMessage(text, who='bot') {
      const div = document.createElement('div');
      div.className = 'msg ' + (who === 'user' ? 'user' : 'bot');
      div.textContent = text;
      chat.appendChild(div);
      chat.scrollTop = chat.scrollHeight;
    }

    function procesarDNI(dni) {
      dni = dni.trim();
      if (!/^\d{6,11}$/.test(dni)) {
        pushMessage('❌ DNI inválido. Usa sólo números (6-11 dígitos).', 'bot');
        return;
      }
      if (mapa.hasOwnProperty(dni)) {
        pushMessage(`✅ Contraseña: ${mapa[dni]}`, 'bot');
      } else {
        pushMessage('⚠️ DNI no encontrado en la base.', 'bot');
      }
    }

    sendBtn.addEventListener('click', () => {
      const val = dniInput.value;
      if (!val) return;
      pushMessage(val, 'user');
      procesarDNI(val);
      dniInput.value = '';
      dniInput.focus();
    });

    dniInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') sendBtn.click();
    });

    pushMessage('👋 Hola! Ingresa tu DNI para obtener la contraseña.', 'bot');
  </script>
</body>
</html>
