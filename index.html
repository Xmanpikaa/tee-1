import React, { useState, useRef, useCallback } from "react";

/* ============================================================
   TeePublic USA POD SEO AI Elite
   ------------------------------------------------------------
   Token-efficient architecture (image sent ONCE):
     Call 1  [vision] : Design Analyzer + Niche Classifier
     Call 2  [text]   : Keyword Engine + Listing + Market Scoring
   No SERP / no scraping / no external APIs. Image + reasoning only.
   ============================================================ */

const MODEL = "claude-sonnet-4-6";
const ACCEPT = ["image/png", "image/jpeg", "image/webp"];

/* ---------- API layer ---------- */
async function callClaude(messages, maxTokens = 1000) {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ model: MODEL, max_tokens: maxTokens, messages }),
  });
  if (!res.ok) throw new Error(`Service returned ${res.status}. Try again.`);
  const data = await res.json();
  return (data.content || [])
    .map((b) => (b.type === "text" ? b.text : ""))
    .filter(Boolean)
    .join("\n");
}

function parseJSON(raw) {
  const clean = raw.replace(/```json/gi, "").replace(/```/g, "").trim();
  const s = clean.indexOf("{");
  const e = clean.lastIndexOf("}");
  if (s === -1 || e === -1) throw new Error("Could not read the AI response.");
  return JSON.parse(clean.slice(s, e + 1));
}

function fileToBase64(file) {
  return new Promise((resolve, reject) => {
    const r = new FileReader();
    r.onload = () => resolve(r.result.split(",")[1]);
    r.onerror = () => reject(new Error("Could not read the image file."));
    r.readAsDataURL(file);
  });
}

/* ---------- Prompt builders (modular, compact) ---------- */
const visionPrompt = (ctx) =>
  `You are an elite Print-on-Demand analyst for the TeePublic USA market. Analyze this POD design image. Return ONLY minified JSON, no markdown, exact schema:
{"design":{"text":"exact text/quote in design or \\"\\"","typography":"","elements":"","palette":["max 5 hex or color names"],"style":"","emotion":"","gift":""},"niche":{"primary":"","secondary":"","micro":"","audience":""}}
Rules: be specific, concise. "style" = one of vintage distressed, retro sunset, bold typography, minimalist, grunge, cartoon, patriotic, kawaii, or the closest match.${ctx ? " User context: " + ctx : ""}`;

const listingPrompt = (analysis, ctx) =>
  `You are an elite TeePublic USA SEO strategist. Using this design analysis, build one optimized listing. Return ONLY minified JSON, no markdown, exact schema:
{"keywords":{"primary":"","secondary":["4 items"],"longtail":["4 items"],"evergreen":["3 items"],"gift":["3 items"]},"listing":{"title":"","description":"","tags":["exactly 15"]},"scores":{"demand":0,"competition":0,"evergreen":0,"trend":0,"potential":0}}
Rules: USA buyer intent, TeePublic search behavior, natural keywords (no stuffing), conversion-first. Title = 60-80 chars, strongest keyword near the front, human readable. Description = 3-4 persuasive sentences including niche, gift angle, emotional hooks. Tags = 15 natural buyer-intent phrases. Scores 0-10; "competition" higher = MORE saturated. Analysis: ${JSON.stringify(
    analysis
  )}${ctx ? " Extra context: " + ctx : ""}`;

/* ---------- Small UI pieces ---------- */
function CopyBtn({ text, label = "Copy" }) {
  const [done, setDone] = useState(false);
  const copy = async () => {
    try {
      await navigator.clipboard.writeText(text);
      setDone(true);
      setTimeout(() => setDone(false), 1400);
    } catch {
      /* clipboard blocked */
    }
  };
  return (
    <button className={`copy ${done ? "copied" : ""}`} onClick={copy}>
      {done ? "Copied" : label}
    </button>
  );
}

function Meter({ name, value, invert = false }) {
  const v = Math.max(0, Math.min(10, Number(value) || 0));
  // invert = "high is bad" (competition)
  const good = invert ? v <= 4 : v >= 7;
  const mid = invert ? v <= 7 : v >= 4;
  const tone = good ? "hi" : mid ? "mid" : "lo";
  return (
    <div className="meter">
      <div className="meter-top">
        <span className="meter-name">{name}</span>
        <span className={`meter-val ${tone}`}>{v.toFixed(1)}</span>
      </div>
      <div className="meter-track">
        <div className={`meter-fill ${tone}`} style={{ width: `${v * 10}%` }} />
      </div>
    </div>
  );
}

function Field({ title, children }) {
  return (
    <div className="field">
      <span className="field-title">{title}</span>
      <div className="field-body">{children}</div>
    </div>
  );
}

function Chips({ items }) {
  return (
    <div className="chips">
      {(items || []).map((t, i) => (
        <span className="chip" key={i}>
          {t}
        </span>
      ))}
    </div>
  );
}

/* ============================================================
   App
   ============================================================ */
export default function App() {
  const [file, setFile] = useState(null);
  const [preview, setPreview] = useState("");
  const [ctx, setCtx] = useState("");
  const [phase, setPhase] = useState("idle"); // idle|analyzing|generating|done|error
  const [analysis, setAnalysis] = useState(null);
  const [result, setResult] = useState(null);
  const [err, setErr] = useState("");
  const inputRef = useRef(null);

  const busy = phase === "analyzing" || phase === "generating";

  const setImage = useCallback((f) => {
    if (!f) return;
    if (!ACCEPT.includes(f.type)) {
      setErr("Unsupported file. Upload a PNG, JPG, or WEBP.");
      return;
    }
    setErr("");
    setFile(f);
    setPreview(URL.createObjectURL(f));
    setResult(null);
    setAnalysis(null);
    setPhase("idle");
  }, []);

  const onDrop = (e) => {
    e.preventDefault();
    setImage(e.dataTransfer.files?.[0]);
  };

  const generate = async () => {
    if (!file) return;
    setErr("");
    setResult(null);
    setAnalysis(null);
    try {
      // ---- Call 1: vision (Design Analyzer + Niche Classifier) ----
      setPhase("analyzing");
      const b64 = await fileToBase64(file);
      const media = file.type === "image/jpeg" ? "image/jpeg" : file.type;
      const rawA = await callClaude([
        {
          role: "user",
          content: [
            { type: "image", source: { type: "base64", media_type: media, data: b64 } },
            { type: "text", text: visionPrompt(ctx.trim()) },
          ],
        },
      ]);
      const a = parseJSON(rawA);
      setAnalysis(a);

      // ---- Call 2: text (Keywords + Listing + Scoring) ----
      setPhase("generating");
      const rawL = await callClaude([
        { role: "user", content: listingPrompt(a, ctx.trim()) },
      ]);
      const l = parseJSON(rawL);
      setResult(l);
      setPhase("done");
    } catch (e) {
      setErr(e.message || "Something went wrong.");
      setPhase("error");
    }
  };

  const reset = () => {
    setFile(null);
    setPreview("");
    setResult(null);
    setAnalysis(null);
    setErr("");
    setPhase("idle");
    if (inputRef.current) inputRef.current.value = "";
  };

  const title = result?.listing?.title || "";
  const titleLen = title.length;
  const titleOk = titleLen >= 60 && titleLen <= 80;

  return (
    <div className="app">
      <style>{CSS}</style>

      {/* Header */}
      <header className="hero">
        <div className="hero-glow" />
        <div className="hero-inner">
          <div className="badge">USA MARKET · TEEPUBLIC</div>
          <h1>
            POD SEO <span className="grad">AI Elite</span>
          </h1>
          <p className="sub">
            Upload a design. Get a conversion-ready TeePublic listing — analysis, niche,
            keywords, copy, and market scores. No scraping, no search APIs.
          </p>
        </div>
      </header>

      <main className="grid">
        {/* LEFT: controls */}
        <section className="panel">
          <div className="panel-head">
            <span className="step-dot">1</span>
            <h2>Upload design</h2>
          </div>

          <div
            className={`drop ${preview ? "has" : ""} ${busy ? "locked" : ""}`}
            onClick={() => !busy && inputRef.current?.click()}
            onDragOver={(e) => e.preventDefault()}
            onDrop={onDrop}
          >
            <input
              ref={inputRef}
              type="file"
              accept="image/png,image/jpeg,image/webp"
              hidden
              onChange={(e) => setImage(e.target.files?.[0])}
            />
            {preview ? (
              <div className="preview-wrap">
                <img src={preview} alt="design preview" />
                {phase === "analyzing" && <div className="scan" />}
              </div>
            ) : (
              <div className="drop-empty">
                <div className="drop-ico">＋</div>
                <p>Drop or click to upload</p>
                <span>PNG · JPG · WEBP</span>
              </div>
            )}
          </div>

          <div className="panel-head mt">
            <span className="step-dot">2</span>
            <h2>
              Listing context <em>optional</em>
            </h2>
          </div>
          <textarea
            className="ctx"
            placeholder="Add hints: niche, occasion, audience, keywords to favor…"
            value={ctx}
            onChange={(e) => setCtx(e.target.value)}
            disabled={busy}
          />

          <button className="cta" onClick={generate} disabled={!file || busy}>
            {phase === "analyzing"
              ? "Analyzing design…"
              : phase === "generating"
              ? "Building listing…"
              : "Generate listing"}
          </button>

          {(result || file) && (
            <button className="ghost" onClick={reset} disabled={busy}>
              Reset
            </button>
          )}

          {err && <div className="error">{err}</div>}

          {busy && (
            <div className="steps">
              <span className={phase === "analyzing" ? "on" : "ok"}>
                Design + niche
              </span>
              <span className={phase === "generating" ? "on" : phase === "analyzing" ? "" : "ok"}>
                Keywords · listing · scores
              </span>
            </div>
          )}
        </section>

        {/* RIGHT: dashboard */}
        <section className="report">
          {!result && !busy && (
            <div className="empty">
              <div className="empty-ico">◪</div>
              <h3>Your listing report appears here</h3>
              <p>Upload a design and hit Generate to build the full breakdown.</p>
            </div>
          )}

          {busy && (
            <div className="empty">
              <div className="loader" />
              <h3>
                {phase === "analyzing"
                  ? "Reading the design DNA…"
                  : "Engineering the SEO listing…"}
              </h3>
              <p>This usually takes a few seconds.</p>
            </div>
          )}

          {result && (
            <div className="dash">
              {/* Headline score */}
              <div className="card headline">
                <div>
                  <span className="k">Sales potential</span>
                  <div className="big">
                    {(Number(result.scores?.potential) || 0).toFixed(1)}
                    <em>/10</em>
                  </div>
                </div>
                <div className="head-sub">
                  <Chips items={[analysis?.niche?.primary, analysis?.design?.style].filter(Boolean)} />
                </div>
              </div>

              {/* Listing */}
              <div className="card">
                <div className="card-head">
                  <h3>SEO title</h3>
                  <span className={`count ${titleOk ? "ok" : "warn"}`}>
                    {titleLen} chars
                  </span>
                </div>
                <p className="title-out">{title}</p>
                <CopyBtn text={title} label="Copy title" />

                <div className="divider" />

                <div className="card-head">
                  <h3>Description</h3>
                  <CopyBtn text={result.listing?.description || ""} />
                </div>
                <p className="desc-out">{result.listing?.description}</p>

                <div className="divider" />

                <div className="card-head">
                  <h3>Tags <span className="muted">· 15</span></h3>
                  <CopyBtn text={(result.listing?.tags || []).join(", ")} label="Copy all" />
                </div>
                <Chips items={result.listing?.tags} />
              </div>

              {/* Market analytics */}
              <div className="card">
                <div className="card-head">
                  <h3>Market analytics</h3>
                </div>
                <div className="meters">
                  <Meter name="Demand" value={result.scores?.demand} />
                  <Meter name="Competition" value={result.scores?.competition} invert />
                  <Meter name="Evergreen" value={result.scores?.evergreen} />
                  <Meter name="Trend" value={result.scores?.trend} />
                </div>
                <p className="hint">Competition inverted — lower bar means less saturated.</p>
              </div>

              {/* Keywords */}
              <div className="card">
                <div className="card-head">
                  <h3>Keyword engine</h3>
                </div>
                <Field title="Primary">
                  <span className="chip solid">{result.keywords?.primary}</span>
                </Field>
                <Field title="Secondary"><Chips items={result.keywords?.secondary} /></Field>
                <Field title="Long-tail"><Chips items={result.keywords?.longtail} /></Field>
                <Field title="Evergreen"><Chips items={result.keywords?.evergreen} /></Field>
                <Field title="Gift intent"><Chips items={result.keywords?.gift} /></Field>
              </div>

              {/* Design DNA + Niche */}
              <div className="card two">
                <div>
                  <div className="card-head"><h3>Design DNA</h3></div>
                  <Field title="Text / quote">{analysis?.design?.text || "—"}</Field>
                  <Field title="Style">{analysis?.design?.style}</Field>
                  <Field title="Typography">{analysis?.design?.typography}</Field>
                  <Field title="Elements">{analysis?.design?.elements}</Field>
                  <Field title="Emotion">{analysis?.design?.emotion}</Field>
                  <Field title="Gift intent">{analysis?.design?.gift}</Field>
                  <Field title="Palette">
                    <div className="swatches">
                      {(analysis?.design?.palette || []).map((c, i) => (
                        <span key={i} className="sw" title={c}
                          style={{ background: /^#|rgb|hsl/i.test(c) ? c : "transparent" }}>
                          {/^#|rgb|hsl/i.test(c) ? "" : c}
                        </span>
                      ))}
                    </div>
                  </Field>
                </div>
                <div>
                  <div className="card-head"><h3>Niche map</h3></div>
                  <Field title="Primary">{analysis?.niche?.primary}</Field>
                  <Field title="Secondary">{analysis?.niche?.secondary}</Field>
                  <Field title="Micro">{analysis?.niche?.micro}</Field>
                  <Field title="Audience">{analysis?.niche?.audience}</Field>
                </div>
              </div>
            </div>
          )}
        </section>
      </main>

      <footer className="foot">
        Image analysis + internal SEO reasoning · optimized for {MODEL}
      </footer>
    </div>
  );
}

/* ============================================================
   Styles — premium dark-blue SaaS dashboard
   ============================================================ */
const CSS = `
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@600&display=swap');

*{box-sizing:border-box}
.app{
  --bg:#070B16; --bg2:#0B1220;
  --card:#101B33; --card2:#16233F;
  --line:rgba(120,150,220,.12); --line2:rgba(120,150,220,.24);
  --text:#E8EDF7; --muted:#8A96B0;
  --accent:#4F7CFF; --cyan:#22D3EE; --violet:#8B7CFF;
  --hi:#3DD68C; --mid:#F5B14C; --lo:#F26D6D;
  --grad:linear-gradient(135deg,#4F7CFF 0%,#22D3EE 100%);
  min-height:100vh; background:
    radial-gradient(1200px 600px at 80% -10%, rgba(79,124,255,.14), transparent 60%),
    radial-gradient(900px 500px at -5% 10%, rgba(34,211,238,.08), transparent 55%),
    var(--bg);
  color:var(--text); font-family:Inter,system-ui,sans-serif;
  -webkit-font-smoothing:antialiased;
}
.grad{background:var(--grad);-webkit-background-clip:text;background-clip:text;color:transparent}

/* hero */
.hero{position:relative;overflow:hidden;padding:56px 24px 34px;border-bottom:1px solid var(--line)}
.hero-glow{position:absolute;inset:-40% 20% auto;height:340px;background:var(--grad);
  filter:blur(140px);opacity:.22;pointer-events:none}
.hero-inner{position:relative;max-width:1180px;margin:0 auto}
.badge{display:inline-block;font:600 11px/1 Inter;letter-spacing:.18em;color:var(--cyan);
  border:1px solid var(--line2);border-radius:999px;padding:7px 12px;margin-bottom:16px}
.hero h1{font:700 44px/1.02 Sora;margin:0;letter-spacing:-.02em}
.sub{max-width:620px;margin:14px 0 0;color:var(--muted);font-size:15px;line-height:1.6}

/* layout */
.grid{max-width:1180px;margin:28px auto;padding:0 24px;display:grid;
  grid-template-columns:380px 1fr;gap:22px;align-items:start}
@media(max-width:900px){.grid{grid-template-columns:1fr}}

/* panel */
.panel{background:linear-gradient(180deg,var(--card),var(--bg2));border:1px solid var(--line);
  border-radius:20px;padding:20px;position:sticky;top:20px}
@media(max-width:900px){.panel{position:static}}
.panel-head{display:flex;align-items:center;gap:10px;margin-bottom:12px}
.panel-head.mt{margin-top:22px}
.panel-head h2{font:600 15px Sora;margin:0}
.panel-head em{font-style:normal;color:var(--muted);font-size:12px;font-weight:500}
.step-dot{width:24px;height:24px;border-radius:8px;display:grid;place-items:center;
  font:600 12px JetBrains Mono;background:var(--card2);border:1px solid var(--line2);color:var(--cyan)}

.drop{border:1.5px dashed var(--line2);border-radius:16px;background:rgba(255,255,255,.015);
  cursor:pointer;transition:.2s;overflow:hidden}
.drop:hover{border-color:var(--accent);background:rgba(79,124,255,.05)}
.drop.locked{cursor:default;opacity:.9}
.drop-empty{padding:40px 16px;text-align:center;color:var(--muted)}
.drop-ico{width:46px;height:46px;margin:0 auto 12px;border-radius:12px;display:grid;place-items:center;
  font-size:24px;background:var(--card2);border:1px solid var(--line2);color:var(--accent)}
.drop-empty p{margin:0 0 4px;color:var(--text);font-weight:500}
.drop-empty span{font-size:12px}
.preview-wrap{position:relative;background:#0a0f1c}
.preview-wrap img{display:block;width:100%;max-height:300px;object-fit:contain}
.scan{position:absolute;left:0;right:0;height:3px;top:0;
  background:linear-gradient(90deg,transparent,var(--cyan),transparent);
  box-shadow:0 0 18px 3px rgba(34,211,238,.6);animation:scan 1.5s ease-in-out infinite}
@keyframes scan{0%{top:0}50%{top:calc(100% - 3px)}100%{top:0}}

.ctx{width:100%;min-height:82px;resize:vertical;background:var(--bg2);color:var(--text);
  border:1px solid var(--line);border-radius:12px;padding:12px;font:400 14px Inter;outline:none}
.ctx:focus{border-color:var(--accent)}
.ctx::placeholder{color:#5c6885}

.cta{width:100%;margin-top:18px;padding:14px;border:none;border-radius:12px;cursor:pointer;
  font:600 15px Sora;color:#05101f;background:var(--grad);letter-spacing:.01em;
  box-shadow:0 8px 24px -8px rgba(34,211,238,.5);transition:.15s}
.cta:hover:not(:disabled){transform:translateY(-1px);box-shadow:0 12px 30px -8px rgba(34,211,238,.6)}
.cta:disabled{opacity:.45;cursor:not-allowed;box-shadow:none}
.ghost{width:100%;margin-top:10px;padding:11px;border:1px solid var(--line2);border-radius:12px;
  background:transparent;color:var(--muted);font:500 14px Inter;cursor:pointer}
.ghost:hover:not(:disabled){color:var(--text);border-color:var(--accent)}

.error{margin-top:14px;padding:11px 13px;border-radius:10px;font-size:13px;
  background:rgba(242,109,109,.1);border:1px solid rgba(242,109,109,.35);color:#ffbcbc}
.steps{margin-top:16px;display:flex;flex-direction:column;gap:8px}
.steps span{font-size:12px;color:var(--muted);padding-left:20px;position:relative}
.steps span::before{content:"";position:absolute;left:0;top:3px;width:10px;height:10px;border-radius:50%;
  border:2px solid var(--line2)}
.steps .on{color:var(--text)}
.steps .on::before{border-color:var(--cyan);animation:pulse 1s infinite}
.steps .ok::before{border-color:var(--hi);background:var(--hi)}
@keyframes pulse{50%{box-shadow:0 0 0 4px rgba(34,211,238,.15)}}

/* report */
.report{min-height:420px}
.empty{background:linear-gradient(180deg,var(--card),var(--bg2));border:1px solid var(--line);
  border-radius:20px;padding:70px 24px;text-align:center;color:var(--muted)}
.empty-ico{font-size:40px;color:var(--accent);opacity:.5;margin-bottom:8px}
.empty h3{font:600 18px Sora;color:var(--text);margin:6px 0}
.empty p{margin:0;font-size:14px}
.loader{width:40px;height:40px;margin:0 auto 16px;border-radius:50%;
  border:3px solid var(--line2);border-top-color:var(--cyan);animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}

.dash{display:flex;flex-direction:column;gap:16px;animation:up .4s ease}
@keyframes up{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:none}}
.card{background:linear-gradient(180deg,var(--card),var(--bg2));border:1px solid var(--line);
  border-radius:18px;padding:20px}
.card-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;gap:10px}
.card-head h3{font:600 15px Sora;margin:0}
.card-head .muted{color:var(--muted);font-weight:500}
.divider{height:1px;background:var(--line);margin:18px 0}

.headline{display:flex;align-items:center;justify-content:space-between;gap:16px;
  border-color:var(--line2);background:
    radial-gradient(500px 200px at 100% 0,rgba(34,211,238,.12),transparent 60%),
    linear-gradient(180deg,var(--card),var(--bg2))}
.headline .k{font:600 12px Inter;letter-spacing:.12em;text-transform:uppercase;color:var(--muted)}
.big{font:600 46px JetBrains Mono;line-height:1;margin-top:6px;
  background:var(--grad);-webkit-background-clip:text;background-clip:text;color:transparent}
.big em{font-size:18px;color:var(--muted);font-style:normal;margin-left:4px}
.head-sub{max-width:50%}

.count{font:600 12px JetBrains Mono;padding:4px 9px;border-radius:8px}
.count.ok{color:var(--hi);background:rgba(61,214,140,.12)}
.count.warn{color:var(--mid);background:rgba(245,177,76,.12)}
.title-out{font:600 17px Sora;line-height:1.4;margin:0 0 12px}
.desc-out{color:#cdd6e8;font-size:14.5px;line-height:1.65;margin:0}

.copy{margin-top:2px;padding:7px 13px;border:1px solid var(--line2);border-radius:9px;
  background:transparent;color:var(--muted);font:500 12.5px Inter;cursor:pointer;transition:.15s}
.copy:hover{color:var(--text);border-color:var(--accent)}
.copy.copied{color:var(--hi);border-color:var(--hi)}

.chips{display:flex;flex-wrap:wrap;gap:8px}
.chip{font-size:13px;padding:6px 11px;border-radius:9px;background:var(--card2);
  border:1px solid var(--line);color:#d3dcef}
.chip.solid{background:rgba(79,124,255,.15);border-color:rgba(79,124,255,.4);color:#bcd0ff;font-weight:600}

.field{display:grid;grid-template-columns:110px 1fr;gap:12px;padding:9px 0;border-bottom:1px solid var(--line)}
.field:last-child{border-bottom:none}
.field-title{font:600 11px Inter;letter-spacing:.08em;text-transform:uppercase;color:var(--muted);padding-top:2px}
.field-body{font-size:14px;color:#dbe3f2;line-height:1.5}

.two{display:grid;grid-template-columns:1fr 1fr;gap:24px}
@media(max-width:620px){.two{grid-template-columns:1fr}.field{grid-template-columns:92px 1fr}}

.swatches{display:flex;flex-wrap:wrap;gap:6px}
.sw{min-width:26px;height:26px;border-radius:7px;border:1px solid var(--line2);
  display:grid;place-items:center;font-size:11px;color:#cdd6e8;padding:0 6px}

.meters{display:flex;flex-direction:column;gap:14px}
.meter-top{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px}
.meter-name{font-size:13px;color:#cdd6e8}
.meter-val{font:600 14px JetBrains Mono}
.meter-val.hi{color:var(--hi)} .meter-val.mid{color:var(--mid)} .meter-val.lo{color:var(--lo)}
.meter-track{height:8px;border-radius:6px;background:var(--bg2);overflow:hidden;border:1px solid var(--line)}
.meter-fill{height:100%;border-radius:6px;transition:width .6s cubic-bezier(.2,.8,.2,1)}
.meter-fill.hi{background:linear-gradient(90deg,#2fae74,#3DD68C)}
.meter-fill.mid{background:linear-gradient(90deg,#d89a3c,#F5B14C)}
.meter-fill.lo{background:linear-gradient(90deg,#d95757,#F26D6D)}
.hint{margin:12px 0 0;font-size:12px;color:var(--muted)}

.foot{text-align:center;padding:26px;color:#5c6885;font-size:12.5px;border-top:1px solid var(--line);margin-top:20px}
`;
