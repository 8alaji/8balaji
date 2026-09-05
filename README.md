<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Phonebook</title>
  <style>
    :root {
      --primary: #2563eb;
      --primary-hover: #1d4ed8;
      --bg: #0f172a;
      --surface: #1e293b;
      --surface-card: #243248;
      --border: #334155;
      --text: #f8fafc;
      --text-muted: #94a3b8;
      --accent: #38bdf8;
      --success: #22c55e;
      --warning: #eab308;
      --danger: #ef4444;
      --radius: 12px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Noto Sans", Ubuntu, Cantarell, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      display: flex;
      justify-content: center;
      min-height: 100vh;
      padding: 0;
    }

    .app-container {
      width: 100%;
      max-width: 680px;
      display: flex;
      flex-direction: column;
      height: 100vh;
      background-color: var(--surface);
      box-shadow: 0 10px 25px -5px rgba(0,0,0,0.5);
      position: relative;
    }

    header {
      padding: 16px 20px 12px;
      background: rgba(30, 41, 59, 0.95);
      backdrop-filter: blur(8px);
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 10;
    }

    .header-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .title-group h1 {
      font-size: 1.4rem;
      font-weight: 700;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .badge-count {
      font-size: 0.75rem;
      background: var(--surface-card);
      border: 1px solid var(--border);
      color: var(--accent);
      padding: 2px 8px;
      border-radius: 999px;
      font-weight: 600;
    }

    .header-actions {
      display: flex;
      gap: 8px;
    }

    .btn-icon {
      background: var(--surface-card);
      border: 1px solid var(--border);
      color: var(--text);
      width: 36px;
      height: 36px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      font-size: 1rem;
      transition: all 0.2s;
    }

    .btn-icon:hover {
      background: var(--primary);
      border-color: var(--primary);
    }

    .search-box {
      position: relative;
      margin-bottom: 10px;
    }

    .search-box input {
      width: 100%;
      padding: 10px 14px 10px 38px;
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: 10px;
      color: var(--text);
      font-size: 0.95rem;
      outline: none;
      transition: border-color 0.2s;
    }

    .search-box input:focus {
      border-color: var(--accent);
    }

    .search-icon {
      position: absolute;
      left: 12px;
      top: 50%;
      transform: translateY(-50%);
      color: var(--text-muted);
      pointer-events: none;
      font-size: 0.9rem;
    }

    .clear-search {
      position: absolute;
      right: 12px;
      top: 50%;
      transform: translateY(-50%);
      background: none;
      border: none;
      color: var(--text-muted);
      cursor: pointer;
      display: none;
      font-size: 1rem;
    }

    .filter-tabs {
      display: flex;
      gap: 8px;
      overflow-x: auto;
      scrollbar-width: none;
      padding-bottom: 4px;
    }

    .filter-tabs::-webkit-scrollbar {
      display: none;
    }

    .tab-pill {
      background: var(--bg);
      border: 1px solid var(--border);
      color: var(--text-muted);
      padding: 5px 12px;
      border-radius: 999px;
      font-size: 0.8rem;
      font-weight: 500;
      white-space: nowrap;
      cursor: pointer;
      transition: all 0.2s;
    }

    .tab-pill.active {
      background: var(--primary);
      color: #fff;
      border-color: var(--primary);
    }

    .contact-list-wrap {
      flex: 1;
      overflow-y: auto;
      padding: 8px 16px 80px;
      scrollbar-width: thin;
      scrollbar-color: var(--border) transparent;
    }

    .section-header {
      font-size: 0.75rem;
      font-weight: 700;
      color: var(--accent);
      padding: 12px 6px 4px;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }

    .contact-card {
      display: flex;
      align-items: center;
      gap: 12px;
      background: var(--surface-card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 10px 14px;
      margin-bottom: 8px;
      cursor: pointer;
      transition: transform 0.15s ease, border-color 0.15s ease;
    }

    .contact-card:hover {
      border-color: var(--accent);
      transform: translateY(-1px);
    }

    .avatar {
      width: 44px;
      height: 44px;
      border-radius: 50%;
      background: linear-gradient(135deg, #3b82f6, #8b5cf6);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 1rem;
      flex-shrink: 0;
      overflow: hidden;
      border: 2px solid var(--border);
    }

    .contact-info {
      flex: 1;
      min-width: 0;
    }

    .contact-name {
      font-size: 0.95rem;
      font-weight: 600;
      color: var(--text);
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .contact-phone {
      font-size: 0.8rem;
      color: var(--text-muted);
      margin-top: 2px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .badge-mini {
      font-size: 0.65rem;
      padding: 1px 5px;
      border-radius: 4px;
      font-weight: 600;
    }
    .badge-fav { background: rgba(234, 179, 8, 0.2); color: var(--warning); border: 1px solid rgba(234, 179, 8, 0.4); }
    .badge-recent { background: rgba(34, 197, 94, 0.2); color: var(--success); border: 1px solid rgba(34, 197, 94, 0.4); }
    .badge-bank { background: rgba(14, 165, 233, 0.2); color: #38bdf8; border: 1px solid rgba(14, 165, 233, 0.4); }

    .quick-actions {
      display: flex;
      gap: 6px;
    }

    .btn-action {
      background: var(--surface);
      border: 1px solid var(--border);
      color: var(--text);
      width: 34px;
      height: 34px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.9rem;
      text-decoration: none;
      transition: all 0.2s;
    }

    .btn-action.call:hover { background: var(--success); color: #fff; border-color: var(--success); }
    .btn-action.wa:hover { background: #25d366; color: #fff; border-color: #25d366; }

    /* Modal / Details Drawer */
    .modal-backdrop {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.7);
      backdrop-filter: blur(4px);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 100;
      padding: 16px;
    }

    .modal-backdrop.active {
      display: flex;
    }

    .modal-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      width: 100%;
      max-width: 440px;
      max-height: 90vh;
      overflow-y: auto;
      padding: 24px;
      position: relative;
      animation: modalSlide 0.2s ease-out;
    }

    @keyframes modalSlide {
      from { transform: translateY(20px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }

    .modal-close {
      position: absolute;
      top: 16px;
      right: 16px;
      background: var(--surface-card);
      border: 1px solid var(--border);
      color: var(--text-muted);
      width: 32px;
      height: 32px;
      border-radius: 50%;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .modal-header-info {
      text-align: center;
      margin-bottom: 20px;
    }

    .modal-avatar {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      background: linear-gradient(135deg, #3b82f6, #9333ea);
      margin: 0 auto 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      font-weight: 700;
      border: 3px solid var(--accent);
    }

    .modal-name {
      font-size: 1.3rem;
      font-weight: 700;
      margin-bottom: 4px;
    }

    .modal-detail-item {
      background: var(--surface-card);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 14px;
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .modal-detail-label {
      font-size: 0.75rem;
      color: var(--text-muted);
      text-transform: uppercase;
      margin-bottom: 2px;
    }

    .modal-detail-val {
      font-size: 0.95rem;
      font-weight: 600;
      word-break: break-all;
    }

    .btn-modal-action {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      width: 100%;
      padding: 12px;
      border-radius: 10px;
      font-weight: 600;
      font-size: 0.95rem;
      text-decoration: none;
      cursor: pointer;
      border: none;
      margin-top: 8px;
      transition: opacity 0.2s;
    }
    .btn-modal-action.call { background: var(--success); color: #fff; }
    .btn-modal-action.wa { background: #25d366; color: #fff; }
    .btn-modal-action.copy { background: var(--surface-card); color: var(--text); border: 1px solid var(--border); }

    /* Toast Notification */
    .toast {
      position: fixed;
      bottom: 24px;
      left: 50%;
      transform: translateX(-50%) translateY(100px);
      background: var(--accent);
      color: #0f172a;
      padding: 10px 20px;
      border-radius: 999px;
      font-weight: 600;
      font-size: 0.85rem;
      z-index: 200;
      transition: transform 0.25s ease-out;
      box-shadow: 0 8px 20px rgba(0,0,0,0.4);
    }
    .toast.show {
      transform: translateX(-50%) translateY(0);
    }

    .empty-state {
      text-align: center;
      padding: 60px 20px;
      color: var(--text-muted);
    }
    .empty-state span {
      font-size: 2.5rem;
      display: block;
      margin-bottom: 12px;
    }
  </style>
</head>
<body>

<div class="app-container">
  <header>
    <div class="header-top">
      <div class="title-group">
        <h1>Contacts <span class="badge-count" id="countBadge">0</span></h1>
      </div>
      <div class="header-actions">
        <button class="btn-icon" title="Add Contact" onclick="openAddModal()">➕</button>
        <button class="btn-icon" title="Export Contacts" onclick="exportContacts()">📤</button>
      </div>
    </div>

    <div class="search-box">
      <span class="search-icon">🔍</span>
      <input type="text" id="searchInput" placeholder="Search by name, number, place..." oninput="handleSearch()">
      <button class="clear-search" id="clearBtn" onclick="clearSearch()">✕</button>
    </div>

    <div class="filter-tabs">
      <button class="tab-pill active" onclick="setFilter('all', this)">All</button>
      <button class="tab-pill" onclick="setFilter('favorites', this)">⭐ Starred</button>
      <button class="tab-pill" onclick="setFilter('recent', this)">📞 Recent</button>
      <button class="tab-pill" onclick="setFilter('banks', this)">🏦 Banks & Services</button>
      <button class="tab-pill" onclick="setFilter('location', this)">📍 With Address</button>
    </div>
  </header>

  <main class="contact-list-wrap" id="contactsContainer">
    <!-- Contact cards will be rendered dynamically -->
  </main>
</div>

<!-- Details Modal -->
<div class="modal-backdrop" id="detailModal">
  <div class="modal-card">
    <button class="modal-close" onclick="closeModal('detailModal')">✕</button>
    <div class="modal-header-info">
      <div class="modal-avatar" id="mAvatar">?</div>
      <div class="modal-name" id="mName">Contact Name</div>
      <div id="mBadges" style="display:flex; justify-content:center; gap:4px; margin-top:4px;"></div>
    </div>

    <div id="mFields"></div>

    <div style="margin-top: 16px;">
      <a id="mCallBtn" class="btn-modal-action call" href="#">📞 Call Primary</a>
      <a id="mWaBtn" class="btn-modal-action wa" href="#" target="_blank">💬 Chat on WhatsApp</a>
      <button class="btn-modal-action copy" onclick="copyNumber()">📋 Copy Number</button>
    </div>
  </div>
</div>

<!-- Add Contact Modal -->
<div class="modal-backdrop" id="addModal">
  <div class="modal-card">
    <button class="modal-close" onclick="closeModal('addModal')">✕</button>
    <h3 style="margin-bottom: 16px; font-weight:700;">Add New Contact</h3>
    <div style="display:flex; flex-direction:column; gap:12px;">
      <div>
        <label class="modal-detail-label">Full Name</label>
        <input type="text" id="addName" class="search-box input" style="width:100%; padding:8px 12px; background:var(--bg); border:1px solid var(--border); border-radius:8px; color:var(--text);" placeholder="e.g. Ramesh Patil">
      </div>
      <div>
        <label class="modal-detail-label">Phone Number</label>
        <input type="tel" id="addPhone" class="search-box input" style="width:100%; padding:8px 12px; background:var(--bg); border:1px solid var(--border); border-radius:8px; color:var(--text);" placeholder="e.g. +919876543210">
      </div>
      <div>
        <label class="modal-detail-label">Alternate Number / Note (Optional)</label>
        <input type="text" id="addAlt" class="search-box input" style="width:100%; padding:8px 12px; background:var(--bg); border:1px solid var(--border); border-radius:8px; color:var(--text);" placeholder="Work / Bank info">
      </div>
      <div>
        <label class="modal-detail-label">Address / Location (Optional)</label>
        <input type="text" id="addLoc" class="search-box input" style="width:100%; padding:8px 12px; background:var(--bg); border:1px solid var(--border); border-radius:8px; color:var(--text);" placeholder="e.g. Maharashtra, India">
      </div>
      <button class="btn-modal-action call" onclick="saveContact()" style="margin-top:10px;">Save Contact</button>
    </div>
  </div>
</div>

<div class="toast" id="toast">Copied to clipboard!</div>

<script>
// Master contacts data parsed from 20260101_104200.vcf with all UID/Aadhaar identifiers redacted
const RAW_CONTACTS = [
  { n: "Shek", p: "8421313944" },
  { n: "Rahul", p: "9049431290" },
  { n: "Dilip .Bele", p: "+918380928274" },
  { n: "M G Bank", p: "80001763192", b: true },
  { n: "U", p: "4303701425161" },
  { n: "Letsup", p: "77710012345" },
  { n: "Manoj Shinde", p: "+919172834334", loc: "Maharashtra, India" },
  { n: "Lutetlhati", p: "+917972955987", alt: "07709049549", rec: true, loc: "Maharashtra, India" },
  { n: "Baja", p: "7507473312" },
  { n: "Tanubai Vithalrao D", p: "[Aadhaar Redacted]" },
  { n: "R uid", p: "[Aadhaar Redacted]" },
  { n: "BOI", p: "09266135135", b: true },
  { n: "RP Bank", p: "0000002311002002478", b: true },
  { n: "BOI Bank", p: "077118110000862", b: true },
  { n: "BOIS Bank", p: "077110110001132", b: true },
  { n: "D SP Bank", p: "002311002200910", b: true },
  { n: "Rushi UID", p: "[Aadhaar Redacted]" },
  { n: "जयप्रकाश Bank", p: "0004066003174", b: true },
  { n: "Deepak UID", p: "[Aadhaar Redacted]" },
  { n: "TP Bank", p: "0023110020040082", b: true },
  { n: "Rekha UID", p: "[Aadhaar Redacted]" },
  { n: "Papa UID", p: "[Aadhaar Redacted]" },
  { n: "IDBI Bank A/c", p: "0504104000065788", b: true },
  { n: "Gulabrao Daji", p: "8552934660" },
  { n: "Rupesh Da", p: "+919146985521", bday: "2004-11-20" },
  { n: "अंकुश टा़", p: "+919307241306" },
  { n: "Munna Tractor", p: "8262902801" },
  { n: "अभिजित सरनाईक", p: "9284633691" },
  { n: "Let Up", p: "+918956799130" },
  { n: "दत्ता गोरेगाव", p: "8605186616" },
  { n: "संजू पवार", p: "8830884383" },
  { n: "अरुण", p: "+917507027947" },
  { n: "भरतराव दुधगावकर", p: "9673097697" },
  { n: "करडा", p: "+919604930017" },
  { n: "D S", p: "+918669551678" },
  { n: "Vilasrao Sangari", p: "+917798855252" },
  { n: "Shinde Netaji", p: "+919359587082", loc: "Maharashtra, India" },
  { n: "कल्याण शिंदे", p: "+918668690733" },
  { n: "9049300260 (No Name)", p: "9049300260" },
  { n: "Balu Patange", p: "+919049300207", loc: "Maharashtra, India" },
  { n: "बाबुराव दे,", p: "9067541385" },
  { n: "शिवा काका", p: "9850749970" },
  { n: "वि,कासवकर", p: "9552039677" },
  { n: "वाकी आक्का", p: "+917350701611" },
  { n: "Uttam P", p: "+919834534738" },
  { n: "Shesherav Patil", p: "+919403062743", loc: "Maharashtra, India" },
  { n: "Faruk Misatri", p: "+917013143420", loc: "Andhra Pradesh, India" },
  { n: "Noor Bhai. Higni", p: "+918766819157", loc: "Maharashtra, India" },
  { n: "दिनेश घुगे", p: "+919657943756" },
  { n: "Pinka Potra", p: "+917066409113", loc: "Maharashtra, India" },
  { n: "Rajesh Patil", p: "+919359220182" },
  { n: "Dnanu", p: "None" },
  { n: "Self Help", p: "111", b: true },
  { n: "Balance in form", p: "1112", b: true },
  { n: "Bonus Cards", p: "444", b: true },
  { n: "Patange S T,", p: "+917020252265" },
  { n: "G.K.Patil", p: "+917875858493" },
  { n: "Rangarao Mama", p: "+918007672589" },
  { n: "Dr. vishal", p: "+918999675336", alt: "+918390199308", rec: true },
  { n: "Shivajirao patan", p: "+918408884028" },
  { n: "आघावसर", p: "+918857048722" },
  { n: "Gorlegov", p: "+919527724093" },
  { n: "राधा", p: "+919604121699" },
  { n: "आपा", p: "+919604135101" },
  { n: "Postman", p: "+919689936634" },
  { n: "F.R.Patange", p: "+919767622737" },
  { n: "Dr.Nana", p: "+919767741788" },
  { n: "P.Bapu", p: "+919767936771" },
  { n: "Nanu da.", p: "+919970241864" },
  { n: "Bitu2", p: "07350920108" },
  { n: "Gulabrao", p: "07588678712" },
  { n: "Vodafone Care", p: "111", b: true },
  { n: "check internate", p: "11122", b: true },
  { n: "Top Services", p: "123", b: true },
  { n: "Recharge", p: "140", b: true },
  { n: "Call C.", p: "155223", b: true },
  { n: "Complaints Only", p: "198", b: true },
  { n: "Shivajirao saheb", p: "7030072330" },
  { n: "रंगा", p: "7768851241" },
  { n: "दिलीप काळे", p: "8007220600" },
  { n: "Raju Daji", p: "8698537980" },
  { n: "Pavde b.", p: "8805848974" },
  { n: "P.R.Nilknate", p: "8806468511" },
  { n: "L.Saheb", p: "9404070954" },
  { n: "B.M.", p: "9421867693" },
  { n: "हातगाव काका", p: "9423538651" },
  { n: "आघाव", p: "9527762117", alt: "+918888610146", rec: true },
  { n: "पो.Pat", p: "9545315585" },
  { n: "महाराज", p: "9545485690" },
  { n: "Vikas Bhau", p: "9623679922" },
  { n: "Sakharam", p: "9764877875" },
  { n: "R.Pavde", p: "9767345309" },
  { n: "Balance Informat", p: "11124", b: true },
  { n: "lone net balance", p: "13042", b: true },
  { n: "check Balance ba", p: "99491", b: true },
  { n: "Ranga", p: "+917768851241" },
  { n: "M.M.", p: "+917798561790" },
  { n: "COMPANY", p: "+918061020360", b: true },
  { n: "Remeshrao परभ", p: "+918087268551" },
  { n: "Gaju दुकानदार", p: "+918380928275" },
  { n: "पांगरा शिंदे", p: "+918390206393" },
  { n: "सुधाकर दे पा", p: "+918605039289" },
  { n: "B.Mama Takalga", p: "+918605178126" },
  { n: "D2", p: "+918856024230" },
  { n: "Pune Gajanan", p: "+918888249339" },
  { n: "Sudharshan", p: "+918888582702" },
  { n: "B.Mastar", p: "+918888781851" },
  { n: "अपा", p: "+919604135101" },
  { n: "Vijukaka", p: "+919767462660" },
  { n: "Sunil Renapur", p: "+919921399600" },
  { n: "Kiran", p: "+919922668450" },
  { n: "Dipak A/C N.", p: "002311002200910", b: true },
  { n: "बाळूमाम", p: "07218296460" },
  { n: "IDBI-BANK", p: "1800226999", b: true },
  { n: "Lala", p: "7030212082" },
  { n: "मुना", p: "7057946945" },
  { n: "Bele D.", p: "8007312911" },
  { n: "K,v,k,", p: "8448444593" },
  { n: "Satish", p: "8806267385" },
  { n: "R.U.N.2
