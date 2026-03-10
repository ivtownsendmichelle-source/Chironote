# Chironote
Companion app built for Owen at Cornerstone
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ChiroNote — Cornerstone Health</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Source+Sans+3:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --navy: #1B2A4A;
    --navy-light: #243559;
    --navy-dark: #111c33;
    --blue: #2B8FD6;
    --blue-light: #4aa5e8;
    --blue-pale: #e8f4fc;
    --cream: #F5ECD7;
    --cream-dark: #ede0c4;
    --cream-light: #faf6ee;
    --red: #C0392B;
    --red-light: #e74c3c;
    --green: #27ae60;
    --text: #1B2A4A;
    --text-light: #5a6a85;
    --white: #ffffff;
    --shadow: 0 4px 24px rgba(27,42,74,0.10);
    --shadow-lg: 0 8px 40px rgba(27,42,74,0.16);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Source Sans 3', sans-serif; background: var(--cream-light); color: var(--text); min-height: 100vh; }
  .header { background: var(--navy); padding: 18px 32px; display: flex; align-items: center; gap: 16px; box-shadow: var(--shadow-lg); }
  .logo-ribbon { width: 44px; height: 44px; position: relative; flex-shrink: 0; }
  .logo-ribbon svg { width: 100%; height: 100%; }
  .header-text h1 { font-family: 'Playfair Display', serif; font-size: 1.5rem; color: var(--white); font-weight: 700; letter-spacing: 0.02em; }
  .header-text p { font-size: 0.78rem; color: #8ba4c8; letter-spacing: 0.12em; text-transform: uppercase; font-weight: 500; }
  .header-right { margin-left: auto; display: flex; align-items: center; gap: 10px; }
  .date-display { color: #8ba4c8; font-size: 0.82rem; font-weight: 400; }
  .main { display: grid; grid-template-columns: 280px 1fr 260px; gap: 0; min-height: calc(100vh - 80px); }
  .left-panel { background: var(--navy-light); padding: 24px 20px; display: flex; flex-direction: column; gap: 20px; }
  .panel-label { font-size: 0.7rem; font-weight: 600; letter-spacing: 0.14em; text-transform: uppercase; color: #6a84a8; margin-bottom: 8px; }
  .patient-search { position: relative; }
  .patient-search input { width: 100%; background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.12); border-radius: 8px; padding: 10px 14px; color: var(--white); font-family: 'Source Sans 3', sans-serif; font-size: 0.88rem; outline: none; transition: border 0.2s; }
  .patient-search input::placeholder { color: #6a84a8; }
  .patient-search input:focus { border-color: var(--blue); }
  .autocomplete-list { position: absolute; top: 100%; left: 0; right: 0; background: var(--navy-dark); border: 1px solid rgba(255,255,255,0.12); border-radius: 8px; margin-top: 4px; overflow: hidden; z-index: 100; display: none; }
  .autocomplete-list.show { display: block; }
  .autocomplete-item { padding: 10px 14px; color: var(--white); font-size: 0.88rem; cursor: pointer; transition: background 0.15s; }
  .autocomplete-item:hover { background: rgba(43,143,214,0.2); }
  .add-patient-btn { width: 100%; background: transparent; border: 1px dashed rgba(43,143,214,0.4); border-radius: 8px; padding: 9px; color: var(--blue-light); font-size: 0.82rem; cursor: pointer; font-family: 'Source Sans 3', sans-serif; transition: all 0.2s; }
  .add-patient-btn:hover { background: rgba(43,143,214,0.1); border-color: var(--blue); }
  .template-selector { display: flex; flex-direction: column; gap: 6px; }
  .template-btn { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.08); border-radius: 8px; padding: 10px 14px; color: #c8d8ee; font-size: 0.85rem; cursor: pointer; font-family: 'Source Sans 3', sans-serif; text-align: left; transition: all 0.2s; }
  .template-btn:hover { background: rgba(43,143,214,0.15); border-color: rgba(43,143,214,0.3); }
  .template-btn.active { background: rgba(43,143,214,0.25); border-color: var(--blue); color: var(--white); font-weight: 600; }
  .history-list { flex: 1; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; }
  .history-item { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.07); border-radius: 8px; padding: 10px 12px; cursor: pointer; transition: all 0.2s; }
  .history-item:hover { background: rgba(43,143,214,0.12); border-color: rgba(43,143,214,0.2); }
  .history-date { font-size: 0.75rem; color: #6a84a8; }
  .history-type { font-size: 0.82rem; color: #c8d8ee; font-weight: 500; margin-top: 2px; }
  .no-history { color: #4a5e7a; font-size: 0.82rem; text-align: center; padding: 20px 0; }
  .center-panel { background: var(--cream-light); padding: 28px 32px; display: flex; flex-direction: column; gap: 20px; }
  .patient-banner { background: var(--white); border-radius: 12px; padding: 16px 20px; box-shadow: var(--shadow); display: flex; align-items: center; gap: 16px; border-left: 4px solid var(--blue); min-height: 60px; }
  .patient-name { font-family: 'Playfair Display', serif; font-size: 1.2rem; color: var(--navy); font-weight: 600; }
  .patient-sub { font-size: 0.8rem; color: var(--text-light); margin-top: 2px; }
  .no-patient { color: var(--text-light); font-size: 0.9rem; font-style: italic; }
  .record-section { display: flex; flex-direction: column; align-items: center; gap: 14px; }
  .mic-btn { width: 88px; height: 88px; border-radius: 50%; border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.25s; position: relative; background: var(--navy); box-shadow: var(--shadow-lg); }
  .mic-btn:hover { transform: scale(1.05); }
  .mic-btn.recording { background: var(--red); animation: pulse 1.4s infinite; }
  .mic-btn svg { width: 36px; height: 36px; fill: white; }
  @keyframes pulse { 0%,100% { box-shadow: 0 0 0 0 rgba(192,57,43,0.4); } 50% { box-shadow: 0 0 0 18px rgba(192,57,43,0); } }
  .record-status { font-size: 0.82rem; color: var(--text-light); letter-spacing: 0.06em; }
  .record-status.active { color: var(--red); font-weight: 600; }
  .transcript-box { background: var(--white); border-radius: 12px; padding: 18px; border: 1px solid var(--cream-dark); min-height: 90px; font-size: 0.9rem; color: var(--text); line-height: 1.6; box-shadow: var(--shadow); }
  .transcript-box .placeholder { color: #aab4c4; font-style: italic; }
  .note-output { background: var(--white); border-radius: 14px; padding: 24px; box-shadow: var(--shadow-lg); border: 1px solid var(--cream-dark); flex: 1; }
  .note-output h3 { font-family: 'Playfair Display', serif; font-size: 1.05rem; color: var(--navy); margin-bottom: 16px; padding-bottom: 10px; border-bottom: 2px solid var(--cream-dark); }
  .soap-section { margin-bottom: 18px; }
  .soap-label { font-size: 0.72rem; font-weight: 700; letter-spacing: 0.14em; text-transform: uppercase; color: var(--blue); margin-bottom: 6px; }
  .soap-content { font-size: 0.88rem; line-height: 1.65; color: var(--text); background: var(--cream-light); border-radius: 8px; padding: 10px 14px; border-left: 3px solid var(--blue-pale); min-height: 40px; outline: none; white-space: pre-wrap; }
  .soap-content:focus { border-left-color: var(--blue); background: #fff; }
  .soap-content[contenteditable]:empty:before { content: attr(data-placeholder); color: #aab4c4; font-style: italic; }
  .action-bar { display: flex; gap: 10px; flex-wrap: wrap; }
  .btn { padding: 9px 20px; border-radius: 8px; border: none; cursor: pointer; font-family: 'Source Sans 3', sans-serif; font-size: 0.85rem; font-weight: 600; transition: all 0.2s; }
  .btn-primary { background: var(--blue); color: white; }
  .btn-primary:hover { background: var(--blue-light); }
  .btn-secondary { background: var(--cream-dark); color: var(--navy); }
  .btn-secondary:hover { background: #ddd0b0; }
  .btn-danger { background: transparent; border: 1px solid rgba(192,57,43,0.3); color: var(--red); }
  .btn-danger:hover { background: rgba(192,57,43,0.05); }
  .btn-success { background: var(--green); color: white; }
  .btn-success:hover { background: #219a52; }
  .right-panel { background: var(--white); padding: 24px 18px; border-left: 1px solid var(--cream-dark); display: flex; flex-direction: column; gap: 18px; }
  .spine-container { display: flex; flex-direction: column; align-items: center; gap: 4px; }
  .spine-region { width: 100%; display: flex; flex-direction: column; gap: 3px; margin-bottom: 6px; }
  .region-label { font-size: 0.68rem; font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; color: var(--text-light); text-align: center; margin-bottom: 4px; }
  .vertebra-row { display: flex; justify-content: center; gap: 4px; }
  .vertebra { background: #e8eef5; border: 1.5px solid #c8d8ee; border-radius: 6px; cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 0.68rem; font-weight: 600; color: var(--navy); transition: all 0.18s; user-select: none; }
  .vertebra:hover { background: var(--blue-pale); border-color: var(--blue); }
  .vertebra.selected { background: var(--red); border-color: var(--red-light); color: white; box-shadow: 0 2px 8px rgba(192,57,43,0.3); }
  .vertebra.cervical { width: 38px; height: 26px; }
  .vertebra.thoracic { width: 44px; height: 22px; }
  .vertebra.lumbar { width: 52px; height: 24px; }
  .vertebra.sacral { width: 60px; height: 28px; border-radius: 10px; }
  .selected-segments { background: var(--cream-light); border-radius: 8px; padding: 10px 12px; min-height: 40px; }
  .selected-segments p { font-size: 0.78rem; color: var(--text-light); }
  .segment-tag { display: inline-block; background: var(--red); color: white; border-radius: 12px; padding: 2px 8px; font-size: 0.72rem; font-weight: 600; margin: 2px; cursor: pointer; }
  .clear-spine-btn { background: transparent; border: 1px solid rgba(192,57,43,0.25); border-radius: 6px; padding: 6px 12px; color: var(--red); font-size: 0.78rem; cursor: pointer; font-family: 'Source Sans 3', sans-serif; transition: all 0.2s; width: 100%; }
  .clear-spine-btn:hover { background: rgba(192,57,43,0.05); }
  .modal-overlay { position: fixed; inset: 0; background: rgba(17,28,51,0.7); display: none; align-items: center; justify-content: center; z-index: 1000; }
  .modal-overlay.show { display: flex; }
  .modal { background: var(--white); border-radius: 16px; padding: 32px; width: 400px; box-shadow: var(--shadow-lg); }
  .modal h3 { font-family: 'Playfair Display', serif; color: var(--navy); margin-bottom: 20px; font-size: 1.2rem; }
  .modal input { width: 100%; border: 1.5px solid var(--cream-dark); border-radius: 8px; padding: 10px 14px; font-family: 'Source Sans 3', sans-serif; font-size: 0.9rem; color: var(--text); outline: none; transition: border 0.2s; margin-bottom: 8px; }
  .modal input:focus { border-color: var(--blue); }
  .modal-btns { display: flex; gap: 10px; margin-top: 12px; }
  .toast { position: fixed; bottom: 28px; right: 28px; background: var(--navy); color: white; padding: 12px 20px; border-radius: 10px; font-size: 0.85rem; box-shadow: var(--shadow-lg); opacity: 0; transform: translateY(10px); transition: all 0.3s; pointer-events: none; z-index: 2000; }
  .toast.show { opacity: 1; transform: translateY(0); }
  .generating { display: flex; align-items: center; gap: 10px; padding: 20px; color: var(--text-light); font-style: italic; }
  .spinner { width: 20px; height: 20px; border: 2px solid var(--cream-dark); border-top-color: var(--blue); border-radius: 50%; animation: spin 0.8s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }
  ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-track { background: transparent; } ::-webkit-scrollbar-thumb { background: var(--cream-dark); border-radius: 4px; }
</style>
</head>
<body>

<header class="header">
  <div class="logo-ribbon">
    <svg viewBox="0 0 44 44" fill="none">
      <circle cx="22" cy="22" r="20" fill="#243559" stroke="#2B8FD6" stroke-width="1.5"/>
      <path d="M22 8 C18 12 14 16 14 20 C14 24 18 26 22 26 C26 26 30 24 30 20 C30 16 26 12 22 8Z" fill="#2B8FD6" opacity="0.6"/>
      <path d="M16 20 Q22 30 28 20" stroke="#F5ECD7" stroke-width="1.5" fill="none" stroke-linecap="round"/>
      <circle cx="22" cy="20" r="3" fill="#F5ECD7"/>
      <line x1="22" y1="23" x2="22" y2="36" stroke="#F5ECD7" stroke-width="2" stroke-linecap="round"/>
      <line x1="16" y1="26" x2="28" y2="26" stroke="#F5ECD7" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </div>
  <div class="header-text">
    <h1>ChiroNote</h1>
    <p>Cornerstone Health — Clinical Documentation</p>
  </div>
  <div class="header-right">
    <span class="date-display" id="dateDisplay"></span>
  </div>
</header>

<div class="main">
  <div class="left-panel">
    <div>
      <div class="panel-label">Patient</div>
      <div class="patient-search">
        <input type="text" id="patientInput" placeholder="Search patient name..." autocomplete="off">
        <div class="autocomplete-list" id="autocompleteList"></div>
      </div>
      <div style="margin-top:8px">
        <button class="add-patient-btn" onclick="openAddPatient()">+ Add New Patient</button>
      </div>
    </div>
    <div>
      <div class="panel-label">Note Type</div>
      <div class="template-selector">
        <button class="template-btn active" onclick="selectTemplate(this,'soap_new')" data-template="soap_new">New Patient SOAP</button>
        <button class="template-btn" onclick="selectTemplate(this,'soap_followup')" data-template="soap_followup">Follow Up Note</button>
        <button class="template-btn" onclick="selectTemplate(this,'referral')" data-template="referral">Referral Letter</button>
      </div>
    </div>
    <div style="flex:1;overflow:hidden;display:flex;flex-direction:column;">
      <div class="panel-label">Visit History</div>
      <div class="history-list" id="historyList">
        <div class="no-history">Select a patient to view history</div>
      </div>
    </div>
  </div>

  <div class="center-panel">
    <div class="patient-banner" id="patientBanner">
      <div><div class="no-patient">No patient selected — search or add a patient to begin</div></div>
    </div>
    <div class="record-section">
      <button class="mic-btn" id="micBtn" onclick="toggleRecording()">
        <svg viewBox="0 0 24 24"><path d="M12 1a4 4 0 0 1 4 4v6a4 4 0 0 1-8 0V5a4 4 0 0 1 4-4zm-7 10a7 7 0 0 0 14 0h-2a5 5 0 0 1-10 0H5zm7 9v-3h-2v3H8v2h8v-2h-2z"/></svg>
      </button>
      <div class="record-status" id="recordStatus">Click microphone to begin dictation</div>
    </div>
    <div>
      <div class="panel-label" style="color:#5a6a85;margin-bottom:6px;">Live Transcription</div>
      <div class="transcript-box" id="transcriptBox">
        <span class="placeholder">Your spoken words will appear here as you dictate...</span>
      </div>
    </div>
    <div class="note-output" id="noteOutput">
      <h3 id="noteTitle">Clinical Note</h3>
      <div id="noteContent">
        <div style="color:#aab4c4;font-style:italic;font-size:0.88rem;padding:10px 0;">Dictate your clinical findings above, then the formatted note will appear here.</div>
      </div>
    </div>
    <div class="action-bar">
      <button class="btn btn-primary" onclick="generateNote()">Generate Note</button>
      <button class="btn btn-success" onclick="saveNote()">Save Note</button>
      <button class="btn btn-secondary" onclick="copyNote()">Copy to Clipboard</button>
      <button class="btn btn-danger" onclick="clearAll()">Clear</button>
    </div>
  </div>

  <div class="right-panel">
    <div>
      <div class="panel-label">Spinal Segments</div>
      <div style="font-size:0.75rem;color:#aab4c4;margin-bottom:10px;">Click to mark areas of concern</div>
    </div>
    <div class="spine-container" id="spineContainer"></div>
    <div>
      <div class="panel-label" style="margin-bottom:6px;">Marked Segments</div>
      <div class="selected-segments" id="selectedSegments"><p>None selected</p></div>
      <button class="clear-spine-btn" onclick="clearSpine()" style="margin-top:8px;">Clear All Segments</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="addPatientModal">
  <div class="modal">
    <h3>Add New Patient</h3>
    <input type="text" id="newPatientFirst" placeholder="First name">
    <input type="text" id="newPatientLast" placeholder="Last name">
    <input type="text" id="newPatientDOB" placeholder="Date of birth (MM/DD/YYYY)">
    <div class="modal-btns">
      <button class="btn btn-primary" onclick="saveNewPatient()">Add Patient</button>
      <button class="btn btn-secondary" onclick="closeModal()">Cancel</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="viewNoteModal">
  <div class="modal" style="width:560px;max-height:80vh;overflow-y:auto;">
    <h3 id="viewNoteTitle">Visit Note</h3>
    <div id="viewNoteContent" style="font-size:0.88rem;line-height:1.7;color:#333;margin-bottom:20px;"></div>
    <div class="modal-btns">
      <button class="btn btn-secondary" onclick="document.getElementById('viewNoteModal').classList.remove('show')">Close</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
let currentPatient = null;
let currentTemplate = 'soap_new';
let isRecording = false;
let recognition = null;
let fullTranscript = '';
let selectedSegments = [];
let patients = JSON.parse(localStorage.getItem('chironote_patients') || '[]');
let notes = JSON.parse(localStorage.getItem('chironote_notes') || '[]');

document.getElementById('dateDisplay').textContent = new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

const spineData = [
  { region: 'Cervical', segments: ['C1','C2','C3','C4','C5','C6','C7'], type: 'cervical', rows: [[0,1,2,3],[4,5,6]] },
  { region: 'Thoracic', segments: ['T1','T2','T3','T4','T5','T6','T7','T8','T9','T10','T11','T12'], type: 'thoracic', rows: [[0,1,2,3],[4,5,6,7],[8,9,10,11]] },
  { region: 'Lumbar', segments: ['L1','L2','L3','L4','L5'], type: 'lumbar', rows: [[0,1,2],[3,4]] },
  { region: 'Sacral / Coccyx', segments: ['S1','Coccyx'], type: 'sacral', rows: [[0,1]] }
];

function buildSpine() {
  const container = document.getElementById('spineContainer');
  spineData.forEach(region => {
    const regionDiv = document.createElement('div');
    regionDiv.className = 'spine-region';
    const label = document.createElement('div');
    label.className = 'region-label';
    label.textContent = region.region;
    regionDiv.appendChild(label);
    region.rows.forEach(row => {
      const rowDiv = document.createElement('div');
      rowDiv.className = 'vertebra-row';
      row.forEach(i => {
        const seg = region.segments[i];
        const v = document.createElement('div');
        v.className = `vertebra ${region.type}`;
        v.textContent = seg;
        v.dataset.seg = seg;
        v.onclick = () => toggleSegment(seg, v);
        rowDiv.appendChild(v);
      });
      regionDiv.appendChild(rowDiv);
    });
    container.appendChild(regionDiv);
  });
}

function toggleSegment(seg, el) {
  const idx = selectedSegments.indexOf(seg);
  if (idx === -1) { selectedSegments.push(seg); el.classList.add('selected'); }
  else { selectedSegments.splice(idx, 1); el.classList.remove('selected'); }
  updateSegmentDisplay();
}

function updateSegmentDisplay() {
  const box = document.getElementById('selectedSegments');
  if (selectedSegments.length === 0) { box.innerHTML = '<p>None selected</p>'; return; }
  box.innerHTML = selectedSegments.map(s => `<span class="segment-tag" onclick="removeSegment('${s}')">${s} ×</span>`).join('');
}

function removeSegment(seg) {
  selectedSegments = selectedSegments.filter(s => s !== seg);
  document.querySelectorAll(`.vertebra[data-seg="${seg}"]`).forEach(el => el.classList.remove('selected'));
  updateSegmentDisplay();
}

function clearSpine() {
  selectedSegments = [];
  document.querySelectorAll('.vertebra').forEach(el => el.classList.remove('selected'));
  updateSegmentDisplay();
}

function setupSpeechRecognition() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SpeechRecognition) { document.getElementById('recordStatus').textContent = 'Speech recognition not supported in this browser. Use Chrome.'; return; }
  recognition = new SpeechRecognition();
  recognition.continuous = true;
  recognition.interimResults = true;
  recognition.lang = 'en-US';
  let interim = '';
  recognition.onresult = e => {
    interim = '';
    for (let i = e.resultIndex; i < e.results.length; i++) {
      if (e.results[i].isFinal) { fullTranscript += e.results[i][0].transcript + ' '; }
      else { interim = e.results[i][0].transcript; }
    }
    const box = document.getElementById('transcriptBox');
    box.innerHTML = `<span style="color:#333">${fullTranscript}</span><span style="color:#aab4c4;font-style:italic">${interim}</span>`;
  };
  recognition.onerror = e => { if (e.error !== 'no-speech') { showToast('Microphone error: ' + e.error); stopRecording(); } };
  recognition.onend = () => { if (isRecording) recognition.start(); };
}

function toggleRecording() {
  if (!currentPatient) { showToast('Please select a patient first'); return; }
  isRecording ? stopRecording() : startRecording();
}

function startRecording() {
  if (!recognition) { showToast('Speech recognition not available'); return; }
  isRecording = true;
  recognition.start();
  document.getElementById('micBtn').classList.add('recording');
  document.getElementById('recordStatus').textContent = 'Recording... Click to stop';
  document.getElementById('recordStatus').className = 'record-status active';
  const box = document.getElementById('transcriptBox');
  if (box.querySelector('.placeholder')) box.innerHTML = '';
}

function stopRecording() {
  isRecording = false;
  if (recognition) recognition.stop();
  document.getElementById('micBtn').classList.remove('recording');
  document.getElementById('recordStatus').textContent = 'Click microphone to begin dictation';
  document.getElementById('recordStatus').className = 'record-status';
}

document.getElementById('patientInput').addEventListener('input', function() {
  const val = this.value.trim().toLowerCase();
  const list = document.getElementById('autocompleteList');
  if (!val) { list.classList.remove('show'); return; }
  const matches = patients.filter(p => `${p.first} ${p.last}`.toLowerCase().includes(val));
  if (matches.length === 0) { list.classList.remove('show'); return; }
  list.innerHTML = matches.map(p => `<div class="autocomplete-item" onclick="selectPatient('${p.id}')">${p.first} ${p.last}${p.dob ? ' — ' + p.dob : ''}</div>`).join('');
  list.classList.add('show');
});

document.addEventListener('click', e => { if (!e.target.closest('.patient-search')) document.getElementById('autocompleteList').classList.remove('show'); });

function selectPatient(id) {
  currentPatient = patients.find(p => p.id === id);
  document.getElementById('patientInput').value = `${currentPatient.first} ${currentPatient.last}`;
  document.getElementById('autocompleteList').classList.remove('show');
  document.getElementById('patientBanner').innerHTML = `<div><div class="patient-name">${currentPatient.first} ${currentPatient.last}</div><div class="patient-sub">${currentPatient.dob ? 'DOB: ' + currentPatient.dob + '  |  ' : ''}Patient ID: ${currentPatient.id}</div></div>`;
  loadHistory();
}

function openAddPatient() { document.getElementById('addPatientModal').classList.add('show'); document.getElementById('newPatientFirst').focus(); }
function closeModal() { document.getElementById('addPatientModal').classList.remove('show'); }

function saveNewPatient() {
  const first = document.getElementById('newPatientFirst').value.trim();
  const last = document.getElementById('newPatientLast').value.trim();
  const dob = document.getElementById('newPatientDOB').value.trim();
  if (!first || !last) { showToast('Please enter first and last name'); return; }
  const patient = { id: Date.now().toString(), first, last, dob };
  patients.push(patient);
  localStorage.setItem('chironote_patients', JSON.stringify(patients));
  closeModal();
  document.getElementById('newPatientFirst').value = '';
  document.getElementById('newPatientLast').value = '';
  document.getElementById('newPatientDOB').value = '';
  selectPatient(patient.id);
  showToast(`${first} ${last} added`);
}

function selectTemplate(btn, template) {
  document.querySelectorAll('.template-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  currentTemplate = template;
  const titles = { soap_new: 'New Patient SOAP Note', soap_followup: 'Follow Up SOAP Note', referral: 'Referral Letter' };
  document.getElementById('noteTitle').textContent = titles[template];
}

async function generateNote() {
  if (!fullTranscript.trim()) { showToast('Please dictate your clinical findings first'); return; }
  const segs = selectedSegments.length > 0 ? `Marked spinal segments: ${selectedSegments.join(', ')}.` : '';
  const patientName = currentPatient ? `${currentPatient.first} ${currentPatient.last}` : 'Patient';
  const today = new Date().toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
  let prompt = '';
  if (currentTemplate === 'soap_new') {
    prompt = `You are a chiropractic clinical documentation assistant. Format the following dictated notes into a professional new patient SOAP note. Use chiropractic-specific terminology including spinal segments, range of motion measurements, subluxation, adjustment techniques, and treatment response language. ${segs} Patient: ${patientName}. Date: ${today}. Dictated notes: "${fullTranscript}". Return only the formatted SOAP note with four clearly labeled sections: SUBJECTIVE, OBJECTIVE, ASSESSMENT, PLAN. Do not include any preamble.`;
  } else if (currentTemplate === 'soap_followup') {
    prompt = `You are a chiropractic clinical documentation assistant. Format the following dictated notes into a professional follow-up SOAP note. Use chiropractic-specific terminology. ${segs} Patient: ${patientName}. Date: ${today}. Dictated notes: "${fullTranscript}". Return only the formatted follow-up SOAP note with four clearly labeled sections: SUBJECTIVE, OBJECTIVE, ASSESSMENT, PLAN. Keep it concise and progress-focused. Do not include any preamble.`;
  } else {
    prompt = `You are a chiropractic clinical documentation assistant. Format the following dictated notes into a professional referral letter from a chiropractor. Use professional clinical language. ${segs} Patient: ${patientName}. Date: ${today}. Dictated notes: "${fullTranscript}". Return only the formatted referral letter ready to send. Do not include any preamble.`;
  }
  const noteContent = document.getElementById('noteContent');
  noteContent.innerHTML = '<div class="generating"><div class="spinner"></div> Generating note...</div>';
  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ model: 'claude-sonnet-4-20250514', max_tokens: 1000, messages: [{ role: 'user', content: prompt }] })
    });
    const data = await response.json();
    const text = data.content?.find(c => c.type === 'text')?.text || '';
    renderNote(text);
  } catch (err) {
    noteContent.innerHTML = '<div style="color:#c0392b;padding:10px;">Error generating note. Please try again.</div>';
  }
}

function renderNote(text) {
  const noteContent = document.getElementById('noteContent');
  if (currentTemplate === 'referral') {
    noteContent.innerHTML = `<div class="soap-content" contenteditable="true" style="white-space:pre-wrap">${text}</div>`;
    return;
  }
  const sections = { SUBJECTIVE: '', OBJECTIVE: '', ASSESSMENT: '', PLAN: '' };
  let current = null;
  text.split('\n').forEach(line => {
    const upper = line.trim().toUpperCase();
    if (upper.startsWith('SUBJECTIVE')) { current = 'SUBJECTIVE'; }
    else if (upper.startsWith('OBJECTIVE')) { current = 'OBJECTIVE'; }
    else if (upper.startsWith('ASSESSMENT')) { current = 'ASSESSMENT'; }
    else if (upper.startsWith('PLAN')) { current = 'PLAN'; }
    else if (current && line.trim()) { sections[current] += line.trim() + '\n'; }
  });
  noteContent.innerHTML = Object.entries(sections).map(([label, content]) =>
    `<div class="soap-section"><div class="soap-label">${label}</div><div class="soap-content" contenteditable="true" data-placeholder="No ${label.toLowerCase()} findings documented">${content.trim()}</div></div>`
  ).join('');
}

function saveNote() {
  if (!currentPatient) { showToast('Please select a patient first'); return; }
  const contents = document.querySelectorAll('.soap-content');
  if (contents.length === 0) { showToast('Please generate a note first'); return; }
  let noteText = '';
  if (currentTemplate !== 'referral') {
    ['SUBJECTIVE','OBJECTIVE','ASSESSMENT','PLAN'].forEach((label, i) => {
      if (contents[i]) noteText += `${label}:\n${contents[i].textContent}\n\n`;
    });
  } else { noteText = contents[0]?.textContent || ''; }
  const note = {
    id: Date.now().toString(), patientId: currentPatient.id,
    date: new Date().toLocaleDateString('en-US'), template: currentTemplate,
    segments: [...selectedSegments], text: noteText, transcript: fullTranscript
  };
  notes.push(note);
  localStorage.setItem('chironote_notes', JSON.stringify(notes));
  loadHistory();
  showToast('Note saved successfully');
}

function copyNote() {
  const contents = document.querySelectorAll('.soap-content');
  if (contents.length === 0) { showToast('Nothing to copy yet'); return; }
  let text = `${document.getElementById('noteTitle').textContent}\n`;
  if (currentPatient) text += `Patient: ${currentPatient.first} ${currentPatient.last}\n`;
  text += `Date: ${new Date().toLocaleDateString('en-US')}\n\n`;
  if (currentTemplate !== 'referral') {
    ['SUBJECTIVE','OBJECTIVE','ASSESSMENT','PLAN'].forEach((label, i) => {
      if (contents[i]) text += `${label}:\n${contents[i].textContent}\n\n`;
    });
  } else { text = contents[0]?.textContent || ''; }
  if (selectedSegments.length > 0) text += `\nMarked segments: ${selectedSegments.join(', ')}`;
  navigator.clipboard.writeText(text).then(() => showToast('Copied to clipboard'));
}

function clearAll() {
  fullTranscript = '';
  document.getElementById('transcriptBox').innerHTML = '<span class="placeholder">Your spoken words will appear here as you dictate...</span>';
  document.getElementById('noteContent').innerHTML = '<div style="color:#aab4c4;font-style:italic;font-size:0.88rem;padding:10px 0;">Dictate your clinical findings above, then the formatted note will appear here.</div>';
  if (isRecording) stopRecording();
}

function loadHistory() {
  if (!currentPatient) return;
  const patientNotes = notes.filter(n => n.patientId === currentPatient.id).reverse();
  const list = document.getElementById('historyList');
  if (patientNotes.length === 0) { list.innerHTML = '<div class="no-history">No previous visits</div>'; return; }
  const labels = { soap_new: 'New Patient SOAP', soap_followup: 'Follow Up Note', referral: 'Referral Letter' };
  list.innerHTML = patientNotes.map(n =>
    `<div class="history-item" onclick="viewNote('${n.id}')"><div class="history-date">${n.date}</div><div class="history-type">${labels[n.template] || n.template}</div>${n.segments?.length > 0 ? `<div style="font-size:0.72rem;color:#6a84a8;margin-top:2px">${n.segments.join(', ')}</div>` : ''}</div>`
  ).join('');
}

function viewNote(id) {
  const note = notes.find(n => n.id === id);
  if (!note) return;
  const labels = { soap_new: 'New Patient SOAP', soap_followup: 'Follow Up Note', referral: 'Referral Letter' };
  document.getElementById('viewNoteTitle').textContent = `${labels[note.template]} — ${note.date}`;
  document.getElementById('viewNoteContent').innerHTML = `<pre style="white-space:pre-wrap;font-family:'Source Sans 3',sans-serif;font-size:0.88rem">${note.text}</pre>${note.segments?.length > 0 ? `<div style="margin-top:12px;font-size:0.8rem;color:#888"><strong>Marked segments:</strong> ${note.segments.join(', ')}</div>` : ''}`;
  document.getElementById('viewNoteModal').classList.add('show');
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2800);
}

buildSpine();
setupSpeechRecognition();
</script>
</body>
</html>
