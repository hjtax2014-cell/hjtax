<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="더이안 세무">
<meta name="theme-color" content="#0f1729">
<meta name="description" content="더이안 세무회계 - 급여 실수령액, 양도소득세, 증여세, 상속세 계산기">
<title>더이안 세무회계</title>
<link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIuuNlOydtOyViCDshLjrrLTtmozsgywiLAogICJzaG9ydF9uYW1lIjogIuuNlOydtOyIhOyEuCIsCiAgImRpc3BsYXkiOiAic3RhbmRhbG9uZSIsCiAgImJhY2tncm91bmRfY29sb3IiOiAiIzBmMTcyOSIsCiAgInRoZW1lX2NvbG9yIjogIiMwZjE3MjkiLAogICJzdGFydF91cmwiOiAiLiIsCiAgImljb25zIjogW10KfQ==">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;600;700;900&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
<style>
:root {
  --bg-deep: #0f1729;
  --bg-card: #1a2332;
  --bg-input: #0d1420;
  --border: #2a3a4e;
  --accent: #4facfe;
  --accent2: #00f2fe;
  --gold: #f6c343;
  --text: #e8edf5;
  --text-dim: #6b7d99;
  --text-muted: #3d5068;
  --green: #36d399;
  --red: #f87272;
  --tab-h: 68px;
  --safe-b: env(safe-area-inset-bottom, 0px);
}
* { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
html { font-size:16px; }
body {
  font-family:'Noto Sans KR',-apple-system,sans-serif;
  background:var(--bg-deep);
  color:var(--text);
  min-height:100vh;
  overflow-x:hidden;
  padding-bottom:calc(var(--tab-h) + var(--safe-b) + 16px);
}

/* ── 헤더 ── */
.header {
  text-align:center;
  padding:48px 20px 20px;
  position:relative;
  overflow:hidden;
}
.header::before {
  content:'';
  position:absolute; top:-80px; left:50%; transform:translateX(-50%);
  width:400px; height:400px;
  background:radial-gradient(circle, rgba(79,172,254,0.08) 0%, transparent 70%);
  pointer-events:none;
}
.header-badge {
  display:inline-flex; align-items:center; gap:8px;
  background:linear-gradient(135deg, rgba(79,172,254,0.12), rgba(0,242,254,0.06));
  border:1px solid rgba(79,172,254,0.2);
  padding:6px 18px; border-radius:30px;
  margin-bottom:6px;
}
.header-badge span.icon { font-size:20px; }
.header-badge span.name {
  font-family:'Playfair Display',serif;
  font-size:18px; font-weight:900; color:#fff;
  letter-spacing:0.5px;
}
.header-sub {
  font-size:10px; color:var(--text-muted);
  letter-spacing:3px; text-transform:uppercase;
}

/* ── 탭 내비게이션 ── */
.tab-bar {
  position:fixed; bottom:0; left:0; right:0; z-index:100;
  display:flex; justify-content:center; gap:2px;
  background:linear-gradient(180deg, rgba(15,23,41,0.92), rgba(15,23,41,0.98));
  backdrop-filter:blur(20px); -webkit-backdrop-filter:blur(20px);
  border-top:1px solid rgba(79,172,254,0.1);
  padding:6px 8px calc(6px + var(--safe-b));
  height:calc(var(--tab-h) + var(--safe-b));
}
.tab-btn {
  flex:1; max-width:76px;
  display:flex; flex-direction:column; align-items:center; gap:3px;
  background:none; border:none; cursor:pointer;
  padding:6px 4px; border-radius:12px;
  transition:all 0.25s;
  position:relative;
}
.tab-btn.active { background:rgba(79,172,254,0.1); }
.tab-btn.active::before {
  content:''; position:absolute; top:-1px; left:50%; transform:translateX(-50%);
  width:24px; height:2px; border-radius:2px;
  background:linear-gradient(90deg, var(--accent), var(--accent2));
}
.tab-btn .tab-icon { font-size:20px; transition:transform 0.2s; }
.tab-btn.active .tab-icon { transform:scale(1.15); }
.tab-btn .tab-label {
  font-size:9px; font-weight:600; color:var(--text-muted);
  white-space:nowrap; transition:color 0.2s;
}
.tab-btn.active .tab-label { color:var(--accent); }

/* ── 계산기 카드 ── */
.calc-wrap {
  max-width:440px; margin:0 auto; padding:0 16px;
}
.calc-card {
  background:var(--bg-card);
  border:1px solid var(--border);
  border-radius:20px;
  padding:24px 20px;
  position:relative;
  overflow:hidden;
  animation:slideUp 0.4s ease;
}
@keyframes slideUp {
  from { opacity:0; transform:translateY(20px); }
  to { opacity:1; transform:translateY(0); }
}
.calc-card::before {
  content:''; position:absolute; top:0; left:0; right:0; height:3px;
  background:linear-gradient(90deg, var(--accent), var(--accent2));
}
.calc-title {
  font-size:15px; font-weight:700; color:var(--accent);
  margin-bottom:20px; display:flex; align-items:center; gap:8px;
}

/* ── 입력 필드 ── */
.field { margin-bottom:14px; }
.field-label {
  display:block; font-size:11px; font-weight:600;
  color:var(--text-dim); margin-bottom:4px;
  letter-spacing:0.5px;
}
.field-hint { font-size:10px; color:var(--text-muted); margin-top:2px; }
.input-wrap {
  display:flex; align-items:center; gap:6px;
}
.input-wrap input {
  flex:1; background:var(--bg-input);
  border:1.5px solid var(--border);
  border-radius:10px; padding:10px 12px;
  font-size:16px; color:var(--text);
  font-family:inherit; outline:none;
  transition:border-color 0.2s;
}
.input-wrap input:focus { border-color:var(--accent); }
.input-wrap .suffix {
  font-size:12px; color:var(--text-muted); min-width:16px;
}

/* ── 토글 ── */
.toggle-row {
  display:flex; align-items:center; gap:10px;
  margin-bottom:10px; cursor:pointer;
  font-size:13px; color:var(--text-dim);
}
.toggle-track {
  width:40px; height:22px; border-radius:11px;
  background:var(--border); position:relative;
  transition:background 0.2s; flex-shrink:0;
}
.toggle-track.on { background:var(--accent); }
.toggle-knob {
  width:18px; height:18px; border-radius:9px;
  background:#fff; position:absolute; top:2px; left:2px;
  transition:left 0.2s; box-shadow:0 1px 4px rgba(0,0,0,0.3);
}
.toggle-track.on .toggle-knob { left:20px; }

/* ── 관계 선택 칩 ── */
.chip-group { display:flex; flex-wrap:wrap; gap:6px; margin-bottom:4px; }
.chip {
  padding:5px 12px; font-size:11px; border-radius:20px;
  border:1px solid var(--border); background:transparent;
  color:var(--text-dim); cursor:pointer;
  transition:all 0.15s; font-family:inherit;
}
.chip.active {
  border-color:var(--accent); background:rgba(79,172,254,0.12);
  color:var(--accent); font-weight:600;
}

/* ── 결과 영역 ── */
.results { margin-top:20px; }
.results-header {
  font-size:10px; font-weight:700; color:var(--text-muted);
  letter-spacing:2px; text-transform:uppercase;
  margin-bottom:10px;
  padding-bottom:6px;
  border-bottom:1px solid var(--border);
}
.result-row {
  display:flex; justify-content:space-between; align-items:center;
  padding:7px 0;
  border-bottom:1px solid rgba(42,58,78,0.4);
}
.result-row.sub { padding-left:12px; }
.result-row .r-label {
  font-size:12px; color:var(--text-dim); font-weight:400;
}
.result-row.sub .r-label { font-size:11px; color:var(--text-muted); }
.result-row.bold .r-label { font-weight:600; color:var(--text-dim); }
.result-row .r-value {
  font-size:13px; font-weight:500; color:var(--text);
  font-variant-numeric:tabular-nums;
}
.result-row.sub .r-value { font-size:12px; color:var(--text-dim); }
.result-row.bold .r-value { font-weight:700; }

/* ── 하이라이트 결과 ── */
.result-highlight {
  display:flex; justify-content:space-between; align-items:center;
  margin:12px -20px 0; padding:14px 20px;
  background:linear-gradient(135deg, rgba(79,172,254,0.1), rgba(0,242,254,0.05));
  border-top:1px solid rgba(79,172,254,0.15);
}
.result-highlight .r-label {
  font-size:13px; font-weight:600; color:var(--accent);
}
.result-highlight .r-value {
  font-size:20px; font-weight:900; color:#fff;
  text-shadow:0 0 20px rgba(79,172,254,0.3);
}

/* ── 푸터 ── */
.footer {
  text-align:center; margin-top:24px; padding:0 20px;
  font-size:10px; color:var(--text-muted); line-height:1.7;
}

/* ── 안내 배너 ── */
.install-banner {
  max-width:440px; margin:16px auto 0; padding:0 16px;
}
.install-banner-inner {
  background:linear-gradient(135deg, rgba(246,195,67,0.08), rgba(246,195,67,0.03));
  border:1px solid rgba(246,195,67,0.15);
  border-radius:14px; padding:14px 16px;
  display:flex; align-items:flex-start; gap:10px;
  font-size:11px; color:var(--text-dim); line-height:1.5;
}
.install-banner-inner .ib-icon { font-size:18px; flex-shrink:0; margin-top:1px; }
.install-banner-inner strong { color:var(--gold); }
</style>
</head>
<body>

<div id="app"></div>

<script>
// ══════════════════════════════════════════
// 더이안 세무회계 PWA - 계산 엔진
// ══════════════════════════════════════════

// ── 간이세액표 (2024.2월 개정, 부양가족 1인) ──
const TT=[[1060000,0],[1120000,0],[1180000,0],[1240000,0],[1300000,0],[1360000,0],[1420000,0],[1480000,0],[1540000,0],[1600000,0],[1660000,0],[1720000,0],[1780000,0],[1840000,0],[1900000,0],[1960000,0],[2020000,0],[2080000,0],[2140000,0],[2200000,0],[2260000,7360],[2320000,12200],[2380000,17040],[2440000,21870],[2500000,26710],[2560000,31540],[2620000,36380],[2680000,41210],[2740000,46050],[2800000,50880],[2860000,55720],[2920000,60550],[2980000,65390],[3040000,70220],[3100000,75060],[3160000,79890],[3220000,84730],[3280000,89560],[3340000,94400],[3400000,99230],[3460000,104070],[3520000,108710],[3580000,113340],[3640000,117980],[3700000,122620],[3760000,127250],[3820000,131890],[3880000,136530],[3940000,141160],[4000000,145800],[4060000,150440],[4120000,155070],[4180000,159710],[4240000,164350],[4300000,168980],[4360000,173620],[4420000,178260],[4480000,182890],[4540000,187530],[4600000,192170],[4660000,196800],[4720000,201440],[4780000,206080],[4840000,210710],[4900000,215350],[4960000,219990],[5020000,224620],[5080000,229260],[5140000,233900],[5200000,238530],[5260000,243170],[5320000,247810],[5380000,252440],[5440000,257080],[5500000,261720],[5560000,266350],[5620000,270990],[5680000,275630],[5740000,280260],[5800000,284900],[5860000,289540],[5920000,294170],[5980000,298810],[6040000,303450],[6100000,308080],[6160000,312720],[6220000,317360],[6280000,321990],[6340000,326630],[6400000,331270],[6460000,335900],[6520000,340540],[6580000,345180],[6640000,349810],[6700000,354450],[6760000,359090],[6820000,363720],[6880000,368360],[6940000,373000],[7000000,377630],[7060000,384490],[7120000,391350],[7180000,398220],[7240000,405080],[7300000,411940],[7360000,418810],[7420000,425670],[7480000,432530],[7540000,439400],[7600000,446260],[7660000,453120],[7720000,459990],[7780000,466850],[7840000,473710],[7900000,480580],[7960000,487440],[8020000,494300],[8080000,501170],[8140000,508030],[8200000,514890],[8260000,521760],[8320000,528620],[8380000,535480],[8440000,542350],[8500000,549210],[8560000,556070],[8620000,562940],[8680000,569800],[8740000,576660],[8800000,583530],[8860000,590390],[8920000,597250],[8980000,604120],[9040000,610980],[9100000,617840],[9160000,624710],[9220000,631570],[9280000,638430],[9340000,645300],[9400000,652160],[9460000,659020],[9520000,665890],[9580000,672750],[9640000,679610],[9700000,686480],[9760000,693340],[9820000,700200],[9880000,707070],[9940000,713930],[10000000,720790]];

function lookupTax(g){
  if(g<1060000)return 0;
  if(g>=10000000)return 720790+Math.floor((g-10000000)*0.38*0.98);
  let t=0;for(let i=TT.length-1;i>=0;i--){if(g>=TT[i][0]){t=TT[i][1];break;}}return t;
}

const R={p:0.0475,h:0.03595,l:0.1314,e:0.009};
const PM=6170000;

function ins4(g){
  const p=Math.floor(Math.min(g,PM)*R.p), h=Math.floor(g*R.h), l=Math.floor(h*R.l), e=Math.floor(g*R.e);
  return {p,h,l,e,t:p+h+l+e};
}

function g2n(g){
  const i=ins4(g), tx=lookupTax(g), lt=Math.floor(tx*0.1), td=i.t+tx+lt;
  return {gross:g,net:g-td,...i,tx,lt,td};
}

function n2g(target){
  let guess=target/(1-R.p-R.h-R.h*R.l-R.e);
  for(let i=0;i<20;i++){const r=g2n(Math.round(guess));const d=target-r.net;if(Math.abs(d)<=1)break;guess+=d;}
  return g2n(Math.round(guess));
}

function calcTransfer(sp,ap,ex,yr,multi,reg){
  const gain=sp-ap-ex;
  if(gain<=0)return{gain,tg:0,tb:0,dr:0,dd:0,rate:0,sc:0,tax:0,lt2:0,total:0};
  let dr=0;
  if(!multi&&yr>=3)dr=Math.min(yr*0.02,0.30);
  const dd=Math.floor(gain*dr), tg=gain-dd, tb=Math.max(tg-2500000,0);
  let rate,cd;
  if(tb<=14e6){rate=0.06;cd=0;}else if(tb<=5e7){rate=0.15;cd=1260000;}
  else if(tb<=88e6){rate=0.24;cd=5760000;}else if(tb<=15e7){rate=0.35;cd=15440000;}
  else if(tb<=3e8){rate=0.38;cd=19940000;}else if(tb<=5e8){rate=0.40;cd=25940000;}
  else if(tb<=1e9){rate=0.42;cd=35940000;}else{rate=0.45;cd=65940000;}
  let tax=Math.floor(tb*rate-cd), sc=0;
  if(multi&&reg){sc=Math.floor(tb*0.20);tax+=sc;}
  const lt2=Math.floor(tax*0.1);
  return{gain,dr,dd,tg,tb,rate,sc,tax,lt2,total:tax+lt2};
}

const GIFT_EX=[{l:'배우자',a:6e8},{l:'직계존속→성년',a:5e7},{l:'직계존속→미성년',a:2e7},{l:'직계비속',a:5e7},{l:'기타친족',a:1e7},{l:'타인',a:0}];

function calcGift(v,ex){
  const tb=Math.max(v-ex,0);
  let rate,cd;
  if(tb<=1e8){rate=0.10;cd=0;}else if(tb<=5e8){rate=0.20;cd=1e7;}
  else if(tb<=1e9){rate=0.30;cd=6e7;}else if(tb<=3e9){rate=0.40;cd=16e7;}
  else{rate=0.50;cd=46e7;}
  return{tb,rate,tax:Math.max(Math.floor(tb*rate-cd),0)};
}

function calcInherit(est,debt,fun,ded){
  const ne=Math.max(est-debt-fun,0), tb=Math.max(ne-ded,0);
  let rate,cd;
  if(tb<=1e8){rate=0.10;cd=0;}else if(tb<=5e8){rate=0.20;cd=1e7;}
  else if(tb<=1e9){rate=0.30;cd=6e7;}else if(tb<=3e9){rate=0.40;cd=16e7;}
  else{rate=0.50;cd=46e7;}
  return{ne,ded,tb,rate,tax:Math.max(Math.floor(tb*rate-cd),0)};
}

// ══════════════════════════════════════════
// UI 렌더링
// ══════════════════════════════════════════
const fmt=n=>(n||0).toLocaleString('ko-KR');
const fmtW=n=>fmt(n)+'원';

const TABS=[
  {id:'fwd',label:'급여→실수령',icon:'💵'},
  {id:'rev',label:'실수령→세전',icon:'🔄'},
  {id:'tr',label:'양도소득세',icon:'🏠'},
  {id:'gift',label:'증여세',icon:'🎁'},
  {id:'inh',label:'상속세',icon:'📜'}
];

let state={
  tab:'fwd',
  fwd:{gross:4054357},
  rev:{net:3500000},
  tr:{sp:900000000,ap:600000000,ex:10000000,yr:5,multi:false,reg:false},
  gift:{val:500000000,ei:1},
  inh:{est:2000000000,debt:100000000,fun:10000000,ded:500000000}
};

function render(){
  const app=document.getElementById('app');
  app.innerHTML=`
    ${renderHeader()}
    <div class="calc-wrap">${renderCalc()}</div>
    ${renderInstallBanner()}
    ${renderFooter()}
    ${renderTabBar()}
  `;
  bindEvents();
}

function renderHeader(){
  return `<div class="header">
    <div class="header-badge">
      <span class="icon">⚖️</span>
      <span class="name">THE IAN</span>
    </div>
    <div class="header-sub">Tax & Accounting</div>
  </div>`;
}

function renderTabBar(){
  return `<div class="tab-bar">
    ${TABS.map(t=>`<button class="tab-btn${state.tab===t.id?' active':''}" data-tab="${t.id}">
      <span class="tab-icon">${t.icon}</span>
      <span class="tab-label">${t.label}</span>
    </button>`).join('')}
  </div>`;
}

function field(id,label,val,suffix='원',hint=''){
  return `<div class="field">
    <label class="field-label">${label}</label>
    <div class="input-wrap">
      <input type="text" inputmode="numeric" data-id="${id}" value="${val?fmt(val):''}" placeholder="0">
      <span class="suffix">${suffix}</span>
    </div>
    ${hint?`<div class="field-hint">${hint}</div>`:''}
  </div>`;
}

function toggle(id,label,checked){
  return `<div class="toggle-row" data-toggle="${id}">
    <div class="toggle-track${checked?' on':''}"><div class="toggle-knob"></div></div>
    <span>${label}</span>
  </div>`;
}

function row(label,value,cls=''){
  return `<div class="result-row ${cls}"><span class="r-label">${label}</span><span class="r-value">${value}</span></div>`;
}
function hl(label,value){
  return `<div class="result-highlight"><span class="r-label">${label}</span><span class="r-value">${value}</span></div>`;
}

function renderCalc(){
  const t=state.tab;
  let title='', body='';
  
  if(t==='fwd'){
    const s=state.fwd, r=g2n(s.gross);
    title='💵 세전급여 → 실수령액';
    body=field('fwd.gross','세전급여 (월)',s.gross)+`
    <div class="results">
      <div class="results-header">공제 내역</div>
      ${row('국민연금 (4.75%)',fmtW(r.p),'sub')}
      ${row('건강보험 (3.595%)',fmtW(r.h),'sub')}
      ${row('장기요양보험 (13.14%)',fmtW(r.l),'sub')}
      ${row('고용보험 (0.9%)',fmtW(r.e),'sub')}
      ${row('소득세',fmtW(r.tx),'sub')}
      ${row('지방소득세 (10%)',fmtW(r.lt),'sub')}
      ${row('공제합계',fmtW(r.td),'bold')}
      ${hl('✅ 실수령액',fmtW(r.net))}
    </div>`;
  }

  if(t==='rev'){
    const s=state.rev, r=n2g(s.net);
    title='🔄 실수령액 → 세전급여 역산';
    body=field('rev.net','희망 실수령액 (월)',s.net)+`
    <div class="results">
      <div class="results-header">역산 결과</div>
      ${hl('필요 세전급여',fmtW(r.gross))}
      <div style="height:8px"></div>
      ${row('국민연금',fmtW(r.p),'sub')}
      ${row('건강보험',fmtW(r.h),'sub')}
      ${row('장기요양보험',fmtW(r.l),'sub')}
      ${row('고용보험',fmtW(r.e),'sub')}
      ${row('소득세',fmtW(r.tx),'sub')}
      ${row('지방소득세',fmtW(r.lt),'sub')}
      ${row('공제합계',fmtW(r.td),'bold')}
      ${hl('✅ 검증 실수령액',fmtW(r.net))}
    </div>`;
  }

  if(t==='tr'){
    const s=state.tr, r=calcTransfer(s.sp,s.ap,s.ex,s.yr,s.multi,s.reg);
    title='🏠 양도소득세 계산기';
    body=field('tr.sp','양도가액',s.sp)+field('tr.ap','취득가액',s.ap)
      +field('tr.ex','필요경비 (취득세·중개보수 등)',s.ex)
      +field('tr.yr','보유기간',s.yr,'년')
      +toggle('tr.multi','다주택자',s.multi)
      +(s.multi?toggle('tr.reg','조정대상지역',s.reg):'')+`
    <div class="results">
      <div class="results-header">세액 산출</div>
      ${row('양도차익',fmtW(r.gain),'bold')}
      ${r.dr>0?row(`장기보유특별공제 (${(r.dr*100).toFixed(0)}%)`,fmtW(r.dd),'sub'):''}
      ${row('양도소득금액',fmtW(r.tg),'sub')}
      ${row('기본공제',fmtW(2500000),'sub')}
      ${row('과세표준',fmtW(r.tb),'sub')}
      ${row(`세율 (${(r.rate*100).toFixed(0)}%)`,fmtW(r.tax-r.sc),'sub')}
      ${r.sc>0?row('다주택 중과세',fmtW(r.sc),'sub'):''}
      ${row('양도소득세',fmtW(r.tax),'bold')}
      ${row('지방소득세',fmtW(r.lt2),'sub')}
      ${hl('💰 총 납부세액',fmtW(r.total))}
    </div>`;
  }

  if(t==='gift'){
    const s=state.gift, ex=GIFT_EX[s.ei], r=calcGift(s.val,ex.a);
    title='🎁 증여세 계산기';
    body=field('gift.val','증여재산가액',s.val)+`
    <div class="field">
      <label class="field-label">증여자 ↔ 수증자 관계</label>
      <div class="chip-group">
        ${GIFT_EX.map((e,i)=>`<button class="chip${s.ei===i?' active':''}" data-chip="${i}">${e.l}</button>`).join('')}
      </div>
      <div class="field-hint">면제한도: ${fmtW(ex.a)} (10년 합산)</div>
    </div>
    <div class="results">
      <div class="results-header">세액 산출</div>
      ${row('증여재산가액',fmtW(s.val))}
      ${row('증여재산공제',fmtW(ex.a),'sub')}
      ${row('과세표준',fmtW(r.tb),'bold')}
      ${row(`세율 (${(r.rate*100).toFixed(0)}%)`,'','sub')}
      ${hl('💰 증여세 산출세액',fmtW(r.tax))}
    </div>`;
  }

  if(t==='inh'){
    const s=state.inh, r=calcInherit(s.est,s.debt,s.fun,s.ded);
    title='📜 상속세 계산기';
    body=field('inh.est','총 상속재산가액',s.est)
      +field('inh.debt','채무액',s.debt)
      +field('inh.fun','장례비용',s.fun,'원','최대 1,500만원 한도')
      +field('inh.ded','공제액 (일괄공제)',s.ded,'원','기본 5억원 (배우자공제 별도)')+`
    <div class="results">
      <div class="results-header">세액 산출</div>
      ${row('총 상속재산',fmtW(s.est))}
      ${row('채무·장례비용',fmtW(s.debt+s.fun),'sub')}
      ${row('순상속재산',fmtW(r.ne),'bold')}
      ${row('공제액',fmtW(r.ded),'sub')}
      ${row('과세표준',fmtW(r.tb),'bold')}
      ${row(`세율 (${(r.rate*100).toFixed(0)}%)`,'','sub')}
      ${hl('💰 상속세 산출세액',fmtW(r.tax))}
    </div>`;
  }

  return `<div class="calc-card" key="${t}">
    <div class="calc-title">${title}</div>
    ${body}
  </div>`;
}

function renderInstallBanner(){
  return `<div class="install-banner"><div class="install-banner-inner">
    <span class="ib-icon">📲</span>
    <div><strong>홈 화면에 추가</strong>하면 앱처럼 사용할 수 있습니다.<br>
    iOS: 공유 버튼 → "홈 화면에 추가"<br>
    Android: 메뉴(⋮) → "홈 화면에 추가"</div>
  </div></div>`;
}

function renderFooter(){
  return `<div class="footer">
    <p>2026년 세법 기준 · 간이세액표(2024.2월 개정) 적용</p>
    <p>실제 세무 판단은 반드시 세무전문가와 상담하세요</p>
    <p style="margin-top:4px">© 더이안 세무회계 · AI Tax Solutions</p>
  </div>`;
}

function bindEvents(){
  // 탭 전환
  document.querySelectorAll('.tab-btn').forEach(btn=>{
    btn.addEventListener('click',()=>{
      state.tab=btn.dataset.tab;
      render();
    });
  });

  // 숫자 입력
  document.querySelectorAll('input[data-id]').forEach(inp=>{
    inp.addEventListener('input',e=>{
      const raw=e.target.value.replace(/[^0-9]/g,'');
      const num=raw?parseInt(raw,10):0;
      const [section,key]=e.target.dataset.id.split('.');
      state[section][key]=num;
      // 커서 위치 보존
      const pos=e.target.selectionStart;
      const oldLen=e.target.value.length;
      e.target.value=num?fmt(num):'';
      const newLen=e.target.value.length;
      const newPos=Math.max(0,pos+(newLen-oldLen));
      e.target.setSelectionRange(newPos,newPos);
      // 결과만 업데이트
      updateResults();
    });
  });

  // 토글
  document.querySelectorAll('.toggle-row').forEach(el=>{
    el.addEventListener('click',()=>{
      const [section,key]=el.dataset.toggle.split('.');
      state[section][key]=!state[section][key];
      render();
    });
  });

  // 칩 (증여 관계)
  document.querySelectorAll('.chip').forEach(el=>{
    el.addEventListener('click',()=>{
      state.gift.ei=parseInt(el.dataset.chip,10);
      render();
    });
  });
}

function updateResults(){
  // 결과만 빠르게 갱신 (전체 리렌더 대신)
  const wrap=document.querySelector('.calc-wrap');
  if(wrap) wrap.innerHTML=renderCalc();
  // 이벤트 재바인딩 (입력 필드)
  document.querySelectorAll('input[data-id]').forEach(inp=>{
    inp.addEventListener('input',e=>{
      const raw=e.target.value.replace(/[^0-9]/g,'');
      const num=raw?parseInt(raw,10):0;
      const [section,key]=e.target.dataset.id.split('.');
      state[section][key]=num;
      const pos=e.target.selectionStart;
      const oldLen=e.target.value.length;
      e.target.value=num?fmt(num):'';
      const newLen=e.target.value.length;
      e.target.setSelectionRange(Math.max(0,pos+(newLen-oldLen)),Math.max(0,pos+(newLen-oldLen)));
      updateResults();
    });
  });
  document.querySelectorAll('.toggle-row').forEach(el=>{
    el.addEventListener('click',()=>{
      const [s,k]=el.dataset.toggle.split('.');
      state[s][k]=!state[s][k]; render();
    });
  });
  document.querySelectorAll('.chip').forEach(el=>{
    el.addEventListener('click',()=>{
      state.gift.ei=parseInt(el.dataset.chip,10); render();
    });
  });
}

// 초기 렌더
render();
</script>
</body>
</html>
