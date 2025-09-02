<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>مترجم شبيه بجوجل</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      margin: 0;
      padding: 30px;
      text-align: center;
    }
    h1 {
      color: #333;
    }
    textarea {
      width: 90%;
      height: 120px;
      padding: 12px;
      margin: 10px 0;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      resize: vertical;
    }
    select, button {
      padding: 10px 15px;
      font-size: 16px;
      margin: 5px;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
    button {
      background: #4285F4;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background: #357ae8;
    }
    #outputText {
      width: 90%;
      min-height: 100px;
      margin-top: 20px;
      padding: 12px;
      border-radius: 8px;
      border: 1px solid #ccc;
      background: #fff;
      text-align: left;
      direction: ltr;
    }
    .controls {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      align-items: center;
    }
    .swap-btn {
      background: #fbbc05;
      margin: 0 10px;
    }
  </style>
</head>
<body>
  <h1>🌐 مترجم شبيه بجوجل</h1>

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

  <button onclick="translateText()">ترجم الآن</button>
  <button onclick="copyTranslation()">نسخ الترجمة</button>

  <div id="outputText">🔎 الترجمة ستظهر هنا...</div>

  <script>
    async function translateText() {
      const text = document.getElementById("inputText").value;
      const source = document.getElementById("sourceLang").value;
      const target = document.getElementById("targetLang").value;
      const outputDiv = document.getElementById("outputText");

      if (!text.trim()) {
        outputDiv.textContent = "⚠️ الرجاء إدخال نص للترجمة.";
        return;
      }

      outputDiv.textContent = "⏳ جاري الترجمة...";

      try {
        const response = await fetch("https://libretranslate.com/translate", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            q: text,
            source: source,
            target: target,
            format: "text"
          })
        });

        const data = await response.json();

        if (data.translatedText) {
          outputDiv.textContent = data.translatedText;
          // ضبط اتجاه النص
          outputDiv.style.direction = (target === "ar") ? "rtl" : "ltr";
        } else {
          outputDiv.textContent = "❌ لم يتم الحصول على ترجمة.";
          console.log(data);
        }
      } catch (error) {
        outputDiv.textContent = "🚨 خطأ في الاتصال بخدمة الترجمة.";
        console.error(error);
      }
    }

    function swapLanguages() {
      const sourceSelect = document.getElementById("sourceLang");
      const targetSelect = document.getElementById("targetLang");
      const temp = sourceSelect.value;
      sourceSelect.value = targetSelect.value;
      targetSelect.value = temp;

      // نسخ الترجمة للنص الأصلي إذا موجود
      const outputDiv = document.getElementById("outputText");
      const inputText = document.getElementById("inputText");
      if (outputDiv.textContent && outputDiv.textContent !== "🔎 الترجمة ستظهر هنا...") {
        inputText.value = outputDiv.textContent;
        outputDiv.textContent = "🔎 الترجمة ستظهر هنا...";
      }
    }

    function copyTranslation() {
      const outputDiv = document.getElementById("outputText");
      if (outputDiv.textContent && outputDiv.textContent !== "🔎 الترجمة ستظهر هنا...") {
        navigator.clipboard.writeText(outputDiv.textContent)
          .then(() => alert("تم نسخ الترجمة! ✅"))
          .catch(err => alert("خطأ عند النسخ: " + err));
      }
    }
  </script>
</body>
</html>
