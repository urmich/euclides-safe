# הכספת של אוקלידס — Euclid's Safe

A Hebrew (RTL) geometry escape-room web game for grades 9–10. Built as a **single self-contained `index.html`** — inline CSS, vanilla JS, inline SVG. **Zero dependencies, zero build step.**

## Files
- `index.html` — the entire game.
- `staticwebapp.config.json` — Azure Static Web Apps config (SPA fallback + headers).

## Run locally

Easiest — just open the file:

```bash
xdg-open index.html      # Linux
```

Or serve it (recommended, so audio/localStorage behave like production):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy to Azure Static Web Apps (from your local machine)

You have a **private subscription** and can deploy directly. Two options:

### Option A — Azure CLI (`az`)
```bash
az login                                   # sign in
az account set --subscription "<your-subscription-id>"

# Create the Static Web App (only once)
az staticwebapp create \
  --name euclids-safe \
  --resource-group <your-rg> \
  --location "westeurope" \
  --sku Free

# Get a deployment token
TOKEN=$(az staticwebapp secrets list --name euclids-safe \
  --query "properties.apiKey" -o tsv)

# Deploy the current folder (the app root is "." since there is no build)
npx @azure/static-web-apps-cli deploy . \
  --deployment-token "$TOKEN" \
  --env production
```

### Option B — SWA CLI directly
```bash
npm install -g @azure/static-web-apps-cli
swa login            # interactive Azure login
swa deploy ./ --env production
```

> No build step is required. The deploy "app location" is the folder containing `index.html` (`.`), and there is no `output_location`/`api_location`.

## Game design notes
- Rooms 1–4: 3-stage lock (identify structure → choose theorem → numeric answer). Codes: 65, 73, 48, 75.
- Room 5: critical-thinking error lab (similar triangles ≠ equal sides).
- Room 6: final keypad — sequence **65734875**.
- Features: localStorage resume on refresh, global MM:SS timer, hint button after 3 consecutive wrong answers, Web Audio feedback sounds, shake animation on error, confetti + final time on victory, full keyboard support (Enter / numeric keypad).
