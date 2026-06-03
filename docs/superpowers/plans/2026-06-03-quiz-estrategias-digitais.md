# Quiz Estratégias Digitais — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar `diagnostico-estrategiasdigitais/index.html` — quiz de diagnóstico digital (marketing + IA) para eventos presenciais, com captura de lead completa em Google Sheets e CTA para WhatsApp, usando a identidade visual da Estratégias Digitais.

**Architecture:** HTML/CSS/JS puro em arquivo único dentro da pasta `diagnostico-estrategiasdigitais/`. Integração com Google Sheets via Apps Script (GET + URLSearchParams). Deploy como projeto Vercel separado apontando para este subdiretório.

**Tech Stack:** HTML5, CSS3, JavaScript vanilla, Google Fonts (Inter), Google Apps Script

---

## File Map

| Arquivo | Ação | Responsabilidade |
|---------|------|-----------------|
| `diagnostico-estrategiasdigitais/index.html` | Criar | Quiz completo — todo CSS + JS inline |
| `diagnostico-estrategiasdigitais/logo-ed.png` | Usuário salva | Logo da Estratégias Digitais |
| Google Apps Script (externo) | Criar | Endpoint que grava na planilha |

---

## Task 1: Google Apps Script — Endpoint da Planilha

**Spreadsheet ID:** `1D1UaVxOnUhvfkEeneZQmtbiL4IKrNrku`

- [ ] **Step 1: Abrir o Apps Script**

  Acesse: https://script.google.com → clique em "Novo projeto"

- [ ] **Step 2: Cole o código abaixo e salve**

```javascript
function doGet(e) {
  const sheet = SpreadsheetApp
    .openById('1D1UaVxOnUhvfkEeneZQmtbiL4IKrNrku')
    .getActiveSheet();

  const p = e.parameter;

  sheet.appendRow([
    new Date().toLocaleString('pt-BR', { timeZone: 'America/Sao_Paulo' }),
    p.nome           || '',
    p.empresa        || '',
    p.whatsapp       || '',
    p.pontuacao      || '',
    p.faixa          || '',
    p.dimensao_fraca || '',
    p.p1 || '', // Presença Online
    p.p2 || '', // Conteúdo Digital
    p.p3 || '', // Tráfego
    p.p4 || '', // Captação de Leads
    p.p5 || '', // Processo de Vendas
    p.p6 || '', // Copy/Mensagem
    p.p7 || '', // Uso de IA
    p.p8 || ''  // Automação
  ]);

  return ContentService.createTextOutput('ok');
}
```

- [ ] **Step 3: Configurar cabeçalhos na planilha**

  Abra a planilha e adicione na linha 1:
  `Data/Hora | Nome | Empresa | WhatsApp | Pontuação | Faixa | Dimensão Mais Fraca | P1-Presença | P2-Conteúdo | P3-Tráfego | P4-Leads | P5-Vendas | P6-Copy | P7-IA | P8-Automação`

- [ ] **Step 4: Publicar como Web App**

  Menu: Implantar → Nova implantação → Tipo: App da Web
  - Executar como: **Eu (meu e-mail)**
  - Quem tem acesso: **Qualquer pessoa**
  - Clique em Implantar → copie a URL gerada

- [ ] **Step 5: Guardar a URL**

  Salve a URL no formato:
  `https://script.google.com/macros/s/XXXXXXXX/exec`

  Você vai colá-la na constante `PLANILHA_URL` do HTML (Task 2).

---

## Task 2: Scaffold — Estrutura Base do HTML

**Files:**
- Create: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Salvar a logo**

  Salve o arquivo PNG da logo como:
  `c:\Users\walte\raio-x-360\diagnostico-estrategiasdigitais\logo-ed.png`

- [ ] **Step 2: Criar o arquivo index.html com skeleton completo**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Diagnóstico Digital — Estratégias Digitais</title>
  <meta name="description" content="Descubra o nível de maturidade digital da sua empresa em 3 minutos.">
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>📊</text></svg>">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
  <style>
    /* === VARIÁVEIS === */
    :root {
      --primary:      #00AEEF;
      --primary-dark: #0077B6;
      --primary-hover:#0099D4;
      --bg:           #FFFFFF;
      --bg-alt:       #F0FAFF;
      --text:         #3D4C6B;
      --text-muted:   #64748B;
      --border:       #E2E8F0;
      --shadow:       0 2px 16px rgba(0,174,239,0.10);
      --shadow-lg:    0 4px 32px rgba(0,174,239,0.16);
      --radius:       14px;
      --radius-sm:    8px;
      --red:          #EF4444;
      --yellow:       #F59E0B;
      --green:        #10B981;
      --blue:         #00AEEF;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Inter', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
    }

    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 32px 20px 60px;
    }

    /* === SCREENS === */
    .screen { display: none; }
    .screen.active { display: block; animation: fadeUp 0.35s ease forwards; }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(16px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* === LOGO === */
    .logo-wrap {
      text-align: center;
      margin-bottom: 28px;
    }
    .logo-wrap img {
      height: 56px;
      width: auto;
      object-fit: contain;
    }
    .logo-fallback {
      font-size: 1.1rem;
      font-weight: 800;
      color: var(--primary);
      letter-spacing: -0.5px;
    }

    /* === BOTÕES === */
    .btn-primary {
      display: block;
      width: 100%;
      background: linear-gradient(135deg, var(--primary), var(--primary-dark));
      color: #fff;
      font-family: 'Inter', sans-serif;
      font-weight: 700;
      font-size: 1.05rem;
      padding: 16px 24px;
      border: none;
      border-radius: var(--radius-sm);
      cursor: pointer;
      text-align: center;
      transition: opacity 0.2s, transform 0.15s;
      box-shadow: 0 4px 16px rgba(0,174,239,0.30);
    }
    .btn-primary:hover { opacity: 0.92; transform: translateY(-1px); }
    .btn-primary:active { transform: translateY(0); }

    /* === INPUTS === */
    .field-input {
      width: 100%;
      padding: 14px 16px;
      border: 2px solid var(--border);
      border-radius: var(--radius-sm);
      font-family: 'Inter', sans-serif;
      font-size: 1rem;
      color: var(--text);
      background: var(--bg);
      transition: border-color 0.2s;
      outline: none;
      margin-bottom: 14px;
    }
    .field-input:focus { border-color: var(--primary); }
    .field-input::placeholder { color: var(--text-muted); }

    /* === CARD === */
    .card {
      background: var(--bg);
      border: 1.5px solid var(--border);
      border-radius: var(--radius);
      padding: 24px;
      box-shadow: var(--shadow);
    }

    /* === PROGRESS === */
    .progress-wrap {
      margin-bottom: 24px;
    }
    .progress-label {
      display: flex;
      justify-content: space-between;
      font-size: 0.78rem;
      color: var(--text-muted);
      margin-bottom: 6px;
    }
    .progress-bar-bg {
      height: 6px;
      background: var(--border);
      border-radius: 99px;
      overflow: hidden;
    }
    .progress-bar-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--primary), var(--primary-dark));
      border-radius: 99px;
      transition: width 0.4s ease;
    }

    /* === SCALE BUTTONS === */
    .scale-wrap {
      display: flex;
      gap: 10px;
      margin-top: 20px;
      justify-content: center;
    }
    .scale-btn {
      flex: 1;
      max-width: 56px;
      aspect-ratio: 1;
      background: var(--bg-alt);
      border: 2px solid var(--border);
      border-radius: 10px;
      font-size: 1.1rem;
      font-weight: 700;
      color: var(--text-muted);
      cursor: pointer;
      transition: all 0.18s;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .scale-btn:hover { border-color: var(--primary); color: var(--primary); }
    .scale-btn.selected {
      background: var(--primary);
      border-color: var(--primary);
      color: #fff;
      transform: scale(1.08);
      box-shadow: 0 4px 12px rgba(0,174,239,0.35);
    }
    .scale-labels {
      display: flex;
      justify-content: space-between;
      font-size: 0.72rem;
      color: var(--text-muted);
      margin-top: 8px;
      padding: 0 4px;
    }

    /* === DIMENSION BADGE === */
    .dim-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: var(--bg-alt);
      border: 1.5px solid var(--primary);
      border-radius: 99px;
      padding: 4px 14px;
      font-size: 0.82rem;
      font-weight: 600;
      color: var(--primary);
      margin-bottom: 18px;
    }

    /* === RESULT BARS === */
    .dim-bar-wrap { margin-bottom: 12px; }
    .dim-bar-header {
      display: flex;
      justify-content: space-between;
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--text);
      margin-bottom: 5px;
    }
    .dim-bar-bg {
      height: 10px;
      background: var(--border);
      border-radius: 99px;
      overflow: hidden;
    }
    .dim-bar-fill {
      height: 100%;
      border-radius: 99px;
      transition: width 1s ease;
    }

    /* === SCORE CIRCLE === */
    .score-circle {
      width: 120px;
      height: 120px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--primary), var(--primary-dark));
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin: 0 auto 20px;
      box-shadow: 0 4px 24px rgba(0,174,239,0.35);
    }
    .score-circle .score-num {
      font-size: 2.2rem;
      font-weight: 800;
      color: #fff;
      line-height: 1;
    }
    .score-circle .score-label {
      font-size: 0.7rem;
      color: rgba(255,255,255,0.85);
      margin-top: 2px;
    }

    /* === LOADING === */
    .spinner {
      width: 48px; height: 48px;
      border: 4px solid var(--border);
      border-top-color: var(--primary);
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
      margin: 0 auto 20px;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* === MICRO-COPY === */
    .micro { font-size: 0.78rem; color: var(--text-muted); text-align: center; margin-top: 10px; }

    /* === SEPARADOR === */
    .divider {
      border: none;
      border-top: 1.5px solid var(--border);
      margin: 24px 0;
    }

    @media (max-width: 480px) {
      .container { padding: 24px 16px 60px; }
      .scale-btn { font-size: 1rem; }
    }
  </style>
</head>
<body>

  <!-- TELA 1: INTRO -->
  <div id="screen-intro" class="screen active">
    <div class="container">
      <div class="logo-wrap">
        <img src="logo-ed.png" alt="Estratégias Digitais"
             onerror="this.style.display='none'; document.getElementById('logo-fallback').style.display='block'">
        <div id="logo-fallback" class="logo-fallback" style="display:none">Estratégias Digitais</div>
      </div>
      <!-- conteúdo da intro — Task 3 -->
    </div>
  </div>

  <!-- TELA 2: CAPTURA (Nome + Empresa) -->
  <div id="screen-capture" class="screen">
    <div class="container">
      <div class="logo-wrap">
        <img src="logo-ed.png" alt="Estratégias Digitais"
             onerror="this.style.display='none'">
      </div>
      <!-- conteúdo da captura — Task 3 -->
    </div>
  </div>

  <!-- TELA 3: QUIZ -->
  <div id="screen-quiz" class="screen">
    <div class="container" id="quiz-container">
      <!-- gerado por JS — Task 4 -->
    </div>
  </div>

  <!-- TELA 4: PROCESSANDO -->
  <div id="screen-processing" class="screen">
    <div class="container">
      <!-- conteúdo — Task 5 -->
    </div>
  </div>

  <!-- TELA 5: RESULTADO -->
  <div id="screen-result" class="screen">
    <div class="container" id="result-container">
      <!-- gerado por JS — Task 6 -->
    </div>
  </div>

  <script>
    // === STATE ===
    let userName    = '';
    let userEmpresa = '';
    let userWpp     = '';
    const answers   = []; // 8 posições, índice 0-7
    let currentQ    = 0;  // pergunta atual (0-7)

    const PLANILHA_URL = 'COLE_A_URL_DO_APPS_SCRIPT_AQUI';

    const DIMENSIONS = [
      { name: 'Presença Digital',     icon: '🌐', questions: [
          'Como você avalia a presença online da sua empresa? (site, redes sociais, Google)',
          'Seu conteúdo digital gera engajamento e atrai potenciais clientes de forma consistente?'
      ]},
      { name: 'Atração de Clientes',  icon: '🎯', questions: [
          'Você tem uma estratégia clara e ativa de tráfego pago ou orgânico?',
          'Seu funil de captação de leads gera novos contatos de forma previsível todo mês?'
      ]},
      { name: 'Conversão e Vendas',   icon: '💰', questions: [
          'Seu processo de vendas está documentado e é seguido pela equipe?',
          'Suas mensagens de vendas (copy) comunicam claramente o valor que você entrega?'
      ]},
      { name: 'Inteligência Artificial', icon: '🤖', questions: [
          'Você utiliza ferramentas de IA no dia a dia da sua empresa?',
          'Seus processos internos têm algum nível de automação implementado?'
      ]}
    ];

    const MAX_SCORE = 40; // 8 × 5

    const FAIXAS = [
      { min: 0,  max: 16, emoji: '🔴', title: 'Negócio Analógico',  color: '#EF4444', bg: '#FEF2F2' },
      { min: 17, max: 26, emoji: '🟡', title: 'Em Digitalização',   color: '#F59E0B', bg: '#FFFBEB' },
      { min: 27, max: 34, emoji: '🟢', title: 'Tração Digital',     color: '#10B981', bg: '#ECFDF5' },
      { min: 35, max: 40, emoji: '🔵', title: 'Máquina Digital',    color: '#00AEEF', bg: '#F0FAFF' }
    ];

    // helpers — Task 4+
    function showScreen(id) {
      document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }
  </script>

</body>
</html>
```

- [ ] **Step 3: Abrir no navegador e verificar**

  Abra `diagnostico-estrategiasdigitais/index.html` no browser.
  Esperado: página branca com logo (ou fallback texto) visível, sem erros no console.

- [ ] **Step 4: Commit**

```bash
git add diagnostico-estrategiasdigitais/
git commit -m "feat: scaffold quiz-ed — estrutura base, CSS, variáveis e state JS"
```

---

## Task 3: Tela de Intro + Tela de Captura (Nome + Empresa)

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Substituir o comentário dentro de `#screen-intro`**

  Localizar: `<!-- conteúdo da intro — Task 3 -->`  
  Substituir por:

```html
      <div style="text-align:center; margin-bottom:32px;">
        <div style="display:inline-flex;align-items:center;gap:8px;background:var(--bg-alt);border:1.5px solid var(--primary);border-radius:99px;padding:5px 16px;font-size:0.82rem;font-weight:600;color:var(--primary);margin-bottom:20px;">
          📊 Diagnóstico Digital Gratuito
        </div>
        <h1 style="font-size:clamp(1.6rem,5vw,2.2rem);font-weight:800;color:var(--text);line-height:1.25;margin-bottom:14px;">
          Qual é o nível digital<br>da sua empresa?
        </h1>
        <p style="font-size:1rem;color:var(--text-muted);line-height:1.65;max-width:440px;margin:0 auto 24px;">
          Responda 8 perguntas e descubra onde estão os gargalos que impedem seu negócio de crescer com marketing e IA.
        </p>
        <div style="display:flex;justify-content:center;gap:20px;margin-bottom:28px;">
          <div style="text-align:center;">
            <div style="font-size:1.4rem;font-weight:800;color:var(--primary);">8</div>
            <div style="font-size:0.72rem;color:var(--text-muted);">perguntas</div>
          </div>
          <div style="width:1px;background:var(--border);"></div>
          <div style="text-align:center;">
            <div style="font-size:1.4rem;font-weight:800;color:var(--primary);">3 min</div>
            <div style="font-size:0.72rem;color:var(--text-muted);">duração</div>
          </div>
          <div style="width:1px;background:var(--border);"></div>
          <div style="text-align:center;">
            <div style="font-size:1.4rem;font-weight:800;color:var(--primary);">100%</div>
            <div style="font-size:0.72rem;color:var(--text-muted);">gratuito</div>
          </div>
        </div>
        <button class="btn-primary" onclick="showScreen('screen-capture')">
          Iniciar Diagnóstico →
        </button>
        <p class="micro" style="margin-top:14px;">Resultado imediato e personalizado para o seu negócio</p>
      </div>
```

- [ ] **Step 2: Substituir o comentário dentro de `#screen-capture`**

  Localizar: `<!-- conteúdo da captura — Task 3 -->`  
  Substituir por:

```html
      <div style="text-align:center;margin-bottom:28px;">
        <h2 style="font-size:1.4rem;font-weight:800;color:var(--text);margin-bottom:8px;">Antes de começar...</h2>
        <p style="color:var(--text-muted);font-size:0.95rem;">Preencha para personalizar seu resultado</p>
      </div>
      <div class="card">
        <label style="font-size:0.85rem;font-weight:600;color:var(--text);display:block;margin-bottom:6px;">Seu nome</label>
        <input id="input-nome" type="text" class="field-input"
               placeholder="Como posso te chamar?"
               onkeydown="if(event.key==='Enter') document.getElementById('input-empresa').focus()">

        <label style="font-size:0.85rem;font-weight:600;color:var(--text);display:block;margin-bottom:6px;">Nome da empresa</label>
        <input id="input-empresa" type="text" class="field-input"
               placeholder="Nome da sua empresa"
               onkeydown="if(event.key==='Enter') startQuiz()"
               style="margin-bottom:20px;">

        <button class="btn-primary" onclick="startQuiz()">Começar o Diagnóstico →</button>
      </div>
      <p class="micro" style="margin-top:14px;">🔒 Seus dados são usados apenas para personalizar o resultado</p>
```

- [ ] **Step 3: Adicionar função `startQuiz()` no bloco `<script>`**

  Inserir antes do `</script>`:

```javascript
    function startQuiz() {
      const nome    = document.getElementById('input-nome').value.trim();
      const empresa = document.getElementById('input-empresa').value.trim();
      if (!nome)    { document.getElementById('input-nome').focus();    return; }
      if (!empresa) { document.getElementById('input-empresa').focus(); return; }
      userName    = nome;
      userEmpresa = empresa;
      currentQ    = 0;
      answers.length = 0;
      renderQuestion();
      showScreen('screen-quiz');
    }
```

- [ ] **Step 4: Verificar no browser**

  Abra o arquivo. Clique "Iniciar Diagnóstico" → deve ir para tela de captura.
  Tente avançar sem preencher → deve focar no campo vazio.
  Preencha os dois campos → deve avançar (erro esperado: quiz ainda não existe).

- [ ] **Step 5: Commit**

```bash
git add diagnostico-estrategiasdigitais/index.html
git commit -m "feat: telas intro e captura nome+empresa"
```

---

## Task 4: Engine de Perguntas — 8 Questões com Escala 1-5

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Adicionar função `renderQuestion()` no `<script>`**

  Inserir antes do `</script>`:

```javascript
    function renderQuestion() {
      // Calcular dimensão e posição dentro da dimensão
      const dimIndex = Math.floor(currentQ / 2); // 0-3
      const qInDim   = currentQ % 2;              // 0 ou 1
      const dim      = DIMENSIONS[dimIndex];
      const pergunta = dim.questions[qInDim];
      const total    = 8;
      const pct      = Math.round((currentQ / total) * 100);

      document.getElementById('quiz-container').innerHTML = `
        <div class="progress-wrap">
          <div class="progress-label">
            <span>Pergunta ${currentQ + 1} de ${total}</span>
            <span>${pct}%</span>
          </div>
          <div class="progress-bar-bg">
            <div class="progress-bar-fill" style="width:${pct}%"></div>
          </div>
        </div>

        <div class="dim-badge">${dim.icon} ${dim.name}</div>

        <div class="card">
          <p style="font-size:1.05rem;font-weight:600;color:var(--text);line-height:1.55;margin-bottom:4px;">
            ${pergunta}
          </p>
          <div class="scale-wrap" id="scale-btns">
            ${[1,2,3,4,5].map(n => `
              <button class="scale-btn" onclick="selectAnswer(${n})" id="btn-${n}">${n}</button>
            `).join('')}
          </div>
          <div class="scale-labels">
            <span>Quase nunca</span>
            <span>Sempre</span>
          </div>
        </div>
      `;
    }

    function selectAnswer(value) {
      // Highlight selecionado
      [1,2,3,4,5].forEach(n => {
        document.getElementById(`btn-${n}`).classList.toggle('selected', n === value);
      });

      // Avançar após pequeno delay
      setTimeout(() => {
        answers[currentQ] = value;
        currentQ++;

        if (currentQ < 8) {
          renderQuestion();
        } else {
          showScreen('screen-processing');
          setTimeout(showResult, 2000);
        }
      }, 380);
    }
```

- [ ] **Step 2: Verificar no browser**

  Preencha nome + empresa → deve entrar no quiz.
  Responda as 8 perguntas → após a última deve ir para tela de processamento (que está vazia ainda, tudo bem).
  Confirmar: barra de progresso avança, badge de dimensão muda a cada 2 perguntas.

- [ ] **Step 3: Commit**

```bash
git add diagnostico-estrategiasdigitais/index.html
git commit -m "feat: engine de perguntas — 8 questões, 4 dimensões, auto-avança"
```

---

## Task 5: Tela de Processamento + Cálculo de Scores

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Substituir comentário dentro de `#screen-processing`**

  Localizar: `<!-- conteúdo — Task 5 -->`  
  Substituir por:

```html
      <div style="text-align:center;padding:60px 0;">
        <div class="spinner"></div>
        <h2 style="font-size:1.3rem;font-weight:700;color:var(--text);margin-bottom:10px;" id="proc-title">
          Analisando o perfil digital...
        </h2>
        <p style="color:var(--text-muted);font-size:0.92rem;" id="proc-sub">
          Processando as respostas da <strong id="proc-empresa"></strong>
        </p>
      </div>
```

- [ ] **Step 2: Adicionar função `showResult()` e helpers de cálculo no `<script>`**

  Inserir antes do `</script>`:

```javascript
    function calcScores() {
      // Score total (0–40)
      const total = answers.reduce((s, a) => s + (a || 0), 0);

      // Scores por dimensão (cada uma tem 2 perguntas → max 10)
      const dimScores = DIMENSIONS.map((dim, i) => ({
        name:  dim.name,
        icon:  dim.icon,
        score: (answers[i * 2] || 0) + (answers[i * 2 + 1] || 0), // 0-10
        pct:   Math.round(((( answers[i * 2] || 0) + (answers[i * 2 + 1] || 0)) / 10) * 100)
      }));

      // Dimensão mais fraca
      const weakest = dimScores.reduce((a, b) => a.score < b.score ? a : b);

      // Faixa
      const faixa = FAIXAS.find(f => total >= f.min && total <= f.max) ?? FAIXAS[FAIXAS.length - 1];

      // Display 0-100
      const displayScore = Math.round((total / MAX_SCORE) * 100);

      return { total, displayScore, dimScores, weakest, faixa };
    }

    function showResult() {
      // Preencher empresa no processando (se ainda visível)
      const el = document.getElementById('proc-empresa');
      if (el) el.textContent = userEmpresa;

      const { displayScore, dimScores, weakest, faixa } = calcScores();

      // Montar HTML do resultado
      document.getElementById('result-container').innerHTML = buildResultHTML(displayScore, dimScores, weakest, faixa);

      showScreen('screen-result');

      // Animar score
      animateCount(document.getElementById('score-num'), 0, displayScore, 1000);

      // Animar barras
      setTimeout(() => {
        dimScores.forEach((d, i) => {
          const bar = document.getElementById(`bar-fill-${i}`);
          if (bar) bar.style.width = d.pct + '%';
        });
      }, 200);

      // Salvar lead (sem WhatsApp por enquanto)
      salvarLead({ nome: userName, empresa: userEmpresa, whatsapp: '', pontuacao: displayScore, faixa: faixa.title, dimensao_fraca: weakest.name, respostas: answers });
    }

    function animateCount(el, from, to, duration) {
      if (!el) return;
      const start = performance.now();
      function step(now) {
        const progress = Math.min((now - start) / duration, 1);
        el.textContent = Math.round(from + (to - from) * progress);
        if (progress < 1) requestAnimationFrame(step);
      }
      requestAnimationFrame(step);
    }
```

- [ ] **Step 3: Adicionar `buildResultHTML` no `<script>` (esqueleto — Task 6 vai completar)**

  Inserir antes do `</script>`:

```javascript
    function buildResultHTML(displayScore, dimScores, weakest, faixa) {
      return `<p style="color:var(--text-muted);text-align:center;">Resultado em construção — Task 6</p>`;
    }
```

- [ ] **Step 4: Verificar no browser**

  Complete o quiz → deve aparecer tela de processamento por 2s → depois tela de resultado com texto placeholder.
  Sem erros no console.

- [ ] **Step 5: Commit**

```bash
git add diagnostico-estrategiasdigitais/index.html
git commit -m "feat: tela processamento + cálculo de scores por dimensão"
```

---

## Task 6: Tela de Resultado — Score, Barras e Fechamento Personalizado

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Adicionar `getClosing()` no `<script>`**

  Inserir antes do `</script>`:

```javascript
    function getClosing(faixa, weakestName) {
      const map = {
        '#EF4444': `${userName}, o diagnóstico da <strong>${userEmpresa}</strong> é claro: o digital ainda não trabalha por você. Cada mês sem estratégia estruturada é receita que fica na mesa. Vamos mapear juntos onde está o maior gargalo e o que priorizar agora.`,
        '#F59E0B': `${userName}, a <strong>${userEmpresa}</strong> já deu os primeiros passos no digital — mas o esforço ainda não gera resultado proporcional. O maior gap está em <strong>${weakestName}</strong>. Com os ajustes certos, isso muda rápido.`,
        '#10B981': `${userName}, a <strong>${userEmpresa}</strong> tem uma base digital sólida. O que impede o próximo nível são pontos cegos em <strong>${weakestName}</strong>. Precisão nessa área pode desbloquear um crescimento significativo.`,
        '#00AEEF': `${userName}, a <strong>${userEmpresa}</strong> está entre as empresas mais preparadas digitalmente. O próximo movimento é ativar IA e automação nos processos certos para crescer sem aumentar custo operacional.`
      };
      return map[faixa.color] || map['#F59E0B'];
    }
```

- [ ] **Step 2: Substituir `buildResultHTML()` pela versão completa**

  Localizar e substituir a função `buildResultHTML` por:

```javascript
    function buildResultHTML(displayScore, dimScores, weakest, faixa) {
      const dimBarsHTML = dimScores.map((d, i) => `
        <div class="dim-bar-wrap">
          <div class="dim-bar-header">
            <span>${d.icon} ${d.name}</span>
            <span style="color:var(--primary)">${d.pct}%</span>
          </div>
          <div class="dim-bar-bg">
            <div class="dim-bar-fill" id="bar-fill-${i}"
                 style="width:0%;background:${d.score <= 4 ? '#EF4444' : d.score <= 6 ? '#F59E0B' : d.score <= 8 ? '#10B981' : '#00AEEF'}">
            </div>
          </div>
        </div>
      `).join('');

      return `
        <!-- Faixa + Score -->
        <div style="text-align:center;margin-bottom:28px;">
          <div style="font-size:0.85rem;color:var(--text-muted);margin-bottom:16px;">Diagnóstico de ${userEmpresa}</div>
          <div class="score-circle">
            <span class="score-num" id="score-num">0</span>
            <span class="score-label">de 100 pts</span>
          </div>
          <div style="display:inline-flex;align-items:center;gap:8px;background:${faixa.bg};border:1.5px solid ${faixa.color};border-radius:99px;padding:6px 18px;margin-bottom:10px;">
            <span style="font-size:1.1rem;">${faixa.emoji}</span>
            <span style="font-size:0.9rem;font-weight:700;color:${faixa.color};">${faixa.title}</span>
          </div>
        </div>

        <!-- Barras por dimensão -->
        <div class="card" style="margin-bottom:20px;">
          <h3 style="font-size:1rem;font-weight:700;color:var(--text);margin-bottom:16px;">Desempenho por dimensão</h3>
          ${dimBarsHTML}
          <p style="font-size:0.78rem;color:var(--text-muted);margin-top:12px;">
            ⚠️ Maior gap identificado: <strong style="color:${weakest.score <= 4 ? '#EF4444' : '#F59E0B'}">${weakest.icon} ${weakest.name}</strong>
          </p>
        </div>

        <!-- Fechamento personalizado -->
        <div class="card" style="margin-bottom:20px;border-left:4px solid ${faixa.color};">
          <p style="font-size:0.95rem;line-height:1.7;color:var(--text-muted);">${getClosing(faixa, weakest.name)}</p>
        </div>

        <!-- WhatsApp + CTA -->
        <div style="background:linear-gradient(135deg, var(--primary), var(--primary-dark));border-radius:var(--radius);padding:24px;text-align:center;">
          <h3 style="color:#fff;font-size:1.1rem;font-weight:700;margin-bottom:6px;">
            Quer um plano de ação para a ${userEmpresa}?
          </h3>
          <p style="color:rgba(255,255,255,0.85);font-size:0.88rem;margin-bottom:18px;">
            Informe seu WhatsApp e receba um diagnóstico aprofundado gratuito
          </p>
          <div style="position:relative;margin-bottom:12px;">
            <span style="position:absolute;left:14px;top:50%;transform:translateY(-50%);font-size:1rem;pointer-events:none;">📱</span>
            <input id="input-whatsapp" type="tel" maxlength="15"
                   style="width:100%;padding:14px 14px 14px 42px;border:none;border-radius:8px;font-family:'Inter',sans-serif;font-size:1rem;outline:none;"
                   placeholder="(61) 99999-9999"
                   oninput="maskWpp(this)"
                   onkeydown="if(event.key==='Enter') abrirWhatsApp()">
          </div>
          <button class="btn-primary"
                  onclick="abrirWhatsApp()"
                  style="background:#fff;color:var(--primary);box-shadow:none;">
            📲 Quero meu Diagnóstico Gratuito
          </button>
          <p style="color:rgba(255,255,255,0.7);font-size:0.75rem;margin-top:10px;">
            WhatsApp opcional — você já pode entrar em contato agora
          </p>
        </div>

        <div style="text-align:center;margin-top:24px;">
          <button onclick="resetQuiz()" style="background:none;border:none;color:var(--text-muted);font-size:0.82rem;cursor:pointer;text-decoration:underline;">
            Refazer diagnóstico
          </button>
        </div>
      `;
    }
```

- [ ] **Step 3: Verificar no browser**

  Complete o quiz → resultado deve mostrar:
  - Círculo azul com pontuação animando de 0 até o valor
  - Badge de faixa (🔴/🟡/🟢/🔵)
  - 4 barras animando após aparecer
  - Texto de fechamento personalizado com nome + empresa + dimensão mais fraca
  - Campo WhatsApp + botão CTA

- [ ] **Step 4: Commit**

```bash
git add diagnostico-estrategiasdigitais/index.html
git commit -m "feat: tela resultado — score, barras, faixa e fechamento personalizado"
```

---

## Task 7: WhatsApp CTA + Google Sheets Integration

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Adicionar `maskWpp()`, `abrirWhatsApp()` e `salvarLead()` no `<script>`**

  Inserir antes do `</script>`:

```javascript
    function maskWpp(el) {
      let v = el.value.replace(/\D/g, '').slice(0, 11);
      if (v.length > 10)     v = v.replace(/^(\d{2})(\d{5})(\d{4})$/, '($1) $2-$3');
      else if (v.length > 6) v = v.replace(/^(\d{2})(\d{4})(\d*)/, '($1) $2-$3');
      else if (v.length > 2) v = v.replace(/^(\d{2})(\d*)/, '($1) $2');
      else if (v.length > 0) v = v.replace(/^(\d*)/, '($1');
      el.value = v;
    }

    function abrirWhatsApp() {
      const wppEl = document.getElementById('input-whatsapp');
      const wpp   = wppEl ? wppEl.value.trim() : '';

      // Re-salvar com WhatsApp
      const { displayScore, weakest, faixa } = calcScores();
      salvarLead({
        nome:          userName,
        empresa:       userEmpresa,
        whatsapp:      wpp,
        pontuacao:     displayScore,
        faixa:         faixa.title,
        dimensao_fraca: weakest.name,
        respostas:     answers
      });

      const msg = encodeURIComponent(
        `Olá Walter, acabei de fazer o diagnóstico digital da ${userEmpresa} e quero conversar sobre os resultados.`
      );
      window.open(`https://wa.me/5561999354363?text=${msg}`, '_blank');
    }

    function salvarLead(dados) {
      if (!PLANILHA_URL || PLANILHA_URL.startsWith('COLE')) return;
      const params = new URLSearchParams({
        nome:           dados.nome          || '',
        empresa:        dados.empresa       || '',
        whatsapp:       dados.whatsapp      || '',
        pontuacao:      dados.pontuacao     || '',
        faixa:          dados.faixa         || '',
        dimensao_fraca: dados.dimensao_fraca || '',
        p1: dados.respostas[0] || '',
        p2: dados.respostas[1] || '',
        p3: dados.respostas[2] || '',
        p4: dados.respostas[3] || '',
        p5: dados.respostas[4] || '',
        p6: dados.respostas[5] || '',
        p7: dados.respostas[6] || '',
        p8: dados.respostas[7] || ''
      });
      fetch(`${PLANILHA_URL}?${params}`, { mode: 'no-cors' }).catch(() => {});
    }

    function resetQuiz() {
      currentQ = 0;
      answers.length = 0;
      userName = '';
      userEmpresa = '';
      showScreen('screen-intro');
    }
```

- [ ] **Step 2: Atualizar `PLANILHA_URL` com a URL do Apps Script da Task 1**

  Localizar:
  ```javascript
  const PLANILHA_URL = 'COLE_A_URL_DO_APPS_SCRIPT_AQUI';
  ```
  Substituir pela URL real obtida na Task 1.

- [ ] **Step 3: Verificar integração**

  Complete o quiz, insira um WhatsApp e clique no CTA.
  Esperado:
  - Abre WhatsApp com mensagem pré-preenchida
  - Na planilha, aparece uma linha com todos os dados (nome, empresa, wpp, pontuação, faixa, dimensão mais fraca, respostas P1-P8)

- [ ] **Step 4: Commit**

```bash
git add diagnostico-estrategiasdigitais/index.html
git commit -m "feat: WhatsApp CTA + integração Google Sheets com todas as respostas"
```

---

## Task 8: Polish + Deploy Vercel Separado

**Files:**
- Modify: `diagnostico-estrategiasdigitais/index.html`

- [ ] **Step 1: Testar fluxo completo no mobile**

  Abra o arquivo no celular (ou DevTools → modo mobile).
  Verificar:
  - Logo carrega corretamente
  - Botões de escala (1-5) são fáceis de tocar
  - Texto legível sem zoom
  - Resultado cabe na tela sem scroll excessivo

- [ ] **Step 2: Adicionar meta OG para compartilhamento**

  Inserir dentro do `<head>`, após o `<title>`:

```html
  <meta property="og:title" content="Diagnóstico Digital — Estratégias Digitais">
  <meta property="og:description" content="Descubra o nível de maturidade digital da sua empresa em 3 minutos.">
  <meta property="og:type" content="website">
  <meta name="theme-color" content="#00AEEF">
```

- [ ] **Step 3: Commit final antes do deploy**

```bash
git add diagnostico-estrategiasdigitais/
git commit -m "feat: polish — meta OG, ajustes mobile, quiz-ed completo"
git push
```

- [ ] **Step 4: Criar novo projeto no Vercel**

  1. Acesse vercel.com → New Project
  2. Importe o repositório `raio-x-360` (o mesmo já usado)
  3. Em **Root Directory** → clique em "Edit" → selecione `diagnostico-estrategiasdigitais`
  4. Framework Preset: **Other**
  5. Clique em **Deploy**

- [ ] **Step 5: Configurar nome do projeto**

  Após o deploy, vá em Settings → General → Project Name.
  Sugestão: `diagnostico-ed`
  Isso gera a URL: `diagnostico-ed.vercel.app`

- [ ] **Step 6: Verificar URL ao vivo**

  Acesse `diagnostico-ed.vercel.app` (ou o nome escolhido).
  Esperado: quiz funciona igual ao local, logo aparece, Google Sheets recebe os dados.

- [ ] **Step 7: Gerar QR Code para o evento**

  Acesse qr.io ou qrcode-monkey.com e gere um QR code apontando para a URL final.
  Sugestão de cor do QR: `#00AEEF` (azul da Estratégias Digitais).

---

## Self-Review

### Spec coverage

| Requisito do spec | Task |
|-------------------|------|
| 8 perguntas, 4 dimensões × 2 | Task 4 |
| Escala 1-5 | Task 4 |
| Captura: nome + empresa no início | Task 3 |
| Captura: WhatsApp no resultado | Task 6 + 7 |
| Score 0-100 normalizado | Task 5 |
| 4 faixas (🔴🟡🟢🔵) | Task 5 + 6 |
| Gráfico de barras por dimensão | Task 6 |
| Fechamento personalizado (nome + empresa + dimensão mais fraca) | Task 6 |
| CTA WhatsApp pré-preenchido | Task 7 |
| Google Sheets com todas as 15 colunas | Task 1 + 7 |
| Visual: branco + azul #00AEEF + Inter | Task 2 |
| Logo Estratégias Digitais | Task 2 |
| Pasta separada `diagnostico-estrategiasdigitais/` | Task 2 |
| Deploy Vercel separado | Task 8 |
| Sem referência ao Bruno Capanema | ✅ nenhuma presente |

### Placeholder scan
- Nenhum "TBD" ou "TODO" no plano ✅
- Task 3 (buildResultHTML esqueleto) é substituído na Task 6 ✅
- Todos os valores de cores, URLs e números são exatos ✅

### Type consistency
- `answers[]` usado consistentemente como array de 8 posições indexadas 0-7 ✅
- `calcScores()` retorna `{ total, displayScore, dimScores, weakest, faixa }` — todos os campos consumidos em `showResult()` e `abrirWhatsApp()` ✅
- `salvarLead({ nome, empresa, whatsapp, pontuacao, faixa, dimensao_fraca, respostas })` — interface consistente nas duas chamadas ✅
