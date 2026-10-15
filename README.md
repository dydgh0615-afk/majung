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
    --stamp-soft:#f1e2dc;
    --gold:#a6822f;
    --gold-soft:#f4ecd8;
    --green:#3c6e4a;
    --green-soft:#e4ede2;
    --blue:#33587a;
    --blue-soft:#e3ecf3;
    --muted:#8a8477;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:var(--paper);
    color:var(--ink);
    font-family:"Noto Sans KR","Malgun Gothic",sans-serif;
    padding:20px 12px 80px;
  }
  .layout-container{
    max-width:1350px;
    margin:0 auto;
    display:grid;
    grid-template-columns: 1fr 440px;
    gap:20px;
    align-items:start;
  }
  @media(max-width:1024px){
    .layout-container{grid-template-columns: 1fr;}
  }
  .main-column{min-width:0;}
  .side-column{
    background:#fffdf7;
    border:1px solid var(--line);
    border-radius:12px;
    padding:16px;
    position:sticky;
    top:20px;
    box-shadow:0 4px 12px rgba(0,0,0,.03);
    display:flex;
    flex-direction:column;
    gap:16px;
  }
  .head{
    display:flex;
    justify-content:space-between;
    align-items:flex-end;
    border-bottom:2px solid var(--ink);
    padding-bottom:14px;
    margin-bottom:18px;
  }
  .head h1{font-size:22px;margin:0;letter-spacing:-0.5px;}
  .head .sub{font-size:12px;color:var(--muted);margin-top:4px;}
  .count{font-size:12px;color:var(--muted);text-align:right;}
  
  .memo-box{
    margin-bottom:18px;
    border:1px solid var(--line);
    border-radius:10px;
    background:#fffdf7;
    box-shadow:inset 0 0 0 1px rgba(166,130,47,.08);
    overflow:hidden;
  }
  .memo-tabs{
    display:flex;background:#f2efe9;border-bottom:1px solid var(--line);flex-wrap:wrap;
  }
  .memo-tab{
    flex:1;padding:8px 4px;text-align:center;font-size:11.5px;font-weight:700;cursor:pointer;color:var(--muted);background:transparent;border:none;white-space:nowrap;
  }
  .memo-tab.active{background:#fffdf7;color:var(--gold);border-bottom:2px solid var(--gold);}
  .memo-body{padding:12px 14px;}
  .memo-input-wrap{display:flex;flex-direction:column;gap:8px;margin-bottom:14px;background:#fff;border:1px solid var(--line);border-radius:8px;padding:10px;}
  .memo-input-wrap textarea{
    width:100%;border:none;background:transparent;font-size:13px;font-family:inherit;color:var(--ink);resize:vertical;min-height:50px;line-height:1.6;
  }
  .memo-input-wrap textarea:focus{outline:none;}
  .memo-toolbar{display:flex;justify-content:space-between;align-items:center;border-top:1px dashed var(--line);padding-top:8px;}
  .memo-date-info{font-size:11px;color:var(--muted);}
  .memo-save-btn{border:none;background:var(--gold);color:#fff;padding:5px 12px;border-radius:6px;font-size:12px;font-weight:700;cursor:pointer;}
  
  .memo-list{display:flex;flex-direction:column;gap:8px;max-height:180px;overflow-y:auto;}
  .memo-item{background:#fff;border:1px solid var(--line);border-radius:8px;padding:10px;font-size:12px;}
  .memo-item-head{display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:4px;border-bottom:1px solid #f2efe9;padding-bottom:4px;}
  .memo-item-content{white-space:pre-wrap;line-height:1.5;color:var(--ink);}
  .memo-item-del{border:none;background:none;color:var(--stamp);cursor:pointer;font-size:11px;}

  .searchbar{margin-bottom:12px;}
  .searchbar input{
    width:100%;border:1px solid var(--line);border-radius:999px;padding:9px 16px;font-size:13px;font-family:inherit;background:#fff;color:var(--ink);
  }
  .searchbar input:focus{outline:2px solid var(--gold);border-color:var(--gold);}
  
  .filter-section{margin-bottom:12px;display:flex;flex-direction:column;gap:8px;background:#f2efe9;padding:10px 12px;border-radius:10px;border:1px solid var(--line);}
  .filter-row{display:flex;gap:6px;align-items:center;flex-wrap:wrap;}
  .filter-row-label{font-size:11.5px;font-weight:700;color:var(--muted);width:52px;flex:none;}
  .filter-row-content{display:flex;gap:6px;flex-wrap:wrap;align-items:center;flex:1;}

  .filter-btn{
    border:1px solid var(--line);background:#fff;padding:5px 10px;border-radius:999px;font-size:11.5px;cursor:pointer;color:var(--muted);white-space:nowrap;
  }
  .filter-btn.active{border-color:var(--ink);color:var(--ink);background:var(--gold-soft);font-weight:700;}
  
  .action-toolbar{display:flex;gap:8px;margin-bottom:16px;justify-content:space-between;align-items:center;flex-wrap:wrap;}
  .action-left{display:flex;gap:6px;align-items:center;}
  .action-right{display:flex;gap:8px;align-items:center;}

  .add-btn{
    border:none;background:var(--ink);color:#fff;padding:7px 14px;border-radius:999px;font-size:12.5px;cursor:pointer;font-weight:700;
  }
  .add-btn:hover{opacity:.88;}

  .card{border:1px solid var(--line);border-radius:10px;background:#fff;margin-bottom:10px;overflow:hidden;}
  .card.moved{opacity:.6;}
  .card-head{display:flex;align-items:center;gap:8px;padding:12px 14px;cursor:pointer;user-select:none;}
  
  .multi-chk{
    width:18px;height:18px;flex:none;border:1.5px solid var(--line);border-radius:4px;cursor:pointer;display:flex;align-items:center;justify-content:center;background:#fff;accent-color:var(--stamp);
  }
  .chk{
    width:18px;height:18px;flex:none;border:1.5px solid var(--line);border-radius:5px;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:12px;color:var(--green);
  }
  .chk.on{border-color:var(--green);background:var(--green-soft);}
  .badge{flex:none;font-size:11px;font-weight:700;padding:3px 8px;border-radius:6px;white-space:nowrap;}
  .badge.매매{background:var(--stamp-soft);color:var(--stamp);}
  .badge.전세{background:var(--green-soft);color:var(--green);}
  .badge.월세{background:var(--gold-soft);color:var(--gold);}
  .badge.신축{background:#ede9fe;color:#7c3aed;}
  .subbadge{flex:none;font-size:10px;font-weight:700;padding:2px 6px;border-radius:5px;background:var(--blue-soft);color:var(--blue);white-space:nowrap;}
  .subbadge.created-time{background:#f3f4f6;color:#4b5563;}
  .subbadge.inquiry-count{background:var(--gold-soft);color:var(--gold);}
  .holdbtn{
    flex:none;font-size:10px;font-weight:700;padding:2px 6px;border-radius:5px;border:1px solid var(--line);background:#fff;color:var(--muted);cursor:pointer;white-space:nowrap;
  }
  .holdbtn.on{border-color:#b06a00;background:#fbe9cf;color:#8a4b00;}
  .card.held{opacity:.55;}
  .subbadge.hold{background:#fbe9cf;color:#8a4b00;}
  .subbadge.moved-time{background:var(--stamp-soft);color:var(--stamp);}
  .subbadge.trash{background:#fee2e2;color:#991b1b;}

  .card-del-btn{
    flex:none;font-size:10px;font-weight:700;padding:2px 6px;border-radius:5px;border:1px solid var(--stamp);background:#fff;color:var(--stamp);cursor:pointer;white-space:nowrap;
  }
  .card-del-btn:hover{background:var(--stamp-soft);}

  .title-block{flex:1;min-width:0;}
  .addr{font-size:14px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
  .price{font-size:12px;color:var(--stamp);font-weight:700;margin-top:1px;}
  .caret{flex:none;font-size:12px;color:var(--muted);transition:transform .15s ease;}
  .card.open .caret{transform:rotate(90deg);}
  .detail{display:none;padding:4px 14px 16px;border-top:1px dashed var(--line);}
  .card.open .detail{display:block;}
  .movehint{font-size:11px;color:var(--muted);margin-top:10px;}
  .grid{display:grid;grid-template-columns:88px 1fr;gap:7px 10px;margin-top:12px;}
  .grid label{font-size:12px;color:var(--muted);padding-top:7px;white-space:nowrap;}
  .grid input, .grid textarea{
    width:100%;border:1px solid var(--line);border-radius:6px;padding:6px 8px;font-size:13px;font-family:inherit;background:#fdfcf9;color:var(--ink);
  }
  .grid textarea{resize:vertical;min-height:40px;line-height:1.5;}
  .grid input:focus, .grid textarea:focus{outline:2px solid var(--gold);border-color:var(--gold);}
  .row-actions{display:flex;justify-content:space-between;align-items:center;margin-top:14px;}
  .saved{font-size:11px;color:var(--green);opacity:0;transition:opacity .3s;}
  .saved.show{opacity:1;}
  .restore-btn{border:1px solid var(--green);color:var(--green);background:#fff;font-size:12px;padding:6px 12px;border-radius:999px;cursor:pointer;}
  .empty{text-align:center;color:var(--muted);font-size:13px;padding:40px 0;}
  select.type-select{border:1px solid var(--line);border-radius:6px;padding:6px 8px;font-size:13px;font-family:inherit;background:#fdfcf9;}

  .side-section { border-bottom: 1px solid var(--line); padding-bottom: 14px; }
  .side-section h3 { font-size: 14.5px; margin: 0 0 10px; color: var(--ink); border-left: 3px solid var(--gold); padding-left: 6px; }
  .needs-group{display:flex;flex-direction:column;gap:6px;margin-bottom:10px;}
  .needs-group label{font-size:11px;font-weight:700;color:var(--muted);}
  .needs-group textarea, .needs-group input, .needs-group select{
    width:100%;border:1px solid var(--line);border-radius:6px;padding:6px 8px;font-size:12px;background:#fff;font-family:inherit;
  }
  .rec-results{display:flex;flex-direction:column;gap:8px;max-height:220px;overflow-y:auto;margin-top:8px;}
  .rec-card{background:#fff;border:1px solid var(--line);border-radius:8px;padding:8px 10px;font-size:11.5px;display:flex;flex-direction:column;gap:3px;}
  .rec-card-top{display:flex;justify-content:space-between;align-items:center;}
  .rec-addr{font-weight:700;font-size:12px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:200px;}
  .rec-price{color:var(--stamp);font-weight:700;}
  
  .client-card { background:#fff; border:1px solid var(--line); border-radius:10px; padding:12px; font-size:12.5px; margin-bottom:10px; display:flex; flex-direction:column; gap:8px; box-shadow:0 2px 6px rgba(0,0,0,.02); }
  .client-card.remind-alert { border-color:var(--stamp); background:#fff5f5; }
  .client-head { display:flex; justify-content:space-between; align-items:center; font-weight:700; font-size:13.5px; }
  .client-sub { font-size:11.5px; color:var(--muted); line-height:1.5; background:#faf9f6; padding:6px 8px; border-radius:6px; }
  .consult-history { background:#fff; border:1px solid var(--line); border-radius:6px; padding:8px; font-size:11.5px; margin-top:4px; display:flex; flex-direction:column; gap:6px; max-height:140px; overflow-y:auto; }
  .consult-history-item { display:flex; justify-content:space-between; align-items:flex-start; border-bottom:1px dashed #f2efe9; padding-bottom:4px; }

  .modal-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,.45); z-index:999; align-items:center; justify-content:center; }
  .modal-box { background:#fff; border-radius:14px; padding:24px; width:min(580px,92vw); max-height:90vh; display:flex; flex-direction:column; gap:12px; overflow-y:auto; box-shadow:0 10px 30px rgba(0,0,0,.15); }
  
  .brief-btn{
    border:none;background:var(--blue);color:#fff;padding:6px 10px;border-radius:6px;font-size:11.5px;font-weight:700;cursor:pointer;width:100%;text-align:center;
  }
  .client-pagination { display:flex; justify-content:center; align-items:center; gap:6px; margin-top:8px; }
  .client-page-btn { border:1px solid var(--line); background:#fff; color:var(--ink); width:26px; height:26px; border-radius:4px; font-size:11px; font-weight:700; cursor:pointer; }
  .client-page-btn.active { background:var(--ink); color:#fff; border-color:var(--ink); }

  .pagination{display:flex; justify-content:center; align-items:center; gap:4px; margin-top:24px; flex-wrap:wrap;}
  .page-btn{
    border:1px solid var(--line); background:#fff; color:var(--ink); width:32px; height:32px; border-radius:6px; font-size:12px; font-weight:700; cursor:pointer; display:flex; align-items:center; justify-content:center;
  }
  .page-btn.active{ background:var(--ink); color:#fff; border-color:var(--ink); }
  .page-btn:disabled{ opacity:0.4; cursor:not-allowed; }
</style>
</head>
<body>

<div id="parseModal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.4);z-index:999;align-items:center;justify-content:center;">
  <div style="background:#fff;border-radius:12px;padding:20px;width:min(560px,90vw);max-height:85vh;display:flex;flex-direction:column;gap:10px;">
    <div style="font-size:15px;font-weight:700;color:var(--ink);">✨ 매물 텍스트 스마트 다중 자동입력</div>
    <textarea id="parseTextarea" placeholder="여기에 매물 양식 텍스트를 통째로 붙여넣으세요..." style="width:100%;min-height:180px;border:1px solid var(--line);border-radius:8px;padding:10px;font-size:12.5px;font-family:inherit;line-height:1.5;"></textarea>
    <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:4px;">
      <button id="parseCancelBtn" style="border:1px solid var(--line);background:#fff;padding:8px 14px;border-radius:8px;cursor:pointer;font-size:13px;">취소</button>
      <button id="parseConfirmBtn" style="border:none;background:var(--green);color:#fff;padding:8px 16px;border-radius:8px;cursor:pointer;font-size:13px;font-weight:700;">지능형 분석 후 등록</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="clientModal">
  <div class="modal-box">
    <div style="font-size:17px;font-weight:700;color:var(--ink);border-bottom:2px solid var(--gold);padding-bottom:8px;">👤 문의 손님 등록 및 리마인드 설정</div>
    <div class="needs-group">
      <label>관련 매물 정보</label>
      <input type="text" id="cListingTarget" readonly style="background:#f2efe9;font-weight:bold;">
    </div>
    <div class="needs-group">
      <label>손님 성함 / 연락처</label>
      <input type="text" id="cName" placeholder="예: 김철수 (010-1234-5678)">
    </div>
    <div class="needs-group">
      <label>플랫폼 출처</label>
      <input type="text" id="cPlatform" placeholder="예: 네이버부동산, 직방 등">
    </div>
    <div class="needs-group">
      <label>거래 종류 및 목적</label>
      <select id="cPurpose">
        <option value="매매(실입주)">매매 (실입주)</option>
        <option value="매매(투자)">매매 (투자)</option>
        <option value="전세">전세</option>
        <option value="월세">월세</option>
      </select>
    </div>
    <div class="needs-group">
      <label>희망 지역 / 예산 / 기타 요구사항</label>
      <textarea id="cDetail" style="min-height:50px;" placeholder="예: 강동구 일대, 예산 5억 안쪽 등"></textarea>
    </div>
    <div class="needs-group">
      <label>💬 초기 상담 내용 기록 (1차 상담)</label>
      <textarea id="cInitialConsult" style="min-height:50px;" placeholder="1차 상담 내용 요약"></textarea>
    </div>
    <div style="display:flex;gap:10px;justify-content:flex-end;margin-top:8px;">
      <button id="clientCancelBtn" style="border:1px solid var(--line);background:#fff;padding:10px 18px;border-radius:8px;cursor:pointer;font-size:13px;">취소</button>
      <button id="clientSaveBtn" style="border:none;background:var(--blue);color:#fff;padding:10px 20px;border-radius:8px;cursor:pointer;font-size:13px;font-weight:700;">손님 등록하기</button>
    </div>
  </div>
</div>

<div class="layout-container">
  <div class="main-column">
    <div class="memo-box">
      <div class="memo-tabs">
        <button class="memo-tab active" data-mtype="month">📅 월별 메모</button>
        <button class="memo-tab" data-mtype="week">📆 주간별 메모</button>
        <button class="memo-tab" data-mtype="day">📌 일별 메모</button>
        <button class="memo-tab" data-mtype="year">📈 연별 메모</button>
        <button class="memo-tab" data-mtype="contract">🤝 계약/일정</button>
      </div>
      <div class="memo-body">
        <div class="memo-input-wrap">
          <textarea id="memoInput" placeholder="이 기간에 직원들과 공유할 새로운 메모 또는 계약 일정을 작성하세요..."></textarea>
          <div class="memo-toolbar">
            <span class="memo-date-info" id="memoDatePreview">작성 시각: 실시간 자동 기록</span>
            <button class="memo-save-btn" id="memoAddBtn">메모 등록하기 ✍️</button>
          </div>
        </div>
        <div class="memo-list" id="memoListContainer"></div>
      </div>
    </div>

    <div class="head">
      <div>
        <h1>🏢 마중부동산 중개법인 통합 관리 시스템</h1>
        <div class="sub">매물별 문의 연동 및 통합 관리 시스템</div>
      </div>
      <div class="count" id="countBox"></div>
    </div>
    <div id="statusLine" style="font-size:13px;font-weight:700;color:#fff;background:#8a8477;padding:8px 12px;border-radius:8px;margin:0 0 14px;text-align:center;">서버 연결 중…</div>

    <div class="searchbar">
      <input type="text" id="searchInput" placeholder="주소, 금액, 비고, 연락처 등으로 검색">
    </div>

    <div class="filter-section" id="filterSection">
      <div class="filter-row">
        <div class="filter-row-label">상태필터</div>
        <div class="filter-row-content" id="statusFiltersContainer">
          <button class="filter-btn active" data-f="전체">전체</button>
          <button class="filter-btn" data-f="매매">매매</button>
          <button class="filter-btn" data-f="전세">전세</button>
          <button class="filter-btn" data-f="월세">월세</button>
          <button class="filter-btn" data-f="나감">나감</button>
          <button class="filter-btn" data-f="보류">보류</button>
          <button class="filter-btn" data-f="신축">신축</button>
          <button class="filter-btn" data-f="휴지통">🗑 휴지통</button>
        </div>
      </div>
      <div class="filter-row" id="districtRow" style="display:none;">
        <div class="filter-row-label">행정구역</div>
        <div class="filter-row-content" id="districtFilters"></div>
      </div>
    </div>

    <div class="action-toolbar">
      <div class="action-left">
        <input type="checkbox" id="selectAllTrashChk" style="display:none;width:18px;height:18px;cursor:pointer;" title="전체 선택">
        <label for="selectAllTrashChk" id="selectAllTrashLabel" style="display:none;font-size:12px;font-weight:bold;cursor:pointer;">전체선택</label>
        <button class="add-btn" id="multiDeleteBtn" style="background:var(--stamp);display:none;">🗑 선택 삭제</button>
        <button class="add-btn" id="trashPurgeAllBtn" style="background:var(--stamp);display:none;">🔥 휴지통 전체 강제삭제</button>
        <button class="add-btn" id="multiRestoreBtn" style="background:var(--green);display:none;">♻️ 선택 복구</button>
      </div>
      <div class="action-right">
        <button class="add-btn" id="parseOpenBtn" style="background:var(--gold);">✨ 텍스트 자동입력</button>
        <button class="add-btn" id="addBtn">+ 매물 추가</button>
      </div>
    </div>

    <div id="listContainerWrapper">
      <div id="directList"></div>
    </div>
    <div class="empty" id="emptyMsg" style="display:none;">등록된 매물이 없어요. [+ 매물 추가] 또는 [✨ 텍스트 자동입력]을 눌러보세요!</div>
    <div class="pagination" id="paginationBox"></div>
  </div>

  <div class="side-column">
    <div class="side-section">
      <h3>🤖 AI 자연어 니즈 추천</h3>
      <div class="needs-group">
        <label>손님 니즈를 자유롭게 적어보세요</label>
        <textarea id="aiNeedsInput" placeholder="예: 강동구일대, 투자금 2.5억 또는 매매 5억안쪽 등"></textarea>
      </div>
      <button class="brief-btn" id="aiSearchBtn" style="background:var(--gold);">🎯 자동 추천 매물 찾기</button>
      <div style="font-size:11px;color:var(--muted);margin:8px 0 4px;font-weight:700;" id="aiRecCountTitle">추천 결과 (0건)</div>
      <div class="rec-results" id="aiRecResultsContainer"></div>
      <button class="brief-btn" id="copyBriefBtn" style="background:var(--blue);margin-top:8px;">📋 답장 유도형 문자 생성 및 복사</button>
    </div>

    <div class="side-section" style="border-bottom:none;">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <h3 style="margin:0;border:none;padding:0;">👥 문의 손님 관리</h3>
        <button class="add-btn" id="openGeneralClientBtn" style="font-size:11px;padding:5px 10px;background:var(--blue);">+ 일반 손님 등록</button>
      </div>
      <div style="font-size:11px;color:var(--muted);margin-bottom:8px;">매물 상세 안에서 문의 고객을 등록하거나 일반 손님을 등록하세요.</div>
      <div id="clientListContainer" style="display:flex;flex-direction:column;gap:8px;"></div>
      <div class="client-pagination" id="clientPaginationBox"></div>
    </div>
  </div>
</div>

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

const FIELDS = [
  ["addr","주소"], ["price","금액"], ["mgmtFee","관리비/포함내역"], ["structure","구조/베란다여부"],
  ["floor","해당층/총층"], ["year","준공년도"], ["moveIn","입주가능일"], ["parking","주차"],
  ["option","옵션"], ["area","전용/공용"], ["landShare","지분면적"], ["officialPrice","공시가"],
  ["loan","융자금"], ["violation","위반여부"], ["owner","소유자정보"], ["tenant","세입자여부"],
  ["insurance","보증보험가능여부"], ["note","비고"], ["contact","연락처"], ["ref","참고"]
];

const CATEGORIES = [
  {key:"매매", type:"매매", sub:""},
  {key:"전세", type:"전세", sub:""},
  {key:"월세", type:"월세", sub:""},
  {key:"신축", type:"신축", sub:""},
  {key:"나감", keepType:true, sub:"나감"},
  {key:"보류", keepType:true, keepSub:true, hold:true}
];

const DONG_TO_GU = {
  "잠실동":"송파구", "신천동":"송파구", "풍납동":"송파구", "거여동":"송파구", "마천동":"송파구", "방이동":"송파구", "오금동":"송파구", "송파동":"송파구", "석촌동":"송파구", "삼전동":"송파구", "가락동":"송파구", "문정동":"송파구", "장지동":"송파구", "오륜동":"송파구", "위례동":"송파구",
  "사당동":"동작구", "상도동":"동작구", "노량진동":"동작구", "본동":"동작구", "흑석동":"동작구", "동작동":"동작구", "대방동":"동작구", "신대방동":"동작구",
  "역삼동":"강남구", "개포동":"강남구", "청담동":"강남구", "삼성동":"강남구", "대치동":"강남구", "신사동":"강남구", "논현동":"강남구", "압구정동":"강남구", "세곡동":"강남구", "자곡동":"강남구", "율현동":"강남구", "일원동":"강남구", "수서동":"강남구", "도곡동":"강남구",
  "서초동":"서초구", "잠원동":"서초구", "반포동":"서초구", "방배동":"서초구", "양재동":"서초구", "우면동":"서초구", "원지동":"서초구", "내곡동":"서초구", "신원동":"서초구",
  "관악구":"관악구", "봉천동":"관악구", "신림동":"관악구", "남현동":"관악구",
  "구로구":"구로구", "신도림동":"구로구", "구로동":"구로구", "가리봉동":"구로구", "고척동":"구로구", "개봉동":"구로구", "오류동":"구로구", "항동":"구로구", "궁동":"구로구", "천왕동":"구로구",
  "영등포구":"영등포구", "영등포동":"영등포구", "여의도동":"영등포구", "당산동":"영등포구", "도림동":"영등포구", "문래동":"영등포구", "양평동":"영등포구", "양화동":"영등포구", "신길동":"영등포구", "대림동":"영등포구",
  "마포구":"마포구", "아현동":"마포구", "공덕동":"마포구", "신공덕동":"마포구", "도화동":"마포구", "용강동":"마포구", "토정동":"마포구", "마포동":"마포구", "대흥동":"마포구", "염리동":"마포구", "노고산동":"마포구", "신수동":"마포구", "창전동":"마포구", "상수동":"마포구", "하중동":"마포구", "신정동":"마포구", "당인동":"마포구", "구수동":"마포구", "현석동":"마포구", "창후동":"마포구", "합정동":"마포구", "서교동":"마포구", "동교동":"마포구", "성산동":"마포구", "중동":"마포구", "상암동":"마포구",
  "용산구":"용산구", "후암동":"용산구", "용산동":"용산구", "남영동":"용산구", "동자동":"용산구", "갈월동":"용산구", "청파동":"용산구", "원효로":"용산구", "효창동":"용산구", "용문동":"용산구", "도원동":"용산구", "한강로":"용산구", "이촌동":"용산구", "이태원동":"용산구", "한남동":"용산구", "보광동":"용산구", "서빙고동":"용산구", "동빙고동":"용산구", "주성동":"용산구",
  "강동구":"강동구", "천호동":"강동구", "성내동":"강동구", "길동":"강동구", "명일동":"강동구", "상일동":"강동구", "암사동":"강동구", "둔촌동":"강동구"
};

function detectGu(addrText){
  if(!addrText) return "";
  for(const [dong, gu] of Object.entries(DONG_TO_GU)){
    if(addrText.includes(dong)) return gu;
  }
  const matchGu = addrText.match(/([가-힣]+구)/);
  return matchGu ? matchGu[1] : "";
}

function getFormattedNow(){
  const d = new Date();
  return `${d.getFullYear()}.${String(d.getMonth() + 1).padStart(2, '0')}.${String(d.getDate()).padStart(2, '0')} ${String(d.getHours()).padStart(2, '0')}:${String(d.getMinutes()).padStart(2, '0')}`;
}

let listings = [];
let clients = [];
let openId = null;
let statusFilter = "전체";
let districtFilter = "전체";
let searchQuery = "";
let currentPage = 1;
let clientCurrentPage = 1;
const ITEMS_PER_PAGE = 10;
const CLIENTS_PER_PAGE = 4;
let currentMemoType = "month";
let memosData = { month: [], week: [], day: [], year: [], contract: [] };
let activeClientListingId = null;

function setStatus(msg, ok){
  const el = document.getElementById("statusLine");
  if(!el) return;
  el.textContent = msg;
  el.style.background = ok === false ? "#8a3324" : (ok === true ? "#3c6e4a" : "#8a8477");
}

async function saveToFirebase(){
  try {
    await set(ref(db, 'majung_data/listings'), listings);
    await set(ref(db, 'majung_data/clients'), clients);
    setStatus("🟢 파이어베이스 실시간 동기화 완료", true);
  } catch(e) {
    setStatus("⚠ 저장 실패: " + e.message, false);
  }
}

function initRealtimeSync(){
  try {
    onValue(ref(db, 'majung_data/listings'), (snapshot) => {
      const data = snapshot.val();
      listings = (data && Array.isArray(data)) ? data : [];
      render();
    }, (error) => { setStatus("⚠ 연결 오류: " + error.message, false); });

    onValue(ref(db, 'majung_data/clients'), (snapshot) => {
      const data = snapshot.val();
      clients = (data && Array.isArray(data)) ? data : [];
      renderClients();
    });

    onValue(ref(db, 'majung_data/memos'), (snapshot) => {
      const val = snapshot.val();
      if(val && typeof val === "object"){
        ["month", "week", "day", "year", "contract"].forEach(type => {
          if(Array.isArray(val[type])) memosData[type] = val[type];
        });
      }
      renderMemos();
    });
  } catch(e) { setStatus("⚠ 연결 실패: " + e.message, false); }
}

function newListing(){
  return {
    id: "n"+Date.now()+Math.floor(Math.random()*1000),
    type:"월세", sub:"", movedTime:"", hold:false, isDeleted:false, deletedAt:null,
    createdAt: getFormattedNow(), addr:"", price:"", mgmtFee:"", structure:"",
    floor:"", year:"", moveIn:"", parking:"", option:"", area:"", landShare:"",
    officialPrice:"", loan:"", violation:"", owner:"", tenant:"", insurance:"",
    note:"", contact:"", ref:"", files:[], photos:[]
  };
}

function parseListingText(text){
  const item = newListing();
  const cleanFull = text.trim();
  if(cleanFull.includes("매매")) item.type = "매매";
  else if(cleanFull.includes("전세")) item.type = "전세";
  else if(cleanFull.includes("월세") || cleanFull.includes("보증금")) item.type = "월세";
  else if(cleanFull.includes("신축")) item.type = "신축";

  const lines = cleanFull.split("\n");
  lines.forEach((line, index) => {
    const clean = line.trim();
    if(!clean) return;
    if(index === 0 && !clean.includes(":")) { item.addr = clean; return; }
    let colonIdx = clean.indexOf(":");
    if(colonIdx === -1) colonIdx = clean.indexOf("：");
    if(colonIdx !== -1) {
      let key = clean.substring(0, colonIdx).trim();
      let val = clean.substring(colonIdx + 1).trim();
      if(key.includes("주소")) item.addr = val;
      else if(key.includes("가격") || key.includes("보증금") || key.includes("월세")) item.price = val;
      else if(key.includes("관리비")) item.mgmtFee = val;
      else if(key.includes("주차")) item.parking = val;
      else if(key.includes("옵션")) item.option = val;
      else if(key.includes("연락처")) item.contact = val;
    }
  });
  if(!item.addr) item.addr = lines[0] || "신규 매물";
  return item;
}

function render(){
  const directListEl = document.getElementById("directList");
  const emptyMsg = document.getElementById("emptyMsg");
  directListEl.innerHTML = "";

  const filtered = listings.filter(l => {
    if(statusFilter === "휴지통") return !!l.isDeleted;
    if(l.isDeleted) return false;
    if(statusFilter === "보류") return !!l.hold;
    if(statusFilter === "나감") return l.sub === "나감";
    if(statusFilter !== "전체" && l.type !== statusFilter) return false;
    return true;
  });

  document.getElementById("countBox").textContent = `총 ${listings.filter(l => !l.isDeleted).length}건`;
  emptyMsg.style.display = filtered.length ? "none" : "block";

  filtered.slice(0, ITEMS_PER_PAGE).forEach(item => {
    const isMoved = !item.isDeleted && item.sub === "나감";
    const isHeld = !item.isDeleted && !!item.hold;
    const isTrash = !!item.isDeleted;
    const inquiryCount = clients.filter(c => c.listingId === item.id).length;

    const card = document.createElement("div");
    card.className = "card" + (item.id === openId ? " open" : "");
    card.innerHTML = `
      <div class="card-head">
        <input type="checkbox" class="multi-chk" data-item-id="${item.id}">
        <div class="badge ${item.type}">${item.type}</div>
        ${inquiryCount > 0 ? `<div class="subbadge inquiry-count">📞 문의 ${inquiryCount}건</div>` : ""}
        <div class="title-block">
          <div class="addr">${escapeHtml(item.addr || "(주소 미입력)")}</div>
          <div class="price">${escapeHtml(item.price || "")}</div>
        </div>
        ${isTrash ? `<button class="card-del-btn" data-act="force-purge" style="background:var(--stamp);color:#fff;">🔥 강제삭제</button>` : `<button class="card-del-btn" data-act="card-del">삭제</button>`}
        <div class="caret">▶</div>
      </div>
      <div class="detail">
        <div style="background:#fbf8f0;border:1px solid var(--line);border-radius:8px;padding:10px;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center;">
          <div>
            <div style="font-size:12px;font-weight:700;color:var(--gold);">👤 이 매물 문의 고객 관리</div>
            <div style="font-size:11px;color:var(--muted);margin-top:2px;">연결된 문의: <b>${inquiryCount}건</b></div>
          </div>
          <button class="add-btn" data-act="open-client-modal" style="font-size:11.5px;padding:6px 12px;background:var(--blue);">+ 고객등록</button>
        </div>
      </div>
    `;

    card.querySelector(".card-head").addEventListener("click", (e)=>{
      if(e.target.closest('[data-act]')) return;
      openId = (openId === item.id) ? null : item.id;
      render();
    });

    card.querySelector('[data-act="open-client-modal"]')?.addEventListener("click", () => {
      activeClientListingId = item.id;
      document.getElementById("cListingTarget").value = `[${item.type}] ${item.addr} (${item.price})`;
      document.getElementById("clientModal").style.display = "flex";
    });

    card.querySelector('[data-act="card-del"]')?.addEventListener("click", async (e)=>{
      e.stopPropagation();
      if(confirm("휴지통으로 보낼까요?")) { item.isDeleted = true; item.deletedAt = Date.now(); await saveToFirebase(); render(); }
    });

    directListEl.appendChild(card);
  });
}

document.getElementById("openGeneralClientBtn").addEventListener("click", () => {
  activeClientListingId = "";
  document.getElementById("cListingTarget").value = "일반 고객 문의 (연동 매물 없음)";
  document.getElementById("clientModal").style.display = "flex";
});

document.getElementById("clientCancelBtn").addEventListener("click", () => {
  document.getElementById("clientModal").style.display = "none";
});

document.getElementById("clientSaveBtn").addEventListener("click", async () => {
  const name = document.getElementById("cName").value.trim();
  if(!name) { alert("성함을 입력해주세요."); return; }
  clients.unshift({
    id: "c" + Date.now(),
    listingId: activeClientListingId || "",
    name: name,
    purpose: document.getElementById("cPurpose").value,
    createdAt: getFormattedNow(),
    history: [{ id: "h"+Date.now(), round: 1, time: getFormattedNow(), text: document.getElementById("cInitialConsult").value.trim() || "문의 접수" }]
  });
  await saveToFirebase();
  document.getElementById("clientModal").style.display = "none";
  renderClients();
  render();
  alert("등록되었습니다.");
});

function renderClients(){
  const container = document.getElementById("clientListContainer");
  if(!container) return;
  container.innerHTML = clients.length ? "" : "<div style='text-align:center;color:var(--muted);font-size:12px;padding:10px 0;'>등록된 손님이 없습니다.</div>";
  clients.slice(0, 4).forEach(c => {
    const card = document.createElement("div");
    card.className = "client-card";
    card.innerHTML = `<div class="client-head"><span>👤 ${escapeHtml(c.name)}</span></div><div class="client-sub">목적: ${escapeHtml(c.purpose)}</div>`;
    container.appendChild(card);
  });
}

function escapeHtml(s){
  return String(s || "").replace(/[&<>"']/g, c=>({ "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;" }[c]));
}

function renderMemos(){
  const container = document.getElementById("memoListContainer");
  if(!container) return;
  container.innerHTML = (memosData[currentMemoType]||[]).map(m => `<div class="memo-item"><div class="memo-item-head">${m.time}</div><div class="memo-item-content">${escapeHtml(m.text)}</div></div>`).join("");
}

document.getElementById("addBtn").addEventListener("click", async ()=>{
  listings.unshift(newListing());
  openId = listings[0].id;
  await saveToFirebase(); render();
});

document.getElementById("parseOpenBtn").addEventListener("click", ()=> { document.getElementById("parseModal").style.display = "flex"; });
document.getElementById("parseCancelBtn").addEventListener("click", ()=> { document.getElementById("parseModal").style.display = "none"; });
document.getElementById("parseConfirmBtn").addEventListener("click", async ()=>{
  const raw = document.getElementById("parseTextarea").value.trim();
  if(!raw) return;
  listings.unshift(parseListingText(raw));
  await saveToFirebase();
  document.getElementById("parseModal").style.display = "none";
  render();
  alert("일괄 등록되었습니다.");
});

document.getElementById("statusFiltersContainer").addEventListener("click",(e)=>{
  const btn = e.target.closest(".filter-btn");
  if(!btn) return;
  statusFilter = btn.dataset.f;
  document.querySelectorAll("#statusFiltersContainer .filter-btn").forEach(b=>b.classList.remove("active"));
  btn.classList.add("active");
  render();
});

document.querySelectorAll(".memo-tab").forEach(tab => {
  tab.addEventListener("click", () => {
    document.querySelectorAll(".memo-tab").forEach(t => t.classList.remove("active"));
    tab.classList.add("active");
    currentMemoType = tab.dataset.mtype;
    renderMemos();
  });
});

document.getElementById("memoAddBtn").addEventListener("click", async () => {
  const textVal = document.getElementById("memoInput").value.trim();
  if(!textVal) return;
  if(!memosData[currentMemoType]) memosData[currentMemoType] = [];
  memosData[currentMemoType].push({ id: Date.now(), text: textVal, time: getFormattedNow() });
  document.getElementById("memoInput").value = "";
  await set(ref(db, 'majung_data/memos'), memosData);
  renderMemos();
});

initRealtimeSync();
</script>
</body>
</html>
