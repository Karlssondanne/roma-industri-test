<!DOCTYPE html>
<html lang="sv">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ROMA Test – Industri</title>
  <style>
    :root{
      --bg:#0f172a; --card:#111827; --text:#e5e7eb; --muted:#9ca3af;
      --accent:#22c55e; --accent-2:#3b82f6; --warn:#ef4444; --focus:#f59e0b; --border:#1f2937;
    }
    html,body{height:100%}
    body{margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,Inter,Arial;
      background:linear-gradient(180deg,#0b1226,#0f172a 30%,#0b1226); color:var(--text);
      display:flex; align-items:center; justify-content:center;}
    .app{ width:min(900px,92vw); margin:24px; }
    .card{ background:linear-gradient(180deg,#0b1020 0%,#101827 60%,#0b1020);
      border:1px solid var(--border); border-radius:16px; padding:20px 18px; box-shadow:0 10px 30px rgba(0,0,0,.45);}
    h1{font-size:1.6rem;margin:0 0 8px 0}
    .sub{color:var(--muted);margin-bottom:16px}
    .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
    .input, select{background:#0b1222; color:var(--text); border:1px solid var(--border);
      border-radius:10px; padding:10px 12px; outline:none; width:100%;}
    .input:focus, select:focus{border-color:var(--focus); box-shadow:0 0 0 3px rgba(245,158,11,.2)}
    label{font-weight:600; font-size:.95rem}
    .btn{ background:var(--accent-2); color:white; padding:12px 16px; border:none; border-radius:10px; cursor:pointer;
      font-weight:700; transition:transform .04s ease, filter .2s ease; user-select:none;}
    .btn:hover{filter:brightness(1.08)} .btn:active{transform:translateY(1px)}
    .btn.secondary{ background:#0b1222; color:var(--text); border:1px solid var(--border) }
    .btn.warn{ background:var(--warn) }
    .meta{display:flex; gap:8px; flex-wrap:wrap; align-items:center; margin:6px 0 14px 0}
    .chip{ background:#0b1222; border:1px solid var(--border); padding:6px 10px; border-radius:999px;
      color:var(--muted); font-weight:600; font-size:.85rem;}
    .progress{height:10px; background:#0b1222; border:1px solid var(--border); border-radius:999px; overflow:hidden}
    .bar{height:100%; width:0%; background:linear-gradient(90deg,var(--accent),#16a34a)}
    .question{margin:10px 0 8px 0; font-size:1.1rem; font-weight:700}
    .category{color:var(--accent); font-weight:800; letter-spacing:.3px; text-transform:uppercase; font-size:.8rem}
    .options{display:grid; gap:10px; margin-top:12px}
    .opt{ display:flex; gap:10px; align-items:flex-start; padding:12px; border-radius:12px;
      border:1px solid var(--border); background:#0b1222; cursor:pointer;}
    .opt input{margin-top:3px} .opt:hover{border-color:#23314d}
    .footer{display:flex; justify-content:space-between; align-items:center; gap:12px; margin-top:16px}
    .timer{font-weight:800; letter-spacing:.5px} .muted{color:var(--muted)} .hide{display:none}
    .result-grid{display:grid; grid-template-columns:1fr; gap:12px; margin-top:12px}
    .stat{ background:#0b1222; border:1px solid var(--border); border-radius:12px; padding:12px;
      display:flex; justify-content:space-between; align-items:center}
    .good{color:var(--accent)} .bad{color:var(--warn)}
    .review{margin-top:14px}
    .rev-item{border:1px solid var(--border); border-radius:10px; padding:10px; background:#0b1222; margin-bottom:10px}
    .rev-item .correct{color:var(--accent)} .rev-item .wrong{color:var(--warn)}
    .kbd{font-family:ui-monospace, Menlo, Consolas, monospace; background:#0b1222; border:1px solid var(--border); padding:2px 6px; border-radius:6px}
    @media (min-width:640px){ .result-grid{grid-template-columns:repeat(2,1fr)} }
  </style>
</head>
<body>
  <main class="app">
    <!-- START -->
    <section id="start" class="card" aria-labelledby="start-title">
      <h1 id="start-title">ROMA Test – Industri</h1>
      <p class="sub">Mäter <strong>Följa instruktioner</strong>, <strong>Uppmärksamhet på detaljer</strong> och <strong>Numerisk slutledningsförmåga</strong>. Tidsbegränsning: 20 minuter.</p>
      <div class="row" style="gap:12px">
        <div style="flex:2 1 260px">
          <label for="participant">Deltagarens namn</label>
          <input id="participant" class="input" placeholder="För- och efternamn" />
        </div>
        <div style="flex:1 1 160px">
          <label for="duration">Tidsgräns</label>
          <select id="duration" class="input" aria-label="Välj tidsgräns">
            <option value="20" selected>20 min (standard)</option>
            <option value="15">15 min</option>
            <option value="10">10 min</option>
            <option value="0">Ingen tidsgräns</option>
          </select>
        </div>
      </div>
      <div class="meta">
        <span class="chip">Mobil &amp; dator</span>
        <span class="chip">1 fråga i taget</span>
        <span class="chip">Resultat per område</span>
      </div>
      <button id="startBtn" class="btn" aria-describedby="kb">Starta test</button>
      <p id="kb" class="muted" style="margin-top:10px">Tips: Använd <span class="kbd">Tab</span> och piltangenter för att navigera.</p>
    </section>

    <!-- QUIZ -->
    <section id="quiz" class="card hide" aria-live="polite">
      <div class="meta">
        <span id="cat" class="chip category">Kategori</span>
        <span id="progressText" class="chip">Fråga 1 / N</span>
        <span id="timer" class="chip timer">20:00</span>
      </div>
      <div class="progress" aria-label="Framsteg"><div id="bar" class="bar"></div></div>
      <p id="qtext" class="question"></p>
      <div id="options" class="options" role="radiogroup" aria-label="Svarsalternativ"></div>
      <div class="footer">
        <button id="prevBtn" class="btn secondary">Föregående</button>
        <div style="display:flex; gap:8px">
          <button id="saveBtn" class="btn secondary">Spara &amp; fortsätt</button>
          <button id="nextBtn" class="btn">Nästa</button>
          <button id="submitBtn" class="btn warn">Avsluta test</button>
        </div>
      </div>
      <p class="muted" id="timeWarn" aria-live="assertive"></p>
    </section>

    <!-- RESULT -->
    <section id="result" class="card hide" aria-labelledby="result-title">
      <h1 id="result-title">Resultat</h1>
      <p class="sub" id="summary"></p>
      <div class="result-grid" id="stats"></div>
      <div style="display:flex; gap:8px; flex-wrap:wrap; margin-top:12px">
        <button id="exportBtn" class="btn secondary">Exportera resultat (CSV)</button>
        <button id="retryBtn" class="btn">Gör om testet</button>
      </div>
      <div class="review" id="review"></div>
    </section>
  </main>

  <script>
    // ===== KONFIG =====
    const TEST_NAME = "ROMA Test – Industri";
    const PASS_THRESHOLD = 0.70; // 70% godkänt
    const QUESTIONS_PER_RUN = 12; // visade frågor (upp till storleken på banken)

    const QUESTIONS = [
      // --- FÖLJA INSTRUKTIONER ---
      {id:1,cat:"Följa instruktioner",text:"Arbetsinstruktion: 1) Kontrollera artikelnummer 2) Montera del A i del B 3) Dra åt med 10 Nm 4) Märk produkten med etikett. Vilket steg kommer direkt efter att del A monterats i del B?",options:["Märk produkten","Dra åt med 10 Nm","Kontrollera artikelnummer","Packa produkten"],correct:1,rationale:"Efter montering dras rätt moment (10 Nm) innan märkning."},
      {id:2,cat:"Följa instruktioner",text:"En säkerhetsinstruktion anger ordningen: 1) Sätt på låsning 2) Stoppa maskinen 3) Tagga ut. Vilket är första steget?",options:["Stoppa maskinen","Sätt på låsning","Tagga ut","Kontrollera verktyg"],correct:1,rationale:"Låsning först för att förhindra oavsiktlig start (LOTO)."},
      {id:3,cat:"Följa instruktioner",text:"5S innehåller: Sortera, Systematisera, Städa, Standardisera, Skapa vana. Vilket steg handlar om att märka upp verktyg för enkel åtkomst?",options:["Skapa vana","Städa","Systematisera","Standardisera"],correct:2,rationale:"Systematisera = ordning/placering/uppmärkning."},
      {id:4,cat:"Följa instruktioner",text:"Checklistan kräver att moment X signeras före moment Y. Y är signerat men X saknas. Vad gör du?",options:["Fortsätt; någon glömde X","Signera X i efterhand utan att göra det","Återställ: utför & signera X, verifiera Y; rapportera avvikelse","Ignorera"],correct:2,rationale:"Rätt är att återställa sekvensen och dokumentera avvikelsen."},

      // --- UPPMÄRKSAMHET PÅ DETALJER ---
      {id:5,cat:"Uppmärksamhet på detaljer",text:"Vilket serienummer skiljer sig från de andra?",options:["AB‑4739‑X","AB‑4739‑X","AB‑4739‑Y","AB‑4739‑X"],correct:2,rationale:"Tredje alternativet har annat suffix (Y)."},
      {id:6,cat:"Uppmärksamhet på detaljer",text:"Vilket artikelnummer innehåller bokstaven O (inte siffran 0)?",options:["100‑77‑K","1O0‑77‑K","110‑77‑K","100‑77‑R"],correct:1,rationale:"‘1O0’ har ett O i mitten."},
      {id:7,cat:"Uppmärksamhet på detaljer",text:"Välj etiketten som exakt matchar: LOT:2026‑02‑03, BATCH 14, REV C",options:["LOT:2026‑02‑03, BATCH 14, REV C","LOT:2026‑2‑03, BATCH 14, REV C","LOT:2026‑02‑03, BATCH 15, REV C","LOT:2026‑02‑03, BATCH 14, REV D"],correct:0,rationale:"Endast det första matchar exakt."},
      {id:8,cat:"Uppmärksamhet på detaljer",text:"Vilken rad har ett skrivfel (stavning/tecken)?",options:["Kalibrering genomförs 07:30","Kalibrering genomförs 07;30","Kalibrering genomförs 07:30 ","Kalibrering genomförs 07:30"],correct:1,rationale:"Semikolon (;) är felaktigt i tidangivelsen."},

      // --- NUMERISK SLUTLEDNINGSFÖRMÅGA ---
      {id:9,cat:"Numerisk slutledningsförmåga",text:"Talserie: 2 – 4 – 8 – 16 – ?",options:["24","18","32","64"],correct:2,rationale:"Dubblering → 32."},
      {id:10,cat:"Numerisk slutledningsförmåga",text:"En maskin producerar 15 detaljer på 5 minuter. Hur många på 20 minuter (samma takt)?",options:["45","60","75","90"],correct:1,rationale:"3 st/min → 20 min = 60."},
      {id:11,cat:"Numerisk slutledningsförmåga",text:"Spill är 2% av 850 enheter. Hur många skrotas?",options:["17","15","18","14"],correct:0,rationale:"0,02 × 850 = 17."},
      {id:12,cat:"Numerisk slutledningsförmåga",text:"Takt 120 st/h ökas med 10%. Ny takt?",options:["130","132","142","118"],correct:1,rationale:"10% av 120 är 12 → 132 st/h."},
    ];

    // ===== HJÄLPMETODER =====
    function shuffle(a){ const arr=[...a]; for(let i=arr.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [arr[i],arr[j]]=[arr[j],arr[i]];} return arr; }
    function shuffleOptions(options){ const idx = options.map((t,i)=>({t,i})); const s = shuffle(idx); return { options: s.map(o=>o.t), map: s.map(o=>o.i) }; }
    function fmtTime(sec){ const m = Math.floor(sec/60).toString().padStart(2,'0'); const s = Math.floor(sec%60).toString().padStart(2,'0'); return `${m}:${s}`; }

    // Balanserad urvalsfunktion (så långt det går)
    function buildBalancedQuiz(all, perRun){
      const byCat = {};
      all.forEach(q => { byCat[q.cat] = byCat[q.cat] || []; byCat[q.cat].push(q); });
      Object.values(byCat).forEach(arr => arr.sort(()=>Math.random()-0.5));
      const cats = Object.keys(byCat);
      const out = [];
      let i=0;
      while(out.length < Math.min(perRun, all.length)){
        const c = cats[i % cats.length];
        if(byCat[c].length) out.push(byCat[c].shift());
        i++;
        if(i>5000) break;
      }
      return shuffle(out);
    }

    // ===== TILLSTÅND =====
    let QUIZ = buildBalancedQuiz(QUESTIONS, QUESTIONS_PER_RUN);
    let idx = 0;
    let answers = {}; // qid -> visningsIndex
    let timerId = null;
    let remainingSec = 20 * 60;

    // ===== DOM =====
    const elStart = document.getElementById("start");
    const elQuiz = document.getElementById("quiz");
    const elResult = document.getElementById("result");
    const elStartBtn = document.getElementById("startBtn");
    const elParticipant = document.getElementById("participant");
    const elDuration = document.getElementById("duration");
    const elQText = document.getElementById("qtext");
    const elOptions = document.getElementById("options");
    const elCat = document.getElementById("cat");
    const elProgressText = document.getElementById("progressText");
    const elBar = document.getElementById("bar");
    const elTimer = document.getElementById("timer");
    const elTimeWarn = document.getElementById("timeWarn");
    const elPrev = document.getElementById("prevBtn");
    const elNext = document.getElementById("nextBtn");
    const elSave = document.getElementById("saveBtn");
    const elSubmit = document.getElementById("submitBtn");
    const elSummary = document.getElementById("summary");
    const elStats = document.getElementById("stats");
    const elReview = document.getElementById("review");
    const elExport = document.getElementById("exportBtn");
    const elRetry = document.getElementById("retryBtn");

    // ===== TIMER =====
    function updateTimer(){
      if(remainingSec <= 0){
        elTimer.textContent = "00:00";
        elTimeWarn.textContent = "Tiden är slut. Testet avslutas automatiskt.";
        submitQuiz(); return;
      }
      remainingSec -= 1;
      elTimer.textContent = fmtTime(remainingSec);
      if(remainingSec === 300){ elTimeWarn.textContent = "5 minuter kvar."; }
      else if(remainingSec === 60){ elTimeWarn.textContent = "1 minut kvar."; }
      else if(remainingSec <= 10){ elTimeWarn.textContent = `Tiden tar slut om ${remainingSec} s.`; }
    }
    function startTimer(){
      const minutes = parseInt(elDuration.value,10);
      if(minutes > 0){
        remainingSec = minutes * 60;
        elTimer.parentElement.classList.remove("hide");
        elTimer.textContent = fmtTime(remainingSec);
        timerId = setInterval(updateTimer, 1000);
      } else {
        elTimer.parentElement.classList.add("hide");
      }
    }

    // ===== UI =====
    function render(){
      const q = QUIZ[idx];
      elCat.textContent = q.cat;
      elQText.textContent = q.text;

      // Slumpa svarsalternativ per fråga (en gång)
      if(!q._shuffled){
        const { options, map } = shuffleOptions(q.options);
        q._opts = options;     // visningsordning
        q._map  = map;         // visningsIndex -> originalIndex
        q._shuffled = true;
      }

      elOptions.innerHTML = "";
      q._opts.forEach((opt, visIndex) => {
        const id = `q${q.id}_o${visIndex}`;
        const wrap = document.createElement("label");
        wrap.className = "opt";
        wrap.setAttribute("for", id);

        const radio = document.createElement("input");
        radio.type = "radio";
        radio.name = `q_${q.id}`;
        radio.id = id;
        radio.value = visIndex;
        radio.checked = answers[q.id] === visIndex;
        radio.addEventListener("change", ()=> { answers[q.id] = visIndex; });

        const text = document.createElement("div");
        text.innerText = opt;

        wrap.appendChild(radio);
        wrap.appendChild(text);
        elOptions.appendChild(wrap);
      });

      elProgressText.textContent = `Fråga ${idx+1} / ${QUIZ.length}`;
      elBar.style.width = `${((idx+1)/QUIZ.length)*100}%`;

      elPrev.disabled = idx === 0;
      elNext.disabled = idx === QUIZ.length - 1;
    }

    function next(){ if(idx < QUIZ.length - 1){ idx++; render(); } }
    function prev(){ if(idx > 0){ idx--; render(); } }

    // ===== RESULTAT =====
    function submitQuiz(){
      clearInterval(timerId);
      let total = 0;
      const byCat = {};
      const possibleByCat = {};
      QUIZ.forEach(q=>{
        if(!(q.cat in byCat)){ byCat[q.cat]=0; possibleByCat[q.cat]=0; }
        possibleByCat[q.cat]+=1;
        if(answers[q.id] != null){
          const chosenOriginal = q._map ? q._map[answers[q.id]] : answers[q.id];
          if(chosenOriginal === q.correct){ total+=1; byCat[q.cat]+=1; }
        }
      });
      const percent = total / QUIZ.length;
      const pass = percent >= PASS_THRESHOLD;

      elQuiz.classList.add("hide");
      elResult.classList.remove("hide");

      const participant = (elParticipant.value || "Okänd").trim();
      elSummary.innerHTML = `<strong>${participant}</strong> – ${new Date().toLocaleString()}<br/>
      Totalpoäng: <strong>${total}/${QUIZ.length}</strong> (${Math.round(percent*100)}%) –
      <span class="${pass?'good':'bad'}">${pass?'GODKÄND':'EJ GODKÄND'}</span> (gräns: ${Math.round(PASS_THRESHOLD*100)}%)`;

      elStats.innerHTML = "";
      Object.keys(possibleByCat).forEach(cat=>{
        const got = byCat[cat] || 0;
        const tot = possibleByCat[cat];
        const pct = Math.round((got/tot)*100);
        const div = document.createElement("div");
        div.className = "stat";
        div.innerHTML = `<div><div class="category" style="color:var(--text)">${cat}</div>
          <div class="muted">${got} av ${tot} (${pct}%)</div></div>
          <div style="min-width:120px"><div class="progress">
            <div class="bar" style="width:${pct}%; background:linear-gradient(90deg,#3b82f6,#22c55e)"></div></div></div>`;
        elStats.appendChild(div);
      });

      elReview.innerHTML = "<h3>Genomgång</h3>";
      QUIZ.forEach((q, i)=>{
        const userVis = answers[q.id];
        const isCorrect = (userVis != null) && ((q._map ? q._map[userVis] : userVis) === q.correct);
        const userTxt = (userVis != null) ? (q._opts ? q._opts[userVis] : q.options[userVis]) : "(ej besvarad)";
        const correctTxt = q.options[q.correct];
        const div = document.createElement("div");
        div.className = "rev-item";
        div.innerHTML = `
          <div class="category">${q.cat}</div>
          <div style="font-weight:700; margin-top:4px">Fråga ${i+1}: ${q.text}</div>
          <div style="margin-top:6px">
            Ditt svar: <span class="${isCorrect?'correct':'wrong'}">${userTxt}</span><br/>
            Rätt svar: <span class="correct">${correctTxt}</span>
          </div>
          <div class="muted" style="margin-top:6px">${q.rationale || ""}</div>
        `;
        elReview.appendChild(div);
      });

      // Spara enkel historik lokalt
      try{
        const key = "roma_industri_results";
        const prev = JSON.parse(localStorage.getItem(key) || "[]");
        prev.push({ test: TEST_NAME, participant, time: new Date().toISOString(), total, totalQuestions: QUIZ.length, byCategory: byCat });
        localStorage.setItem(key, JSON.stringify(prev));
      }catch(e){}
    }

    function exportCSV(){
      const participant = (elParticipant.value || "Okänd").replaceAll(","," ");
      const header = ["Test","Deltagare","Tid","Total","Max","Kategori","Rätt","MaxKategori"];
      const rows = [];
      const now = new Date().toISOString();

      let total = 0;
      const byCat = {};
      const possibleByCat = {};
      QUIZ.forEach(q=>{
        if(!(q.cat in byCat)){ byCat[q.cat]=0; possibleByCat[q.cat]=0; }
        possibleByCat[q.cat]+=1;
        if(answers[q.id] != null){
          const chosenOriginal = q._map ? q._map[answers[q.id]] : answers[q.id];
          if(chosenOriginal === q.correct){ total+=1; byCat[q.cat]+=1; }
        }
      });

      Object.keys(possibleByCat).forEach(cat=>{
        rows.push([TEST_NAME, participant, now, total, QUIZ.length, cat, byCat[cat]||0, possibleByCat[cat]]);
      });

      const csv = [header.join(",")].concat(rows.map(r=>r.join(","))).join("\n");
      const blob = new Blob([csv], {type:"text/csv;charset=utf-8"});
      const a = document.createElement("a");
      a.href = URL.createObjectURL(blob);
      a.download = `resultat_${participant || "deltagare"}_${Date.now()}.csv`;
      a.click();
      URL.revokeObjectURL(a.href);
    }

    // ===== HÄNDELSER =====
    document.getElementById("startBtn").addEventListener("click", ()=>{
      answers = {}; idx = 0;
      document.getElementById("start").classList.add("hide");
      document.getElementById("result").classList.add("hide");
      document.getElementById("quiz").classList.remove("hide");

      // Timer
      clearInterval(timerId);
      const minutes = parseInt(elDuration.value,10);
      if(minutes>0){ remainingSec = minutes*60; }
      startTimer();

      // Ny blandning inför nytt försök
      QUIZ = buildBalancedQuiz(QUESTIONS, QUESTIONS_PER_RUN);
      render();

      // Fokus på första alternativet
      setTimeout(()=> {
        const first = document.querySelector("#options input[type=radio]");
        if(first) first.focus();
      }, 0);
    });

    document.getElementById("nextBtn").addEventListener("click", ()=> next());
    document.getElementById("prevBtn").addEventListener("click", ()=> prev());
    document.getElementById("saveBtn").addEventListener("click", ()=> next());
    document.getElementById("submitBtn").addEventListener("click", ()=> {
      if(confirm("Vill du avsluta och visa resultat?")) submitQuiz();
    });

    document.getElementById("exportBtn").addEventListener("click", exportCSV);
    document.getElementById("retryBtn").addEventListener("click", ()=> window.location.reload());

    // Piltangenter för nästa/föregående
    document.addEventListener("keydown", (e)=>{
      if(document.getElementById("quiz").classList.contains("hide")) return;
      if(e.key === "ArrowRight") next();
      if(e.key === "ArrowLeft") prev();
    });
  </script>
</body>
</html>
