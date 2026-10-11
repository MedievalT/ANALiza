<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>ANALiza - Private</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-zinc-900 text-white p-4 md:p-8">
<div class="max-w-6xl mx-auto grid md:grid-cols-[1fr_360px] gap-8">

  <!-- LEFT: INPUTS + RESULT -->
  <div>
    <h1 class="text-3xl font-black">ANALiza</h1>
    <p class="text-zinc-400 mb-6 text-sm">Private • 9 metrics • Max 27p • Auto-saves</p>

    <input id="ticker" placeholder="TICKER e.g. NVDA"
      class="w-full p-3 rounded-xl bg-zinc-800 text-lg font-black uppercase outline-none focus:ring-2 ring-white mb-6"
      oninput="render()">

    <div id="metrics" class="space-y-3"></div>

    <div class="grid grid-cols-3 gap-3 mt-8">
      <button onclick="saveAnalysis()" class="col-span-2 bg-white text-black font-black py-3 rounded-xl">SAVE THIS COMPANY</button>
      <button onclick="clearHistory()" class="bg-zinc-800 py-3 rounded-xl text-sm">Clear All</button>
    </div>

    <!-- LIVE RESULT CARD (in page) -->
    <div id="resultCard" class="bg-zinc-800 rounded-2xl p-6 mt-8">
      <div class="flex justify-between items-center">
        <div>
          <div id="outTicker" class="text-4xl font-black">—</div>
          <div id="outDate" class="text-zinc-400 text-xs mt-1"></div>
        </div>
        <div id="outScoreBox" class="w-24 h-24 rounded-2xl flex flex-col items-center justify-center bg-zinc-700">
          <span id="outScore" class="text-3xl font-black">0</span>
          <span class="text-xs font-bold">/27</span>
        </div>
      </div>
      <div id="outRec" class="mt-6 w-full py-4 rounded-xl text-center text-xl font-black tracking-widest bg-zinc-700">SELECT METRICS</div>
      <div id="outBreakdown" class="mt-6 space-y-2"></div>
      <div class="mt-6">
        <div class="flex justify-between text-xs text-zinc-400 mb-2"><span>Score</span><span id="outPercent">0%</span></div>
        <div class="w-full bg-zinc-900 h-3 rounded-full overflow-hidden"><div id="outBar" class="h-full transition-all" style="width:0%"></div></div>
      </div>
    </div>
  </div>

  <!-- RIGHT: HISTORY -->
  <div class="bg-zinc-800 rounded-2xl p-5 h-fit sticky top-6">
    <div id="historyPanel"></div>
    <button onclick="exportJson()" class="w-full mt-4 bg-zinc-700 py-2 rounded-lg text-sm">Export Backup (JSON)</button>
  </div>

</div>

<script>
const METRICS = [
  { key:'pe', name:'P/E', options:[ {label:'<15', pts:3}, {label:'15-20', pts:2}, {label:'>20', pts:1} ] },
  { key:'debt', name:'Debt', options:[ {label:'Low', pts:3}, {label:'Medium', pts:2}, {label:'Large', pts:1} ] },
  { key:'fcf', name:'Free Cash Flow', options:[ {label:'Growing', pts:3}, {label:'Alternating', pts:2}, {label:'Inconsistent', pts:1} ] },
  { key:'sbc', name:'Stock Based Comp / FCF', options:[ {label:'Under 10%', pts:3}, {label:'10-15%', pts:2}, {label:'>15%', pts:1} ] },
  { key:'buybacks', name:'Buybacks', options:[ {label:'Consistent 10y', pts:3}, {label:'Low', pts:2}, {label:'No buybacks', pts:1} ] },
  { key:'cash', name:'Cash vs Market Cap', options:[ {label:'10% of mkt cap', pts:3}, {label:'Under 10%', pts:2}, {label:'Low cash', pts:1} ] },
  { key:'moat', name:'Moat', options:[ {label:'Moat', pts:3}, {label:'Top 10 in sector', pts:2}, {label:'No moat', pts:1} ] },
  { key:'div', name:'Dividend Payout Ratio', options:[ {label:'Under 35%', pts:3}, {label:'35-55%', pts:2}, {label:'>55%', pts:1} ] },
  { key:'eps', name:'Growing EPS', options:[ {label:'Consistent', pts:3}, {label:'Low growth', pts:2}, {label:'Inconsistent', pts:1} ] },
];

let selections = {};
METRICS.forEach(m=> selections[m.key]=m.options[0]);

const container = document.getElementById('metrics');
METRICS.forEach(m=>{
  container.innerHTML+=`
    <div class="bg-zinc-800 p-4 rounded-xl flex justify-between items-center">
      <span class="font-medium text-[15px]">${m.name}</span>
      <select id="sel-${m.key}" onchange="updateSel('${m.key}',this.value)" class="bg-zinc-700 rounded-lg px-3 py-1.5 text-sm">
        ${m.options.map((o,i)=>`<option value="${i}">${o.label} (${o.pts}p)</option>`).join('')}
      </select>
    </div>`;
});

function updateSel(k,v){ selections[k]=METRICS.find(x=>x.key==k).options[v]; render(); }
function getRec(t){
  if(t>=23) return {text:'STRONG BUY', bg:'#22c55e', color:'black'};
  if(t>=18) return {text:'BUY', bg:'#84cc16', color:'black'};
  if(t>=12) return {text:'HOLD', bg:'#eab308', color:'black'};
  return {text:'AVOID', bg:'#ef4444', color:'white'};
}
function render(){
  const ticker=document.getElementById('ticker').value||'—';
  let total=0; Object.values(selections).forEach(s=>total+=s.pts);
  const rec=getRec(total); const max=27; const pct=Math.round(total/max*100);
  document.getElementById('outTicker').innerText=ticker.toUpperCase();
  document.getElementById('outDate').innerText=new Date().toLocaleString();
  document.getElementById('outScore').innerText=total;
  document.getElementById('outScoreBox').style.background=rec.bg;
  document.getElementById('outScoreBox').style.color=rec.color;
  document.getElementById('outRec').innerText=rec.text;
  document.getElementById('outRec').style.background=rec.bg;
  document.getElementById('outRec').style.color=rec.color;
  document.getElementById('outPercent').innerText=pct+'%';
  document.getElementById('outBar').style.width=pct+'%';
  document.getElementById('outBar').style.background=rec.bg;
  document.getElementById('outBreakdown').innerHTML=METRICS.map(m=>{
    const sel=selections[m.key]; const col=sel.pts==3?'#22c55e':sel.pts==2?'#eab308':'#ef4444';
    return `<div class="flex justify-between items-center bg-zinc-900 p-3 rounded-lg text-sm">
      <span>${m.name}</span><span class="font-bold px-2 py-1 rounded" style="background:${col};color:${sel.pts==1?'white':'black'}">${sel.label} • ${sel.pts}p</span></div>`;
  }).join('');
}
function saveAnalysis(){
  const ticker=document.getElementById('ticker').value||'UNKNOWN';
  if(ticker.length<1) return alert('Add ticker first');
  let total=0; Object.values(selections).forEach(s=>total+=s.pts);
  const rec=getRec(total);
  const entry={ ticker:ticker.toUpperCase(), date:new Date().toLocaleString(), selections:JSON.parse(JSON.stringify(selections)), total, rec:rec.text };
  let h=JSON.parse(localStorage.getItem('analizaHistory')||'[]');
  h.unshift(entry); localStorage.setItem('analizaHistory',JSON.stringify(h)); renderHistory();
}
function renderHistory(){
  let h=JSON.parse(localStorage.getItem('analizaHistory')||'[]');
