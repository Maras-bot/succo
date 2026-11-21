# Succo — il tuo pal nel browser

> Legge la pagina con te, ti dà il succo e non manda i tuoi dati da nessuna parte.

Succo è un companion AI **open source, local-first e privacy-first** che vive nel tuo browser come estensione. Niente account, niente server, niente cloud: il modello gira sul tuo computer.

**Stato:** progetto in costruzione pubblica (build-log). Funziona, ma è giovane. Segnala tutto quello che si rompe.

**Sito previsto:** [succo.app](https://succo.app) · **Codice previsto:** [github.com/getsucco](https://github.com/getsucco)  
(Nulla è ancora registrato o pubblicato.)

---

## Cosa fa

- **TL;DR della pagina:** il succo di un articolo in pochi secondi.
- **Chat con la pagina:** fai domande su quello che stai leggendo.
- **Tono da pal:** risposte chiare, dirette e amichevoli, senza fronzoli.
- **Zero dati in uscita:** il testo della pagina viene elaborato solo in locale.

## Cosa NON fa

- Non ti chiede un account.
- Non invia la pagina, la cronologia o le tue domande a server esterni.
- Non raccoglie statistiche d'uso né telemetria.
- Non vende, non condivide e non conserva i tuoi dati fuori dal tuo computer.

---

## Hardware

### Minimi
- **RAM:** 8 GB (circa 3 GB liberi per il modello)
- **CPU:** x86_64 o Apple Silicon (M1 o successivo)
- **GPU:** non richiesta (funziona in CPU)
- **Spazio su disco:** circa 3 GB liberi (runtime + modello 3B)
- **Sistema operativo:** macOS 13+, Windows 10/11 (64-bit), o una distro Linux recente

### Consigliati
- **RAM:** 16 GB
- **GPU:** NVIDIA con almeno 6 GB di VRAM (CUDA), oppure Apple Silicon con almeno 16 GB di memoria unificata
- **Spazio su disco:** circa 8 GB liberi (spazio per il modello 3B e, se vuoi, anche il 7B)
- **Rete:** serve solo per scaricare Succo e il modello; a runtime non è necessaria

### Modello consigliato (locale)
- **Predefinito:** `qwen2.5:3b` (Ollama) / GGUF `Q4_K_M` (LM Studio) — circa 2 GB di download, 2,5–3,5 GB di RAM in uso
- **Più leggero:** `llama3.2:1b` — circa 1,3 GB di download, 1,5–2 GB di RAM in uso
- **Più qualità (opzionale, ≥16 GB RAM):** `qwen2.5:7b-instruct-q4_K_M`

---

## Come iniziare

### 1. Installa Ollama e il modello
```bash
ollama pull qwen2.5:3b-instruct-q4_K_M
```

### 2. Scarica il progetto
```bash
git clone https://github.com/getsucco/succo.git
cd succo
```

### 3. Carica l'estensione
1. Apri `chrome://extensions`.
2. Attiva la **Modalità sviluppatore**.
3. Clicca **Carica estensione non pacchettizzata** e scegli la cartella del progetto.

### 4. Usalo
Apri una pagina qualunque, clicca l'icona di Succo e chiedi: *"Dammi il succo."*

---

## Privacy locale

Succo è pensato per tenere testo e chat **sul tuo computer**. Il modello gira in locale (Ollama o LM Studio); **nessun byte del contenuto della pagina viene inviato a server esterni** per l'inferenza.

### Cosa fa Succo
- Legge il contenuto della tab corrente **solo sul tuo dispositivo**
- Invia prompt e contesto **solo** a `http://127.0.0.1` (Ollama sulla porta `11434`, oppure LM Studio tipicamente su `1234`)
- Non usa cloud AI, telemetria di inferenza né proxy remoti per le risposte

### Come configurare Ollama in modo sicuro
1. Lascia Ollama in ascolto su **localhost** (default: `127.0.0.1:11434`).
2. **Non** impostare `OLLAMA_HOST=0.0.0.0` e non aprire la porta sul router/firewall.
3. Autorizza solo l'origine dell'estensione:
   ```bash
   OLLAMA_ORIGINS=chrome-extension://<ID_ESTENSIONE_SUCCO>
   ```
   In sviluppo (load unpacked, ID variabile) puoi usare temporaneamente:
   ```bash
   OLLAMA_ORIGINS=chrome-extension://*
   ```
   In produzione preferisci l'ID fisso dello store.
4. Riavvia Ollama dopo aver cambiato le variabili d'ambiente.

### LM Studio
- Avvia il server locale su `127.0.0.1`
- Non abilitare "Serve on Network" / bind su `0.0.0.0`

### Come verificare che resti tutto in locale
1. Con Ollama/LM Studio spenti, Succo deve segnalare che il modello locale non è raggiungibile.
2. Da DevTools dell'estensione → Network: le richieste AI devono andare **solo** a `127.0.0.1` (porte `11434` o `1234`).
3. Controlla il bind della porta:
   - macOS/Linux: `lsof -iTCP:11434 -sTCP:LISTEN` → deve risultare `127.0.0.1`, non `0.0.0.0`
   - Windows: `netstat -ano | findstr 11434`
4. Test offline: dopo aver scaricato il modello, disconnetti la rete; TL;DR e chat devono continuare a funzionare.

> **Nota:** scaricare Ollama/LM Studio e i pesi del modello richiede internet. Una volta installati, Succo può lavorare senza connessione.

### Permessi dell'estensione

| Permesso | Perché serve |
|---|---|
| `activeTab` | Leggere la pagina che stai guardando, solo quando lo chiedi tu. |
| `storage` | Salvare le tue preferenze in locale. |
| `http://127.0.0.1/*` | Parlare con il modello che gira sul tuo computer. |

---

## Disclaimer

- Succo è software sperimentale, fornito "così com'è", senza garanzie.
- Le risposte sono generate da un modello AI e possono contenere errori. Verifica sempre le informazioni importanti.
- La privacy dipende anche dal tuo ambiente: altre estensioni, il browser e il sistema operativo hanno le loro regole.
- Se colleghi un modello remoto al posto di quello locale, i dati usciranno dal tuo computer. È una scelta tua, e l'estensione te lo segnala.
- **Non siamo Succo AI** (succo.ai): siamo un'estensione browser locale, non un SaaS di form o sondaggi.

---

## Contribuire

Issue, idee e pull request sono benvenute. Leggi `CONTRIBUTING.md` prima di iniziare.

**Regola d'oro del progetto:** nessuna funzione che mandi dati fuori dal browser senza un consenso esplicito e visibile.

## Licenza

Rilasciato con licenza [MIT](LICENSE). © 2026 Maras Mignani.
