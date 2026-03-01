<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lua Script Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css" integrity="sha512-z3gLpd7yknf1YoNbCzqRKc4qyor8gaKU1qmn+CShxbuBusANI9QpRohGBreCFkKxLhei6S9CQXFEbbKuqLosA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
    
    <style>
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism-okaidia.min.css');
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.css');
    </style>
    
    <style>
        /* ===== GLOBAL FONT OVERRIDE ===== */
        /* Apply Fredoka One broadly but NOT to icon fonts or code */
        body, button, input, textarea, select, 
        h1, h2, h3, h4, h5, h6, p, span, div, a, label, td, th {
            font-family: 'Fredoka One', cursive;
        }
        /* Preserve icon fonts */
        .fa, .fas, .far, .fab, .fal, .fad, [class^="fa-"], [class*=" fa-"] {
            font-family: "Font Awesome 6 Free", "Font Awesome 6 Brands" !important;
        }
        /* Preserve monospace for code */
        code, pre, .CodeMirror, .CodeMirror * {
            font-family: 'Fira Code', 'Consolas', monospace !important;
        }

        body { 
            font-family: 'Fredoka One', cursive; 
            background-color: #0d0d0d; 
            color: #f0f0f0; 
            padding: 12px; 
            margin: 0; 
            font-size: 16px;
        }
        .container { max-width: 800px; margin: auto; }
        
        /* ===== HEADER ===== */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
            background: #1a1a2e;
            padding: 12px 16px;
            border-radius: 14px;
            border: 2px solid #2a2a4a;
            position: relative;
            box-shadow: 0 4px 15px rgba(0,123,255,0.15);
        }
        .logo { font-size: 30px; font-weight: bold; color: #f0f0f0; letter-spacing: 1px; }
        .logo-hub { color: #3d9bff; text-shadow: 0 0 10px rgba(61,155,255,0.5); }
        .header-actions { display: flex; align-items: center; gap: 12px; }
        
        /* ===== PROFILE PFP ===== */
        .pfp-container { position: relative; }
        .pfp-header { 
            width: 48px; height: 48px; 
            border-radius: 50%; 
            object-fit: cover; 
            border: 3px solid #3d9bff; 
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 0 8px rgba(61,155,255,0.4);
        }
        .pfp-header:hover { transform: scale(1.08); box-shadow: 0 0 14px rgba(61,155,255,0.7); }
        .ban-counter {
            position: absolute; top: -6px; right: -6px;
            background: #e03545; color: white; border-radius: 50%;
            width: 22px; height: 22px; font-size: 12px;
            display: flex; justify-content: center; align-items: center;
            border: 2px solid #0d0d0d; font-weight: bold;
        }

        /* ===== BUTTONS ===== */
        .btn { 
            padding: 11px 18px; border: none; border-radius: 10px; 
            cursor: pointer; font-family: 'Fredoka One', cursive; 
            font-size: 16px; transition: all 0.2s;
            -webkit-tap-highlight-color: transparent;
        }
        .btn-login { background: #3d9bff; color: white; font-size: 16px; box-shadow: 0 3px 10px rgba(61,155,255,0.4); }
        .btn-logout { background: #444; color: #f0f0f0; }
        .btn-admin { background: #c0303f; color: white; font-size: 17px; padding: 9px 13px; border: 2px solid #e05060; border-radius: 50%; box-shadow: 0 3px 10px rgba(220,53,69,0.4);}
        .btn:hover { opacity: 0.88; transform: translateY(-1px); }
        .btn:active { transform: translateY(0); }

        /* ===== USER PROFILE PANEL ===== */
        .profile-panel {
            display: none;
            position: absolute;
            top: calc(100% + 12px);
            right: 0;
            background: #1a1a2e;
            border: 2px solid #3d9bff;
            border-radius: 14px;
            padding: 22px 18px;
            width: 270px;
            z-index: 1000;
            box-shadow: 0 8px 25px rgba(0,0,0,0.7);
            text-align: center;
        }
        .profile-panel.show { display: block; }
        
        .panel-pfp { width: 88px; height: 88px; border-radius: 50%; border: 4px solid #3d9bff; margin-bottom: 10px; box-shadow: 0 0 15px rgba(61,155,255,0.4); }
        
        .panel-username { 
            font-size: 20px; margin-bottom: 6px; cursor: pointer; 
            display: inline-block; padding: 3px 10px; border-radius: 6px;
            color: #ffffff;
        }
        .panel-username:hover { background: #2a2a4a; }
        
        .owner-tag { color: #ffd700; font-size: 13px; display: block; margin-bottom: 6px; font-weight: bold; text-shadow: 0 0 8px rgba(255,215,0,0.5); }
        .warning-tag {
            color: #ff6b6b; font-size: 13px;
            display: inline-block; margin-bottom: 12px; background: rgba(220,53,69,0.25);
            padding: 3px 8px; border-radius: 6px; cursor: help; border: 1px solid rgba(220,53,69,0.4);
        }
        .beloved-title { font-size: 13px; color: #bbb; margin-top: 16px; }
        .beloved-script { 
            background: #252540; 
            padding: 11px; 
            border-radius: 10px; 
            margin-top: 6px; 
            font-size: 15px; 
            border: 1px solid #3a3a5a;
            color: #e0e0e0;
        }
        .panel-logout { width: 100%; margin-top: 18px; background: #dc3545; font-size: 16px; }

        /* ===== SEARCH BAR ===== */
        .search-container {
            display: flex;
            gap: 10px;
            margin-bottom: 16px;
            background: #1a1a2e;
            padding: 10px;
            border-radius: 14px;
            border: 2px solid #2a2a4a;
            align-items: center;
            flex-wrap: wrap;
        }
        .search-input { 
            flex-grow: 1; 
            flex-basis: 150px;
            padding: 13px 14px;
            background: #252540; 
            border: 2px solid #3a3a6a; 
            color: #f0f0f0; 
            border-radius: 10px;
            font-family: 'Fredoka One', cursive;
            font-size: 16px;
        }
        .search-input::placeholder { color: #888; }
        .search-input:focus { outline: none; border-color: #3d9bff; }
        
        /* ===== CUSTOM DROPDOWN ===== */
        .custom-select-wrapper {
            position: relative;
            user-select: none;
            min-width: 130px;
            flex-shrink: 0;
        }
        .custom-select {
            background: #252540;
            border: 2px solid #3a3a6a;
            color: #f0f0f0;
            padding: 13px 14px;
            border-radius: 10px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-family: 'Fredoka One', cursive;
            font-size: 16px;
        }
        .custom-select:hover { border-color: #3d9bff; }
        .custom-select-options {
            display: none;
            position: absolute;
            top: calc(100% + 6px);
            left: 0;
            right: 0;
            background: #252540;
            border: 2px solid #3a3a6a;
            border-radius: 10px;
            z-index: 10;
            overflow: hidden;
            box-shadow: 0 6px 15px rgba(0,0,0,0.6);
        }
        .custom-select-options.open { display: block; }
        .custom-select-option {
            padding: 13px 14px;
            cursor: pointer;
            font-family: 'Fredoka One', cursive;
            font-size: 16px;
            transition: background 0.2s;
            color: #f0f0f0;
        }
        .custom-select-option:hover { background: #33335a; }
        .custom-select-option.selected { background: #3d9bff; color: white; }

        /* ===== CARDS ===== */
        .card { 
            background: #1a1a2e; 
            border: 2px solid #2a2a4a; 
            border-radius: 14px; 
            padding: 20px; 
            margin-bottom: 18px;
            transition: all 0.3s ease;
            position: relative;
        }
        .card:hover { border-color: #3d9bff; box-shadow: 0 4px 15px rgba(61,155,255,0.1); }
        h2 { font-size: 22px; margin-top: 0; margin-bottom: 8px; color: #f0f0f0; }
        h3 { font-size: 20px; margin-top: 0; margin-bottom: 6px; color: #f0f0f0; }
        p.desc { color: #c0c0c0; font-size: 15px; margin-bottom: 15px; }
        
        * { box-sizing: border-box; }

        /* ===== CODE BLOCKS - Dark bg + Lua syntax glow ===== */
        pre[class*="language-"] {
            background: #0a0a14 !important;
            border-radius: 10px;
            overflow-x: auto;
            max-height: 420px;
            margin-top: 12px;
            white-space: pre;
            border: 1px solid #1e1e3a;
            box-shadow: 0 0 20px rgba(61,155,255,0.08), inset 0 0 30px rgba(0,0,0,0.4);
            padding: 16px !important;
        }
        code[class*="language-"] {
            font-family: 'Fira Code', monospace !important;
            font-size: 14px;
            background: transparent !important;
        }
        /* Lua keyword glow */
        .token.keyword { color: #c792ea !important; text-shadow: 0 0 8px rgba(199,146,234,0.6) !important; font-style: italic; }
        .token.string { color: #c3e88d !important; text-shadow: 0 0 6px rgba(195,232,141,0.5) !important; }
        .token.number { color: #f78c6c !important; text-shadow: 0 0 6px rgba(247,140,108,0.5) !important; }
        .token.comment { color: #546e7a !important; font-style: italic; }
        .token.function { color: #82aaff !important; text-shadow: 0 0 8px rgba(130,170,255,0.5) !important; }
        .token.boolean { color: #ff5370 !important; text-shadow: 0 0 6px rgba(255,83,112,0.5) !important; }
        .token.operator { color: #89ddff !important; }
        .token.punctuation { color: #89ddff !important; }
        .token.builtin { color: #ffcb6b !important; text-shadow: 0 0 6px rgba(255,203,107,0.4) !important; }
        /* Line numbers */
        .line-numbers .line-numbers-rows { border-right: 1px solid #2a2a4a !important; }
        .line-numbers-rows > span:before { color: #444 !important; }
        
        /* ===== INPUTS & TEXTAREAS ===== */
        input, textarea { 
            width: 100%; padding: 13px 14px; margin: 8px 0; 
            background: #252540; 
            border: 2px solid #3a3a6a; 
            color: #f0f0f0; 
            border-radius: 10px; 
            font-family: 'Fredoka One', cursive;
            font-size: 16px;
        }
        input::placeholder, textarea::placeholder { color: #888; }
        input:focus, textarea:focus { outline: none; border-color: #3d9bff; }

        /* ===== CARD MENU (Three Dots) ===== */
        .card-options {
            position: absolute;
            top: 18px;
            right: 18px;
            cursor: pointer;
            color: #aaa;
            padding: 6px;
            font-size: 20px;
            transition: color 0.2s;
            z-index: 5;
        }
        .card-options:hover { color: #f0f0f0; }
        
        .dropdown-menu {
            display: none;
            position: absolute;
            top: 100%;
            right: 0;
            background: #252540;
            border-radius: 10px;
            padding: 6px;
            z-index: 100;
            min-width: 160px;
            box-shadow: 0 6px 15px rgba(0,0,0,0.7);
            border: 1px solid #3a3a6a;
        }
        .dropdown-menu button {
            display: flex;
            align-items: center;
            gap: 8px;
            width: 100%;
            background: none;
            border: none;
            color: #f0f0f0;
            text-align: left;
            padding: 12px 10px;
            cursor: pointer;
            font-family: 'Fredoka One', cursive;
            font-size: 15px;
            border-radius: 6px;
        }
        .dropdown-menu button:hover { background: #33335a; }
        .dropdown-menu button.danger { color: #ff6b6b; }

        .btn-post { background: #3d9bff; color: white; width: 100%; font-size: 17px; padding: 13px; box-shadow: 0 3px 12px rgba(61,155,255,0.35); }
        
        /* ===== REACTION BUTTONS ===== */
        .btn-reaction { 
            background: #252540; 
            color: #ccc; 
            padding: 8px 14px; 
            font-size: 14px; 
            font-family: 'Fredoka One', cursive;
            border: 1px solid #3a3a6a;
        }
        .btn-reaction.active-like { background: #28a745; color: white; border-color: #28a745; }
        .btn-reaction.active-dislike { background: #dc3545; color: white; border-color: #dc3545; }
        
        /* ===== RATING SLIDER ===== */
        .rating-container {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 15px;
            color: #ccc;
            background: #252540;
            padding: 8px 14px;
            border-radius: 10px;
            border: 1px solid #3a3a6a;
            flex-wrap: wrap;
        }
        .rating-slider {
            -webkit-appearance: none;
            flex: 1;
            min-width: 100px;
            height: 10px;
            background: #3a3a6a;
            border-radius: 5px;
            outline: none;
        }
        .rating-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 22px;
            height: 22px;
            background: #3d9bff;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 6px rgba(61,155,255,0.5);
        }

        /* ===== COMMENTS ===== */
        .comment-section { margin-top: 18px; border-top: 1px solid #2a2a4a; padding-top: 15px; }
        .comment-input {
            width: 100%; padding: 12px;
            background: #252540; border: 2px solid #3a3a6a;
            border-radius: 10px; color: #f0f0f0;
            font-size: 15px;
        }
        .comment-item {
            background: #252540; padding: 12px; border-radius: 10px;
            margin-top: 10px; font-size: 15px; 
            position: relative; border: 1px solid #3a3a6a;
        }
        .reply-item { margin-left: 20px; border-left: 3px solid #3d9bff; }
        
        .author-tag { 
            color: #3d9bff; font-weight: bold; margin-bottom: 5px; 
            display: flex; align-items: center; gap: 6px;
            font-family: 'Fredoka One', cursive;
            font-size: 15px;
        }
        .edited-tag { color: #999; font-size: 11px; font-weight: normal; }
        
        .pfp-tiny { width: 18px; height: 18px; border-radius: 50%; object-fit: cover; }
        .comment-text { word-wrap: break-word; white-space: pre-wrap; padding-right: 28px; font-size: 15px; color: #e8e8e8; }
        .comment-actions { display: flex; gap: 6px; margin-top: 6px; flex-wrap: wrap; }
        .comment-options { position: absolute; top: 10px; right: 10px; cursor: pointer; color: #aaa; font-size: 16px; }
        .comment-options:hover { color: #f0f0f0; }
        .stats-text { font-size: 15px; color: #bbb; margin-top: 12px; display: flex; gap: 15px; flex-wrap: wrap; }
        .toggle-replies { background: none; border: none; color: #3d9bff; cursor: pointer; font-size: 13px; margin-top: 6px; }

        /* ===== MODAL ===== */
        .modal {
            display: none; position: fixed; top: 0; left: 0; 
            width: 100%; height: 100%; background-color: rgba(0,0,0,0.88);
            justify-content: center; align-items: flex-start; z-index: 2000;
            overflow-y: auto;
            padding: 20px 0;
        }
        .modal.show { display: flex; }
        
        .modal-content {
            background: #1a1a2e; padding: 28px 22px; border-radius: 14px;
            border: 2px solid #3d9bff; text-align: center;
            width: 92%; max-width: 420px;
            margin: auto;
        }
        .modal-buttons { display: flex; justify-content: center; gap: 10px; margin-top: 20px; flex-wrap: wrap; }
        .modal-buttons .btn { flex: 1; min-width: 100px; }
        
        .pfp-option { width: 54px; height: 54px; border-radius: 50%; cursor: pointer; border: 3px solid transparent; transition: border-color 0.2s; }
        .pfp-option.selected { border-color: #3d9bff; box-shadow: 0 0 8px rgba(61,155,255,0.5); }
        
        .auth-toggle { font-size: 15px; color: #bbb; margin-top: 16px; cursor: pointer; }
        .auth-toggle span { color: #3d9bff; text-decoration: underline; }

        /* ===== ADMIN PANEL ===== */
        .admin-modal-content { max-width: 95vw; text-align: left; border-color: #dc3545; overflow-x: auto; }
        .admin-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #2a2a4a; padding-bottom: 12px; margin-bottom: 16px; }
        .admin-table { width: 100%; border-collapse: separate; border-spacing: 0; margin-top: 15px; color: #f0f0f0; font-size: 14px; min-width: 500px; }
        .admin-table th, .admin-table td { border-bottom: 1px solid #2a2a4a; padding: 14px; text-align: left; }
        .admin-table th { background: #252540; color: #ccc; font-weight: bold; }
        .admin-table tr:hover { background: #1e1e38; }
        .report-comment-preview { background: #0d0d1a; padding: 10px; border-radius: 8px; font-size: 12px; color: #bbb; margin-top: 5px; border: 1px solid #2a2a4a; font-family: 'Fira Code', monospace;}
        
        /* ===== TOAST ===== */
        #toast {
            position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%);
            background: #28a745; color: white; padding: 14px 26px; border-radius: 10px;
            display: none; z-index: 3000; font-family: 'Fredoka One', cursive; 
            font-size: 16px; text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            max-width: 90vw;
        }
        #toast.show { display: block; }
        #toast.ban { background: #dc3545; }
        #toast.unban { background: #ffc107; color: #121212; }

        /* ===== MOBILE RESPONSIVE ===== */
        @media (max-width: 600px) {
            body { padding: 8px; }
            .logo { font-size: 24px; }
            .header { padding: 10px 12px; }
            .pfp-header { width: 42px; height: 42px; }
            .btn { font-size: 15px; padding: 10px 14px; }
            .profile-panel { width: calc(100vw - 32px); right: -60px; }
            .search-container { padding: 8px; gap: 8px; }
            .custom-select-wrapper { min-width: 110px; }
            .card { padding: 16px; }
            h2 { font-size: 20px; }
            h3 { font-size: 18px; }
            .stats-text { flex-direction: column; gap: 8px; }
            .admin-modal-content { padding: 16px 12px; }
            .modal-content { padding: 20px 16px; }
            pre[class*="language-"] { font-size: 12px; }
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
        
        <p style="margin-top:15px; color:#aaa;">Choose Profile Picture:</p>
        <div style="display:flex; justify-content:center; gap:10px; margin-bottom:15px;">
            <img src="https://api.dicebear.com/8.x/pixel-art/svg?seed=1" class="pfp-option selected" onclick="selectPfp(this, 'seed=1')">
            <img src="https://api.dicebear.com/8.x/pixel-art/svg?seed=2" class="pfp-option" onclick="selectPfp(this, 'seed=2')">
            <img src="https://api.dicebear.com/8.x/pixel-art/svg?seed=3" class="pfp-option" onclick="selectPfp(this, 'seed=3')">
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
        <p style="color:white;" id="deleteModalText">Are you sure you want to Delete this item?</p>
        
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
        <p style="color:#aaa;">Welcome, Administrator Tasin Redwan.</p>
        
        <div id="adminContentArea"></div>
        
    </div>
</div>

<div id="banModal" class="modal">
    <div class="modal-content" style="border-color:#dc3545;">
        <h3>🚫 Ban User?</h3>
        <p style="color:white;" id="banModalContext"></p>
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
                <span class="author-tag" style="font-size:13px; color:#3d9bff; display:flex; align-items:center; gap:5px; margin-bottom:10px;">
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
        const pfp = `https://api.dicebear.com/8.x/pixel-art/svg?${selectedPfpSeed}`;
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
