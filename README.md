<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Bloating & Stress Diary</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #f7f5f2;
    --surface: #ffffff;
    --border: rgba(0,0,0,0.1);
    --border-mid: rgba(0,0,0,0.18);
    --text: #1a1a1a;
    --text-muted: #6b6b6b;
    --text-faint: #aaa;
    --radius: 12px;
    --radius-sm: 8px;
    --teal: #1D9E75;
    --teal-light: #E1F5EE;
    --teal-dark: #0F6E56;
    --amber: #EF9F27;
    --amber-light: #FAEEDA;
    --amber-dark: #854F0B;
    --red: #E24B4A;
    --red-light: #FCEBEB;
    --red-dark: #A32D2D;
    --green: #639922;
    --coral: #D85A30;
  }

  body {
    font-family: 'Georgia', serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 2rem 1rem 4rem;
  }

  .page {
    max-width: 680px;
    margin: 0 auto;
  }

  header {
    margin-bottom: 2rem;
    border-bottom: 1.5px solid var(--text);
    padding-bottom: 1.25rem;
  }

  header h1 {
    font-size: 28px;
    font-weight: normal;
    letter-spacing: -0.5px;
    color: var(--text);
    margin-bottom: 4px;
  }

  header p {
    font-size: 13px;
    color: var(--text-muted);
    font-family: 'Courier New', monospace;
    letter-spacing: 0.03em;
  }

  .summary-bar {
    display: flex;
    gap: 0;
    margin-bottom: 1.75rem;
    border: 1px solid var(--border-mid);
    border-radius: var(--radius-sm);
    overflow: hidden;
    background: var(--surface);
  }

  .sum-item {
    flex: 1;
    padding: 10px 14px;
    border-right: 1px solid var(--border);
    font-family: 'Courier New', monospace;
  }

  .sum-item:last-child { border-right: none; }

  .sum-item .sum-lbl {
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-faint);
    margin-bottom: 2px;
  }

  .sum-item .sum-val {
    font-size: 18px;
    font-weight: bold;
    color: var(--text);
  }

  .toolbar {
    display: flex;
    gap: 8px;
    margin-bottom: 1.25rem;
    flex-wrap: wrap;
  }

  .toolbar-btn {
    font-size: 12px;
    font-family: 'Courier New', monospace;
    letter-spacing: 0.05em;
    padding: 7px 14px;
    border-radius: var(--radius-sm);
    border: 1px solid var(--border-mid);
    background: var(--surface);
    color: var(--text-muted);
    cursor: pointer;
    transition: all 0.15s;
  }

  .toolbar-btn:hover { background: var(--bg); color: var(--text); }
  .toolbar-btn.save-btn { border-color: var(--teal); color: var(--teal-dark); background: var(--teal-light); }
  .toolbar-btn.save-btn:hover { background: #c8ede0; }

  .save-notice {
    font-size: 11px;
    font-family: 'Courier New', monospace;
    color: var(--text-faint);
    align-self: center;
    margin-left: auto;
    opacity: 0;
    transition: opacity 0.3s;
  }

  .save-notice.show { opacity: 1; }

  .day-card {
    background: var(--surface);
    border: 1px solid var(--border-mid);
    border-radius: var(--radius);
    margin-bottom: 16px;
    overflow: hidden;
  }

  .day-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 18px;
    border-bottom: 1px solid var(--border);
    background: var(--bg);
  }

  .day-header input {
    font-size: 15px;
    font-family: 'Georgia', serif;
    font-weight: bold;
    border: none;
    background: transparent;
    color: var(--text);
    outline: none;
    width: 240px;
  }

  .remove-btn {
    font-size: 11px;
    font-family: 'Courier New', monospace;
    color: var(--text-faint);
    border: none;
    background: none;
    cursor: pointer;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  .remove-btn:hover { color: var(--red); }

  .meal-row {
    padding: 12px 18px;
    border-bottom: 1px solid var(--border);
  }

  .meal-row:last-of-type { border-bottom: none; }

  .meal-label {
    font-size: 10px;
    font-family: 'Courier New', monospace;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-faint);
    margin-bottom: 6px;
  }

  .meal-food-input {
    width: 100%;
    font-size: 14px;
    font-family: 'Georgia', serif;
    color: var(--text);
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 6px 10px;
    margin-bottom: 8px;
    outline: none;
    transition: border-color 0.15s;
  }

  .meal-food-input:focus { border-color: var(--border-mid); }

  .bloat-row {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .bloat-lbl {
    font-size: 12px;
    color: var(--text-muted);
    font-family: 'Courier New', monospace;
    min-width: 58px;
  }

  .toggle-group { display: flex; gap: 4px; }

  .toggle-btn {
    font-size: 12px;
    font-family: 'Courier New', monospace;
    padding: 4px 13px;
    border-radius: 20px;
    border: 1px solid var(--border-mid);
    background: transparent;
    color: var(--text-muted);
    cursor: pointer;
    transition: all 0.15s;
  }

  .toggle-btn:hover { background: var(--bg); }
  .toggle-btn.active-no { background: var(--teal-light); color: var(--teal-dark); border-color: var(--teal); }
  .toggle-btn.active-mild { background: var(--amber-light); color: var(--amber-dark); border-color: var(--amber); }
  .toggle-btn.active-yes { background: var(--red-light); color: var(--red-dark); border-color: var(--red); }

  .stress-section {
    padding: 14px 18px 16px;
    border-top: 1.5px solid var(--border);
    background: var(--bg);
  }

  .stress-title {
    font-size: 10px;
    font-family: 'Courier New', monospace;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-faint);
    margin-bottom: 10px;
  }

  .stress-row { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }

  .stress-track { display: flex; gap: 5px; }

  .stress-dot {
    width: 28px;
    height: 28px;
    border-radius: 50%;
    border: 1.5px solid var(--border-mid);
    background: transparent;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 11px;
    font-family: 'Courier New', monospace;
    font-weight: bold;
    color: var(--text-faint);
    transition: all 0.15s;
  }

  .stress-dot:hover { border-color: var(--text-muted); color: var(--text-muted); }
  .stress-dot.s1.active { background: #1D9E75; border-color: #1D9E75; color: white; }
  .stress-dot.s2.active { background: #639922; border-color: #639922; color: white; }
  .stress-dot.s3.active { background: #EF9F27; border-color: #EF9F27; color: white; }
  .stress-dot.s4.active { background: #D85A30; border-color: #D85A30; color: white; }
  .stress-dot.s5.active { background: #E24B4A; border-color: #E24B4A; color: white; }

  .stress-val {
    font-size: 12px;
    font-family: 'Courier New', monospace;
    color: var(--text-muted);
    min-width: 70px;
  }

  .notes-lbl {
    font-size: 10px;
    font-family: 'Courier New', monospace;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-faint);
    margin-bottom: 6px;
  }

  .notes-input {
    width: 100%;
    font-size: 13px;
    font-family: 'Georgia', serif;
    color: var(--text);
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 8px 10px;
    resize: none;
    height: 60px;
    outline: none;
    transition: border-color 0.15s;
  }

  .notes-input:focus { border-color: var(--border-mid); }

  .add-day-btn {
    width: 100%;
    padding: 12px;
    border-radius: var(--radius-sm);
    border: 1.5px dashed var(--border-mid);
    background: transparent;
    color: var(--text-muted);
    font-size: 14px;
    font-family: 'Courier New', monospace;
    letter-spacing: 0.05em;
    cursor: pointer;
    transition: all 0.15s;
    margin-top: 4px;
  }

  .add-day-btn:hover { background: var(--surface); border-color: var(--text-muted); color: var(--text); }

  @media print {
    .toolbar, .add-day-btn, .remove-btn { display: none !important; }
    body { padding: 0; background: white; }
    .day-card { break-inside: avoid; }
  }
</style>
</head>
<body>
<div class="page">

  <header>
    <h1>Bloating &amp; Stress Diary</h1>
    <p>Track meals, bloating, and daily stress levels</p>
  </header>

  <div class="summary-bar">
    <div class="sum-item"><div class="sum-lbl">Days logged</div><div class="sum-val" id="sum-days">0</div></div>
    <div class="sum-item"><div class="sum-lbl">Bloating events</div><div class="sum-val" id="sum-bloat">0</div></div>
    <div class="sum-item"><div class="sum-lbl">Avg stress</div><div class="sum-val" id="sum-stress">—</div></div>
  </div>

  <div class="toolbar">
    <button class="toolbar-btn save-btn" onclick="saveData()">💾 Save diary</button>
    <button class="toolbar-btn" onclick="exportCSV()">↓ Export CSV</button>
    <button class="toolbar-btn" onclick="window.print()">⎙ Print</button>
    <span class="save-notice" id="save-notice">Saved ✓</span>
  </div>

  <div id="days-container"></div>
  <button class="add-day-btn" onclick="addDay()">+ Add day</button>

</div>

<script>
const MEALS = ['Breakfast','Lunch','Dinner','Snack 1','Snack 2'];
const STRESS_LABELS = ['','Very low','Low','Moderate','High','Very high'];
const STORAGE_KEY = 'bloating-diary-v1';
let dayCount = 0;

function getToday() {
  return new Date().toLocaleDateString('en-NZ', {weekday:'long', day:'numeric', month:'short', year:'numeric'});
}

// ── Build a day card from optional saved data ──────────────────────────
function addDay(saved) {
  dayCount++;
  const id = 'day-' + dayCount;
  const dateVal = saved ? saved.date : (dayCount === 1 ? getToday() : '');

  const mealsHTML = MEALS.map((m, mi) => {
    const food = saved ? (saved.meals[mi]?.food || '') : '';
    return `
    <div class="meal-row">
      <div class="meal-label">${m}</div>
      <input class="meal-food-input" type="text" placeholder="What did you eat?" value="${escAttr(food)}" oninput="autoSave()" />
      <div class="bloat-row">
        <span class="bloat-lbl">Bloating:</span>
        <div class="toggle-group">
          <button class="toggle-btn" onclick="setBloat(this,'no')">No</button>
          <button class="toggle-btn" onclick="setBloat(this,'mild')">Mild</button>
          <button class="toggle-btn" onclick="setBloat(this,'yes')">Yes</button>
        </div>
      </div>
    </div>`;
  }).join('');

  const dotsHTML = [1,2,3,4,5].map(n =>
    `<div class="stress-dot s${n}" onclick="setStress('${id}',${n})" title="${STRESS_LABELS[n]}">${n}</div>`
  ).join('');

  const notes = saved ? (saved.notes || '') : '';

  const card = document.createElement('div');
  card.className = 'day-card';
  card.id = id;
  card.innerHTML = `
    <div class="day-header">
      <input type="text" value="${escAttr(dateVal)}" placeholder="Date (e.g. Monday 7 Apr 2026)" oninput="autoSave()" />
      <button class="remove-btn" onclick="removeDay('${id}')">Remove</button>
    </div>
    ${mealsHTML}
    <div class="stress-section">
      <div class="stress-title">Stress level today</div>
      <div class="stress-row">
        <div class="stress-track">${dotsHTML}</div>
        <div class="stress-val" id="stress-lbl-${id}">Not rated</div>
      </div>
      <div class="notes-lbl">Notes / symptoms / triggers</div>
      <textarea class="notes-input" placeholder="Any other symptoms, triggers, or observations..." oninput="autoSave()">${escText(notes)}</textarea>
    </div>`;

  document.getElementById('days-container').appendChild(card);

  // Restore bloat toggles
  if (saved) {
    card.querySelectorAll('.meal-row').forEach((row, mi) => {
      const bloatVal = saved.meals[mi]?.bloat;
      if (bloatVal) {
        const btn = [...row.querySelectorAll('.toggle-btn')].find(b => b.textContent.toLowerCase() === bloatVal);
        if (btn) setBloat(btn, bloatVal, true);
      }
    });
    if (saved.stress) setStress(id, saved.stress, true);
  }

  updateSummary();
}

function setBloat(btn, val, silent) {
  btn.parentElement.querySelectorAll('.toggle-btn').forEach(b =>
    b.classList.remove('active-yes','active-no','active-mild')
  );
  btn.classList.add('active-' + val);
  updateSummary();
  if (!silent) autoSave();
}

function setStress(dayId, val, silent) {
  const card = document.getElementById(dayId);
  card.querySelectorAll('.stress-dot').forEach((d, i) => {
    d.classList.toggle('active', i < val);
  });
  document.getElementById('stress-lbl-' + dayId).textContent = STRESS_LABELS[val];
  updateSummary();
  if (!silent) autoSave();
}

function removeDay(id) {
  const el = document.getElementById(id);
  if (el) el.remove();
  updateSummary();
  autoSave();
}

function updateSummary() {
  const cards = document.querySelectorAll('.day-card');
  document.getElementById('sum-days').textContent = cards.length;

  let bloatCount = 0;
  document.querySelectorAll('.toggle-btn.active-yes, .toggle-btn.active-mild').forEach(() => bloatCount++);
  document.getElementById('sum-bloat').textContent = bloatCount;

  let total = 0, count = 0;
  cards.forEach(card => {
    const lbl = card.querySelector('[id^="stress-lbl-"]');
    if (lbl && lbl.textContent !== 'Not rated') {
      const idx = STRESS_LABELS.indexOf(lbl.textContent);
      if (idx > 0) { total += idx; count++; }
    }
  });
  document.getElementById('sum-stress').textContent = count ? (total/count).toFixed(1) + '/5' : '—';
}

// ── Serialise all cards to plain objects ──────────────────────────────
function collectData() {
  const days = [];
  document.querySelectorAll('.day-card').forEach(card => {
    const date = card.querySelector('.day-header input').value;
    const notes = card.querySelector('.notes-input').value;
    const stressLbl = card.querySelector('[id^="stress-lbl-"]').textContent;
    const stressVal = STRESS_LABELS.indexOf(stressLbl);
    const meals = [];
    card.querySelectorAll('.meal-row').forEach((row, i) => {
      const food = row.querySelector('.meal-food-input').value;
      const activeBtn = row.querySelector('.toggle-btn[class*="active-"]');
      const bloat = activeBtn ? activeBtn.textContent.toLowerCase() : null;
      meals.push({ meal: MEALS[i], food, bloat });
    });
    days.push({ date, meals, stress: stressVal > 0 ? stressVal : null, notes });
  });
  return days;
}

// ── localStorage persistence ──────────────────────────────────────────
let autoSaveTimer;
function autoSave() {
  clearTimeout(autoSaveTimer);
  autoSaveTimer = setTimeout(saveData, 1200);
}

function saveData() {
  try {
    const data = collectData();
    localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
    const notice = document.getElementById('save-notice');
    notice.classList.add('show');
    setTimeout(() => notice.classList.remove('show'), 2000);
  } catch(e) {
    alert('Could not save — your browser may have storage disabled.');
  }
}

function loadData() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return null;
    return JSON.parse(raw);
  } catch(e) { return null; }
}

// ── CSV export ────────────────────────────────────────────────────────
function exportCSV() {
  const data = collectData();
  const rows = [['Date','Meal','Food','Bloating','Stress','Notes']];
  data.forEach(day => {
    day.meals.forEach((m, i) => {
      rows.push([
        i === 0 ? day.date : '',
        m.meal,
        m.food,
        m.bloat || '',
        i === 0 && day.stress ? STRESS_LABELS[day.stress] : '',
        i === 0 ? day.notes : ''
      ]);
    });
  });
  const csv = rows.map(r => r.map(cell => `"${String(cell).replace(/"/g,'""')}"`).join(',')).join('\n');
  const blob = new Blob([csv], {type:'text/csv'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'bloating-diary.csv';
  a.click();
}

// ── Helpers ───────────────────────────────────────────────────────────
function escAttr(s) { return String(s).replace(/"/g,'&quot;').replace(/'/g,'&#39;'); }
function escText(s) { return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;'); }

// ── Init ──────────────────────────────────────────────────────────────
(function init() {
  const saved = loadData();
  if (saved && saved.length > 0) {
    saved.forEach(day => addDay(day));
  } else {
    addDay();
  }
})();
</script>
</body>
</html>
