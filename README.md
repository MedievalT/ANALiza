<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Stock Lab - Private</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-zinc-900 text-white p-4 md:p-8">
<div class="max-w-5xl mx-auto grid md:grid-cols-2 gap-8">

  <!-- LEFT: INPUTS -->
  <div>
    <h1 class="text-2xl font-bold mb-1">My Stock Evaluation</h1>
    <p class="text-zinc-400 mb-6">100% private • No APIs</p>

    <input id="ticker" placeholder="TICKER e.g. AAPL"
      class="w-full p-3 rounded-lg bg-zinc-800 text-lg font-bold uppercase mb-6 outline-none focus:ring-2 ring-white"
      oninput="render()">

    <div id="metrics" class="space-y-3"></div>
  </div>

  <!-- RIGHT: LIVE RESULT -->
  <div class="bg-zinc-800 rounded-2xl p-6 h-fit sticky top-6">
    <div class="flex justify-between items-start">
      <div>
        <div id="outTicker" class="text-4xl font-black">—</div>
        <div id="outDate" class="text-zinc-400 text-sm mt-1"></div>
      </div>
      <div id="outScoreBox" class="w-24 h-24 rounded-2xl flex flex-col items-center justify-center">
        <span id="outScore" class="text-3xl font-black">0</span>
        <span class="text-xs font-bold opacity-70">/24</span>
      </div>
    </div>

    <div id="outRec" class="mt-6 w-full py-4 rounded-xl text-center text-xl font-black tracking-widest">SELECT METRICS</div>

    <div class="mt-8 space-y-3" id="outBreakdown"></div>

    <div class="mt-8 pt-6 border-t border-zinc-700">
      <div class="flex justify-between text-sm text-zinc-400 mb-2">
        <span>Score Progress</span><span id="outPercent">0%</span>
      </div>
      <div class="w-full bg-zinc-900 h-3 rounded-full overflow-hidden">
        <div id="outBar" class="h-full transition-all duration-500" style="width:0%"></div>
      </div>
    </div>
  </div>

</div>

<script>
const METRICS = [
  { key: 'debt', name: 'Total Debt', options: [ {label:'Low Debt', pts:3}, {label:'Mid Debt', pts:2}, {label:'Huge Debt', pts:1} ] },
  { key: 'cashflow', name: 'Cash Flow / Share', options: [ {label:'Growing', pts:3}, {label:'Flat', pts:2}, {label:'Downwards', pts:1} ] },
  { key: 'margin', name: 'Profit Margin', options: [ {label:'>20% High', pts:3}, {label:'10-20% Mid', pts:2}, {label:'<10% Low', pts:1} ] },
  { key: 'revenue', name: 'Revenue Growth 3Y', options: [ {label:'Growing', pts:3}, {label:'Flat', pts:2}, {label:'Declining', pts:1} ] },
  { key: 'moat', name: 'Competitive Moat', options: [ {label:'Strong', pts:3}, {label:'Medium', pts:2}, {label:'Weak', pts:1} ] },
  { key: 'management', name: 'Management', options: [ {label:'Excellent', pts:3}, {label:'OK', pts:2}, {label:'Poor', pts:1} ] },
  { key: 'valuation', name: 'Valuation (P/E)', options: [ {label:'Cheap', pts:3}, {label:'Fair', pts:2}, {label:'Expensive', pts:1} ] },
  { key: 'dividend', name: 'Dividend / Buyback', options: [ {label:'Strong', pts:3}, {label:'Some', pts:2}, {label:'None', pts:1} ] },
];

let selections = {};
METRICS.forEach(m => selections[m.key] = m.options[0]);

const container = document.getElementById('metrics');
METRICS.forEach(m => {
  container.innerHTML += `
    <div class="bg-zinc-800 p-4 rounded-xl flex justify-between items-center">
      <span class="font-medium text-sm">${m.name}</span>
      <select id="sel-${m.key}" onchange="updateSel('${m.key}', this.value)"
        class="bg-zinc-700 rounded-lg px-3 py-1.5 text-sm">
        ${m.options.map((o,i)=>`<option value="${i}">${o.label} (${o.pts}p)</option>`).join('')}
      </select>
    </div>`;
});

function updateSel(key, idx){ selections[key] = METRICS.find(x=>x.key==key).options[idx]; render(); }

function getRec(total){
  if(total>=21) return {text:"STRONG BUY", bg:"#22c55e", color:"black"};
  if(total>=16) return {text:"BUY", bg:"#84cc16", color:"black"};
  if(total>=10) return {text:"HOLD", bg:"#eab308", color:"black"};
  return {text:"AVOID", bg:"#ef4444", color:"white"};
}

function render(){
  const ticker = document.getElementById('ticker').value || '—';
  let total = 0; Object.values(selections).forEach(s=> total+=s.pts);
  const rec = getRec(total);
  const max = METRICS.length * 3;
  const pct = Math.round((total/max)*100);

  document.getElementById('outTicker').innerText = ticker.toUpperCase();
  document.getElementById('outDate').innerText = new Date().toLocaleDateString();
  document.getElementById('outScore').innerText = total;
  document.getElementById('outScoreBox').style.background = rec.bg;
  document.getElementById('outScoreBox').style.color = rec.color;

  document.getElementById('outRec').innerText = rec.text;
  document.getElementById('outRec').style.background = rec.bg;
  document.getElementById('outRec').style.color = rec.color;

  document.getElementById('outPercent').innerText = pct + "%";
  document.getElementById('outBar').style.width = pct + "%";
  document.getElementById('outBar').style.background = rec.bg;

  document.getElementById('outBreakdown').innerHTML = METRICS.map(m=>{
    const sel = selections[m.key];
    const col = sel.pts==3? "#22c55e" : sel.pts==2? "#eab308" : "#ef4444";
    return `<div class="flex justify-between items-center bg-zinc-900 p-3 rounded-lg">
      <span class="text-sm">${m.name}</span>
      <span class="text-sm font-bold px-2 py-1 rounded" style="background:${col}; color:${sel.pts==1?'white':'black'}">${sel.label} • ${sel.pts}p</span>
    </div>`
  }).join('');
}
render();
</script>
</body>
</html>
Compune
Scrie pentru Tintea Adrian
