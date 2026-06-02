<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Learning def Functions</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #12121a;
    --card: #1a1a26;
    --border: #2a2a40;
    --accent: #7c6af7;
    --accent2: #f7a26a;
    --accent3: #6af7c0;
    --text: #e8e8f0;
    --muted: #888899;
    --code-bg: #0d0d18;
    --keyword: #c792ea;
    --funcname: #82aaff;
    --string: #c3e88d;
    --number: #f78c6c;
    --comment: #546e7a;
    --param: #ffcb6b;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Animated background grid */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(124,106,247,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(124,106,247,0.04) 1px, transparent 1px);
    background-size: 40px 40px;
    z-index: 0;
    pointer-events: none;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 48px 24px 80px;
    position: relative;
    z-index: 1;
  }

  /* Header */
  header {
    text-align: center;
    margin-bottom: 56px;
    animation: fadeDown 0.7s ease both;
  }

  .pill {
    display: inline-block;
    background: rgba(124,106,247,0.15);
    border: 1px solid rgba(124,106,247,0.35);
    color: var(--accent);
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.12em;
    padding: 5px 14px;
    border-radius: 100px;
    margin-bottom: 20px;
    text-transform: uppercase;
  }

  h1 {
    font-size: clamp(2.2rem, 6vw, 3.6rem);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.02em;
    background: linear-gradient(135deg, #fff 30%, var(--accent) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 16px;
  }

  .subtitle {
    color: var(--muted);
    font-size: 1rem;
    font-weight: 400;
    letter-spacing: 0.01em;
  }

  /* Progress bar */
  .progress-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px 24px;
    margin-bottom: 40px;
    display: flex;
    align-items: center;
    gap: 16px;
    animation: fadeDown 0.7s 0.15s ease both;
  }

  .progress-label {
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    color: var(--muted);
    white-space: nowrap;
  }

  .progress-bar {
    flex: 1;
    height: 6px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--accent), var(--accent3));
    border-radius: 99px;
    transition: width 0.6s cubic-bezier(0.4,0,0.2,1);
    width: 0%;
  }

  .progress-pct {
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    font-weight: 700;
    color: var(--accent);
    min-width: 36px;
    text-align: right;
  }

  /* Lesson cards */
  .lesson {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 32px;
    margin-bottom: 28px;
    animation: fadeUp 0.5s ease both;
    transition: border-color 0.3s;
  }

  .lesson:hover { border-color: rgba(124,106,247,0.4); }

  .lesson-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 22px;
  }

  .lesson-num {
    width: 36px; height: 36px;
    border-radius: 10px;
    background: rgba(124,106,247,0.15);
    border: 1px solid rgba(124,106,247,0.3);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    font-weight: 700;
    flex-shrink: 0;
  }

  .lesson-title {
    font-size: 1.15rem;
    font-weight: 700;
    letter-spacing: -0.01em;
  }

  .lesson-body {
    color: #b0b0c8;
    font-size: 0.95rem;
    line-height: 1.7;
    margin-bottom: 22px;
  }

  .lesson-body strong { color: var(--text); }

  /* Code blocks */
  .code-block {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 24px;
    font-family: 'Space Mono', monospace;
    font-size: 13.5px;
    line-height: 1.85;
    overflow-x: auto;
    margin-bottom: 18px;
    position: relative;
  }

  .code-block .line-comment { color: var(--comment); }
  .code-block .kw { color: var(--keyword); }
  .code-block .fn { color: var(--funcname); }
  .code-block .str { color: var(--string); }
  .code-block .num { color: var(--number); }
  .code-block .prm { color: var(--param); }
  .code-block .ret { color: var(--accent3); }
  .code-block .output { color: var(--accent2); }

  .code-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .code-label::before {
    content: '';
    display: inline-block;
    width: 7px; height: 7px;
    border-radius: 50%;
    background: var(--accent);
  }

  /* Anatomy diagram */
  .anatomy {
    background: var(--code-bg);
    border: 1px solid rgba(124,106,247,0.3);
    border-radius: 12px;
    padding: 24px;
    margin-bottom: 20px;
    font-family: 'Space Mono', monospace;
    font-size: 14px;
  }

  .anatomy-line {
    display: flex;
    align-items: baseline;
    flex-wrap: wrap;
    gap: 0;
    margin-bottom: 8px;
  }

  .tag {
    display: inline-block;
    font-size: 10px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    padding: 2px 7px;
    border-radius: 5px;
    margin-left: 10px;
    vertical-align: middle;
  }

  .tag-kw { background: rgba(199,146,234,0.18); color: #c792ea; }
  .tag-fn { background: rgba(130,170,255,0.18); color: #82aaff; }
  .tag-pm { background: rgba(255,203,107,0.18); color: #ffcb6b; }
  .tag-col { background: rgba(106,247,192,0.18); color: #6af7c0; }
  .tag-body { background: rgba(247,162,106,0.18); color: #f7a26a; }

  /* Quiz */
  .quiz-section {
    background: linear-gradient(135deg, rgba(124,106,247,0.08), rgba(106,247,192,0.05));
    border: 1px solid rgba(124,106,247,0.25);
    border-radius: 18px;
    padding: 32px;
    margin-bottom: 28px;
    animation: fadeUp 0.5s 0.3s ease both;
  }

  .quiz-title {
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 6px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .quiz-icon { font-size: 1.3rem; }

  .quiz-q {
    color: var(--muted);
    font-size: 0.92rem;
    margin-bottom: 20px;
    line-height: 1.6;
  }

  .options {
    display: grid;
    gap: 10px;
  }

  .opt-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 13px 18px;
    color: var(--text);
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    cursor: pointer;
    text-align: left;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .opt-btn:hover {
    border-color: var(--accent);
    background: rgba(124,106,247,0.1);
    transform: translateX(4px);
  }

  .opt-btn.correct {
    border-color: var(--accent3);
    background: rgba(106,247,192,0.1);
    color: var(--accent3);
  }

  .opt-btn.wrong {
    border-color: #f76a6a;
    background: rgba(247,106,106,0.1);
    color: #f76a6a;
  }

  .opt-letter {
    width: 24px; height: 24px;
    border-radius: 6px;
    background: var(--border);
    display: flex; align-items: center; justify-content: center;
    font-size: 11px;
    font-weight: 700;
    flex-shrink: 0;
  }

  .feedback {
    margin-top: 14px;
    padding: 12px 16px;
    border-radius: 10px;
    font-size: 0.88rem;
    font-weight: 600;
    display: none;
  }

  .feedback.show { display: block; }
  .feedback.good { background: rgba(106,247,192,0.1); color: var(--accent3); border: 1px solid rgba(106,247,192,0.3); }
  .feedback.bad { background: rgba(247,106,106,0.1); color: #f78a8a; border: 1px solid rgba(247,106,106,0.3); }

  /* Challenge */
  .challenge {
    background: var(--card);
    border: 1px dashed rgba(247,162,106,0.4);
    border-radius: 18px;
    padding: 32px;
    margin-bottom: 28px;
    animation: fadeUp 0.5s 0.4s ease both;
  }

  .challenge-title {
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 8px;
    color: var(--accent2);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .challenge-desc {
    color: var(--muted);
    font-size: 0.92rem;
    line-height: 1.65;
    margin-bottom: 18px;
  }

  textarea {
    width: 100%;
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    color: var(--text);
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    line-height: 1.8;
    padding: 18px 20px;
    resize: vertical;
    min-height: 120px;
    outline: none;
    transition: border-color 0.2s;
    margin-bottom: 14px;
  }

  textarea:focus { border-color: var(--accent2); }

  .run-btn {
    background: linear-gradient(135deg, var(--accent), #5e4fe0);
    border: none;
    border-radius: 10px;
    color: #fff;
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 700;
    padding: 12px 26px;
    cursor: pointer;
    letter-spacing: 0.03em;
    transition: all 0.2s;
    display: inline-flex;
    align-items: center;
    gap: 8px;
  }

  .run-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(124,106,247,0.35);
  }

  .run-btn:active { transform: translateY(0); }

  .output-box {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 20px;
    margin-top: 14px;
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    color: var(--accent3);
    min-height: 44px;
    display: none;
    white-space: pre-wrap;
  }

  .output-box.show { display: block; }

  /* Cheatsheet */
  .cheatsheet {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 32px;
    animation: fadeUp 0.5s 0.5s ease both;
  }

  .cheatsheet h2 {
    font-size: 1.2rem;
    font-weight: 700;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .cs-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
  }

  @media (max-width: 540px) {
    .cs-grid { grid-template-columns: 1fr; }
  }

  .cs-item {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px 18px;
  }

  .cs-label {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 8px;
  }

  .cs-code {
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    color: #ccc;
    line-height: 1.8;
  }

  /* Animations */
  @keyframes fadeDown {
    from { opacity: 0; transform: translateY(-20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .lesson:nth-child(1) { animation-delay: 0.1s; }
  .lesson:nth-child(2) { animation-delay: 0.2s; }
  .lesson:nth-child(3) { animation-delay: 0.3s; }
</style>
</head>
<body>
<div class="container">

  <header>
    <div class="pill">Python Fundamentals</div>
    <h1>The def Function</h1>
    <p class="subtitle">Write reusable, clean code — one function at a time.</p>
  </header>

  <div class="progress-wrap">
    <span class="progress-label">Progress</span>
    <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
    <span class="progress-pct" id="progressPct">0%</span>
  </div>

  <!-- LESSON 1 -->
  <div class="lesson">
    <div class="lesson-header">
      <div class="lesson-num">01</div>
      <div class="lesson-title">What is a Function?</div>
    </div>
    <div class="lesson-body">
      A <strong>function</strong> is a reusable block of code that does a specific job. Instead of writing the same code over and over, you define it once and <strong>call it</strong> whenever you need it.<br><br>
      In Python, you create a function using the <strong>def</strong> keyword (short for <em>define</em>).
    </div>

    <div class="code-label">Syntax Anatomy</div>
    <div class="anatomy">
      <div class="anatomy-line">
        <span class="kw">def</span><span class="tag tag-kw">keyword</span>
        &nbsp;
        <span class="fn">greet</span><span class="tag tag-fn">function name</span>
        <span>(</span><span class="prm">name</span><span class="tag tag-pm">parameter</span>
        <span>)</span>
        <span class="ret">:</span><span class="tag tag-col">colon</span>
      </div>
      <div style="margin-left:28px; color: #888; font-size:12px;">
        &nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#b0b0c8;">print(</span><span class="str">"Hello, "</span> <span style="color:#b0b0c8;">+</span> <span class="prm">name</span><span style="color:#b0b0c8;">)</span>
        <span class="tag tag-body">body (indented!)</span>
      </div>
    </div>

    <div class="code-label">Full Example</div>
    <div class="code-block">
<span class="line-comment"># 1. Define the function</span>
<span class="kw">def</span> <span class="fn">greet</span>(<span class="prm">name</span>):
    <span class="fn">print</span>(<span class="str">"Hello, "</span> + <span class="prm">name</span> + <span class="str">"!"</span>)

<span class="line-comment"># 2. Call the function</span>
<span class="fn">greet</span>(<span class="str">"Alice"</span>)   <span class="line-comment"># → Hello, Alice!</span>
<span class="fn">greet</span>(<span class="str">"Bob"</span>)     <span class="line-comment"># → Hello, Bob!</span>
    </div>
  </div>

  <!-- LESSON 2 -->
  <div class="lesson">
    <div class="lesson-header">
      <div class="lesson-num">02</div>
      <div class="lesson-title">Parameters & Arguments</div>
    </div>
    <div class="lesson-body">
      <strong>Parameters</strong> are the variable names in the function definition (the inputs it expects).<br>
      <strong>Arguments</strong> are the actual values you pass in when calling the function.<br><br>
      Functions can have <strong>multiple parameters</strong>, and you can give them <strong>default values</strong>.
    </div>

    <div class="code-label">Multiple Parameters</div>
    <div class="code-block">
<span class="kw">def</span> <span class="fn">add</span>(<span class="prm">a</span>, <span class="prm">b</span>):
    result = <span class="prm">a</span> + <span class="prm">b</span>
    <span class="fn">print</span>(result)

<span class="fn">add</span>(<span class="num">3</span>, <span class="num">7</span>)    <span class="line-comment"># → 10</span>
<span class="fn">add</span>(<span class="num">10</span>, <span class="num">20</span>)  <span class="line-comment"># → 30</span>
    </div>

    <div class="code-label">Default Parameter Values</div>
    <div class="code-block">
<span class="kw">def</span> <span class="fn">greet</span>(<span class="prm">name</span>, <span class="prm">language</span>=<span class="str">"English"</span>):
    <span class="kw">if</span> <span class="prm">language</span> == <span class="str">"Spanish"</span>:
        <span class="fn">print</span>(<span class="str">"Hola, "</span> + <span class="prm">name</span>)
    <span class="kw">else</span>:
        <span class="fn">print</span>(<span class="str">"Hello, "</span> + <span class="prm">name</span>)

<span class="fn">greet</span>(<span class="str">"Maria"</span>)                   <span class="line-comment"># → Hello, Maria</span>
<span class="fn">greet</span>(<span class="str">"Carlos"</span>, <span class="str">"Spanish"</span>)       <span class="line-comment"># → Hola, Carlos</span>
    </div>
  </div>

  <!-- LESSON 3 -->
  <div class="lesson">
    <div class="lesson-header">
      <div class="lesson-num">03</div>
      <div class="lesson-title">The return Statement</div>
    </div>
    <div class="lesson-body">
      Instead of just printing, a function can <strong>return</strong> a value back to wherever it was called. This makes functions much more powerful — you can use the result in further calculations!
    </div>

    <div class="code-label">print vs return</div>
    <div class="code-block">
<span class="line-comment"># ❌ print only — can't use the result later</span>
<span class="kw">def</span> <span class="fn">square_print</span>(<span class="prm">n</span>):
    <span class="fn">print</span>(<span class="prm">n</span> * <span class="prm">n</span>)

<span class="line-comment"># ✅ return — you can use the value!</span>
<span class="kw">def</span> <span class="fn">square</span>(<span class="prm">n</span>):
    <span class="kw">return</span> <span class="prm">n</span> * <span class="prm">n</span>

result = <span class="fn">square</span>(<span class="num">5</span>)         <span class="line-comment"># result = 25</span>
big = <span class="fn">square</span>(<span class="num">10</span>) + <span class="fn">square</span>(<span class="num">3</span>) <span class="line-comment"># 100 + 9 = 109</span>
<span class="fn">print</span>(big)                    <span class="line-comment"># → 109</span>
    </div>
  </div>

  <!-- QUIZ -->
  <div class="quiz-section">
    <div class="quiz-title">
      <span class="quiz-icon">🧠</span> Quick Quiz
    </div>
    <div class="quiz-q">What keyword do you use to send a value <em>back</em> from a function?</div>
    <div class="options" id="quizOptions">
      <button class="opt-btn" onclick="checkAnswer(this, false)">
        <span class="opt-letter">A</span> def
      </button>
      <button class="opt-btn" onclick="checkAnswer(this, false)">
        <span class="opt-letter">B</span> print
      </button>
      <button class="opt-btn" onclick="checkAnswer(this, true)">
        <span class="opt-letter">C</span> return
      </button>
      <button class="opt-btn" onclick="checkAnswer(this, false)">
        <span class="opt-letter">D</span> send
      </button>
    </div>
    <div class="feedback" id="feedback"></div>
  </div>

  <!-- CHALLENGE -->
  <div class="challenge">
    <div class="challenge-title">
      ⚡ Mini Challenge
    </div>
    <div class="challenge-desc">
      Write a function called <code style="background:rgba(255,255,255,0.08);padding:2px 6px;border-radius:5px;font-family:'Space Mono',monospace;">multiply</code> that takes two numbers and <strong>returns</strong> their product. Then call it with any two numbers you like!<br><br>
      Hit <strong>Run</strong> to check your solution.
    </div>
    <textarea id="codeInput" placeholder="def multiply(a, b):&#10;    # your code here&#10;&#10;print(multiply(4, 5))"></textarea>
    <button class="run-btn" onclick="runCode()">▶ Run Code</button>
    <div class="output-box" id="outputBox"></div>
  </div>

  <!-- CHEATSHEET -->
  <div class="cheatsheet">
    <h2>📋 def Function Cheat Sheet</h2>
    <div class="cs-grid">
      <div class="cs-item">
        <div class="cs-label">Basic Function</div>
        <div class="cs-code">
          <span style="color:#c792ea;">def</span> <span style="color:#82aaff;">say_hi</span>():<br>
          &nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#82aaff;">print</span>(<span style="color:#c3e88d;">"Hi!"</span>)<br>
          <span style="color:#82aaff;">say_hi</span>()
        </div>
      </div>
      <div class="cs-item">
        <div class="cs-label">With Parameters</div>
        <div class="cs-code">
          <span style="color:#c792ea;">def</span> <span style="color:#82aaff;">add</span>(<span style="color:#ffc