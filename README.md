<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<title>AI Smart Solar Grid Energy Manager</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
margin:0;
font-family:Segoe UI,Arial;
background:#020617;
color:white;
overflow-x:hidden;
}

/* Boot Screen */

#boot{
position:fixed;
width:100%;
height:100%;
background:black;
display:flex;
justify-content:center;
align-items:center;
flex-direction:column;
font-size:22px;
letter-spacing:1px;
z-index:9999;
}

header{
text-align:center;
padding:60px;
background:linear-gradient(120deg,#020617,#1e293b);
border-bottom:1px solid #334155;
}

h1{
font-size:48px;
margin:0;
}

.subtitle{
color:#94a3b8;
margin-top:10px;
}

/* Dashboard */

.dashboard{
display:flex;
justify-content:center;
flex-wrap:wrap;
gap:20px;
padding:40px;
}

.card{
background:#1e293b;
padding:25px;
border-radius:12px;
width:200px;
text-align:center;
box-shadow:0 0 20px rgba(0,255,255,0.15);
transition:0.3s;
}

.card:hover{
transform:translateY(-5px);
}

.card h2{
font-size:32px;
margin:10px 0;
}

/* Energy Flow */

.energyFlow{
display:flex;
justify-content:center;
align-items:center;
gap:80px;
margin-top:40px;
flex-wrap:wrap;
}

.box{
width:130px;
height:130px;
background:#1e293b;
border-radius:14px;
display:flex;
justify-content:center;
align-items:center;
font-size:18px;
box-shadow:0 0 20px rgba(0,255,255,0.15);
}

.flow{
font-size:32px;
animation:pulse 1s infinite;
}

@keyframes pulse{
0%{opacity:0.3}
50%{opacity:1}
100%{opacity:0.3}
}

/* Table */

section{
padding:50px;
max-width:1200px;
margin:auto;
}

table{
width:100%;
border-collapse:collapse;
margin-top:30px;
background:#020617;
}

td,th{
border:1px solid #334155;
padding:10px;
text-align:center;
}

th{
background:#1e293b;
}

/* Chart */

canvas{
background:white;
border-radius:10px;
margin-top:20px;
}

</style>

</head>

<body>

<!-- Boot Screen -->

<div id="boot">
<div id="bootText">Initializing System...</div>
</div>

<header>

<h1>⚡ Smart Solar-Grid Energy Manager</h1>
<p class="subtitle">AI Based IoT Load Optimization System</p>

</header>

<!-- Dashboard -->

<div class="dashboard">

<div class="card">
<h3>Total Load</h3>
<h2 id="load">0W</h2>
</div>

<div class="card">
<h3>Solar Power</h3>
<h2 id="solar">0W</h2>
</div>

<div class="card">
<h3>Grid Power</h3>
<h2 id="grid">0W</h2>
</div>

<div class="card">
<h3>Active Loads</h3>
<h2 id="active">0</h2>
</div>

</div>

<!-- Energy Flow -->

<section>

<h2 style="text-align:center">Energy Flow Visualization</h2>

<div class="energyFlow">

<div class="box">☀ Solar</div>

<div class="flow">➡</div>

<div class="box">🏠 House</div>

<div class="flow">⬅</div>

<div class="box">⚡ Grid</div>

</div>

</section>

<!-- Data Table -->

<section>

<h2>24 Hour Smart Energy Simulation</h2>

<table id="table">

<tr>
<th>Time</th>
<th>Price</th>
<th>Active Loads</th>
<th>Total Load (W)</th>
<th>Solar (W)</th>
<th>Grid (W)</th>
<th>Solar %</th>
<th>Grid %</th>
</tr>

</table>

</section>

<!-- Chart -->

<section>

<h2>Solar vs Grid Energy Usage</h2>

<canvas id="chart"></canvas>

</section>

<script>

/* Boot Animation */

const bootMessages=[
"Initializing Energy AI...",
"Connecting Solar Sensors...",
"Connecting Grid Meter...",
"Connecting IoT Load Controller...",
"Running Optimization Algorithm...",
"System Ready"
]

let bootIndex=0

function bootSequence(){

if(bootIndex<bootMessages.length){

document.getElementById("bootText").innerText=bootMessages[bootIndex]

bootIndex++

setTimeout(bootSequence,900)

}else{

document.getElementById("boot").style.display="none"

startSimulation()

}

}

bootSequence()

/* Chart */

const solarData=[]
const gridData=[]
const labels=[]

const chart=new Chart(document.getElementById("chart"),{

type:"line",

data:{
labels:labels,
datasets:[
{
label:"Solar Power",
data:solarData,
borderColor:"orange",
borderWidth:3,
fill:false,
tension:0.3
},
{
label:"Grid Power",
data:gridData,
borderColor:"cyan",
borderWidth:3,
fill:false,
tension:0.3
}
]
},

options:{
responsive:true,
plugins:{
legend:{labels:{color:"black"}}
},
scales:{
x:{ticks:{color:"black"}},
y:{ticks:{color:"black"}}
}
}

})

/* Simulation Logic */

let hour=0

function startSimulation(){

setInterval(runHour,700)

}

function runHour(){

if(hour>23) return

let price
let loads

/* price logic */

if(hour<6){price=4;loads=1}
else if(hour<9){price=3;loads=2}
else if(hour<14){price=1;loads=4}
else if(hour<17){price=2;loads=3}
else if(hour<20){price=3;loads=2}
else{price=4;loads=3}

let totalLoad=loads*100

/* solar availability */

let solar=0

if(hour>=10 && hour<=14) solar=totalLoad
else if(hour>=7 && hour<=9) solar=totalLoad*0.4
else if(hour>=15 && hour<=17) solar=totalLoad*0.4

solar=Math.round(solar)

let grid=totalLoad-solar

/* update dashboard */

document.getElementById("load").innerText=totalLoad+" W"
document.getElementById("solar").innerText=solar+" W"
document.getElementById("grid").innerText=grid+" W"
document.getElementById("active").innerText=loads

let solarP=Math.round((solar/totalLoad)*100)
let gridP=100-solarP

/* add table row */

let table=document.getElementById("table")

let row=table.insertRow()

row.insertCell(0).innerText=hour+":00"
row.insertCell(1).innerText=price
row.insertCell(2).innerText=loads
row.insertCell(3).innerText=totalLoad
row.insertCell(4).innerText=solar
row.insertCell(5).innerText=grid
row.insertCell(6).innerText=solarP+"%"
row.insertCell(7).innerText=gridP+"%"

/* update chart */

labels.push(hour)
solarData.push(solar)
gridData.push(grid)

chart.update()

hour++

}

</script>

</body>
</html>
