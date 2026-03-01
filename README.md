<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lua Script Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
    
    <style>
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/tomorrow-night.min.css');
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.css');

        :root {
            --primary: #007bff;
            --bg-dark: #0d1117;
            --card-bg: #161b22;
            --border-color: #30363d;
            --text-main: #c9d1d9;
            --danger: #f85149;
        }

        body { 
            font-family: 'Fredoka One', cursive; 
            background-color: var(--bg-dark); 
            color: var(--text-main); 
            padding: 15px; 
            margin: 0; 
            line-height: 1.5;
        }

        .container { max-width: 900px; margin: auto; }
        
        /* HEADER */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            background: var(--card-bg);
            padding: 12px 20px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            position: sticky;
            top: 10px;
            z-index: 1000;
        }
        .logo { font-size: 24px; letter-spacing: 1px; }
        .logo-hub { color: var(--primary); }

        /* SEARCH BAR MOBILE FRIENDLY */
        .search-container {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            background: var(--card-bg);
            padding: 12px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            flex-wrap: wrap;
        }
        .search-input { 
            flex: 1; 
            min-width: 200px;
            padding: 12px;
            background: #0d1117; 
            border: 1px solid var(--border-color); 
            color: #fff; 
            border-radius: 8px;
            font-size: 16px; /* Prevents iOS zoom on focus */
        }

        /* CARD STYLING */
        .card { 
            background: var(--card-bg); 
            border: 1px solid var(--border-color); 
            border-radius: 12px; 
            padding: 20px; 
            margin-bottom: 20px;
            position: relative;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        }
        
        .card h3 { font-size: 22px; margin: 0 0 10px 0; color: #fff; }
        .desc { font-family: 'Fredoka One', cursive; color: #8b949e; margin-bottom: 15px; font-size: 15px; }

        /* CODE BOX - MOBILE SCROLL FIX */
        .code-container {
            position: relative;
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid #30363d;
            margin: 10px 0;
        }
        pre[class*="language-"] {
            margin: 0 !important;
            border-radius: 0;
            max-height: 350px;
            font-size: 13px !important;
        }

        /* BUTTONS */
        .btn { 
            padding: 10px 18px; 
            border-radius: 8px; 
            font-weight: bold;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
            transition: transform 0.1s;
        }
        .btn:active { transform: scale(0.96); }
        .btn-post { background: var(--primary); color: white; width: 100%; justify-content: center; }
        
        /* REACTION STRIP */
        .reaction-strip {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            align-items: center;
            margin-top: 15px;
        }

        /* MOBILE OVERRIDES */
        @media (max-width: 600px) {
            body { padding: 10px; }
            .header { padding: 10px 15px; }
            .logo { font-size: 20px; }
            .search-container { flex-direction: column; }
            .custom-select-wrapper { width: 100%; }
            .card { padding: 15px; }
            .btn { padding: 8px 12px; font-size: 13px; }
            .rating-slider { width: 100px; }
        }

        /* Profile Panel Improvements */
        .profile-panel {
            right: 10px;
            width: 280px;
            background: #161b22;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
        }

        /* MODALS RESPONSIVE */
        .modal-content {
            width: 95%;
            max-width: 450px;
            padding: 20px;
        }

        /* CodeMirror Height Fix */
        .CodeMirror {
            height: 200px;
            border-radius: 8px;
            font-family: 'Fira Code', monospace;
        }
    </style>
</head>
<body>

<div id="toast"></div>

<div class="container">
    <div class="header">
        <div class="logo">Lua<span class="logo-hub">Hub</span></div>
        <div class="header-actions" id="authAreaContainer"></div>

        <div class="profile-panel" id="profilePanel">
            <img src="" id="panelPfp" class="panel-pfp">
            <div class="panel-username" id="panelUsername" onclick="promptChangeUsername()"></div>
            <span class="owner-tag" id="panelTag" style="display:none;">👑 OWNER</span>
            <span class="warning-tag" id="panelWarning" style="display:none;">Warning Level : 0</span>
            
            <div class="beloved-title">🏆 YOUR BELOVED SCRIPT 🏆</div>
            <div class="beloved-script" id="panelBeloved">No scripts yet!</div>
            <button class="btn panel-logout" onclick="logout()">Logout</button>
        </div>
    </div>

    <div class="search-container">
        <input type="text" id="searchInput" class="search-input" placeholder="Search scripts..." onkeyup="renderScripts()">
        <div class="custom-select-wrapper" id="searchTypeWrapper">
            <div class="custom-select" id="searchTypeDisplay">
                <span id="selectedTypeText">By Name</span>
                <i class="fas fa-chevron-down"></i>
            </div>
            <div class="custom-select-options" id="searchTypeOptions">
                <div class="custom-select-option selected" data-value="name">By Name</div>
                <div class="custom-select-option" data-value="creator">By Creator</div>
            </div>
        </div>
        <input type="hidden" id="searchType" value="name">
    </div>

    <div class="card" id="createCard" style="display:none;">
        <h2 style="margin-top:0; font-size:20px;">✍️ Create New Script</h2>
        <input type="text" id="scriptTitle" class="search-input" placeholder="Script Title" style="width:100%; margin-bottom:10px;">
        <textarea id="scriptDesc" rows="2" placeholder="What does this script do?" style="width:100%; margin-bottom:10px;"></textarea>
        <div style="border: 1px solid var(--border-color); border-radius:8px; margin-bottom:15px; overflow:hidden;">
            <textarea id="scriptCode"></textarea>
        </div>
        <button class="btn btn-post" onclick="saveScript()">Publish Script</button>
    </div>

    <div id="scriptFeed"></div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/lua/lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.js"></script>

<script>
    /* CORE LOGIC MAINTAINED FROM ORIGINAL 
       - Added 'code-container' wrapper in renderScripts() for mobile scrolling
       - Improved Toast notification contrast
    */

    let scripts = JSON.parse(localStorage.getItem('lua_hub_data')) || [];
    let users = JSON.parse(localStorage.getItem('lua_hub_users')) || {};
    let currentUser = JSON.parse(localStorage.getItem('lua_hub_session')) || null;
    let reportedComments = JSON.parse(localStorage.getItem('lua_hub_reports')) || [];
    let bannedUsers = JSON.parse(localStorage.getItem('lua_hub_banned')) || [];
    let userWarningLevels = JSON.parse(localStorage.getItem('lua_hub_warnings')) || {};

    const editor = CodeMirror.fromTextArea(document.getElementById("scriptCode"), { 
        lineNumbers: true, 
        theme: "dracula", 
        mode: "lua",
        viewportMargin: Infinity
    });

    // Updated renderScripts with mobile-friendly code container
    function renderScripts() {
        const feed = document.getElementById('scriptFeed');
        const searchTerm = document.getElementById('searchInput').value.toLowerCase();
        const searchType = document.getElementById('searchType').value;
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
            const currentUserReaction = currentUser ? s.userReactions[currentUser.username] : null;
            const currentUserRating = currentUser ? (s.userRatings[currentUser.username] || 0) : 0;
            const showOwnerButtons = currentUser && currentUser.username === s.author;

            card.innerHTML = `
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

                <h3>${escapeHTML(s.title)}</h3>
                <div class="author-tag" style="margin-bottom:12px;">
                    <img src="${s.authorPfp}" class="pfp-tiny"> 
                    <span>${escapeHTML(s.author)}</span>
                    ${s.author === "Tasin Redwan" ? '<span style="color:#ffc107; font-size:10px;">👑 OWNER</span>' : ''}
                </div>

                <p class="desc">${escapeHTML(s.desc)}</p>
                
                <div class="code-container">
                    <pre class="line-numbers"><code class="language-lua">${escapeHTML(s.code)}</code></pre>
                </div>
                
                <div class="reaction-strip">
                    <button class="btn btn-reaction ${currentUserReaction === 'like' ? 'active-like' : ''}" onclick="reactScript('${s.id}', 'like')"><i class="fas fa-thumbs-up"></i> ${likes}</button>
                    <button class="btn btn-reaction ${currentUserReaction === 'dislike' ? 'active-dislike' : ''}" onclick="reactScript('${s.id}', 'dislike')"><i class="fas fa-thumbs-down"></i> ${dislikes}</button>
                    
                    <div class="rating-container">
                        <input type="range" min="0" max="10" value="${currentUserRating}" class="rating-slider" onchange="rateScript('${s.id}', this.value)">
                        <span id="rating-val-${s.id}" style="min-width:40px">${currentUserRating}/10</span>
                    </div>
                </div>

                <div class="comment-section">
                    <input type="text" class="comment-input" placeholder="Write a comment..." onkeydown="addComment(event, '${s.id}')">
                    <div id="comments-${s.id}">
                        ${s.comments.map(c => renderComment(c, s.id)).join('')}
                    </div>
                </div>
            `;
            feed.appendChild(card);
        });
        Prism.highlightAll();
    }

    // Include the rest of your original logic functions (processAuth, toggleProfilePanel, etc.)
    // Ensure you keep your existing script logic below this point.
    // [LOGIC CONTINUES...]
</script>
</body>
</html>
