<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Library Management - St. Xavier's</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .navbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #222;
            color: white;
            padding: 10px 20px;
            text-align: center;
        }
        .menu-btn {
            font-size: 24px;
            cursor: pointer;
            background: none;
            border: none;
            color: white;
        }
        .sidebar {
            width: 250px;
            height: 100vh;
            background: #333;
            position: fixed;
            left: -250px;
            top: 0;
            padding-top: 20px;
            transition: 0.3s;
        }
        .sidebar a {
            display: block;
            padding: 10px;
            color: white;
            text-decoration: none;
        }
        .sidebar a:hover {
            background: #444;
        }
        .content {
            margin-left: 20px;
            padding: 20px;
        }
        .hidden { display: none; }
        .dark-mode {
            background-color: #222;
            color: white;
        }
        .btn {
            padding: 8px 12px;
            background: blue;
            color: white;
            border: none;
            cursor: pointer;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 8px;
            text-align: left;
        }
        #login-container {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }
        #main-container {
            display: none;
        }
    </style>
</head>
<body>

    <!-- Login Page -->
    <div id="login-container">
        <h2>Library Management Login</h2>
        <input type="text" id="username" placeholder="Username">
        <input type="password" id="password" placeholder="Password">
        <button class="btn" onclick="login()">Login</button>
    </div>

    <!-- Main System -->
    <div id="main-container">
        <!-- Navbar -->
        <div class="navbar">
            <button class="menu-btn" onclick="toggleMenu()">☰</button>
            <span>📚 Library Management - St. Xavier's</span>
            <button class="btn" onclick="logout()">Logout</button>
        </div>

        <!-- Sidebar -->
        <div class="sidebar" id="sidebar">
            <a href="#" onclick="showSection('dashboard')">🏠 Home</a>
            <a href="#" onclick="showSection('book-location')">📍 Book Locations</a>
            <a href="#" onclick="showSection('issued-books')">📖 Issued Books</a>
        </div>

        <!-- Dashboard -->
        <div class="content hidden" id="dashboard">
            <h2>Library Dashboard</h2>
            <button class="btn" onclick="showSection('issue-form')">Issue Book</button>
            <h3>Overdue Books</h3>
            <table id="overdue-books">
                <tr>
                    <th>Student</th>
                    <th>Book</th>
                    <th>Due Date</th>
                </tr>
            </table>
        </div>

        <!-- Book Locations -->
        <div class="content hidden" id="book-location">
            <h2>Book Locations</h2>
            <input type="text" id="book-name" placeholder="Book Name">
            <input type="text" id="book-location-input" placeholder="Location">
            <button class="btn" onclick="addBookLocation()">Save</button>
            <h3>Stored Locations</h3>
            <table id="book-location-table">
                <tr>
                    <th>Book Name</th>
                    <th>Location</th>
                </tr>
            </table>
        </div>

        <!-- Issued Books -->
        <div class="content hidden" id="issued-books">
            <h2>Issued Books</h2>
            <table id="issued-table">
                <tr>
                    <th>Student</th>
                    <th>Book</th>
                    <th>Issue Date</th>
                    <th>Return Date</th>
                    <th>Action</th>
                </tr>
            </table>
        </div>
    </div>

<script>
    let currentUser = localStorage.getItem("username") || "admin";
    let currentPass = localStorage.getItem("password") || "admin123";

    function login() {
        let user = document.getElementById("username").value;
        let pass = document.getElementById("password").value;

        if (user === currentUser && pass === currentPass) {
            localStorage.setItem("loggedIn", "true");
            document.getElementById("login-container").style.display = "none";
            document.getElementById("main-container").style.display = "block";
        } else {
            alert("Invalid Credentials");
        }
    }

    function logout() {
        localStorage.setItem("loggedIn", "false");
        location.reload();
    }

    function toggleMenu() {
        let sidebar = document.getElementById("sidebar");
        sidebar.style.left = (sidebar.style.left === "0px") ? "-250px" : "0px";
    }

    function showSection(sectionId) {
        document.querySelectorAll(".content").forEach(sec => sec.classList.add("hidden"));
        document.getElementById(sectionId).classList.remove("hidden");
    }

    function addBookLocation() {
        let bookName = document.getElementById("book-name").value;
        let location = document.getElementById("book-location-input").value;

        let table = document.getElementById("book-location-table");
        let row = table.insertRow();
        row.innerHTML = `<td>${bookName}</td><td>${location}</td>`;

        document.getElementById("book-name").value = "";
        document.getElementById("book-location-input").value = "";
    }

    function checkLoginStatus() {
        if (localStorage.getItem("loggedIn") !== "true") {
            document.getElementById("login-container").style.display = "block";
            document.getElementById("main-container").style.display = "none";
        } else {
            document.getElementById("login-container").style.display = "none";
            document.getElementById("main-container").style.display = "block";
        }
    }

    checkLoginStatus();
</script>

</body>
</html>
