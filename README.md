<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mission JEE: Elite Tracker V16</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        
        :root { 
            --bg: #f8fafc; --card: #ffffff; --primary: #6366f1; --secondary: #f43f5e; 
            --text: #1e293b; --text-light: #64748b; --border: #e2e8f0;
            --math: #8b5cf6; --phy: #0ea5e9; --chem: #10b981; --star: #f59e0b; --test: #f43f5e;
        }
        
        body { 
            background: var(--bg); color: var(--text); font-family: 'Plus Jakarta Sans', sans-serif; 
            padding: 25px; margin: 0; display: flex; flex-direction: column; align-items: center; 
            background-image: radial-gradient(circle, #e2e8f0 1.2px, transparent 1.2px); background-size: 24px 24px;
        }

        .header-stats { max-width: 1450px; width: 100%; display: grid; grid-template-columns: repeat(4, 1fr); gap: 20px; margin-bottom: 25px; }
        .stat-card { background: var(--card); padding: 18px; border-radius: 20px; border: 1px solid var(--border); text-align: center; box-shadow: 0 4px 12px rgba(0,0,0,0.03); }
        .stat-val { font-size: 1.8rem; font-weight: 800; color: var(--text); letter-spacing: -1px; }
        .stat-lab { font-size: 0.75rem; color: var(--text-light); text-transform: uppercase; font-weight: 700; margin-bottom: 5px; }
        .sub-breakdown { display: flex; justify-content: space-around; margin-top: 12px; border-top: 1px solid #f1f5f9; padding-top: 12px; font-size: 0.8rem; font-weight: 700; }
        
        .app-container { max-width: 1450px; width: 100%; display: grid; grid-template-columns: 2.2fr 1fr; gap: 30px; }
        .calendar-card { background: var(--card); padding: 30px; border-radius: 24px; border: 1px solid var(--border); box-shadow: 0 10px 30px rgba(0,0,0,0.04); }
        .calendar-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px; font-size: 1.8rem; font-weight: 800; }
        .calendar-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 12px; }
        .day-name { text-align: center; color: var(--text-light); font-weight: 800; font-size: 0.8rem; text-transform: uppercase; padding-bottom: 15px; }
        
        .day { min-height: 150px; padding: 12px; background: #fff; border-radius: 18px; cursor: pointer; border: 1px solid var(--border); transition: 0.3s; position: relative; }
        .day:hover { border-color: var(--primary); background: #fdfdff; }
        .day.has-data { border-top: 5px solid var(--primary); }
        .day.today { border: 2px solid var(--primary); }
        .day-num { font-weight: 800; font-size: 1.2rem; }

        .star-display { color: var(--star); font-size: 0.8rem; margin-bottom: 8px; }
        .chap-tag { font-size: 0.65rem; padding: 4px 8px; border-radius: 8px; margin-top: 4px; font-weight: 700; color: white; display: block; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .test-tag { background: var(--test); font-size: 0.65rem; padding: 5px; border-radius: 8px; margin-top: 6px; font-weight: 800; color: white; text-align: center; }
        
        .side-panel { background: #fff; padding: 30px; border-radius: 24px; border: 1px solid var(--border); position: sticky; top: 25px; }
        .input-group { background: #f8fafc; padding: 15px; border-radius: 15px; margin-bottom: 12px; border: 1px solid var(--border); }
        label { font-weight: 800; color: var(--text-light); display: block; margin-bottom: 6px; font-size: 0.65rem; text-transform: uppercase; }
        input, textarea, select { width: 100%; padding: 10px; border: 1px solid var(--border); border-radius: 10px; margin-bottom: 8px; font-family: inherit; font-size: 0.9rem; }
        
        .tf-row { display: flex; gap: 8px; align-items: center; font-size: 0.7rem; font-weight: 800; background: white; padding: 8px; border-radius: 10px; border: 1px solid #e2e8f0; margin-bottom: 8px;}
        .tf-row input { width: 45px; margin: 0; }

        .binary-q { background: #fff; border: 1px solid var(--border); border-radius: 10px; padding: 8px; display: flex; align-items: center; justify-content: space-between; font-size: 0.75rem; font-weight: 700; margin-bottom: 6px; }
        .binary-q input { width: 18px; height: 18px; accent-color: var(--primary); }

        .star-rating { display: flex; gap: 8px; font-size: 2rem; cursor: pointer; color: #e2e8f0; margin-bottom: 15px; justify-content: center; }
        .star-rating .star.active { color: var(--star); }

        /* BUTTON FIX: Ensured Chemistry is visible */
        .sub-toggle { display: flex; gap: 8px; margin-bottom: 15px; flex-wrap: nowrap; }
        .sub-btn { flex: 1; padding: 10px 5px; border: 1px solid var(--border); border-radius: 12px; cursor: pointer; font-weight: 800; font-size: 0.65rem; background: #fff; transition: 0.2s; }
        .sub-btn.active-M { background: var(--math); color: white; border-color: var(--math); }
        .sub-btn.active-P { background: var(--phy); color: white; border-color: var(--phy); }
        .sub-btn.active-C { background: var(--chem); color: white; border-color: var(--chem); }

        .btn-save { background: var(--primary); border: none; width: 100%; padding: 15px; border-radius: 15px; color: white; font-weight: 800; cursor: pointer; font-size: 1rem; }
        .hidden { display: none; }
        .view-box { background: #f8fafc; padding: 12px; border-radius: 15px; border-left: 5px solid var(--primary); margin-bottom: 10px; font-size: 0.8rem; }
        .subject-note { background: #fff; border: 1px dashed var(--border); font-size: 0.7rem; padding: 8px; border-radius: 8px; margin-top: 5px; color: var(--text-light); }
    </style>
</head>
<body>

<div class="header-stats">
    <div class="stat-card"><div class="stat-lab">Weekly Total</div><div class="stat-val" id="weeklyTotal">0</div>
        <div class="sub-breakdown"><span style="color:var(--math)">M:<span id="wM">0</span></span><span style="color:var(--phy)">P:<span id="wP">0</span></span><span style="color:var(--chem)">C:<span id="wC">0</span></span></div></div>
    <div class="stat-card"><div class="stat-lab">Monthly Total</div><div class="stat-val" id="monthlyTotal">0</div>
        <div class="sub-breakdown"><span style="color:var(--math)">M:<span id="mM">0</span></span><span style="color:var(--phy)">P:<span id="mP">0</span></span><span style="color:var(--chem)">C:<span id="mC">0</span></span></div></div>
    <div class="stat-card"><div class="stat-lab">Effort Score</div><div class="stat-val" id="avgEffort">0.0</div></div>
    <div class="stat-card"><div class="stat-lab">Total Logs</div><div class="stat-val" id="streakVal">0</div></div>
</div>

<div class="app-container">
    <div class="calendar-card">
        <div class="calendar-header">
            <button onclick="changeMonth(-1)" style="border:none; background:none; cursor:pointer;">◀</button>
            <span id="monthDisplay"></span>
            <button onclick="changeMonth(1)" style="border:none; background:none; cursor:pointer;">▶</button>
        </div>
        <div class="calendar-grid" id="calendarGrid"></div>
    </div>

    <div class="side-panel">
        <h3 id="selectedDateText" style="margin-top:0; font-weight:800;">Select a date</h3>
        
        <div id="logArea" class="hidden">
            <div class="star-rating" id="starInput">
                <span class="star" onclick="setStars(1)">★</span><span class="star" onclick="setStars(2)">★</span><span class="star" onclick="setStars(3)">★</span><span class="star" onclick="setStars(4)">★</span><span class="star" onclick="setStars(5)">★</span>
            </div>
            
            <div class="sub-toggle">
                <button class="sub-btn" id="btnM" onclick="toggleSub('M')">MATHS</button>
                <button class="sub-btn" id="btnP" onclick="toggleSub('P')">PHYSICS</button>
                <button class="sub-btn" id="btnC" onclick="toggleSub('C')">CHEMISTRY</button>
            </div>
            
            <div id="boxM" class="hidden input-group" style="border-top: 4px solid var(--math)">
                <label>📐 Maths (Tarun Sir)</label>
                <input type="text" id="mCh" placeholder="Chapter Name">
                <input type="number" id="mQ" placeholder="Total Qs Practiced">
                <div class="tf-row">T/F: <input type="number" id="mCor" placeholder="OK"> / <input type="number" id="mOut" placeholder="Tot"></div>
                <div style="display:flex; gap:5px;"><div class="binary-q" style="flex:1">DPP <input type="checkbox" id="md"></div><div class="binary-q" style="flex:1">HW <input type="checkbox" id="mh"></div></div>
                <textarea id="mNote" placeholder="Math Notes..." style="height:40px;"></textarea>
            </div>

            <div id="boxP" class="hidden input-group" style="border-top: 4px solid var(--phy)">
                <label>🍎 Physics (Saleem Sir)</label>
                <input type="text" id="pCh" placeholder="Chapter Name">
                <input type="number" id="pQ" placeholder="Total Qs Practiced">
                <div style="display:flex; gap:5px;"><div class="binary-q" style="flex:1">DPP <input type="checkbox" id="pd"></div><div class="binary-q" style="flex:1">HW <input type="checkbox" id="ph"></div></div>
                <textarea id="pNote" placeholder="Physics Notes..." style="height:40px;"></textarea>
            </div>

            <div id="boxC" class="hidden input-group" style="border-top: 4px solid var(--chem)">
                <label>🧪 Chemistry</label>
                <select id="cType">
                    <option value="Physical">Physical (Faisal Sir)</option>
                    <option value="Inorganic">Inorganic (Om Pandey Sir)</option>
                    <option value="Organic">Organic (Ashutosh Sir)</option>
                </select>
                <input type="text" id="cCh" placeholder="Chapter Name">
                <input type="number" id="cQ" placeholder="Total Qs Practiced">
                <div style="display:flex; gap:5px;"><div class="binary-q" style="flex:1">DPP <input type="checkbox" id="cd"></div><div class="binary-q" style="flex:1">HW <input type="checkbox" id="ch"></div></div>
                <textarea id="cNote" placeholder="Chem Notes..." style="height:40px;"></textarea>
            </div>

            <div class="binary-q" style="border: 2px solid var(--test); color: var(--test);"><b>TEST DAY?</b> <input type="checkbox" id="isTest" onchange="document.getElementById('testFields').classList.toggle('hidden', !this.checked)"></div>
            <div id="testFields" class="hidden input-group" style="border-top: 4px solid var(--test)">
                <input type="text" id="tName" placeholder="Test Name">
                <input type="number" id="tTotal" placeholder="Max Marks">
                <div class="tf-row">M:<input type="number" id="tm"> P:<input type="number" id="tp"> C:<input type="number" id="tc"></div>
            </div>

            <textarea id="note" placeholder="Daily Journal..." style="height:50px; margin-top:10px;"></textarea>
            <button class="btn-save" onclick="saveData()">SAVE PROGRESS</button>
        </div>
        <div id="viewArea" class="hidden"></div>
    </div>
</div>

<script>
    let viewDate = new Date();
    let selectedKey = "";
    let effortStars = 0;
    let active = { M: false, P: false, C: false };

    function init() { render(); }
    function setStars(n) { effortStars = n; document.querySelectorAll('.star').forEach((s, i) => s.classList.toggle('active', i < n)); }
    
    function toggleSub(s) { 
        active[s] = !active[s]; 
        document.getElementById('btn'+s).classList.toggle('active-'+s); 
        document.getElementById('box'+s).classList.toggle('hidden'); 
    }

    function changeMonth(dir) { viewDate.setMonth(viewDate.getMonth() + dir); render(); }

    function render() {
        const grid = document.getElementById('calendarGrid');
        const history = JSON.parse(localStorage.getItem('iit_final_v16')) || {};
        const now = new Date();
        const todayKey = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')}`;
        const monWeek = new Date(now); monWeek.setDate(now.getDate() - (now.getDay() === 0 ? 6 : now.getDay() - 1)); monWeek.setHours(0,0,0,0);

        grid.innerHTML = "";
        document.getElementById('monthDisplay').innerText = viewDate.toLocaleString('default', { month: 'long', year: 'numeric' });
        ['Mon','Tue','Wed','Thu','Fri','Sat','Sun'].forEach(d => grid.innerHTML += `<div class="day-name">${d}</div>`);
        
        let firstDay = new Date(viewDate.getFullYear(), viewDate.getMonth(), 1).getDay();
        let offset = (firstDay === 0) ? 6 : firstDay - 1; 
        for(let i=0; i<offset; i++) grid.innerHTML += `<div></div>`;

        let mSt = {m:0, p:0, c:0, t:0}, wSt = {m:0, p:0, c:0, t:0}, logged = 0, starsTotal = 0;
        const daysInMonth = new Date(viewDate.getFullYear(), viewDate.getMonth() + 1, 0).getDate();

        for(let d=1; d<=daysInMonth; d++) {
            const curDate = new Date(viewDate.getFullYear(), viewDate.getMonth(), d);
            const key = `${viewDate.getFullYear()}-${String(viewDate.getMonth() + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`;
            const data = history[key];
            const isFuture = curDate > now && key !== todayKey;
            const dayDiv = document.createElement('div');
            dayDiv.className = `day ${data ? 'has-data' : ''} ${key === todayKey ? 'today' : ''}`;
            dayDiv.innerHTML = `<div class="day-num">${d}</div>`;
            
            if(data) {
                logged++; starsTotal += parseInt(data.stars);
                if(data.test) dayDiv.innerHTML += `<div class="test-tag">🏆 ${data.test.name}</div>`;
                ['m','p','c'].forEach(s => {
                    if(data[s]) {
                        const q = parseInt(data[s].q) || 0; mSt[s] += q; mSt.t += q;
                        if(curDate >= monWeek) { wSt[s] += q; wSt.t += q; }
                        dayDiv.innerHTML += `<div class="chap-tag" style="background:var(--${s==='m'?'math':s==='p'?'phy':'chem'})">${s.toUpperCase()}: ${q}Q</div>`;
                    }
                });
            }
            if(!isFuture) dayDiv.onclick = () => open(key);
            grid.appendChild(dayDiv);
        }
        document.getElementById('weeklyTotal').innerText = wSt.t;
        document.getElementById('wM').innerText = wSt.m; document.getElementById('wP').innerText = wSt.p; document.getElementById('wC').innerText = wSt.c;
        document.getElementById('monthlyTotal').innerText = mSt.t;
        document.getElementById('mM').innerText = mSt.m; document.getElementById('mP').innerText = mSt.p; document.getElementById('mC').innerText = mSt.c;
        document.getElementById('avgEffort').innerText = logged ? (starsTotal/logged).toFixed(1) : '0.0';
        document.getElementById('streakVal').innerText = logged;
    }

    function open(key) {
        selectedKey = key;
        const data = (JSON.parse(localStorage.getItem('iit_final_v16')) || {})[key];
        document.getElementById('selectedDateText').innerText = key;
        document.getElementById('logArea').classList.toggle('hidden', !!data);
        document.getElementById('viewArea').classList.toggle('hidden', !data);
        setStars(0);

        if(data) {
            let starH = ''; for(let i=0; i<5; i++) starH += (i < data.stars) ? '★' : '☆';
            document.getElementById('viewArea').innerHTML = `
                <div style="color:var(--star); font-size:1.8rem; text-align:center; margin-bottom:15px;">${starH}</div>
                ${data.test ? `<div class="view-box" style="border-left-color:var(--test); background:#fff5f5"><b>TEST: ${data.test.name}</b><br>Score: ${data.test.score}/${data.test.total} (M:${data.test.m} P:${data.test.p} C:${data.test.c})</div>` : ''}
                ${data.m ? `<div class="view-box" style="border-left-color:var(--math)"><b>Maths:</b> ${data.m.ch}<br>${data.m.q}Q (Acc: ${data.m.cor}/${data.m.out})${data.m.note ? `<div class="subject-note">${data.m.note}</div>` : ''}</div>` : ''}
                ${data.p ? `<div class="view-box" style="border-left-color:var(--phy)"><b>Physics:</b> ${data.p.ch}<br>${data.p.q}Q Practiced${data.p.note ? `<div class="subject-note">${data.p.note}</div>` : ''}</div>` : ''}
                ${data.c ? `<div class="view-box" style="border-left-color:var(--chem)"><b>Chem (${data.c.type}):</b> ${data.c.ch}<br>${data.c.q}Q Practiced${data.c.note ? `<div class="subject-note">${data.c.note}</div>` : ''}</div>` : ''}
                ${data.note ? `<div style="font-size:0.8rem; background:#fefce8; padding:10px; border-radius:12px;"><b>Journal:</b> ${data.note}</div>` : ''}
                <button onclick="deleteEntry('${key}')" style="color:red; border:none; background:none; cursor:pointer; font-size:0.7rem; margin-top:15px; width:100%;">[Delete Entry]</button>
            `;
        }
    }

    function deleteEntry(key) {
        if(confirm('Delete?')) {
            let h = JSON.parse(localStorage.getItem('iit_final_v16'));
            delete h[key];
            localStorage.setItem('iit_final_v16', JSON.stringify(h));
            location.reload();
        }
    }

    function saveData() {
        if(effortStars === 0) return alert("Select stars!");
        const history = JSON.parse(localStorage.getItem('iit_final_v16')) || {};
        history[selectedKey] = {
            stars: effortStars, note: document.getElementById('note').value,
            m: active.M ? { ch: document.getElementById('mCh').value, q: document.getElementById('mQ').value, cor: document.getElementById('mCor').value, out: document.getElementById('mOut').value, note: document.getElementById('mNote').value, d: document.getElementById('md').checked, h: document.getElementById('mh').checked } : null,
            p: active.P ? { ch: document.getElementById('pCh').value, q: document.getElementById('pQ').value, note: document.getElementById('pNote').value, d: document.getElementById('pd').checked, h: document.getElementById('ph').checked } : null,
            c: active.C ? { type: document.getElementById('cType').value, ch: document.getElementById('cCh').value, q: document.getElementById('cQ').value, note: document.getElementById('cNote').value, d: document.getElementById('cd').checked, h: document.getElementById('ch').checked } : null,
            test: document.getElementById('isTest').checked ? { name: document.getElementById('tName').value, total: document.getElementById('tTotal').value, m: document.getElementById('tm').value, p: document.getElementById('tp').value, c: document.getElementById('tc').value, score: (Number(document.getElementById('tm').value)+Number(document.getElementById('tp').value)+Number(document.getElementById('tc').value)) } : null
        };
        localStorage.setItem('iit_final_v16', JSON.stringify(history));
        location.reload();
    }
    init();
</script>
</body>
</html># JEE-TRACKER
FOR DAILY PROGRESS TRACKING
