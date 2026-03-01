<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lua Script Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
    
    <style>
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/tomorrow-night.min.css');
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.css');
    </style>
    
    <style>
        body { font-family: 'Fredoka One', cursive; background-color: #121212; color: #ffffff; padding: 20px; margin: 0; }
        .container { max-width: 800px; margin: auto; }
        
        /* PROFESSIONAL HEADER */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            background: #1e1e1e;
            padding: 10px 20px;
            border-radius: 12px;
            border: 2px solid #333;
            position: relative;
        }
        .logo { font-size: 28px; font-weight: bold; }
        .logo-hub { color: #007bff; }
        .header-actions { display: flex; align-items: center; gap: 15px; }
        
        /* Header PFP */
        .pfp-container { position: relative; }
        .pfp-header { 
            width: 45px; height: 45px; 
            border-radius: 50%; 
            object-fit: cover; 
            border: 3px solid #007bff; 
            cursor: pointer;
            transition: transform 0.2s;
        }
        .pfp-header:hover { transform: scale(1.05); }
        .ban-counter {
            position: absolute; top: -5px; right: -5px;
            background: #dc3545; color: white; border-radius: 50%;
            width: 20px; height: 20px; font-size: 11px;
            display: flex; justify-content: center; align-items: center;
            border: 2px solid #121212; font-family: sans-serif;
        }

        .btn { 
            padding: 10px 15px; border: none; border-radius: 8px; 
            cursor: pointer; font-family: 'Fredoka One', cursive; 
            font-size: 14px; transition: all 0.2s;
        }
        .btn-login { background: #007bff; color: white; }
        .btn-logout { background: #555; color: white; }
        .btn-admin { background: #dc3545; color: white; font-size: 16px; padding: 8px 12px; border: 1px solid #444; border-radius: 50%;}
        .btn:hover { opacity: 0.9; }

        /* --- USER PROFILE PANEL (MODAL) --- */
        .profile-panel {
            display: none;
            position: absolute;
            top: calc(100% + 10px);
            right: 0;
            background: #1e1e1e;
            border: 2px solid #007bff;
            border-radius: 12px;
            padding: 20px;
            width: 250px;
            z-index: 1000;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            text-align: center;
        }
        .profile-panel.show { display: block; }
        
        .panel-pfp { width: 80px; height: 80px; border-radius: 50%; border: 4px solid #007bff; margin-bottom: 10px; }
        
        /* Updated Username Style */
        .panel-username { 
            font-size: 18px; margin-bottom: 5px; cursor: pointer; 
            display: inline-block; padding: 2px 8px; border-radius: 4px;
        }
        .panel-username:hover { background: #333; }
        
        .owner-tag { color: #ffc107; font-size: 12px; font-family: sans-serif; display: block; margin-bottom: 5px; font-weight: bold;}
        .warning-tag {
            color: #dc3545; font-size: 12px; font-family: sans-serif;
            display: inline-block; margin-bottom: 10px; background: rgba(220,53,69,0.2);
            padding: 2px 6px; border-radius: 4px; cursor: help;
        }
        .beloved-title { font-size: 12px; color: #aaa; margin-top: 15px; }
        .beloved-script { 
            background: #2a2a2a; 
            padding: 10px; 
            border-radius: 8px; 
            margin-top: 5px; 
            font-size: 14px; 
            border: 1px solid #333;
        }
        .panel-logout { width: 100%; margin-top: 20px; background: #dc3545; }

        /* PROFESSIONAL SEARCH BAR */
        .search-container {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            background: #1e1e1e;
            padding: 10px;
            border-radius: 12px;
            border: 2px solid #333;
            align-items: center;
        }
        .search-input { 
            flex-grow: 1; padding: 12px;
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px;
            font-family: 'Fredoka One', cursive; 
        }
        
        /* Custom Dropdown Styling */
        .custom-select-wrapper {
            position: relative;
            user-select: none;
            min-width: 150px;
        }
        .custom-select {
            background: #2a2a2a;
            border: 2px solid #444;
            color: #fff;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-family: 'Fredoka One', cursive;
            font-size: 14px;
        }
        .custom-select:hover { border-color: #555; }
        .custom-select-options {
            display: none;
            position: absolute;
            top: calc(100% + 5px);
            left: 0;
            right: 0;
            background: #2a2a2a;
            border: 2px solid #444;
            border-radius: 8px;
            z-index: 10;
            overflow: hidden;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        .custom-select-options.open { display: block; }
        .custom-select-option {
            padding: 12px;
            cursor: pointer;
            font-family: 'Fredoka One', cursive;
            font-size: 14px;
            transition: background 0.2s;
        }
        .custom-select-option:hover { background: #333; }
        .custom-select-option.selected { background: #007bff; color: white; }

        /* Card Styles */
        .card { 
            background: #1e1e1e; 
            border: 2px solid #333; 
            border-radius: 12px; 
            padding: 20px; 
            margin-bottom: 20px;
            transition: all 0.3s ease;
            position: relative;
        }
        .card:hover { border-color: #555; }
        h3 { font-size: 20px; margin-top: 0; margin-bottom: 5px; }
        p.desc { color: #aaa; font-family: sans-serif; margin-bottom: 15px; }
        
        /* Fix Code Overflow */
        pre[class*="language-"] {
            border-radius: 8px;
            overflow-x: auto;
            max-height: 400px;
            margin-top: 10px;
            white-space: pre;
        }
        code[class*="language-"] {
            font-family: 'Fira Code', monospace;
            font-size: 14px;
        }
        
        input, textarea { 
            width: 100%; padding: 12px; margin: 10px 0; 
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px; 
            box-sizing: border-box;
            font-family: 'Fredoka One', cursive; 
        }

        /* CARD MENU (Three Dots) */
        .card-options {
            position: absolute;
            top: 20px;
            right: 20px;
            cursor: pointer;
            color: #888;
            padding: 5px;
            font-size: 18px;
            transition: color 0.2s;
            z-index: 5;
        }
        .card-options:hover { color: white; }
        
        .dropdown-menu {
            display: none;
            position: absolute;
            top: 100%;
            right: 0;
            background: #333;
            border-radius: 8px;
            padding: 5px;
            z-index: 100;
            min-width: 150px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
            border: 1px solid #444;
        }
        .dropdown-menu button {
            display: flex;
            align-items: center;
            gap: 8px;
            width: 100%;
            background: none;
            border: none;
            color: white;
            text-align: left;
            padding: 10px;
            cursor: pointer;
            font-family: 'Fredoka One', cursive;
            font-size: 14px;
            border-radius: 4px;
        }
        .dropdown-menu button:hover { background: #444; }
        .dropdown-menu button.danger { color: #dc3545; }

        .btn-post { background: #007bff; color: white; width: 100%; }
        
        /* Reaction Buttons */
        .btn-reaction { background: #333; color: #aaa; padding: 5px 10px; font-size: 12px; font-family: 'Fredoka One', cursive; }
        .btn-reaction.active-like { background: #28a745; color: white; }
        .btn-reaction.active-dislike { background: #dc3545; color: white; }
        
        /* 10-Point Rating Slider */
        .rating-container {
            display: flex;
            align-items: center;
            gap: 10px;
            font-family: sans-serif;
            font-size: 14px;
            color: #aaa;
            background: #2a2a2a;
            padding: 5px 10px;
            border-radius: 8px;
        }
        .rating-slider {
            -webkit-appearance: none;
            width: 150px;
            height: 8px;
            background: #444;
            border-radius: 5px;
            outline: none;
        }
        .rating-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 18px;
            height: 18px;
            background: #007bff;
            border-radius: 50%;
            cursor: pointer;
        }

        /* Comments Styling */
        .comment-section { margin-top: 20px; border-top: 1px solid #444; padding-top: 15px; }
        .comment-input {
            width: 100%; padding: 10px;
            background: #2a2a2a; border: 1px solid #444;
            border-radius: 8px; color: white;
            font-family: sans-serif; box-sizing: border-box;
        }
        .comment-item {
            background: #2a2a2a; padding: 10px; border-radius: 8px;
            margin-top: 10px; font-family: sans-serif; font-size: 14px; position: relative;
        }
        .reply-item { margin-left: 25px; border-left: 2px solid #444; }
        
        /* Updated Author Tag Styling for "edited" marker */
        .author-tag { 
            color: #007bff; font-weight: bold; margin-bottom: 5px; 
            display: flex; align-items: center; gap: 5px;
            font-family: 'Fredoka One', cursive;
        }
        .edited-tag { color: #888; font-size: 10px; font-weight: normal; font-family: sans-serif; }
        
        .pfp-tiny { width: 16px; height: 16px; border-radius: 50%; object-fit: cover; }
        .comment-text { word-wrap: break-word; white-space: pre-wrap; padding-right: 25px; font-family: sans-serif; color: #eee; }
        .comment-actions { display: flex; gap: 5px; margin-top: 5px; }
        .comment-options { position: absolute; top: 10px; right: 10px; cursor: pointer; color: #888; }
        .stats-text { font-family: sans-serif; font-size: 14px; color: #aaa; margin-top: 10px; display: flex; gap: 15px; }
        .toggle-replies { background: none; border: none; color: #007bff; cursor: pointer; font-size: 12px; margin-top: 5px; font-family: sans-serif; }

        /* MODAL */
        .modal {
            display: none; position: fixed; top: 0; left: 0; 
            width: 100%; height: 100%; background-color: rgba(0,0,0,0.85);
            justify-content: center; align-items: center; z-index: 2000;
            overflow-y: auto;
        }
        .modal.show { display: flex; }
        
        .modal-content {
            background: #1e1e1e; padding: 25px; border-radius: 12px;
            border: 2px solid #007bff; text-align: center;
            width: 90%; max-width: 400px;
        }
        .modal-buttons { display: flex; justify-content: center; gap: 10px; margin-top: 20px; }
        
        .pfp-option { width: 50px; height: 50px; border-radius: 50%; cursor: pointer; border: 3px solid transparent; }
        .pfp-option.selected { border-color: #007bff; }
        
        .auth-toggle { font-family: sans-serif; font-size: 14px; color: #aaa; margin-top: 15px; cursor: pointer; }
        .auth-toggle span { color: #007bff; text-decoration: underline; }

        /* ADMIN PANEL STYLING - PROFESSIONAL */
        .admin-modal-content { max-width: 900px; text-align: left; border-color: #dc3545; }
        .admin-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #333; padding-bottom: 10px; margin-bottom: 15px; }
        .admin-table { width: 100%; border-collapse: separate; border-spacing: 0; margin-top: 15px; font-family: sans-serif; color: white; font-size: 14px;}
        .admin-table th, .admin-table td { border-bottom: 1px solid #333; padding: 15px; text-align: left; }
        .admin-table th { background: #2a2a2a; color: #aaa; font-weight: bold; }
        .admin-table tr:hover { background: #252525; }
        .report-comment-preview { background: #121212; padding: 10px; border-radius: 8px; font-size: 12px; color: #aaa; margin-top: 5px; border: 1px solid #333; font-family: 'Fira Code', monospace;}
        
        /* TOAST NOTIFICATION */
        #toast {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            background: #28a745; color: white; padding: 15px 25px; border-radius: 8px;
            display: none; z-index: 3000; font-family: 'Fredoka One', cursive; text-align: center;
        }
        #toast.show { display: block; }
        #toast.ban { background: #dc3545; }
        #toast.unban { background: #ffc107; color: #121212; }
    </style>
</head>
<body>

<div id="toast"></div>

<div class="container">
    
    <div class="header">
        <div class="logo">Lua<span class="logo-hub">Hub</span></div>
        
        <div class="header-actions" id="authAreaContainer">
            </div>

        <div class="profile-panel" id="profilePanel">
            <img src="" id="panelPfp" class="panel-pfp">
            <div class="panel-username" id="panelUsername" onclick="promptChangeUsername()" title="Click to change name"></div>
            <span class="owner-tag" id="panelTag" style="display:none;">👑 OWNER</span>
            <span class="warning-tag" id="panelWarning" style="display:none;" title="Warning Level means that the account was banned and is in danger, if the Warning Level gets to Level : 10, the user will be banned forever!">Warning Level : 0</span>
            
            <div class="beloved-title">🏆 YOUR BELOVED SCRIPT 🏆</div>
            <div class="beloved-script" id="panelBeloved">No scripts yet!</div>
            
            <button class="btn panel-logout" onclick="logout()">Logout</button>
        </div>
    </div>

    <div class="search-container">
        <input type="text" id="searchInput" class="search-input" placeholder="Search..." onkeyup="renderScripts()">
        
        <div class="custom-select-wrapper" id="searchTypeWrapper">
            <div class="custom-select" id="searchTypeDisplay">
                <span id="selectedTypeText">Name</span>
                <i class="fas fa-chevron-down" style="font-size:12px; margin-left:8px;"></i>
            </div>
            <div class="custom-select-options" id="searchTypeOptions">
                <div class="custom-select-option selected" data-value="name">Name</div>
                <div class="custom-select-option" data-value="creator">Creator</div>
            </div>
        </div>
        <input type="hidden" id="searchType" value="name">
    </div>

    <div class="card" id="createCard" style="display:none;">
        <h2>✍️ Create Script</h2>
        <input type="text" id="scriptTitle" placeholder="Script Title">
        <textarea id="scriptDesc" rows="2" placeholder="Description"></textarea>
        <textarea id="scriptCode"></textarea>
        <button class="btn btn-post" onclick="saveScript()">Post Script</button>
    </div>

    <div id="scriptFeed"></div>
</div>

<div id="authModal" class="modal">
    <div class="modal-content">
        <h3 id="authTitle">Login</h3>
        <input type="text" id="authUsername" placeholder="Username">
        <input type="password" id="authPassword" placeholder="Password">
        
        <p style="font-family:sans-serif; margin-top:15px; color:#aaa;">Choose Profile Picture:</p>
        <div style="display:flex; justify-content:center; gap:10px; margin-bottom:15px;">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=1" class="pfp-option selected" onclick="selectPfp(this, 'seed=1')">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=2" class="pfp-option" onclick="selectPfp(this, 'seed=2')">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=3" class="pfp-option" onclick="selectPfp(this, 'seed=3')">
        </div>

        <div class="modal-buttons">
            <button class="btn btn-post" id="authSubmitBtn" onclick="processAuth()">Login</button>
            <button class="btn btn-logout" onclick="closeAuthModal()">Cancel</button>
        </div>
        
        <div class="auth-toggle" id="authToggleText" onclick="toggleAuthType()">
            Don't have an account? <span>Sign Up</span>
        </div>
    </div>
</div>

<div id="inputModal" class="modal">
    <div class="modal-content">
        <h3 id="inputModalTitle">Edit</h3>
        <textarea id="inputModalText" rows="4"></textarea>
        
        <div class="modal-buttons">
            <button class="btn btn-post" id="inputModalSubmit">Submit</button>
            <button class="btn btn-logout" onclick="closeInputModal()">Cancel</button>
        </div>
    </div>
</div>

<div id="deleteModal" class="modal">
    <div class="modal-content" style="border-color:#dc3545;">
        <h3>⚠️ Delete Item?</h3>
        <p style="color:white; font-family:sans-serif;" id="deleteModalText">Are you sure you want to Delete this item?</p>
        
        <div class="modal-buttons">
            <button class="btn btn-logout" style="background:#dc3545;" id="deleteModalConfirm">Yes</button>
            <button class="btn btn-post" onclick="closeDeleteModal()">No</button>
        </div>
    </div>
</div>

<div id="adminModal" class="modal">
    <div class="modal-content admin-modal-content">
        <div class="admin-header">
            <h3>🛡️ Admin Dashboard</h3>
            <button class="btn" style="background:#333;" onclick="closeAdminModal()">X</button>
        </div>
        <p style="color:#aaa; font-family:sans-serif;">Welcome, Administrator Tasin Redwan.</p>
        
        <div id="adminContentArea"></div>
        
    </div>
</div>

<div id="banModal" class="modal">
    <div class="modal-content" style="border-color:#dc3545;">
        <h3>🚫 Ban User?</h3>
        <p style="color:white; font-family:sans-serif;" id="banModalContext"></p>
        <input type="number" id="banDays" placeholder="Days to Ban">
        <textarea id="banReason" placeholder="Reason for Ban" rows="3"></textarea>
        
        <div class="modal-buttons">
            <button class="btn btn-logout" style="background:#dc3545;" id="banModalConfirm">Ban</button>
            <button class="btn btn-post" onclick="closeBanModal()">Cancel</button>
        </div>
    </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/lua/lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.js"></script>

<script>
    // --- INIT & MIGRATION ---
    let scripts = JSON.parse(localStorage.getItem('lua_hub_data')) || [];
    let users = JSON.parse(localStorage.getItem('lua_hub_users')) || {};
    let currentUser = JSON.parse(localStorage.getItem('lua_hub_session')) || null;
    let reportedComments = JSON.parse(localStorage.getItem('lua_hub_reports')) || [];
    let bannedUsers = JSON.parse(localStorage.getItem('lua_hub_banned')) || [];
    let userWarningLevels = JSON.parse(localStorage.getItem('lua_hub_warnings')) || {};

    // Check if current user is banned
    if(currentUser && bannedUsers.includes(currentUser.username)) {
        logout();
        alert("Your account has been banned.");
    }
    
    let selectedPfpSeed = 'seed=1';
    let authType = 'login';
    
    // Data Migration
    scripts.forEach(s => {
        if(!s.userReactions) s.userReactions = {};
        if(!s.userRatings) s.userRatings = {};
        if(!s.comments) s.comments = [];
        s.comments.forEach(c => {
            if(!c.replies) c.replies = [];
            if(!c.userReactions) c.userReactions = {};
            if(c.edited === undefined) c.edited = false; // Add edited flag
            c.replies.forEach(r => {
                if(r.edited === undefined) r.edited = false;
            });
        });
    });
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
    
    // --- CUSTOM DROPDOWN LOGIC ---
    const dropdownDisplay = document.getElementById('searchTypeDisplay');
    const dropdownOptions = document.getElementById('searchTypeOptions');
    const hiddenInput = document.getElementById('searchType');
    const selectedText = document.getElementById('selectedTypeText');

    dropdownDisplay.addEventListener('click', (e) => {
        e.stopPropagation();
        dropdownOptions.classList.toggle('open');
    });

    document.querySelectorAll('.custom-select-option').forEach(option => {
        option.addEventListener('click', (e) => {
            e.stopPropagation();
            const value = option.getAttribute('data-value');
            const text = option.innerText;
            hiddenInput.value = value;
            selectedText.innerText = text;
            dropdownOptions.classList.remove('open');
            document.querySelectorAll('.custom-select-option').forEach(opt => opt.classList.remove('selected'));
            option.classList.add('selected');
            renderScripts();
        });
    });

    document.addEventListener('click', () => {
        dropdownOptions.classList.remove('open');
        document.getElementById('profilePanel').classList.remove('show');
        document.querySelectorAll('.dropdown-menu').forEach(m => m.style.display = 'none');
    });

    // --- PROFILE PANEL LOGIC ---
    function toggleProfilePanel(e) {
        e.stopPropagation();
        const panel = document.getElementById('profilePanel');
        panel.classList.toggle('show');
        
        if(panel.classList.contains('show')) {
            updatePanelContent();
        }
    }

    function updatePanelContent() {
        if(!currentUser) return;
        document.getElementById('panelPfp').src = currentUser.pfp;
        document.getElementById('panelUsername').innerText = currentUser.username;
        
        // OWNER TAG & BAN COUNTER
        const tag = document.getElementById('panelTag');
        const warnTag = document.getElementById('panelWarning');
        
        if(currentUser.username === "Tasin Redwan") {
            tag.style.display = 'block';
            warnTag.style.display = 'none';
        } else {
            tag.style.display = 'none';
            // WARNING TAG
            let level = userWarningLevels[currentUser.username] || 0;
            if(level > 0) {
                warnTag.style.display = 'inline-block';
                warnTag.innerText = `Warning Level : ${level}`;
            } else {
                warnTag.style.display = 'none';
            }
        }

        // Calculate Beloved Script
        let userScripts = scripts.filter(s => s.author === currentUser.username);
        let beloved = null;
        let maxScore = -1;
        
        userScripts.forEach(s => {
            let likes = Object.values(s.userReactions).filter(r => r === 'like').length;
            let totalRating = Object.values(s.userRatings).reduce((a,b) => a+b, 0);
            let score = likes + totalRating;
            
            if(score > maxScore) {
                maxScore = score;
                beloved = s;
            }
        });
        
        const belovedEl = document.getElementById('panelBeloved');
        if(beloved) {
            belovedEl.innerText = beloved.title + " (Score: " + maxScore + ")";
        } else {
            belovedEl.innerText = "No scripts yet!";
        }
    }
    
    // --- CHANGE USERNAME LOGIC ---
    function promptChangeUsername() {
        const newName = prompt("Enter new username:", currentUser.username);
        if(newName && newName !== currentUser.username) {
            if(users[newName]) return alert("Username already exists!");
            
            const oldName = currentUser.username;
            
            // 1. Update user in users list
            users[newName] = users[oldName];
            users[newName].username = newName;
            delete users[oldName];
            localStorage.setItem('lua_hub_users', JSON.stringify(users));
            
            // 2. Update current session
            currentUser.username = newName;
            localStorage.setItem('lua_hub_session', JSON.stringify(currentUser));
            
            // 3. Update author names in scripts and comments
            scripts.forEach(s => {
                if(s.author === oldName) s.author = newName;
                s.comments.forEach(c => {
                    if(c.author === oldName) c.author = newName;
                    c.replies.forEach(r => {
                        if(r.author === oldName) r.author = newName;
                    });
                });
            });
            localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
            
            // 4. Update reports
            reportedComments.forEach(r => {
                if(r.reporter === oldName) r.reporter = newName;
                if(r.reportedUser === oldName) r.reportedUser = newName;
            });
            localStorage.setItem('lua_hub_reports', JSON.stringify(reportedComments));
            
            // 5. Update warnings
            if(userWarningLevels[oldName]) {
                userWarningLevels[newName] = userWarningLevels[oldName];
                delete userWarningLevels[oldName];
                localStorage.setItem('lua_hub_warnings', JSON.stringify(userWarningLevels));
            }
            
            updatePanelContent();
            renderScripts();
            showToast("Username updated!");
        }
    }

    // --- MAIN RENDER ---
    function renderScripts() {
        const feed = document.getElementById('scriptFeed');
        const searchTerm = document.getElementById('searchInput').value.toLowerCase();
        const searchType = hiddenInput.value;
        feed.innerHTML = '';

        const filteredScripts = scripts.filter(s => {
            if(searchType === 'name') return s.title.toLowerCase().includes(searchTerm);
            if(searchType === 'creator') return s.author.toLowerCase().includes(searchTerm);
            return true;
        });

        filteredScripts.forEach((s) => {
            const card = document.createElement('div');
            card.className = 'card';
            
            let likes = Object.values(s.userReactions).filter(r => r === 'like').length;
            let dislikes = Object.values(s.userReactions).filter(r => r === 'dislike').length;
            const totalReactions = likes + dislikes;
            const ratio = totalReactions === 0 ? 0 : Math.round((likes / totalReactions) * 100);
            
            const currentUserReaction = currentUser ? s.userReactions[currentUser.username] : null;
            const currentUserRating = currentUser ? (s.userRatings[currentUser.username] || 0) : 0;
            const showOwnerButtons = currentUser && currentUser.username === s.author;
            
            // Calculate Total Comments + Replies
            let totalCommentCount = 0;
            function countAll(commentsArray) {
                totalCommentCount += commentsArray.length;
                commentsArray.forEach(c => {
                    if(c.replies && c.replies.length > 0) countAll(c.replies);
                });
            }
            countAll(s.comments);
            
            card.innerHTML = `
                <h3>${escapeHTML(s.title)}</h3>
                <span class="author-tag" style="font-family:sans-serif; font-size:12px; color:#007bff; display:flex; align-items:center; gap:5px; margin-bottom:10px;">
                    <img src="${s.authorPfp}" class="pfp-tiny"> ${escapeHTML(s.author)}
                    ${s.author === "Tasin Redwan" ? '<span style="color:#ffc107;">👑 OWNER</span>' : ''}
                </span>
                
                <div class="card-options" onclick="toggleDropdown(event, '${s.id}')">
                    <i class="fas fa-ellipsis-v"></i>
                    <div class="dropdown-menu" id="dropdown-${s.id}">
                        <button onclick="fallbackCopy('${s.id}')"><i class="fas fa-copy"></i> Copy</button>
                        <button onclick="shareScript('${s.id}')"><i class="fas fa-share-alt"></i> Share</button>
                        ${showOwnerButtons ? `
                            <button onclick="editScript('${s.id}')"><i class="fas fa-edit"></i> Edit</button>
                            <button onclick="openDeleteModal('script', '${s.id}')" class="danger"><i class="fas fa-trash"></i> Delete</button>
                        ` : ''}
                    </div>
                </div>

                <p class="desc">${escapeHTML(s.desc)}</p>
                <pre class="line-numbers"><code class="language-lua">${escapeHTML(s.code)}</code></pre>
                
                <div style="margin-top:15px; display:flex; align-items:center; gap:10px; flex-wrap:wrap;">
                    <button class="btn btn-reaction ${currentUserReaction === 'like' ? 'active-like' : ''}" onclick="reactScript('${s.id}', 'like')"><i class="fas fa-thumbs-up"></i> ${likes}</button>
                    <button class="btn btn-reaction ${currentUserReaction === 'dislike' ? 'active-dislike' : ''}" onclick="reactScript('${s.id}', 'dislike')"><i class="fas fa-thumbs-down"></i> ${dislikes}</button>
                    
                    <div class="rating-container">
                        <input type="range" min="0" max="10" value="${currentUserRating}" class="rating-slider" onchange="rateScript('${s.id}', this.value)">
                        <span id="rating-val-${s.id}">${currentUserRating}/10</span>
                    </div>
                </div>
                
                <div class="stats-text">
                    <span>${ratio}% Like Ratio</span>
                    <span>${totalCommentCount} Comments</span>
                </div>

                <div class="comment-section">
                    <input type="text" class="comment-input" placeholder="Add a comment..." onkeydown="addComment(event, '${s.id}')">
                    <div id="comments-${s.id}">
                        ${s.comments.map(c => renderComment(c, s.id)).join('')}
                    </div>
                </div>
            `;
            feed.appendChild(card);
        });
        
        Prism.highlightAll();
    }

    function renderComment(comment, scriptId, isReply = false) {
        const cReaction = currentUser ? comment.userReactions[currentUser.username] : null;
        let cLikes = Object.values(comment.userReactions).filter(r => r === 'like').length;
        let cDislikes = Object.values(comment.userReactions).filter(r => r === 'dislike').length;
        const showOwnerButtons = currentUser && currentUser.username === comment.author;
        const isAppOwner = currentUser && currentUser.username === "Tasin Redwan";

        let repliesHtml = "";
        if(comment.replies && comment.replies.length > 0) {
            repliesHtml = `
                <div id="replies-${comment.id}" style="display:none; margin-top:5px;">
                    ${comment.replies.map(r => renderComment(r, scriptId, true)).join('')}
                </div>
                <button class="toggle-replies" onclick="toggleReplies('${comment.id}')">View ${comment.replies.length} Replies</button>
            `;
        }

        return `
            <div class="comment-item ${isReply ? 'reply-item' : ''}" id="c-${comment.id}">
                <span class="author-tag"><img src="${comment.authorPfp}" class="pfp-tiny"> ${escapeHTML(comment.author)}
                    ${comment.author === "Tasin Redwan" ? '<span style="color:#ffc107;">👑 OWNER</span>' : ''}
                    ${comment.edited ? '<span class="edited-tag">(edited)</span>' : ''}
                </span>
                <div class="comment-text">${escapeHTML(comment.text)}</div>
                
                <div class="comment-options" onclick="toggleDropdown(event, '${comment.id}')">
                    <i class="fas fa-ellipsis-v"></i>
                    <div class="dropdown-menu" id="dropdown-${comment.id}">
                        ${showOwnerButtons ? `
                            <button onclick="openInputModal('edit', 'comment', '${scriptId}', '${comment.id}')"><i class="fas fa-edit"></i> Edit</button>
                            <button onclick="openDeleteModal('comment', '${scriptId}', '${comment.id}')" class="danger"><i class="fas fa-trash"></i> Delete</button>
                        ` : `
                            <button onclick="openInputModal('report', 'comment', '${scriptId}', '${comment.id}')"><i class="fas fa-flag"></i> Report</button>
                        `}
                        ${isAppOwner && !showOwnerButtons ? `
                            <button onclick="openBanModalFromComment('${scriptId}', '${comment.id}', '${comment.author}')" class="danger"><i class="fas fa-ban"></i> Ban User</button>
                        ` : ''}
                    </div>
                </div>
                
                <div class="comment-actions">
                    <button class="btn btn-reaction ${cReaction === 'like' ? 'active-like' : ''}" style="padding: 2px 5px; font-size:10px;" onclick="reactComment('${scriptId}', '${comment.id}', 'like')"><i class="fas fa-thumbs-up"></i> ${cLikes}</button>
                    <button class="btn btn-reaction ${cReaction === 'dislike' ? 'active-dislike' : ''}" style="padding: 2px 5px; font-size:10px;" onclick="reactComment('${scriptId}', '${comment.id}', 'dislike')"><i class="fas fa-thumbs-down"></i> ${cDislikes}</button>
                    <button class="btn btn-reaction" style="padding: 2px 5px; font-size:10px;" onclick="openInputModal('reply', 'comment', '${scriptId}', '${comment.id}')"><i class="fas fa-reply"></i> Reply</button>
                </div>
                
                ${repliesHtml}
            </div>
        `;
    }
    
    function toggleReplies(commentId) {
        const div = document.getElementById(`replies-${commentId}`);
        const btn = div.nextElementSibling;
        if(div.style.display === "none") {
            div.style.display = "block";
            btn.innerText = "Hide Replies";
        } else {
            div.style.display = "none";
            btn.innerText = `View ${div.children.length} Replies`;
        }
    }

    // --- CARD FUNCTIONS ---
    function toggleDropdown(event, id) {
        event.stopPropagation();
        const menu = document.getElementById(`dropdown-${id}`);
        document.querySelectorAll('.dropdown-menu').forEach(m => {
            if(m !== menu) m.style.display = 'none';
        });
        menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
    }

    function fallbackCopy(scriptId) {
        if(currentUser && bannedUsers.includes(currentUser.username)) return showToast("Banned!");
        const script = scripts.find(s => s.id === scriptId);
        navigator.clipboard.writeText(script.code).then(() => showToast("Copied!"));
    }

    function shareScript(scriptId) {
        const url = window.location.href.split('?')[0] + "?script=" + scriptId;
        navigator.clipboard.writeText(url).then(() => showToast("Link copied!"));
    }

    function editScript(scriptId) {
        const scriptIndex = scripts.findIndex(s => s.id === scriptId);
        const s = scripts[scriptIndex];
        document.getElementById('scriptTitle').value = s.title;
        document.getElementById('scriptDesc').value = s.desc;
        editor.setValue(s.code);
        scripts.splice(scriptIndex, 1);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
        window.scrollTo(0,0);
        showToast("Editing script...");
    }

    // --- COMMENT FUNCTIONS ---
    function addComment(e, scriptId) {
        if(!currentUser) return showToast("Login first!");
        if (e.key === 'Enter' && e.target.value.trim() !== "") {
            const script = scripts.find(s => s.id === scriptId);
            if(!script) return;
            script.comments.push({
                id: 'c' + Date.now(),
                author: currentUser.username,
                authorPfp: currentUser.pfp,
                text: e.target.value,
                userReactions: {},
                replies: [],
                edited: false
            });
            localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
            e.target.value = '';
            renderScripts();
        }
    }

    function reactComment(scriptId, commentId, type) {
        if(!currentUser) return showToast("Login first!");
        const script = scripts.find(s => s.id === scriptId);
        const findAndReact = (items) => {
            for (let item of items) {
                if (item.id === commentId) {
                    if(item.userReactions[currentUser.username] === type) delete item.userReactions[currentUser.username];
                    else item.userReactions[currentUser.username] = type;
                    return true;
                }
                if (item.replies && findAndReact(item.replies)) return true;
            }
            return false;
        }
        findAndReact(script.comments);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
    }

    // --- MODAL FUNCTIONS (Professional) ---
    function openInputModal(mode, type, scriptId, commentId) {
        const modal = document.getElementById('inputModal');
        const title = document.getElementById('inputModalTitle');
        const textArea = document.getElementById('inputModalText');
        const submitBtn = document.getElementById('inputModalSubmit');
        
        modal.classList.add('show');
        textArea.value = "";
        textArea.placeholder = "";
        
        if (mode === 'edit') {
            title.innerText = `Edit ${type}`;
            const script = scripts.find(s => s.id === scriptId);
            const findItem = (items) => {
                for(let item of items) {
                    if(item.id === commentId) return item;
                    let found = findItem(item.replies);
                    if(found) return found;
                }
            }
            const item = findItem(script.comments);
            textArea.value = item.text;
            submitBtn.onclick = () => submitInputEdit(scriptId, commentId, textArea.value);
        } else if (mode === 'report') {
            title.innerText = `Report ${type}`;
            textArea.placeholder = "Reason for reporting...";
            submitBtn.onclick = () => submitReport(scriptId, commentId, textArea.value);
        } else if (mode === 'reply') {
            title.innerText = "Reply";
            textArea.placeholder = "Enter your reply...";
            submitBtn.onclick = () => submitReply(scriptId, commentId, textArea.value);
        }
    }

    function closeInputModal() { document.getElementById('inputModal').classList.remove('show'); }

    function submitInputEdit(scriptId, itemId, newText) {
        if(!newText.trim()) return showToast("Required");
        const script = scripts.find(s => s.id === scriptId);
        const findAndEdit = (items) => {
            for (let item of items) {
                if (item.id === itemId) { 
                    item.text = newText;
                    item.edited = true; // Mark as edited
                    return true; 
                }
                if (item.replies && findAndEdit(item.replies)) return true;
            }
            return false;
        }
        findAndEdit(script.comments);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
        closeInputModal();
    }

    function submitReply(scriptId, parentId, text) {
        if(!text.trim()) return showToast("Required");
        const script = scripts.find(s => s.id === scriptId);
        const findAndReply = (items) => {
            for (let item of items) {
                if (item.id === parentId) {
                    item.replies.push({
                        id: 'r' + Date.now(),
                        author: currentUser.username,
                        authorPfp: currentUser.pfp,
                        text: text,
                        userReactions: {},
                        replies: [],
                        edited: false
                    });
                    return true;
                }
                if (item.replies && findAndReply(item.replies)) return true;
            }
            return false;
        }
        findAndReply(script.comments);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
        closeInputModal();
    }

    function submitReport(scriptId, commentId, reason) {
        if(!reason.trim()) return showToast("Reason Required");
        const script = scripts.find(s => s.id === scriptId);
        const findItem = (items) => {
            for(let item of items) {
                if(item.id === commentId) return item;
                let found = findItem(item.replies);
                if(found) return found;
            }
        }
        const comment = findItem(script.comments);
        reportedComments.push({
            reporter: currentUser.username,
            reportedUser: comment.author,
            reason: reason,
            commentText: comment.text,
            scriptId: scriptId,
            commentId: commentId
        });
        localStorage.setItem('lua_hub_reports', JSON.stringify(reportedComments));
        showToast("Reported!");
        closeInputModal();
    }

    function openDeleteModal(type, scriptId, itemId) {
        const modal = document.getElementById('deleteModal');
        const text = document.getElementById('deleteModalText');
        const confirmBtn = document.getElementById('deleteModalConfirm');
        
        text.innerText = `Are you sure you want to delete this ${type}?`;
        modal.classList.add('show');
        
        confirmBtn.onclick = () => {
            if(type === 'script') {
                scripts = scripts.filter(s => s.id !== scriptId);
            } else {
                const script = scripts.find(s => s.id === scriptId);
                const findAndDelete = (items) => {
                    for (let i = 0; i < items.length; i++) {
                        if (items[i].id === itemId) { items.splice(i, 1); return true; }
                        if (items[i].replies && findAndDelete(items[i].replies)) return true;
                    }
                    return false;
                }
                findAndDelete(script.comments);
            }
            localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
            renderScripts();
            closeDeleteModal();
            showToast("Deleted!");
        }
    }

    function closeDeleteModal() { document.getElementById('deleteModal').classList.remove('show'); }

    // --- ADMIN FUNCTIONS ---
    function openAdminModal() { 
        document.getElementById('adminModal').classList.add('show'); 
        showAdminContent();
    }
    function closeAdminModal() { document.getElementById('adminModal').classList.remove('show'); }

    function showAdminContent() {
        const container = document.getElementById('adminContentArea');
        container.innerHTML = "";
        
        let reportHtml = `<h4>📋 Reported Comments</h4>`;
        if(reportedComments.length === 0) {
            reportHtml += "<p style='font-family:sans-serif; color:#aaa; font-size:14px; margin-bottom:20px;'>No reports yet.</p>";
        } else {
            reportHtml += `
                <table class="admin-table">
                    <tr><th>Reporter</th><th>User</th><th>Reason / Comment</th><th>Action</th></tr>
                    ${reportedComments.map((r, index) => `
                        <tr>
                            <td>${escapeHTML(r.reporter)}</td>
                            <td>${escapeHTML(r.reportedUser)}</td>
                            <td>
                                <b>${escapeHTML(r.reason)}</b>
                                <div class="report-comment-preview">"${escapeHTML(r.commentText)}"</div>
                            </td>
                            <td>
                                <button class="btn btn-reaction" style="background:#dc3545; color:white; font-size:12px; margin-right:5px;" onclick="openBanModalFromReport(${index})">Ban</button>
                                <button class="btn btn-reaction" style="background:#555; color:white; font-size:12px;" onclick="dismissReport(${index})">Dismiss</button>
                            </td>
                        </tr>
                    `).join('')}
                </table>
            `;
        }

        let banHtml = `<h4 style="margin-top:30px;">🚫 Banned Users</h4>`;
        if(bannedUsers.length === 0) {
            banHtml += "<p style='font-family:sans-serif; color:#aaa; font-size:14px;'>No users banned.</p>";
        } else {
            banHtml += `
                <table class="admin-table">
                    <tr><th>Username</th><th>Action</th></tr>
                    ${bannedUsers.map((user) => `
                        <tr>
                            <td>${escapeHTML(user)}</td>
                            <td>
                                <button class="btn btn-reaction" style="background:#28a745; color:white; font-size:12px;" onclick="unbanUser('${user}')">Unban</button>
                            </td>
                        </tr>
                    `).join('')}
                </table>
            `;
        }
        
        container.innerHTML = reportHtml + banHtml;
    }

    function dismissReport(index) {
        reportedComments.splice(index, 1);
        localStorage.setItem('lua_hub_reports', JSON.stringify(reportedComments));
        showAdminContent();
    }

    function openBanModalFromReport(reportIndex) {
        const report = reportedComments[reportIndex];
        openBanModal(report.reportedUser, reportIndex, report.scriptId, report.commentId);
    }
    
    function openBanModalFromComment(scriptId, commentId, username) {
        openBanModal(username, null, scriptId, commentId);
    }

    function openBanModal(username, reportIndex, scriptId, commentId) {
        const modal = document.getElementById('banModal');
        const confirmBtn = document.getElementById('banModalConfirm');
        const context = document.getElementById('banModalContext');
        
        context.innerText = `Ban user: ${username}?`;
        modal.classList.add('show');
        
        confirmBtn.onclick = () => {
            const days = document.getElementById('banDays').value;
            const reason = document.getElementById('banReason').value;
            if(!days || !reason) return showToast("Fields required");
            
            // BAN LOGIC
            if(!bannedUsers.includes(username)) {
                bannedUsers.push(username);
                localStorage.setItem('lua_hub_banned', JSON.stringify(bannedUsers));
                // Increment Warning Level
                userWarningLevels[username] = (userWarningLevels[username] || 0) + 1;
                localStorage.setItem('lua_hub_warnings', JSON.stringify(userWarningLevels));
                showToast(`User ${username} banned!`, 'ban');
            }
            
            // PURGE COMMENT LOGIC
            const script = scripts.find(s => s.id === scriptId);
            if(script) {
                script.comments = script.comments.filter(c => c.id !== commentId);
                localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
                renderScripts();
            }
            
            if(reportIndex !== null) dismissReport(reportIndex);
            
            // If the modal was opened from admin panel, refresh it
            if(document.getElementById('adminModal').classList.contains('show')) showAdminContent();
            updateLoginUI();
            
            closeBanModal();
        }
    }
    
    function unbanUser(username) {
        bannedUsers = bannedUsers.filter(u => u !== username);
        localStorage.setItem('lua_hub_banned', JSON.stringify(bannedUsers));
        
        // Mark user for unban notification
        let unbannedUsers = JSON.parse(localStorage.getItem('lua_hub_unbanned_notify')) || [];
        unbannedUsers.push(username);
        localStorage.setItem('lua_hub_unbanned_notify', JSON.stringify(unbannedUsers));
        
        showToast(`${username} unbanned!`, 'unban');
        showAdminContent();
        updateLoginUI();
    }

    function closeBanModal() { document.getElementById('banModal').classList.remove('show'); }

    // --- AUTH & UTIL ---
    function updateLoginUI() {
        const authArea = document.getElementById('authAreaContainer');
        const createCard = document.getElementById('createCard');
        if(currentUser) {
            // Count total unique users banned to show on your PFP
            let bannedCount = bannedUsers.length;
            
            authArea.innerHTML = `
                ${currentUser.username === "Tasin Redwan" ? `<button class="btn btn-admin" onclick="openAdminModal()" title="Admin Panel"><i class="fas fa-shield-alt"></i></button>` : ""}
                <div class="pfp-container">
                    <img src="${currentUser.pfp}" class="pfp-header" onclick="toggleProfilePanel(event)">
                    ${currentUser.username === "Tasin Redwan" && bannedCount > 0 ? `<div class="ban-counter">${bannedCount}</div>` : ''}
                </div>
            `;
            createCard.style.display = 'block';
            
            // Check for unban notification
            checkUnbanNotification();
        } else {
            authArea.innerHTML = `<button class="btn btn-login" onclick="openAuthModal('login')">Login</button>`;
            createCard.style.display = 'none';
        }
    }

    function checkUnbanNotification() {
        let unbannedUsers = JSON.parse(localStorage.getItem('lua_hub_unbanned_notify')) || [];
        if(unbannedUsers.includes(currentUser.username)) {
            showToast("You recently got unbanned, better be following our guidelines!", 'unban', 5000);
            // Remove from notify list
            unbannedUsers = unbannedUsers.filter(u => u !== currentUser.username);
            localStorage.setItem('lua_hub_unbanned_notify', JSON.stringify(unbannedUsers));
        }
    }

    function logout() {
        currentUser = null;
        localStorage.removeItem('lua_hub_session');
        document.getElementById('profilePanel').classList.remove('show');
        updateLoginUI();
        renderScripts();
    }
    
    function showToast(message, type = '', duration = 3000) {
        const toast = document.getElementById('toast');
        toast.innerText = message;
        toast.className = 'show ' + type;
        setTimeout(() => toast.classList.remove('show'), duration);
    }
    function escapeHTML(str) { return str.replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
    function selectPfp(imgElement, seed) {
        document.querySelectorAll('.pfp-option').forEach(img => img.classList.remove('selected'));
        imgElement.classList.add('selected');
        selectedPfpSeed = seed;
    }
    function closeAuthModal() { document.getElementById('authModal').classList.remove('show'); }
    function openAuthModal(type) {
        authType = type;
        updateAuthModalUI();
        document.getElementById('authModal').classList.add('show');
    }
    function toggleAuthType() {
        authType = authType === 'login' ? 'register' : 'login';
        updateAuthModalUI();
    }
    function updateAuthModalUI() {
        const title = document.getElementById('authTitle');
        const submitBtn = document.getElementById('authSubmitBtn');
        const toggleText = document.getElementById('authToggleText');
        if(authType === 'login') {
            title.innerText = 'Login';
            submitBtn.innerText = 'Login';
            toggleText.innerHTML = "Don't have an account? <span>Sign Up</span>";
        } else {
            title.innerText = 'Sign Up';
            submitBtn.innerText = 'Register';
            toggleText.innerHTML = "Already have an account? <span>Login</span>";
        }
    }
    function processAuth() {
        const username = document.getElementById('authUsername').value.trim();
        const password = document.getElementById('authPassword').value;
        const pfp = `https://api.dicebear.com/7.x/pixel-art/svg?${selectedPfpSeed}`;
        if(!username || !password) return showToast("All fields required");
        
        if(authType === 'register') {
            if(users[username]) return showToast("Username already exists!");
            users[username] = { username, password, pfp };
            localStorage.setItem('lua_hub_users', JSON.stringify(users));
            showToast("Account created! Please login.");
            toggleAuthType();
        } else {
            // Check if banned
            if(bannedUsers.includes(username)) return showToast("Account is banned!", 'ban');
            
            if(users[username] && users[username].password === password) {
                currentUser = users[username];
                localStorage.setItem('lua_hub_session', JSON.stringify(currentUser));
                closeAuthModal();
                updateLoginUI();
                renderScripts();
                showToast("Logged in!");
            } else {
                showToast("Incorrect username or password");
            }
        }
    }
    function rateScript(id, rating) {
        if(!currentUser) return showToast("Login first!");
        if(bannedUsers.includes(currentUser.username)) return showToast("Banned!", 'ban');
        const script = scripts.find(s => s.id === id);
        if(!script) return;
        script.userRatings[currentUser.username] = parseInt(rating);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        document.getElementById(`rating-val-${id}`).innerText = `${rating}/10`;
        showToast("Rating saved!");
    }
    function reactScript(id, type) {
        if(!currentUser) return showToast("Login first!");
        if(bannedUsers.includes(currentUser.username)) return showToast("Banned!", 'ban');
        const script = scripts.find(s => s.id === id);
        if(!script) return;
        if(script.userReactions[currentUser.username] === type) delete script.userReactions[currentUser.username];
        else script.userReactions[currentUser.username] = type;
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
    }
    const editor = CodeMirror.fromTextArea(document.getElementById("scriptCode"), { lineNumbers: false, theme: "dracula", mode: "lua" });
    function saveScript() {
        if(!currentUser) return;
        if(bannedUsers.includes(currentUser.username)) return showToast("Banned!", 'ban');
        const title = document.getElementById('scriptTitle').value;
        const desc = document.getElementById('scriptDesc').value;
        const code = editor.getValue();
        if(!title || !code) return showToast("Title and Code required!");
        const newScript = { id: 'id' + Date.now(), author: currentUser.username, authorPfp: currentUser.pfp, title, desc, code, userReactions: {}, userRatings: {}, comments: [] };
        scripts.unshift(newScript);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
        document.getElementById('scriptTitle').value = '';
        document.getElementById('scriptDesc').value = '';
        editor.setValue('');
        showToast("Published!");
    }

    updateLoginUI();
    renderScripts();
</script>

</body>
</html>
