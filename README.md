<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>School Attendance System</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,400;0,500;1,400&family=Fraunces:ital,wght@0,300;0,600;1,300;1,600&display=swap" rel="stylesheet"/>
<style>
:root{--bg:#f0ede6;--surface:#faf8f3;--card:#fffef9;--ink:#1c1a16;--ink2:#5c5a54;--ink3:#9c9a94;--border:#ddd9cf;--present:#1e6e45;--present-bg:#d5f0e0;--absent:#a01f30;--absent-bg:#fde8eb;--accent:#bf6a20;--accent-bg:#fef0e0;--mono:'DM Mono',monospace;--serif:'Fraunces',serif;}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);font-family:var(--mono);color:var(--ink);min-height:100vh;overflow-x:hidden;}
.shell{display:flex;height:100vh;overflow:hidden;}

/* SIDEBAR */
.sidebar{width:265px;flex-shrink:0;background:var(--ink);color:#f0ede6;display:flex;flex-direction:column;overflow:hidden;}
.sidebar-head{padding:22px 18px 14px;border-bottom:1px solid #2e2c28;}
.sidebar-logo{font-family:var(--serif);font-size:21px;font-weight:300;}
.sidebar-logo em{font-style:italic;color:#c9a96e;}
.sidebar-date{font-size:10px;color:#666;letter-spacing:0.08em;margin-top:3px;text-transform:uppercase;}
.sidebar-section{font-size:10px;text-transform:uppercase;letter-spacing:0.1em;color:#555;padding:14px 18px 6px;}
.sidebar-classes{flex:1;overflow-y:auto;padding:0 8px 10px;}
.sidebar-classes::-webkit-scrollbar{width:3px;}
.sidebar-classes::-webkit-scrollbar-thumb{background:#333;border-radius:2px;}
.class-item{padding:11px 12px;border-radius:7px;cursor:pointer;margin-bottom:3px;transition:background 0.12s;position:relative;border-left:3px solid transparent;}
.class-item:hover{background:#252320;}
.class-item.active{background:#252320;}
.class-item-name{font-size:13px;font-weight:500;}
.class-item-teacher{font-size:11px;color:#777;margin-top:1px;}
.class-item-badge{position:absolute;right:10px;top:50%;transform:translateY(-50%);font-size:10px;padding:2px 7px;border-radius:10px;font-weight:500;}
.badge-live{background:#1e6e45;color:#d5f0e0;}
.badge-done{background:#2e2c28;color:#888;}
.badge-none{background:#252320;color:#555;}
.sidebar-bottom{border-top:1px solid #2e2c28;padding:12px 8px;}
.btn-add-class{width:100%;font-family:var(--mono);font-size:12px;padding:9px;border:1px dashed #444;border-radius:7px;background:transparent;color:#888;cursor:pointer;letter-spacing:0.04em;transition:all 0.15s;}
.btn-add-class:hover{border-color:#aaa;color:#f0ede6;}
.overview-btn{width:100%;font-family:var(--mono);font-size:12px;padding:9px;border:1px solid #2e2c28;border-radius:7px;background:#2a2824;color:#c9a96e;cursor:pointer;margin-bottom:6px;letter-spacing:0.04em;transition:all 0.15s;}
.overview-btn:hover{background:#333;}

/* CONTENT */
.content{flex:1;overflow-y:auto;display:flex;flex-direction:column;min-width:0;}
.topbar{background:var(--card);border-bottom:1px solid var(--border);padding:14px 28px;display:flex;align-items:center;gap:12px;flex-wrap:wrap;flex-shrink:0;}
.topbar-title{font-family:var(--serif);font-size:19px;font-weight:300;flex:1;min-width:0;}
.topbar-teacher{font-size:12px;color:var(--ink2);padding:5px 13px;background:var(--accent-bg);border-radius:20px;border:1px solid #f0d0a0;white-space:nowrap;}
.btn{font-family:var(--mono);font-size:12px;font-weight:500;padding:7px 14px;border-radius:7px;cursor:pointer;border:1px solid transparent;transition:all 0.13s;letter-spacing:0.03em;white-space:nowrap;}
.btn-dark{background:var(--ink);color:#f0ede6;}.btn-dark:hover{background:#333;}
.btn-outline{background:transparent;border-color:var(--border);color:var(--ink);}.btn-outline:hover{background:var(--bg);}
.btn-p{background:var(--present-bg);color:var(--present);border-color:var(--present);}.btn-p:hover{background:var(--present);color:#fff;}
.btn-a{background:var(--absent-bg);color:var(--absent);border-color:var(--absent);}.btn-a:hover{background:var(--absent);color:#fff;}

/* STATS */
.stats-row{display:flex;border-bottom:1px solid var(--border);background:var(--surface);flex-shrink:0;overflow-x:auto;}
.stat{flex:1;min-width:90px;padding:13px 20px;border-right:1px solid var(--border);}
.stat:last-child{border-right:none;}
.stat-label{font-size:10px;text-transform:uppercase;letter-spacing:0.09em;color:var(--ink3);margin-bottom:3px;}
.stat-val{font-family:var(--serif);font-size:26px;font-weight:600;line-height:1;}
.stat-val.g{color:var(--present);}.stat-val.r{color:var(--absent);}.stat-val.a{color:var(--accent);}.stat-val.m{color:var(--ink3);}
.pbar{height:3px;background:var(--border);border-radius:2px;margin-top:5px;overflow:hidden;}
.pbar-fill{height:100%;background:var(--present);border-radius:2px;transition:width 0.4s;}

/* TABS */
.tabs{display:flex;border-bottom:1px solid var(--border);background:var(--card);padding:0 28px;flex-shrink:0;}
.tab{font-size:12px;padding:10px 16px;cursor:pointer;border-bottom:2px solid transparent;color:var(--ink2);letter-spacing:0.04em;transition:all 0.13s;}
.tab.active{border-bottom-color:var(--accent);color:var(--ink);}
.view{display:none;padding:24px 28px;}
.view.active{display:block;}

/* OVERVIEW CARDS */
.ov-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(250px,1fr));gap:14px;}
.ov-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:16px 18px;cursor:pointer;transition:all 0.13s;}
.ov-card:hover{transform:translateY(-2px);box-shadow:0 4px 16px rgba(0,0,0,0.07);}
.ov-class-name{font-family:var(--serif);font-size:17px;font-weight:600;}
.ov-sub{font-size:11px;color:var(--ink3);margin-top:1px;}
.ov-teacher-row{font-size:12px;color:var(--ink2);margin:8px 0;}
.ov-teacher-row b{font-weight:500;}
.ov-numbers{display:flex;gap:14px;margin-bottom:8px;}
.ov-num-val{font-family:var(--serif);font-size:21px;font-weight:600;}
.ov-num-label{font-size:10px;color:var(--ink3);text-transform:uppercase;letter-spacing:0.06em;}
.ov-pbar{height:4px;background:var(--border);border-radius:2px;overflow:hidden;margin-bottom:12px;}
.ov-pbar-fill{height:100%;border-radius:2px;transition:width 0.4s;}
.ov-btn{width:100%;font-family:var(--mono);font-size:11px;padding:8px;border-radius:6px;border:none;color:#fff;cursor:pointer;letter-spacing:0.04em;transition:opacity 0.15s;}
.ov-btn:hover{opacity:0.85;}

/* GRID */
.grid-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;gap:10px;flex-wrap:wrap;}
.search-box{font-family:var(--mono);font-size:12px;padding:6px 12px;border:1px solid var(--border);border-radius:20px;background:var(--bg);color:var(--ink);outline:none;width:170px;}
.search-box:focus{border-color:var(--accent);}
.stu-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(155px,1fr));gap:9px;}
.stu-card{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:13px;cursor:pointer;transition:all 0.12s;position:relative;user-select:none;}
.stu-card:hover{transform:translateY(-1px);border-color:var(--accent);}
.stu-card.present{border-color:var(--present);background:#f2fbf5;}
.stu-card.absent{border-color:var(--absent);background:#fff5f6;}
.stu-dot{position:absolute;top:10px;right:10px;width:8px;height:8px;border-radius:50%;}
.stu-dot.present{background:var(--present);}.stu-dot.absent{background:var(--absent);}.stu-dot.none{background:var(--border);}
.stu-name{font-size:13px;font-weight:500;line-height:1.3;padding-right:14px;}
.stu-roll{font-size:11px;color:var(--ink3);margin-top:2px;}
.stu-status{font-size:10px;font-weight:500;text-transform:uppercase;letter-spacing:0.06em;padding:2px 7px;border-radius:10px;margin-top:7px;display:inline-block;}
.stu-status.present{background:var(--present-bg);color:var(--present);}
.stu-status.absent{background:var(--absent-bg);color:var(--absent);}
.stu-status.none{background:var(--bg);color:var(--ink3);}

/* TABLE */
table{width:100%;border-collapse:collapse;font-size:12px;background:var(--card);border-radius:10px;overflow:hidden;border:1px solid var(--border);}
thead{background:var(--ink);color:#f0ede6;}
th{padding:9px 13px;text-align:left;font-weight:500;font-size:11px;text-transform:uppercase;letter-spacing:0.06em;}
td{padding:8px 13px;border-bottom:1px solid var(--border);}
tr:last-child td{border-bottom:none;}
tr:hover td{background:#faf8f3;}
.td-p{color:var(--present);font-weight:500;}.td-a{color:var(--absent);font-weight:500;}.td-n{color:var(--ink3);}

/* LOG */
.log-box{background:var(--card);border:1px solid var(--border);border-radius:10px;max-height:500px;overflow-y:auto;}
.log-entry{display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--border);font-size:12px;animation:fin 0.2s ease;}
.log-entry:last-child{border-bottom:none;}
@keyframes fin{from{opacity:0;transform:translateX(5px)}to{opacity:1}}
.log-class-tag{font-size:10px;padding:1px 7px;border-radius:10px;flex-shrink:0;}
.log-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;}
.log-dot.present{background:var(--present);}.log-dot.absent{background:var(--absent);}
.log-name{flex:1;}
.log-badge{font-size:10px;font-weight:500;padding:1px 7px;border-radius:10px;flex-shrink:0;}
.log-badge.present{background:var(--present-bg);color:var(--present);}
.log-badge.absent{background:var(--absent-bg);color:var(--absent);}
.log-time{font-size:10px;color:var(--ink3);flex-shrink:0;}
.log-empty{padding:28px;color:var(--ink3);font-size:12px;text-align:center;}

/* MODALS */
.overlay{display:none;position:fixed;inset:0;background:rgba(28,26,22,0.6);z-index:200;align-items:center;justify-content:center;}
.overlay.open{display:flex;}
.modal{background:var(--card);border-radius:14px;padding:26px 30px;width:400px;max-width:95vw;}
.modal h2{font-family:var(--serif);font-size:21px;font-weight:300;margin-bottom:16px;}
.modal label{font-size:11px;text-transform:uppercase;letter-spacing:0.07em;color:var(--ink2);display:block;margin:11px 0 3px;}
.modal input{width:100%;font-family:var(--mono);font-size:13px;padding:8px 11px;border:1px solid var(--border);border-radius:7px;background:var(--bg);color:var(--ink);outline:none;}
.modal input:focus{border-color:var(--accent);}
.modal-row{display:flex;gap:10px;}
.modal-row>div{flex:1;}
.modal-actions{display:flex;gap:10px;margin-top:18px;justify-content:flex-end;}
.swatches{display:flex;gap:7px;margin-top:5px;flex-wrap:wrap;}
.swatch{width:24px;height:24px;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:all 0.13s;}
.swatch.sel{border-color:var(--ink);transform:scale(1.18);}

/* Delete button */
.del-btn{font-family:var(--mono);font-size:10px;padding:2px 8px;border:1px solid var(--absent);color:var(--absent);background:var(--absent-bg);border-radius:5px;cursor:pointer;transition:all 0.12s;}
.del-btn:hover{background:var(--absent);color:#fff;}
</style>
</head>
<body>
<div class="shell">

<!-- SIDEBAR -->
<div class="sidebar">
  <div class="sidebar-head">
    <div class="sidebar-logo">School <em>Attendance</em></div>
    <div class="sidebar-date" id="sidebar-date"></div>
  </div>
  <div style="padding:10px 8px 0;">
    <button class="overview-btn" onclick="goOverview()">⊞ All Classes Overview</button>
  </div>
  <div class="sidebar-section">Classes</div>
  <div class="sidebar-classes" id="sidebar-classes"></div>
  <div class="sidebar-bottom">
    <button class="btn-add-class" onclick="openAddClass()">+ Add New Class</button>
  </div>
</div>

<!-- MAIN -->
<div class="content">
  <div class="topbar" id="topbar">
    <div class="topbar-title" id="topbar-title">All Classes Overview</div>
    <input type="date" id="dateInput" style="font-family:var(--mono);font-size:12px;padding:6px 11px;border:1px solid var(--border);border-radius:7px;background:var(--bg);color:var(--ink);outline:none;" onchange="changeDate()"/>
    <div id="topbar-teacher" style="display:none;" class="topbar-teacher"></div>
    <div id="topbar-actions" style="display:flex;gap:7px;flex-wrap:wrap;"></div>
  </div>

  <div class="stats-row">
    <div class="stat"><div class="stat-label">Students</div><div class="stat-val a" id="st-total">—</div></div>
    <div class="stat"><div class="stat-label">Present</div><div class="stat-val g" id="st-present">—</div></div>
    <div class="stat"><div class="stat-label">Absent</div><div class="stat-val r" id="st-absent">—</div></div>
    <div class="stat"><div class="stat-label">Pending</div><div class="stat-val m" id="st-none">—</div></div>
    <div class="stat">
      <div class="stat-label">Rate</div>
      <div class="stat-val a" id="st-pct">—</div>
      <div class="pbar"><div class="pbar-fill" id="pbar-fill" style="width:0%"></div></div>
    </div>
  </div>

  <div class="tabs" id="tabs">
    <div class="tab active" id="tab-overview" onclick="switchTab('overview',this)">All Classes</div>
    <div class="tab" id="tab-grid" style="display:none;" onclick="switchTab('grid',this)">Grid</div>
    <div class="tab" id="tab-table" style="display:none;" onclick="switchTab('table',this)">Table</div>
    <div class="tab" id="tab-log" onclick="switchTab('log',this)">Activity Log</div>
  </div>

  <!-- OVERVIEW -->
  <div class="view active" id="view-overview">
    <div class="ov-grid" id="ov-grid"></div>
  </div>

  <!-- GRID -->
  <div class="view" id="view-grid">
    <div class="grid-header">
      <div style="display:flex;gap:7px;flex-wrap:wrap;">
        <button class="btn btn-p" onclick="markAll('present')">✓ All Present</button>
        <button class="btn btn-a" onclick="markAll('absent')">✗ All Absent</button>
        <button class="btn btn-outline" onclick="resetAll()">Reset</button>
      </div>
      <div style="display:flex;gap:7px;align-items:center;flex-wrap:wrap;">
        <button class="btn btn-outline" onclick="openAddStudent()">+ Student</button>
        <input class="search-box" id="searchBox" type="text" placeholder="Search..." oninput="renderGrid()"/>
      </div>
    </div>
    <div class="stu-grid" id="stu-grid"></div>
  </div>

  <!-- TABLE -->
  <div class="view" id="view-table">
    <div style="display:flex;justify-content:flex-end;margin-bottom:11px;gap:7px;">
      <button class="btn btn-outline" onclick="openAddStudent()">+ Student</button>
      <button class="btn btn-dark" onclick="exportCSV()">↓ Export CSV</button>
    </div>
    <table>
      <thead><tr><th>#</th><th>Roll</th><th>Name</th><th>Status</th><th>Time</th><th>Mark</th></tr></thead>
      <tbody id="table-body"></tbody>
    </table>
  </div>

  <!-- LOG -->
  <div class="view" id="view-log">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
      <div style="font-family:var(--serif);font-size:15px;font-style:italic;color:var(--ink2);">All activity across classes</div>
      <button class="btn btn-outline" style="font-size:11px;padding:4px 10px;" onclick="clearLog()">Clear</button>
    </div>
    <div class="log-box" id="log-box"></div>
  </div>
</div>
</div>

<!-- ADD CLASS MODAL -->
<div class="overlay" id="ov-class" onclick="closeOv('ov-class',event)">
<div class="modal">
  <h2>Add New Class</h2>
  <div class="modal-row">
    <div><label>Class Name</label><input id="ac-name" placeholder="e.g. Class 10-B"/></div>
    <div><label>Subject</label><input id="ac-subject" placeholder="e.g. Physics"/></div>
  </div>
  <label>Teacher Name</label><input id="ac-teacher" placeholder="e.g. Ms. Priya Menon"/>
  <div class="modal-row">
    <div><label>Period / Time</label><input id="ac-period" placeholder="9:00 – 10:00 AM"/></div>
    <div><label>Room</label><input id="ac-room" placeholder="Room 205"/></div>
  </div>
  <label>Class Color</label>
  <div class="swatches" id="color-swatches"></div>
  <div class="modal-actions">
    <button class="btn btn-outline" onclick="closeOv('ov-class')">Cancel</button>
    <button class="btn btn-dark" onclick="submitAddClass()">Create Class</button>
  </div>
</div>
</div>

<!-- ADD STUDENT MODAL -->
<div class="overlay" id="ov-student" onclick="closeOv('ov-student',event)">
<div class="modal">
  <h2>Add Student</h2>
  <div class="modal-row">
    <div><label>Full Name</label><input id="as-name" placeholder="Student name"/></div>
    <div><label>Roll No.</label><input id="as-roll" placeholder="01"/></div>
  </div>
  <div class="modal-actions">
    <button class="btn btn-outline" onclick="closeOv('ov-student')">Cancel</button>
    <button class="btn btn-dark" onclick="submitAddStudent()">Add Student</button>
  </div>
</div>
</div>

<script>
const COLORS=['#bf6a20','#1e6e45','#a01f30','#1a5fa0','#6a3ea0','#0e7b7b','#8b5e00','#c04080'];
let classes=[
  {id:1,name:'Class 9-A',subject:'Mathematics',teacher:'Mr. Rajesh Kumar',period:'8:00–9:00 AM',room:'Room 101',color:'#bf6a20',
   students:[{id:1,name:'Aarav Sharma',roll:'01'},{id:2,name:'Bhavna Patel',roll:'02'},{id:3,name:'Chetan Reddy',roll:'03'},{id:4,name:'Divya Nair',roll:'04'},{id:5,name:'Elan Murugan',roll:'05'},{id:6,name:'Fatima Shaikh',roll:'06'}]},
  {id:2,name:'Class 10-B',subject:'Physics',teacher:'Ms. Priya Menon',period:'9:00–10:00 AM',room:'Room 205',color:'#1e6e45',
   students:[{id:1,name:'Gaurav Mishra',roll:'01'},{id:2,name:'Hema Iyer',roll:'02'},{id:3,name:'Imran Khan',roll:'03'},{id:4,name:'Janani Krishnan',roll:'04'},{id:5,name:'Karthik Rajan',roll:'05'},{id:6,name:'Lavanya Suresh',roll:'06'},{id:7,name:'Mohan Das',roll:'07'}]},
  {id:3,name:'Class 11-C',subject:'Chemistry',teacher:'Dr. Sunita Rao',period:'10:00–11:00 AM',room:'Lab 3',color:'#a01f30',
   students:[{id:1,name:'Naveen Pillai',roll:'01'},{id:2,name:'Oksana Varma',roll:'02'},{id:3,name:'Pranav Joshi',roll:'03'},{id:4,name:'Qureshi Ayaan',roll:'04'},{id:5,name:'Riya Mehta',roll:'05'}]},
  {id:4,name:'Class 12-D',subject:'Biology',teacher:'Mr. Arun Nambiar',period:'11:00 AM–12:00 PM',room:'Lab 1',color:'#1a5fa0',
   students:[{id:1,name:'Siddharth Jain',roll:'01'},{id:2,name:'Tara Pillai',roll:'02'},{id:3,name:'Uday Sharma',roll:'03'},{id:4,name:'Vani Krishnan',roll:'04'},{id:5,name:'Wasim Akhtar',roll:'05'},{id:6,name:'Xena Patel',roll:'06'}]},
];
let attendance={};
let globalLog=[];
let activeClassId=null;
let activeTab='overview';
let selColor=COLORS[0];
let nextClassId=5;
let currentDate=new Date().toISOString().split('T')[0];
document.getElementById('dateInput').value=currentDate;
classes.forEach(c=>{attendance[c.id]={};});

function updateSidebarDate(){
  const d=new Date(currentDate+'T00:00:00');
  document.getElementById('sidebar-date').textContent=d.toLocaleDateString('en-IN',{weekday:'short',day:'numeric',month:'short',year:'numeric'});
}
updateSidebarDate();

function renderSidebar(){
  updateSidebarDate();
  const el=document.getElementById('sidebar-classes');
  el.innerHTML=classes.map(c=>{
    const att=attendance[c.id]||{};
    const marked=Object.keys(att).length;
    const present=Object.values(att).filter(x=>x.status==='present').length;
    let badge=`<span class="class-item-badge badge-none">${c.students.length}stu</span>`;
    if(marked>0&&marked<c.students.length)badge=`<span class="class-item-badge badge-live">${marked}/${c.students.length}</span>`;
    else if(marked===c.students.length&&marked>0)badge=`<span class="class-item-badge badge-done">✓</span>`;
    return`<div class="class-item${activeClassId===c.id?' active':''}" onclick="selectClass(${c.id})" style="border-left-color:${c.color}">
      <div class="class-item-name">${c.name}</div>
      <div class="class-item-teacher">${c.subject} · ${c.teacher.split(' ').slice(-1)[0]}</div>
      ${badge}
    </div>`;
  }).join('');
}

function goOverview(){
  activeClassId=null;
  document.getElementById('tab-grid').style.display='none';
  document.getElementById('tab-table').style.display='none';
  document.getElementById('topbar-title').textContent='All Classes Overview';
  document.getElementById('topbar-teacher').style.display='none';
  document.getElementById('topbar-actions').innerHTML='';
  switchTab('overview',document.getElementById('tab-overview'));
  renderStats();
  renderSidebar();
}

function selectClass(id){
  activeClassId=id;
  const c=classes.find(x=>x.id===id);
  document.getElementById('tab-grid').style.display='';
  document.getElementById('tab-table').style.display='';
  document.getElementById('topbar-title').innerHTML=`<span style="color:${c.color}">${c.name}</span> — ${c.subject}`;
  document.getElementById('topbar-teacher').style.display='';
  document.getElementById('topbar-teacher').textContent='👤 '+c.teacher+' · '+c.period+' · '+c.room;
  document.getElementById('topbar-actions').innerHTML=`<button class="btn btn-outline del-btn" onclick="deleteClass(${c.id})">Delete Class</button>`;
  switchTab('grid',document.getElementById('tab-grid'));
  renderStats();
  renderSidebar();
}

function deleteClass(id){
  if(!confirm('Delete this class and all its attendance data?'))return;
  classes=classes.filter(x=>x.id!==id);
  delete attendance[id];
  goOverview();
}

function switchTab(name,el){
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
  el.classList.add('active');
  activeTab=name;
  document.getElementById('view-'+name).classList.add('active');
  if(name==='grid')renderGrid();
  else if(name==='table')renderTable();
  else if(name==='overview')renderOverview();
  else if(name==='log')renderLog();
}

function renderStats(){
  if(!activeClassId){
    let total=0,present=0,absent=0,none=0;
    classes.forEach(c=>{
      total+=c.students.length;
      const att=attendance[c.id]||{};
      c.students.forEach(s=>{
        const st=att[s.id]?.status||'none';
        if(st==='present')present++;else if(st==='absent')absent++;else none++;
      });
    });
    const marked=present+absent;
    const pct=marked?Math.round(present/marked*100):null;
    document.getElementById('st-total').textContent=total;
    document.getElementById('st-present').textContent=present;
    document.getElementById('st-absent').textContent=absent;
    document.getElementById('st-none').textContent=none;
    document.getElementById('st-pct').textContent=pct!==null?pct+'%':'—';
    document.getElementById('pbar-fill').style.width=(pct||0)+'%';
    return;
  }
  const c=classes.find(x=>x.id===activeClassId);
  const att=attendance[c.id]||{};
  const total=c.students.length;
  const present=c.students.filter(s=>att[s.id]?.status==='present').length;
  const absent=c.students.filter(s=>att[s.id]?.status==='absent').length;
  const none=total-present-absent;
  const marked=present+absent;
  const pct=marked?Math.round(present/marked*100):null;
  document.getElementById('st-total').textContent=total;
  document.getElementById('st-present').textContent=present;
  document.getElementById('st-absent').textContent=absent;
  document.getElementById('st-none').textContent=none;
  document.getElementById('st-pct').textContent=pct!==null?pct+'%':'—';
  document.getElementById('pbar-fill').style.width=(pct||0)+'%';
}

function renderOverview(){
  renderStats();
  const el=document.getElementById('ov-grid');
  if(!classes.length){el.innerHTML='<div style="color:var(--ink3);font-size:13px;padding:40px 0;">No classes yet. Add one from the sidebar.</div>';return;}
  el.innerHTML=classes.map(c=>{
    const att=attendance[c.id]||{};
    const total=c.students.length;
    const present=c.students.filter(s=>att[s.id]?.status==='present').length;
    const absent=c.students.filter(s=>att[s.id]?.status==='absent').length;
    const none=total-present-absent;
    const marked=present+absent;
    const pct=marked?Math.round(present/marked*100):0;
    return`<div class="ov-card" onclick="selectClass(${c.id})">
      <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px;">
        <div>
          <div class="ov-class-name" style="color:${c.color}">${c.name}</div>
          <div class="ov-sub">${c.subject} · ${c.period} · ${c.room}</div>
        </div>
        <div style="font-size:10px;color:var(--ink3);text-align:right;margin-top:2px;">${c.students.length} students</div>
      </div>
      <div class="ov-teacher-row">Teacher: <b>${c.teacher}</b></div>
      <div class="ov-numbers">
        <div><div class="ov-num-val" style="color:var(--present)">${present}</div><div class="ov-num-label">Present</div></div>
        <div><div class="ov-num-val" style="color:var(--absent)">${absent}</div><div class="ov-num-label">Absent</div></div>
        <div><div class="ov-num-val" style="color:var(--ink3)">${none}</div><div class="ov-num-label">Pending</div></div>
        <div><div class="ov-num-val" style="color:var(--accent)">${pct?pct+'%':'—'}</div><div class="ov-num-label">Rate</div></div>
      </div>
      <div class="ov-pbar"><div class="ov-pbar-fill" style="width:${pct}%;background:${c.color}"></div></div>
      <button class="ov-btn" style="background:${c.color}" onclick="event.stopPropagation();selectClass(${c.id})">Take Attendance →</button>
    </div>`;
  }).join('');
}

function renderGrid(){
  if(!activeClassId)return;
  const c=classes.find(x=>x.id===activeClassId);
  const att=attendance[c.id]||{};
  const q=(document.getElementById('searchBox').value||'').toLowerCase();
  const filtered=c.students.filter(s=>s.name.toLowerCase().includes(q)||s.roll.includes(q));
  document.getElementById('stu-grid').innerHTML=filtered.map(s=>{
    const st=att[s.id]?.status||'none';
    return`<div class="stu-card ${st}" onclick="cycleStatus(${s.id})">
      <div class="stu-dot ${st}"></div>
      <div class="stu-name">${s.name}</div>
      <div class="stu-roll">Roll: ${s.roll}</div>
      <div class="stu-status ${st}">${st==='none'?'Tap to mark':st}</div>
    </div>`;
  }).join('');
  renderStats();
}

function cycleStatus(sid){
  const c=classes.find(x=>x.id===activeClassId);
  if(!attendance[c.id])attendance[c.id]={};
  const cur=attendance[c.id][sid]?.status||'none';
  const now=new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
  const s=c.students.find(x=>x.id===sid);
  if(cur==='none'){attendance[c.id][sid]={status:'present',time:now};logEntry(c,s,'present',now);}
  else if(cur==='present'){attendance[c.id][sid]={status:'absent',time:now};logEntry(c,s,'absent',now);}
  else{delete attendance[c.id][sid];}
  renderGrid();renderSidebar();if(activeTab==='table')renderTable();
}

function markAll(status){
  if(!activeClassId)return;
  const c=classes.find(x=>x.id===activeClassId);
  if(!attendance[c.id])attendance[c.id]={};
  const now=new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
  c.students.forEach(s=>{attendance[c.id][s.id]={status,time:now};});
  logEntry(c,{name:'All students'},status,now);
  renderGrid();renderStats();renderSidebar();if(activeTab==='table')renderTable();
}

function resetAll(){
  if(!activeClassId)return;
  attendance[activeClassId]={};
  renderGrid();renderStats();renderSidebar();
}

function renderTable(){
  if(!activeClassId)return;
  const c=classes.find(x=>x.id===activeClassId);
  const att=attendance[c.id]||{};
  document.getElementById('table-body').innerHTML=c.students.map((s,i)=>{
    const st=att[s.id]?.status||'none';
    const tm=att[s.id]?.time||'—';
    return`<tr>
      <td>${i+1}</td><td>${s.roll}</td><td>${s.name}</td>
      <td class="${st==='present'?'td-p':st==='absent'?'td-a':'td-n'}">${st==='none'?'—':st.charAt(0).toUpperCase()+st.slice(1)}</td>
      <td style="color:var(--ink3)">${tm}</td>
      <td>
        <button onclick="quickMark(${s.id},'present')" style="font-family:var(--mono);font-size:10px;padding:2px 8px;border:1px solid var(--present);color:var(--present);background:var(--present-bg);border-radius:5px;cursor:pointer;margin-right:4px;">P</button>
        <button onclick="quickMark(${s.id},'absent')" style="font-family:var(--mono);font-size:10px;padding:2px 8px;border:1px solid var(--absent);color:var(--absent);background:var(--absent-bg);border-radius:5px;cursor:pointer;">A</button>
      </td>
    </tr>`;
  }).join('');
}

function quickMark(sid,status){
  const c=classes.find(x=>x.id===activeClassId);
  if(!attendance[c.id])attendance[c.id]={};
  const now=new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
  const s=c.students.find(x=>x.id===sid);
  attendance[c.id][sid]={status,time:now};
  logEntry(c,s,status,now);
  renderTable();renderStats();renderSidebar();if(activeTab==='grid')renderGrid();
}

function logEntry(cls,student,status,time){
  globalLog.unshift({className:cls.name,classColor:cls.color,studentName:student.name,status,time});
  if(globalLog.length>120)globalLog.pop();
  if(activeTab==='log')renderLog();
}

function renderLog(){
  const el=document.getElementById('log-box');
  if(!globalLog.length){el.innerHTML='<div class="log-empty">No activity yet.</div>';return;}
  el.innerHTML=globalLog.map(l=>`
    <div class="log-entry">
      <span class="log-class-tag" style="background:${l.classColor}22;color:${l.classColor}">${l.className}</span>
      <div class="log-dot ${l.status}"></div>
      <div class="log-name">${l.studentName}</div>
      <span class="log-badge ${l.status}">${l.status==='present'?'Present':'Absent'}</span>
      <div class="log-time">${l.time}</div>
    </div>`).join('');
}

function clearLog(){globalLog=[];renderLog();}

function exportCSV(){
  if(!activeClassId)return;
  const c=classes.find(x=>x.id===activeClassId);
  const att=attendance[c.id]||{};
  let csv=`Class,${c.name}\nSubject,${c.subject}\nTeacher,${c.teacher}\nDate,${currentDate}\n\nRoll,Name,Status,Time\n`;
  c.students.forEach(s=>{csv+=`${s.roll},${s.name},${att[s.id]?.status||'not marked'},${att[s.id]?.time||'—'}\n`;});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(new Blob([csv],{type:'text/csv'}));
  a.download=`attendance_${c.name.replace(/\s/g,'_')}_${currentDate}.csv`;
  a.click();
}

function openAddClass(){
  selColor=COLORS[classes.length%COLORS.length];
  renderSwatches();
  ['ac-name','ac-subject','ac-teacher','ac-period','ac-room'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('ov-class').classList.add('open');
  setTimeout(()=>document.getElementById('ac-name').focus(),100);
}

function renderSwatches(){
  document.getElementById('color-swatches').innerHTML=COLORS.map(col=>`<div class="swatch${selColor===col?' sel':''}" style="background:${col}" onclick="selColor='${col}';renderSwatches()"></div>`).join('');
}

function submitAddClass(){
  const name=document.getElementById('ac-name').value.trim();
  const subject=document.getElementById('ac-subject').value.trim();
  const teacher=document.getElementById('ac-teacher').value.trim();
  const period=document.getElementById('ac-period').value.trim();
  const room=document.getElementById('ac-room').value.trim();
  if(!name||!teacher){alert('Class name and teacher are required.');return;}
  const nc={id:nextClassId++,name,subject:subject||'General',teacher,period:period||'—',room:room||'—',color:selColor,students:[]};
  classes.push(nc);
  attendance[nc.id]={};
  closeOv('ov-class');
  renderSidebar();
  renderOverview();
  selectClass(nc.id);
}

function openAddStudent(){
  if(!activeClassId)return;
  document.getElementById('as-name').value='';
  document.getElementById('as-roll').value='';
  document.getElementById('ov-student').classList.add('open');
  setTimeout(()=>document.getElementById('as-name').focus(),100);
}

function submitAddStudent(){
  if(!activeClassId)return;
  const name=document.getElementById('as-name').value.trim();
  const roll=document.getElementById('as-roll').value.trim();
  if(!name){document.getElementById('as-name').focus();return;}
  const c=classes.find(x=>x.id===activeClassId);
  const nextId=Math.max(0,...c.students.map(s=>s.id))+1;
  c.students.push({id:nextId,name,roll:roll||String(nextId).padStart(2,'0')});
  closeOv('ov-student');
  renderGrid();renderStats();renderTable();renderSidebar();
}

function changeDate(){
  currentDate=document.getElementById('dateInput').value;
  updateSidebarDate();
}

function closeOv(id,e){
  if(!e||e.target===document.getElementById(id))document.getElementById(id).classList.remove('open');
}

document.getElementById('as-name').addEventListener('keydown',e=>{if(e.key==='Enter')document.getElementById('as-roll').focus();});
document.getElementById('as-roll').addEventListener('keydown',e=>{if(e.key==='Enter')submitAddStudent();});
document.getElementById('ac-teacher').addEventListener('keydown',e=>{if(e.key==='Enter')document.getElementById('ac-period').focus();});

renderSidebar();
renderOverview();
renderStats();
</script>
</body>
</html>
