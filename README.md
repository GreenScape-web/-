<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مترجم Google API</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f1f3f4; margin:0; padding:0; display:flex; justify-content:center; align-items:flex-start; min-height:100vh; }
    .translator-container { margin-top:50px; background:#fff; width:90%; max-width:900px; border-radius:12px; box-shadow:0 4px 20px rgba(0,0,0,0.1); padding:25px; }
    h1 { text-align:center; color:#202124; font-weight:normal; margin-bottom:20px; }
    textarea { width:100%; height:140px; padding:15px; font-size:16px; border:1px solid #dadce0; border-radius:8px; resize:vertical; box-sizing:border-box; }
    .controls { display:flex; justify-content:space-between; align-items:center; margin:15px 0; flex-wrap:wrap; gap:10px; }
    select, button { padding:10px 15px; font-size:16px; border-radius:8px; border:1px solid #dadce0; cursor:pointer; }
    button { background:#1a73e8; color:#fff; border:none; transition:0.3s; }
    button:hover { background:#155ab6; }
    .swap-btn { background:#fbbc05; color:#202124; }
    .swap-btn:hover { background:#e6a700; }
    #outputText { width:100%; min-height:140px; padding:15px; border-radius:8px; border:1px solid #dadce0; background:#f8f9fa; margin-top:15px; text-align:left; font-size:16px; box-sizing:border-box; white-space:pre-wrap; }
    .btn-copy { background:#34a853; }
    .btn-copy:hover { background:#2c8c45; }
    @media (max-width:600px){ .controls{ flex-direction:column; align-items:stretch; } }
  </style>
</head>
<body>
  <div class="translator-container">
    <h1>🌐 مترجم Google API</h1>
    <textarea id="inputText" placeholder="اكتب النص هنا..."></textarea>

    <div class="controls">
      <select id="sourceLang">
        <option value="auto">كشف تلقائي</option>
        <option value="ar">العربية</option>
        <option value="en">الإنجليزية</option>
        <option value="fr">الفرنسية</option>
        <option value="de">الألمانية</option>
        <option value="es">الإسبانية</option>
      </select>

      <button class="swap-btn" onclick="swapLanguages()">🔄 تبديل اللغات</button>

      <select id="targetLang">
        <option value="en">الإنجليزية</option>
        <option value="ar">العربية</option>
        <option value="fr">الفرنسية</option>
        <option value="de">الألمانية</option>
        <option value="es">الإسبانية</option>
      </select>
    </div>

    <div class="controls">
      <button onclick="translateText()">ترجم الآن</button>
      <button class="btn-copy" onclick="copyTranslation()">نسخ الترجمة</button>
    </div>

    <div id="outputText">🔎 الترجمة ستظهر هنا...</div>
  </div>

  <script>
    const API_KEY = "AIzaSyDVW5lyGxvmgYIZQwudyF6LUfnYvJXaiKE";

    async function translateText() {
      const text = document.getElementById("inputText").value;
      const source = document.getElementById("sourceLang").value;
      const target = document.getElementById("targetLang").value;
      const outputDiv = document.getElementById("outputText");

      if(!text.trim()){ outputDiv.textContent="⚠️ الرجاء إدخال نص للترجمة."; return; }
      outputDiv.textContent="⏳ جاري الترجمة...";

      try{
        const url = `https://translation.googleapis.com/language/translate/v2?key=${API_KEY}`;
        const response = await fetch(url, {
          method:"POST",
          headers:{ "Content-Type":"application/json" },
          body: JSON.stringify({ q: text, source: source, target: target, format:"text" })
        });

        const data = await response.json();
        if(data.data && data.data.translations && data.data.translations[0]){
          outputDiv.textContent = data.data.translations[0].translatedText;
          outputDiv.style.direction = (target==="ar")?"rtl":"ltr";
        } else {
          outputDiv.textContent="❌ لم يتم الحصول على ترجمة.";
          console.log(data);
        }
      }catch(err){
        outputDiv.textContent="🚨 خطأ في الاتصال بـ Google API";
        console.error(err);
      }
    }

    function swapLanguages(){
      const sourceSelect = document.getElementById("sourceLang");
      const targetSelect = document.getElementById("targetLang");
      [sourceSelect.value, targetSelect.value] = [targetSelect.value, sourceSelect.value];

      const outputDiv = document.getElementById("outputText");
      const inputText = document.getElementById("inputText");
      if(outputDiv.textContent && outputDiv.textContent !== "🔎 الترجمة ستظهر هنا..."){
        inputText.value = outputDiv.textContent;
        outputDiv.textContent = "🔎 الترجمة ستظهر هنا...";
      }
    }

    function copyTranslation(){
      const outputDiv = document.getElementById("outputText");
      if(outputDiv.textContent && outputDiv.textContent !== "🔎 الترجمة ستظهر هنا..."){
        navigator.clipboard.writeText(outputDiv.textContent)
          .then(()=>alert("تم نسخ الترجمة! ✅"))
          .catch(err=>alert("خطأ عند النسخ: "+err));
      }
    }
  </script>
</body>
</html>
