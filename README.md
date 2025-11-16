<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Colourful Dashboard</title>

  <!-- DaisyUI + Tailwind -->
  <link href="https://cdn.jsdelivr.net/npm/daisyui@4.6.0/dist/full.css" rel="stylesheet" />
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Handlebars.js -->
  <script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.min.js"></script>

  <style>
    body {
      background: linear-gradient(135deg, #fce7f3, #e0f2fe, #fef3c7);
      background-size: 200% 200%;
      animation: gradientMove 10s ease infinite;
    }

    @keyframes gradientMove {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Card Styling */
    .fancy-card {
      background: linear-gradient(135deg, #ffffffcc, #ffffffdd);
      border-radius: 1rem;
      padding: 1.2rem;
      box-shadow: 0 4px 20px rgba(0,0,0,0.08);
      backdrop-filter: blur(10px);
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    .fancy-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 8px 25px rgba(0,0,0,0.12);
    }

    /* Sidebar Styling */
    .sidebar {
      background: linear-gradient(180deg, #4f46e5, #9333ea);
      color: white;
    }
    .menu a {
      color: white !important;
      font-weight: 500;
      border-radius: 6px;
    }
    .menu a:hover {
      background: rgba(255,255,255,0.15) !important;
    }

    /* Header gradient */
    .header-gradient {
      background: linear-gradient(90deg, #6366f1, #ec4899, #f59e0b);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
  </style>
</head>

<body>

  <div class="drawer lg:drawer-open">
    <input id="sidebarDrawer" type="checkbox" class="drawer-toggle" />
    <div class="drawer-content flex flex-col p-4" id="headerContainer"></div>
    <div class="drawer-side" id="sidebarContainer"></div>
  </div>

  <!-- SIDEBAR TEMPLATE -->
  <script id="sidebarTemplate" type="text/x-handlebars-template">
    <label for="sidebarDrawer" class="drawer-overlay"></label>

    <aside class="w-64 min-h-full p-4 shadow sidebar">
      <h2 class="text-3xl font-bold mb-6">✨ Menu</h2>

      <ul class="menu space-y-2">
        <li><a data-page="dashboard">Dashboard</a></li>
        <li><a data-page="analytics">Analytics</a></li>
        <li><a data-page="users">Users</a></li>
        <li><a data-page="settings">Settings</a></li>
        <li><a id="logoutBtnSidebar">Logout</a></li>
      </ul>
    </aside>
  </script>

  <!-- HEADER TEMPLATE -->
  <script id="headerTemplate" type="text/x-handlebars-template">
    <div class="flex items-center justify-between mb-4">
      <label for="sidebarDrawer" class="btn btn-primary drawer-button lg:hidden">☰</label>
      <h1 class="text-3xl font-extrabold header-gradient">{{title}}</h1>

      <button id="logoutBtn" class="btn btn-error btn-sm hidden lg:flex">Logout</button>
    </div>

    <div id="contentContainer" class="space-y-4"></div>
  </script>

  <!-- DASHBOARD TEMPLATE -->
  <script id="dashboardTemplate" type="text/x-handlebars-template">
    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Overview</h2>
      <p class="text-gray-700 mt-1">
        Welcome to your vibrant dashboard!
      </p>
    </div>

    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Analytics Summary</h2>
      <ul class="mt-2 text-gray-700">
        <li>• Weekly active users increased by 12%</li>
        <li>• Engagement rate: 68%</li>
        <li>• Bounce rate improved by 5%</li>
      </ul>
    </div>

    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Recent Users</h2>
      <ul class="mt-2 text-gray-700">
        <li>• John Doe — Joined today</li>
        <li>• Sarah Green — Joined yesterday</li>
        <li>• Amit Sharma — Joined this week</li>
      </ul>
    </div>

    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Settings Preview</h2>
      <ul class="mt-2 text-gray-700">
        <li>• Notification: Enabled</li>
        <li>• Dashboard theme: Light</li>
        <li>• Account type: Admin</li>
      </ul>
    </div>
  </script>

  <!-- ANALYTICS -->
  <script id="analyticsTemplate" type="text/x-handlebars-template">
    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Analytics</h2>
      <ul class="mt-2 text-gray-700">
        <li>• Total visitors this week: 5,230</li>
        <li>• New sign-ups: 422</li>
        <li>• Conversion rate: 14.2%</li>
        <li>• Time spent per user: 5m 45s</li>
      </ul>
    </div>
  </script>

  <!-- USERS -->
  <script id="usersTemplate" type="text/x-handlebars-template">
    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Users Management</h2>
      <ul class="mt-2 text-gray-700">
        <li>• John Doe – Active</li>
        <li>• Priya Singh – Active</li>
        <li>• Michael Lee – Suspended</li>
        <li>• Asha Patel – Pending verification</li>
      </ul>
    </div>
  </script>

  <!-- SETTINGS -->
  <script id="settingsTemplate" type="text/x-handlebars-template">
    <div class="fancy-card">
      <h2 class="text-lg font-semibold">Settings</h2>
      <ul class="mt-2 text-gray-700">
        <li>• Enable notifications</li>
        <li>• Change theme (Dark / Light)</li>
        <li>• Update profile information</li>
        <li>• Change password</li>
      </ul>
    </div>
  </script>

  <!-- JAVASCRIPT LOGIC (unchanged) -->
  <script>
    const sidebarHTML = Handlebars.compile(document.getElementById("sidebarTemplate").innerHTML);
    const headerHTML = Handlebars.compile(document.getElementById("headerTemplate").innerHTML);

    const dashboardHTML = Handlebars.compile(document.getElementById("dashboardTemplate").innerHTML);
    const analyticsHTML = Handlebars.compile(document.getElementById("analyticsTemplate").innerHTML);
    const usersHTML = Handlebars.compile(document.getElementById("usersTemplate").innerHTML);
    const settingsHTML = Handlebars.compile(document.getElementById("settingsTemplate").innerHTML);

    document.getElementById("sidebarContainer").innerHTML = sidebarHTML();
    loadPage("dashboard");

    function loadPage(page) {
      let title = "Dashboard";
      let content = "";

      if (page === "dashboard") content = dashboardHTML();
      if (page === "analytics") { title = "Analytics"; content = analyticsHTML(); }
      if (page === "users") { title = "Users"; content = usersHTML(); }
      if (page === "settings") { title = "Settings"; content = settingsHTML(); }

      document.getElementById("headerContainer").innerHTML = headerHTML({ title });
      document.getElementById("contentContainer").innerHTML = content;

      attachSidebarEvents();
      attachLogoutEvents();
    }

    function attachSidebarEvents() {
      document.querySelectorAll("[data-page]").forEach(link => {
        link.addEventListener("click", () => loadPage(link.dataset.page));
      });
    }

    function attachLogoutEvents() {
      const btn1 = document.getElementById("logoutBtn");
      const btn2 = document.getElementById("logoutBtnSidebar");

      [btn1, btn2].forEach(btn => {
        if (btn) {
          btn.addEventListener("click", () => {
            if (confirm("Logout?")) {
              alert("Logged out!");
              window.location.href = "/login";
            }
          });
        }
      });
    }
  </script>

</body>
</html>
