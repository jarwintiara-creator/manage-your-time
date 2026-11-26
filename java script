// State Global
let currentUser = null;
let currentFilter = 'all';
let deadlineInterval = null;

// Elemen DOM
const authContainer = document.getElementById('auth-container');
const dashboardContainer = document.getElementById('dashboard-container');
const authMessage = document.getElementById('auth-message');
const notifAlertBox = document.getElementById('notif-alert-box');

// Inisialisasi Aplikasi saat Halaman Dimuat
document.addEventListener('DOMContentLoaded', () => {
    const savedSession = localStorage.getItem('myt_active_user');
    if (savedSession) {
        currentUser = JSON.parse(savedSession);
        showDashboard();
    }
});

// Meminta Izin Notifikasi Browser
function requestNotificationPermission() {
    if (!('Notification' in window)) {
        alert('Browser ini tidak mendukung notifikasi sistem. Pengingat tetap tampil di dalam aplikasi.');
        return;
    }

    if (Notification.permission === 'granted') {
        updateNotifButtonUI();
        return;
    }

    Notification.requestPermission().then((permission) => {
        updateNotifButtonUI();
        if (permission === 'granted') {
            new Notification('Manage Your Time', {
                body: 'Notifikasi tenggat waktu kegiatan asrama telah diaktifkan!'
            });
        }
    });
}

// Perbarui Tampilan Tombol Izin Notifikasi
function updateNotifButtonUI() {
    const btn = document.getElementById('btn-notif-perm');
    if (!btn || !('Notification' in window)) return;

    if (Notification.permission === 'granted') {
        btn.textContent = 'Notif Aktif';
        btn.classList.add('active');
    } else if (Notification.permission === 'denied') {
        btn.textContent = 'Notif Diblokir';
        btn.classList.remove('active');
    } else {
        btn.textContent = 'Aktifkan Notif';
        btn.classList.remove('active');
    }
}

// Berpindah Tab Login / Daftar
function switchAuthTab(tab) {
    authMessage.textContent = '';
    if (tab === 'login') {
        document.getElementById('tab-login').classList.add('active');
        document.getElementById('tab-register').classList.remove('active');
        document.getElementById('login-form').classList.remove('hidden');
        document.getElementById('register-form').classList.add('hidden');
    } else {
        document.getElementById('tab-register').classList.add('active');
        document.getElementById('tab-login').classList.remove('active');
        document.getElementById('register-form').classList.remove('hidden');
        document.getElementById('login-form').classList.add('hidden');
    }
}

// Proses Pendaftaran Akun Baru
document.getElementById('register-form').addEventListener('submit', (e) => {
    e.preventDefault();
    const fullname = document.getElementById('reg-fullname').value.trim();
    const room = document.getElementById('reg-room').value.trim();
    const username = document.getElementById('reg-username').value.trim().toLowerCase();
    const password = document.getElementById('reg-password').value;

    let users = JSON.parse(localStorage.getItem('myt_users')) || [];

    if (users.some(u => u.username === username)) {
        authMessage.style.color = '#ef4444';
        authMessage.textContent = 'Username sudah terdaftar. Gunakan username lain.';
        return;
    }

    const newUser = { fullname, room, username, password };
    users.push(newUser);
    localStorage.setItem('myt_users', JSON.stringify(users));

    authMessage.style.color = '#10b981';
    authMessage.textContent = 'Akun berhasil dibuat! Silakan masuk.';
    document.getElementById('register-form').reset();
    setTimeout(() => switchAuthTab('login'), 1200);
});

// Proses Login Pengguna
document.getElementById('login-form').addEventListener('submit', (e) => {
    e.preventDefault();
    const username = document.getElementById('login-username').value.trim().toLowerCase();
    const password = document.getElementById('login-password').value;

    let users = JSON.parse(localStorage.getItem('myt_users')) || [];
    const validUser = users.find(u => u.username === username && u.password === password);

    if (validUser) {
        currentUser = validUser;
        localStorage.setItem('myt_active_user', JSON.stringify(currentUser));
        document.getElementById('login-form').reset();
        showDashboard();
    } else {
        authMessage.style.color = '#ef4444';
        authMessage.textContent = 'Username atau kata sandi salah!';
    }
});

// Menampilkan Dashboard & Memulai Pemantauan Tenggat Waktu
function showDashboard() {
    authContainer.classList.add('hidden');
    dashboardContainer.classList.remove('hidden');

    document.getElementById('welcome-text').textContent = `Halo, ${currentUser.fullname}!`;
    document.getElementById('room-badge').textContent = `Kamar: ${currentUser.room}`;
    updateNotifButtonUI();
    renderTasks();
    checkDeadlines();

    if (deadlineInterval) clearInterval(deadlineInterval);
    deadlineInterval = setInterval(() => {
        checkDeadlines();
        renderTasks();
    }, 15000);
}

// Proses Logout
function logout() {
    if (deadlineInterval) clearInterval(deadlineInterval);
    currentUser = null;
    localStorage.removeItem('myt_active_user');
    notifAlertBox.classList.add('hidden');
    notifAlertBox.innerHTML = '';
    dashboardContainer.classList.add('hidden');
    authContainer.classList.remove('hidden');
    authMessage.textContent = '';
}

// Mengambil Data Tugas Milik Pengguna yang Sedang Login
function getUserTasks() {
    const key = `myt_tasks_${currentUser.username}`;
    return JSON.parse(localStorage.getItem(key)) || [];
}

// Menyimpan Data Tugas Milik Pengguna yang Sedang Login
function saveUserTasks(tasks) {
    const key = `myt_tasks_${currentUser.username}`;
    localStorage.setItem(key, JSON.stringify(tasks));
}

// Menambah Tugas Baru
document.getElementById('todo-form').addEventListener('submit', (e) => {
    e.preventDefault();
    const title = document.getElementById('task-title').value.trim();
    const category = document.getElementById('task-category').value;
    const deadline = document.getElementById('task-deadline').value;

    if (!title || !deadline) return;

    const tasks = getUserTasks();
    const newTask = {
        id: Date.now(),
        title,
        category,
        deadline,
        completed: false,
        notified: false,
        alertDismissed: false
    };

    tasks.push(newTask);
    saveUserTasks(tasks);
    document.getElementById('todo-form').reset();
    renderTasks();
    checkDeadlines();
});

// Mengubah Status Selesai / Belum Selesai
function toggleTask(id) {
    const tasks = getUserTasks();
    const updated = tasks.map(task => {
        if (task.id === id) {
            const nextCompleted = !task.completed;
            return {
                ...task,
                completed: nextCompleted,
                alertDismissed: nextCompleted ? true : task.alertDismissed
            };
        }
        return task;
    });
    saveUserTasks(updated);
    renderTasks();
    renderAlertBanner();
}

// Menghapus Tugas
function deleteTask(id) {
    const tasks = getUserTasks();
    const filtered = tasks.filter(task => task.id !== id);
    saveUserTasks(filtered);
    renderTasks();
    renderAlertBanner();
}

// Menutup Satu Item Peringatan di Banner
function dismissAlert(id) {
    const tasks = getUserTasks();
    const updated = tasks.map(task =>
        task.id === id ? { ...task, alertDismissed: true } : task
    );
    saveUserTasks(updated);
    renderAlertBanner();
}

// Mengecek Tenggat Waktu Tugas yang Sudah Selesai / Terlewat
function checkDeadlines() {
    if (!currentUser) return;
    const tasks = getUserTasks();
    const now = new Date();
    let hasChanged = false;

    const updated = tasks.map(task => {
        const deadlineReached = !task.completed && new Date(task.deadline) <= now;
        if (deadlineReached && !task.notified) {
            hasChanged = true;
            sendSystemNotification(task);
            return { ...task, notified: true, alertDismissed: false };
        }
        return task;
    });

    if (hasChanged) {
        saveUserTasks(updated);
    }
    renderAlertBanner();
}

// Mengirim Notifikasi Browser
function sendSystemNotification(task) {
    if (!('Notification' in window)) return;
    if (Notification.permission === 'granted') {
        new Notification('Tenggat Waktu Terlewat - Manage Your Time', {
            body: `Kegiatan "${task.title}" (${task.category}) telah mencapai batas waktu.`
        });
    }
}

// Menampilkan Banner Notifikasi di Dalam Dashboard
function renderAlertBanner() {
    if (!currentUser) return;
    const tasks = getUserTasks();
    const now = new Date();

    const activeAlerts = tasks.filter(
        task => !task.completed && new Date(task.deadline) <= now && !task.alertDismissed
    );

    if (activeAlerts.length === 0) {
        notifAlertBox.classList.add('hidden');
        notifAlertBox.innerHTML = '';
        return;
    }

    notifAlertBox.classList.remove('hidden');
    notifAlertBox.innerHTML = activeAlerts.map(task => `
        <div class="notif-item">
            <span><strong>Pengingat Tenggat Waktu:</strong> "${task.title}" sudah melewati batas waktu (${formatDeadline(task.deadline)}).</span>
            <button class="btn-dismiss" onclick="dismissAlert(${task.id})">Tutup</button>
        </div>
    `).join('');
}

// Filter Tampilan Tugas
function filterTasks(filterType, btnElement) {
    currentFilter = filterType;
    document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
    btnElement.classList.add('active');
    renderTasks();
}

// Format Tanggal Tenggat Waktu
function formatDeadline(dateString) {
    const options = { day: 'numeric', month: 'short', year: 'numeric', hour: '2-digit', minute: '2-digit' };
    return new Date(dateString).toLocaleDateString('id-ID', options);
}

// Render Daftar Tugas ke Layar
function renderTasks() {
    const tasks = getUserTasks();
    const taskListEl = document.getElementById('task-list');
    taskListEl.innerHTML = '';

    // Update Statistik
    const total = tasks.length;
    const completedCount = tasks.filter(t => t.completed).length;
    const pendingCount = total - completedCount;

    document.getElementById('stat-total').textContent = total;
    document.getElementById('stat-pending').textContent = pendingCount;
    document.getElementById('stat-done').textContent = completedCount;

    // Urutkan berdasarkan tenggat waktu terdekat
    tasks.sort((a, b) => new Date(a.deadline) - new Date(b.deadline));

    // Terapkan Filter
    const filteredTasks = tasks.filter(task => {
        if (currentFilter === 'pending') return !task.completed;
        if (currentFilter === 'completed') return task.completed;
        return true;
    });

    if (filteredTasks.length === 0) {
        taskListEl.innerHTML = `<li style="text-align:center; color:#64748b; padding:20px; font-size:14px;">Belum ada daftar kegiatan pada kategori ini.</li>`;
        return;
    }

    const now = new Date();

    filteredTasks.forEach(task => {
        const isOverdue = !task.completed && new Date(task.deadline) < now;
        const li = document.createElement('li');
        li.className = `task-item ${task.completed ? 'completed' : ''}`;

        li.innerHTML = `
            <div class="task-left">
                <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''} onchange="toggleTask(${task.id})">
                <div>
                    <div class="task-title">${task.title}</div>
                    <div class="task-meta">
                        <span class="tag-category">${task.category}</span>
                        <span class="tag-deadline ${isOverdue ? 'overdue' : ''}">
                            ⏰ ${formatDeadline(task.deadline)} ${isOverdue ? '(Terlewat!)' : ''}
                        </span>
                    </div>
                </div>
            </div>
            <button class="btn-delete" onclick="deleteTask(${task.id})">Hapus</button>
        `;
        taskListEl.appendChild(li);
    });
}
