<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>醫學文獻簡報製作App</title>
  <style>
    :root{
      --bg:#ffffff; /* white background */
      --card:#0ea5a4; /* teal */
      --accent1:#ff6b6b; /* coral */
      --accent2:#ffd166; /* yellow */
      --glass: rgba(0,0,0,0.06);
      --text:#000000;
      --footer-bg: #062B5FCC; /* Green background for footer */
      --footer-text-color: #FFFFFF; /* blue text for footer */
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial; background:var(--bg); color:var(--text);
    }
    .wrap{max-width:920px;margin:32px auto;padding:28px;border-radius:18px;background:#f9fafb; box-shadow: 0 10px 30px rgba(0,0,0,0.1);}
    
    header{
      display:flex;align-items:center;justify-content:center;
      padding:24px 24px; 
      border-radius:14px;
      background:linear-gradient(90deg,#062B5FCC); 
      margin-bottom: 30px; 
      text-align: center; 
      position: relative;
      overflow: hidden;
      color: #ffffff; /* 文字改為白色 */
    }
    
    header::after{
      content:'W.Doctor';
      position:absolute;
      inset:0;
      display:flex;
      align-items:center;
      justify-content:center;
      font-size:72px;
      font-weight:900;
      color:rgba(255,255,255,0.1);
      letter-spacing:2px;
      pointer-events:none;
      z-index:0;
      transform: rotate(-8deg);
    }
    
    .header-content {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        position: relative;
        z-index: 1;
    }
    
    h1{
        font-size:30px;
        margin:0;
        color:inherit; /* 繼承 header 的白色 */
        font-weight: 800;
    } 
    
    p.lead{
      margin: 8px 0 0;
      color:#333;
      font-size:17px;
      line-height: 1.6;
    } 

    .controls{display:flex;gap:14px;flex-wrap:wrap;margin-bottom:28px;}
    label{font-weight:700;font-size:16px}
    select, button, textarea{font-size:16px;padding:12px 14px;border-radius:10px;border:1px solid #ccc;outline:none;transition: border-color 0.3s;}
    select{min-width:160px;background:#fff;color:#000}
    select:focus, textarea:focus {border-color: var(--card);}

    .btn-primary{background:linear-gradient(90deg,var(--card),#06b6d4);color:#fff;cursor:pointer;border: none;font-weight: 600;}
    .btn-ghost{background:transparent;border:2px solid #ccc;color:#000;cursor:pointer;font-weight: 600;}
    .btn-primary:hover{opacity: 0.9;}
    .btn-ghost:hover{border-color: #aaa;}

    .prompt-box{margin-top:12px;background:#fff;padding:24px;border-radius:12px;border:1px solid #ccc}
    textarea.prompt{width:100%;min-height:220px;font-size:18px;background:transparent;color:#000;resize:vertical;border: none;padding: 0;}

    .small{font-size:13px;color:#555}
    
    footer{
        display:flex;
        align-items:center;
        justify-content:center; 
        margin:32px -28px -28px; 
        padding: 16px 28px; 
        background: var(--footer-bg); 
        border-bottom-left-radius: 18px;
        border-bottom-right-radius: 18px;
    } 
    
    .footer-text{
        font-size:14px;
        font-weight:600;
        color: var(--footer-text-color);
        text-align: center;
    } 

    @media (max-width:720px){
      .controls{flex-direction:column}
      .controls-right{margin-left:0;width:100%;justify-content:space-between;display: flex; gap: 14px;}
      header{padding:16px;}
      footer{margin:32px -28px -28px;}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="header-content">
        <h1>醫學文獻簡報製作App</h1>
      </div>
    </header>

    <div class="controls">
      <div style="display:flex;flex-direction:column;gap:8px">
        <label for="template">文獻種類 / Literature Type</label>
        <select id="template">
          <option value="original">Original Article</option>
          <option value="systematic">Systematic Review and Meta-Analysis</option></select>
      </div>

      <div style="display:flex;flex-direction:column;gap:8px">
        <label for="lang">Language / 語言</label>
        <select id="lang">
          <option value="English">English</option>
          <option value="中文">中文</option>
        </select>
      </div>

      <div style="display:flex;flex-direction:column;gap:8px">
        <label for="slides">Number of Slides / 張數</label>
        <select id="slides">
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="30">30</option>
        </select>
      </div>

      <div class="controls-right" style="align-self: flex-end; gap: 14px;">
        <button class="btn-primary" id="genBtn">Generate Prompt</button>
        <button class="btn-ghost" id="clearBtn">Clear</button>
      </div>
    </div>

    <div class="prompt-box">
      <div style="display:flex;align-items:center;justify-content:space-between;gap:12px; margin-bottom: 12px;">
        <div>
          <div style="font-weight:800;font-size:18px">Generated Prompt</div>
          <div class="small">Automatically inserts chosen language and slide count</div>
        </div>
      </div>

      <textarea id="promptArea" class="prompt" readonly></textarea>
      
      <div style="margin-top: 16px; display: flex; justify-content: flex-end;">
        <button id="copyBtn" class="btn-primary">Copy Prompt</button>
      </div>
    </div>

    <footer>
      <div class="footer-text">請將產生的咒語，按下複製按鍵，貼到ChatGPT, gemini or DeepSeek，同時上傳文獻PDF檔案，A I自動生成簡報標題和內容，使用前請先專業評估和確認數據</div>
      </footer>

      <footer>
        <div class="footer-text">© 2025 W.Doctor — 版權所有。<a href="#" style="color: #FFFFFF; text-decoration: underline; margin-left:6px;">著作權保護</a>｜<a href="#" style="color: #FFFFFF; text-decoration: underline; margin-left:6px;">隱私權條款</a></div>
    </footer>
  </div>

  <script>
    const langEl = document.getElementById('lang');
    const slidesEl = document.getElementById('slides');
    const genBtn = document.getElementById('genBtn');
    const promptArea = document.getElementById('promptArea');
    const copyBtn = document.getElementById('copyBtn');
    const clearBtn = document.getElementById('clearBtn');
    const templateEl = document.getElementById('template');

    function makePrompt(language, slides, template){
      const header = `${language === '中文' ? '語言: 中文' : 'Language: English'}, Slides: ${slides}.`;
      // PPT instruction is now only for article templates
      const ppt_instruction = "Based on the uploaded PDF file, produce a ready-to-present PPT file.";

      if(template === 'original'){
        // 修改：在 original article 提示前加上 "You are a medical expert."
        const original_prompt = `You are a medical expert. Generate a concise, slide-style presentation of the following medical article with sections: (1) Basic Info, (2) Background & Objective, (3) Methods, (4) Results with key statistics, (5) Conclusion, (6) Critical Appraisal (strengths, limitations, evidence level), (7) Clinical Implications, and (8) Discussion Questions. Use clear bullet points (3–5 per slide), professional and presentation-ready.`;
        return `${header}\n\n${ppt_instruction}\n\n${original_prompt}`;
      } else if(template === 'systematic'){
        // 修改：將 "I am" 改成 "You are"
        const systematic_prompt = `You are a medical expert presenting a systematic review/meta-analysis. Create a concise presentation outline focusing on the PICO framework, search methodology, risk of bias, forest plot interpretation, and GRADE assessment. Structure it for an evidence-based summary for peers.`;
        return `${header}\n\n${ppt_instruction}\n\n${systematic_prompt}`;
      } else {
        // Fallback: default to original template to avoid unsupported options
        const original_prompt = `You are a medical expert. Generate a concise, slide-style presentation of the following medical article with sections: (1) Basic Info, (2) Background & Objective, (3) Methods, (4) Results with key statistics, (5) Conclusion, (6) Critical Appraisal (strengths, limitations, evidence level), (7) Clinical Implications, and (8) Discussion Questions. Use clear bullet points (3–5 per slide), professional and presentation-ready.`;
        return `${header}\n\n${ppt_instruction}\n\n${original_prompt}`;
      }
    }

    genBtn.addEventListener('click', ()=>{
      const p = makePrompt(langEl.value, slidesEl.value, templateEl.value);
      promptArea.value = p;
      promptArea.focus();
      promptArea.select();
    });

    copyBtn.addEventListener('click', async ()=>{
      try{
        await navigator.clipboard.writeText(promptArea.value);
        copyBtn.textContent = '已複製!';
        setTimeout(()=> copyBtn.textContent = 'Copy Prompt', 1600);
      }catch(e){
        alert('複製失敗 — 請手動選取並複製。');
      }
    });

    clearBtn.addEventListener('click', ()=>{ promptArea.value = ''; });

    window.addEventListener('load', ()=>{ genBtn.click(); });
  </script>
</body>
</html>
