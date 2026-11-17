# optometrist
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<meta name="theme-color" content="#0b1220">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>فحص النظر – حرف E (بالكاميرا)</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{margin:0;padding:0;height:100%;overscroll-behavior:none;}
body{font-family:system-ui,-apple-system,"Segoe UI",Tahoma,"Noto Kufi Arabic",sans-serif;
  background:#0b1220;color:#e8eefc;overflow:hidden;user-select:none;-webkit-user-select:none;touch-action:manipulation;}
#app{height:100vh;height:100dvh;max-width:680px;margin:0 auto;display:flex;flex-direction:column;position:relative;
  padding:env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);}
.screen{display:none;flex-direction:column;height:100%;min-height:0;}
.screen.active{display:flex;}

.topbar{padding:18px 20px 6px;text-align:center;flex:0 0 auto;}
.topbar h1{margin:0;font-size:22px;font-weight:800;}
.topbar .sub{font-size:12.5px;color:#8ea3c7;display:block;margin-top:4px;}
.content{flex:1;overflow-y:auto;padding:12px 16px 24px;display:flex;flex-direction:column;gap:12px;}

.card{background:#131c2e;border:1px solid #1e2a44;border-radius:16px;padding:14px 16px;font-size:14px;line-height:1.6;white-space:pre-line;}
.card.warn{background:#2a1f10;border-color:#4a3a17;color:#fbbf24;}
.row{display:flex;gap:10px;align-items:center;}
.row.between{justify-content:space-between;}
.row.gap{gap:10px;}
.center{text-align:center;}
.tiny{font-size:12px;color:#8ea3c7;line-height:1.6;}
.tiny.center{text-align:center;}

.btn{flex:1;padding:14px 18px;border-radius:14px;border:1px solid #26304a;background:#182238;
  color:#e8eefc;font-size:15px;font-weight:700;font-family:inherit;cursor:pointer;transition:.15s;min-height:50px;}
.btn:active{transform:scale(.97);background:#1e2a44;}
.btn.primary{background:linear-gradient(135deg,#2563eb,#3b82f6);border-color:#3b82f6;color:#fff;}
.btn.big{font-size:17px;min-height:58px;}
.btn.ghost{background:transparent;border-color:#26304a;}
.btn.on{background:#134e3a;border-color:#22c55e;color:#bbf7d0;}
.link{background:none;border:none;color:#60a5fa;font:inherit;font-weight:700;cursor:pointer;padding:0;text-decoration:underline;}

.eye-select{display:flex;gap:10px;}
.seg{flex:1;padding:15px 10px;border-radius:14px;border:2px solid #26304a;background:#131c2e;
  color:#8ea3c7;font:inherit;font-weight:700;font-size:14.5px;cursor:pointer;transition:.18s;}
.seg.active{border-color:#3b82f6;background:#17243f;color:#dbeafe;}

input[type=range]{-webkit-appearance:none;appearance:none;width:100%;height:34px;background:transparent;margin:6px 0;}
input[type=range]::-webkit-slider-runnable-track{height:6px;border-radius:3px;background:#26304a;}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:26px;height:26px;border-radius:50%;
  background:#3b82f6;border:3px solid #0b1220;margin-top:-10px;}

.calib-wrap{display:flex;justify-content:center;align-items:center;padding:20px 0;min-height:150px;}
.calib-card{width:300px;height:190px;border:3px dashed #3b82f6;border-radius:12px;
  background:rgba(59,130,246,.07);display:flex;align-items:center;justify-content:center;
  color:#60a5fa;font-size:12.5px;text-align:center;padding:10px;}

#screen-test{padding:0;position:relative;}
.hud{display:flex;justify-content:space-around;padding:10px 8px 6px;flex:0 0 auto;}
.hud-item{text-align:center;flex:1;}
.hud-item span{display:block;font-size:10.5px;color:#7d90b3;}
.hud-item b{font-size:14px;font-weight:800;color:#dbeafe;}
#trialTimerWrap[hidden]{display:none;}
.progress{height:4px;background:#1a2337;flex:0 0 auto;overflow:hidden;}
.progress > i{display:block;height:100%;width:0;background:linear-gradient(90deg,#3b82f6,#22c55e);transition:width .3s;}

.letter-stage{flex:1;min-height:0;position:relative;background:#ffffff;
  margin:10px 14px;border-radius:18px;overflow:hidden;box-shadow:inset 0 0 0 1px rgba(0,0,0,.06);}
.letter{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%) rotate(0deg);
  transform-origin:center center;will-change:transform;}
.letter .bar{position:absolute;left:0;width:100%;height:20%;background:#000;}
.letter .b1{top:0;}.letter .b2{top:40%;}.letter .b3{top:80%;}
.letter .spine{position:absolute;left:0;top:0;width:20%;height:100%;background:#000;}

/* ---- نافذة الكاميرا ---- */
#camWrap{
  position:absolute;left:10px;bottom:10px;
  width:130px;height:130px;border-radius:16px;overflow:hidden;
  border:2px solid #22c55e;background:#000;z-index:20;
  box-shadow:0 6px 20px rgba(0,0,0,.5);
  transition:opacity .2s;
}
#camWrap.off{border-color:#475569;opacity:.35;}
#camWrap.off .cam-indicator{display:none;}
#camVideo{width:100%;height:100%;object-fit:cover;transform:scaleX(-1);display:block;}
#camOverlay{position:absolute;inset:0;pointer-events:none;}
.cam-indicator{
  position:absolute;top:6px;left:6px;display:flex;align-items:center;gap:4px;
  background:rgba(0,0,0,.65);color:#4ade80;font-size:9px;font-weight:700;
  padding:2px 6px;border-radius:999px;
}
.cam-indicator .dot{width:6px;height:6px;border-radius:50%;background:#4ade80;
  box-shadow:0 0 6px #22c55e;animation:pulse 1.4s infinite;}
@keyframes pulse{50%{opacity:.35;}}

#gestureHint{
  position:absolute;left:50%;bottom:calc(130px + 20px);
  transform:translateX(-50%);
  background:rgba(0,0,0,.7);color:#4ade80;font-size:13px;font-weight:800;
  padding:8px 16px;border-radius:999px;z-index:21;pointer-events:none;
  opacity:0;transition:opacity .25s,transform .25s;
  white-space:nowrap;
}
#gestureHint.show{opacity:1;transform:translateX(-50%) translateY(-4px);}

.timeout-prompt{
  position:absolute;inset:0;z-index:30;display:none;align-items:center;justify-content:center;
  padding:24px;background:rgba(3,8,18,.78);
}
.timeout-prompt.show{display:flex;}
.timeout-card{
  width:min(100%,360px);padding:22px;border:1px solid #334155;border-radius:18px;
  background:#131c2e;text-align:center;box-shadow:0 12px 40px rgba(0,0,0,.45);
}
.timeout-card h2{margin:0 0 8px;font-size:20px;}
.timeout-card p{margin:0 0 16px;color:#a8b8d2;font-size:13px;line-height:1.6;}
.timeout-card .btn{width:100%;margin-top:8px;}

#camToggle{
  position:absolute;right:10px;bottom:10px;z-index:22;
  width:52px;height:52px;border-radius:14px;
  background:#182238;border:1px solid #26304a;color:#dbeafe;
  font-size:22px;cursor:pointer;display:flex;align-items:center;justify-content:center;
}
#camToggle.on{background:#134e3a;border-color:#22c55e;color:#4ade80;}

.pad{direction:ltr;display:grid;grid-template-columns:repeat(3,1fr);
  grid-template-rows:repeat(3,minmax(46px,58px));gap:8px;padding:4px 16px;flex:0 0 auto;}
.dirbtn{border-radius:16px;border:1px solid #26304a;background:#182238;color:#dbeafe;
  font-size:22px;cursor:pointer;display:flex;align-items:center;justify-content:center;font-family:inherit;}
.dirbtn:active{background:#2563eb;border-color:#3b82f6;transform:scale(.95);}
.dirbtn.up{grid-area:1/2/2/3;}.dirbtn.left{grid-area:2/1/3/2;}
.dirbtn.right{grid-area:2/3/3/4;}.dirbtn.down{grid-area:3/2/4/3;}

#cantSee{margin:2px 16px 14px;flex:0 0 auto;min-height:46px;font-size:14px;}

.result-hero{text-align:center;padding:26px 16px;border-radius:22px;
  background:linear-gradient(160deg,#16233c,#111a2b);border:1px solid #22304d;}
.result-hero .big{font-size:64px;font-weight:900;line-height:1;letter-spacing:-2px;
  background:linear-gradient(135deg,#60a5fa,#22c55e);-webkit-background-clip:text;
  background-clip:text;color:transparent;}
.result-hero .unit{font-size:14px;color:#8ea3c7;margin-top:8px;}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.stat{background:#131c2e;border:1px solid #1e2a44;border-radius:14px;padding:12px;text-align:center;}
.stat span{display:block;font-size:11px;color:#7d90b3;margin-bottom:4px;}
.stat b{font-size:17px;color:#dbeafe;font-weight:800;}
.badge{display:inline-block;padding:6px 14px;border-radius:999px;font-size:13px;font-weight:800;}
.badge.good{background:rgba(34,197,94,.15);color:#4ade80;}
.badge.mid{background:rgba(251,191,36,.15);color:#fbbf24;}
.badge.low{background:rgba(239,68,68,.15);color:#f87171;}

ol.steps{margin:8px 0 0;padding-inline-start:20px;line-height:2;}
ol.steps li{font-size:13.5px;color:#b9c8e0;}

.feedback{position:absolute;inset:0;pointer-events:none;opacity:0;transition:opacity .12s;}
.feedback.ok{background:rgba(34,197,94,.22);opacity:1;}
.feedback.bad{background:rgba(239,68,68,.22);opacity:1;}
</style>
</head>
<body>
<div id="app">

  <section class="screen active" id="screen-home">
    <div class="topbar"><h1>فحص النظر</h1><span class="sub">اختبار حدة الإبصار — حرف E الدوّار</span></div>
    <div class="content">

      <div class="eye-select">
        <button class="seg active" data-eye="right">العين اليمنى</button>
        <button class="seg" data-eye="left">العين اليسرى</button>
      </div>

      <div class="card warn" id="calibWarn" style="display:none">
        ⚠️ لم تتم المعايرة بعد — النتائج قد تكون غير دقيقة.
        <button class="link" data-go="calib">معايرة الآن</button>
      </div>

      <div class="card">
        <div class="row between" style="margin-bottom:6px">
          <span>مسافة الفحص</span>
          <b id="distLabel" style="color:#60a5fa">1 متر</b>
        </div>
        <input type="range" id="distRange" min="0" max="10" step="1" value="3">
        <div class="tiny" id="rangeHint"></div>
      </div>

      <div class="card">
        <div class="row between" style="margin-bottom:6px">
          <span>وضع الإجابة</span>
          <b id="modeLabel" style="color:#60a5fa">باللمس</b>
        </div>
        <div class="eye-select">
          <button class="seg active" data-ans="touch">👆 لمس</button>
          <button class="seg" data-ans="camera">📷 كاميرا (إشارة يد)</button>
        </div>
        <div class="tiny" style="margin-top:8px">
          وضع الكاميرا يستخدم الكاميرا الأمامية لتتبع يدك عبر الإشارة بالإصبع. يحتاج إضاءة جيدة.
        </div>
      </div>

      <div class="card">
        <div class="row between" style="margin-bottom:6px">
          <span>دقة الفحص</span>
          <b id="testModeLabel" style="color:#60a5fa">سريع</b>
        </div>
        <div class="eye-select">
          <button class="seg active" data-mode="quick">سريع (3)</button>
          <button class="seg" data-mode="accurate">دقيق (5)</button>
        </div>
      </div>

      <button class="btn primary big" id="btnStart">ابدأ الفحص</button>
      <div class="row gap">
        <button class="btn ghost" data-go="calib">المعايرة</button>
        <button class="btn ghost" data-go="help">التعليمات</button>
      </div>
      <p class="tiny center">هذا التطبيق أداة مساعدة تقريبية وليس بديلاً عن الفحص الطبي.</p>
    </div>
  </section>

  <section class="screen" id="screen-calib">
    <div class="topbar"><h1>معايرة الشاشة</h1><span class="sub">خطوة ضرورية</span></div>
    <div class="content">
      <div class="card">
        ضع بطاقة بنكية أفقياً على الشاشة، وحرّك المؤشر حتى يتساوى الإطار الأزرق مع عرض البطاقة الحقيقي (85.6 مم).
      </div>
      <div class="calib-wrap"><div class="calib-card" id="calibCard">ضع البطاقة هنا</div></div>
      <input type="range" id="calibRange" min="120" max="520" step="1" value="300">
      <div class="row gap">
        <button class="btn ghost" id="calibReset">إعادة ضبط</button>
        <button class="btn primary" id="calibSave">حفظ</button>
      </div>
      <button class="btn ghost" data-go="home">رجوع</button>
    </div>
  </section>

  <section class="screen" id="screen-help">
    <div class="topbar"><h1>التعليمات</h1></div>
    <div class="content">
      <div class="card">
        <b>وضع الكاميرا (الإشارة بالإصبع):</b>
        <ol class="steps">
          <li>اسمح للمتصفح باستخدام الكاميرا.</li>
          <li>ارفع يدك أمام الكاميرا على مسافة 30–60 سم.</li>
          <li>أشر بإصبع السبابة نحو الاتجاه الذي يشير إليه الحرف.</li>
          <li>ابقِ الإشارة ثابتة لثانية تقريباً حتى تؤكد.</li>
          <li>انتظر 3 ثوانٍ بين الأسئلة، وأزل يدك ثم أعد إظهارها لتجنب تكرار الإشارة السابقة.</li>
          <li>لديك 15 ثانية للاختيار بالكاميرا؛ عند انتهاء الوقت يمكنك إعادة السؤال أو الانتقال للسؤال التالي، ويُحسب تجاوزه خطأً.</li>
          <li>أو استخدم الأزرار بالأسفل كبديل.</li>
        </ol>
      </div>
      <div class="card warn">
        ⚠️ إضاءة جيدة ضرورية. الكاميرا الأمامية تعمل داخل المتصفح فقط، ولا يتم إرسال أي فيديو لأي مكان.
      </div>
      <button class="btn ghost" data-go="home">رجوع</button>
    </div>
  </section>

  <section class="screen" id="screen-test">
    <div class="hud">
      <div class="hud-item"><span>العين</span><b id="hudEye">اليمنى</b></div>
      <div class="hud-item"><span>المسافة</span><b id="hudDist">1 م</b></div>
      <div class="hud-item"><span>المستوى</span><b id="hudLevel">1/1</b></div>
      <div class="hud-item" id="trialTimerWrap" hidden><span id="trialTimerLabel">الوقت للاختيار</span><b id="trialTimer">15</b></div>
    </div>
    <div class="progress"><i id="progressBar"></i></div>

    <div class="letter-stage" id="letterStage">
      <div class="letter" id="letter">
        <div class="bar b1"></div><div class="bar b2"></div><div class="bar b3"></div>
        <div class="spine"></div>
      </div>
      <div class="feedback" id="feedback"></div>
    </div>

    <div class="pad">
      <button class="dirbtn up"    data-dir="3">▲</button>
      <button class="dirbtn left"  data-dir="2">◀</button>
      <button class="dirbtn right" data-dir="0">▶</button>
      <button class="dirbtn down"  data-dir="1">▼</button>
    </div>
    <button class="btn ghost" id="cantSee">لا أرى الحرف</button>

    <!-- كاميرا -->
    <div id="camWrap" class="off">
      <video id="camVideo" playsinline muted autoplay></video>
      <canvas id="camOverlay"></canvas>
      <div class="cam-indicator"><span class="dot"></span>مباشر</div>
    </div>
    <button id="camToggle" aria-label="تشغيل/إيقاف الكاميرا">📷</button>
    <div id="gestureHint"></div>
    <div class="timeout-prompt" id="timeoutPrompt">
      <div class="timeout-card">
        <h2>انتهى وقت الاختيار</h2>
        <p>اختر إعادة السؤال أو تجاوزه. تجاوز السؤال يُحسب كإجابة خاطئة.</p>
        <button class="btn primary" id="retryTrial">إعادة السؤال</button>
        <button class="btn ghost" id="skipTrial">السؤال التالي</button>
      </div>
    </div>
  </section>

  <section class="screen" id="screen-result">
    <div class="topbar"><h1>النتيجة</h1><span class="sub" id="resEye">العين اليمنى</span></div>
    <div class="content">
      <div class="result-hero">
        <div class="big" id="resAcuity">—</div>
        <div class="unit">حدة الإبصار (عشري)</div>
        <div style="margin-top:14px"><span class="badge good" id="resBadge">—</span></div>
      </div>
      <div class="grid2">
        <div class="stat"><span>Snellen (قدم)</span><b id="resSnellen">—</b></div>
        <div class="stat"><span>Snellen (متر)</span><b id="resMeter">—</b></div>
        <div class="stat"><span>logMAR</span><b id="resLogmar">—</b></div>
        <div class="stat"><span>العين</span><b id="resEye2">—</b></div>
      </div>
      <div class="card" id="resAdvice">—</div>
      <div class="row gap"><button class="btn primary" id="btnOtherEye">فحص العين الأخرى</button></div>
      <div class="row gap">
        <button class="btn ghost" id="btnRetry">إعادة الفحص</button>
        <button class="btn ghost" data-go="home">الرئيسية</button>
      </div>
    </div>
  </section>

</div>

<!-- ===== MediaPipe Hands (تحميل من CDN) ===== -->
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.4.1675469240/hands.js" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils@0.3.1675466862/camera_utils.js" crossorigin="anonymous"></script>

<script>
(function(){
'use strict';

/* =========================================================
   ثوابت
   ========================================================= */
const ARC_TAN   = Math.tan((1/60) * Math.PI / 180);
const DISTANCES = [0.4, 0.5, 0.75, 1, 1.5, 2, 2.5, 3, 4, 5, 6];
const MIN_PX    = 7;
const CARD_MM   = 85.6;
const LS_KEY    = 'tumbling_e_test_v2';
const INTER_TRIAL_DELAY_MS = 3000;
const TRIAL_TIMEOUT_SECONDS = 15;

/* إعدادات اكتشاف الإشارة */
const GESTURE_MIN_LEN      = 0.06;  // أدنى طول متجه (طبيعي 0-1)
const GESTURE_CONFIRM_MS   = 1000;  // مدة تثبيت الإشارة قبل التأكيد
const GESTURE_COOLDOWN_MS  = 1500;  // منع تكرار نفس الإشارة فوراً

/* =========================================================
   الحالة
   ========================================================= */
const state = {
  pxPerMm: 0, calibrated: false,
  distIdx: 3, eye: 'right', quick: true, answerMode: 'touch',
  levels: [], li: 0, correct: 0, wrong: 0, dir: -1,
  locked: false, running: false, lastPassed: null, reachedLimit: false,
  results: { right: null, left: null }
};

const $  = (s, r) => (r || document).querySelector(s);
const $$ = (s, r) => Array.prototype.slice.call((r || document).querySelectorAll(s));
function show(id){ $$('.screen').forEach(s => s.classList.toggle('active', s.id === id)); }
function go(name){ show('screen-' + name); }
function distLabel(d){ if (d<1) return Math.round(d*100)+' سم'; return (d%1===0?d:d.toFixed(2))+' متر'; }
function distShort(d){ if (d<1) return Math.round(d*100)+' سم'; return d+' م'; }
function fmtNum(n){ return (Math.round(n*100)/100).toString(); }

function letterHeightPx(a){
  const Dmm = DISTANCES[state.distIdx] * 1000;
  const ppm = state.pxPerMm > 0 ? state.pxPerMm : 5.5;
  return (5 * Dmm * ARC_TAN / a) * ppm;
}

/* =========================================================
   التخزين
   ========================================================= */
function load(){
  try{
    const s = JSON.parse(localStorage.getItem(LS_KEY) || '{}');
    if (s.pxPerMm > 0){ state.pxPerMm = s.pxPerMm; state.calibrated = true; }
    if (typeof s.distIdx === 'number') state.distIdx = s.distIdx;
    if (s.eye === 'right' || s.eye === 'left') state.eye = s.eye;
    if (typeof s.quick === 'boolean') state.quick = s.quick;
    if (s.answerMode === 'touch' || s.answerMode === 'camera') state.answerMode = s.answerMode;
  }catch(e){}
}
function save(){
  try{
    localStorage.setItem(LS_KEY, JSON.stringify({
      pxPerMm: state.pxPerMm, distIdx: state.distIdx,
      eye: state.eye, quick: state.quick, answerMode: state.answerMode
    }));
  }catch(e){}
}

/* =========================================================
   المستويات
   ========================================================= */
function computeLevels(){
  const stage   = $('#letterStage');
  const maxSize = Math.min(stage.clientWidth, stage.clientHeight) * 0.78;
  const out = [];
  for (let lm = 1.3; lm >= -0.3001; lm -= 0.1){
    const a  = Math.pow(10, -lm);
    const px = letterHeightPx(a);
    if (px <= maxSize && px >= MIN_PX) out.push({ a, px });
  }
  return out;
}

/* =========================================================
   بدء الفحص
   ========================================================= */
function beginTest(){
  clearTrialTimers();
  $('#timeoutPrompt').classList.remove('show');
  state.lastPassed = null; state.reachedLimit = false;
  state.li = 0; state.correct = 0; state.wrong = 0;
  state.dir = -1; state.locked = false;

  $('#hudEye').textContent  = state.eye === 'right' ? 'اليمنى' : 'اليسرى';
  $('#hudDist').textContent = distShort(DISTANCES[state.distIdx]);
  $('#progressBar').style.width = '0%';
  $('#letter').style.opacity = '0';
  show('screen-test');

  requestAnimationFrame(() => requestAnimationFrame(() => {
    const levels = computeLevels();
    if (!levels.length){
      $('#letter').style.opacity = '1';
      alert('لا يمكن عرض الحروف بالحجم المطلوب. غيّر المسافة أو أعد المعايرة.');
      go('home'); return;
    }
    state.levels = levels;
    state.running = true;
    $('#letter').style.opacity = '1';
    startLevel();

    // تشغيل الكاميرا تلقائياً إن كان الوضع كاميرا
    if (state.answerMode === 'camera') startCamera();
  }));

  requestWakeLock();
}

function startLevel(){ state.correct = 0; state.wrong = 0; updateHud(); nextTrial(); }

function nextTrial(){
  let d = Math.floor(Math.random() * 4);
  if (d === state.dir) d = (d + 1 + Math.floor(Math.random() * 3)) % 4;
  state.dir = d;
  renderLetter(); updateHud();
  state.locked = false;
  startTrialTimer();
}

function renderLetter(){
  const lv = state.levels[state.li];
  const el = $('#letter');
  el.style.width  = lv.px.toFixed(2) + 'px';
  el.style.height = lv.px.toFixed(2) + 'px';
  el.style.transform = 'translate(-50%,-50%) rotate(' + (state.dir * 90) + 'deg)';
}

function updateHud(){
  const total = state.levels.length;
  $('#hudLevel').textContent = Math.min(state.li+1, total) + '/' + total;
  $('#progressBar').style.width = (state.li / total * 100).toFixed(1) + '%';
}

let trialTimerInterval = null;
let transitionTimerInterval = null;

function clearTrialTimers(){
  clearInterval(trialTimerInterval);
  clearInterval(transitionTimerInterval);
  trialTimerInterval = null;
  transitionTimerInterval = null;
}

function updateTrialTimer(label, seconds){
  $('#trialTimerLabel').textContent = label;
  $('#trialTimer').textContent = seconds;
  $('#trialTimerWrap').hidden = false;
}

function stopTrialTimer(){
  clearInterval(trialTimerInterval);
  trialTimerInterval = null;
}

function startTrialTimer(){
  stopTrialTimer();
  $('#timeoutPrompt').classList.remove('show');
  if (state.answerMode !== 'camera'){
    $('#trialTimerWrap').hidden = true;
    return;
  }
  if (!camRunning){
    updateTrialTimer('بانتظار تشغيل الكاميرا', '—');
    return;
  }

  let remaining = TRIAL_TIMEOUT_SECONDS;
  updateTrialTimer('الوقت للاختيار', remaining);
  trialTimerInterval = setInterval(() => {
    remaining--;
    if (remaining <= 0){
      stopTrialTimer();
      state.locked = true;
      updateTrialTimer('انتهى وقت الاختيار', 0);
      $('#timeoutPrompt').classList.add('show');
      hideGestureHint();
      return;
    }
    updateTrialTimer('الوقت للاختيار', remaining);
  }, 1000);
}

function startInterTrialDelay(){
  stopTrialTimer();
  state.locked = true;
  let remaining = INTER_TRIAL_DELAY_MS / 1000;
  updateTrialTimer('السؤال التالي بعد', remaining);
  transitionTimerInterval = setInterval(() => {
    remaining--;
    if (remaining <= 0){
      clearInterval(transitionTimerInterval);
      transitionTimerInterval = null;
      const need = state.quick ? 2 : 3;
      const total = state.quick ? 3 : 5;
      if (state.correct >= need) return endLevel(true);
      if (state.wrong >= need) return endLevel(false);
      if (state.correct + state.wrong >= total) return endLevel(state.correct >= need);
      state.locked = false;
      nextTrial();
      return;
    }
    updateTrialTimer('السؤال التالي بعد', remaining);
  }, 1000);
}

/* =========================================================
   الإجابة
   ========================================================= */
function answer(dir){
  if (!state.running || state.locked) return;
  state.locked = true;
  stopTrialTimer();
  if (state.answerMode === 'camera') gesture.awaitingRelease = true;

  const ok = (dir === state.dir);
  if (ok) state.correct++; else state.wrong++;
  flash(ok);
  startInterTrialDelay();
}

function flash(ok){
  const fb = $('#feedback');
  fb.className = 'feedback ' + (ok ? 'ok' : 'bad');
  setTimeout(() => fb.className = 'feedback', 320);
}

function endLevel(passed){
  if (passed){
    state.lastPassed = state.levels[state.li].a;
    if (state.li + 1 < state.levels.length){ state.li++; startLevel(); return; }
    state.reachedLimit = true;
  }
  finish();
}

function finish(){
  state.running = false;
  clearTrialTimers();
  $('#timeoutPrompt').classList.remove('show');
  $('#trialTimerWrap').hidden = true;
  stopCamera();
  state.results[state.eye] = state.lastPassed;
  renderResult();
  show('screen-result');
}

/* =========================================================
   النتيجة
   ========================================================= */
function renderResult(){
  const a = state.lastPassed;
  const eye = state.eye === 'right' ? 'اليمنى' : 'اليسرى';
  const reachedLimit = state.reachedLimit;

  $('#resEye').textContent  = 'العين ' + eye;
  $('#resEye2').textContent = eye;

  if (a == null){
    $('#resAcuity').textContent  = '—';
    $('#resSnellen').textContent = '—';
    $('#resMeter').textContent   = '—';
    $('#resLogmar').textContent  = '—';
    $('#resBadge').className = 'badge low';
    $('#resBadge').textContent = 'غير محددة';
    $('#resAdvice').textContent = 'لم يتم تجاوز أصغر مستوى. تأكد من الإضاءة أو اقترب قليلاً.';
    return;
  }

  const dec = a, sn20 = Math.round(20/dec), sn6 = Math.round(6/dec);
  const logmar = -Math.log10(dec);
  const prefix = reachedLimit ? '≥ ' : '';
  const better = reachedLimit ? ' أو أفضل' : '';

  $('#resAcuity').textContent  = prefix + fmtNum(dec);
  $('#resSnellen').textContent = '20 / ' + sn20 + better;
  $('#resMeter').textContent   = '6 / ' + sn6 + better;
  $('#resLogmar').textContent  = (reachedLimit ? '≤ ' : '') + logmar.toFixed(2);

  let cls='good', txt='', adv='';
  if (reachedLimit){
    if (dec >= 1.0){ cls='good'; txt='إبصار طبيعي';
      adv='أكملت جميع المستويات بنجاح، ونتيجتك ضمن المعدل الطبيعي.'; }
    else { cls='good'; txt='أفضل من ' + fmtNum(dec);
      adv='أكملت جميع المستويات المتاحة بنجاح ✅\n' +
          'حدة إبصارك على الأقل ' + fmtNum(dec) + '.\n' +
          '💡 لقياس أدق، أعد الفحص من مسافة أبعد (2–3 أمتار).'; }
  } else {
    if (dec >= 1.0){ cls='good'; txt='إبصار طبيعي'; adv='حدة الإبصار طبيعية. حافظ على فحص دوري.'; }
    else if (dec >= 0.7){ cls='good'; txt='قريب من الطبيعي'; adv='قريب من الطبيعي. راجع أخصائي إن استمرت الأعراض.'; }
    else if (dec >= 0.3){ cls='mid';  txt='ضعف متوسط';  adv='يُنصح بزيارة طبيب عيون لفحص الانكسار.'; }
    else { cls='low'; txt='ضعف شديد'; adv='راجع طبيب عيون في أقرب وقت.'; }
  }
  $('#resBadge').className = 'badge ' + cls;
  $('#resBadge').textContent = txt;
  $('#resAdvice').textContent = adv;
}

/* =========================================================
   Wake Lock
   ========================================================= */
let wakeLock = null;
async function requestWakeLock(){
  try{
    if ('wakeLock' in navigator && !wakeLock){
      wakeLock = await navigator.wakeLock.request('screen');
      wakeLock.addEventListener('release', () => { wakeLock = null; });
    }
  }catch(e){}
}

/* =========================================================
   الكاميرا + MediaPipe Hands
   ========================================================= */
let handsModel = null;
let cameraObj  = null;
let camRunning = false;

/* حالة تتبع الإشارة */
const gesture = {
  candidate: -1,
  candidateSince: 0,
  lastConfirmedDir: -1,
  lastConfirmedAt: 0,
  awaitingRelease: false
};

const DIR_NAMES = ['يمين','أسفل','يسار','أعلى'];

async function startCamera(){
  if (camRunning || typeof Hands === 'undefined') return;

  const video = $('#camVideo');
  const overlay = $('#camOverlay');

  // تهيئة النموذج
  handsModel = new Hands({
    locateFile: (file) => 'https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.4.1675469240/' + file
  });
  handsModel.setOptions({
    maxNumHands: 1,
    modelComplexity: 0,
    minDetectionConfidence: 0.6,
    minTrackingConfidence: 0.6
  });
  handsModel.onResults(onHandResults);

  try{
    cameraObj = new Camera(video, {
      onFrame: async () => {
        try { await handsModel.send({ image: video }); } catch(e){}
      },
      width: 320, height: 320, facingMode: 'user'
    });
    await cameraObj.start();
    camRunning = true;
    $('#camWrap').classList.remove('off');
    $('#camToggle').classList.add('on');
    if (state.running && !state.locked) startTrialTimer();
  }catch(err){
    console.warn('Camera start failed:', err);
    stopCamera();
    // الرجوع لوضع اللمس
    state.answerMode = 'touch';
    startTrialTimer();
    save();
    updateHomeUI();
    alert('لم نتمكن من الوصول إلى الكاميرا. تأكد من منح الإذن أو استخدم وضع اللمس.');
  }
}

function stopCamera(){
  try{ if (cameraObj){ cameraObj.stop(); cameraObj = null; } }catch(e){}
  try{ if (handsModel){ handsModel.close(); handsModel = null; } }catch(e){}
  camRunning = false;
  $('#camWrap').classList.add('off');
  $('#camToggle').classList.remove('on');
  clearOverlay();
  hideGestureHint();
  resetGestureState();
  if (state.running) startTrialTimer();
}

function resetGestureState(){
  gesture.candidate = -1;
  gesture.candidateSince = 0;
  gesture.lastConfirmedDir = -1;
  gesture.lastConfirmedAt = 0;
  gesture.awaitingRelease = false;
}

/* =========================================================
   تحليل نتيجة MediaPipe
   ========================================================= */
function onHandResults(results){
  const overlay = $('#camOverlay');
  const video = $('#camVideo');
  const w = video.videoWidth  || 320;
  const h = video.videoHeight || 320;
  if (overlay.width !== w)  overlay.width  = w;
  if (overlay.height !== h) overlay.height = h;

  const ctx = overlay.getContext('2d');
  ctx.clearRect(0, 0, w, h);

  if (!results.multiHandLandmarks || results.multiHandLandmarks.length === 0){
    resetGestureState();
    hideGestureHint();
    return;
  }

  const lm = results.multiHandLandmarks[0];

  // رسم نقاط اليد للمعاينة
  drawHand(ctx, lm, w, h);
  if (!state.running || state.locked){
    gesture.candidate = -1;
    gesture.candidateSince = 0;
    hideGestureHint();
    return;
  }

  // كشف الاتجاه: من الرسغ (0) إلى طرف السبابة (8)
  const wrist = lm[0];
  const tip   = lm[8];
  const dx = tip.x - wrist.x;
  const dy = tip.y - wrist.y;
  const len = Math.hypot(dx, dy);

  // إذا كانت الإشارة قصيرة جداً — لا نعتبرها
  if (len < GESTURE_MIN_LEN){
    resetGestureState();
    hideGestureHint();
    return;
  }
  if (gesture.awaitingRelease){
    gesture.candidate = -1;
    gesture.candidateSince = 0;
    showGestureHint('أزل يدك ثم أعد إظهارها للاختيار', false);
    return;
  }

  let dir;
  if (Math.abs(dx) > Math.abs(dy)) dir = dx > 0 ? 2 : 0;  // يسار / يمين (مرآة)
  else                              dir = dy > 0 ? 1 : 3;  // أسفل / أعلى

  const now = performance.now();

  // تأكيد الإشارة بعد ثباتها
  if (dir === gesture.candidate){
    const held = now - gesture.candidateSince;
    const progress = Math.min(1, held / GESTURE_CONFIRM_MS);
    showGestureHint('أشير ' + DIR_NAMES[dir] + '  ' + Math.round(progress*100) + '%', false);

    if (held >= GESTURE_CONFIRM_MS){
      // منع التكرار السريع
      if (!(dir === gesture.lastConfirmedDir && now - gesture.lastConfirmedAt < GESTURE_COOLDOWN_MS)){
        gesture.lastConfirmedDir = dir;
        gesture.lastConfirmedAt  = now;
        showGestureHint('✓ ' + DIR_NAMES[dir], true);
        answer(dir);
        setTimeout(hideGestureHint, 400);
      }
      gesture.candidate = -1;
      gesture.candidateSince = 0;
      gesture.awaitingRelease = true;
    }
  } else {
    gesture.candidate = dir;
    gesture.candidateSince = now;
    showGestureHint('أشير ' + DIR_NAMES[dir] + '  0%', false);
  }
}

/* رسم هيكل اليد بسيط */
function drawHand(ctx, lm, w, h){
  const connections = [
    [0,1],[1,2],[2,3],[3,4],
    [0,5],[5,6],[6,7],[7,8],
    [0,9],[9,10],[10,11],[11,12],
    [0,13],[13,14],[14,15],[15,16],
    [0,17],[17,18],[18,19],[19,20],
    [5,9],[9,13],[13,17]
  ];
  ctx.strokeStyle = '#22c55e';
  ctx.lineWidth   = 2;
  ctx.beginPath();
  connections.forEach(([a,b]) => {
    ctx.moveTo(lm[a].x * w, lm[a].y * h);
    ctx.lineTo(lm[b].x * w, lm[b].y * h);
  });
  ctx.stroke();

  lm.forEach((p, i) => {
    ctx.beginPath();
    ctx.arc(p.x * w, p.y * h, i === 8 ? 5 : 3, 0, Math.PI*2);
    ctx.fillStyle = i === 8 ? '#fbbf24' : '#60a5fa';
    ctx.fill();
  });
}

function clearOverlay(){
  const overlay = $('#camOverlay');
  const ctx = overlay.getContext('2d');
  ctx.clearRect(0, 0, overlay.width, overlay.height);
}

/* =========================================================
   تلميح الإشارة
   ========================================================= */
let hintTimer = null;
function showGestureHint(text, confirmed){
  const el = $('#gestureHint');
  el.textContent = text;
  el.style.color = confirmed ? '#22c55e' : '#fbbf24';
  el.classList.add('show');
  clearTimeout(hintTimer);
  if (!confirmed) hintTimer = setTimeout(hideGestureHint, 500);
}
function hideGestureHint(){
  $('#gestureHint').classList.remove('show');
}

/* =========================================================
   المعايرة
   ========================================================= */
function initCalib(){
  const card  = $('#calibCard');
  const range = $('#calibRange');
  const maxW = Math.min(520, window.innerWidth - 60);
  range.max = maxW;
  let initial = state.pxPerMm > 0 ? Math.round(state.pxPerMm * CARD_MM) : 300;
  initial = Math.max(Number(range.min), Math.min(maxW, initial));
  range.value = initial;
  card.style.width = initial + 'px';
  range.addEventListener('input', () => { card.style.width = range.value + 'px'; });
  $('#calibReset').addEventListener('click', () => { range.value = 300; card.style.width = '300px'; });
  $('#calibSave').addEventListener('click', () => {
    state.pxPerMm = parseFloat(range.value) / CARD_MM;
    state.calibrated = true; save(); updateHomeUI(); go('home');
  });
}

/* =========================================================
   الواجهة
   ========================================================= */
function updateHomeUI(){
  $('#calibWarn').style.display = state.calibrated ? 'none' : 'block';

  $$('.seg[data-eye]').forEach(b => b.classList.toggle('active', b.dataset.eye === state.eye));
  $$('.seg[data-mode]').forEach(b => b.classList.toggle('active', (b.dataset.mode==='quick') === state.quick));
  $$('.seg[data-ans]').forEach(b => b.classList.toggle('active', b.dataset.ans === state.answerMode));

  $('#modeLabel').textContent = state.answerMode === 'camera' ? '📷 كاميرا' : '👆 لمس';
  $('#testModeLabel').textContent = state.quick ? 'سريع' : 'دقيق';
  $('#distRange').value = state.distIdx;
  $('#distLabel').textContent = distLabel(DISTANCES[state.distIdx]);

  updateRangeHint();
}

function updateRangeHint(){
  const D   = DISTANCES[state.distIdx] * 1000;
  const ppm = state.pxPerMm > 0 ? state.pxPerMm : 5.5;
  const bestAc  = 5 * D * ARC_TAN * ppm / MIN_PX;
  const maxPx   = Math.min(window.innerWidth, window.innerHeight) * 0.6;
  const worstAc = 5 * D * ARC_TAN * ppm / maxPx;

  let html = 'النطاق التقريبي: <b style="color:#93c5fd">' +
    (worstAc >= 0.05 ? fmtNum(worstAc) : '—') + '</b> إلى <b style="color:#93c5fd">' +
    (bestAc >= 0.05 ? fmtNum(bestAc) : '—') + '</b>';
  if (bestAc < 0.8) html += '<br><span style="color:#fbbf24">⚠️ جرّب مسافة 2–3 أمتار لقياس أدق.</span>';
  if (!state.calibrated) html += ' <span style="color:#fbbf24">• غير معاير</span>';
  $('#rangeHint').innerHTML = html;
}

/* =========================================================
   الأحداث
   ========================================================= */
function bindEvents(){
  $$('[data-go]').forEach(b => b.addEventListener('click', () => go(b.dataset.go)));

  $$('.seg[data-eye]').forEach(b => b.addEventListener('click', () => {
    state.eye = b.dataset.eye; save(); updateHomeUI();
  }));
  $$('.seg[data-mode]').forEach(b => b.addEventListener('click', () => {
    state.quick = (b.dataset.mode === 'quick'); save(); updateHomeUI();
  }));
  $$('.seg[data-ans]').forEach(b => b.addEventListener('click', () => {
    state.answerMode = b.dataset.ans; save(); updateHomeUI();
  }));

  $('#distRange').addEventListener('input', e => {
    state.distIdx = Number(e.target.value); save(); updateHomeUI();
  });

  $('#btnStart').addEventListener('click', () => {
    if (!state.calibrated){
      if (confirm('لم تقم بمعايرة الشاشة. هل تريد المعايرة أولاً؟')){ go('calib'); return; }
    }
    beginTest();
  });

  $$('.dirbtn').forEach(b => b.addEventListener('click', () => answer(Number(b.dataset.dir))));
  $('#cantSee').addEventListener('click', () => answer(-1));
  $('#retryTrial').addEventListener('click', () => {
    $('#timeoutPrompt').classList.remove('show');
    state.locked = false;
    resetGestureState();
    startTrialTimer();
  });
  $('#skipTrial').addEventListener('click', () => {
    $('#timeoutPrompt').classList.remove('show');
    state.locked = false;
    if (state.answerMode === 'camera') gesture.awaitingRelease = true;
    answer(-1);
  });

  document.addEventListener('keydown', e => {
    if (!state.running) return;
    const map = { ArrowRight:0, ArrowDown:1, ArrowLeft:2, ArrowUp:3 };
    if (map.hasOwnProperty(e.key)){ e.preventDefault(); answer(map[e.key]); }
    if (e.key === ' ') { e.preventDefault(); answer(-1); }
  });

  // سحب على الشاشة
  const stage = $('#letterStage');
  let sx=0, sy=0, tracking=false;
  stage.addEventListener('touchstart', e => {
    if (e.touches.length !== 1) return;
    sx = e.touches[0].clientX; sy = e.touches[0].clientY; tracking = true;
  }, { passive: true });
  stage.addEventListener('touchend', e => {
    if (!tracking) return; tracking = false;
    const t = e.changedTouches[0];
    const dx = t.clientX - sx, dy = t.clientY - sy;
    if (Math.hypot(dx, dy) < 28) return;
    if (Math.abs(dx) > Math.abs(dy)) answer(dx > 0 ? 0 : 2);
    else answer(dy > 0 ? 1 : 3);
  }, { passive: true });

  // زر الكاميرا
  $('#camToggle').addEventListener('click', () => {
    if (camRunning) stopCamera(); else startCamera();
  });

  $('#btnOtherEye').addEventListener('click', () => {
    state.eye = (state.eye === 'right') ? 'left' : 'right';
    save(); beginTest();
  });
  $('#btnRetry').addEventListener('click', () => beginTest());

  window.addEventListener('resize', () => {
    if (state.running && state.levels.length){
      const lv = state.levels[state.li];
      const px = letterHeightPx(lv.a);
      if (px > MIN_PX){ lv.px = px; renderLetter(); }
    }
    updateRangeHint();
  });
}

/* =========================================================
   تشغيل
   ========================================================= */
function init(){
  load();
  initCalib();
  bindEvents();
  updateHomeUI();
}

// MediaPipe قد يتأخر في التحميل — ننتظر DOMContentLoaded
document.addEventListener('DOMContentLoaded', init);
document.addEventListener('gesturestart', e => e.preventDefault());

// تحذير إذا لم تُحمَّل مكتبة MediaPipe
window.addEventListener('load', () => {
  if (typeof Hands === 'undefined'){
    console.warn('MediaPipe Hands لم يتم تحميله من CDN — وضع الكاميرا لن يعمل.');
  }
});

})();
</script>
</body>
</html>ss