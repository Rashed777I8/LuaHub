<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
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
        /* --- MOBILE RESPONSIVE FIXES --- */
        body { 
            font-family: 'Fredoka One', cursive; 
            background-color: #121212; 
            color: #ffffff; 
            padding: 5px; 
            margin: 0; 
            box-sizing: border-box; 
            overflow-x: hidden; 
        }
        .container { 
            width: 100%; 
            max-width: 500px; 
            margin: auto; 
            padding: 5px;
            box-sizing: border-box;
        }
        
        /* PROFESSIONAL HEADER */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            background: #1e1e1e;
            padding: 10px;
            border-radius: 12px;
            border: 2px solid #333;
            position: relative;
        }
        .logo { font-size: 20px; font-weight: bold; }
        .logo-hub { color: #007bff; }
        .header-actions { display: flex; align-items: center; gap: 10px; }
        
        /* Header PFP */
        .pfp-container { position: relative; }
        .pfp-header { 
            width: 40px; height: 40px; 
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
            padding: 15px;
            width: 220px;
            z-index: 1000;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            text-align: center;
        }
        .profile-panel.show { display: block; }
        
        .panel-pfp { width: 70px; height: 70px; border-radius: 50%; border: 4px solid #007bff; margin-bottom: 10px; }
        
        /* Updated Username Style */
        .panel-username { 
            font-size: 16px; margin-bottom: 5px; cursor: pointer; 
            display: inline-block; padding: 2px 8px; border-radius: 4px;
        }
        .panel-username:hover { background: #333; }
        
        .owner-tag { color: #ffc107; font-size: 12px; font-family: sans-serif; display: block; margin-bottom: 5px; font-weight: bold;}
        .warning-tag {
            color: #dc3545; font-size: 12px; font-family: sans-serif;
            display: inline-block; margin-bottom: 10px; background: rgba(220,53,69,0.2);
            padding: 2px 6px; border-radius: 4px; cursor: help;
        }
        .beloved-title { font-size: 11px; color: #aaa; margin-top: 10px; }
        .beloved-script { 
            background: #2a2a2a; 
            padding: 8px; 
            border-radius: 8px; 
            margin-top: 5px; 
            font-size: 13px; 
            border: 1px solid #333;
        }
        .panel-logout { width: 100%; margin-top: 15px; background: #dc3545; }

        /* PROFESSIONAL SEARCH BAR */
        .search-container {
            display: flex;
            flex-wrap: wrap; /* MOBILE FIX */
            gap: 10px;
            margin-bottom: 20px;
            background: #1e1e1e;
            padding: 10px;
            border-radius: 12px;
            border: 2px solid #333;
            align-items: center;
        }
        .search-input { 
            flex-grow: 1; padding: 10px;
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px;
            font-family: 'Fredoka One', cursive; 
            font-size: 14px;
            min-width: 150px; /* MOBILE FIX */
        }
        
        /* Custom Dropdown Styling */
        .custom-select-wrapper {
            position: relative;
            user-select: none;
            min-width: 100px;
        }
        .custom-select {
            background: #2a2a2a;
            border: 2px solid #444;
            color: #fff;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-family: 'Fredoka One', cursive;
            font-size: 13px;
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
            padding: 10px;
            cursor: pointer;
            font-family: 'Fredoka One', cursive;
            font-size: 13px;
            transition: background 0.2s;
        }
        .custom-select-option:hover { background: #333; }
        .custom-select-option.selected { background: #007bff; color: white; }

        /* Card Styles */
        .card { 
            background: #1e1e1e; 
            border: 2px solid #333; 
            border-radius: 12px; 
            padding: 15px; 
            margin-bottom: 20px;
            transition: all 0.3s ease;
            position: relative;
            box-sizing: border-box; /* MOBILE FIX */
        }
        .card:hover { border-color: #555; }
        h3 { font-size: 18px; margin-top: 0; margin-bottom: 5px; }
        p.desc { color: #aaa; font-family: sans-serif; margin-bottom: 15px; font-size: 14px; }
        
        /* --- DARK CODEBOX BACKGROUND --- */
        pre[class*="language-"] {
            border-radius: 8px;
            overflow-x: auto;
            max-height: 400px;
            margin-top: 10px;
            white-space: pre;
            background: #121212 !important; 
            border: 1px solid #333;
            width: 100%; /* MOBILE FIX */
            box-sizing: border-box;
        }
        code[class*="language-"] {
            font-family: 'Fira Code', monospace;
            font-size: 12px; /* MOBILE FIX */
        }
        
        input, textarea { 
            width: 100%; padding: 10px; margin: 10px 0; 
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px; 
            box-sizing: border-box;
            font-family: 'Fredoka One', cursive; 
        }

        /* CARD MENU (Three Dots) */
        .card-options {
            position: absolute;
            top: 15px;
            right: 15px;
            cursor: pointer;
            color: #888;
            padding: 5px;
            font-size: 16px;
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
            min-width: 130px;
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
            font-size: 13px;
            border-radius: 4px;
        }
        .dropdown-menu button:hover { background: #444; }
        .dropdown-menu button.danger { color: #dc3545; }

        .btn-post { background: #007bff; color: white; width: 100%; }
        
        /* Reaction Buttons */
        .btn-reaction { background: #333; color: #aaa; padding: 5px 8px; font-size: 11px; font-family: 'Fredoka One', cursive; }
        .btn-reaction.active-like { background: #28a745; color: white; }
        .btn-reaction.active-dislike { background: #dc3545; color: white; }
        
        /* 10-Point Rating Slider */
        .rating-container {
            display: flex;
            align-items: center;
            gap: 8px;
            font-family: sans-serif;
            font-size: 12px;
            color: #aaa;
            background: #2a2a2a;
            padding: 5px 8px;
            border-radius: 8px;
            flex-grow: 1;
            flex-wrap: wrap; /* MOBILE FIX */
        }
        .rating-slider {
            -webkit-appearance: none;
            width: 100px;
            height: 6px;
            background: #444;
            border-radius: 5px;
            outline: none;
        }
        .rating-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: #007bff;
            border-radius: 50%;
            cursor: pointer;
        }

        /* Comments Styling */
        .comment-section { margin-top: 15px; border-top: 1px solid #444; padding-top: 10px; }
        .comment-input {
            width: 100%; padding: 8px;
            background: #2a2a2a; border: 1px solid #444;
            border-radius: 8px; color: white;
            font-family: sans-serif; box-sizing: border-box;
            font-size: 13px;
        }
        .comment-item {
            background: #2a2a2a; padding: 8px; border-radius: 8px;
            margin-top: 8px; font-family: sans-serif; font-size: 13px; position: relative;
        }
        .reply-item { margin-left: 15px; border-left: 2px solid #444; }
        
        /* Updated Author Tag Styling for "edited" marker */
        .author-tag { 
            color: #007bff; font-weight: bold; margin-bottom: 3px; 
            display: flex; align-items: center; gap: 5px;
            font-family: 'Fredoka One', cursive;
            font-size: 12px;
        }
        .edited-tag { color: #888; font-size: 9px; font-weight: normal; font-family: sans-serif; }
        
        .pfp-tiny { width: 14px; height: 14px; border-radius: 50%; object-fit: cover; }
        .comment-text { word-wrap: break-word; white-space: pre-wrap; padding-right: 20px; font-family: sans-serif; color: #eee; font-size: 13px; }
        .comment-actions { display: flex; gap: 5px; margin-top: 5px; }
        .comment-options { position: absolute; top: 8px; right: 8px; cursor: pointer; color: #888; font-size: 14px; }
        .stats-text { font-family: sans-serif; font-size: 12px; color: #aaa; margin-top: 10px; display: flex; gap: 10px; }
        .toggle-replies { background: none; border: none; color: #007bff; cursor: pointer; font-size: 11px; margin-top: 3px; font-family: sans-serif; }

        /* MODAL */
        .modal {
            display: none; position: fixed; top: 0; left: 0; 
            width: 100%; height: 100%; background-color: rgba(0,0,0,0.85);
            justify-content: center; align-items: center; z-index: 2000;
            overflow-y: auto; padding: 10px; box-sizing: border-box;
        }
        .modal.show { display: flex; }
        
        .modal-content {
            background: #1e1e1e; padding: 20px; border-radius: 12px;
            border: 2px solid #007bff; text-align: center;
            width: 100%; max-width: 350px;
            box-sizing: border-box; /* MOBILE FIX */
        }
        .modal-buttons { display: flex; justify-content: center; gap: 10px; margin-top: 15px; }
        
        .pfp-option { width: 40px; height: 40px; border-radius: 50%; cursor: pointer; border: 3px solid transparent; }
        .pfp-option.selected { border-color: #007bff; }
        
        .auth-toggle { font-family: sans-serif; font-size: 13px; color: #aaa; margin-top: 10px; cursor: pointer; }
        .auth-toggle span { color: #007bff; text-decoration: underline; }

        /* ADMIN PANEL STYLING - PROFESSIONAL */
        .admin-modal-content { max-width: 500px; text-align: left; border-color: #dc3545; }
        .admin-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #333; padding-bottom: 10px; margin-bottom: 15px; }
        .admin-table { width: 100%; border-collapse: separate; border-spacing: 0; margin-top: 10px; font-family: sans-serif; color: white; font-size: 12px;}
        .admin-table th, .admin-table td { border-bottom: 1px solid #333; padding: 10px 5px; text-align: left; }
        .admin-table th { background: #2a2a2a; color: #aaa; font-weight: bold; }
        .admin-table tr:hover { background: #252525; }
        .report-comment-preview { background: #121212; padding: 5px; border-radius: 5px; font-size: 11px; color: #aaa; margin-top: 3px; border: 1px solid #333; font-family: 'Fira Code', monospace;}
        
        /* TOAST NOTIFICATION */
        #toast {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            background: #28a745; color: white; padding: 10px 20px; border-radius: 8px;
            display: none; z-index: 3000; font-family: 'Fredoka One', cursive; text-align: center; font-size: 14px;
        }
        #toast.show { display: block; }
        #toast.ban { background: #dc3545; }
        #toast.unban { background: #ffc107; color: #121212; }

        /* --- FOLDER STYLES --- */
        .btn-add-folder {
            background: #28a745;
            color: white;
            border: none;
            border-radius: 50%;
            width: 36px;
            height: 36px;
            font-size: 22px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Fredoka One', cursive;
            transition: all 0.2s;
            line-height: 1;
        }
        .btn-add-folder:hover { opacity: 0.85; transform: scale(1.08); }

        .folder-card {
            background: #1a1a2e;
            border: 2px solid #2a2a5e;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 20px;
            box-sizing: border-box;
            position: relative;
        }
        .folder-header {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
        }
        .folder-icon { font-size: 20px; }
        .folder-title {
            font-size: 17px;
            font-family: 'Fredoka One', cursive;
            cursor: pointer;
            padding: 2px 6px;
            border-radius: 4px;
            flex-grow: 1;
        }
        .folder-title:hover { background: #2a2a4e; }
        .folder-desc {
            color: #8888aa;
            font-family: sans-serif;
            font-size: 13px;
            margin-bottom: 10px;
            cursor: pointer;
            padding: 2px 4px;
            border-radius: 4px;
            display: inline-block;
        }
        .folder-desc:hover { background: #2a2a4e; }
        .folder-scripts { margin-top: 10px; }
        .folder-add-script-area {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }
        .btn-script-type {
            flex: 1;
            background: #2a2a4e;
            color: #aaaaff;
            border: 1px solid #3a3a6e;
            border-radius: 8px;
            padding: 8px 5px;
            font-family: 'Fredoka One', cursive;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .btn-script-type:hover { background: #3a3a6e; color: white; }
        .folder-script-card {
            background: #121225;
            border: 1px solid #2a2a5e;
            border-radius: 10px;
            padding: 12px;
            margin-bottom: 10px;
            position: relative;
            box-sizing: border-box;
        }
        .script-type-badge {
            display: inline-block;
            font-size: 10px;
            font-family: 'Fira Code', monospace;
            padding: 2px 7px;
            border-radius: 4px;
            margin-bottom: 6px;
            font-weight: bold;
        }
        .badge-server { background: #2a4a2a; color: #4caf50; border: 1px solid #3a6a3a; }
        .badge-local { background: #4a2a2a; color: #ff7043; border: 1px solid #6a3a3a; }
        .folder-script-title {
            font-size: 15px;
            font-family: 'Fredoka One', cursive;
            margin-bottom: 3px;
        }
        .folder-script-desc {
            color: #7788aa;
            font-family: sans-serif;
            font-size: 12px;
            margin-bottom: 8px;
        }
        .folder-card-options {
            position: absolute;
            top: 10px;
            right: 10px;
            cursor: pointer;
            color: #666;
            font-size: 14px;
            padding: 3px;
        }
        .folder-card-options:hover { color: white; }

        /* Script type chooser inside folder create modal */
        .script-type-chooser {
            display: flex;
            gap: 10px;
            margin: 10px 0;
        }
        .script-type-btn-big {
            flex: 1;
            background: #2a2a4e;
            color: #aaaaff;
            border: 2px solid #3a3a6e;
            border-radius: 10px;
            padding: 14px 8px;
            font-family: 'Fredoka One', cursive;
            font-size: 15px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .script-type-btn-big:hover, .script-type-btn-big.selected { background: #3a3a8e; color: white; border-color: #007bff; }
        .script-type-label {
            display: block;
            font-size: 10px;
            font-family: 'Fira Code', monospace;
            margin-top: 4px;
            opacity: 0.7;
        }
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
                <i class="fas fa-chevron-down" style="font-size:11px; margin-left:5px;"></i>
            </div>
            <div class="custom-select-options" id="searchTypeOptions">
                <div class="custom-select-option selected" data-value="name">Name</div>
                <div class="custom-select-option" data-value="creator">Creator</div>
            </div>
        </div>
        <input type="hidden" id="searchType" value="name">
    </div>

    <!-- createCard is now hidden; scripts only go inside folders -->
    <div class="card" id="createCard" style="display:none;">
        <h2 style="font-size:18px;">✍️ Create Script</h2>
        <input type="text" id="scriptTitle" placeholder="Script Title">
        <textarea id="scriptDesc" rows="2" placeholder="Description"></textarea>
        <textarea id="scriptCode"></textarea>
        <button class="btn btn-post" onclick="saveScript()">Post Script</button>
    </div>

    <div id="scriptFeed"></div>
</div>

<!-- AUTH MODAL -->
<div id="authModal" class="modal">
    <div class="modal-content">
        <h3 id="authTitle" style="font-size:18px;">Login</h3>
        <input type="text" id="authUsername" placeholder="Username">
        <input type="password" id="authPassword" placeholder="Password">
        
        <p style="font-family:sans-serif; margin-top:10px; color:#aaa; font-size:13px;">Choose Profile Picture:</p>
        <div style="display:flex; justify-content:center; gap:8px; margin-bottom:10px;">
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
        <h3 id="inputModalTitle" style="font-size:18px;">Edit</h3>
        <textarea id="inputModalText" rows="3"></textarea>
        
        <div class="modal-buttons">
            <button class="btn btn-post" id="inputModalSubmit">Submit</button>
            <button class="btn btn-logout" onclick="closeInputModal()">Cancel</button>
        </div>
    </div>
</div>

<div id="deleteModal" class="modal">
    <div class="modal-content" style="border-color:#dc3545;">
        <h3 style="font-size:18px;">⚠️ Delete Item?</h3>
        <p style="color:white; font-family:sans-serif; font-size:14px;" id="deleteModalText">Are you sure you want to Delete this item?</p>
        
        <div class="modal-buttons">
            <button class="btn btn-logout" style="background:#dc3545;" id="deleteModalConfirm">Yes</button>
            <button class="btn btn-post" onclick="closeDeleteModal()">No</button>
        </div>
    </div>
</div>

<div id="adminModal" class="modal">
    <div class="modal-content admin-modal-content">
        <div class="admin-header">
            <h3 style="font-size:18px;">🛡️ Admin Dashboard</h3>
            <button class="btn" style="background:#333; padding:5px 10px;" onclick="closeAdminModal()">X</button>
        </div>
        <p style="color:#aaa; font-family:sans-serif; font-size:13px;">Welcome, Administrator Tasin Redwan.</p>
        
        <div id="adminContentArea"></div>
        
    </div>
</div>

<div id="banModal" class="modal">
    <div class="modal-content" style="border-color:#dc3545;">
        <h3 style="font-size:18px;">🚫 Ban User?</h3>
        <p style="color:white; font-family:sans-serif; font-size:14px;" id="banModalContext"></p>
        <input type="number" id="banDays" placeholder="Days to Ban">
        <textarea id="banReason" placeholder="Reason for Ban" rows="2"></textarea>
        
        <div class="modal-buttons">
            <button class="btn btn-logout" style="background:#dc3545;" id="banModalConfirm">Ban</button>
            <button class="btn btn-post" onclick="closeBanModal()">Cancel</button>
        </div>
    </div>
</div>

<!-- FOLDER CREATE MODAL -->
<div id="folderModal" class="modal">
    <div class="modal-content" style="border-color:#28a745; text-align:left;">
        <h3 style="font-size:18px; text-align:center;">📁 Create Folder</h3>
        <input type="text" id="folderNameInput" placeholder="Folder Name" style="margin-bottom:5px;">
        <textarea id="folderDescInput" rows="2" placeholder="Folder Description (optional)"></textarea>
        <div class="modal-buttons">
            <button class="btn btn-post" style="background:#28a745;" onclick="submitCreateFolder()">Create</button>
            <button class="btn btn-logout" onclick="closeFolderModal()">Cancel</button>
        </div>
    </div>
</div>

<!-- ADD SCRIPT TO FOLDER MODAL -->
<div id="addScriptModal" class="modal">
    <div class="modal-content" style="border-color:#5555ff; text-align:left;">
        <h3 style="font-size:17px; text-align:center;" id="addScriptModalTitle">Add Script to Folder</h3>
        
        <!-- Step 1: choose type -->
        <div id="addScriptStep1">
            <p style="color:#aaa; font-family:sans-serif; font-size:13px; text-align:center; margin-bottom:5px;">Choose Script Type:</p>
            <div class="script-type-chooser">
                <button class="script-type-btn-big" onclick="chooseScriptType('ServerScript')">
                    🖥️ ServerScript
                    <span class="script-type-label">Runs on Server</span>
                </button>
                <button class="script-type-btn-big" onclick="chooseScriptType('LocalScript')">
                    💻 LocalScript
                    <span class="script-type-label">Runs on Client</span>
                </button>
            </div>
        </div>

        <!-- Step 2: fill in script details -->
        <div id="addScriptStep2" style="display:none;">
            <div id="chosenTypeBadgeArea" style="margin-bottom:8px;"></div>
            <input type="text" id="folderScriptTitle" placeholder="Script Title">
            <textarea id="folderScriptDesc" rows="2" placeholder="Description (optional)"></textarea>
            <textarea id="folderScriptCode"></textarea>
            <div class="modal-buttons">
                <button class="btn btn-post" onclick="submitAddScriptToFolder()">Add Script</button>
                <button class="btn btn-logout" onclick="backToScriptTypeChoice()">Back</button>
                <button class="btn btn-logout" onclick="closeAddScriptModal()">Cancel</button>
            </div>
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

    // --- FOLDER DATA ---
    let folders = JSON.parse(localStorage.getItem('lua_hub_folders')) || [];

    // Check if current user is banned
    if(currentUser && bannedUsers.includes(currentUser.username)) {
        logout();
        alert("Your account has been banned.");
    }
    
    let selectedPfpSeed = 'seed=1';
    let authType = 'login';
    
    // Data Migration & Deduplication
    scripts.forEach(s => {
        if(!s.userReactions) s.userReactions = {};
        if(!s.userRatings) s.userRatings = {};
        if(!s.comments) s.comments = [];
        
        // Fix [SHARED] text
        if(s.title.startsWith("[SHARED] ")) {
            s.title = s.title.substring(9);
        }
        
        s.comments.forEach(c => {
            if(!c.replies) c.replies = [];
            if(!c.userReactions) c.userReactions = {};
            if(c.edited === undefined) c.edited = false;
            c.replies.forEach(r => {
                if(r.edited === undefined) r.edited = false;
            });
        });
    });

    // Deduplication Logic
    let uniqueScripts = [];
    let seen = new Set();
    scripts.forEach(s => {
        if(!seen.has(s.id)) {
            uniqueScripts.push(s);
            seen.add(s.id);
        }
    });
    scripts = uniqueScripts;
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

        // Calculate Beloved Script (from both standalone scripts and folder scripts)
        let allUserScripts = scripts.filter(s => s.author === currentUser.username);
        // also check folder scripts
        folders.forEach(f => {
            if(f.scripts) f.scripts.forEach(fs => {
                if(fs.author === currentUser.username) {
                    allUserScripts.push({...fs, userReactions: fs.userReactions || {}, userRatings: fs.userRatings || {}});
                }
            });
        });

        let beloved = null;
        let maxScore = -1;
        
        allUserScripts.forEach(s => {
            let likes = Object.values(s.userReactions || {}).filter(r => r === 'like').length;
            let totalRating = Object.values(s.userRatings || {}).reduce((a,b) => a+b, 0);
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

            // Update author names in folder scripts
            folders.forEach(f => {
                if(f.author === oldName) f.author = newName;
                if(f.scripts) f.scripts.forEach(fs => {
                    if(fs.author === oldName) fs.author = newName;
                });
            });
            localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
            
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

        // Render Folders first
        const filteredFolders = folders.filter(f => {
            if(searchType === 'name') return f.name.toLowerCase().includes(searchTerm);
            if(searchType === 'creator') return f.author.toLowerCase().includes(searchTerm);
            return true;
        });
        filteredFolders.forEach(f => {
            feed.appendChild(renderFolderCard(f));
        });

        // Render standalone scripts (legacy / if any remain)
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
                <span class="author-tag" style="font-family:sans-serif; font-size:11px; color:#007bff; display:flex; align-items:center; gap:5px; margin-bottom:10px;">
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
                
                <div style="margin-top:15px; display:flex; align-items:center; gap:5px; flex-wrap:wrap;">
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

    // --- FOLDER RENDER ---
    function renderFolderCard(folder) {
        const div = document.createElement('div');
        div.className = 'folder-card';
        const isOwner = currentUser && currentUser.username === folder.author;

        let scriptsHtml = '';
        if(folder.scripts && folder.scripts.length > 0) {
            scriptsHtml = folder.scripts.map(fs => renderFolderScript(fs, folder.id, isOwner)).join('');
        } else {
            scriptsHtml = `<p style="color:#555; font-family:sans-serif; font-size:12px; margin:5px 0;">No scripts yet. Add one below!</p>`;
        }

        div.innerHTML = `
            <div class="folder-header">
                <span class="folder-icon">📁</span>
                <span class="folder-title" onclick="${isOwner ? `renameFolderTitle('${folder.id}', this)` : ''}" title="${isOwner ? 'Click to rename' : ''}">${escapeHTML(folder.name)}</span>
                <div class="card-options" onclick="toggleDropdown(event, 'folder-${folder.id}')" style="position:static; margin-left:auto;">
                    <i class="fas fa-ellipsis-v"></i>
                    <div class="dropdown-menu" id="dropdown-folder-${folder.id}" style="right:0; top:auto;">
                        ${isOwner ? `<button onclick="openDeleteFolderModal('${folder.id}')" class="danger"><i class="fas fa-trash"></i> Delete Folder</button>` : ''}
                    </div>
                </div>
            </div>
            <span class="folder-desc" onclick="${isOwner ? `renameFolderDesc('${folder.id}', this)` : ''}" title="${isOwner ? 'Click to edit description' : ''}">${escapeHTML(folder.desc || 'No description.')}</span>
            <p style="font-family:sans-serif; font-size:11px; color:#556; margin:0 0 8px 0;">By ${escapeHTML(folder.author)} ${folder.author === 'Tasin Redwan' ? '👑' : ''}</p>

            <div class="folder-scripts" id="folder-scripts-${folder.id}">
                ${scriptsHtml}
            </div>

            ${isOwner ? `
            <div class="folder-add-script-area" style="margin-top:10px;">
                <button class="btn-script-type" onclick="openAddScriptModal('${folder.id}', 'ServerScript')">
                    <i class="fas fa-plus"></i> ServerScript
                </button>
                <button class="btn-script-type" onclick="openAddScriptModal('${folder.id}', 'LocalScript')">
                    <i class="fas fa-plus"></i> LocalScript
                </button>
            </div>
            ` : ''}
        `;
        return div;
    }

    function renderFolderScript(fs, folderId, isOwner) {
        const badgeClass = fs.scriptType === 'ServerScript' ? 'badge-server' : 'badge-local';
        return `
            <div class="folder-script-card" id="fs-${fs.id}">
                <span class="script-type-badge ${badgeClass}">${escapeHTML(fs.scriptType)}</span>
                <div class="folder-card-options" onclick="toggleDropdown(event, 'fs-${fs.id}')">
                    <i class="fas fa-ellipsis-v"></i>
                    <div class="dropdown-menu" id="dropdown-fs-${fs.id}" style="right:0; top:auto; position:absolute;">
                        <button onclick="copyFolderScript('${folderId}', '${fs.id}')"><i class="fas fa-copy"></i> Copy</button>
                        ${isOwner ? `<button onclick="openDeleteFolderScriptModal('${folderId}', '${fs.id}')" class="danger"><i class="fas fa-trash"></i> Delete</button>` : ''}
                    </div>
                </div>
                <div class="folder-script-title">${escapeHTML(fs.title)}</div>
                ${fs.desc ? `<div class="folder-script-desc">${escapeHTML(fs.desc)}</div>` : ''}
                <pre class="line-numbers" style="margin-top:5px;"><code class="language-lua">${escapeHTML(fs.code)}</code></pre>
            </div>
        `;
    }

    // --- FOLDER FUNCTIONS ---
    function openFolderModal() {
        if(!currentUser) return showToast("Login first!");
        document.getElementById('folderNameInput').value = '';
        document.getElementById('folderDescInput').value = '';
        document.getElementById('folderModal').classList.add('show');
    }
    function closeFolderModal() { document.getElementById('folderModal').classList.remove('show'); }

    function submitCreateFolder() {
        const name = document.getElementById('folderNameInput').value.trim();
        const desc = document.getElementById('folderDescInput').value.trim();
        if(!name) return showToast("Folder name required!");
        const newFolder = {
            id: 'f' + Date.now(),
            name,
            desc,
            author: currentUser.username,
            authorPfp: currentUser.pfp,
            scripts: []
        };
        folders.unshift(newFolder);
        localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
        closeFolderModal();
        renderScripts();
        showToast("Folder created!");
    }

    function renameFolderTitle(folderId, el) {
        const folder = folders.find(f => f.id === folderId);
        if(!folder) return;
        const newName = prompt("Rename folder:", folder.name);
        if(newName && newName.trim()) {
            folder.name = newName.trim();
            localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
            el.innerText = folder.name;
        }
    }

    function renameFolderDesc(folderId, el) {
        const folder = folders.find(f => f.id === folderId);
        if(!folder) return;
        const newDesc = prompt("Edit description:", folder.desc || '');
        if(newDesc !== null) {
            folder.desc = newDesc.trim();
            localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
            el.innerText = folder.desc || 'No description.';
        }
    }

    function openDeleteFolderModal(folderId) {
        const modal = document.getElementById('deleteModal');
        document.getElementById('deleteModalText').innerText = 'Are you sure you want to delete this folder and all its scripts?';
        modal.classList.add('show');
        document.getElementById('deleteModalConfirm').onclick = () => {
            folders = folders.filter(f => f.id !== folderId);
            localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
            renderScripts();
            closeDeleteModal();
            showToast("Folder deleted!");
        };
    }

    function openDeleteFolderScriptModal(folderId, scriptId) {
        const modal = document.getElementById('deleteModal');
        document.getElementById('deleteModalText').innerText = 'Are you sure you want to delete this script?';
        modal.classList.add('show');
        document.getElementById('deleteModalConfirm').onclick = () => {
            const folder = folders.find(f => f.id === folderId);
            if(folder) {
                folder.scripts = folder.scripts.filter(s => s.id !== scriptId);
                localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
                renderScripts();
                showToast("Script deleted!");
            }
            closeDeleteModal();
        };
    }

    function copyFolderScript(folderId, scriptId) {
        const folder = folders.find(f => f.id === folderId);
        if(!folder) return;
        const fs = folder.scripts.find(s => s.id === scriptId);
        if(!fs) return;
        navigator.clipboard.writeText(fs.code).then(() => showToast("Copied!"));
    }

    // --- ADD SCRIPT TO FOLDER MODAL ---
    let _addScriptFolderId = null;
    let _addScriptType = null;
    let _folderScriptEditor = null;

    function openAddScriptModal(folderId, scriptType) {
        if(!currentUser) return showToast("Login first!");
        _addScriptFolderId = folderId;
        _addScriptType = scriptType;
        document.getElementById('folderScriptTitle').value = '';
        document.getElementById('folderScriptDesc').value = '';
        document.getElementById('folderScriptCode').value = '';
        // Go directly to step 2
        document.getElementById('addScriptStep1').style.display = 'none';
        document.getElementById('addScriptStep2').style.display = 'block';
        const badgeClass = scriptType === 'ServerScript' ? 'badge-server' : 'badge-local';
        document.getElementById('chosenTypeBadgeArea').innerHTML = `<span class="script-type-badge ${badgeClass}">${scriptType}</span>`;
        document.getElementById('addScriptModal').classList.add('show');
        // Init CodeMirror for folder script
        setTimeout(() => {
            if(_folderScriptEditor) {
                _folderScriptEditor.toTextArea();
                _folderScriptEditor = null;
            }
            _folderScriptEditor = CodeMirror.fromTextArea(document.getElementById('folderScriptCode'), {
                lineNumbers: false,
                theme: 'dracula',
                mode: 'lua'
            });
        }, 100);
    }

    function chooseScriptType(type) {
        _addScriptType = type;
        document.getElementById('addScriptStep1').style.display = 'none';
        document.getElementById('addScriptStep2').style.display = 'block';
        const badgeClass = type === 'ServerScript' ? 'badge-server' : 'badge-local';
        document.getElementById('chosenTypeBadgeArea').innerHTML = `<span class="script-type-badge ${badgeClass}">${type}</span>`;
        setTimeout(() => {
            if(_folderScriptEditor) {
                _folderScriptEditor.toTextArea();
                _folderScriptEditor = null;
            }
            _folderScriptEditor = CodeMirror.fromTextArea(document.getElementById('folderScriptCode'), {
                lineNumbers: false,
                theme: 'dracula',
                mode: 'lua'
            });
        }, 100);
    }

    function backToScriptTypeChoice() {
        document.getElementById('addScriptStep1').style.display = 'block';
        document.getElementById('addScriptStep2').style.display = 'none';
        if(_folderScriptEditor) {
            _folderScriptEditor.toTextArea();
            _folderScriptEditor = null;
        }
    }

    function closeAddScriptModal() {
        document.getElementById('addScriptModal').classList.remove('show');
        document.getElementById('addScriptStep1').style.display = 'block';
        document.getElementById('addScriptStep2').style.display = 'none';
        if(_folderScriptEditor) {
            _folderScriptEditor.toTextArea();
            _folderScriptEditor = null;
        }
        _addScriptFolderId = null;
        _addScriptType = null;
    }

    function submitAddScriptToFolder() {
        if(!currentUser) return showToast("Login first!");
        const title = document.getElementById('folderScriptTitle').value.trim();
        const desc = document.getElementById('folderScriptDesc').value.trim();
        const code = _folderScriptEditor ? _folderScriptEditor.getValue() : document.getElementById('folderScriptCode').value;
        if(!title || !code) return showToast("Title and Code required!");
        const folder = folders.find(f => f.id === _addScriptFolderId);
        if(!folder) return;
        folder.scripts.push({
            id: 'fs' + Date.now(),
            title,
            desc,
            code,
            scriptType: _addScriptType,
            author: currentUser.username,
            authorPfp: currentUser.pfp,
            userReactions: {},
            userRatings: {}
        });
        localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
        closeAddScriptModal();
        renderScripts();
        showToast("Script added!");
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

    // --- UNIVERSAL SHARE LOGIC (Base64) ---
    function shareScript(scriptId) {
        const script = scripts.find(s => s.id === scriptId);
        if (!script) return;

        const shareObj = {
            t: script.title,
            d: script.desc,
            c: script.code,
            a: script.author,
            p: script.authorPfp,
            id: script.id
        };

        try {
            const encodedData = btoa(unescape(encodeURIComponent(JSON.stringify(shareObj))));
            const shareUrl = window.location.origin + window.location.pathname + "?data=" + encodedData;

            if (navigator.share) {
                navigator.share({
                    title: script.title,
                    text: `Check out this Lua script: ${script.title}`,
                    url: shareUrl
                }).catch(() => {
                    copyToClipboard(shareUrl);
                });
            } else {
                copyToClipboard(shareUrl);
            }
        } catch (e) {
            showToast("Error generating link", "ban");
        }
    }

    function copyToClipboard(text) {
        navigator.clipboard.writeText(text).then(() => {
            showToast("Universal link copied!");
        });
    }

    // --- CHECK URL FOR SHARED DATA ON LOAD ---
    function checkIncomingShare() {
        const urlParams = new URLSearchParams(window.location.search);
        const sharedData = urlParams.get('data');

        if (sharedData) {
            try {
                const decodedData = JSON.parse(decodeURIComponent(escape(atob(sharedData))));
                
                // Deduplicate on import
                if(!scripts.some(s => s.id === decodedData.id)) {
                    scripts.unshift({
                        id: decodedData.id,
                        title: decodedData.t,
                        desc: decodedData.d,
                        code: decodedData.c,
                        author: decodedData.a,
                        authorPfp: decodedData.p,
                        userReactions: {},
                        userRatings: {},
                        comments: []
                    });
                    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
                    showToast("Script imported!");
                }
                
                window.history.replaceState({}, document.title, window.location.pathname);
            } catch (e) {
                console.error("Failed to decode shared script");
            }
        }
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
        
        let reportHtml = `<h4 style="font-size:15px; margin-bottom:5px;">📋 Reported Comments</h4>`;
        if(reportedComments.length === 0) {
            reportHtml += "<p style='font-family:sans-serif; color:#aaa; font-size:13px; margin-bottom:20px;'>No reports yet.</p>";
        } else {
            reportHtml += `
                <table class="admin-table">
                    <tr><th>User</th><th>Reason</th><th>Action</th></tr>
                    ${reportedComments.map((r, index) => `
                        <tr>
                            <td>${escapeHTML(r.reportedUser)}</td>
                            <td>
                                <b>${escapeHTML(r.reason)}</b>
                                <div class="report-comment-preview">"${escapeHTML(r.commentText)}"</div>
                            </td>
                            <td>
                                <button class="btn btn-reaction" style="background:#dc3545; color:white; font-size:10px; padding:3px;" onclick="openBanModalFromReport(${index})">Ban</button>
                                <button class="btn btn-reaction" style="background:#555; color:white; font-size:10px; padding:3px;" onclick="dismissReport(${index})">X</button>
                            </td>
                        </tr>
                    `).join('')}
                </table>
            `;
        }

        let banHtml = `<h4 style="margin-top:20px; font-size:15px;">🚫 Banned Users</h4>`;
        if(bannedUsers.length === 0) {
            banHtml += "<p style='font-family:sans-serif; color:#aaa; font-size:13px;'>No users banned.</p>";
        } else {
            banHtml += `
                <table class="admin-table">
                    <tr><th>Username</th><th>Action</th></tr>
                    ${bannedUsers.map((user) => `
                        <tr>
                            <td>${escapeHTML(user)}</td>
                            <td>
                                <button class="btn btn-reaction" style="background:#28a745; color:white; font-size:10px; padding:3px;" onclick="unbanUser('${user}')">Unban</button>
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
                <button class="btn-add-folder" onclick="openFolderModal()" title="Create Folder">+</button>
                <div class="pfp-container">
                    <img src="${currentUser.pfp}" class="pfp-header" onclick="toggleProfilePanel(event)">
                    ${currentUser.username === "Tasin Redwan" && bannedCount > 0 ? `<div class="ban-counter">${bannedCount}</div>` : ''}
                </div>
            `;
            createCard.style.display = 'none';
            
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

    // --- INITIALIZATION ---
    window.onload = () => {
        checkIncomingShare(); // Check for shared data via URL
        updateLoginUI();
        renderScripts();
    };
</script>

</body>
</html>
