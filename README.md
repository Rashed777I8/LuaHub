<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>LuaHub</title>
<link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code:wght@400;500&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism-tomorrow.min.css">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#07070f;--bg2:#0c0c1a;--bg3:#11112a;--bg4:#161635;--bg5:#1c1c42;
  --border:#1e1e50;--border2:#262665;
  --accent:#5b8ef0;--accent2:#7b6ef6;
  --green:#27ae60;--red:#e74c3c;--gold:#f1c40f;
  --text:#e0e0ff;--text2:#8888bb;--text3:#44446a;
  --server:#2ecc71;--local:#e74c3c;
  --r:14px;--rsm:9px;
  --sh:0 8px 32px rgba(0,0,0,0.65);--shsm:0 4px 18px rgba(0,0,0,0.45);
  --tr:0.22s cubic-bezier(0.4,0,0.2,1);
}
html{scroll-behavior:smooth}
body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;-webkit-font-smoothing:antialiased}
body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(91,142,240,0.025) 1px,transparent 1px),linear-gradient(90deg,rgba(91,142,240,0.025) 1px,transparent 1px);background-size:44px 44px;pointer-events:none;z-index:0}
body::after{content:'';position:fixed;top:-40%;left:-40%;width:180%;height:180%;background:radial-gradient(ellipse at 25% 20%,rgba(91,142,240,0.07) 0%,transparent 55%),radial-gradient(ellipse at 75% 75%,rgba(123,110,246,0.05) 0%,transparent 55%);pointer-events:none;z-index:0;animation:bgp 9s ease-in-out infinite alternate}
@keyframes bgp{from{opacity:0.6}to{opacity:1}}
.wrap{width:100%;max-width:560px;margin:0 auto;padding:10px 8px 50px;position:relative;z-index:1}

/* ANIMATIONS */
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@keyframes slideDown{from{opacity:0;transform:translateY(-8px) scale(0.97)}to{opacity:1;transform:translateY(0) scale(1)}}
@keyframes popIn{from{opacity:0;transform:scale(0.9) translateY(14px)}to{opacity:1;transform:scale(1) translateY(0)}}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.4}}
.au{animation:fadeUp 0.32s var(--tr) both}
.au1{animation:fadeUp 0.32s var(--tr) 0.05s both}
.au2{animation:fadeUp 0.32s var(--tr) 0.1s both}

/* HEADER */
.header{display:flex;justify-content:space-between;align-items:center;padding:14px 18px;background:linear-gradient(135deg,var(--bg3),var(--bg4));border:1.5px solid var(--border2);border-radius:18px;margin-bottom:14px;position:relative;box-shadow:var(--shsm),inset 0 1px 0 rgba(255,255,255,0.03)}
.logo{font-family:'Fredoka One',cursive;font-size:26px;line-height:1;letter-spacing:-0.5px;display:flex;align-items:center;gap:2px}
.logo-lua{color:var(--text)}
.logo-hub{color:var(--accent);text-shadow:0 0 22px rgba(91,142,240,0.5)}
.logo-dot{display:inline-block;width:6px;height:6px;background:var(--accent);border-radius:50%;margin-bottom:10px;animation:blink 2.5s infinite}
.hact{display:flex;align-items:center;gap:10px}

/* ICON BUTTON */
.ibtn{width:38px;height:38px;border:none;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;font-size:15px;transition:all var(--tr)}
.ibtn:hover{transform:scale(1.08)}
.ibtn.admin-btn{background:linear-gradient(135deg,#c0392b,#96281b);color:white;box-shadow:0 2px 10px rgba(192,57,43,0.4)}
.ibtn.add-btn{background:linear-gradient(135deg,var(--green),#1e8449);color:white;box-shadow:0 2px 10px rgba(39,174,96,0.35);font-size:20px;font-family:'Fredoka One',cursive}
.ibtn.add-btn:hover{box-shadow:0 4px 18px rgba(39,174,96,0.55)}

/* PFP */
.pfp-wrap{position:relative}
.pfp-hdr{width:40px;height:40px;border-radius:50%;object-fit:cover;border:2.5px solid var(--accent);cursor:pointer;transition:all var(--tr);display:block}
.pfp-hdr:hover{transform:scale(1.08);box-shadow:0 0 14px rgba(91,142,240,0.5)}
.ban-badge{position:absolute;top:-4px;right:-4px;background:var(--red);color:white;border-radius:50%;width:18px;height:18px;font-size:9px;font-weight:700;display:flex;align-items:center;justify-content:center;border:2px solid var(--bg);font-family:'Inter',sans-serif}

/* PROFILE PANEL */
.pp{display:none;position:absolute;top:calc(100% + 10px);right:0;width:240px;background:linear-gradient(160deg,var(--bg3),var(--bg4));border:1.5px solid var(--border2);border-radius:16px;padding:18px 16px;z-index:500;box-shadow:var(--sh);text-align:center}
.pp.show{display:block;animation:slideDown 0.2s var(--tr) both}
.pp-av{width:72px;height:72px;border-radius:50%;border:3px solid var(--accent);margin-bottom:10px;box-shadow:0 0 22px rgba(91,142,240,0.3)}
.pp-nm{font-family:'Fredoka One',cursive;font-size:17px;color:var(--text);margin-bottom:4px;cursor:pointer;padding:2px 8px;border-radius:6px;transition:background var(--tr);display:inline-block}
.pp-nm:hover{background:var(--bg5)}
.pp-own{color:var(--gold);font-size:11px;font-family:'Inter',sans-serif;font-weight:600;margin-bottom:4px;display:block}
.pp-wrn{color:var(--red);font-size:11px;display:inline-block;margin-bottom:10px;background:rgba(231,76,60,0.1);padding:2px 8px;border-radius:5px;border:1px solid rgba(231,76,60,0.2);font-family:'Inter',sans-serif}
.pp-bl-lbl{font-size:10px;color:var(--text3);letter-spacing:1px;text-transform:uppercase;margin-top:10px;font-family:'Inter',sans-serif}
.pp-bl{background:var(--bg2);padding:8px 10px;border-radius:8px;margin-top:5px;font-size:12px;border:1px solid var(--border);color:var(--text2);font-family:'Inter',sans-serif;text-align:left}
.pp-out{width:100%;margin-top:14px;background:linear-gradient(135deg,var(--red),#c0392b);color:white;border:none;border-radius:var(--rsm);padding:9px;font-family:'Fredoka One',cursive;font-size:14px;cursor:pointer;transition:all var(--tr)}
.pp-out:hover{opacity:0.88;transform:translateY(-1px)}

/* SEARCH */
.sw{display:flex;gap:8px;align-items:stretch;background:var(--bg3);border:1.5px solid var(--border2);border-radius:var(--r);padding:10px;margin-bottom:14px}
.si{flex:1;min-width:0;background:var(--bg5);border:1.5px solid var(--border2);border-radius:var(--rsm);color:var(--text);padding:10px 14px;font-family:'Inter',sans-serif;font-size:14px;outline:none;transition:border var(--tr)}
.si:focus{border-color:var(--accent)}
.si::placeholder{color:var(--text3)}
.csel-w{position:relative;user-select:none;min-width:145px}
.csel{background:var(--bg5);border:1.5px solid var(--border2);border-radius:var(--rsm);color:var(--text2);padding:10px 12px;cursor:pointer;display:flex;align-items:center;justify-content:space-between;gap:6px;font-family:'Inter',sans-serif;font-size:12px;transition:border var(--tr),color var(--tr);white-space:nowrap;height:100%}
.csel:hover{border-color:var(--accent);color:var(--text)}
.csel i.arr{font-size:9px;transition:transform var(--tr)}
.csel.open i.arr{transform:rotate(180deg)}
.csel-opts{display:none;position:absolute;top:calc(100% + 6px);right:0;background:var(--bg4);border:1.5px solid var(--border2);border-radius:11px;z-index:300;overflow:hidden;box-shadow:var(--sh);min-width:170px}
.csel-opts.open{display:block;animation:slideDown 0.16s var(--tr) both}
.csel-opt{padding:10px 14px;cursor:pointer;font-family:'Inter',sans-serif;font-size:13px;color:var(--text2);transition:background var(--tr),color var(--tr);display:flex;align-items:center;gap:9px}
.csel-opt:hover{background:var(--bg5);color:var(--text)}
.csel-opt.act{background:rgba(91,142,240,0.13);color:var(--accent);font-weight:600}
.csel-opt i{width:15px;text-align:center;font-size:11px}

/* CARD (legacy) */
.card{background:linear-gradient(160deg,var(--bg3),var(--bg4));border:1.5px solid var(--border2);border-radius:var(--r);padding:18px;margin-bottom:14px;position:relative;box-shadow:var(--shsm);transition:border var(--tr)}
.card:hover{border-color:#3535aa}

/* DROP MENU */
.dots-btn{background:transparent;border:none;color:var(--text3);cursor:pointer;padding:5px 7px;border-radius:7px;font-size:14px;transition:all var(--tr)}
.dots-btn:hover{background:var(--bg5);color:var(--text)}
.drop-menu{display:none;position:absolute;background:var(--bg4);border:1.5px solid var(--border2);border-radius:11px;padding:5px;z-index:400;min-width:160px;box-shadow:var(--sh)}
.drop-menu.open{display:block;animation:slideDown 0.15s var(--tr) both}
.drop-menu button{display:flex;align-items:center;gap:9px;width:100%;background:none;border:none;color:var(--text2);text-align:left;padding:9px 12px;cursor:pointer;font-family:'Inter',sans-serif;font-size:13px;font-weight:500;border-radius:7px;transition:background var(--tr),color var(--tr)}
.drop-menu button:hover{background:var(--bg5);color:var(--text)}
.drop-menu button i{width:14px;text-align:center;font-size:12px;color:var(--text3)}
.drop-menu button.dng{color:rgba(231,76,60,0.85)}
.drop-menu button.dng i{color:rgba(231,76,60,0.6)}
.drop-menu button.dng:hover{background:rgba(231,76,60,0.1);color:var(--red)}
.menu-sep{height:1px;background:var(--border);margin:4px 8px}

/* CODE */
pre[class*="language-"]{border-radius:var(--rsm);overflow-x:auto;max-height:360px;margin-top:10px;background:#04040e !important;border:1px solid var(--border);font-size:12px}
code[class*="language-"]{font-family:'Fira Code',monospace;font-size:12px}
.CodeMirror{border-radius:var(--rsm);font-family:'Fira Code',monospace;font-size:12px;min-height:120px;background:#04040e !important;border:1.5px solid var(--border2)}
.CodeMirror-scroll{min-height:120px}

/* REACTIONS */
.rxns{display:flex;align-items:center;gap:6px;flex-wrap:wrap;margin-top:12px}
.rbtn{display:inline-flex;align-items:center;gap:5px;background:var(--bg5);border:1px solid var(--border2);border-radius:20px;color:var(--text3);padding:5px 12px;font-size:12px;font-family:'Inter',sans-serif;font-weight:500;cursor:pointer;transition:all var(--tr)}
.rbtn:hover{background:var(--bg4);color:var(--text2)}
.rbtn.lkd{background:rgba(46,204,113,0.13);border-color:rgba(46,204,113,0.3);color:var(--server)}
.rbtn.dlkd{background:rgba(231,76,60,0.13);border-color:rgba(231,76,60,0.3);color:var(--local)}
.stats{font-size:11px;color:var(--text3);margin-top:10px;display:flex;gap:14px;font-family:'Inter',sans-serif}

/* MODAL */
.modal{display:none;position:fixed;inset:0;background:rgba(3,3,12,0.88);backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px);justify-content:center;align-items:center;z-index:1000;padding:16px;overflow-y:auto}
.modal.show{display:flex}
.modal-box{background:linear-gradient(160deg,var(--bg3),var(--bg4));border:1.5px solid var(--border2);border-radius:20px;padding:24px;width:100%;max-width:400px;box-shadow:0 24px 70px rgba(0,0,0,0.85);position:relative;text-align:center}
.modal-box.wide{max-width:490px}
.modal-box.dng{border-color:var(--red)}
.modal-box.suc{border-color:var(--green)}
.modal.show .modal-box{animation:popIn 0.26s cubic-bezier(0.34,1.56,0.64,1) both}
.mt{font-family:'Fredoka One',cursive;font-size:19px;color:var(--text);margin-bottom:12px}
.ms{font-size:13px;color:var(--text2);font-family:'Inter',sans-serif;margin-bottom:12px;line-height:1.5}
.mbtns{display:flex;justify-content:center;gap:10px;margin-top:16px;flex-wrap:wrap}
.flbl{font-size:11px;color:var(--text3);font-family:'Inter',sans-serif;text-align:left;display:block;margin:8px 0 2px;letter-spacing:0.3px}
.modal input:not([type=range]):not([type=hidden]),.modal textarea{width:100%;padding:10px 14px;margin:4px 0;background:var(--bg5);border:1.5px solid var(--border2);color:var(--text);border-radius:var(--rsm);font-family:'Inter',sans-serif;font-size:13px;outline:none;transition:border var(--tr);resize:vertical}
.modal input:focus,.modal textarea:focus{border-color:var(--accent)}

/* BUTTONS */
.btn{padding:10px 20px;border:none;border-radius:var(--rsm);cursor:pointer;font-family:'Fredoka One',cursive;font-size:14px;transition:all var(--tr);display:inline-flex;align-items:center;justify-content:center;gap:7px}
.btn:hover{opacity:0.88;transform:translateY(-1px)}
.btn:active{transform:translateY(0)}
.btn.pr{background:linear-gradient(135deg,var(--accent),#2563eb);color:white;box-shadow:0 2px 14px rgba(91,142,240,0.35)}
.btn.se{background:var(--bg5);color:var(--text2);border:1.5px solid var(--border2)}
.btn.se:hover{color:var(--text)}
.btn.dng{background:linear-gradient(135deg,var(--red),#c0392b);color:white}
.btn.suc{background:linear-gradient(135deg,var(--green),#1e8449);color:white}
.btn.full{width:100%}
.btn-login{background:linear-gradient(135deg,var(--accent),#2563eb);color:white;border:none;border-radius:var(--rsm);padding:9px 18px;font-family:'Fredoka One',cursive;font-size:14px;cursor:pointer;transition:all var(--tr)}
.btn-login:hover{opacity:0.88;transform:translateY(-1px)}

/* AUTH MODAL */
.auth-methods{display:flex;flex-direction:column;gap:9px;margin:12px 0}
.apbtn{display:flex;align-items:center;gap:12px;width:100%;padding:12px 16px;background:var(--bg5);border:1.5px solid var(--border2);border-radius:var(--rsm);color:var(--text);cursor:pointer;font-family:'Inter',sans-serif;font-size:14px;font-weight:500;transition:all var(--tr);text-align:left}
.apbtn:hover{background:var(--bg4);border-color:var(--accent);transform:translateY(-1px);box-shadow:0 4px 14px rgba(0,0,0,0.35)}
.apbtn img{width:20px;height:20px;flex-shrink:0}
.apbtn .an{flex:1}
.apbtn .aa{color:var(--text3);font-size:11px}
.adiv{display:flex;align-items:center;gap:10px;margin:12px 0;color:var(--text3);font-size:12px;font-family:'Inter',sans-serif}
.adiv::before,.adiv::after{content:'';flex:1;height:1px;background:var(--border)}
.atog{font-family:'Inter',sans-serif;font-size:13px;color:var(--text3);margin-top:12px;cursor:pointer}
.atog span{color:var(--accent);text-decoration:underline}
.pfp-row{display:flex;justify-content:center;gap:8px;margin:8px 0;flex-wrap:wrap}
.pfp-opt{width:40px;height:40px;border-radius:50%;border:3px solid transparent;cursor:pointer;transition:all var(--tr)}
.pfp-opt:hover{transform:scale(1.1)}
.pfp-opt.sel{border-color:var(--accent);transform:scale(1.12);box-shadow:0 0 12px rgba(91,142,240,0.4)}

/* TOAST */
#toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:var(--green);color:white;padding:11px 22px;border-radius:12px;display:none;z-index:5000;font-family:'Fredoka One',cursive;font-size:14px;box-shadow:0 4px 20px rgba(0,0,0,0.5);white-space:nowrap;pointer-events:none}
#toast.show{display:block;animation:popIn 0.2s var(--tr) both}
#toast.err{background:var(--red)}
#toast.wrn{background:#f39c12;color:#111}
#toast.inf{background:var(--accent)}

/* FOLDER CARD */
.fcard{background:linear-gradient(160deg,#090920,#0e0e2c);border:1.5px solid #1c1c55;border-radius:18px;padding:18px;margin-bottom:14px;position:relative;box-shadow:var(--shsm);transition:border var(--tr),box-shadow var(--tr)}
.fcard:hover{border-color:#2c2c88;box-shadow:0 8px 30px rgba(0,0,0,0.5)}
.fhd{display:flex;align-items:center;gap:8px;margin-bottom:6px}
.fchev{background:none;border:none;cursor:pointer;color:#2a2a88;font-size:13px;padding:5px;border-radius:6px;transition:all var(--tr);display:flex;align-items:center;justify-content:center;flex-shrink:0}
.fchev:hover{background:var(--bg5);color:#7777cc}
.fchev.open{transform:rotate(90deg)}
.ftitle{font-family:'Fredoka One',cursive;font-size:18px;color:#d0d0ff;flex:1;line-height:1.2}
.fdesc{color:#3a3a7a;font-family:'Inter',sans-serif;font-size:12px;margin-bottom:8px;line-height:1.5}
.fmeta{display:flex;align-items:center;gap:8px;flex-wrap:wrap;font-family:'Inter',sans-serif;font-size:11px;color:#2a2a6a;margin-bottom:6px}
.frxns{display:flex;align-items:center;gap:6px;margin:8px 0 4px}

/* FOLDER COLLAPSE */
.fbody{overflow:hidden;max-height:0;transition:max-height 0.44s cubic-bezier(0.4,0,0.2,1),opacity 0.3s ease;opacity:0}
.fbody.open{max-height:99999px;opacity:1}
.fadd-row{display:flex;gap:8px;margin-top:12px}
.sadd-btn{flex:1;display:flex;align-items:center;justify-content:center;gap:6px;background:#0c0c30;color:#4444aa;border:1px solid #1a1a55;border-radius:var(--rsm);padding:9px;font-family:'Fredoka One',cursive;font-size:13px;cursor:pointer;transition:all var(--tr)}
.sadd-btn:hover{background:#14144a;color:#8888ff;border-color:var(--accent)}

/* SCRIPT CARD (inside folder) */
.scard{background:#060618;border:1px solid #111148;border-radius:12px;padding:14px;margin-bottom:10px;position:relative;transition:border var(--tr)}
.scard:hover{border-color:#202070}
.sbadge{display:inline-flex;align-items:center;gap:5px;font-size:10px;font-family:'Fira Code',monospace;font-weight:600;padding:3px 9px;border-radius:20px;margin-bottom:8px}
.sbadge.srv{background:rgba(46,204,113,0.1);color:var(--server);border:1px solid rgba(46,204,113,0.22)}
.sbadge.lcl{background:rgba(231,76,60,0.1);color:var(--local);border:1px solid rgba(231,76,60,0.22)}
.stitle{font-family:'Fredoka One',cursive;font-size:15px;color:var(--text);margin-bottom:3px}
.sdesc{font-family:'Inter',sans-serif;font-size:12px;color:#33336a;margin-bottom:7px}
.sdots{position:absolute;top:10px;right:10px;background:transparent;border:none;color:#22225a;cursor:pointer;padding:4px 6px;border-radius:6px;font-size:13px;transition:all var(--tr)}
.sdots:hover{background:var(--bg5);color:var(--text2)}

/* TYPE CHOOSER */
.trow{display:flex;gap:10px;margin:12px 0}
.tbtn{flex:1;display:flex;flex-direction:column;align-items:center;gap:5px;background:#0c0c30;color:#4444aa;border:2px solid #1a1a55;border-radius:12px;padding:16px 8px;font-family:'Fredoka One',cursive;font-size:15px;cursor:pointer;transition:all var(--tr)}
.tbtn:hover{background:#141448;color:#aaaaff;border-color:var(--accent);box-shadow:0 0 14px rgba(91,142,240,0.18)}
.tbtn small{font-family:'Fira Code',monospace;font-size:9px;opacity:0.5}

/* ADMIN */
.atabs{display:flex;gap:8px;margin-bottom:14px}
.atab{flex:1;background:var(--bg5);border:1px solid var(--border2);color:var(--text3);padding:9px;border-radius:var(--rsm);cursor:pointer;font-family:'Fredoka One',cursive;font-size:13px;transition:all var(--tr)}
.atab.act{background:var(--accent);color:white;border-color:var(--accent)}
.atbl{width:100%;border-collapse:collapse;font-family:'Inter',sans-serif;color:var(--text2);font-size:12px}
.atbl th{background:var(--bg5);color:var(--text3);padding:8px 6px;text-align:left;font-weight:600}
.atbl td{border-bottom:1px solid var(--border);padding:8px 6px;vertical-align:top}
.atbl tr:last-child td{border-bottom:none}
.rprev{background:var(--bg2);padding:4px 7px;border-radius:5px;font-size:10px;color:var(--text3);margin-top:3px;border:1px solid var(--border);font-family:'Fira Code',monospace;word-break:break-all}
.rbdg{display:inline-block;font-size:10px;padding:1px 7px;border-radius:20px;font-family:'Fredoka One',cursive}
.rbdg.bf{background:rgba(91,142,240,0.13);color:var(--accent);border:1px solid rgba(91,142,240,0.22)}
.rbdg.bs{background:rgba(123,110,246,0.13);color:#9b8ef6;border:1px solid rgba(123,110,246,0.22)}
.rbdg.bc{background:rgba(231,126,34,0.13);color:#e67e22;border:1px solid rgba(231,126,34,0.22)}

/* USER PROFILE MODAL */
.up-av{width:80px;height:80px;border-radius:50%;border:3px solid var(--accent);margin-bottom:10px;box-shadow:0 0 24px rgba(91,142,240,0.3)}
.up-nm{font-family:'Fredoka One',cursive;font-size:20px;color:var(--text);margin-bottom:4px}
.up-join{font-family:'Inter',sans-serif;font-size:11px;color:var(--text3);margin-bottom:12px}
.up-bel{background:linear-gradient(135deg,rgba(91,142,240,0.08),rgba(123,110,246,0.08));border:1px solid rgba(91,142,240,0.18);border-radius:var(--rsm);padding:10px 12px;margin:10px 0;text-align:left}
.up-bel-lbl{font-size:10px;color:var(--text3);font-family:'Inter',sans-serif;text-transform:uppercase;letter-spacing:1px;margin-bottom:4px}
.up-bel-nm{font-family:'Fredoka One',cursive;font-size:14px;color:var(--gold)}
.uf-item{background:var(--bg5);border:1px solid var(--border2);border-radius:var(--rsm);padding:10px 12px;margin-bottom:6px;text-align:left}
.uf-nm{font-family:'Fredoka One',cursive;font-size:14px;color:#c0c0ff;margin-bottom:3px}
.uf-ct{font-family:'Inter',sans-serif;font-size:11px;color:var(--text3)}

/* SHARE */
.shr-row{display:flex;gap:8px;align-items:center}
.shr-row input{flex:1;color:var(--accent);font-family:'Fira Code',monospace;font-size:11px;background:#04040e;cursor:pointer}

/* EMPTY */
.empty{text-align:center;padding:40px 20px;color:var(--text3);font-family:'Inter',sans-serif}
.empty i{font-size:36px;margin-bottom:12px;display:block;opacity:0.35}
.empty p{font-size:14px;line-height:1.6}

/* SCROLLBAR */
::-webkit-scrollbar{width:4px;height:4px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--border2);border-radius:10px}
::-webkit-scrollbar-thumb:hover{background:var(--accent)}

/* PFP tiny */
.pfp-xs{width:16px;height:16px;border-radius:50%;object-fit:cover}
</style>
</head>
<body>
<div id="toast"></div>
<div class="wrap">
  <!-- HEADER -->
  <div class="header au">
    <div class="logo">
      <span class="logo-lua">Lua</span><span class="logo-hub">Hub</span><span class="logo-dot"></span>
    </div>
    <div class="hact" id="hact"></div>
    <div class="pp" id="pp">
      <img src="" id="pp-av" class="pp-av">
      <div class="pp-nm" id="pp-nm" onclick="openChgName()"></div>
      <span class="pp-own" id="pp-own" style="display:none">&#x1F451; OWNER</span>
      <span class="pp-wrn" id="pp-wrn" style="display:none">&#x26A0; Warning Level: 0</span>
      <div class="pp-bl-lbl">&#x1F3C6; Beloved Script</div>
      <div class="pp-bl" id="pp-bl">No scripts yet!</div>
      <button class="pp-out" onclick="logout()">Logout</button>
    </div>
  </div>

  <!-- SEARCH -->
  <div class="sw au1">
    <input type="text" id="sq" class="si" placeholder="&#128269;  Search..." oninput="renderAll()">
    <div class="csel-w" id="filterW">
      <div class="csel" id="cselBtn" onclick="tglCsel()">
        <span id="cselLbl" style="display:flex;align-items:center;gap:5px"><i class="fas fa-folder" style="font-size:10px"></i>Folders</span>
        <i class="fas fa-chevron-down arr"></i>
      </div>
      <div class="csel-opts" id="cselOpts">
        <div class="csel-opt act" data-v="folder" onclick="setFilter('folder',this)"><i class="fas fa-folder"></i>Folders</div>
        <div class="csel-opt" data-v="creator" onclick="setFilter('creator',this)"><i class="fas fa-user"></i>Creator</div>
        <div class="csel-sep" style="height:1px;background:var(--border);margin:3px 10px"></div>
        <div class="csel-opt" data-v="server" onclick="setFilter('server',this)"><i class="fas fa-server"></i>ServerScript only</div>
        <div class="csel-opt" data-v="local" onclick="setFilter('local',this)"><i class="fas fa-laptop"></i>LocalScript only</div>
      </div>
    </div>
  </div>

  <div id="feed"></div>
</div>

<!-- AUTH MODAL -->
<div class="modal" id="authModal">
  <div class="modal-box">
    <div class="mt" id="authTitle">Welcome to LuaHub</div>
    <div class="ms" id="authSub">Sign in to share your Roblox scripts.</div>
    <div class="auth-methods" id="authProviders">
      <button class="apbtn" onclick="oauthLogin('github')">
        <img src="https://api.iconify.design/mdi:github.svg?color=%23ffffff" alt="">
        <span class="an">Continue with GitHub</span><i class="fas fa-arrow-right aa"></i>
      </button>
      <button class="apbtn" onclick="oauthLogin('google')">
        <img src="https://api.iconify.design/flat-color-icons:google.svg" alt="">
        <span class="an">Continue with Google</span><i class="fas fa-arrow-right aa"></i>
      </button>
      <button class="apbtn" onclick="oauthLogin('apple')">
        <img src="https://api.iconify.design/ic:baseline-apple.svg?color=%23ffffff" alt="">
        <span class="an">Continue with Apple</span><i class="fas fa-arrow-right aa"></i>
      </button>
    </div>
    <div class="adiv">or use username &amp; password</div>
    <input type="text" id="authUser" placeholder="Username">
    <input type="password" id="authPass" placeholder="Password">
    <div id="authPfpRow" style="display:none">
      <div class="flbl">Choose Avatar</div>
      <div class="pfp-row" id="authPfpOptions">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=1" class="pfp-opt sel" onclick="selPfp(this,'seed=1')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=2" class="pfp-opt" onclick="selPfp(this,'seed=2')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=3" class="pfp-opt" onclick="selPfp(this,'seed=3')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=4" class="pfp-opt" onclick="selPfp(this,'seed=4')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=5" class="pfp-opt" onclick="selPfp(this,'seed=5')">
      </div>
    </div>
    <div class="mbtns">
      <button class="btn pr" id="authSubmit" onclick="doAuth()">Login</button>
      <button class="btn se" onclick="closeM('authModal')">Cancel</button>
    </div>
    <div class="atog" id="authTog" onclick="tglAuth()">Don't have an account? <span>Sign Up</span></div>
  </div>
</div>

<!-- OAUTH MODAL -->
<div class="modal" id="oauthModal">
  <div class="modal-box" style="max-width:340px">
    <div style="font-size:38px;margin-bottom:10px" id="oiIcon">&#x1F510;</div>
    <div class="mt" id="oiTitle">Connecting...</div>
    <div class="ms" id="oiSub">Completing sign-in...</div>
    <div id="oiSpinner" style="margin:18px auto;width:36px;height:36px;border:3px solid var(--border2);border-top-color:var(--accent);border-radius:50%;animation:spin 0.8s linear infinite"></div>
    <div id="oiStep2" style="display:none">
      <input type="text" id="oiUser" placeholder="Choose a username">
      <div class="flbl">Choose Avatar</div>
      <div class="pfp-row" id="oiPfpRow">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=10" class="pfp-opt sel" onclick="selPfp(this,'seed=10')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=11" class="pfp-opt" onclick="selPfp(this,'seed=11')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=12" class="pfp-opt" onclick="selPfp(this,'seed=12')">
        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=13" class="pfp-opt" onclick="selPfp(this,'seed=13')">
      </div>
      <div class="mbtns"><button class="btn pr" onclick="finishOauth()">Create Account</button></div>
    </div>
  </div>
</div>

<!-- CHANGE NAME -->
<div class="modal" id="chgNameModal">
  <div class="modal-box">
    <div class="mt">&#x270F;&#xFE0F; Change Username</div>
    <span class="flbl">New Username</span>
    <input type="text" id="newName" placeholder="Enter new username">
    <div class="mbtns">
      <button class="btn pr" onclick="doChgName()">Save</button>
      <button class="btn se" onclick="closeM('chgNameModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- CREATE FOLDER -->
<div class="modal" id="newFolderModal">
  <div class="modal-box suc">
    <div class="mt">&#x1F4C1; Create Folder</div>
    <span class="flbl">Folder Name *</span>
    <input type="text" id="nfName" placeholder="My Awesome Scripts">
    <span class="flbl">Description</span>
    <textarea id="nfDesc" rows="2" placeholder="What's in here?"></textarea>
    <div class="mbtns">
      <button class="btn suc" onclick="doCreateFolder()">Create Folder</button>
      <button class="btn se" onclick="closeM('newFolderModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- EDIT FOLDER -->
<div class="modal" id="editFolderModal">
  <div class="modal-box">
    <div class="mt">&#x270F;&#xFE0F; Edit Folder</div>
    <input type="hidden" id="efId">
    <span class="flbl">Name *</span><input type="text" id="efName">
    <span class="flbl">Description</span><textarea id="efDesc" rows="2"></textarea>
    <div class="mbtns">
      <button class="btn pr" onclick="doEditFolder()">Save</button>
      <button class="btn se" onclick="closeM('editFolderModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- SCRIPT MODAL -->
<div class="modal" id="scriptModal">
  <div class="modal-box wide">
    <div class="mt" id="smTitle">Add Script</div>
    <input type="hidden" id="smFid"><input type="hidden" id="smSid"><input type="hidden" id="smType">
    <div id="smS1">
      <div class="ms">Choose the script type:</div>
      <div class="trow">
        <button class="tbtn" onclick="smChoose('ServerScript')">&#x1F5A5;&#xFE0F; ServerScript<small>Runs on Server</small></button>
        <button class="tbtn" onclick="smChoose('LocalScript')">&#x1F4BB; LocalScript<small>Runs on Client</small></button>
      </div>
    </div>
    <div id="smS2" style="display:none">
      <div id="smBadge" style="margin-bottom:10px"></div>
      <span class="flbl">Title *</span><input type="text" id="smSTitle" placeholder="Script Title">
      <span class="flbl">Description</span><textarea id="smSDesc" rows="2" placeholder="Optional"></textarea>
      <span class="flbl">Code *</span><textarea id="smSCode"></textarea>
      <div class="mbtns">
        <button class="btn pr" onclick="doSaveScript()">Save Script</button>
        <button class="btn se" id="smBack" onclick="smGoBack()">&#x2190; Back</button>
        <button class="btn se" onclick="closeScriptModal()">Cancel</button>
      </div>
    </div>
  </div>
</div>

<!-- DELETE -->
<div class="modal" id="delModal">
  <div class="modal-box dng">
    <div style="font-size:36px;margin-bottom:10px">&#x1F5D1;&#xFE0F;</div>
    <div class="mt" id="delTitle">Delete?</div>
    <div class="ms" id="delSub">This cannot be undone.</div>
    <div class="mbtns">
      <button class="btn dng" id="delOk">Yes, Delete</button>
      <button class="btn se" onclick="closeM('delModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- REPORT -->
<div class="modal" id="repModal">
  <div class="modal-box dng">
    <div style="font-size:32px;margin-bottom:8px">&#x1F6A9;</div>
    <div class="mt" id="repTitle">Report</div>
    <div class="ms" id="repCtx" style="background:var(--bg5);padding:8px 10px;border-radius:8px;border:1px solid var(--border);text-align:left"></div>
    <span class="flbl">Reason *</span>
    <textarea id="repReason" rows="3" placeholder="Describe the issue clearly..."></textarea>
    <div class="mbtns">
      <button class="btn dng" id="repOk">Submit Report</button>
      <button class="btn se" onclick="closeM('repModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- BAN -->
<div class="modal" id="banModal">
  <div class="modal-box dng">
    <div style="font-size:32px;margin-bottom:8px">&#x1F6AB;</div>
    <div class="mt">Ban User</div>
    <div class="ms" id="banCtx"></div>
    <span class="flbl">Duration (days)</span>
    <input type="number" id="banDays" placeholder="e.g. 7" min="1">
    <span class="flbl">Reason</span>
    <textarea id="banReason" rows="2" placeholder="State the reason..."></textarea>
    <div class="mbtns">
      <button class="btn dng" id="banOk">Ban User</button>
      <button class="btn se" onclick="closeM('banModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- ADMIN -->
<div class="modal" id="adminModal">
  <div class="modal-box wide dng" style="max-width:520px;text-align:left">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;border-bottom:1px solid var(--border);padding-bottom:12px">
      <div class="mt" style="margin:0">&#x1F6E1;&#xFE0F; Admin Panel</div>
      <button class="btn se" style="padding:5px 12px" onclick="closeM('adminModal')">&#x2715;</button>
    </div>
    <div style="font-size:12px;color:var(--text3);font-family:'Inter',sans-serif;margin-bottom:12px">Welcome, Administrator Tasin Redwan.</div>
    <div class="atabs">
      <button class="atab act" id="tabRep" onclick="adminTab('reports')">&#x1F4CB; Reports</button>
      <button class="atab" id="tabBan" onclick="adminTab('bans')">&#x1F6AB; Banned</button>
    </div>
    <div id="adminArea"></div>
  </div>
</div>

<!-- COMMENT MODAL -->
<div class="modal" id="comModal">
  <div class="modal-box">
    <div class="mt" id="comTitle">Comment</div>
    <textarea id="comText" rows="3" placeholder=""></textarea>
    <div class="mbtns">
      <button class="btn pr" id="comOk">Submit</button>
      <button class="btn se" onclick="closeM('comModal')">Cancel</button>
    </div>
  </div>
</div>

<!-- SHARE -->
<div class="modal" id="shrModal">
  <div class="modal-box">
    <div style="font-size:32px;margin-bottom:8px">&#x1F517;</div>
    <div class="mt">Share Script</div>
    <div class="ms">Works across all devices.</div>
    <div class="shr-row">
      <input type="text" id="shrLink" readonly onclick="this.select()">
      <button class="btn pr" style="white-space:nowrap;padding:10px 14px" onclick="copyShrLink()">Copy</button>
    </div>
    <div class="mbtns"><button class="btn se" onclick="closeM('shrModal')">Close</button></div>
  </div>
</div>

<!-- USER PROFILE MODAL -->
<div class="modal" id="userModal">
  <div class="modal-box wide" style="text-align:center">
    <button class="btn se" style="position:absolute;top:14px;right:14px;padding:5px 10px" onclick="closeM('userModal')">&#x2715;</button>
    <img src="" id="um-av" class="up-av">
    <div class="up-nm" id="um-nm"></div>
    <div id="um-own" style="display:none;color:var(--gold);font-size:12px;margin-bottom:4px;font-family:'Inter',sans-serif;font-weight:600">&#x1F451; OWNER</div>
    <div class="up-join" id="um-join"></div>
    <div class="up-bel" id="um-bel" style="display:none">
      <div class="up-bel-lbl">&#x1F3C6; Beloved Script</div>
      <div class="up-bel-nm" id="um-bel-nm"></div>
    </div>
    <div id="um-folders" style="text-align:left;margin-top:12px;max-height:280px;overflow-y:auto"></div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/lua/lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.js"></script>
<script>
// === DATA ===
const LS = k=>JSON.parse(localStorage.getItem('lh_'+k)||'null');
const WS = (k,v)=>localStorage.setItem('lh_'+k,JSON.stringify(v));
let D = {
  users:   LS('users')   ||{},
  folders: LS('folders') ||[],
  banned:  LS('banned')  ||[],
  warnings:LS('warnings')||{},
  reports: LS('reports') ||[],
  unrep:   LS('unrep')   ||[],
};
let curUser = LS('session');
let openF={}, selPfpSeed='seed=1', authMode='login', admTab='reports', _sme=null, _fv='folder';

if(curUser&&D.banned.includes(curUser.username)){curUser=null;localStorage.removeItem('lh_session');}

D.folders.forEach(f=>{
  if(!f.userReactions) f.userReactions={};
  if(!f.scripts) f.scripts=[];
  if(!f.createdAt) f.createdAt=Date.now();
  f.scripts.forEach(fs=>{if(!fs.userReactions)fs.userReactions={};if(!fs.userRatings)fs.userRatings={};});
});
WS('folders',D.folders);

function sv(k){WS(k,D[k]);}
function esc(s){return String(s||'').replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]));}
function uid(){return '_'+Math.random().toString(36).slice(2)+Date.now().toString(36);}
function fmtD(ts){return new Date(ts).toLocaleDateString('en-US',{month:'long',day:'numeric',year:'numeric'});}
function isAdmin(){return curUser&&curUser.username==='Tasin Redwan';}

// === TOAST ===
function toast(msg,type,dur=2800){
  const el=document.getElementById('toast');
  el.textContent=msg;el.className='show'+(type?' '+type:'');
  clearTimeout(el._t);el._t=setTimeout(()=>el.className='',dur);
}

// === MODAL ===
function openM(id){document.getElementById(id).classList.add('show');}
function closeM(id){document.getElementById(id).classList.remove('show');}

// === GLOBAL CLICKS ===
document.addEventListener('click',e=>{
  document.querySelectorAll('.drop-menu.open').forEach(m=>{if(!m.closest('.fcard,.card')?.contains(e.target)&&!m.contains(e.target))m.classList.remove('open');});
  if(!document.getElementById('pp').contains(e.target)&&!document.getElementById('hact').contains(e.target))
    document.getElementById('pp').classList.remove('show');
  if(!document.getElementById('filterW').contains(e.target)){
    document.getElementById('cselOpts').classList.remove('open');
    document.getElementById('cselBtn').classList.remove('open');
  }
});

// === SEARCH FILTER ===
function tglCsel(){document.getElementById('cselOpts').classList.toggle('open');document.getElementById('cselBtn').classList.toggle('open');}
function setFilter(v,el){
  _fv=v;
  document.querySelectorAll('.csel-opt').forEach(o=>o.classList.remove('act'));
  el.classList.add('act');
  document.getElementById('cselLbl').innerHTML=el.innerHTML;
  document.getElementById('cselOpts').classList.remove('open');
  document.getElementById('cselBtn').classList.remove('open');
  renderAll();
}

// === PROFILE PANEL ===
function tglPP(e){e.stopPropagation();document.getElementById('pp').classList.toggle('show');updPP();}
function updPP(){
  if(!curUser)return;
  document.getElementById('pp-av').src=curUser.pfp;
  document.getElementById('pp-nm').textContent=curUser.username;
  const isOw=curUser.username==='Tasin Redwan';
  document.getElementById('pp-own').style.display=isOw?'block':'none';
  const lvl=D.warnings[curUser.username]||0;
  const wEl=document.getElementById('pp-wrn');
  if(!isOw&&lvl>0){wEl.style.display='inline-block';wEl.textContent=`\u26A0 Warning Level: ${lvl}`;}
  else wEl.style.display='none';
  const bel=getBeloved(curUser.username);
  document.getElementById('pp-bl').textContent=bel?bel.title:'No scripts yet!';
}
function getBeloved(u){
  let best=null,bs=-1;
  D.folders.forEach(f=>(f.scripts||[]).filter(fs=>fs.author===u).forEach(fs=>{
    let sc=Object.values(fs.userReactions||{}).filter(r=>r==='like').length+Object.values(fs.userRatings||{}).reduce((a,b)=>a+b,0);
    if(sc>bs){bs=sc;best=fs;}
  }));
  return best;
}

// === HEADER ===
function renderHdr(){
  const a=document.getElementById('hact');
  if(curUser){
    const bc=D.banned.length;
    a.innerHTML=`
      ${isAdmin()?`<button class="ibtn admin-btn" onclick="openAdmin()" title="Admin"><i class="fas fa-shield-alt"></i></button>`:''}
      <button class="ibtn add-btn" onclick="openNewFolder()" title="Create Folder">+</button>
      <div class="pfp-wrap">
        <img src="${curUser.pfp}" class="pfp-hdr" onclick="tglPP(event)" title="${esc(curUser.username)}">
        ${isAdmin()&&bc>0?`<div class="ban-badge">${bc}</div>`:''}
      </div>`;
    chkUnban();
  } else {
    a.innerHTML=`<button class="btn-login" onclick="openAuthM()">Login</button>`;
  }
}
function chkUnban(){
  if(!curUser)return;
  if(D.unrep.includes(curUser.username)){
    toast("You've been unbanned — follow the guidelines!",'wrn',5000);
    D.unrep=D.unrep.filter(u=>u!==curUser.username);WS('unrep',D.unrep);
  }
}

// === RENDER ALL ===
function renderAll(){
  const feed=document.getElementById('feed');
  const q=document.getElementById('sq').value.toLowerCase().trim();
  feed.innerHTML='';
  let show=[...D.folders];
  if(_fv==='folder'){if(q)show=show.filter(f=>f.name.toLowerCase().includes(q));}
  else if(_fv==='creator'){if(q)show=show.filter(f=>(f.author||'').toLowerCase().includes(q));}
  else if(_fv==='server'){show=show.filter(f=>(f.scripts||[]).some(s=>s.scriptType==='ServerScript'&&(!q||s.title.toLowerCase().includes(q)||f.name.toLowerCase().includes(q))));}
  else if(_fv==='local'){show=show.filter(f=>(f.scripts||[]).some(s=>s.scriptType==='LocalScript'&&(!q||s.title.toLowerCase().includes(q)||f.name.toLowerCase().includes(q))));}
  if(!show.length){
    feed.innerHTML=`<div class="empty"><i class="fas fa-folder-open"></i><p>No folders found.<br>${curUser?'Click <b>+</b> to create your first folder!':'Login to create and share folders.'}</p></div>`;
    return;
  }
  show.forEach(f=>feed.appendChild(buildFC(f)));
  Prism.highlightAll();
}

// === FOLDER CARD ===
function buildFC(f){
  const el=document.createElement('div');
  el.className='fcard au';
  const mine=curUser&&curUser.username===f.author;
  const admin=isAdmin();
  const isOpen=!!openF[f.id];
  const likes=Object.values(f.userReactions||{}).filter(r=>r==='like').length;
  const dlikes=Object.values(f.userReactions||{}).filter(r=>r==='dislike').length;
  const myR=curUser?(f.userReactions||{})[curUser.username]:null;
  const cnt=(f.scripts||[]).length;
  let scHtml='';
  if(_fv==='server'||_fv==='local'){
    const tp=_fv==='server'?'ServerScript':'LocalScript';
    const flt=(f.scripts||[]).filter(s=>s.scriptType===tp);
    scHtml=flt.length?flt.map(fs=>buildSC(fs,f.id,mine,admin)).join(''):`<p style="color:var(--text3);font-family:'Inter',sans-serif;font-size:12px;padding:6px 0;margin:0">No ${tp}s here.</p>`;
  } else {
    scHtml=cnt?(f.scripts||[]).map(fs=>buildSC(fs,f.id,mine,admin)).join(''):`<p style="color:var(--text3);font-family:'Inter',sans-serif;font-size:12px;padding:6px 0;margin:0">No scripts yet${mine?' — add one below!':'.'}</p>`;
  }
  el.innerHTML=`
    <div class="fhd">
      <button class="fchev${isOpen?' open':''}" onclick="tglF('${f.id}',this)"><i class="fas fa-chevron-right"></i></button>
      <span style="font-size:20px">&#x1F4C1;</span>
      <span class="ftitle">${esc(f.name)}</span>
      <div style="position:relative;margin-left:auto">
        <button class="dots-btn" onclick="tglM(event,'fdm-${f.id}')"><i class="fas fa-ellipsis-v"></i></button>
        <div class="drop-menu" id="fdm-${f.id}" style="position:absolute;right:0;top:100%;min-width:170px">
          ${mine?`<button onclick="openEditFolder('${f.id}')"><i class="fas fa-edit"></i>Edit Folder</button><div class="menu-sep"></div><button class="dng" onclick="openDel('folder','${f.id}',null)"><i class="fas fa-trash"></i>Delete Folder</button>`
          :`<button class="dng" onclick="openRep('folder','${f.id}',null)"><i class="fas fa-flag"></i>Report</button>`}
          ${admin&&!mine?`<button class="dng" onclick="openBan('${esc(f.author)}',null,null,null)"><i class="fas fa-ban"></i>Ban Creator</button>`:''}
        </div>
      </div>
    </div>
    ${f.desc?`<div class="fdesc">${esc(f.desc)}</div>`:''}
    <div class="fmeta">
      <img src="${f.authorPfp||''}" class="pfp-xs" style="cursor:pointer;border:1px solid var(--border2)" onclick="openUserProfile('${esc(f.author)}')">
      <span style="cursor:pointer;color:var(--accent);font-weight:500" onclick="openUserProfile('${esc(f.author)}')">${esc(f.author)}${f.author==='Tasin Redwan'?' &#x1F451;':''}</span>
      <span>&#x2022;</span><span>${cnt} script${cnt!==1?'s':''}</span>
      <span>&#x2022;</span><span>${fmtD(f.createdAt||Date.now())}</span>
    </div>
    <div class="frxns">
      <button class="rbtn${myR==='like'?' lkd':''}" onclick="rxnF('${f.id}','like')"><i class="fas fa-thumbs-up"></i> ${likes}</button>
      <button class="rbtn${myR==='dislike'?' dlkd':''}" onclick="rxnF('${f.id}','dislike')"><i class="fas fa-thumbs-down"></i> ${dlikes}</button>
    </div>
    <div class="fbody${isOpen?' open':''}" id="fb-${f.id}">
      <div id="fi-${f.id}">${scHtml}</div>
      ${mine?`<div class="fadd-row">
        <button class="sadd-btn" onclick="openSM('${f.id}','ServerScript')"><i class="fas fa-plus"></i>ServerScript</button>
        <button class="sadd-btn" onclick="openSM('${f.id}','LocalScript')"><i class="fas fa-plus"></i>LocalScript</button>
      </div>`:''}
    </div>`;
  return el;
}

function tglF(id,btn){
  openF[id]=!openF[id];
  btn.classList.toggle('open',openF[id]);
  document.getElementById('fb-'+id).classList.toggle('open',openF[id]);
}

// === SCRIPT CARD ===
function buildSC(fs,fid,mine,admin){
  const bc=fs.scriptType==='ServerScript'?'srv':'lcl';
  const notMine=!mine;
  return `<div class="scard" id="sc-${fs.id}">
    <span class="sbadge ${bc}">${fs.scriptType==='ServerScript'?'&#x1F5A5;&#xFE0F; ServerScript':'&#x1F4BB; LocalScript'}</span>
    <div style="position:relative;float:right;display:inline-block">
      <button class="sdots" onclick="tglM(event,'sdm-${fs.id}')"><i class="fas fa-ellipsis-v"></i></button>
      <div class="drop-menu" id="sdm-${fs.id}" style="position:absolute;right:0;top:100%;min-width:155px">
        <button onclick="copyCode('${fid}','${fs.id}')"><i class="fas fa-copy"></i>Copy Code</button>
        <button onclick="openShr('${fid}','${fs.id}')"><i class="fas fa-share-alt"></i>Share</button>
        ${mine?`<div class="menu-sep"></div><button onclick="openEditSM('${fid}','${fs.id}')"><i class="fas fa-edit"></i>Edit</button><button class="dng" onclick="openDel('script','${fid}','${fs.id}')"><i class="fas fa-trash"></i>Delete</button>`:''}
        ${notMine?`<div class="menu-sep"></div><button class="dng" onclick="openRep('script','${fid}','${fs.id}')"><i class="fas fa-flag"></i>Report</button>`:''}
        ${admin&&notMine?`<button class="dng" onclick="openBan('${esc(fs.author)}',null,null,null)"><i class="fas fa-ban"></i>Ban Author</button>`:''}
      </div>
    </div>
    <div class="stitle">${esc(fs.title)}</div>
    ${fs.desc?`<div class="sdesc">${esc(fs.desc)}</div>`:''}
    <pre class="line-numbers"><code class="language-lua">${esc(fs.code)}</code></pre>
  </div>`;
}

// === THREE DOT ===
function tglM(e,id){
  e.stopPropagation();
  const m=document.getElementById(id);
  document.querySelectorAll('.drop-menu.open').forEach(x=>{if(x!==m)x.classList.remove('open');});
  m&&m.classList.toggle('open');
}

// === FOLDER REACTIONS ===
function rxnF(fid,type){
  if(!curUser)return toast("Login first!",'err');
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  if(!f.userReactions)f.userReactions={};
  if(f.userReactions[curUser.username]===type)delete f.userReactions[curUser.username];
  else f.userReactions[curUser.username]=type;
  sv('folders');renderAll();
}

// === COPY / SHARE ===
function copyCode(fid,sid){
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  const fs=f.scripts.find(s=>s.id===sid);if(!fs)return;
  navigator.clipboard.writeText(fs.code).then(()=>toast("Code copied!"));
}
function openShr(fid,sid){
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  const fs=f.scripts.find(s=>s.id===sid);if(!fs)return;
  const enc=btoa(unescape(encodeURIComponent(JSON.stringify({t:fs.title,d:fs.desc,c:fs.code,a:fs.author,p:fs.authorPfp,id:fs.id,st:fs.scriptType}))));
  document.getElementById('shrLink').value=`${location.origin}${location.pathname}?share=${enc}`;
  openM('shrModal');
}
function copyShrLink(){navigator.clipboard.writeText(document.getElementById('shrLink').value).then(()=>toast("Link copied!"));}
function chkShare(){
  const p=new URLSearchParams(location.search);
  const s=p.get('share');if(!s)return;
  try{
    const o=JSON.parse(decodeURIComponent(escape(atob(s))));
    let imp=D.folders.find(f=>f.id==='f_imp');
    if(!imp){imp={id:'f_imp',name:'Imported Scripts',desc:'Shared from other users',author:'System',authorPfp:'https://api.dicebear.com/7.x/pixel-art/svg?seed=sys',scripts:[],userReactions:{},createdAt:Date.now()};D.folders.unshift(imp);}
    if(!imp.scripts.some(x=>x.id===o.id)){imp.scripts.push({id:o.id,title:o.t,desc:o.d||'',code:o.c,scriptType:o.st||'ServerScript',author:o.a,authorPfp:o.p,userReactions:{},userRatings:{}});openF['f_imp']=true;sv('folders');toast("Script imported!",'inf');}
  }catch(e){}
  history.replaceState({},'',location.pathname);
}

// === CREATE / EDIT FOLDER ===
function openNewFolder(){if(!curUser)return toast("Login first!",'err');document.getElementById('nfName').value='';document.getElementById('nfDesc').value='';openM('newFolderModal');}
function doCreateFolder(){
  const nm=document.getElementById('nfName').value.trim();
  if(!nm)return toast("Folder name required!",'err');
  const f={id:uid(),name:nm,desc:document.getElementById('nfDesc').value.trim(),author:curUser.username,authorPfp:curUser.pfp,scripts:[],userReactions:{},createdAt:Date.now()};
  D.folders.unshift(f);openF[f.id]=true;sv('folders');closeM('newFolderModal');renderAll();toast("Folder created!");
}
function openEditFolder(fid){
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  document.getElementById('efId').value=fid;document.getElementById('efName').value=f.name;document.getElementById('efDesc').value=f.desc||'';openM('editFolderModal');
}
function doEditFolder(){
  const id=document.getElementById('efId').value;
  const nm=document.getElementById('efName').value.trim();
  if(!nm)return toast("Name required!",'err');
  const f=D.folders.find(f=>f.id===id);if(!f)return;
  f.name=nm;f.desc=document.getElementById('efDesc').value.trim();sv('folders');closeM('editFolderModal');renderAll();toast("Folder updated!");
}

// === SCRIPT MODAL ===
function openSM(fid,preType){
  if(!curUser)return toast("Login first!",'err');
  document.getElementById('smFid').value=fid;document.getElementById('smSid').value='';document.getElementById('smType').value='';
  document.getElementById('smTitle').textContent='Add Script';
  document.getElementById('smS1').style.display='block';document.getElementById('smS2').style.display='none';
  ['smSTitle','smSDesc'].forEach(id=>document.getElementById(id).value='');
  if(_sme){_sme.toTextArea();_sme=null;}
  openM('scriptModal');
  if(preType)setTimeout(()=>smChoose(preType),40);
}
function smChoose(type){
  document.getElementById('smType').value=type;
  document.getElementById('smS1').style.display='none';document.getElementById('smS2').style.display='block';
  document.getElementById('smBack').style.display='';
  const bc=type==='ServerScript'?'srv':'lcl';
  document.getElementById('smBadge').innerHTML=`<span class="sbadge ${bc}">${type==='ServerScript'?'&#x1F5A5;&#xFE0F; ServerScript':'&#x1F4BB; LocalScript'}</span>`;
  setTimeout(()=>{if(_sme){_sme.toTextArea();_sme=null;}_sme=CodeMirror.fromTextArea(document.getElementById('smSCode'),{lineNumbers:true,theme:'dracula',mode:'lua'});},60);
}
function smGoBack(){document.getElementById('smS1').style.display='block';document.getElementById('smS2').style.display='none';if(_sme){_sme.toTextArea();_sme=null;}}
function openEditSM(fid,sid){
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  const fs=f.scripts.find(s=>s.id===sid);if(!fs)return;
  document.getElementById('smFid').value=fid;document.getElementById('smSid').value=sid;document.getElementById('smType').value=fs.scriptType;
  document.getElementById('smTitle').textContent='Edit Script';
  document.getElementById('smS1').style.display='none';document.getElementById('smS2').style.display='block';
  document.getElementById('smBack').style.display='none';
  const bc=fs.scriptType==='ServerScript'?'srv':'lcl';
  document.getElementById('smBadge').innerHTML=`<span class="sbadge ${bc}">${fs.scriptType==='ServerScript'?'&#x1F5A5;&#xFE0F; ServerScript':'&#x1F4BB; LocalScript'}</span>`;
  document.getElementById('smSTitle').value=fs.title;document.getElementById('smSDesc').value=fs.desc||'';document.getElementById('smSCode').value=fs.code;
  openM('scriptModal');
  setTimeout(()=>{if(_sme){_sme.toTextArea();_sme=null;}_sme=CodeMirror.fromTextArea(document.getElementById('smSCode'),{lineNumbers:true,theme:'dracula',mode:'lua'});_sme.setValue(fs.code);},60);
}
function closeScriptModal(){if(_sme){_sme.toTextArea();_sme=null;}closeM('scriptModal');}
function doSaveScript(){
  const fid=document.getElementById('smFid').value,sid=document.getElementById('smSid').value,type=document.getElementById('smType').value;
  const title=document.getElementById('smSTitle').value.trim(),desc=document.getElementById('smSDesc').value.trim();
  const code=_sme?_sme.getValue():'';
  if(!title||!code)return toast("Title and Code required!",'err');
  const f=D.folders.find(f=>f.id===fid);if(!f)return;
  if(sid){const fs=f.scripts.find(s=>s.id===sid);if(!fs)return;fs.title=title;fs.desc=desc;fs.code=code;toast("Script updated!");}
  else{f.scripts.push({id:uid(),title,desc,code,scriptType:type,author:curUser.username,authorPfp:curUser.pfp,userReactions:{},userRatings:{}});toast("Script added!");}
  sv('folders');if(_sme){_sme.toTextArea();_sme=null;}closeM('scriptModal');renderAll();
}

// === DELETE ===
function openDel(type,id1,id2){
  const t={folder:'Delete Folder?',script:'Delete Script?'};
  const s={folder:'This will permanently delete the folder and all its scripts.',script:'This script will be permanently removed.'};
  document.getElementById('delTitle').textContent=t[type]||'Delete?';
  document.getElementById('delSub').textContent=s[type]||'This cannot be undone.';
  openM('delModal');
  document.getElementById('delOk').onclick=()=>{
    if(type==='folder'){D.folders=D.folders.filter(f=>f.id!==id1);delete openF[id1];sv('folders');}
    else if(type==='script'){const f=D.folders.find(f=>f.id===id1);if(f){f.scripts=f.scripts.filter(s=>s.id!==id2);sv('folders');}}
    closeM('delModal');renderAll();toast("Deleted!");
  };
}

// === REPORT ===
function openRep(type,id1,id2){
  if(!curUser)return toast("Login first!",'err');
  let ctx='';
  if(type==='folder'){const f=D.folders.find(f=>f.id===id1);ctx=f?`Folder: "${esc(f.name)}" by ${esc(f.author)}`:''}
  else{const f=D.folders.find(f=>f.id===id1);const fs=f&&f.scripts.find(s=>s.id===id2);ctx=fs?`Script: "${esc(fs.title)}" by ${esc(fs.author)}`:''}
  document.getElementById('repTitle').textContent=`\u{1F6A9} Report ${type.charAt(0).toUpperCase()+type.slice(1)}`;
  document.getElementById('repCtx').textContent=ctx;document.getElementById('repReason').value='';
  document.getElementById('repOk').onclick=()=>{
    const reason=document.getElementById('repReason').value.trim();
    if(!reason)return toast("Reason required!",'err');
    let ru='',inm='';
    if(type==='folder'){const f=D.folders.find(f=>f.id===id1);if(!f)return;ru=f.author;inm=f.name;}
    else{const f=D.folders.find(f=>f.id===id1);const fs=f&&f.scripts.find(s=>s.id===id2);if(!fs)return;ru=fs.author;inm=fs.title;}
    D.reports.push({type,id1,id2,reporter:curUser.username,reportedUser:ru,reason,itemName:inm,ts:Date.now()});
    sv('reports');closeM('repModal');toast("Reported!",'inf');
  };
  openM('repModal');
}

// === BAN ===
function openBan(username,rRef,_a,_b){
  document.getElementById('banCtx').textContent=`You are about to ban: ${username}`;
  document.getElementById('banDays').value='';document.getElementById('banReason').value='';
  openM('banModal');
  document.getElementById('banOk').onclick=()=>{
    const days=document.getElementById('banDays').value,reason=document.getElementById('banReason').value.trim();
    if(!days||!reason)return toast("Fill all fields!",'err');
    if(!D.banned.includes(username)){
      D.banned.push(username);D.warnings[username]=(D.warnings[username]||0)+1;
      sv('banned');sv('warnings');
      const lvl=D.warnings[username];
      toast(lvl>=10?`${username} permanently banned!`:`${username} banned for ${days} days (Warn ${lvl}/10)`,'err');
    }
    if(rRef)dismissRep(rRef.i);
    closeM('banModal');renderHdr();renderAll();
    if(document.getElementById('adminModal').classList.contains('show'))showAdmin();
  };
}
function unban(u){
  D.banned=D.banned.filter(x=>x!==u);sv('banned');
  D.unrep.push(u);WS('unrep',D.unrep);
  toast(`${u} unbanned!`,'wrn');showAdmin();renderHdr();
}

// === ADMIN ===
function openAdmin(){openM('adminModal');showAdmin();}
function adminTab(t){
  admTab=t;
  document.getElementById('tabRep').classList.toggle('act',t==='reports');
  document.getElementById('tabBan').classList.toggle('act',t==='bans');
  showAdmin();
}
function dismissRep(i){D.reports.splice(Number(i),1);sv('reports');showAdmin();}
function showAdmin(){
  const a=document.getElementById('adminArea');
  if(admTab==='reports'){
    if(!D.reports.length){a.innerHTML=`<p style="color:var(--text3);font-family:'Inter',sans-serif;font-size:13px;padding:10px 0">No reports yet.</p>`;return;}
    a.innerHTML=`<table class="atbl">
      <tr><th>Type</th><th>User</th><th>Details</th><th></th></tr>
      ${D.reports.map((r,i)=>`<tr>
        <td><span class="rbdg ${r.type==='folder'?'bf':'bs'}">${r.type}</span></td>
        <td style="font-family:'Fredoka One',cursive;font-size:13px">${esc(r.reportedUser)}</td>
        <td><b style="font-size:11px">${esc(r.reason)}</b><div class="rprev">${esc(r.itemName||'')}</div></td>
        <td style="white-space:nowrap">
          <button class="rbtn" style="background:var(--red);color:white;border-color:var(--red);padding:3px 8px;font-size:10px;margin-bottom:3px" onclick="openBan('${esc(r.reportedUser)}',{i:${i}},null,null)">Ban</button><br>
          <button class="rbtn" style="padding:3px 8px;font-size:10px" onclick="dismissRep(${i})">&#x2715;</button>
        </td>
      </tr>`).join('')}
    </table>`;
  } else {
    if(!D.banned.length){a.innerHTML=`<p style="color:var(--text3);font-family:'Inter',sans-serif;font-size:13px;padding:10px 0">No banned users.</p>`;return;}
    a.innerHTML=`<table class="atbl">
      <tr><th>Username</th><th>Warnings</th><th></th></tr>
      ${D.banned.map(u=>`<tr>
        <td style="font-family:'Fredoka One',cursive;font-size:13px">${esc(u)}</td>
        <td style="color:var(--red);font-weight:700">&#x26A0; ${D.warnings[u]||1}/10</td>
        <td><button class="rbtn" style="background:var(--green);color:white;border-color:var(--green);padding:3px 8px;font-size:10px" onclick="unban('${u}')">Unban</button></td>
      </tr>`).join('')}
    </table>`;
  }
}

// === USER PROFILE ===
function openUserProfile(username){
  const u=D.users[username];if(!u)return;
  document.getElementById('um-av').src=u.pfp||'';
  document.getElementById('um-nm').textContent=username;
  document.getElementById('um-own').style.display=username==='Tasin Redwan'?'block':'none';
  document.getElementById('um-join').textContent=`Joined ${fmtD(u.joinedAt||Date.now())}`;
  const bel=getBeloved(username);
  const bEl=document.getElementById('um-bel');
  if(bel){bEl.style.display='block';document.getElementById('um-bel-nm').textContent=bel.title;}else bEl.style.display='none';
  const uf=D.folders.filter(f=>f.author===username);
  const fa=document.getElementById('um-folders');
  if(uf.length){
    fa.innerHTML=`<div style="font-family:'Fredoka One',cursive;font-size:14px;color:var(--text2);margin-bottom:8px;display:flex;align-items:center;gap:6px"><i class="fas fa-folder" style="color:var(--accent)"></i>Folders (${uf.length})</div>`;
    uf.forEach(f=>{fa.innerHTML+=`<div class="uf-item"><div class="uf-nm">&#x1F4C1; ${esc(f.name)}</div><div class="uf-ct">${(f.scripts||[]).length} scripts${f.desc?' &bull; '+esc(f.desc.substring(0,40)):''}<span style="color:rgba(91,142,240,0.5);margin-left:8px">&#x1F44D; ${Object.values(f.userReactions||{}).filter(r=>r==='like').length}</span></div></div>`;});
  } else {
    fa.innerHTML=`<p style="color:var(--text3);font-family:'Inter',sans-serif;font-size:13px;text-align:center;margin-top:10px">No folders yet.</p>`;
  }
  openM('userModal');
}

// === CHANGE NAME ===
function openChgName(){document.getElementById('newName').value=curUser?curUser.username:'';openM('chgNameModal');}
function doChgName(){
  const n=document.getElementById('newName').value.trim();
  if(!n)return toast("Name required",'err');
  if(n===curUser.username)return closeM('chgNameModal');
  if(D.users[n])return toast("Username taken!",'err');
  const old=curUser.username;
  D.users[n]={...D.users[old],username:n};delete D.users[old];sv('users');
  curUser.username=n;localStorage.setItem('lh_session',JSON.stringify(curUser));
  D.folders.forEach(f=>{if(f.author===old)f.author=n;(f.scripts||[]).forEach(fs=>{if(fs.author===old)fs.author=n;});});
  sv('folders');if(D.warnings[old]){D.warnings[n]=D.warnings[old];delete D.warnings[old];sv('warnings');}
  closeM('chgNameModal');updPP();renderAll();renderHdr();toast("Username updated!");
}

// === AUTH ===
function openAuthM(){authMode='login';updAuth();openM('authModal');}
function tglAuth(){authMode=authMode==='login'?'register':'login';updAuth();}
function updAuth(){
  const l=authMode==='login';
  document.getElementById('authTitle').textContent=l?'Welcome Back':'Create Account';
  document.getElementById('authSub').textContent=l?'Sign in to share your Roblox scripts.':'Join LuaHub and start sharing!';
  document.getElementById('authSubmit').textContent=l?'Login':'Create Account';
  document.getElementById('authTog').innerHTML=l?"Don't have an account? <span>Sign Up</span>":"Already have an account? <span>Login</span>";
  document.getElementById('authPfpRow').style.display=l?'none':'block';
}
function selPfp(el,seed){
  document.querySelectorAll('.pfp-opt').forEach(i=>i.classList.remove('sel'));
  el.classList.add('sel');selPfpSeed=seed;
}
function doAuth(){
  const u=document.getElementById('authUser').value.trim(),p=document.getElementById('authPass').value;
  if(!u||!p)return toast("All fields required",'err');
  if(authMode==='register'){
    if(D.users[u])return toast("Username taken!",'err');
    D.users[u]={username:u,password:p,pfp:`https://api.dicebear.com/7.x/pixel-art/svg?${selPfpSeed}`,joinedAt:Date.now()};
    sv('users');toast("Account created! Please login.");authMode='login';updAuth();
  } else {
    if(D.banned.includes(u))return toast("Account is banned!",'err');
    const usr=D.users[u];
    if(usr&&usr.password===p){curUser=usr;localStorage.setItem('lh_session',JSON.stringify(curUser));closeM('authModal');renderHdr();renderAll();toast("Welcome back!");}
    else toast("Wrong credentials",'err');
  }
}

// === OAUTH ===
let _oauthP='';
function oauthLogin(p){
  _oauthP=p;selPfpSeed='seed=10';
  const icons={github:'<img src="https://api.iconify.design/mdi:github.svg?color=%23ffffff" style="width:36px">',google:'<img src="https://api.iconify.design/flat-color-icons:google.svg" style="width:36px">',apple:'<img src="https://api.iconify.design/ic:baseline-apple.svg?color=%23ffffff" style="width:36px">'};
  document.getElementById('oiIcon').innerHTML=icons[p]||'&#x1F510;';
  document.getElementById('oiTitle').textContent='Connecting...';
  document.getElementById('oiSub').innerHTML=`Completing sign-in via <b>${p.charAt(0).toUpperCase()+p.slice(1)}</b>`;
  document.getElementById('oiSpinner').style.display='block';
  document.getElementById('oiStep2').style.display='none';
  document.getElementById('oiUser').value='';
  closeM('authModal');openM('oauthModal');
  setTimeout(()=>{
    document.getElementById('oiSpinner').style.display='none';
    document.getElementById('oiTitle').textContent='Almost there!';
    document.getElementById('oiSub').textContent='Choose your LuaHub username';
    document.getElementById('oiStep2').style.display='block';
    // re-add pfp sel listeners
    document.querySelectorAll('#oiPfpRow .pfp-opt').forEach((el,i)=>{
      el.onclick=()=>selPfp(el,'seed='+(10+i));
    });
  },1600);
}
function finishOauth(){
  const u=document.getElementById('oiUser').value.trim();
  if(!u)return toast("Username required",'err');
  if(D.users[u])return toast("Username taken!",'err');
  D.users[u]={username:u,password:uid(),pfp:`https://api.dicebear.com/7.x/pixel-art/svg?${selPfpSeed}`,joinedAt:Date.now(),provider:_oauthP};
  sv('users');curUser=D.users[u];localStorage.setItem('lh_session',JSON.stringify(curUser));
  closeM('oauthModal');renderHdr();renderAll();toast(`Signed in with ${_oauthP.charAt(0).toUpperCase()+_oauthP.slice(1)}!`,'inf');
}
function logout(){curUser=null;localStorage.removeItem('lh_session');document.getElementById('pp').classList.remove('show');renderHdr();renderAll();}

// === INIT ===
window.onload=()=>{chkShare();renderHdr();renderAll();};
</script>
</body>
</html>
