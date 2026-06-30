<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>16-char Code Generator / Tester</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial; padding: 24px; max-width: 800px; margin: auto; }
    h1 { margin-top: 0; }
    .controls { display:flex; gap:8px; align-items:center; margin-bottom: 12px; }
    button { padding:8px 12px; font-size:14px; }
    input[type="number"] { width:80px; padding:6px; }
    .list { margin-top:16px; border:1px solid #ddd; padding:8px; border-radius:6px; min-height:72px; background:#fafafa; }
    .item { display:flex; align-items:center; gap:8px; padding:6px; border-radius:4px; margin-bottom:6px; }
    .item .code { font-family: "Courier New", monospace; font-weight:600; }
    .pending { background: #fffbe6; }    /* yellow-ish while checking */
    .bad { background: #ffe8e8; color:#700; } /* red for fail */
    .good { background: #e8ffea; color:#064; } /* green for success */
    label.small { font-size:13px; color:#444; }
    .info { margin-left:8px; color:#666; font-size:13px; }
    .settings { display:flex; gap:12px; align-items:center; margin-left:12px; }
    .footer { margin-top:18px; color:#666; font-size:13px; }
  </style>
</head>
<body>
  <h1>16-character Code Generator & Tester</h1>

  <div class="controls">
    <button id="nextBtn">Next</button>
    <button id="stopBtn" disabled>Stop</button>
    <div class="settings">
      <label class="small">Simulated success chance:
        <input id="chance" type="number" min="0" max="100" value="5"> %
      </label>
      <span class="info">(Higher = more likely to stop quickly)</span>
    </div>
  </div>

  <div>
    <label class="small">Recent codes (first/oldest is removed when a code fails):</label>
  </div>
  <div id="list" class="list" aria-live="polite"></div>

  <div class="footer">
    How it works: Click Next to start. The app generates a 16-character alphanumeric code and "checks" it (simulated). If the check fails it marks the code red and then erases the first (oldest) code from the list and continues automatically; if a code passes the check it turns green and the process stops. Use the percentage input to set the simulated success probability.
  </div>

  <script>
    // --- Configuration / helpers ---
    const CODE_LEN = 16;
    const CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const nextBtn = document.getElementById('nextBtn');
    const stopBtn = document.getElementById('stopBtn');
    const listEl = document.getElementById('list');
    const chanceInput = document.getElementById('chance');

    let running = false;
    let stopRequested = false;

    function generateCode(len = CODE_LEN) {
      let out = '';
      const r = crypto.getRandomValues ? crypto.getRandomValues(new Uint32Array(len)) : null;
      for (let i = 0; i < len; i++) {
        if (r) {
          out += CHARS[r[i] % CHARS.length];
        } else {
          out += CHARS[Math.floor(Math.random() * CHARS.length)];
        }
      }
      return out;
    }

    function addItem(code, state = 'pending') {
      const el = document.createElement('div');
      el.className = 'item ' + state;
      const codeSpan = document.createElement('span');
      codeSpan.className = 'code';
      codeSpan.textContent = code;
      const stateSpan = document.createElement('span');
      stateSpan.className = 'state';
      stateSpan.textContent = state === 'pending' ? 'checking...' : (state === 'good' ? 'GOOD' : 'BAD');
      el.appendChild(codeSpan);
      el.appendChild(stateSpan);
      listEl.appendChild(el);
      return el;
    }

    function removeFirstItem() {
      const first = listEl.firstElementChild;
      if (first) first.remove();
    }

    // Simulated validator: returns a Promise<boolean>
    // Uses the chance input: percentage chance to return true.
    function validateCodeSimulated(code) {
      const chance = Number(chanceInput.value) || 0;
      // simulate network/processing delay 200-700ms
      const delay = 200 + Math.random() * 500;
      return new Promise(resolve => {
        setTimeout(() => {
          // success if random < chance%
          const success = (Math.random() * 100) < chance;
          resolve(!!success);
        }, delay);
      });
    }

    // The main loop: generate a code, add to list, validate.
    async function iterateOnce() {
      if (stopRequested) return false;
      const code = generateCode();
      const itemEl = addItem(code, 'pending');
      // mark visually while checking
      await sleep(60); // tiny UX delay so pending shows
      const ok = await validateCodeSimulated(code);
      if (ok) {
        itemEl.classList.remove('pending');
        itemEl.classList.add('good');
        itemEl.querySelector('.state').textContent = 'GOOD';
        // stop the process
        running = false;
        stopRequested = false;
        nextBtn.disabled = false;
        stopBtn.disabled = true;
        return true;
      } else {
        // mark as bad
        itemEl.classList.remove('pending');
        itemEl.classList.add('bad');
        itemEl.querySelector('.state').textContent = 'BAD';
        // remove the first (oldest) code from list after small delay
        await sleep(180);
        removeFirstItem();
        // continue (return false to indicate not done)
        return false;
      }
    }

    async function runLoop() {
      running = true;
      stopRequested = false;
      nextBtn.disabled = true;
      stopBtn.disabled = false;
      while (!stopRequested) {
        const done = await iterateOnce();
        if (done) break;
        // small throttle so UI doesn't starve
        await sleep(80);
      }
      // ensure buttons reflect state
      running = false;
      nextBtn.disabled = false;
      stopBtn.disabled = true;
    }

    // Basic sleep helper
    function sleep(ms) { return new Promise(res => setTimeout(res, ms)); }

    // UI event handlers
    nextBtn.addEventListener('click', () => {
      if (!running) {
        // clear previous list and start
        // depending on desired behaviour we can clear or keep; spec implies older ones erased when fail
        // We'll keep the existing list but the loop will remove items when failures happen.
        runLoop();
      }
    });

    stopBtn.addEventListener('click', () => {
      if (running) {
        stopRequested = true;
        // let the current check finish then stop; user can also disable immediately:
        // To immediate stop: uncomment the line below:
        // running = false;
      }
    });

    // Allow pressing Enter on the chance input to start
    chanceInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        if (!running) runLoop();
      }
    });

    // seed the list with nothing initially; optional quick demo: keep empty

    // Accessibility: announce when a code becomes GOOD
    const observer = new MutationObserver(muts => {
      for (const m of muts) {
        for (const n of m.addedNodes) {
          if (n.nodeType === 1 && n.classList.contains('good')) {
            n.setAttribute('aria-live','polite');
          }
        }
      }
    });
    observer.observe(listEl, { childList: true, subtree: true });

  </script>
</body>
</html>
