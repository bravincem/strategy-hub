<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Deriv Strategy Hub — V2</title>

<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f4f6fa;color:#172033}
.top{height:64px;background:#fff;border-bottom:1px solid #e5e7eb;display:flex;align-items:center;padding:0 14px;gap:12px;position:sticky;top:0;z-index:20}
.logo{font-weight:800;font-size:18px}
.hamb{font-size:23px;cursor:pointer}
.topright{margin-left:auto;display:flex;align-items:center;gap:7px}
.status{font-size:12px;background:#eef1f6;color:#475569;padding:8px 10px;border-radius:20px;font-weight:800}
.status.live{background:#eaf8ef;color:#17803d}
.status.err{background:#fff0f0;color:#b42318}

button{
border:0;
border-radius:9px;
padding:10px 13px;
font-weight:700;
cursor:pointer
}

.dark{background:#111827;color:#fff}
.soft{background:#eef1f6;color:#172033}

.layout{
display:flex;
min-height:calc(100vh - 64px)
}

.side{
width:220px;
background:#111827;
color:#cbd5e1;
padding:14px 10px;
flex-shrink:0
}

.side h3{
color:#fff;
padding:0 10px;
margin:8px 0 16px
}

.side button{
width:100%;
text-align:left;
background:transparent;
color:#cbd5e1;
margin:2px 0
}

.side button.active,
.side button:hover{
background:#fff;
color:#111827
}

.main{
flex:1;
max-width:1120px;
margin:auto;
padding:18px;
width:100%
}

.page{display:none}
.page.active{display:block}

.cards{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:12px
}

.card{
background:#fff;
border:1px solid #e5e7eb;
border-radius:15px;
padding:16px;
box-shadow:0 2px 10px #00000008
}

.card h3{margin:0 0 7px}
.muted{color:#64748b;font-size:13px}

.metric{
font-size:23px;
font-weight:800;
margin-top:7px
}

.green{color:#16803c}
.red{color:#c62828}

.section{margin-top:16px}

.sectiontitle{
display:flex;
justify-content:space-between;
align-items:center;
margin-bottom:10px
}

.builder{
display:grid;
grid-template-columns:repeat(2,1fr);
gap:12px
}

.field{
display:flex;
flex-direction:column;
gap:6px
}

.field label{
font-size:12px;
font-weight:800
}

.field input,
.field select{
padding:11px;
border:1px solid #d7dce5;
border-radius:9px;
background:#fff
}

.actions{
display:flex;
flex-wrap:wrap;
gap:8px;
margin-top:14px
}

.notice{
background:#fff8df;
border:1px solid #f0df9c;
padding:12px;
border-radius:12px
}

.log{
background:#111827;
color:#d1d5db;
border-radius:12px;
padding:12px;
min-height:170px;
max-height:280px;
overflow:auto;
font:12px monospace;
margin-top:12px
}

.freegrid{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:12px
}

.tickbox{
display:flex;
align-items:center;
gap:10px;
background:#111827;
color:#fff;
border-radius:12px;
padding:12px;
margin-top:12px
}

.tickdigit{
font-size:28px;
font-weight:900;
margin-left:auto
}

.bottom{display:none}

@media(max-width:760px){

.side{
display:none;
position:fixed;
left:0;
top:64px;
bottom:0;
z-index:30;
width:230px
}

.main{padding:12px}

.cards{
grid-template-columns:1fr 1fr
}

.builder{
grid-template-columns:1fr
}

.freegrid{
grid-template-columns:1fr 1fr
}

.topright .status{
display:none
}

.logo{
font-size:16px
}

.bottom{
display:block;
position:fixed;
bottom:0;
left:0;
right:0;
background:#111827;
color:#fff;
text-align:center;
padding:8px;
font-size:11px;
z-index:20
}

}
</style>
</head>

<body>

<header class="top">

<span class="hamb" onclick="toggleSide()">☰</span>

<span class="logo">
Deriv Strategy Hub
</span>

<div class="topright">

<span id="conn" class="status">
● OFFLINE
</span>

<button class="soft" onclick="connectMarket()">
Market
</button>

<button class="dark"
onclick="alert('Live account trading is not enabled in V2. We will add authenticated demo trading after the market feed is verified.')">
Log in
</button>

</div>

</header>


<div class="layout">

<aside class="side" id="side">

<h3>Strategy Hub</h3>

<button class="active"
onclick="show('dashboard',this)">
⌂ Dashboard
</button>

<button onclick="show('builder',this)">
⚡ Bot Builder
</button>

<button onclick="show('free',this)">
🤖 Free Bots
</button>

<button onclick="show('signals',this)">
✦ Signals
</button>

<button onclick="show('results',this)">
▣ Results
</button>

<button onclick="show('journal',this)">
▤ Trade Log
</button>

</aside>


<main class="main">


<section id="dashboard" class="page active">

<div class="sectiontitle">

<div>
<h2 style="margin:0">
Dashboard
</h2>

<p class="muted">
Live market-data strategy workspace
</p>
</div>

<button class="dark"
onclick="show('builder',document.querySelectorAll('.side button')[1])">
+ Build Bot
</button>

</div>


<div class="cards">

<div class="card">

<div class="muted">
Market Feed
</div>

<div class="metric green"
id="dfeed">
OFFLINE
</div>

</div>


<div class="card">

<div class="muted">
Last Digit
</div>

<div class="metric"
id="ddigit">
—
</div>

</div>


<div class="card">

<div class="muted">
Demo P/L
</div>

<div class="metric"
id="dpnl">
$0.00
</div>

</div>


<div class="card">

<div class="muted">
Win Rate
</div>

<div class="metric"
id="dwinrate">
0%
</div>

</div>

</div>


<div class="tickbox">

<div>

<b>
Volatility 100 (1s)
</b>

<div id="quote"
class="muted"
style="color:#cbd5e1">

Waiting for live tick...

</div>

</div>

<div class="tickdigit"
id="bigdigit">
—
</div>

</div>


<div class="section">

<div class="sectiontitle">

<h3 style="margin:0">
Quick Actions
</h3>

</div>


<div class="cards">

<div class="card"
onclick="show('builder',document.querySelectorAll('.side button')[1])">

<h3>⚡ Quick Start</h3>

<p class="muted">
Configure Over 1 / Under 8.
</p>

</div>


<div class="card"
onclick="runBacktest()">

<h3>📊 Backtest</h3>

<p class="muted">
Test the strategy with live-tick samples.
</p>

</div>


<div class="card"
onclick="show('free',document.querySelectorAll('.side button')[2])">

<h3>🤖 Free Bots</h3>

<p class="muted">
Ready-made demo presets.
</p>

</div>


<div class="card"
onclick="newSignal()">

<h3>✦ Signal</h3>

<p class="muted">
Generate a rule-based demo signal.
</p>

</div>

</div>

</div>


<div class="section notice">

<b>V2 safety:</b>

The market feed is real Deriv public data, but the bot result engine is still a simulation.

It does not place trades or use real account money.

</div>

</section>


<section id="builder" class="page">

<div class="sectiontitle">

<div>

<h2 style="margin:0">
Bot Builder
</h2>

<p class="muted">
Use real ticks for strategy testing.
</p>

</div>

<span class="status">
● LIVE DATA / DEMO TRADES
</span>

</div>


<div class="card builder">


<div class="field">

<label>
MARKET
</label>

<select id="market">

<option value="1HZ100V">
Volatility 100 (1s)
</option>

<option value="1HZ25V">
Volatility 25 (1s)
</option>

<option value="1HZ50V">
Volatility 50 (1s)
</option>

<option value="1HZ75V">
Volatility 75 (1s)
</option>

</select>

</div>


<div class="field">

<label>
CONTRACT RULE
</label>

<select id="contract">

<option>
Over 1
</option>

<option>
Under 8
</option>

<option>
Rise
</option>

<option>
Fall
</option>

</select>

</div>


<div class="field">

<label>
DURATION
</label>

<select>

<option>
1 tick
</option>

</select>

</div>


<div class="field">

<label>
STAKE ($)
</label>

<input
id="stake"
type="number"
min=".35"
step=".01"
value="1">

</div>


<div class="field">

<label>
MARTINGALE
</label>

<input
id="mg"
type="number"
min="1"
step=".1"
value="2">

</div>


<div class="field">

<label>
MAX STEPS
</label>

<select id="steps">

<option>4</option>
<option>3</option>
<option>5</option>
<option>6</option>

</select>

</div>

</div>


<div class="actions">

<button class="dark"
onclick="startBot()">
▶ Start Demo
</button>

<button class="soft"
onclick="stopBot()">
■ Stop
</button>

<button class="soft"
onclick="runBacktest()">
Backtest Samples
</button>

<button class="soft"
onclick="clearLog()">
Clear
</button>

<button class="soft"
onclick="connectMarket()">
Reconnect Feed
</button>

</div>


<div class="cards section">


<div class="card">

<div class="muted">
Simulated Balance
</div>

<div class="metric"
id="balance">
$100.00
</div>

</div>


<div class="card">

<div class="muted">
Wins
</div>

<div class="metric green"
id="wins">
0
</div>

</div>


<div class="card">

<div class="muted">
Losses
</div>

<div class="metric red"
id="losses">
0
</div>

</div>


<div class="card">

<div class="muted">
Net P/L
</div>

<div class="metric"
id="pnl">
$0.00
</div>

</div>

</div>


<div id="log" class="log">

V2 ready.

Connect the market feed, then start the demo strategy.

</div>

</section>


<section id="free" class="page">

<h2>
Free Bots
</h2>

<p class="muted">
Presets use live tick data but do not place real trades.
</p>


<div class="freegrid">


<div class="card">

<h3>
Over 1
</h3>

<p class="muted">
1 tick • Martingale 2
</p>

<button class="dark"
onclick="loadPreset('Over 1')">
Use Bot
</button>

</div>


<div class="card">

<h3>
Under 8
</h3>

<p class="muted">
1 tick • Martingale 2
</p>

<button class="dark"
onclick="loadPreset('Under 8')">
Use Bot
</button>

</div>


<div class="card">

<h3>
Rise
</h3>

<p class="muted">
1 tick • Demo
</p>

<button class="dark"
onclick="loadPreset('Rise')">
Use Bot
</button>

</div>


<div class="card">

<h3>
Fall
</h3>

<p class="muted">
1 tick • Demo
</p>

<button class="dark"
onclick="loadPreset('Fall')">
Use Bot
</button>

</div>

</div>

</section>


<section id="signals" class="page">

<h2>
Signals
</h2>

<div class="card">

<p id="signal">
Waiting for a signal.
</p>

<button class="dark"
onclick="newSignal()">
Generate Signal
</button>

</div>

</section>


<section id="results" class="page">

<h2>
Results
</h2>

<div class="cards">


<div class="card">

<div class="muted">
Trades
</div>

<div class="metric"
id="total">
0
</div>

</div>


<div class="card">

<div class="muted">
Wins
</div>

<div class="metric green"
id="rwins">
0
</div>

</div>


<div class="card">

<div class="muted">
Losses
</div>

<div class="metric red"
id="rlosses">
0
</div>

</div>


<div class="card">

<div class="muted">
Net P/L
</div>

<div class="metric"
id="rpnl">
$0.00
</div>

</div>

</div>


<div class="section notice">

These are simulated results calculated from live market ticks.

They are not actual Deriv contract payouts.

</div>

</section>


<section id="journal" class="page">

<h2>
Trade Log
</h2>

<div class="card">

<p class="muted">
Open Bot Builder to view the detailed tick-by-tick log.
</p>

<button class="dark"
onclick="show('builder',document.querySelectorAll('.side button')[1])">
Open Log
</button>

</div>

</section>

</main>

</div>


<div class="bottom">
LIVE MARKET DATA • DEMO STRATEGY • 1 TICK
</div>


<script>

let ws=null;
let feedConnected=false;
let running=false;

let balance=100;
let wins=0;
let losses=0;
let pnl=0;
let total=0;
let step=0;

let lastTick=null;
let tickSamples=[];


const payout=.95;


function show(id,btn){

document
.querySelectorAll('.page')
.forEach(x=>x.classList.remove('active'));

document
.getElementById(id)
.classList.add('active');

document
.querySelectorAll('.side button')
.forEach(x=>x.classList.remove('active'));

if(btn){
btn.classList.add('active');
}

if(window.innerWidth<=760){

document.getElementById('side')
.style.display='none';

}

}


function toggleSide(){

const s=document.getElementById('side');

s.style.display=
s.style.display==='block'
?'none'
:'block';

}


function setConn(text,cls=''){

const c=document.getElementById('conn');

c.textContent=text;

c.className='status '+cls;

document.getElementById('dfeed')
.textContent=text.replace('● ','');
}


function connectMarket(){

if(ws){

try{
ws.close();
}catch(e){}

}


const symbol=
document.getElementById('market').value;


setConn('● CONNECTING');


ws=new WebSocket(
'wss://api.derivws.com/trading/v1/options/ws/public'
);


ws.onopen=()=>{

setConn('● LIVE','live');

ws.send(
JSON.stringify({
ticks:symbol,
subscribe:1,
req_id:1
})
);

add(
'Live feed connected: '+symbol
);

};


ws.onmessage=(ev)=>{

try{

const data=JSON.parse(ev.data);


if(
data.msg_type==='tick'
&&
data.tick
){

handleTick(data.tick);

}


if(data.error){

setConn('● ERROR','err');

add(
'Feed error: '+
data.error.message
);

}

}catch(e){}

};


ws.onerror=()=>{

setConn('● ERROR','err');

add(
'WebSocket error. Try Reconnect Feed.'
);

};


ws.onclose=()=>{

if(feedConnected){

feedConnected=false;

setConn('● OFFLINE');

}

};

}


function getLastDigit(quote){

const s=String(quote);

const cleaned=
s.replace(/[^0-9]/g,'');

return cleaned.length
?Number(cleaned.slice(-1))
:null;

}


function handleTick(tick){

feedConnected=true;

lastTick=tick;

const d=
getLastDigit(tick.quote);

if(d===null)
return;


tickSamples.push(d);


if(tickSamples.length>500)
tickSamples.shift();


document.getElementById('quote')
.textContent=
'Live quote: '+
tick.quote+
' • '+
new Date(
tick.epoch*1000
).toLocaleTimeString();


document.getElementById('ddigit')
.textContent=d;

document.getElementById('bigdigit')
.textContent=d;


if(running)
processLiveTrade(d);

}


function ruleWin(d,c){

if(c==='Over 1')
return d>1;

if(c==='Under 8')
return d<8;

if(c==='Rise')
return d>=5;

if(c==='Fall')
return d<5;

return false;

}


function processLiveTrade(d){

const base=
parseFloat(
document.getElementById('stake').value
)||1;


const mg=
parseFloat(
document.getElementById('mg').value
)||2;


const max=
parseInt(
document.getElementById('steps').value
)||4;


const c=
document.getElementById('contract').value;


const st=
base*Math.pow(mg,step);


const ok=
ruleWin(d,c);


total++;


if(ok){

const profit=
st*payout;

balance+=profit;

pnl+=profit;

wins++;

step=0;


add(
'LIVE TICK • WIN • '+
c+
' • digit '+
d+
' • simulated P/L +$'+
profit.toFixed(2)
);

}else{

balance-=st;

pnl-=st;

losses++;

step=
step+1>=max
?0
:step+1;


add(
'LIVE TICK • LOSS • '+
c+
' • digit '+
d+
' • simulated P/L -$'+
st.toFixed(2)+
' • next step '+
step
);

}


update();

}


function startBot(){

if(running)
return;


if(
!ws ||
ws.readyState!==WebSocket.OPEN
){

add(
'Connect the market feed first.'
);

connectMarket();

return;

}


running=true;


add(
'Demo strategy started using live market ticks.'
);

}


function stopBot(){

running=false;

add(
'Demo strategy stopped.'
);

}


function add(s){

const l=
document.getElementById('log');

l.innerHTML+=
'<br>'+s;

l.scrollTop=
l.scrollHeight;

}


function clearLog(){

document.getElementById('log')
.innerHTML=
'Log cleared.';

}


function update(){

const wr=
total
?((wins/total)*100).toFixed(1)
:'0';


document.getElementById('balance')
.textContent=
'$'+balance.toFixed(2);


document.getElementById('wins')
.textContent=wins;


document.getElementById('losses')
.textContent=losses;


document.getElementById('pnl')
.textContent=
'$'+pnl.toFixed(2);


document.getElementById('total')
.textContent=total;


document.getElementById('rwins')
.textContent=wins;


document.getElementById('rlosses')
.textContent=losses;


document.getElementById('rpnl')
.textContent=
'$'+pnl.toFixed(2);


document.getElementById('dpnl')
.textContent=
'$'+pnl.toFixed(2);


document.getElementById('dwinrate')
.textContent=
wr+'%';

}


function runBacktest(){

if(!tickSamples.length){

add(
'No live tick samples yet. Connect the feed first.'
);

connectMarket();

return;

}


stopBot();

balance=100;
wins=0;
losses=0;
pnl=0;
total=0;
step=0;


clearLog();


add(
'Backtest started using '+
tickSamples.length+
' captured live-tick samples.'
);


const samples=
tickSamples.slice(-100);


for(
const d of samples
){

const base=
parseFloat(
document.getElementById('stake').value
)||1;


const mg=
parseFloat(
document.getElementById('mg').value
)||2;


const max=
parseInt(
document.getElementById('steps').value
)||4;


const c=
document.getElementById('contract').value;


const st=
base*Math.pow(mg,step);


total++;


if(
ruleWin(d,c)
){

const p=
st*payout;

balance+=p;

pnl+=p;

wins++;

step=0;

}else{

balance-=st;

pnl-=st;

losses++;

step=
step+1>=max
?0
:step+1;

}

}


add(
'Backtest complete: '+
samples.length+
' samples.'
);


update();

}


function loadPreset(c){

document.getElementById('contract')
.value=c;


show(
'builder',
document.querySelectorAll('.side button')[1]
);


add(
'Loaded preset: '+c
);

}


function newSignal(){

const c=
document.getElementById('contract')
.value;


const d=
lastTick===null
?'—'
:lastTick;


document.getElementById('signal')
.textContent=
'Current rule: '+
c+
' • latest digit: '+
d+
' • demo only.';


show(
'signals',
document.querySelectorAll('.side button')[3]
);

}


document
.getElementById('market')
.addEventListener(
'change',
connectMarket
);


update();

connectMarket();

</script>

</body>
</html>
