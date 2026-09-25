<!DOCTYPE html>

<html lang="pl">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Stock Control Task Board</title>

    <link rel="stylesheet" href=https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css>

    <style>

        :root {

            --primary-color: #4a6fa5;

            --secondary-color: #166088;

            --accent-color: #4fc3f7;

            --background-color: #f8f9fa;

            --card-color: #ffffff;

            --text-color: #333333;

            --text-light: #666666;

            --success-color: #4caf50;

            --warning-color: #ff9800;

            --danger-color: #f44336;

            --border-radius: 12px;

            --box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);

            --transition: all 0.4s cubic-bezier(0.25, 0.8, 0.25, 1);

            --sidebar-width: 300px;

        }

 

        /* Tryb ciemny */

        :root.dark {

            --primary-color: #6ba3d6;

            --secondary-color: #4a8bc6;

            --accent-color: #4fc3f7;

            --background-color: #121212;

            --card-color: #1e1e1e;

            --text-color: #e0e0e0;

            --text-light: #a0a0a0;

            --success-color: #66bb6a;

            --warning-color: #ffb74d;

            --danger-color: #ef5350;

            --box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);

        }

 

        * {

            margin: 0;

            padding: 0;

            box-sizing: border-box;

            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

        }

 

        body {

            background-color: var(--background-color);

            color: var(--text-color);

            line-height: 1.6;

            min-height: 100vh;

            transition: var(--transition);

        }

 

        .container {

            max-width: 1200px;

            margin: 0 auto;

            padding: 20px;

        }

 

        /* Przycisk trybu ciemnego/jałowego */

        .theme-toggle {

            position: fixed;

            top: 20px;

            right: 20px;

            z-index: 1000;

            background-color: var(--primary-color);

            color: white;

            border: none;

            width: 50px;

            height: 50px;

            border-radius: 50%;

            cursor: pointer;

            display: flex;

            align-items: center;

            justify-content: center;

            box-shadow: var(--box-shadow);

            transition: var(--transition);

        }

 

        .theme-toggle:hover {

            transform: scale(1.1);

            background-color: var(--secondary-color);

        }

 

        /* Sekcja logowania */

        .login-section {

            display: flex;

            justify-content: center;

            align-items: center;

            min-height: 100vh;

            background-color: var(--background-color);

            transition: var(--transition);

        }

 

        .login-box {

            background-color: var(--card-color);

            padding: 40px;

            border-radius: var(--border-radius);

            box-shadow: var(--box-shadow);

            width: 100%;

            max-width: 400px;

            text-align: center;

            transition: var(--transition);

        }

 

        .login-box h2 {

            color: var(--primary-color);

            margin-bottom: 30px;

            font-size: 2rem;

        }

 

        .login-box .form-group {

            margin-bottom: 20px;

            text-align: left;

        }

 

        .login-box label {

            display: block;

            margin-bottom: 8px;

            font-weight: 500;

            color: var(--text-color);

        }

 

        .login-box input {

            width: 100%;

            padding: 12px;

            border: 1px solid #ddd;

            border-radius: var(--border-radius);

            font-size: 16px;

            background-color: var(--card-color);

            color: var(--text-color);

            transition: var(--transition);

        }

 

        .login-box input:focus {

            border-color: var(--accent-color);

            outline: none;

            box-shadow: 0 0 0 3px rgba(79, 195, 247, 0.2);

        }

 

        .login-box button {

            width: 100%;

            background-color: var(--primary-color);

            color: white;

            border: none;

            padding: 12px;

            border-radius: var(--border-radius);

            font-size: 16px;

            font-weight: 500;

            cursor: pointer;

            transition: var(--transition);

            margin-top: 10px;

        }

 

        .login-box button:hover {

            background-color: var(--secondary-color);

            transform: translateY(-2px);

        }

 

        #errorMessage {

            color: var(--danger-color);

            margin-top: 15px;

            font-weight: 500;

        }

 

        /* Główna zawartość strony */

        #mainContent {

            display: none;

        }

 

        .header {

            display: flex;

            justify-content: space-between;

            align-items: center;

            margin-bottom: 30px;

            flex-wrap: wrap;

            gap: 20px;

        }

 

        .header h1 {

            font-size: 2.2rem;

            color: var(--primary-color);

            margin: 0;

        }

 

        .user-info {

            display: flex;

            align-items: center;

            gap: 10px;

            background-color: var(--card-color);

            padding: 10px 20px;

            border-radius: var(--border-radius);

            box-shadow: var(--box-shadow);

        }

 

        .user-info i {

            font-size: 20px;

            color: var(--primary-color);

        }

 

        .user-info span {

            font-weight: 500;

        }

 

        .logout-btn {

            background-color: var(--danger-color);

            margin-left: 10px;

        }

 

        .logout-btn:hover {

            background-color: #d32f2f;

        }

 

        /* Formularz dodawania zadania */

        .task-form {

            background-color: var(--card-color);

            padding: 30px;

            border-radius: var(--border-radius);

            box-shadow: var(--box-shadow);

            margin-bottom: 30px;

            transition: var(--transition);

        }

 

        .task-form h2 {

            color: var(--secondary-color);

            margin-bottom: 20px;

            font-size: 1.5rem;

            border-bottom: 2px solid var(--accent-color);

            padding-bottom: 10px;

        }

 

        .form-group {

            margin-bottom: 20px;

        }

 

        .form-group label {

            display: block;

            margin-bottom: 8px;

            font-weight: 500;

            color: var(--text-color);

        }

 

        .form-group input,

        .form-group textarea {

            width: 100%;

            padding: 12px;

            border: 1px solid #ddd;

            border-radius: var(--border-radius);

            font-size: 16px;

            background-color: var(--card-color);

            color: var(--text-color);

            transition: var(--transition);

        }

 

        .form-group input:focus,

        .form-group textarea:focus {

            border-color: var(--accent-color);

            outline: none;

            box-shadow: 0 0 0 3px rgba(79, 195, 247, 0.2);

        }

 

        .form-group textarea {

            resize: vertical;

            min-height: 100px;

        }

 

        /* Filtr zadań */

        .status-filter {

            margin-bottom: 20px;

            display: flex;

            align-items: center;

            gap: 15px;

            flex-wrap: wrap;

        }

 

        .status-filter label {

            font-weight: 500;

            color: var(--text-color);

        }

 

        .status-filter select {

            padding: 10px 15px;

            border-radius: var(--border-radius);

            border: 1px solid #ddd;

            background-color: var(--card-color);

            color: var(--text-color);

            cursor: pointer;

            transition: var(--transition);

        }

 

        .status-filter select:focus {

            outline: none;

            border-color: var(--accent-color);

            box-shadow: 0 0 0 3px rgba(79, 195, 247, 0.2);

        }

 

        /* Tabele z zadaniami */

        table {

            width: 100%;

            border-collapse: collapse;

            margin-top: 20px;

            background-color: var(--card-color);

            border-radius: var(--border-radius);

            overflow: hidden;

            box-shadow: var(--box-shadow);

            transition: var(--transition);

        }

 

        th, td {

            padding: 15px;

            text-align: left;

            border-bottom: 1px solid #ddd;

        }

 

        th {

            background-color: var(--primary-color);

            color: white;

            font-weight: 600;

        }

 

        tr {

            transition: var(--transition);

        }

 

        tr:hover {

            background-color: rgba(74, 111, 165, 0.08);

        }

 

        .status {

            display: inline-block;

            padding: 6px 12px;

            border-radius: 20px;

            font-size: 14px;

            font-weight: 500;

            text-transform: capitalize;

        }

 

        .status.pending {

            background-color: rgba(255, 152, 0, 0.2);

            color: var(--warning-color);

        }

 

        .status.completed {

            background-color: rgba(76, 175, 80, 0.2);

            color: var(--success-color);

        }

 

        .status.overdue {

            background-color: rgba(244, 67, 54, 0.2);

            color: var(--danger-color);

        }

 

        /* Przyciski akcji */

        .actions {

            display: flex;

            gap: 8px;

            flex-wrap: wrap;

        }

 

        .actions button {

            padding: 8px 12px;

            font-size: 14px;

            border-radius: var(--border-radius);

            cursor: pointer;

            transition: var(--transition);

            display: flex;

            align-items: center;

            gap: 5px;

        }

 

        .actions .complete-btn {

            background-color: var(--success-color);

        }

 

        .actions .complete-btn:hover {

            background-color: #3e8e41;

        }

 

        .actions .archive-btn {

            background-color: var(--accent-color);

        }

 

        .actions .archive-btn:hover {

            background-color: #3ab7f5;

        }

 

        .actions .delete-btn {

            background-color: var(--danger-color);

        }

 

        .actions .delete-btn:hover {

            background-color: #d32f2f;

        }

 

        .actions .restore-btn {

            background-color: #9e9e9e;

        }

 

        .actions .restore-btn:hover {

            background-color: #757575;

        }

 

        /* Archiwum */

        .archive-section {

            margin-top: 40px;

        }

 

        .archive-section h2 {

            color: var(--secondary-color);

            margin-bottom: 20px;

            font-size: 1.5rem;

            border-bottom: 2px solid var(--accent-color);

            padding-bottom: 10px;

        }

 

        /* Animacje */

        @keyframes fadeIn {

            from { opacity: 0; transform: translateY(20px); }

            to { opacity: 1; transform: translateY(0); }

        }

 

        .login-box, #mainContent {

            animation: fadeIn 0.5s ease-out;

        }

 

        /* Responsywność */

        @media (max-width: 768px) {

            .container {

                padding: 15px;

            }

 

            th, td {

                padding: 12px 10px;

                font-size: 14px;

            }

 

            .header {

                flex-direction: column;

                align-items: flex-start;

            }

 

            .user-info {

                width: 100%;

                justify-content: space-between;

            }

 

            .task-form, .login-box {

                padding: 25px;

            }

        }

    </style>

</head>

<body>

    <!-- Przycisk trybu ciemnego/jałowego -->

    <button class="theme-toggle" onclick="toggleTheme()">

        <i class="fas fa-moon"></i>

    </button>

 

    <!-- Sekcja logowania -->

    <div id="loginSection" class="login-section">

        <div class="login-box">

            <h2>🔒 Zaloguj się</h2>

            <form id="loginForm">

                <div class="form-group">

                    <label for="password">Hasło:</label>

                    <input type="password" id="password" placeholder="WPISZ HASŁO" required>

                </div>

                <button type="button" onclick="login()">

                    <i class="fas fa-sign-in-alt"></i> Zaloguj

                </button>

            </form>

            <p id="errorMessage"></p>

        </div>

    </div>

 

    <!-- Główna zawartość strony -->

    <div id="mainContent" class="container">

        <div class="header">

            <h1>📋 Stock Control Task Board</h1>

            <div class="user-info">

                <i class="fas fa-user-circle"></i>

                <span>Zalogowany użytkownik</span>

                <button class="logout-btn" onclick="logout()">

                    <i class="fas fa-sign-out-alt"></i> Wyloguj

                </button>

            </div>

        </div>

 

        <div class="task-form">

            <h2>📝 Dodaj nowe zadanie</h2>

            <form id="taskForm">

                <div class="form-group">

                    <label for="orderDate">Data zlecenia:</label>

                    <input type="date" id="orderDate" required>

                </div>

                <div class="form-group">

                    <label for="assignedTo">Komu zlecono:</label>

                    <input type="text" id="assignedTo" placeholder="Imię lub dział" required>

                </div>

                <div class="form-group">

                    <label for="assignedBy">Kto zlecił:</label>

                    <input type="text" id="assignedBy" placeholder="Imię lub dział" required>

                </div>

                <div class="form-group">

                    <label for="description">Opis zadania:</label>

                    <textarea id="description" rows="3" placeholder="Opis zadania" required></textarea>

                </div>

                <div class="form-group">

                    <label for="deadline">Data realizacji (do kiedy):</label>

                    <input type="date" id="deadline" required>

                </div>

                <div class="form-group">

                    <label for="notes">Uwagi:</label>

                    <textarea id="notes" rows="2" placeholder="Dodatkowe uwagi"></textarea>

                </div>

                <button type="button" onclick="addTask()">

                    <i class="fas fa-plus-circle"></i> Dodaj zadanie

                </button>

            </form>

        </div>

 

        <div class="status-filter">

            <label for="statusFilter">Filtruj po statusie:</label>

            <select id="statusFilter" onchange="filterTasks()">

                <option value="all">Wszystkie</option>

                <option value="completed">Zakończone</option>

                <option value="pending">Oczekujące</option>

                <option value="overdue">Przeterminowane</option>

            </select>

        </div>

 

        <h2>📋 Lista zadań</h2>

        <table id="tasksTable">

            <thead>

                <tr>

                    <th>Data zlecenia</th>

                    <th>Komu zlecono</th>

                    <th>Kto zlecił</th>

                    <th>Opis zadania</th>

                    <th>Data realizacji</th>

                    <th>Status</th>

                    <th>Uwagi</th>

                    <th>Akcje</th>

                </tr>

            </thead>

            <tbody id="tasksBody">

                <!-- Zadania będą wpisywane tutaj przez JavaScript -->

            </tbody>

        </table>

 

        <div class="archive-section">

            <h2>🗃️ Archiwum zadań</h2>

            <table id="archiveTable">

                <thead>

                    <tr>

                        <th>Data zlecenia</th>

                        <th>Komu zlecono</th>

                        <th>Kto zlecił</th>

                        <th>Opis zadania</th>

                        <th>Data realizacji</th>

                        <th>Data zakończenia</th>

                        <th>Uwagi</th>

                        <th>Akcje</th>

                    </tr>

                </thead>

                <tbody id="archiveBody">

                    <!-- Zarchiwizowane zadania będą wpisywane tutaj przez JavaScript -->

                </tbody>

            </table>

        </div>

    </div>

 

    <script>

        // Hasło (zmień je na swoje!)

        const PASSWORD = "Kontrola";

        let tasks = [];

        let isLoggedIn = false;

 

        // Funkcja logowania

        function login() {

            const enteredPassword = document.getElementById("password").value;

            const errorMessage = document.getElementById("errorMessage");

 

            if (enteredPassword === PASSWORD) {

                isLoggedIn = true;

                // Zapisz, że użytkownik jest zalogowany

                localStorage.setItem('stockControlLoggedIn', 'true');

                localStorage.setItem('stockControlPassword', PASSWORD);

                document.getElementById("loginSection").style.display = "none";

                document.getElementById("mainContent").style.display = "block";

                loadTasks();

                renderTasks();

                renderArchive();

            } else {

                errorMessage.textContent = "❌ Nieprawidłowe hasło!";

            }

        }

 

        // Funkcja wylogowania

        function logout() {

            isLoggedIn = false;

            localStorage.removeItem('stockControlLoggedIn');

            localStorage.removeItem('stockControlPassword');

            document.getElementById("loginSection").style.display = "flex";

            document.getElementById("mainContent").style.display = "none";

            document.getElementById("password").value = "";

            document.getElementById("errorMessage").textContent = "";

        }

 

        // Funkcja przełączania trybu ciemnego/jałowego

        function toggleTheme() {

            document.documentElement.classList.toggle("dark");

            const icon = document.querySelector(".theme-toggle i");

            if (document.documentElement.classList.contains("dark")) {

                icon.classList.remove("fa-moon");

                icon.classList.add("fa-sun");

            } else {

                icon.classList.remove("fa-sun");

                icon.classList.add("fa-moon");

            }

        }

 

        // Sprawdź, czy użytkownik jest zalogowany przy ładowaniu strony

        document.addEventListener('DOMContentLoaded', () => {

            const savedLoggedIn = localStorage.getItem('stockControlLoggedIn');

            if (savedLoggedIn === 'true') {

                isLoggedIn = true;

                document.getElementById("loginSection").style.display = "none";

                document.getElementById("mainContent").style.display = "block";

                loadTasks();

                renderTasks();

                renderArchive();

            }

        });

 

        // Funkcja zapisująca zadania do localStorage

        function saveTasks() {

            localStorage.setItem('stockControlTasks', JSON.stringify(tasks));

        }

 

        // Funkcja pobierająca zadania z localStorage

        function loadTasks() {

            const savedTasks = localStorage.getItem('stockControlTasks');

            if (savedTasks) {

                tasks = JSON.parse(savedTasks);

            } else {

                // Domyślne dane, jeśli localStorage jest pusty

                tasks = [

                    {

                        id: 1,

                        orderDate: "2026-09-20",

                        assignedTo: "Magdalena Kowalska",

                        assignedBy: "Janusz Nowak",

                        description: "Sprawdzić stan magazynu części A1-B5",

                        deadline: "2026-09-25",

                        status: "pending",

                        notes: "Priorytet: wysoki",

                        completionDate: null

                    },

                    {

                        id: 2,

                        orderDate: "2026-09-18",

                        assignedTo: "Dział Logistyki",

                        assignedBy: "Anna Wiśniewska",

                        description: "Zamówić nowe opakowania foliowe",

                        deadline: "2026-09-22",

                        status: "completed",

                        notes: "Zrealizowano w terminie",

                        completionDate: "2026-09-20"

                    },

                    {

                        id: 3,

                        orderDate: "2026-09-15",

                        assignedTo: "Piotr Lewandowski",

                        assignedBy: "Magdalena Kowalska",

                        description: "Przeprowadzić inwentaryzację półki C7-D3",

                        deadline: "2026-09-24",

                        status: "overdue",

                        notes: "Opóźnienie spowodowane brakiem pracownika",

                        completionDate: null

                    }

                ];

                saveTasks();

            }

        }

 

        // Funkcja generująca unikalne ID

        function generateId() {

            return Date.now();

        }

 

        // Funkcja dodająca zadanie

        function addTask() {

            if (!isLoggedIn) return;

 

            const orderDate = document.getElementById("orderDate").value;

            const assignedTo = document.getElementById("assignedTo").value;

            const assignedBy = document.getElementById("assignedBy").value;

            const description = document.getElementById("description").value;

            const deadline = document.getElementById("deadline").value;

            const notes = document.getElementById("notes").value;

 

            if (!orderDate || !assignedTo || !assignedBy || !description || !deadline) {

                alert("⚠️ Wszystkie pola są wymagane!");

                return;

            }

 

            const newTask = {

                id: generateId(),

                orderDate,

                assignedTo,

                assignedBy,

                description,

                deadline,

                status: "pending",

                notes,

                completionDate: null

            };

 

            tasks.push(newTask);

            saveTasks();

            renderTasks();

            renderArchive();

            document.getElementById("taskForm").reset();

        }

 

        // Funkcja renderująca zadania

        function renderTasks() {

            if (!isLoggedIn) return;

 

            const tasksBody = document.getElementById("tasksBody");

            tasksBody.innerHTML = "";

 

            tasks.forEach(task => {

                if (task.status !== "completed") {

                    const row = document.createElement("tr");

 

                    // Sprawdzanie, czy zadanie jest przeterminowane

                    const today = new Date();

                    const deadlineDate = new Date(task.deadline);

                    const isOverdue = today > deadlineDate && task.status !== "completed";

 

                    row.innerHTML = `

                        <td>${task.orderDate}</td>

                        <td>${task.assignedTo}</td>

                        <td>${task.assignedBy}</td>

                        <td>${task.description}</td>

                        <td>${task.deadline}</td>

                        <td>

                            <span class="status ${isOverdue ? 'overdue' : task.status}">

                                ${task.status === "pending" ? "Oczekujące" : task.status === "completed" ? "Zakończone" : "Przeterminowane"}

                            </span>

                        </td>

                        <td>${task.notes || "-"}</td>

                        <td class="actions">

                            <button class="complete-btn" onclick="completeTask(${task.id})">

                                <i class="fas fa-check-circle"></i> Zakończ

                            </button>

                            <button class="archive-btn" onclick="archiveTask(${task.id})">

                                <i class="fas fa-archive"></i> Archiwizuj

                            </button>

                            <button class="delete-btn" onclick="deleteTask(${task.id})">

                                <i class="fas fa-trash-alt"></i> Usuń

                            </button>

                        </td>

                    `;

 

                    tasksBody.appendChild(row);

                }

            });

        }

 

        // Funkcja renderująca archiwum

        function renderArchive() {

            if (!isLoggedIn) return;

 

            const archiveBody = document.getElementById("archiveBody");

            archiveBody.innerHTML = "";

 

            tasks.forEach(task => {

                if (task.status === "completed") {

                    const row = document.createElement("tr");

                    row.innerHTML = `

                        <td>${task.orderDate}</td>

                        <td>${task.assignedTo}</td>

                        <td>${task.assignedBy}</td>

                        <td>${task.description}</td>

                        <td>${task.deadline}</td>

                        <td>${task.completionDate}</td>

                        <td>${task.notes || "-"}</td>

                        <td class="actions">

                            <button class="restore-btn" onclick="restoreTask(${task.id})">

                                <i class="fas fa-undo"></i> Przywróć

                            </button>

                            <button class="delete-btn" onclick="permanentDeleteTask(${task.id})">

                                <i class="fas fa-times-circle"></i> Usuń na stałe

                            </button>

                        </td>

                    `;

                    archiveBody.appendChild(row);

                }

            });

        }

 

        // Funkcja oznaczająca zadanie jako zakończone

        function completeTask(id) {

            if (!isLoggedIn) return;

 

            const task = tasks.find(t => t.id === id);

            if (task) {

                task.status = "completed";

                task.completionDate = new Date().toISOString().split('T');

                saveTasks();

                renderTasks();

                renderArchive();

            }

        }

 

        // Funkcja archiwizująca zadanie

        function archiveTask(id) {

            if (!isLoggedIn) return;

 

            const task = tasks.find(t => t.id === id);

            if (task) {

                task.status = "archived";

                saveTasks();

                renderTasks();

                renderArchive();

            }

        }

 

        // Funkcja przywracająca zadanie z archiwum

        function restoreTask(id) {

            if (!isLoggedIn) return;

 

            const task = tasks.find(t => t.id === id);

            if (task) {

                task.status = "pending";

                task.completionDate = null;

                saveTasks();

                renderTasks();

                renderArchive();

            }

        }

 

        // Funkcja usuwająca zadanie na stałe (z archiwum)

        function permanentDeleteTask(id) {

            if (!isLoggedIn) return;

 

            tasks = tasks.filter(t => t.id !== id);

            saveTasks();

            renderArchive();

        }

 

        // Funkcja usuwająca zadanie (z listy aktywnych)

        function deleteTask(id) {

            if (!isLoggedIn) return;

 

            tasks = tasks.filter(t => t.id !== id);

            saveTasks();

            renderTasks();

            renderArchive();

        }

 

        // Funkcja filtrująca zadania

        function filterTasks() {

            if (!isLoggedIn) return;

 

            const filter = document.getElementById("statusFilter").value;

            const rows = document.querySelectorAll("#tasksBody tr");

 

            rows.forEach(row => {

                const status = row.querySelector(".status").textContent.trim().toLowerCase();

                if (filter === "all") {

                    row.style.display = "";

                } else if (filter === "completed" && status.includes("zakończone")) {

                    row.style.display = "";

                } else if (filter === "pending" && status.includes("oczekujące")) {

                    row.style.display = "";

                } else if (filter === "overdue" && status.includes("przeterminowane")) {

                    row.style.display = "";

                } else {

                    row.style.display = "none";

                }

            });

        }

    </script>

</body>

</html>
