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
        * { box-sizing: border-box; }
        body {
            font-family: 'Fredoka One', cursive;
            background-color: #0d0d0d;
            color: #ffffff;
            padding: 5px;
            margin: 0;
            overflow-x: hidden;
        }
        .container { width: 100%; max-width: 520px; margin: auto; padding: 5px; }

        /* HEADER */
        .header {
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 15px;
            background: linear-gradient(135deg,#141420,#1a1a2e);
            padding: 12px 15px; border-radius: 14px;
            border: 1.5px solid #2a2a4a; position: relative;
            box-shadow: 0 4px 20px rgba(0,0,0,0.5);
        }
        .logo { font-family: 'Fredoka One', cursive; font-size: 22px; }
        .logo-lua { color: #e8e8ff; }
        .logo-hub { color: #5b8ef0; }
        .header-actions { display: flex; align-items: center; gap: 10px; }

        /* PFP */
        .pfp-container { position: relative; }
        .pfp-header { width: 40px; height: 40px; border-radius: 50%; object-fit: cover; border: 2.5px solid #5b8ef0; cursor: pointer; transition: transform 0.2s, box-shadow 0.2s; }
        .pfp-header:hover { transform: scale(1.07); box-shadow: 0 0 12px rgba(91,142,240,0.5); }
        .ban-counter { position: absolute; top: -5px; right: -5px; background: #e74c3c; color: white; border-radius: 50%; width: 19px; height: 19px; font-size: 10px; display: flex; justify-content: center; align-items: center; border: 2px solid #0d0d0d; font-family: sans-serif; font-weight: bold; }

        /* BUTTONS */
        .btn { padding: 9px 16px; border: none; border-radius: 9px; cursor: pointer; font-family: 'Fredoka One', cursive; font-size: 14px; transition: all 0.2s; }
        .btn:hover { opacity: 0.87; transform: translateY(-1px); }
        .btn-login { background: linear-gradient(135deg,#5b8ef0,#2563eb); color: white; }
        .btn-logout { background: #1e1e2e; color: #888; border: 1px solid #2a2a3e; }
        .btn-admin { background: #c0392b; color: white; font-size: 15px; padding: 8px 11px; border-radius: 50%; border: none; }
        .btn-post { background: linear-gradient(135deg,#5b8ef0,#2563eb); color: white; width: 100%; }
        .btn-green { background: linear-gradient(135deg,#27ae60,#1a7a42); color: white; }
        .btn-red { background: linear-gradient(135deg,#e74c3c,#c0392b); color: white; }
        .btn-add-folder { background: linear-gradient(135deg,#27ae60,#1e8449); color: white; border: none; border-radius: 50%; width: 36px; height: 36px; font-size: 20px; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.2s; box-shadow: 0 2px 10px rgba(39,174,96,0.35); }
        .btn-add-folder:hover { transform: scale(1.1); box-shadow: 0 4px 16px rgba(39,174,96,0.55); }

        /* PROFILE PANEL */
        .profile-panel { display: none; position: absolute; top: calc(100% + 10px); right: 0; background: linear-gradient(160deg,#161626,#1a1a30); border: 1.5px solid #30306a; border-radius: 14px; padding: 18px 15px; width: 230px; z-index: 1000; box-shadow: 0 8px 32px rgba(0,0,0,0.65); text-align: center; }
        .profile-panel.show { display: block; animation: fadeDown 0.18s ease; }
        @keyframes fadeDown { from{opacity:0;transform:translateY(-6px)} to{opacity:1;transform:translateY(0)} }
        @keyframes popIn { from{opacity:0;transform:scale(0.95)} to{opacity:1;transform:scale(1)} }
        .panel-pfp { width: 72px; height: 72px; border-radius: 50%; border: 3px solid #5b8ef0; margin-bottom: 10px; }
        .panel-username { font-size: 16px; margin-bottom: 5px; cursor: pointer; display: inline-block; padding: 3px 10px; border-radius: 6px; transition: background 0.2s; }
        .panel-username:hover { background: #22223a; }
        .owner-tag { color: #f1c40f; font-size: 12px; font-family: sans-serif; display: block; margin-bottom: 5px; font-weight: bold; }
        .warning-tag { color: #e74c3c; font-size: 11px; font-family: sans-serif; display: inline-block; margin-bottom: 10px; background: rgba(231,76,60,0.12); padding: 2px 7px; border-radius: 5px; cursor: help; border: 1px solid rgba(231,76,60,0.25); }
        .beloved-title { font-size: 10px; color: #555; margin-top: 10px; letter-spacing: 1px; text-transform: uppercase; }
        .beloved-script { background: #1a1a2c; padding: 8px; border-radius: 8px; margin-top: 5px; font-size: 12px; border: 1px solid #252540; color: #999; font-family: sans-serif; }
        .panel-logout { width: 100%; margin-top: 14px; background: linear-gradient(135deg,#e74c3c,#c0392b); color: white; }

        /* SEARCH */
        .search-container { display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 18px; background: #141420; padding: 10px; border-radius: 13px; border: 1.5px solid #252540; align-items: center; }
        .search-input { flex-grow: 1; padding: 10px; min-width: 140px; background: #1a1a2c; border: 1.5px solid #2a2a44; color: #fff; border-radius: 9px; font-family: 'Fredoka One', cursive; font-size: 14px; outline: none; transition: border 0.2s; }
        .search-input:focus { border-color: #5b8ef0; }

        /* DROPDOWN */
        .custom-select-wrapper { position: relative; user-select: none; min-width: 100px; }
        .custom-select { background: #1a1a2c; border: 1.5px solid #2a2a44; color: #fff; padding: 10px; border-radius: 9px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; font-family: 'Fredoka One', cursive; font-size: 13px; }
        .custom-select:hover { border-color: #5b8ef0; }
        .custom-select-options { display: none; position: absolute; top: calc(100% + 5px); left: 0; right: 0; background: #1a1a2c; border: 1.5px solid #2a2a44; border-radius: 9px; z-index: 20; overflow: hidden; box-shadow: 0 6px 18px rgba(0,0,0,0.55); }
        .custom-select-options.open { display: block; }
        .custom-select-option { padding: 10px; cursor: pointer; font-family: 'Fredoka One', cursive; font-size: 13px; transition: background 0.15s; }
        .custom-select-option:hover { background: #22223a; }
        .custom-select-option.selected { background: #5b8ef0; color: white; }

        /* LEGACY CARD */
        .card { background: #141420; border: 1.5px solid #22223a; border-radius: 14px; padding: 16px; margin-bottom: 20px; position: relative; transition: border 0.25s; }
        .card:hover { border-color: #32326a; }
        h3 { font-size: 18px; margin: 0 0 5px; }
        p.desc { color: #777; font-family: sans-serif; margin-bottom: 14px; font-size: 13px; }

        /* CODE */
        pre[class*="language-"] { border-radius: 9px; overflow-x: auto; max-height: 380px; margin-top: 10px; white-space: pre; background: #080812 !important; border: 1px solid #1a1a30; width: 100%; }
        code[class*="language-"] { font-family: 'Fira Code', monospace; font-size: 12px; }
        input, textarea { width: 100%; padding: 10px; margin: 8px 0; background: #1a1a2c; border: 1.5px solid #2a2a44; color: #fff; border-radius: 9px; font-family: 'Fredoka One', cursive; outline: none; transition: border 0.2s; }
        input:focus, textarea:focus { border-color: #5b8ef0; }

        /* THREE DOT MENU */
        .card-options { position: absolute; top: 14px; right: 14px; cursor: pointer; color: #444; padding: 5px; font-size: 15px; transition: color 0.2s; z-index: 5; }
        .card-options:hover { color: #aaa; }
        .dropdown-menu { display: none; position: absolute; top: 100%; right: 0; background: #1a1a2c; border-radius: 11px; padding: 5px; z-index: 200; min-width: 150px; box-shadow: 0 8px 26px rgba(0,0,0,0.65); border: 1px solid #22223a; }
        .dropdown-menu.show { display: block; animation: fadeDown 0.14s ease; }
        .dropdown-menu button { display: flex; align-items: center; gap: 9px; width: 100%; background: none; border: none; color: #bbb; text-align: left; padding: 9px 11px; cursor: pointer; font-family: 'Fredoka One', cursive; font-size: 13px; border-radius: 7px; transition: background 0.14s; }
        .dropdown-menu button:hover { background: #22223a; color: white; }
        .dropdown-menu button.danger { color: #e74c3c; }
        .dropdown-menu button.danger:hover { background: rgba(231,76,60,0.13); }

        /* REACTIONS */
        .btn-reaction { background: #1a1a2c; color: #666; padding: 5px 9px; font-size: 11px; font-family: 'Fredoka One', cursive; border: 1px solid #222240; border-radius: 7px; cursor: pointer; transition: all 0.16s; }
        .btn-reaction:hover { background: #22223a; color: #bbb; }
        .btn-reaction.active-like { background: rgba(39,174,96,0.18); color: #2ecc71; border-color: rgba(39,174,96,0.4); }
        .btn-reaction.active-dislike { background: rgba(231,76,60,0.18); color: #e74c3c; border-color: rgba(231,76,60,0.4); }

        /* RATING */
        .rating-container { display: flex; align-items: center; gap: 8px; font-family: sans-serif; font-size: 12px; color: #555; background: #141424; padding: 5px 10px; border-radius: 8px; flex-grow: 1; flex-wrap: wrap; border: 1px solid #1e1e34; }
        .rating-slider { -webkit-appearance: none; width: 90px; height: 5px; background: #2a2a44; border-radius: 4px; outline: none; }
        .rating-slider::-webkit-slider-thumb { -webkit-appearance: none; width: 14px; height: 14px; background: #5b8ef0; border-radius: 50%; cursor: pointer; }

        /* COMMENTS */
        .comment-section { margin-top: 14px; border-top: 1px solid #1e1e30; padding-top: 10px; }
        .comment-input { width: 100%; padding: 8px 12px; background: #141424; border: 1px solid #1e1e34; border-radius: 8px; color: white; font-family: sans-serif; font-size: 13px; }
        .comment-input:focus { border-color: #5b8ef0; outline: none; }
        .comment-item { background: #141424; padding: 9px 10px; border-radius: 9px; margin-top: 8px; font-family: sans-serif; font-size: 13px; position: relative; border: 1px solid #1e1e34; }
        .reply-item { margin-left: 14px; border-left: 2px solid #22224a; }
        .author-tag { color: #5b8ef0; font-weight: bold; margin-bottom: 4px; display: flex; align-items: center; gap: 5px; font-family: 'Fredoka One', cursive; font-size: 12px; }
        .edited-tag { color: #444; font-size: 9px; font-weight: normal; font-family: sans-serif; }
        .pfp-tiny { width: 14px; height: 14px; border-radius: 50%; object-fit: cover; }
        .comment-text { word-wrap: break-word; white-space: pre-wrap; padding-right: 22px; font-family: sans-serif; color: #ccc; font-size: 13px; }
        .comment-actions { display: flex; gap: 5px; margin-top: 6px; }
        .comment-options { position: absolute; top: 8px; right: 8px; cursor: pointer; color: #444; font-size: 14px; }
        .comment-options:hover { color: #aaa; }
        .stats-text { font-family: sans-serif; font-size: 11px; color: #444; margin-top: 10px; display: flex; gap: 12px; }
        .toggle-replies { background: none; border: none; color: #5b8ef0; cursor: pointer; font-size: 11px; margin-top: 3px; font-family: sans-serif; }

        /* MODAL */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); justify-content: center; align-items: center; z-index: 2000; overflow-y: auto; padding: 12px; }
        .modal.show { display: flex; }
        .modal-content { background: linear-gradient(160deg,#141424,#1a1a2e); padding: 22px; border-radius: 16px; border: 1.5px solid #5b8ef0; text-align: center; width: 100%; max-width: 370px; box-shadow: 0 14px 45px rgba(0,0,0,0.75); animation: popIn 0.18s ease; }
        .modal-content.danger-border { border-color: #e74c3c; }
        .modal-content.green-border { border-color: #27ae60; }
        .modal-content.wide { max-width: 480px; }
        .modal-buttons { display: flex; justify-content: center; gap: 10px; margin-top: 16px; flex-wrap: wrap; }
        .modal-title { font-size: 18px; margin: 0 0 12px; }
        .field-label { font-size: 11px; color: #555; font-family: sans-serif; text-align: left; display: block; margin: 6px 0 -4px; }

        /* AUTH */
        .pfp-option { width: 40px; height: 40px; border-radius: 50%; cursor: pointer; border: 3px solid transparent; transition: border 0.2s, transform 0.2s; }
        .pfp-option.selected { border-color: #5b8ef0; transform: scale(1.1); }
        .auth-toggle { font-family: sans-serif; font-size: 13px; color: #555; margin-top: 12px; cursor: pointer; }
        .auth-toggle span { color: #5b8ef0; text-decoration: underline; }

        /* ADMIN */
        .admin-modal-content { max-width: 520px; text-align: left; border-color: #e74c3c; }
        .admin-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1e1e2e; padding-bottom: 12px; margin-bottom: 14px; }
        .admin-tabs { display: flex; gap: 8px; margin-bottom: 14px; }
        .admin-tab { flex: 1; background: #1a1a2c; border: 1px solid #22223a; color: #777; padding: 8px; border-radius: 9px; cursor: pointer; font-family: 'Fredoka One', cursive; font-size: 13px; transition: all 0.18s; }
        .admin-tab.active { background: #5b8ef0; color: white; border-color: #5b8ef0; }
        .admin-table { width: 100%; border-collapse: collapse; font-family: sans-serif; color: #bbb; font-size: 12px; }
        .admin-table th, .admin-table td { border-bottom: 1px solid #1a1a2c; padding: 9px 5px; text-align: left; }
        .admin-table th { background: #141424; color: #555; }
        .admin-table tr:hover td { background: #1a1a2c; }
        .report-preview { background: #0a0a16; padding: 4px 7px; border-radius: 5px; font-size: 10px; color: #555; margin-top: 3px; border: 1px solid #1a1a2c; font-family: 'Fira Code', monospace; word-break: break-all; }
        .report-badge { display: inline-block; font-size: 10px; padding: 1px 6px; border-radius: 4px; font-family: 'Fredoka One', cursive; }
        .badge-folder-report { background: rgba(91,142,240,0.15); color: #5b8ef0; border: 1px solid rgba(91,142,240,0.25); }
        .badge-script-report { background: rgba(155,89,182,0.15); color: #9b59b6; border: 1px solid rgba(155,89,182,0.25); }
        .badge-comment-report { background: rgba(231,126,34,0.15); color: #e67e22; border: 1px solid rgba(231,126,34,0.25); }

        /* TOAST */
        #toast { position: fixed; bottom: 22px; left: 50%; transform: translateX(-50%); background: #27ae60; color: white; padding: 11px 22px; border-radius: 10px; display: none; z-index: 3000; font-family: 'Fredoka One', cursive; font-size: 14px; box-shadow: 0 4px 18px rgba(0,0,0,0.45); white-space: nowrap; }
        #toast.show { display: block; animation: fadeDown 0.18s ease; }
        #toast.ban { background: #e74c3c; }
        #toast.unban { background: #f39c12; color: #111; }
        #toast.info { background: #5b8ef0; }

        /* FOLDER CARD */
        .folder-card { background: linear-gradient(160deg,#0e0e22,#111128); border: 1.5px solid #20205a; border-radius: 16px; padding: 16px; margin-bottom: 20px; position: relative; box-shadow: 0 4px 22px rgba(0,0,0,0.35); transition: border 0.25s; }
        .folder-card:hover { border-color: #30308a; }
        .folder-header { display: flex; align-items: center; gap: 7px; margin-bottom: 5px; }
        .folder-toggle-btn { background: none; border: none; cursor: pointer; color: #33336a; font-size: 13px; padding: 4px; transition: color 0.2s, transform 0.3s; display: flex; align-items: center; }
        .folder-toggle-btn:hover { color: #7777cc; }
        .folder-toggle-btn.open { transform: rotate(90deg); }
        .folder-title-text { font-size: 17px; font-family: 'Fredoka One', cursive; flex-grow: 1; color: #d0d0ff; }
        .folder-meta { font-family: sans-serif; font-size: 11px; color: #33336a; margin-bottom: 6px; display: flex; align-items: center; gap: 7px; flex-wrap: wrap; }
        .folder-desc-text { color: #44447a; font-family: sans-serif; font-size: 12px; margin-bottom: 8px; line-height: 1.5; }
        .folder-reactions { display: flex; align-items: center; gap: 8px; margin: 8px 0 4px; flex-wrap: wrap; }

        /* SMOOTH COLLAPSE */
        .folder-scripts-wrap { overflow: hidden; max-height: 0; transition: max-height 0.42s cubic-bezier(0.4,0,0.2,1), opacity 0.3s ease; opacity: 0; }
        .folder-scripts-wrap.open { max-height: 9999px; opacity: 1; }

        /* ADD SCRIPT AREA */
        .folder-add-script-area { display: flex; gap: 8px; margin-top: 12px; }
        .btn-script-type { flex: 1; background: #16163a; color: #6666cc; border: 1px solid #22225a; border-radius: 9px; padding: 8px 5px; font-family: 'Fredoka One', cursive; font-size: 13px; cursor: pointer; transition: all 0.18s; display: flex; align-items: center; justify-content: center; gap: 5px; }
        .btn-script-type:hover { background: #22225a; color: #aaaaff; border-color: #5b8ef0; }

        /* SCRIPT INSIDE FOLDER */
        .folder-script-card { background: #0a0a1e; border: 1px solid #18185a; border-radius: 11px; padding: 13px; margin-bottom: 10px; position: relative; }
        .script-type-badge { display: inline-block; font-size: 10px; font-family: 'Fira Code', monospace; padding: 2px 8px; border-radius: 5px; margin-bottom: 7px; font-weight: bold; }
        .badge-server { background: rgba(39,174,96,0.13); color: #2ecc71; border: 1px solid rgba(39,174,96,0.28); }
        .badge-local { background: rgba(231,76,60,0.13); color: #e74c3c; border: 1px solid rgba(231,76,60,0.28); }
        .folder-script-title { font-size: 15px; font-family: 'Fredoka One', cursive; margin-bottom: 3px; color: #ddd; }
        .folder-script-desc { color: #44446a; font-family: sans-serif; font-size: 12px; margin-bottom: 7px; }
        .folder-card-options { cursor: pointer; color: #33335a; font-size: 14px; padding: 3px; }
        .folder-card-options:hover { color: #aaa; }

        /* SCRIPT TYPE CHOOSER */
        .script-type-chooser { display: flex; gap: 10px; margin: 10px 0; }
        .script-type-btn-big { flex: 1; background: #16163a; color: #6666cc; border: 2px solid #22225a; border-radius: 11px; padding: 14px 8px; font-family: 'Fredoka One', cursive; font-size: 15px; cursor: pointer; transition: all 0.2s; text-align: center; }
        .script-type-btn-big:hover { background: #22225a; color: #aaaaff; border-color: #5b8ef0; }
        .script-type-label { display: block; font-size: 10px; font-family: 'Fira Code', monospace; margin-top: 4px; opacity: 0.55; }

        /* CODEMIRROR in modals */
        .CodeMirror { border-radius: 9px; font-family: 'Fira Code', monospace; font-size: 12px; min-height: 130px; }
    </style>
</head>
<body>

<div id="toast"></div>

<div class="container">
    <div class="header">
        <div class="logo"><span class="logo-lua">Lua</span><span class="logo-hub">Hub</span></div>
        <div class="header-actions" id="authAreaContainer"></div>
        <div class="profile-panel" id="profilePanel">
            <img src="" id="panelPfp" class="panel-pfp">
            <div class="panel-username" id="panelUsername" onclick="openChangeUsernameModal()" title="Click to rename"></div>
            <span class="owner-tag" id="panelTag" style="display:none;">👑 OWNER</span>
            <span class="warning-tag" id="panelWarning" style="display:none;" title="Reaches 10 = permanent ban">⚠ Warning Level : 0</span>
            <div class="beloved-title">🏆 YOUR BELOVED SCRIPT</div>
            <div class="beloved-script" id="panelBeloved">No scripts yet!</div>
            <button class="btn panel-logout" onclick="logout()">Logout</button>
        </div>
    </div>

    <div class="search-container">
        <input type="text" id="searchInput" class="search-input" placeholder="🔍 Search..." onkeyup="renderScripts()">
        <div class="custom-select-wrapper">
            <div class="custom-select" id="searchTypeDisplay">
                <span id="selectedTypeText">Name</span>
                <i class="fas fa-chevron-down" style="font-size:10px; margin-left:5px;"></i>
            </div>
            <div class="custom-select-options" id="searchTypeOptions">
                <div class="custom-select-option selected" data-value="name">Name</div>
                <div class="custom-select-option" data-value="creator">Creator</div>
            </div>
        </div>
        <input type="hidden" id="searchType" value="name">
    </div>

    <div class="card" id="createCard" style="display:none;">
        <h2 style="font-size:18px;">✍️ Create Script</h2>
        <input type="text" id="scriptTitle" placeholder="Script Title">
        <textarea id="scriptDesc" rows="2" placeholder="Description"></textarea>
        <textarea id="scriptCode"></textarea>
        <button class="btn btn-post" onclick="saveScript()">Post Script</button>
    </div>

    <div id="scriptFeed"></div>
</div>

<!-- AUTH -->
<div id="authModal" class="modal">
    <div class="modal-content">
        <h3 class="modal-title" id="authTitle">Login</h3>
        <input type="text" id="authUsername" placeholder="Username">
        <input type="password" id="authPassword" placeholder="Password">
        <p style="font-family:sans-serif; color:#555; font-size:12px; margin:8px 0 5px; text-align:left;">Choose Avatar:</p>
        <div style="display:flex; justify-content:center; gap:10px; margin-bottom:12px;">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=1" class="pfp-option selected" onclick="selectPfp(this,'seed=1')">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=2" class="pfp-option" onclick="selectPfp(this,'seed=2')">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=3" class="pfp-option" onclick="selectPfp(this,'seed=3')">
        </div>
        <div class="modal-buttons">
            <button class="btn btn-post" id="authSubmitBtn" onclick="processAuth()">Login</button>
            <button class="btn btn-logout" onclick="closeModal('authModal')">Cancel</button>
        </div>
        <div class="auth-toggle" id="authToggleText" onclick="toggleAuthType()">Don't have an account? <span>Sign Up</span></div>
    </div>
</div>

<!-- CHANGE USERNAME -->
<div id="changeUsernameModal" class="modal">
    <div class="modal-content">
        <h3 class="modal-title">✏️ Change Username</h3>
        <span class="field-label">New Username</span>
        <input type="text" id="newUsernameInput" placeholder="Enter new name">
        <div class="modal-buttons">
            <button class="btn btn-post" onclick="submitChangeUsername()">Save</button>
            <button class="btn btn-logout" onclick="closeModal('changeUsernameModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- COMMENT INPUT (edit / reply) -->
<div id="inputModal" class="modal">
    <div class="modal-content">
        <h3 class="modal-title" id="inputModalTitle">Edit</h3>
        <textarea id="inputModalText" rows="3" placeholder=""></textarea>
        <div class="modal-buttons">
            <button class="btn btn-post" id="inputModalSubmit">Submit</button>
            <button class="btn btn-logout" onclick="closeModal('inputModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- DELETE CONFIRM -->
<div id="deleteModal" class="modal">
    <div class="modal-content danger-border">
        <div style="font-size:32px; margin-bottom:8px;">🗑️</div>
        <h3 class="modal-title" id="deleteModalTitle">Delete?</h3>
        <p style="color:#aaa; font-family:sans-serif; font-size:13px; margin:0 0 5px;" id="deleteModalText">Are you sure?</p>
        <div class="modal-buttons">
            <button class="btn btn-red" id="deleteModalConfirm">Yes, Delete</button>
            <button class="btn btn-logout" onclick="closeModal('deleteModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- ADMIN -->
<div id="adminModal" class="modal">
    <div class="modal-content admin-modal-content wide">
        <div class="admin-header">
            <h3 style="font-size:18px; margin:0;">🛡️ Admin Dashboard</h3>
            <button class="btn btn-logout" style="padding:5px 12px;" onclick="closeModal('adminModal')">✕ Close</button>
        </div>
        <p style="color:#444; font-family:sans-serif; font-size:12px; margin:0 0 12px;">Welcome, Administrator Tasin Redwan.</p>
        <div class="admin-tabs">
            <button class="admin-tab active" id="tabReports" onclick="adminTab('reports')">📋 Reports</button>
            <button class="admin-tab" id="tabBans" onclick="adminTab('bans')">🚫 Banned</button>
        </div>
        <div id="adminContentArea"></div>
    </div>
</div>

<!-- BAN -->
<div id="banModal" class="modal">
    <div class="modal-content danger-border">
        <div style="font-size:30px; margin-bottom:8px;">🚫</div>
        <h3 class="modal-title">Ban User</h3>
        <p style="color:#aaa; font-family:sans-serif; font-size:13px; margin:0 0 8px;" id="banModalContext"></p>
        <span class="field-label">Duration (days)</span>
        <input type="number" id="banDays" placeholder="e.g. 7" min="1">
        <span class="field-label">Reason</span>
        <textarea id="banReason" rows="2" placeholder="State the reason..."></textarea>
        <div class="modal-buttons">
            <button class="btn btn-red" id="banModalConfirm">Ban User</button>
            <button class="btn btn-logout" onclick="closeModal('banModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- CREATE FOLDER -->
<div id="folderModal" class="modal">
    <div class="modal-content green-border" style="text-align:left;">
        <h3 class="modal-title" style="text-align:center;">📁 Create Folder</h3>
        <span class="field-label">Folder Name *</span>
        <input type="text" id="folderNameInput" placeholder="My Awesome Scripts">
        <span class="field-label">Description</span>
        <textarea id="folderDescInput" rows="2" placeholder="What's in this folder?"></textarea>
        <div class="modal-buttons">
            <button class="btn btn-green" onclick="submitCreateFolder()">Create Folder</button>
            <button class="btn btn-logout" onclick="closeModal('folderModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- EDIT FOLDER -->
<div id="editFolderModal" class="modal">
    <div class="modal-content" style="text-align:left;">
        <h3 class="modal-title" style="text-align:center;">✏️ Edit Folder</h3>
        <input type="hidden" id="editFolderId">
        <span class="field-label">Folder Name *</span>
        <input type="text" id="editFolderName" placeholder="Folder Name">
        <span class="field-label">Description</span>
        <textarea id="editFolderDesc" rows="2" placeholder="Description"></textarea>
        <div class="modal-buttons">
            <button class="btn btn-post" onclick="submitEditFolder()">Save Changes</button>
            <button class="btn btn-logout" onclick="closeModal('editFolderModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- EDIT FOLDER SCRIPT -->
<div id="editFolderScriptModal" class="modal">
    <div class="modal-content wide" style="text-align:left;">
        <h3 class="modal-title" style="text-align:center;">✏️ Edit Script</h3>
        <input type="hidden" id="editFsId">
        <input type="hidden" id="editFsFolderId">
        <div id="editFsBadgeArea" style="margin-bottom:8px;"></div>
        <span class="field-label">Title *</span>
        <input type="text" id="editFsTitle" placeholder="Script Title">
        <span class="field-label">Description</span>
        <textarea id="editFsDesc" rows="2" placeholder="Description"></textarea>
        <span class="field-label">Code *</span>
        <textarea id="editFsCode"></textarea>
        <div class="modal-buttons">
            <button class="btn btn-post" onclick="submitEditFolderScript()">Save Changes</button>
            <button class="btn btn-logout" onclick="closeEditFolderScriptModal()">Cancel</button>
        </div>
    </div>
</div>

<!-- ADD SCRIPT TO FOLDER -->
<div id="addScriptModal" class="modal">
    <div class="modal-content wide" style="text-align:left;">
        <h3 class="modal-title" id="addScriptModalTitle" style="text-align:center;">Add Script</h3>
        <div id="addScriptStep1">
            <p style="color:#555; font-family:sans-serif; font-size:13px; text-align:center; margin-bottom:8px;">Choose Script Type:</p>
            <div class="script-type-chooser">
                <button class="script-type-btn-big" onclick="chooseScriptType('ServerScript')">🖥️ ServerScript<span class="script-type-label">Runs on Server</span></button>
                <button class="script-type-btn-big" onclick="chooseScriptType('LocalScript')">💻 LocalScript<span class="script-type-label">Runs on Client</span></button>
            </div>
        </div>
        <div id="addScriptStep2" style="display:none;">
            <div id="chosenTypeBadgeArea" style="margin-bottom:8px;"></div>
            <span class="field-label">Title *</span>
            <input type="text" id="folderScriptTitle" placeholder="Script Title">
            <span class="field-label">Description</span>
            <textarea id="folderScriptDesc" rows="2" placeholder="Optional description"></textarea>
            <span class="field-label">Code *</span>
            <textarea id="folderScriptCode"></textarea>
            <div class="modal-buttons">
                <button class="btn btn-post" onclick="submitAddScriptToFolder()">Add Script</button>
                <button class="btn btn-logout" onclick="backToScriptTypeChoice()">← Back</button>
                <button class="btn btn-logout" onclick="closeAddScriptModal()">Cancel</button>
            </div>
        </div>
    </div>
</div>

<!-- REPORT -->
<div id="reportModal" class="modal">
    <div class="modal-content danger-border" style="text-align:left;">
        <div style="font-size:28px; text-align:center; margin-bottom:8px;">🚩</div>
        <h3 class="modal-title" id="reportModalTitle" style="text-align:center;">Report</h3>
        <p style="color:#666; font-family:sans-serif; font-size:12px; margin-bottom:8px; background:#0d0d18; padding:8px; border-radius:7px; border:1px solid #1a1a2c;" id="reportModalContext"></p>
        <span class="field-label">Reason *</span>
        <textarea id="reportReason" rows="3" placeholder="Describe the issue clearly..."></textarea>
        <div class="modal-buttons">
            <button class="btn btn-red" id="reportSubmitBtn">Submit Report</button>
            <button class="btn btn-logout" onclick="closeModal('reportModal')">Cancel</button>
        </div>
    </div>
</div>

<!-- SHARE -->
<div id="shareModal" class="modal">
    <div class="modal-content" style="text-align:left;">
        <h3 class="modal-title" style="text-align:center;">🔗 Share Script</h3>
        <p style="color:#555; font-family:sans-serif; font-size:12px; text-align:center; margin-bottom:10px;">This link works across all devices.</p>
        <div style="display:flex; gap:8px; align-items:center;">
            <input type="text" id="shareLinkInput" readonly style="flex:1; color:#5b8ef0; font-size:11px; font-family:'Fira Code',monospace; cursor:pointer; background:#0d0d1e;">
            <button class="btn btn-post" style="width:auto; padding:10px 14px; white-space:nowrap;" onclick="copyShareLink()">Copy</button>
        </div>
        <div class="modal-buttons">
            <button class="btn btn-logout" onclick="closeModal('shareModal')">Close</button>
        </div>
    </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/lua/lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.js"></script>

<script>
// ════════════════════════════════════
// DATA
// ════════════════════════════════════
let scripts = JSON.parse(localStorage.getItem('lua_hub_data')) || [];
let users = JSON.parse(localStorage.getItem('lua_hub_users')) || {};
let currentUser = JSON.parse(localStorage.getItem('lua_hub_session')) || null;
let reportedComments = JSON.parse(localStorage.getItem('lua_hub_reports')) || [];
let bannedUsers = JSON.parse(localStorage.getItem('lua_hub_banned')) || [];
let userWarningLevels = JSON.parse(localStorage.getItem('lua_hub_warnings')) || {};
let folders = JSON.parse(localStorage.getItem('lua_hub_folders')) || [];
let folderReports = JSON.parse(localStorage.getItem('lua_hub_folder_reports')) || [];
let openFolders = {};

if (currentUser && bannedUsers.includes(currentUser.username)) { currentUser = null; localStorage.removeItem('lua_hub_session'); }

let selectedPfpSeed = 'seed=1';
let authType = 'login';

// Migration
scripts.forEach(s => {
    if (!s.userReactions) s.userReactions = {};
    if (!s.userRatings) s.userRatings = {};
    if (!s.comments) s.comments = [];
    if (s.title && s.title.startsWith("[SHARED] ")) s.title = s.title.substring(9);
    s.comments.forEach(c => {
        if (!c.replies) c.replies = [];
        if (!c.userReactions) c.userReactions = {};
        if (c.edited === undefined) c.edited = false;
        c.replies.forEach(r => { if (r.edited === undefined) r.edited = false; });
    });
});
folders.forEach(f => {
    if (!f.userReactions) f.userReactions = {};
    if (!f.scripts) f.scripts = [];
    f.scripts.forEach(fs => {
        if (!fs.userReactions) fs.userReactions = {};
        if (!fs.userRatings) fs.userRatings = {};
    });
});

// Dedup
let seenIds = new Set();
scripts = scripts.filter(s => { if (seenIds.has(s.id)) return false; seenIds.add(s.id); return true; });
localStorage.setItem('lua_hub_data', JSON.stringify(scripts));

// ════════════════════════════════════
// SEARCH DROPDOWN
// ════════════════════════════════════
const dropdownDisplay = document.getElementById('searchTypeDisplay');
const dropdownOptions = document.getElementById('searchTypeOptions');
const hiddenInput = document.getElementById('searchType');
const selectedText = document.getElementById('selectedTypeText');

dropdownDisplay.addEventListener('click', e => { e.stopPropagation(); dropdownOptions.classList.toggle('open'); });
document.querySelectorAll('.custom-select-option').forEach(opt => {
    opt.addEventListener('click', e => {
        e.stopPropagation();
        hiddenInput.value = opt.dataset.value;
        selectedText.innerText = opt.innerText;
        dropdownOptions.classList.remove('open');
        document.querySelectorAll('.custom-select-option').forEach(o => o.classList.remove('selected'));
        opt.classList.add('selected');
        renderScripts();
    });
});
document.addEventListener('click', () => {
    dropdownOptions.classList.remove('open');
    document.getElementById('profilePanel').classList.remove('show');
    document.querySelectorAll('.dropdown-menu').forEach(m => m.classList.remove('show'));
});

// ════════════════════════════════════
// MODAL HELPERS
// ════════════════════════════════════
function closeModal(id) { document.getElementById(id).classList.remove('show'); }
function openModal(id) { document.getElementById(id).classList.add('show'); }

// ════════════════════════════════════
// PROFILE PANEL
// ════════════════════════════════════
function toggleProfilePanel(e) {
    e.stopPropagation();
    document.getElementById('profilePanel').classList.toggle('show');
    updatePanelContent();
}
function updatePanelContent() {
    if (!currentUser) return;
    document.getElementById('panelPfp').src = currentUser.pfp;
    document.getElementById('panelUsername').innerText = currentUser.username;
    const tag = document.getElementById('panelTag');
    const warnTag = document.getElementById('panelWarning');
    if (currentUser.username === "Tasin Redwan") {
        tag.style.display = 'block'; warnTag.style.display = 'none';
    } else {
        tag.style.display = 'none';
        let lvl = userWarningLevels[currentUser.username] || 0;
        if (lvl > 0) { warnTag.style.display = 'inline-block'; warnTag.innerText = `⚠ Warning Level : ${lvl}`; }
        else warnTag.style.display = 'none';
    }
    let all = scripts.filter(s => s.author === currentUser.username);
    folders.forEach(f => { if (f.scripts) f.scripts.filter(fs => fs.author === currentUser.username).forEach(fs => all.push({ ...fs })); });
    let beloved = null, maxScore = -1;
    all.forEach(s => {
        let score = Object.values(s.userReactions || {}).filter(r => r === 'like').length + Object.values(s.userRatings || {}).reduce((a, b) => a + b, 0);
        if (score > maxScore) { maxScore = score; beloved = s; }
    });
    document.getElementById('panelBeloved').innerText = beloved ? `${beloved.title} (${maxScore}pts)` : 'No scripts yet!';
}

function openChangeUsernameModal() {
    document.getElementById('newUsernameInput').value = currentUser ? currentUser.username : '';
    openModal('changeUsernameModal');
}
function submitChangeUsername() {
    const n = document.getElementById('newUsernameInput').value.trim();
    if (!n) return showToast("Name required", 'ban');
    if (n === currentUser.username) return closeModal('changeUsernameModal');
    if (users[n]) return showToast("Username taken!", 'ban');
    const old = currentUser.username;
    users[n] = { ...users[old], username: n };
    delete users[old];
    localStorage.setItem('lua_hub_users', JSON.stringify(users));
    currentUser.username = n;
    localStorage.setItem('lua_hub_session', JSON.stringify(currentUser));
    scripts.forEach(s => {
        if (s.author === old) s.author = n;
        s.comments.forEach(c => { if (c.author === old) c.author = n; c.replies.forEach(r => { if (r.author === old) r.author = n; }); });
    });
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
    folders.forEach(f => { if (f.author === old) f.author = n; if (f.scripts) f.scripts.forEach(fs => { if (fs.author === old) fs.author = n; }); });
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    if (userWarningLevels[old]) { userWarningLevels[n] = userWarningLevels[old]; delete userWarningLevels[old]; localStorage.setItem('lua_hub_warnings', JSON.stringify(userWarningLevels)); }
    closeModal('changeUsernameModal'); updatePanelContent(); renderScripts(); showToast("Username updated!");
}

// ════════════════════════════════════
// RENDER
// ════════════════════════════════════
function renderScripts() {
    const feed = document.getElementById('scriptFeed');
    const term = document.getElementById('searchInput').value.toLowerCase();
    const type = hiddenInput.value;
    feed.innerHTML = '';
    folders
        .filter(f => type === 'name' ? f.name.toLowerCase().includes(term) : (f.author || '').toLowerCase().includes(term))
        .forEach(f => feed.appendChild(renderFolderCard(f)));
    scripts
        .filter(s => type === 'name' ? s.title.toLowerCase().includes(term) : s.author.toLowerCase().includes(term))
        .forEach(s => feed.appendChild(buildScriptCard(s)));
    Prism.highlightAll();
}

// FOLDER CARD
function renderFolderCard(folder) {
    const el = document.createElement('div');
    el.className = 'folder-card';
    const isOwner = currentUser && currentUser.username === folder.author;
    const isAdmin = currentUser && currentUser.username === 'Tasin Redwan';
    const isOpen = !!openFolders[folder.id];
    let fLikes = Object.values(folder.userReactions || {}).filter(r => r === 'like').length;
    let fDislikes = Object.values(folder.userReactions || {}).filter(r => r === 'dislike').length;
    const myFR = currentUser ? (folder.userReactions || {})[currentUser.username] : null;
    const count = (folder.scripts || []).length;

    const scriptsInner = count > 0
        ? (folder.scripts || []).map(fs => buildFolderScriptCard(fs, folder.id, isOwner, isAdmin)).join('')
        : `<p style="color:#22224a; font-family:sans-serif; font-size:12px; padding:6px 0; margin:0;">No scripts yet${isOwner ? ' — add one below!' : '.'}</p>`;

    el.innerHTML = `
        <div class="folder-header">
            <button class="folder-toggle-btn${isOpen ? ' open' : ''}" onclick="toggleFolder('${folder.id}',this)" title="${isOpen?'Collapse':'Expand'}"><i class="fas fa-chevron-right"></i></button>
            <span style="font-size:19px;">📁</span>
            <span class="folder-title-text">${escapeHTML(folder.name)}</span>
            <div style="position:relative; margin-left:auto;">
                <button class="folder-card-options" onclick="toggleDropdown(event,'fold-${folder.id}')" style="display:inline-block;"><i class="fas fa-ellipsis-v"></i></button>
                <div class="dropdown-menu" id="dropdown-fold-${folder.id}" style="right:0;">
                    ${isOwner ? `
                        <button onclick="openEditFolderModal('${folder.id}')"><i class="fas fa-edit"></i> Edit Folder</button>
                        <button onclick="openDeleteFolderModal('${folder.id}')" class="danger"><i class="fas fa-trash"></i> Delete Folder</button>
                    ` : `<button onclick="openFolderReportModal('${folder.id}')" class="danger"><i class="fas fa-flag"></i> Report</button>`}
                    ${isAdmin && !isOwner ? `<button onclick="openBanModal('${folder.author}',null,null,null)" class="danger"><i class="fas fa-ban"></i> Ban Creator</button>` : ''}
                </div>
            </div>
        </div>
        ${folder.desc ? `<div class="folder-desc-text">${escapeHTML(folder.desc)}</div>` : ''}
        <div class="folder-meta">
            <img src="${folder.authorPfp||''}" class="pfp-tiny"> ${escapeHTML(folder.author||'')}
            ${(folder.author||'') === 'Tasin Redwan' ? '<span style="color:#f1c40f;">👑</span>' : ''}
            <span>•</span> <span>${count} script${count !== 1 ? 's' : ''}</span>
        </div>
        <div class="folder-reactions">
            <button class="btn-reaction${myFR==='like'?' active-like':''}" onclick="reactFolder('${folder.id}','like')"><i class="fas fa-thumbs-up"></i> ${fLikes}</button>
            <button class="btn-reaction${myFR==='dislike'?' active-dislike':''}" onclick="reactFolder('${folder.id}','dislike')"><i class="fas fa-thumbs-down"></i> ${fDislikes}</button>
        </div>
        <div class="folder-scripts-wrap${isOpen?' open':''}" id="fwrap-${folder.id}">
            <div id="finner-${folder.id}" style="margin-top:10px;">${scriptsInner}</div>
            ${isOwner ? `<div class="folder-add-script-area">
                <button class="btn-script-type" onclick="openAddScriptModal('${folder.id}','ServerScript')"><i class="fas fa-plus"></i> ServerScript</button>
                <button class="btn-script-type" onclick="openAddScriptModal('${folder.id}','LocalScript')"><i class="fas fa-plus"></i> LocalScript</button>
            </div>` : ''}
        </div>`;
    return el;
}

function toggleFolder(id, btn) {
    openFolders[id] = !openFolders[id];
    btn.classList.toggle('open', openFolders[id]);
    document.getElementById(`fwrap-${id}`).classList.toggle('open', openFolders[id]);
}

function buildFolderScriptCard(fs, folderId, isOwner, isAdmin) {
    const bc = fs.scriptType === 'ServerScript' ? 'badge-server' : 'badge-local';
    const notMine = !isOwner;
    return `<div class="folder-script-card" id="fsc-${fs.id}">
        <span class="script-type-badge ${bc}">${escapeHTML(fs.scriptType)}</span>
        <div style="position:relative; float:right; display:inline-block;">
            <button class="folder-card-options" onclick="toggleDropdown(event,'fsdrop-${fs.id}')"><i class="fas fa-ellipsis-v"></i></button>
            <div class="dropdown-menu" id="dropdown-fsdrop-${fs.id}" style="right:0;">
                <button onclick="copyFolderScript('${folderId}','${fs.id}')"><i class="fas fa-copy"></i> Copy Code</button>
                <button onclick="openShareFolderScript('${folderId}','${fs.id}')"><i class="fas fa-share-alt"></i> Share</button>
                ${isOwner ? `
                    <button onclick="openEditFolderScriptModal('${folderId}','${fs.id}')"><i class="fas fa-edit"></i> Edit</button>
                    <button onclick="openDeleteFolderScriptModal('${folderId}','${fs.id}')" class="danger"><i class="fas fa-trash"></i> Delete</button>
                ` : ''}
                ${notMine ? `<button onclick="openScriptReportModal('${folderId}','${fs.id}')" class="danger"><i class="fas fa-flag"></i> Report</button>` : ''}
                ${isAdmin && notMine ? `<button onclick="openBanModal('${fs.author}',null,null,null)" class="danger"><i class="fas fa-ban"></i> Ban Author</button>` : ''}
            </div>
        </div>
        <div class="folder-script-title">${escapeHTML(fs.title)}</div>
        ${fs.desc ? `<div class="folder-script-desc">${escapeHTML(fs.desc)}</div>` : ''}
        <pre class="line-numbers" style="margin-top:6px;"><code class="language-lua">${escapeHTML(fs.code)}</code></pre>
    </div>`;
}

// STANDALONE SCRIPT CARD
function buildScriptCard(s) {
    const card = document.createElement('div');
    card.className = 'card';
    let likes = Object.values(s.userReactions).filter(r => r === 'like').length;
    let dislikes = Object.values(s.userReactions).filter(r => r === 'dislike').length;
    const total = likes + dislikes;
    const ratio = total === 0 ? 0 : Math.round((likes / total) * 100);
    const myR = currentUser ? s.userReactions[currentUser.username] : null;
    const myRat = currentUser ? (s.userRatings[currentUser.username] || 0) : 0;
    const isOwner = currentUser && currentUser.username === s.author;
    let cc = 0; function countC(a) { cc += a.length; a.forEach(c => c.replies && countC(c.replies)); } countC(s.comments);
    card.innerHTML = `
        <h3>${escapeHTML(s.title)}</h3>
        <span class="author-tag" style="font-size:11px; margin-bottom:10px; display:flex;">
            <img src="${s.authorPfp}" class="pfp-tiny"> ${escapeHTML(s.author)}
            ${s.author === "Tasin Redwan" ? '<span style="color:#f1c40f; margin-left:3px;">👑</span>' : ''}
        </span>
        <div class="card-options" onclick="toggleDropdown(event,'${s.id}')">
            <i class="fas fa-ellipsis-v"></i>
            <div class="dropdown-menu" id="dropdown-${s.id}">
                <button onclick="fallbackCopy('${s.id}')"><i class="fas fa-copy"></i> Copy</button>
                <button onclick="openShareScriptModal('${s.id}')"><i class="fas fa-share-alt"></i> Share</button>
                ${isOwner ? `
                    <button onclick="editLegacyScript('${s.id}')"><i class="fas fa-edit"></i> Edit</button>
                    <button onclick="openDeleteModal('script','${s.id}',null)" class="danger"><i class="fas fa-trash"></i> Delete</button>
                ` : ''}
            </div>
        </div>
        <p class="desc">${escapeHTML(s.desc)}</p>
        <pre class="line-numbers"><code class="language-lua">${escapeHTML(s.code)}</code></pre>
        <div style="margin-top:14px; display:flex; align-items:center; gap:6px; flex-wrap:wrap;">
            <button class="btn-reaction${myR==='like'?' active-like':''}" onclick="reactScript('${s.id}','like')"><i class="fas fa-thumbs-up"></i> ${likes}</button>
            <button class="btn-reaction${myR==='dislike'?' active-dislike':''}" onclick="reactScript('${s.id}','dislike')"><i class="fas fa-thumbs-down"></i> ${dislikes}</button>
            <div class="rating-container">
                <input type="range" min="0" max="10" value="${myRat}" class="rating-slider" onchange="rateScript('${s.id}',this.value)">
                <span id="rv-${s.id}">${myRat}/10</span>
            </div>
        </div>
        <div class="stats-text"><span>${ratio}% Liked</span><span>${cc} Comments</span></div>
        <div class="comment-section">
            <input class="comment-input" placeholder="Add a comment... (Enter)" onkeydown="addComment(event,'${s.id}')">
            <div id="comments-${s.id}">${s.comments.map(c => renderComment(c, s.id)).join('')}</div>
        </div>`;
    return card;
}

function renderComment(c, scriptId, isReply = false) {
    const myR = currentUser ? c.userReactions[currentUser.username] : null;
    let cL = Object.values(c.userReactions).filter(r => r === 'like').length;
    let cD = Object.values(c.userReactions).filter(r => r === 'dislike').length;
    const isOwner = currentUser && currentUser.username === c.author;
    const isAdmin = currentUser && currentUser.username === "Tasin Redwan";
    let rHtml = '';
    if (c.replies && c.replies.length > 0) {
        rHtml = `<div id="replies-${c.id}" style="display:none;">${c.replies.map(r => renderComment(r, scriptId, true)).join('')}</div>
        <button class="toggle-replies" onclick="toggleReplies('${c.id}')">View ${c.replies.length} Replies</button>`;
    }
    return `<div class="comment-item${isReply?' reply-item':''}" id="c-${c.id}">
        <span class="author-tag"><img src="${c.authorPfp}" class="pfp-tiny"> ${escapeHTML(c.author)}${c.author==="Tasin Redwan"?'<span style="color:#f1c40f;">👑</span>':''}${c.edited?'<span class="edited-tag">(edited)</span>':''}</span>
        <div class="comment-text">${escapeHTML(c.text)}</div>
        <div class="comment-options" onclick="toggleDropdown(event,'${c.id}')"><i class="fas fa-ellipsis-v"></i>
            <div class="dropdown-menu" id="dropdown-${c.id}">
                ${isOwner ? `<button onclick="openCommentEditModal('${scriptId}','${c.id}')"><i class="fas fa-edit"></i> Edit</button>
                    <button onclick="openDeleteModal('comment','${scriptId}','${c.id}')" class="danger"><i class="fas fa-trash"></i> Delete</button>`
                : `<button onclick="openCommentReportModal('${scriptId}','${c.id}')" class="danger"><i class="fas fa-flag"></i> Report</button>`}
                ${isAdmin&&!isOwner?`<button onclick="openBanModal('${c.author}',null,'${scriptId}','${c.id}')" class="danger"><i class="fas fa-ban"></i> Ban</button>`:''}
            </div>
        </div>
        <div class="comment-actions">
            <button class="btn-reaction${myR==='like'?' active-like':''}" onclick="reactComment('${scriptId}','${c.id}','like')"><i class="fas fa-thumbs-up"></i> ${cL}</button>
            <button class="btn-reaction${myR==='dislike'?' active-dislike':''}" onclick="reactComment('${scriptId}','${c.id}','dislike')"><i class="fas fa-thumbs-down"></i> ${cD}</button>
            <button class="btn-reaction" onclick="openCommentReplyModal('${scriptId}','${c.id}')"><i class="fas fa-reply"></i> Reply</button>
        </div>${rHtml}</div>`;
}
function toggleReplies(id) {
    const d = document.getElementById(`replies-${id}`); const b = d.nextElementSibling;
    const h = d.style.display === 'none'; d.style.display = h ? 'block' : 'none';
    b.innerText = h ? 'Hide Replies' : `View ${d.children.length} Replies`;
}

// ════════════════════════════════════
// FOLDER REACTIONS
// ════════════════════════════════════
function reactFolder(folderId, type) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const f = folders.find(f => f.id === folderId);
    if (!f) return;
    if (!f.userReactions) f.userReactions = {};
    if (f.userReactions[currentUser.username] === type) delete f.userReactions[currentUser.username];
    else f.userReactions[currentUser.username] = type;
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    renderScripts();
}

// ════════════════════════════════════
// DROPDOWN
// ════════════════════════════════════
function toggleDropdown(e, id) {
    e.stopPropagation();
    const m = document.getElementById(`dropdown-${id}`);
    document.querySelectorAll('.dropdown-menu').forEach(x => { if (x !== m) x.classList.remove('show'); });
    if (m) m.classList.toggle('show');
}

// ════════════════════════════════════
// COPY / SHARE
// ════════════════════════════════════
function fallbackCopy(sid) { const s = scripts.find(s => s.id === sid); if (s) navigator.clipboard.writeText(s.code).then(() => showToast("Code copied!")); }
function copyFolderScript(fid, sid) { const f = folders.find(f => f.id === fid); if (!f) return; const fs = f.scripts.find(s => s.id === sid); if (fs) navigator.clipboard.writeText(fs.code).then(() => showToast("Code copied!")); }
function openShareScriptModal(sid) {
    const s = scripts.find(s => s.id === sid); if (!s) return;
    const enc = btoa(unescape(encodeURIComponent(JSON.stringify({t:s.title,d:s.desc,c:s.code,a:s.author,p:s.authorPfp,id:s.id}))));
    document.getElementById('shareLinkInput').value = `${location.origin}${location.pathname}?data=${enc}`;
    openModal('shareModal');
}
function openShareFolderScript(fid, sid) {
    const f = folders.find(f => f.id === fid); if (!f) return;
    const fs = f.scripts.find(s => s.id === sid); if (!fs) return;
    const enc = btoa(unescape(encodeURIComponent(JSON.stringify({t:fs.title,d:fs.desc,c:fs.code,a:fs.author,p:fs.authorPfp,id:fs.id,st:fs.scriptType}))));
    document.getElementById('shareLinkInput').value = `${location.origin}${location.pathname}?share=${enc}`;
    openModal('shareModal');
}
function copyShareLink() { navigator.clipboard.writeText(document.getElementById('shareLinkInput').value).then(() => showToast("Link copied!")); }
function checkIncomingShare() {
    const p = new URLSearchParams(location.search);
    const d = p.get('data'), s = p.get('share');
    if (d) {
        try {
            const o = JSON.parse(decodeURIComponent(escape(atob(d))));
            if (!scripts.some(x => x.id === o.id)) { scripts.unshift({id:o.id,title:o.t,desc:o.d,code:o.c,author:o.a,authorPfp:o.p,userReactions:{},userRatings:{},comments:[]}); localStorage.setItem('lua_hub_data', JSON.stringify(scripts)); showToast("Script imported!"); }
        } catch(e){}
        history.replaceState({}, '', location.pathname);
    }
    if (s) {
        try {
            const o = JSON.parse(decodeURIComponent(escape(atob(s))));
            let imp = folders.find(f => f.id === 'f_import');
            if (!imp) { imp = {id:'f_import',name:'Imported Scripts',desc:'Shared scripts from other users',author:'System',authorPfp:'',scripts:[],userReactions:{}}; folders.unshift(imp); }
            if (!imp.scripts.some(x => x.id === o.id)) { imp.scripts.push({id:o.id,title:o.t,desc:o.d||'',code:o.c,scriptType:o.st||'ServerScript',author:o.a,authorPfp:o.p,userReactions:{},userRatings:{}}); openFolders['f_import']=true; localStorage.setItem('lua_hub_folders', JSON.stringify(folders)); showToast("Script imported into 'Imported Scripts'!"); }
        } catch(e){}
        history.replaceState({}, '', location.pathname);
    }
}

// ════════════════════════════════════
// EDIT FOLDER
// ════════════════════════════════════
function openEditFolderModal(fid) {
    const f = folders.find(f => f.id === fid); if (!f) return;
    document.getElementById('editFolderId').value = fid;
    document.getElementById('editFolderName').value = f.name;
    document.getElementById('editFolderDesc').value = f.desc || '';
    openModal('editFolderModal');
}
function submitEditFolder() {
    const id = document.getElementById('editFolderId').value;
    const name = document.getElementById('editFolderName').value.trim();
    const desc = document.getElementById('editFolderDesc').value.trim();
    if (!name) return showToast("Name required!", 'ban');
    const f = folders.find(f => f.id === id); if (!f) return;
    f.name = name; f.desc = desc;
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    closeModal('editFolderModal'); renderScripts(); showToast("Folder updated!");
}

// ════════════════════════════════════
// EDIT FOLDER SCRIPT
// ════════════════════════════════════
let _editFsEditor = null;
function openEditFolderScriptModal(fid, sid) {
    const f = folders.find(f => f.id === fid); if (!f) return;
    const fs = f.scripts.find(s => s.id === sid); if (!fs) return;
    document.getElementById('editFsId').value = sid;
    document.getElementById('editFsFolderId').value = fid;
    document.getElementById('editFsTitle').value = fs.title;
    document.getElementById('editFsDesc').value = fs.desc || '';
    document.getElementById('editFsCode').value = fs.code;
    const bc = fs.scriptType === 'ServerScript' ? 'badge-server' : 'badge-local';
    document.getElementById('editFsBadgeArea').innerHTML = `<span class="script-type-badge ${bc}">${escapeHTML(fs.scriptType)}</span>`;
    openModal('editFolderScriptModal');
    setTimeout(() => {
        if (_editFsEditor) { _editFsEditor.toTextArea(); _editFsEditor = null; }
        _editFsEditor = CodeMirror.fromTextArea(document.getElementById('editFsCode'), {lineNumbers:true, theme:'dracula', mode:'lua'});
        _editFsEditor.setValue(fs.code);
    }, 80);
}
function closeEditFolderScriptModal() {
    if (_editFsEditor) { _editFsEditor.toTextArea(); _editFsEditor = null; }
    closeModal('editFolderScriptModal');
}
function submitEditFolderScript() {
    const fid = document.getElementById('editFsFolderId').value;
    const sid = document.getElementById('editFsId').value;
    const title = document.getElementById('editFsTitle').value.trim();
    const desc = document.getElementById('editFsDesc').value.trim();
    const code = _editFsEditor ? _editFsEditor.getValue() : '';
    if (!title || !code) return showToast("Title and Code required!", 'ban');
    const f = folders.find(f => f.id === fid); if (!f) return;
    const fs = f.scripts.find(s => s.id === sid); if (!fs) return;
    fs.title = title; fs.desc = desc; fs.code = code;
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    closeEditFolderScriptModal(); renderScripts(); showToast("Script updated!");
}

// ════════════════════════════════════
// FOLDER CRUD
// ════════════════════════════════════
function openFolderModal() {
    if (!currentUser) return showToast("Login first!", 'ban');
    document.getElementById('folderNameInput').value = '';
    document.getElementById('folderDescInput').value = '';
    openModal('folderModal');
}
function submitCreateFolder() {
    const name = document.getElementById('folderNameInput').value.trim();
    const desc = document.getElementById('folderDescInput').value.trim();
    if (!name) return showToast("Folder name required!", 'ban');
    const f = {id:'f'+Date.now(), name, desc, author:currentUser.username, authorPfp:currentUser.pfp, scripts:[], userReactions:{}};
    folders.unshift(f); openFolders[f.id] = true;
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    closeModal('folderModal'); renderScripts(); showToast("Folder created!");
}
function openDeleteFolderModal(fid) {
    document.getElementById('deleteModalTitle').innerText = 'Delete Folder?';
    document.getElementById('deleteModalText').innerText = 'This will permanently delete the folder and all its scripts.';
    openModal('deleteModal');
    document.getElementById('deleteModalConfirm').onclick = () => {
        folders = folders.filter(f => f.id !== fid); delete openFolders[fid];
        localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
        closeModal('deleteModal'); renderScripts(); showToast("Folder deleted!");
    };
}
function openDeleteFolderScriptModal(fid, sid) {
    document.getElementById('deleteModalTitle').innerText = 'Delete Script?';
    document.getElementById('deleteModalText').innerText = 'This script will be permanently removed.';
    openModal('deleteModal');
    document.getElementById('deleteModalConfirm').onclick = () => {
        const f = folders.find(f => f.id === fid);
        if (f) { f.scripts = f.scripts.filter(s => s.id !== sid); localStorage.setItem('lua_hub_folders', JSON.stringify(folders)); }
        closeModal('deleteModal'); renderScripts(); showToast("Script deleted!");
    };
}

// ════════════════════════════════════
// ADD SCRIPT
// ════════════════════════════════════
let _addFid = null, _addType = null, _addEditor = null;
function openAddScriptModal(fid, scriptType) {
    if (!currentUser) return showToast("Login first!", 'ban');
    _addFid = fid;
    ['folderScriptTitle','folderScriptDesc'].forEach(id => document.getElementById(id).value = '');
    document.getElementById('addScriptStep1').style.display = 'block';
    document.getElementById('addScriptStep2').style.display = 'none';
    openModal('addScriptModal');
    if (scriptType) setTimeout(() => chooseScriptType(scriptType), 50);
}
function chooseScriptType(type) {
    _addType = type;
    document.getElementById('addScriptStep1').style.display = 'none';
    document.getElementById('addScriptStep2').style.display = 'block';
    const bc = type === 'ServerScript' ? 'badge-server' : 'badge-local';
    document.getElementById('chosenTypeBadgeArea').innerHTML = `<span class="script-type-badge ${bc}">${type}</span>`;
    setTimeout(() => {
        if (_addEditor) { _addEditor.toTextArea(); _addEditor = null; }
        _addEditor = CodeMirror.fromTextArea(document.getElementById('folderScriptCode'), {lineNumbers:true, theme:'dracula', mode:'lua'});
    }, 80);
}
function backToScriptTypeChoice() {
    document.getElementById('addScriptStep1').style.display = 'block';
    document.getElementById('addScriptStep2').style.display = 'none';
    if (_addEditor) { _addEditor.toTextArea(); _addEditor = null; }
}
function closeAddScriptModal() {
    if (_addEditor) { _addEditor.toTextArea(); _addEditor = null; }
    closeModal('addScriptModal');
}
function submitAddScriptToFolder() {
    if (!currentUser) return showToast("Login first!", 'ban');
    const title = document.getElementById('folderScriptTitle').value.trim();
    const desc = document.getElementById('folderScriptDesc').value.trim();
    const code = _addEditor ? _addEditor.getValue() : '';
    if (!title || !code) return showToast("Title and Code required!", 'ban');
    const f = folders.find(f => f.id === _addFid); if (!f) return;
    f.scripts.push({id:'fs'+Date.now(), title, desc, code, scriptType:_addType, author:currentUser.username, authorPfp:currentUser.pfp, userReactions:{}, userRatings:{}});
    localStorage.setItem('lua_hub_folders', JSON.stringify(folders));
    closeAddScriptModal(); renderScripts(); showToast("Script added!");
}

// ════════════════════════════════════
// REPORT SYSTEM
// ════════════════════════════════════
function openFolderReportModal(fid) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const f = folders.find(f => f.id === fid); if (!f) return;
    document.getElementById('reportModalTitle').innerText = '🚩 Report Folder';
    document.getElementById('reportModalContext').innerText = `"${f.name}" by ${f.author}`;
    document.getElementById('reportReason').value = '';
    document.getElementById('reportSubmitBtn').onclick = () => {
        const reason = document.getElementById('reportReason').value.trim();
        if (!reason) return showToast("Reason required!", 'ban');
        folderReports.push({type:'folder', folderId:fid, scriptId:null, reporter:currentUser.username, reportedUser:f.author, reason, itemName:f.name, ts:Date.now()});
        localStorage.setItem('lua_hub_folder_reports', JSON.stringify(folderReports));
        closeModal('reportModal'); showToast("Folder reported!", 'info');
    };
    openModal('reportModal');
}
function openScriptReportModal(fid, sid) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const f = folders.find(f => f.id === fid); if (!f) return;
    const fs = f.scripts.find(s => s.id === sid); if (!fs) return;
    document.getElementById('reportModalTitle').innerText = '🚩 Report Script';
    document.getElementById('reportModalContext').innerText = `"${fs.title}" by ${fs.author}`;
    document.getElementById('reportReason').value = '';
    document.getElementById('reportSubmitBtn').onclick = () => {
        const reason = document.getElementById('reportReason').value.trim();
        if (!reason) return showToast("Reason required!", 'ban');
        folderReports.push({type:'script', folderId:fid, scriptId:sid, reporter:currentUser.username, reportedUser:fs.author, reason, itemName:fs.title, ts:Date.now()});
        localStorage.setItem('lua_hub_folder_reports', JSON.stringify(folderReports));
        closeModal('reportModal'); showToast("Script reported!", 'info');
    };
    openModal('reportModal');
}
function openCommentReportModal(scriptId, commentId) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const s = scripts.find(s => s.id === scriptId);
    const findC = (arr) => { for (let c of arr) { if (c.id === commentId) return c; let r = findC(c.replies); if (r) return r; } };
    const c = s ? findC(s.comments) : null;
    document.getElementById('reportModalTitle').innerText = '🚩 Report Comment';
    document.getElementById('reportModalContext').innerText = c ? `By ${c.author}: "${c.text.substring(0,50)}${c.text.length>50?'...':''}"` : '';
    document.getElementById('reportReason').value = '';
    document.getElementById('reportSubmitBtn').onclick = () => {
        const reason = document.getElementById('reportReason').value.trim();
        if (!reason) return showToast("Reason required!", 'ban');
        if (!c) return;
        reportedComments.push({reporter:currentUser.username, reportedUser:c.author, reason, commentText:c.text, scriptId, commentId});
        localStorage.setItem('lua_hub_reports', JSON.stringify(reportedComments));
        closeModal('reportModal'); showToast("Comment reported!", 'info');
    };
    openModal('reportModal');
}

// ════════════════════════════════════
// COMMENT MODALS
// ════════════════════════════════════
function openCommentEditModal(scriptId, commentId) {
    const s = scripts.find(s => s.id === scriptId);
    const findC = (arr) => { for (let c of arr) { if (c.id === commentId) return c; let r = findC(c.replies); if (r) return r; } };
    const c = s ? findC(s.comments) : null; if (!c) return;
    document.getElementById('inputModalTitle').innerText = '✏️ Edit Comment';
    document.getElementById('inputModalText').value = c.text;
    document.getElementById('inputModalText').placeholder = '';
    document.getElementById('inputModalSubmit').onclick = () => {
        const t = document.getElementById('inputModalText').value.trim();
        if (!t) return showToast("Required", 'ban');
        c.text = t; c.edited = true;
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        closeModal('inputModal'); renderScripts();
    };
    openModal('inputModal');
}
function openCommentReplyModal(scriptId, parentId) {
    if (!currentUser) return showToast("Login first!", 'ban');
    document.getElementById('inputModalTitle').innerText = '↩️ Reply';
    document.getElementById('inputModalText').value = '';
    document.getElementById('inputModalText').placeholder = 'Write your reply...';
    document.getElementById('inputModalSubmit').onclick = () => {
        const t = document.getElementById('inputModalText').value.trim();
        if (!t) return showToast("Required", 'ban');
        const s = scripts.find(s => s.id === scriptId);
        const doReply = (arr) => { for (let item of arr) { if (item.id === parentId) { item.replies.push({id:'r'+Date.now(), author:currentUser.username, authorPfp:currentUser.pfp, text:t, userReactions:{}, replies:[], edited:false}); return true; } if (doReply(item.replies)) return true; } };
        doReply(s.comments);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        closeModal('inputModal'); renderScripts();
    };
    openModal('inputModal');
}

// ════════════════════════════════════
// ADMIN
// ════════════════════════════════════
let _adminTab = 'reports';
function adminTab(t) {
    _adminTab = t;
    document.getElementById('tabReports').classList.toggle('active', t==='reports');
    document.getElementById('tabBans').classList.toggle('active', t==='bans');
    showAdminContent();
}
function openAdminModal() { openModal('adminModal'); showAdminContent(); }
function showAdminContent() {
    const area = document.getElementById('adminContentArea');
    if (_adminTab === 'reports') {
        const all = [
            ...reportedComments.map((r,i) => ({...r, _src:'comment', _i:i, _label:'Comment', _css:'badge-comment-report', _preview:r.commentText})),
            ...folderReports.map((r,i) => ({...r, _src:r.type, _i:i, _label:r.type==='folder'?'Folder':'Script', _css:r.type==='folder'?'badge-folder-report':'badge-script-report', _preview:r.itemName}))
        ];
        if (!all.length) { area.innerHTML = `<p style="color:#444; font-family:sans-serif; font-size:13px; padding:10px 0;">No reports yet.</p>`; return; }
        area.innerHTML = `<table class="admin-table">
            <tr><th>Type</th><th>User</th><th>Details</th><th>Action</th></tr>
            ${all.map(r => `<tr>
                <td><span class="report-badge ${r._css}">${r._label}</span></td>
                <td style="font-family:'Fredoka One'; font-size:13px;">${escapeHTML(r.reportedUser)}</td>
                <td><b style="font-size:11px;">${escapeHTML(r.reason)}</b><div class="report-preview">${escapeHTML(r._preview||'')}</div></td>
                <td style="white-space:nowrap; padding-right:5px;">
                    <button class="btn-reaction" style="background:#e74c3c;color:white;padding:3px 8px;font-size:10px;margin-bottom:3px;" onclick="adminBanFromReport('${r._src}',${r._i},'${escapeJS(r.reportedUser)}','${r.folderId||r.scriptId||''}','${r.commentId||''}')">Ban</button>
                    <button class="btn-reaction" style="background:#1a1a2c;color:#888;padding:3px 8px;font-size:10px;" onclick="dismissReport('${r._src}',${r._i})">✕</button>
                </td>
            </tr>`).join('')}
        </table>`;
    } else {
        if (!bannedUsers.length) { area.innerHTML = `<p style="color:#444; font-family:sans-serif; font-size:13px; padding:10px 0;">No banned users.</p>`; return; }
        area.innerHTML = `<table class="admin-table">
            <tr><th>Username</th><th>Warnings</th><th>Action</th></tr>
            ${bannedUsers.map(u => `<tr>
                <td style="font-family:'Fredoka One'; font-size:13px;">${escapeHTML(u)}</td>
                <td style="color:#e74c3c; font-family:sans-serif; font-weight:bold;">⚠ ${userWarningLevels[u]||1}/10</td>
                <td><button class="btn-reaction" style="background:#27ae60;color:white;padding:3px 8px;font-size:10px;" onclick="unbanUser('${u}')">Unban</button></td>
            </tr>`).join('')}
        </table>`;
    }
}
function escapeJS(s) { return String(s||'').replace(/'/g,"&#39;"); }
function dismissReport(src, idx) {
    if (src === 'comment') { reportedComments.splice(Number(idx),1); localStorage.setItem('lua_hub_reports', JSON.stringify(reportedComments)); }
    else { folderReports.splice(Number(idx),1); localStorage.setItem('lua_hub_folder_reports', JSON.stringify(folderReports)); }
    showAdminContent();
}
function adminBanFromReport(src, idx, username, refId, commentId) {
    openBanModal(username, {src, index:Number(idx)}, refId, commentId);
}
function openBanModal(username, reportRef, scriptOrFolderId, commentId) {
    document.getElementById('banModalContext').innerText = `You are about to ban: ${username}`;
    document.getElementById('banDays').value = '';
    document.getElementById('banReason').value = '';
    openModal('banModal');
    document.getElementById('banModalConfirm').onclick = () => {
        const days = document.getElementById('banDays').value;
        const reason = document.getElementById('banReason').value.trim();
        if (!days || !reason) return showToast("Fill all fields!", 'ban');
        if (!bannedUsers.includes(username)) {
            bannedUsers.push(username);
            userWarningLevels[username] = (userWarningLevels[username] || 0) + 1;
            localStorage.setItem('lua_hub_banned', JSON.stringify(bannedUsers));
            localStorage.setItem('lua_hub_warnings', JSON.stringify(userWarningLevels));
            const lvl = userWarningLevels[username];
            if (lvl >= 10) showToast(`${username} permanently banned! (10/10 warnings)`, 'ban');
            else showToast(`${username} banned for ${days} days (Warning ${lvl}/10)`, 'ban');
        }
        if (commentId && scriptOrFolderId) {
            const s = scripts.find(s => s.id === scriptOrFolderId);
            if (s) { const del = (arr) => arr.forEach((c,i) => { if (c.id===commentId) arr.splice(i,1); else del(c.replies); }); del(s.comments); localStorage.setItem('lua_hub_data', JSON.stringify(scripts)); }
        }
        if (reportRef) dismissReport(reportRef.src, reportRef.index);
        closeModal('banModal');
        updateLoginUI(); renderScripts();
        if (document.getElementById('adminModal').classList.contains('show')) showAdminContent();
    };
}
function unbanUser(username) {
    bannedUsers = bannedUsers.filter(u => u !== username);
    localStorage.setItem('lua_hub_banned', JSON.stringify(bannedUsers));
    let n = JSON.parse(localStorage.getItem('lua_hub_unbanned_notify')) || [];
    n.push(username); localStorage.setItem('lua_hub_unbanned_notify', JSON.stringify(n));
    showToast(`${username} unbanned!`, 'unban');
    showAdminContent(); updateLoginUI();
}

// ════════════════════════════════════
// LEGACY SCRIPT ACTIONS
// ════════════════════════════════════
function editLegacyScript(sid) {
    const s = scripts.find(s => s.id === sid); if (!s) return;
    document.getElementById('scriptTitle').value = s.title;
    document.getElementById('scriptDesc').value = s.desc;
    editor.setValue(s.code);
    scripts.splice(scripts.indexOf(s), 1);
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
    document.getElementById('createCard').style.display = 'block';
    renderScripts(); window.scrollTo(0, 0); showToast("Editing — repost when done", 'info');
}
function openDeleteModal(type, scriptId, itemId) {
    document.getElementById('deleteModalTitle').innerText = `Delete ${type==='script'?'Script':'Comment'}?`;
    document.getElementById('deleteModalText').innerText = `This will permanently delete this ${type}.`;
    openModal('deleteModal');
    document.getElementById('deleteModalConfirm').onclick = () => {
        if (type === 'script') {
            scripts = scripts.filter(s => s.id !== scriptId);
        } else {
            const s = scripts.find(s => s.id === scriptId);
            const del = (arr) => { for (let i=0;i<arr.length;i++) { if (arr[i].id===itemId) { arr.splice(i,1); return true; } if (del(arr[i].replies)) return true; } };
            del(s.comments);
        }
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        closeModal('deleteModal'); renderScripts(); showToast("Deleted!");
    };
}
function addComment(e, scriptId) {
    if (!currentUser) return showToast("Login first!", 'ban');
    if (e.key === 'Enter' && e.target.value.trim()) {
        const s = scripts.find(s => s.id === scriptId); if (!s) return;
        s.comments.push({id:'c'+Date.now(), author:currentUser.username, authorPfp:currentUser.pfp, text:e.target.value, userReactions:{}, replies:[], edited:false});
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        e.target.value = ''; renderScripts();
    }
}
function reactComment(scriptId, commentId, type) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const s = scripts.find(s => s.id === scriptId);
    const ra = (arr) => { for (let c of arr) { if (c.id===commentId) { if (c.userReactions[currentUser.username]===type) delete c.userReactions[currentUser.username]; else c.userReactions[currentUser.username]=type; return true; } if (ra(c.replies)) return true; } };
    ra(s.comments);
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts)); renderScripts();
}
function reactScript(id, type) {
    if (!currentUser) return showToast("Login first!", 'ban');
    if (bannedUsers.includes(currentUser.username)) return showToast("Banned!", 'ban');
    const s = scripts.find(s => s.id === id); if (!s) return;
    if (s.userReactions[currentUser.username]===type) delete s.userReactions[currentUser.username]; else s.userReactions[currentUser.username]=type;
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts)); renderScripts();
}
function rateScript(id, rating) {
    if (!currentUser) return showToast("Login first!", 'ban');
    const s = scripts.find(s => s.id === id); if (!s) return;
    s.userRatings[currentUser.username] = parseInt(rating);
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
    const el = document.getElementById(`rv-${id}`); if (el) el.innerText = `${rating}/10`;
    showToast("Rating saved!");
}

// ════════════════════════════════════
// AUTH
// ════════════════════════════════════
function updateLoginUI() {
    const area = document.getElementById('authAreaContainer');
    if (currentUser) {
        const bc = bannedUsers.length;
        area.innerHTML = `
            ${currentUser.username==="Tasin Redwan"?`<button class="btn btn-admin" onclick="openAdminModal()" title="Admin Panel"><i class="fas fa-shield-alt"></i></button>`:''}
            <button class="btn-add-folder" onclick="openFolderModal()" title="Create Folder">+</button>
            <div class="pfp-container">
                <img src="${currentUser.pfp}" class="pfp-header" onclick="toggleProfilePanel(event)">
                ${currentUser.username==="Tasin Redwan"&&bc>0?`<div class="ban-counter">${bc}</div>`:''}
            </div>`;
        document.getElementById('createCard').style.display = 'none';
        checkUnbanNotification();
    } else {
        area.innerHTML = `<button class="btn btn-login" onclick="openAuthModal('login')">Login</button>`;
        document.getElementById('createCard').style.display = 'none';
    }
}
function checkUnbanNotification() {
    let n = JSON.parse(localStorage.getItem('lua_hub_unbanned_notify')) || [];
    if (n.includes(currentUser.username)) {
        showToast("You've been unbanned — follow the guidelines!", 'unban', 5000);
        localStorage.setItem('lua_hub_unbanned_notify', JSON.stringify(n.filter(u => u !== currentUser.username)));
    }
}
function logout() { currentUser = null; localStorage.removeItem('lua_hub_session'); document.getElementById('profilePanel').classList.remove('show'); updateLoginUI(); renderScripts(); }
function openAuthModal(t) { authType = t; updateAuthModalUI(); openModal('authModal'); }
function toggleAuthType() { authType = authType==='login'?'register':'login'; updateAuthModalUI(); }
function updateAuthModalUI() {
    const l = authType==='login';
    document.getElementById('authTitle').innerText = l ? 'Login' : 'Sign Up';
    document.getElementById('authSubmitBtn').innerText = l ? 'Login' : 'Register';
    document.getElementById('authToggleText').innerHTML = l ? "Don't have an account? <span>Sign Up</span>" : "Already have an account? <span>Login</span>";
}
function processAuth() {
    const username = document.getElementById('authUsername').value.trim();
    const password = document.getElementById('authPassword').value;
    const pfp = `https://api.dicebear.com/7.x/pixel-art/svg?${selectedPfpSeed}`;
    if (!username || !password) return showToast("All fields required", 'ban');
    if (authType === 'register') {
        if (users[username]) return showToast("Username taken!", 'ban');
        users[username] = {username, password, pfp};
        localStorage.setItem('lua_hub_users', JSON.stringify(users));
        showToast("Account created! Please login."); toggleAuthType();
    } else {
        if (bannedUsers.includes(username)) return showToast("Account is banned!", 'ban');
        if (users[username] && users[username].password === password) {
            currentUser = users[username];
            localStorage.setItem('lua_hub_session', JSON.stringify(currentUser));
            closeModal('authModal'); updateLoginUI(); renderScripts(); showToast("Welcome back!");
        } else showToast("Wrong credentials", 'ban');
    }
}
function selectPfp(el, seed) { document.querySelectorAll('.pfp-option').forEach(i => i.classList.remove('selected')); el.classList.add('selected'); selectedPfpSeed = seed; }

// ════════════════════════════════════
// UTILS
// ════════════════════════════════════
function escapeHTML(s) { return String(s||'').replace(/[&<>"']/g, m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
function showToast(msg, type='', dur=3000) {
    const t = document.getElementById('toast'); t.innerText = msg; t.className = `show ${type}`;
    clearTimeout(t._tid); t._tid = setTimeout(() => t.classList.remove('show'), dur);
}

// Legacy editor
const editor = CodeMirror.fromTextArea(document.getElementById('scriptCode'), {lineNumbers:false, theme:'dracula', mode:'lua'});
function saveScript() {
    if (!currentUser || bannedUsers.includes(currentUser.username)) return showToast("Banned!", 'ban');
    const title = document.getElementById('scriptTitle').value.trim();
    const desc = document.getElementById('scriptDesc').value.trim();
    const code = editor.getValue();
    if (!title || !code) return showToast("Title and Code required!", 'ban');
    scripts.unshift({id:'id'+Date.now(), author:currentUser.username, authorPfp:currentUser.pfp, title, desc, code, userReactions:{}, userRatings:{}, comments:[]});
    localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
    document.getElementById('createCard').style.display = 'none';
    document.getElementById('scriptTitle').value = ''; document.getElementById('scriptDesc').value = ''; editor.setValue('');
    renderScripts(); showToast("Published!");
}

// INIT
window.onload = () => { checkIncomingShare(); updateLoginUI(); renderScripts(); };
</script>
</body>
</html>
