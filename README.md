<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Papan Algoritma — Dasar Algoritma Pemrograman</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Kalam:wght@400;700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
  :root{
    --board-dark:#16302a;
    --board:#1e3f34;
    --board-light:#28503f;
    --board-rail:#5a3a22;
    --chalk:#f2efe3;
    --chalk-dim:#c8d2c4;
    --yellow:#f2c14e;
    --coral:#e8927c;
    --blue:#87b7d6;
    --green-ok:#8fd19e;
    --font-display:'Kalam',cursive;
    --font-body:'Inter',sans-serif;
    --font-mono:'JetBrains Mono',monospace;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--board-dark);
    color:var(--chalk);
    font-family:var(--font-body);
    line-height:1.6;
  }
  ::selection{background:var(--yellow);color:var(--board-dark);}

  /* Chalk texture overlay */
  .chalk-texture{
    position:fixed; inset:0; pointer-events:none; z-index:0; opacity:.05; mix-blend-mode:overlay;
    background-image:radial-gradient(circle at 20% 30%, #fff 0, transparent 1px),
                      radial-gradient(circle at 60% 70%, #fff 0, transparent 1px),
                      radial-gradient(circle at 80% 20%, #fff 0, transparent 1px);
    background-size:140px 140px;
  }

  h1,h2,h3,.hand{font-family:var(--font-display); font-weight:700; letter-spacing:.5px;}

  nav.progress{
    position:sticky; top:0; z-index:50;
    display:flex; gap:0; overflow-x:auto;
    background:var(--board-dark);
    border-bottom:3px solid var(--board-rail);
    padding:0 12px;
  }
  nav.progress a{
    color:var(--chalk-dim); text-decoration:none; font-size:.78rem; font-weight:600;
    padding:12px 14px; white-space:nowrap; border-bottom:3px solid transparent; transition:.2s;
    font-family:var(--font-body);
  }
  nav.progress a:hover, nav.progress a.active{color:var(--yellow); border-bottom-color:var(--yellow);}

  .wrap{max-width:920px; margin:0 auto; padding:0 24px;}

  /* HERO */
  .hero{
    padding:90px 24px 70px; text-align:center; position:relative; z-index:1;
    background:
      radial-gradient(ellipse at 50% 0%, rgba(242,193,78,.08), transparent 60%);
    border-bottom:6px double var(--board-rail);
  }
  .hero .eyebrow{
    display:inline-block; color:var(--yellow); font-family:var(--font-mono); font-size:.8rem;
    letter-spacing:3px; text-transform:uppercase; border:1px dashed var(--yellow); padding:4px 12px; border-radius:20px; margin-bottom:22px;
  }
  .hero h1{font-size:clamp(2.4rem,6vw,4.2rem); margin:0 0 10px; color:var(--chalk);}
  .hero h1 span{color:var(--coral);}
  .hero p{font-size:1.15rem; color:var(--chalk-dim); max-width:560px; margin:0 auto;}
  .hero .subject{margin-top:28px; font-size:.85rem; color:var(--chalk-dim); font-family:var(--font-mono);}

  /* recipe strip - signature element */
  .recipe-strip{display:flex; justify-content:center; gap:26px; margin-top:40px; flex-wrap:wrap;}
  .recipe-strip .step{
    display:flex; flex-direction:column; align-items:center; gap:8px; width:110px;
  }
  .recipe-strip .step .icon{font-size:2rem;}
  .recipe-strip .step span{font-size:.78rem; color:var(--chalk-dim);}
  .recipe-strip .arrow{align-self:center; color:var(--yellow); font-size:1.4rem; margin-top:-30px;}

  section{padding:64px 0; position:relative; z-index:1; border-bottom:1px dashed rgba(242,239,227,.15);}
  .kicker{font-family:var(--font-mono); color:var(--yellow); font-size:.78rem; letter-spacing:2px; text-transform:uppercase;}
  section h2{font-size:2.1rem; margin:6px 0 18px; color:var(--chalk);}
  section > .wrap > p.lead{color:var(--chalk-dim); font-size:1.05rem; max-width:640px;}

  .card{
    background:var(--board); border:1px solid rgba(242,239,227,.12); border-radius:14px;
    padding:22px; box-shadow:0 6px 18px rgba(0,0,0,.25);
  }

  /* Pengertian - contoh interaktif */
  .example-picker{display:flex; gap:12px; flex-wrap:wrap; margin:22px 0;}
  .example-picker button{
    background:var(--board-light); color:var(--chalk); border:1px solid rgba(242,239,227,.2);
    border-radius:10px; padding:12px 18px; font-family:var(--font-body); font-weight:600; cursor:pointer;
    font-size:.92rem; transition:.2s;
  }
  .example-picker button:hover{border-color:var(--yellow); color:var(--yellow);}
  .example-picker button.active{background:var(--yellow); color:var(--board-dark); border-color:var(--yellow);}
  .example-output{min-height:60px;}
  .example-output ol{padding-left:22px;}
  .example-output li{margin-bottom:8px; color:var(--chalk);}
  .example-output .hint{color:var(--chalk-dim); font-size:.85rem;}

  /* Ciri-ciri flip cards */
  .ciri-grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(170px,1fr)); gap:16px; margin-top:24px;}
  .flip-card{perspective:1000px; height:180px; cursor:pointer;}
  .flip-inner{position:relative; width:100%; height:100%; transition:transform .5s; transform-style:preserve-3d;}
  .flip-card.flipped .flip-inner{transform:rotateY(180deg);}
  .flip-front,.flip-back{
    position:absolute; inset:0; border-radius:12px; padding:18px; backface-visibility:hidden;
    display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center; gap:8px;
  }
  .flip-front{background:var(--board-light); border:1px solid rgba(242,239,227,.15);}
  .flip-front .num{font-family:var(--font-display); font-size:1.7rem; color:var(--yellow);}
  .flip-front .term{font-weight:700; font-size:.95rem;}
  .flip-back{background:var(--coral); color:var(--board-dark); transform:rotateY(180deg); font-size:.85rem; font-weight:500;}
  .flip-hint{text-align:center; color:var(--chalk-dim); font-size:.8rem; margin-top:12px; font-family:var(--font-mono);}

  /* Pseudocode */
  .pseudo-box{
    background:#0f2119; border:1px solid rgba(242,239,227,.15); border-radius:12px; padding:20px 24px;
    font-family:var(--font-mono); font-size:.92rem; color:var(--green-ok); overflow-x:auto;
  }
  .pseudo-box .line{padding:2px 0; white-space:pre;}
  .pseudo-box .line.hl{background:rgba(242,193,78,.18); color:var(--yellow); border-radius:4px;}
  .kw{color:var(--blue); font-weight:600;}

  .order-game{margin-top:24px;}
  .pool, .answer{
    min-height:56px; border:2px dashed rgba(242,239,227,.25); border-radius:10px; padding:10px;
    display:flex; flex-wrap:wrap; gap:8px; margin-bottom:14px;
  }
  .answer{border-style:solid; border-color:rgba(242,239,227,.15);}
  .chip{
    background:var(--board-light); border:1px solid rgba(242,239,227,.2); border-radius:8px; padding:8px 12px;
    font-family:var(--font-mono); font-size:.82rem; cursor:pointer; user-select:none;
  }
  .chip:hover{border-color:var(--yellow);}
  .game-controls{display:flex; gap:10px; align-items:center; flex-wrap:wrap;}
  button.action{
    background:var(--yellow); color:var(--board-dark); border:none; border-radius:8px; padding:10px 18px;
    font-weight:700; cursor:pointer; font-family:var(--font-body); font-size:.88rem;
  }
  button.action:hover{filter:brightness(1.08);}
  button.ghost{background:transparent; color:var(--chalk-dim); border:1px solid rgba(242,239,227,.25); border-radius:8px; padding:10px 16px; cursor:pointer; font-family:var(--font-body);}
  .feedback{margin-top:12px; font-weight:600; font-size:.92rem;}
  .feedback.ok{color:var(--green-ok);}
  .feedback.bad{color:var(--coral);}

  /* Flowchart */
  .flow-symbols{display:grid; grid-template-columns:repeat(auto-fit,minmax(150px,1fr)); gap:14px; margin:22px 0;}
  .symbol-card{background:var(--board-light); border-radius:12px; padding:16px; text-align:center; border:1px solid rgba(242,239,227,.12);}
  .symbol-card svg{margin-bottom:8px;}
  .symbol-card .name{font-weight:700; font-size:.85rem;}
  .symbol-card .desc{font-size:.76rem; color:var(--chalk-dim); margin-top:4px;}

  .flow-builder{display:flex; gap:32px; margin-top:28px; flex-wrap:wrap; align-items:flex-start;}
  .flow-canvas{flex:1; min-width:260px; display:flex; justify-content:center;}
  .flow-sync{flex:1; min-width:260px;}
  .flow-controls{display:flex; gap:10px; margin-top:16px; justify-content:center;}
  .step-note{margin-top:14px; text-align:center; color:var(--chalk-dim); font-size:.85rem; min-height:20px;}

  /* Quiz */
  .quiz-card{margin-bottom:18px;}
  .quiz-q{font-weight:600; margin-bottom:12px;}
  .quiz-opts{display:flex; flex-direction:column; gap:8px;}
  .quiz-opt{
    background:var(--board-light); border:1px solid rgba(242,239,227,.15); border-radius:8px; padding:10px 14px;
    cursor:pointer; font-size:.9rem; transition:.15s;
  }
  .quiz-opt:hover{border-color:var(--yellow);}
  .quiz-opt.correct{background:rgba(143,209,158,.25); border-color:var(--green-ok);}
  .quiz-opt.wrong{background:rgba(232,146,124,.25); border-color:var(--coral);}
  .quiz-score{text-align:center; margin-top:20px; font-family:var(--font-display); font-size:1.6rem; color:var(--yellow);}

  .teaser{
    text-align:center; padding:70px 24px; background:radial-gradient(ellipse at 50% 100%, rgba(135,183,214,.1), transparent 60%);
  }
  .teaser h2{margin-bottom:10px;}
  .teaser .pill{display:inline-block; background:var(--blue); color:var(--board-dark); padding:8px 18px; border-radius:20px; font-weight:700; margin-top:16px; font-size:.85rem;}

  footer{text-align:center; padding:30px; color:var(--chalk-dim); font-size:.8rem;}

  @media (prefers-reduced-motion:reduce){*{transition:none !important; animation:none !important;}}
  @media (max-width:600px){
    .hero{padding:60px 18px 50px;}
    section{padding:46px 0;}
  }
</style>
</head>
<body>
<div class="chalk-texture"></div>

<nav class="progress" id="navbar">
  <a href="#pengertian">1. Pengertian</a>
  <a href="#ciri">2. Ciri-ciri</a>
  <a href="#pseudocode">3. Pseudocode</a>
  <a href="#flowchart">4. Flowchart</a>
  <a href="#kuis">5. Kuis</a>
</nav>

<header class="hero">
  <span class="eyebrow">Koding &amp; Kecerdasan Artifisial · Fase E</span>
  <h1>Papan <span>Algoritma</span></h1>
  <p>Sebelum belajar percabangan dan perulangan, kita perlu tahu dulu: apa itu algoritma, ciri-cirinya, dan cara menuliskannya sebagai pseudocode &amp; flowchart.</p>
  <div class="subject">Media Pembelajaran Interaktif — Dasar Algoritma Pemrograman</div>

  <div class="recipe-strip">
    <div class="step"><div class="icon">🥚</div><span>Bahan (Input)</span></div>
    <div class="arrow">→</div>
    <div class="step"><div class="icon">🔥</div><span>Langkah (Proses)</span></div>
    <div class="arrow">→</div>
    <div class="step"><div class="icon">🍳</div><span>Hidangan (Output)</span></div>
  </div>
</header>

<main class="wrap" style="max-width:none;padding:0;">

<!-- 1. PENGERTIAN -->
<section id="pengertian">
  <div class="wrap">
    <div class="kicker">01 — Konsep Dasar</div>
    <h2>Apa itu Algoritma?</h2>
    <p class="lead">Algoritma adalah <strong>urutan langkah-langkah logis dan sistematis</strong> untuk menyelesaikan suatu masalah. Sama seperti resep masakan: ada bahan (input), langkah memasak (proses), dan hidangan jadi (output).</p>

    <div class="card">
      <p style="margin-top:0;color:var(--chalk-dim);font-size:.9rem;">Klik salah satu aktivitas sehari-hari untuk melihat "algoritma"-nya:</p>
      <div class="example-picker" id="examplePicker">
        <button data-ex="mie">Membuat Mi Instan</button>
        <button data-ex="baterai">Mengisi Baterai HP</button>
        <button data-ex="komputer">Menyalakan Komputer</button>
      </div>
      <div class="example-output card" id="exampleOutput" style="background:var(--board-dark);">
        <p class="hint">👆 Pilih salah satu tombol di atas untuk memulai.</p>
      </div>
    </div>
  </div>
</section>

<!-- 2. CIRI-CIRI -->
<section id="ciri">
  <div class="wrap">
    <div class="kicker">02 — Karakteristik</div>
    <h2>Ciri-ciri Algoritma</h2>
    <p class="lead">Tidak semua urutan langkah bisa disebut algoritma. Menurut Donald Knuth, algoritma yang baik punya 5 ciri berikut. Klik tiap kartu untuk membuka penjelasannya.</p>

    <div class="ciri-grid" id="ciriGrid"></div>
    <div class="flip-hint">klik kartu untuk membalik ↻</div>
  </div>
</section>

<!-- 3. PSEUDOCODE -->
<section id="pseudocode">
  <div class="wrap">
    <div class="kicker">03 — Penyajian Algoritma</div>
    <h2>Pseudocode</h2>
    <p class="lead">Pseudocode adalah cara menuliskan algoritma menggunakan bahasa yang mirip bahasa pemrograman, tapi tetap mudah dibaca manusia — belum terikat aturan ketat suatu bahasa pemrograman tertentu.</p>

    <p style="color:var(--chalk-dim);font-size:.9rem;">Contoh kasus yang akan kita pakai di seluruh bagian ini: <strong style="color:var(--chalk);">menghitung luas persegi panjang.</strong></p>

    <div class="pseudo-box" id="pseudoBox">
      <div class="line"><span class="kw">MULAI</span></div>
      <div class="line">  <span class="kw">DEKLARASI</span> panjang, lebar, luas : integer</div>
      <div class="line">  <span class="kw">INPUT</span> panjang</div>
      <div class="line">  <span class="kw">INPUT</span> lebar</div>
      <div class="line">  luas ← panjang * lebar</div>
      <div class="line">  <span class="kw">OUTPUT</span> luas</div>
      <div class="line"><span class="kw">SELESAI</span></div>
    </div>

    <div class="card order-game">
      <h3 style="margin-top:0;font-family:var(--font-body);font-size:1.05rem;">🎯 Latihan: Susun Pseudocode</h3>
      <p style="color:var(--chalk-dim);font-size:.88rem;">Langkah-langkah di bawah ini teracak. Klik langkah sesuai urutan yang benar untuk memindahkannya ke kotak jawaban.</p>
      <div class="pool" id="pool"></div>
      <div class="answer" id="answer"></div>
      <div class="game-controls">
        <button class="action" id="checkOrder">Periksa Urutan</button>
        <button class="ghost" id="resetOrder">Ulangi</button>
      </div>
      <div class="feedback" id="orderFeedback"></div>
    </div>
  </div>
</section>

<!-- 4. FLOWCHART -->
<section id="flowchart">
  <div class="wrap">
    <div class="kicker">04 — Penyajian Algoritma</div>
    <h2>Flowchart (Diagram Alir)</h2>
    <p class="lead">Flowchart menyajikan algoritma dalam bentuk simbol-simbol grafis yang saling terhubung oleh garis alir, sehingga urutan langkah lebih mudah divisualisasikan.</p>

    <div class="flow-symbols" id="symbolGrid"></div>

    <h3 style="font-family:var(--font-body);font-size:1.05rem;margin-top:34px;">Bangun Flowchart Selangkah demi Selangkah</h3>
    <p style="color:var(--chalk-dim);font-size:.88rem;">Kasus yang sama: menghitung luas persegi panjang. Tekan "Lanjut" dan perhatikan pseudocode di sebelah kanan ikut tersorot.</p>

    <div class="flow-builder">
      <div class="flow-canvas">
        <svg id="flowSvg" width="260" height="420" viewBox="0 0 260 420"></svg>
      </div>
      <div class="flow-sync">
        <div class="pseudo-box" id="syncPseudo" style="font-size:.85rem;"></div>
        <div class="flow-controls">
          <button class="ghost" id="flowPrev">← Kembali</button>
          <button class="action" id="flowNext">Lanjut →</button>
        </div>
        <div class="step-note" id="flowNote"></div>
      </div>
    </div>
  </div>
</section>

<!-- 5. KUIS -->
<section id="kuis">
  <div class="wrap">
    <div class="kicker">05 — Uji Pemahaman</div>
    <h2>Kuis Singkat</h2>
    <p class="lead">Jawab 5 pertanyaan berikut untuk mengecek pemahamanmu sebelum lanjut ke materi percabangan &amp; perulangan.</p>
    <div id="quizContainer"></div>
    <div class="quiz-score" id="quizScore"></div>
  </div>
</section>

</main>

<div class="teaser">
  <div class="kicker">Selanjutnya</div>
  <h2>Struktur Kontrol: Percabangan &amp; Perulangan</h2>
  <p style="color:var(--chalk-dim);max-width:520px;margin:0 auto;">Setelah memahami dasar algoritma, pseudocode, dan flowchart, kita akan belajar bagaimana algoritma bisa "mengambil keputusan" (percabangan) dan "mengulang langkah" (perulangan) — dua simbol yang tadi baru kita kenalkan namanya saja.</p>
  <div class="pill">🚦 Percabangan → ⟲ Perulangan</div>
</div>

<footer>Media Pembelajaran Interaktif · Koding dan Kecerdasan Artifisial · Fase E, Kelas X</footer>

<script>
/* ---------- Nav active state ---------- */
const navLinks = document.querySelectorAll('nav.progress a');
const sections = document.querySelectorAll('main section');
window.addEventListener('scroll', () => {
  let cur = '';
  sections.forEach(s => { if (window.scrollY >= s.offsetTop - 90) cur = s.id; });
  navLinks.forEach(a => a.classList.toggle('active', a.getAttribute('href') === '#' + cur));
});

/* ---------- 1. Pengertian: contoh interaktif ---------- */
const examples = {
  mie: {
    title: '🍜 Algoritma Membuat Mi Instan',
    steps: [
      'Didihkan air dalam panci',
      'Masukkan mi ke dalam air mendidih',
      'Masak selama 3 menit sambil diaduk',
      'Tiriskan mi',
      'Campurkan bumbu, minyak, dan mi',
      'Aduk rata, mi siap disajikan'
    ]
  },
  baterai: {
    title: '🔋 Algoritma Mengisi Baterai HP',
    steps: [
      'Ambil kabel charger',
      'Colokkan ujung kabel ke HP',
      'Colokkan ujung lain ke stop kontak',
      'Tunggu hingga indikator baterai penuh',
      'Cabut charger dari HP'
    ]
  },
  komputer: {
    title: '💻 Algoritma Menyalakan Komputer',
    steps: [
      'Pastikan kabel daya terhubung',
      'Tekan tombol power pada CPU',
      'Tunggu proses booting selesai',
      'Masukkan username dan password',
      'Komputer siap digunakan'
    ]
  }
};
const exampleOutput = document.getElementById('exampleOutput');
document.getElementById('examplePicker').addEventListener('click', (e) => {
  const btn = e.target.closest('button[data-ex]');
  if (!btn) return;
  document.querySelectorAll('#examplePicker button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  const data = examples[btn.dataset.ex];
  exampleOutput.innerHTML = `<strong>${data.title}</strong><ol>${data.steps.map(s => `<li>${s}</li>`).join('')}</ol><p class="hint">Perhatikan: langkahnya berurutan, jelas, dan pasti berhenti setelah selesai — itulah ciri sebuah algoritma.</p>`;
});

/* ---------- 2. Ciri-ciri: flip cards ---------- */
const ciriData = [
  { term: 'Finiteness', id: 'Berhingga', desc: 'Algoritma harus berhenti setelah menjalankan sejumlah langkah tertentu, tidak boleh berjalan tanpa akhir.' },
  { term: 'Definiteness', id: 'Pasti / Tidak Ambigu', desc: 'Setiap langkah harus didefinisikan dengan tepat dan tidak menimbulkan makna ganda.' },
  { term: 'Input', id: 'Masukan', desc: 'Algoritma memiliki nol atau lebih masukan (data) yang diberikan sebelum atau selama proses.' },
  { term: 'Output', id: 'Keluaran', desc: 'Algoritma menghasilkan satu atau lebih keluaran yang berhubungan langsung dengan masukan.' },
  { term: 'Effectiveness', id: 'Efektif', desc: 'Setiap langkah harus sederhana, wajar dikerjakan, dan dalam waktu yang masuk akal.' },
];
const ciriGrid = document.getElementById('ciriGrid');
ciriData.forEach((c, i) => {
  const card = document.createElement('div');
  card.className = 'flip-card';
  card.innerHTML = `
    <div class="flip-inner">
      <div class="flip-front">
        <div class="num">0${i+1}</div>
        <div class="term">${c.term}</div>
        <div style="font-size:.75rem;color:var(--chalk-dim);">${c.id}</div>
      </div>
      <div class="flip-back">${c.desc}</div>
    </div>`;
  card.addEventListener('click', () => card.classList.toggle('flipped'));
  ciriGrid.appendChild(card);
});

/* ---------- 3. Pseudocode ordering game ---------- */
const correctSteps = [
  'MULAI',
  'DEKLARASI panjang, lebar, luas : integer',
  'INPUT panjang',
  'INPUT lebar',
  'luas ← panjang * lebar',
  'OUTPUT luas',
  'SELESAI'
];
let shuffled = [];
const pool = document.getElementById('pool');
const answer = document.getElementById('answer');
const orderFeedback = document.getElementById('orderFeedback');

function shuffleArr(arr){
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--){
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
function renderGame(){
  pool.innerHTML = '';
  answer.innerHTML = '';
  orderFeedback.textContent = '';
  orderFeedback.className = 'feedback';
  shuffled.forEach(text => {
    const chip = document.createElement('div');
    chip.className = 'chip';
    chip.textContent = text;
    chip.addEventListener('click', () => {
      answer.appendChild(chip);
      chip.classList.add('placed');
      chip.onclick = () => { pool.appendChild(chip); };
    });
    pool.appendChild(chip);
  });
}
function initGame(){
  shuffled = shuffleArr(correctSteps);
  renderGame();
}
document.getElementById('checkOrder').addEventListener('click', () => {
  const chips = [...answer.children].map(c => c.textContent);
  if (chips.length !== correctSteps.length){
    orderFeedback.textContent = 'Masih ada langkah yang belum dipindahkan ke kotak jawaban.';
    orderFeedback.className = 'feedback bad';
    return;
  }
  const isCorrect = chips.every((t, i) => t === correctSteps[i]);
  orderFeedback.textContent = isCorrect ? '✅ Benar! Urutan pseudocode-mu sudah tepat.' : '❌ Urutan belum tepat, coba periksa lagi mana yang harus dijalankan lebih dulu.';
  orderFeedback.className = 'feedback ' + (isCorrect ? 'ok' : 'bad');
});
document.getElementById('resetOrder').addEventListener('click', initGame);
initGame();

/* ---------- 4. Flowchart symbols ---------- */
const symbolGrid = document.getElementById('symbolGrid');
const symbolSvgs = [
  { name:'Terminator', desc:'Mulai / Selesai', svg:`<svg width="70" height="46" viewBox="0 0 70 46"><rect x="2" y="2" width="66" height="42" rx="21" fill="none" stroke="#f2c14e" stroke-width="2.5"/></svg>` },
  { name:'Input/Output', desc:'Masukan / Keluaran data', svg:`<svg width="70" height="46" viewBox="0 0 70 46"><polygon points="14,2 68,2 56,44 2,44" fill="none" stroke="#87b7d6" stroke-width="2.5"/></svg>` },
  { name:'Proses', desc:'Langkah pengolahan data', svg:`<svg width="70" height="46" viewBox="0 0 70 46"><rect x="2" y="2" width="66" height="42" fill="none" stroke="#8fd19e" stroke-width="2.5"/></svg>` },
  { name:'Keputusan', desc:'Percabangan (dipelajari berikutnya)', svg:`<svg width="70" height="46" viewBox="0 0 70 46"><polygon points="35,2 68,23 35,44 2,23" fill="none" stroke="#e8927c" stroke-width="2.5"/></svg>` },
  { name:'Garis Alir', desc:'Penghubung & arah proses', svg:`<svg width="70" height="46" viewBox="0 0 70 46"><line x1="4" y1="23" x2="58" y2="23" stroke="#f2efe3" stroke-width="2.5"/><polygon points="58,16 68,23 58,30" fill="#f2efe3"/></svg>` },
];
symbolSvgs.forEach(s => {
  const el = document.createElement('div');
  el.className = 'symbol-card';
  el.innerHTML = `${s.svg}<div class="name">${s.name}</div><div class="desc">${s.desc}</div>`;
  symbolGrid.appendChild(el);
});

/* ---------- 4b. Flowchart step builder ---------- */
const flowSteps = [
  { shape:'term', label:'MULAI', pseudoLine:0 },
  { shape:'io', label:'INPUT panjang', pseudoLine:2 },
  { shape:'io', label:'INPUT lebar', pseudoLine:3 },
  { shape:'proc', label:'luas ← p × l', pseudoLine:4 },
  { shape:'io', label:'OUTPUT luas', pseudoLine:5 },
  { shape:'term', label:'SELESAI', pseudoLine:6 },
];
let flowIdx = 0;
const flowSvg = document.getElementById('flowSvg');
const flowNote = document.getElementById('flowNote');
const syncPseudo = document.getElementById('syncPseudo');

function shapeSvg(step, y){
  const cx = 130;
  if (step.shape === 'term'){
    return `<rect x="${cx-70}" y="${y}" width="140" height="42" rx="21" fill="#1e3f34" stroke="#f2c14e" stroke-width="2.5"/><text x="${cx}" y="${y+26}" text-anchor="middle" fill="#f2efe3" font-family="JetBrains Mono" font-size="13">${step.label}</text>`;
  }
  if (step.shape === 'io'){
    return `<polygon points="${cx-80},${y} ${cx+80},${y} ${cx+65},${y+42} ${cx-65},${y+42}" fill="#1e3f34" stroke="#87b7d6" stroke-width="2.5"/><text x="${cx}" y="${y+26}" text-anchor="middle" fill="#f2efe3" font-family="JetBrains Mono" font-size="12">${step.label}</text>`;
  }
  return `<rect x="${cx-70}" y="${y}" width="140" height="42" fill="#1e3f34" stroke="#8fd19e" stroke-width="2.5"/><text x="${cx}" y="${y+26}" text-anchor="middle" fill="#f2efe3" font-family="JetBrains Mono" font-size="12">${step.label}</text>`;
}

function renderFlow(){
  let svgContent = '';
  const gap = 66;
  for (let i = 0; i <= flowIdx; i++){
    const y = 10 + i * gap;
    svgContent += shapeSvg(flowSteps[i], y);
    if (i > 0){
      const prevY = 10 + (i-1) * gap + 42;
      svgContent += `<line x1="130" y1="${prevY}" x2="130" y2="${y}" stroke="#f2efe3" stroke-width="2"/><polygon points="126,${y-8} 134,${y-8} 130,${y}" fill="#f2efe3"/>`;
    }
  }
  flowSvg.innerHTML = svgContent;
  flowSvg.setAttribute('height', 10 + (flowIdx+1) * gap + 20);

  // sync pseudocode highlight
  const lines = document.querySelectorAll('#pseudoBox .line');
  const targetLine = flowSteps[flowIdx].pseudoLine;
  syncPseudo.innerHTML = correctSteps.map((s, i) => `<div class="line${i === targetLine ? ' hl' : ''}">${'  '.repeat(i>0&&i<6?1:0)}${s}</div>`).join('');

  flowNote.textContent = `Langkah ${flowIdx+1} dari ${flowSteps.length}: ${flowSteps[flowIdx].label}`;
  document.getElementById('flowPrev').disabled = flowIdx === 0;
  document.getElementById('flowNext').disabled = flowIdx === flowSteps.length - 1;
}
document.getElementById('flowNext').addEventListener('click', () => { if (flowIdx < flowSteps.length-1){ flowIdx++; renderFlow(); }});
document.getElementById('flowPrev').addEventListener('click', () => { if (flowIdx > 0){ flowIdx--; renderFlow(); }});
renderFlow();

/* ---------- 5. Quiz ---------- */
const quizData = [
  { q:'Apa yang dimaksud dengan algoritma?', opts:['Nama bahasa pemrograman','Urutan langkah logis dan sistematis untuk memecahkan masalah','Simbol pada flowchart','Perangkat keras komputer'], correct:1 },
  { q:'Ciri algoritma yang menyatakan bahwa proses harus berhenti setelah sejumlah langkah disebut...', opts:['Input','Definiteness','Finiteness','Output'], correct:2 },
  { q:'Pseudocode paling tepat digambarkan sebagai...', opts:['Bahasa pemrograman resmi seperti Python','Tulisan mirip bahasa pemrograman yang mudah dibaca manusia','Kumpulan simbol grafis','Kode mesin biner'], correct:1 },
  { q:'Simbol jajar genjang (parallelogram) pada flowchart digunakan untuk...', opts:['Mulai/Selesai','Proses pengolahan data','Input/Output','Percabangan'], correct:2 },
  { q:'Simbol diamond/belah ketupat pada flowchart akan dipelajari lebih lanjut pada materi...', opts:['Deklarasi variabel','Percabangan (decision)','Output program','Penulisan komentar'], correct:1 },
];
let quizAnswers = new Array(quizData.length).fill(null);
const quizContainer = document.getElementById('quizContainer');
quizData.forEach((item, qi) => {
  const card = document.createElement('div');
  card.className = 'card quiz-card';
  card.innerHTML = `<div class="quiz-q">${qi+1}. ${item.q}</div>
    <div class="quiz-opts">${item.opts.map((o,oi) => `<div class="quiz-opt" data-q="${qi}" data-o="${oi}">${o}</div>`).join('')}</div>`;
  quizContainer.appendChild(card);
});
quizContainer.addEventListener('click', (e) => {
  const opt = e.target.closest('.quiz-opt');
  if (!opt) return;
  const qi = +opt.dataset.q, oi = +opt.dataset.o;
  if (quizAnswers[qi] !== null) return; // already answered
  quizAnswers[qi] = oi;
  const siblings = opt.parentElement.children;
  [...siblings].forEach((s, i) => {
    if (i === quizData[qi].correct) s.classList.add('correct');
    else if (i === oi) s.classList.add('wrong');
  });
  if (quizAnswers.every(a => a !== null)){
    const score = quizAnswers.filter((a,i) => a === quizData[i].correct).length;
    document.getElementById('quizScore').textContent = `Skor kamu: ${score} / ${quizData.length} 🎓`;
  }
});
</script>
</body>
</html>
