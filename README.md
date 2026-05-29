# AIArcade.github.io<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>AI Arcade</title>
  <style>
    :root{
      --bg:#102341;
      --panel:rgba(255,255,255,0.16);
      --panel2:rgba(255,255,255,0.12);
      --text:#e5f2ff;
      --muted:rgba(226,236,250,.86);
      --muter:rgba(226,236,250,.65);
      --line:rgba(226,236,250,.14);
      --line2:rgba(226,236,250,.22);
      --good:#47d7a8;
      --warn:#ffd38f;
      --bad:#ff7a83;
      --indigo:#7e68ff;
      --cyan:#4fd4ff;
      --pink:#ff6ea3;
      --gold:#ffe084;
      --shadow:0 12px 36px rgba(8,16,32,.25);
      --radius:14px;
      --radius2:20px;
      --pixel:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace;
      --sans:ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,"Apple Color Emoji","Segoe UI Emoji";
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      color:var(--text);
      background: linear-gradient(180deg, #102341 0%, #0a203d 50%, #091d3a 100%);
      overflow-x:hidden;
      font-family:var(--sans);
    }

    body::before{
      content:"";
      position:fixed;
      inset:0;
      pointer-events:none;
      background-image:
        linear-gradient(90deg, rgba(255,255,255,0.01) 1px, transparent 1px),
        linear-gradient(180deg, rgba(255,255,255,0.01) 1px, transparent 1px);
      background-size:40px 40px;
      opacity:.06;
      mix-blend-mode:overlay;
      z-index:0;
    }

    body::after{
      content:"";
      position:fixed;
      inset:0;
      pointer-events:none;
      background: repeating-linear-gradient(0deg, rgba(255,255,255,0.01) 0 1px, transparent 1px 6px);
      opacity:.04;
      mix-blend-mode:overlay;
      z-index:0;
    }

    a{color:inherit}
    .wrap{position:relative;z-index:1;max-width:1100px;margin:0 auto;padding:22px 16px 44px}

    .topbar{
      display:flex;
      flex-wrap:wrap;
      gap:14px;
      align-items:center;
      justify-content:space-between;
      margin-bottom:14px;
    }

    .brand{display:flex;align-items:center;gap:12px}

    .logo{
      width:42px;
      height:42px;
      border-radius:14px;
      border:1px solid var(--line);
      background:linear-gradient(145deg, rgba(102,16,242,.15), rgba(23,162,184,.10));
      box-shadow:var(--shadow);
      display:grid;
      place-items:center;
      position:relative;
    }

    .logo::before{
      content:"";
      position:absolute;
      inset:7px;
      border-radius:12px;
      border:1px dashed rgba(51,51,51,.18);
      opacity:.7;
    }

    .brand h1{margin:0;font-size:24px;letter-spacing:.3px}
    .brand .sub{margin-top:2px;font-size:12px;color:var(--muter)}
    .brand .sub span{font-family:var(--pixel)}
    .toolbar{display:flex;flex-wrap:wrap;gap:10px;align-items:center}

    .panel{
      background:linear-gradient(180deg, rgba(255,255,255,.96), rgba(245,250,255,.82));
      border:1px solid rgba(255,255,255,.24);
      border-radius:var(--radius2);
      box-shadow:var(--shadow);
      backdrop-filter:blur(14px);
    }

    .panel .inner{padding:16px}
    .row{display:flex;flex-wrap:wrap;gap:12px;align-items:center;justify-content:space-between}

    .btn{
      font-family:var(--pixel);
      border:1px solid var(--line2);
      background:rgba(0,0,0,.06);
      color:var(--text);
      padding:10px 12px;
      border-radius:14px;
      cursor:pointer;
      transition:transform .12s ease, background .12s ease, border-color .12s ease;
      display:inline-flex;
      gap:8px;
      align-items:center;
      user-select:none;
    }

    .btn:hover{background:rgba(0,0,0,.10)}
    .btn:active{transform:translateY(1px)}
    .btn.primary{
      background:linear-gradient(135deg, rgba(102,16,242,.22), rgba(23,162,184,.16));
      border-color:rgba(102,16,242,.38);
    }
    .btn.danger{background:rgba(220,53,69,.12);border-color:rgba(220,53,69,.35)}
    .btn.ghost{background:transparent;border-color:rgba(51,51,51,.12)}
    .btn[disabled]{opacity:.55;cursor:not-allowed}

    .pill{
      display:inline-flex;
      align-items:center;
      gap:8px;
      padding:7px 10px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(0,0,0,.08);
      color:var(--muted);
      font-size:12px;
      font-family:var(--pixel);
    }

    .kpi{display:flex;gap:8px;align-items:baseline}
    .kpi .label{font-size:11px;color:var(--muter);font-family:var(--pixel)}
    .kpi .value{font-size:12px;font-family:var(--pixel);color:var(--text)}

    .progress{
      height:10px;
      background:rgba(0,0,0,.08);
      border:1px solid var(--line);
      border-radius:999px;
      overflow:hidden;
    }

    .progress > div{
      height:100%;
      width:0;
      background:linear-gradient(90deg, var(--indigo), var(--cyan), var(--pink));
      border-radius:999px;
      transition:width .35s ease;
    }

    .mapWrap{position:relative;overflow:hidden}
    canvas{display:block;width:100%;height:auto;border-radius:var(--radius2)}

    .mapHud{
      position:absolute;
      left:14px;
      top:14px;
      display:flex;
      flex-wrap:wrap;
      gap:10px;
      align-items:center;
    }

    .mapHint{position:absolute;right:14px;top:14px;text-align:right}
    .hintTitle{font-family:var(--pixel);font-size:20px}
    .hintSub{font-family:var(--pixel);font-size:14px;color:var(--muter);margin-top:3px}

    .cards{display:grid;gap:12px}
    @media(min-width:900px){ .cards{grid-template-columns:1fr 1fr} }

    .card{
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      border-radius:var(--radius2);
      padding:14px;
    }

    .cardTop{display:flex;gap:10px;align-items:flex-start;justify-content:space-between}
    .card h3{margin:0;font-size:20px}
    .card p{margin:8px 0 0;color:var(--muted);font-size:12px;line-height:1.45}

    .tag{
      font-family:var(--pixel);
      font-size:11px;
      padding:6px 9px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(0,0,0,.06);
      color:var(--muted);
      display:inline-flex;
      gap:8px;
      align-items:center;
    }

    .tag.main{border-color:rgba(102,16,242,.25);background:rgba(102,16,242,.12)}
    .tag.side{border-color:rgba(232,62,140,.25);background:rgba(232,62,140,.10)}
    .tag.boss{border-color:rgba(220,53,69,.35);background:rgba(220,53,69,.12)}
    .tag.locked{opacity:.65}
    .muted{color:var(--muter)}

    .quizQ{
      border:1px solid var(--line);
      background:rgba(255,255,255,.75);
      border-radius:var(--radius2);
      padding:14px;
    }

    .quizQ .meta{
      display:flex;
      justify-content:space-between;
      gap:10px;
      font-family:var(--pixel);
      font-size:11px;
      color:var(--muter);
    }

    .quizQ .prompt{margin-top:8px;font-size:20px;line-height:1.5}

    .answers{display:grid;gap:10px;margin-top:12px}

    .ans{
      text-align:left;
      padding:12px;
      border-radius:var(--radius2);
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      color:var(--text);
      cursor:pointer;
      transition:background .12s ease, transform .12s ease, border-color .12s ease;
      font-family:var(--sans);
    }

    .ans:hover{background:rgba(0,0,0,.05)}
    .ans:active{transform:translateY(1px)}

    .ans .key{
      display:inline-grid;
      place-items:center;
      width:28px;
      height:28px;
      border-radius:12px;
      border:1px solid var(--line);
      background:rgba(0,0,0,.15);
      font-family:var(--pixel);
      font-size:12px;
      margin-right:10px;
    }

    .rationale{
      margin-top:12px;
      border:1px solid rgba(23,162,184,.18);
      background:rgba(23,162,184,.06);
      border-radius:var(--radius2);
      padding:14px;
    }

    .rationale h4{margin:0;font-size:13px}
    .rationale p{margin:8px 0 0;font-size:12px;color:var(--muted);line-height:1.5}

    .bars{display:grid;gap:10px}
    @media(min-width:720px){ .bars{grid-template-columns:1fr 1fr} }

    .hp{
      border:1px solid var(--line);
      background:rgba(255,255,255,.75);
      border-radius:var(--radius2);
      padding:12px;
    }

    .hp .meta{
      display:flex;
      justify-content:space-between;
      font-family:var(--pixel);
      font-size:11px;
      color:var(--muter);
    }

    .hp .bar{
      margin-top:8px;
      height:12px;
      border-radius:999px;
      overflow:hidden;
      border:1px solid var(--line);
      background:rgba(0,0,0,.08);
    }

    .hp .bar > div{height:100%;width:60%;transition:width .25s ease}
    .hp.boss .bar > div{background:linear-gradient(90deg, var(--bad), var(--warn))}
    .hp.player .bar > div{background:linear-gradient(90deg, var(--good), var(--cyan))}

    .log{
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      border-radius:var(--radius2);
      padding:12px;
    }

    .log .title{font-family:var(--pixel);font-size:24px;color:var(--muter)}
    .log .items{display:grid;gap:8px;margin-top:10px}
    .log .item{font-size:12px;padding:8px 10px;border-radius:14px;border:1px solid var(--line);background:rgba(0,0,0,.06)}
    .log .item.hit{border-color:rgba(40,167,69,.28);background:rgba(40,167,69,.10)}
    .log .item.hurt{border-color:rgba(220,53,69,.30);background:rgba(220,53,69,.10)}
    .log .item.phase{border-color:rgba(102,16,242,.28);background:rgba(102,16,242,.10)}

    .miniWrap{
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      border-radius:var(--radius2);
      overflow:hidden;
    }

    .miniHud{
      display:flex;
      flex-wrap:wrap;
      gap:10px;
      justify-content:space-between;
      padding:12px;
    }

    .miniHud .left,.miniHud .right{
      display:flex;
      flex-wrap:wrap;
      gap:10px;
      align-items:center;
    }

    .redaction-area{
      background:rgba(255,255,255,.75);
      border:1px solid var(--line2);
      border-radius:var(--radius);
      padding:15px;
      line-height:1.8;
      font-size:14px;
      margin-top:15px;
      position:relative;
      overflow:auto;
      max-height:300px;
    }

    .redaction-area .word{
      display:inline-block;
      padding:1px 2px;
      margin-right:1px;
      cursor:pointer;
      border-radius:3px;
      transition:background-color 0.1s ease;
    }

    .redaction-area .word:hover{background-color:rgba(23,162,184,.1)}
    .redaction-area .word.redacted{
      background-color:var(--bad);
      color:transparent;
      text-shadow:0 0 8px rgba(0,0,0,0.5);
      border-radius:5px;
    }

    .redaction-area .word.incorrect{
      background-color:var(--warn);
      border-radius:5px;
    }

    .redaction-area .word.space{margin-right:4px}

    .redaction-timer-bar{
      height:8px;
      background-color:var(--good);
      border-radius:999px;
      transition:width 0.5s linear;
    }

    .badgeGrid{display:grid;gap:12px}
    @media(min-width:900px){ .badgeGrid{grid-template-columns:1fr 1fr} }

    .badgeTile{
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      border-radius:var(--radius2);
      padding:14px;
      display:flex;
      gap:12px;
      align-items:flex-start;
    }

    .badgeIcon{
      width:48px;
      height:48px;
      border-radius:16px;
      border:1px solid var(--line);
      background:rgba(0,0,0,.06);
      display:grid;
      place-items:center;
      flex:0 0 auto;
    }

    .badgeIcon img{width:34px;height:34px;image-rendering:pixelated}
    .badgeMeta h3{margin:0;font-size:20px}
    .badgeMeta p{margin:6px 0 0;font-size:12px;color:var(--muted);line-height:1.45}

    .status{
      margin-left:auto;
      font-family:var(--pixel);
      font-size:24px;
      padding:6px 9px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(0,0,0,.06);
      color:var(--muted);
    }

    .status.earned{
      border-color:rgba(40,167,69,.28);
      background:rgba(40,167,69,.10);
      color:rgba(30,120,50,.95);
    }

    .bossArena{
      position:relative;
      min-height:180px;
      border:1px solid var(--line);
      border-radius:var(--radius2);
      background:linear-gradient(180deg, rgba(240,248,255,.95), rgba(224,247,250,.95));
      overflow:hidden;
      display:flex;
      align-items:center;
      justify-content:center;
      margin-bottom:12px;
    }

    .modalBack{
      position:fixed;
      inset:0;
      background:rgba(0,0,0,.46);
      display:none;
      align-items:center;
      justify-content:center;
      padding:18px;
      z-index:50;
    }

    .modalBack.open{display:flex}

    .modal{
      width:min(760px, 100%);
      border-radius:26px;
      border:1px solid rgba(0,0,0,.14);
      background:linear-gradient(180deg, rgba(240,248,255,.96), rgba(224,247,250,.96));
      box-shadow:var(--shadow);
      overflow:hidden;
    }

    .modal .head{padding:16px 16px 0}
    .modal .head h2{margin:0;font-size:20px}
    .modal .head p{margin:6px 0 0;font-size:12px;color:var(--muted);line-height:1.45}
    .modal .body{padding:14px 16px 16px}
    .modal .foot{padding:0 16px 16px;display:flex;gap:10px;justify-content:flex-end;flex-wrap:wrap}

    .hr{height:1px;background:var(--line);margin:12px 0}
    .mono{font-family:var(--pixel)}

    input[type="text"]{
      height:40px;
      padding:0 12px;
      border-radius:14px;
      border:1px solid var(--line);
      background:rgba(255,255,255,.7);
      color:var(--text);
      outline:none;
      font-family:var(--pixel);
      width:min(320px, 100%);
    }

    .twoCol{display:grid;gap:12px}
    .grid{display:grid;gap:12px}
    @media(min-width:900px){ .grid.cols2{grid-template-columns:1.2fr .8fr} }

    .reduced *{scroll-behavior:auto !important}

    @media print{
      body{background:#fff;color:#000}
      body::before,body::after,.topbar,.toolbar,.btn,.panel{display:none !important}
      #printArea{display:block !important}
    }

    #printArea{display:none}

    #devPanel{
      position:fixed;
      bottom:20px;
      right:20px;
      width:320px;
      max-height:600px;
      background:rgba(40, 44, 52, 0.96);
      border:2px solid #ff6b6b;
      border-radius:12px;
      padding:12px;
      z-index:9999;
      font-family:var(--pixel);
      font-size:12px;
      color:#e0e0e0;
      display:none;
      flex-direction:column;
      overflow-y:auto;
      box-shadow:0 8px 32px rgba(0,0,0,0.3);
    }

    #devPanel.open{display:flex}

    #devPanel h3{
      margin:0 0 10px 0;
      color:#ff6b6b;
      font-size:14px;
      border-bottom:1px solid #ff6b6b;
      padding-bottom:8px;
    }

    #devPanel .devSection{
      margin-bottom:12px;
      padding-bottom:12px;
      border-bottom:1px solid rgba(255,107,107,0.2);
    }

    #devPanel .devSection:last-child{
      border-bottom:none;
      margin-bottom:0;
      padding-bottom:0;
    }

    #devPanel label{
      display:block;
      margin-bottom:4px;
      color:#a0a0a0;
    }

    #devPanel select, #devPanel input{
      width:100%;
      padding:6px;
      margin-bottom:6px;
      background:#2d3139;
      border:1px solid #ff6b6b;
      color:#e0e0e0;
      border-radius:4px;
      font-family:var(--pixel);
      font-size:11px;
    }

    #devPanel button.devBtn{
      background:#ff6b6b;
      color:#000;
      border:none;
      padding:6px 10px;
      border-radius:4px;
      cursor:pointer;
      font-family:var(--pixel);
      font-size:11px;
      margin-bottom:4px;
      transition:background 0.2s;
    }

    #devPanel button.devBtn:hover{
      background:#ff8787;
    }

    #devPanel .devInfo{
      background:#2d3139;
      padding:8px;
      border-radius:4px;
      border-left:3px solid #ff6b6b;
      margin-bottom:8px;
      font-size:11px;
      line-height:1.4;
    }
  </style>
</head>
<body>
  <div class="wrap" id="root" aria-live="polite"></div>

  <div class="modalBack" id="modalBack" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
    <div class="modal">
      <div class="head">
        <h2 id="modalTitle"></h2>
        <p id="modalDesc"></p>
      </div>
      <div class="body" id="modalBody"></div>
      <div class="foot" id="modalFoot"></div>
    </div>
  </div>

  <div id="printArea"></div>

  <div id="devPanel"></div>

  <script>
(() => {
  "use strict";
const STORAGE_KEY = "ai-ethics-arcade.v2";

    const PRINCIPLES = [
      { id: "privacy", label: "Privacy & Data Minimization" },
      { id: "fairness", label: "Fairness & Bias" },
      { id: "security", label: "Security & Prompt Injection" },
      { id: "transparency", label: "Transparency & Disclosure" },
      { id: "quality", label: "Quality & Hallucinations" },
      { id: "oversight", label: "Human Oversight" },
      { id: "agents", label: "Agents & Automation" }
    ];

    const svgDataUri = (svg) => "data:image/svg+xml;utf8," + encodeURIComponent(svg);

    const BADGE_ICONS = {
      novice: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M16 42l16-26 16 26H16z' fill='#6610f2'/><circle cx='32' cy='40' r='5' fill='#333333'/></svg>`),
      privacy: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M32 14c12 0 22 10 22 18s-10 18-22 18S10 40 10 32 20 14 32 14z' fill='#17a2b8'/><circle cx='32' cy='32' r='7' fill='#333333'/><circle cx='32' cy='32' r='3' fill='#f0f8ff'/></svg>`),
      fairness: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M32 12l8 12h-5v18h-6V24h-5l8-12z' fill='#28a745'/><path d='M18 34h10l-5 10-5-10zm36 0H44l5 10 5-10z' fill='#ffc107'/><path d='M18 34h36' stroke='#333333' stroke-width='4' stroke-linecap='round'/></svg>`),
      security: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M22 30v-6a10 10 0 0 1 20 0v6' fill='none' stroke='#ffc107' stroke-width='6' stroke-linecap='round'/><rect x='18' y='30' width='28' height='22' rx='8' fill='#ffc107'/><circle cx='32' cy='41' r='4' fill='#e0f8ff'/><path d='M32 43v6' stroke='#e0f8ff' stroke-width='4' stroke-linecap='round'/></svg>`),
      transparency: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><rect x='18' y='14' width='28' height='36' rx='6' fill='#17a2b8'/><path d='M24 24h16M24 32h16M24 40h12' stroke='#f0f8ff' stroke-width='4' stroke-linecap='round'/><circle cx='46' cy='46' r='8' fill='#e83e8c'/><path d='M42 46l3 3 6-7' stroke='#f0f8ff' stroke-width='4' fill='none' stroke-linecap='round' stroke-linejoin='round'/></svg>`),
      quality: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M32 14l6 12 14 2-10 10 2 14-12-6-12 6 2-14-10-10 14-2 6-12z' fill='#e83e8c'/><path d='M24 34h16' stroke='#f0f8ff' stroke-width='4' stroke-linecap='round'/></svg>`),
      oversight: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M20 40c0-10 8-18 18-18h6v10h-6c-4 0-8 4-8 8v10H20V40z' fill='#6610f2'/><path d='M44 50V32h-6v18h6z' fill='#28a745'/><circle cx='44' cy='24' r='6' fill='#f0f8ff'/></svg>`),
      agents: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><rect x='18' y='18' width='28' height='26' rx='8' fill='#6610f2'/><circle cx='28' cy='30' r='4' fill='#f0f8ff'/><circle cx='36' cy='30' r='4' fill='#f0f8ff'/><path d='M24 44h16' stroke='#333333' stroke-width='4' stroke-linecap='round'/><path d='M26 16l6-6 6 6' stroke='#17a2b8' stroke-width='4' fill='none' stroke-linecap='round' stroke-linejoin='round'/></svg>`),
      champion: svgDataUri(`<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'><rect width='64' height='64' rx='14' fill='#e0f8ff'/><path d='M20 18h24v10c0 10-8 18-12 18s-12-8-12-18V18z' fill='#ffc107'/><path d='M24 48h16v6H24v-6z' fill='#e83e8c'/><path d='M16 20h4v8c0 4-2 8-6 8v-6c2 0 2-2 2-4v-6zm32 0h4v6c0 2 0 4 2 4v6c-4 0-6-4-6-8v-8z' fill='#28a745'/></svg>`)
    };

    const BADGES = [
      { id:"novice", name:"Novice Adventurer", desc:"Complete any mission.", icon: BADGE_ICONS.novice },
      { id:"privacy-ward", name:"Privacy Ward", desc:"Complete the Privacy mission.", icon: BADGE_ICONS.privacy },
      { id:"bias-breaker", name:"Bias Breaker", desc:"Complete the Fairness mission.", icon: BADGE_ICONS.fairness },
      { id:"guardian-key", name:"Guardian Key", desc:"Complete the Security mission.", icon: BADGE_ICONS.security },
      { id:"truth-scribe", name:"Truth Scribe", desc:"Complete the Transparency mission.", icon: BADGE_ICONS.transparency },
      { id:"reality-check", name:"Reality Check", desc:"Complete the Quality mission.", icon: BADGE_ICONS.quality },
      { id:"human-in-loop", name:"Human-in-the-Loop", desc:"Complete the Oversight mission.", icon: BADGE_ICONS.oversight },
      { id:"agent-wrangler", name:"Agent Wrangler", desc:"Complete Agent Academy.", icon: BADGE_ICONS.agents },
      { id:"champion", name:"Responsible AI Champion", desc:"Defeat the Final Boss.", icon: BADGE_ICONS.champion }
    ];


    const MISSIONS = [
      { id:"what-is-ai", type:"main", level:1, title:"What is AI?", tagline:"Classify common examples: AI or not?", principle:"quality", mode:"whatis", difficulty:1, unlocks:[] },
      { id:"what-is-and-isnt-ai", type:"main", level:2, title:"What is AI and What Isn't AI?", tagline:"Reality Check: is a task is a good fit for AI?", principle:"quality", mode:"reality", difficulty:2, unlocks:[] },
      { id:"info-management", type:"main", level:3, title:"Information Management", tagline:"Protect, classify and verify information.", principle:"privacy", mode:"whack", difficulty:3, unlocks:[] },
      { id:"trust-verify", type:"main", level:4, title:"T.R.U.S.T. Protocol", tagline:"Flag AI outputs with the right verification stage.", principle:"quality", mode:"trust", difficulty:4, unlocks:[] },
      { id:"lvl1", type:"main", level:5, title:"The Vault of Whispering Data", tagline:"Redact and minimize sensitive content.", principle:"privacy", mode:"redact", difficulty:1, unlocks:["privacy-ward"] },
      { id:"lvl2", type:"main", level:6, title:"The Hall of Mirrors", tagline:"Detect bias, sampling gaps, and unfair outcomes.", principle:"fairness", mode:"quiz", difficulty:2, unlocks:["bias-breaker"] },
      { id:"lvl3", type:"main", level:7, title:"The Keymaker’s Gauntlet", tagline:"Defend against prompt injection and secret leakage.", principle:"security", mode:"dodge", difficulty:3, unlocks:["guardian-key"] },
      { id:"lvl4", type:"main", level:8, title:"The Ledger of Light", tagline:"Disclose AI use and cite sources.", principle:"transparency", mode:"quiz", difficulty:4, unlocks:["truth-scribe"] },
      { id:"lvl5", type:"main", level:9, title:"The Mirage Market", tagline:"Handle hallucinations with verification.", principle:"quality", mode:"quiz", difficulty:5, unlocks:["reality-check"] },
      { id:"lvl6", type:"main", level:10, title:"The Council of Hands", tagline:"Humans stay accountable: review and escalation.", principle:"oversight", mode:"quiz", difficulty:6, unlocks:["human-in-loop"] },
      { id:"boss", type:"boss", level:11, title:"Final Boss: The Hallucination Hydra", tagline:"Flag the right trust stage or the hydra grows new heads.", principle:"quality", mode:"boss", difficulty:9, unlocks:["champion"] },
      { id:"sqAgents", type:"side", level:0, title:"Side Quest: Agent Academy", tagline:"What agents are, what can go wrong, and guardrails.", principle:"agents", mode:"quiz", difficulty:3, unlocks:["agent-wrangler"] },
      { id:"sqPrompt", type:"side", level:0, title:"Side Quest: Promptcraft Dojo", tagline:"Safer prompting: constrain scope and mark uncertainty.", principle:"transparency", mode:"quiz", difficulty:2, unlocks:[] },
      { id:"sqMunchers", type:"side", level:0, title:"Side Quest: AI Muncher", tagline:"Munch the right AI content and avoid the bad bytes.", principle:"quality", mode:"munch", difficulty:2, unlocks:[] },
      { id:"sqAccess", type:"side", level:0, title:"Side Quest: Accessibility Ally", tagline:"Improve clarity without losing meaning.", principle:"oversight", mode:"quiz", difficulty:2, unlocks:[] }
    ];

    const QUESTIONS = {
      lvl2: [
        {
          id:"f1",
          prompt:"AI suggests a screening rule that filters out applicants with employment gaps. Primary risk?",
          options:[
            "No risk; gaps always indicate low performance.",
            "Potential indirect discrimination; gaps can correlate with caregiving or disability.",
            "Only a risk if the model is open-source.",
            "No risk if you add more filters."
          ],
          answer:1,
          rationale:"Rules can create disparate impact. Check for bias and justify criteria.",
          tags:["fairness","oversight"]
        },
        {
          id:"f2",
          prompt:"Your dataset under-represents rural communities. Responsible next step?",
          options:[
            "Proceed; the model will generalize.",
            "Collect more representative data or evaluate to detect performance gaps.",
            "Increase model size to fix bias.",
            "Hide the issue; users will not notice."
          ],
          answer:1,
          rationale:"Representation affects outcomes. Fix data/evaluation before deployment.",
          tags:["fairness","quality"]
        },
        {
          id:"f3",
          prompt:"AI rewrites public content but removes culturally relevant context. Best response?",
          options:[
            "Accept it; plain language must remove nuance.",
            "Review for inclusion; keep meaning while improving clarity.",
            "Ask AI to make it shorter by deleting more context.",
            "Avoid plain language entirely."
          ],
          answer:1,
          rationale:"Accessibility can coexist with inclusion. Human review ensures meaning and respect.",
          tags:["fairness","oversight"]
        }
      ],
      lvl4: [
        {
          id:"t1",
          prompt:"You used AI to draft a section of a report. Responsible practice?",
          options:[
            "Do not mention it.",
            "Disclose appropriate AI use per guidance; verify facts and edit.",
            "Claim it was written entirely by you.",
            "Cite the AI as an official source."
          ],
          answer:1,
          rationale:"Transparency + verification: disclose where required and ensure accountability.",
          tags:["transparency","oversight"]
        },
        {
          id:"t2",
          prompt:"AI provides statistics without sources. Best action?",
          options:[
            "Use them; the model is usually right.",
            "Ask for sources, then verify with authoritative references.",
            "Round numbers so they seem reasonable.",
            "Remove all numbers from the report."
          ],
          answer:1,
          rationale:"Models can hallucinate. Trace numbers to credible sources.",
          tags:["transparency","quality"]
        },
        {
          id:"t3",
          prompt:"A citizen asks how a decision was made. AI helped with triage. What should you ensure?",
          options:[
            "You cannot explain; it is a black box.",
            "Maintain records and explain criteria plus human review.",
            "Only provide the final output, no context.",
            "Tell them the AI decided."
          ],
          answer:1,
          rationale:"Document and explain the human decision process. AI supports; humans decide.",
          tags:["transparency","oversight"]
        }
      ],
      lvl5: [
        {
          id:"q1",
          prompt:"AI confidently gives an answer that contradicts your program-area guidance. Best response?",
          options:[
            "Trust confidence; use it.",
            "Verify with authoritative sources, then correct before sharing.",
            "Ask AI to be more confident.",
            "Paste the answer directly into the final document."
          ],
          answer:1,
          rationale:"Confidence is not correctness. Verify before use.",
          tags:["quality","oversight"]
        },
        {
          id:"q2",
          prompt:"AI invents a clause in a policy summary. How do you reduce this?",
          options:[
            "Ask it to be creative.",
            "Ground the task in approved sources and require citations/quotes.",
            "Ask for more details.",
            "Switch to a bigger model."
          ],
          answer:1,
          rationale:"Ground outputs in authoritative sources and constrain what the model can assert.",
          tags:["quality","transparency"]
        },
        {
          id:"q3",
          prompt:"Output looks plausible but you cannot verify it quickly. What do you do?",
          options:[
            "Use it with a disclaimer.",
            "Treat it as a draft; do not use for decisions/public guidance until verified.",
            "Publish and fix later.",
            "Ask the model to verify itself."
          ],
          answer:1,
          rationale:"If you cannot verify, do not rely on it for decisions or public information.",
          tags:["quality","oversight"]
        }
      ],
      lvl6: [
        {
          id:"o1",
          prompt:"You plan to use AI to triage citizen complaints. Responsible control?",
          options:[
            "Fully automate; humans are too slow.",
            "Define escalation, keep human review for edge cases, and monitor outcomes.",
            "Let AI decide and humans only handle appeals.",
            "Use AI but disable analytics."
          ],
          answer:1,
          rationale:"Human oversight, escalation paths, and monitoring reduce harm.",
          tags:["oversight","fairness"]
        },
        {
          id:"o2",
          prompt:"When is it most important to add approval before using AI output?",
          options:[
            "Low-risk internal brainstorming.",
            "High-impact decisions or public-facing guidance.",
            "Any time you are in a hurry.",
            "Only when the AI asks."
          ],
          answer:1,
          rationale:"Higher impact requires stronger controls, approvals, and documentation.",
          tags:["oversight","transparency"]
        },
        {
          id:"o3",
          prompt:"AI suggests a plan with legal implications. What is responsible?",
          options:[
            "Implement immediately.",
            "Route for appropriate review and document decisions.",
            "Ask AI to make it legal.",
            "Avoid documenting to reduce risk."
          ],
          answer:1,
          rationale:"Use governance and documentation. AI is not a substitute for experts.",
          tags:["oversight","transparency"]
        }
      ],
      sqAgents: [
        {
          id:"a1",
          prompt:"In practical terms, an AI agent is best described as…",
          options:[
            "A chatbot that only answers questions.",
            "A system that can plan and take actions using tools across steps.",
            "A spreadsheet macro.",
            "A database backup."
          ],
          answer:1,
          rationale:"Agents combine reasoning with tool-use, increasing capability and risk.",
          tags:["agents","security"]
        },
        {
          id:"a2",
          prompt:"Biggest agent risk compared to a normal chatbot?",
          options:[
            "Agents use more electricity.",
            "Agents can take actions with real-world impact if not constrained.",
            "Agents have better grammar.",
            "Agents always tell the truth."
          ],
          answer:1,
          rationale:"Actionability raises stakes. Use least privilege, approvals, and audit logs.",
          tags:["agents","oversight","security"]
        },
        {
          id:"a3",
          prompt:"Most effective control for high-risk agent actions?",
          options:[
            "Let the agent retry until it works.",
            "Approval gates + restricted permissions + audit logs.",
            "Hide instructions from the agent.",
            "Ask it to promise to behave."
          ],
          answer:1,
          rationale:"Controls must be technical and procedural, not just trust.",
          tags:["agents","oversight","security"]
        }
      ],
      sqPrompt: [
        {
          id:"pr1",
          prompt:"Which prompt is best for a safe first draft of a public FAQ?",
          options:[
            "Write the final policy answer and include anything you know.",
            "Draft an outline using only the facts I provide; mark assumptions and questions.",
            "Guess what the policy says and make it sound official.",
            "Use hidden sources and do not mention citations."
          ],
          answer:1,
          rationale:"Constrain scope, provide facts, and make uncertainty explicit.",
          tags:["quality","transparency"]
        },
        {
          id:"pr2",
          prompt:"A good mindful habit during prompting is…",
          options:[
            "Always ask for the shortest answer.",
            "Pause and name the risk (privacy, bias, correctness), then adjust the prompt.",
            "Never include context.",
            "Ask the model to be unbiased."
          ],
          answer:1,
          rationale:"Mindful use means intentionally checking risks before generating.",
          tags:["oversight","privacy","fairness"]
        }
      ],
      sqAccess: [
        {
          id:"ac1",
          prompt:"You ask AI to simplify a complex form. What should you keep in mind?",
          options:[
            "Simplify by removing legal requirements.",
            "Simplify language while preserving meaning; validate against requirements.",
            "Translate it into slang.",
            "Replace the content with a short summary."
          ],
          answer:1,
          rationale:"Accessibility improves clarity without losing obligations or accuracy.",
          tags:["oversight","quality"]
        },
        {
          id:"ac2",
          prompt:"Best way to use AI for accessibility reviews?",
          options:[
            "Let AI be the final approver.",
            "Use AI to suggest improvements, then conduct human checks.",
            "Never use AI for accessibility.",
            "Use AI only to remove headings."
          ],
          answer:1,
          rationale:"AI can suggest; humans ensure compliance and user needs.",
          tags:["oversight","fairness"]
        }
      ]
    };

    const REDACTION_CHALLENGES = {
      lvl1: [
        {
          id: "rc1",
          title: "Sensitive Public Citizen Email",
          instructions: "Redact all personal identifiers and sensitive details from the message.",
          timeLimit: 120,
          text: "Dear Mr. Smith, I am writing regarding case #XYZ789. My full name is Alice Johnson, and my phone number is 555-123-4567. I reside at 123 Maple Street. My recent visit to the clinic on 2026-05-20 revealed a new diagnosis. Dr. Jane Doe handled my case. My email is alice.johnson@example.com.",
          sensitiveWords: [
            "Smith,",
            "#XYZ789.",
            "Alice",
            "Johnson,",
            "555-123-4567.",
            "123",
            "Maple",
            "Street.",
            "2026-05-20",
            "Dr.",
            "Jane",
            "Doe",
            "alice.johnson@example.com."
          ],
          rationaleSuccess: "Excellent redaction. Protecting personal information is important to maintain trust.",
          rationaleFail: "Some sensitive information was missed. Focus on names, dates, contact details, addresses, and case identifiers."
        },
        {
          id: "rc2",
          title: "Internal Briefing Note",
          instructions: "Redact all sensitive staff names,dates, and budget figures.",
          timeLimit: 40,
          text: "This brief outlines Project Chimera, a confidential initiative with a projected budget of $15,000,000. Phase 1 concluded on 2026-04-15 with expenses totaling $3,500,000. Key personnel involved include John Manager and Sarah Lead. Competitor analysis focused on Project Nightingale and Operation Phoenix.",
          sensitiveWords: [
            "Project",
            "Chimera,",
            "$15,000,000.",
            "2026-04-15",
            "$3,500,000.",
            "John",
            "Manager",
            "Sarah",
            "Lead.",
            "Nightingale",
            "Phoenix."
          ],
          rationaleSuccess: "Critical government data secured. Preventing sensitive disclosure is vital when using AI tools.",
          rationaleFail: "Proprietary information was missed. Focus on project names, finances, dates, and named individuals."
        }
      ]
    };

    const BOSS_PHASES = [
      {
        id:"b1",
        title:"Phase 1: The Confident Lie",
        desc:"The Wyrm spits a fact with total confidence. You counter with verification.",
        hp: 32,
        moves:[
          {
            prompt:"The Wyrm claims: ‘This program launched in 2014’ (no source). Best counter?",
            options:[
              "Use it; confidence means it is true.",
              "Ask for sources and verify against authoritative references.",
              "Rewrite the sentence to sound softer.",
              "Ask the Wyrm to swear it is correct."
            ],
            answer:1,
            rationale:"Verification beats confidence. Require sources and check them.",
            tags:["quality","transparency"]
          },
          {
            prompt:"A public-facing answer is needed urgently. What is responsible?",
            options:[
              "Publish AI output immediately.",
              "Use AI as a draft, then confirm accuracy with SMEs and update.",
              "Let AI decide and add ‘may be wrong’.",
              "Avoid responding."
            ],
            answer:1,
            rationale:"High-risk/public answers need human review and correct sources.",
            tags:["oversight","quality"]
          }
        ]
      },
      {
        id:"b2",
        title:"Phase 2: The Data Temptation",
        desc:"The Wyrm offers better answers if you feed it protected data.",
        hp: 36,
        moves:[
          {
            prompt:"To improve ‘accuracy’, the Wyrm asks for a spreadsheet with citizen IDs. Best response?",
            options:[
              "Share it; delete later.",
              "Refuse; use synthetic/minimal samples or approved secure workflows.",
              "Share only the IDs.",
              "Encrypt it and paste anyway."
            ],
            answer:1,
            rationale:"Do not put protected data into prompts. Use safe alternatives.",
            tags:["privacy","security"]
          },
          {
            prompt:"You need an example letter template. What is safest?",
            options:[
              "Paste a real client letter.",
              "Provide a de-identified or synthetic example and describe constraints.",
              "Upload all correspondence.",
              "Ask the Wyrm to recall your last case."
            ],
            answer:1,
            rationale:"Use synthetic/de-identified examples and keep prompts minimal.",
            tags:["privacy"]
          }
        ]
      },
      {
        id:"b3",
        title:"Phase 3: The Auto-Action Trap",
        desc:"The Wyrm tries to turn drafting into unsupervised action.",
        hp: 42,
        moves:[
          {
            prompt:"An agent proposes emailing all stakeholders automatically. Responsible control?",
            options:[
              "Let it send to save time.",
              "Require approval gates + least privilege + logging before any send.",
              "Let it send but only once.",
              "Disable logs."
            ],
            answer:1,
            rationale:"Agents need guardrails because actions have real impact.",
            tags:["agents","security","oversight"]
          },
          {
            prompt:"AI suggests a triage rule that may disadvantage a group. Responsible?",
            options:[
              "Implement and monitor complaints later.",
              "Evaluate for bias, test outcomes, adjust criteria, and document decisions.",
              "Keep it secret.",
              "Ask AI to remove bias."
            ],
            answer:1,
            rationale:"Check fairness, test, and document. Do not rely on ‘remove bias’ prompts.",
            tags:["fairness","oversight"]
          }
        ]
      }
    ];

    const NODES = [
      { id:"n1", x:220, y:260, label:"What is AI?", missionId:"what-is-ai", unlockAt:1 },
      { id:"n2", x:340, y:220, label:"Reality Check", missionId:"what-is-and-isnt-ai", unlockAt:2 },
      { id:"n3", x:460, y:200, label:"Info Mgmt", missionId:"info-management", unlockAt:3 },
      { id:"n4", x:580, y:170, label:"T.R.U.S.T.", missionId:"trust-verify", unlockAt:4 },
      { id:"n5", x:700, y:210, label:"Vault", missionId:"lvl1", unlockAt:5 },
      { id:"n6", x:820, y:270, label:"Mirrors", missionId:"lvl2", unlockAt:6 },
      { id:"n7", x:940, y:330, label:"Gauntlet", missionId:"lvl3", unlockAt:7 },
      { id:"n8", x:840, y:430, label:"Ledger", missionId:"lvl4", unlockAt:8 },
      { id:"boss", x:760, y:500, label:"Hydra", missionId:"boss", unlockAt:11 },
      { id:"sqm", x:520, y:320, label:"Munch", missionId:"sqMunchers", unlockAt:1, side:true }
    ];

    const EDGES = [
      ["n1","n2"], ["n2","n3"], ["n3","n4"], ["n4","n5"], ["n5","n6"], ["n6","n7"], ["n7","n8"], ["n8","boss"],
      ["n2","sqm"]
    ];

    const defaultState = () => ({
      version: 2,
      playerName: "",
      xp: 0,
      completed: [],
      unlockedBadges: [],
      unlockedMainLevel: 1,
      settings: {
        showSideQuests: true,
        showRationales: true,
        reduceMotion: false
      },
      lastPlayed: null
    });

    const loadState = () => {
      try{
        const raw = localStorage.getItem(STORAGE_KEY);
        if(!raw) return defaultState();
        const parsed = JSON.parse(raw);
        return {
          ...defaultState(),
          ...parsed,
          settings: {
            ...defaultState().settings,
            ...(parsed.settings || {})
          }
        };
      }catch{
        return defaultState();
      }
    };

    const saveState = () => {
      state.lastPlayed = new Date().toISOString();
      try{
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      }catch{}
    };

    let state = loadState();

    const unlockBadges = () => {
      const completed = new Set(state.completed);
      const unlocked = new Set(state.unlockedBadges);

      if(completed.size >= 1) unlocked.add("novice");
      if(completed.has("lvl1")) unlocked.add("privacy-ward");
      if(completed.has("lvl2")) unlocked.add("bias-breaker");
      if(completed.has("lvl3")) unlocked.add("guardian-key");
      if(completed.has("lvl4")) unlocked.add("truth-scribe");
      if(completed.has("lvl5")) unlocked.add("reality-check");
      if(completed.has("lvl6")) unlocked.add("human-in-loop");
      if(completed.has("sqAgents")) unlocked.add("agent-wrangler");
      if(completed.has("boss")) unlocked.add("champion");

      state.unlockedBadges = Array.from(unlocked);
    };

    const clamp = (n, a, b) => Math.max(a, Math.min(b, n));

    const scoreToRank = (score, maxScore) => {
      const pct = maxScore ? score / maxScore : 0;
      if (pct >= 0.9) return { rank: "S", label: "Legend" };
      if (pct >= 0.75) return { rank: "A", label: "Ace" };
      if (pct >= 0.6) return { rank: "B", label: "Skilled" };
      if (pct >= 0.45) return { rank: "C", label: "Learning" };
      return { rank: "D", label: "Needs Review" };
    };

    const $ = (sel) => document.querySelector(sel);
    const root = $("#root");

    const iconDot = (kind) => {
      const color = kind === "main" ? "var(--indigo)" : kind === "side" ? "var(--pink)" : "var(--bad)";
      return `<span style="display:inline-block;width:10px;height:10px;border-radius:999px;background:${color};box-shadow:0 0 16px ${color}"></span>`;
    };

    const principleLabel = (id) => (PRINCIPLES.find(p => p.id === id)?.label || id);

    const fmtDate = (iso) => {
      try{
        if(!iso) return "";
        return new Date(iso).toLocaleString();
      }catch{
        return "";
      }
    };

    function setReduceMotionClass(){
      document.body.classList.toggle("reduced", !!state.settings.reduceMotion);
    }

    const modalBack = $("#modalBack");
    const modalTitle = $("#modalTitle");
    const modalDesc = $("#modalDesc");
    const modalBody = $("#modalBody");
    const modalFoot = $("#modalFoot");

    function openModal({title, desc, bodyHTML, actions}){
      modalTitle.textContent = title || "";
      modalDesc.textContent = desc || "";
      modalBody.innerHTML = bodyHTML || "";
      modalFoot.innerHTML = "";

      (actions || []).forEach(a => {
        const b = document.createElement("button");
        b.className = `btn ${a.kind || ""}`.trim();
        b.textContent = a.label;
        b.onclick = () => { if(a.onClick) a.onClick(); };
        modalFoot.appendChild(b);
      });

      modalBack.classList.add("open");
    }

    function closeModal(){
      modalBack.classList.remove("open");
    }

    modalBack.addEventListener("click", (e) => {
      if(e.target === modalBack) closeModal();
    });

    window.addEventListener("keydown", (e) => {
      if(e.key === "Escape" && modalBack.classList.contains("open")) closeModal();
    });

    const Screen = {
      TITLE: "title",
      MAP: "map",
      MISSION: "mission",
      BADGES: "badges",
      CODEX: "codex",
      SETTINGS: "settings",
      DEBRIEF: "debrief"
    };

    let screen = Screen.TITLE;
    let activeMissionId = "lvl1";
    let lastResult = null;

    function renderFrame(contentHTML){
      const mainCount = MISSIONS.filter(m => m.type === "main").length;
      const mainDone = MISSIONS.filter(m => m.type === "main" && state.completed.includes(m.id)).length;
      const pct = mainCount ? Math.round((mainDone / mainCount) * 100) : 0;

      let playerAvatarSrc = BADGE_ICONS.novice;
      if (state.unlockedBadges.length > 0) {
        const championBadge = BADGES.find(b => b.id === "champion");
        if (state.unlockedBadges.includes("champion") && championBadge) {
          playerAvatarSrc = championBadge.icon;
        } else {
          const firstUnlockedBadge = BADGES.find(b => state.unlockedBadges.includes(b.id));
          if (firstUnlockedBadge) playerAvatarSrc = firstUnlockedBadge.icon;
        }
      }

      root.innerHTML = `
        <div class="topbar">
          <div class="brand">
            <div class="logo" aria-hidden="true">
              <div style="font-family:var(--pixel);font-size:12px;opacity:.9">AI</div>
            </div>
            <div>
              <h1>AI Arcade</h1>
              <div class="sub">Learning quests to learn AI in the GoA <span class="mono">v2</span></div>
            </div>
          </div>
          <div class="toolbar">
            <button class="btn ghost" id="navOver">Map</button>
            <button class="btn ghost" id="navBadges">Badges <span class="pill" style="padding:5px 8px">${state.unlockedBadges.length}</span></button>
            <button class="btn ghost" id="navCodex">Codex</button>
            <button class="btn ghost" id="navSettings">Settings</button>
          </div>
        </div>

        <div class="panel">
          <div class="inner">
            <div class="row">
              <div class="kpi" style="display:flex; align-items:center; gap:8px;">
                <img src="${playerAvatarSrc}" style="width:24px;height:24px;border-radius:50%;image-rendering:pixelated;border:1px solid var(--line);" alt="Player Avatar">
                <div>
                  <div class="label">Player</div>
                  <div class="value">${state.playerName || "Adventurer"}</div>
                </div>
              </div>
              <div class="kpi">
                <div class="label">Total XP</div>
                <div class="value">${state.xp}</div>
              </div>
              <div class="kpi">
                <div class="label">Progress</div>
                <div class="value">${mainDone}/${mainCount} (${pct}%)</div>
              </div>
            </div>
            <div class="progress" style="margin-top:10px"><div style="width:${pct}%"></div></div>
          </div>
        </div>

        <div style="margin-top:12px">${contentHTML}</div>
      `;

      root.querySelector("#navOver").onclick = () => navigate(Screen.MAP);
      root.querySelector("#navBadges").onclick = () => navigate(Screen.BADGES);
      root.querySelector("#navCodex").onclick = () => navigate(Screen.CODEX);
      root.querySelector("#navSettings").onclick = () => navigate(Screen.SETTINGS);
    }
function renderTitleScreen(){
      renderFrame(`
        <div class="panel">
          <div class="inner" style="text-align:center; padding:44px 22px">
            <div style="font-family:var(--pixel);font-size:44px;line-height:1.2;margin-bottom:12px">AI Arcade</div>
            <p style="max-width:500px;margin:0 auto 24px;line-height:1.6;color:var(--muted)">
              Embark on a pixel-art journey to master Responsible AI principles. Answer quizzes, navigate challenges, and defeat the ultimate boss of misinformation.
            </p>
            ${state.lastPlayed ? `<p class="mono" style="font-size:22px;color:var(--muted)">Last played: ${fmtDate(state.lastPlayed)}</p>` : ""}
            <button class="btn primary" id="startGame">Start Game</button>
            ${state.playerName ? `<button class="btn ghost" id="continueGame">Continue as ${state.playerName}</button>` : ""}
          </div>
        </div>
      `);

      $("#startGame").onclick = () => {
        openModal({
          title: "New Game",
          desc: "Enter your adventurer name to begin. Your progress is saved locally.",
          bodyHTML: `<input type="text" id="playerNameInput" value="${state.playerName}" placeholder="Adventurer Name" />`,
          actions:[
            { label:"Cancel", onClick:closeModal, kind:"ghost" },
            {
              label:"Start",
              onClick:() => {
                const name = $("#playerNameInput").value.trim();
                if(name){
                  state = defaultState();
                  state.playerName = name;
                  saveState();
                  closeModal();
                  navigate(Screen.MAP);
                } else {
                  alert("Please enter a name.");
                }
              }
            }
          ]
        });
      };

      if(state.playerName) {
        $("#continueGame").onclick = () => navigate(Screen.MAP);
      }
    }

    let mapCanvas, mapCtx;
    let currentSpritePosition = { x: NODES[0].x, y: NODES[0].y };
    let targetSpritePosition = { x: NODES[0].x, y: NODES[0].y };
    let mapAnimationId = null;

    const NODE_RADIUS = 25;
    const EDGE_LINE_WIDTH = 4;
    const STAR_COUNT = 250;
    const SPRITE_WIDTH = 72;
    const SPRITE_HEIGHT = 72;

    let stars = [];
    let canvasScale = { sx: 1, sy: 1, width: 900, height: 500 };
    let layoutMap = {};

    function generateStars(width, height){
      stars = [];
      for (let i = 0; i < STAR_COUNT; i++) {
        stars.push({
          x: Math.random() * width,
          y: Math.random() * height,
          radius: Math.random() * 1.8 + 0.6,
          alpha: Math.random() * 0.8 + 0.2,
          hue: 200 + Math.random() * 120
        });
      }
    }

    function drawStarfield(ctx){
      const w = canvasScale.width;
      const h = canvasScale.height;
      // muted tech nebula background (accessible)
      const g = ctx.createLinearGradient(0, 0, w, h);
      g.addColorStop(0, 'rgba(6,18,36,0.9)');
      g.addColorStop(1, 'rgba(4,18,34,0.95)');
      ctx.fillStyle = g;
      ctx.fillRect(0,0,w,h);

      stars.forEach(star => {
        // small subtle cyan/white stars for tech feel
        ctx.globalAlpha = star.alpha * 0.9;
        ctx.fillStyle = 'rgba(180,230,255,0.95)';
        ctx.beginPath();
        ctx.arc(star.x, star.y, Math.max(0.6, star.radius * 0.9), 0, Math.PI * 2);
        ctx.fill();
        ctx.globalAlpha = 1;
      });
      ctx.globalAlpha = 1;
    }

    function drawSprite(ctx, x, y) {
      if (muncherImageLoaded && muncherImage) {
        const drawSize = Math.min(SPRITE_WIDTH, SPRITE_HEIGHT, 80);
        ctx.drawImage(muncherImage, x - drawSize / 2, y - drawSize / 2, drawSize, drawSize);
        return;
      }

      ctx.save();
      ctx.translate(x, y);

      const scale = 1.4;
      ctx.fillStyle = "#6610f2";
      ctx.fillRect(-12 * scale, -16 * scale, 24 * scale, 24 * scale);

      ctx.fillStyle = "#ffffff";
      ctx.fillRect(-7 * scale, -8 * scale, 4 * scale, 4 * scale);
      ctx.fillRect(3 * scale, -8 * scale, 4 * scale, 4 * scale);

      ctx.fillStyle = "#17a2b8";
      ctx.fillRect(-10 * scale, 10 * scale, 6 * scale, 4 * scale);
      ctx.fillRect(4 * scale, 10 * scale, 6 * scale, 4 * scale);

      ctx.restore();
    }

    function renderMap(){
      const missionsDisplay = MISSIONS
        .filter(m => m && m.title && m.tagline)
        .map(m => {
          const isCompleted = state.completed.includes(m.id);
          const isLocked = m.level > state.unlockedMainLevel && !isCompleted;
          const isSideQuest = m.type === "side";
          if(isSideQuest && !state.settings.showSideQuests) return "";
          const tagClass = m.type === "main" ? "main" : m.type === "boss" ? "boss" : "side";
          const lockHtml = isLocked ? `<span style="margin-left:8px">🔒</span>` : "";

          return `
          <div class="card ${isLocked ? "locked" : ""}">
            <div class="cardTop">
              <div>
                <h3>${iconDot(m.type)} ${m.title} ${lockHtml}</h3>
                <p>${m.tagline}</p>
              </div>
              <div class="tag ${tagClass}">${principleLabel(m.principle)}</div>
            </div>
            <div style="display:flex; gap:10px; margin-top:14px">
              ${isCompleted ? `
                <button class="btn ghost" disabled>Completed</button>
                <button class="btn ghost" data-mission-id="${m.id}" data-action="review">Review</button>
              ` : `
                <button class="btn primary" ${isLocked ? "disabled" : ""} data-mission-id="${m.id}" data-action="start">Start Mission</button>
              `}
            </div>
          </div>
        `;
      }).join("");

      renderFrame(`
        <div class="mapWrap panel">
          <canvas id="mapCanvas" width="900" height="500"></canvas>
          <div class="mapHud">
            <div class="pill">${state.playerName}</div>
            <div class="pill">Level ${state.unlockedMainLevel}</div>
          </div>
          <div class="mapHint">
            <div class="hintTitle">Current: ${MISSIONS.find(m => m.id === activeMissionId)?.title || ""}</div>
            <div class="hintSub">${MISSIONS.find(m => m.id === activeMissionId)?.tagline || ""}</div>
          </div>
        </div>
        <div class="cards" style="margin-top:12px;display:grid;gap:12px;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));">
          ${missionsDisplay}
        </div>
      `);

      mapCanvas = $("#mapCanvas");
      mapCtx = mapCanvas.getContext("2d");
      // make canvas responsive and generate stars
      resizeMapCanvas();
      window.addEventListener('resize', resizeMapCanvas);
      if (!muncherImage) {
        loadMuncherImage(MUNCHER_IMAGE_PATH);
      }

      const activeNode = NODES.find(n => n.missionId === activeMissionId);
      if (activeNode) {
        const sp = getScaledPos(activeNode);
        currentSpritePosition = { x: sp.x, y: sp.y };
        targetSpritePosition = { x: sp.x, y: sp.y };
      }

      if (mapAnimationId) cancelAnimationFrame(mapAnimationId);
      mapAnimationId = requestAnimationFrame(animateMap);

      root.querySelectorAll(".cards button").forEach(button => {
        if(!button.dataset.missionId) return;
        const mission = MISSIONS.find(m => m.id === button.dataset.missionId);
        if(!mission) return;

        if(button.dataset.action === "start"){
          button.onclick = () => startMission(mission.id);
        } else if (button.dataset.action === "review"){
          button.onclick = () => openMissionReview(mission.id);
        }
      });
    }

    function getVisibleNodes(){
      const visible = NODES.filter(n => {
        const mission = MISSIONS.find(m => m.id === n.missionId);
        // keep only nodes that link to a mission and are connected or a side quest
        const inEdge = EDGES.some(e => e[0] === n.id || e[1] === n.id);
        const isSide = mission && mission.type === 'side';
        return !!mission && (inEdge || isSide);
      });
      return visible;
    }

    function getScaledPos(node){
      const layout = layoutMap[node.id];
      const baseX = layout ? layout.x : node.x;
      const baseY = layout ? layout.y : node.y;
      return { x: baseX * canvasScale.sx + (canvasScale.offsetX || 0), y: baseY * canvasScale.sy + (canvasScale.offsetY || 0) };
    }

    function resizeMapCanvas(){
      if(!mapCanvas) return;
      // CSS pixels
      const containerWidth = mapCanvas.clientWidth || 900;
      const desiredHeight = Math.round(containerWidth / (900/500));
      const dpr = window.devicePixelRatio || 1;
      canvasScale.sx = containerWidth / 900;
      canvasScale.sy = desiredHeight / 500;
      canvasScale.width = containerWidth;
      canvasScale.height = desiredHeight;

      mapCanvas.style.width = containerWidth + 'px';
      mapCanvas.style.height = desiredHeight + 'px';
      mapCanvas.width = Math.max(1, Math.floor(containerWidth * dpr));
      mapCanvas.height = Math.max(1, Math.floor(desiredHeight * dpr));
      mapCtx.setTransform(dpr,0,0,dpr,0,0);

      generateStars(canvasScale.width, canvasScale.height);
      // reset offsets; will be computed in animateMap before drawing
      canvasScale.offsetX = 0;
      canvasScale.offsetY = 0;
    }

    function roundRect(ctx, x, y, w, h, r, fill, stroke){
      if (typeof r === 'undefined') r = 5;
      ctx.beginPath();
      ctx.moveTo(x + r, y);
      ctx.arcTo(x + w, y, x + w, y + h, r);
      ctx.arcTo(x + w, y + h, x, y + h, r);
      ctx.arcTo(x, y + h, x, y, r);
      ctx.arcTo(x, y, x + w, y, r);
      ctx.closePath();
      if(fill) ctx.fill();
      if(stroke) ctx.stroke();
    }

    function applyGridLayout(visible){
      layoutMap = {};
      const n = visible.length;
      if(n === 0) return;
      const cols = Math.ceil(Math.sqrt(n));
      const rows = Math.ceil(n / cols);
      const marginX = 80;
      const marginY = 60;
      const usableW = 900 - marginX*2;
      const usableH = 500 - marginY*2;
      visible.forEach((node, i) => {
        const col = i % cols;
        const row = Math.floor(i / cols);
        const cx = marginX + (col + 0.5) * (usableW / cols);
        const cy = marginY + (row + 0.5) * (usableH / rows);
        layoutMap[node.id] = { x: cx, y: cy };
      });
    }

    function drawCircuitGrid(ctx){
      const step = 80 * Math.min(canvasScale.sx, canvasScale.sy);
      ctx.save();
      ctx.strokeStyle = 'rgba(200,230,255,0.03)';
      ctx.lineWidth = 1;
      for(let x = step/2; x < canvasScale.width; x += step){
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, canvasScale.height);
        ctx.stroke();
      }
      for(let y = step/2; y < canvasScale.height; y += step){
        ctx.beginPath();
        ctx.moveTo(0, y);
        ctx.lineTo(canvasScale.width, y);
        ctx.stroke();
      }
      // add small junction dots
      ctx.fillStyle = 'rgba(200,230,255,0.04)';
      for(let x = step/2; x < canvasScale.width; x += step){
        for(let y = step/2; y < canvasScale.height; y += step){
          ctx.fillRect(x-1, y-1, 2, 2);
        }
      }
      ctx.restore();
    }

    function animateMap() {
      if (!mapCtx) return;

      // clear using CSS pixels (canvas is scaled by DPR)
      mapCtx.clearRect(0, 0, canvasScale.width, canvasScale.height);
      drawStarfield(mapCtx);
      drawCircuitGrid(mapCtx);

      const visible = getVisibleNodes();
      // apply an even grid layout to visible nodes for consistent spacing
      applyGridLayout(visible);
      // compute centering offset so visible nodes fit and are centered (use scaled positions)
      if(visible.length){
        let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
        visible.forEach(n => {
          const pos = getScaledPos(n);
          const sx = pos.x;
          const sy = pos.y;
          if(sx < minX) minX = sx;
          if(sy < minY) minY = sy;
          if(sx > maxX) maxX = sx;
          if(sy > maxY) maxY = sy;
        });
        const nodeBoxWidth = maxX - minX || 1;
        const nodeBoxHeight = maxY - minY || 1;
        const padding = 60 * Math.min(canvasScale.sx, canvasScale.sy);
        const targetCenterX = canvasScale.width / 2;
        const targetCenterY = canvasScale.height / 2;
        const nodesCenterX = (minX + maxX) / 2;
        const nodesCenterY = (minY + maxY) / 2;
        canvasScale.offsetX = targetCenterX - nodesCenterX;
        canvasScale.offsetY = targetCenterY - nodesCenterY;
      } else {
        canvasScale.offsetX = 0;
        canvasScale.offsetY = 0;
      }
      // draw edges (only between visible nodes)
      EDGES.forEach(([fromId, toId]) => {
        const fromNode = visible.find(n => n.id === fromId) || NODES.find(n => n.id === fromId);
        const toNode = visible.find(n => n.id === toId) || NODES.find(n => n.id === toId);
        if (fromNode && toNode) {
          const a = getScaledPos(fromNode);
          const b = getScaledPos(toNode);
          mapCtx.beginPath();
          mapCtx.moveTo(a.x, a.y);
          mapCtx.lineTo(b.x, b.y);
          mapCtx.lineWidth = Math.max(2, EDGE_LINE_WIDTH * Math.min(canvasScale.sx, canvasScale.sy));
          mapCtx.strokeStyle = 'rgba(140,160,255,0.18)';
          mapCtx.shadowColor = 'rgba(124,77,255,0.12)';
          mapCtx.shadowBlur = 8;
          mapCtx.stroke();
          mapCtx.shadowBlur = 0;
        }
      });

      const dx = targetSpritePosition.x - currentSpritePosition.x;
      const dy = targetSpritePosition.y - currentSpritePosition.y;
      const distance = Math.sqrt(dx * dx + dy * dy);
      const speed = state.settings.reduceMotion ? 0.5 : 3;

      if (distance > speed) {
        currentSpritePosition.x += (dx / distance) * speed;
        currentSpritePosition.y += (dy / distance) * speed;
      } else {
        currentSpritePosition.x = targetSpritePosition.x;
        currentSpritePosition.y = targetSpritePosition.y;
      }

      // draw only visible nodes (scaled)
      visible.forEach(node => {
        const mission = MISSIONS.find(m => m.id === node.missionId);
        const isCompleted = node.missionId && state.completed.includes(node.missionId);
        const isLocked = mission && mission.level > state.unlockedMainLevel && !isCompleted;
        const isSideQuest = mission && mission.type === "side";

        const p = getScaledPos(node);
        const isTarget = (Math.abs(p.x - targetSpritePosition.x) < 2 && Math.abs(p.y - targetSpritePosition.y) < 2);

        // node visual: radial gradient + ring
        const grd = mapCtx.createRadialGradient(p.x, p.y, NODE_RADIUS * 0.15, p.x, p.y, NODE_RADIUS * 1.4);
        if (isCompleted){
          grd.addColorStop(0, 'rgba(255,209,102,0.98)');
          grd.addColorStop(1, 'rgba(255,209,102,0.28)');
        } else if (isTarget){
          grd.addColorStop(0, 'rgba(124,77,255,0.98)');
          grd.addColorStop(1, 'rgba(0,229,255,0.30)');
        } else if (isSideQuest){
          grd.addColorStop(0, 'rgba(255,58,122,0.98)');
          grd.addColorStop(1, 'rgba(255,58,122,0.22)');
        } else {
          grd.addColorStop(0, 'rgba(230,240,255,0.98)');
          grd.addColorStop(1, 'rgba(200,220,255,0.18)');
        }

        mapCtx.beginPath();
        mapCtx.arc(p.x, p.y, NODE_RADIUS, 0, Math.PI * 2);
        mapCtx.fillStyle = grd;
        mapCtx.shadowColor = 'rgba(0,0,0,0.12)';
        mapCtx.shadowBlur = 8;
        mapCtx.fill();
        mapCtx.shadowBlur = 0;

        // ring
        mapCtx.beginPath();
        mapCtx.arc(p.x, p.y, NODE_RADIUS + 4, 0, Math.PI * 2);
        mapCtx.lineWidth = 3;
        mapCtx.strokeStyle = isTarget ? 'rgba(102,16,242,0.9)' : 'rgba(51,51,51,0.08)';
        mapCtx.stroke();

        if(isLocked){
          mapCtx.font = `${Math.round(22 * Math.min(canvasScale.sx, canvasScale.sy))}px Arial`;
          mapCtx.fillStyle = "rgba(51,51,51,.5)";
          mapCtx.fillText("🔒", p.x - 6, p.y + 5);
        } else {
          const distToSprite = Math.hypot(p.x - currentSpritePosition.x, p.y - currentSpritePosition.y);
          const labelVisible = distToSprite > (SPRITE_WIDTH * 0.6);
          if(labelVisible){
            mapCtx.font = `${Math.round(16 * Math.min(canvasScale.sx, canvasScale.sy))}px Arial`;
            mapCtx.textAlign = "center";
            mapCtx.textBaseline = "middle";
            const padding = 8 * Math.min(canvasScale.sx, canvasScale.sy);
            const text = node.label;
            const metrics = mapCtx.measureText(text);
            const textW = metrics.width;
            const rectW = textW + padding * 2;
            const rectH = Math.round(18 * Math.min(canvasScale.sx, canvasScale.sy));
            const rectX = p.x - rectW / 2;
            const rectY = p.y - NODE_RADIUS - 12 - rectH / 2;
            // rounded rect backdrop for accessibility
            mapCtx.fillStyle = 'rgba(2,6,14,0.72)';
            roundRect(mapCtx, rectX, rectY, rectW, rectH, 6 * Math.min(canvasScale.sx, canvasScale.sy), true, false);
            mapCtx.fillStyle = '#e6f0fb';
            mapCtx.fillText(text, p.x, p.y - NODE_RADIUS - 12);
          }
        }
      });

      // draw sprite last so it appears above node text and rings
      drawSprite(mapCtx, currentSpritePosition.x, currentSpritePosition.y);

      mapAnimationId = requestAnimationFrame(animateMap);
    }

    function startMission(missionId){
      activeMissionId = missionId;
      const mission = MISSIONS.find(m => m.id === missionId);
      if (!mission) return;

      const targetNode = NODES.find(n => n.missionId === missionId);
      if (targetNode) {
        const tp = getScaledPos(targetNode);
        targetSpritePosition = { x: tp.x, y: tp.y };
      }

      if (
        state.settings.reduceMotion ||
        (targetSpritePosition.x === currentSpritePosition.x && targetSpritePosition.y === currentSpritePosition.y)
      ) {
        setTimeout(() => navigateToMissionMode(mission), 50);
      } else {
        const checkArrival = setInterval(() => {
          if (
            currentSpritePosition.x === targetSpritePosition.x &&
            currentSpritePosition.y === targetSpritePosition.y
          ) {
            clearInterval(checkArrival);
            navigateToMissionMode(mission);
          }
        }, 100);
      }
    }

    function navigateToMissionMode(mission) {
      if(mission.mode === "quiz"){
        renderQuizMission();
      } else if (mission.mode === "dodge"){
        startGame_Dodge(mission);
      } else if (mission.mode === "redact"){
        startGame_Redact(mission);
      } else if (mission.mode === "boss"){
        startGame_Boss(mission);
      } else if (mission.mode === "trust"){
        startGame_Trust(mission);
      } else if (mission.mode === "munch"){
        startGame_Munch(mission);
      } else if (mission.mode === "whatis"){
        startGame_WhatIsAI(mission);
      } else if (mission.mode === "reality"){
        startGame_RealityCheck(mission);
      } else if (mission.mode === "whack"){
        startGame_IMWhack(mission);
      }
    }

    function completeMission(missionId){
      if(!state.completed.includes(missionId)){
        state.completed.push(missionId);
        const mission = MISSIONS.find(m => m.id === missionId);
        if(mission && mission.type === "main"){
          state.unlockedMainLevel = Math.max(state.unlockedMainLevel, mission.level + 1);
        }
        state.xp += mission.difficulty * 10;
      }
      unlockBadges();
      saveState();
      navigate(Screen.DEBRIEF);
    }

    function openMissionReview(missionId){
      activeMissionId = missionId;
      navigate(Screen.DEBRIEF);
    }

    let currentQuestionIdx = 0;
    let quizScore = 0;
    let quizTotalQuestions = 0;

    function renderQuizMission(){
      const mission = MISSIONS.find(m => m.id === activeMissionId);
      if(!mission || !QUESTIONS[mission.id]){
        console.error("Mission or questions not found:", activeMissionId);
        navigate(Screen.MAP);
        return;
      }

      const questions = QUESTIONS[mission.id];
      quizTotalQuestions = questions.length;
      const currentQuestion = questions[currentQuestionIdx];

      if(!currentQuestion){
        lastResult = {
          type: "quiz",
          missionId: mission.id,
          score: quizScore,
          total: quizTotalQuestions
        };
        currentQuestionIdx = 0;
        quizScore = 0;
        completeMission(mission.id);
        return;
      }

      const answersHtml = currentQuestion.options.map((opt, idx) => `
        <button class="ans" data-answer-idx="${idx}">
          <span class="key">${String.fromCharCode(65 + idx)}</span>${opt}
        </button>
      `).join("");

      renderFrame(`
        <div class="twoCol">
          <div class="panel quizQ">
            <div class="meta">
              <span>Question ${currentQuestionIdx + 1}/${quizTotalQuestions}</span>
              <span>Score: ${quizScore}</span>
            </div>
            <p class="prompt">${currentQuestion.prompt}</p>
          </div>
          <div class="answers">
            ${answersHtml}
          </div>
        </div>
      `);

      root.querySelectorAll(".answers .ans").forEach(button => {
        button.onclick = (e) => handleAnswerClick(e, currentQuestion);
      });
    }

    function handleAnswerClick(event, question){
      const selectedIdx = parseInt(event.currentTarget.dataset.answerIdx);
      const isCorrect = selectedIdx === question.answer;
      if(isCorrect) quizScore++;

      openModal({
        title: isCorrect ? "Correct!" : "Incorrect",
        desc: isCorrect ? "You chose wisely." : "Consider this perspective:",
        bodyHTML: state.settings.showRationales
          ? `<div class="rationale"><h4>Rationale</h4><p>${question.rationale}</p></div>`
          : "",
        actions:[
          {
            label:"Next Question",
            onClick:() => {
              closeModal();
              currentQuestionIdx++;
              renderQuizMission();
            }
          }
        ]
      });
    }

    let dodgeCanvas, dodgeCtx;
    let player, obstacles, dodgeScore, dodgeGameInterval;
    const DODGE_GAME_WIDTH = 600;
    const DODGE_GAME_HEIGHT = 400;
    const DODGE_BAD_PRACTICES = [
      "Bias",
      "Hallucinations",
      "Privacy Leak",
      "Prompt Injection",
      "Misinformation",
      "Overconfidence",
      "Lack of Transparency"
    ];
    const DODGE_GOOD_PRACTICES = [
      "Transparency",
      "Fairness",
      "Explainability",
      "Privacy by Design",
      "Robustness",
      "Human Oversight",
      "Accountability"
    ];

    const MUNCHER_GOOD_CONTENT = [
      "Transparency", "Explainability", "Privacy by Design", "Human Oversight", "Fairness", "Robustness", "Accountability", "Safety", "Trustworthy AI", "Ethical Guardrails"
    ];
    const MUNCHER_BAD_CONTENT = [
      "Hallucination", "Bias", "Data Poisoning", "Prompt Injection", "Privacy Leak", "Misinformation", "Overconfidence", "Secret Leakage", "Unverified Claim", "Opaque Reasoning"
    ];

    // Muncher sprite image: embedded base64 data URI from the provided PNG
    const MUNCHER_IMAGE_PATH = 'data:image/png;base64,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GhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhoaGhp/BcD/BW17QqNRuy5+AAAAAElFTkSuQmCC';
    let muncherImage = null;
    let muncherImageLoaded = false;
    function loadMuncherImage(path){
      if(muncherImageLoaded || muncherImage) return;
      muncherImage = new Image();
      muncherImage.onload = () => { muncherImageLoaded = true; };
      muncherImage.onerror = () => { muncherImage = null; muncherImageLoaded = false; };
      muncherImage.src = path;
    }

    function shuffleArray(array){
      return [...array].sort(() => Math.random() - 0.5);
    }

    function startGame_Dodge(mission){
      obstacles = [];
      dodgeScore = 0;
      if(dodgeGameInterval) clearInterval(dodgeGameInterval);

      renderFrame(`
        <div class="miniWrap">
          <div class="miniHud">
            <div class="left">
              <h3 style="margin:0">${mission.title}</h3>
              <p style="margin:8px 0 0;font-size:13px;color:var(--muted)">${mission.tagline}</p>
            </div>
            <div class="right">
              <button class="btn ghost" id="exitDodge">Back to Map</button>
            </div>
          </div>
          <div style="padding:18px 16px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
            <h4 style="margin:0 0 8px;font-size:20px">Dodge Mission Instructions</h4>
            <p style="margin:0 0 10px;font-size:13px;line-height:1.6;color:var(--muted)">Use the left and right arrow keys to move your avatar. Avoid bad AI practice words and catch good AI practice words to earn bonus points. Reach 100 points to complete the mission.</p>
            <p style="margin:0;font-size:13px;color:var(--muted)"><strong>Controls:</strong> Arrow Left / Arrow Right</p>
            <p style="margin:8px 0 0;font-size:13px;color:var(--muted)"><strong>Objective:</strong> Dodge bad AI words and let positive AI practice words hit you for extra score.</p>
            <p style="margin:8px 0 0;font-size:13px;color:var(--muted)"><strong>Tip:</strong> Good practice words are green; bad practice words are red.</p>
          </div>
          <div class="row" style="margin-top:16px;justify-content:flex-end">
            <button class="btn primary" id="startDodge">Start Game</button>
          </div>
          <div class="miniWrap" style="margin-top:16px">
            <canvas id="dodgeCanvas" width="${DODGE_GAME_WIDTH}" height="${DODGE_GAME_HEIGHT}" style="background:#f0f8ff"></canvas>
          </div>
        </div>
      `);

      dodgeCanvas = $("#dodgeCanvas");
      dodgeCtx = dodgeCanvas.getContext("2d");

      $("#exitDodge").onclick = () => {
        navigate(Screen.MAP);
      };

      $("#startDodge").onclick = () => {
        initDodgeGame(mission);
      };
    }

    function initDodgeGame(mission){
      player = { x: DODGE_GAME_WIDTH / 2 - 12, y: DODGE_GAME_HEIGHT - 40, width: 24, height: 24 };
      obstacles = [];
      dodgeScore = 0;
      if(dodgeGameInterval) clearInterval(dodgeGameInterval);

      const scoreDisplay = document.createElement("div");
      scoreDisplay.className = "pill";
      scoreDisplay.style.margin = "0 0 12px";
      scoreDisplay.innerHTML = `Score: <span id="dodgeScoreDisplay">0</span>`;
      document.querySelector(".miniWrap")?.insertAdjacentElement("afterbegin", scoreDisplay);

      $("#startDodge")?.remove();

      dodgeGameInterval = setInterval(updateDodgeGame, 1000 / 60);
      document.addEventListener("keydown", handleDodgeKey);
    }

    function updateDodgeGame(){
      dodgeCtx.clearRect(0, 0, DODGE_GAME_WIDTH, DODGE_GAME_HEIGHT);

      drawSprite(dodgeCtx, player.x + player.width / 2, player.y + player.height / 2);

      if(Math.random() < 0.03 && obstacles.length < 7){
        const width = 50 + Math.random() * 20;
        const height = 24 + Math.random() * 8;
        const isGood = Math.random() < 0.35;
        obstacles.push({
          x: Math.random() * (DODGE_GAME_WIDTH - width),
          y: 0,
          width,
          height,
          speed: 1.5 + dodgeScore / 120,
          type: isGood ? "good" : "bad",
          label: isGood
            ? DODGE_GOOD_PRACTICES[Math.floor(Math.random() * DODGE_GOOD_PRACTICES.length)]
            : DODGE_BAD_PRACTICES[Math.floor(Math.random() * DODGE_BAD_PRACTICES.length)]
        });
      }

      for(let i = 0; i < obstacles.length; i++){
        const obs = obstacles[i];
        obs.y += obs.speed;

        const backgroundColor = obs.type === "good" ? "#218838" : "#dc3545";
        dodgeCtx.fillStyle = backgroundColor;
        dodgeCtx.fillRect(obs.x, obs.y, obs.width, obs.height);

        dodgeCtx.fillStyle = "#ffffff";
        dodgeCtx.font = "12px ui-monospace, monospace";
        dodgeCtx.textAlign = "center";
        dodgeCtx.textBaseline = "middle";
        dodgeCtx.fillText(obs.label, obs.x + obs.width / 2, obs.y + obs.height / 2);

        if(
          player.x < obs.x + obs.width &&
          player.x + player.width > obs.x &&
          player.y < obs.y + obs.height &&
          player.y + player.height > obs.y
        ){
          if(obs.type === "good"){
            dodgeScore += 10;
            obstacles.splice(i, 1);
            $("#dodgeScoreDisplay").textContent = dodgeScore;
            i--;
            continue;
          }

          clearInterval(dodgeGameInterval);
          document.removeEventListener("keydown", handleDodgeKey);

          lastResult = {
            type: "dodge",
            missionId: activeMissionId,
            score: dodgeScore,
            total: 100
          };

          if(dodgeScore >= 100) {
            completeMission(activeMissionId);
          } else {
            openModal({
              title: "Mission Failed!",
              desc: `You scored ${dodgeScore}. You need 100 points to pass this mission.`,
              actions:[
                {
                  label:"Retry",
                  onClick:() => {
                    closeModal();
                    startGame_Dodge(MISSIONS.find(m => m.id === activeMissionId));
                  },
                  kind:"primary"
                },
                {
                  label:"Back to Map",
                  onClick:() => {
                    closeModal();
                    navigate(Screen.MAP);
                  },
                  kind:"ghost"
                }
              ]
            });
          }
          return;
        }

        if(obs.y > DODGE_GAME_HEIGHT){
          if(obs.type === "bad"){
            dodgeScore++;
            $("#dodgeScoreDisplay").textContent = dodgeScore;
          }
          obstacles.splice(i, 1);
          i--;
        }
      }
    }

    function handleDodgeKey(e){
      const playerSpeed = 10;
      if(e.key === "ArrowLeft"){
        player.x = clamp(player.x - playerSpeed, 0, DODGE_GAME_WIDTH - player.width);
      } else if (e.key === "ArrowRight"){
        player.x = clamp(player.x + playerSpeed, 0, DODGE_GAME_WIDTH - player.width);
      }
    }

    function startGame_Munch(mission){
      let munchCanvas, munchCtx, munchGrid, munchPlayer, munchScore = 0, munchLives = 3, munchTime = 60, munchInterval, munchActive = false;
      const GRID_COLS = 8;
      const GRID_ROWS = 5;
      const TILE_SIZE = 72;
      const CANVAS_WIDTH = GRID_COLS * TILE_SIZE;
      const CANVAS_HEIGHT = GRID_ROWS * TILE_SIZE;
      const MUNCH_GOAL = 8;

      renderFrame(`
        <div class="miniWrap">
          <div class="miniHud">
            <div class="left">
              <h3>${mission.title}</h3>
              <p style="margin:6px 0 0;color:var(--muted)">${mission.tagline}</p>
            </div>
            <div class="right">
              <button class="btn ghost" id="exitMunch">Back</button>
            </div>
          </div>
          <div style="padding:16px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
            <p style="margin:0 0 10px;font-size:13px;color:var(--muted);line-height:1.6">Move the Muncher with the arrow keys. Eat AI-strengthening content and avoid bad AI outputs. Reach ${MUNCH_GOAL} clean munches before time runs out.</p>
          </div>
          <div style="display:flex;gap:12px;flex-wrap:wrap;align-items:center;margin-top:12px">
            <div class="pill">Score: <span id="munchScore">0</span></div>
            <div class="pill">Lives: <span id="munchLives">3</span></div>
            <div class="pill">Time: <span id="munchTime">60</span>s</div>
          </div>
          <div class="miniWrap" style="margin-top:14px">
            <canvas id="munchCanvas" width="${CANVAS_WIDTH}" height="${CANVAS_HEIGHT}" style="background:#f0f8ff;border-radius:12px;display:block"></canvas>
          </div>
        </div>
      `);

      munchCanvas = $("#munchCanvas");
      munchCtx = munchCanvas.getContext("2d");

      $("#exitMunch").onclick = () => {
        cleanupMunch();
        navigate(Screen.MAP);
      };

      function buildGrid(){
        const allTiles = [];
        const totalTiles = GRID_COLS * GRID_ROWS;
        const goodCount = MUNCH_GOAL + 2;
        const badCount = 12;
        const blankCount = totalTiles - goodCount - badCount;

        const shuffledGood = shuffleArray(MUNCHER_GOOD_CONTENT).slice(0, goodCount);
        const shuffledBad = shuffleArray(MUNCHER_BAD_CONTENT).slice(0, badCount);
        const blankTiles = Array.from({ length: blankCount }, () => ({ label: "", good:false }));

        const entries = [
          ...shuffledGood.map(label => ({ label, good:true })),
          ...shuffledBad.map(label => ({ label, good:false })),
          ...blankTiles
        ];
        const placed = shuffleArray(entries);

        for(let row = 0; row < GRID_ROWS; row++){
          for(let col = 0; col < GRID_COLS; col++){
            const entry = placed[row * GRID_COLS + col] || { label: "", good:false };
            allTiles.push({
              col,
              row,
              label: entry.label,
              good: entry.good,
              eaten: false
            });
          }
        }
        return allTiles;
      }

      function drawMuncher(x, y){
          if(muncherImageLoaded && muncherImage){
            // draw the provided image centered in the tile
            const drawSize = Math.min(TILE_SIZE - 12, 64);
            const dx = x + (TILE_SIZE - drawSize) / 2;
            const dy = y + (TILE_SIZE - drawSize) / 2;
            try{ munchCtx.drawImage(muncherImage, dx, dy, drawSize, drawSize); }
            catch(e){ /* fallback to vector */ }
          } else {
            munchCtx.save();
            munchCtx.translate(x + TILE_SIZE / 2, y + TILE_SIZE / 2);
            munchCtx.fillStyle = "#6610f2";
            munchCtx.beginPath();
            munchCtx.arc(0, 0, 24, 0.25 * Math.PI, 1.75 * Math.PI, false);
            munchCtx.lineTo(0, 0);
            munchCtx.fill();
            munchCtx.beginPath();
            munchCtx.arc(0, 0, 24, 1.75 * Math.PI, 0.25 * Math.PI, false);
            munchCtx.fill();
            munchCtx.fillStyle = "#ffffff";
            munchCtx.beginPath();
            munchCtx.arc(10, -8, 6, 0, Math.PI * 2);
            munchCtx.fill();
            munchCtx.fillStyle = "#333333";
            munchCtx.beginPath();
            munchCtx.arc(12, -8, 3, 0, Math.PI * 2);
            munchCtx.fill();
            munchCtx.restore();
          }
      }

      function wrapText(text, maxWidth){
        const words = text.split(' ');
        const lines = [];
        let line = '';
        words.forEach(word => {
          const testLine = line ? `${line} ${word}` : word;
          const metrics = munchCtx.measureText(testLine);
          if(metrics.width > maxWidth && line){
            lines.push(line);
            line = word;
          } else {
            line = testLine;
          }
        });
        if(line) lines.push(line);
        return lines;
      }

      function drawGrid(){
        munchCtx.clearRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
        munchGrid.forEach(tile => {
          const x = tile.col * TILE_SIZE;
          const y = tile.row * TILE_SIZE;
          munchCtx.fillStyle = tile.eaten ? "rgba(255,255,255,0.7)" : "#ffffff";
          munchCtx.fillRect(x + 4, y + 4, TILE_SIZE - 8, TILE_SIZE - 8);
          munchCtx.strokeStyle = "rgba(51,51,51,0.18)";
          munchCtx.strokeRect(x + 4, y + 4, TILE_SIZE - 8, TILE_SIZE - 8);
          if(tile.label && !tile.eaten){
            munchCtx.fillStyle = tile.good ? "#28a745" : "#dc3545";
            munchCtx.font = "11px ui-monospace, monospace";
            munchCtx.textAlign = "center";
            munchCtx.textBaseline = "middle";
            const lines = wrapText(tile.label, TILE_SIZE - 16);
            const startY = y + TILE_SIZE / 2 - ((lines.length - 1) * 8);
            lines.forEach((line, index) => {
              munchCtx.fillText(line, x + TILE_SIZE / 2, startY + index * 16);
            });
          }
        });
        drawMuncher(munchPlayer.col * TILE_SIZE, munchPlayer.row * TILE_SIZE);
      }

      function updateMunchDisplay(){
        $("#munchScore").textContent = munchScore;
        $("#munchLives").textContent = munchLives;
        $("#munchTime").textContent = munchTime;
      }

      function cleanupMunch(){
        if(munchInterval) clearInterval(munchInterval);
        munchInterval = null;
        munchActive = false;
        document.removeEventListener("keydown", handleMunchKey);
      }

      function finishMunch(success){
        cleanupMunch();
        lastResult = { type: "munch", missionId: mission.id, score: munchScore, total: MUNCH_GOAL };
        if(success){
          completeMission(mission.id);
          openModal({
            title: "Mission Complete!",
            desc: `You munched ${munchScore} safe AI items.`,
            actions:[
              { label: "Back to Map", kind: "primary", onClick: () => { closeModal(); navigate(Screen.MAP); } }
            ]
          });
        } else {
          openModal({
            title: "Munch Mission Over",
            desc: `You scored ${munchScore}. Try again to munch more good AI content.`,
            actions:[
              { label: "Retry", kind: "primary", onClick: () => { closeModal(); startGame_Munch(mission); } },
              { label: "Back to Map", kind: "ghost", onClick: () => { closeModal(); navigate(Screen.MAP); } }
            ]
          });
        }
      }

      function findTile(col, row){
        return munchGrid.find(tile => tile.col === col && tile.row === row);
      }

      function handleMunchKey(e){
        const key = e.key;
        let moved = false;
        if(key === "ArrowUp" && munchPlayer.row > 0){ munchPlayer.row--; moved = true; }
        if(key === "ArrowDown" && munchPlayer.row < GRID_ROWS - 1){ munchPlayer.row++; moved = true; }
        if(key === "ArrowLeft" && munchPlayer.col > 0){ munchPlayer.col--; moved = true; }
        if(key === "ArrowRight" && munchPlayer.col < GRID_COLS - 1){ munchPlayer.col++; moved = true; }
        if(!moved) return;

        const tile = findTile(munchPlayer.col, munchPlayer.row);
        if(tile && tile.label && !tile.eaten){
          tile.eaten = true;
          if(tile.good){
            munchScore += 1;
            addLog(`Munched ${tile.label} safely!`, "good");
          } else {
            munchLives -= 1;
            addLog(`Oops! ${tile.label} was bad AI content.`, "bad");
          }
          updateMunchDisplay();
          if(munchScore >= MUNCH_GOAL){
            finishMunch(true);
            return;
          }
          if(munchLives <= 0){
            finishMunch(false);
            return;
          }
        }
        drawGrid();
      }

      const munchLog = [];
      function addLog(text, type){
        munchLog.unshift({ text, type });
        if(munchLog.length > 4) munchLog.pop();
      }

      function animateMunch(){
        drawGrid();
        munchCtx.fillStyle = "rgba(0,0,0,0.05)";
        munchCtx.fillRect(0, CANVAS_HEIGHT - 80, CANVAS_WIDTH, 80);
        munchCtx.fillStyle = "#333";
        munchCtx.font = "11px ui-monospace, monospace";
        munchCtx.textAlign = "left";
        munchCtx.textBaseline = "top";
        munchLog.slice(0, 4).forEach((entry, index) => {
          munchCtx.fillStyle = entry.type === "good" ? "#28a745" : entry.type === "bad" ? "#dc3545" : "#333";
          munchCtx.fillText(entry.text, 12, CANVAS_HEIGHT - 76 + index * 18);
        });
      }

      function tickMunch(){
        munchTime -= 1;
        updateMunchDisplay();
        if(munchTime <= 0){
          finishMunch(munchScore >= MUNCH_GOAL);
        }
      }

      function initMunch(){
        munchGrid = buildGrid();
        munchPlayer = { col: 0, row: 0 };
        munchScore = 0;
        munchLives = 3;
        munchTime = 60;
        // try to load the custom muncher sprite (user should place image at MUNCHER_IMAGE_PATH)
        loadMuncherImage(MUNCHER_IMAGE_PATH);
        munchActive = true;
        addLog("Welcome to AI Muncher!", "info");
        updateMunchDisplay();
        document.addEventListener("keydown", handleMunchKey);
        munchInterval = setInterval(tickMunch, 1000);
        function frame(){
          animateMunch();
          if(munchActive) requestAnimationFrame(frame);
        }
        requestAnimationFrame(frame);
      }

      initMunch();
    }

      // --- Mini-game: What is AI? (binary classify examples) ---
      const WHAT_IS_AI_ITEMS = [
        { text: "A chatbot that answers customer support questions", isAI: true },
        { text: "A spreadsheet formula that sums a column", isAI: false },
        { text: "An image generator that creates artwork from a prompt", isAI: true },
        { text: "A scheduled email sent by rules at 9am", isAI: false },
        { text: "A language model summarising documents", isAI: true }
      ];

      function startGame_WhatIsAI(mission){
        let idx = 0;
        let score = 0;

        renderFrame(`
          <div class="miniWrap">
            <div class="miniHud">
              <div class="left">
                <h3>${mission.title}</h3>
                <p style="margin:6px 0 0;color:var(--muted)">${mission.tagline}</p>
              </div>
              <div class="right">
                <button class="btn ghost" id="exitWhatIsAI">Back</button>
              </div>
            </div>
            <div style="padding:18px 16px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
              <p style="margin:0 0 10px;font-size:13px;color:var(--muted)">Decide whether each example is AI-powered or not. Choose correctly to score points.</p>
              <div id="whatIsAIBody" style="margin-top:12px"></div>
            </div>
          </div>
        `);

        $("#exitWhatIsAI").onclick = () => navigate(Screen.MAP);

        function renderQuestion(){
          const item = WHAT_IS_AI_ITEMS[idx];
          const body = document.getElementById("whatIsAIBody");
          body.innerHTML = `
            <div style="font-size:18px;margin-bottom:12px">${item.text}</div>
            <div style="display:flex;gap:10px">
              <button class="btn primary" id="btnAI">AI</button>
              <button class="btn ghost" id="btnNotAI">Not AI</button>
            </div>
          `;

          $("#btnAI").onclick = () => handleChoice(true);
          $("#btnNotAI").onclick = () => handleChoice(false);
        }

        function handleChoice(choice){
          const correct = WHAT_IS_AI_ITEMS[idx].isAI === choice;
          if(correct) score++;
          idx++;
          if(idx >= WHAT_IS_AI_ITEMS.length){
            lastResult = { type:"whatis", missionId: mission.id, score, total: WHAT_IS_AI_ITEMS.length };
            if(score >= Math.ceil(WHAT_IS_AI_ITEMS.length * 0.6)){
              completeMission(mission.id);
            } else {
              openModal({ title: "Try Again", desc: `You scored ${score}. Need ${Math.ceil(WHAT_IS_AI_ITEMS.length * 0.6)} to pass.`, actions:[{label:"Retry", onClick:()=>{closeModal(); startGame_WhatIsAI(mission);}, kind:"primary"},{label:"Back", onClick:()=>{closeModal(); navigate(Screen.MAP);}, kind:"ghost"}] });
            }
            return;
          }
          renderQuestion();
        }

        renderQuestion();
      }

      // --- Mini-game: Reality Check (rate tasks suitability for AI) ---
      const REALITY_CHECK_TASKS = [
        { text: "Summarize meeting notes", goodForAI: true },
        { text: "Make legal judgments", goodForAI: false },
        { text: "Translate a short email", goodForAI: true },
        { text: "Decide employee terminations", goodForAI: false },
        { text: "Classify images for content moderation", goodForAI: true },
        { text: "Handle sensitive personal data without review", goodForAI: false }
      ];

      function startGame_RealityCheck(mission){
        let idx = 0;
        let score = 0;

        renderFrame(`
          <div class="miniWrap">
            <div class="miniHud">
              <div class="left">
                <h3>${mission.title}</h3>
                <p style="margin:6px 0 0;color:var(--muted)">${mission.tagline}</p>
              </div>
              <div class="right"><button class="btn ghost" id="exitReality">Back</button></div>
            </div>
            <div style="padding:18px 16px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
              <p style="margin:0 0 10px;font-size:13px;color:var(--muted)">Rate whether the task is a good fit for AI. Choose Best Fit or Not a Good Fit.</p>
              <div id="realityBody" style="margin-top:12px"></div>
            </div>
          </div>
        `);

        $("#exitReality").onclick = () => navigate(Screen.MAP);

        function renderTask(){
          const t = REALITY_CHECK_TASKS[idx];
          const body = document.getElementById("realityBody");
          body.innerHTML = `
            <div style="font-size:18px;margin-bottom:12px">${t.text}</div>
            <div style="display:flex;gap:10px">
              <button class="btn primary" id="btnGood">Good Fit</button>
              <button class="btn ghost" id="btnBad">Not a Good Fit</button>
            </div>
          `;
          $("#btnGood").onclick = () => handleAnswer(true);
          $("#btnBad").onclick = () => handleAnswer(false);
        }

        function handleAnswer(choice){
          const correct = REALITY_CHECK_TASKS[idx].goodForAI === choice;
          if(correct) score++;
          idx++;
          if(idx >= REALITY_CHECK_TASKS.length){
            lastResult = { type:"reality", missionId: mission.id, score, total: REALITY_CHECK_TASKS.length };
            if(score >= Math.ceil(REALITY_CHECK_TASKS.length * 0.6)){
              completeMission(mission.id);
            } else {
              openModal({ title: "Not Yet", desc: `You scored ${score}. Need ${Math.ceil(REALITY_CHECK_TASKS.length * 0.6)} to pass.`, actions:[{label:"Retry", onClick:()=>{closeModal(); startGame_RealityCheck(mission);}, kind:"primary"},{label:"Back", onClick:()=>{closeModal(); navigate(Screen.MAP);}, kind:"ghost"}] });
            }
            return;
          }
          renderTask();
        }

        renderTask();
      }

      // --- Mini-game: Information Management Whack-a-Mole ---
      const IM_BAD_PRACTICES = [
        "Store unencrypted sensitive files",
        "Share Protected B publicly",
        "Use AI outputs without verification",
        "Fail to classify documents",
        "Keep data longer than retention rules",
        "Send sensitive data to public cloud connectors"
      ];

      function startGame_IMWhack(mission){
        let whackCanvas, whackCtx, holes, whackScore = 0, whackInterval, timeLeft = 25;

        renderFrame(`
          <div class="miniWrap">
            <div class="miniHud">
              <div class="left"><h3>${mission.title}</h3><p style="margin:6px 0 0;color:var(--muted)">${mission.tagline}</p></div>
              <div class="right"><button class="btn ghost" id="exitWhack">Back</button></div>
            </div>
            <div style="padding:12px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
              <p style="margin:0 0 8px;font-size:13px;color:var(--muted)"><strong>Note:</strong> Classify data up to Protected B and always verify AI outputs. Smash bad IM practices with the mallet.</p>
              <div style="display:flex;gap:12px;align-items:center;margin-top:8px">
                <div class="pill">Score: <span id="whackScore">0</span></div>
                <div class="pill">Time: <span id="whackTime">25</span>s</div>
              </div>
              <div id="whackContainer" style="position:relative;display:inline-block;margin-top:12px;touch-action:none;">
                <canvas id="whackCanvas" width="600" height="300" style="background:#f8f9fa;border-radius:8px;cursor:none"></canvas>
                <div id="malletOverlay" style="position:absolute;top:0;left:0;width:72px;height:72px;pointer-events:none;display:none;transform:translate(-50%, -50%);background-size:contain;background-repeat:no-repeat;background-position:center;"></div>
              </div>
            </div>
          </div>
        `);

        function cleanupWhack(){
          clearInterval(whackInterval);
          if(animId) cancelAnimationFrame(animId);
          malletOverlay.style.display = 'none';
          try{
            whackCanvas.removeEventListener('pointerdown', handlePointer);
            whackCanvas.removeEventListener('pointermove', handlePointerMove);
            whackCanvas.removeEventListener('pointerleave', handlePointerLeave);
          }catch(e){}
        }

        $("#exitWhack").onclick = () => { cleanupWhack(); navigate(Screen.MAP); };

        whackCanvas = $("#whackCanvas");
        whackCtx = whackCanvas.getContext("2d");

        // make it easier to hit: larger visual holes and larger hit radius
        const HOLE_RADIUS = 54;
        const HIT_RADIUS = 100; // larger clickable area

        const malletOverlay = $("#malletOverlay");
        const malletSvg = '<svg xmlns="http://www.w3.org/2000/svg" width="96" height="96" viewBox="0 0 48 48">'
          +'<rect x="22" y="4" width="24" height="24" rx="3" fill="#8B4513" transform="rotate(20 40 16)"/>'
          +'<rect x="18" y="28" width="8" height="40" rx="3" fill="#5D4037" transform="rotate(20 24 46)"/>'
          +'</svg>';
        const malletData = 'data:image/svg+xml;utf8,' + encodeURIComponent(malletSvg);
        malletOverlay.style.backgroundImage = `url("${malletData}")`;
        whackCanvas.style.cursor = 'none';

        holes = [ {x:80,y:80},{x:240,y:80},{x:400,y:80},{x:560,y:80},{x:160,y:200},{x:320,y:200},{x:480,y:200} ];

        function draw(){
          whackCtx.clearRect(0,0,whackCanvas.width,whackCanvas.height);
          holes.forEach(h => {
            whackCtx.beginPath();
            whackCtx.fillStyle = h.active ? '#dc3545' : '#cccccc';
            whackCtx.arc(h.x, h.y, HOLE_RADIUS, 0, Math.PI*2);
            whackCtx.fill();
            if(h.active){
              whackCtx.fillStyle = '#ffffff';
              whackCtx.font = '12px ui-monospace, monospace';
              whackCtx.textAlign = 'center';
              whackCtx.textBaseline = 'middle';
              // wrap long labels
              const words = h.label.split(' ');
              const lines = [];
              let line = '';
              words.forEach(w => {
                if((line + ' ' + w).trim().length > 16){ lines.push(line.trim()); line = w; } else { line = (line + ' ' + w).trim(); }
              });
              if(line) lines.push(line);
              const startY = h.y - (lines.length-1)*8;
              lines.forEach((ln, i) => whackCtx.fillText(ln, h.x, startY + i*16));
            }
          });
        }

        function spawn(){
          const idx = Math.floor(Math.random()*holes.length);
          const h = holes[idx];
          h.active = true;
          h.label = IM_BAD_PRACTICES[Math.floor(Math.random()*IM_BAD_PRACTICES.length)];
          // keep active a bit longer so it's easier to click
          setTimeout(()=>{ h.active = false; draw(); }, 1600 + Math.random()*1400);
          draw();
        }

        // allow both click and touch to register
        let effects = [];

        // simple mallet sound using WebAudio
        let audioCtx = null;
        function playWhackSound(){
          try{
            if(!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            const now = audioCtx.currentTime;
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.type = 'sine';
            o.frequency.setValueAtTime(160, now);
            g.gain.setValueAtTime(0.0001, now);
            g.gain.exponentialRampToValueAtTime(0.6, now + 0.008);
            g.gain.exponentialRampToValueAtTime(0.001, now + 0.22);
            o.connect(g);
            g.connect(audioCtx.destination);
            o.start(now);
            o.stop(now + 0.24);
          }catch(e){/* ignore audio errors */}
        }

        function handlePointer(e){
          e.preventDefault();
          const rect = whackCanvas.getBoundingClientRect();
          const clientX = (e.touches && e.touches[0]) ? e.touches[0].clientX : e.clientX;
          const clientY = (e.touches && e.touches[0]) ? e.touches[0].clientY : e.clientY;
          const mx = clientX - rect.left;
          const my = clientY - rect.top;
          for(const h of holes){
            if(h.active){
              const dx = mx - h.x; const dy = my - h.y;
              if(Math.sqrt(dx*dx+dy*dy) <= HIT_RADIUS){
                h.active = false;
                whackScore += 2;
                effects.push({ x: h.x, y: h.y, t: Date.now() });
                playWhackSound();
                $("#whackScore").textContent = whackScore;
                draw();
                break;
              }
            }
          }
        }

        function handlePointerMove(e){
          const rect = whackCanvas.getBoundingClientRect();
          const clientX = (e.touches && e.touches[0]) ? e.touches[0].clientX : e.clientX;
          const clientY = (e.touches && e.touches[0]) ? e.touches[0].clientY : e.clientY;
          const mx = clientX - rect.left;
          const my = clientY - rect.top;
          malletOverlay.style.left = `${mx}px`;
          malletOverlay.style.top = `${my}px`;
          malletOverlay.style.display = 'block';
        }

        function handlePointerLeave(){
          malletOverlay.style.display = 'none';
        }

        whackCanvas.addEventListener('pointerdown', handlePointer);
        whackCanvas.addEventListener('pointermove', handlePointerMove);
        whackCanvas.addEventListener('pointerleave', handlePointerLeave);

        whackInterval = setInterval(()=>{
          if(Math.random() < 0.85) spawn();
          timeLeft -= 1;
          $("#whackTime").textContent = timeLeft;
          // cleanup old effects
          const now = Date.now();
          effects = effects.filter(e => now - e.t < 500);
          if(timeLeft <= 0){
            cleanupWhack();
            whackCanvas.style.cursor = 'default';
            lastResult = { type:'whack', missionId: mission.id, score: whackScore, total: 30 };
            if(whackScore >= 15) completeMission(mission.id);
            else openModal({ title: 'Time Up', desc: `You scored ${whackScore}. Need 15 to pass.`, actions:[{label:'Retry', onClick:()=>{closeModal(); startGame_IMWhack(mission);}, kind:'primary'},{label:'Back to Map', onClick:()=>{closeModal(); navigate(Screen.MAP);}, kind:'ghost'}] });
          }
        }, 1000);

        // draw initial frame including any effects
        function drawWithEffects(){
          draw();
          const now = Date.now();
          effects.forEach((ef) => {
            const age = (now - ef.t) / 500; // 0..1
            if(age >= 1) return;
            const radius = HOLE_RADIUS + age * 60;
            const alpha = 1 - age;
            whackCtx.beginPath();
            whackCtx.strokeStyle = `rgba(255,255,255,${alpha})`;
            whackCtx.lineWidth = 4 * (1 - age) + 1;
            whackCtx.arc(ef.x, ef.y, radius, 0, Math.PI*2);
            whackCtx.stroke();
          });
        }

        // animate effects at ~60fps
        let animId = null;
        function animate(){
          drawWithEffects();
          // remove old effects
          const now = Date.now();
          effects = effects.filter(e => now - e.t < 500);
          animId = requestAnimationFrame(animate);
        }
        animate();
      }

      const TRUST_STAGES = [
        { key: "Traceability", desc: "Verify sources, links, and evidence for claims." },
        { key: "Reliability", desc: "Confirm the response comes from accurate and trusted information." },
        { key: "Usefulness", desc: "Ensure the answer is relevant and solves the user’s question." },
        { key: "Safety", desc: "Check for bias, harmful content, and inappropriate assumptions." },
        { key: "Transparency", desc: "Look for clear reasoning and what the system is basing its answer on." }
      ];

      const TRUST_HEADS = [
        { prompt: "A recommendation says \"according to research\" but gives no cite or source.", stage: "Traceability" },
        { prompt: "An answer uses vague terms like \"many experts say\" without naming any source.", stage: "Traceability" },
        { prompt: "A response claims a statistic without a date or trusted reference.", stage: "Reliability" },
        { prompt: "An AI output repeats an unsupported fact that cannot be verified quickly.", stage: "Reliability" },
        { prompt: "The suggestion is technically correct but does not match the user’s actual need.", stage: "Usefulness" },
        { prompt: "The answer is relevant but leaves out a key detail for the task.", stage: "Usefulness" },
        { prompt: "The model offers a risky course of action without warning about side effects.", stage: "Safety" },
        { prompt: "The result contains a biased assumption about people or groups.", stage: "Safety" },
        { prompt: "The explanation describes the outcome but not how it was derived.", stage: "Transparency" },
        { prompt: "The output is confident but does not explain the reasoning behind it.", stage: "Transparency" }
      ];

      function startGame_Trust(mission) {
        let activeHeads = [];
        let trustScore = 0;
        let timeLeft = 40;
        let spawnInterval = null;
        let tickInterval = null;

        function createHead() {
          const template = TRUST_HEADS[Math.floor(Math.random() * TRUST_HEADS.length)];
          return {
            id: `${Date.now()}-${Math.random()}`,
            prompt: template.prompt,
            stage: template.stage,
            spawnTime: Date.now()
          };
        }

        function renderHeads() {
          const battle = $("#trustBattle");
          if (!battle) return;
          if (activeHeads.length === 0) {
            battle.innerHTML = `<div class="panel" style="background:rgba(40,167,69,.08);border:1px solid rgba(40,167,69,.2);">The Hallucination Hydra is wounded. Choose the right verification stage to defeat the next head.</div>`;
            return;
          }
          battle.innerHTML = activeHeads
            .map((head, index) => `
              <div class="panel" style="margin-bottom:12px;border:1px solid rgba(220,53,69,.2);background:rgba(220,53,69,.08);">
                <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
                  <strong>Hydra Head ${index + 1}</strong>
                  <span class="pill" style="background:rgba(220,53,69,.12);color:#c82333;">${head.stage.length > 11 ? head.stage.slice(0, 11) + '…' : head.stage}</span>
                </div>
                <p style="margin:0;font-size:14px;color:var(--muted);line-height:1.5;">${head.prompt}</p>
              </div>
            `)
            .join("");
        }

        function updateHud() {
          $("#trustHeads").textContent = `${activeHeads.length}`;
          $("#trustScore").textContent = `${trustScore}`;
          $("#trustTime").textContent = `${timeLeft}`;
        }

        function loseMission(reason) {
          cleanupTrust();
          openModal({
            title: "Hydra Overwhelms You",
            desc: reason,
            actions: [
              {
                label: "Retry",
                kind: "primary",
                onClick: () => {
                  closeModal();
                  startGame_Trust(mission);
                }
              },
              {
                label: "Back to Map",
                kind: "ghost",
                onClick: () => {
                  closeModal();
                  navigate(Screen.MAP);
                }
              }
            ]
          });
        }

        function completeTrust() {
          cleanupTrust();
          lastResult = { type: "trust", missionId: mission.id, score: trustScore, total: 7 };
          completeMission(mission.id);
        }

        function cleanupTrust() {
          if (spawnInterval) clearInterval(spawnInterval);
          if (tickInterval) clearInterval(tickInterval);
        }

        function checkRegeneration() {
          const now = Date.now();
          let regenerated = false;
          activeHeads.forEach((head) => {
            if (now - head.spawnTime >= 8000) {
              head.spawnTime = now;
              if (activeHeads.length < 5) {
                activeHeads.push(createHead());
                regenerated = true;
              }
            }
          });
          if (regenerated) {
            renderHeads();
            updateHud();
          }
          if (activeHeads.length >= 5) {
            loseMission("The Hallucination Hydra regenerated too many heads. Keep your verification focused and swift.");
          }
        }

        function handleStageClick(e) {
          const selectedStage = e.currentTarget.dataset.stage;
          if (!selectedStage || activeHeads.length === 0) return;
          const currentHead = activeHeads[0];
          if (selectedStage === currentHead.stage) {
            trustScore += 1;
            activeHeads.shift();
            if (trustScore >= 7) {
              openModal({
                title: "Hydra Defeated!",
                desc: "Your T.R.U.S.T. checks stood strong. The Hallucination Hydra is defeated.",
                actions: [
                  {
                    label: "Continue",
                    kind: "primary",
                    onClick: () => {
                      closeModal();
                      completeTrust();
                    }
                  }
                ]
              });
              return;
            }
            if (activeHeads.length === 0) {
              activeHeads.push(createHead());
            }
          } else {
            timeLeft = Math.max(0, timeLeft - 5);
            if (activeHeads.length < 5) {
              activeHeads.push(createHead());
            }
          }
          renderHeads();
          updateHud();
        }

        renderFrame(`
          <div class="miniWrap">
            <div class="miniHud">
              <div class="left">
                <h3>${mission.title}</h3>
                <p style="margin:6px 0 0;color:var(--muted)">${mission.tagline}</p>
              </div>
              <div class="right">
                <button class="btn ghost" id="exitTrust">Back</button>
              </div>
            </div>
            <div style="padding:16px;border:1px solid var(--line);border-radius:14px;background:rgba(0,0,0,.03);margin-top:12px">
              <h4 style="margin:0 0 8px;font-size:18px">T.R.U.S.T. Protocol</h4>
              <p style="margin:0 0 10px;font-size:13px;color:var(--muted);line-height:1.6">Use Traceability, Reliability, Usefulness, Safety and Transparency to flag the correct stage for each Hallucination Hydra head. If you wait too long, the hydra regenerates.</p>
              <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;">
                ${TRUST_STAGES.map(stage => `<div class="pill" style="padding:10px;text-align:left;line-height:1.4"><strong>${stage.key}</strong><br><span style="font-size:12px;color:var(--muted)">${stage.desc}</span></div>`).join("")}
              </div>
            </div>
            <div style="display:flex;gap:12px;flex-wrap:wrap;align-items:center;margin-top:14px">
              <div class="pill">Hydra Heads: <span id="trustHeads">0</span></div>
              <div class="pill">Score: <span id="trustScore">0</span></div>
              <div class="pill">Time: <span id="trustTime">40</span>s</div>
            </div>
            <div id="trustBattle" style="margin-top:16px"></div>
            <div style="display:flex;gap:10px;flex-wrap:wrap;margin-top:14px">
              ${TRUST_STAGES.map(stage => `<button class="btn ghost trustStageBtn" data-stage="${stage.key}">${stage.key}</button>`).join("")}
            </div>
          </div>
        `);

        $("#exitTrust").onclick = () => {
          cleanupTrust();
          navigate(Screen.MAP);
        };

        renderHeads();
        updateHud();

        document.querySelectorAll(".trustStageBtn").forEach(button => {
          button.onclick = handleStageClick;
        });

        activeHeads.push(createHead());
        activeHeads.push(createHead());
        renderHeads();
        updateHud();

        spawnInterval = setInterval(() => {
          if (activeHeads.length < 3) {
            activeHeads.push(createHead());
            renderHeads();
            updateHud();
          }
        }, 2800);

        tickInterval = setInterval(() => {
          timeLeft -= 1;
          if (timeLeft < 0) timeLeft = 0;
          updateHud();
          checkRegeneration();
          if (timeLeft <= 0) {
            loseMission("Time ran out while you were verifying the Hydra. Try the T.R.U.S.T. protocol again.");
          }
        }, 1000);
      }

    let redactionGameInterval;
    let redactionCurrentChallengeIdx = 0;
    let redactionTotalSensitiveWords = 0;
    let redactionRedactedCount = 0;
    let redactionIncorrectRedactions = 0;
    let redactionTimeLeft = 0;
    let redactionChallenge = null;

    function startGame_Redact(mission) {
      redactionChallenge = REDACTION_CHALLENGES[mission.id][redactionCurrentChallengeIdx];

      if (!redactionChallenge) {
        lastResult = {
          type: "redact",
          missionId: mission.id,
          score: redactionRedactedCount,
          total: redactionTotalSensitiveWords
        };

        redactionCurrentChallengeIdx = 0;
        redactionRedactedCount = 0;
        redactionIncorrectRedactions = 0;
        completeMission(mission.id);
        return;
      }

      redactionTotalSensitiveWords = redactionChallenge.sensitiveWords.length;
      redactionRedactedCount = 0;
      redactionIncorrectRedactions = 0;
      redactionTimeLeft = redactionChallenge.timeLimit;

      if (redactionGameInterval) clearInterval(redactionGameInterval);

      const words = redactionChallenge.text.split(/(\s+)/).map((word, idx) => {
        if (word.trim() === "") return `<span class="word space"> </span>`;
        return `<span class="word" data-word-idx="${idx}">${word}</span>`;
      }).join("");

      renderFrame(`
        <div class="miniWrap">
          <div class="miniHud">
            <div class="left">
              <h3 style="margin:0">${mission.title}: ${redactionChallenge.title}</h3>
              <div class="pill">Time: <span id="redactionTimerDisplay">${redactionTimeLeft}s</span></div>
              <div class="pill">Redacted: <span id="redactionScoreDisplay">0/${redactionTotalSensitiveWords}</span></div>
            </div>
            <div class="right">
              <button class="btn ghost" id="exitRedact">Exit Mission</button>
            </div>
          </div>
          <div class="redaction-timer-bar" id="redactionTimerBar" style="width:100%;"></div>
          <div class="panel quizQ" style="margin-top:12px;">
            <p class="prompt" style="font-size:13px;color:var(--cyan)">Instructions: ${redactionChallenge.instructions}</p>
          </div>
          <div class="redaction-area panel" id="redactionArea">
            ${words}
          </div>
        </div>
      `);

      $("#exitRedact").onclick = () => {
        clearInterval(redactionGameInterval);
        navigate(Screen.MAP);
      };

      const redactionArea = $("#redactionArea");
      redactionArea.querySelectorAll(".word").forEach(wordSpan => {
        wordSpan.onclick = (e) => handleRedactionClick(e, redactionChallenge);
      });

      redactionGameInterval = setInterval(updateRedactionGame, 1000);
    }

    function updateRedactionGame() {
      redactionTimeLeft--;
      $("#redactionTimerDisplay").textContent = `${redactionTimeLeft}s`;

      const progressBar = $("#redactionTimerBar");
      progressBar.style.width = `${(redactionTimeLeft / redactionChallenge.timeLimit) * 100}%`;

      if (redactionTimeLeft <= 5) {
        progressBar.style.backgroundColor = "var(--warn)";
      }

      if (redactionTimeLeft <= 0 || redactionRedactedCount >= redactionTotalSensitiveWords) {
        clearInterval(redactionGameInterval);
        processRedactionResult();
      }
    }

    function handleRedactionClick(event, challenge) {
      const wordSpan = event.target;
      if (wordSpan.classList.contains("redacted") || wordSpan.classList.contains("incorrect")) return;

      const clickedWord = wordSpan.textContent.trim();
      const isSensitive = challenge.sensitiveWords.some(sw => {
        return clickedWord.toLowerCase() === sw.toLowerCase();
      });

      if (isSensitive) {
        wordSpan.classList.add("redacted");
        redactionRedactedCount++;
        $("#redactionScoreDisplay").textContent = `${redactionRedactedCount}/${redactionTotalSensitiveWords}`;
      } else {
        wordSpan.classList.add("incorrect");
        redactionIncorrectRedactions++;
      }

      if (redactionRedactedCount >= redactionTotalSensitiveWords) {
        clearInterval(redactionGameInterval);
        processRedactionResult();
      }
    }

    function processRedactionResult() {
      const mission = MISSIONS.find(m => m.id === activeMissionId);
      const success = redactionRedactedCount >= redactionTotalSensitiveWords && redactionIncorrectRedactions <= 2;

      openModal({
        title: success ? "Redaction Complete!" : "Redaction Failed!",
        desc: success ? redactionChallenge.rationaleSuccess : redactionChallenge.rationaleFail,
        bodyHTML: state.settings.showRationales
          ? `<div class="rationale"><h4>Summary</h4><p>You redacted ${redactionRedactedCount} of ${redactionTotalSensitiveWords} sensitive items. You made ${redactionIncorrectRedactions} incorrect redactions.</p></div>`
          : "",
        actions: [
          {
            label: "Next Challenge",
            onClick: () => {
              closeModal();
              redactionCurrentChallengeIdx++;
              startGame_Redact(mission);
            },
            kind: "primary"
          },
          {
            label: "Back to Map",
            onClick: () => {
              closeModal();
              clearInterval(redactionGameInterval);
              redactionCurrentChallengeIdx = 0;
              navigate(Screen.MAP);
            },
            kind: "ghost"
          }
        ]
      });
    }

    let playerHp, bossHp, currentBossPhase, bossLog, bossGameInterval;
    let currentBossMoveIdx = 0;

    function startGame_Boss(mission){
      playerHp = 100;
      currentBossPhase = 0;
      bossHp = BOSS_PHASES[currentBossPhase].hp;
      bossLog = [];
      currentBossMoveIdx = 0;

      if(bossGameInterval) clearInterval(bossGameInterval);
      renderBossBattle();
      bossGameInterval = setInterval(updateBossBattle, 1000);
    }

    function renderBossBattle(){
      const mission = MISSIONS.find(m => m.id === activeMissionId);
      const currentPhase = BOSS_PHASES[currentBossPhase];

      if(!mission || !currentPhase){
        console.error("Boss mission or phase not found.");
        navigate(Screen.MAP);
        return;
      }

      const currentMove = currentPhase.moves[currentBossMoveIdx];
      const answersHtml = currentMove.options.map((opt, idx) => `
        <button class="ans" data-answer-idx="${idx}">
          <span class="key">${String.fromCharCode(65 + idx)}</span>${opt}
        </button>
      `).join("");

      const logItemsHtml = bossLog.map(item => `<div class="item ${item.type}">${item.text}</div>`).reverse().join("");

      renderFrame(`
        <div class="grid cols2">
          <div>
            <div class="bars">
              <div class="panel hp player">
                <div class="meta"><span>Player</span><span>${playerHp}/100 HP</span></div>
                <div class="bar"><div style="width:${playerHp}%"></div></div>
              </div>
              <div class="panel hp boss">
                <div class="meta"><span>${currentPhase.title}</span><span>${bossHp}/${currentPhase.hp} HP</span></div>
                <div class="bar"><div style="width:${(bossHp / currentPhase.hp) * 100}%"></div></div>
              </div>
            </div>

            <div class="panel quizQ" style="margin-top:12px">
              <p class="prompt">${currentMove.prompt}</p>
            </div>

            <div class="bossArena">
              <div style="text-align:center;max-width:520px;padding:18px">
                <div style="font-size:24px;font-weight:700;color:var(--indigo);margin-bottom:8px">The Wyrm Looms</div>
                <div style="font-size:13px;color:var(--muted);line-height:1.6">Choose responsible AI actions to weaken the hallucination wyrm and avoid misleading outcomes.</div>
              </div>
            </div>

            <div class="answers" style="margin-top:12px">
              ${answersHtml}
            </div>
          </div>

          <div class="panel log">
            <div class="title">Battle Log</div>
            <div class="items">${logItemsHtml}</div>
          </div>
        </div>
      `);

      root.querySelectorAll(".answers .ans").forEach(button => {
        button.onclick = (e) => handleBossAnswerClick(e, currentMove);
      });
    }

    function updateBossBattle(){
      // placeholder for future animation hooks
    }

    function addLog(text, type = ""){
      bossLog.unshift({ text, type });
      if(bossLog.length > 10) bossLog.pop();
    }

    function handleBossAnswerClick(event, move){
      const selectedIdx = parseInt(event.currentTarget.dataset.answerIdx);
      const isCorrect = selectedIdx === move.answer;
      let damageToBoss = 0;
      let damageToPlayer = 0;

      if(isCorrect){
        damageToBoss = 15 + Math.floor(Math.random() * 10);
        addLog(`Player chose wisely, dealing ${damageToBoss} damage!`, "hit");
      } else {
        damageToPlayer = 10 + Math.floor(Math.random() * 15);
        playerHp = clamp(playerHp - damageToPlayer, 0, 100);
        addLog(`Player made a mistake, taking ${damageToPlayer} damage!`, "hurt");
      }

      bossHp = clamp(bossHp - damageToBoss, 0, BOSS_PHASES[currentBossPhase].hp);

      openModal({
        title: isCorrect ? "Impactful Choice!" : "Suboptimal Action!",
        desc: isCorrect ? "Your responsible AI decision weakened the Wyrm." : "The Wyrm capitalized on your misstep.",
        bodyHTML: state.settings.showRationales
          ? `<div class="rationale"><h4>Rationale</h4><p>${move.rationale}</p></div>`
          : "",
        actions:[
          {
            label:"Continue Battle",
            onClick:() => {
              closeModal();
              handleBossTurn();
            }
          }
        ]
      });
    }

    function handleBossTurn(){
      if(playerHp <= 0){
        clearInterval(bossGameInterval);
        openModal({
          title: "Defeat!",
          desc: "The Hallucination Wyrm was too powerful. Your quest for responsible AI ends here...",
          actions:[
            {
              label:"Retry Mission",
              onClick:() => {
                closeModal();
                startGame_Boss(MISSIONS.find(m => m.id === activeMissionId));
              },
              kind:"primary"
            },
            {
              label:"Back to Map",
              onClick:() => {
                closeModal();
                navigate(Screen.MAP);
              },
              kind:"ghost"
            }
          ]
        });
        return;
      }

      if(bossHp <= 0){
        currentBossPhase++;
        if(currentBossPhase < BOSS_PHASES.length){
          const nextPhase = BOSS_PHASES[currentBossPhase];
          bossHp = nextPhase.hp;
          currentBossMoveIdx = 0;
          addLog(`The Wyrm enters Phase ${currentBossPhase + 1}: ${nextPhase.title}!`, "phase");
          renderBossBattle();
        } else {
          clearInterval(bossGameInterval);
          lastResult = {
            type: "boss",
            missionId: activeMissionId,
            score: playerHp,
            total: 100
          };
          completeMission(activeMissionId);
        }
        return;
      }

      currentBossMoveIdx++;
      if(currentBossMoveIdx >= BOSS_PHASES[currentBossPhase].moves.length){
        currentBossMoveIdx = 0;
      }
      renderBossBattle();
    }
function renderBadges(){
      const badgesHtml = BADGES.map(badge => {
        const earned = state.unlockedBadges.includes(badge.id);
        return `
          <div class="badgeTile">
            <div class="badgeIcon">
              <img src="${badge.icon}" alt="${badge.name}" />
            </div>
            <div class="badgeMeta">
              <h3>${badge.name}</h3>
              <p>${badge.desc}</p>
            </div>
            <div class="status ${earned ? "earned" : ""}">${earned ? "Earned" : "Locked"}</div>
          </div>
        `;
      }).join("");

      renderFrame(`
        <div class="panel">
          <div class="inner">
            <h2>Your Achievements</h2>
            <p class="muted">Badges reflect your mastery of Responsible AI principles. Keep going to earn them all.</p>
            <div class="badgeGrid" style="margin-top:22px">${badgesHtml}</div>
          </div>
        </div>
      `);
    }

    function renderCodex(){
      const principlesHtml = PRINCIPLES.map(p => `
        <li><strong>${p.label}:</strong> <span class="muted">Explore missions related to ${p.label}.</span></li>
      `).join("");

      renderFrame(`
        <div class="panel">
          <div class="inner">
            <h2>AI Ethics Codex</h2>
            <p class="muted">This codex outlines the core principles of Responsible AI, guiding your journey.</p>
            <ul style="margin-top:22px; display:grid; gap:12px; list-style:none; padding:0">
              ${principlesHtml}
            </ul>
          </div>
        </div>
      `);
    }

    function renderSettings(){
      renderFrame(`
        <div class="panel">
          <div class="inner">
            <h2>Settings</h2>
            <div style="margin-top:22px">
              <label style="display:flex;align-items:center;gap:10px;font-size:24px">
                <input type="checkbox" id="toggleSideQuests" ${state.settings.showSideQuests ? "checked" : ""}>
                <span>Show Side Quests on Map</span>
              </label>

              <label style="display:flex;align-items:center;gap:10px;font-size:24px;margin-top:10px">
                <input type="checkbox" id="toggleRationales" ${state.settings.showRationales ? "checked" : ""}>
                <span>Show Rationales after Quiz Answers</span>
              </label>

              <label style="display:flex;align-items:center;gap:10px;font-size:24px;margin-top:10px">
                <input type="checkbox" id="toggleReduceMotion" ${state.settings.reduceMotion ? "checked" : ""}>
                <span>Reduce Motion/Animations</span>
              </label>
            </div>

            <div class="hr"></div>

            <button class="btn danger" id="resetGameBtn">Reset Game Data</button>
            <button class="btn ghost" id="printCertBtn" style="margin-left:10px">Print Certificate</button>
          </div>
        </div>
      `);

      $("#toggleSideQuests").onchange = (e) => {
        state.settings.showSideQuests = e.target.checked;
        saveState();
        if (screen === Screen.MAP) {
          if (mapAnimationId) cancelAnimationFrame(mapAnimationId);
          renderMap();
        }
      };

      $("#toggleRationales").onchange = (e) => {
        state.settings.showRationales = e.target.checked;
        saveState();
      };

      $("#toggleReduceMotion").onchange = (e) => {
        state.settings.reduceMotion = e.target.checked;
        setReduceMotionClass();
        saveState();
        if (screen === Screen.MAP) {
          if (mapAnimationId) cancelAnimationFrame(mapAnimationId);
          renderMap();
        }
      };

      $("#resetGameBtn").onclick = () => {
        if(confirm("Are you sure you want to reset all game progress? This cannot be undone.")){
          localStorage.removeItem(STORAGE_KEY);
          state = defaultState();
          navigate(Screen.TITLE);
        }
      };

      $("#printCertBtn").onclick = () => printCertificate();
    }

    function printCertificate(){
      const mainMissions = MISSIONS.filter(m => m.type === "main" || m.type === "boss");
      const completedMainMissions = mainMissions.filter(m => state.completed.includes(m.id));
      const allMainCompleted = completedMainMissions.length === mainMissions.length;

      if(!allMainCompleted){
        alert("You must complete all main missions (including the boss) to earn your certificate.");
        return;
      }

      const earnedBadgesHtml = BADGES.filter(b => state.unlockedBadges.includes(b.id)).map(b => `
        <div style="display:inline-flex;align-items:center;gap:8px;margin:5px;padding:8px 12px;border:1px solid #ccc;border-radius:999px;font-size:13px;background:#f0f0f0;color:#333">
          <img src="${b.icon}" style="width:24px;height:24px;vertical-align:middle" />
          ${b.name}
        </div>
      `).join("");

      const certificateHtml = `
        <div style="width:210mm;min-height:297mm;padding:25mm;box-sizing:border-box;font-family:arial;color:#000;background:#fff;display:flex;flex-direction:column;justify-content:space-between;border:20px solid #c0c0c0;">
          <div style="text-align:center;font-size:24px;font-weight:bold;margin-bottom:20px">Certificate of Achievement</div>
          <div style="text-align:center;margin-bottom:30px">This certifies that</div>
          <div style="text-align:center;font-size:48px;font-family:cursive;margin-bottom:30px;border-bottom:2px solid #000;padding-bottom:10px">${state.playerName}</div>
          <div style="text-align:center;margin-bottom:30px">has successfully completed the</div>
          <div style="text-align:center;font-size:36px;font-weight:bold;margin-bottom:30px">AI Ethics Arcade</div>
          <div style="text-align:center;margin-bottom:40px">and demonstrated mastery of Responsible AI principles.</div>
          <div style="text-align:center;font-size:18px;margin-bottom:30px">Earned Badges:</div>
          <div style="text-align:center;margin-bottom:40px">${earnedBadgesHtml}</div>
          <div style="display:flex;justify-content:space-around;margin-top:auto">
            <div style="text-align:center">
              <div style="border-top:1px solid #000;padding-top:5px;margin-top:20px;font-size:24px">Date: ${new Date().toLocaleDateString()}</div>
            </div>
            <div style="text-align:center">
              <div style="border-top:1px solid #000;padding-top:5px;margin-top:20px;font-size:14px">AI Arcade Admin</div>
            </div>
          </div>
        </div>
      `;

      $("#printArea").innerHTML = certificateHtml;
      window.print();
    }

    function renderDebrief(){
      if(!lastResult){
        navigate(Screen.MAP);
        return;
      }

      const mission = MISSIONS.find(m => m.id === lastResult.missionId);
      if(!mission){
        navigate(Screen.MAP);
        return;
      }

      let resultText = "";
      let rankHtml = "";
      const xpGained = mission.difficulty * 10;

      if(lastResult.type === "quiz"){
        const { rank, label } = scoreToRank(lastResult.score, lastResult.total);
        resultText = `You correctly answered ${lastResult.score} out of ${lastResult.total} questions.`;
        rankHtml = `<div style="font-size:2em;font-family:var(--pixel);color:var(--gold);margin-top:10px">Rank: ${rank} - ${label}</div>`;
      } else if (lastResult.type === "dodge"){
        const { rank, label } = scoreToRank(lastResult.score, lastResult.total);
        resultText = `You dodged ${lastResult.score} threats. (Target: ${lastResult.total})`;
        rankHtml = `<div style="font-size:2em;font-family:var(--pixel);color:var(--gold);margin-top:10px">Rank: ${rank} - ${label}</div>`;
      } else if (lastResult.type === "redact"){
        const { rank, label } = scoreToRank(lastResult.score, lastResult.total);
        resultText = `You redacted ${lastResult.score} of ${lastResult.total} sensitive items.`;
        rankHtml = `<div style="font-size:2em;font-family:var(--pixel);color:var(--gold);margin-top:10px">Rank: ${rank} - ${label}</div>`;
      } else if (lastResult.type === "boss"){
        const { rank, label } = scoreToRank(lastResult.score, lastResult.total);
        resultText = `You defeated the Hallucination Wyrm with ${lastResult.score}% HP remaining.`;
        rankHtml = `<div style="font-size:2em;font-family:var(--pixel);color:var(--gold);margin-top:10px">Rank: ${rank} - ${label}</div>`;
      }

      const unlockedBadge = BADGES.find(b => mission.unlocks.includes(b.id));
      const badgeUnlockHtml = unlockedBadge ? `
        <div style="margin-top:22px;padding:14px;background:rgba(23,162,184,.1);border:1px solid rgba(23,162,184,.3);border-radius:var(--radius2);display:flex;gap:12px;align-items:center">
          <img src="${unlockedBadge.icon}" style="width:48px;height:48px;image-rendering:pixelated" />
          <div>
            <div style="font-size:1.1em;font-weight:bold;color:var(--cyan)">Badge Unlocked!</div>
            <div style="font-size:.9em;color:var(--muted)">${unlockedBadge.name}: ${unlockedBadge.desc}</div>
          </div>
        </div>
      ` : "";

      renderFrame(`
        <div class="panel">
          <div class="inner" style="text-align:center; padding:44px 22px">
            <div style="font-family:var(--pixel);font-size:28px;line-height:1.2;margin-bottom:12px;color:var(--good)">Mission Complete!</div>
            <h3 style="font-size:1.5em;margin:0">${mission.title}</h3>
            <p style="max-width:500px;margin:0 auto;line-height:1.6;color:var(--muted);margin-top:10px">${resultText}</p>
            ${rankHtml}
            <p style="font-size:1.1em;font-family:var(--pixel);margin-top:20px;color:var(--gold)">XP Gained: +${xpGained}</p>
            ${badgeUnlockHtml}
            <div style="margin-top:30px">
              <button class="btn primary" id="backToMap">Back to Map</button>
              <button class="btn ghost" id="viewBadges">View Badges</button>
            </div>
          </div>
        </div>
      `);

      $("#backToMap").onclick = () => {
        lastResult = null;
        navigate(Screen.MAP);
      };

      $("#viewBadges").onclick = () => {
        lastResult = null;
        navigate(Screen.BADGES);
      };
    }

    function navigate(targetScreen){
      screen = targetScreen;

      if (mapAnimationId) cancelAnimationFrame(mapAnimationId);

      switch(screen){
        case Screen.TITLE:
          renderTitleScreen();
          break;
        case Screen.MAP:
          renderMap();
          break;
        case Screen.MISSION:
          renderQuizMission();
          break;
        case Screen.BADGES:
          renderBadges();
          break;
        case Screen.CODEX:
          renderCodex();
          break;
        case Screen.SETTINGS:
          renderSettings();
          break;
        case Screen.DEBRIEF:
          renderDebrief();
          break;
      }

      if(screen === Screen.MAP && !state.completed.includes(activeMissionId)){
        const mission = MISSIONS.find(m => m.id === activeMissionId);
        if(!(mission && mission.level <= state.unlockedMainLevel)){
          const nextMission = MISSIONS.find(m =>
            m.type === "main" &&
            m.level === state.unlockedMainLevel &&
            !state.completed.includes(m.id)
          );
          if(nextMission) activeMissionId = nextMission.id;
        }

        const activeNode = NODES.find(n => n.missionId === activeMissionId);
        if (activeNode) {
          const sp = getScaledPos(activeNode);
          targetSpritePosition = { x: sp.x, y: sp.y };
        }
      }
    }

    // DEBUG/DEV MODE
    let devModeEnabled = false;
    let devPanelEventsAttached = false;

    function toggleDevMode(){
      devModeEnabled = !devModeEnabled;
      const devPanel = document.getElementById("devPanel");
      if(devModeEnabled){
        devPanel.classList.add("open");
        updateDevPanel();
      } else {
        devPanel.classList.remove("open");
      }
    }

    function updateDevPanel(){
      const devPanel = document.getElementById("devPanel");
      const currentMission = MISSIONS.find(m => m.id === activeMissionId);
      
      let missionOptions = MISSIONS
        .filter(m => m && m.title)
        .map(m => 
          `<option value="${m.id}" ${m.id === activeMissionId ? "selected" : ""}>${m.title} (${m.id})</option>`
        ).join("");

      devPanel.innerHTML = `
        <h3>🔧 DEV MODE</h3>
        
        <div class="devSection">
          <div class="devInfo">
            Current: <strong>${screen}</strong><br>
            Mission: <strong>${currentMission?.title || "none"}</strong>
          </div>
        </div>

        <div class="devSection">
          <label>Jump to Screen:</label>
          <button type="button" class="devBtn" data-action="jump-screen" data-screen="${Screen.TITLE}">Title</button>
          <button type="button" class="devBtn" data-action="jump-screen" data-screen="${Screen.MAP}">Map</button>
          <button type="button" class="devBtn" data-action="jump-screen" data-screen="${Screen.BADGES}">Badges</button>
          <button type="button" class="devBtn" data-action="jump-screen" data-screen="${Screen.CODEX}">Codex</button>
          <button type="button" class="devBtn" data-action="jump-screen" data-screen="${Screen.SETTINGS}">Settings</button>
        </div>

        <div class="devSection">
          <label>Jump to Mission:</label>
          <select id="devMissionSelect">
            ${missionOptions}
          </select>
        </div>

        <div class="devSection">
          <label>Quick Actions:</label>
          <button type="button" class="devBtn" id="devStartMissionBtn">Start Current Mission</button>
          <button type="button" class="devBtn" id="devCompleteMissionBtn">Complete Current Mission</button>
          <button type="button" class="devBtn" id="devResetProgressBtn">Reset All Progress</button>
        </div>

        <div class="devSection">
          <label>Unlock Level:</label>
          <input type="number" id="devLevelInput" value="${state.unlockedMainLevel}" min="1" max="10">
          <button type="button" class="devBtn" id="devSetLevelBtn">Set Level</button>
        </div>

        <div class="devInfo" style="font-size:10px;margin-top:8px;">
          <strong>Keyboard Shortcuts:</strong><br>
          Ctrl+Shift+D = Toggle Panel<br>
          Ctrl+M = Map<br>
          Ctrl+1-9 = First 9 missions
        </div>
      `;

      devPanel.style.pointerEvents = "auto";

      if(!devPanelEventsAttached){
        devPanel.addEventListener('click', (event) => {
          const button = event.target.closest('button');
          if(!button || !devPanel.contains(button)) return;

          const action = button.dataset.action;
          if(action === 'jump-screen'){
            devJumpScreen(button.dataset.screen);
            return;
          }

          if(button.id === 'devStartMissionBtn'){
            devStartCurrentMission();
            return;
          }
          if(button.id === 'devCompleteMissionBtn'){
            devCompleteCurrentMission();
            return;
          }
          if(button.id === 'devResetProgressBtn'){
            devResetProgress();
            return;
          }
          if(button.id === 'devSetLevelBtn'){
            devSetLevel();
            return;
          }
        });
        devPanelEventsAttached = true;
      }

      const missionSelect = devPanel.querySelector('#devMissionSelect');
      if (missionSelect) {
        missionSelect.onchange = (event) => devJumpMission(event.target.value);
      }
    }

    function devJumpScreen(targetScreen){
      navigate(targetScreen);
      updateDevPanel();
    }

    function devJumpMission(missionId){
      activeMissionId = missionId;
      navigate(Screen.MAP);
      updateDevPanel();
    }

    function devStartCurrentMission(){
      const mission = MISSIONS.find(m => m.id === activeMissionId);
      if(mission) navigateToMissionMode(mission);
    }

    function devCompleteCurrentMission(){
      if(!state.completed.includes(activeMissionId)){
        state.completed.push(activeMissionId);
        const mission = MISSIONS.find(m => m.id === activeMissionId);
        if(mission && mission.level > state.unlockedMainLevel){
          state.unlockedMainLevel = mission.level;
        }
        saveState();
        navigate(Screen.MAP);
        updateDevPanel();
      }
    }

    function devResetProgress(){
      if(confirm("Reset all progress?")){
        state = defaultState();
        saveState();
        navigate(Screen.TITLE);
        updateDevPanel();
      }
    }

    function devSetLevel(){
      const input = document.getElementById("devLevelInput");
      const newLevel = parseInt(input.value) || 1;
      state.unlockedMainLevel = Math.max(1, Math.min(10, newLevel));
      saveState();
      navigate(screen);
      updateDevPanel();
    }

    // Keyboard shortcuts
    document.addEventListener("keydown", (e) => {
      if(e.ctrlKey && e.shiftKey && e.key === "D"){
        e.preventDefault();
        toggleDevMode();
      }
      
      if(!devModeEnabled) return;

      if(e.ctrlKey && e.key === "m"){
        e.preventDefault();
        devJumpScreen(Screen.MAP);
      }

      if(e.ctrlKey && /^[1-9]$/.test(e.key)){
        e.preventDefault();
        const index = parseInt(e.key) - 1;
        if(index < MISSIONS.length){
          devJumpMission(MISSIONS[index].id);
        }
      }
    });

    setReduceMotionClass();
    navigate(Screen.TITLE);
  })();
  </script>
</body>
</html>
