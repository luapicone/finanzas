# FinTrack — Tracker Financiero Personal

Tracker de inversiones, gastos e ingresos con precios en tiempo real y chat con IA.

## Estructura del proyecto

```
fintrack/
├── index.html          # Estructura principal de la app
├── css/
│   └── style.css       # Todos los estilos (tema oscuro)
├── js/
│   ├── storage.js      # Persistencia en localStorage
│   ├── prices.js       # Precios en tiempo real (Yahoo Finance + CoinGecko)
│   ├── render.js       # Funciones de renderizado del DOM
│   ├── chat.js         # Integración con Claude API
│   └── app.js          # Inicialización y orquestación
└── README.md
```

## Cómo usar

### Opción 1 — Abrir directo en el navegador
Simplemente abrí `index.html` en tu navegador. No necesita servidor.

> ⚠️ Nota: algunos navegadores bloquean fetch() desde archivos locales.
> Si ves errores de CORS, usá la Opción 2.

### Opción 2 — Servidor local (recomendado)

**Con Python:**
```bash
cd fintrack
python3 -m http.server 8080
# Abrí http://localhost:8080
```

**Con Node.js (npx):**
```bash
cd fintrack
npx serve .
# Abrí la URL que aparece en la terminal
```

**Con VS Code:**
Instalá la extensión "Live Server" y hacé click en "Go Live".

## Funcionalidades

### Chat con IA
Hablá con el asistente en lenguaje natural:
- **Inversiones:** "Compré 10 AAPL a $150" · "Compré 0.5 BTC a $40000" · "Tengo 5 SPY a $450"
- **Ingresos:** "Cobré sueldo $500.000" · "Ingreso freelance $80.000"
- **Gastos:** "Gasté $8.500 en supermercado" · "Pagué $80.000 de alquiler"
- **Consultas:** "¿Cómo está mi portafolio?" · "¿Cuánto gasté este mes?"

### Precios en tiempo real
- **Acciones y ETFs** (AAPL, NVDA, MELI, SPY, QQQ, etc.) → Yahoo Finance (gratis)
- **Criptomonedas** (BTC, ETH, SOL, ADA, etc.) → CoinGecko (gratis)
- Auto-refresh cada 60 segundos
- Actualización manual con el botón "↻ Actualizar"

### Persistencia
Todos los datos se guardan en `localStorage` del navegador. Permanecen entre sesiones.

### Vistas
- **Resumen:** cards con capital, PnL, ingresos, gastos y balance del mes
- **Operaciones:** tabla completa con precio compra/actual, capital valorizado y PnL por operación + PnL general
- **Gastos:** historial completo con barras por categoría
- **Ingresos:** historial completo con barras por categoría

## Configuración de la API de Claude

La app usa la API de Anthropic para parsear el lenguaje natural.
El endpoint `/v1/messages` debe estar accesible desde el navegador.

Si usás tu propia API key, agregala en `js/chat.js`:

```javascript
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': 'TU_API_KEY_ACÁ',           // ← agregar
    'anthropic-version': '2023-06-01',        // ← agregar
    'anthropic-dangerous-direct-browser-access': 'true', // ← agregar
  },
  ...
});
```

> ⚠️ No expongas tu API key en código público.

## Tickers soportados

### Acciones populares
AAPL, MSFT, GOOGL, AMZN, NVDA, META, TSLA, MELI, BABA, JPM, NFLX, DIS, V, MA...

### ETFs populares
SPY, QQQ, IWM, VTI, VOO, DIA, GLD, SLV, VNQ, ARKK...

### Criptos
BTC, ETH, SOL, BNB, ADA, AVAX, DOT, MATIC, LINK, XRP, LTC, DOGE, SHIB, UNI, ATOM, NEAR, OP, ARB, TON, SUI, APT...

### Acciones argentinas (ADRs)
GGAL, YPF, PAM, BMA, LOMA, CEPU, SUPV, EDN, TGS...

## Notas

- Los precios de Yahoo Finance pueden fallar si el navegador bloquea la request por CORS. En ese caso, usá un servidor local.
- CoinGecko tiene límite de ~30 llamadas/minuto en el plan gratuito.
- Los datos se guardan localmente en tu navegador, no en ningún servidor externo.
