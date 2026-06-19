<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VocabMaster – Học Từ Vựng</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Noto+Sans+SC:wght@300;400;500;700&display=swap');

  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --surface2: #222636;
    --surface3: #2a2f44;
    --border: #2e3347;
    --accent: #6c63ff;
    --accent2: #00d4aa;
    --accent3: #ff6b6b;
    --accent4: #ffd166;
    --accent5: #4ea8ff;
    --text: #e8eaf0;
    --text2: #8890a4;
    --text3: #5c6478;
    --success: #00d4aa;
    --danger: #ff6b6b;
    --warn: #ffd166;
    --radius: 12px;
    --radius-sm: 8px;
    --shadow: 0 4px 24px rgba(0,0,0,.4);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }
  body { font-family: 'Inter', 'Noto Sans SC', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; }

  /* ── SIDEBAR ── */
  #app { display: flex; height: 100vh; overflow: hidden; }
  #sidebar { width: 230px; flex-shrink: 0; background: var(--surface); border-right: 1px solid var(--border); display: flex; flex-direction: column; padding: 20px 0; overflow-y: auto; }
  #sidebar::-webkit-scrollbar { width: 4px; }
  #sidebar::-webkit-scrollbar-thumb { background: var(--border); }
  .logo { padding: 0 20px 20px; border-bottom: 1px solid var(--border); margin-bottom: 16px; }
  .logo h1 { font-size: 18px; font-weight: 700; background: linear-gradient(135deg, var(--accent), var(--accent2)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .logo span { font-size: 11px; color: var(--text3); font-weight: 400; display: block; margin-top: 2px; }

  .nav-item { display: flex; align-items: center; gap: 10px; padding: 9px 20px; cursor: pointer; color: var(--text2); font-size: 13px; font-weight: 500; transition: all .18s; border-left: 3px solid transparent; }
  .nav-item:hover { color: var(--text); background: rgba(108,99,255,.08); }
  .nav-item.active { color: var(--accent); background: rgba(108,99,255,.12); border-left-color: var(--accent); }
  .nav-item .icon { font-size: 15px; width: 18px; text-align: center; }
  .nav-section { font-size: 10px; color: var(--text3); font-weight: 600; letter-spacing: .08em; text-transform: uppercase; padding: 12px 20px 4px; }

  .sidebar-stats { margin-top: auto; padding: 16px 20px; border-top: 1px solid var(--border); }
  .stat-row { display: flex; justify-content: space-between; font-size: 11.5px; color: var(--text2); margin-bottom: 5px; }
  .stat-row b { color: var(--text); }
  .xp-bar-wrap { margin-top: 10px; }
  .xp-label { display: flex; justify-content: space-between; font-size: 11px; color: var(--text3); margin-bottom: 4px; }
  .xp-bar { height: 6px; background: var(--surface2); border-radius: 3px; overflow: hidden; }
  .xp-fill { height: 100%; background: linear-gradient(90deg, var(--accent4), var(--accent3)); border-radius: 3px; transition: width .4s; }

  /* ── MAIN ── */
  #main { flex: 1; overflow-y: auto; padding: 28px 32px; }
  #main::-webkit-scrollbar { width: 6px; }
  #main::-webkit-scrollbar-track { background: transparent; }
  #main::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  .page { display: none; }
  .page.active { display: block; }

  .page-header { margin-bottom: 22px; display: flex; justify-content: space-between; align-items: flex-start; flex-wrap: wrap; gap: 12px; }
  .page-header h2 { font-size: 22px; font-weight: 700; }
  .page-header p { color: var(--text2); font-size: 13.5px; margin-top: 4px; }

  /* ── CARDS ── */
  .card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; }
  .card + .card { margin-top: 14px; }

  /* ── FOLDERS ── */
  .folder-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 14px; margin-bottom: 24px; }
  .folder-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 18px; cursor: pointer; transition: all .15s; position: relative; }
  .folder-card:hover { border-color: var(--accent); transform: translateY(-2px); }
  .folder-card.active-folder { border-color: var(--accent); background: rgba(108,99,255,.08); }
  .folder-icon { font-size: 26px; margin-bottom: 8px; }
  .folder-name { font-weight: 700; font-size: 15px; }
  .folder-count { font-size: 12px; color: var(--text2); margin-top: 3px; }
  .folder-del { position: absolute; top: 10px; right: 10px; background: none; border: none; color: var(--text3); font-size: 13px; cursor: pointer; opacity: 0; transition: .15s; }
  .folder-card:hover .folder-del { opacity: 1; }
  .folder-del:hover { color: var(--danger); }
  .folder-new { border: 2px dashed var(--border); display: flex; align-items: center; justify-content: center; flex-direction: column; gap: 6px; color: var(--text3); }
  .folder-new:hover { border-color: var(--accent); color: var(--accent); }

  /* ── VOCAB LIST ── */
  .filter-bar { display: flex; gap: 10px; margin-bottom: 18px; flex-wrap: wrap; align-items: center; }
  .filter-bar input { flex: 1; min-width: 180px; background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); color: var(--text); font-size: 13.5px; padding: 8px 12px; outline: none; }
  .filter-bar input:focus { border-color: var(--accent); }
  .filter-bar select { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); color: var(--text); font-size: 13px; padding: 8px 10px; outline: none; cursor: pointer; }
  .btn { display: inline-flex; align-items: center; gap: 6px; background: var(--accent); color: #fff; border: none; border-radius: var(--radius-sm); padding: 8px 16px; font-size: 13px; font-weight: 600; cursor: pointer; transition: all .15s; white-space: nowrap; }
  .btn:hover { opacity: .88; transform: translateY(-1px); }
  .btn.secondary { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .btn.secondary:hover { border-color: var(--accent); color: var(--accent); }
  .btn.danger { background: var(--accent3); }
  .btn.success { background: var(--accent2); color: #0f1117; }
  .btn.sm { padding: 5px 10px; font-size: 12px; }
  .btn:disabled { opacity: .4; cursor: not-allowed; transform: none; }

  .vocab-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 14px; }
  .vocab-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; position: relative; transition: border-color .15s; }
  .vocab-card:hover { border-color: var(--accent); }
  .vocab-card .word-row { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
  .vocab-card .word { font-size: 18px; font-weight: 700; color: var(--text); }
  .vocab-card .pos { font-size: 11px; color: var(--accent5); background: rgba(78,168,255,.12); padding: 1px 7px; border-radius: 10px; font-weight: 600; }
  .vocab-card .ipa { font-size: 12.5px; color: var(--accent); font-family: monospace; margin: 4px 0 6px; }
  .vocab-card .definition { font-size: 13px; color: var(--text2); line-height: 1.5; }
  .vocab-card .example { font-size: 12px; color: var(--text3); font-style: italic; margin-top: 6px; border-left: 2px solid var(--border); padding-left: 8px; }
  .vocab-card .synonyms { font-size: 11.5px; color: var(--accent2); margin-top: 6px; }
  .vocab-card .antonyms { font-size: 11.5px; color: var(--accent3); margin-top: 2px; }
  .vocab-card .tags { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }
  .tag { font-size: 11px; padding: 2px 8px; border-radius: 20px; font-weight: 500; }
  .fav-btn { position: absolute; top: 12px; right: 12px; background: none; border: none; font-size: 16px; cursor: pointer; opacity: .5; transition: .15s; }
  .fav-btn:hover, .fav-btn.active { opacity: 1; }
  .audio-btn { background: rgba(108,99,255,.15); border: none; color: var(--accent); border-radius: 50%; width: 28px; height: 28px; cursor: pointer; font-size: 13px; display: inline-flex; align-items: center; justify-content: center; transition: .15s; flex-shrink: 0; }
  .audio-btn:hover { background: var(--accent); color: #fff; }
  .level-select { margin-top: 10px; }
  .level-select select { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-size: 12px; padding: 6px 8px; outline: none; cursor: pointer; }

  /* Level colors (0-5): 0=Học1,1=Học2,2=Học3,3=Học4,4=Chưa thuộc,5=Đã nhớ */
  .lvl-0 { background: rgba(78,168,255,.15); color: var(--accent5); }
  .lvl-1 { background: rgba(108,99,255,.15); color: var(--accent); }
  .lvl-2 { background: rgba(255,209,102,.15); color: var(--warn); }
  .lvl-3 { background: rgba(255,166,77,.15); color: #ffa64d; }
  .lvl-4 { background: rgba(255,107,107,.15); color: var(--accent3); }
  .lvl-5 { background: rgba(0,212,170,.15); color: var(--success); }

  /* ── FLASHCARD ── */
  .flashcard-wrap { perspective: 1200px; max-width: 540px; margin: 0 auto 18px; }
  .flashcard { width: 100%; height: 300px; position: relative; transform-style: preserve-3d; transition: transform .5s cubic-bezier(.4,0,.2,1); cursor: pointer; border-radius: var(--radius); }
  .flashcard.flipped { transform: rotateY(180deg); }
  .fc-front, .fc-back { position: absolute; inset: 0; border-radius: var(--radius); backface-visibility: hidden; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 32px; text-align: center; border: 1px solid var(--border); }
  .fc-front { background: linear-gradient(135deg, var(--surface) 0%, var(--surface2) 100%); }
  .fc-back { background: linear-gradient(135deg, rgba(108,99,255,.15) 0%, var(--surface) 100%); transform: rotateY(180deg); }
  .fc-word { font-size: 36px; font-weight: 800; background: linear-gradient(135deg, var(--accent), var(--accent2)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .fc-ipa { font-size: 16px; color: var(--accent); margin: 6px 0; font-family: monospace; }
  .fc-hint { font-size: 12.5px; color: var(--text3); margin-top: 12px; }
  .fc-definition { font-size: 18px; color: var(--text); font-weight: 600; }
  .fc-example { font-size: 13px; color: var(--text2); margin-top: 10px; font-style: italic; }
  .fc-controls { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; margin-top: 14px; }
  .fc-info { display: flex; justify-content: space-between; max-width: 540px; margin: 0 auto 10px; font-size: 13px; color: var(--text2); align-items: center; }
  .progress-bar { height: 4px; background: var(--border); border-radius: 2px; margin-bottom: 16px; overflow: hidden; max-width: 540px; margin-left: auto; margin-right: auto; }
  .progress-fill { height: 100%; background: linear-gradient(90deg, var(--accent), var(--accent2)); border-radius: 2px; transition: width .3s; }
  .level-rate-row { display: flex; gap: 6px; justify-content: center; flex-wrap: wrap; margin-top: 14px; max-width: 540px; margin-left: auto; margin-right: auto; }
  .level-rate-btn { flex: 1; min-width: 80px; padding: 8px 6px; font-size: 11.5px; border: 1px solid var(--border); background: var(--surface2); border-radius: 8px; cursor: pointer; transition: .15s; text-align: center; font-weight: 600; }
  .level-rate-btn:hover { transform: translateY(-1px); }

  /* Timer ring */
  .timer-wrap { display: flex; align-items: center; gap: 8px; }
  .timer-ring { position: relative; width: 36px; height: 36px; }
  .timer-ring svg { transform: rotate(-90deg); }
  .timer-ring circle { fill: none; stroke-width: 4; }
  .timer-bg { stroke: var(--border); }
  .timer-fg { stroke: var(--accent2); stroke-linecap: round; transition: stroke-dashoffset .2s linear, stroke .2s; }
  .timer-num { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; }

  /* ── GROUP TABS (flashcard nhóm ôn tập) ── */
  .group-tabs { display: flex; gap: 8px; margin-bottom: 16px; flex-wrap: wrap; }
  .group-tab { padding: 7px 14px; border-radius: 20px; font-size: 12.5px; font-weight: 600; cursor: pointer; border: 1px solid var(--border); background: var(--surface2); color: var(--text2); transition: .15s; }
  .group-tab.active { border-color: var(--accent); color: var(--accent); background: rgba(108,99,255,.12); }

  /* ── QUIZ / EXERCISES (shared) ── */
  .quiz-wrap { max-width: 620px; margin: 0 auto; }
  .quiz-question { font-size: 16px; font-weight: 600; color: var(--text); margin-bottom: 18px; line-height: 1.6; background: var(--surface2); border-radius: var(--radius-sm); padding: 16px 20px; border-left: 3px solid var(--accent); }
  .quiz-options { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 18px; }
  .quiz-opt { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 14px 16px; cursor: pointer; font-size: 14px; color: var(--text); transition: all .15s; text-align: left; }
  .quiz-opt:hover { border-color: var(--accent); background: rgba(108,99,255,.08); }
  .quiz-opt.correct { border-color: var(--success); background: rgba(0,212,170,.1); color: var(--success); }
  .quiz-opt.wrong { border-color: var(--danger); background: rgba(255,107,107,.1); color: var(--danger); }
  .quiz-opt:disabled { cursor: default; }
  .quiz-fill input, .text-input { width: 100%; background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); color: var(--text); font-size: 15px; padding: 12px 16px; outline: none; margin-bottom: 12px; font-family: inherit; }
  .quiz-fill input:focus, .text-input:focus { border-color: var(--accent); }
  .quiz-fill input.correct, .text-input.correct { border-color: var(--success); background: rgba(0,212,170,.08); }
  .quiz-fill input.wrong, .text-input.wrong { border-color: var(--danger); background: rgba(255,107,107,.08); }
  .quiz-feedback { font-size: 13.5px; padding: 10px 14px; border-radius: var(--radius-sm); margin-bottom: 14px; }
  .quiz-feedback.correct { background: rgba(0,212,170,.1); color: var(--success); border: 1px solid rgba(0,212,170,.3); }
  .quiz-feedback.wrong { background: rgba(255,107,107,.1); color: var(--danger); border: 1px solid rgba(255,107,107,.3); }
  .quiz-score { text-align: center; padding: 36px 20px; }
  .quiz-score .big { font-size: 60px; font-weight: 800; background: linear-gradient(135deg, var(--accent), var(--accent2)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .quiz-score .label { font-size: 15px; color: var(--text2); margin-bottom: 20px; }
  .quiz-score .xp-earned { font-size: 14px; color: var(--accent4); font-weight: 600; margin-bottom: 20px; }
  .quiz-mode-switch { display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; }
  .mode-btn { flex: 1; min-width: 130px; padding: 10px 8px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); color: var(--text2); font-size: 12.5px; cursor: pointer; transition: .15s; text-align: center; }
  .mode-btn.active { background: rgba(108,99,255,.15); border-color: var(--accent); color: var(--accent); font-weight: 600; }
  .answer-dir { display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; }
  .dir-btn { flex: 1; min-width: 200px; padding: 8px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); color: var(--text2); font-size: 12.5px; cursor: pointer; transition: .15s; text-align: center; }
  .dir-btn.active { background: rgba(0,212,170,.12); border-color: var(--accent2); color: var(--accent2); }
  .count-pills { display: flex; gap: 8px; }
  .count-pill { padding: 7px 16px; border-radius: 20px; font-size: 13px; font-weight: 600; cursor: pointer; border: 1px solid var(--border); background: var(--surface2); color: var(--text2); transition: .15s; }
  .count-pill.active { border-color: var(--accent4); color: var(--accent4); background: rgba(255,209,102,.1); }
  .count-pill .pts { font-size: 10.5px; opacity: .7; display: block; }

  /* ── READING ── */
  .reading-wrap { max-width: 720px; margin: 0 auto; }
  .reading-passage { font-size: 15.5px; line-height: 2; color: var(--text); background: var(--surface); border-radius: var(--radius); padding: 28px 32px; border: 1px solid var(--border); margin-bottom: 20px; }
  .blank-input { display: inline-block; border: none; border-bottom: 2px solid var(--accent); background: transparent; color: var(--accent2); font-size: 15.5px; font-family: inherit; outline: none; min-width: 100px; padding: 0 4px; text-align: center; transition: .2s; }
  .blank-input.correct { border-color: var(--success); color: var(--success); }
  .blank-input.wrong { border-color: var(--danger); color: var(--danger); }
  .word-bank { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 20px; }
  .word-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: 20px; padding: 5px 14px; font-size: 13px; color: var(--text); cursor: pointer; transition: .15s; }
  .word-chip:hover { border-color: var(--accent); color: var(--accent); }
  .word-chip.used { opacity: .35; }
  .reading-label { font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: .06em; font-weight: 600; margin-bottom: 8px; }

  /* ── MATCHING ── */
  .match-wrap { max-width: 720px; margin: 0 auto; display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .match-col { display: flex; flex-direction: column; gap: 8px; }
  .match-item { background: var(--surface); border: 2px solid var(--border); border-radius: var(--radius-sm); padding: 12px 14px; font-size: 13.5px; cursor: pointer; transition: .15s; }
  .match-item:hover { border-color: var(--accent); }
  .match-item.selected { border-color: var(--accent); background: rgba(108,99,255,.12); }
  .match-item.matched-correct { border-color: var(--success); background: rgba(0,212,170,.1); color: var(--success); cursor: default; }
  .match-item.matched-wrong-flash { border-color: var(--danger); background: rgba(255,107,107,.15); }
  .match-item.disabled { pointer-events: none; opacity: .5; }

  /* ── DICTATION / NGHE VIẾT ── */
  .dictation-box { max-width: 560px; margin: 0 auto; text-align: center; }
  .dictation-audio-btn { width: 90px; height: 90px; border-radius: 50%; background: linear-gradient(135deg, var(--accent), var(--accent5)); border: none; color: #fff; font-size: 32px; cursor: pointer; box-shadow: 0 8px 24px rgba(108,99,255,.3); transition: .2s; margin-bottom: 20px; }
  .dictation-audio-btn:hover { transform: scale(1.06); }
  .dictation-tools { display: flex; gap: 8px; justify-content: center; margin: 16px 0; flex-wrap: wrap; }
  .hint-display { font-family: monospace; font-size: 20px; letter-spacing: 4px; color: var(--accent4); margin: 10px 0; min-height: 28px; }
  .example-display { font-size: 13.5px; color: var(--text2); font-style: italic; background: var(--surface2); padding: 12px 16px; border-radius: 8px; margin: 10px 0; min-height: 20px; }

  /* ── WRITING ── */
  .writing-wrap { max-width: 680px; margin: 0 auto; }
  textarea.text-input { resize: vertical; min-height: 90px; line-height: 1.6; }
  .writing-prompt { font-size: 15px; font-weight: 600; background: var(--surface2); border-left: 3px solid var(--accent4); padding: 16px 20px; border-radius: var(--radius-sm); margin-bottom: 16px; line-height: 1.6; }
  .writing-score-box { background: var(--surface2); border-radius: var(--radius-sm); padding: 16px 18px; margin-top: 14px; }
  .writing-score-box .score-big { font-size: 28px; font-weight: 800; color: var(--accent4); }
  .diff-correct { color: var(--success); }
  .diff-wrong { color: var(--danger); text-decoration: line-through; }
  .diff-missing { color: var(--warn); }

  /* ── STATS BAR ── */
  .stats-row { display: grid; grid-template-columns: repeat(6, 1fr); gap: 10px; margin-bottom: 24px; }
  .stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 10px; text-align: center; }
  .stat-card .num { font-size: 22px; font-weight: 800; }
  .stat-card .lbl { font-size: 10.5px; color: var(--text2); margin-top: 2px; line-height: 1.3; }

  /* ── MISC ── */
  .divider { border: none; border-top: 1px solid var(--border); margin: 20px 0; }
  .empty { text-align: center; color: var(--text3); padding: 40px; font-size: 14px; }
  .badge { display: inline-block; font-size: 11px; font-weight: 600; padding: 2px 8px; border-radius: 10px; }

  /* ── UPLOAD ── */
  .upload-zone { border: 2px dashed var(--border); border-radius: var(--radius); padding: 40px; text-align: center; color: var(--text2); cursor: pointer; transition: .2s; }
  .upload-zone:hover { border-color: var(--accent); color: var(--text); }

  /* ── HANZI ── */
  .hanzi-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; display: flex; flex-direction: column; align-items: center; }
  .hanzi-char { font-size: 64px; font-family: 'Noto Sans SC', sans-serif; margin-bottom: 8px; }
  .hanzi-writer-box { display: flex; justify-content: center; margin: 12px 0; }
  .pinyin { font-size: 18px; color: var(--accent); margin-bottom: 4px; }
  .hanzi-meaning { font-size: 14px; color: var(--text2); }
  .hanzi-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 14px; }
  .stroke-controls { display: flex; gap: 8px; justify-content: center; margin-top: 10px; }

  /* ── MOBILE ── */
  #hamburger { display: none; background: none; border: none; color: var(--text); font-size: 22px; cursor: pointer; padding: 8px; }
  @media (max-width: 768px) {
    #hamburger { display: block; position: fixed; top: 12px; left: 12px; z-index: 200; }
    #sidebar { position: fixed; left: -240px; top: 0; height: 100vh; z-index: 150; transition: left .25s; }
    #sidebar.open { left: 0; }
    #main { padding: 64px 16px 20px; }
    .vocab-grid, .hanzi-grid, .folder-grid { grid-template-columns: 1fr; }
    .quiz-options { grid-template-columns: 1fr; }
    .stats-row { grid-template-columns: repeat(3, 1fr); }
    .match-wrap { grid-template-columns: 1fr; }
  }

  @keyframes pop { 0%{transform:scale(.95);opacity:0} 100%{transform:scale(1);opacity:1} }
  .page.active { animation: pop .2s ease; }
  @keyframes shake { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-6px)} 75%{transform:translateX(6px)} }
  .shake { animation: shake .25s; }
  [title] { cursor: help; }
</style>
</head>
<body>
<div id="app">
  <button id="hamburger" onclick="toggleSidebar()">☰</button>

  <!-- SIDEBAR -->
  <nav id="sidebar">
    <div class="logo">
      <h1>VocabMaster</h1>
      <span>Học từ vựng thông minh</span>
    </div>
    <div class="nav-section">Quản lý</div>
    <div class="nav-item active" onclick="goPage('vocab')"><span class="icon">📚</span> Từ vựng &amp; Folder</div>
    <div class="nav-item" onclick="goPage('favorites')"><span class="icon">⭐</span> Yêu thích</div>
    <div class="nav-section">Luyện tập</div>
    <div class="nav-item" onclick="goPage('flashcard')"><span class="icon">🃏</span> Flashcard</div>
    <div class="nav-item" onclick="goPage('quiz')"><span class="icon">✍️</span> Điền từ / Trắc nghiệm</div>
    <div class="nav-item" onclick="goPage('gapfill')"><span class="icon">🧩</span> Gap-fill</div>
    <div class="nav-item" onclick="goPage('matching')"><span class="icon">🔗</span> Matching</div>
    <div class="nav-item" onclick="goPage('dictation')"><span class="icon">🎧</span> Nghe viết</div>
    <div class="nav-item" onclick="goPage('writing')"><span class="icon">📝</span> Writing</div>
    <div class="nav-item" onclick="goPage('reading')"><span class="icon">📖</span> Đọc hiểu</div>
    <div class="nav-section">Tiếng Trung</div>
    <div class="nav-item" onclick="goPage('chinese')"><span class="icon">🈶</span> Hán tự &amp; Nét chữ</div>
    <div class="nav-section">Công cụ</div>
    <div class="nav-item" onclick="goPage('upload')"><span class="icon">⬆️</span> Import dữ liệu</div>
    <div class="sidebar-stats">
      <div class="stat-row"><span>🔵 Học 1 lần</span><b id="sb-0">0</b></div>
      <div class="stat-row"><span>🟣 Học 2 lần</span><b id="sb-1">0</b></div>
      <div class="stat-row"><span>🟡 Học 3 lần</span><b id="sb-2">0</b></div>
      <div class="stat-row"><span>🟠 Học 4 lần</span><b id="sb-3">0</b></div>
      <div class="stat-row"><span>🔴 Chưa thuộc</span><b id="sb-4">0</b></div>
      <div class="stat-row"><span>🟢 Đã nhớ</span><b id="sb-5">0</b></div>
      <div class="xp-bar-wrap">
        <div class="xp-label"><span>⚡ Điểm tích lũy</span><b id="xp-total">0</b></div>
        <div class="xp-bar"><div class="xp-fill" id="xp-fill" style="width:0%"></div></div>
      </div>
    </div>
  </nav>

  <!-- MAIN -->
  <main id="main">

    <!-- ═══════════════ VOCAB + FOLDERS PAGE ═══════════════ -->
    <div id="page-vocab" class="page active">
      <div class="page-header">
        <div>
          <h2>📚 Kho từ vựng</h2>
          <p>Quản lý từ vựng theo folder &amp; lọc theo 6 mức độ thuộc</p>
        </div>
      </div>

      <div class="reading-label">Folder của bạn</div>
      <div class="folder-grid" id="folder-grid"></div>

      <div class="stats-row">
        <div class="stat-card" style="border-color:var(--accent5)"><div class="num" style="color:var(--accent5)" id="st-0">0</div><div class="lbl">Học 1 lần</div></div>
        <div class="stat-card" style="border-color:var(--accent)"><div class="num" style="color:var(--accent)" id="st-1">0</div><div class="lbl">Học 2 lần</div></div>
        <div class="stat-card" style="border-color:var(--warn)"><div class="num" style="color:var(--warn)" id="st-2">0</div><div class="lbl">Học 3 lần</div></div>
        <div class="stat-card" style="border-color:#ffa64d"><div class="num" style="color:#ffa64d" id="st-3">0</div><div class="lbl">Học 4 lần</div></div>
        <div class="stat-card" style="border-color:var(--accent3)"><div class="num" style="color:var(--accent3)" id="st-4">0</div><div class="lbl">Chưa thuộc</div></div>
        <div class="stat-card" style="border-color:var(--success)"><div class="num" style="color:var(--success)" id="st-5">0</div><div class="lbl">Đã nhớ</div></div>
      </div>

      <div class="filter-bar">
        <input type="text" id="search-input" placeholder="🔍 Tìm kiếm từ vựng..." oninput="renderVocab()">
        <select id="filter-folder" onchange="renderVocab()">
          <option value="all">Tất cả folder</option>
        </select>
        <select id="filter-level" onchange="renderVocab()">
          <option value="all">Tất cả mức độ</option>
          <option value="0">Học 1 lần</option>
          <option value="1">Học 2 lần</option>
          <option value="2">Học 3 lần</option>
          <option value="3">Học 4 lần</option>
          <option value="4">Chưa thuộc</option>
          <option value="5">Đã nhớ</option>
        </select>
        <button class="btn secondary sm" onclick="goPage('upload')">⬆️ Import</button>
      </div>
      <div id="vocab-grid" class="vocab-grid"></div>
    </div>

    <!-- ═══════════════ FAVORITES ═══════════════ -->
    <div id="page-favorites" class="page">
      <div class="page-header"><div><h2>⭐ Từ yêu thích</h2><p>Những từ bạn đã đánh dấu để ôn luyện</p></div></div>
      <div id="fav-grid" class="vocab-grid"></div>
    </div>

    <!-- ═══════════════ FLASHCARD ═══════════════ -->
    <div id="page-flashcard" class="page">
      <div class="page-header"><div><h2>🃏 Flashcard</h2><p>Đếm ngược 30s mỗi từ – tự đánh giá mức độ thuộc để phân nhóm ôn tập</p></div></div>
      <div id="fc-setup" class="card">
        <div style="display:flex;flex-direction:column;gap:14px;">
          <div>
            <div class="reading-label">Folder</div>
            <select id="fc-folder" style="background:var(--surface2);border:1px solid var(--border);border-radius:var(--radius-sm);color:var(--text);padding:8px 12px;font-size:13px;outline:none;width:100%;max-width:300px;">
              <option value="all">Tất cả folder</option>
            </select>
          </div>
          <div>
            <div class="reading-label">Nhóm ôn tập</div>
            <div class="group-tabs" id="fc-group-tabs">
              <div class="group-tab active" data-grp="all" onclick="setFCGroup('all')">Tất cả</div>
              <div class="group-tab" data-grp="new" onclick="setFCGroup('new')">🆕 Học 1 lần</div>
              <div class="group-tab" data-grp="learning" onclick="setFCGroup('learning')">📝 Đang học (2-4 lần)</div>
              <div class="group-tab" data-grp="hard" onclick="setFCGroup('hard')">🔴 Chưa thuộc</div>
              <div class="group-tab" data-grp="known" onclick="setFCGroup('known')">🟢 Đã nhớ</div>
            </div>
          </div>
          <div>
            <div class="reading-label">Số lượng từ học</div>
            <div class="count-pills" id="fc-count-pills">
              <div class="count-pill active" data-n="10" onclick="setFCCount(10)">10<span class="pts">+10đ/từ</span></div>
              <div class="count-pill" data-n="20" onclick="setFCCount(20)">20<span class="pts">+15đ/từ</span></div>
              <div class="count-pill" data-n="30" onclick="setFCCount(30)">30<span class="pts">+20đ/từ</span></div>
              <div class="count-pill" data-n="50" onclick="setFCCount(50)">50<span class="pts">+25đ/từ</span></div>
            </div>
          </div>
          <button class="btn" style="align-self:flex-start;" onclick="startFlashcard()">▶ Bắt đầu</button>
        </div>
      </div>
      <div id="fc-game" style="display:none;">
        <div class="fc-info">
          <span id="fc-counter">0 / 0</span>
          <div class="timer-wrap">
            <div class="timer-ring">
              <svg width="36" height="36"><circle class="timer-bg" cx="18" cy="18" r="15"/><circle class="timer-fg" id="fc-timer-circle" cx="18" cy="18" r="15" stroke-dasharray="94.2" stroke-dashoffset="0"/></svg>
              <div class="timer-num" id="fc-timer-num">30</div>
            </div>
          </div>
          <span id="fc-score-display">✅ 0 &nbsp; ❌ 0 &nbsp; ⚡ 0đ</span>
        </div>
        <div class="progress-bar"><div class="progress-fill" id="fc-progress"></div></div>
        <div class="flashcard-wrap">
          <div class="flashcard" id="flashcard" onclick="flipCard()">
            <div class="fc-front">
              <div class="fc-word" id="fc-word"></div>
              <div class="fc-ipa" id="fc-ipa"></div>
              <div class="fc-hint">Nhấn để xem nghĩa</div>
            </div>
            <div class="fc-back">
              <div class="fc-definition" id="fc-def"></div>
              <div class="fc-example" id="fc-ex"></div>
            </div>
          </div>
        </div>
        <div class="fc-controls">
          <button class="btn secondary" onclick="speakWord()">🔊 Phát âm</button>
        </div>
        <div class="level-rate-row" id="fc-level-row"></div>
        <div style="text-align:center;margin-top:16px;">
          <button class="btn secondary sm" onclick="endFlashcard()">🔚 Kết thúc</button>
        </div>
      </div>
      <div id="fc-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="fc-final-pct"></div>
          <div class="label" id="fc-final-label"></div>
          <div class="xp-earned" id="fc-final-xp"></div>
          <button class="btn" onclick="backToFCSetup()">🔄 Học tiếp</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ QUIZ (FILL + MC) ═══════════════ -->
    <div id="page-quiz" class="page">
      <div class="page-header"><div><h2>✍️ Điền từ &amp; Trắc nghiệm</h2><p>Đếm ngược 30s mỗi câu – chọn số lượng câu hỏi để nhận điểm</p></div></div>
      <div id="quiz-setup" class="card">
        <div style="margin-bottom:14px;">
          <div class="reading-label">Dạng câu hỏi</div>
          <div class="quiz-mode-switch">
            <div class="mode-btn active" id="qm-mc" onclick="setQuizMode('mc')">🔘 Trắc nghiệm 4 đáp án</div>
            <div class="mode-btn" id="qm-fill" onclick="setQuizMode('fill')">✏️ Điền từ (fill in the blank)</div>
          </div>
        </div>
        <div style="margin-bottom:14px;">
          <div class="reading-label">Chiều hỏi</div>
          <div class="answer-dir">
            <div class="dir-btn active" id="dir-en-vi" onclick="setQuizDir('en-vi')">🇬🇧 Hỏi Tiếng Anh → 🇻🇳 Trả lời Tiếng Việt</div>
            <div class="dir-btn" id="dir-vi-en" onclick="setQuizDir('vi-en')">🇻🇳 Hỏi Tiếng Việt → 🇬🇧 Trả lời Tiếng Anh</div>
          </div>
        </div>
        <div>
          <div class="reading-label">Số lượng câu hỏi</div>
          <div class="count-pills" id="quiz-count-pills">
            <div class="count-pill active" data-n="10" onclick="setQuizCount(10)">10<span class="pts">+10đ/câu</span></div>
            <div class="count-pill" data-n="20" onclick="setQuizCount(20)">20<span class="pts">+15đ/câu</span></div>
            <div class="count-pill" data-n="30" onclick="setQuizCount(30)">30<span class="pts">+20đ/câu</span></div>
            <div class="count-pill" data-n="50" onclick="setQuizCount(50)">50<span class="pts">+25đ/câu</span></div>
          </div>
        </div>
        <button class="btn" style="margin-top:16px;" onclick="startQuiz()">▶ Bắt đầu kiểm tra</button>
      </div>
      <div id="quiz-game" style="display:none;">
        <div class="quiz-wrap">
          <div class="fc-info">
            <span id="quiz-counter">Câu 0/0</span>
            <div class="timer-wrap">
              <div class="timer-ring">
                <svg width="36" height="36"><circle class="timer-bg" cx="18" cy="18" r="15"/><circle class="timer-fg" id="quiz-timer-circle" cx="18" cy="18" r="15" stroke-dasharray="94.2" stroke-dashoffset="0"/></svg>
                <div class="timer-num" id="quiz-timer-num">30</div>
              </div>
            </div>
            <span id="quiz-score-disp">⚡ 0đ</span>
          </div>
          <div class="progress-bar"><div class="progress-fill" id="quiz-progress"></div></div>
          <div class="quiz-question" id="quiz-q"></div>
          <div id="quiz-mc-opts" class="quiz-options"></div>
          <div id="quiz-fill-area" class="quiz-fill" style="display:none;">
            <input type="text" id="quiz-fill-input" placeholder="Nhập câu trả lời..." onkeydown="if(event.key==='Enter')checkFill()">
            <button class="btn" onclick="checkFill()">Kiểm tra</button>
          </div>
          <div id="quiz-feedback" class="quiz-feedback" style="display:none;"></div>
          <div style="display:flex;gap:10px;margin-top:10px;">
            <button class="btn secondary" id="quiz-audio-btn" onclick="speakCurrent()">🔊</button>
            <button class="btn" id="quiz-next-btn" onclick="nextQuiz()" style="display:none;">Câu tiếp →</button>
          </div>
        </div>
      </div>
      <div id="quiz-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="quiz-final-score"></div>
          <div class="label" id="quiz-final-label"></div>
          <div class="xp-earned" id="quiz-final-xp"></div>
          <button class="btn" onclick="startQuiz()">🔄 Làm lại</button>&nbsp;
          <button class="btn secondary" onclick="backToQuizSetup()">⚙️ Cài đặt</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ GAP-FILL PAGE ═══════════════ -->
    <div id="page-gapfill" class="page">
      <div class="page-header"><div><h2>🧩 Gap-fill</h2><p>Điền từ vào chỗ trống trong câu có ngữ cảnh cụ thể (thứ tự xáo trộn)</p></div></div>
      <div id="gapfill-setup" class="card">
        <div class="reading-label">Số lượng câu hỏi</div>
        <div class="count-pills" id="gapfill-count-pills">
          <div class="count-pill active" data-n="10" onclick="setGapfillCount(10)">10<span class="pts">+10đ/câu</span></div>
          <div class="count-pill" data-n="20" onclick="setGapfillCount(20)">20<span class="pts">+15đ/câu</span></div>
          <div class="count-pill" data-n="30" onclick="setGapfillCount(30)">30<span class="pts">+20đ/câu</span></div>
          <div class="count-pill" data-n="50" onclick="setGapfillCount(50)">50<span class="pts">+25đ/câu</span></div>
        </div>
        <button class="btn" style="margin-top:16px;" onclick="startGapfill()">▶ Bắt đầu</button>
      </div>
      <div id="gapfill-game" style="display:none;">
        <div class="quiz-wrap">
          <div class="fc-info">
            <span id="gapfill-counter">Câu 0/0</span>
            <div class="timer-wrap">
              <div class="timer-ring">
                <svg width="36" height="36"><circle class="timer-bg" cx="18" cy="18" r="15"/><circle class="timer-fg" id="gapfill-timer-circle" cx="18" cy="18" r="15" stroke-dasharray="94.2" stroke-dashoffset="0"/></svg>
                <div class="timer-num" id="gapfill-timer-num">30</div>
              </div>
            </div>
            <span id="gapfill-score-disp">⚡ 0đ</span>
          </div>
          <div class="progress-bar"><div class="progress-fill" id="gapfill-progress"></div></div>
          <div class="quiz-question" id="gapfill-sentence"></div>
          <div class="quiz-fill">
            <input type="text" id="gapfill-input" placeholder="Nhập từ thích hợp..." onkeydown="if(event.key==='Enter')checkGapfill()">
            <button class="btn" onclick="checkGapfill()">Kiểm tra</button>
          </div>
          <div id="gapfill-feedback" class="quiz-feedback" style="display:none;"></div>
          <button class="btn" id="gapfill-next-btn" onclick="nextGapfill()" style="display:none;">Câu tiếp →</button>
        </div>
      </div>
      <div id="gapfill-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="gapfill-final-score"></div>
          <div class="label" id="gapfill-final-label"></div>
          <div class="xp-earned" id="gapfill-final-xp"></div>
          <button class="btn" onclick="startGapfill()">🔄 Làm lại</button>&nbsp;
          <button class="btn secondary" onclick="backToGapfillSetup()">⚙️ Cài đặt</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ MATCHING PAGE ═══════════════ -->
    <div id="page-matching" class="page">
      <div class="page-header"><div><h2>🔗 Matching</h2><p>Nối từ vựng với định nghĩa tiếng Anh tương ứng</p></div></div>
      <div id="matching-setup" class="card">
        <div class="reading-label">Số lượng từ (mỗi vòng tối đa 10 cặp)</div>
        <div class="count-pills" id="matching-count-pills">
          <div class="count-pill active" data-n="10" onclick="setMatchingCount(10)">10<span class="pts">+10đ/cặp</span></div>
          <div class="count-pill" data-n="20" onclick="setMatchingCount(20)">20<span class="pts">+15đ/cặp</span></div>
          <div class="count-pill" data-n="30" onclick="setMatchingCount(30)">30<span class="pts">+20đ/cặp</span></div>
          <div class="count-pill" data-n="50" onclick="setMatchingCount(50)">50<span class="pts">+25đ/cặp</span></div>
        </div>
        <button class="btn" style="margin-top:16px;" onclick="startMatching()">▶ Bắt đầu</button>
      </div>
      <div id="matching-game" style="display:none;">
        <div class="fc-info" style="max-width:720px;">
          <span id="matching-counter">Vòng 1</span>
          <span id="matching-score-disp">⚡ 0đ &nbsp; ✅ 0 &nbsp; ❌ 0</span>
        </div>
        <div class="progress-bar" style="max-width:720px;"><div class="progress-fill" id="matching-progress"></div></div>
        <div class="match-wrap">
          <div class="match-col" id="match-col-word"></div>
          <div class="match-col" id="match-col-def"></div>
        </div>
        <div style="text-align:center;margin-top:18px;">
          <button class="btn secondary sm" onclick="endMatching()">🔚 Kết thúc</button>
        </div>
      </div>
      <div id="matching-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="matching-final-score"></div>
          <div class="label" id="matching-final-label"></div>
          <div class="xp-earned" id="matching-final-xp"></div>
          <button class="btn" onclick="startMatching()">🔄 Làm lại</button>&nbsp;
          <button class="btn secondary" onclick="backToMatchingSetup()">⚙️ Cài đặt</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ DICTATION / NGHE VIẾT ═══════════════ -->
    <div id="page-dictation" class="page">
      <div class="page-header"><div><h2>🎧 Nghe viết</h2><p>Nghe phát âm rồi gõ lại từ tiếng Anh bạn nghe được</p></div></div>
      <div id="dictation-setup" class="card">
        <div class="reading-label">Số lượng từ</div>
        <div class="count-pills" id="dictation-count-pills">
          <div class="count-pill active" data-n="10" onclick="setDictationCount(10)">10<span class="pts">+10đ/từ</span></div>
          <div class="count-pill" data-n="20" onclick="setDictationCount(20)">20<span class="pts">+15đ/từ</span></div>
          <div class="count-pill" data-n="30" onclick="setDictationCount(30)">30<span class="pts">+20đ/từ</span></div>
          <div class="count-pill" data-n="50" onclick="setDictationCount(50)">50<span class="pts">+25đ/từ</span></div>
        </div>
        <button class="btn" style="margin-top:16px;" onclick="startDictation()">▶ Bắt đầu</button>
      </div>
      <div id="dictation-game" style="display:none;">
        <div class="dictation-box">
          <div class="fc-info" style="max-width:560px;">
            <span id="dictation-counter">Từ 0/0</span>
            <div class="timer-wrap">
              <div class="timer-ring">
                <svg width="36" height="36"><circle class="timer-bg" cx="18" cy="18" r="15"/><circle class="timer-fg" id="dictation-timer-circle" cx="18" cy="18" r="15" stroke-dasharray="94.2" stroke-dashoffset="0"/></svg>
                <div class="timer-num" id="dictation-timer-num">30</div>
              </div>
            </div>
            <span id="dictation-score-disp">⚡ 0đ</span>
          </div>
          <div class="progress-bar" style="max-width:560px;"><div class="progress-fill" id="dictation-progress"></div></div>
          <button class="dictation-audio-btn" onclick="playDictationAudio()">🔊</button>
          <div><button class="btn secondary sm" onclick="playDictationAudio()">🔁 Nghe lại</button></div>
          <div class="example-display" id="dictation-example" style="display:none;"></div>
          <div class="hint-display" id="dictation-hint"></div>
          <input type="text" class="text-input" id="dictation-input" placeholder="Gõ từ bạn nghe được..." style="margin-top:14px;" onkeydown="if(event.key==='Enter')checkDictation()">
          <div class="dictation-tools">
            <button class="btn secondary sm" onclick="showDictationExample()">📄 Xem ví dụ</button>
            <button class="btn secondary sm" onclick="showDictationHint()">💡 Gợi ý</button>
            <button class="btn success sm" onclick="checkDictation()">✅ Kiểm tra</button>
          </div>
          <div id="dictation-feedback" class="quiz-feedback" style="display:none;"></div>
          <button class="btn" id="dictation-next-btn" onclick="nextDictation()" style="display:none;">Từ tiếp →</button>
        </div>
      </div>
      <div id="dictation-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="dictation-final-score"></div>
          <div class="label" id="dictation-final-label"></div>
          <div class="xp-earned" id="dictation-final-xp"></div>
          <button class="btn" onclick="startDictation()">🔄 Làm lại</button>&nbsp;
          <button class="btn secondary" onclick="backToDictationSetup()">⚙️ Cài đặt</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ WRITING PAGE ═══════════════ -->
    <div id="page-writing" class="page">
      <div class="page-header"><div><h2>📝 Writing</h2><p>Biên phiên dịch, dịch ngược và dictation câu</p></div></div>
      <div id="writing-setup" class="card">
        <div class="reading-label">Chọn bài tập</div>
        <div class="quiz-mode-switch">
          <div class="mode-btn active" id="wm-vi-en" onclick="setWritingMode('vi-en')">🇻🇳→🇬🇧 Biên phiên dịch</div>
          <div class="mode-btn" id="wm-back" onclick="setWritingMode('back')">🔄 Dịch ngược</div>
          <div class="mode-btn" id="wm-dict" onclick="setWritingMode('dict')">🎧 Dictation câu</div>
        </div>
        <div class="reading-label" style="margin-top:14px;">Số lượng câu</div>
        <div class="count-pills" id="writing-count-pills">
          <div class="count-pill active" data-n="10" onclick="setWritingCount(10)">10<span class="pts">+10đ/câu</span></div>
          <div class="count-pill" data-n="20" onclick="setWritingCount(20)">20<span class="pts">+15đ/câu</span></div>
          <div class="count-pill" data-n="30" onclick="setWritingCount(30)">30<span class="pts">+20đ/câu</span></div>
          <div class="count-pill" data-n="50" onclick="setWritingCount(50)">50<span class="pts">+25đ/câu</span></div>
        </div>
        <button class="btn" style="margin-top:16px;" onclick="startWriting()">▶ Bắt đầu</button>
      </div>
      <div id="writing-game" style="display:none;">
        <div class="writing-wrap">
          <div class="fc-info">
            <span id="writing-counter">Câu 0/0</span>
            <span id="writing-score-disp">⚡ 0đ</span>
          </div>
          <div class="progress-bar"><div class="progress-fill" id="writing-progress"></div></div>
          <div class="writing-prompt" id="writing-prompt"></div>
          <div id="writing-audio-row" style="display:none;margin-bottom:12px;">
            <button class="btn secondary sm" onclick="playWritingAudio()">🔊 Nghe câu</button>
          </div>
          <textarea class="text-input" id="writing-input" placeholder="Nhập câu trả lời của bạn..."></textarea>
          <button class="btn success" onclick="checkWriting()">✅ Chấm điểm</button>
          <div id="writing-score-box" class="writing-score-box" style="display:none;"></div>
          <button class="btn" id="writing-next-btn" onclick="nextWriting()" style="display:none;margin-top:12px;">Câu tiếp →</button>
        </div>
      </div>
      <div id="writing-result" style="display:none;">
        <div class="quiz-score">
          <div class="big" id="writing-final-score"></div>
          <div class="label" id="writing-final-label"></div>
          <div class="xp-earned" id="writing-final-xp"></div>
          <button class="btn" onclick="startWriting()">🔄 Làm lại</button>&nbsp;
          <button class="btn secondary" onclick="backToWritingSetup()">⚙️ Cài đặt</button>
        </div>
      </div>
    </div>

    <!-- ═══════════════ READING PAGE ═══════════════ -->
    <div id="page-reading" class="page">
      <div class="page-header"><div><h2>📖 Đọc hiểu</h2><p>Điền từ vào đoạn văn dựa theo ngữ cảnh</p></div></div>
      <div id="reading-select" class="card">
        <div class="reading-label">Chọn bài đọc</div>
        <div id="reading-list" style="display:flex;flex-direction:column;gap:8px;margin-top:8px;"></div>
      </div>
      <div id="reading-game" style="display:none;">
        <div class="reading-wrap">
          <div style="margin-bottom:16px;">
            <div class="reading-label">Danh sách từ vựng</div>
            <div id="reading-wordbank" class="word-bank"></div>
          </div>
          <div class="reading-label">Bài đọc – Điền từ thích hợp</div>
          <div class="reading-passage" id="reading-passage"></div>
          <div style="display:flex;gap:10px;flex-wrap:wrap;">
            <button class="btn success" onclick="checkReading()">✅ Kiểm tra</button>
            <button class="btn secondary" onclick="resetReading()">🔄 Làm lại</button>
            <button class="btn secondary" onclick="showReadingHint()">💡 Gợi ý</button>
            <button class="btn secondary" onclick="backToReadingSelect()">← Chọn bài khác</button>
          </div>
          <div id="reading-result" style="display:none;margin-top:16px;"></div>
        </div>
      </div>
    </div>

    <!-- ═══════════════ CHINESE PAGE ═══════════════ -->
    <div id="page-chinese" class="page">
      <div class="page-header"><div><h2>🈶 Tiếng Trung – Hán tự &amp; Nét chữ</h2><p>Học nét chữ với HanziWriter animation</p></div></div>
      <div class="filter-bar">
        <input type="text" id="zh-search" placeholder="🔍 Tìm Hán tự..." oninput="renderChinese()">
        <button class="btn secondary sm" onclick="goPage('upload')">⬆️ Import từ vựng Trung</button>
      </div>
      <div id="hanzi-grid" class="hanzi-grid"></div>
    </div>

    <!-- ═══════════════ UPLOAD PAGE ═══════════════ -->
    <div id="page-upload" class="page">
      <div class="page-header"><div><h2>⬆️ Import dữ liệu</h2><p>Tải lên file từ vựng JSON</p></div></div>
      <div class="card">
        <div class="reading-label">Thêm vào folder</div>
        <select id="upload-target-folder" style="background:var(--surface2);border:1px solid var(--border);border-radius:var(--radius-sm);color:var(--text);padding:8px 12px;font-size:13px;outline:none;width:100%;max-width:300px;margin-bottom:14px;">
        </select>
        <div class="reading-label">File JSON – Từ vựng Tiếng Anh</div>
        <p style="font-size:13px;color:var(--text2);margin-bottom:12px;">Định dạng: mảng các object với các trường: <code style="color:var(--accent);">word, ipa, pos, definition, definitionEN, example, synonyms, antonyms, level</code></p>
        <div class="upload-zone" onclick="document.getElementById('json-upload').click()">
          📂 Nhấn để chọn file JSON hoặc kéo thả vào đây
          <input type="file" id="json-upload" accept=".json" style="display:none;" onchange="importJSON(event)">
        </div>
        <div id="import-status" style="margin-top:12px;font-size:13.5px;color:var(--accent2);display:none;"></div>
      </div>
      <div class="card">
        <div class="reading-label">File JSON – Từ vựng Tiếng Trung</div>
        <p style="font-size:13px;color:var(--text2);margin-bottom:12px;">Định dạng: mảng với trường: <code style="color:var(--accent);">hanzi, pinyin, meaning, example, level</code></p>
        <div class="upload-zone" onclick="document.getElementById('json-zh-upload').click()">
          📂 Nhấn để chọn file JSON Tiếng Trung
          <input type="file" id="json-zh-upload" accept=".json" style="display:none;" onchange="importChineseJSON(event)">
        </div>
      </div>
      <div class="card">
        <div class="reading-label">Mẫu JSON tham khảo – Tiếng Anh</div>
        <pre style="background:var(--surface2);padding:14px;border-radius:8px;font-size:12px;color:var(--accent2);overflow-x:auto;">[
  {
    "word": "perseverance",
    "ipa": "/ˌpərsəˈvɪrəns/",
    "pos": "noun",
    "definition": "Kiên trì; sự cố gắng liên tục dù gặp khó khăn",
    "definitionEN": "Continued effort to achieve despite difficulties",
    "example": "She showed great perseverance in learning English every day.",
    "synonyms": "Persistence, Tenacity",
    "antonyms": "Laziness, Giving up",
    "level": 0
  }
]</pre>
        <p style="font-size:12px;color:var(--text3);margin-top:8px;">level: 0=Học 1 lần, 1=Học 2 lần, 2=Học 3 lần, 3=Học 4 lần, 4=Chưa thuộc, 5=Đã nhớ</p>
        <button class="btn secondary sm" style="margin-top:10px;" onclick="downloadSample()">⬇️ Tải file mẫu</button>
      </div>
    </div>

  </main>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/hanzi-writer/3.5.2/hanzi-writer.min.js"></script>

<script>
// ═══════════════════════════════════════════════════════
//  CONSTANTS
// ═══════════════════════════════════════════════════════
const LEVEL_NAMES = ['Học 1 lần', 'Học 2 lần', 'Học 3 lần', 'Học 4 lần', 'Chưa thuộc', 'Đã nhớ'];
const LEVEL_ICONS = ['🔵', '🟣', '🟡', '🟠', '🔴', '🟢'];
const TIME_PER_ITEM = 30; // giây đếm ngược mỗi từ/câu
const POINTS_BY_COUNT = { 10: 10, 20: 15, 30: 20, 50: 25 };

// ═══════════════════════════════════════════════════════
//  DATA STORE
// ═══════════════════════════════════════════════════════
let vocabData = [];
let chineseData = [];
let folders = [];
let favorites = new Set();
let totalXP = 0;
let activeFolderId = 'all';

const BUILT_IN_VOCAB = [
  { word: "Perseverance", ipa: "/ˌpərsəˈvɪrəns/", pos: "noun", definition: "Kiên trì; sự cố gắng liên tục dù gặp khó khăn", definitionEN: "Continued effort to do or achieve something despite difficulties", example: "She showed great perseverance in learning English every day.", synonyms: "Persistence, Tenacity", antonyms: "Laziness, Giving up", level: 1, folder: "core" },
  { word: "Eloquent", ipa: "/ˈɛləkwənt/", pos: "adjective", definition: "Hùng hồn, diễn đạt lưu loát và thuyết phục", definitionEN: "Fluent or persuasive in speaking or writing", example: "The speaker was so eloquent that everyone listened attentively.", synonyms: "Articulate, Fluent", antonyms: "Inarticulate, Tongue-tied", level: 3, folder: "core" },
  { word: "Ambiguous", ipa: "/æmˈbɪgjuəs/", pos: "adjective", definition: "Mơ hồ, có thể hiểu theo nhiều cách", definitionEN: "Open to more than one interpretation; not having one obvious meaning", example: "The instructions were ambiguous and confused the students.", synonyms: "Unclear, Vague", antonyms: "Clear, Explicit", level: 2, folder: "core" },
  { word: "Resilient", ipa: "/rɪˈzɪljənt/", pos: "adjective", definition: "Kiên cường; có khả năng phục hồi nhanh từ khó khăn", definitionEN: "Able to recover quickly from difficult conditions", example: "Children are often more resilient than adults think.", synonyms: "Tough, Adaptable", antonyms: "Fragile, Vulnerable", level: 2, folder: "core" },
  { word: "Preliminary", ipa: "/prɪˈlɪməˌnɛri/", pos: "adjective", definition: "Sơ bộ; diễn ra trước sự việc quan trọng hơn", definitionEN: "Coming before a more important action or event", example: "The team underwent preliminary tests before the main competition started.", synonyms: "Initial, Preparatory", antonyms: "Final, Closing", level: 4, folder: "core" },
  { word: "Trivial", ipa: "/ˈtrɪviəl/", pos: "adjective", definition: "Tầm thường, không quan trọng", definitionEN: "Of little importance or value", example: "The mistake was so trivial that it didn't affect the final result.", synonyms: "Insignificant, Minor", antonyms: "Important, Significant", level: 2, folder: "core" },
  { word: "Culprit", ipa: "/ˈkəlprɪt/", pos: "noun", definition: "Thủ phạm, người gây ra tội lỗi hoặc vấn đề", definitionEN: "A person responsible for a crime or problem", example: "The police finally found the culprit behind the robbery.", synonyms: "Offender, Criminal", antonyms: "Victim, Innocent", level: 2, folder: "core" },
  { word: "Mobilize", ipa: "/ˈmoʊbəˌlaɪz/", pos: "verb", definition: "Huy động, tổ chức và chuẩn bị cho hành động", definitionEN: "To organize and prepare for action", example: "The government mobilized resources to deal with the natural disaster.", synonyms: "Organize, Deploy", antonyms: "Demobilize, Disperse", level: 2, folder: "core" },
  { word: "Topple", ipa: "/ˈtɑpəl/", pos: "verb", definition: "Lật đổ; rơi hoặc làm rơi", definitionEN: "To fall or cause something to fall", example: "The strong winds toppled several trees in the area.", synonyms: "Overthrow, Knock down", antonyms: "Stabilize, Support", level: 2, folder: "core" },
  { word: "Frayed", ipa: "/freɪd/", pos: "adjective", definition: "Sờn, căng thẳng; bị mòn hoặc mỏi mệt", definitionEN: "Worn out or strained", example: "After a long argument, their patience was completely frayed.", synonyms: "Worn, Strained", antonyms: "Fresh, Calm", level: 2, folder: "core" },
  { word: "Piled up", ipa: "/paɪld əp/", pos: "phrasal verb", definition: "Chồng chất, tích lũy với số lượng lớn", definitionEN: "To accumulate in large amounts", example: "Work has piled up while I was on vacation.", synonyms: "Accumulate, Build up", antonyms: "Decrease, Reduce", level: 2, folder: "core" },
  { word: "Intact", ipa: "/ɪnˈtækt/", pos: "adjective", definition: "Nguyên vẹn; không bị hư hại hoặc thay đổi", definitionEN: "Not damaged or altered", example: "The ancient building remained intact after the earthquake.", synonyms: "Undamaged, Whole", antonyms: "Damaged, Broken", level: 2, folder: "core" },
  { word: "Ruminate", ipa: "/ˈrumɪˌneɪt/", pos: "verb", definition: "Suy ngẫm sâu; nhai lại (nghĩa bóng là ngẫm nghĩ)", definitionEN: "Think deeply about something", example: "He tends to ruminate on past mistakes instead of moving forward.", synonyms: "Contemplate, Ponder", antonyms: "Ignore, Disregard", level: 2, folder: "core" },
  { word: "Erratic", ipa: "/ɪˈrætɪk/", pos: "adjective", definition: "Thất thường; không thể đoán trước hoặc nhất quán", definitionEN: "Unpredictable or inconsistent", example: "His erratic behavior made everyone uncomfortable.", synonyms: "Unpredictable, Irregular", antonyms: "Stable, Consistent", level: 2, folder: "core" },
  { word: "Endowment", ipa: "/ɛnˈdaʊmənt/", pos: "noun", definition: "Quỹ tài trợ; năng khiếu tự nhiên", definitionEN: "A donation or natural gift", example: "The university received a large endowment to support research.", synonyms: "Fund, Grant", antonyms: "Lack, Deficiency", level: 2, folder: "core" },
  { word: "Upheaval", ipa: "/əˈphivəlz/", pos: "noun", definition: "Biến động lớn; sự xáo trộn hoặc thay đổi triệt để", definitionEN: "Great changes or disturbances that cause problems", example: "The company underwent massive financial upheavals during the recession.", synonyms: "Turmoil, Disruption", antonyms: "Stability, Order", level: 2, folder: "core" },
  { word: "Contemplation", ipa: "/ˌkɑntəmˈpleɪʃən/", pos: "noun", definition: "Sự suy ngẫm, trầm tư; cân nhắc kỹ lưỡng", definitionEN: "Deep reflective thought", example: "She spent hours in deep contemplation before making her final decision.", synonyms: "Meditation, Reflection", antonyms: "Ignorance, Neglect", level: 0, folder: "advanced" },
  { word: "Robust", ipa: "/roʊˈbəst/", pos: "adjective", definition: "Mạnh mẽ, kiên cố, vững vàng", definitionEN: "Strong and healthy; vigorous; sturdy in construction", example: "The company developed a robust economic model to withstand market crashes.", synonyms: "Strong, Sturdy, Resilient", antonyms: "Weak, Fragile", level: 0, folder: "advanced" },
  { word: "Bribe", ipa: "/braɪb/", pos: "noun/verb", definition: "Hối lộ, đút lót", definitionEN: "Dishonestly persuade someone by a gift of money", example: "He was accused of offering a bribe to a government official.", synonyms: "Payoff, Kickback, Induce", antonyms: "N/A", level: 0, folder: "advanced" },
  { word: "Unconventional", ipa: "/ˌənkənˈvɛnʃənəl/", pos: "adjective", definition: "Độc đáo, trái với thông thường, không theo quy lối cũ", definitionEN: "Not conforming to what is generally done or believed", example: "She is known for her unconventional teaching methods that students love.", synonyms: "Unusual, Unorthodox", antonyms: "Conventional, Traditional", level: 0, folder: "advanced" },
  { word: "Fraud", ipa: "/frɔd/", pos: "noun", definition: "Sự gian lận, lừa gạt", definitionEN: "Wrongful deception intended to result in financial gain", example: "The investment scheme turned out to be a massive corporate fraud.", synonyms: "Deception, Scam, Trickery", antonyms: "Honesty, Integrity", level: 0, folder: "advanced" },
  { word: "Embezzle", ipa: "/ɪmˈbɛzəlɪŋ/", pos: "verb", definition: "Tham ô, biển thủ công quỹ", definitionEN: "Stealing or misappropriating money placed in one's trust", example: "The accountant was arrested for embezzling millions from the charity fund.", synonyms: "Misappropriating, Stealing", antonyms: "Returning, Compensating", level: 0, folder: "advanced" },
  { word: "Patron", ipa: "/ˈpeɪtrən/", pos: "noun", definition: "Nhà tài trợ, khách hàng quen, hộ pháp", definitionEN: "A person who gives financial or other support to a cause", example: "The college art museum is looking for new patrons.", synonyms: "Sponsor, Backer, Supporter", antonyms: "Opponent, Detractor", level: 0, folder: "advanced" },
  { word: "Trajectory", ipa: "/trəˈdʒektəri/", pos: "noun", definition: "Quỹ đạo, đường đạn", definitionEN: "The path followed by an object moving under forces", example: "The missile's trajectory was constantly tracked by radar.", synonyms: "Path, Course, Route", antonyms: "N/A", level: 0, folder: "advanced" },
  { word: "Astray", ipa: "/əˈstreɪ/", pos: "adverb", definition: "Lạc đường, lầm đường lạc lối", definitionEN: "Away from the correct path or direction", example: "The letter must have gone astray in the post.", synonyms: "Lost, Off track, Wandering", antonyms: "On track, Right", level: 0, folder: "advanced" },
  { word: "Inferior", ipa: "/ɪnˈfɪəriər/", pos: "adjective", definition: "Kém hơn, hạ đẳng, cấp dưới", definitionEN: "Lower in rank, status, or quality", example: "His later work was vastly inferior to his early masterpieces.", synonyms: "Substandard, Second-rate", antonyms: "Superior, Better", level: 0, folder: "advanced" },
  { word: "Recalibrate", ipa: "/ˌriːˈkælɪbreɪtɪd/", pos: "verb", definition: "Hiệu chỉnh lại, điều chỉnh lại thông số", definitionEN: "Calibrate or adjust an instrument again", example: "The scientists recalibrated the laboratory equipment before the experiment.", synonyms: "Readjust, Fine-tune", antonyms: "Desensitize", level: 0, folder: "advanced" },
  { word: "Dabble", ipa: "/ˈdæbəl/", pos: "verb", definition: "Học đòi, làm chơi bời; tham gia hời hợt", definitionEN: "Take part in an activity in a casual or superficial way", example: "I try everything and I'll just dabble my feet into certain things.", synonyms: "Tinker, Flirt with", antonyms: "Take seriously, Dedicate", level: 0, folder: "advanced" },
  { word: "Demeanor", ipa: "/dɪˈmiːnər/", pos: "noun", definition: "Thái độ, cách cư xử, diện mạo", definitionEN: "Outward behavior or bearing; attitude", example: "She has the calm demeanor of a natural leader.", synonyms: "Behavior, Manner, Conduct", antonyms: "N/A", level: 0, folder: "advanced" },
  { word: "Dismantle", ipa: "/dɪsˈmæntl/", pos: "verb", definition: "Tháo dỡ (máy móc), phá hủy", definitionEN: "Take a machine or structure to pieces", example: "The mechanic had to dismantle the engine to find the problem.", synonyms: "Take apart, Disassemble", antonyms: "Assemble, Build, Construct", level: 0, folder: "advanced" },
  { word: "Emulate", ipa: "/ˈemjuleɪt/", pos: "verb", definition: "Tranh đua, bắt chước; cố vượt qua ai đó", definitionEN: "Match or surpass, typically by imitation", example: "They hope to emulate the success of other software companies.", synonyms: "Imitate, Copy, Rival", antonyms: "Neglect, Ignore", level: 0, folder: "advanced" },
  { word: "Indebted", ipa: "/ɪnˈdetɪd/", pos: "adjective", definition: "Mắc nợ, mang ơn, biết ơn", definitionEN: "Owing money or gratitude for a service", example: "Giving that envelope to you, you feel indebted to them.", synonyms: "Grateful, Obligated", antonyms: "Ungrateful, Unthankful", level: 0, folder: "advanced" },
  { word: "Psychotic", ipa: "/saɪˈkɒtɪk/", pos: "adjective", definition: "Mắc bệnh tâm thần phân liệt, loạn thần", definitionEN: "Relating to psychosis; having a severe mental disorder", example: "He showed psychotic symptoms after the trauma.", synonyms: "Deranged, Disturbed", antonyms: "Sane, Rational", level: 4, folder: "advanced" },
  { word: "Bane", ipa: "/beɪn/", pos: "noun", definition: "Nguyên nhân của sự phiền não hoặc bất hạnh", definitionEN: "A cause of great distress or annoyance", example: "Mosquitoes are the bane of summer evenings.", synonyms: "Curse, Affliction, Plague", antonyms: "Blessing, Benefit", level: 4, folder: "advanced" },
  { word: "Endeavor", ipa: "/ɪnˈdevər/", pos: "noun/verb", definition: "Nỗ lực, cố gắng; công việc hoặc dự án nghiêm túc", definitionEN: "An attempt to achieve a goal; earnest work", example: "Space exploration is humanity's greatest endeavor.", synonyms: "Effort, Attempt, Undertaking", antonyms: "Laziness, Inactivity", level: 4, folder: "advanced" },
  { word: "Revival", ipa: "/rɪˈvaɪvəl/", pos: "noun", definition: "Sự hồi sinh, sự khôi phục lại", definitionEN: "Improvement in condition, strength, or popularity", example: "The city saw a cultural revival in the arts district.", synonyms: "Resurgence, Renewal, Renaissance", antonyms: "Decline, Downfall", level: 4, folder: "advanced" },
  { word: "Sentiment", ipa: "/ˈsɛntɪmənt/", pos: "noun", definition: "Tình cảm, cảm xúc, quan điểm", definitionEN: "A view or opinion held based on feeling", example: "Public sentiment shifted quickly after the announcement.", synonyms: "Feeling, Emotion, Opinion", antonyms: "Indifference, Apathy", level: 4, folder: "advanced" },
  { word: "Subdued", ipa: "/səbˈdjuːd/", pos: "adjective", definition: "Yên lặng, trầm lắng; kém mạnh mẽ hơn bình thường", definitionEN: "Quiet and reflective; lacking in intensity", example: "After the loss, the team was noticeably subdued.", synonyms: "Quiet, Muted, Restrained", antonyms: "Exuberant, Lively", level: 4, folder: "advanced" },
  { word: "Hostage", ipa: "/ˈhɒstɪdʒ/", pos: "noun", definition: "Con tin", definitionEN: "A person held as security for the fulfillment of a condition", example: "The kidnappers demanded ransom for the hostage.", synonyms: "Captive, Prisoner", antonyms: "Captor, Liberator", level: 4, folder: "advanced" },
  { word: "Redact", ipa: "/rɪˈdækt/", pos: "verb", definition: "Biên tập; che giấu thông tin nhạy cảm", definitionEN: "Edit text for publication by removing sensitive parts", example: "The classified documents were heavily redacted before release.", synonyms: "Censor, Edit, Expunge", antonyms: "Reveal, Disclose", level: 4, folder: "advanced" },
  { word: "Accommodate", ipa: "/əˈkɒmədeɪt/", pos: "verb", definition: "Chứa chỗ; thích nghi; đáp ứng nhu cầu của ai", definitionEN: "Provide space or adapt to needs", example: "The hotel can accommodate up to 300 guests.", synonyms: "House, Adapt, Contain", antonyms: "Exclude, Reject", level: 4, folder: "advanced" },
  { word: "Disposable", ipa: "/dɪˈspoʊzəbl/", pos: "adjective", definition: "Dùng một lần; có thể bỏ đi; thu nhập khả dụng", definitionEN: "Designed to be used once then discarded", example: "The café switched from disposable cups to reusable ones.", synonyms: "Throwaway, Single-use", antonyms: "Reusable, Permanent", level: 4, folder: "advanced" },
  { word: "Hijack", ipa: "/ˈhaɪdʒæk/", pos: "verb", definition: "Chiếm đoạt; cướp (máy bay, xe); lấy lại quyền kiểm soát", definitionEN: "Illegally seize control of a vehicle or redirect for one's own purpose", example: "Hackers hijacked the social media accounts.", synonyms: "Seize, Commandeer, Usurp", antonyms: "Surrender, Release", level: 4, folder: "advanced" },
  { word: "Synthetic", ipa: "/sɪnˈθɛtɪk/", pos: "adjective", definition: "Tổng hợp, nhân tạo; không tự nhiên", definitionEN: "Made by chemical synthesis rather than natural processes", example: "Synthetic fibers are more durable than natural ones.", synonyms: "Artificial, Man-made", antonyms: "Natural, Organic", level: 4, folder: "advanced" },
  { word: "Potent", ipa: "/ˈpoʊtənt/", pos: "adjective", definition: "Mạnh mẽ, hiệu lực cao; có ảnh hưởng lớn", definitionEN: "Having great power, influence, or effect", example: "The new drug is far more potent than previous treatments.", synonyms: "Powerful, Effective, Strong", antonyms: "Weak, Impotent", level: 4, folder: "advanced" },
  { word: "Lucrative", ipa: "/ˈluːkrətɪv/", pos: "adjective", definition: "Sinh lời, mang lại nhiều lợi nhuận", definitionEN: "Producing a great deal of profit", example: "The tech startup proved to be extremely lucrative.", synonyms: "Profitable, Rewarding", antonyms: "Unprofitable, Costly", level: 4, folder: "advanced" },
  { word: "Integrity", ipa: "/ɪnˈtɛgrɪti/", pos: "noun", definition: "Sự chính trực, liêm chính; tính toàn vẹn", definitionEN: "The quality of being honest and having strong moral principles", example: "Her integrity made her the most trusted person on the team.", synonyms: "Honesty, Uprightness", antonyms: "Dishonesty, Corruption", level: 4, folder: "advanced" },
  { word: "Insidious", ipa: "/ɪnˈsɪdiəs/", pos: "adjective", definition: "Thâm hiểm, xảo quyệt; nguy hiểm theo cách tinh vi", definitionEN: "Proceeding in a gradual, subtle but harmful way", example: "The insidious spread of misinformation threatened democracy.", synonyms: "Sneaky, Treacherous, Subtle", antonyms: "Harmless, Benign", level: 4, folder: "advanced" },
  { word: "Susceptible", ipa: "/səˈsɛptɪbl/", pos: "adjective", definition: "Dễ bị tổn thương; dễ bị ảnh hưởng bởi", definitionEN: "Likely to be influenced or harmed by a particular thing", example: "Young children are more susceptible to infections.", synonyms: "Vulnerable, Prone, Sensitive", antonyms: "Resistant, Immune", level: 4, folder: "advanced" },
  { word: "Stipulate", ipa: "/ˈstɪpjuleɪt/", pos: "verb", definition: "Quy định rõ ràng, điều kiện hóa; nêu như điều kiện", definitionEN: "Demand or specify as part of an agreement", example: "The contract stipulated that payment was due within 30 days.", synonyms: "Specify, Require, Mandate", antonyms: "Allow, Permit", level: 4, folder: "advanced" },
  { word: "Abstain", ipa: "/əbˈsteɪn/", pos: "verb", definition: "Kiêng, tự kiềm chế; bỏ phiếu trắng", definitionEN: "Restrain oneself from doing or enjoying something", example: "He chose to abstain from alcohol during the competition.", synonyms: "Refrain, Withhold, Forbear", antonyms: "Indulge, Partake", level: 4, folder: "advanced" },
  { word: "Anhedonia", ipa: "/ˌænhɪˈdoʊniə/", pos: "noun", definition: "Mất khả năng cảm nhận niềm vui", definitionEN: "The inability to feel pleasure in normally pleasurable activities", example: "Depression often causes anhedonia in patients.", synonyms: "Joylessness, Apathy", antonyms: "Pleasure, Enjoyment", level: 4, folder: "advanced" },
];

const BUILT_IN_CHINESE = [
  { hanzi: "学习", pinyin: "xuéxí", meaning: "Học tập / to study", example: "我每天都在学习。", level: 1 },
  { hanzi: "朋友", pinyin: "péngyou", meaning: "Bạn bè / friend", example: "他是我的好朋友。", level: 1 },
  { hanzi: "工作", pinyin: "gōngzuò", meaning: "Công việc / work", example: "我喜欢我的工作。", level: 1 },
  { hanzi: "语言", pinyin: "yǔyán", meaning: "Ngôn ngữ / language", example: "汉语是一门美丽的语言。", level: 2 },
  { hanzi: "知识", pinyin: "zhīshi", meaning: "Kiến thức / knowledge", example: "知识就是力量。", level: 2 },
  { hanzi: "机会", pinyin: "jīhuì", meaning: "Cơ hội / opportunity", example: "不要错过这个好机会。", level: 2 },
  { hanzi: "努力", pinyin: "nǔlì", meaning: "Nỗ lực / to work hard", example: "他很努力地学习中文。", level: 1 },
  { hanzi: "成功", pinyin: "chénggōng", meaning: "Thành công / success", example: "成功来自努力。", level: 2 },
  { hanzi: "梦想", pinyin: "mèngxiǎng", meaning: "Ước mơ / dream", example: "我的梦想是环游世界。", level: 2 },
  { hanzi: "进步", pinyin: "jìnbù", meaning: "Tiến bộ / progress", example: "她的汉语进步很快。", level: 2 },
  { hanzi: "经验", pinyin: "jīngyàn", meaning: "Kinh nghiệm / experience", example: "工作经验很重要。", level: 3 },
  { hanzi: "挑战", pinyin: "tiǎozhàn", meaning: "Thách thức / challenge", example: "学习语言是一个挑战。", level: 3 },
];

const DEFAULT_FOLDERS = [
  { id: "core", name: "Từ vựng cơ bản", icon: "📗" },
  { id: "advanced", name: "Từ vựng nâng cao", icon: "📕" },
];

// ═══════════════════════════════════════════════════════
//  INIT
// ═══════════════════════════════════════════════════════
function init() {
  const saved = localStorage.getItem('vm_vocab');
  if (saved) { vocabData = JSON.parse(saved); }
  else {
    vocabData = BUILT_IN_VOCAB.map((v, i) => ({ ...v, id: i, favorite: false, level: v.level ?? 0 }));
    saveVocab();
  }
  const savedFolders = localStorage.getItem('vm_folders');
  folders = savedFolders ? JSON.parse(savedFolders) : [...DEFAULT_FOLDERS];
  if (!savedFolders) saveFolders();

  const savedZh = localStorage.getItem('vm_chinese');
  chineseData = savedZh ? JSON.parse(savedZh) : BUILT_IN_CHINESE.map((v, i) => ({ ...v, id: i }));
  if (!savedZh) saveChinese();

  const savedFavs = localStorage.getItem('vm_favs');
  if (savedFavs) favorites = new Set(JSON.parse(savedFavs));

  totalXP = parseInt(localStorage.getItem('vm_xp') || '0');

  populateFolderSelects();
  updateStats();
  renderFolders();
  renderVocab();
  initReadingPassages();
}

function saveVocab() { localStorage.setItem('vm_vocab', JSON.stringify(vocabData)); }
function saveChinese() { localStorage.setItem('vm_chinese', JSON.stringify(chineseData)); }
function saveFavs() { localStorage.setItem('vm_favs', JSON.stringify([...favorites])); }
function saveFolders() { localStorage.setItem('vm_folders', JSON.stringify(folders)); }
function saveXP() { localStorage.setItem('vm_xp', String(totalXP)); }

function addXP(n) {
  totalXP += n;
  saveXP();
  updateStats();
}

function updateStats() {
  const c = [0, 0, 0, 0, 0, 0];
  vocabData.forEach(v => { c[v.level ?? 0]++; });
  for (let i = 0; i < 6; i++) {
    const elS = document.getElementById('st-' + i);
    const elB = document.getElementById('sb-' + i);
    if (elS) elS.textContent = c[i];
    if (elB) elB.textContent = c[i];
  }
  document.getElementById('xp-total').textContent = totalXP + 'đ';
  const pct = Math.min(100, (totalXP % 1000) / 10);
  document.getElementById('xp-fill').style.width = pct + '%';
}

// ═══════════════════════════════════════════════════════
//  NAVIGATION
// ═══════════════════════════════════════════════════════
function goPage(name) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  document.querySelectorAll('.nav-item').forEach(n => {
    if (n.getAttribute('onclick') && n.getAttribute('onclick').includes("'" + name + "'")) n.classList.add('active');
  });
  if (name === 'vocab') { renderFolders(); renderVocab(); }
  if (name === 'favorites') renderFavorites();
  if (name === 'chinese') renderChinese();
  if (name === 'reading') renderReadingSelect();
  document.getElementById('sidebar').classList.remove('open');
}

function toggleSidebar() { document.getElementById('sidebar').classList.toggle('open'); }

// ═══════════════════════════════════════════════════════
//  FOLDERS
// ═══════════════════════════════════════════════════════
function populateFolderSelects() {
  const selects = ['filter-folder', 'fc-folder', 'upload-target-folder'];
  selects.forEach(id => {
    const sel = document.getElementById(id);
    if (!sel) return;
    const keepFirst = id !== 'upload-target-folder';
    sel.innerHTML = (keepFirst ? `<option value="all">${id === 'upload-target-folder' ? '' : 'Tất cả folder'}</option>` : '') +
      folders.map(f => `<option value="${f.id}">${f.icon} ${f.name}</option>`).join('');
  });
}

function renderFolders() {
  const grid = document.getElementById('folder-grid');
  grid.innerHTML = folders.map(f => {
    const count = vocabData.filter(v => v.folder === f.id).length;
    return `<div class="folder-card ${activeFolderId === f.id ? 'active-folder' : ''}" onclick="selectFolder('${f.id}')">
      <button class="folder-del" onclick="event.stopPropagation();deleteFolder('${f.id}')" title="Xóa folder">✕</button>
      <div class="folder-icon">${f.icon}</div>
      <div class="folder-name">${f.name}</div>
      <div class="folder-count">${count} từ</div>
    </div>`;
  }).join('') + `<div class="folder-card folder-new" onclick="createFolder()">
      <div class="folder-icon">➕</div>
      <div class="folder-name">Tạo folder mới</div>
    </div>`;
}

function selectFolder(id) {
  activeFolderId = activeFolderId === id ? 'all' : id;
  document.getElementById('filter-folder').value = activeFolderId;
  renderFolders();
  renderVocab();
}

function createFolder() {
  const name = prompt('Tên folder mới:');
  if (!name || !name.trim()) return;
  const icons = ['📗','📘','📙','📕','📒','📓','🗂️','🏷️'];
  const icon = icons[folders.length % icons.length];
  const id = 'f_' + Date.now();
  folders.push({ id, name: name.trim(), icon });
  saveFolders();
  populateFolderSelects();
  renderFolders();
}

function deleteFolder(id) {
  if (!confirm('Xóa folder này? Các từ vựng trong folder sẽ không bị xóa nhưng mất gắn nhãn folder.')) return;
  folders = folders.filter(f => f.id !== id);
  vocabData.forEach(v => { if (v.folder === id) v.folder = null; });
  saveVocab(); saveFolders();
  populateFolderSelects();
  if (activeFolderId === id) activeFolderId = 'all';
  renderFolders();
  renderVocab();
}

// ═══════════════════════════════════════════════════════
//  VOCAB LIST
// ═══════════════════════════════════════════════════════
function renderVocab() {
  const q = document.getElementById('search-input').value.toLowerCase();
  const ff = document.getElementById('filter-folder').value;
  const fl = document.getElementById('filter-level').value;
  let filtered = vocabData.filter(v => {
    const matchQ = !q || v.word.toLowerCase().includes(q) || v.definition.toLowerCase().includes(q);
    const matchF = ff === 'all' || v.folder === ff;
    const matchL = fl === 'all' || String(v.level ?? 0) === fl;
    return matchQ && matchF && matchL;
  });
  const grid = document.getElementById('vocab-grid');
  if (!filtered.length) { grid.innerHTML = '<div class="empty">Không tìm thấy từ nào</div>'; return; }
  grid.innerHTML = filtered.map(v => vocabCardHTML(v)).join('');
  updateStats();
}

function vocabCardHTML(v) {
  const lvl = v.level ?? 0;
  const isFav = favorites.has(v.word);
  return `<div class="vocab-card">
    <button class="fav-btn ${isFav ? 'active' : ''}" onclick="toggleFav('${v.word.replace(/'/g,"\\'")}')" title="Đánh dấu yêu thích">${isFav ? '⭐' : '☆'}</button>
    <div class="word-row">
      <div class="word">${v.word}</div>
      ${v.pos ? `<span class="pos">${v.pos}</span>` : ''}
      <button class="audio-btn" onclick="speak('${v.word.replace(/'/g,"\\'")}','en-US')" title="Phát âm">🔊</button>
    </div>
    <div class="ipa">${v.ipa || ''}</div>
    <div class="definition">${v.definition}</div>
    ${v.definitionEN ? `<div class="definition" style="color:var(--text3);font-size:12px;margin-top:3px;">${v.definitionEN}</div>` : ''}
    ${v.example ? `<div class="example">${v.example}</div>` : ''}
    ${v.synonyms ? `<div class="synonyms">↔️ Đồng nghĩa: ${v.synonyms}</div>` : ''}
    ${v.antonyms && v.antonyms !== 'N/A' ? `<div class="antonyms">⚡ Trái nghĩa: ${v.antonyms}</div>` : ''}
    <div class="tags"><span class="tag lvl-${lvl}">${LEVEL_ICONS[lvl]} ${LEVEL_NAMES[lvl]}</span></div>
    <div class="level-select">
      <select onchange="setLevel('${v.word.replace(/'/g,"\\'")}', this.value)">
        ${LEVEL_NAMES.map((n, i) => `<option value="${i}" ${i === lvl ? 'selected' : ''}>${LEVEL_ICONS[i]} ${n}</option>`).join('')}
      </select>
    </div>
  </div>`;
}

function setLevel(word, lvl) {
  const v = vocabData.find(x => x.word === word);
  if (v) { v.level = parseInt(lvl); saveVocab(); renderVocab(); updateStats(); }
}

function toggleFav(word) {
  if (favorites.has(word)) favorites.delete(word);
  else favorites.add(word);
  saveFavs();
  renderVocab();
  renderFavorites();
}

function renderFavorites() {
  const grid = document.getElementById('fav-grid');
  const favs = vocabData.filter(v => favorites.has(v.word));
  if (!favs.length) { grid.innerHTML = '<div class="empty">Chưa có từ yêu thích nào.<br>Nhấn ☆ trên thẻ từ để thêm.</div>'; return; }
  grid.innerHTML = favs.map(v => vocabCardHTML(v)).join('');
}

// ═══════════════════════════════════════════════════════
//  AUDIO / TTS
// ═══════════════════════════════════════════════════════
function speak(text, lang = 'en-US') {
  if (!window.speechSynthesis) return;
  window.speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(text);
  u.lang = lang; u.rate = 0.85;
  const voices = window.speechSynthesis.getVoices();
  const v = voices.find(x => x.lang === lang) || voices.find(x => x.lang.startsWith(lang.split('-')[0]));
  if (v) u.voice = v;
  window.speechSynthesis.speak(u);
}

// ═══════════════════════════════════════════════════════
//  GENERIC COUNTDOWN TIMER HELPER
// ═══════════════════════════════════════════════════════
function makeTimer(circleEl, numEl, seconds, onTick, onExpire) {
  const RADIUS = 15, CIRC = 2 * Math.PI * RADIUS;
  let remaining = seconds;
  circleEl.style.strokeDasharray = CIRC;
  circleEl.style.strokeDashoffset = 0;
  numEl.textContent = remaining;
  circleEl.style.stroke = 'var(--accent2)';
  const id = setInterval(() => {
    remaining -= 0.1;
    if (remaining <= 0) {
      clearInterval(id);
      numEl.textContent = 0;
      circleEl.style.strokeDashoffset = CIRC;
      onExpire();
      return;
    }
    numEl.textContent = Math.ceil(remaining);
    const offset = CIRC * (1 - remaining / seconds);
    circleEl.style.strokeDashoffset = offset;
    if (remaining < seconds * 0.3) circleEl.style.stroke = 'var(--danger)';
    else if (remaining < seconds * 0.6) circleEl.style.stroke = 'var(--warn)';
    if (onTick) onTick(remaining);
  }, 100);
  return id;
}

function pointsFor(count) { return POINTS_BY_COUNT[count] || 10; }

// ═══════════════════════════════════════════════════════
//  FLASHCARD
// ═══════════════════════════════════════════════════════
let fcDeck = [], fcIdx = 0, fcCorrect = 0, fcWrong = 0, fcXP = 0, fcGroup = 'all', fcCount = 10, fcTimerId = null;

function setFCGroup(g) {
  fcGroup = g;
  document.querySelectorAll('#fc-group-tabs .group-tab').forEach(t => t.classList.toggle('active', t.dataset.grp === g));
}
function setFCCount(n) {
  fcCount = n;
  document.querySelectorAll('#fc-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function filterByGroup(list, grp) {
  if (grp === 'new') return list.filter(v => (v.level ?? 0) === 0);
  if (grp === 'learning') return list.filter(v => [1,2,3].includes(v.level ?? 0));
  if (grp === 'hard') return list.filter(v => (v.level ?? 0) === 4);
  if (grp === 'known') return list.filter(v => (v.level ?? 0) === 5);
  return list;
}

function startFlashcard() {
  const folder = document.getElementById('fc-folder').value;
  let pool = vocabData.filter(v => folder === 'all' || v.folder === folder);
  pool = filterByGroup(pool, fcGroup);
  if (!pool.length) { alert('Không có từ nào phù hợp với lựa chọn này!'); return; }
  fcDeck = shuffle([...pool]).slice(0, fcCount);
  fcIdx = 0; fcCorrect = 0; fcWrong = 0; fcXP = 0;
  document.getElementById('fc-setup').style.display = 'none';
  document.getElementById('fc-result').style.display = 'none';
  document.getElementById('fc-game').style.display = 'block';
  showFC();
}

function showFC() {
  if (fcTimerId) clearInterval(fcTimerId);
  if (fcIdx >= fcDeck.length) { finishFlashcard(); return; }
  const v = fcDeck[fcIdx];
  document.getElementById('fc-word').textContent = v.word;
  document.getElementById('fc-ipa').textContent = v.ipa || '';
  document.getElementById('fc-def').textContent = v.definition;
  document.getElementById('fc-ex').textContent = v.example || '';
  document.getElementById('fc-counter').textContent = `${fcIdx + 1} / ${fcDeck.length}`;
  document.getElementById('fc-score-display').textContent = `✅ ${fcCorrect}   ❌ ${fcWrong}   ⚡ ${fcXP}đ`;
  document.getElementById('fc-progress').style.width = ((fcIdx / fcDeck.length) * 100) + '%';
  const card = document.getElementById('flashcard');
  card.classList.remove('flipped');

  // Level rating buttons
  const row = document.getElementById('fc-level-row');
  row.innerHTML = LEVEL_NAMES.map((n, i) => `<button class="level-rate-btn lvl-${i}" onclick="rateFC(${i})">${LEVEL_ICONS[i]} ${n}</button>`).join('');

  fcTimerId = makeTimer(
    document.getElementById('fc-timer-circle'),
    document.getElementById('fc-timer-num'),
    TIME_PER_ITEM,
    null,
    () => { rateFC(4, true); } // hết giờ => Chưa thuộc, không cộng điểm
  );
}

function flipCard() {
  document.getElementById('flashcard').classList.toggle('flipped');
  speak(fcDeck[fcIdx]?.word || '', 'en-US');
}

function rateFC(newLevel, timeout) {
  if (fcTimerId) clearInterval(fcTimerId);
  const v = fcDeck[fcIdx];
  v.level = newLevel;
  saveVocab();
  if (newLevel === 5 && !timeout) {
    fcCorrect++;
    const pts = pointsFor(fcCount);
    fcXP += pts;
    addXP(pts);
  } else if (newLevel === 4) {
    fcWrong++;
  } else {
    // học 1-4 lần: cộng điểm nhỏ vì có nỗ lực ôn tập, không tính hết giờ
    if (!timeout) { fcXP += Math.round(pointsFor(fcCount) / 2); addXP(Math.round(pointsFor(fcCount) / 2)); }
  }
  fcIdx++;
  showFC();
}

function speakWord() { speak(fcDeck[fcIdx]?.word || '', 'en-US'); }

function finishFlashcard() {
  document.getElementById('fc-game').style.display = 'none';
  document.getElementById('fc-result').style.display = 'block';
  const pct = Math.round((fcCorrect / fcDeck.length) * 100);
  document.getElementById('fc-final-pct').textContent = pct + '%';
  document.getElementById('fc-final-label').textContent = `${fcCorrect}/${fcDeck.length} từ đã nhớ – ${fcWrong} từ chưa thuộc`;
  document.getElementById('fc-final-xp').textContent = `⚡ Bạn đã kiếm được ${fcXP} điểm!`;
}

function endFlashcard() {
  if (fcTimerId) clearInterval(fcTimerId);
  finishFlashcard();
}

function backToFCSetup() {
  document.getElementById('fc-result').style.display = 'none';
  document.getElementById('fc-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  QUIZ (FILL + MULTIPLE CHOICE)
// ═══════════════════════════════════════════════════════
let quizMode = 'mc', quizDir = 'en-vi', quizCount = 10;
let quizQuestions = [], quizIdx = 0, quizScore = 0, quizXP = 0, quizTimerId = null;

function setQuizMode(m) {
  quizMode = m;
  ['mc','fill'].forEach(x => document.getElementById('qm-'+x).classList.toggle('active', x === m));
}
function setQuizDir(d) {
  quizDir = d;
  ['en-vi','vi-en'].forEach(x => document.getElementById('dir-'+x).classList.toggle('active', x === d));
}
function setQuizCount(n) {
  quizCount = n;
  document.querySelectorAll('#quiz-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function startQuiz() {
  const n = Math.min(quizCount, vocabData.length);
  quizQuestions = shuffle([...vocabData]).slice(0, n);
  quizIdx = 0; quizScore = 0; quizXP = 0;
  document.getElementById('quiz-setup').style.display = 'none';
  document.getElementById('quiz-result').style.display = 'none';
  document.getElementById('quiz-game').style.display = 'block';
  showQuestion();
}

function showQuestion() {
  if (quizTimerId) clearInterval(quizTimerId);
  if (quizIdx >= quizQuestions.length) { showQuizResult(); return; }
  const v = quizQuestions[quizIdx];
  document.getElementById('quiz-counter').textContent = `Câu ${quizIdx + 1}/${quizQuestions.length}`;
  document.getElementById('quiz-score-disp').textContent = `⚡ ${quizXP}đ`;
  document.getElementById('quiz-progress').style.width = ((quizIdx / quizQuestions.length) * 100) + '%';
  document.getElementById('quiz-feedback').style.display = 'none';
  document.getElementById('quiz-next-btn').style.display = 'none';

  const qText = quizDir === 'en-vi'
    ? `Từ tiếng Anh: <strong>${v.word}</strong> ${v.ipa ? `<span style="color:var(--accent);font-size:14px;">${v.ipa}</span>` : ''}`
    : `Nghĩa tiếng Việt: <strong>${v.definition}</strong>`;
  document.getElementById('quiz-q').innerHTML = qText;

  if (quizMode === 'mc') {
    document.getElementById('quiz-mc-opts').style.display = 'grid';
    document.getElementById('quiz-fill-area').style.display = 'none';
    let wrong = shuffle(vocabData.filter(x => x.word !== v.word)).slice(0, 3);
    let opts = shuffle([v, ...wrong]);
    document.getElementById('quiz-mc-opts').innerHTML = opts.map((opt) => {
      const label = quizDir === 'en-vi' ? opt.definition : opt.word;
      return `<button class="quiz-opt" onclick="checkMC(this, '${opt.word === v.word ? 'correct' : 'wrong'}', '${v.word.replace(/'/g, "\\'")}', '${v.definition.replace(/'/g, "\\'")}')">${label}</button>`;
    }).join('');
  } else {
    document.getElementById('quiz-mc-opts').style.display = 'none';
    document.getElementById('quiz-fill-area').style.display = 'block';
    document.getElementById('quiz-fill-input').value = '';
    document.getElementById('quiz-fill-input').className = '';
    setTimeout(() => document.getElementById('quiz-fill-input').focus(), 100);
  }

  quizTimerId = makeTimer(
    document.getElementById('quiz-timer-circle'),
    document.getElementById('quiz-timer-num'),
    TIME_PER_ITEM,
    null,
    () => quizTimeout()
  );
}

function quizTimeout() {
  document.querySelectorAll('.quiz-opt').forEach(b => b.disabled = true);
  if (quizMode === 'mc') {
    document.querySelectorAll('.quiz-opt').forEach(b => {
      if (b.getAttribute('onclick')?.includes("'correct'")) b.classList.add('correct');
    });
  } else {
    document.getElementById('quiz-fill-input').className = 'wrong';
  }
  showFeedback(false, `⏰ Hết giờ! Không tính là đã thuộc.`);
  document.getElementById('quiz-next-btn').style.display = 'inline-flex';
}

function checkMC(btn, result, word, def) {
  if (quizTimerId) clearInterval(quizTimerId);
  document.querySelectorAll('.quiz-opt').forEach(b => b.disabled = true);
  btn.classList.add(result);
  if (result === 'correct') {
    quizScore++;
    const pts = pointsFor(quizCount);
    quizXP += pts; addXP(pts);
    showFeedback(true, `✅ Chính xác! +${pts}đ`);
  } else {
    document.querySelectorAll('.quiz-opt').forEach(b => {
      if (b.getAttribute('onclick')?.includes("'correct'")) b.classList.add('correct');
    });
    showFeedback(false, `❌ Sai! Đáp án đúng: ${quizDir === 'en-vi' ? def : word}`);
  }
  document.getElementById('quiz-next-btn').style.display = 'inline-flex';
  speak(word, 'en-US');
}

function checkFill() {
  if (quizTimerId) clearInterval(quizTimerId);
  const inp = document.getElementById('quiz-fill-input');
  const v = quizQuestions[quizIdx];
  const answer = quizDir === 'en-vi' ? v.definition.toLowerCase() : v.word.toLowerCase();
  const userAnswer = inp.value.trim().toLowerCase();
  const correct = userAnswer === answer || (answer.includes(userAnswer) && userAnswer.length > 3);
  inp.className = correct ? 'correct' : 'wrong';
  if (correct) {
    quizScore++;
    const pts = pointsFor(quizCount);
    quizXP += pts; addXP(pts);
    showFeedback(true, `✅ Chính xác! +${pts}đ`);
  } else {
    showFeedback(false, `❌ Đáp án đúng: ${quizDir === 'en-vi' ? v.definition : v.word}`);
  }
  document.getElementById('quiz-next-btn').style.display = 'inline-flex';
  speak(v.word, 'en-US');
}

function showFeedback(ok, msg) {
  const fb = document.getElementById('quiz-feedback');
  fb.className = 'quiz-feedback ' + (ok ? 'correct' : 'wrong');
  fb.textContent = msg;
  fb.style.display = 'block';
}

function nextQuiz() { quizIdx++; showQuestion(); }
function speakCurrent() { speak(quizQuestions[quizIdx]?.word || '', 'en-US'); }

function showQuizResult() {
  document.getElementById('quiz-game').style.display = 'none';
  document.getElementById('quiz-result').style.display = 'block';
  const pct = Math.round((quizScore / quizQuestions.length) * 100);
  document.getElementById('quiz-final-score').textContent = pct + '%';
  document.getElementById('quiz-final-label').textContent = `${quizScore}/${quizQuestions.length} câu đúng – ${pct >= 80 ? '🎉 Xuất sắc!' : pct >= 60 ? '👍 Khá tốt!' : '📚 Cần ôn thêm!'}`;
  document.getElementById('quiz-final-xp').textContent = `⚡ Tổng điểm kiếm được: ${quizXP}đ`;
}

function backToQuizSetup() {
  document.getElementById('quiz-game').style.display = 'none';
  document.getElementById('quiz-result').style.display = 'none';
  document.getElementById('quiz-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  GAP-FILL (câu có ngữ cảnh, điền 1 từ, thứ tự xáo trộn)
// ═══════════════════════════════════════════════════════
let gapfillCount = 10, gapfillQ = [], gapfillIdx = 0, gapfillScore = 0, gapfillXP = 0, gapfillTimerId = null;

function setGapfillCount(n) {
  gapfillCount = n;
  document.querySelectorAll('#gapfill-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function buildGapSentence(v) {
  // Tạo câu có chỗ trống từ ví dụ có sẵn, thay từ chính bằng dấu gạch
  if (!v.example) return null;
  const re = new RegExp(v.word, 'i');
  if (!re.test(v.example)) return v.example; // fallback: không thay được
  return v.example.replace(re, '______');
}

function startGapfill() {
  const pool = vocabData.filter(v => v.example && v.example.length > 0);
  gapfillQ = shuffle([...pool]).slice(0, Math.min(gapfillCount, pool.length));
  if (!gapfillQ.length) { alert('Không có câu ví dụ nào để tạo bài tập!'); return; }
  gapfillIdx = 0; gapfillScore = 0; gapfillXP = 0;
  document.getElementById('gapfill-setup').style.display = 'none';
  document.getElementById('gapfill-result').style.display = 'none';
  document.getElementById('gapfill-game').style.display = 'block';
  showGapfill();
}

function showGapfill() {
  if (gapfillTimerId) clearInterval(gapfillTimerId);
  if (gapfillIdx >= gapfillQ.length) { finishGapfill(); return; }
  const v = gapfillQ[gapfillIdx];
  document.getElementById('gapfill-counter').textContent = `Câu ${gapfillIdx + 1}/${gapfillQ.length}`;
  document.getElementById('gapfill-score-disp').textContent = `⚡ ${gapfillXP}đ`;
  document.getElementById('gapfill-progress').style.width = ((gapfillIdx / gapfillQ.length) * 100) + '%';
  document.getElementById('gapfill-sentence').textContent = buildGapSentence(v);
  document.getElementById('gapfill-input').value = '';
  document.getElementById('gapfill-input').className = '';
  document.getElementById('gapfill-feedback').style.display = 'none';
  document.getElementById('gapfill-next-btn').style.display = 'none';
  setTimeout(() => document.getElementById('gapfill-input').focus(), 100);

  gapfillTimerId = makeTimer(
    document.getElementById('gapfill-timer-circle'),
    document.getElementById('gapfill-timer-num'),
    TIME_PER_ITEM, null,
    () => gapfillTimeout()
  );
}

function gapfillTimeout() {
  const v = gapfillQ[gapfillIdx];
  document.getElementById('gapfill-input').className = 'wrong';
  showGapfillFeedback(false, `⏰ Hết giờ! Đáp án đúng: ${v.word}`);
}

function checkGapfill() {
  if (gapfillTimerId) clearInterval(gapfillTimerId);
  const v = gapfillQ[gapfillIdx];
  const inp = document.getElementById('gapfill-input');
  const userAns = inp.value.trim().toLowerCase();
  const correct = userAns === v.word.toLowerCase();
  inp.className = correct ? 'correct' : 'wrong';
  if (correct) {
    gapfillScore++;
    const pts = pointsFor(gapfillCount);
    gapfillXP += pts; addXP(pts);
    showGapfillFeedback(true, `✅ Chính xác! +${pts}đ`);
  } else {
    showGapfillFeedback(false, `❌ Đáp án đúng: ${v.word}`);
  }
  speak(v.word, 'en-US');
}

function showGapfillFeedback(ok, msg) {
  const fb = document.getElementById('gapfill-feedback');
  fb.className = 'quiz-feedback ' + (ok ? 'correct' : 'wrong');
  fb.textContent = msg;
  fb.style.display = 'block';
  document.getElementById('gapfill-next-btn').style.display = 'inline-flex';
}

function nextGapfill() { gapfillIdx++; showGapfill(); }

function finishGapfill() {
  document.getElementById('gapfill-game').style.display = 'none';
  document.getElementById('gapfill-result').style.display = 'block';
  const pct = Math.round((gapfillScore / gapfillQ.length) * 100);
  document.getElementById('gapfill-final-score').textContent = pct + '%';
  document.getElementById('gapfill-final-label').textContent = `${gapfillScore}/${gapfillQ.length} câu đúng`;
  document.getElementById('gapfill-final-xp').textContent = `⚡ Tổng điểm kiếm được: ${gapfillXP}đ`;
}

function backToGapfillSetup() {
  document.getElementById('gapfill-result').style.display = 'none';
  document.getElementById('gapfill-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  MATCHING
// ═══════════════════════════════════════════════════════
let matchingCount = 10, matchingPool = [], matchingRoundWords = [], matchingRoundDefs = [];
let matchingSelectedWord = null, matchingSolvedWords = new Set(), matchingScore = 0, matchingWrong = 0, matchingXP = 0;
let matchingRound = 0, matchingTotalRounds = 0;

function setMatchingCount(n) {
  matchingCount = n;
  document.querySelectorAll('#matching-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function startMatching() {
  matchingPool = shuffle([...vocabData]).slice(0, matchingCount);
  if (!matchingPool.length) { alert('Không có từ vựng nào!'); return; }
  matchingRound = 0;
  matchingTotalRounds = Math.ceil(matchingPool.length / 10);
  matchingScore = 0; matchingWrong = 0; matchingXP = 0;
  document.getElementById('matching-setup').style.display = 'none';
  document.getElementById('matching-result').style.display = 'none';
  document.getElementById('matching-game').style.display = 'block';
  loadMatchingRound();
}

function loadMatchingRound() {
  const start = matchingRound * 10;
  const roundWords = matchingPool.slice(start, start + 10);
  if (!roundWords.length) { finishMatching(); return; }
  matchingRoundWords = shuffle([...roundWords]);
  matchingRoundDefs = shuffle([...roundWords]);
  matchingSelectedWord = null;
  matchingSolvedWords = new Set();

  document.getElementById('matching-counter').textContent = `Vòng ${matchingRound + 1}/${matchingTotalRounds}`;
  document.getElementById('matching-progress').style.width = ((matchingRound / matchingTotalRounds) * 100) + '%';
  renderMatching();
}

function renderMatching() {
  document.getElementById('matching-score-disp').textContent = `⚡ ${matchingXP}đ &nbsp; ✅ ${matchingScore} &nbsp; ❌ ${matchingWrong}`;
  const wordCol = document.getElementById('match-col-word');
  const defCol = document.getElementById('match-col-def');
  wordCol.innerHTML = matchingRoundWords.map(v => {
    const solved = matchingSolvedWords.has(v.word);
    const selected = matchingSelectedWord === v.word;
    return `<div class="match-item ${solved ? 'matched-correct disabled' : ''} ${selected ? 'selected' : ''}" data-word="${v.word.replace(/"/g,'&quot;')}" onclick="selectMatchWord('${v.word.replace(/'/g,"\\'")}')">${v.word}</div>`;
  }).join('');
  defCol.innerHTML = matchingRoundDefs.map(v => {
    const solved = matchingSolvedWords.has(v.word);
    return `<div class="match-item ${solved ? 'matched-correct disabled' : ''}" data-defword="${v.word.replace(/"/g,'&quot;')}" onclick="selectMatchDef('${v.word.replace(/'/g,"\\'")}')">${v.definition}</div>`;
  }).join('');
}

function selectMatchWord(word) {
  if (matchingSolvedWords.has(word)) return;
  matchingSelectedWord = word;
  renderMatching();
}

function selectMatchDef(defWord) {
  if (!matchingSelectedWord || matchingSolvedWords.has(defWord)) return;
  const isCorrect = matchingSelectedWord === defWord;
  if (isCorrect) {
    matchingSolvedWords.add(defWord);
    matchingScore++;
    const pts = Math.round(pointsFor(matchingCount));
    matchingXP += pts; addXP(pts);
    matchingSelectedWord = null;
    renderMatching();
    if (matchingSolvedWords.size === matchingRoundWords.length) {
      setTimeout(() => { matchingRound++; loadMatchingRound(); }, 500);
    }
  } else {
    matchingWrong++;
    // flash wrong
    const defEl = document.querySelector(`[data-defword="${defWord.replace(/"/g,'&quot;')}"]`);
    const wordEl = document.querySelector(`[data-word="${matchingSelectedWord.replace(/"/g,'&quot;')}"]`);
    if (defEl) { defEl.classList.add('matched-wrong-flash'); setTimeout(() => defEl.classList.remove('matched-wrong-flash'), 400); }
    if (wordEl) wordEl.classList.add('shake');
    setTimeout(() => { if (wordEl) wordEl.classList.remove('shake'); }, 300);
    matchingSelectedWord = null;
    renderMatching();
    document.getElementById('matching-score-disp').textContent = `⚡ ${matchingXP}đ &nbsp; ✅ ${matchingScore} &nbsp; ❌ ${matchingWrong}`;
  }
}

function finishMatching() {
  document.getElementById('matching-game').style.display = 'none';
  document.getElementById('matching-result').style.display = 'block';
  const total = matchingScore + matchingWrong;
  const pct = total ? Math.round((matchingScore / total) * 100) : 100;
  document.getElementById('matching-final-score').textContent = pct + '%';
  document.getElementById('matching-final-label').textContent = `${matchingScore} cặp đúng, ${matchingWrong} lần sai`;
  document.getElementById('matching-final-xp').textContent = `⚡ Tổng điểm kiếm được: ${matchingXP}đ`;
}

function endMatching() { finishMatching(); }

function backToMatchingSetup() {
  document.getElementById('matching-result').style.display = 'none';
  document.getElementById('matching-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  DICTATION / NGHE VIẾT
// ═══════════════════════════════════════════════════════
let dictationCount = 10, dictationQ = [], dictationIdx = 0, dictationScore = 0, dictationXP = 0, dictationTimerId = null;

function setDictationCount(n) {
  dictationCount = n;
  document.querySelectorAll('#dictation-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function startDictation() {
  dictationQ = shuffle([...vocabData]).slice(0, Math.min(dictationCount, vocabData.length));
  if (!dictationQ.length) { alert('Không có từ vựng nào!'); return; }
  dictationIdx = 0; dictationScore = 0; dictationXP = 0;
  document.getElementById('dictation-setup').style.display = 'none';
  document.getElementById('dictation-result').style.display = 'none';
  document.getElementById('dictation-game').style.display = 'block';
  showDictation();
}

function showDictation() {
  if (dictationTimerId) clearInterval(dictationTimerId);
  if (dictationIdx >= dictationQ.length) { finishDictation(); return; }
  document.getElementById('dictation-counter').textContent = `Từ ${dictationIdx + 1}/${dictationQ.length}`;
  document.getElementById('dictation-score-disp').textContent = `⚡ ${dictationXP}đ`;
  document.getElementById('dictation-progress').style.width = ((dictationIdx / dictationQ.length) * 100) + '%';
  document.getElementById('dictation-input').value = '';
  document.getElementById('dictation-input').className = 'text-input';
  document.getElementById('dictation-hint').textContent = '';
  document.getElementById('dictation-example').style.display = 'none';
  document.getElementById('dictation-feedback').style.display = 'none';
  document.getElementById('dictation-next-btn').style.display = 'none';
  setTimeout(() => playDictationAudio(), 300);

  dictationTimerId = makeTimer(
    document.getElementById('dictation-timer-circle'),
    document.getElementById('dictation-timer-num'),
    TIME_PER_ITEM, null,
    () => dictationTimeout()
  );
}

function playDictationAudio() { speak(dictationQ[dictationIdx]?.word || '', 'en-US'); }

function showDictationExample() {
  const v = dictationQ[dictationIdx];
  const masked = v.example ? v.example.replace(new RegExp(v.word, 'gi'), '_____') : '(Không có ví dụ)';
  const el = document.getElementById('dictation-example');
  el.textContent = masked;
  el.style.display = 'block';
}

function showDictationHint() {
  const v = dictationQ[dictationIdx];
  const w = v.word;
  let hint;
  if (w.length <= 2) hint = w[0] + '—'.repeat(Math.max(1, w.length - 1));
  else hint = w[0] + w[1] + '—'.repeat(Math.max(1, w.length - 4)) + w[w.length - 2] + w[w.length - 1];
  document.getElementById('dictation-hint').textContent = `${hint}  (${w.length} chữ)`;
}

function dictationTimeout() {
  const v = dictationQ[dictationIdx];
  document.getElementById('dictation-input').className = 'text-input wrong';
  showDictationFeedback(false, `⏰ Hết giờ! Đáp án đúng: ${v.word}`);
}

function checkDictation() {
  if (dictationTimerId) clearInterval(dictationTimerId);
  const v = dictationQ[dictationIdx];
  const inp = document.getElementById('dictation-input');
  const correct = inp.value.trim().toLowerCase() === v.word.toLowerCase();
  inp.className = 'text-input ' + (correct ? 'correct' : 'wrong');
  if (correct) {
    dictationScore++;
    const pts = pointsFor(dictationCount);
    dictationXP += pts; addXP(pts);
    showDictationFeedback(true, `✅ Chính xác! +${pts}đ`);
  } else {
    showDictationFeedback(false, `❌ Đáp án đúng: ${v.word}`);
  }
}

function showDictationFeedback(ok, msg) {
  const fb = document.getElementById('dictation-feedback');
  fb.className = 'quiz-feedback ' + (ok ? 'correct' : 'wrong');
  fb.textContent = msg;
  fb.style.display = 'block';
  document.getElementById('dictation-next-btn').style.display = 'inline-flex';
}

function nextDictation() { dictationIdx++; showDictation(); }

function finishDictation() {
  document.getElementById('dictation-game').style.display = 'none';
  document.getElementById('dictation-result').style.display = 'block';
  const pct = Math.round((dictationScore / dictationQ.length) * 100);
  document.getElementById('dictation-final-score').textContent = pct + '%';
  document.getElementById('dictation-final-label').textContent = `${dictationScore}/${dictationQ.length} từ đúng`;
  document.getElementById('dictation-final-xp').textContent = `⚡ Tổng điểm kiếm được: ${dictationXP}đ`;
}

function backToDictationSetup() {
  document.getElementById('dictation-result').style.display = 'none';
  document.getElementById('dictation-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  WRITING (Biên phiên dịch / Dịch ngược / Dictation câu)
// ═══════════════════════════════════════════════════════
let writingMode = 'vi-en', writingCount = 10, writingQ = [], writingIdx = 0, writingXP = 0, writingTotalScore = 0;

function setWritingMode(m) {
  writingMode = m;
  ['vi-en','back','dict'].forEach(x => document.getElementById('wm-'+x).classList.toggle('active', x === m));
}
function setWritingCount(n) {
  writingCount = n;
  document.querySelectorAll('#writing-count-pills .count-pill').forEach(p => p.classList.toggle('active', parseInt(p.dataset.n) === n));
}

function startWriting() {
  const pool = vocabData.filter(v => v.example && v.example.length > 0);
  writingQ = shuffle([...pool]).slice(0, Math.min(writingCount, pool.length));
  if (!writingQ.length) { alert('Không có câu ví dụ nào!'); return; }
  writingIdx = 0; writingXP = 0; writingTotalScore = 0;
  document.getElementById('writing-setup').style.display = 'none';
  document.getElementById('writing-result').style.display = 'none';
  document.getElementById('writing-game').style.display = 'block';
  showWriting();
}

function showWriting() {
  if (writingIdx >= writingQ.length) { finishWriting(); return; }
  const v = writingQ[writingIdx];
  document.getElementById('writing-counter').textContent = `Câu ${writingIdx + 1}/${writingQ.length}`;
  document.getElementById('writing-score-disp').textContent = `⚡ ${writingXP}đ`;
  document.getElementById('writing-progress').style.width = ((writingIdx / writingQ.length) * 100) + '%';
  document.getElementById('writing-input').value = '';
  document.getElementById('writing-input').className = 'text-input';
  document.getElementById('writing-score-box').style.display = 'none';
  document.getElementById('writing-next-btn').style.display = 'none';
  document.getElementById('writing-audio-row').style.display = 'none';

  if (writingMode === 'vi-en') {
    document.getElementById('writing-prompt').innerHTML = `Dịch câu sau sang tiếng Anh:<br><strong style="color:var(--accent4);">${v.definition} (sử dụng từ "<em>${v.word}</em>")</strong>`;
    document.getElementById('writing-input').placeholder = 'Nhập bản dịch tiếng Anh...';
  } else if (writingMode === 'back') {
    document.getElementById('writing-prompt').innerHTML = `Câu gốc tiếng Anh: <strong>${v.example}</strong><br>Hãy dịch sang tiếng Việt, sau đó dịch ngược lại tiếng Anh để so sánh với bản gốc:`;
    document.getElementById('writing-input').placeholder = 'Dịch ngược lại tiếng Anh...';
  } else {
    document.getElementById('writing-prompt').innerHTML = `Nghe câu và gõ lại chính xác từng chữ:`;
    document.getElementById('writing-audio-row').style.display = 'block';
    document.getElementById('writing-input').placeholder = 'Gõ lại câu bạn nghe được...';
    setTimeout(() => playWritingAudio(), 300);
  }
}

function playWritingAudio() {
  const v = writingQ[writingIdx];
  speak(v.example || v.word, 'en-US');
}

// Simple similarity-based auto-grading (Levenshtein-based percentage)
function levenshtein(a, b) {
  const m = a.length, n = b.length;
  const dp = Array.from({length: m + 1}, () => new Array(n + 1).fill(0));
  for (let i = 0; i <= m; i++) dp[i][0] = i;
  for (let j = 0; j <= n; j++) dp[0][j] = j;
  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      dp[i][j] = a[i-1] === b[j-1] ? dp[i-1][j-1] : 1 + Math.min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]);
    }
  }
  return dp[m][n];
}

function similarityPct(a, b) {
  a = a.toLowerCase().trim(); b = b.toLowerCase().trim();
  if (!a.length && !b.length) return 100;
  const dist = levenshtein(a, b);
  const maxLen = Math.max(a.length, b.length, 1);
  return Math.max(0, Math.round((1 - dist / maxLen) * 100));
}

function diffWords(target, input) {
  const tWords = target.toLowerCase().replace(/[.,!?]/g,'').split(/\s+/);
  const iWords = input.toLowerCase().replace(/[.,!?]/g,'').split(/\s+/);
  return tWords.map((tw, i) => {
    if (iWords[i] === tw) return `<span class="diff-correct">${tw}</span>`;
    if (iWords[i]) return `<span class="diff-wrong">${iWords[i]}</span> <span class="diff-missing">(${tw})</span>`;
    return `<span class="diff-missing">[thiếu: ${tw}]</span>`;
  }).join(' ');
}

function checkWriting() {
  const v = writingQ[writingIdx];
  const input = document.getElementById('writing-input').value.trim();
  let target, pct, gramNote;

  if (writingMode === 'vi-en') {
    target = v.example;
    pct = similarityPct(target, input);
    gramNote = pct >= 80 ? '✅ Ngữ pháp và từ vựng tốt!' : pct >= 50 ? '⚠️ Còn vài chỗ chưa chính xác, kiểm tra ngữ pháp & chính tả.' : '❌ Cần xem lại câu, so sánh với bản gợi ý.';
  } else if (writingMode === 'back') {
    target = v.example;
    pct = similarityPct(target, input);
    gramNote = pct >= 80 ? '✅ Bản dịch ngược rất gần với câu gốc!' : pct >= 50 ? '⚠️ Có sự khác biệt về cách diễn đạt so với bản gốc.' : '❌ Khác biệt lớn so với câu gốc, nên ôn lại.';
  } else {
    target = v.example || v.word;
    pct = similarityPct(target, input);
    gramNote = pct >= 90 ? '✅ Nghe và gõ chính xác!' : pct >= 60 ? '⚠️ Gần đúng, còn vài từ nghe sai.' : '❌ Cần nghe lại kỹ hơn.';
  }

  const pts = pct >= 80 ? pointsFor(writingCount) : pct >= 50 ? Math.round(pointsFor(writingCount) / 2) : 0;
  writingXP += pts; addXP(pts);
  writingTotalScore += pct;

  const box = document.getElementById('writing-score-box');
  box.style.display = 'block';
  box.innerHTML = `
    <div class="score-big">${pct}%</div>
    <div style="font-size:13px;color:var(--text2);margin:6px 0;">${gramNote}</div>
    <div style="font-size:13px;margin-top:10px;"><b>Câu gốc:</b> ${target}</div>
    <div style="font-size:13px;margin-top:6px;"><b>So sánh từng từ:</b><br>${diffWords(target, input)}</div>
    <div style="font-size:12.5px;color:var(--accent4);margin-top:8px;">⚡ +${pts}đ</div>
  `;
  document.getElementById('writing-next-btn').style.display = 'inline-flex';
}

function nextWriting() { writingIdx++; showWriting(); }

function finishWriting() {
  document.getElementById('writing-game').style.display = 'none';
  document.getElementById('writing-result').style.display = 'block';
  const avgPct = Math.round(writingTotalScore / writingQ.length);
  document.getElementById('writing-final-score').textContent = avgPct + '%';
  document.getElementById('writing-final-label').textContent = `Điểm trung bình trên ${writingQ.length} câu`;
  document.getElementById('writing-final-xp').textContent = `⚡ Tổng điểm kiếm được: ${writingXP}đ`;
}

function backToWritingSetup() {
  document.getElementById('writing-result').style.display = 'none';
  document.getElementById('writing-setup').style.display = 'block';
}

// ═══════════════════════════════════════════════════════
//  READING COMPREHENSION
// ═══════════════════════════════════════════════════════
const READING_PASSAGES = [
  {
    title: "The Power of Perseverance",
    level: "Intermediate",
    words: ["perseverance", "resilient", "endeavor", "emulate", "robust"],
    passage: `In every __1__ (endeavor) of human achievement, the quality of __2__ (perseverance) has proven to be one of the most decisive factors. Those who succeed are not always the most talented; they are often the most __3__ (resilient) individuals who refuse to give up when faced with adversity. To become truly successful, one must __4__ (emulate) the habits of great achievers while building a __5__ (robust) foundation of discipline and focus.`,
    blanks: ["endeavor", "perseverance", "resilient", "emulate", "robust"],
  },
  {
    title: "Ethics in Modern Business",
    level: "Upper-Intermediate",
    words: ["integrity", "fraud", "bribe", "embezzle", "insidious"],
    passage: `Maintaining __1__ (integrity) in business is essential for long-term success. Companies that engage in __2__ (fraud) or allow employees to __3__ (embezzle) funds inevitably face legal consequences and public backlash. The most __4__ (insidious) threat to business ethics is the normalization of small acts of corruption—executives who accept a __5__ (bribe) often justify it as a minor compromise, unaware of the systemic damage they cause.`,
    blanks: ["integrity", "fraud", "embezzle", "insidious", "bribe"],
  },
  {
    title: "Language and Communication",
    level: "Intermediate",
    words: ["eloquent", "ambiguous", "demeanor", "unconventional", "articulate"],
    passage: `An __1__ (eloquent) speaker can transform a difficult idea into a message that resonates with any audience. However, language becomes problematic when it is __2__ (ambiguous)—open to multiple interpretations. A speaker's __3__ (demeanor) on stage often matters as much as the words themselves. The most memorable communicators tend to use __4__ (unconventional) approaches that challenge their audience to think differently, while remaining precise and __5__ (articulate) in their delivery.`,
    blanks: ["eloquent", "ambiguous", "demeanor", "unconventional", "articulate"],
  },
];

let currentReading = null, readingBlanks = [];

function initReadingPassages() { renderReadingSelect(); }

function renderReadingSelect() {
  const list = document.getElementById('reading-list');
  list.innerHTML = READING_PASSAGES.map((p, i) => `
    <div class="card" style="cursor:pointer;transition:.15s;" onmouseover="this.style.borderColor='var(--accent)'" onmouseout="this.style.borderColor='var(--border)'" onclick="startReading(${i})">
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <div>
          <div style="font-weight:600;font-size:15px;">${p.title}</div>
          <div style="font-size:12px;color:var(--text2);margin-top:4px;">Từ vựng: ${p.words.join(', ')}</div>
        </div>
        <span class="badge lvl-1">${p.level}</span>
      </div>
    </div>`).join('');
}

function startReading(idx) {
  currentReading = READING_PASSAGES[idx];
  document.getElementById('reading-select').style.display = 'none';
  document.getElementById('reading-game').style.display = 'block';
  document.getElementById('reading-result').style.display = 'none';

  const wb = document.getElementById('reading-wordbank');
  const shuffledWords = shuffle([...currentReading.words]);
  wb.innerHTML = shuffledWords.map(w => `<span class="word-chip" data-word="${w}" onclick="fillFromBank(this,'${w}')">${w}</span>`).join('');

  let passageHTML = currentReading.passage;
  passageHTML = passageHTML.replace(/__(\d+)\s*\(([^)]+)\)/g, (m, num, word) =>
    `<input class="blank-input" data-answer="${word}" data-num="${num}" size="${word.length + 2}" placeholder="${'_'.repeat(word.length)}" oninput="onBlankInput(this)">`
  );
  document.getElementById('reading-passage').innerHTML = passageHTML;
  readingBlanks = document.querySelectorAll('.blank-input');
}

function fillFromBank(chip, word) {
  const active = [...readingBlanks].find(b => b === document.activeElement) || [...readingBlanks].find(b => !b.value);
  if (active) { active.value = word; chip.classList.add('used'); onBlankInput(active); }
}

function onBlankInput(inp) { inp.className = 'blank-input'; }

function checkReading() {
  let correct = 0;
  readingBlanks.forEach(b => {
    const ans = b.value.trim().toLowerCase();
    const expected = b.dataset.answer.toLowerCase();
    if (ans === expected) { b.classList.add('correct'); correct++; }
    else { b.classList.add('wrong'); b.value = b.dataset.answer; }
  });
  const total = readingBlanks.length;
  const res = document.getElementById('reading-result');
  res.style.display = 'block';
  const pts = correct * 5;
  if (pts > 0) addXP(pts);
  res.innerHTML = `<div class="quiz-feedback ${correct === total ? 'correct' : 'wrong'}">
    ${correct === total ? '🎉 Hoàn hảo! Tất cả đều đúng!' : `✅ ${correct}/${total} câu đúng – Đáp án đã được điền vào ô sai`} &nbsp; ⚡ +${pts}đ
  </div>`;
}

function resetReading() { startReading(READING_PASSAGES.indexOf(currentReading)); }

function showReadingHint() {
  const firstWrong = [...readingBlanks].find(b => !b.value);
  if (firstWrong) {
    firstWrong.placeholder = firstWrong.dataset.answer.substring(0, 2) + '...';
    firstWrong.focus();
  }
}

function backToReadingSelect() {
  document.getElementById('reading-select').style.display = 'block';
  document.getElementById('reading-game').style.display = 'none';
}

// ═══════════════════════════════════════════════════════
//  CHINESE / HANZIWRITER
// ═══════════════════════════════════════════════════════
function renderChinese() {
  const q = document.getElementById('zh-search').value.toLowerCase();
  const filtered = chineseData.filter(v => !q || v.hanzi.includes(q) || v.pinyin.includes(q) || v.meaning.toLowerCase().includes(q));
  const grid = document.getElementById('hanzi-grid');
  if (!filtered.length) { grid.innerHTML = '<div class="empty">Không tìm thấy Hán tự nào</div>'; return; }
  grid.innerHTML = filtered.map((v, i) => `
    <div class="hanzi-card">
      <div class="hanzi-char">${v.hanzi}</div>
      <div class="pinyin">${v.pinyin}</div>
      <div class="hanzi-meaning">${v.meaning}</div>
      ${v.example ? `<div class="example" style="text-align:center;margin:8px 0;">${v.example}</div>` : ''}
      <div class="hanzi-writer-box" id="hzbox-${i}"><div id="hz-target-${i}"></div></div>
      <div class="stroke-controls">
        <button class="btn sm secondary" onclick="animateHanzi(${i},'${v.hanzi}')">▶ Xem nét</button>
        <button class="btn sm secondary" onclick="quizHanzi(${i},'${v.hanzi}')">✏️ Tập viết</button>
        <button class="btn sm secondary" onclick="speak('${v.hanzi}','zh-CN')">🔊</button>
      </div>
    </div>`).join('');
}

function animateHanzi(idx, char) {
  const el = document.getElementById('hz-target-' + idx);
  el.innerHTML = '';
  if (typeof HanziWriter === 'undefined') { el.textContent = '⚠️ Cần kết nối internet để tải HanziWriter'; return; }
  try {
    const w = HanziWriter.create(el, char, { width: 120, height: 120, padding: 8, strokeColor: '#6c63ff', radicalColor: '#00d4aa', showOutline: true });
    w.animateCharacter();
  } catch(e) { el.textContent = char; }
}

function quizHanzi(idx, char) {
  const el = document.getElementById('hz-target-' + idx);
  el.innerHTML = '';
  if (typeof HanziWriter === 'undefined') { el.textContent = '⚠️ Cần kết nối internet'; return; }
  try {
    const w = HanziWriter.create(el, char, { width: 120, height: 120, padding: 8, strokeColor: '#6c63ff', showOutline: false, drawingColor: '#00d4aa', drawingWidth: 4 });
    w.quiz({ onMistake: () => {}, onComplete: () => { setTimeout(() => animateHanzi(idx, char), 800); } });
  } catch(e) { el.textContent = char; }
}

// ═══════════════════════════════════════════════════════
//  IMPORT / EXPORT
// ═══════════════════════════════════════════════════════
function importJSON(e) {
  const file = e.target.files[0];
  if (!file) return;
  const targetFolder = document.getElementById('upload-target-folder').value || null;
  const reader = new FileReader();
  reader.onload = ev => {
    try {
      const data = JSON.parse(ev.target.result);
      if (!Array.isArray(data)) throw new Error('Phải là mảng JSON');
      const newWords = data.map((v, i) => ({
        word: v.word || '', ipa: v.ipa || '', pos: v.pos || '', definition: v.definition || '', definitionEN: v.definitionEN || '',
        example: v.example || '', synonyms: v.synonyms || '', antonyms: v.antonyms || '',
        level: v.level ?? 0, favorite: false, folder: targetFolder, id: vocabData.length + i
      }));
      vocabData = [...vocabData, ...newWords];
      saveVocab(); updateStats(); renderFolders();
      const st = document.getElementById('import-status');
      st.style.display = 'block';
      st.textContent = `✅ Đã import thành công ${newWords.length} từ vựng!`;
    } catch(err) { alert('Lỗi đọc file: ' + err.message); }
  };
  reader.readAsText(file, 'UTF-8');
}

function importChineseJSON(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    try {
      const data = JSON.parse(ev.target.result);
      const newChars = data.map((v, i) => ({ hanzi: v.hanzi || '', pinyin: v.pinyin || '', meaning: v.meaning || '', example: v.example || '', level: v.level || 1, id: chineseData.length + i }));
      chineseData = [...chineseData, ...newChars];
      saveChinese();
      alert(`✅ Đã import ${newChars.length} Hán tự!`);
    } catch(err) { alert('Lỗi: ' + err.message); }
  };
  reader.readAsText(file, 'UTF-8');
}

function downloadSample() {
  const sample = [
    { word: "example", ipa: "/ɪɡˈzæmpəl/", pos: "noun", definition: "Ví dụ, mẫu để noi theo", definitionEN: "A thing characteristic of its kind", example: "This is an example sentence.", synonyms: "Instance, Case", antonyms: "Exception", level: 0 }
  ];
  const blob = new Blob([JSON.stringify(sample, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href = url; a.download = 'vocab_sample.json'; a.click();
  URL.revokeObjectURL(url);
}

// ═══════════════════════════════════════════════════════
//  UTILS
// ═══════════════════════════════════════════════════════
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

window.speechSynthesis?.addEventListener?.('voiceschanged', () => window.speechSynthesis.getVoices());
window.speechSynthesis?.getVoices?.();

document.addEventListener('DOMContentLoaded', init);
</script>
</body>
</html>
