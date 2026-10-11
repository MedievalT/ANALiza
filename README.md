<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Stock Lab - Private</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-zinc-900 text-white p-4 md:p-8">
<div class="max-w-3xl mx-auto">
  <h1 class="text-2xl font-bold mb-2">My Stock Evaluation</h1>
  <p class="text-zinc-400 mb-6">Private tool - no API, just your metrics</p>

  <input id="ticker" placeholder="Ticker e.g. AAPL" class="w-full p-3 rounded bg-zinc-800 mb-6 text-lg font-bold uppercase">

  <div id="metrics" class="space-y-3"></div>

  <button onclick="generate()" class="w-full mt-8 bg-white text-black font-bold py-3 rounded-lg">Generate Analysis Image</button>

  <canvas id="resultCanvas" width="1080" height="1350" class="w-full mt-8 rounded-xl hidden"></canvas>
  <a id="download" class="hidden mt-4 block text-center bg-zinc-800 py-3 rounded">Download PNG</a>
</div>

<script>
const METRICS = [
  { key: 'debt', name: 'Total Debt', options: [ {label:'Low Debt', pts:3}, {label:'Mid Debt', pts:2}, {label:'Huge Debt', pts:1} ] },
  { key: 'cashflow', name: 'Cash Flow / Share', options: [ {label:'Growing', pts:3}, {label:'Linear / Flat', pts:2}, {label:'Downwards', pts:1} ] },
  { key: 'margin', name: 'Profit Margin', options: [ {label:'>20% High', pts:3}, {label:'10-20% Mid', pts:2}, {label:'<10% Low', pts:1} ] },
  { key: 'revenue', name: 'Revenue Growth 3Y', options: [ {label:'Growing', pts:3}, {label:'Flat', pts:2}, {label:'Declining', pts:1} ] },
  { key: 'moat', name: 'Competitive Moat', options: [ {label:'Strong', pts:3}, {label:'Medium', pts:2}, {label:'Weak', pts:1} ] },
  { key: 'management', name: 'Management', options: [ {label:'Excellent', pts:3}, {label:'OK', pts:2}, {label:'Poor', pts:1} ] },
  { key: 'valuation', name: 'Valuation (P/E)', options: [ {label:'Cheap', pts:3}, {label:'Fair', pts:2}, {label:'Expensive', pts:1} ] },
  { key: 'dividend', name: 'Dividend / Buyback', options: [ {label:'Strong', pts:3}, {label:'Some', pts:2}, {label:'None', pts:1} ] },
];

let selections = {};
const container = document.getElementById('metrics');
METRICS.forEach(m => {
  selections[m.key] = m.options[0];
  let html = `<div class="bg-zinc-800 p-4 rounded-lg flex justify-between items-center">
    <span class="font-medium">${m.name}</span>
    <select onchange="selections['${m.key}']=METRICS.find(x=>x.key=='${m.key}').options[this.value]" class="bg-zinc-700 rounded px-3 py-1">
      ${m.options.map((o,i)=>`<option value="${i}">${o.label} (${o.pts}p)</option>`).join('')}
    </select>
  </div>`;
  container.innerHTML += html;
});

function getRecommendation(total){
  if(total>=21) return {text:"STRONG BUY", color:"#22c55e"};
  if(total>=16) return {text:"BUY", color:"#84cc16"};
  if(total>=10) return {text:"HOLD", color:"#eab308"};
  return {text:"AVOID", color:"#ef4444"};
}

function generate(){
  const ticker = document.getElementById('ticker').value || 'STOCK';
  let total = 0; Object.values(selections).forEach(s=> total+=s.pts);
  const rec = getRecommendation(total);

  const canvas = document.getElementById('resultCanvas');
  const ctx = canvas.getContext('2d');
  canvas.classList.remove('hidden');

  // background
  ctx.fillStyle = "#18181b"; ctx.fillRect(0,0,1080,1350);

  // header
  ctx.fillStyle="white"; ctx.font="bold 80px sans-serif"; ctx.fillText(ticker.toUpperCase(), 60, 130);
  ctx.fillStyle="#a1a1aa"; ctx.font="30px sans-serif"; ctx.fillText(new Date().toLocaleDateString() + " • Private Analysis", 60, 180);

  // score circle
  ctx.fillStyle=rec.color; ctx.beginPath(); ctx.arc(840, 140, 110, 0, Math.PI*2); ctx.fill();
  ctx.fillStyle="black"; ctx.font="bold 70px sans-serif"; ctx.fillText(total+"", 800, 165);
  ctx.font="20px sans-serif"; ctx.fillText("/24", 870, 165);

  // metrics list
  let y=280;
  METRICS.forEach(m=>{
    const sel = selections[m.key];
    ctx.fillStyle="#27272a"; ctx.fillRect(60, y, 960, 85);
    ctx.fillStyle="white"; ctx.font="28px sans-serif"; ctx.fillText(m.name, 90, y+53);
    ctx.fillStyle= sel.pts==3? "#22c55e" : sel.pts==2? "#eab308" : "#ef4444";
    ctx.font="bold 26px sans-serif"; ctx.fillText(sel.label + " • " + sel.pts+"p", 600, y+53);
    y+=105;
  });

  // recommendation bar
  y+=20;
  ctx.fillStyle=rec.color; ctx.fillRect(60, y, 960, 140);
  ctx.fillStyle="black"; ctx.font="bold 50px sans-serif"; ctx.fillText(rec.text, 90, y+85);
  ctx.font="28px sans-serif"; ctx.fillText("Total Score: "+total+" / 24", 600, y+85);

  document.getElementById('download').classList.remove('hidden');
  document.getElementById('download').href = canvas.toDataURL();
  document.getElementById('download').download = ticker+"_analysis.png";
}
</script>
</body>
</html>
