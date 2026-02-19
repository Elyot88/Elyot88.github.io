<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CryptoOracle — Stratégies d'investissement</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:#080c14;--surface:#0d1420;--border:#1e2d4a;
    --accent:#00f5a0;--accent2:#00d4ff;--warn:#ff6b35;
    --text:#e8f4ff;--muted:#5a7a9f;--up:#00f5a0;--down:#ff4466;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  body{background:var(--bg);color:var(--text);font-family:'Syne',sans-serif;min-height:100vh;overflow-x:hidden;}
  body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(0,245,160,0.03) 1px,transparent 1px),linear-gradient(90deg,rgba(0,245,160,0.03) 1px,transparent 1px);background-size:40px 40px;pointer-events:none;z-index:0;}
  .orb1,.orb2{position:fixed;border-radius:50%;filter:blur(120px);opacity:.12;pointer-events:none;z-index:0;animation:drift 8s ease-in-out infinite alternate;}
  .orb1{width:500px;height:500px;background:var(--accent);top:-100px;right:-100px;}
  .orb2{width:400px;height:400px;background:var(--accent2);bottom:-100px;left:-100px;animation-delay:-4s;}
  @keyframes drift{to{transform:translate(30px,30px);}}
  .container{max-width:1200px;margin:0 auto;padding:0 24px;position:relative;z-index:1;}

  header{padding:32px 0 24px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;}
  .logo{font-family:'Space Mono',monospace;font-size:20px;font-weight:700;}
  .logo span{color:var(--accent);}
  .header-right{display:flex;align-items:center;gap:12px;}
  .date-badge{font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);}
  .live-badge{font-family:'Space Mono',monospace;font-size:11px;padding:4px 12px;border:1px solid var(--accent);color:var(--accent);border-radius:2px;text-transform:uppercase;letter-spacing:2px;animation:pulse 2s ease-in-out infinite;}
  @keyframes pulse{0%,100%{box-shadow:0 0 8px rgba(0,245,160,.3);}50%{box-shadow:0 0 20px rgba(0,245,160,.6);opacity:.7;}}

  .disclaimer{background:rgba(255,107,53,.08);border:1px solid rgba(255,107,53,.3);border-left:3px solid var(--warn);padding:12px 20px;margin:24px 0;font-size:13px;color:#ff9a70;font-family:'Space Mono',monospace;border-radius:2px;}

  .ticker-wrap{overflow:hidden;background:var(--surface);border:1px solid var(--border);border-radius:4px;margin-bottom:32px;}
  .ticker{display:flex;gap:48px;padding:12px 0;animation:scroll 40s linear infinite;white-space:nowrap;}
  .ticker-item{display:flex;align-items:center;gap:8px;font-family:'Space Mono',monospace;font-size:13px;}
  .ticker-item .name{color:var(--muted);}
  .ticker-item .change.up{color:var(--up);}
  .ticker-item .change.down{color:var(--down);}
  @keyframes scroll{to{transform:translateX(-50%);}}

  .section-title{font-size:11px;font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:3px;color:var(--accent);margin-bottom:14px;}

  /* ====== DAILY PICK ====== */
  .daily-pick{background:linear-gradient(135deg,rgba(0,245,160,0.06),rgba(0,212,255,0.06));border:1px solid rgba(0,245,160,0.3);border-radius:10px;padding:28px;margin-bottom:32px;position:relative;overflow:hidden;}
  .daily-pick::before{content:'';position:absolute;top:0;right:0;width:200px;height:200px;background:radial-gradient(circle,rgba(0,245,160,0.08),transparent 70%);pointer-events:none;}
  .daily-pick-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;flex-wrap:wrap;gap:12px;}
  .daily-pick-title{font-size:11px;font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:3px;color:var(--accent);}
  .daily-pick-date{font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);}
  .daily-pick-main{display:grid;grid-template-columns:auto 1fr auto;gap:24px;align-items:start;}
  .pick-icon-wrap{text-align:center;}
  .pick-icon{font-size:48px;display:block;margin-bottom:4px;}
  .pick-sym{font-family:'Space Mono',monospace;font-size:13px;color:var(--accent);font-weight:700;}
  .pick-info h2{font-size:26px;font-weight:800;letter-spacing:-1px;margin-bottom:6px;}
  .pick-info h2 span{background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
  .pick-info .pick-rationale{font-size:14px;color:#8ab0cf;line-height:1.7;max-width:560px;}
  .pick-levels{display:flex;flex-direction:column;gap:10px;min-width:200px;}
  .pick-level{background:rgba(0,0,0,0.3);border-radius:6px;padding:12px 16px;border:1px solid var(--border);}
  .pick-level .lv-label{font-family:'Space Mono',monospace;font-size:10px;text-transform:uppercase;letter-spacing:1px;color:var(--muted);margin-bottom:4px;}
  .pick-level .lv-val{font-family:'Space Mono',monospace;font-size:18px;font-weight:700;}
  .lv-entry{color:var(--accent2);}
  .lv-sl{color:var(--down);}
  .lv-tp{color:var(--up);}
  .pick-tags{display:flex;flex-wrap:wrap;gap:8px;margin-top:16px;}
  .pick-tag{font-family:'Space Mono',monospace;font-size:10px;padding:4px 10px;border:1px solid var(--border);border-radius:2px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;}
  .pick-tag.green{border-color:rgba(0,245,160,.3);color:var(--accent);}
  .pick-tag.orange{border-color:rgba(255,107,53,.3);color:var(--warn);}

  /* ====== ALTCOIN SEASON INDEX ====== */
  .altseason-card{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:24px;margin-bottom:32px;}
  .altseason-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;flex-wrap:wrap;gap:12px;}
  .altseason-header h3{font-size:13px;font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:2px;color:var(--accent);}
  .altseason-score-wrap{display:flex;align-items:center;gap:20px;flex-wrap:wrap;}
  .altseason-score{font-family:'Space Mono',monospace;font-size:52px;font-weight:700;line-height:1;}
  .altseason-label{font-size:11px;font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:2px;padding:4px 12px;border-radius:2px;margin-top:6px;display:inline-block;}
  .label-btc{background:rgba(247,147,26,.15);color:#f7931a;border:1px solid rgba(247,147,26,.3);}
  .label-neutral{background:rgba(255,200,50,.15);color:#ffc832;border:1px solid rgba(255,200,50,.3);}
  .label-alt{background:rgba(0,245,160,.15);color:var(--accent);border:1px solid rgba(0,245,160,.3);}
  .altseason-bar-wrap{margin:16px 0 12px;}
  .altseason-bar-track{position:relative;height:16px;background:rgba(0,0,0,.4);border-radius:8px;border:1px solid var(--border);overflow:hidden;}
  .altseason-bar-fill{height:100%;border-radius:8px;transition:width 1s ease;background:linear-gradient(90deg,#f7931a,#ffc832,var(--accent));}
  .altseason-bar-needle{position:absolute;top:-4px;height:24px;width:3px;background:white;border-radius:2px;box-shadow:0 0 8px rgba(255,255,255,.6);transition:left 1s ease;}
  .altseason-markers{display:flex;justify-content:space-between;font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);margin-top:6px;padding:0 2px;}
  .altseason-zones{display:flex;gap:16px;flex-wrap:wrap;margin-top:16px;}
  .altseason-zone{display:flex;align-items:center;gap:6px;font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);}
  .zone-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
  .altseason-meta{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-top:16px;}
  .alt-meta-card{background:rgba(0,0,0,.25);border:1px solid var(--border);border-radius:6px;padding:12px;}
  .alt-meta-card .amc-label{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:4px;}
  .alt-meta-card .amc-val{font-family:'Space Mono',monospace;font-size:17px;font-weight:700;}
  .altseason-analysis{font-size:13px;color:#8ab0cf;line-height:1.7;margin-top:16px;border-top:1px solid var(--border);padding-top:16px;}
  .altseason-analysis strong{color:var(--text);}

  /* ====== SELECTOR ====== */
  .tab-bar{display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap;}
  .tab-btn{font-family:'Space Mono',monospace;font-size:11px;text-transform:uppercase;letter-spacing:1px;padding:7px 16px;border:1px solid var(--border);background:transparent;color:var(--muted);border-radius:3px;cursor:pointer;transition:all .2s;}
  .tab-btn:hover{border-color:rgba(0,245,160,.4);color:var(--text);}
  .tab-btn.active{border-color:var(--accent);color:var(--accent);background:rgba(0,245,160,.05);}
  .tab-btn.uni-tab.active{border-color:var(--warn);color:var(--warn);background:rgba(255,107,53,.05);}
  .legend{display:flex;gap:20px;margin-bottom:16px;font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);}
  .legend-item{display:flex;align-items:center;gap:6px;}
  .legend-dot{width:8px;height:8px;border-radius:50%;}
  .crypto-search-wrap{position:relative;margin-bottom:12px;}
  .crypto-search{width:100%;background:var(--surface);border:1px solid var(--border);color:var(--text);padding:10px 14px 10px 36px;border-radius:4px;font-family:'Space Mono',monospace;font-size:13px;outline:none;transition:border-color .2s;}
  .crypto-search:focus{border-color:var(--accent);}
  .crypto-search::placeholder{color:var(--muted);}
  .search-icon{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:var(--muted);}
  .crypto-panel{display:none;}
  .crypto-panel.active{display:block;}
  .crypto-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(95px,1fr));gap:8px;max-height:320px;overflow-y:auto;padding-right:4px;margin-bottom:20px;}
  .crypto-grid::-webkit-scrollbar{width:3px;}
  .crypto-grid::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px;}
  .crypto-btn{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:10px 6px;text-align:center;cursor:pointer;transition:all .18s;position:relative;overflow:hidden;}
  .crypto-btn:hover{border-color:rgba(0,245,160,.35);transform:translateY(-1px);}
  .crypto-btn.active{border-color:var(--accent);box-shadow:0 0 16px rgba(0,245,160,.15);background:rgba(0,245,160,.05);}
  .crypto-btn .icon{font-size:18px;display:block;margin-bottom:3px;}
  .crypto-btn .sym{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);display:block;font-weight:700;}
  .crypto-btn .cname{font-family:'Space Mono',monospace;font-size:8px;color:#3a5070;display:block;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
  .crypto-btn.active .sym{color:var(--accent);}
  .rank-badge{position:absolute;top:3px;right:4px;font-family:'Space Mono',monospace;font-size:8px;color:#3a5070;}
  .crypto-btn.unicorn{border-color:rgba(255,107,53,.3);}
  .crypto-btn.unicorn:hover{border-color:rgba(255,107,53,.6);}
  .crypto-btn.unicorn.active{border-color:var(--warn);box-shadow:0 0 16px rgba(255,107,53,.2);background:rgba(255,107,53,.05);}
  .crypto-btn.unicorn .sym{color:#e08060;}
  .crypto-btn.unicorn.active .sym{color:var(--warn);}
  .uni-corner{position:absolute;top:2px;left:3px;font-size:9px;}
  .unicorn-alert{background:rgba(255,107,53,.08);border:1px solid rgba(255,107,53,.3);border-radius:6px;padding:14px 18px;margin-bottom:16px;font-family:'Space Mono',monospace;font-size:12px;color:#ff9a70;line-height:1.7;}
  .unicorn-alert strong{color:var(--warn);}

  /* ====== CONFIG ====== */
  .config-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-bottom:24px;}
  .config-card{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:20px;transition:border-color .2s;}
  .config-card:hover{border-color:rgba(0,245,160,.25);}
  .config-card label{display:block;font-size:11px;text-transform:uppercase;letter-spacing:2px;color:var(--muted);margin-bottom:10px;font-family:'Space Mono',monospace;}
  select,input[type="range"],input[type="number"]{width:100%;background:var(--bg);border:1px solid var(--border);color:var(--text);padding:10px 12px;border-radius:4px;font-family:'Space Mono',monospace;font-size:13px;outline:none;transition:border-color .2s;-webkit-appearance:none;}
  select:focus,input:focus{border-color:var(--accent);}
  select option{background:#0d1420;}
  input[type="range"]{padding:0;height:4px;cursor:pointer;accent-color:var(--accent);margin-top:8px;}
  .range-display{text-align:right;font-family:'Space Mono',monospace;font-size:18px;font-weight:700;color:var(--accent);margin-top:6px;}
  .generate-btn{width:100%;padding:20px;background:linear-gradient(135deg,var(--accent),var(--accent2));border:none;border-radius:6px;color:#000;font-family:'Syne',sans-serif;font-size:16px;font-weight:800;text-transform:uppercase;letter-spacing:2px;cursor:pointer;transition:all .2s;position:relative;overflow:hidden;margin-bottom:40px;}
  .generate-btn:hover{transform:translateY(-2px);box-shadow:0 8px 40px rgba(0,245,160,.3);}
  .generate-btn .shimmer{position:absolute;top:0;left:-100%;width:100%;height:100%;background:linear-gradient(90deg,transparent,rgba(255,255,255,.2),transparent);animation:shimmer 2s ease-in-out infinite;}
  @keyframes shimmer{to{left:100%;}}

  /* ====== OUTPUT ====== */
  #strategy-output{display:none;}
  #strategy-output.visible{display:block;animation:fadeIn .5s ease;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);}}
  .strategy-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:24px;padding-bottom:20px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:12px;}
  .strategy-name{font-size:32px;font-weight:800;letter-spacing:-1px;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
  .risk-badge{font-family:'Space Mono',monospace;font-size:12px;padding:6px 16px;border-radius:2px;text-transform:uppercase;letter-spacing:2px;}
  .risk-low{background:rgba(0,245,160,.15);color:var(--accent);border:1px solid rgba(0,245,160,.3);}
  .risk-mid{background:rgba(255,200,50,.15);color:#ffc832;border:1px solid rgba(255,200,50,.3);}
  .risk-high{background:rgba(255,68,102,.15);color:var(--down);border:1px solid rgba(255,68,102,.3);}
  .metrics-row{display:grid;grid-template-columns:repeat(4,1fr);gap:16px;margin-bottom:24px;}
  .metric-card{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:18px;}
  .metric-card .label{font-size:10px;text-transform:uppercase;letter-spacing:2px;color:var(--muted);font-family:'Space Mono',monospace;margin-bottom:8px;}
  .metric-card .value{font-size:24px;font-weight:700;font-family:'Space Mono',monospace;}
  .positive{color:var(--up);}.warning{color:#ffc832;}.negative{color:var(--down);}
  .strategy-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:24px;}
  .strategy-card{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:24px;}
  .strategy-card h3{font-size:13px;text-transform:uppercase;letter-spacing:2px;color:var(--accent);font-weight:600;margin-bottom:16px;font-family:'Space Mono',monospace;}
  .signal-row{display:flex;align-items:flex-start;gap:12px;padding:12px 0;border-bottom:1px solid rgba(30,45,74,.8);}
  .signal-row:last-child{border-bottom:none;}
  .signal-dot{width:8px;height:8px;border-radius:50%;margin-top:6px;flex-shrink:0;}
  .dot-buy{background:var(--up);box-shadow:0 0 8px rgba(0,245,160,.5);}
  .dot-sell{background:var(--down);box-shadow:0 0 8px rgba(255,68,102,.5);}
  .dot-hold{background:#ffc832;box-shadow:0 0 8px rgba(255,200,50,.5);}
  .dot-warn{background:var(--warn);box-shadow:0 0 8px rgba(255,107,53,.5);}
  .signal-text{font-size:14px;line-height:1.6;color:#a0bcd8;}
  .signal-text strong{color:var(--text);font-weight:600;}
  .timeline{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:24px;margin-bottom:24px;}
  .timeline h3{font-size:13px;text-transform:uppercase;letter-spacing:2px;color:var(--accent);font-family:'Space Mono',monospace;margin-bottom:20px;}
  .timeline-steps{display:grid;grid-template-columns:repeat(4,1fr);position:relative;}
  .timeline-steps::before{content:'';position:absolute;top:20px;left:20px;right:20px;height:1px;background:linear-gradient(90deg,var(--accent),var(--accent2));opacity:.4;}
  .t-step{text-align:center;}
  .t-dot{width:16px;height:16px;border-radius:50%;margin:12px auto 14px;border:2px solid var(--accent);background:var(--bg);position:relative;z-index:1;}
  .t-dot.filled{background:var(--accent);box-shadow:0 0 12px rgba(0,245,160,.4);}
  .t-label{font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:1px;}
  .t-desc{font-size:13px;margin-top:6px;color:var(--text);}
  .t-step-num{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);}
  .indicators-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:24px;}
  .indicator-card{background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:16px;display:flex;align-items:center;gap:12px;}
  .indicator-icon{width:36px;height:36px;border-radius:4px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
  .ind-green{background:rgba(0,245,160,.1);}.ind-red{background:rgba(255,68,102,.1);}.ind-yellow{background:rgba(255,200,50,.1);}
  .indicator-info .iname{font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;text-transform:uppercase;letter-spacing:1px;}
  .indicator-info .ival{font-size:18px;font-weight:700;font-family:'Space Mono',monospace;margin-top:2px;}
  .indicator-info .isig{font-size:11px;margin-top:2px;color:var(--muted);}
  #loading{display:none;text-align:center;padding:40px;}
  #loading.visible{display:block;}
  .loader{width:40px;height:40px;border:2px solid var(--border);border-top-color:var(--accent);border-radius:50%;animation:spin .7s linear infinite;margin:0 auto 16px;}
  @keyframes spin{to{transform:rotate(360deg);}}
  .loading-text{font-family:'Space Mono',monospace;font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:2px;}

  /* ====== PLATFORMS ====== */
  .platforms-section{margin-top:56px;padding-top:32px;border-top:1px solid var(--border);margin-bottom:48px;}
  .platforms-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:16px;}
  .platform-card{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:24px;text-decoration:none;transition:all .2s;display:block;position:relative;overflow:hidden;}
  .platform-card::after{content:'';position:absolute;bottom:0;left:0;right:0;height:2px;opacity:0;transition:opacity .2s;}
  .platform-card.cb::after{background:var(--accent);}
  .platform-card.bn::after{background:#f3ba2f;}
  .platform-card.kr::after{background:#5741d9;}
  .platform-card:hover{transform:translateY(-4px);}
  .platform-card.cb:hover{border-color:rgba(0,245,160,.4);box-shadow:0 12px 40px rgba(0,245,160,.08);}
  .platform-card.bn:hover{border-color:rgba(243,186,47,.4);box-shadow:0 12px 40px rgba(243,186,47,.08);}
  .platform-card.kr:hover{border-color:rgba(87,65,217,.5);box-shadow:0 12px 40px rgba(87,65,217,.1);}
  .platform-card:hover::after{opacity:1;}
  .p-top{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;}
  .p-logo-wrap{display:flex;align-items:center;gap:10px;}
  .p-emoji{font-size:26px;}
  .p-name{font-size:20px;font-weight:800;color:var(--text);letter-spacing:-.5px;}
  .p-rank{font-family:'Space Mono',monospace;font-size:10px;padding:3px 8px;border-radius:2px;text-transform:uppercase;letter-spacing:1px;}
  .rank-cb{color:var(--accent);border:1px solid rgba(0,245,160,.3);}
  .rank-bn{color:#f3ba2f;border:1px solid rgba(243,186,47,.3);}
  .rank-kr{color:#8b6fff;border:1px solid rgba(87,65,217,.3);}
  .p-desc{font-size:13px;color:#7a9bbf;line-height:1.7;margin-bottom:14px;}
  .p-tags{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:16px;}
  .p-tag{font-family:'Space Mono',monospace;font-size:10px;padding:3px 9px;border:1px solid var(--border);border-radius:2px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;}
  .p-link{display:inline-flex;align-items:center;gap:6px;font-family:'Space Mono',monospace;font-size:11px;text-transform:uppercase;letter-spacing:1px;}
  .p-link-cb{color:var(--accent);}.p-link-bn{color:#f3ba2f;}.p-link-kr{color:#8b6fff;}
  footer{text-align:center;padding:24px 0;border-top:1px solid var(--border);font-family:'Space Mono',monospace;font-size:11px;color:#3a5070;}
  /* ====== NEWS SECTION ====== */
  .news-section{margin-bottom:40px;}
  .news-filter-bar{display:flex;gap:8px;margin-bottom:18px;flex-wrap:wrap;align-items:center;}
  .news-filter-btn{font-family:'Space Mono',monospace;font-size:10px;padding:5px 12px;border:1px solid var(--border);background:transparent;color:var(--muted);border-radius:2px;cursor:pointer;transition:all .15s;text-transform:uppercase;letter-spacing:1px;}
  .news-filter-btn:hover{border-color:rgba(0,245,160,.4);color:var(--text);}
  .news-filter-btn.active{border-color:var(--accent);color:var(--accent);background:rgba(0,245,160,.05);}
  .news-filter-btn.cat-market.active{border-color:#f7931a;color:#f7931a;background:rgba(247,147,26,.05);}
  .news-filter-btn.cat-defi.active{border-color:#00d4ff;color:#00d4ff;background:rgba(0,212,255,.05);}
  .news-filter-btn.cat-reg.active{border-color:#a78bfa;color:#a78bfa;background:rgba(167,139,250,.05);}
  .news-filter-btn.cat-trend.active{border-color:#ff6b35;color:#ff6b35;background:rgba(255,107,53,.05);}
  .news-filter-btn.cat-x.active{border-color:#1d9bf0;color:#1d9bf0;background:rgba(29,155,240,.05);}

  .news-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
  .news-card{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:20px;cursor:pointer;transition:all .2s;position:relative;overflow:hidden;}
  .news-card::before{content:'';position:absolute;left:0;top:0;bottom:0;width:3px;background:var(--accent);transition:width .2s;}
  .news-card:hover{transform:translateY(-2px);border-color:rgba(0,245,160,.3);}
  .news-card:hover::before{width:5px;}
  .news-card.cat-market::before{background:#f7931a;}
  .news-card.cat-market:hover{border-color:rgba(247,147,26,.3);}
  .news-card.cat-defi::before{background:#00d4ff;}
  .news-card.cat-defi:hover{border-color:rgba(0,212,255,.3);}
  .news-card.cat-reg::before{background:#a78bfa;}
  .news-card.cat-reg:hover{border-color:rgba(167,139,250,.3);}
  .news-card.cat-trend::before{background:#ff6b35;}
  .news-card.cat-trend:hover{border-color:rgba(255,107,53,.3);}
  .news-card.cat-x::before{background:#1d9bf0;}
  .news-card.cat-x:hover{border-color:rgba(29,155,240,.3);}
  .news-card.hidden{display:none;}
  .news-card.featured{grid-column:1/-1;display:grid;grid-template-columns:1fr auto;gap:20px;align-items:start;}
  .news-top{display:flex;align-items:center;gap:10px;margin-bottom:10px;flex-wrap:wrap;}
  .news-cat-badge{font-family:'Space Mono',monospace;font-size:9px;padding:3px 8px;border-radius:2px;text-transform:uppercase;letter-spacing:1px;font-weight:700;}
  .badge-market{background:rgba(247,147,26,.15);color:#f7931a;border:1px solid rgba(247,147,26,.3);}
  .badge-defi{background:rgba(0,212,255,.15);color:#00d4ff;border:1px solid rgba(0,212,255,.3);}
  .badge-reg{background:rgba(167,139,250,.15);color:#a78bfa;border:1px solid rgba(167,139,250,.3);}
  .badge-trend{background:rgba(255,107,53,.15);color:#ff6b35;border:1px solid rgba(255,107,53,.3);}
  .badge-x{background:rgba(29,155,240,.15);color:#1d9bf0;border:1px solid rgba(29,155,240,.3);}
  .badge-hot{background:rgba(255,68,102,.15);color:#ff4466;border:1px solid rgba(255,68,102,.3);}
  .news-time{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);}
  .news-source{font-family:'Space Mono',monospace;font-size:10px;color:#3a5070;}
  .news-title{font-size:16px;font-weight:700;color:var(--text);line-height:1.4;margin-bottom:8px;letter-spacing:-.3px;}
  .news-card.featured .news-title{font-size:20px;}
  .news-body{font-size:13px;color:#7a9bbf;line-height:1.7;}
  .news-tags{display:flex;flex-wrap:wrap;gap:6px;margin-top:12px;}
  .news-tag{font-family:'Space Mono',monospace;font-size:9px;padding:2px 7px;border:1px solid var(--border);border-radius:2px;color:var(--muted);text-transform:uppercase;}
  .news-sentiment{display:flex;align-items:center;gap:6px;margin-top:12px;font-family:'Space Mono',monospace;font-size:11px;}
  .sent-bar{flex:1;height:4px;background:var(--border);border-radius:2px;overflow:hidden;}
  .sent-fill{height:100%;border-radius:2px;transition:width .5s ease;}
  .sent-bullish{background:var(--up);}
  .sent-bearish{background:var(--down);}
  .sent-neutral{background:#ffc832;}
  .x-thread-card{background:rgba(29,155,240,.05);border:1px solid rgba(29,155,240,.2);border-radius:8px;padding:16px;margin-top:14px;}
  .x-handle{font-family:'Space Mono',monospace;font-size:12px;color:#1d9bf0;margin-bottom:6px;}
  .x-content{font-size:13px;color:#8ab0cf;line-height:1.6;}
  .x-stats{display:flex;gap:16px;margin-top:10px;font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);}
  .news-featured-right{min-width:200px;}
  .news-impact{background:rgba(0,0,0,.3);border:1px solid var(--border);border-radius:6px;padding:16px;margin-top:0;}
  .news-impact .ni-label{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:12px;}
  .ni-row{display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid rgba(30,45,74,.5);}
  .ni-row:last-child{border-bottom:none;}
  .ni-coin{font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);}
  .ni-impact{font-family:'Space Mono',monospace;font-size:11px;font-weight:700;}
  .impact-pos{color:var(--up);}.impact-neg{color:var(--down);}.impact-neu{color:#ffc832;}

  ::-webkit-scrollbar{width:4px;}
  ::-webkit-scrollbar-track{background:var(--bg);}
  ::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px;}

  @media(max-width:900px){
    .config-grid,.strategy-grid,.platforms-grid{grid-template-columns:1fr;}
    .metrics-row,.indicators-grid{grid-template-columns:1fr 1fr;}
    .timeline-steps{grid-template-columns:1fr 1fr;}
    .crypto-grid{grid-template-columns:repeat(auto-fill,minmax(80px,1fr));}
    .daily-pick-main{grid-template-columns:1fr;}
    .altseason-meta{grid-template-columns:1fr 1fr;}
  }
</style>
</head>
<body>
<div class="orb1"></div><div class="orb2"></div>
<div class="container">

  <header>
    <div class="logo">Crypto<span>Oracle</span></div>
    <div class="header-right">
      <div class="date-badge">Jeu. 19 Fév. 2026</div>
      <div class="live-badge">⬤ Live</div>
    </div>
  </header>

  <div class="disclaimer">⚠ Avertissement : Cet outil est à titre éducatif uniquement. Les informations ne constituent pas des recommandations financières. Investir en cryptomonnaies comporte des risques importants de perte en capital. Faites vos propres recherches (DYOR).</div>

  <div class="ticker-wrap"><div class="ticker" id="ticker"></div></div>

  <!-- ====== DAILY PICK ====== -->
  <div class="section-title">// Conseil du jour — 19 Février 2026</div>
  <div class="daily-pick">
    <div class="daily-pick-header">
      <div class="daily-pick-title">🎯 Opportunité identifiée du jour</div>
      <div class="daily-pick-date">Analyse basée sur données marché temps réel</div>
    </div>
    <div class="daily-pick-main">
      <div class="pick-icon-wrap">
        <span class="pick-icon">₿</span>
        <span class="pick-sym">BTC/USDT</span>
      </div>
      <div class="pick-info">
        <h2><span>Bitcoin (BTC)</span> — Achat sur support</h2>
        <p class="pick-rationale">
          Bitcoin consolide autour de <strong>$66 000–$68 000</strong> après une correction de -46% depuis son ATH de $126 000 en octobre 2025. Le RSI journalier est à <strong>~33</strong> (zone de survente), signal historiquement favorable pour un rebond technique. Les baleines on-chain accumulent dans la zone <strong>$60k–$69k</strong> (demand cluster selon Glassnode). La dominance BTC est à <strong>59,7%</strong> — en phase "Bitcoin Season" — ce qui renforce la thèse d'accumulation sur BTC avant toute rotation altcoin. Le Supreme Court ruling sur les tarifs US vendredi pourrait créer de la volatilité : positionnement avant l'événement avec stop serré.
        </p>
        <div class="pick-tags">
          <span class="pick-tag green">RSI ~33 survente</span>
          <span class="pick-tag green">Accumulation baleines</span>
          <span class="pick-tag green">Bitcoin Season</span>
          <span class="pick-tag orange">Volatilité ruling vendredi</span>
          <span class="pick-tag orange">Marché en Extreme Fear</span>
        </div>
      </div>
      <div class="pick-levels">
        <div class="pick-level">
          <div class="lv-label">Zone d'entrée</div>
          <div class="lv-val lv-entry">$66 500–68 000</div>
        </div>
        <div class="pick-level">
          <div class="lv-label">Stop-Loss</div>
          <div class="lv-val lv-sl">$62 000 (-8%)</div>
        </div>
        <div class="pick-level">
          <div class="lv-label">Take-Profit 1</div>
          <div class="lv-val lv-tp">$75 000 (+11%)</div>
        </div>
        <div class="pick-level">
          <div class="lv-label">Take-Profit 2</div>
          <div class="lv-val lv-tp">$79 000 (+17%)</div>
        </div>
        <div class="pick-level" style="background:rgba(0,245,160,0.05);border-color:rgba(0,245,160,0.2);">
          <div class="lv-label">Ratio Risque/Récomp.</div>
          <div class="lv-val" style="color:var(--accent2);">1 : 2.1</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== ALTCOIN SEASON INDEX ====== -->
  <div class="section-title">// Indice Saison Bitcoin / Altcoin</div>
  <div class="altseason-card">
    <div class="altseason-header">
      <h3>// Altcoin Season Index — CMC</h3>
      <div style="font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);">Mis à jour : 19 Fév. 2026</div>
    </div>
    <div class="altseason-score-wrap">
      <div>
        <div class="altseason-score" style="color:#f7931a;">29<span style="font-size:24px;color:var(--muted)">/100</span></div>
        <div class="altseason-label label-btc">🟠 Bitcoin Season</div>
      </div>
      <div style="flex:1;min-width:200px;">
        <div style="font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);margin-bottom:8px;">29 des 100 plus grandes altcoins ont surperformé BTC sur 90 jours</div>
        <div class="altseason-bar-wrap">
          <div class="altseason-bar-track">
            <div class="altseason-bar-fill" style="width:29%"></div>
            <div class="altseason-bar-needle" style="left:calc(29% - 1px)"></div>
          </div>
          <div class="altseason-markers">
            <span>0</span><span>25</span><span>50</span><span>75</span><span>100</span>
          </div>
        </div>
        <div class="altseason-zones">
          <div class="altseason-zone"><div class="zone-dot" style="background:#f7931a"></div>0–25 Bitcoin Dominant</div>
          <div class="altseason-zone"><div class="zone-dot" style="background:#ffc832"></div>26–74 Neutre</div>
          <div class="altseason-zone"><div class="zone-dot" style="background:var(--accent)"></div>75–100 Altcoin Season</div>
        </div>
      </div>
    </div>
    <div class="altseason-meta">
      <div class="alt-meta-card">
        <div class="amc-label">Dominance BTC</div>
        <div class="amc-val" style="color:#f7931a;">59.7%</div>
      </div>
      <div class="alt-meta-card">
        <div class="amc-label">Fear &amp; Greed Index</div>
        <div class="amc-val" style="color:var(--down);">12 — Extreme Fear</div>
      </div>
      <div class="alt-meta-card">
        <div class="amc-label">Dernier Altcoin Season</div>
        <div class="amc-val" style="color:var(--muted);">Déc. 2024 (Index 88)</div>
      </div>
    </div>
    <div class="altseason-analysis">
      <strong>Analyse :</strong> L'indice est à 29/100, clairement en territoire <strong>"Bitcoin Season"</strong>. Le seuil de 75 déclenchant l'altcoin season officielle est loin. La dominance BTC à 59,7% et l'indice Fear &amp; Greed à 12 (Extreme Fear) indiquent une aversion au risque généralisée : les capitaux fuient vers BTC ou les stablecoins. Historiquement, après un pic de dominance BTC proche des 60%, une rotation vers les altcoins devient possible — mais le signal de confirmation (dominance BTC en baisse soutenue + index > 50) n'est pas encore là. <strong>Stratégie : privilégier BTC, maintenir de la liquidité en stablecoins</strong>, et surveiller un retournement de dominance pour positionner les altcoins de qualité.
    </div>
  </div>

  <!-- ====== NEWS SECTION ====== -->
  <div class="news-section">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:8px;">
      <div class="section-title" style="margin-bottom:0">// News &amp; Trends &mdash; X &amp; Marchés</div>
      <div style="font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);">Mis à jour : 19 Fév. 2026 &middot; 12h00 CET</div>
    </div>
    <div class="news-filter-bar">
      <span style="font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;">Filtrer :</span>
      <button class="news-filter-btn active" onclick="filterNews('all',this)">Tous</button>
      <button class="news-filter-btn cat-market" onclick="filterNews('cat-market',this)">📊 Marché</button>
      <button class="news-filter-btn cat-defi" onclick="filterNews('cat-defi',this)">🔗 DeFi</button>
      <button class="news-filter-btn cat-reg" onclick="filterNews('cat-reg',this)">⚖️ Régulation</button>
      <button class="news-filter-btn cat-trend" onclick="filterNews('cat-trend',this)">🔥 Trending</button>
      <button class="news-filter-btn cat-x" onclick="filterNews('cat-x',this)">🐦 Voix de X</button>
    </div>
    <div class="news-grid" id="newsGrid">

      <div class="news-card featured cat-market" data-cat="cat-market">
        <div>
          <div class="news-top">
            <span class="news-cat-badge badge-market">📊 Marché</span>
            <span class="news-cat-badge badge-hot">🔥 Breaking</span>
            <span class="news-time">Il y a 3h</span>
            <span class="news-source">&middot; CryptoTicker / VanEck</span>
          </div>
          <div class="news-title">Bitcoin à $67 200 : "Deleveraging ordonné" selon VanEck &mdash; le pire est peut-être passé</div>
          <div class="news-body">Après son ATH de $126 000 en octobre 2025, BTC a corrigé de -46%. VanEck qualifie ce mouvement de "deleveraging ordonné" : l'open interest futures a chuté de 20%+, la volatilité reste sous les niveaux des précédents bear markets. Bitcoin est à -2.88σ sous sa MA200, un niveau historiquement associé à des rebonds. Les ETF Bitcoin enregistrent des entrées nettes depuis 3 jours consécutifs. Le ruling de la Cour Suprême US sur les tarifs vendredi 20/02 est le prochain catalyseur majeur.</div>
          <div class="news-tags">
            <span class="news-tag">$BTC</span><span class="news-tag">Deleveraging</span><span class="news-tag">VanEck</span><span class="news-tag">ETF Inflows</span><span class="news-tag">Support $62k</span>
          </div>
          <div class="news-sentiment">
            <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
            <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:62%"></div></div>
            <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">62% Haussier</span>
          </div>
        </div>
        <div class="news-featured-right">
          <div class="news-impact">
            <div class="ni-label">Impact sur actifs</div>
            <div class="ni-row"><span class="ni-coin">BTC</span><span class="ni-impact impact-pos">▲ Haussier</span></div>
            <div class="ni-row"><span class="ni-coin">ETH</span><span class="ni-impact impact-neu">◆ Neutre</span></div>
            <div class="ni-row"><span class="ni-coin">Altcoins</span><span class="ni-impact impact-neg">▼ Prudence</span></div>
            <div class="ni-row"><span class="ni-coin">Stablecoins</span><span class="ni-impact impact-pos">▲ Afflux</span></div>
          </div>
        </div>
      </div>

      <div class="news-card cat-x" data-cat="cat-x">
        <div class="news-top">
          <span class="news-cat-badge badge-x">🐦 X Feature</span>
          <span class="news-time">Il y a 5 jours</span>
          <span class="news-source">&middot; BeInCrypto</span>
        </div>
        <div class="news-title">X lance les "Smart Cashtags" : trader la crypto directement depuis votre timeline</div>
        <div class="news-body">Nikita Bier (head of product X) annonce le 14 fév. les "Smart Cashtags" : cliquer sur $BTC affiche un graphique live et permet d'acheter directement sans quitter X. Une révolution pour les traders retail.</div>
        <div class="x-thread-card">
          <div class="x-handle">@nikitabier &middot; 14 Fév. 2026</div>
          <div class="x-content">"Yes, we are launching crypto trading features on X. I genuinely want crypto to proliferate on X..."</div>
          <div class="x-stats"><span>❤️ 24.3K</span><span>🔁 8.1K</span><span>💬 3.2K</span></div>
        </div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:78%"></div></div>
          <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">78% Bullish</span>
        </div>
      </div>

      <div class="news-card cat-market" data-cat="cat-market">
        <div class="news-top">
          <span class="news-cat-badge badge-market">📊 Macro</span>
          <span class="news-cat-badge badge-hot">⚠️ À surveiller</span>
          <span class="news-time">Demain 20 Fév.</span>
          <span class="news-source">&middot; Finance Magnates</span>
        </div>
        <div class="news-title">⚖️ Ruling Cour Suprême US sur les tarifs Trump &mdash; crypto en alerte volatilité demain</div>
        <div class="news-body">La Cour Suprême rend son verdict vendredi 20/02. En janvier, un simple délai avait fait bondir BTC de +$2 000 en 1h et liquidé $39M de shorts. Paul Howard (Wincent) cite cela comme "le catalyseur macro le plus important" de la semaine. Positionnez vos stops.</div>
        <div class="news-tags">
          <span class="news-tag">Volatilité demain</span><span class="news-tag">Macro</span><span class="news-tag">Trump</span><span class="news-tag">Stop-Loss critique</span>
        </div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-neutral" style="width:50%"></div></div>
          <span style="font-size:11px;color:#ffc832;font-family:'Space Mono',monospace;">Incertain</span>
        </div>
      </div>

      <div class="news-card cat-trend" data-cat="cat-trend">
        <div class="news-top">
          <span class="news-cat-badge badge-trend">🔥 Trending #1</span>
          <span class="news-time">Il y a 2h</span>
          <span class="news-source">&middot; Crypto CT</span>
        </div>
        <div class="news-title">$HYPE Hyperliquid explose : volumes perps record à $8B/jour, +12% en 24h</div>
        <div class="news-body">Hyperliquid ($HYPE) est le sujet le plus viral du Crypto Twitter ce matin. La plateforme de perps décentralisés vient d'atteindre $8 milliards de volume journalier — rivalisant avec des CEX majeurs. La proposition HIP-4 fait monter la spéculation.</div>
        <div class="x-thread-card">
          <div class="x-handle">@Ansem &middot; Il y a 4h</div>
          <div class="x-content">"HYPE just flipped Bybit in perp volume for the first time. This is not a drill. Decentralized perps are winning. $HYPE to $50 is not a fantasy."</div>
          <div class="x-stats"><span>❤️ 18.7K</span><span>🔁 5.4K</span><span>💬 2.1K</span></div>
        </div>
        <div class="news-tags"><span class="news-tag">$HYPE</span><span class="news-tag">DeFi Perps</span><span class="news-tag">HIP-4</span><span class="news-tag">Volume Record</span></div>
      </div>

      <div class="news-card cat-defi" data-cat="cat-defi">
        <div class="news-top">
          <span class="news-cat-badge badge-defi">🔗 DeFi</span>
          <span class="news-time">Il y a 6h</span>
          <span class="news-source">&middot; Coinbase</span>
        </div>
        <div class="news-title">Coinbase accepte XRP, DOGE, ADA et LTC comme collatéral pour emprunter en USDC via Morpho</div>
        <div class="news-body">Coinbase intègre XRP, Dogecoin, Cardano et Litecoin comme collatéral pour des prêts onchain via Morpho. Emprunt jusqu'à $100 000 USDC sans vendre ses tokens. Disponible USA (hors NY).</div>
        <div class="news-tags"><span class="news-tag">$XRP</span><span class="news-tag">$DOGE</span><span class="news-tag">$ADA</span><span class="news-tag">Morpho</span><span class="news-tag">DeFi Lending</span></div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:71%"></div></div>
          <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">71% Bullish</span>
        </div>
      </div>

      <div class="news-card cat-reg" data-cat="cat-reg">
        <div class="news-top">
          <span class="news-cat-badge badge-reg">⚖️ RWA</span>
          <span class="news-time">Il y a 8h</span>
          <span class="news-source">&middot; Securitize</span>
        </div>
        <div class="news-title">Securitize tokenise le Trump International Hotel des Maldives avec World Liberty Financial</div>
        <div class="news-body">Securitize s'associe à World Liberty Financial et DarGlobal pour tokeniser le Trump International Hotel des Maldives. Le narratif RWA (Real World Assets) continue de dominer sur X en 2026.</div>
        <div class="news-tags"><span class="news-tag">RWA</span><span class="news-tag">Tokenisation</span><span class="news-tag">Trump</span><span class="news-tag">Immobilier</span></div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-neutral" style="width:48%"></div></div>
          <span style="font-size:11px;color:#ffc832;font-family:'Space Mono',monospace;">Polarisé</span>
        </div>
      </div>

      <div class="news-card cat-defi" data-cat="cat-defi">
        <div class="news-top">
          <span class="news-cat-badge badge-defi">🔗 Finance</span>
          <span class="news-time">Il y a 10h</span>
          <span class="news-source">&middot; Bloomberg</span>
        </div>
        <div class="news-title">Ledn vend $188M d'obligations adossées à Bitcoin &mdash; première mondiale sur le marché ABS</div>
        <div class="news-body">Ledn Inc émet $188M d'obligations ABS (Asset-Backed Securities) garanties par 5 400 prêts BTC à 11,8% de taux moyen. La tranche investment grade pricée à +335bps. Une première absolue dans la tokenisation de la dette Bitcoin.</div>
        <div class="news-tags"><span class="news-tag">$BTC</span><span class="news-tag">ABS</span><span class="news-tag">Obligations</span><span class="news-tag">TradFi</span></div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:81%"></div></div>
          <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">81% Bullish</span>
        </div>
      </div>

      <div class="news-card cat-reg" data-cat="cat-reg">
        <div class="news-top">
          <span class="news-cat-badge badge-reg">⚖️ ETF</span>
          <span class="news-time">Il y a 12h</span>
          <span class="news-source">&middot; Canary / Nasdaq</span>
        </div>
        <div class="news-title">Canary Funds lance SUIS &mdash; premier ETF Spot américain sur Sui coté au Nasdaq</div>
        <div class="news-body">Canary Funds lance SUIS sur le Nasdaq, premier ETF spot US sur Sui Network. Sui se développe dans les stablecoins et paiements. Ouvre l'accès institutionnel à SUI via les marchés financiers traditionnels.</div>
        <div class="news-tags"><span class="news-tag">$SUI</span><span class="news-tag">ETF Spot</span><span class="news-tag">Nasdaq</span><span class="news-tag">Institutionnel</span></div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:84%"></div></div>
          <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">84% Bullish</span>
        </div>
      </div>

      <div class="news-card cat-trend" data-cat="cat-trend">
        <div class="news-top">
          <span class="news-cat-badge badge-trend">🔥 AI x Crypto</span>
          <span class="news-time">Il y a 9h</span>
          <span class="news-source">&middot; Phantom</span>
        </div>
        <div class="news-title">Phantom Wallet lance un serveur MCP : les agents IA peuvent swapper et signer des transactions</div>
        <div class="news-body">Phantom Wallet lance son MCP Server : les agents IA (Claude, OpenClaw, etc.) peuvent désormais gérer des wallets, swapper et signer des transactions sur toutes les chains Phantom. L'intersection IA x crypto s'accélère.</div>
        <div class="x-thread-card">
          <div class="x-handle">@phantom &middot; Il y a 9h</div>
          <div class="x-content">"Introducing the Phantom MCP Server 🦞 Agents can swap, sign, and manage addresses across all of Phantom's supported chains. Ready to work with Claude or any MCP-compatible client."</div>
          <div class="x-stats"><span>❤️ 12.4K</span><span>🔁 4.8K</span><span>💬 892</span></div>
        </div>
        <div class="news-tags"><span class="news-tag">MCP</span><span class="news-tag">Agents IA</span><span class="news-tag">Phantom</span><span class="news-tag">Solana</span></div>
      </div>

      <div class="news-card cat-x" data-cat="cat-x">
        <div class="news-top">
          <span class="news-cat-badge badge-x">🐦 Voix de X</span>
          <span class="news-time">Il y a 1 jour</span>
          <span class="news-source">&middot; @saylor</span>
        </div>
        <div class="news-title">Michael Saylor réaffirme $150 000 BTC en 2026 &mdash; Strategy accumule pendant la correction</div>
        <div class="news-body">Strategy de Saylor continue d'acheter pendant la correction. Standard Chartered maintient $150k de target. Polymarket donne 79% de probabilité à BTC de dépasser $100k en 2026. Les institutionnels représentent 24% de l'activité BTC (vs 18% en 2024).</div>
        <div class="x-thread-card">
          <div class="x-handle">@saylor &middot; Il y a 18h</div>
          <div class="x-content">"Bitcoin is the exit strategy. Every correction in BTC history has been followed by a new all-time high. We are buying. 2026 target: $150,000."</div>
          <div class="x-stats"><span>❤️ 47.2K</span><span>🔁 14.1K</span><span>💬 5.9K</span></div>
        </div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-bullish" style="width:88%"></div></div>
          <span style="font-size:11px;color:var(--up);font-family:'Space Mono',monospace;">88% Bullish</span>
        </div>
      </div>

      <div class="news-card cat-reg" data-cat="cat-reg">
        <div class="news-top">
          <span class="news-cat-badge badge-reg">⚖️ Régulation</span>
          <span class="news-time">Il y a 13 jours</span>
          <span class="news-source">&middot; PBOC / CSRC</span>
        </div>
        <div class="news-title">Chine : cadre réglementaire RWA officiel publié par la PBOC &mdash; impact majeur sur le DeFi global</div>
        <div class="news-body">Le 6 février, la PBOC et sept agences publient le décret Yin Fa [2026] No. 42 sur les risques crypto. La CSRC publie des règles sur les tokens ABS offshore. La Chine ouvre la porte à la tokenisation tout en encadrant strictement les risques.</div>
        <div class="news-tags"><span class="news-tag">Chine</span><span class="news-tag">PBOC</span><span class="news-tag">RWA</span><span class="news-tag">Régulation</span></div>
        <div class="news-sentiment">
          <span style="font-size:11px;color:var(--muted);font-family:'Space Mono',monospace;white-space:nowrap;">Sentiment X :</span>
          <div class="sent-bar"><div class="sent-fill sent-neutral" style="width:54%"></div></div>
          <span style="font-size:11px;color:#ffc832;font-family:'Space Mono',monospace;">Mitigé</span>
        </div>
      </div>

    </div>
  </div>

  <!-- ====== CRYPTO SELECTOR ====== -->
  <div class="section-title">// Sélection de la cryptomonnaie</div>
  <div class="tab-bar">
    <button class="tab-btn active" onclick="switchTab('top50',this)">🏆 Top 50 — Market Cap</button>
    <button class="tab-btn uni-tab" onclick="switchTab('unicorns',this)">🦄 Licornes Trending — X &amp; Reddit</button>
  </div>
  <div class="legend">
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent)"></div>#1–50 Market Cap</div>
    <div class="legend-item"><div class="legend-dot" style="background:var(--warn)"></div>🦄 Viral X / Reddit</div>
  </div>
  <div class="crypto-search-wrap">
    <span class="search-icon">🔍</span>
    <input type="text" class="crypto-search" id="cryptoSearch" placeholder="Rechercher... (ex: BTC, Solana, HYPE)" oninput="filterCryptos(this.value)">
  </div>
  <div class="crypto-panel active" id="panel-top50">
    <div class="crypto-grid" id="top50Grid"></div>
  </div>
  <div class="crypto-panel" id="panel-unicorns">
    <div class="unicorn-alert">🦄 <strong>Licornes Trending — Fév. 2026</strong> — Sélection basée sur les tendances virales sur <strong>X (Twitter)</strong> et <strong>Reddit</strong>. Haute volatilité, fort potentiel de gain ET de perte. Max <strong>5–10%</strong> de votre portefeuille. En Bitcoin Season, ces actifs présentent un risque amplifié.</div>
    <div class="crypto-grid" id="unicornGrid"></div>
  </div>

  <!-- ====== CONFIG ====== -->
  <div class="section-title" style="margin-top:24px">// Configuration du profil</div>
  <div class="config-grid">
    <div class="config-card">
      <label>Horizon d'investissement</label>
      <select id="horizon">
        <option value="court">Court terme (1–7 jours)</option>
        <option value="moyen" selected>Moyen terme (1–3 mois)</option>
        <option value="long">Long terme (6–12 mois)</option>
      </select>
    </div>
    <div class="config-card">
      <label>Profil de risque</label>
      <select id="riskProfile">
        <option value="conservateur">Conservateur</option>
        <option value="equilibre" selected>Équilibré</option>
        <option value="agressif">Agressif</option>
      </select>
    </div>
    <div class="config-card">
      <label>Capital (€)</label>
      <input type="number" id="capital" value="1000" min="100" step="100">
    </div>
    <div class="config-card">
      <label>Allocation cryptos (%)</label>
      <input type="range" id="allocation" min="5" max="100" value="30">
      <div class="range-display" id="allocDisplay">30%</div>
    </div>
    <div class="config-card">
      <label>Stop-Loss (%)</label>
      <input type="range" id="stopLoss" min="3" max="30" value="10">
      <div class="range-display" id="stopDisplay">-10%</div>
    </div>
    <div class="config-card">
      <label>Take-Profit (%)</label>
      <input type="range" id="takeProfit" min="5" max="100" value="25">
      <div class="range-display" id="tpDisplay">+25%</div>
    </div>
  </div>

  <button class="generate-btn" onclick="generateStrategy()">
    <div class="shimmer"></div>Générer la stratégie
  </button>

  <div id="loading"><div class="loader"></div><div class="loading-text">Analyse en cours...</div></div>

  <!-- ====== STRATEGY OUTPUT ====== -->
  <div id="strategy-output">
    <div class="strategy-header">
      <div>
        <div style="font-family:'Space Mono',monospace;font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:8px">Stratégie recommandée</div>
        <div class="strategy-name" id="stratName"></div>
      </div>
      <div class="risk-badge" id="riskBadge"></div>
    </div>
    <div class="metrics-row">
      <div class="metric-card"><div class="label">Capital alloué</div><div class="value" id="metCapital" style="color:var(--accent2)"></div></div>
      <div class="metric-card"><div class="label">Stop-Loss</div><div class="value negative" id="metStopLoss"></div></div>
      <div class="metric-card"><div class="label">Take-Profit</div><div class="value positive" id="metTakeProfit"></div></div>
      <div class="metric-card"><div class="label">Score de confiance</div><div class="value" id="metConfidence"></div></div>
    </div>
    <div class="section-title">// Indicateurs techniques</div>
    <div class="indicators-grid" id="indicators"></div>
    <div class="strategy-grid">
      <div class="strategy-card"><h3>// Signaux d'achat</h3><div id="buySignals"></div></div>
      <div class="strategy-card"><h3>// Signaux de vente</h3><div id="sellSignals"></div></div>
    </div>
    <div class="timeline"><h3>// Plan d'action recommandé</h3><div class="timeline-steps" id="timelineSteps"></div></div>
    <div class="strategy-card" style="margin-bottom:40px"><h3>// Règles de gestion des risques</h3><div id="riskRules"></div></div>
  </div>

  <!-- ====== PLATFORMS ====== -->
  <div class="platforms-section">
    <div class="section-title">// Plateformes de trading recommandées</div>
    <div class="platforms-grid">
      <a href="https://www.coinbase.com" target="_blank" rel="noopener" class="platform-card cb">
        <div class="p-top"><div class="p-logo-wrap"><div class="p-emoji">🔵</div><div class="p-name">Coinbase</div></div><span class="p-rank rank-cb">Réglementé</span></div>
        <div class="p-desc">La plateforme la plus réglementée. Cotée au NASDAQ (COIN), idéale débutants et institutionnels. Disponible dans l'UE.</div>
        <div class="p-tags"><span class="p-tag">+250 Cryptos</span><span class="p-tag">Staking</span><span class="p-tag">Wallet</span><span class="p-tag">UE / USA</span></div>
        <div class="p-link p-link-cb">Accéder à Coinbase →</div>
      </a>
      <a href="https://www.binance.com" target="_blank" rel="noopener" class="platform-card bn">
        <div class="p-top"><div class="p-logo-wrap"><div class="p-emoji">🟡</div><div class="p-name">Binance</div></div><span class="p-rank rank-bn">Volume #1</span></div>
        <div class="p-desc">Le plus grand exchange mondial. 350+ cryptos, futures, staking et DeFi. Frais parmi les plus bas du marché.</div>
        <div class="p-tags"><span class="p-tag">350+ Cryptos</span><span class="p-tag">Futures</span><span class="p-tag">DeFi</span><span class="p-tag">App mobile</span></div>
        <div class="p-link p-link-bn">Accéder à Binance →</div>
      </a>
      <a href="https://www.kraken.com" target="_blank" rel="noopener" class="platform-card kr">
        <div class="p-top"><div class="p-logo-wrap"><div class="p-emoji">🐙</div><div class="p-name">Kraken</div></div><span class="p-rank rank-kr">Sécurité++</span></div>
        <div class="p-desc">Réputé pour sa sécurité exemplaire. Idéal pour les traders avancés en Europe. Frais compétitifs et large choix de paires.</div>
        <div class="p-tags"><span class="p-tag">Pro Trading</span><span class="p-tag">Futures</span><span class="p-tag">Europe</span><span class="p-tag">Staking</span></div>
        <div class="p-link p-link-kr">Accéder à Kraken →</div>
      </a>
    </div>
  </div>

  <footer>CryptoOracle v3.0 — Données : 19 Fév. 2026 — Outil éducatif uniquement — Ne constitue pas un conseil financier</footer>
</div>

<script>
// ===== DATA =====
const TOP50=[
  {sym:'BTC',name:'Bitcoin',icon:'₿',rank:1},{sym:'ETH',name:'Ethereum',icon:'Ξ',rank:2},
  {sym:'USDT',name:'Tether',icon:'₮',rank:3},{sym:'XRP',name:'Ripple',icon:'✕',rank:4},
  {sym:'BNB',name:'BNB',icon:'◆',rank:5},{sym:'SOL',name:'Solana',icon:'◎',rank:6},
  {sym:'USDC',name:'USD Coin',icon:'$',rank:7},{sym:'DOGE',name:'Dogecoin',icon:'Ð',rank:8},
  {sym:'ADA',name:'Cardano',icon:'₳',rank:9},{sym:'TRX',name:'TRON',icon:'Ŧ',rank:10},
  {sym:'AVAX',name:'Avalanche',icon:'🔺',rank:11},{sym:'LINK',name:'Chainlink',icon:'⬡',rank:12},
  {sym:'SHIB',name:'Shiba Inu',icon:'🐕',rank:13},{sym:'TON',name:'Toncoin',icon:'💎',rank:14},
  {sym:'DOT',name:'Polkadot',icon:'●',rank:15},{sym:'HBAR',name:'Hedera',icon:'ħ',rank:16},
  {sym:'SUI',name:'Sui',icon:'💧',rank:17},{sym:'BCH',name:'Bitcoin Cash',icon:'Ƀ',rank:18},
  {sym:'LTC',name:'Litecoin',icon:'Ł',rank:19},{sym:'UNI',name:'Uniswap',icon:'🦄',rank:20},
  {sym:'NEAR',name:'NEAR Protocol',icon:'Ⓝ',rank:21},{sym:'PEPE',name:'Pepe',icon:'🐸',rank:22},
  {sym:'APT',name:'Aptos',icon:'◉',rank:23},{sym:'ICP',name:'Internet Comp.',icon:'∞',rank:24},
  {sym:'ETC',name:'Ethereum Classic',icon:'♦',rank:25},{sym:'POL',name:'Polygon',icon:'⬟',rank:26},
  {sym:'RENDER',name:'Render',icon:'🎮',rank:27},{sym:'TAO',name:'Bittensor',icon:'τ',rank:28},
  {sym:'OM',name:'MANTRA',icon:'☯',rank:29},{sym:'ATOM',name:'Cosmos',icon:'⚛',rank:30},
  {sym:'IMX',name:'Immutable',icon:'🔷',rank:31},{sym:'FET',name:'Fetch.ai',icon:'🤖',rank:32},
  {sym:'OP',name:'Optimism',icon:'🔴',rank:33},{sym:'ARB',name:'Arbitrum',icon:'🔵',rank:34},
  {sym:'FLOKI',name:'Floki',icon:'⚡',rank:35},{sym:'INJ',name:'Injective',icon:'💉',rank:36},
  {sym:'STX',name:'Stacks',icon:'📦',rank:37},{sym:'BONK',name:'Bonk',icon:'🐶',rank:38},
  {sym:'FIL',name:'Filecoin',icon:'📁',rank:39},{sym:'ALGO',name:'Algorand',icon:'▲',rank:40},
  {sym:'VET',name:'VeChain',icon:'✔',rank:41},{sym:'GRT',name:'The Graph',icon:'📊',rank:42},
  {sym:'MKR',name:'Maker',icon:'Μ',rank:43},{sym:'AAVE',name:'Aave',icon:'👻',rank:44},
  {sym:'RUNE',name:'THORChain',icon:'⚡',rank:45},{sym:'SAND',name:'The Sandbox',icon:'🏖',rank:46},
  {sym:'MANA',name:'Decentraland',icon:'🌐',rank:47},{sym:'AXS',name:'Axie Infinity',icon:'🐾',rank:48},
  {sym:'EGLD',name:'MultiversX',icon:'⚙',rank:49},{sym:'FLOW',name:'Flow',icon:'💫',rank:50},
];
const UNICORNS=[
  {sym:'HYPE',name:'Hyperliquid',icon:'💹',why:'🔥 Trending #1 X — Perps DeFi décentralisés, volumes record fév. 2026'},
  {sym:'BERA',name:'Berachain',icon:'🐻',why:'🔥 X & Reddit — Nouveau L1, launch récent, forte communauté'},
  {sym:'ENA',name:'Ethena',icon:'🌐',why:'📈 Reddit — Stablecoin USDe, yield élevé, narratif RWA'},
  {sym:'ONDO',name:'Ondo Finance',icon:'🏦',why:'📈 Reddit finance — Tokenisation bons Trésor US'},
  {sym:'WIF',name:'dogwifhat',icon:'🎩',why:'🔥 Viral X Solana — Meme coin, forte traction communautaire'},
  {sym:'PENGU',name:'Pudgy Penguins',icon:'🐧',why:'📈 X — NFT → token, listing KuCoin & Binance Alpha'},
  {sym:'AI16Z',name:'ai16z',icon:'🤖',why:'🔥 X AI narrative — Agent IA investisseur autonome, DAO'},
  {sym:'VIRTUAL',name:'Virtuals Protocol',icon:'🎭',why:'📈 Reddit AI — Plateformes agents IA tokenisés'},
  {sym:'ZEREBRO',name:'Zerebro',icon:'🧠',why:'🔥 Viral X — Agent IA composant et vendant de la musique'},
  {sym:'TRUMP',name:'TRUMP Official',icon:'🇺🇸',why:'📈 X politique — Meme coin officiel Donald Trump'},
];

let selectedCrypto='BTC',selectedIsUnicorn=false;

function buildGrids(){
  document.getElementById('top50Grid').innerHTML=TOP50.map(c=>`
    <div class="crypto-btn${c.sym==='BTC'?' active':''}" data-sym="${c.sym}" data-name="${c.name.toLowerCase()}" data-uni="false" onclick="selectCrypto(this,false)">
      <span class="rank-badge">#${c.rank}</span>
      <span class="icon">${c.icon}</span><span class="sym">${c.sym}</span><span class="cname">${c.name}</span>
    </div>`).join('');
  document.getElementById('unicornGrid').innerHTML=UNICORNS.map(c=>`
    <div class="crypto-btn unicorn" data-sym="${c.sym}" data-name="${c.name.toLowerCase()}" data-uni="true" onclick="selectCrypto(this,true)" title="${c.why}">
      <span class="uni-corner">🦄</span>
      <span class="icon">${c.icon}</span><span class="sym">${c.sym}</span><span class="cname">${c.name}</span>
    </div>`).join('');
}

function selectCrypto(btn,isUni){
  document.querySelectorAll('.crypto-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  selectedCrypto=btn.dataset.sym;
  selectedIsUnicorn=isUni;
}
function switchTab(tab,btn){
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.crypto-panel').forEach(p=>p.classList.remove('active'));
  document.getElementById('panel-'+tab).classList.add('active');
}
function filterCryptos(q){
  q=q.toLowerCase().trim();
  document.querySelectorAll('.crypto-btn').forEach(btn=>{
    btn.style.display=(!q||btn.dataset.sym.toLowerCase().includes(q)||btn.dataset.name.includes(q))?'':'none';
  });
  if(q){
    const v50=[...document.querySelectorAll('#top50Grid .crypto-btn')].filter(b=>b.style.display!=='none').length;
    const vUni=[...document.querySelectorAll('#unicornGrid .crypto-btn')].filter(b=>b.style.display!=='none').length;
    if(v50===0&&vUni>0)switchTab('unicorns',document.querySelectorAll('.tab-btn')[1]);
    else if(vUni===0&&v50>0)switchTab('top50',document.querySelectorAll('.tab-btn')[0]);
  }
}

// ===== TICKER =====
function buildTicker(){
  const data=[
    {sym:'BTC',price:'$67,200',chg:'+0.5%',up:true},{sym:'ETH',price:'$1,981',chg:'-0.85%',up:false},
    {sym:'SOL',price:'$138',chg:'-1.3%',up:false},{sym:'XRP',price:'$1.68',chg:'-2.1%',up:false},
    {sym:'HYPE',price:'$16.8',chg:'+3.2%',up:true},{sym:'BNB',price:'$598',chg:'-0.5%',up:false},
    {sym:'DOGE',price:'$0.123',chg:'-3.4%',up:false},{sym:'WIF',price:'$0.98',chg:'+5.2%',up:true},
    {sym:'BERA',price:'$4.12',chg:'+2.7%',up:true},{sym:'ONDO',price:'$1.08',chg:'+1.4%',up:true},
    {sym:'AVAX',price:'$25',chg:'-1.7%',up:false},{sym:'ADA',price:'$0.52',chg:'-2.2%',up:false},
  ];
  const el=document.getElementById('ticker');
  let html='';
  for(let i=0;i<2;i++)data.forEach(d=>{
    html+=`<div class="ticker-item"><span class="name">${d.sym}/USDT</span><span class="price">${d.price}</span><span class="change ${d.up?'up':'down'}">${d.chg}</span></div>`;
  });
  el.innerHTML=html;
}

// ===== RANGES =====
document.getElementById('allocation').oninput=function(){document.getElementById('allocDisplay').textContent=this.value+'%';};
document.getElementById('stopLoss').oninput=function(){document.getElementById('stopDisplay').textContent='-'+this.value+'%';};
document.getElementById('takeProfit').oninput=function(){document.getElementById('tpDisplay').textContent='+'+this.value+'%';};

// ===== STRATEGY =====
function generateStrategy(){
  const loading=document.getElementById('loading'),output=document.getElementById('strategy-output');
  output.classList.remove('visible');loading.classList.add('visible');
  setTimeout(()=>{loading.classList.remove('visible');buildStrategy();output.classList.add('visible');output.scrollIntoView({behavior:'smooth'});},1500);
}
function sig(text,dot){return`<div class="signal-row"><div class="signal-dot ${dot}"></div><div class="signal-text">${text}</div></div>`;}

function buildStrategy(){
  const capital=parseFloat(document.getElementById('capital').value)||1000;
  const alloc=parseFloat(document.getElementById('allocation').value)/100;
  const stopLoss=parseFloat(document.getElementById('stopLoss').value);
  const takeProfit=parseFloat(document.getElementById('takeProfit').value);
  const horizon=document.getElementById('horizon').value;
  const risk=document.getElementById('riskProfile').value;
  const crypto=selectedCrypto,isUni=selectedIsUnicorn;
  const effRisk=isUni?'agressif':risk;
  const allocCap=(capital*alloc).toFixed(0);

  const strats={
    conservateur:{court:'DCA Défensif',moyen:'DCA + Hold Prudent',long:'HODL Stratégique'},
    equilibre:{court:'Swing Trading Modéré',moyen:'DCA + Swing Trading',long:'DCA Long Terme'},
    agressif:{court:'Day Trading Actif',moyen:'Momentum Trading',long:'Growth Agressif'},
  };
  document.getElementById('stratName').textContent=(isUni?'🦄 ':'')+strats[effRisk][horizon];
  const badge=document.getElementById('riskBadge');
  badge.textContent=isUni?'Agressif • Licorne':{conservateur:'Conservateur',equilibre:'Équilibré',agressif:'Agressif'}[risk];
  badge.className='risk-badge '+{conservateur:'risk-low',equilibre:'risk-mid',agressif:'risk-high'}[effRisk];
  document.getElementById('metCapital').textContent='€'+allocCap;
  document.getElementById('metStopLoss').textContent='-'+stopLoss+'%';
  document.getElementById('metTakeProfit').textContent='+'+takeProfit+'%';
  const conf=effRisk==='conservateur'?82:effRisk==='equilibre'?72:isUni?57:61;
  const confEl=document.getElementById('metConfidence');
  confEl.textContent=conf+'%';
  confEl.className='value '+(conf>=75?'positive':conf>=60?'warning':'negative');

  const rsi=effRisk==='conservateur'?41:effRisk==='equilibre'?52:65;
  const maUp=horizon!=='court';
  const fg=effRisk==='conservateur'?35:effRisk==='equilibre'?55:isUni?79:72;

  // Context Bitcoin Season warning
  const btcSeasonCtx=effRisk==='agressif'&&crypto!=='BTC'?
    sig(`⚠️ <strong>Bitcoin Season en cours (Index: 29/100)</strong> — La dominance BTC à 59,7% signifie que les altcoins surperforment rarement en ce moment. Réduisez votre exposition sur ${crypto} et privilégiez BTC ou stablecoins en attendant un retournement.`,'dot-warn'):'';

  const inds=[
    {l:'RSI (14)',v:rsi,s:rsi<40?'Survente — signal achat':rsi>60?'Surachat — prudence':'Zone neutre',cls:rsi<40?'ind-green':rsi>60?'ind-red':'ind-yellow',ico:'📊',col:rsi<40?'var(--up)':rsi>60?'var(--down)':'#ffc832'},
    {l:'MACD',v:maUp?'Haussier':'Neutre',s:'Signal de momentum '+(maUp?'positif':'neutre'),cls:maUp?'ind-green':'ind-yellow',ico:'📈',col:maUp?'var(--up)':'#ffc832'},
    {l:'Boll. Bands',v:rsi<45?'Survente':'Neutre',s:rsi<45?'Prix sous bande inf.':'Prix dans canal moyen',cls:rsi<45?'ind-green':'ind-yellow',ico:'〰️',col:rsi<45?'var(--up)':'#ffc832'},
    {l:'MA 50/200',v:maUp?'Haussier':'Neutre',s:horizon==='long'?'Golden Cross proche':'Tendance à surveiller',cls:maUp?'ind-green':'ind-yellow',ico:'🎯',col:maUp?'var(--up)':'#ffc832'},
    {l:'Alt. Season',v:'29/100',s:'Bitcoin Season — dominance 59.7%',cls:'ind-red',ico:'🟠',col:'#f7931a'},
    {l:'Fear & Greed',v:fg+'/100',s:fg<25?'😱 Extreme Fear':fg<50?'😨 Peur':fg>65?'😈 Cupidité':'😐 Neutre',cls:fg<40?'ind-green':fg>65?'ind-red':'ind-yellow',ico:'😨',col:fg<40?'var(--up)':fg>65?'var(--down)':'#ffc832'},
  ];
  document.getElementById('indicators').innerHTML=inds.map(i=>`
    <div class="indicator-card">
      <div class="indicator-icon ${i.cls}">${i.ico}</div>
      <div class="indicator-info"><div class="iname">${i.l}</div><div class="ival" style="color:${i.col}">${i.v}</div><div class="isig">${i.s}</div></div>
    </div>`).join('');

  const warnUni=isUni?sig(`⚠️ <strong>Licorne ${crypto}</strong> — Actif hautement spéculatif. Max 5–10% du portefeuille. En Bitcoin Season, risque amplifié.`,'dot-warn'):'';

  const buyMap={
    conservateur:[
      sig(`<strong>DCA progressif</strong> — Divisez €${allocCap} en 4 achats hebdomadaires de €${(allocCap/4).toFixed(0)}. Lissez le timing.`,'dot-buy'),
      sig(`<strong>Zone d'achat</strong> — Entrez quand RSI < 45 et prix sous MA20. Actuellement en zone de survente : favorable.`,'dot-buy'),
      sig(`<strong>Renforcement sur creux</strong> — Ajoutez +20% de position si baisse supplémentaire de 8%.`,'dot-hold'),
    ],
    equilibre:[
      sig(`<strong>Entrée initiale 50%</strong> — Investissez €${(allocCap/2).toFixed(0)} si RSI < 55. Gardez €${(allocCap/2).toFixed(0)} en réserve.`,'dot-buy'),
      sig(`<strong>Swing sur support</strong> — Achetez lors de retours sur support. Ciblez rebonds de +10 à +20%.`,'dot-buy'),
      sig(`<strong>Breakout avec volume</strong> — Ajoutez 25% si le prix casse une résistance avec fort volume.`,'dot-hold'),
    ],
    agressif:[
      sig(`<strong>Entrée rapide 60%</strong> — Investissez €${(allocCap*0.6).toFixed(0)} pour saisir le momentum. Réserve €${(allocCap*0.4).toFixed(0)}.`,'dot-buy'),
      sig(`<strong>Pyramiding</strong> — Ajoutez à chaque +5% de progression si les volumes confirment.`,'dot-buy'),
      sig(`<strong>Baleines on-chain</strong> — Surveillez les flux baleines pour valider l'entrée.`,'dot-hold'),
    ],
  };
  document.getElementById('buySignals').innerHTML=btcSeasonCtx+warnUni+buyMap[effRisk].join('');

  const sellMap={
    conservateur:[
      sig(`<strong>Prise de profit par paliers</strong> — Vendez 25% à +${(takeProfit/2).toFixed(0)}% et 25% à +${takeProfit}%.`,'dot-sell'),
      sig(`<strong>Stop-Loss strict à -${stopLoss}%</strong> — Clôturez immédiatement. Perte max sur cette position : €${(allocCap*stopLoss/100).toFixed(0)}.`,'dot-warn'),
      sig(`<strong>Rééquilibrage</strong> — Revenez à l'allocation cible si la position dépasse 40% du portefeuille.`,'dot-hold'),
    ],
    equilibre:[
      sig(`<strong>3 paliers TP</strong> — Vendez 33% à +${(takeProfit*0.6).toFixed(0)}%, +${takeProfit}%, et +${(takeProfit*1.5).toFixed(0)}%.`,'dot-sell'),
      sig(`<strong>Trailing Stop</strong> — Stop suiveur à -${Math.round(stopLoss*0.8)}% une fois en profit pour protéger les gains.`,'dot-warn'),
      sig(`<strong>Signal de retournement</strong> — Vendez si RSI > 75 puis redescend < 70 avec chandelier baissier.`,'dot-sell'),
    ],
    agressif:[
      sig(`<strong>Sortie rapide 50%</strong> — Prenez la moitié dès +${takeProfit}%. Laissez courir le reste.`,'dot-sell'),
      sig(`<strong>Stop-Loss ferme à -${stopLoss}%</strong> — Clôturez tout. Pas d'exception, pas d'espoir.`,'dot-warn'),
      sig(`<strong>Signal de distribution</strong> — Vendez si volume chute et prix stagne 3 jours.`,'dot-sell'),
    ],
  };
  document.getElementById('sellSignals').innerHTML=sellMap[effRisk].join('');

  const tls={
    court:[{s:'J.1',l:'Analyse',d:'Vérifier RSI/MACD',f:true},{s:'J.2',l:'Entrée',d:'Passer les ordres',f:true},{s:'J.4',l:'Suivi',d:'Ajuster les stops',f:false},{s:'J.7',l:'Sortie',d:'Réaliser profits',f:false}],
    moyen:[{s:'Sem 1',l:'Position',d:'1ère tranche achat',f:true},{s:'Sem 3',l:'Renforce',d:'2ème tranche',f:false},{s:'Sem 6',l:'Évaluation',d:'Revoir stratégie',f:false},{s:'Sem 12',l:'Sortie',d:'Prise de profits',f:false}],
    long:[{s:'M.1',l:'Accum.',d:'Commencer le DCA',f:true},{s:'M.3',l:'Consolidation',d:'Maintenir achats',f:false},{s:'M.6',l:'Croissance',d:'Évaluer + Hold',f:false},{s:'M.12',l:'Récolte',d:'Réaliser profits',f:false}],
  };
  document.getElementById('timelineSteps').innerHTML=tls[horizon].map(t=>`
    <div class="t-step"><div class="t-step-num">${t.s}</div><div class="t-dot${t.f?' filled':''}"></div><div class="t-label">${t.l}</div><div class="t-desc">${t.d}</div></div>`).join('');

  const rules=[
    sig(`<strong>Règle des 1-2%</strong> — Ne risquez jamais plus de 1–2% du capital total par trade. Ici : max €${(capital*0.02).toFixed(0)} de perte par position.`,'dot-warn'),
    sig(`<strong>Contexte marché</strong> — ${effRisk==='agressif'&&crypto!=='BTC'?'Bitcoin Season actif : surexposition aux altcoins déconseillée. Préférez BTC ou stablecoins.':'Bitcoin Season = favorisez BTC. Les altcoins performeront mieux une fois l\'index > 50.'}`,'dot-warn'),
    sig(`<strong>Discipline émotionnelle</strong> — Stop-loss à -${stopLoss}% à respecter impérativement. Perte acceptée : €${(allocCap*stopLoss/100).toFixed(0)} max.`,'dot-hold'),
    sig(`<strong>Liquidité</strong> — Conservez 20%+ en cash/stablecoins pour saisir les opportunités et gérer les imprévus.`,'dot-buy'),
    sig(`<strong>Revue hebdomadaire</strong> — Réévaluez chaque semaine. Surveiller en particulier l'indice Altcoin Season pour détecter une rotation.`,'dot-hold'),
  ];
  document.getElementById('riskRules').innerHTML=rules.join('');
}


// ===== NEWS FILTER =====
function filterNews(cat, btn) {
  document.querySelectorAll('.news-filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.news-card').forEach(card => {
    if (cat === 'all' || card.dataset.cat === cat) {
      card.classList.remove('hidden');
    } else {
      card.classList.add('hidden');
    }
  });
  // Featured card: only hide if not matching (keep grid layout)
  const featured = document.querySelector('.news-card.featured');
  if (featured && cat !== 'all' && featured.dataset.cat !== cat) {
    featured.style.gridColumn = '1';
  } else if (featured) {
    featured.style.gridColumn = '';
  }
}

buildGrids();
buildTicker();
</script>
</body>
</html>
