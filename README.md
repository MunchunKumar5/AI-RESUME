# AI-RESUME
Check Resume Score 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ResumeAI — Intelligent Resume Analyzer</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;1,9..40,300&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #111118;
    --surface2: #1a1a24;
    --border: rgba(255,255,255,0.07);
    --accent: #6ee7b7;
    --accent2: #818cf8;
    --accent3: #f472b6;
    --text: #e8e8f0;
    --muted: #6b6b80;
    --danger: #f87171;
    --warning: #fbbf24;
    --success: #34d399;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Background grid */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(110,231,183,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(110,231,183,0.03) 1px, transparent 1px);
    background-size: 48px 48px;
    pointer-events: none;
    z-index: 0;
  }

  /* Glow blobs */
  .blob {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    pointer-events: none;
    z-index: 0;
  }
  .blob-1 { width: 500px; height: 500px; background: rgba(110,231,183,0.06); top: -100px; right: -100px; }
  .blob-2 { width: 400px; height: 400px; background: rgba(129,140,248,0.05); bottom: 100px; left: -100px; }

  /* Header */
  header {
    position: relative; z-index: 10;
    padding: 28px 48px;
    display: flex; align-items: center; justify-content: space-between;
    border-bottom: 1px solid var(--border);
    backdrop-filter: blur(10px);
  }

  .logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.4rem;
    letter-spacing: -0.02em;
    display: flex; align-items: center; gap: 10px;
  }

  .logo-icon {
    width: 32px; height: 32px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
  }

  .logo span { color: var(--accent); }

  .badge {
    font-size: 0.7rem;
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--muted);
    border: 1px solid var(--border);
    padding: 4px 12px;
    border-radius: 20px;
  }

  /* Main layout */
  main {
    position: relative; z-index: 10;
    max-width: 1100px;
    margin: 0 auto;
    padding: 60px 48px;
  }

  /* Hero */
  .hero {
    text-align: center;
    margin-bottom: 56px;
    animation: fadeUp 0.6s ease both;
  }

  .hero-tag {
    display: inline-flex; align-items: center; gap: 8px;
    font-size: 0.75rem;
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent);
    background: rgba(110,231,183,0.08);
    border: 1px solid rgba(110,231,183,0.2);
    padding: 6px 16px;
    border-radius: 20px;
    margin-bottom: 24px;
  }

  .hero-tag::before { content: '◈'; }

  h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2.2rem, 5vw, 3.6rem);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.03em;
    margin-bottom: 18px;
  }

  h1 em {
    font-style: normal;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero p {
    font-size: 1.05rem;
    color: var(--muted);
    font-weight: 300;
    max-width: 520px;
    margin: 0 auto;
    line-height: 1.7;
  }

  /* Upload + Job section */
  .input-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    margin-bottom: 24px;
    animation: fadeUp 0.6s 0.1s ease both;
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 28px;
    transition: border-color 0.2s;
  }

  .card:hover { border-color: rgba(255,255,255,0.12); }

  .card-label {
    font-family: 'Syne', sans-serif;
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 16px;
    display: flex; align-items: center; gap: 8px;
  }

  .card-label span {
    width: 20px; height: 20px;
    background: var(--surface2);
    border-radius: 6px;
    display: inline-flex; align-items: center; justify-content: center;
    font-size: 10px;
  }

  /* Drop zone */
  .dropzone {
    border: 1.5px dashed rgba(110,231,183,0.25);
    border-radius: 12px;
    padding: 36px 24px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    background: rgba(110,231,183,0.02);
    position: relative;
  }

  .dropzone:hover, .dropzone.drag-over {
    border-color: var(--accent);
    background: rgba(110,231,183,0.05);
  }

  .dropzone input[type="file"] {
    position: absolute; inset: 0; opacity: 0; cursor: pointer; width: 100%;
  }

  .drop-icon { font-size: 2rem; margin-bottom: 12px; }

  .drop-title {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.95rem;
    margin-bottom: 6px;
  }

  .drop-sub { font-size: 0.8rem; color: var(--muted); }

  .file-selected {
    display: none;
    align-items: center; gap: 10px;
    background: rgba(110,231,183,0.08);
    border-radius: 8px;
    padding: 10px 14px;
    font-size: 0.85rem;
    color: var(--accent);
    margin-top: 12px;
  }

  /* Textarea */
  textarea {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 12px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.88rem;
    line-height: 1.6;
    padding: 16px;
    resize: none;
    height: 160px;
    outline: none;
    transition: border-color 0.2s;
  }

  textarea::placeholder { color: var(--muted); }
  textarea:focus { border-color: rgba(129,140,248,0.4); }

  /* Options row */
  .options-row {
    display: flex; gap: 12px; flex-wrap: wrap;
    margin-bottom: 28px;
    animation: fadeUp 0.6s 0.15s ease both;
  }

  .option-chip {
    display: flex; align-items: center; gap: 8px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 18px;
    font-size: 0.82rem;
    cursor: pointer;
    transition: all 0.2s;
    user-select: none;
    color: var(--muted);
  }

  .option-chip input { display: none; }

  .option-chip:hover { border-color: rgba(255,255,255,0.15); color: var(--text); }

  .option-chip.active {
    background: rgba(110,231,183,0.08);
    border-color: rgba(110,231,183,0.3);
    color: var(--accent);
  }

  /* Analyze button */
  .analyze-btn {
    width: 100%;
    padding: 18px;
    background: linear-gradient(135deg, var(--accent), #10b981);
    color: #0a0a0f;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 1rem;
    letter-spacing: 0.02em;
    border: none;
    border-radius: 14px;
    cursor: pointer;
    transition: all 0.2s;
    display: flex; align-items: center; justify-content: center; gap: 10px;
    animation: fadeUp 0.6s 0.2s ease both;
  }

  .analyze-btn:hover { transform: translateY(-2px); box-shadow: 0 12px 40px rgba(110,231,183,0.25); }
  .analyze-btn:active { transform: translateY(0); }
  .analyze-btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

  /* Loading */
  .loading {
    display: none;
    text-align: center;
    padding: 60px;
    animation: fadeUp 0.4s ease both;
  }

  .loading.show { display: block; }

  .spinner {
    width: 48px; height: 48px;
    border: 3px solid var(--surface2);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin: 0 auto 20px;
  }

  .loading-text {
    font-family: 'Syne', sans-serif;
    font-size: 0.9rem;
    color: var(--muted);
  }

  .loading-steps {
    display: flex; justify-content: center; gap: 32px;
    margin-top: 32px;
  }

  .loading-step {
    display: flex; flex-direction: column; align-items: center; gap: 8px;
    font-size: 0.75rem; color: var(--muted);
  }

  .loading-step .dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--surface2);
    animation: pulse 1.5s ease infinite;
  }

  .loading-step:nth-child(1) .dot { animation-delay: 0s; background: var(--accent); }
  .loading-step:nth-child(2) .dot { animation-delay: 0.3s; }
  .loading-step:nth-child(3) .dot { animation-delay: 0.6s; }
  .loading-step:nth-child(4) .dot { animation-delay: 0.9s; }

  /* Results */
  #results {
    display: none;
    animation: fadeUp 0.5s ease both;
  }

  #results.show { display: block; }

  .results-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 32px;
  }

  .results-title {
    font-family: 'Syne', sans-serif;
    font-size: 1.4rem;
    font-weight: 800;
  }

  .reset-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--muted);
    font-family: 'Syne', sans-serif;
    font-size: 0.8rem;
    font-weight: 600;
    padding: 8px 18px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .reset-btn:hover { border-color: rgba(255,255,255,0.15); color: var(--text); }

  /* Score cards */
  .score-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }

  .score-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 20px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }

  .score-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
  }

  .score-card.overall::before { background: linear-gradient(90deg, var(--accent), var(--accent2)); }
  .score-card.ats::before { background: var(--accent2); }
  .score-card.impact::before { background: var(--accent3); }
  .score-card.match::before { background: var(--warning); }

  .score-number {
    font-family: 'Syne', sans-serif;
    font-size: 2.2rem;
    font-weight: 800;
    line-height: 1;
    margin-bottom: 6px;
  }

  .score-card.overall .score-number { color: var(--accent); }
  .score-card.ats .score-number { color: var(--accent2); }
  .score-card.impact .score-number { color: var(--accent3); }
  .score-card.match .score-number { color: var(--warning); }

  .score-label { font-size: 0.72rem; color: var(--muted); font-weight: 500; }

  /* Main results grid */
  .results-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 20px;
  }

  .result-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 24px;
  }

  .result-card-title {
    font-family: 'Syne', sans-serif;
    font-size: 0.78rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 16px;
    display: flex; align-items: center; gap: 8px;
  }

  .result-card-title .icon {
    width: 22px; height: 22px;
    border-radius: 6px;
    display: flex; align-items: center; justify-content: center;
    font-size: 11px;
  }

  /* Strength / weakness items */
  .item-list { display: flex; flex-direction: column; gap: 10px; }

  .item {
    display: flex; align-items: flex-start; gap: 12px;
    font-size: 0.85rem;
    line-height: 1.5;
    padding: 10px 14px;
    border-radius: 10px;
  }

  .item.strength {
    background: rgba(52,211,153,0.06);
    border: 1px solid rgba(52,211,153,0.12);
    color: #a7f3d0;
  }

  .item.weakness {
    background: rgba(248,113,113,0.06);
    border: 1px solid rgba(248,113,113,0.12);
    color: #fca5a5;
  }

  .item.suggestion {
    background: rgba(129,140,248,0.06);
    border: 1px solid rgba(129,140,248,0.12);
    color: #c7d2fe;
  }

  .item-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    flex-shrink: 0;
    margin-top: 6px;
  }

  .strength .item-dot { background: var(--success); }
  .weakness .item-dot { background: var(--danger); }
  .suggestion .item-dot { background: var(--accent2); }

  /* Keywords */
  .keyword-grid { display: flex; flex-wrap: wrap; gap: 8px; }

  .keyword {
    font-size: 0.78rem;
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    padding: 5px 12px;
    border-radius: 20px;
  }

  .keyword.found {
    background: rgba(52,211,153,0.1);
    border: 1px solid rgba(52,211,153,0.25);
    color: var(--success);
  }

  .keyword.missing {
    background: rgba(248,113,113,0.1);
    border: 1px solid rgba(248,113,113,0.25);
    color: var(--danger);
  }

  /* Summary card */
  .summary-card {
    background: linear-gradient(135deg, rgba(110,231,183,0.05), rgba(129,140,248,0.05));
    border: 1px solid rgba(110,231,183,0.15);
    border-radius: 16px;
    padding: 28px;
    margin-bottom: 20px;
  }

  .summary-text {
    font-size: 0.95rem;
    line-height: 1.8;
    color: #c8c8d8;
  }

  /* Error */
  .error-box {
    display: none;
    background: rgba(248,113,113,0.08);
    border: 1px solid rgba(248,113,113,0.2);
    border-radius: 14px;
    padding: 20px 24px;
    color: var(--danger);
    font-size: 0.88rem;
    line-height: 1.6;
    margin-top: 16px;
  }

  .error-box.show { display: block; }

  /* Animations */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  @keyframes pulse {
    0%, 100% { opacity: 0.3; transform: scale(0.9); }
    50% { opacity: 1; transform: scale(1.1); }
  }

  /* Responsive */
  @media (max-width: 768px) {
    header { padding: 20px 24px; }
    main { padding: 40px 24px; }
    .input-grid { grid-template-columns: 1fr; }
    .score-grid { grid-template-columns: repeat(2, 1fr); }
    .results-grid { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
<div class="blob blob-1"></div>
<div class="blob blob-2"></div>

<header>
  <div class="logo">
    <div class="logo-icon">✦</div>
    Resume<span>AI</span>
  </div>
  <div class="badge">Powered by Claude</div>
</header>

<main>
  <!-- Hero -->
  <div class="hero">
    <div class="hero-tag">AI-Powered Analysis</div>
    <h1>Analyze Your Resume<br>with <em>Precision</em></h1>
    <p>Get an instant, deep analysis of your resume — ATS score, keyword gaps, strength evaluation, and actionable improvements.</p>
  </div>

  <!-- Input form -->
  <div id="inputSection">
    <div class="input-grid">
      <!-- Resume paste -->
      <div class="card">
        <div class="card-label"><span>📄</span> Your Resume</div>
        <textarea id="resumeText" placeholder="Paste your full resume text here...&#10;&#10;Include all sections: contact info, summary, experience, education, skills, etc." style="height:200px;"></textarea>
      </div>

      <!-- Job description -->
      <div class="card">
        <div class="card-label"><span>💼</span> Job Description (Optional)</div>
        <textarea id="jobDesc" placeholder="Paste the job posting here...&#10;&#10;Adding a job description enables keyword matching, ATS optimization, and tailored suggestions." style="height:200px;"></textarea>
      </div>
    </div>

    <!-- Analysis options -->
    <div class="options-row">
      <label class="option-chip active" id="chip-ats">
        <input type="checkbox" checked> 🤖 ATS Score
      </label>
      <label class="option-chip active" id="chip-keywords">
        <input type="checkbox" checked> 🔑 Keyword Analysis
      </label>
      <label class="option-chip active" id="chip-impact">
        <input type="checkbox" checked> ⚡ Impact Metrics
      </label>
      <label class="option-chip active" id="chip-suggestions">
        <input type="checkbox" checked> 💡 Improvements
      </label>
    </div>

    <button class="analyze-btn" onclick="analyzeResume()">
      <span>✦</span> Analyze My Resume
    </button>

    <div class="error-box" id="errorBox"></div>
  </div>

  <!-- Loading -->
  <div class="loading" id="loadingSection">
    <div class="spinner"></div>
    <div class="loading-text">Analyzing your resume with AI...</div>
    <div class="loading-steps">
      <div class="loading-step"><div class="dot"></div>Parsing</div>
      <div class="loading-step"><div class="dot"></div>Scoring</div>
      <div class="loading-step"><div class="dot"></div>Keywords</div>
      <div class="loading-step"><div class="dot"></div>Insights</div>
    </div>
  </div>

  <!-- Results -->
  <div id="results">
    <div class="results-header">
      <div class="results-title">Analysis Complete ✦</div>
      <button class="reset-btn" onclick="resetApp()">← Analyze Another</button>
    </div>

    <!-- Score cards -->
    <div class="score-grid" id="scoreGrid"></div>

    <!-- Summary -->
    <div class="summary-card" id="summaryCard"></div>

    <!-- Main results -->
    <div class="results-grid">
      <div class="result-card" id="strengthsCard"></div>
      <div class="result-card" id="weaknessesCard"></div>
      <div class="result-card" id="suggestionsCard"></div>
      <div class="result-card" id="keywordsCard"></div>
    </div>
  </div>
</main>

<script>
// ───────────────────────────────────────────────
// SYSTEM PROMPT — The heart of the AI analyzer
// ───────────────────────────────────────────────
function buildSystemPrompt() {
  return `You are ResumeAI, an expert resume analyst and career coach with deep knowledge of:
- ATS (Applicant Tracking System) optimization
- Hiring manager psychology and recruiter behavior
- Industry-specific resume best practices
- Keyword optimization and job matching
- Quantifiable impact and achievement framing
- Modern resume formatting standards

Your task is to perform a comprehensive, honest, and actionable resume analysis.

ANALYSIS FRAMEWORK:
1. Overall Score (0–100): Holistic quality considering all dimensions
2. ATS Score (0–100): How well the resume will pass automated screening
3. Impact Score (0–100): Strength of achievements, use of metrics, action verbs
4. Job Match Score (0–100): Relevance to the provided job description (or general marketability if no JD provided)

EVALUATION CRITERIA:
- Contact information completeness
- Professional summary effectiveness
- Work experience: action verbs, quantified achievements, relevance
- Skills section: technical and soft skills balance
- Education section: relevance and completeness
- Formatting: consistency, readability, length appropriateness
- ATS compatibility: keyword density, standard section headers, no tables/graphics
- Grammar and spelling
- Tailoring: specificity vs. generic statements

RESPONSE FORMAT — Return ONLY a valid JSON object with no markdown, no preamble, no explanation outside JSON:
{
  "scores": {
    "overall": <number 0-100>,
    "ats": <number 0-100>,
    "impact": <number 0-100>,
    "jobMatch": <number 0-100>
  },
  "summary": "<2-3 sentence executive summary of the resume quality and key recommendation>",
  "strengths": [
    "<specific strength 1>",
    "<specific strength 2>",
    "<specific strength 3>"
  ],
  "weaknesses": [
    "<specific weakness 1>",
    "<specific weakness 2>",
    "<specific weakness 3>"
  ],
  "suggestions": [
    "<concrete, actionable improvement 1>",
    "<concrete, actionable improvement 2>",
    "<concrete, actionable improvement 3>",
    "<concrete, actionable improvement 4>"
  ],
  "keywordsFound": ["<keyword1>", "<keyword2>", "<keyword3>"],
  "keywordsMissing": ["<missing1>", "<missing2>", "<missing3>"]
}

IMPORTANT RULES:
- Be specific and honest — vague feedback is useless
- Reference actual content from the resume when possible
- Missing keywords should be relevant to the role/industry
- Scores should reflect reality — don't inflate artificially
- Return ONLY JSON — no text before or after`;
}

// ───────────────────────────────────────────────
// USER PROMPT — Dynamic, context-aware
// ───────────────────────────────────────────────
function buildUserPrompt(resumeText, jobDesc) {
  let prompt = `Please analyze the following resume:\n\n`;
  prompt += `=== RESUME ===\n${resumeText}\n\n`;

  if (jobDesc && jobDesc.trim().length > 20) {
    prompt += `=== TARGET JOB DESCRIPTION ===\n${jobDesc}\n\n`;
    prompt += `Compare the resume against this job description for keyword matching and relevance scoring.`;
  } else {
    prompt += `No specific job description provided. Evaluate for general professional marketability and ATS compatibility.`;
  }

  return prompt;
}

// ───────────────────────────────────────────────
// MAIN ANALYSIS FUNCTION
// ───────────────────────────────────────────────
async function analyzeResume() {
  const resumeText = document.getElementById('resumeText').value.trim();
  const jobDesc = document.getElementById('jobDesc').value.trim();
  const errorBox = document.getElementById('errorBox');

  errorBox.classList.remove('show');

  if (!resumeText || resumeText.length < 50) {
    errorBox.textContent = '⚠ Please paste your resume text (minimum 50 characters) to begin analysis.';
    errorBox.classList.add('show');
    return;
  }

  // Show loading, hide form
  document.getElementById('inputSection').style.display = 'none';
  document.getElementById('loadingSection').classList.add('show');

  // Animate loading steps
  const steps = document.querySelectorAll('.loading-step .dot');
  let currentStep = 0;
  const stepInterval = setInterval(() => {
    steps.forEach((d, i) => {
      d.style.background = i <= currentStep ? 'var(--accent)' : 'var(--surface2)';
      d.style.opacity = i <= currentStep ? '1' : '0.3';
    });
    currentStep = (currentStep + 1) % (steps.length + 1);
  }, 600);

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: buildSystemPrompt(),
        messages: [{ role: 'user', content: buildUserPrompt(resumeText, jobDesc) }]
      })
    });

    clearInterval(stepInterval);

    if (!response.ok) {
      const err = await response.json();
      throw new Error(err.error?.message || 'API request failed');
    }

    const data = await response.json();
    const raw = data.content.map(b => b.text || '').join('');
    const clean = raw.replace(/```json|```/g, '').trim();
    const result = JSON.parse(clean);

    renderResults(result);

  } catch (err) {
    clearInterval(stepInterval);
    document.getElementById('loadingSection').classList.remove('show');
    document.getElementById('inputSection').style.display = 'block';
    errorBox.textContent = `⚠ Analysis failed: ${err.message}. Please check your input and try again.`;
    errorBox.classList.add('show');
  }
}

// ───────────────────────────────────────────────
// RENDER RESULTS
// ───────────────────────────────────────────────
function renderResults(data) {
  document.getElementById('loadingSection').classList.remove('show');

  // Scores
  const { overall, ats, impact, jobMatch } = data.scores;
  document.getElementById('scoreGrid').innerHTML = `
    <div class="score-card overall">
      <div class="score-number">${overall}</div>
      <div class="score-label">Overall Score</div>
    </div>
    <div class="score-card ats">
      <div class="score-number">${ats}</div>
      <div class="score-label">ATS Score</div>
    </div>
    <div class="score-card impact">
      <div class="score-number">${impact}</div>
      <div class="score-label">Impact Score</div>
    </div>
    <div class="score-card match">
      <div class="score-number">${jobMatch}</div>
      <div class="score-label">Job Match</div>
    </div>
  `;

  // Summary
  document.getElementById('summaryCard').innerHTML = `
    <div class="result-card-title" style="margin-bottom:12px;"><span class="icon" style="background:rgba(110,231,183,0.1)">✦</span> Executive Summary</div>
    <div class="summary-text">${data.summary}</div>
  `;

  // Strengths
  document.getElementById('strengthsCard').innerHTML = `
    <div class="result-card-title"><span class="icon" style="background:rgba(52,211,153,0.1)">✓</span> Strengths</div>
    <div class="item-list">
      ${data.strengths.map(s => `<div class="item strength"><div class="item-dot"></div><span>${s}</span></div>`).join('')}
    </div>
  `;

  // Weaknesses
  document.getElementById('weaknessesCard').innerHTML = `
    <div class="result-card-title"><span class="icon" style="background:rgba(248,113,113,0.1)">✗</span> Areas to Improve</div>
    <div class="item-list">
      ${data.weaknesses.map(w => `<div class="item weakness"><div class="item-dot"></div><span>${w}</span></div>`).join('')}
    </div>
  `;

  // Suggestions
  document.getElementById('suggestionsCard').innerHTML = `
    <div class="result-card-title"><span class="icon" style="background:rgba(129,140,248,0.1)">→</span> Action Items</div>
    <div class="item-list">
      ${data.suggestions.map(s => `<div class="item suggestion"><div class="item-dot"></div><span>${s}</span></div>`).join('')}
    </div>
  `;

  // Keywords
  const foundKW = (data.keywordsFound || []).map(k => `<span class="keyword found">✓ ${k}</span>`).join('');
  const missingKW = (data.keywordsMissing || []).map(k => `<span class="keyword missing">✗ ${k}</span>`).join('');
  document.getElementById('keywordsCard').innerHTML = `
    <div class="result-card-title"><span class="icon" style="background:rgba(251,191,36,0.1)">🔑</span> Keywords</div>
    <div style="margin-bottom:10px; font-size:0.75rem; color:var(--muted); font-family:'Syne',sans-serif; font-weight:600; letter-spacing:0.06em; text-transform:uppercase;">Found</div>
    <div class="keyword-grid" style="margin-bottom:16px;">${foundKW || '<span style="font-size:0.82rem;color:var(--muted)">None detected</span>'}</div>
    <div style="margin-bottom:10px; font-size:0.75rem; color:var(--muted); font-family:'Syne',sans-serif; font-weight:600; letter-spacing:0.06em; text-transform:uppercase;">Missing / Add These</div>
    <div class="keyword-grid">${missingKW || '<span style="font-size:0.82rem;color:var(--muted)">None identified</span>'}</div>
  `;

  document.getElementById('results').classList.add('show');
  document.getElementById('results').scrollIntoView({ behavior: 'smooth', block: 'start' });
}

// ───────────────────────────────────────────────
// RESET
// ───────────────────────────────────────────────
function resetApp() {
  document.getElementById('results').classList.remove('show');
  document.getElementById('inputSection').style.display = 'block';
  document.getElementById('resumeText').value = '';
  document.getElementById('jobDesc').value = '';
  document.getElementById('errorBox').classList.remove('show');
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Option chips toggle
document.querySelectorAll('.option-chip').forEach(chip => {
  chip.addEventListener('click', () => chip.classList.toggle('active'));
});
</script>
</body>
</html>
