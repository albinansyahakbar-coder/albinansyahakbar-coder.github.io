<!DOCTYPE html>
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SinyalPro — Sinyal Trading Live</title>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#070b12;
    --panel:rgba(17,24,39,.72);
    --line:rgba(255,255,255,.07);
    --green:#00e5a0;
    --red:#ff4d6d;
    --blue:#5b8cff;
    --amber:#ffb340;
    --txt:#e6edf7;
    --muted:#7b8798;
  }
  body{
    font-family:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;
    background:var(--bg);
    color:var(--txt);
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
    background-image:
      radial-gradient(circle at 15% -5%, rgba(0,229,160,.10), transparent 45%),
      radial-gradient(circle at 88% 5%, rgba(91,140,255,.12), transparent 45%);
    background-attachment:fixed;
  }
  .wrap{max-width:1460px;margin:0 auto;padding:18px 18px 40px}

  /* ---------- HEADER ---------- */
  header{
    display:flex;justify-content:space-between;align-items:center;
    gap:16px;flex-wrap:wrap;margin-bottom:18px;
  }
  .brand{display:flex;align-items:center;gap:12px}
  .logo{
    width:42px;height:42px;border-radius:12px;
    background:linear-gradient(135deg,#00e5a0,#5b8cff);
    display:grid;place-items:center;font-size:20px;
    box-shadow:0 8px 24px rgba(0,229,160,.28);
  }
  .brand h1{font-size:18px;font-weight:700;letter-spacing:-.3px}
  .brand p{font-size:11px;color:var(--muted);letter-spacing:.6px;text-transform:uppercase}
  .status{
    display:flex;align-items:center;gap:8px;
    background:var(--panel);border:1px solid var(--line);
    padding:9px 15px;border-radius:999px;font-size:12.5px;
    backdrop-filter:blur(12px);
  }
  .dot{
    width:8px;height:8px;border-radius:50%;background:var(--green);
    box-shadow:0 0 0 0 rgba(0,229,160,.7);animation:pulse 1.8s infinite;
  }
  @keyframes pulse{
    0%{box-shadow:0 0 0 0 rgba(0,229,160,.65)}
    70%{box-shadow:0 0 0 9px rgba(0,229,160,0)}
    100%{box-shadow:0 0 0 0 rgba(0,229,160,0)}
  }
  .clock{font-variant-numeric:tabular-nums;color:var(--muted)}

  /* ---------- LAYOUT ---------- */
  main{display:grid;grid-template-columns:minmax(0,1fr) 372px;gap:16px}
  @media(max-width:1080px){main{grid-template-columns:1fr}}

  .card{
    background:var(--panel);
    border:1px solid var(--line);
    border-radius:16px;
    backdrop-filter:blur(14px);
    box-shadow:0 18px 44px -22px rgba(0,0,0,.9);
  }

  /* ---------- CHART PANEL ---------- */
  .chart-panel{display:flex;flex-direction:column;height:600px;overflow:hidden}
  @media(max-width:1080px){.chart-panel{height:520px}}

  .tabs{
    display:flex;gap:6px;padding:12px 14px;overflow-x:auto;
    border-bottom:1px solid var(--line);scrollbar-width:none;
  }
  .tabs::-webkit-scrollbar{display:none}
  .tab{
    background:transparent;border:1px solid transparent;color:var(--muted);
    padding:7px 14px;border-radius:9px;font-size:12.5px;font-weight:600;
    cursor:pointer;white-space:nowrap;transition:.18s;font-family:inherit;
    display:flex;align-items:center;gap:7px;
  }
  .tab:hover{color:var(--txt);background:rgba(255,255,255,.04)}
  .tab.active{
    color:#03130e;background:linear-gradient(135deg,#00e5a0,#27d3ff);
    box-shadow:0 6px 18px -6px rgba(0,229,160,.7);
  }
  .tab .chg{font-size:10.5px;opacity:.85}

  .price-head{
    display:flex;align-items:flex-end;gap:16px;
    padding:14px 20px 6px;flex-wrap:wrap;
  }
  .price-head .sym{font-size:13px;color:var(--muted);font-weight:600;letter-spacing:.4px}
  .price{
    font-size:34px;font-weight:700;letter-spacing:-1px;
    font-variant-numeric:tabular-nums;line-height:1;
    transition:color .25s;
  }
  .price.up{color:var(--green)}
  .price.down{color:var(--red)}
  .chg-pill{
    font-size:12.5px;font-weight:700;padding:5px 11px;border-radius:8px;
    font-variant-numeric:tabular-nums;
  }
  .chg-pill.up{background:rgba(0,229,160,.13);color:var(--green)}
  .chg-pill.down{background:rgba(255,77,109,.13);color:var(--red)}

  .legend{
    display:flex;gap:16px;padding:2px 20px 8px;font-size:11.5px;color:var(--muted);
    flex-wrap:wrap;
  }
  .legend span{display:flex;align-items:center;gap:6px}
  .legend i{width:14px;height:2.5px;border-radius:2px;display:block}

  .chart-wrap{position:relative;flex:1;min-height:260px;margin:0 6px 6px}
  canvas{position:absolute;inset:0;width:100%;height:100%;display:block}

  /* ---------- SIDE ---------- */
  .side{display:flex;flex-direction:column;gap:16px}

  .signal-card{padding:20px;position:relative;overflow:hidden}
  .signal-card::before{
    content:'';position:absolute;inset:0;opacity:.14;pointer-events:none;
    background:radial-gradient(circle at 50% 0%, var(--glow,#00e5a0), transparent 70%);
    transition:.4s;
  }
  .sig-label{font-size:11px;letter-spacing:1.4px;text-transform:uppercase;color:var(--muted);margin-bottom:10px}
  .sig-main{
    font-size:44px;font-weight:800;letter-spacing:-2px;line-height:1;
    margin-bottom:6px;transition:.3s;
  }
  .sig-main.buy{color:var(--green);text-shadow:0 0 34px rgba(0,229,160,.55)}
  .sig-main.sell{color:var(--red);text-shadow:0 0 34px rgba(255,77,109,.55)}
  .sig-main.neutral{color:var(--muted);text-shadow:none}
  .sig-sub{font-size:12.5px;color:var(--muted);margin-bottom:16px}

  .meter{height:6px;border-radius:99px;background:rgba(255,255,255,.07);overflow:hidden;margin-bottom:6px}
  .meter-fill{height:100%;border-radius:99px;transition:width .5s cubic-bezier(.4,0,.2,1),background .4s}
  .meter-txt{display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:18px}

  .levels{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px}
  .lvl{
    background:rgba(255,255,255,.035);border:1px solid var(--line);
    border-radius:11px;padding:10px 8px;text-align:center;
  }
  .lvl small{display:block;font-size:10px;letter-spacing:.9px;color:var(--muted);text-transform:uppercase;margin-bottom:5px}
  .lvl b{font-size:13.5px;font-variant-numeric:tabular-nums;font-weight:700}
  .lvl.entry b{color:#dbe6f5}
  .lvl.tp b{color:var(--green)}
  .lvl.sl b{color:var(--red)}

  /* indicators */
  .ind-card{padding:16px 18px}
  .ind-title{font-size:11px;letter-spacing:1.3px;text-transform:uppercase;color:var(--muted);margin-bottom:14px}
  .ind-row{display:flex;justify-content:space-between;align-items:center;padding:9px 0;border-bottom:1px solid rgba(255,255,255,.045);font-size:13px}
  .ind-row:last-child{border-bottom:none;padding-bottom:0}
  .ind-row span:first-child{color:var(--muted)}
  .ind-row b{font-variant-numeric:tabular-nums;font-weight:600}
  .badge{font-size:10.5px;font-weight:700;padding:3px 9px;border-radius:6px;letter-spacing:.4px}
  .badge.g{background:rgba(0,229,160,.14);color:var(--green)}
  .badge.r{background:rgba(255,77,109,.14);color:var(--red)}
  .badge.n{background:rgba(255,255,255,.07);color:var(--muted)}
  .badge.a{background:rgba(255,179,64,.14);color:var(--amber)}

  /* history */
  .hist-card{padding:16px 18px;flex:1;display:flex;flex-direction:column;min-height:0}
  .hist-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
  .hist-title{font-size:11px;letter-spacing:1.3px;text-transform:uppercase;color:var(--muted)}
  .winrate{font-size:11.5px;color:var(--muted)}
  .winrate b{color:var(--green);font-variant-numeric:tabular-nums}
  .hist-list{overflow-y:auto;max-height:250px;display:flex;flex-direction:column;gap:7px;padding-right:4px}
  .hist-list::-webkit-scrollbar{width:5px}
  .hist-list::-webkit-scrollbar-thumb{background:rgba(255,255,255,.1);border-radius:9px}
  .hist-item{
    display:flex;align-items:center;gap:10px;
    background:rgba(255,255,255,.032);border:1px solid var(--line);
    border-radius:10px;padding:9px 11px;font-size:12px;
    animation:slideIn .35s ease;
  }
  @keyframes slideIn{from{opacity:0;transform:translateY(-7px)}to{opacity:1;transform:none}}
  .hist-item .tag{
    font-size:10px;font-weight:800;padding:3px 7px;border-radius:5px;letter-spacing:.5px;
  }
  .tag.buy{background:rgba(0,229,160,.16);color:var(--green)}
  .tag.sell{background:rgba(255,77,109,.16);color:var(--red)}
  .hist-item .pair{font-weight:600;color:#cdd8e8;flex:1;font-size:11.5px}
  .hist-item .pr{color:var(--muted);font-variant-numeric:tabular-nums;font-size:11.5px}
  .hist-item .st{font-size:10px;font-weight:800;letter-spacing:.5px;padding:2px 6px;border-radius:5px}
  .st.tp{background:rgba(0,229,160,.16);color:var(--green)}
  .st.sl{background:rgba(255,77,109,.16);color:var(--red)}
  .st.aktif{background:rgba(255,179,64,.16);color:var(--amber)}
  .empty{text-align:center;color:var(--muted);font-size:12px;padding:22px 0}

  /* toast */
  .toast-box{position:fixed;top:18px;right:18px;display:flex;flex-direction:column;gap:9px;z-index:99}
  .toast{
    background:rgba(15,22,34,.96);border:1px solid var(--line);
    border-left:3px solid var(--green);
    border-radius:12px;padding:13px 17px;min-width:250px;
    box-shadow:0 20px 44px -12px rgba(0,0,0,.9);
    animation:toastIn .35s cubic-bezier(.2,.9,.3,1.2);
  }
  .toast.sell{border-left-color:var(--red)}
  .toast b{display:block;font-size:12.5px;margin-bottom:3px}
  .toast small{color:var(--muted);font-size:11.5px}
  @keyframes toastIn{from{opacity:0;transform:translateX(40px)}to{opacity:1;transform:none}}
  .toast.out{animation:toastOut .3s forwards}
  @keyframes toastOut{to{opacity:0;transform:translateX(40px)}}

  footer{text-align:center;color:var(--muted);font-size:11px;margin-top:22px;line-height:1.7}
</style>
</head>
<body>
<div class="wrap">

  <header>
    <div class="brand">
      <div class="logo">📈</div>
      <div>
        <h1>SinyalPro Live</h1>
        <p>Real-time Trading Signals</p>
      </div>
    </div>
    <div class="status">
      <div class="dot"></div>
      <span>Pasar Terhubung</span>
      <span class="clock" id="clock">--:--:--</span>
    </div>
  </header>

  <main>
    <!-- ============ KIRI: CHART ============ -->
    <section class="card chart-panel">
      <div class="tabs" id="tabs"></div>

      <div class="price-head">
        <div>
          <div class="sym" id="curSym">BTC/USDT</div>
        </div>
        <div class="price" id="curPrice">0.00</div>
        <div class="chg-pill up" id="curChg">+0.00%</div>
      </div>

      <div class="legend">
        <span><i style="background:#5b8cff"></i>MA 9</span>
        <span><i style="background:#ffb340"></i>MA 21</span>
        <span><i style="background:rgba(255,255,255,.28)"></i>Volume</span>
      </div>

      <div class="chart-wrap">
        <canvas id="chart"></canvas>
      </div>
    </section>

    <!-- ============ KANAN: PANEL SINYAL ============ -->
    <aside class="side">

      <div class="card signal-card" id="sigCard">
        <div class="sig-label">Sinyal Saat Ini</div>
        <div class="sig-main neutral" id="sigMain">NETRAL</div>
        <div class="sig-sub" id="sigSub">Menunggu konfirmasi indikator…</div>

        <div class="meter"><div class="meter-fill" id="meterFill" style="width:50%;background:#7b8798"></div></div>
        <div class="meter-txt"><span>Kekuatan Sinyal</span><span id="meterVal">50%</span></div>

        <div class="levels">
          <div class="lvl entry"><small>Entry</small><b id="lvEntry">—</b></div>
          <div class="lvl tp"><small>Take Profit</small><b id="lvTp">—</b></div>
          <div class="lvl sl"><small>Stop Loss</small><b id="lvSl">—</b></div>
        </div>
      </div>

      <div class="card ind-card">
        <div class="ind-title">Indikator Teknikal</div>
        <div class="ind-row"><span>MA 9 (Cepat)</span><b id="indFast">—</b></div>
        <div class="ind-row"><span>MA 21 (Lambat)</span><b id="indSlow">—</b></div>
        <div class="ind-row"><span>Tren MA</span><span class="badge n" id="indTrend">NETRAL</span></div>
        <div class="ind-row"><span>RSI (14)</span><b id="indRsi">—</b></div>
        <div class="ind-row"><span>Zona RSI</span><span class="badge n" id="indRsiZone">NORMAL</span></div>
        <div class="ind-row"><span>Momentum</span><span class="badge n" id="indMom">DATAR</span></div>
      </div>

      <div class="card hist-card">
        <div class="hist-head">
          <div class="hist-title">Riwayat Sinyal</div>
          <div class="winrate">Win rate: <b id="winRate">—</b></div>
        </div>
        <div class="hist-list" id="histList">
          <div class="empty">Belum ada sinyal terdeteksi</div>
        </div>
      </div>

    </aside>
  </main>

  <footer>
    ⚠️ Simulasi edukasi — data pergerakan harga dibangkitkan secara sintetis.<br>
    Bukan nasihat keuangan. Selalu lakukan riset Anda sendiri.
  </footer>
</div>

<div class="toast-box" id="toastBox"></div>

<script>
/* =========================================================
   1. KONFIGURASI PASAR
   ========================================================= */
const PAIRS = [
  { s:'BTC/USDT',  p:67432.50, v:0.0020, dec:2 },
  { s:'ETH/USDT',  p:3487.20,  v:0.0024, dec:2 },
  { s:'SOL/USDT',  p:172.45,   v:0.0032, dec:2 },
  { s:'XAU/USD',   p:2338.60,  v:0.0010, dec:2 },
  { s:'EUR/USD',   p:1.08650,  v:0.0006, dec:5 },
  { s:'GBP/JPY',   p:198.420,  v:0.0011, dec:3 },
];

const MAX_CANDLES   = 220;
const VISIBLE       = 70;
const TICK_MS       = 800;   // interval update
const TICKS_PER_BAR = 6;     // tick per candle baru
const MA_FAST = 9, MA_SLOW = 21, RSI_PERIOD = 14;

/* =========================================================
   2. STATE
   ========================================================= */
let activeIdx = 0;
const market = PAIRS.map(cfg => ({
  ...cfg,
  candles: [],
  tick: 0,
  lastPrice: cfg.p,
  prevClose: cfg.p,
  signals: [],
  lastCross: 0
}));

const history = [];       // semua sinyal (semua pair)
let historyRendered = 0;

/* =========================================================
   3. UTILITAS
   ========================================================= */
const clamp = (v,a,b)=>Math.max(a,Math.min(b,v));

function fmt(n, dec){
  return n.toLocaleString('en-US',{minimumFractionDigits:dec, maximumFractionDigits:dec});
}

function smaSeries(candles, period){
  const out = new Array(candles.length).fill(null);
  let sum = 0;
  for(let i=0;i<candles.length;i++){
    sum += candles[i].c;
    if(i >= period) sum -= candles[i-period].c;
    if(i >= period-1) out[i] = sum/period;
  }
  return out;
}

function rsiValue(candles, period){
  if(candles.length < period+1) return null;
  let gain = 0, loss = 0;
  for(let i = candles.length-period; i < candles.length; i++){
    const d = candles[i].c - candles[i-1].c;
    if(d > 0) gain += d; else loss -= d;
  }
  if(loss === 0) return 100;
  const rs = (gain/period) / (loss/period);
  return 100 - 100/(1+rs);
}

function atr(candles, period=14){
  let sum = 0, n = 0;
  const start = Math.max(1, candles.length-period);
  for(let i=start;i<candles.length;i++){
    const c = candles[i], pv = candles[i-1];
    sum += Math.max(c.h-c.l, Math.abs(c.h-pv.c), Math.abs(c.l-pv.c));
    n++;
  }
  return n ? sum/n : (candles[candles.length-1].c * 0.005);
}

/* =========================================================
   4. GENERATOR DATA AWAL
   ========================================================= */
function seedCandles(price, vol, n){
  const arr = [];
  let p = price;
  const now = Date.now();
  for(let i=0;i<n;i++){
    const o = p;
    const c = o * (1 + (Math.random()-0.5)*vol*2);
    const h = Math.max(o,c) * (1 + Math.random()*vol*0.7);
    const l = Math.min(o,c) * (1 - Math.random()*vol*0.7);
    arr.push({ o, h, l, c, v: 20+Math.random()*80, t: now - (n-i)*5000 });
    p = c;
  }
  return arr;
}

market.forEach(m => {
  m.candles = seedCandles(m.p, m.v, MAX_CANDLES);
  m.lastPrice = m.candles[m.candles.length-1].c;
  m.prevClose = m.lastPrice;
});

/* =========================================================
   5. SIMULASI TICK
   ========================================================= */
function tick(m){
  const last = m.candles[m.candles.length-1];
  const drift = (Math.random()-0.5) * m.v * 0.55;
  const np = last.c * (1 + drift);

  last.c = np;
  last.h = Math.max(last.h, np);
  last.l = Math.min(last.l, np);
  last.v += Math.random()*6;

  m.lastPrice = np;
  m.tick++;

  // candle baru
  if(m.tick >= TICKS_PER_BAR){
    m.tick = 0;
    m.candles.push({ o:np, h:np, l:np, c:np, v:8+Math.random()*22, t: Date.now() });
    if(m.candles.length > MAX_CANDLES) m.candles.shift();
    evaluateSignal(m);   // cek sinyal hanya saat candle close
  }

  checkOpenSignals(m, np);
}

/* =========================================================
   6. LOGIKA SINYAL
   ========================================================= */
function computeIndicators(m){
  const c = m.candles;
  const fastArr = smaSeries(c, MA_FAST);
  const slowArr = smaSeries(c, MA_SLOW);
  const i = c.length - 1;

  const fast = fastArr[i];
  const slow = slowArr[i];
  const fastPrev = fastArr[i-1];
  const slowPrev = slowArr[i-1];
  const rsi = rsiValue(c, RSI_PERIOD);
  const a = atr(c, 14);

  return { fast, slow, fastPrev, slowPrev, rsi, atr:a, fastArr, slowArr };
}

function evaluateSignal(m){
  if(m.candles.length < MA_SLOW + 5) return;

  const ind = computeIndicators(m);
  if(ind.fast == null || ind.slow == null || ind.fastPrev == null || ind.slowPrev == null) return;

  const crossUp   = ind.fastPrev <= ind.slowPrev && ind.fast > ind.slow;
  const crossDown = ind.fastPrev >= ind.slowPrev && ind.fast < ind.slow;

  if(!crossUp && !crossDown) return;

  // cooldown 25 detik per pair
  if(Date.now() - m.lastCross < 25000) return;

  const rsi = ind.rsi ?? 50;
  // filter: jangan beli saat overbought ekstrem, jangan jual saat oversold ekstrem
  if(crossUp && rsi > 78) return;
  if(crossDown && rsi < 22) return;

  const type = crossUp ? 'BUY' : 'SELL';
  const entry = m.lastPrice;
  const dist = Math.max(ind.atr * 1.4, entry * 0.0012);

  const sig = {
    id: Date.now() + Math.random(),
    pair: m.s,
    dec: m.dec,
    type,
    entry,
    tp: type === 'BUY' ? entry + dist*2.2 : entry - dist*2.2,
    sl: type === 'BUY' ? entry - dist*1.1 : entry + dist*1.1,
    status: 'AKTIF',
    time: new Date(),
    rsi
  };

  m.lastCross = Date.now();
  m.signals.push(sig);
  history.unshift(sig);
  if(history.length > 40) history.pop();

  notify(sig);
  renderHistory();
}

function checkOpenSignals(m, price){
  m.signals.forEach(s => {
    if(s.status !== 'AKTIF') return;
    if(s.type === 'BUY'){
      if(price >= s.tp) s.status = 'TP';
      else if(price <= s.sl) s.status = 'SL';
    } else {
      if(price <= s.tp) s.status = 'TP';
      else if(price >= s.sl) s.status = 'SL';
    }
  });
}

/* =========================================================
   7. RENDER — HEADER HARGA & TABS
   ========================================================= */
function renderTabs(){
  const box = document.getElementById('tabs');
  box.innerHTML = '';
  market.forEach((m, i) => {
    const chg = ((m.lastPrice - m.prevClose)/m.prevClose*100);
    const btn = document.createElement('button');
    btn.className = 'tab' + (i === activeIdx ? ' active' : '');
    btn.innerHTML = `${m.s} <span class="chg" style="color:${i===activeIdx?'inherit':(chg>=0?'#00e5a0':'#ff4d6d')}">${chg>=0?'+':''}${chg.toFixed(2)}%</span>`;
    btn.onclick = () => { activeIdx = i; renderTabs(); renderPriceHead(); draw(); };
    box.appendChild(btn);
  });
}

function renderPriceHead(){
  const m = market[activeIdx];
  const chg = (m.lastPrice - m.prevClose)/m.prevClose*100;
  const priceEl = document.getElementById('curPrice');

  document.getElementById('curSym').textContent = m.s;
  priceEl.textContent = fmt(m.lastPrice, m.dec);
  priceEl.className = 'price ' + (chg >= 0 ? 'up' : 'down');

  const pill = document.getElementById('curChg');
  pill.textContent = (chg>=0?'+':'') + chg.toFixed(2) + '%';
  pill.className = 'chg-pill ' + (chg >= 0 ? 'up' : 'down');
}

/* ===================================================
