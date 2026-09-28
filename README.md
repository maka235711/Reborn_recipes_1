[index_mobile_fixed (1).html](https://github.com/user-attachments/files/32756141/index_mobile_fixed.1.html)
# Reborn_recipes_1<!DOCTYPE html>
<html lang="ja"><head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="color-scheme" content="light dark">
<title>錬金レシピ検索</title>
<style>
:root{
  --bg:#fff;--fg:#1f2328;--mut:#6b7280;--line:#e5e7eb;--card:#f6f7f9;
  --u:#b45309;--uu:#be123c;--god:#7c3aed;--acc:#2563eb;--hl:#eff4ff;
  --mark:#fde68a;--mark-fg:#1f2328;
  --star:#f59e0b;
  --glow:#d97706;--glow-bg:#fff7e0;--glow-soft:rgba(245,158,11,.45);
  --radius:10px;
  --tap:44px;
}
@media (prefers-color-scheme:dark){
  :root:not([data-theme="light"]){
    --bg:#0f1216;--fg:#e6e8eb;--mut:#9aa3ad;--line:#252b32;--card:#171c22;
    --u:#f2b45c;--uu:#ff7a93;--god:#b794f6;--acc:#3b82f6;--hl:#1a2230;
    --mark:#7c5e00;--mark-fg:#fff;
    --star:#fbbf24;
    --glow:#fbbf24;--glow-bg:#2a2410;--glow-soft:rgba(251,191,36,.4);
  }
}
*{box-sizing:border-box}
html,body{margin:0;padding:0}
body{
  background:var(--bg);color:var(--fg);
  font:15px/1.5 system-ui,-apple-system,"Hiragino Sans","Yu Gothic UI","Yu Gothic",Meiryo,sans-serif;
  -webkit-text-size-adjust:100%;-webkit-tap-highlight-color:transparent;
}
main{
  max-width:960px;margin:0 auto;
  padding:calc(12px + env(safe-area-inset-top,0px)) calc(12px + env(safe-area-inset-right,0px))
          calc(24px + env(safe-area-inset-bottom,0px)) calc(12px + env(safe-area-inset-left,0px));
}
h1{font-size:20px;margin:0 0 6px;font-weight:700;letter-spacing:.01em;display:flex;align-items:center;gap:6px}
h1 svg{width:22px;height:22px;color:var(--acc)}
.mut{color:var(--mut);font-size:12px;line-height:1.7}
p{margin:0}
 
#form{margin:16px 0 8px}
.grid{display:grid;gap:10px;grid-template-columns:1fr}
@media (min-width:600px){ .grid{grid-template-columns:1fr 1fr} }
@media (min-width:880px){ .grid{grid-template-columns:repeat(4,1fr)} }
.field{position:relative;display:flex;flex-direction:column;gap:4px;min-width:0}
.field>label{font-size:12px;color:var(--mut);padding-left:3px}
.field input,.field select,.field button{
  width:100%;font:inherit;font-size:16px;
  padding:10px 12px;
  border:1px solid var(--line);border-radius:var(--radius);
  background:var(--card);color:var(--fg);
  min-width:0;
}
.field input::placeholder{color:var(--mut);opacity:.75}
.field input:focus,.field select:focus,.field button:focus{
  outline:2px solid var(--acc);outline-offset:-1px;border-color:transparent;
}
.field button{cursor:pointer}
.field button:active{background:var(--hl)}
.field--btn{justify-content:flex-end}
.field.noresult input::placeholder{color:var(--uu);opacity:1}
.field.disabled input{
  opacity:.4;cursor:not-allowed;background:var(--line);
}
.field.disabled>label::after{
  content:" (使用不可)";
  color:var(--uu);font-size:11px;
}
.field .clear-inp{
  position:absolute;right:6px;bottom:6px;
  width:28px;height:28px;padding:0;border:0;background:transparent;
  color:var(--mut);cursor:pointer;display:none;
  align-items:center;justify-content:center;border-radius:50%;
}
.field.has-val .clear-inp{display:flex}
.field .clear-inp:hover{color:var(--uu);background:var(--hl)}
 
.list{
  position:fixed;top:0;left:0;width:200px;
  max-height:calc(5 * 44px);
  overflow-y:auto;-webkit-overflow-scrolling:touch;overscroll-behavior:contain;
  background:var(--bg);border:1px solid var(--line);
  border-radius:var(--radius);box-shadow:0 8px 24px rgba(0,0,0,.18);
  z-index:1000;display:none;padding:4px;
}
.list.on{display:block}
.list .item{
  padding:10px 12px;border-radius:6px;cursor:pointer;
  font-size:15px;line-height:1.4;
  white-space:nowrap;overflow:hidden;text-overflow:ellipsis;
  -webkit-user-select:none;user-select:none;
  display:flex;align-items:center;gap:8px;
}
.list .item .nm{flex:1;overflow:hidden;text-overflow:ellipsis}
.list .item .ct{font-size:11px;color:var(--mut);flex:0 0 auto}
.list .item.act,.list .item:hover{background:var(--hl)}
.list .grp{
  padding:8px 12px 4px;font-size:11px;color:var(--mut);
  border-top:1px solid var(--line);margin-top:4px;
  display:flex;align-items:center;gap:4px;
}
.list .grp:first-child{border-top:0;margin-top:0}
.list .empty{padding:10px 12px;color:var(--fg);font-size:13px;background:var(--card);border-radius:6px}
/* ===== 素材3候補の「レシピあり」表示 ===== */
.list .item.glow{
  font-weight:600;color:var(--fg);
  background:var(--glow-bg);
  box-shadow:inset 0 0 0 1px var(--glow),0 0 9px var(--glow-soft);
}
.list .item.glow .nm::before{content:"\2726";color:var(--glow);margin-right:6px}
.list .item.glow.act,.list .item.glow:hover{background:var(--hl)}
.list .item.dim{opacity:.5}
.list .grp.g-on{color:var(--glow);font-weight:600}
.cand-mode{margin:10px 0 0;display:flex;flex-wrap:wrap;align-items:center;gap:8px}
.cand-mode .lbl{font-size:12px;color:var(--mut)}
.seg{display:inline-flex;border:1px solid var(--line);border-radius:var(--radius);overflow:hidden}
.seg button{
  font:inherit;font-size:13px;padding:7px 12px;cursor:pointer;min-height:36px;
  background:var(--card);color:var(--fg);border:0;
}
.seg button+button{border-left:1px solid var(--line)}
.seg button.on{background:var(--acc);color:#fff}
 
/* ===== 所持素材 ===== */
.owned{margin:10px 0 0}
.owned-btn{
  font:inherit;font-size:13px;padding:8px 12px;cursor:pointer;
  background:transparent;color:var(--acc);border:1px dashed var(--line);
  border-radius:var(--radius);width:100%;text-align:left;
  display:flex;align-items:center;gap:6px;
}
.owned-btn:active{background:var(--hl)}
.owned-btn svg{width:14px;height:14px;transition:transform .15s}
.owned-btn.on svg{transform:rotate(180deg)}
.owned-body{
  margin-top:8px;display:flex;flex-direction:column;gap:8px;
  padding:10px;border:1px solid var(--line);border-radius:var(--radius);background:var(--bg);
}
.owned-body[hidden]{display:none}
.owned-tags-wrap{
  display:flex;flex-wrap:wrap;align-items:center;gap:6px;min-height:26px;
}
.owned-tags-wrap>.lbl{font-size:12px;color:var(--mut);flex:0 0 auto}
.tags{display:flex;flex-wrap:wrap;gap:6px}
.tag{
  display:inline-flex;align-items:center;gap:4px;
  padding:4px 8px;font-size:13px;
  background:var(--hl);color:var(--fg);border-radius:999px;
  border:1px solid var(--acc);
}
.tag button{
  width:auto;padding:0 4px;font-size:14px;line-height:1;
  background:transparent;border:0;cursor:pointer;color:var(--acc);
}
.tag button:hover{color:var(--uu)}
.owned-search{
  font:inherit;font-size:14px;padding:8px 10px;
  border:1px solid var(--line);border-radius:8px;
  background:var(--card);color:var(--fg);width:100%;
}
.owned-search:focus{outline:2px solid var(--acc);outline-offset:-1px;border-color:transparent}
.owned-grid{
  display:grid;grid-template-columns:repeat(2,1fr);gap:6px;
  max-height:280px;overflow-y:auto;-webkit-overflow-scrolling:touch;overscroll-behavior:contain;
  padding:8px;background:var(--card);border:1px solid var(--line);border-radius:var(--radius);
}
@media (min-width:600px){ .owned-grid{grid-template-columns:repeat(3,1fr)} }
@media (min-width:880px){ .owned-grid{grid-template-columns:repeat(4,1fr)} }
.owned-grid button{
  font:inherit;font-size:13px;padding:8px 10px;text-align:left;
  background:var(--bg);color:var(--fg);
  border:1px solid var(--line);border-radius:6px;cursor:pointer;
  white-space:nowrap;overflow:hidden;text-overflow:ellipsis;
  -webkit-user-select:none;user-select:none;
}
.owned-grid button:active{background:var(--hl)}
.owned-grid button.on{
  background:var(--hl);border-color:var(--acc);color:var(--acc);font-weight:600;
}
.owned-grid .none{
  grid-column:1/-1;text-align:center;padding:14px;color:var(--mut);font-size:12px;
}
 
/* ===== 最近見た ===== */
.recents{margin:10px 0 0}
.recents[hidden]{display:none}
.recents .hd{
  font-size:11px;color:var(--mut);margin-bottom:4px;
  display:flex;align-items:center;gap:4px;
}
.recents .hd svg{width:12px;height:12px}
.recents .list2{display:flex;flex-wrap:wrap;gap:6px}
.recents .chip{
  display:inline-flex;align-items:center;gap:4px;
  padding:5px 10px;font-size:12px;
  background:var(--card);color:var(--fg);
  border:1px solid var(--line);border-radius:999px;cursor:pointer;
  max-width:220px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;
}
.recents .chip:hover,.recents .chip:active{border-color:var(--acc);color:var(--acc)}
.recents .chip .x{
  padding:0 2px;color:var(--mut);border:0;background:transparent;
  cursor:pointer;font-size:12px;line-height:1;
}
 
.count{margin:10px 0 6px;font-size:13px;display:flex;flex-wrap:wrap;align-items:center;gap:12px}
.count b{font-size:18px;margin-right:2px}
.count .mut{font-size:12px}
.fav-only{display:inline-flex;align-items:center;gap:4px;font-size:12px;color:var(--mut);cursor:pointer}
.fav-only input{margin:0}
.sort-info{font-size:11px;color:var(--mut);display:inline-flex;align-items:center;gap:4px}
.sort-info svg{width:12px;height:12px}
 
.table-wrap{border-radius:var(--radius);overflow:auto;max-height:80vh}
table{border-collapse:collapse;width:100%;font-size:14px}
th,td{padding:10px;border-bottom:1px solid var(--line);text-align:left;white-space:nowrap}
/* ライト／ダーク両テーマで表の文字色と行背景を明示 */
tbody{color:var(--fg)}
tbody td{color:var(--fg);background-color:transparent}
tbody tr{background-color:var(--bg)}
tbody tr:nth-child(even){background-color:var(--card)}
thead th{
  font-size:11px;color:var(--mut);font-weight:600;
  background:var(--card);letter-spacing:.04em;
  position:sticky;top:0;z-index:5;
  cursor:pointer;user-select:none;
}
thead th[data-sort]:hover{color:var(--fg);background:var(--hl)}
thead th .arr{margin-left:3px;opacity:.3;font-size:10px}
thead th.on .arr{opacity:1;color:var(--acc)}
thead th.fav,th.fav{cursor:default}
td.q{text-align:right;font-variant-numeric:tabular-nums}
td.fav,th.fav{width:36px;text-align:center;padding:6px}
tbody tr:last-child td{border-bottom:0}
tbody tr.flash{animation:flash 1.4s ease-out}
@keyframes flash{
  0%{background:var(--hl)}
  100%{background:transparent}
}
.g1{color:var(--mut)}
.g2{color:var(--u);font-weight:600}
.g3{color:var(--uu);font-weight:700}
.g4{color:var(--god);font-weight:800}
.rn{display:none}
.star{
  font:inherit;font-size:17px;line-height:1;
  background:transparent;border:0;padding:4px;cursor:pointer;
  color:var(--line);display:inline-flex;align-items:center;justify-content:center;
  width:32px;height:32px;
}
.star svg{width:20px;height:20px}
.star.on{color:var(--star)}
td.empty,.empty{color:var(--mut);text-align:center;padding:28px 8px}
mark{background:var(--mark);color:var(--mark-fg);padding:0 1px;border-radius:2px}
.more{
  display:block;width:100%;margin:14px 0 0;
  padding:12px;font:inherit;font-size:15px;
  border:1px solid var(--line);border-radius:var(--radius);
  background:var(--card);color:var(--fg);cursor:pointer;
}
.more:active{background:var(--hl)}
 
#totop{
  position:fixed;right:16px;bottom:calc(16px + env(safe-area-inset-bottom,0px));
  width:46px;height:46px;border-radius:50%;
  border:1px solid var(--line);background:var(--bg);color:var(--fg);
  cursor:pointer;display:none;align-items:center;justify-content:center;
  box-shadow:0 4px 14px rgba(0,0,0,.18);z-index:100;
}
#totop.on{display:flex}
#totop svg{width:20px;height:20px}
 
/* 横画面の最適化 */
@media (orientation: landscape) and (max-height:520px){
  main{padding:8px}
  h1{font-size:16px;margin:0 0 4px}
  h1 svg{width:18px;height:18px}
  .mut{display:none}
  #form{margin:8px 0 4px}
  .grid{gap:6px}
  .field input,.field select,.field button{padding:8px 10px;font-size:14px}
  .count{margin:6px 0 4px;font-size:12px}
  .count b{font-size:15px}
  .list{max-height:calc(3 * 40px)}
  .list .item{padding:8px 10px;font-size:13px}
  .table-wrap{max-height:60vh}
  th,td{padding:6px 8px;font-size:12px}
}
 
@media (max-width:680px){
  table,tbody{display:block}
  thead{display:none}
  /* 1行目: 成果物 / 個数 / 評価 / ★   2行目: 素材1 + 素材2 + 素材3 */
  tbody tr{
    display:flex;flex-wrap:wrap;align-items:center;column-gap:8px;row-gap:0;
    margin-bottom:6px;padding:6px 6px 8px 12px;
    background:var(--card);border:1px solid var(--line);border-radius:var(--radius);
  }
  tbody td{display:block;padding:0;border:0;white-space:normal;text-align:left}
  tbody td::before{content:none}
  tbody td[data-l="成果物"]{order:1;flex:1 1 0;min-width:0;font-size:16px;line-height:1.35;overflow-wrap:anywhere}
  tbody td.q{order:2;flex:0 0 auto;font-size:15px;font-weight:600}
  tbody td.q::before{content:"\00d7";color:var(--mut);font-weight:400;font-size:12px;margin-right:1px}
  tbody td[data-l="評価"]{order:3;flex:0 0 auto;min-width:2.4em;text-align:center;font-size:12px;line-height:1.6;padding:0 6px;border:1px solid currentColor;border-radius:999px}
  tbody td[data-l="評価"]:empty{display:none}
  tbody td.fav{order:4;flex:0 0 auto;width:34px;height:34px;padding:0;display:flex;align-items:center;justify-content:center}
  tbody td.fav .star{width:34px;height:34px;padding:0}
  tbody td.fav .star svg{width:19px;height:19px}
  tbody tr::after{content:"";order:5;flex:0 0 100%;height:0}
  tbody td[data-l^="素材"]{order:6;flex:0 1 auto;min-width:0;font-size:13px;line-height:1.5;color:var(--mut);overflow-wrap:anywhere}
  tbody td[data-l="素材2"]::before,tbody td[data-l="素材3"]::before{content:"+";color:var(--mut);opacity:.6;margin-right:8px}
  .rs{display:none}
  .rn{display:inline}
  tbody tr.empty-row{background:transparent;border:0;padding:20px 0;margin:0;display:block}
  tbody tr.empty-row::after{content:none}
  tbody tr.empty-row td{display:block;text-align:center}
}
/* ===== ライト／ダーク両モードの表表示を安定させる ===== */
/* 素材名・成果物・個数は常にテーマの通常文字色で表示 */
tbody td:not([data-l="評価"]) {
  color: var(--fg) !important;
}
/* PC表示では各セルにテーマの行背景色を適用 */
@media (min-width: 681px) {
  tbody tr:not(.empty-row) > td {
    background-color: var(--bg) !important;
  }
  tbody tr:not(.empty-row):nth-child(even) > td {
    background-color: var(--card) !important;
  }
}
/* 検索結果がない場合も文字を読みやすく */
tbody tr.empty-row td {
  color: var(--fg);
  background-color: var(--card);
}

/* iPhoneなどのモバイル表示：カード背景と文字色をテーマに合わせる */
@media (max-width: 680px) {
  tbody tr:not(.empty-row) {
    background-color: var(--card) !important;
    color: var(--fg) !important;
  }
  tbody tr:not(.empty-row) > td {
    color: var(--fg) !important;
    background-color: transparent !important;
  }
  tbody tr:not(.empty-row) > td[data-l^="素材"] {
    color: var(--fg) !important;
  }
  tbody tr.empty-row,
  tbody tr.empty-row > td {
    color: var(--fg) !important;
    background-color: var(--card) !important;
  }
}

</style>
</head><body>
<main>
<h1>
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 3v6L4 18a2 2 0 0 0 1.8 3h12.4A2 2 0 0 0 20 18L15 9V3"/><path d="M9 3h6"/></svg>
  錬金レシピ検索
</h1>
<p class="mut">素材の並び順は関係なし・名前の一部だけでも検索できます (あいまい一致対応)。<br>
評価の目安: 神 20個以上、超有用 14〜19個、有用 10〜13個、不要 1〜9個。個数は投入数100のときの値。<br>
データ取得日: 2026-09-28 ／ ゲーム 1.0.4<br>
※素材を選ぶと、成果物の候補は「そのレシピがあるもの」だけに絞られます。<br>
※素材を1つ選ぶと、他の素材欄で「同じレシピに入りうる素材」が光ります。2つ選ぶと、残りの欄で「そのレシピが成立する素材」だけが光ります (下のボタンで「光るものだけ表示」に切替可)。</p>
 
<form id="form" onsubmit="return false" autocomplete="off">
  <div class="grid">
    <div class="field" id="f_m1">
      <label for="m1">素材1</label>
      <input id="m1" type="text" placeholder="例: スライムの核" autocomplete="off" autocapitalize="off" spellcheck="false">
      <button class="clear-inp" data-for="m1" type="button" aria-label="クリア">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
      </button>
      <div class="list" id="m1_list" role="listbox"></div>
    </div>
    <div class="field" id="f_m2">
      <label for="m2">素材2</label>
      <input id="m2" type="text" placeholder="素材名を入力" autocomplete="off" autocapitalize="off" spellcheck="false">
      <button class="clear-inp" data-for="m2" type="button" aria-label="クリア">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
      </button>
      <div class="list" id="m2_list" role="listbox"></div>
    </div>
    <div class="field" id="f_m3">
      <label for="m3">素材3</label>
      <input id="m3" type="text" placeholder="素材名を入力" autocomplete="off" autocapitalize="off" spellcheck="false">
      <button class="clear-inp" data-for="m3" type="button" aria-label="クリア">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
      </button>
      <div class="list" id="m3_list" role="listbox"></div>
    </div>
    <div class="field" id="f_out">
      <label for="out">成果物</label>
      <input id="out" type="text" placeholder="作りたい物" autocomplete="off" autocapitalize="off" spellcheck="false">
      <button class="clear-inp" data-for="out" type="button" aria-label="クリア">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
      </button>
      <div class="list" id="out_list" role="listbox"></div>
    </div>
    <div class="field">
      <label for="qn">個数</label>
      <select id="qn"></select>
    </div>
    <div class="field">
      <label for="g">評価</label>
      <select id="g"></select>
    </div>
    <div class="field field--btn">
      <label>&nbsp;</label>
      <button id="clr" type="button">条件をクリア</button>
    </div>
  </div>
 
  <div class="cand-mode" id="cand_mode">
    <span class="lbl">素材の候補 (素材を選択中):</span>
    <div class="seg" role="group" aria-label="素材候補の表示方法">
      <button type="button" data-cm="glow">全部表示・光らせる</button>
      <button type="button" data-cm="only">光るものだけ表示</button>
    </div>
  </div>

  <div class="owned">
    <button type="button" class="owned-btn" id="owned_toggle">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"/></svg>
      所持素材で絞り込む
    </button>
    <div class="owned-body" id="owned_body" hidden>
      <div class="owned-tags-wrap">
        <span class="lbl">選択中:</span>
        <div class="tags" id="owned_tags"></div>
      </div>
      <input id="owned_search" class="owned-search" type="text" placeholder="素材名で絞り込み" autocomplete="off" autocapitalize="off" spellcheck="false">
      <div class="owned-grid" id="owned_grid"></div>
    </div>
  </div>
 
  <div class="recents" id="recents" hidden>
    <div class="hd">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      最近見たレシピ
    </div>
    <div class="list2" id="recents_list"></div>
  </div>
</form>
 
<p class="count">
  <b id="cnt">0</b>件
  <label class="fav-only"><input type="checkbox" id="fav_only"> お気に入りのみ</label>
  <span class="sort-info" id="sort_info" hidden>
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18M6 12h12M10 18h4"/></svg>
    <span id="sort_label"></span>
  </span>
  <span class="mut" id="hint"></span>
</p>
 
<div class="table-wrap">
  <table>
    <thead><tr>
      <th class="fav" aria-label="お気に入り"></th>
      <th data-sort="0">素材1<span class="arr">▼</span></th>
      <th data-sort="1">素材2<span class="arr">▼</span></th>
      <th data-sort="2">素材3<span class="arr">▼</span></th>
      <th data-sort="3">成果物<span class="arr">▼</span></th>
      <th data-sort="4" class="on">個数<span class="arr">▼</span></th>
      <th data-sort="5">評価<span class="arr">▼</span></th>
    </tr></thead>
    <tbody id="tb"></tbody>
  </table>
</div>
 
<button id="more" type="button" class="more">もっと見る</button>
 
<button id="totop" type="button" aria-label="トップへ戻る">
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 19V5M5 12l7-7 7 7"/></svg>
</button>
</main>
 
<script>
const D={"n":["スライムの核","ナーガの鱗","リーフィアの葉","ローパーの蔓","光環の薄衣","光鐘核","凍土鋼","凍灯液","剛力の結晶","力の結晶","古木の枝","古根の芯","古樹心材","古鐘紐","古骨の紋章","回廊片","塩泡の真珠","塩泡灯","境界柄","外郭石","大地精霊石","天流竜角","天礁甲殻","天絹","山頂晶","岩獣の爪","岩鋼","嵐冠","川精霊の宝珠","巨兵の氷晶核","平原鳥の羽","忘却糸","戦士の誓印","捕食者の髄","星尾毛","星屑砂","星磁片","星羽","星脈鋼","星霊露","星骨","晶弓の弦","晶脈鱗","晶角","暁章","暁鏡板","月影糸","根走りの爪","森戦士の紋章","樹洞爪","機動の結晶","残光粉","氷殻片","氷礫針","池精霊の宝珠","沈都番兵の徽章","沼灯胞子","泥鰭膜","洞窟悪魔の角","海溝ウナギの滑皮","深淵の残火","深淵墨","深淵矢柄","深珊瑚核","深蒼クラゲの水膜","淵殻","淵毛","溶岩粘体の核","潮流真珠","潮爪の甲殻","潮羽粉","潮葉の尾","澄晶","火の粉鬼の角","火口守の装甲","灯粉","灰冠片","灰霊の魂片","炉核","炉槌柄","炉殻","炉糸","熱風翅","玄武岩心","王環片","王盾片","琥珀の種殻","琥珀背甲","環天鱗","環晶片","生体合金","生存の結晶","疾風の結晶","白晶甲","盾殻","石ゴーレムの核","砂丘甲殻","砂岩心","砂潜りの鱗","砂灯粉","砂牙","砂走りの牙","硝角","礁走りの牙","礫針","神秘の雫","祭壇残響","祭壇精霊片","秘奥の結晶","秘術の結晶","空洞粉","糸獣核","終鐘片","織糸","羽根草綿","翠緑の精髄","耐圧甲殻","胞子精の燐粉","草ゴーレムの苔","蒼晶核","蔓潜みの蔓核","蔓鞭繊維","薄光羽","虚樹の甲皮","虚海触腕","虚空牙","蜃気楼珠","裂羽","裂風羽","誘光の牙","赤晶滓","踏骨","軌道石","迷宮樹液","迷宮芽の種","迷宮鹿の角","遺跡妖精の鱗","遺跡鋼","遺跡騎士章","遺跡鬼火","重星鉱","野鼠の尾","鏡氷片","鏡衛板","門番の装甲","門鍵鋼","雪晶牙","雲環徽章","雲翼膜","霜冠布","霜毛","霜綿","霧氷の翼膜","霧氷羽","響銅片","風精霊の羽","黒潮竜鱗"],"r":[[0,1,141,154,1],[0,1,147,111,1],[0,1,148,133,1],[0,4,142,133,1],[0,5,75,133,1],[0,5,77,136,1],[0,6,25,98,1],[0,6,125,5,1],[0,8,48,73,2],[0,9,43,68,1],[0,10,30,59,1],[0,10,67,74,2],[0,10,81,133,1],[0,12,73,80,1],[0,12,98,9,1],[0,13,56,101,1],[0,14,20,43,1],[0,14,106,90,2],[0,16,45,145,1],[0,17,19,14,1],[0,17,40,62,1],[0,17,46,82,1],[0,17,127,32,1],[0,18,70,151,1],[0,18,119,41,1],[0,18,138,53,1],[0,20,37,134,1],[0,20,122,14,1],[0,21,33,137,1],[0,21,104,117,1],[0,23,58,8,2],[0,24,140,9,1],[0,24,150,117,1],[0,25,129,141,1],[0,26,124,80,1],[0,27,29,87,1],[0,27,53,89,1],[0,27,102,78,1],[0,28,92,141,2],[0,29,122,133,1],[0,31,43,45,1],[0,31,67,63,1],[0,31,124,149,1],[0,31,125,89,1],[0,32,50,1,4],[0,32,97,105,2],[0,33,45,44,1],[0,34,110,78,1],[0,37,81,81,1],[0,37,97,113,1],[0,38,54,17,1],[0,39,132,105,1],[0,40,114,96,1],[0,41,66,44,1],[0,41,121,83,1],[0,42,80,108,1],[0,42,151,74,1],[0,43,89,89,1],[0,44,48,9,1],[0,45,147,124,1],[0,48,52,81,1],[0,52,123,46,1],[0,55,57,152,1],[0,55,98,65,1],[0,56,73,156,1],[0,56,74,145,1],[0,56,81,5,1],[0,56,142,47,1],[0,57,122,87,1],[0,58,155,71,1],[0,61,90,97,1],[0,62,121,54,1],[0,62,156,0,1],[0,64,68,78,1],[0,64,82,75,1],[0,65,83,79,1],[0,65,117,48,1],[0,65,145,115,1],[0,67,106,47,1],[0,67,147,138,1],[0,70,105,77,1],[0,76,101,103,1],[0,76,110,129,1],[0,76,133,95,1],[0,76,151,31,1],[0,77,115,70,1],[0,77,131,146,1],[0,78,138,130,1],[0,79,89,67,1],[0,79,112,95,1],[0,80,92,122,1],[0,80,135,84,1],[0,82,85,29,1],[0,82,125,153,1],[0,83,102,33,1],[0,84,106,137,1],[0,85,102,84,1],[0,85,137,63,1],[0,86,116,4,1],[0,87,139,113,1],[0,90,131,119,1],[0,93,105,101,1],[0,93,111,20,1],[0,93,138,63,1],[0,94,149,75,1],[0,95,153,43,1],[0,96,152,35,1],[0,99,122,28,1],[0,99,142,129,1],[0,100,152,138,1],[0,104,115,54,1],[0,109,122,78,1],[0,111,130,149,1],[0,111,136,7,1],[0,113,120,30,1],[0,113,122,90,1],[0,114,131,130,1],[0,114,145,126,1],[0,115,156,125,1],[0,118,147,34,1],[0,120,156,148,1],[0,125,150,91,1],[0,127,137,117,1],[0,128,129,78,1],[0,138,145,47,1],[1,2,143,120,3],[1,3,57,33,1],[1,3,108,80,1],[1,4,92,18,2],[1,5,54,122,2],[1,5,60,53,3],[1,5,110,102,2],[1,5,142,9,2],[1,6,91,18,1],[1,7,109,135,2],[1,8,84,104,2],[1,8,134,3,2],[1,9,110,90,1],[1,9,150,102,1],[1,10,113,148,2],[1,11,27,12,3],[1,11,113,29,2],[1,11,139,11,3],[1,12,32,126,7],[1,12,117,77,6],[1,14,93,136,2],[1,16,36,129,4],[1,16,116,81,2],[1,18,41,61,2],[1,18,52,50,1],[1,18,129,20,2],[1,20,29,63,3],[1,20,94,33,1],[1,20,105,139,6],[1,21,142,79,2],[1,22,149,66,2],[1,23,26,79,2],[1,23,90,44,2],[1,23,125,103,2],[1,26,77,145,2],[1,27,37,156,3],[1,29,45,126,3],[1,29,142,71,3],[1,30,39,8,2],[1,31,81,63,2],[1,32,115,25,4],[1,33,51,32,1],[1,33,102,119,1],[1,35,36,58,4],[1,35,55,92,4],[1,36,106,15,2],[1,36,132,70,3],[1,36,141,77,1],[1,37,93,56,3],[1,38,75,104,2],[1,39,144,13,4],[1,39,146,127,2],[1,40,45,144,3],[1,40,121,118,2],[1,41,100,49,2],[1,42,143,76,2],[1,44,67,101,3],[1,44,104,128,2],[1,44,155,51,2],[1,45,98,60,2],[1,46,144,17,3],[1,49,134,70,3],[1,50,92,25,2],[1,51,147,89,2],[1,53,106,27,3],[1,54,55,95,3],[1,55,64,148,4],[1,55,95,52,3],[1,55,149,110,2],[1,56,119,43,3],[1,57,95,78,2],[1,58,71,125,2],[1,58,84,126,2],[1,58,98,136,1],[1,61,83,125,2],[1,62,137,121,2],[1,64,73,97,5],[1,64,117,66,2],[1,64,128,94,2],[1,67,87,33,1],[1,71,108,116,3],[1,72,74,24,2],[1,72,99,65,2],[1,72,103,13,2],[1,73,155,92,8],[1,75,105,146,2],[1,77,79,86,2],[1,77,114,89,3],[1,78,110,59,2],[1,79,117,42,2],[1,79,135,57,2],[1,80,153,46,2],[1,82,95,42,2],[1,83,135,106,8],[1,84,130,72,2],[1,86,139,124,3],[1,87,127,86,2],[1,89,148,130,2],[1,90,141,77,2],[1,91,106,10,3],[1,95,128,150,3],[1,101,153,149,3],[1,106,126,86,3],[1,108,138,9,5],[1,109,125,67,1],[1,111,153,95,2],[1,112,114,85,2],[1,119,145,127,2],[1,121,146,66,2],[1,124,140,87,3],[1,125,148,7,2],[1,129,142,53,3],[1,131,145,70,2],[1,139,156,128,4],[1,152,155,5,3],[2,4,74,67,10],[2,6,33,149,1],[2,7,53,55,8],[2,8,93,116,8],[2,9,23,99,1],[2,9,65,76,1],[2,9,107,114,2],[2,9,129,128,3],[2,10,27,138,10],[2,10,123,61,11],[2,11,64,13,7],[2,11,155,12,7],[2,12,45,112,6],[2,12,69,44,7],[2,14,63,55,11],[2,14,111,40,6],[2,15,48,122,5],[2,15,69,94,6],[2,16,146,150,8],[2,17,59,139,7],[2,17,123,63,7],[2,19,141,44,1],[2,21,57,59,7],[2,21,64,114,7],[2,21,91,137,1],[2,22,87,24,4],[2,22,127,45,6],[2,23,46,106,9],[2,24,77,42,5],[2,25,100,73,3],[2,27,135,60,6],[2,27,149,123,8],[2,28,37,15,2],[2,28,63,48,5],[2,28,155,12,10],[2,30,36,130,1],[2,31,85,19,6],[2,32,40,14,7],[2,33,135,0,3],[2,36,88,124,9],[2,37,55,17,7],[2,37,107,123,8],[2,38,106,99,6],[2,39,108,7,8],[2,39,146,124,9],[2,40,83,138,8],[2,41,85,47,6],[2,42,111,74,6],[2,45,124,32,6],[2,46,112,127,6],[2,47,70,63,7],[2,48,92,73,16],[2,49,148,66,6],[2,50,148,28,1],[2,51,144,29,6],[2,51,154,86,6],[2,52,86,129,7],[2,55,84,52,6],[2,56,73,18,6],[2,56,84,94,6],[2,56,146,110,6],[2,57,128,16,7],[2,58,135,50,3],[2,59,103,12,11],[2,61,108,125,6],[2,61,121,132,7],[2,63,80,15,6],[2,63,117,45,7],[2,64,101,143,8],[2,67,107,101,17],[2,68,73,103,5],[2,69,103,42,8],[2,70,98,150,6],[2,71,78,84,6],[2,71,99,74,6],[2,72,78,93,6],[2,74,153,137,10],[2,79,107,98,6],[2,79,124,64,6],[2,80,81,91,1],[2,80,112,135,5],[2,80,149,79,6],[2,81,103,19,6],[2,82,130,73,6],[2,83,135,126,15],[2,84,96,128,6],[2,84,112,89,6],[2,86,90,4,2],[2,87,140,19,6],[2,88,142,88,10],[2,89,129,81,6],[2,90,95,91,3],[2,90,103,152,2],[2,90,145,55,1],[2,91,96,126,2],[2,92,135,8,16],[2,95,109,79,1],[2,95,127,153,3],[2,96,99,93,6],[2,97,133,101,15],[2,97,139,130,6],[2,99,120,23,6],[2,99,124,104,6],[2,102,110,49,6],[2,105,110,46,1],[2,105,150,25,2],[2,107,122,61,9],[2,110,113,99,6],[2,111,149,44,6],[2,116,145,88,6],[2,128,131,137,6],[2,129,152,107,8],[2,131,147,106,6],[2,134,149,50,1],[2,135,145,151,5],[2,144,151,28,4],[2,146,147,98,6],[2,148,150,80,6],[3,4,6,53,1],[3,4,44,130,1],[3,4,72,20,1],[3,4,100,29,1],[3,5,147,134,1],[3,6,130,13,1],[3,7,110,65,1],[3,7,128,136,1],[3,8,48,115,2],[3,8,52,140,1],[3,8,80,88,1],[3,8,102,72,1],[3,9,16,73,1],[3,9,109,29,1],[3,9,121,153,1],[3,9,129,75,1],[3,9,139,26,3],[3,10,133,50,3],[3,12,15,114,1],[3,12,110,113,1],[3,12,118,104,1],[3,13,25,99,1],[3,14,75,139,1],[3,16,68,52,1],[3,18,140,106,1],[3,21,25,38,1],[3,21,28,48,1],[3,21,92,55,1],[3,21,93,21,1],[3,21,108,71,1],[3,22,96,1,1],[3,23,114,52,1],[3,23,151,34,1],[3,24,86,51,1],[3,24,100,17,1],[3,24,141,47,1],[3,24,145,19,1],[3,27,111,45,1],[3,28,46,116,1],[3,28,47,144,1],[3,31,136,43,1],[3,32,79,133,1],[3,33,144,147,1],[3,34,110,93,1],[3,35,138,89,1],[3,37,90,82,1],[3,37,138,32,1],[3,39,108,87,1],[3,40,115,112,1],[3,41,138,69,1],[3,43,103,40,1],[3,43,123,95,1],[3,44,154,77,1],[3,46,93,36,1],[3,46,94,14,1],[3,47,91,79,1],[3,48,60,77,2],[3,49,55,32,1],[3,49,74,26,1],[3,51,150,87,1],[3,51,153,106,1],[3,52,67,132,1],[3,52,80,82,1],[3,52,145,124,1],[3,52,156,138,1],[3,54,126,131,1],[3,55,95,19,1],[3,55,123,60,1],[3,55,130,87,1],[3,58,71,59,1],[3,59,99,8,1],[3,59,115,22,1],[3,60,145,16,1],[3,61,124,51,1],[3,62,81,8,1],[3,62,103,53,1],[3,62,141,58,1],[3,64,72,3,1],[3,65,141,132,1],[3,66,123,140,1],[3,67,117,122,1],[3,68,83,68,2],[3,68,98,14,1],[3,68,117,126,2],[3,70,82,43,1],[3,71,116,5,1],[3,72,109,35,1],[3,75,134,47,1],[3,78,141,95,1],[3,82,94,135,1],[3,83,117,25,2],[3,84,139,118,1],[3,85,95,91,1],[3,87,98,34,1],[3,87,156,66,1],[3,90,130,134,1],[3,91,128,104,1],[3,92,112,24,1],[3,92,128,52,1],[3,92,155,42,1],[3,95,134,32,4],[3,97,142,153,1],[3,98,114,4,1],[3,100,113,108,1],[3,101,130,137,1],[3,102,155,137,1],[3,104,149,123,1],[3,110,132,142,1],[3,113,117,15,1],[3,114,138,141,1],[3,139,152,76,1],[3,142,147,84,1],[3,144,150,56,1],[4,5,8,79,8],[4,5,145,61,13],[4,7,43,54,2],[4,8,64,64,14],[4,8,122,89,12],[4,9,79,108,1],[4,9,142,75,2],[4,10,105,104,1],[4,11,148,35,16],[4,13,42,109,2],[4,13,135,92,8],[4,14,71,21,10],[4,14,131,119,8],[4,15,89,84,14],[4,15,106,1,2],[4,16,93,82,12],[4,16,96,62,9],[4,16,99,147,12],[4,17,105,58,2],[4,17,122,156,16],[4,19,109,5,1],[4,19,119,3,1],[4,19,128,69,11],[4,20,64,112,5],[4,20,96,95,6],[4,20,137,95,6],[4,22,34,5,18],[4,23,38,38,9],[4,23,107,100,6],[4,25,144,5,4],[4,27,105,124,3],[4,28,146,94,2],[4,29,152,43,20],[4,31,112,4,14],[4,33,35,82,1],[4,34,48,29,7],[4,34,115,106,1],[4,35,88,70,17],[4,35,131,7,14],[4,38,144,50,1],[4,39,108,99,8],[4,39,127,28,2],[4,44,93,47,17],[4,46,49,149,17],[4,46,149,36,20],[4,49,77,35,9],[4,49,153,115,1],[4,50,56,67,1],[4,50,154,50,1],[4,51,139,9,1],[4,52,144,131,6],[4,54,94,127,1],[4,56,64,64,15],[4,57,71,69,13],[4,57,92,40,9],[4,59,66,98,13],[4,59,107,135,9],[4,60,117,45,4],[4,61,82,69,12],[4,61,126,57,10],[4,62,123,46,11],[4,62,144,9,1],[4,63,120,70,13],[4,65,69,134,5],[4,65,153,81,14],[4,67,136,50,1],[4,67,155,0,1],[4,69,134,135,9],[4,70,124,9,2],[4,71,145,99,14],[4,72,114,29,14],[4,72,116,99,13],[4,74,93,150,11],[4,74,104,106,7],[4,74,145,40,8],[4,77,82,129,8],[4,80,89,92,8],[4,80,109,150,1],[4,81,125,109,1],[4,84,99,103,11],[4,84,102,99,14],[4,85,138,155,6],[4,85,147,33,1],[4,90,91,52,1],[4,97,138,89,11],[4,97,146,104,9],[4,101,145,79,8],[4,104,127,118,1],[4,105,128,135,3],[4,113,140,152,14],[4,114,144,51,6],[4,118,136,125,1],[4,121,127,42,14],[4,123,130,49,11],[4,124,125,69,11],[4,126,139,148,12],[4,127,139,0,1],[4,135,145,9,1],[5,6,50,76,1],[5,7,74,1,3],[5,8,10,82,6],[5,8,44,14,10],[5,8,102,44,8],[5,9,75,20,2],[5,9,80,8,1],[5,9,104,24,1],[5,9,120,53,2],[5,11,95,131,3],[5,13,31,52,15],[5,14,44,151,11],[5,14,93,10,7],[5,16,103,103,14],[5,16,141,146,1],[5,19,92,47,8],[5,19,115,154,1],[5,20,22,54,2],[5,20,26,89,4],[5,22,68,126,3],[5,23,64,5,7],[5,23,89,97,7],[5,25,138,148,4],[5,26,66,118,1],[5,26,85,36,3],[5,27,35,2,7],[5,27,54,14,2],[5,29,120,32,6],[5,30,97,113,1],[5,30,145,122,1],[5,32,48,31,5],[5,32,114,102,5],[5,32,123,124,6],[5,33,86,135,1],[5,33,117,37,1],[5,34,139,30,2],[5,35,96,110,8],[5,36,58,63,4],[5,36,98,81,17],[5,37,134,63,6],[5,39,125,42,16],[5,42,121,129,16],[5,44,144,141,1],[5,46,124,144,7],[5,48,124,149,6],[5,49,50,15,1],[5,49,67,61,9],[5,49,101,136,2],[5,50,81,40,1],[5,50,122,129,1],[5,50,146,18,1],[5,51,61,102,13],[5,52,78,14,9],[5,53,80,87,19],[5,57,130,109,1],[5,58,100,93,3],[5,59,150,83,10],[5,61,87,130,14],[5,61,90,155,2],[5,62,119,22,14],[5,62,138,58,3],[5,62,146,139,7],[5,63,115,122,1],[5,64,131,61,13],[5,65,93,106,7],[5,65,96,84,8],[5,68,98,38,2],[5,73,138,117,9],[5,75,76,93,19],[5,76,112,145,19],[5,76,136,61,1],[5,78,100,127,20],[5,79,154,119,15],[5,80,98,19,19],[5,80,152,126,9],[5,80,155,79,6],[5,81,127,75,19],[5,84,106,117,7],[5,84,143,112,17],[5,86,151,133,6],[5,87,117,43,13],[5,89,154,1,3],[5,90,153,49,2],[5,91,134,97,1],[5,91,145,33,1],[5,91,149,142,1],[5,95,139,81,3],[5,96,115,5,1],[5,98,154,24,3],[5,102,105,42,1],[5,108,137,148,7],[5,108,139,53,9],[5,114,138,147,8],[5,115,153,92,1],[5,121,129,29,16],[5,123,135,93,6],[5,124,128,88,17],[5,126,143,96,11],[5,130,156,88,14],[5,134,147,8,6],[5,139,140,135,6],[5,139,141,40,1],[5,140,146,125,15],[5,142,156,38,17],[5,146,153,88,18],[5,149,153,46,19],[5,149,154,1,3],[6,7,40,117,14],[6,8,78,46,8],[6,11,39,57,17],[6,12,133,67,8],[6,13,110,49,15],[6,14,58,24,5],[6,14,66,36,9],[6,14,89,143,12],[6,15,114,134,5],[6,16,58,34,4],[6,19,29,42,15],[6,19,107,153,7],[6,20,74,54,2],[6,20,155,145,5],[6,21,122,148,22],[6,21,156,93,18],[6,22,57,92,9],[6,22,130,144,6],[6,23,86,22,7],[6,24,54,48,2],[6,24,57,55,4],[6,26,45,144,4],[6,29,34,69,15],[6,29,60,63,5],[6,29,95,6,5],[6,30,49,148,2],[6,30,138,25,2],[6,32,67,123,8],[6,33,50,64,1],[6,34,62,15,15],[6,35,40,146,20],[6,35,48,85,5],[6,35,114,147,17],[6,36,125,74,8],[6,36,137,66,6],[6,37,89,2,8],[6,37,95,14,4],[6,38,60,98,3],[6,39,85,108,8],[6,39,130,16,12],[6,40,87,146,18],[6,41,77,41,11],[6,41,82,90,1],[6,42,76,41,16],[6,42,124,50,1],[6,45,70,128,17],[6,47,124,68,3],[6,47,125,79,15],[6,48,154,19,5],[6,54,82,61,1],[6,55,110,67,8],[6,56,144,95,4],[6,56,149,58,4],[6,56,152,22,16],[6,58,76,19,3],[6,62,86,73,8],[6,62,150,120,11],[6,64,103,88,19],[6,65,109,150,1],[6,71,142,77,9],[6,72,131,54,1],[6,74,92,120,13],[6,76,103,85,11],[6,78,81,152,16],[6,78,109,27,1],[6,80,146,139,7],[6,81,145,154,15],[6,82,148,77,8],[6,83,95,111,3],[6,85,144,145,6],[6,86,117,138,8],[6,87,111,75,15],[6,87,131,106,7],[6,88,92,25,5],[6,97,139,109,2],[6,98,149,95,3],[6,99,111,47,15],[6,100,135,34,5],[6,101,113,10,6],[6,102,143,85,15],[6,103,149,31,11],[6,105,146,26,2],[6,111,122,22,14],[6,115,139,57,1],[6,117,140,48,8],[6,117,141,144,1],[6,119,132,122,19],[6,129,147,136,2],[6,131,138,120,7],[6,131,156,126,8],[6,134,140,143,7],[6,135,141,85,1],[6,141,147,132,1],[6,141,156,69,1],[6,153,156,62,13],[7,8,13,114,9],[7,9,142,110,1],[7,9,149,63,2],[7,10,16,69,8],[7,10,136,99,1],[7,11,20,12,6],[7,11,76,121,19],[7,11,79,60,3],[7,12,36,25,5],[7,13,45,71,18],[7,15,27,147,14],[7,15,91,39,1],[7,15,124,118,1],[7,16,71,104,12],[7,16,131,59,11],[7,17,99,18,17],[7,18,135,44,5],[7,19,101,67,8],[7,20,152,55,7],[7,21,117,118,2],[7,23,98,8,6],[7,24,37,53,4],[7,24,54,26,2],[7,24,145,142,3],[7,25,46,111,3],[7,26,60,70,4],[7,27,91,16,1],[7,27,147,64,18],[7,28,152,82,2],[7,29,30,38,2],[7,29,76,106,7],[7,29,95,56,4],[7,30,34,149,2],[7,30,132,37,2],[7,31,54,35,1],[7,31,92,152,8],[7,32,95,134,5],[7,32,132,41,7],[7,33,35,28,1],[7,34,91,142,1],[7,35,78,53,17],[7,37,144,36,8],[7,39,133,69,7],[7,41,128,74,11],[7,43,67,138,10],[7,43,83,25,5],[7,43,97,50,1],[7,44,74,109,2],[7,45,155,5,7],[7,46,143,34,21],[7,47,146,28,3],[7,48,79,90,1],[7,51,134,97,5],[7,51,144,66,6],[7,52,99,131,17],[7,52,129,42,18],[7,52,152,71,21],[7,54,103,77,2],[7,55,149,29,18],[7,56,63,71,15],[7,57,70,104,19],[7,57,134,139,6],[7,59,75,58,4],[7,60,86,106,4],[7,62,92,91,1],[7,62,132,137,6],[7,64,74,53,11],[7,65,120,96,9],[7,69,99,27,12],[7,70,127,139,7],[7,70,143,141,1],[7,70,145,113,17],[7,74,150,69,11],[7,75,141,20,1],[7,75,145,134,5],[7,76,98,109,1],[7,77,152,44,10],[7,77,153,155,8],[7,78,113,90,1],[7,79,84,89,16],[7,79,145,135,5],[7,82,145,100,17],[7,86,123,41,13],[7,87,101,152,10],[7,87,137,100,6],[7,89,105,60,2],[7,92,111,10,6],[7,92,124,38,11],[7,94,134,81,5],[7,96,134,134,7],[7,97,124,153,12],[7,100,125,123,10],[7,103,140,82,12],[7,105,127,132,1],[7,107,137,80,6],[7,109,124,14,2],[7,114,135,4,6],[7,122,143,17,19],[7,127,142,137,6],[7,130,153,149,16],[7,135,141,58,1],[7,136,137,150,2],[7,137,142,91,1],[8,9,24,121,2],[8,9,50,144,2],[8,9,117,85,1],[8,10,65,5,6],[8,10,105,35,2],[8,11,155,2,7],[8,12,64,61,11],[8,13,104,7,8],[8,14,144,101,17],[8,15,31,127,8],[8,15,93,87,8],[8,15,144,15,6],[8,15,151,43,8],[8,17,22,78,8],[8,17,123,44,9],[8,17,142,15,8],[8,18,96,111,8],[8,19,67,142,8],[8,20,67,38,8],[8,20,146,43,7],[8,21,50,148,1],[8,21,91,47,1],[8,21,103,131,8],[8,21,105,46,2],[8,23,25,119,6],[8,23,111,127,6],[8,24,89,36,5],[8,27,84,44,8],[8,27,131,103,8],[8,27,152,143,11],[8,28,29,45,3],[8,28,67,14,7],[8,28,135,46,4],[8,29,85,54,1],[8,31,73,153,8],[8,32,43,141,1],[8,32,64,153,8],[8,33,79,91,1],[8,34,80,127,8],[8,35,44,51,8],[8,36,132,80,8],[8,38,109,99,1],[8,39,153,138,11],[8,40,154,72,8],[8,42,146,96,11],[8,43,81,15,8],[8,43,117,0,1],[8,45,59,120,10],[8,45,87,24,4],[8,45,144,55,7],[8,49,92,11,9],[8,50,107,118,2],[8,53,54,23,2],[8,53,75,103,10],[8,53,148,21,11],[8,54,69,27,3],[8,59,90,9,3],[8,59,101,155,11],[8,60,88,105,3],[8,61,71,85,8],[8,61,114,59,9],[8,62,81,140,8],[8,63,91,147,1],[8,63,119,117,14],[8,64,101,106,13],[8,64,146,141,1],[8,65,87,155,6],[8,66,101,121,8],[8,67,146,39,12],[8,69,90,24,3],[8,73,110,87,8],[8,75,142,154,10],[8,77,86,87,9],[8,77,107,38,11],[8,77,153,12,10],[8,81,156,89,8],[8,83,126,78,8],[8,84,104,40,8],[8,84,127,4,8],[8,87,130,36,8],[8,88,104,127,8],[8,88,116,105,3],[8,88,154,143,11],[8,90,115,64,1],[8,90,151,139,2],[8,91,128,65,1],[8,92,116,135,10],[8,94,144,109,1],[8,96,123,148,14],[8,96,154,92,13],[8,99,133,23,5],[8,100,122,55,8],[8,101,112,97,8],[8,102,145,4,8],[8,105,146,68,2],[8,115,145,104,1],[8,116,125,32,5],[8,117,124,42,11],[8,121,154,68,3],[8,122,153,13,12],[8,125,145,80,8],[8,138,140,19,7],[8,144,156,75,8],[9,10,55,138,3],[9,10,83,106,5],[9,11,83,148,2],[9,12,17,56,2],[9,12,137,103,3],[9,13,39,83,2],[9,13,88,59,2],[9,14,77,54,5],[9,14,108,63,3],[9,15,90,143,1],[9,16,139,39,2],[9,16,147,103,3],[9,17,107,48,2],[9,18,25,81,1],[9,18,117,42,1],[9,19,66,96,1],[9,19,110,44,1],[9,20,56,137,2],[9,21,55,144,3],[9,21,61,65,1],[9,21,122,51,1],[9,22,130,56,1],[9,24,46,152,2],[9,24,145,82,1],[9,25,47,117,2],[9,28,45,103,2],[9,28,94,39,1],[9,28,127,1,1],[9,30,111,128,1],[9,30,132,28,2],[9,32,51,15,1],[9,32,123,81,1],[9,34,142,3,1],[9,35,124,126,2],[9,36,40,8,2],[9,37,90,120,2],[9,37,106,128,2],[9,40,65,2,1],[9,41,115,54,1],[9,41,139,116,2],[9,41,143,120,2],[9,42,58,3,1],[9,43,97,141,1],[9,43,108,94,1],[9,44,140,127,1],[9,45,141,35,1],[9,47,151,121,2],[9,48,144,156,3],[9,49,66,31,1],[9,50,68,68,8],[9,52,68,61,2],[9,52,90,12,2],[9,52,110,78,1],[9,53,57,132,2],[9,53,120,143,2],[9,54,67,23,5],[9,56,142,94,1],[9,58,63,88,3],[9,58,93,126,2],[9,59,121,7,2],[9,59,124,80,1],[9,61,74,133,3],[9,61,123,143,2],[9,62,137,54,1],[9,64,136,23,3],[9,68,98,15,1],[9,70,127,106,1],[9,71,100,82,1],[9,72,146,84,1],[9,73,97,8,5],[9,73,123,18,1],[9,74,107,21,3],[9,74,133,96,5],[9,75,85,21,1],[9,75,133,133,2],[9,76,105,153,1],[9,77,131,99,1],[9,78,90,48,1],[9,78,151,79,1],[9,79,150,81,1],[9,82,152,21,1],[9,83,120,14,4],[9,87,147,144,2],[9,88,153,13,2],[9,89,131,83,1],[9,89,146,129,2],[9,91,110,69,1],[9,92,101,31,1],[9,94,110,20,1],[9,94,127,45,1],[9,95,149,0,1],[9,98,150,109,1],[9,99,138,113,1],[9,100,120,32,1],[9,100,128,143,1],[9,101,108,100,1],[9,105,146,24,2],[9,113,153,91,1],[9,124,149,137,2],[10,11,31,9,1],[10,11,59,146,7],[10,11,81,112,6],[10,11,95,83,4],[10,13,19,117,6],[10,13,126,57,7],[10,14,138,100,6],[10,15,59,49,6],[10,15,134,69,5],[10,16,31,109,1],[10,16,139,8,13],[10,16,153,91,1],[10,17,36,90,2],[10,17,103,57,7],[10,18,40,26,3],[10,19,21,150,6],[10,19,67,91,1],[10,21,71,156,7],[10,21,86,84,6],[10,21,98,80,6],[10,21,154,20,8],[10,22,67,155,10],[10,22,120,123,10],[10,23,106,9,6],[10,23,156,26,6],[10,25,37,99,3],[10,25,70,65,3],[10,25,100,40,3],[10,26,51,131,3],[10,26,137,69,7],[10,27,39,66,6],[10,28,107,34,3],[10,30,31,150,1],[10,30,152,144,2],[10,31,126,12,6],[10,34,68,118,2],[10,38,39,27,9],[10,38,84,82,6],[10,39,82,22,6],[10,40,94,128,6],[10,40,107,114,7],[10,43,76,146,6],[10,43,103,153,8],[10,43,141,120,1],[10,45,80,23,6],[10,46,53,70,7],[10,47,138,143,7],[10,48,155,41,7],[10,51,114,24,3],[10,54,100,113,1],[10,56,87,9,2],[10,57,104,117,6],[10,59,121,23,7],[10,63,115,30,1],[10,65,137,108,6],[10,67,95,51,3],[10,68,70,40,3],[10,68,89,53,3],[10,68,142,114,3],[10,74,90,156,3],[10,75,153,121,7],[10,83,129,19,6],[10,84,87,151,6],[10,85,104,35,6],[10,86,148,31,6],[10,87,129,147,7],[10,97,119,121,7],[10,98,107,121,6],[10,99,143,153,6],[10,100,108,149,6],[10,103,132,44,7],[10,104,136,65,1],[10,106,139,44,7],[10,107,145,152,6],[10,108,127,34,6],[10,112,124,19,6],[10,114,143,75,7],[10,117,123,146,10],[10,129,137,23,11],[10,129,140,136,2],[10,142,150,80,6],[11,12,50,113,1],[11,12,64,127,6],[11,12,94,112,6],[11,13,66,81,15],[11,14,80,87,9],[11,15,142,15,15],[11,16,43,147,13],[11,18,67,148,8],[11,19,50,7,1],[11,20,84,54,1],[11,21,56,26,4],[11,22,62,152,14],[11,22,138,122,8],[11,23,122,156,7],[11,23,130,151,6],[11,24,86,92,4],[11,26,126,101,4],[11,26,131,125,3],[11,27,146,4,16],[11,27,152,128,16],[11,29,133,70,6],[11,32,125,106,5],[11,32,147,57,6],[11,32,156,64,6],[11,33,65,65,1],[11,33,80,60,1],[11,34,40,99,20],[11,34,64,88,15],[11,34,110,134,5],[11,36,123,98,11],[11,37,43,55,15],[11,37,44,8,9],[11,37,98,122,17],[11,38,71,84,16],[11,39,81,20,5],[11,40,65,44,19],[11,42,81,90,1],[11,42,153,82,16],[11,43,65,72,17],[11,45,126,59,10],[11,46,136,125,1],[11,47,130,59,14],[11,47,139,58,4],[11,48,61,25,4],[11,48,110,121,5],[11,50,109,40,1],[11,51,79,154,15],[11,53,64,13,15],[11,54,101,24,2],[11,55,88,20,6],[11,56,71,152,20],[11,57,109,135,2],[11,62,111,34,18],[11,63,81,97,9],[11,63,155,124,7],[11,66,133,32,5],[11,70,111,23,6],[11,72,103,123,10],[11,72,130,93,17],[11,73,95,60,4],[11,73,115,123,1],[11,76,90,102,1],[11,76,109,102,1],[11,76,137,14,6],[11,77,110,2,6],[11,77,147,112,8],[11,78,84,83,8],[11,80,131,8,8],[11,80,156,81,14],[11,81,112,107,7],[11,81,148,145,14],[11,85,151,78,15],[11,86,120,6,12],[11,87,148,7,16],[11,89,128,5,16],[11,90,137,45,2],[11,93,96,99,9],[11,93,125,143,17],[11,93,151,22,16],[11,94,112,137,6],[11,95,105,31,1],[11,95,115,61,1],[11,96,136,24,2],[11,96,149,121,10],[11,99,110,30,1],[11,100,101,127,9],[11,101,104,2,6],[11,101,146,87,10],[11,103,109,72,1],[11,104,151,135,5],[11,106,124,37,8],[11,106,130,61,7],[11,111,127,77,8],[11,111,138,63,7],[11,115,132,145,1],[11,120,129,65,11],[11,120,137,81,6],[11,121,148,90,2],[11,130,146,67,8],[12,13,89,136,2],[12,13,103,126,10],[12,14,130,135,5],[12,15,142,72,6],[12,15,148,96,6],[12,18,65,9,1],[12,18,109,6,1],[12,19,143,19,6],[12,20,109,87,2],[12,23,140,145,6],[12,23,153,11,7],[12,24,105,59,3],[12,25,73,37,4],[12,26,123,123,8],[12,27,50,111,1],[12,27,87,126,7],[12,28,40,139,3],[12,28,102,23,2],[12,29,152,103,8],[12,30,51,53,1],[12,31,125,87,6],[12,33,67,80,1],[12,34,128,122,8],[12,36,51,37,6],[12,36,151,5,7],[12,37,149,9,2],[12,38,98,56,6],[12,39,64,43,8],[12,40,61,97,8],[12,40,128,87,7],[12,41,68,63,3],[12,42,88,100,6],[12,47,65,56,6],[12,47,98,94,6],[12,47,100,97,6],[12,48,57,29,6],[12,48,69,108,11],[12,48,94,155,5],[12,49,146,72,6],[12,50,72,128,1],[12,50,107,32,3],[12,50,149,138,1],[12,51,59,67,6],[12,51,134,50,1],[12,56,76,140,6],[12,57,90,47,2],[12,57,122,133,6],[12,57,134,34,6],[12,57,138,95,4],[12,58,62,139,3],[12,58,65,85,3],[12,62,69,144,6],[12,62,113,6,6],[12,63,130,9,1],[12,65,74,41,6],[12,65,120,128,6],[12,66,80,57,6],[12,66,81,90,1],[12,66,140,37,6],[12,66,141,92,1],[12,71,140,49,7],[12,71,148,124,7],[12,77,95,141,2],[12,79,83,66,6],[12,80,81,105,1],[12,80,115,137,1],[12,84,120,76,6],[12,84,132,129,6],[12,87,154,61,7],[12,90,102,113,1],[12,94,96,124,6],[12,96,105,116,3],[12,96,122,141,1],[12,97,134,140,8],[12,97,152,150,8],[12,97,156,136,3],[12,98,112,154,6],[12,98,124,39,6],[12,99,128,154,6],[12,103,151,120,10],[12,112,140,11,6],[12,113,115,120,1],[12,114,116,155,7],[12,115,142,120,1],[12,126,129,28,5],[12,133,140,111,5],[12,140,143,62,6],[13,14,116,73,13],[13,17,21,66,14],[13,17,76,49,16],[13,17,79,56,16],[13,17,112,97,8],[13,17,129,18,13],[13,18,139,75,7],[13,22,49,101,10],[13,22,61,87,16],[13,23,131,42,6],[13,25,71,154,4],[13,26,149,153,5],[13,27,77,84,8],[13,28,50,139,1],[13,30,123,140,2],[13,31,105,154,1],[13,31,110,112,15],[13,33,101,51,1],[13,33,155,90,1],[13,37,145,99,15],[13,41,73,77,11],[13,41,111,151,15],[13,42,45,49,18],[13,42,120,14,12],[13,43,97,31,8],[13,43,135,40,7],[13,44,67,43,10],[13,44,121,84,15],[13,44,133,72,5],[13,45,116,142,17],[13,45,146,36,19],[13,46,133,55,8],[13,46,135,24,5],[13,48,91,86,1],[13,48,123,48,8],[13,49,134,77,6],[13,50,140,8,1],[13,52,85,110,15],[13,52,109,44,2],[13,53,63,154,18],[13,53,92,110,8],[13,55,69,118,2],[13,56,136,121,2],[13,56,144,144,7],[13,58,60,36,5],[13,58,134,29,5],[13,59,70,17,16],[13,59,77,123,13],[13,60,114,112,3],[13,63,81,53,14],[13,65,86,36,15],[13,65,124,55,13],[13,65,125,29,15],[13,66,153,64,13],[13,68,86,51,2],[13,68,93,59,3],[13,70,72,151,15],[13,71,115,42,1],[13,71,143,98,16],[13,75,141,105,1],[13,77,98,130,8],[13,79,98,90,1],[13,80,119,35,15],[13,80,123,67,8],[13,82,137,80,6],[13,85,120,38,10],[13,86,123,90,2],[13,89,97,127,9],[13,90,108,57,2],[13,90,136,97,2],[13,98,105,77,1],[13,99,147,53,15],[13,102,114,37,16],[13,106,110,115,1],[13,106,135,138,8],[13,108,155,95,5],[13,110,145,97,8],[13,111,153,72,15],[13,112,123,136,1],[13,116,124,9,2],[13,121,132,45,18],[13,122,148,70,17],[13,133,146,124,8],[13,139,150,57,8],[13,141,153,101,1],[13,154,156,126,14],[14,15,98,5,8],[14,15,147,4,8],[14,17,27,65,9],[14,17,86,89,10],[14,17,121,91,1],[14,17,155,88,7],[14,18,37,148,8],[14,19,122,106,7],[14,19,131,87,8],[14,20,71,30,2],[14,20,101,151,8],[14,21,56,132,10],[14,21,147,125,8],[14,21,149,136,2],[14,22,92,85,8],[14,23,97,129,11],[14,23,130,71,6],[14,24,45,136,2],[14,24,140,8,5],[14,25,100,133,3],[14,25,138,68,7],[14,27,90,136,3],[14,27,123,153,14],[14,28,36,144,4],[14,30,65,110,1],[14,31,119,109,1],[14,32,45,149,6],[14,32,125,66,5],[14,33,71,144,1],[14,33,87,35,1],[14,36,79,114,9],[14,36,151,120,13],[14,37,48,73,7],[14,37,52,20,7],[14,37,63,82,9],[14,37,119,123,11],[14,38,70,19,8],[14,40,46,42,11],[14,40,79,90,1],[14,43,54,76,1],[14,43,57,103,10],[14,43,62,143,9],[14,43,111,46,8],[14,43,156,136,2],[14,45,121,59,10],[14,45,122,153,11],[14,46,79,122,9],[14,46,154,155,9],[14,47,70,52,10],[14,47,104,79,9],[14,49,130,27,9],[14,51,116,39,8],[14,51,147,103,8],[14,52,149,33,1],[14,56,61,117,10],[14,56,63,16,10],[14,59,134,97,10],[14,60,77,145,3],[14,61,69,35,13],[14,61,96,139,13],[14,61,139,80,7],[14,62,67,94,8],[14,63,120,143,12],[14,64,72,37,8],[14,66,84,14,8],[14,66,136,107,1],[14,67,145,33,1],[14,68,95,70,3],[14,69,92,130,8],[14,69,106,126,15],[14,70,145,74,8],[14,71,136,148,2],[14,73,107,7,10],[14,73,130,14,8],[14,75,100,49,9],[14,78,143,55,9],[14,80,134,101,5],[14,80,152,127,9],[14,81,127,18,8],[14,81,130,33,1],[14,82,104,17,9],[14,82,142,98,9],[14,83,84,97,8],[14,83,132,81,8],[14,85,125,20,5],[14,86,130,101,9],[14,87,93,45,10],[14,87,150,71,10],[14,88,136,20,3],[14,88,143,102,9],[14,90,138,72,1],[14,91,93,31,1],[14,93,96,91,1],[14,95,100,124,3],[14,98,115,109,1],[14,100,147,147,9],[14,100,153,113,8],[14,102,110,119,8],[14,103,109,102,1],[14,103,117,67,17],[14,103,126,75,11],[14,104,128,154,9],[14,109,127,107,1],[14,110,112,47,8],[14,112,148,95,3],[14,113,153,148,8],[14,114,121,83,9],[14,114,138,76,7],[14,117,144,59,11],[14,123,148,137,10],[14,126,127,50,1],[14,130,138,70,7],[14,131,133,36,5],[15,16,63,95,3],[15,16,129,156,11],[15,16,154,64,11],[15,17,92,45,8],[15,17,124,91,1],[15,18,127,0,1],[15,19,35,75,16],[15,19,70,135,5],[15,19,71,94,20],[15,19,117,100,10],[15,19,129,132,13],[15,20,87,102,5],[15,21,42,111,14],[15,21,75,2,6],[15,21,137,136,1],[15,22,100,7,14],[15,24,46,121,3],[15,24,75,76,3],[15,26,126,25,3],[15,26,155,108,3],[15,27,57,92,8],[15,28,53,92,2],[15,28,120,4,2],[15,30,122,10,1],[15,30,128,39,1],[15,32,85,34,5],[15,33,86,121,1],[15,35,95,33,1],[15,36,61,101,8],[15,36,108,101,8],[15,36,144,145,6],[15,37,62,61,13],[15,37,111,69,11],[15,40,91,41,1],[15,41,101,20,5],[15,41,102,108,8],[15,42,62,9,1],[15,42,70,29,17],[15,42,72,84,17],[15,42,79,66,17],[15,42,129,137,6],[15,43,53,22,14],[15,43,124,72,13],[15,44,127,39,16],[15,45,94,134,5],[15,46,91,92,1],[15,46,96,98,8],[15,47,84,138,7],[15,49,71,92,8],[15,49,104,79,20],[15,50,129,116,1],[15,50,130,3,1],[15,52,108,94,8],[15,53,63,102,13],[15,53,86,121,17],[15,53,131,48,5],[15,54,126,56,1],[15,61,69,83,8],[15,61,135,31,5],[15,61,143,114,13],[15,61,147,140,13],[15,61,153,126,8],[15,61,156,122,13],[15,63,97,89,8],[15,64,134,11,5],[15,65,90,119,1],[15,66,128,82,14],[15,66,133,4,5],[15,66,149,113,17],[15,69,129,48,5],[15,73,74,108,8],[15,74,155,112,6],[15,77,89,112,8],[15,77,136,26,1],[15,80,103,57,11],[15,80,126,58,3],[15,82,127,52,17],[15,85,148,75,14],[15,85,155,2,6],[15,86,98,87,20],[15,88,101,122,8],[15,93,129,12,6],[15,98,138,5,7],[15,101,154,124,8],[15,103,120,8,8],[15,105,143,84,1],[15,105,144,33,1],[15,109,124,156,1],[15,109,150,20,1],[15,117,121,9,1],[15,124,134,34,5],[15,125,134,20,5],[15,127,134,124,5],[15,128,133,100,5],[15,131,133,11,5],[15,133,139,85,5],[15,139,155,81,6],[16,17,28,82,2],[16,17,127,112,11],[16,18,26,96,3],[16,18,42,139,7],[16,18,62,58,3],[16,18,84,4,11],[16,18,129,21,11],[16,19,94,49,11],[16,20,31,125,5],[16,20,50,6,1],[16,20,154,67,8],[16,23,73,121,7],[16,23,151,79,6],[16,24,110,35,3],[16,24,134,48,7],[16,24,140,146,5],[16,25,144,46,5],[16,29,83,114,9],[16,29,143,17,14],[16,30,97,114,2],[16,32,93,80,5],[16,33,45,130,1],[16,34,95,18,3],[16,35,100,89,12],[16,36,133,75,7],[16,37,61,120,14],[16,37,77,63,10],[16,37,128,36,15],[16,38,96,104,9],[16,39,41,11,13],[16,40,74,14,10],[16,40,88,1,3],[16,40,138,42,9],[16,42,139,19,7],[16,44,143,125,11],[16,45,84,1,2],[16,45,129,101,11],[16,45,142,141,1],[16,45,148,68,3],[16,46,150,104,12],[16,47,111,80,11],[16,49,63,136,2],[16,49,73,0,1],[16,49,122,10,7],[16,50,132,3,1],[16,51,74,71,8],[16,53,72,95,3],[16,53,79,38,12],[16,53,106,69,10],[16,54,108,81,1],[16,54,121,4,2],[16,54,125,36,1],[16,54,137,7,2],[16,54,151,109,2],[16,55,56,18,11],[16,56,134,103,6],[16,56,151,24,4],[16,58,99,122,3],[16,58,137,24,7],[16,59,69,125,11],[16,59,100,131,11],[16,62,79,12,6],[16,64,72,52,11],[16,64,84,120,10],[16,64,100,51,11],[16,65,113,3,1],[16,66,99,60,3],[16,66,143,16,12],[16,67,124,124,15],[16,72,128,83,8],[16,73,84,68,2],[16,75,79,90,1],[16,76,131,140,11],[16,77,144,37,8],[16,78,102,32,5],[16,79,132,20,5],[16,80,123,150,11],[16,81,135,53,5],[16,82,126,127,9],[16,82,148,114,12],[16,85,88,24,3],[16,85,116,147,11],[16,87,155,79,6],[16,90,93,84,1],[16,90,138,63,3],[16,91,148,134,1],[16,92,152,13,11],[16,94,143,78,11],[16,95,146,143,5],[16,97,126,71,10],[16,97,141,143,1],[16,98,138,29,7],[16,98,149,30,1],[16,101,138,74,15],[16,105,124,141,1],[16,105,131,56,1],[16,106,152,113,7],[16,108,124,136,3],[16,110,112,110,11],[16,110,149,136,1],[16,112,116,97,8],[16,115,154,99,1],[16,122,150,89,16],[16,133,148,73,9],[16,136,142,11,2],[16,136,155,36,2],[16,147,156,34,15],[17,18,77,103,8],[17,18,82,105,1],[17,19,72,122,16],[17,21,145,133,5],[17,22,52,26,4],[17,22,139,79,7],[17,23,136,51,1],[17,25,80,57,3],[17,27,138,16,8],[17,27,140,0,1],[17,30,112,156,1],[17,30,135,124,2],[17,32,148,83,6],[17,33,41,88,1],[17,33,49,65,1],[17,33,52,83,1],[17,34,145,146,15],[17,38,68,71,3],[17,38,84,15,16],[17,41,137,32,6],[17,41,139,19,7],[17,43,49,52,21],[17,43,86,99,19],[17,43,136,118,2],[17,47,99,27,15],[17,47,151,61,16],[17,48,111,46,5],[17,49,108,19,8],[17,50,133,68,1],[17,50,148,116,1],[17,51,132,67,8],[17,53,67,35,9],[17,54,82,22,1],[17,54,112,58,1],[17,56,123,13,12],[17,57,113,0,1],[17,58,123,155,4],[17,59,141,101,1],[17,61,62,27,13],[17,61,63,108,9],[17,61,105,14,2],[17,62,147,137,6],[17,62,153,43,15],[17,63,94,124,13],[17,70,75,39,19],[17,71,95,45,4],[17,73,93,15,8],[17,75,102,141,1],[17,75,106,68,3],[17,75,110,67,8],[17,76,83,109,1],[17,78,95,149,3],[17,78,110,85,20],[17,78,132,9,1],[17,80,92,63,8],[17,80,149,92,8],[17,83,108,36,9],[17,85,112,37,18],[17,86,134,111,5],[17,88,106,149,8],[17,89,95,131,3],[17,90,143,113,1],[17,90,151,139,2],[17,91,149,25,1],[17,93,106,50,1],[17,96,143,93,10],[17,99,128,10,6],[17,102,134,58,3],[17,104,116,132,14],[17,104,132,50,1],[17,106,113,38,7],[17,106,129,12,7],[17,108,111,142,8],[17,111,126,53,8],[17,111,130,97,8],[17,112,126,116,8],[17,113,145,106,7],[17,114,121,134,6],[17,114,141,7,1],[17,115,119,109,1],[17,117,128,114,13],[17,119,134,75,6],[17,122,147,89,17],[17,123,127,30,1],[17,123,130,8,8],[17,126,155,27,7],[17,128,152,108,9],[17,136,151,145,1],[18,19,78,25,3],[18,20,139,12,5],[18,21,83,144,6],[18,21,142,121,14],[18,24,26,8,3],[18,24,31,82,3],[18,25,61,153,3],[18,25,71,81,3],[18,25,105,2,1],[18,25,149,94,3],[18,26,137,23,3],[18,27,114,38,14],[18,29,138,56,7],[18,32,141,121,1],[18,33,77,43,1],[18,33,94,105,1],[18,34,41,98,17],[18,34,73,70,8],[18,35,92,100,8],[18,35,134,120,5],[18,36,37,60,3],[18,38,42,30,1],[18,39,59,138,7],[18,39,78,14,8],[18,39,100,68,2],[18,40,72,14,8],[18,40,78,63,13],[18,40,137,128,6],[18,43,78,18,17],[18,43,137,78,6],[18,44,55,46,13],[18,44,77,41,8],[18,44,79,11,19],[18,44,126,115,1],[18,45,69,153,11],[18,47,68,51,2],[18,47,94,118,1],[18,47,100,23,6],[18,49,83,13,8],[18,49,119,6,14],[18,53,54,82,1],[18,53,92,31,8],[18,53,131,74,8],[18,54,60,101,1],[18,57,108,15,8],[18,61,121,120,10],[18,62,68,121,2],[18,64,128,76,13],[18,66,128,107,7],[18,66,149,68,2],[18,68,78,127,2],[18,71,133,102,5],[18,72,124,115,1],[18,72,156,89,13],[18,75,84,93,17],[18,78,155,37,6],[18,79,111,52,17],[18,81,97,54,1],[18,82,125,147,14],[18,82,145,14,8],[18,83,100,99,8],[18,83,139,102,7],[18,90,94,84,1],[18,93,107,140,7],[18,93,109,94,1],[18,93,152,28,2],[18,95,129,130,3],[18,95,153,71,3],[18,97,139,10,6],[18,99,154,75,15],[18,100,114,21,14],[18,103,122,139,7],[18,110,119,50,1],[18,122,138,104,7],[18,126,155,50,1],[18,126,156,127,8],[18,131,142,8,8],[18,136,152,13,1],[18,139,141,152,1],[19,21,37,141,1],[19,22,99,71,14],[19,23,102,134,5],[19,24,37,107,3],[19,24,68,119,2],[19,24,145,126,3],[19,25,48,47,3],[19,25,121,97,3],[19,26,71,3,1],[19,26,78,89,3],[19,26,99,20,3],[19,27,77,155,6],[19,27,85,4,14],[19,28,104,33,1],[19,29,58,140,3],[19,30,127,21,1],[19,31,59,4,13],[19,32,93,31,5],[19,33,66,106,1],[19,33,147,84,1],[19,34,64,130,13],[19,34,68,12,2],[19,34,125,146,15],[19,34,151,121,15],[19,36,42,149,16],[19,36,67,8,8],[19,36,87,12,6],[19,39,65,66,16],[19,42,51,138,7],[19,42,57,5,17],[19,42,125,107,7],[19,43,133,117,5],[19,44,45,53,17],[19,44,54,14,1],[19,45,153,88,14],[19,46,63,41,13],[19,46,153,55,13],[19,47,98,24,3],[19,48,65,95,3],[19,48,119,127,5],[19,49,69,48,5],[19,51,108,29,8],[19,51,127,74,8],[19,54,67,70,1],[19,54,130,135,1],[19,55,123,83,8],[19,56,63,15,13],[19,56,128,87,14],[19,56,155,133,5],[19,60,99,73,3],[19,61,94,18,13],[19,61,102,142,13],[19,62,101,63,8],[19,63,109,11,1],[19,65,79,96,8],[19,66,75,114,18],[19,67,82,1,2],[19,69,112,144,6],[19,70,147,9,1],[19,71,88,77,8],[19,71,109,154,1],[19,71,116,70,13],[19,72,105,112,1],[19,73,95,55,3],[19,74,127,35,8],[19,75,79,87,18],[19,78,125,111,23],[19,80,108,28,2],[19,87,119,114,14],[19,89,106,41,7],[19,94,114,6,15],[19,95,123,136,1],[19,97,122,72,8],[19,101,112,1,2],[19,102,113,6,15],[19,102,152,142,15],[19,105,128,95,1],[19,106,136,10,1],[19,107,141,24,1],[19,112,128,112,14],[19,121,130,25,3],[19,124,130,3,1],[19,125,154,7,15],[19,126,149,13,8],[19,127,147,60,3],[19,130,146,146,15],[20,21,26,134,6],[20,22,59,148,9],[20,23,64,38,8],[20,24,44,124,4],[20,24,45,109,2],[20,25,105,85,1],[20,25,137,137,14],[20,26,146,117,5],[20,28,68,129,5],[20,28,78,84,2],[20,28,128,55,4],[20,31,137,22,5],[20,31,147,69,5],[20,32,146,131,5],[20,34,65,32,5],[20,35,49,145,5],[20,36,83,40,7],[20,37,95,127,3],[20,37,131,138,5],[20,38,55,131,5],[20,38,81,83,5],[20,38,122,108,8],[20,39,142,35,8],[20,41,112,1,2],[20,41,142,53,7],[20,43,106,98,5],[20,43,115,59,1],[20,43,117,40,7],[20,44,106,24,4],[20,45,115,109,1],[20,47,93,101,6],[20,47,119,102,5],[20,47,131,113,5],[20,48,86,140,6],[20,49,58,44,4],[20,49,136,120,2],[20,52,112,107,5],[20,53,69,156,7],[20,55,92,26,6],[20,56,126,75,6],[20,58,155,53,5],[20,59,62,84,5],[20,59,90,43,2],[20,59,109,44,2],[20,60,86,9,2],[20,61,129,126,10],[20,63,95,28,5],[20,63,114,56,6],[20,63,143,76,5],[20,65,131,73,5],[20,66,82,4,5],[20,66,137,112,5],[20,67,78,114,5],[20,67,79,78,5],[20,67,144,92,16],[20,69,83,31,5],[20,69,143,151,7],[20,70,108,76,5],[20,70,120,104,5],[20,71,119,57,6],[20,71,156,107,6],[20,73,127,24,3],[20,76,83,64,5],[20,78,131,92,5],[20,81,131,64,5],[20,83,111,85,5],[20,90,115,38,1],[20,90,137,112,1],[20,91,121,145,1],[20,92,105,109,5],[20,94,97,143,5],[20,98,134,21,5],[20,99,128,39,5],[20,103,105,119,3],[20,103,106,95,7],[20,106,141,10,3],[20,106,144,106,18],[20,107,116,133,10],[20,107,121,88,6],[20,107,130,67,5],[20,109,139,143,2],[20,120,129,115,1],[20,121,125,73,5],[20,123,131,14,5],[20,128,134,56,6],[20,136,144,29,2],[20,136,151,141,1],[21,23,37,98,6],[21,23,128,128,10],[21,25,52,152,5],[21,26,48,153,5],[21,26,67,40,4],[21,27,36,41,20],[21,30,70,52,2],[21,30,101,91,1],[21,32,49,43,6],[21,33,49,150,1],[21,33,75,149,1],[21,33,129,131,1],[21,35,99,71,15],[21,36,126,28,4],[21,37,97,89,11],[21,37,140,132,19],[21,42,55,72,13],[21,43,69,75,15],[21,43,121,43,17],[21,44,109,134,2],[21,44,144,119,7],[21,47,156,6,16],[21,49,92,154,9],[21,51,96,105,1],[21,51,132,118,1],[21,51,144,4,6],[21,51,152,122,14],[21,52,130,43,15],[21,53,109,139,2],[21,53,135,20,7],[21,55,126,133,9],[21,56,84,63,13],[21,56,87,102,15],[21,58,143,108,5],[21,59,153,20,8],[21,60,62,80,3],[21,60,148,49,4],[21,62,81,121,14],[21,65,146,2,6],[21,67,142,7,11],[21,68,112,140,2],[21,70,97,80,9],[21,72,120,121,10],[21,72,151,154,14],[21,73,133,111,5],[21,74,126,72,8],[21,75,123,95,4],[21,75,126,61,11],[21,76,121,148,15],[21,76,132,33,1],[21,76,143,57,15],[21,77,126,127,8],[21,78,88,127,14],[21,78,116,79,14],[21,79,100,13,15],[21,79,102,122,15],[21,79,106,53,7],[21,80,90,40,1],[21,80,148,18,14],[21,81,130,96,9],[21,82,156,24,3],[21,84,103,152,11],[21,85,103,55,11],[21,89,142,74,12],[21,89,146,117,16],[21,97,128,152,12],[21,97,130,92,8],[21,98,102,148,15],[21,98,117,49,11],[21,98,122,86,15],[21,100,151,106,7],[21,100,154,63,14],[21,102,117,31,10],[21,107,127,35,7],[21,107,128,44,9],[21,111,143,75,14],[21,112,148,5,14],[21,122,146,61,20],[21,123,138,37,9],[21,126,145,112,8],[21,129,134,145,5],[21,130,139,0,1],[21,133,143,93,7],[21,133,150,22,7],[21,135,141,113,1],[21,138,155,120,10],[21,145,149,77,8],[22,23,97,8,10],[22,23,102,127,6],[22,23,116,3,1],[22,24,103,47,4],[22,24,145,75,3],[22,25,136,31,1],[22,26,71,136,2],[22,27,32,77,9],[22,28,143,17,3],[22,29,67,110,8],[22,31,103,94,11],[22,32,52,136,2],[22,32,88,111,5],[22,32,93,141,1],[22,32,137,22,9],[22,34,87,62,14],[22,35,61,137,9],[22,35,76,97,9],[22,37,120,53,14],[22,37,135,82,5],[22,38,135,69,8],[22,40,106,88,9],[22,42,53,66,14],[22,42,60,112,3],[22,42,70,64,16],[22,42,114,98,15],[22,43,96,113,8],[22,43,131,56,14],[22,44,79,15,14],[22,44,120,51,10],[22,45,127,57,14],[22,46,82,5,15],[22,47,72,87,14],[22,51,73,60,3],[22,52,71,115,1],[22,52,109,100,1],[22,52,131,92,8],[22,52,137,56,7],[22,53,108,52,11],[22,53,147,74,11],[22,54,60,147,3],[22,54,71,41,2],[22,54,82,61,1],[22,56,72,98,14],[22,56,96,50,1],[22,56,128,109,2],[22,58,85,119,3],[22,59,83,119,14],[22,60,110,28,2],[22,61,92,20,9],[22,62,80,87,14],[22,63,106,45,9],[22,67,89,105,2],[22,67,97,109,3],[22,70,133,35,6],[22,71,94,85,14],[22,71,113,51,14],[22,71,123,106,8],[22,75,109,54,2],[22,77,134,108,9],[22,79,125,46,14],[22,80,81,104,15],[22,82,112,44,14],[22,83,139,91,1],[22,89,133,92,8],[22,92,98,73,8],[22,92,99,2,6],[22,93,107,152,10],[22,93,146,4,20],[22,94,95,78,3],[22,100,129,4,14],[22,100,146,120,11],[22,103,134,147,9],[22,104,152,39,15],[22,105,121,31,1],[22,105,141,120,1],[22,106,120,66,7],[22,106,127,153,7],[22,107,126,110,7],[22,109,151,95,2],[22,111,141,44,1],[22,114,116,156,16],[22,115,140,75,1],[22,116,140,138,11],[22,121,123,114,13],[22,123,130,8,8],[22,132,135,25,4],[22,136,141,86,1],[22,137,151,0,1],[22,142,155,25,5],[22,146,150,133,7],[22,149,151,41,20],[23,24,125,94,3],[23,25,115,147,1],[23,26,103,141,1],[23,27,77,99,6],[23,27,110,123,6],[23,28,111,125,2],[23,30,69,101,3],[23,30,138,85,1],[23,31,88,91,1],[23,31,99,64,6],[23,31,117,140,6],[23,31,132,112,6],[23,32,57,120,6],[23,33,62,71,1],[23,33,115,128,1],[23,33,153,37,1],[23,34,103,33,1],[23,35,59,50,1],[23,35,103,104,6],[23,36,149,18,6],[23,37,57,19,6],[23,37,85,1,2],[23,38,76,99,6],[23,38,122,51,6],[23,39,72,23,6],[23,40,113,69,6],[23,44,59,36,7],[23,47,54,65,1],[23,47,63,54,2],[23,50,107,52,1],[23,50,122,156,1],[23,51,117,111,6],[23,52,144,146,8],[23,54,91,14,2],[23,55,80,1,2],[23,55,127,124,6],[23,57,59,8,7],[23,58,74,52,5],[23,60,63,105,3],[23,62,120,58,3],[23,62,144,46,6],[23,63,88,80,6],[23,65,75,111,6],[23,65,132,148,6],[23,68,124,49,3],[23,69,137,125,6],[23,70,150,63,7],[23,71,80,81,6],[23,72,103,63,6],[23,76,133,74,5],[23,77,88,149,8],[23,77,151,54,2],[23,81,89,129,6],[23,81,111,16,6],[23,82,97,79,6],[23,85,112,102,6],[23,86,133,154,6],[23,86,149,35,7],[23,87,124,28,3],[23,89,148,76,6],[23,91,133,117,2],[23,92,137,99,6],[23,93,124,127,6],[23,93,133,13,7],[23,94,130,145,6],[23,94,156,48,5],[23,95,123,75,4],[23,98,115,146,1],[23,99,122,128,6],[23,101,122,6,9],[23,102,134,65,5],[23,108,132,4,8],[23,110,123,75,6],[23,111,139,30,1],[23,111,152,131,6],[23,114,119,29,7],[23,120,144,84,6],[23,121,150,109,2],[23,124,128,47,7],[23,130,152,16,6],[23,135,144,121,6],[23,144,146,52,8],[23,148,154,65,6],[24,25,77,94,3],[24,25,81,0,1],[24,25,144,69,7],[24,26,137,138,12],[24,29,50,151,1],[24,29,53,10,5],[24,29,85,47,3],[24,29,117,25,5],[24,33,39,148,1],[24,33,98,24,1],[24,33,135,151,1],[24,33,147,104,1],[24,34,132,45,4],[24,34,140,110,3],[24,35,37,77,4],[24,35,93,74,5],[24,37,61,70,4],[24,38,150,34,4],[24,40,64,60,4],[24,43,113,115,1],[24,44,145,78,3],[24,45,60,20,4],[24,45,83,134,4],[24,47,85,76,3],[24,47,95,38,4],[24,47,155,133,4],[24,48,65,117,3],[24,50,70,70,1],[24,50,147,119,1],[24,51,140,31,3],[24,52,63,78,3],[24,52,122,125,3],[24,53,132,118,2],[24,54,68,104,1],[24,57,96,81,3],[24,61,74,43,5],[24,62,64,2,3],[24,63,128,59,6],[24,65,123,17,3],[24,65,125,7,3],[24,66,90,90,1],[24,68,127,120,2],[24,68,134,88,4],[24,68,143,136,2],[24,70,126,13,4],[24,70,132,71,4],[24,71,94,144,3],[24,73,87,22,4],[24,74,139,44,4],[24,76,154,93,3],[24,77,100,6,3],[24,80,145,85,3],[24,83,101,113,3],[24,83,153,123,5],[24,86,101,14,4],[24,88,125,53,3],[24,94,126,84,3],[24,94,145,117,3],[24,97,154,62,3],[24,102,104,61,3],[24,104,125,23,3],[24,107,116,105,3],[24,111,137,61,3],[24,112,149,7,3],[24,113,124,124,3],[24,114,148,124,4],[24,114,153,50,1],[24,115,142,53,1],[24,119,154,13,5],[24,143,152,95,5],[25,26,55,53,5],[25,28,52,85,2],[25,29,82,17,3],[25,29,93,10,5],[25,30,66,110,1],[25,32,122,114,4],[25,33,113,82,1],[25,34,84,77,3],[25,35,114,114,4],[25,36,69,5,4],[25,36,115,90,1],[25,37,38,102,3],[25,37,117,11,4],[25,38,55,53,5],[25,41,110,116,3],[25,41,145,135,3],[25,44,101,56,4],[25,44,138,153,4],[25,47,54,90,2],[25,48,109,122,2],[25,48,124,77,6],[25,48,133,54,8],[25,50,69,67,1],[25,50,70,9,1],[25,51,56,19,3],[25,51,63,155,3],[25,51,121,84,3],[25,53,107,6,5],[25,54,141,0,6],[25,55,64,133,6],[25,55,117,94,3],[25,58,136,67,5],[25,62,89,58,3],[25,62,113,121,3],[25,66,68,147,2],[25,68,93,153,3],[25,69,126,20,7],[25,69,154,17,4],[25,70,133,16,4],[25,71,89,67,4],[25,76,121,56,3],[25,82,143,13,3],[25,84,96,80,3],[25,84,101,65,3],[25,86,98,35,3],[25,89,94,21,3],[25,89,136,84,1],[25,89,139,46,5],[25,93,140,93,5],[25,95,133,122,5],[25,95,144,79,3],[25,97,101,112,3],[25,98,132,82,3],[25,101,104,112,3],[25,101,123,154,5],[25,101,136,144,5],[25,101,147,109,3],[25,105,107,28,6],[25,107,138,79,3],[25,109,111,121,1],[25,111,138,65,3],[25,113,151,49,3],[25,115,153,151,1],[25,123,128,96,6],[25,124,140,99,3],[25,148,156,63,6],[26,27,152,57,4],[26,30,93,70,2],[26,30,145,103,1],[26,32,90,136,8],[26,34,35,51,3],[26,34,90,132,2],[26,34,154,22,4],[26,37,139,127,3],[26,38,123,88,5],[26,40,43,149,4],[26,40,101,51,3],[26,41,81,117,3],[26,41,126,46,5],[26,42,56,29,4],[26,44,87,51,3],[26,44,142,74,4],[26,44,147,20,4],[26,46,130,102,3],[26,49,93,114,4],[26,49,132,151,4],[26,50,146,156,1],[26,52,149,100,3],[26,53,73,125,3],[26,53,90,135,2],[26,53,137,150,5],[26,54,82,9,1],[26,55,76,95,3],[26,55,123,85,3],[26,56,70,118,2],[26,57,123,14,4],[26,62,144,123,3],[26,64,75,97,4],[26,65,70,146,3],[26,66,111,69,3],[26,66,129,84,3],[26,67,75,16,4],[26,67,143,94,3],[26,70,95,129,4],[26,71,90,149,2],[26,73,82,43,3],[26,74,141,54,2],[26,76,81,131,3],[26,78,103,23,3],[26,80,120,51,3],[26,82,108,140,3],[26,83,136,16,3],[26,85,154,152,3],[26,86,122,48,4],[26,88,109,14,3],[26,88,127,13,3],[26,89,104,90,1],[26,89,117,92,5],[26,93,113,110,3],[26,93,143,143,5],[26,98,110,106,3],[26,101,149,155,5],[26,102,123,125,3],[26,103,146,80,3],[26,107,125,3,1],[26,108,109,80,1],[26,110,111,38,3],[26,113,135,23,3],[26,114,120,42,4],[26,115,150,42,1],[26,120,127,127,3],[26,122,155,150,5],[26,132,141,17,1],[26,138,153,154,5],[26,140,153,20,5],[27,28,65,33,1],[27,29,130,111,14],[27,30,63,110,1],[27,30,109,64,3],[27,30,120,128,3],[27,31,142,46,14],[27,32,80,56,5],[27,32,116,120,9],[27,32,123,111,5],[27,33,127,22,1],[27,34,37,94,14],[27,35,143,73,11],[27,36,71,12,7],[27,37,39,38,19],[27,37,75,74,10],[27,40,67,59,10],[27,40,115,146,1],[27,40,135,111,5],[27,40,151,5,18],[27,42,99,49,15],[27,43,53,83,11],[27,43,136,138,2],[27,44,101,136,2],[27,44,124,139,9],[27,45,133,41,6],[27,46,78,65,14],[27,47,74,151,9],[27,47,123,97,10],[27,49,123,105,2],[27,52,66,57,14],[27,52,68,21,3],[27,52,121,99,15],[27,52,126,134,7],[27,55,56,44,15],[27,55,109,49,2],[27,58,153,34,4],[27,59,67,107,12],[27,59,125,64,13],[27,62,108,66,8],[27,63,145,22,13],[27,65,90,38,1],[27,66,96,67,8],[27,69,136,103,3],[27,71,104,138,7],[27,71,109,101,2],[27,71,134,146,6],[27,74,145,8,8],[27,75,91,134,1],[27,75,105,23,2],[27,77,145,72,8],[27,80,150,67,8],[27,82,101,154,9],[27,84,97,74,8],[27,84,147,50,1],[27,89,141,52,1],[27,90,96,147,3],[27,95,151,102,3],[27,96,112,124,8],[27,96,153,131,8],[27,97,154,80,9],[27,98,148,125,14],[27,100,122,6,15],[27,103,119,42,16],[27,106,107,3,1],[27,109,114,56,2],[27,109,139,80,1],[27,110,150,72,14],[27,111,131,137,6],[27,119,133,105,3],[27,121,135,85,5],[27,124,137,54,3],[27,124,149,46,18],[27,125,145,101,8],[27,134,143,114,6],[27,144,156,120,10],[28,29,139,20,3],[28,31,37,28,2],[28,33,123,9,2],[28,34,76,155,2],[28,34,95,16,3],[28,35,99,110,2],[28,38,55,87,3],[28,38,59,55,4],[28,38,91,67,1],[28,39,49,28,3],[28,40,122,46,3],[28,41,45,101,3],[28,41,141,13,1],[28,42,57,57,3],[28,45,66,28,2],[28,45,72,138,2],[28,45,100,31,2],[28,46,149,98,2],[28,47,56,84,2],[28,49,81,98,2],[28,50,84,54,1],[28,52,57,76,2],[28,52,100,40,2],[28,54,155,64,3],[28,55,98,72,2],[28,56,152,92,3],[28,59,61,118,3],[28,60,64,68,5],[28,60,148,122,4],[28,61,62,86,2],[28,61,151,84,2],[28,61,156,74,5],[28,62,95,34,2],[28,63,71,38,3],[28,63,116,115,1],[28,64,134,82,2],[28,66,93,148,2],[28,67,80,79,2],[28,68,147,64,4],[28,71,153,91,1],[28,72,120,132,2],[28,73,75,60,3],[28,74,89,16,4],[28,74,148,78,2],[28,76,93,17,2],[28,78,91,149,1],[28,78,130,92,2],[28,78,134,92,2],[28,79,99,97,2],[28,80,108,100,2],[28,80,145,113,2],[28,83,149,10,3],[28,86,93,142,3],[28,86,116,119,3],[28,86,120,152,3],[28,86,154,103,3],[28,89,145,42,2],[28,91,92,14,2],[28,95,134,17,3],[28,96,137,67,7],[28,96,141,90,2],[28,98,155,152,2],[28,99,119,98,2],[28,100,135,128,2],[28,106,153,133,4],[28,111,135,104,2],[28,117,131,89,2],[28,117,136,65,1],[28,117,137,2,6],[28,123,136,153,2],[28,130,146,56,2],[28,135,151,76,2],[28,138,156,58,5],[28,142,149,130,2],[28,153,154,17,3],[29,31,117,33,1],[29,34,75,64,17],[29,36,128,143,20],[29,37,114,85,17],[29,37,142,64,17],[29,39,60,135,5],[29,39,74,21,11],[29,39,92,53,11],[29,39,144,75,8],[29,41,76,107,7],[29,42,103,119,16],[29,45,66,130,18],[29,47,79,81,19],[29,48,108,78,5],[29,48,141,17,1],[29,51,119,36,14],[29,52,111,117,10],[29,53,89,91,1],[29,57,90,110,1],[29,57,147,99,15],[29,58,115,42,1],[29,59,116,115,1],[29,60,122,126,5],[29,63,75,6,17],[29,63,111,50,1],[29,64,104,68,2],[29,65,123,115,1],[29,65,125,99,17],[29,65,136,100,1],[29,65,146,29,15],[29,66,141,34,1],[29,67,76,141,1],[29,69,130,98,12],[29,71,120,153,13],[29,72,149,24,3],[29,73,97,106,10],[29,74,99,4,8],[29,76,92,2,6],[29,77,84,114,8],[29,77,141,72,1],[29,82,106,116,7],[29,83,108,4,11],[29,84,130,152,17],[29,85,101,85,8],[29,85,103,81,11],[29,86,150,39,19],[29,87,137,17,7],[29,88,112,43,14],[29,89,130,116,14],[29,92,94,77,8],[29,92,145,63,8],[29,94,137,63,6],[29,102,114,20,5],[29,104,124,126,9],[29,106,120,113,7],[29,106,125,86,7],[29,109,126,102,1],[29,113,122,39,16],[29,113,139,132,7],[29,119,146,52,20],[29,121,122,115,1],[29,122,128,126,12],[29,127,155,135,5],[29,138,142,115,1],[30,31,143,39,1],[30,32,115,41,1],[30,33,131,115,1],[30,34,75,59,2],[30,34,148,66,1],[30,36,68,96,2],[30,37,101,146,2],[30,38,63,130,1],[30,39,102,118,1],[30,40,66,154,1],[30,40,148,109,2],[30,41,88,65,1],[30,44,127,131,1],[30,47,55,69,2],[30,47,141,112,1],[30,51,53,22,1],[30,52,62,149,1],[30,53,75,45,2],[30,54,80,9,1],[30,54,87,153,2],[30,55,80,50,1],[30,55,116,147,3],[30,55,140,112,1],[30,56,101,30,2],[30,57,120,60,2],[30,58,63,132,2],[30,58,125,2,1],[30,60,63,15,1],[30,61,152,4,2],[30,63,71,0,1],[30,63,128,13,2],[30,65,90,146,1],[30,67,95,81,1],[30,67,115,91,2],[30,68,155,89,2],[30,71,83,112,1],[30,71,154,152,2],[30,73,136,152,2],[30,74,88,3,1],[30,74,101,48,5],[30,76,128,37,1],[30,78,111,67,1],[30,78,129,52,1],[30,79,112,97,1],[30,83,87,122,2],[30,84,86,80,1],[30,87,140,104,1],[30,87,149,106,2],[30,89,115,58,1],[30,93,106,19,1],[30,93,135,83,2],[30,96,111,103,1],[30,98,140,122,1],[30,101,143,36,2],[30,103,122,21,2],[30,103,155,49,2],[30,107,118,53,2],[30,111,114,53,1],[30,117,155,21,3],[30,121,149,131,1],[30,127,128,125,1],[30,127,129,149,1],[30,133,146,131,1],[30,135,152,42,2],[30,136,147,9,3],[30,140,149,0,1],[30,141,143,70,1],[31,33,46,115,1],[31,34,37,65,18],[31,34,88,10,6],[31,34,120,68,2],[31,36,73,34,8],[31,37,64,79,13],[31,37,97,100,8],[31,37,101,0,1],[31,38,111,54,1],[31,40,56,127,18],[31,40,143,15,17],[31,41,139,7,7],[31,42,115,153,1],[31,42,127,131,17],[31,44,111,20,5],[31,46,94,143,16],[31,47,101,26,3],[31,50,112,63,1],[31,54,57,140,1],[31,59,130,109,1],[31,60,92,37,3],[31,60,127,59,3],[31,62,88,90,1],[31,63,96,62,8],[31,65,141,25,1],[31,67,87,71,8],[31,67,106,26,3],[31,69,83,98,8],[31,69,94,110,11],[31,70,74,154,8],[31,70,88,44,14],[31,72,103,134,5],[31,72,137,35,6],[31,77,92,120,8],[31,80,136,90,1],[31,83,85,7,8],[31,84,91,23,1],[31,85,143,78,17],[31,87,114,24,3],[31,89,91,66,1],[31,90,92,147,1],[31,93,94,55,13],[31,96,129,20,5],[31,97,146,31,8],[31,97,151,148,8],[31,98,139,12,6],[31,101,111,75,8],[31,101,123,104,8],[31,102,104,53,17],[31,103,136,106,1],[31,103,150,92,8],[31,104,120,32,5],[31,104,141,130,1],[31,104,144,144,6],[31,107,150,140,7],[31,111,145,126,8],[31,112,116,148,13],[31,112,153,51,15],[31,119,151,156,13],[31,122,148,80,14],[31,123,134,69,5],[31,139,149,87,7],[31,141,153,54,1],[31,150,151,28,2],[32,35,58,40,4],[32,35,73,153,8],[32,35,149,126,7],[32,36,74,23,8],[32,36,137,136,2],[32,37,133,140,7],[32,38,51,21,5],[32,39,48,78,5],[32,40,72,150,5],[32,41,152,135,7],[32,45,93,134,6],[32,47,135,121,6],[32,48,78,101,5],[32,48,79,51,5],[32,48,92,154,8],[32,50,58,74,2],[32,51,127,20,5],[32,54,99,114,1],[32,54,148,100,1],[32,55,89,4,8],[32,56,71,25,4],[32,57,131,56,5],[32,58,79,38,3],[32,59,67,81,5],[32,59,126,138,10],[32,60,84,67,3],[32,62,119,47,5],[32,63,89,27,8],[32,64,92,49,6],[32,66,146,30,1],[32,67,104,153,5],[32,67,147,100,5],[32,68,81,107,2],[32,68,149,32,3],[32,69,86,45,6],[32,69,127,33,1],[32,69,147,145,5],[32,71,99,72,5],[32,74,91,61,1],[32,74,101,117,12],[32,74,137,103,11],[32,74,144,82,5],[32,76,97,63,5],[32,76,156,102,5],[32,77,83,143,7],[32,78,122,87,5],[32,79,130,17,5],[32,81,152,8,5],[32,84,85,9,1],[32,85,92,76,5],[32,86,115,21,1],[32,88,89,41,7],[32,90,149,87,2],[32,94,156,122,5],[32,96,137,78,5],[32,96,148,133,9],[32,98,150,139,5],[32,99,136,19,1],[32,103,135,127,5],[32,106,128,40,7],[32,107,138,49,6],[32,107,150,73,7],[32,108,114,66,5],[32,108,123,66,5],[32,114,124,43,6],[32,114,131,11,5],[32,114,147,90,2],[32,115,129,33,1],[32,116,123,128,9],[32,116,142,149,7],[32,122,145,113,5],[32,124,137,49,6],[32,124,148,133,9],[32,125,153,94,5],[32,138,141,92,2],[33,34,43,119,1],[33,34,140,8,1],[33,36,90,109,1],[33,36,124,21,1],[33,36,137,59,1],[33,37,63,65,1],[33,37,154,145,1],[33,40,54,48,1],[33,44,124,30,1],[33,45,65,5,1],[33,46,143,65,1],[33,48,93,64,1],[33,48,116,109,1],[33,49,88,73,1],[33,52,70,18,1],[33,52,109,135,1],[33,53,104,69,1],[33,53,143,11,1],[33,56,84,140,1],[33,57,117,129,1],[33,58,85,61,1],[33,58,143,111,1],[33,59,92,123,1],[33,59,117,14,1],[33,61,123,124,1],[33,61,131,118,1],[33,62,81,49,1],[33,62,88,25,1],[33,63,90,90,1],[33,63,125,70,1],[33,65,66,90,1],[33,66,125,115,1],[33,66,133,76,1],[33,67,99,142,1],[33,68,108,154,1],[33,71,135,102,1],[33,72,101,134,1],[33,74,76,123,1],[33,74,127,148,1],[33,81,144,113,1],[33,85,119,120,1],[33,85,120,8,1],[33,87,102,126,1],[33,88,109,131,1],[33,88,114,91,1],[33,88,125,49,1],[33,89,97,97,1],[33,91,146,30,1],[33,93,141,132,1],[33,95,146,20,1],[33,96,135,39,1],[33,98,106,4,1],[33,98,154,5,1],[33,99,121,0,1],[33,101,149,92,1],[33,103,119,140,1],[33,103,152,20,1],[33,105,127,31,1],[33,107,134,6,1],[33,109,151,98,1],[33,115,125,66,1],[33,116,145,8,1],[33,119,128,144,1],[33,121,156,107,1],[33,130,133,102,1],[33,133,142,27,1],[33,137,147,53,1],[33,137,151,132,1],[33,142,153,148,1],[33,143,144,92,1],[34,35,61,90,2],[34,37,82,75,20],[34,37,100,24,3],[34,37,138,152,9],[34,37,148,131,14],[34,40,72,120,10],[34,40,122,116,17],[34,41,128,103,15],[34,42,58,48,4],[34,42,151,35,20],[34,43,108,100,8],[34,43,113,133,5],[34,44,78,117,11],[34,46,134,115,1],[34,48,50,119,1],[34,48,108,7,7],[34,54,69,151,2],[34,54,134,15,1],[34,55,73,115,1],[34,55,102,14,9],[34,56,100,73,8],[34,56,147,99,15],[34,58,143,3,1],[34,59,105,147,2],[34,59,111,145,13],[34,60,84,23,3],[34,60,97,40,4],[34,63,96,106,9],[34,63,102,30,1],[34,64,65,46,13],[34,64,69,89,15],[34,65,97,83,8],[34,65,110,149,17],[34,65,120,114,11],[34,69,88,50,1],[34,70,103,146,14],[34,70,150,66,18],[34,72,91,143,1],[34,73,79,37,8],[34,73,98,14,8],[34,74,93,68,3],[34,74,145,87,8],[34,77,114,23,7],[34,77,115,118,1],[34,77,128,156,10],[34,79,90,80,1],[34,79,149,5,19],[34,83,100,4,8],[34,85,135,94,5],[34,85,136,122,1],[34,86,135,15,5],[34,87,131,123,10],[34,88,156,98,14],[34,89,120,40,14],[34,90,92,131,1],[34,90,104,147,1],[34,90,143,80,1],[34,90,148,78,1],[34,97,153,26,4],[34,98,114,52,19],[34,98,125,89,16],[34,101,140,13,11],[34,105,136,127,1],[34,106,125,145,7],[34,108,126,48,7],[34,108,134,68,3],[34,109,131,88,1],[34,109,155,87,2],[34,110,142,54,1],[34,113,134,139,5],[34,117,153,12,8],[34,120,126,147,11],[34,121,144,20,6],[34,123,144,43,8],[34,127,132,93,18],[34,146,156,19,13],[35,37,55,64,17],[35,37,156,60,4],[35,38,132,77,10],[35,40,129,53,17],[35,41,91,6,1],[35,42,121,73,9],[35,43,71,112,16],[35,43,110,105,1],[35,44,143,137,7],[35,44,153,140,19],[35,45,82,90,1],[35,46,93,26,5],[35,47,49,31,16],[35,47,84,149,16],[35,47,114,59,16],[35,50,101,145,1],[35,50,117,103,1],[35,51,97,5,8],[35,52,63,151,18],[35,52,141,93,1],[35,54,91,145,1],[35,55,72,21,13],[35,55,96,49,10],[35,58,138,40,4],[35,60,147,30,2],[35,61,151,104,14],[35,61,156,31,13],[35,63,88,131,13],[35,63,94,88,13],[35,66,107,56,7],[35,66,128,50,1],[35,70,91,40,1],[35,71,115,106,1],[35,75,130,91,1],[35,80,145,136,1],[35,83,114,15,8],[35,84,134,1,2],[35,86,93,117,13],[35,86,101,17,10],[35,90,125,134,1],[35,91,114,55,1],[35,91,151,102,1],[35,92,136,141,1],[35,94,153,66,15],[35,94,155,50,1],[35,95,115,14,1],[35,95,142,60,5],[35,97,139,132,9],[35,100,131,154,15],[35,100,155,153,6],[35,101,127,141,1],[35,107,109,125,1],[35,110,127,20,5],[35,112,140,92,8],[35,114,154,73,9],[35,115,117,78,1],[35,123,137,119,9],[35,126,127,46,9],[35,127,152,136,1],[35,129,152,112,13],[35,136,142,116,2],[35,143,145,57,16],[35,152,155,84,6],[36,37,39,85,16],[36,37,57,71,18],[36,37,83,136,2],[36,37,155,18,6],[36,38,76,59,14],[36,38,146,89,23],[36,39,42,18,16],[36,39,81,83,8],[36,39,137,115,1],[36,41,97,139,10],[36,42,66,85,16],[36,45,148,16,14],[36,45,150,154,19],[36,47,143,154,18],[36,47,148,51,14],[36,47,155,37,7],[36,47,156,3,1],[36,48,155,119,8],[36,49,51,15,16],[36,51,104,27,14],[36,52,62,154,15],[36,52,101,92,11],[36,52,151,76,16],[36,53,115,74,1],[36,53,143,94,16],[36,53,153,62,15],[36,55,77,104,8],[36,55,85,99,13],[36,56,98,123,11],[36,56,112,60,3],[36,57,63,27,15],[36,58,85,66,3],[36,60,83,41,5],[36,62,85,69,11],[36,63,146,46,20],[36,65,124,142,13],[36,65,128,23,6],[36,70,71,30,2],[36,70,72,84,16],[36,72,116,142,13],[36,74,81,68,2],[36,74,138,3,1],[36,75,145,123,10],[36,78,131,65,16],[36,80,122,0,1],[36,80,139,116,7],[36,83,89,103,12],[36,84,122,75,16],[36,84,135,95,3],[36,86,117,136,2],[36,87,100,7,17],[36,87,147,52,17],[36,88,156,4,20],[36,91,153,63,1],[36,92,116,61,12],[36,92,132,3,1],[36,95,99,62,3],[36,96,106,3,1],[36,101,123,0,1],[36,102,109,132,1],[36,107,132,130,7],[36,112,137,61,6],[36,114,121,134,6],[36,125,148,37,14],[36,126,152,53,12],[36,130,149,88,15],[36,143,147,5,18],[37,38,78,23,6],[37,39,117,118,2],[37,41,116,33,1],[37,42,148,85,14],[37,43,73,9,2],[37,43,95,49,4],[37,46,112,28,2],[37,47,78,82,20],[37,49,91,138,1],[37,50,59,12,1],[37,50,60,117,1],[37,50,61,103,1],[37,50,68,143,1],[37,50,83,99,1],[37,52,80,147,15],[37,52,82,47,19],[37,52,155,49,7],[37,56,132,66,19],[37,57,77,135,6],[37,57,139,55,8],[37,57,151,2,7],[37,58,126,40,4],[37,58,127,135,3],[37,60,85,71,3],[37,62,141,36,1],[37,63,132,122,17],[37,64,71,150,16],[37,65,70,133,5],[37,65,131,60,3],[37,68,154,131,2],[37,69,109,31,1],[37,71,111,47,18],[37,73,134,57,6],[37,76,79,142,16],[37,76,109,124,1],[37,78,99,24,3],[37,78,103,109,1],[37,79,135,144,5],[37,80,112,121,18],[37,81,151,114,16],[37,84,146,55,13],[37,88,126,108,10],[37,92,105,91,1],[37,92,138,59,9],[37,93,111,51,17],[37,93,113,78,17],[37,94,132,67,8],[37,95,133,10,4],[37,103,133,58,4],[37,104,134,92,5],[37,104,146,39,16],[37,105,112,67,1],[37,106,152,23,8],[37,111,120,124,10],[37,111,154,39,15],[37,113,128,3,1],[37,114,130,8,8],[37,117,128,138,9],[37,124,129,138,9],[37,131,149,81,17],[37,133,135,100,5],[37,133,148,67,7],[37,136,145,48,1],[37,137,155,113,6],[37,140,144,125,6],[38,40,129,151,17],[38,42,151,0,1],[38,43,139,130,7],[38,45,47,145,16],[38,47,87,108,9],[38,47,116,113,13],[38,48,120,114,6],[38,48,141,149,1],[38,48,143,71,6],[38,49,86,58,4],[38,49,112,114,16],[38,49,135,15,5],[38,52,91,56,1],[38,53,111,25,3],[38,53,135,38,7],[38,54,127,135,1],[38,56,79,92,8],[38,57,130,78,17],[38,58,136,28,2],[38,59,71,128,16],[38,59,88,146,20],[38,59,95,43,5],[38,62,99,122,16],[38,62,121,101,9],[38,64,98,95,3],[38,64,108,52,11],[38,64,138,45,9],[38,65,139,88,7],[38,66,120,67,8],[38,68,115,84,1],[38,69,90,33,1],[38,71,100,42,17],[38,74,134,32,8],[38,75,80,60,3],[38,76,92,79,8],[38,78,128,0,1],[38,79,87,142,16],[38,79,105,114,1],[38,81,83,52,8],[38,81,102,117,11],[38,81,129,88,14],[38,89,155,2,9],[38,90,94,49,1],[38,95,149,130,3],[38,97,105,112,1],[38,97,108,119,12],[38,99,110,19,16],[38,101,113,148,8],[38,102,106,153,7],[38,109,126,45,2],[38,112,149,111,16],[38,113,131,93,16],[38,113,138,139,7],[38,115,132,136,1],[38,132,144,89,8],[38,133,147,124,8],[38,133,148,67,8],[38,136,152,101,2],[38,137,152,110,6],[38,144,154,117,9],[39,40,51,103,11],[39,41,88,146,20],[39,44,134,77,6],[39,45,110,50,1],[39,45,115,9,1],[39,47,92,128,9],[39,49,116,50,1],[39,50,112,140,1],[39,52,120,67,11],[39,52,152,144,8],[39,53,88,97,12],[39,53,96,152,12],[39,53,122,134,7],[39,60,122,132,4],[39,62,134,151,5],[39,63,69,55,18],[39,64,147,132,17],[39,65,87,17,16],[39,66,81,134,5],[39,67,78,120,8],[39,68,81,32,2],[39,68,112,17,2],[39,70,112,108,8],[39,70,150,37,19],[39,74,112,28,2],[39,74,116,129,12],[39,74,151,65,8],[39,76,99,15,16],[39,78,82,151,16],[39,78,111,45,16],[39,78,119,21,15],[39,81,108,27,8],[39,83,132,44,10],[39,84,92,79,8],[39,87,102,22,15],[39,88,101,73,12],[39,88,147,123,16],[39,89,136,1,2],[39,91,136,78,1],[39,95,142,86,4],[39,98,152,113,16],[39,102,153,86,16],[39,107,151,104,7],[39,108,137,94,6],[39,109,124,82,1],[39,110,132,147,14],[39,111,131,37,16],[39,113,155,138,6],[39,121,151,17,18],[39,125,144,77,6],[39,125,156,79,13],[39,126,138,114,8],[39,126,143,61,12],[39,126,154,43,12],[39,129,150,80,14],[39,130,145,45,16],[40,41,43,12,8],[40,41,92,79,8],[40,41,108,105,2],[40,42,103,87,14],[40,43,87,91,1],[40,43,141,73,1],[40,44,80,63,14],[40,44,129,156,17],[40,48,127,131,5],[40,48,137,95,4],[40,49,68,140,3],[40,49,146,122,18],[40,53,63,90,2],[40,53,154,51,15],[40,56,110,2,6],[40,58,64,69,4],[40,58,80,69,3],[40,58,81,94,3],[40,61,77,119,10],[40,63,93,59,17],[40,65,67,60,3],[40,65,129,29,13],[40,68,152,18,2],[40,70,71,89,19],[40,70,82,128,15],[40,70,115,14,1],[40,70,121,71,22],[40,71,97,95,4],[40,71,128,96,10],[40,72,87,7,17],[40,73,97,13,10],[40,76,133,126,5],[40,77,129,34,10],[40,78,94,120,10],[40,78,153,21,15],[40,80,108,10,6],[40,81,145,55,13],[40,82,129,13,14],[40,83,96,37,10],[40,83,123,67,10],[40,86,133,21,6],[40,92,156,74,10],[40,98,100,114,20],[40,98,112,23,6],[40,101,105,156,2],[40,101,132,15,8],[40,104,117,64,11],[40,105,121,75,2],[40,106,138,42,9],[40,107,151,66,7],[40,115,125,30,1],[40,115,137,47,1],[40,122,129,30,2],[40,136,138,101,2],[40,136,139,51,1],[40,142,146,136,2],[40,153,154,155,8],[41,43,76,8,8],[41,44,110,111,17],[41,44,132,95,4],[41,46,53,10,8],[41,48,52,142,7],[41,48,105,121,2],[41,49,54,10,2],[41,49,124,54,2],[41,51,81,129,13],[41,53,97,77,11],[41,55,82,154,14],[41,57,75,63,15],[41,57,152,92,9],[41,58,81,40,3],[41,58,84,75,3],[41,61,143,73,11],[41,63,76,31,13],[41,63,82,55,14],[41,64,84,36,13],[41,64,130,11,14],[41,66,80,78,18],[41,67,89,57,9],[41,68,75,2,3],[41,70,73,107,8],[41,71,108,44,9],[41,73,111,63,8],[41,74,89,21,11],[41,74,152,16,11],[41,76,88,63,14],[41,77,115,143,1],[41,81,108,65,8],[41,81,114,8,8],[41,87,122,108,9],[41,89,103,145,11],[41,96,134,87,6],[41,96,137,123,8],[41,96,155,106,8],[41,97,128,64,12],[41,99,123,15,10],[41,101,121,121,10],[41,101,154,61,12],[41,105,155,87,2],[41,108,109,42,2],[41,108,143,63,11],[41,111,137,5,6],[41,113,114,7,17],[41,114,119,52,17],[41,122,146,99,16],[41,123,130,45,11],[41,133,150,51,5],[41,135,136,65,1],[41,139,149,148,10],[41,149,154,77,11],[42,43,99,107,7],[42,43,107,42,10],[42,44,52,70,21],[42,44,72,15,17],[42,45,97,98,9],[42,47,61,87,16],[42,47,74,136,2],[42,50,108,33,1],[42,51,148,81,14],[42,54,76,11,1],[42,54,107,16,2],[42,54,118,14,2],[42,55,70,119,16],[42,57,88,30,2],[42,57,111,48,5],[42,58,154,153,5],[42,59,99,126,9],[42,59,138,87,8],[42,60,116,104,3],[42,60,132,80,3],[42,61,63,4,18],[42,61,83,97,11],[42,61,97,2,8],[42,62,94,122,16],[42,62,126,80,9],[42,64,121,107,8],[42,65,84,64,13],[42,65,119,26,3],[42,68,88,86,3],[42,68,90,96,2],[42,69,132,64,15],[42,73,109,60,2],[42,75,115,123,1],[42,77,128,22,11],[42,86,124,115,1],[42,88,108,61,11],[42,88,130,14,9],[42,90,111,7,1],[42,93,107,100,7],[42,97,110,31,8],[42,99,123,9,1],[42,100,132,3,1],[42,101,132,110,8],[42,103,121,51,11],[42,104,137,56,6],[42,105,152,110,1],[42,106,116,119,10],[42,106,124,60,5],[42,106,139,32,7],[42,113,149,29,17],[42,114,137,116,7],[42,117,144,48,7],[42,119,151,138,10],[42,120,129,82,11],[42,120,142,121,13],[42,124,128,82,14],[42,132,156,28,3],[42,133,149,147,7],[42,139,152,96,10],[42,143,148,77,11],[42,143,155,135,7],[43,44,52,14,11],[43,44,124,149,17],[43,45,60,156,4],[43,45,100,124,14],[43,46,121,47,19],[43,47,65,11,18],[43,47,76,139,7],[43,48,151,55,7],[43,49,69,84,11],[43,49,112,52,17],[43,50,130,118,1],[43,54,57,151,2],[43,55,113,131,13],[43,57,115,100,1],[43,58,81,47,3],[43,62,78,28,2],[43,62,81,71,18],[43,62,88,1,2],[43,63,151,24,5],[43,68,71,79,2],[43,68,77,149,3],[43,68,81,12,2],[43,68,135,130,2],[43,69,71,44,14],[43,69,119,154,16],[43,70,135,156,6],[43,70,153,128,17],[43,71,82,22,15],[43,71,130,80,19],[43,74,107,119,10],[43,74,135,74,7],[43,77,117,36,11],[43,79,91,125,1],[43,79,147,139,7],[43,80,138,60,3],[43,80,152,118,1],[43,81,110,67,8],[43,82,131,151,15],[43,83,111,108,8],[43,83,127,21,8],[43,83,139,155,8],[43,89,112,53,16],[43,91,128,41,1],[43,94,104,136,1],[43,94,153,60,3],[43,95,128,17,4],[43,96,104,120,9],[43,100,128,127,14],[43,103,111,141,1],[43,103,142,106,10],[43,110,127,57,17],[43,110,151,43,15],[43,113,130,59,13],[43,115,117,14,1],[43,119,148,107,10],[43,122,143,100,17],[43,129,131,72,13],[43,131,152,28,2],[43,135,137,121,6],[43,140,147,149,20],[43,146,153,91,1],[43,147,151,124,18],[43,150,151,149,21],[44,46,52,152,20],[44,46,156,131,13],[44,47,78,7,19],[44,48,90,120,2],[44,48,152,77,6],[44,49,84,29,17],[44,49,136,28,2],[44,50,59,126,1],[44,50,65,34,1],[44,50,119,91,1],[44,53,58,97,4],[44,53,69,30,2],[44,54,69,115,1],[44,54,115,124,1],[44,55,138,38,9],[44,55,154,7,17],[44,56,135,96,6],[44,56,150,74,9],[44,60,116,88,4],[44,63,65,99,13],[44,63,71,142,16],[44,64,84,111,13],[44,64,136,25,2],[44,65,71,0,1],[44,65,111,133,5],[44,65,136,79,1],[44,71,89,38,19],[44,72,95,82,3],[44,72,155,53,6],[44,74,86,126,9],[44,75,87,66,19],[44,77,97,29,10],[44,78,120,139,7],[44,79,88,128,15],[44,79,93,106,7],[44,80,147,24,3],[44,81,90,50,1],[44,82,135,34,5],[44,83,155,63,7],[44,86,116,92,9],[44,87,108,27,9],[44,89,130,31,16],[44,90,91,77,1],[44,91,149,0,1],[44,96,135,28,3],[44,96,153,76,9],[44,100,110,83,8],[44,101,106,93,9],[44,107,125,75,7],[44,115,127,53,1],[44,117,146,89,13],[44,119,156,56,15],[44,121,153,114,18],[44,124,125,108,8],[44,124,147,138,9],[44,126,141,37,1],[44,129,153,156,17],[44,139,145,72,7],[44,145,156,128,13],[45,46,53,53,20],[45,46,92,38,10],[45,48,91,16,1],[45,50,85,108,1],[45,50,115,23,1],[45,50,134,129,1],[45,51,120,109,1],[45,53,89,103,14],[45,54,80,144,1],[45,55,133,10,6],[45,56,67,79,8],[45,56,109,96,2],[45,57,116,85,13],[45,58,156,95,4],[45,59,90,23,2],[45,59,112,114,13],[45,61,110,115,1],[45,62,68,151,2],[45,62,129,122,13],[45,63,96,139,9],[45,63,132,36,17],[45,64,71,66,13],[45,64,107,41,9],[45,66,120,135,5],[45,67,68,96,3],[45,69,128,93,14],[45,71,150,84,17],[45,73,111,98,8],[45,77,92,84,8],[45,80,108,83,8],[45,81,96,155,6],[45,82,124,85,13],[45,82,132,82,20],[45,83,87,43,9],[45,83,115,43,1],[45,87,92,12,7],[45,92,126,36,10],[45,93,114,100,19],[45,94,136,94,1],[45,95,133,18,3],[45,98,100,23,6],[45,101,146,66,9],[45,103,151,2,7],[45,105,138,99,1],[45,109,148,58,2],[45,111,119,114,14],[45,112,119,4,14],[45,113,137,149,6],[45,119,122,60,4],[45,123,141,92,1],[45,136,140,118,2],[45,144,155,58,4],[46,47,132,89,19],[46,48,100,43,5],[46,49,152,14,10],[46,53,105,91,1],[46,54,88,106,2],[46,56,139,114,8],[46,58,84,95,3],[46,58,130,57,3],[46,58,139,64,5],[46,58,148,135,5],[46,60,93,17,4],[46,60,147,109,2],[46,65,71,146,15],[46,65,147,3,1],[46,66,152,143,16],[46,68,127,61,2],[46,68,139,25,4],[46,69,116,59,18],[46,73,137,133,8],[46,76,152,48,5],[46,77,104,124,8],[46,77,113,70,8],[46,77,142,81,8],[46,79,107,85,7],[46,79,120,130,11],[46,81,119,153,15],[46,81,137,105,1],[46,85,99,10,6],[46,86,121,105,2],[46,87,102,152,17],[46,87,108,4,9],[46,88,99,138,7],[46,88,103,91,1],[46,91,141,59,1],[46,95,141,128,1],[46,95,143,77,5],[46,97,114,89,10],[46,98,131,148,14],[46,98,139,105,1],[46,100,110,149,16],[46,103,124,97,13],[46,103,131,13,11],[46,103,139,64,11],[46,104,135,77,5],[46,105,130,10,1],[46,107,129,5,9],[46,108,131,144,6],[46,108,156,112,8],[46,110,121,17,16],[46,116,139,27,11],[46,117,150,148,15],[46,120,125,101,8],[46,122,126,64,13],[46,122,154,144,9],[46,148,153,126,13],[47,48,112,86,5],[47,48,113,24,3],[47,48,143,19,5],[47,50,104,105,1],[47,55,100,78,14],[47,57,96,56,10],[47,57,99,84,20],[47,58,68,2,3],[47,59,94,150,13],[47,60,91,101,1],[47,60,155,3,1],[47,63,71,9,2],[47,64,68,84,2],[47,64,100,119,14],[47,64,137,79,6],[47,65,70,138,7],[47,65,145,81,20],[47,66,116,18,13],[47,68,146,45,3],[47,70,115,43,1],[47,71,82,103,12],[47,74,84,3,1],[47,74,128,75,9],[47,75,91,121,1],[47,75,130,53,19],[47,80,96,59,9],[47,82,135,131,5],[47,83,108,42,9],[47,85,109,70,1],[47,90,137,72,1],[47,90,154,83,2],[47,91,138,137,1],[47,92,141,145,1],[47,93,110,113,17],[47,93,125,86,17],[47,96,139,146,8],[47,97,140,0,1],[47,97,156,5,10],[47,103,139,147,8],[47,104,119,128,15],[47,105,135,102,1],[47,107,117,22,8],[47,109,124,71,2],[47,111,119,117,10],[47,112,131,127,20],[47,113,122,131,16],[47,113,123,154,10],[47,114,135,49,6],[47,116,136,154,2],[47,117,119,126,10],[47,117,154,59,13],[47,127,133,149,5],[47,129,133,47,6],[48,49,80,82,5],[48,50,102,113,1],[48,52,127,93,5],[48,53,131,150,5],[48,54,91,39,1],[48,54,105,41,2],[48,56,143,81,5],[48,57,78,149,5],[48,57,146,144,6],[48,60,65,35,3],[48,60,141,139,3],[48,61,103,128,9],[48,62,64,1,2],[48,63,141,57,1],[48,66,81,37,5],[48,66,119,115,1],[48,67,142,100,5],[48,69,113,68,2],[48,73,134,73,16],[48,75,82,89,5],[48,84,150,67,5],[48,85,123,106,5],[48,85,154,117,5],[48,87,105,6,2],[48,87,120,76,5],[48,89,133,66,5],[48,90,117,64,3],[48,93,109,114,2],[48,93,141,83,1],[48,94,128,145,5],[48,94,152,46,5],[48,97,130,58,3],[48,99,119,130,5],[48,99,134,141,1],[48,100,147,48,5],[48,101,138,107,15],[48,105,110,102,1],[48,105,126,6,2],[48,105,146,83,2],[48,107,117,56,6],[48,109,117,72,1],[48,110,111,109,1],[48,110,125,104,5],[48,116,146,147,8],[48,122,128,64,8],[48,123,133,28,6],[48,124,146,143,7],[48,127,147,36,5],[48,128,132,151,7],[48,131,134,133,5],[48,131,143,136,1],[48,132,149,20,7],[48,133,150,108,7],[48,136,138,34,2],[48,138,152,148,7],[49,52,143,1,3],[49,53,90,65,1],[49,54,156,20,2],[49,56,132,38,18],[49,57,64,60,4],[49,57,99,64,14],[49,58,144,24,4],[49,59,131,146,13],[49,60,76,28,2],[49,62,76,132,19],[49,62,123,38,11],[49,62,145,67,8],[49,64,81,117,11],[49,64,103,10,7],[49,64,114,131,13],[49,68,78,126,2],[49,69,146,35,14],[49,70,155,69,7],[49,71,92,114,9],[49,71,103,39,14],[49,73,124,58,4],[49,74,111,136,1],[49,74,134,125,5],[49,76,114,2,6],[49,77,85,53,8],[49,78,84,146,15],[49,78,150,146,16],[49,79,112,131,20],[49,79,154,32,5],[49,80,116,84,13],[49,81,123,85,10],[49,81,125,46,16],[49,82,97,106,7],[49,83,147,25,4],[49,86,114,72,20],[49,86,126,94,8],[49,91,145,41,1],[49,97,153,47,10],[49,99,146,0,1],[49,101,102,147,9],[49,101,149,57,10],[49,106,129,134,6],[49,106,135,145,5],[49,106,154,58,4],[49,113,123,73,8],[49,113,124,138,7],[49,120,143,92,9],[49,120,155,26,4],[49,123,136,99,1],[49,124,134,3,1],[49,127,143,135,5],[49,127,151,57,15],[49,132,150,154,18],[49,146,154,14,10],[50,51,73,58,1],[50,56,78,55,1],[50,56,93,110,1],[50,58,140,40,1],[50,58,147,53,1],[50,59,140,82,1],[50,61,97,31,1],[50,61,106,119,1],[50,61,150,31,1],[50,62,103,47,1],[50,62,124,128,1],[50,63,78,39,1],[50,64,121,13,1],[50,65,84,126,1],[50,65,148,76,1],[50,66,96,41,1],[50,66,108,145,1],[50,66,149,101,1],[50,67,88,87,1],[50,67,120,140,1],[50,68,153,86,1],[50,72,75,46,1],[50,72,86,48,1],[50,72,109,10,1],[50,72,139,85,1],[50,75,143,105,1],[50,76,87,113,1],[50,77,83,52,1],[50,79,101,122,1],[50,79,103,93,1],[50,80,101,9,1],[50,81,138,47,1],[50,81,145,74,1],[50,86,148,142,1],[50,87,90,130,1],[50,89,140,95,1],[50,91,135,16,1],[50,92,148,18,1],[50,96,125,44,1],[50,96,133,57,1],[50,97,108,141,2],[50,97,139,26,2],[50,98,124,131,1],[50,100,123,127,1],[50,103,153,120,1],[50,103,154,4,1],[50,104,154,46,1],[50,105,132,20,1],[50,105,150,3,1],[50,106,123,75,1],[50,106,155,100,1],[50,109,123,151,1],[50,112,139,154,1],[50,114,154,122,1],[50,122,147,77,1],[50,122,153,68,1],[50,141,154,116,1],[51,53,137,80,6],[51,56,61,79,13],[51,56,70,96,8],[51,56,86,134,5],[51,56,143,21,14],[51,57,111,14,8],[51,58,80,46,3],[51,58,86,140,3],[51,59,97,119,8],[51,59,153,17,13],[51,60,62,73,3],[51,61,93,54,1],[51,61,130,20,5],[51,62,116,55,13],[51,65,96,76,8],[51,66,107,99,7],[51,70,74,145,8],[51,70,90,24,1],[51,70,128,147,14],[51,70,142,139,7],[51,73,116,111,8],[51,74,154,11,8],[51,76,113,6,15],[51,77,90,147,1],[51,78,117,7,10],[51,78,122,48,5],[51,78,133,7,5],[51,78,146,134,5],[51,82,119,94,14],[51,85,100,64,13],[51,85,144,4,6],[51,89,133,56,5],[51,90,110,111,1],[51,94,109,11,1],[51,97,143,1,2],[51,100,106,18,7],[51,100,148,103,11],[51,101,151,119,8],[51,102,114,124,13],[51,104,119,90,1],[51,111,135,135,5],[51,112,155,61,6],[51,114,126,144,6],[51,116,145,105,1],[51,119,147,90,1],[51,122,145,57,16],[51,123,131,110,10],[51,123,150,70,10],[51,124,128,156,13],[51,127,149,62,17],[51,136,151,75,1],[51,145,149,20,5],[51,146,150,90,1],[51,147,156,30,1],[52,53,93,144,8],[52,53,143,111,17],[52,55,56,108,9],[52,55,104,156,14],[52,55,140,101,12],[52,55,148,82,14],[52,56,131,148,14],[52,57,65,93,18],[52,58,91,68,1],[52,60,98,101,3],[52,60,114,94,3],[52,63,143,41,18],[52,64,145,8,8],[52,65,111,2,6],[52,67,72,98,8],[52,67,102,72,8],[52,68,114,3,1],[52,72,75,89,16],[52,73,122,90,2],[52,74,141,129,1],[52,77,104,131,8],[52,78,145,101,8],[52,80,105,1,1],[52,82,105,60,1],[52,85,112,52,17],[52,85,133,122,5],[52,86,107,147,8],[52,86,136,77,2],[52,87,91,127,1],[52,87,100,46,17],[52,87,152,46,19],[52,88,114,69,14],[52,89,95,142,5],[52,90,96,124,2],[52,91,145,48,1],[52,92,124,123,11],[52,92,154,22,11],[52,94,129,108,8],[52,95,154,106,5],[52,98,115,15,1],[52,100,117,73,8],[52,100,137,127,6],[52,104,120,151,11],[52,104,129,9,1],[52,106,110,41,7],[52,110,135,24,3],[52,111,124,13,13],[52,114,137,15,6],[52,114,153,90,2],[52,117,153,13,15],[52,121,147,24,4],[52,126,130,92,8],[52,128,147,77,11],[52,131,138,20,5],[52,137,142,60,5],[52,138,155,115,1],[52,139,156,10,8],[53,54,105,128,2],[53,54,151,39,2],[53,55,61,21,18],[53,56,94,145,17],[53,56,111,81,17],[53,58,61,109,2],[53,58,105,125,1],[53,59,88,2,8],[53,59,122,93,18],[53,60,156,81,3],[53,61,151,13,18],[53,62,90,100,1],[53,62,109,84,1],[53,65,92,100,8],[53,66,92,63,8],[53,68,86,103,3],[53,70,101,125,8],[53,70,108,39,9],[53,70,149,72,17],[53,71,75,95,4],[53,73,125,151,8],[53,77,141,7,1],[53,77,156,32,7],[53,79,125,153,15],[53,80,141,70,1],[53,87,125,17,17],[53,87,142,110,15],[53,89,104,90,1],[53,90,153,0,1],[53,94,107,103,7],[53,94,149,155,6],[53,100,141,148,1],[53,101,154,12,8],[53,103,148,81,12],[53,107,141,135,1],[53,111,143,99,17],[53,116,143,89,18],[53,116,144,71,7],[53,117,151,156,15],[53,123,141,93,1],[53,123,144,125,6],[53,123,146,10,8],[53,126,144,28,3],[53,132,151,39,20],[53,135,146,45,6],[54,55,70,81,1],[54,58,131,19,1],[54,62,69,42,1],[54,65,132,140,1],[54,68,117,3,2],[54,69,152,107,2],[54,71,144,119,2],[54,73,132,149,2],[54,75,128,153,2],[54,76,128,149,1],[54,76,156,83,1],[54,77,137,130,1],[54,78,124,27,1],[54,80,120,75,1],[54,82,122,137,1],[54,82,152,38,1],[54,83,87,48,2],[54,86,113,125,1],[54,89,96,113,1],[54,89,135,17,2],[54,90,120,150,2],[54,90,129,9,3],[54,91,107,117,2],[54,97,134,74,5],[54,99,130,118,1],[54,105,112,119,1],[54,107,123,141,2],[54,108,148,87,2],[54,115,120,84,1],[54,119,154,1,2],[54,124,137,78,1],[54,142,148,150,2],[55,56,104,77,8],[55,57,76,145,13],[55,59,80,63,14],[55,60,121,97,4],[55,60,138,18,3],[55,60,149,14,5],[55,62,78,30,1],[55,65,106,12,6],[55,65,150,54,1],[55,67,98,9,1],[55,71,88,6,16],[55,72,108,17,8],[55,73,103,37,10],[55,73,119,81,8],[55,75,103,28,3],[55,75,126,143,11],[55,78,100,50,1],[55,79,105,59,1],[55,80,104,144,6],[55,80,150,87,14],[55,82,144,70,6],[55,88,99,11,14],[55,88,112,133,5],[55,89,119,21,20],[55,89,133,58,5],[55,89,137,39,9],[55,91,128,47,1],[55,93,133,123,7],[55,96,123,7,12],[55,97,149,74,11],[55,101,148,56,10],[55,103,140,40,15],[55,104,150,3,1],[55,105,154,24,2],[55,106,139,44,9],[55,109,128,98,1],[55,111,125,145,13],[55,113,124,0,1],[55,114,137,108,7],[55,114,146,141,1],[55,114,149,70,16],[55,115,119,131,1],[55,116,153,72,13],[55,117,122,62,11],[55,121,130,66,13],[55,121,154,98,14],[55,122,132,117,14],[55,127,133,84,5],[55,130,147,93,14],[55,133,139,67,10],[55,136,139,68,3],[56,57,91,149,1],[56,58,108,123,4],[56,59,88,66,13],[56,60,104,106,3],[56,60,117,9,2],[56,61,116,89,15],[56,62,68,75,2],[56,62,122,48,5],[56,63,104,93,14],[56,66,129,65,13],[56,66,156,146,13],[56,70,91,121,1],[56,70,115,43,1],[56,71,143,16,13],[56,73,104,10,6],[56,73,115,123,1],[56,73,123,122,9],[56,74,85,150,8],[56,74,154,69,9],[56,75,76,34,20],[56,76,144,98,6],[56,77,91,92,1],[56,77,149,79,8],[56,78,130,105,1],[56,85,100,16,11],[56,87,102,106,7],[56,91,133,33,1],[56,100,127,147,14],[56,101,109,127,1],[56,102,114,63,14],[56,103,116,125,11],[56,103,129,106,8],[56,103,145,35,11],[56,106,125,88,7],[56,107,126,78,7],[56,107,144,113,6],[56,108,129,57,9],[56,109,110,44,1],[56,109,113,44,1],[56,127,148,137,6],[56,131,135,102,5],[56,134,137,75,6],[56,138,152,144,7],[56,141,151,70,1],[56,144,150,63,7],[57,59,89,94,13],[57,60,87,15,3],[57,60,138,36,4],[57,62,71,90,1],[57,65,66,26,3],[57,65,74,39,8],[57,65,119,44,14],[57,67,101,15,8],[57,69,151,4,13],[57,69,155,79,6],[57,70,99,33,1],[57,70,137,127,6],[57,71,121,58,4],[57,71,129,37,15],[57,72,101,153,8],[57,73,85,103,8],[57,74,97,73,9],[57,76,114,71,22],[57,76,123,56,11],[57,77,137,131,6],[57,77,150,86,9],[57,78,106,60,3],[57,79,128,16,12],[57,81,100,29,19],[57,81,136,156,1],[57,84,90,39,1],[57,85,122,125,16],[57,87,117,43,12],[57,88,112,9,1],[57,89,151,78,16],[57,90,151,6,2],[57,92,126,40,9],[57,93,94,3,1],[57,102,126,144,6],[57,102,145,62,21],[57,105,134,10,2],[57,107,145,3,1],[57,116,119,75,15],[57,116,156,86,15],[57,120,123,89,12],[57,125,141,82,1],[57,129,141,25,1],[57,131,136,146,1],[57,137,143,148,7],[57,146,147,140,16],[58,59,128,96,6],[58,59,155,73,6],[58,60,109,95,12],[58,62,125,50,1],[58,62,148,43,3],[58,63,74,101,6],[58,63,88,146,5],[58,63,102,73,3],[58,63,125,152,3],[58,64,92,51,3],[58,67,96,34,4],[58,67,146,80,3],[58,68,82,96,2],[58,69,107,83,7],[58,70,146,121,4],[58,75,155,102,3],[58,76,140,8,3],[58,78,146,26,3],[58,79,83,79,3],[58,79,113,121,3],[58,81,137,71,3],[58,83,145,112,3],[58,85,126,148,3],[58,86,87,61,4],[58,86,119,27,4],[58,86,126,153,4],[58,88,133,43,5],[58,93,148,40,4],[58,94,134,32,3],[58,98,106,35,3],[58,99,106,15,3],[58,103,126,1,5],[58,105,130,97,1],[58,106,136,151,2],[58,108,116,64,6],[58,109,148,87,2],[58,113,124,74,3],[58,113,140,110,3],[58,114,121,40,4],[58,123,125,18,3],[58,125,143,96,3],[58,133,136,82,1],[58,142,143,153,5],[58,149,153,46,5],[59,62,70,124,13],[59,64,75,53,17],[59,64,106,7,10],[59,66,100,113,13],[59,66,151,80,13],[59,67,95,71,4],[59,68,135,129,5],[59,71,142,110,13],[59,72,156,56,13],[59,74,127,61,8],[59,75,122,130,14],[59,76,88,93,14],[59,80,124,13,14],[59,81,147,38,14],[59,82,84,83,8],[59,83,106,109,3],[59,83,143,66,8],[59,85,124,80,13],[59,86,138,16,8],[59,91,143,62,1],[59,95,115,8,1],[59,95,117,63,6],[59,96,133,148,9],[59,97,128,98,9],[59,98,108,146,8],[59,99,134,122,5],[59,100,102,104,14],[59,100,112,123,10],[59,100,121,97,9],[59,100,153,75,14],[59,103,130,105,1],[59,103,147,9,3],[59,104,131,47,13],[59,113,122,32,5],[59,114,117,30,2],[59,115,143,21,1],[59,121,140,61,16],[59,124,132,40,17],[59,125,133,89,5],[59,125,139,109,1],[59,126,136,63,3],[59,127,149,47,13],[59,134,136,127,1],[59,136,141,57,1],[59,138,148,154,11],[59,144,151,87,7],[59,147,155,145,6],[60,62,114,69,3],[60,62,142,19,3],[60,63,79,26,3],[60,64,141,87,1],[60,65,67,44,3],[60,65,149,138,3],[60,66,93,76,3],[60,68,106,27,4],[60,71,153,2,4],[60,72,78,133,3],[60,74,146,50,1],[60,76,81,74,3],[60,76,129,85,3],[60,77,149,71,4],[60,79,86,148,3],[60,84,112,137,3],[60,84,138,122,3],[60,85,144,115,1],[60,86,90,94,1],[60,90,109,22,3],[60,90,136,83,5],[60,91,137,153,1],[60,95,111,10,3],[60,98,121,20,3],[60,99,107,137,3],[60,99,111,18,3],[60,99,114,20,3],[60,100,150,17,3],[60,104,125,116,3],[60,105,107,49,2],[60,105,152,135,2],[60,107,120,135,8],[60,108,137,59,6],[60,109,123,8,4],[60,111,117,154,3],[60,112,143,27,3],[60,113,116,138,3],[60,125,135,1,2],[60,136,142,114,2],[60,138,155,139,12],[61,65,81,87,13],[61,68,127,98,2],[61,69,93,68,3],[61,71,115,54,1],[61,72,151,43,13],[61,73,103,43,11],[61,73,145,0,1],[61,74,125,39,8],[61,75,150,36,17],[61,76,83,98,8],[61,77,144,148,10],[61,82,136,118,1],[61,82,138,139,7],[61,83,146,70,9],[61,84,119,154,13],[61,86,105,6,2],[61,87,139,75,8],[61,87,143,123,13],[61,90,109,80,1],[61,94,132,16,11],[61,95,96,82,3],[61,97,113,8,8],[61,98,143,144,6],[61,99,128,74,8],[61,100,104,13,14],[61,102,110,103,11],[61,103,137,93,8],[61,103,153,44,14],[61,106,132,113,7],[61,110,111,65,13],[61,111,137,47,6],[61,113,145,144,6],[61,115,140,60,1],[61,120,156,94,10],[61,124,149,76,14],[61,133,146,81,5],[61,144,149,50,1],[62,63,112,150,13],[62,64,82,91,1],[62,73,116,127,8],[62,73,132,153,8],[62,74,88,50,1],[62,75,123,45,11],[62,77,132,52,8],[62,79,112,27,14],[62,79,121,48,5],[62,79,137,147,6],[62,81,141,122,1],[62,82,114,78,21],[62,84,136,92,1],[62,88,100,31,14],[62,88,131,130,14],[62,90,100,46,1],[62,90,151,51,1],[62,91,134,89,1],[62,92,107,80,7],[62,93,132,51,17],[62,95,99,85,3],[62,95,148,51,3],[62,96,147,154,9],[62,100,108,85,8],[62,104,152,147,14],[62,110,147,74,8],[62,115,156,132,1],[62,124,148,1,2],[62,127,132,155,6],[62,130,145,101,8],[62,136,151,23,1],[62,144,153,102,6],[63,64,119,6,22],[63,64,134,60,6],[63,65,143,51,13],[63,66,94,66,13],[63,66,121,120,11],[63,67,82,47,8],[63,67,108,37,10],[63,68,98,91,1],[63,69,115,124,1],[63,71,93,99,14],[63,73,109,92,3],[63,74,101,149,11],[63,74,123,12,11],[63,75,80,145,13],[63,77,139,49,8],[63,82,125,91,1],[63,82,139,118,1],[63,83,99,6,8],[63,85,104,72,13],[63,87,129,154,16],[63,90,136,75,2],[63,92,104,12,6],[63,93,145,130,13],[63,94,96,65,8],[63,94,155,33,1],[63,96,113,111,8],[63,97,108,44,10],[63,97,121,97,10],[63,99,121,17,14],[63,102,109,125,1],[63,102,152,87,14],[63,105,124,133,3],[63,106,149,66,7],[63,108,120,98,8],[63,108,122,152,11],[63,109,150,38,2],[63,115,148,116,1],[63,120,130,32,5],[63,122,129,5,17],[63,126,134,56,6],[63,136,150,0,1],[63,138,156,9,3],[63,146,154,151,22],[64,66,100,72,13],[64,66,121,87,13],[64,67,73,47,9],[64,67,75,93,10],[64,67,116,27,14],[64,68,87,15,2],[64,69,132,139,9],[64,72,149,87,13],[64,73,92,22,14],[64,76,149,77,8],[64,77,79,113,8],[64,79,129,124,14],[64,79,150,94,13],[64,81,84,17,13],[64,82,99,104,14],[64,84,128,11,13],[64,88,137,109,3],[64,89,129,67,12],[64,94,132,84,13],[64,101,136,49,2],[64,102,113,35,13],[64,103,139,16,13],[64,113,135,118,1],[64,113,141,19,1],[64,116,117,61,20],[64,123,133,37,7],[64,127,134,111,5],[64,128,129,72,13],[64,129,147,104,14],[64,143,148,151,18],[64,151,155,20,8],[65,66,74,114,8],[65,66,132,27,14],[65,67,125,24,3],[65,67,127,154,8],[65,67,133,58,3],[65,68,93,20,2],[65,69,124,69,12],[65,70,125,136,1],[65,72,148,12,6],[65,74,143,30,1],[65,75,148,149,14],[65,77,81,91,1],[65,77,137,83,6],[65,79,86,135,5],[65,81,98,18,23],[65,86,153,125,15],[65,89,97,84,8],[65,89,113,60,3],[65,89,139,44,7],[65,89,149,67,8],[65,94,141,5,1],[65,97,101,140,9],[65,101,102,110,8],[65,102,156,12,6],[65,103,106,85,7],[65,106,123,7,7],[65,106,140,99,7],[65,110,141,57,1],[65,110,147,130,14],[65,112,152,67,8],[65,114,142,23,6],[65,115,116,143,1],[65,115,126,2,1],[65,120,152,57,11],[65,127,150,152,18],[65,130,147,11,14],[65,136,143,134,1],[65,145,153,78,15],[66,70,72,33,1],[66,70,156,139,7],[66,73,90,148,1],[66,74,85,8,8],[66,74,125,111,8],[66,74,156,72,8],[66,77,94,119,8],[66,78,138,87,7],[66,78,154,11,15],[66,80,130,90,1],[66,81,108,142,8],[66,82,88,55,13],[66,87,126,137,6],[66,89,142,3,1],[66,91,112,22,1],[66,91,134,83,1],[66,92,130,130,8],[66,94,111,87,20],[66,94,115,37,1],[66,95,124,0,1],[66,96,123,68,2],[66,100,114,31,20],[66,100,120,104,11],[66,100,121,78,21],[66,101,146,1,2],[66,102,105,73,1],[66,105,140,66,1],[66,111,127,84,24],[66,113,121,23,6],[66,125,131,51,24],[66,128,137,10,6],[66,131,132,29,17],[66,131,134,105,1],[66,132,138,22,7],[66,140,155,79,6],[67,68,144,92,8],[67,69,97,119,14],[67,70,116,9,2],[67,71,77,105,2],[67,71,103,143,9],[67,73,139,0,2],[67,79,132,114,8],[67,83,88,141,1],[67,84,104,92,8],[67,84,115,69,1],[67,87,99,148,8],[67,88,98,130,8],[67,88,107,140,11],[67,91,115,65,1],[67,99,134,94,5],[67,104,119,91,1],[67,105,107,149,2],[67,105,112,72,1],[67,106,152,119,10],[67,108,129,148,14],[67,108,147,92,14],[67,112,139,48,5],[67,116,134,121,6],[67,116,145,132,8],[67,120,124,114,9],[67,125,135,83,5],[67,129,143,130,8],[67,136,147,112,1],[67,139,153,58,5],[67,148,154,14,13],[68,69,110,120,2],[68,70,73,121,3],[68,73,102,26,2],[68,74,89,125,2],[68,74,96,53,3],[68,77,78,96,2],[68,78,137,40,2],[68,80,122,57,2],[68,84,92,101,2],[68,85,111,125,2],[68,85,151,122,2],[68,87,144,69,3],[68,87,154,31,2],[68,88,115,32,1],[68,89,145,142,2],[68,92,111,10,2],[68,92,133,102,2],[68,92,152,115,1],[68,93,126,154,3],[68,94,145,143,2],[68,96,127,113,2],[68,98,103,61,2],[68,105,116,35,2],[68,107,131,139,2],[68,111,144,144,2],[68,116,144,144,5],[68,119,135,42,3],[68,132,143,103,3],[68,138,140,95,4],[68,153,156,34,3],[69,70,141,70,1],[69,71,72,106,7],[69,71,143,54,2],[69,73,119,92,14],[69,75,131,150,11],[69,77,136,12,3],[69,77,149,50,1],[69,79,85,98,11],[69,79,93,130,12],[69,82,141,79,1],[69,85,100,75,11],[69,86,88,15,11],[69,86,117,27,13],[69,86,131,65,11],[69,90,138,72,1],[69,92,111,73,8],[69,92,116,97,15],[69,92,123,23,13],[69,98,151,136,1],[69,99,111,68,2],[69,99,141,77,1],[69,102,136,137,1],[69,103,136,98,1],[69,106,123,152,10],[69,107,151,20,8],[69,108,110,63,8],[69,108,140,74,12],[69,111,149,27,11],[69,117,136,10,3],[69,120,155,115,1],[69,120,156,33,1],[69,125,140,58,3],[69,129,133,136,3],[69,129,147,120,18],[69,135,146,104,5],[69,136,155,141,1],[69,143,144,47,7],[70,71,90,126,2],[70,72,128,121,14],[70,72,131,20,5],[70,73,115,107,1],[70,74,78,91,1],[70,76,122,92,8],[70,76,141,151,1],[70,77,128,5,9],[70,78,131,127,20],[70,78,152,21,15],[70,86,130,64,14],[70,88,108,82,8],[70,88,119,73,9],[70,90,148,83,2],[70,91,97,83,1],[70,94,98,2,6],[70,95,104,64,3],[70,95,111,46,3],[70,96,139,92,8],[70,97,131,100,8],[70,99,116,115,1],[70,99,151,37,16],[70,102,122,30,1],[70,107,123,110,7],[70,110,127,8,8],[70,112,127,86,20],[70,116,131,85,13],[70,116,136,139,2],[70,121,155,147,7],[70,122,151,111,15],[70,130,143,6,16],[70,135,145,23,5],[70,136,142,152,2],[70,146,153,102,16],[70,150,152,137,7],[71,72,148,46,14],[71,73,94,154,8],[71,73,117,81,8],[71,76,139,122,7],[71,78,88,13,15],[71,79,139,44,7],[71,80,88,130,15],[71,80,98,127,21],[71,80,115,53,1],[71,82,93,75,19],[71,83,94,59,8],[71,84,119,137,6],[71,85,87,119,14],[71,86,94,0,1],[71,86,127,62,21],[71,86,139,141,1],[71,88,109,95,2],[71,90,104,73,1],[71,91,122,37,1],[71,92,114,74,9],[71,94,150,113,17],[71,95,106,69,4],[71,99,130,42,19],[71,101,129,4,10],[71,102,122,8,8],[71,103,144,15,6],[71,104,129,23,6],[71,107,138,111,7],[71,107,142,84,7],[71,107,149,136,2],[71,108,117,81,8],[71,120,127,110,10],[71,134,155,134,6],[71,138,140,114,8],[72,73,136,23,1],[72,74,151,94,8],[72,76,122,81,16],[72,77,130,136,1],[72,78,127,143,17],[72,80,129,17,13],[72,80,136,119,1],[72,82,154,31,15],[72,83,86,15,8],[72,83,93,88,8],[72,83,121,73,8],[72,84,91,144,1],[72,91,95,98,1],[72,91,106,140,1],[72,92,140,84,8],[72,93,95,59,3],[72,94,96,33,1],[72,94,117,19,10],[72,95,147,22,3],[72,96,132,52,8],[72,101,144,112,6],[72,102,124,68,2],[72,103,111,120,10],[72,104,106,140,7],[72,105,130,72,1],[72,106,139,35,7],[72,108,109,78,1],[72,108,114,10,6],[72,115,137,146,1],[72,124,131,153,13],[72,128,145,50,1],[72,130,132,82,18],[72,133,147,136,1],[72,134,152,117,5],[72,135,148,56,5],[72,138,152,61,7],[72,143,146,151,15],[73,75,105,4,2],[73,76,114,112,8],[73,76,145,18,8],[73,77,110,64,8],[73,82,145,137,6],[73,83,116,12,11],[73,84,111,50,1],[73,86,98,40,8],[73,86,146,136,2],[73,87,89,87,9],[73,87,144,65,6],[73,90,108,148,3],[73,90,146,106,2],[73,91,125,20,1],[73,92,121,34,9],[73,94,98,52,8],[73,94,108,0,1],[73,94,109,151,1],[73,94,137,113,6],[73,97,108,87,9],[73,100,156,121,8],[73,102,115,78,1],[73,106,151,24,5],[73,113,127,150,8],[73,113,148,101,8],[73,125,128,94,8],[73,136,150,98,1],[74,76,109,70,1],[74,76,122,136,1],[74,78,85,116,8],[74,78,136,107,1],[74,80,139,137,6],[74,82,117,25,3],[74,82,122,75,8],[74,83,124,69,15],[74,83,131,9,1],[74,87,96,156,9],[74,88,123,76,8],[74,91,116,26,1],[74,91,156,15,1],[74,93,124,22,11],[74,94,98,109,1],[74,94,138,70,7],[74,94,154,137,6],[74,96,152,70,9],[74,101,126,37,10],[74,103,146,80,8],[74,104,127,139,7],[74,106,114,84,7],[74,109,155,129,3],[74,110,115,22,1],[74,117,153,77,13],[74,119,154,151,13],[74,121,123,139,8],[74,138,148,71,8],[75,76,137,15,6],[75,77,89,34,10],[75,77,110,19,8],[75,81,98,30,1],[75,81,105,89,1],[75,81,106,63,7],[75,82,148,108,8],[75,85,97,76,8],[75,87,112,36,16],[75,88,114,115,1],[75,88,116,115,1],[75,90,138,106,2],[75,91,132,19,1],[75,93,115,81,1],[75,95,150,49,4],[75,96,114,96,10],[75,97,132,28,3],[75,103,147,105,2],[75,105,120,155,2],[75,106,109,107,2],[75,106,128,68,3],[75,108,147,84,8],[75,116,151,3,1],[75,117,129,71,13],[75,119,128,145,14],[75,123,129,103,14],[75,123,144,121,7],[75,124,150,13,17],[75,126,143,108,10],[75,139,142,151,9],[75,139,150,12,8],[76,77,85,69,8],[76,78,150,150,19],[76,79,87,2,6],[76,79,153,8,8],[76,80,115,39,1],[76,80,138,28,2],[76,82,87,63,14],[76,83,141,120,1],[76,83,152,9,1],[76,88,152,84,14],[76,90,138,93,1],[76,91,112,138,1],[76,94,148,85,14],[76,96,112,86,8],[76,97,107,113,7],[76,97,132,134,5],[76,102,133,129,5],[76,106,108,112,7],[76,109,127,16,1],[76,112,113,152,17],[76,113,115,67,1],[76,113,122,90,1],[76,113,152,128,14],[76,116,121,61,14],[76,122,147,119,15],[76,125,154,41,15],[76,132,152,113,17],[76,139,142,16,7],[76,139,154,33,1],[76,145,154,64,13],[77,78,82,92,8],[77,80,153,61,8],[77,82,94,35,8],[77,83,110,139,7],[77,87,136,39,2],[77,88,140,6,12],[77,89,155,86,7],[77,91,121,74,1],[77,92,135,152,7],[77,99,136,57,1],[77,103,127,27,8],[77,105,113,54,1],[77,105,130,81,1],[77,105,135,123,4],[77,108,133,41,7],[77,112,142,28,2],[77,112,145,143,8],[77,112,147,78,8],[77,113,124,143,8],[77,115,126,55,1],[77,119,151,30,2],[77,127,153,41,8],[77,142,144,140,9],[77,150,152,80,8],[78,79,146,85,15],[78,84,137,28,2],[78,89,92,129,8],[78,98,106,41,7],[78,98,154,15,15],[78,99,112,111,23],[78,102,106,8,7],[78,102,132,48,5],[78,104,109,15,1],[78,106,149,7,7],[78,109,134,91,1],[78,112,122,44,16],[78,112,133,85,5],[78,112,146,128,14],[78,114,155,126,6],[78,120,132,98,11],[78,123,124,34,11],[78,126,152,62,9],[78,127,154,79,15],[78,128,148,95,3],[78,131,138,75,7],[78,133,145,47,5],[78,133,152,0,1],[78,135,147,105,1],[78,138,139,71,7],[78,138,149,58,3],[79,81,98,146,16],[79,82,98,96,9],[79,84,111,39,16],[79,85,112,129,13],[79,85,116,122,13],[79,85,139,153,7],[79,85,143,97,8],[79,87,92,67,8],[79,89,142,105,1],[79,93,121,128,15],[79,95,106,108,3],[79,97,117,6,9],[79,100,136,34,1],[79,101,142,92,8],[79,103,127,47,12],[79,105,110,38,1],[79,105,121,11,1],[79,111,152,153,15],[79,113,148,34,14],[79,117,146,99,11],[79,121,147,133,5],[79,123,125,54,1],[79,126,143,61,9],[79,126,153,70,9],[79,129,153,119,14],[79,137,138,151,6],[79,138,145,69,7],[79,151,152,80,16],[80,82,98,133,5],[80,85,91,100,1],[80,85,127,83,8],[80,86,129,0,1],[80,88,102,74,8],[80,88,109,156,1],[80,88,117,27,11],[80,90,150,102,1],[80,90,154,0,1],[80,91,144,108,1],[80,92,94,90,1],[80,92,109,97,1],[80,94,113,29,17],[80,99,100,93,19],[80,99,153,138,7],[80,100,121,49,22],[80,102,116,144,6],[80,103,142,6,12],[80,104,117,82,11],[80,106,107,97,7],[80,106,111,64,7],[80,107,116,26,3],[80,107,135,61,5],[80,111,117,141,1],[80,111,143,82,17],[80,114,143,12,6],[80,117,131,42,10],[80,117,136,36,1],[80,119,124,40,14],[80,119,141,117,1],[80,124,141,144,1],[80,125,146,150,15],[80,126,134,91,1],[80,126,146,153,9],[80,130,143,27,15],[80,132,152,104,19],[80,152,155,28,2],[81,82,95,48,3],[81,82,144,72,6],[81,83,136,82,1],[81,83,155,108,6],[81,84,132,81,18],[81,89,120,35,11],[81,91,93,9,1],[81,92,146,133,5],[81,94,150,60,3],[81,96,105,155,1],[81,97,123,40,9],[81,99,116,108,8],[81,105,139,70,1],[81,107,128,144,6],[81,107,132,109,1],[81,115,151,7,1],[81,119,145,80,14],[81,120,138,57,7],[81,121,124,11,14],[81,122,127,87,16],[81,136,138,57,1],[81,141,148,82,1],[81,152,156,0,1],[82,85,155,35,6],[82,86,114,155,6],[82,92,97,52,8],[82,92,106,47,7],[82,94,122,127,16],[82,100,147,51,14],[82,100,153,103,12],[82,103,124,42,12],[82,106,123,130,7],[82,123,151,99,11],[82,126,146,24,3],[82,131,156,46,13],[82,132,151,67,8],[82,133,142,127,5],[82,138,155,95,3],[82,139,154,151,7],[82,140,156,118,1],[82,141,149,50,1],[83,84,95,5,3],[83,85,137,14,6],[83,86,109,83,2],[83,90,132,107,2],[83,91,154,72,1],[83,92,147,45,10],[83,93,102,109,1],[83,94,110,96,8],[83,94,136,70,1],[83,95,123,93,5],[83,101,104,86,8],[83,101,134,156,10],[83,103,129,131,8],[83,103,141,86,1],[83,104,114,6,8],[83,106,145,8,7],[83,108,156,25,6],[83,117,119,112,8],[83,119,140,65,8],[83,119,147,107,12],[83,128,131,74,8],[83,134,154,107,8],[83,144,147,1,4],[84,87,139,89,7],[84,88,121,111,14],[84,91,106,118,1],[84,93,106,96,7],[84,93,112,95,3],[84,93,139,21,7],[84,94,109,146,1],[84,96,106,31,7],[84,99,150,90,1],[84,104,112,66,23],[84,110,133,103,5],[84,114,138,77,7],[84,114,148,77,8],[84,115,152,64,1],[84,120,153,83,8],[84,120,155,30,1],[84,123,133,34,5],[84,132,139,30,1],[84,133,152,128,5],[84,134,135,85,5],[84,134,137,73,5],[84,141,145,83,1],[85,88,132,49,14],[85,89,143,131,16],[85,90,141,103,1],[85,91,115,82,1],[85,92,148,7,8],[85,93,106,48,5],[85,93,122,132,16],[85,95,133,50,1],[85,99,142,155,6],[85,100,127,111,23],[85,105,120,68,1],[85,105,131,52,1],[85,105,149,122,1],[85,106,121,73,7],[85,109,116,21,1],[85,109,119,50,1],[85,112,130,57,21],[85,113,143,146,15],[85,117,147,121,10],[85,121,152,20,5],[85,122,137,17,6],[85,122,144,19,6],[85,144,152,84,6],[86,87,89,32,6],[86,94,109,80,1],[86,95,152,15,3],[86,96,131,82,8],[86,98,124,12,6],[86,99,110,6,15],[86,99,152,8,8],[86,103,144,55,7],[86,106,154,6,8],[86,108,152,106,8],[86,112,142,138,7],[86,112,149,46,16],[86,119,120,29,13],[86,121,127,25,3],[86,123,129,61,13],[86,128,144,144,7],[86,142,155,6,7],[86,148,153,30,2],[87,89,133,64,6],[87,89,150,154,18],[87,91,150,32,1],[87,92,123,3,1],[87,92,155,131,6],[87,95,121,56,4],[87,97,125,108,8],[87,106,131,0,1],[87,107,108,128,8],[87,110,126,60,3],[87,112,131,152,17],[87,113,114,8,8],[87,113,151,66,15],[87,116,142,135,6],[87,117,122,11,12],[87,119,132,149,17],[87,120,148,98,11],[87,121,143,112,17],[87,124,146,98,14],[87,127,131,124,13],[87,137,156,64,7],[87,141,144,94,1],[87,146,150,25,4],[88,89,92,57,9],[88,89,112,123,10],[88,90,128,3,1],[88,92,109,34,2],[88,93,130,66,14],[88,94,132,11,14],[88,94,147,126,8],[88,96,149,145,8],[88,98,123,5,11],[88,98,139,3,1],[88,100,150,76,15],[88,102,107,87,7],[88,102,144,144,6],[88,104,116,57,14],[88,106,156,73,12],[88,108,154,20,8],[88,109,140,79,1],[88,122,126,63,13],[88,124,146,61,22],[88,132,149,100,15],[89,90,108,67,2],[89,98,135,103,5],[89,101,112,44,8],[89,106,125,151,7],[89,108,112,107,7],[89,109,141,61,1],[89,109,142,60,2],[89,110,133,63,5],[89,112,140,145,16],[89,112,151,1,2],[89,114,117,19,10],[89,120,127,40,11],[89,121,136,0,1],[89,126,132,78,9],[89,126,135,15,5],[89,134,153,117,8],[89,135,147,41,7],[89,137,139,128,9],[89,138,153,55,11],[90,91,101,90,2],[90,91,152,63,1],[90,92,113,21,1],[90,92,132,114,2],[90,93,106,31,1],[90,94,127,67,1],[90,95,122,143,2],[90,96,135,72,1],[90,98,142,149,1],[90,99,117,137,1],[90,100,126,8,1],[90,101,116,80,1],[90,105,127,151,1],[90,105,152,88,2],[90,107,111,102,1],[90,107,128,138,3],[90,107,149,82,1],[90,109,113,98,1],[90,109,124,72,1],[90,111,152,22,1],[90,113,124,133,1],[90,115,137,128,1],[90,123,139,49,2],[90,126,150,80,1],[90,133,155,91,3],[90,142,151,35,2],[91,92,150,16,1],[91,93,105,23,1],[91,95,107,156,1],[91,100,147,72,1],[91,101,105,2,2],[91,102,111,143,1],[91,102,119,125,1],[91,104,120,84,1],[91,104,130,116,1],[91,104,149,29,1],[91,105,106,79,1],[91,111,129,113,1],[91,116,117,26,1],[91,119,140,145,1],[91,128,130,134,1],[91,129,130,7,1],[91,130,147,7,1],[92,95,139,136,5],[92,97,103,68,5],[92,105,119,98,1],[92,108,141,90,2],[92,108,150,140,11],[92,109,148,104,1],[92,114,122,93,9],[92,119,147,55,14],[92,123,151,134,8],[92,123,154,39,12],[92,126,156,18,8],[92,134,155,49,6],[92,135,149,88,7],[92,142,154,39,12],[92,145,148,155,6],[93,94,126,142,8],[93,94,132,7,17],[93,95,138,74,5],[93,97,133,37,7],[93,99,123,119,11],[93,100,107,1,2],[93,103,126,20,7],[93,103,145,37,11],[93,110,149,126,8],[93,114,116,102,14],[93,117,141,105,1],[93,121,139,121,8],[93,122,140,92,11],[93,132,156,76,14],[93,140,154,99,16],[93,145,148,46,14],[93,149,156,40,17],[93,152,154,22,20],[94,95,129,51,3],[94,95,133,13,3],[94,96,145,120,8],[94,97,104,36,8],[94,99,122,42,16],[94,99,124,38,13],[94,102,119,12,6],[94,106,111,84,7],[94,106,150,93,7],[94,111,138,8,7],[94,113,116,120,10],[94,114,122,11,16],[94,114,139,47,7],[94,114,153,87,15],[94,132,153,141,1],[94,134,156,153,5],[94,142,150,28,2],[95,97,136,64,3],[95,98,131,45,3],[95,98,140,108,3],[95,99,116,142,3],[95,102,108,150,3],[95,102,140,153,3],[95,102,150,122,3],[95,106,134,35,5],[95,106,139,154,5],[95,112,142,109,1],[95,114,139,14,4],[95,114,150,26,4],[95,115,152,53,1],[95,123,139,58,8],[95,131,133,30,1],[95,136,142,36,2],[95,137,140,37,4],[95,145,151,99,3],[95,146,149,59,5],[96,97,116,142,14],[96,97,117,56,10],[96,100,101,45,9],[96,100,134,126,5],[96,100,144,151,6],[96,102,141,19,1],[96,104,107,112,7],[96,104,144,111,6],[96,107,121,24,4],[96,111,131,115,1],[96,113,137,124,6],[96,114,144,21,7],[96,117,128,107,12],[96,124,143,31,8],[96,125,133,149,5],[96,133,144,70,6],[97,101,103,49,10],[97,104,115,64,1],[97,105,115,20,2],[97,105,122,59,2],[97,107,112,0,1],[97,111,112,111,8],[97,113,124,106,7],[97,114,131,150,8],[97,114,133,132,6],[97,117,144,92,14],[97,121,141,111,1],[97,121,146,66,9],[97,125,154,43,8],[97,126,128,53,12],[97,126,133,133,15],[97,127,143,11,9],[97,130,150,13,9],[97,144,155,6,10],[98,99,129,128,14],[98,100,128,65,14],[98,101,133,145,5],[98,101,142,24,3],[98,102,107,91,1],[98,102,109,105,1],[98,102,153,102,16],[98,107,124,61,7],[98,122,147,112,14],[98,123,125,134,5],[98,130,143,151,16],[98,132,137,105,1],[98,134,146,124,5],[98,135,143,86,5],[98,145,147,85,14],[98,146,156,25,3],[99,101,133,74,5],[99,102,121,3,1],[99,105,120,21,1],[99,112,137,27,6],[99,112,154,12,6],[99,120,124,27,11],[99,122,141,1,1],[99,126,155,140,6],[99,128,139,155,6],[99,134,135,100,5],[99,136,154,113,1],[99,142,155,52,6],[99,151,155,126,6],[100,103,105,110,1],[100,103,143,55,12],[100,104,156,14,9],[100,105,114,147,1],[100,107,154,38,7],[100,108,145,4,8],[100,109,131,7,1],[100,109,134,115,1],[100,110,132,13,15],[100,111,121,42,17],[100,114,148,82,15],[100,117,133,48,5],[100,119,156,33,1],[100,120,130,101,9],[100,123,148,19,10],[100,124,132,76,14],[100,129,146,55,14],[100,133,135,155,5],[100,144,152,81,6],[101,104,107,130,7],[101,106,132,140,9],[101,112,136,43,1],[101,115,134,12,2],[101,117,120,105,4],[101,119,128,151,14],[101,119,147,58,6],[101,123,137,41,8],[101,127,131,140,8],[101,129,156,68,5],[101,133,139,153,8],[101,136,150,22,2],[101,139,143,63,10],[101,140,156,39,13],[101,151,156,22,14],[102,105,121,149,1],[102,105,147,39,1],[102,110,117,9,1],[102,110,126,11,8],[102,112,149,2,6],[102,114,154,68,2],[102,117,153,150,11],[102,120,152,12,6],[102,123,152,130,11],[102,124,140,155,6],[102,125,134,48,5],[102,125,139,82,7],[102,131,146,114,15],[102,137,147,80,6],[102,141,148,126,1],[103,104,141,143,1],[103,107,148,70,8],[103,110,155,133,5],[103,115,121,124,1],[103,115,133,65,1],[103,116,139,18,7],[103,117,153,105,2],[103,122,133,72,5],[103,122,135,47,6],[103,124,136,120,3],[103,129,152,133,7],[103,131,148,92,8],[103,134,137,78,5],[104,107,122,107,7],[104,107,128,116,7],[104,114,155,5,6],[104,116,142,56,14],[104,116,148,88,14],[104,119,131,16,11],[104,120,133,140,5],[104,122,144,111,6],[104,134,155,81,5],[104,148,156,126,9],[104,151,153,61,14],[105,109,120,67,4],[105,111,125,155,1],[105,119,143,6,2],[105,120,123,120,4],[105,120,152,92,2],[105,125,138,138,1],[105,125,139,154,1],[105,132,139,31,1],[105,134,136,134,8],[105,137,146,37,2],[105,139,140,139,2],[105,144,150,129,2],[105,145,148,5,1],[106,109,112,21,1],[106,111,138,92,7],[106,113,133,154,5],[106,115,127,107,1],[106,119,155,97,10],[106,128,134,39,8],[106,128,142,83,11],[106,131,135,113,5],[106,131,153,52,7],[106,139,149,152,10],[106,147,150,136,2],[107,111,127,112,7],[107,112,146,17,7],[107,112,148,40,7],[107,116,117,155,11],[107,120,149,2,8],[107,121,123,104,7],[107,122,146,50,1],[107,122,151,75,9],[107,126,138,52,10],[107,128,139,58,6],[107,129,135,117,10],[107,130,139,97,7],[107,153,156,26,5],[108,116,146,52,11],[108,119,120,131,8],[108,119,140,39,12],[108,123,139,51,7],[108,130,139,66,7],[108,132,148,46,10],[108,133,145,35,5],[108,138,155,133,16],[108,148,152,36,11],[109,112,135,114,1],[109,114,116,155,2],[109,114,120,50,1],[109,116,132,100,1],[109,116,154,85,1],[109,119,133,2,3],[109,122,126,81,1],[109,123,155,93,2],[109,125,132,81,1],[109,128,144,56,2],[109,131,156,140,1],[109,135,151,115,1],[109,138,148,134,3],[110,119,123,48,5],[110,119,147,12,6],[110,125,149,86,17],[110,128,153,5,14],[110,138,146,10,6],[111,113,152,153,15],[111,113,155,133,5],[111,114,129,120,10],[111,119,126,67,8],[111,120,135,127,5],[111,125,144,80,6],[111,127,136,155,1],[111,130,150,24,3],[111,149,156,120,10],[112,115,142,116,1],[112,117,153,37,10],[112,119,137,98,6],[112,120,150,155,6],[112,127,128,22,14],[112,128,146,115,1],[112,132,137,137,6],[112,139,140,124,7],[112,139,149,58,3],[113,114,148,18,14],[113,115,148,72,1],[113,121,135,8,5],[113,123,155,68,2],[113,127,155,98,6],[113,140,153,29,15],[113,141,153,56,1],[113,142,156,33,1],[113,155,156,10,6],[114,115,136,61,1],[114,121,123,105,2],[114,125,129,6,13],[114,127,138,52,7],[114,129,133,73,6],[114,138,141,127,1],[115,116,152,71,1],[115,119,142,17,1],[115,127,129,133,1],[115,131,141,125,1],[115,131,144,59,1],[115,133,149,30,1],[115,146,148,21,1],[115,146,153,60,1],[116,117,136,64,3],[116,120,137,60,6],[116,122,123,115,1],[116,122,139,16,11],[116,123,134,134,10],[116,127,130,62,13],[116,133,141,80,1],[116,134,144,109,3],[117,119,124,66,11],[117,120,137,154,10],[117,120,155,7,8],[117,133,152,78,5],[117,134,145,51,5],[117,143,153,149,15],[118,136,139,44,2],[118,141,148,62,1],[119,122,149,25,5],[119,123,135,48,9],[119,129,136,69,3],[119,138,146,103,11],[119,141,143,146,1],[119,144,146,106,10],[119,146,153,143,20],[120,121,137,18,6],[120,126,153,93,12],[120,132,147,39,14],[120,133,139,73,12],[120,135,136,33,2],[120,140,153,23,9],[120,140,155,110,6],[120,142,144,78,6],[120,144,153,28,4],[121,122,130,130,17],[121,122,142,101,10],[121,136,155,142,2],[121,137,155,118,2],[122,126,139,29,10],[122,129,146,57,15],[122,138,149,142,10],[122,138,155,32,8],[122,144,155,79,6],[123,127,153,82,11],[123,128,138,127,7],[123,133,142,0,1],[123,133,152,61,7],[123,143,147,27,15],[123,148,154,92,13],[123,152,153,143,15],[124,125,126,56,8],[124,125,150,80,13],[124,126,131,84,8],[124,129,156,106,13],[124,131,142,125,13],[124,134,136,129,3],[124,144,154,68,4],[124,154,156,28,4],[125,126,151,24,3],[125,127,139,94,7],[125,127,155,138,6],[125,134,135,109,1],[125,137,155,139,6],[125,145,146,15,15],[126,145,154,6,8],[127,128,141,42,1],[127,136,155,127,1],[127,137,154,94,6],[127,138,150,128,7],[127,148,155,150,6],[127,153,154,138,7],[128,131,139,152,7],[128,138,140,121,8],[128,147,154,141,1],[129,133,151,102,5],[129,149,156,19,13],[130,138,155,1,2],[130,145,150,21,14],[130,149,154,130,16],[131,132,152,105,1],[131,135,145,138,5],[131,136,146,11,1],[131,137,152,148,6],[131,145,149,71,17],[132,136,151,79,1],[132,142,146,85,15],[132,146,155,9,2],[133,136,141,147,1],[133,136,150,67,2],[133,142,145,24,3],[133,143,153,148,7],[133,145,152,7,5],[134,137,153,123,8],[134,140,141,152,1],[134,140,153,7,7],[134,147,148,151,8],[135,140,144,65,5],[136,139,153,73,2],[138,140,154,61,11],[139,142,154,49,8],[140,141,149,3,1],[141,145,147,145,1],[141,147,149,107,1],[143,145,152,77,8],[143,147,149,35,20],[143,147,154,51,14],[144,145,151,78,6],[145,152,156,156,13],[149,153,156,75,17],[152,154,156,155,8]],"t":[["神",20,1],["超有用",14,1],["有用",10,1],["不要",1,0]],"qmax":24};
const N=D.n, R=D.r, T=D.t, QM=D.qmax;
const $=i=>document.getElementById(i);
const col=new Intl.Collator('ja');
const esc=s=>String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const grade=q=>T.findIndex(t=>q>=t[1]);
 
/* ===== 名前の索引と正規化 (あいまい一致) ===== */
const kata2hira=s=>s.replace(/[\u30a1-\u30f6]/g,c=>String.fromCharCode(c.charCodeAt(0)-0x60));
const norm=s=>kata2hira(String(s).normalize('NFKC').toLowerCase()).replace(/\s+/g,'');
const NAME_SORTED=[...N].sort(col.compare);
const N_NORM=N.map(norm);
const NS_NORM=NAME_SORTED.map(norm);
 
/* 各名前が何レシピに登場するか (サジェスト表示用) */
const NAME_CNT=(function(){
  const c=new Array(N.length).fill(0);
  R.forEach(r=>{ [r[0],r[1],r[2],r[3]].forEach(i=>c[i]++); });
  return c;
})();
 
/* ===== 個数・評価のセレクト ===== */
(function(){
  let h='<option value="">指定なし</option>';
  for(let i=1;i<=QM;i++) h+='<option value="e'+i+'">'+i+'</option>';
  $('qn').innerHTML=h;
})();
(function(){
  let h='<option value="">すべて</option>';
  T.forEach(function(t,i){
    if(i>0 && t[2]) h+='<option value="a'+i+'">'+t[0]+'以上</option>';
    h+='<option value="e'+i+'">'+t[0]+'のみ</option>';
  });
  $('g').innerHTML=h;
})();
 
/* ===== ローカル保存 ===== */
const FAV_KEY='alchemy_favs_v1', HIST_KEY='alchemy_hist_v1', RECENT_KEY='alchemy_recent_v1';
let favs=new Set();
try{ favs=new Set(JSON.parse(localStorage.getItem(FAV_KEY)||'[]')); }catch(e){}
function saveFavs(){ try{ localStorage.setItem(FAV_KEY, JSON.stringify([...favs])); }catch(e){} }
function recipeKey(r){ return r[0]+'|'+r[1]+'|'+r[2]+'|'+r[3]+'|'+r[4]; }
 
let hist={};
try{ hist=JSON.parse(localStorage.getItem(HIST_KEY)||'{}'); }catch(e){}
function addHist(field,value){
  if(!value || !N.includes(value)) return;
  const arr=(hist[field]||[]).filter(v=>v!==value);
  arr.unshift(value);
  hist[field]=arr.slice(0,8);
  try{ localStorage.setItem(HIST_KEY, JSON.stringify(hist)); }catch(e){}
}
 
let recents=[];
try{ recents=JSON.parse(localStorage.getItem(RECENT_KEY)||'[]'); }catch(e){}
function addRecent(r){
  const k=recipeKey(r);
  recents=[k, ...recents.filter(x=>x!==k)].slice(0,8);
  try{ localStorage.setItem(RECENT_KEY, JSON.stringify(recents)); }catch(e){}
  renderRecents();
}
function clearRecent(k){
  recents=recents.filter(x=>x!==k);
  try{ localStorage.setItem(RECENT_KEY, JSON.stringify(recents)); }catch(e){}
  renderRecents();
}
 
/* ===== ハイライト ===== */
function hl(text,q){
  const safe=esc(text);
  if(!q) return safe;
  const nq=norm(q);
  if(!nq) return safe;
  const nn=norm(text);
  const i=nn.indexOf(nq);
  if(i<0) return safe;
  return esc(text.slice(0,i))+'<mark>'+esc(text.slice(i,i+q.length))+'</mark>'+esc(text.slice(i+q.length));
}
function hlMulti(text,queries){
  const ranges=[];
  const nn=norm(text);
  queries.forEach(function(q){
    if(!q) return;
    const nq=norm(q);
    if(!nq) return;
    let i=0;
    while(true){
      const p=nn.indexOf(nq,i);
      if(p<0) break;
      ranges.push([p,p+nq.length]);
      i=p+1;
    }
  });
  if(!ranges.length) return esc(text);
  ranges.sort(function(a,b){ return a[0]-b[0] || a[1]-b[1]; });
  const merged=[];
  ranges.forEach(function(r){
    const last=merged[merged.length-1];
    if(last && r[0]<=last[1]) last[1]=Math.max(last[1],r[1]);
    else merged.push([r[0],r[1]]);
  });
  let out='', cur=0;
  merged.forEach(function(m){
    out+=esc(text.slice(cur,m[0]))+'<mark>'+esc(text.slice(m[0],m[1]))+'</mark>';
    cur=m[1];
  });
  out+=esc(text.slice(cur));
  return out;
}
 
/* ===== リスト位置 ===== */
function positionList(input,list){
  const r=input.getBoundingClientRect();
  const vh=window.innerHeight, vw=window.innerWidth;
  const MAX=Math.min(5*44+8, Math.floor(vh*0.5));
  const below=vh-r.bottom-6, above=r.top-6;
  list.style.width=r.width+'px';
  list.style.left=Math.max(4,Math.min(r.left, vw-r.width-4))+'px';
  if(below>=MAX || below>=above){
    list.style.top=(r.bottom+4)+'px';
    list.style.bottom='';
    list.style.maxHeight=Math.min(MAX, below)+'px';
  }else{
    list.style.top='';
    list.style.bottom=(vh-r.top+4)+'px';
    list.style.maxHeight=Math.min(MAX, above)+'px';
  }
}
 
let openCombo=null;
function setOpen(c){
  if(openCombo && openCombo!==c) openCombo.close();
  openCombo=c;
}
/* 他の素材欄(素材1〜3)で選ばれている名前。同じ素材は1つのレシピに1回しか入らないので、候補から外す */
function takenNames(field){
  const s=new Set();
  ['m1','m2','m3'].forEach(function(id){
    if(id===field) return;
    const v=$(id).value.trim();
    if(v && N.includes(v)) s.add(v);
  });
  return s;
}
/* ===== 素材2つ選択時の「残り1つの候補」 (順番は無視) ===== */
let CAND_MODE='glow';   // 'glow' = 全部表示して光らせる / 'only' = レシピありだけ
try{ if(localStorage.getItem('alchemy_candmode_v1')==='only') CAND_MODE='only'; }catch(e){}
let PAIR=null;
function pairMap(a,b){
  const L=N.length;
  if(!PAIR){
    PAIR=new Map();
    const add=function(x,y,z){
      if(x>y){ const t=x; x=y; y=t; }
      const key=x*L+y;
      let m=PAIR.get(key);
      if(!m){ m=new Map(); PAIR.set(key,m); }
      m.set(z,(m.get(z)||0)+1);
    };
    R.forEach(function(r){ add(r[0],r[1],r[2]); add(r[0],r[2],r[1]); add(r[1],r[2],r[0]); });
  }
  if(a>b){ const t=a; a=b; b=t; }
  return PAIR.get(a*L+b) || new Map();
}
/* 1つの素材と同じレシピに入る素材 {素材index: 一緒のレシピ数} */
let SINGLE=null;
function singleMap(a){
  if(!SINGLE){
    SINGLE=N.map(function(){ return new Map(); });
    R.forEach(function(r){
      const u=[...new Set([r[0],r[1],r[2]])];
      u.forEach(function(x){ u.forEach(function(y){ if(x!==y) SINGLE[x].set(y,(SINGLE[x].get(y)||0)+1); }); });
    });
  }
  return SINGLE[a];
}
/* field以外の素材欄のうち、正しい名前で埋まっているものから候補を出す。
   2つ埋まっている → 3つ目になれる素材 (kind='pair') / 1つだけ → 同じレシピに入りうる素材 (kind='single')
   どちらも {素材index: レシピ数} を返す。0個なら null */
let CAND_KIND='pair';
function pairInfo(field){
  if(field==='out') return null;
  const vs=[...new Set(['m1','m2','m3'].filter(function(id){ return id!==field; })
    .map(function(id){ return $(id).value.trim(); })
    .filter(function(v){ return v && N.includes(v); }))];
  if(vs.length===0) return null;
  if(vs.length===1){ CAND_KIND='single'; return singleMap(N.indexOf(vs[0])); }
  CAND_KIND='pair';
  return pairMap(N.indexOf(vs[0]),N.indexOf(vs[1]));
}
function setCandMode(m){
  CAND_MODE=m;
  try{ localStorage.setItem('alchemy_candmode_v1',m); }catch(e){}
  document.querySelectorAll('#cand_mode button[data-cm]').forEach(function(b){
    const on=b.dataset.cm===m;
    b.classList.toggle('on',on);
    b.setAttribute('aria-pressed',on?'true':'false');
  });
}
function makeCombo(inputId,listId,field){
  const input=$(inputId), list=$(listId);
  let items=[], active=-1, isOpen=false;
 
  function buildItems(){
    const q=input.value.trim();
    const nq=norm(q);
    const isOut=(field==='out');
    const taken=isOut ? new Set() : takenNames(field);   // 成果物欄は素材の重複ルールの対象外
    const pair=pairInfo(field);
    const kind=CAND_KIND;
    const only=!!pair && CAND_MODE==='only';
    const okName=function(n){
      const ni=N.indexOf(n);
      if(isOut) return ni>=0 && outCnt[ni]>0;
      if(only) return ni>=0 && pair.has(ni);
      return true;
    };
    const recents=(hist[field]||[]).filter(function(n){
      return !taken.has(n) && okName(n) && (!nq || norm(n).indexOf(nq)>=0);
    });
    const recSet=new Set(recents);
    const rest=[];
    NAME_SORTED.forEach(function(n,i){
      if(recSet.has(n) || taken.has(n) || !okName(n)) return;
      if(!nq || NS_NORM[i].indexOf(nq)>=0) rest.push(n);
    });
    let nGlow=0;
    if(pair && !only){
      const g=[], o=[];
      rest.forEach(function(n){ (pair.has(N.indexOf(n))?g:o).push(n); });
      nGlow=g.length;
      rest.length=0;
      g.concat(o).forEach(function(n){ rest.push(n); });
    }
    return {recent:recents, rest:rest, q:q, pair:pair, only:only, nGlow:nGlow, kind:kind};
  }
 
  function render(){
    if(!isOpen){ list.classList.remove('on'); return; }
    const r=buildItems();
    items=r.recent.concat(r.rest);
    if(active>=items.length) active=-1;
    const glowMode=!!r.pair && !r.only;
    const NOCAND=r.kind==='single'?'この素材と同じレシピに入る素材はありません':'この2つで錬金できる素材はありません';
    const isG=function(n){ return glowMode && r.pair.has(N.indexOf(n)); };
    if(items.length===0){
      list.innerHTML='<div class="empty">'+(field==='out'?'該当なし':(r.pair?NOCAND:'該当する名前がありません'))+'</div>';
    }else{
      let h='', idx=0;
      if(glowMode && r.nGlow===0) h+='<div class="empty">'+NOCAND+'</div>';
      if(r.recent.length){
        h+='<div class="grp"><svg viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>最近使った名前</div>';
        r.recent.forEach(function(n){
          h+='<div class="item'+(idx===active?' act':'')+(isG(n)?' glow':(glowMode?' dim':''))+'" data-i="'+idx+'" role="option"><span class="nm">'+hl(n,r.q)+'</span></div>';
          idx++;
        });
      }
      if(r.rest.length){
        if(!glowMode && r.recent.length) h+='<div class="grp">すべて</div>';
        r.rest.forEach(function(n,j){
          if(glowMode){
            if(j===0 && r.nGlow>0) h+='<div class="grp g-on">\u2726 '+(r.kind==='single'?'同じレシピに入る':'レシピあり')+' ('+r.nGlow+')</div>';
            if(j===r.nGlow) h+='<div class="grp">'+(r.kind==='single'?'同じレシピに入らない':'レシピなし')+'</div>';
          }
          const ni=N.indexOf(n);
          const ct=ni<0 ? 0 : (field==='out' ? outCnt[ni] : (r.pair ? (r.pair.get(ni)||0) : NAME_CNT[ni]));
          h+='<div class="item'+(idx===active?' act':'')+(isG(n)?' glow':(glowMode?' dim':''))+'" data-i="'+idx+'" role="option"><span class="nm">'+hl(n,r.q)+'</span><span class="ct">'+ct+'</span></div>';
          idx++;
        });
      }
      list.innerHTML=h;
    }
    list.classList.add('on');
    positionList(input,list);
    if(active>=0){
      const el=list.querySelector('.item.act');
      if(el){
        const top=el.offsetTop, bot=top+el.offsetHeight;
        const vt=list.scrollTop, vb=vt+list.clientHeight;
        if(top<vt) list.scrollTop=top;
        else if(bot>vb) list.scrollTop=bot-list.clientHeight;
      }
    }
  }
  function pick(i){
    if(i<0 || i>=items.length) return;
    const val=items[i];
    input.value=val;
    addHist(field,val);
    isOpen=false; active=-1;
    render();
    syncClearBtns();
    applyExclusive();
    run();
    syncURL();
  }
  function open(){
    if(input.disabled) return;
    if(isOpen){ positionList(input,list); return; }
    isOpen=true; active=-1;
    setOpen(handle);
    render();
  }
 
  input.addEventListener('focus',open);
  input.addEventListener('click',open);
  input.addEventListener('input',function(){
    isOpen=true; setOpen(handle); active=-1;
    render();
    syncClearBtns();
    applyExclusive();
    run();
    syncURL();
  });
  input.addEventListener('keydown',function(e){
    if(e.key==='ArrowDown' || e.key==='ArrowUp'){
      if(!isOpen) open();
      e.preventDefault();
      if(e.key==='ArrowDown') active=Math.min(active+1,items.length-1);
      else active=Math.max(active-1,-1);
      render();
      return;
    }
    if(!isOpen) return;
    if(e.key==='Enter'){
      if(active>=0){ e.preventDefault(); pick(active); }
    }else if(e.key==='Escape'){
      isOpen=false; render();
    }
  });
  list.addEventListener('click',function(e){
    const it=e.target.closest('.item');
    if(!it) return;
    e.preventDefault();
    pick(+it.dataset.i);
  });
  list.addEventListener('pointerdown',function(e){ e.stopPropagation(); });
 
  const handle={
    input:input, list:list,
    close:function(){ if(isOpen){ isOpen=false; render(); if(openCombo===handle) openCombo=null; } },
    repos:function(){ if(isOpen) positionList(input,list); },
  };
  return handle;
}
 
const combos=[
  makeCombo('m1','m1_list','m1'),
  makeCombo('m2','m2_list','m2'),
  makeCombo('m3','m3_list','m3'),
  makeCombo('out','out_list','out'),
];
 
/* ===== 個別クリアボタン ===== */
function syncClearBtns(){
  ['m1','m2','m3','out'].forEach(function(id){
    const f=$(id).closest('.field');
    f.classList.toggle('has-val', !!$(id).value.trim());
  });
}
document.querySelectorAll('.clear-inp').forEach(function(btn){
  btn.addEventListener('click',function(e){
    e.preventDefault();
    const id=btn.dataset.for;
    $(id).value='';
    syncClearBtns();
    applyExclusive();
    run();
    syncURL();
  });
});
 
/* ===== 注意文 / 成果物欄の「該当なし」 ===== */
const HINT_DEFAULT='素材を選ぶと、成果物の候補はそのレシピがあるものだけに絞られます。';
let outCnt=[];   // 成果物ごとの、いまの条件に合うレシピ数 (run() が更新)
function updateHint(){
  const hint=$('hint');
  const vs=['m1','m2','m3'].map(function(id){ return $(id).value.trim(); }).filter(function(v){ return v && N.includes(v); });
  const dup=vs.find(function(v,i){ return vs.indexOf(v)!==i; });
  const any=outCnt.length===0 || outCnt.some(function(c){ return c>0; });
  hint.style.color=(dup||!any)?'var(--uu)':'';
  if(dup) hint.textContent='「'+dup+'」は素材欄の2か所には指定できません (同じ素材は1つのレシピに1回だけ)。どちらかを変えてください。';
  else if(!any) hint.textContent='この条件に合うレシピがありません (成果物: 該当なし)。';
  else hint.textContent=HINT_DEFAULT;
}
function refreshOutHint(){
  const any=outCnt.some(function(c){ return c>0; });
  $('out').placeholder=any?'作りたい物':'該当なし';
  $('f_out').classList.toggle('noresult',!any);
  updateHint();
}
function applyExclusive(){ updateHint(); }
 
/* ===== 所持素材 ===== */
const owned=new Set();
function renderOwnedTags(){
  const box=$('owned_tags');
  if(owned.size===0){
    box.innerHTML='<span class="mut" style="font-size:12px">なし</span>';
    return;
  }
  box.innerHTML=[...owned].sort(col.compare).map(function(n){
    return '<span class="tag">'+esc(n)+'<button type="button" data-n="'+esc(n)+'" aria-label="削除">×</button></span>';
  }).join('');
}
function renderOwnedGrid(){
  const q=$('owned_search').value.trim();
  const nq=norm(q);
  const grid=$('owned_grid');
  const list=nq ? NAME_SORTED.filter(function(n,i){ return NS_NORM[i].indexOf(nq)>=0; }) : NAME_SORTED;
  if(list.length===0){
    grid.innerHTML='<div class="none">該当する名前がありません</div>';
    return;
  }
  grid.innerHTML=list.map(function(n){
    const on=owned.has(n);
    return '<button type="button" class="'+(on?'on':'')+'" data-n="'+esc(n)+'">'+(on?'✓ ':'')+esc(n)+'</button>';
  }).join('');
}
$('owned_tags').addEventListener('click',function(e){
  const b=e.target.closest('button[data-n]');
  if(!b) return;
  owned.delete(b.dataset.n);
  renderOwnedTags(); renderOwnedGrid(); run(); syncURL();
});
$('owned_grid').addEventListener('click',function(e){
  const b=e.target.closest('button[data-n]');
  if(!b) return;
  const n=b.dataset.n;
  if(owned.has(n)) owned.delete(n); else owned.add(n);
  renderOwnedTags(); renderOwnedGrid(); run(); syncURL();
});
$('owned_search').addEventListener('input',renderOwnedGrid);
$('owned_toggle').addEventListener('click',function(){
  const body=$('owned_body');
  body.hidden=!body.hidden;
  this.classList.toggle('on', !body.hidden);
  if(!body.hidden){
    renderOwnedTags(); renderOwnedGrid(); $('owned_search').focus();
  }
});
 
/* ===== 最近見た ===== */
function renderRecents(){
  const box=$('recents');
  const list=$('recents_list');
  const items=recents.map(function(k){
    return R.find(function(r){ return recipeKey(r)===k; });
  }).filter(Boolean);
  if(items.length===0){
    box.hidden=true;
    return;
  }
  box.hidden=false;
  list.innerHTML=items.map(function(r){
    return '<span class="chip" data-k="'+esc(recipeKey(r))+'">'+esc(N[r[3]])+' ×'+r[4]+'<button class="x" type="button" data-del="'+esc(recipeKey(r))+'" aria-label="削除">×</button></span>';
  }).join('');
}
$('recents_list').addEventListener('click',function(e){
  const d=e.target.closest('button[data-del]');
  if(d){ e.stopPropagation(); clearRecent(d.dataset.del); return; }
  const chip=e.target.closest('.chip');
  if(!chip) return;
  const k=chip.dataset.k;
  const r=R.find(function(x){ return recipeKey(x)===k; });
  if(!r) return;
  // 成果物名で絞り込み
  $('out').value=N[r[3]];
  ['m1','m2','m3'].forEach(id=>$(id).value='');
  syncClearBtns(); applyExclusive(); run(); syncURL();
  window.scrollTo({top:0,behavior:'smooth'});
});
 
document.addEventListener('pointerdown',function(e){
  if(e.target.closest('.list') || e.target.closest('input')) return;
  combos.forEach(function(c){ c.close(); });
});
 
/* ===== URL 同期 ===== */
function syncURL(){
  const p=new URLSearchParams();
  ['m1','m2','m3','out','qn','g'].forEach(function(id){
    const v=$(id).value.trim();
    if(v) p.set(id,v);
  });
  if(owned.size>0) p.set('own',[...owned].join('|'));
  if($('fav_only').checked) p.set('fav','1');
  if(CAND_MODE==='only') p.set('cm','only');
  const s=p.toString();
  const url=location.pathname+(s?'?'+s:'')+location.hash;
  history.replaceState(null,'',url);
}
function loadFromURL(){
  const p=new URLSearchParams(location.search);
  ['m1','m2','m3','out'].forEach(function(id){
    const v=p.get(id);
    if(v) $(id).value=v;
  });
  const qn=p.get('qn'); if(qn) $('qn').value=qn;
  const g=p.get('g'); if(g) $('g').value=g;
  const ow=p.get('own');
  if(ow) ow.split('|').forEach(function(n){ if(n) owned.add(n); });
  if(p.get('fav')==='1') $('fav_only').checked=true;
  if(p.get('cm')==='only') CAND_MODE='only';
  else if(p.has('cm')) CAND_MODE='glow';
  renderOwnedTags(); renderOwnedGrid();
}
 
/* ===== 検索 ===== */
function matcher(s){
  const nq=norm(s);
  if(!nq) return function(){ return true; };
  const exact=N_NORM.indexOf(nq)>=0;
  if(exact){
    return function(i){ return N_NORM[i]===nq; };
  }
  return function(i){ return N_NORM[i].indexOf(nq)>=0; };
}
 
/* 入力した素材(最大3つ)を、レシピの素材1〜3にそれぞれ別のスロットとして割り当てられるか */
function assignable(ms,r){
  const used=[false,false,false];
  function go(i){
    if(i===ms.length) return true;
    for(let s=0;s<3;s++){
      if(!used[s] && ms[i](r[s])){
        used[s]=true;
        if(go(i+1)) return true;
        used[s]=false;
      }
    }
    return false;
  }
  return go(0);
}

let shown=100, last=[], queries=[];
let sortCol=4, sortDir=-1;   // 4=個数
 
function run(){
  shown=100;
  const rawMats=['m1','m2','m3'].map(function(id){ return $(id).value.trim(); }).filter(Boolean);
  const rawOut=$('out').value.trim();
  const vals=rawMats;
  const ov=rawOut;
 
  const ms=vals.map(matcher);
  const om=ov ? matcher(ov) : null;
  const gv=$('g').value;
  const gk=gv ? +gv.slice(1) : -1;
  const qn=$('qn').value;
  const favOnly=$('fav_only').checked;
 
  queries=vals.slice();
  if(ov) queries.push(ov);
 
  const gok=function(q){
    if(!gv) return true;
    const gi=grade(q);
    return gv.charAt(0)==='a' ? (gi>=0 && gi<=gk) : (gi===gk);
  };
  const qok=function(q){
    if(!qn) return true;
    const mode=qn.charAt(0);
    const n=+qn.slice(1);
    return mode==='a' ? q>=n : q===n;
  };
  const ook=function(r){
    if(owned.size===0) return true;
    return owned.has(N[r[0]]) && owned.has(N[r[1]]) && owned.has(N[r[2]]);
  };
 
  const base=R.filter(function(r){
    if(!assignable(ms,r)) return false;
    if(!ook(r)) return false;
    if(favOnly && !favs.has(recipeKey(r))) return false;
    return gok(r[4]) && qok(r[4]);
  });
  outCnt=new Array(N.length).fill(0);
  base.forEach(function(r){ outCnt[r[3]]++; });
  last=om ? base.filter(function(r){ return om(r[3]); }) : base;
  refreshOutHint();
  applySort();
  render();
}
 
function applySort(){
  const s=sortDir;
  if(sortCol===4){
    last.sort(function(a,b){ return s*(a[4]-b[4]) || col.compare(N[a[3]],N[b[3]]); });
  }else if(sortCol===5){
    // 評価順 = 個数順
    last.sort(function(a,b){ return s*(a[4]-b[4]) || col.compare(N[a[3]],N[b[3]]); });
  }else{
    last.sort(function(a,b){ return s*col.compare(N[a[sortCol]], N[b[sortCol]]) || b[4]-a[4]; });
  }
  const labels=['素材1','素材2','素材3','成果物','個数','評価'];
  $('sort_label').textContent=labels[sortCol]+(sortDir<0?' ▼':' ▲');
  $('sort_info').hidden=false;
  document.querySelectorAll('thead th[data-sort]').forEach(function(th){
    const on=+th.dataset.sort===sortCol;
    th.classList.toggle('on', on);
    const arr=th.querySelector('.arr');
    if(arr) arr.textContent=on ? (sortDir<0?'▼':'▲') : '▼';
  });
}
document.querySelector('thead').addEventListener('click',function(e){
  const th=e.target.closest('th[data-sort]');
  if(!th) return;
  const c=+th.dataset.sort;
  if(sortCol===c){ sortDir=-sortDir; }
  else { sortCol=c; sortDir=(c===4||c===5)?-1:1; }
  applySort();
  render();
});
 
const HN=T.filter(function(t){ return t[2]; }).length;   // 目立たせる評価の数
function clsFor(k){
  if(k<0) return '';
  if(!T[k][2]) return 'g1';
  return 'g'+(HN-k+1);   // 一番下の目立たせる評価=g2、上に行くほど g3, g4 ...
}
function starsFor(k){
  if(k<0) return '';
  return '★'.repeat(Math.max(1, T.length-k));
}
 
const STAR_FILL='<svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/></svg>';
const STAR_OUT='<svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>';
 
function render(){
  $('cnt').textContent=last.length.toLocaleString();
  if(last.length===0){
    $('tb').innerHTML='<tr class="empty-row"><td colspan="7">該当するレシピがありません</td></tr>';
    $('more').style.display='none';
    return;
  }
  let h='';
  const n=Math.min(shown,last.length);
  for(let i=0;i<n;i++){
    const r=last[i], gk=grade(r[4]), k=recipeKey(r), on=favs.has(k);
    const qs=queries;
    h+='<tr data-k="'+esc(k)+'">'
      +'<td class="fav"><button class="star'+(on?' on':'')+'" data-k="'+esc(k)+'" type="button" aria-label="お気に入り">'+(on?STAR_FILL:STAR_OUT)+'</button></td>'
      +'<td data-l="素材1">'+hlMulti(N[r[0]],qs)+'</td>'
      +'<td data-l="素材2">'+hlMulti(N[r[1]],qs)+'</td>'
      +'<td data-l="素材3">'+hlMulti(N[r[2]],qs)+'</td>'
      +'<td data-l="成果物"><b>'+hlMulti(N[r[3]],qs)+'</b></td>'
      +'<td data-l="個数" class="q">'+r[4]+'</td>'
      +'<td data-l="評価" class="'+clsFor(gk)+'">'+(gk<0?'':'<span class="rs">'+starsFor(gk)+'</span><span class="rn">'+esc(T[gk][0])+'</span>')+'</td>'
      +'</tr>';
  }
  $('tb').innerHTML=h;
  $('more').style.display = last.length>shown ? '' : 'none';
}
 
/* ===== イベント ===== */
['qn','g'].forEach(function(id){ $(id).addEventListener('change',function(){ run(); syncURL(); }); });
$('fav_only').addEventListener('change',function(){ run(); syncURL(); });
 
$('tb').addEventListener('click',function(e){
  const b=e.target.closest('.star');
  if(b){
    e.stopPropagation();
    const k=b.dataset.k;
    if(favs.has(k)) favs.delete(k); else favs.add(k);
    saveFavs();
    b.classList.toggle('on', favs.has(k));
    b.innerHTML=favs.has(k)?STAR_FILL:STAR_OUT;
    if($('fav_only').checked) run();
    return;
  }
  const tr=e.target.closest('tr[data-k]');
  if(tr){
    const k=tr.dataset.k;
    const r=R.find(function(x){ return recipeKey(x)===k; });
    if(r) addRecent(r);
  }
});
 
$('more').addEventListener('click',function(){ shown+=200; render(); });
$('clr').addEventListener('click',function(){
  ['m1','m2','m3','out'].forEach(function(id){ $(id).value=''; });
  $('qn').value=''; $('g').value='';
  $('fav_only').checked=false;
  owned.clear();
  renderOwnedTags(); renderOwnedGrid();
  syncClearBtns(); applyExclusive();
  run(); syncURL();
});
 
/* ===== スクロール系 ===== */
function reposAll(){ combos.forEach(function(c){ c.repos(); }); }
window.addEventListener('resize',reposAll);
window.addEventListener('scroll',function(){
  $('totop').classList.toggle('on', window.scrollY>400);
  reposAll();
},true);
if(window.visualViewport){
  window.visualViewport.addEventListener('resize',reposAll);
  window.visualViewport.addEventListener('scroll',reposAll);
}
$('totop').addEventListener('click',function(){
  window.scrollTo({top:0,behavior:'smooth'});
});
 
document.querySelectorAll('#cand_mode button[data-cm]').forEach(function(b){
  b.addEventListener('click',function(){ setCandMode(b.dataset.cm); syncURL(); });
});
/* ===== 起動 ===== */
loadFromURL();
setCandMode(CAND_MODE);
syncClearBtns();
applyExclusive();
renderRecents();
applySort();
run();
</script>
</body></html>
