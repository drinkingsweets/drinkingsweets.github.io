<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="slides-format" content="viewport" />
  <title>ResumeGPT - мониторинг нового микросервиса</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;700;800&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <style type="text/tailwindcss">
    @theme {
      --color-bg: #f4f6fb;
      --color-bg-deep: #e8edf8;
      --color-surface: #ffffff;
      --color-text: #1f2a44;
      --color-text-secondary: #334155;
      --color-text-muted: #64748b;
      --color-accent-1: #3359c9;
      --color-accent-2: #6b8cff;
      --color-accent-3: #9fb6ff;
      --color-glass-bg: rgba(255,255,255,0.82);
      --color-glass-border: rgba(51,89,201,0.12);
      --color-vignette: rgba(0,0,0,0.05);
      --font-display: 'Manrope', sans-serif;
      --font-body: 'Manrope', sans-serif;
    }
  </style>
  <style>
    :root { --color-bg: #f4f6fb; --color-text: #1f2a44; --glow-color-rgb: 51,89,201; }
    *, *::before, *::after { box-sizing: border-box; }
    html, body { background: var(--color-bg); margin: 0; }
    body { font-family: var(--font-body); color: var(--color-text); overflow: hidden; height: 100vh; width: 100vw; }
    .deck { width: 100vw; height: 100vh; position: relative; }
    .slide { position: absolute; inset: 0; background: var(--color-bg); display: flex; flex-direction: column; align-items: center; justify-content: center; opacity: 0; transform: scale(0.97); transition: opacity .6s ease, transform .6s ease; pointer-events: none; overflow: hidden; }
    .slide.active { opacity: 1; transform: scale(1); pointer-events: all; }
    .slide > .content { position: relative; z-index: 2; width: 100%; max-width: 1180px; padding: clamp(1.5rem, 3vw, 3rem); }
    .nav-controls { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); display: flex; align-items: center; gap: 16px; z-index: 100; background: rgba(255,255,255,0.75); backdrop-filter: blur(10px); padding: 10px 20px; border-radius: 999px; border: 1px solid rgba(51,89,201,0.14); }
    .nav-btn { width: 40px; height: 40px; border: none; background: rgba(51,89,201,0.08); color: #3359c9; border-radius: 50%; font-size: 1.2rem; cursor: pointer; }
    .slide-dots { display: flex; gap: 8px; }
    .dot { width: 10px; height: 10px; border-radius: 50%; background: rgba(51,89,201,0.18); cursor: pointer; }
    .dot.active { background: #3359c9; transform: scale(1.2); }
    .slide-counter { font-size: .85rem; color: #64748b; min-width: 42px; text-align: center; }
    .reveal { opacity: 0; transform: translateY(20px); }
    .gradient-mesh { position: absolute; inset: 0; z-index: 0; overflow: hidden; pointer-events: none; }
    .blob { position: absolute; border-radius: 50%; filter: blur(80px); animation: float 14s ease-in-out infinite; }
    .blob:nth-child(2){ animation-duration: 18s; }
    .blob:nth-child(3){ animation-duration: 22s; }
    @keyframes float { 0%,100%{ transform: translate(0,0) scale(1);} 50%{ transform: translate(25px,-20px) scale(1.08);} }
    .slide::before { content:''; position:absolute; inset:0; z-index:1; background: radial-gradient(circle at top right, rgba(107,140,255,.08), transparent 30%); }
    .hero-logo { font-size: 72px; font-weight: 800; color: #3359c9; letter-spacing: -0.04em; }
    .card { background: rgba(255,255,255,.9); border: 1px solid rgba(51,89,201,.12); border-radius: 24px; box-shadow: 0 14px 40px rgba(51,89,201,.08); }
    .mini { font-size: 14px; color: #64748b; letter-spacing: .04em; text-transform: uppercase; }
    .big { font-size: 52px; line-height: 1; font-weight: 800; color: #3359c9; }
    .pill { display:inline-block; padding: 8px 14px; border-radius: 999px; background:#e9efff; color:#3359c9; font-weight:700; font-size:14px; }
    .step { position: relative; padding-left: 26px; }
    .step:before { content:''; position:absolute; left:0; top:10px; width:10px; height:10px; border-radius:50%; background:#3359c9; box-shadow: 0 0 0 6px rgba(51,89,201,.12); }
    .step:after { content:''; position:absolute; left:4px; top:24px; bottom:-18px; width:2px; background: linear-gradient(#b7c7ff, transparent); }
    .step:last-child:after { display:none; }
    .bar-track { height: 12px; background: #e8edf8; border-radius: 999px; overflow: hidden; }
    .bar-fill { height: 100%; border-radius: 999px; background: linear-gradient(90deg,#3359c9,#6b8cff); width: 0; }
  </style>
</head>
<body>
<div class="deck">
  <section class="slide active" data-slide="1">
    <div class="gradient-mesh">
      <div class="blob" style="width:360px;height:360px;top:-80px;right:-60px;background:#9fb6ff;opacity:.28"></div>
      <div class="blob" style="width:280px;height:280px;bottom:-60px;left:-60px;background:#6b8cff;opacity:.18"></div>
      <div class="blob" style="width:220px;height:220px;top:42%;left:12%;background:#3359c9;opacity:.12"></div>
    </div>
    <div class="content">
      <div class="hero-logo reveal">ResumeGPT</div>
      <div class="grid grid-cols-[1.2fr_0.8fr] gap-8 items-center mt-8">
        <div>
          <div class="pill reveal">Новый микросервис · 2 человеко-дня на мониторинг</div>
          <h1 class="reveal text-[54px] leading-[1.05] font-extrabold text-[#1f2a44] mt-5">Что внедрить <span class="text-[#3359c9]">сразу</span>, чтобы быстро ловить сбои и деградацию</h1>
          <p class="reveal text-[24px] leading-[1.35] text-[#334155] mt-5">Для сервиса анализа и оптимизации резюме самое важное - не полный observability-stack, а минимальный контур, который отвечает на три вопроса: сервис жив, пользователи получают ответы, качество и задержка не деградируют.</p>
        </div>
        <div class="card p-7 reveal">
          <div class="mini">База приоритета</div>
          <div class="mt-4 space-y-4 text-[22px] leading-[1.3] text-[#1f2a44]">
            <div><b>RED</b> для API: requests, errors, duration</div>
            <div><b>USE</b> для инфраструктуры: utilization, saturation, errors</div>
            <div><b>3 столпа</b>: метрики сначала, логи обязательно, трейсы точечно</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="slide" data-slide="2">
    <div class="gradient-mesh">
      <div class="blob" style="width:320px;height:320px;top:-80px;right:-50px;background:#9fb6ff;opacity:.18"></div>
      <div class="blob" style="width:260px;height:260px;bottom:-40px;left:-30px;background:#6b8cff;opacity:.14"></div>
    </div>
    <div class="content">
      <div class="mini reveal">Слой минимального observability</div>
      <h2 class="reveal text-[42px] font-extrabold leading-tight mt-2">Пирамида мониторинга: снизу вверх</h2>
      <div class="grid grid-cols-[0.92fr_1.08fr] gap-8 mt-8 items-start">
        <div class="card p-7 reveal">
          <div class="space-y-5 text-[22px]">
            <div><span class="pill">1</span> <span class="ml-3 font-bold">Сбор</span> - healthcheck, runtime, контейнер, HTTP middleware</div>
            <div><span class="pill">2</span> <span class="ml-3 font-bold">Метрики</span> - RED по endpoint, очередям, AI-вызовам, БД</div>
            <div><span class="pill">3</span> <span class="ml-3 font-bold">Логи</span> - структурированные логи с request_id, user_id, vacancy_id</div>
            <div><span class="pill">4</span> <span class="ml-3 font-bold">Трейсы</span> - только критический happy path: API → LLM/ML → БД</div>
            <div><span class="pill">5</span> <span class="ml-3 font-bold">Дашборды</span> - 1 overview + 1 service detail</div>
            <div><span class="pill">6</span> <span class="ml-3 font-bold">Алерты</span> - только paging/urgent сигналы, без шума</div>
          </div>
        </div>
        <div class="grid grid-cols-2 gap-5">
          <div class="card p-6 reveal">
            <div class="mini">RED для ResumeGPT</div>
            <div class="mt-4 space-y-4 text-[20px] leading-[1.35]">
              <div><b>Requests</b>: трафик на /analyze, /match</div>
              <div><b>Errors</b>: 5xx, timeout, LLM fail, parse fail</div>
              <div><b>Duration</b>: p50/p95 ответа и этапов пайплайна</div>
            </div>
          </div>
          <div class="card p-6 reveal">
            <div class="mini">USE для infra</div>
            <div class="mt-4 space-y-4 text-[20px] leading-[1.35]">
              <div><b>Utilization</b>: CPU, memory, connection pool</div>
              <div><b>Saturation</b>: очередь, concurrent jobs, rate limit</div>
              <div><b>Errors</b>: restart, OOM, disk/network issue</div>
            </div>
          </div>
          <div class="card p-6 reveal col-span-2">
            <div class="mini">Три столпа в бюджетном варианте</div>
            <div class="mt-4 grid grid-cols-3 gap-4 text-[18px] leading-[1.35]">
              <div><b>Метрики</b><br>дают быстрое понимание «ломается ли сервис»</div>
              <div><b>Логи</b><br>дают контекст «почему именно упало»</div>
              <div><b>Трейсы</b><br>нужны только на самых дорогих и длинных цепочках</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="slide" data-slide="3">
    <div class="gradient-mesh">
      <div class="blob" style="width:340px;height:340px;top:-80px;right:-60px;background:#9fb6ff;opacity:.2"></div>
      <div class="blob" style="width:240px;height:240px;bottom:-50px;left:-40px;background:#3359c9;opacity:.12"></div>
    </div>
    <div class="content">
      <div class="mini reveal">Порядок внедрения</div>
      <h2 class="reveal text-[42px] font-extrabold leading-tight mt-2">План на 2 человеко-дня: максимум пользы, минимум затрат</h2>
      <div class="grid grid-cols-[0.95fr_1.05fr] gap-8 mt-8 items-start">
        <div class="space-y-4">
          <div class="card p-6 reveal step">
            <div class="text-[24px] font-extrabold text-[#3359c9]">День 1, первая половина</div>
            <div class="text-[20px] leading-[1.35] mt-2">Вшить базовые метрики и health endpoints: RPS, 5xx/4xx, p95 latency, длина очереди, успех AI-анализа, время внешних вызовов.</div>
          </div>
          <div class="card p-6 reveal step">
            <div class="text-[24px] font-extrabold text-[#3359c9]">День 1, вторая половина</div>
            <div class="text-[20px] leading-[1.35] mt-2">Сделать structured logs и correlation id. Один дашборд overview: трафик, ошибки, задержка, загрузка, success rate, cost/time на AI.</div>
          </div>
          <div class="card p-6 reveal step">
            <div class="text-[24px] font-extrabold text-[#3359c9]">День 2</div>
            <div class="text-[20px] leading-[1.35] mt-2">Добавить 3–5 алертов и один trace-путь. Прогнать имитацию отказов: timeout LLM, падение БД, рост latency, чтобы проверить, что сигнал реально приходит.</div>
          </div>
        </div>
        <div class="space-y-5">
          <div class="card p-6 reveal">
            <div class="mini">Какие алерты оставить</div>
            <div class="mt-4 space-y-4 text-[20px] leading-[1.35]">
              <div>Ошибка сервиса: 5xx выше порога 5–10 минут</div>
              <div>Деградация UX: p95 latency выросла выше SLO</div>
              <div>Провал бизнес-функции: success rate анализа резюме упал</div>
              <div>Инфраструктурный риск: memory/queue saturation</div>
            </div>
          </div>
          <div class="card p-6 reveal">
            <div class="mini">Что не делать сейчас</div>
            <div class="mt-4 space-y-3 text-[20px] leading-[1.35]">
              <div>Не строить много дашбордов «на будущее»</div>
              <div>Не трассировать все запросы подряд</div>
              <div>Не заводить десятки алертов на каждую метрику</div>
            </div>
          </div>
          <div class="card p-6 reveal">
            <div class="mini">Целевое покрытие</div>
            <div class="grid grid-cols-3 gap-5 mt-4 items-end">
              <div>
                <div class="big">80%</div>
                <div class="text-[16px] text-[#64748b] mt-2">типовых инцидентов закрываются метриками + логами</div>
              </div>
              <div>
                <div class="text-[18px] font-bold mb-2">Метрики</div>
                <div class="bar-track"><div class="bar-fill" data-width="95%"></div></div>
              </div>
              <div>
                <div class="text-[18px] font-bold mb-2">Логи + trace</div>
                <div class="bar-track"><div class="bar-fill" data-width="70%"></div></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</div>
<div class="nav-controls">
  <button class="nav-btn" onclick="changeSlide(-1)" aria-label="Предыдущий слайд">&#8249;</button>
  <div class="slide-dots" id="dots"></div>
  <button class="nav-btn" onclick="changeSlide(1)" aria-label="Следующий слайд">&#8250;</button>
  <span class="slide-counter" id="counter">1 / 3</span>
</div>
<script>
let current = 1;
const total = document.querySelectorAll('.slide').length;
const dotsContainer = document.getElementById('dots');
const counter = document.getElementById('counter');
for (let i = 1; i <= total; i++) { const dot = document.createElement('div'); dot.className = 'dot' + (i === 1 ? ' active' : ''); dot.onclick = () => goToSlide(i); dotsContainer.appendChild(dot); }
function goToSlide(n) {
  document.querySelector('.slide.active')?.classList.remove('active');
  const next = document.querySelector(`.slide[data-slide="${n}"]`);
  if (next) { next.classList.add('active'); animateSlide(next); }
  current = n; updateNav();
}
function changeSlide(dir) { let next = current + dir; if (next < 1) next = total; if (next > total) next = 1; goToSlide(next); }
function updateNav() { document.querySelectorAll('.dot').forEach((d, i) => d.classList.toggle('active', i + 1 === current)); counter.textContent = `${current} / ${total}`; }
document.addEventListener('keydown', (e) => { if (e.key === 'ArrowRight' || e.key === ' ') { e.preventDefault(); changeSlide(1); } if (e.key === 'ArrowLeft') { e.preventDefault(); changeSlide(-1); } });
function animateSlide(slide) {
  slide.querySelectorAll('.reveal').forEach((el, i) => {
    el.style.transition = 'none'; el.style.opacity = '0'; el.style.transform = 'translateY(20px)'; el.offsetHeight;
    const delay = i * 0.08;
    el.style.transition = `opacity .35s ease ${delay}s, transform .35s ease ${delay}s`;
    el.style.opacity = '1'; el.style.transform = 'translateY(0)';
  });
  slide.querySelectorAll('.bar-fill').forEach((bar, i) => {
    bar.style.width = '0%'; bar.style.transition = `width .6s ease ${0.2 + i*0.1}s`; bar.offsetHeight; bar.style.width = bar.dataset.width;
  });
}
animateSlide(document.querySelector('.slide.active'));
</script>
</body>
</html>
