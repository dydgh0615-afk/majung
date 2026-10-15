<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title>마중부동산 중개법인 통합 관리 시스템</title>
<style>
  :root{
    --ink:#1c1a17;
    --paper:#f7f4ee;
    --line:#d8d0c0;
    --stamp:#8a3324;
    --gold:#a6822f;
    --green:#3c6e4a;
    --blue:#33587a;
    --muted:#8a8477;
  }
  *{box-sizing:border-box;}
  body{margin:0;background:var(--paper);color:var(--ink);font-family:sans-serif;padding:20px;}
  .container{max-width:1200px;margin:0 auto;display:flex;flex-direction:column;gap:15px;}
  .header{font-size:20px;font-weight:bold;border-bottom:2px solid var(--ink);padding-bottom:10px;display:flex;justify-content:space-between;align-items:center;}
  .status-box{background:#8a8477;color:#fff;padding:8px;text-align:center;font-size:13px;border-radius:6px;}
  .toolbar{display:flex;gap:10px;flex-wrap:wrap;}
  .btn{background:var(--ink);color:#fff;border:none;padding:8px 14px;border-radius:6px;cursor:pointer;font-weight:bold;font-size:13px;}
  .btn-gold{background:var(--gold);}
  .btn-green{background:var(--green);}
  .btn-red{background:var(--stamp);}
  .card{background:#fff;border:1px solid var(--line);border-radius:8px;padding:12px;margin-bottom:8px;}
  .card-head{display:flex;justify-content:space-between;align-items:center;cursor:pointer;font-weight:bold;}
  .detail{display:none;margin-top:10px;border-top:1px dashed var(--line);padding-top:10px;}
  .card.open .detail{display:block;}
  input, textarea{width:100%;padding:6px;border:1px solid var(--line);border-radius:4px;font-family:inherit;margin-top:4px;}
  textarea{min-height:50px;}
</style>
</head>
<body>

<div class="container">
  <div class="header">
    <span>🏢 마중부동산 통합 관리 시스템</span>
    <span id="countText" style="font-size:13px;color:var(--muted);">총 0건</span>
  </div>
  
  <div id="statusLine" class="status-box">서버 연결 중…</div>

  <div class="toolbar">
    <button class="btn btn-gold" id="parseBtn">✨ 텍스트 자동입력</button>
    <button class="btn btn-green" id="addBtn">+ 신규 매물 등록</button>
  </div>

  <div id="listingContainer"></div>
</div>

<!-- Firebase SDK -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.1/firebase-app.js";
import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/9.22.1/firebase-database.js";

const firebaseConfig = {
  apiKey: "AIzaSyCTvPE95ugU07AR33pW7lNhAxttGUCrWnA",
  authDomain: "majung-f9a54.firebaseapp.com",
  projectId: "majung-f9a54",
  storageBucket: "majung-f9a54.firebasestorage.app",
  messagingSenderId: "1018053609412",
  appId: "1:1018053609412:web:232057cee64d0e5deec691",
  measurementId: "G-KHM5XDE9LQ"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);

let listings = [];
let openCardId = null;

function setStatus(msg, ok){
  const el = document.getElementById("statusLine");
  if(!el) return;
  el.textContent = msg;
  el.style.background = ok ? "#3c6e4a" : "#8a3324";
}

async function saveToFirebase(){
  try {
    await set(ref(db, 'majung_data/listings'), listings);
    setStatus("🟢 파이어베이스 실시간 동기화 완료", true);
  } catch(e) {
    setStatus("⚠ 저장 실패: " + e.message, false);
  }
}

function initSync(){
  try {
    onValue(ref(db, 'majung_data/listings'), (snapshot) => {
      const data = snapshot.val();
      listings = (data && Array.isArray(data)) ? data : [];
      render();
      setStatus("🟢 파이어베이스 실시간 동기화 완료", true);
    }, (error) => {
      setStatus("⚠ 연결 오류: " + error.message, false);
    });
  } catch(e) {
    setStatus("⚠ 초기화 실패: " + e.message, false);
  }
}

function render(){
  const container = document.getElementById("listingContainer");
  if(!container) return;
  container.innerHTML = "";
  
  document.getElementById("countText").textContent = "총 " + listings.length + "건";

  if(listings.length === 0){
    container.innerHTML = "<div style='text-align:center;color:var(--muted);padding:30px;'>등록된 매물이 없습니다. [+ 신규 매물 등록]을 눌러보세요.</div>";
    return;
  }

  listings.forEach((item) => {
    const card = document.createElement("div");
    card.className = "card" + (item.id === openCardId ? " open" : "");

    const head = document.createElement("div");
    head.className = "card-head";
    
    const titleSpan = document.createElement("span");
    titleSpan.textContent = "[" + (item.type || "월세") + "] " + (item.addr || "주소 미입력") + " (" + (item.price || "금액 미입력") + ")";
    
    const delBtn = document.createElement("button");
    delBtn.className = "btn btn-red";
    delBtn.style.cssText = "padding:4px 8px;font-size:11px;";
    delBtn.textContent = "삭제";

    head.appendChild(titleSpan);
    head.appendChild(delBtn);

    head.addEventListener("click", (e) => {
      if(e.target === delBtn) return;
      openCardId = (openCardId === item.id) ? null : item.id;
      render();
    });

    delBtn.addEventListener("click", async () => {
      if(confirm("정말 삭제하시겠습니까?")){
        listings = listings.filter(function(x){ return x.id !== item.id; });
        await saveToFirebase();
        render();
      }
    });

    card.appendChild(head);

    const detail = document.createElement("div");
    detail.className = "detail";
    
    const label1 = document.createElement("label");
    label1.textContent = "주소";
    const input1 = document.createElement("input");
    input1.type = "text";
    input1.value = item.addr || "";
    input1.addEventListener("input", async () => {
      item.addr = input1.value;
      await saveToFirebase();
    });

    const label2 = document.createElement("label");
    label2.style.cssText = "margin-top:8px;display:block;";
    label2.textContent = "금액";
    const input2 = document.createElement("input");
    input2.type = "text";
    input2.value = item.price || "";
    input2.addEventListener("input", async () => {
      item.price = input2.value;
      await saveToFirebase();
    });

    const label3 = document.createElement("label");
    label3.style.cssText = "margin-top:8px;display:block;";
    label3.textContent = "비고";
    const textarea = document.createElement("textarea");
    textarea.value = item.note || "";
    textarea.addEventListener("input", async () => {
      item.note = textarea.value;
      await saveToFirebase();
    });

    detail.appendChild(label1);
    detail.appendChild(input1);
    detail.appendChild(label2);
    detail.appendChild(input2);
    detail.appendChild(label3);
    detail.appendChild(textarea);

    card.appendChild(detail);
    container.appendChild(card);
  });
}

document.getElementById("addBtn").addEventListener("click", async () => {
  const newItem = {
    id: "n" + Date.now(),
    type: "월세",
    addr: "새 매물 주소",
    price: "보증금 / 월세",
    note: ""
  };
  listings.unshift(newItem);
  openCardId = newItem.id;
  await saveToFirebase();
  render();
});

document.getElementById("parseBtn").addEventListener("click", () => {
  const text = prompt("매물 텍스트를 붙여넣으세요:");
  if(!text) return;
  const newItem = {
    id: "n" + Date.now(),
    type: "월세",
    addr: text.split("\n")[0] || "입력된 주소",
    price: "확인 필요",
    note: text
  };
  listings.unshift(newItem);
  openCardId = newItem.id;
  saveToFirebase();
  render();
});

initSync();
</script>
</body>
</html>
