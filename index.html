/* --- STATE --- */
let vaults = {};
let history = [];
let users = [];
let currentUser = null; 
let myChart = null;
let currentVaultName = null;
let editingUserIndex = null;
let editingVaultOldName = null;

const MAX_LIMIT = 1000000000000; // 1 Trillion Limit

try {
    vaults = JSON.parse(localStorage.getItem('srcs_vaults')) || {};
    history = JSON.parse(localStorage.getItem('srcs_history')) || [];
    users = JSON.parse(localStorage.getItem('srcs_users')) || [];
} catch(e) { localStorage.clear(); }

/* --- INIT --- */
document.addEventListener('DOMContentLoaded', function() {
    if (users.length === 0) {
        users.push({ username: 'admin', password: '123', role: 'superadmin' });
        localStorage.setItem('srcs_users', JSON.stringify(users));
    }
    
    // Default Title
    document.getElementById('pageTitle').innerText = "Overview";

    const sessionUser = sessionStorage.getItem('srcs_session');
    if(sessionUser) {
        currentUser = users.find(u => u.username === sessionUser);
        if(currentUser) showDashboard(); 
        else logout(); 
    } else { 
        showLogin(); 
    }
});

/* --- BACKUP & RESTORE (NEW) --- */
function backupData() {
    const data = {
        users: users,
        vaults: vaults,
        history: history
    };
    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(data));
    const downloadAnchorNode = document.createElement('a');
    downloadAnchorNode.setAttribute("href", dataStr);
    downloadAnchorNode.setAttribute("download", "SRCS_Backup_" + new Date().toISOString().slice(0,10) + ".json");
    document.body.appendChild(downloadAnchorNode);
    downloadAnchorNode.click();
    downloadAnchorNode.remove();
}

function restoreData(input) {
    const file = input.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(e) {
        try {
            const data = JSON.parse(e.target.result);
            if(data.users && data.vaults && data.history) {
                if(confirm("⚠ WARNING: This will overwrite current data. Continue?")) {
                    localStorage.setItem('srcs_users', JSON.stringify(data.users));
                    localStorage.setItem('srcs_vaults', JSON.stringify(data.vaults));
                    localStorage.setItem('srcs_history', JSON.stringify(data.history));
                    alert("Data Restored Successfully!");
                    location.reload();
                }
            } else {
                showAlert("Invalid Backup File", "Error");
            }
        } catch(err) {
            showAlert("Corrupt File", "Error");
        }
    };
    reader.readAsText(file);
}

/* --- TABS --- */
function switchTab(tabId) {
    document.querySelectorAll('.view-section').forEach(el => el.style.display = 'none');
    document.querySelectorAll('.nav-link').forEach(el => el.classList.remove('active'));
    
    const target = document.getElementById(`view-${tabId}`);
    if(target) target.style.display = 'block';
    
    const btns = document.querySelectorAll(`button[onclick="switchTab('${tabId}')"]`);
    if(btns.length > 0) btns[0].classList.add('active');

    const titles = { 'dashboard': 'Overview', 'manage': 'Manage Accounts', 'users': 'User Directory' };
    document.getElementById('pageTitle').innerText = titles[tabId];
}

/* --- VISIBILITY --- */
function showDashboard() {
    document.getElementById('auth-container').style.cssText = "display: none !important;";
    document.getElementById('dashboard-container').style.cssText = "display: flex !important;";
    document.getElementById('displayUsername').innerText = currentUser.username;
    document.getElementById('displayRole').innerText = currentUser.role;
    applyPermissions(); refreshUI(); renderUserTable(); switchTab('dashboard'); 
}

function showLogin() {
    document.getElementById('auth-container').style.cssText = "display: flex !important;";
    document.getElementById('dashboard-container').style.cssText = "display: none !important;";
}

function applyPermissions() {
    const role = currentUser.role;
    const manageNav = document.getElementById('nav-manage');
    const usersNav = document.getElementById('nav-users');
    const dangerZone = document.getElementById('danger-zone');
    
    manageNav.style.display = 'block'; usersNav.style.display = 'block';
    if(dangerZone) dangerZone.style.display = 'none';

    if (role === 'employee') manageNav.style.display = 'none';
    else if (role === 'admin') usersNav.style.display = 'none';
    else if (role === 'superadmin') if(dangerZone) dangerZone.style.display = 'block';
}

/* --- ALERTS --- */
function openOverlay(id) { document.getElementById(id).style.display = 'flex'; }
function closeOverlay(id) { document.getElementById(id).style.display = 'none'; }
function showAlert(msg, title = "Notification") {
    document.getElementById('alertTitle').innerText = title;
    document.getElementById('alertMessage').innerText = msg;
    openOverlay('alertOverlay');
}

/* --- AUTH --- */
function login() {
    const u = document.getElementById('loginUser').value.trim();
    const p = document.getElementById('loginPass').value.trim();
    const user = users.find(acc => acc.username === u && acc.password === p);
    if (user) { currentUser = user; sessionStorage.setItem('srcs_session', u); showDashboard(); } 
    else { showAlert("Invalid Credentials", "Login Failed"); }
}

function logout() { sessionStorage.removeItem('srcs_session'); location.reload(); }

function registerUser() {
    const u = document.getElementById('regUser').value.trim();
    const p = document.getElementById('regPass').value.trim();
    if(!u || !p) return showAlert("Fill fields", "Error");
    if(users.find(user => user.username === u)) return showAlert("Exists", "Error");
    users.push({ username: u, password: p, role: 'employee' });
    localStorage.setItem('srcs_users', JSON.stringify(users));
    showAlert("Created! Log In.", "Success"); toggleAuth();
}

function toggleAuth() {
    const l = document.getElementById('login-form'); const r = document.getElementById('register-form');
    if(l.style.display === 'none') { l.style.display = 'block'; r.style.display = 'none'; } 
    else { l.style.display = 'none'; r.style.display = 'block'; }
}

function togglePassword(id, btn) {
    const inp = document.getElementById(id); const icon = btn.querySelector('i');
    if(inp.type === "password") { inp.type = "text"; icon.className = "bi bi-eye-fill"; } 
    else { inp.type = "password"; icon.className = "bi bi-eye-slash-fill"; }
}

function checkEnter(e, act) { if(e.key === "Enter") act(); }

/* --- FINANCE LOGIC --- */
function addCategory() {
    const name = document.getElementById('catName').value.trim();
    const budget = cleanNumber(document.getElementById('catBudget').value);
    
    if(!name || budget <= 0) return showAlert("Invalid details", "Error");
    if(budget > MAX_LIMIT) return showAlert("Limit exceeded (Max 1 Trillion)", "Error");
    if(vaults[name]) return showAlert("Exists", "Error");
    
    vaults[name] = { initial: budget, balance: budget, spent: 0 };
    document.getElementById('catName').value = ""; document.getElementById('catBudget').value = "";
    saveData(); showAlert("Vault Created", "Success");
}

function deleteCategory(name) {
    if(confirm(`Delete ${name}?`)) { delete vaults[name]; history = history.filter(h => h.cat !== name); saveData(); }
}

function openEditVault(name) {
    if(currentUser.role === 'employee') return;
    editingVaultOldName = name;
    const v = vaults[name];
    document.getElementById('editVaultName').value = name;
    document.getElementById('editVaultBudget').value = v.initial;
    openOverlay('editVaultOverlay');
}

function saveVaultChanges() {
    const newName = document.getElementById('editVaultName').value.trim();
    const newBudget = cleanNumber(document.getElementById('editVaultBudget').value);
    
    if(!newName || newBudget <= 0) return showAlert("Invalid Input", "Error");
    if(newBudget > MAX_LIMIT) return showAlert("Limit exceeded (Max 1 Trillion)", "Error");
    if(newName !== editingVaultOldName && vaults[newName]) return showAlert("Name exists", "Error");

    const oldData = vaults[editingVaultOldName];
    vaults[newName] = { initial: newBudget, spent: oldData.spent, balance: newBudget - oldData.spent };

    if(newName !== editingVaultOldName) {
        delete vaults[editingVaultOldName];
        history.forEach(h => { if(h.cat === editingVaultOldName) h.cat = newName; });
    }

    closeOverlay('editVaultOverlay'); saveData(); showAlert("Account Updated", "Success");
}

function openModal(name) {
    if(currentUser.role === 'employee') return showAlert("View Only", "Denied");
    currentVaultName = name;
    document.getElementById('modalTitle').innerText = `Add Expense: ${name}`;
    document.getElementById('modalSubtitle').innerText = formatMoney(vaults[name].balance);
    document.getElementById('modalItem').value = ""; document.getElementById('modalCost').value = "";
    openOverlay('expenseOverlay');
    setTimeout(() => document.getElementById('modalItem').focus(), 100);
}

function submitModalExpense() {
    if (!currentVaultName) return;
    const item = document.getElementById('modalItem').value.trim();
    const cost = cleanNumber(document.getElementById('modalCost').value);
    if(!item || cost <= 0) return showAlert("Invalid", "Error");
    if(vaults[currentVaultName].balance < cost) return showAlert("Low Funds", "Error");
    vaults[currentVaultName].balance -= cost; vaults[currentVaultName].spent += cost;
    history.unshift({ id: Date.now(), cat: currentVaultName, item, cost, time: new Date().toLocaleTimeString() });
    closeOverlay('expenseOverlay'); saveData();
}

function deleteTransaction(id) {
    if(currentUser.role === 'employee') return;
    if(!confirm("Refund?")) return;
    const idx = history.findIndex(h => h.id === id);
    if(idx === -1) return;
    const tx = history[idx];
    if(vaults[tx.cat]) { vaults[tx.cat].balance += tx.cost; vaults[tx.cat].spent -= tx.cost; }
    history.splice(idx, 1); saveData();
}

/* --- USERS --- */
function createUser() {
    const u = document.getElementById('newUsername').value.trim();
    const p = document.getElementById('newPassword').value.trim();
    const r = document.getElementById('newRole').value;
    if(!u || !p) return showAlert("Fill fields", "Error");
    if(users.find(user => user.username === u)) return showAlert("Exists", "Error");
    users.push({ username: u, password: p, role: r });
    localStorage.setItem('srcs_users', JSON.stringify(users));
    document.getElementById('newUsername').value = ""; document.getElementById('newPassword').value = "";
    renderUserTable(); showAlert("User Created", "Success");
}

function deleteUser(username) {
    if(username === 'admin') return showAlert("Denied", "Error");
    if(confirm(`Delete ${username}?`)) {
        users = users.filter(u => u.username !== username);
        localStorage.setItem('srcs_users', JSON.stringify(users));
        renderUserTable();
    }
}

function openEditUser(username) {
    const idx = users.findIndex(u => u.username === username);
    if(idx === -1) return;
    editingUserIndex = idx;
    document.getElementById('editUsername').value = users[idx].username;
    document.getElementById('editPassword').value = users[idx].password;
    openOverlay('editUserOverlay');
}

function saveUserChanges() {
    if(editingUserIndex === null) return;
    const newU = document.getElementById('editUsername').value.trim();
    const newP = document.getElementById('editPassword').value.trim();
    if(!newU || !newP) return showAlert("Empty fields", "Error");
    const exists = users.find((u, i) => u.username === newU && i !== editingUserIndex);
    if(exists) return showAlert("Username taken", "Error");
    users[editingUserIndex].username = newU;
    users[editingUserIndex].password = newP;
    localStorage.setItem('srcs_users', JSON.stringify(users));
    renderUserTable(); closeOverlay('editUserOverlay'); showAlert("Updated", "Success");
}

function renderUserTable() {
    const tbody = document.getElementById('userTableBody');
    if(!tbody) return;
    tbody.innerHTML = '';
    users.forEach(u => {
        let actionHTML = u.username !== 'admin' 
            ? `<div class="d-flex justify-content-end gap-2">
                <button class="btn btn-sm btn-outline-warning" onclick="openEditUser('${u.username}')"><i class="bi bi-pencil-fill"></i></button>
                <button class="btn btn-sm btn-outline-danger" onclick="deleteUser('${u.username}')"><i class="bi bi-trash-fill"></i></button>
               </div>` 
            : `<i class="bi bi-lock-fill text-muted"></i>`;
        let row = `<tr><td><i class="bi bi-person-circle text-muted me-2"></i>${u.username}</td><td><span class="badge bg-secondary text-info">${u.role}</span></td><td class="text-end">${actionHTML}</td></tr>`;
        tbody.innerHTML += row;
    });
}

/* --- UTILS --- */
function formatMoney(num) { return "PHP " + parseFloat(num).toLocaleString('en-PH', {minimumFractionDigits: 2}); }
function formatMoneyPDF(num) { return "PHP " + parseFloat(num).toLocaleString('en-PH', {minimumFractionDigits: 2}); }
function cleanNumber(str) { return parseFloat(str.replace(/,/g, '')) || 0; }
function formatInput(input) { let val = input.value.replace(/,/g, '').replace(/[^0-9.]/g, ''); if(val) { let parts = val.split('.'); parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ","); input.value = parts.join('.'); } else { input.value = ""; } }
function saveData() { localStorage.setItem('srcs_vaults', JSON.stringify(vaults)); localStorage.setItem('srcs_history', JSON.stringify(history)); refreshUI(); }
function hardReset() { if(confirm("⚠ WIPE ALL DATA?")) { localStorage.clear(); location.reload(); } }

function refreshUI() {
    const vaultContainer = document.getElementById('vaultDisplay');
    const historyBody = document.getElementById('historyTableBody');
    if(!vaultContainer) return;
    vaultContainer.innerHTML = ''; historyBody.innerHTML = '';
    
    let totalBudget = 0, totalSpent = 0;
    let chartLabels = [], chartData = [], chartColors = [];
    const colorPalette = ['#06b6d4', '#a855f7', '#f472b6', '#3b82f6', '#10b981']; 
    let colorIndex = 0;

    Object.keys(vaults).forEach(key => {
        const v = vaults[key];
        totalBudget += v.initial; totalSpent += v.spent;
        let pct = v.initial > 0 ? (v.spent / v.initial) * 100 : 0;
        if(pct > 100) pct = 100;
        const color = pct > 90 ? '#ef4444' : colorPalette[colorIndex % colorPalette.length];
        colorIndex++;

        const safeKey = key.replace(/'/g, "\\'");
        const deleteBtn = currentUser.role !== 'employee' 
            ? `<div class="d-flex gap-2">
                 <button class="btn btn-sm btn-outline-warning py-0 px-2" style="font-size: 0.75rem;" onclick="event.stopPropagation(); openEditVault('${safeKey}')">Edit</button>
                 <button class="btn btn-sm btn-outline-danger py-0 px-2" style="font-size: 0.75rem;" onclick="event.stopPropagation(); deleteCategory('${safeKey}')">Delete</button>
               </div>` 
            : '';

        let col = document.createElement('div');
        col.className = 'col-md-6 col-lg-6';
        col.innerHTML = `
            <div class="vault-card" onclick="openModal('${safeKey}')" style="border-left-color: ${color}">
                <div class="card-header-flex">
                    <h5 class="fw-bold m-0 text-truncate text-white" style="max-width: 50%;" title="${key}">${key}</h5>
                    ${deleteBtn}
                </div>
                <div class="mt-2">
                    <div class="h3 fw-bold text-white mb-1 text-truncate">${formatMoney(v.balance)}</div>
                    <div class="progress-track"><div class="progress-fill" style="width: ${pct}%; background-color: ${color};"></div></div>
                    <div class="d-flex justify-content-between small text-muted"><span>Spent: ${formatMoney(v.spent)}</span><span>${pct.toFixed(0)}%</span></div>
                </div>
            </div>`;
        vaultContainer.appendChild(col);
        if(v.spent > 0) { chartLabels.push(key); chartData.push(v.spent); chartColors.push(color); }
    });

    document.getElementById('dashTotal').innerText = formatMoney(totalBudget);
    document.getElementById('dashSpent').innerText = formatMoney(totalSpent);
    document.getElementById('dashRemain').innerText = formatMoney(totalBudget - totalSpent);

    history.slice(0, 15).forEach(h => {
        const deleteAction = currentUser.role !== 'employee' ? `<button class="btn btn-sm btn-outline-danger py-0 px-2" onclick="deleteTransaction(${h.id})">Delete</button>` : '';
        let row = document.createElement('tr');
        row.innerHTML = `<td>${h.time}</td><td class="text-cyan">${h.cat}</td><td>${h.item}</td><td class="text-end text-danger fw-bold">-${formatMoney(h.cost)}</td><td class="text-center">${deleteAction}</td>`;
        historyBody.appendChild(row);
    });
    updateChart(chartLabels, chartData, chartColors);
}

function updateChart(labels, data, colors) {
    const ctx = document.getElementById('expenseChart');
    if (!ctx) return;
    if(myChart) myChart.destroy();
    
    if (labels.length > 0) {
        myChart = new Chart(ctx.getContext('2d'), {
            type: 'doughnut',
            data: { labels: labels, datasets: [{ data: data, backgroundColor: colors, borderWidth: 0, hoverOffset: 10 }] },
            options: { responsive: true, maintainAspectRatio: false, cutout: '75%', plugins: { legend: { position: 'bottom', labels: { color: '#a1a1aa', padding: 20, usePointStyle: true } } } }
        });
    }
}

async function exportPDF() {
    if(history.length === 0) return showAlert("No data.", "Error");
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    const pageWidth = doc.internal.pageSize.getWidth();
    const dateStr = new Date().toLocaleDateString();

    doc.setFillColor(15, 23, 42); doc.rect(0, 0, pageWidth, 40, 'F');
    doc.setTextColor(6, 182, 212); doc.setFontSize(22); doc.setFont("helvetica", "bold");
    doc.text("SRCS FINANCIAL REPORT", 14, 20);
    doc.setTextColor(200); doc.setFontSize(10); doc.setFont("helvetica", "normal");
    doc.text(`Generated on: ${new Date().toLocaleString()}`, 14, 28);

    let totalBudget = 0, totalSpent = 0;
    Object.values(vaults).forEach(v => { totalBudget += v.initial; totalSpent += v.spent; });
    let totalRemain = totalBudget - totalSpent;

    const cardY = 50; const cardW = (pageWidth - 40) / 3;
    doc.setFillColor(245, 245, 245); doc.rect(14, cardY, cardW, 20, 'F'); doc.rect(14 + cardW + 5, cardY, cardW, 20, 'F'); doc.rect(14 + (cardW + 5) * 2, cardY, cardW, 20, 'F');
    doc.setFontSize(8); doc.setTextColor(100);
    doc.text("TOTAL BUDGET", 18, cardY + 6); doc.text("TOTAL SPENT", 18 + cardW + 5, cardY + 6); doc.text("REMAINING", 18 + (cardW + 5) * 2, cardY + 6);
    doc.setFontSize(11); doc.setTextColor(0); doc.text(formatMoneyPDF(totalBudget), 18, cardY + 14); doc.setTextColor(220, 20, 60); doc.text(formatMoneyPDF(totalSpent), 18 + cardW + 5, cardY + 14); doc.setTextColor(34, 139, 34); doc.text(formatMoneyPDF(totalRemain), 18 + (cardW + 5) * 2, cardY + 14);

    let nextY = 85;
    doc.setTextColor(0); doc.setFontSize(14); doc.setFont("helvetica", "bold");
    doc.text("BUDGET UTILIZATION", 14, nextY); nextY += 10;
    const barWidth = pageWidth - 28;
    Object.keys(vaults).forEach(name => {
        const v = vaults[name];
        let pct = v.initial > 0 ? (v.spent / v.initial) : 0;
        if(pct > 1) pct = 1;
        doc.setFontSize(10); doc.setTextColor(50); doc.setFont("helvetica", "bold");
        doc.text(name, 14, nextY);
        doc.setFont("helvetica", "normal"); doc.setTextColor(100);
        const stats = `${formatMoneyPDF(v.spent)} / ${formatMoneyPDF(v.initial)} (${(pct*100).toFixed(0)}%)`;
        const textWidth = doc.getTextWidth(stats);
        doc.text(stats, pageWidth - 14 - textWidth, nextY);
        nextY += 3;
        doc.setFillColor(230, 230, 230); doc.rect(14, nextY, barWidth, 6, 'F');
        if(pct > 0) { if(pct > 0.9) doc.setFillColor(239, 68, 68); else doc.setFillColor(6, 182, 212); doc.rect(14, nextY, barWidth * pct, 6, 'F'); }
        nextY += 12;
    });
    nextY += 10;
    const rows = history.map(row => [row.cat, row.item, formatMoneyPDF(row.cost), row.time]);
    doc.autoTable({ head: [["Category", "Item", "Cost", "Time"]], body: rows, startY: nextY, theme: 'grid', headStyles: { fillColor: [20, 20, 20], textColor: [6, 182, 212] } });
    doc.save(`SRCS_Report_${dateStr.replace(/\//g, '-')}.pdf`);
}