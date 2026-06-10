class EmailSenderWidget extends HTMLElement {
  connectedCallback() {
    this.render();
  }

  render() {
    this.innerHTML = `
      <style>
        .email-widget {
          padding: 24px;
          font-family: CiscoSansTT, Arial, sans-serif;
          max-width: 400px;
        }
        .email-widget h3 {
          margin: 0 0 20px;
          color: #333;
          font-size: 16px;
        }
        .email-widget label {
          font-size: 13px;
          color: #555;
          font-weight: bold;
        }
        .email-widget input[type="email"] {
          width: 100%;
          margin: 6px 0 16px;
          padding: 10px;
          border: 1px solid #ccc;
          border-radius: 4px;
          box-sizing: border-box;
          font-size: 14px;
        }
        .email-widget input[type="email"]:focus {
          outline: none;
          border-color: #00BCF2;
        }
        .email-widget button {
          background: #00BCF2;
          color: white;
          border: none;
          padding: 10px 24px;
          border-radius: 4px;
          cursor: pointer;
          font-size: 14px;
          width: 100%;
        }
        .email-widget button:hover { background: #0098C4; }
        .email-widget button:disabled {
          background: #ccc;
          cursor: not-allowed;
        }
        #status-msg {
          margin-top: 12px;
          font-size: 13px;
          text-align: center;
        }
      </style>
      <div class="email-widget">
        <h3>📧 Enviar Email al Cliente</h3>
        <label>Correo del destinatario</label>
        <input
          type="email"
          id="destinatario"
          placeholder="cliente@ejemplo.com"
        />
        <button id="send-btn">Enviar</button>
        <div id="status-msg"></div>
      </div>
    `;

    this.querySelector('#send-btn').addEventListener('click', () => this.sendEmail());

    // También permite enviar con la tecla Enter
    this.querySelector('#destinatario').addEventListener('keydown', (e) => {
      if (e.key === 'Enter') this.sendEmail();
    });
  }

  async sendEmail() {
    const destinatario = this.querySelector('#destinatario').value.trim();
    const statusEl = this.querySelector('#status-msg');
    const btn = this.querySelector('#send-btn');

    // Validación básica de email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!destinatario || !emailRegex.test(destinatario)) {
      statusEl.style.color = 'red';
      statusEl.textContent = '⚠️ Ingresa un correo válido.';
      return;
    }

    const WEBHOOK_URL = 'https://hooks.uk.webexconnect.io/events/0FYKERSRU4'; // ← pega aquí tu URL de Webex Connect

    try {
      btn.disabled = true;
      statusEl.style.color = 'gray';
      statusEl.textContent = 'Enviando...';

      const resp = await fetch(WEBHOOK_URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ destinatario })
      });

      if (resp.ok) {
        statusEl.style.color = 'green';
        statusEl.textContent = '✅ Email enviado correctamente.';
        this.querySelector('#destinatario').value = '';
      } else {
        throw new Error(`HTTP ${resp.status}`);
      }
    } catch (err) {
      statusEl.style.color = 'red';
      statusEl.textContent = `❌ Error al enviar: ${err.message}`;
    } finally {
      btn.disabled = false;
    }
  }
}

customElements.define('email-sender-widget', EmailSenderWidget);
