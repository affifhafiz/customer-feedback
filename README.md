<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1">
<title>Quick Feedback</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@500;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  :root{
    --bg:#10161d;
    --surface:#1a232d;
    --surface-2:#212c38;
    --text:#eef2f5;
    --text-dim:#8b9aa8;
    --empty:#e0554f;
    --mid:#f5a623;
    --full:#2dd4a7;
    --border:#2a3642;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:'Inter',sans-serif;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
  }
  .card{
    width:100%;
    max-width:440px;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:18px;
    padding:32px 28px 24px;
    position:relative;
  }
  .eyebrow{
    font-size:12px;
    letter-spacing:.14em;
    text-transform:uppercase;
    color:var(--mid);
    font-weight:600;
    margin:0 0 6px;
  }
  h1{
    font-family:'Oswald',sans-serif;
    font-weight:700;
    text-transform:uppercase;
    font-size:28px;
    letter-spacing:.02em;
    margin:0 0 24px;
    line-height:1.15;
  }
  .gauge-wrap{ margin-bottom:28px; }
  .gauge-track{
    height:14px;
    border-radius:8px;
    background:linear-gradient(90deg,var(--empty),var(--mid),var(--full));
    position:relative;
    margin-bottom:10px;
  }
  input[type=range]{
    -webkit-appearance:none;
    width:100%;
    height:14px;
    background:transparent;
    margin:-14px 0 0;
    position:relative;
    z-index:2;
  }
  input[type=range]::-webkit-slider-thumb{
    -webkit-appearance:none;
    width:26px;height:26px;
    border-radius:50%;
    background:#fff;
    border:3px solid var(--surface-2);
    box-shadow:0 2px 6px rgba(0,0,0,.4);
    cursor:pointer;
    margin-top:-6px;
  }
  input[type=range]::-moz-range-thumb{
    width:26px;height:26px;border-radius:50%;background:#fff;
    border:3px solid var(--surface-2);cursor:pointer;
  }
  .gauge-labels{
    display:flex; justify-content:space-between;
    font-size:11px; color:var(--text-dim); text-transform:uppercase; letter-spacing:.06em;
    margin-bottom:14px;
  }
  .gauge-readout{
    text-align:center;
    font-family:'Oswald',sans-serif;
    font-size:20px;
    font-weight:700;
    text-transform:uppercase;
    min-height:26px;
  }
  .tags{
    display:flex; flex-wrap:wrap; gap:8px;
    margin-bottom:20px;
  }
  .tag{
    padding:8px 14px;
    border-radius:20px;
    border:1px solid var(--border);
    background:var(--surface-2);
    color:var(--text-dim);
    font-size:13px;
    cursor:pointer;
    user-select:none;
    transition:.15s;
  }
  .tag.on{
    background:var(--mid);
    border-color:var(--mid);
    color:#1a1200;
    font-weight:600;
  }
  textarea{
    width:100%;
    background:var(--surface-2);
    border:1px solid var(--border);
    border-radius:10px;
    color:var(--text);
    padding:12px;
    font-family:inherit;
    font-size:14px;
    resize:vertical;
    min-height:70px;
    margin-bottom:20px;
  }
  textarea:focus, .tag:focus-visible, button:focus-visible, input:focus-visible{
    outline:2px solid var(--full); outline-offset:2px;
  }
  button.primary{
    width:100%;
    background:var(--full);
    color:#08201a;
    border:none;
    border-radius:10px;
    padding:14px;
    font-size:15px;
    font-weight:700;
    letter-spacing:.02em;
    cursor:pointer;
  }
  button.primary:disabled{ opacity:.5; cursor:default; }
  .staff-link{
    display:block;
    text-align:center;
    margin-top:18px;
    font-size:12px;
    color:var(--text-dim);
    text-decoration:underline;
    cursor:pointer;
    background:none;border:none;
  }
  .thanks{ text-align:center; padding:20px 0; }
  .thanks .big{ font-size:44px; margin-bottom:10px; }
  .thanks p{ color:var(--text-dim); margin-bottom:24px; }
  .hidden{ display:none !important; }

  /* dashboard */
  .stat-row{ display:flex; gap:12px; margin-bottom:20px; }
  .stat{
    flex:1; background:var(--surface-2); border:1px solid var(--border);
    border-radius:12px; padding:14px; text-align:center;
  }
  .stat .n{ font-family:'Oswald',sans-serif; font-size:26px; font-weight:700; }
  .stat .l{ font-size:11px; color:var(--text-dim); text-transform:uppercase; letter-spacing:.06em; }
  .barrow{ margin-bottom:10px; }
  .barrow .top{ display:flex; justify-content:space-between; font-size:12px; margin-bottom:4px; color:var(--text-dim); }
  .barbg{ height:8px; background:var(--surface-2); border-radius:4px; overflow:hidden; }
  .barfill{ height:100%; background:var(--mid); }
  .comment{
    background:var(--surface-2); border:1px solid var(--border);
    border-radius:10px; padding:10px 12px; margin-bottom:8px; font-size:13px;
  }
  .comment .meta{ color:var(--text-dim); font-size:11px; margin-top:4px; }
  .section-title{ font-size:12px; text-transform:uppercase; letter-spacing:.08em; color:var(--text-dim); margin:22px 0 10px; }
  .row-btns{ display:flex; gap:10px; margin-top:20px; }
  button.secondary{
    flex:1; background:var(--surface-2); color:var(--text); border:1px solid var(--border);
    border-radius:10px; padding:11px; font-size:13px; cursor:pointer;
  }
  .pin-modal{
    position:fixed; inset:0; background:rgba(0,0,0,.6);
    display:flex; align-items:center; justify-content:center; z-index:10;
  }
  .pin-box{
    background:var(--surface); border:1px solid var(--border); border-radius:14px;
    padding:24px; width:260px; text-align:center;
  }
  .pin-box input{
    width:100%; text-align:center; font-size:22px; letter-spacing:.3em;
    background:var(--surface-2); border:1px solid var(--border); border-radius:8px;
    color:var(--text); padding:10px; margin:14px 0; font-family:inherit;
  }
  .err{ color:var(--empty); font-size:12px; margin-top:6px; min-height:16px; }
  #qrBox{ background:#fff; padding:14px; border-radius:10px; display:inline-block; margin-top:10px; }
  .loading{ text-align:center; color:var(--text-dim); font-size:13px; padding:20px 0; }
</style>
</head>
<body>

<div class="card">

  <!-- CUSTOMER VIEW -->
  <div id="customerView">
    <div id="formStage">
      <p class="eyebrow">Quick Feedback</p>
      <h1>How was your visit today?</h1>

      <div class="gauge-wrap">
        <div class="gauge-track"></div>
        <input type="range" id="ratingInput" min="1" max="5" value="3">
        <div class="gauge-labels"><span>Empty</span><span>Full</span></div>
        <div class="gauge-readout" id="gaugeReadout">Half — Okay</div>
      </div>

      <div class="tags" id="tagRow">
        <div class="tag" data-tag="Friendly staff">Friendly staff</div>
        <div class="tag" data-tag="Fast service">Fast service</div>
        <div class="tag" data-tag="Clean store">Clean store</div>
        <div class="tag" data-tag="Fair prices">Fair prices</div>
        <div class="tag" data-tag="Long queue">Long queue</div>
      </div>

      <textarea id="commentInput" placeholder="Anything else? (optional)"></textarea>

      <button class="primary" id="submitBtn">Send feedback</button>
      <div class="err" id="formErr"></div>
      <button class="staff-link" id="staffLinkBtn">Staff login</button>
    </div>

    <div id="thanksStage" class="thanks hidden">
      <div class="big">✅</div>
      <h1 style="margin-bottom:6px;">Thanks!</h1>
      <p>Your feedback helps us improve.</p>
      <button class="primary" id="againBtn">Rate again</button>
    </div>
  </div>

  <!-- DASHBOARD VIEW -->
  <div id="dashboardView" class="hidden">
    <p class="eyebrow">Staff Dashboard</p>
    <h1>Customer Feedback</h1>
    <div id="dashLoading" class="loading">Loading feedback…</div>
    <div id="dashContent" class="hidden">
      <div class="stat-row">
        <div class="stat"><div class="n" id="statAvg">–</div><div class="l">Avg rating</div></div>
        <div class="stat"><div class="n" id="statCount">–</div><div class="l">Responses</div></div>
      </div>

      <div class="section-title">What customers mention</div>
      <div id="tagBars"></div>

      <div class="section-title">Recent comments</div>
      <div id="commentsList"></div>

      <div class="row-btns">
        <button class="secondary" id="qrBtn">Show QR code</button>
        <button class="secondary" id="resetBtn">Clear all data</button>
      </div>
      <div id="qrArea" style="text-align:center;" class="hidden">
        <div id="qrBox"></div>
        <p style="font-size:11px;color:var(--text-dim);max-width:340px;margin:10px auto 0;">
          This encodes the current page link. Publish/share this artifact first to get a permanent link before printing the QR code for the pump.
        </p>
      </div>
    </div>
    <button class="staff-link" id="backBtn">← Back to customer view</button>
  </div>

  <!-- PIN MODAL -->
  <div class="pin-modal hidden" id="pinModal">
    <div class="pin-box">
      <p class="eyebrow" style="text-align:center;">Staff Access</p>
      <div>Enter PIN</div>
      <input type="password" id="pinInput" maxlength="4" inputmode="numeric" placeholder="••••">
      <div class="err" id="pinErr"></div>
      <div class="row-btns">
        <button class="secondary" id="pinCancel">Cancel</button>
        <button class="primary" id="pinOk">Enter</button>
      </div>
    </div>
  </div>

</div>

<script>
const STAFF_PIN = "1234"; // change this before printing the QR code
const RATING_LABELS = {1:"Empty — Not good",2:"Low — Needs work",3:"Half — Okay",4:"3/4 — Good",5:"Full — Excellent!"};
const selectedTags = new Set();

const ratingInput = document.getElementById('ratingInput');
const gaugeReadout = document.getElementById('gaugeReadout');
ratingInput.addEventListener('input', () => {
  gaugeReadout.textContent = RATING_LABELS[ratingInput.value];
});

document.querySelectorAll('.tag').forEach(el=>{
  el.addEventListener('click', ()=>{
    const t = el.dataset.tag;
    if(selectedTags.has(t)){ selectedTags.delete(t); el.classList.remove('on'); }
    else { selectedTags.add(t); el.classList.add('on'); }
  });
});

async function loadEntries(){
  try{
    const res = await window.storage.get('entries', true);
    return res ? JSON.parse(res.value) : [];
  }catch(e){
    return [];
  }
}
async function saveEntries(entries){
  return await window.storage.set('entries', JSON.stringify(entries), true);
}

document.getElementById('submitBtn').addEventListener('click', async ()=>{
  const btn = document.getElementById('submitBtn');
  const errEl = document.getElementById('formErr');
  errEl.textContent = '';
  btn.disabled = true; btn.textContent = 'Sending…';
  try{
    const entries = await loadEntries();
    entries.push({
      id: Date.now()+'-'+Math.random().toString(36).slice(2,7),
      rating: Number(ratingInput.value),
      tags: Array.from(selectedTags),
      comment: document.getElementById('commentInput').value.trim(),
      ts: Date.now()
    });
    const result = await saveEntries(entries);
    if(!result) throw new Error('save failed');
    document.getElementById('formStage').classList.add('hidden');
    document.getElementById('thanksStage').classList.remove('hidden');
  }catch(e){
    errEl.textContent = "Couldn't send feedback — please try again.";
  }finally{
    btn.disabled = false; btn.textContent = 'Send feedback';
  }
});

document.getElementById('againBtn').addEventListener('click', ()=>{
  ratingInput.value = 3;
  gaugeReadout.textContent = RATING_LABELS[3];
  selectedTags.clear();
  document.querySelectorAll('.tag').forEach(el=>el.classList.remove('on'));
  document.getElementById('commentInput').value = '';
  document.getElementById('thanksStage').classList.add('hidden');
  document.getElementById('formStage').classList.remove('hidden');
});

// staff / pin flow
const pinModal = document.getElementById('pinModal');
document.getElementById('staffLinkBtn').addEventListener('click', ()=>{
  document.getElementById('pinInput').value = '';
  document.getElementById('pinErr').textContent = '';
  pinModal.classList.remove('hidden');
});
document.getElementById('pinCancel').addEventListener('click', ()=> pinModal.classList.add('hidden'));
document.getElementById('pinOk').addEventListener('click', async ()=>{
  if(document.getElementById('pinInput').value === STAFF_PIN){
    pinModal.classList.add('hidden');
    document.getElementById('customerView').classList.add('hidden');
    document.getElementById('dashboardView').classList.remove('hidden');
    await renderDashboard();
  } else {
    document.getElementById('pinErr').textContent = 'Wrong PIN';
  }
});

document.getElementById('backBtn').addEventListener('click', ()=>{
  document.getElementById('dashboardView').classList.add('hidden');
  document.getElementById('customerView').classList.remove('hidden');
  document.getElementById('qrArea').classList.add('hidden');
});

async function renderDashboard(){
  document.getElementById('dashLoading').classList.remove('hidden');
  document.getElementById('dashContent').classList.add('hidden');
  const entries = await loadEntries();
  document.getElementById('dashLoading').classList.add('hidden');
  document.getElementById('dashContent').classList.remove('hidden');

  document.getElementById('statCount').textContent = entries.length;
  const avg = entries.length ? (entries.reduce((s,e)=>s+e.rating,0)/entries.length) : 0;
  document.getElementById('statAvg').textContent = entries.length ? avg.toFixed(1)+' / 5' : '–';

  const tagCounts = {};
  entries.forEach(e=> (e.tags||[]).forEach(t=> tagCounts[t]=(tagCounts[t]||0)+1));
  const tagBars = document.getElementById('tagBars');
  tagBars.innerHTML = '';
  const maxCount = Math.max(1, ...Object.values(tagCounts));
  Object.entries(tagCounts).sort((a,b)=>b[1]-a[1]).forEach(([tag,count])=>{
    const row = document.createElement('div');
    row.className = 'barrow';
    row.innerHTML = `<div class="top"><span>${tag}</span><span>${count}</span></div>
      <div class="barbg"><div class="barfill" style="width:${(count/maxCount*100)}%"></div></div>`;
    tagBars.appendChild(row);
  });
  if(Object.keys(tagCounts).length===0){
    tagBars.innerHTML = '<div class="loading">No tags yet</div>';
  }

  const commentsList = document.getElementById('commentsList');
  commentsList.innerHTML = '';
  const withComments = entries.filter(e=>e.comment).sort((a,b)=>b.ts-a.ts).slice(0,15);
  if(withComments.length===0){
    commentsList.innerHTML = '<div class="loading">No comments yet</div>';
  }
  withComments.forEach(e=>{
    const div = document.createElement('div');
    div.className = 'comment';
    div.textContent = e.comment;
    const meta = document.createElement('div');
    meta.className = 'meta';
    meta.textContent = `Rating ${e.rating}/5 · ${new Date(e.ts).toLocaleString()}`;
    div.appendChild(meta);
    commentsList.appendChild(div);
  });
}

document.getElementById('qrBtn').addEventListener('click', ()=>{
  const area = document.getElementById('qrArea');
  const box = document.getElementById('qrBox');
  box.innerHTML = '';
  area.classList.remove('hidden');
  try{
    new QRCode(box, { text: window.location.href, width:180, height:180 });
  }catch(e){
    box.textContent = 'QR generation failed.';
  }
});

document.getElementById('resetBtn').addEventListener('click', async ()=>{
  if(!confirm('Delete all stored feedback? This cannot be undone.')) return;
  try{
    await window.storage.delete('entries', true);
    await renderDashboard();
  }catch(e){
    alert('Could not clear data.');
  }
});
</script>
<img src="https://googleapis.com" alt="QR Code">
</body>
</html>
