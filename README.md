<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>المترجم</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      margin: 0;
      padding: 20px;
      direction: rtl;
      text-align: center;
    }
    textarea {
      width: 90%;
      height: 120px;
      margin: 10px 0;
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 10px;
    }
    select {
      padding: 10px;
      font-size: 16px;
      border-radius: 8px;
      margin: 10px;
    }
    button {
      padding: 10px 20px;
      background: #007BFF;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background: #0056b3;
    }
    #outputText {
      width: 90%;
      min-height: 100px;
      margin-top: 20px;
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 10px;
      background: #fff;
      text-align: left;
    }
  </style>
</head>
<body>
  <h1>🌍 المترجم (LibreTranslate)</h1>
  <textarea id="inputText" placeholder="اكتب النص هنا..."></textarea><br>
  
  <label for="targetLang">اختر اللغة:</label>
  <select id="targetLang">
    <option value="ar">العربية</option>
    <option value="en">الإنجليزية</option>
    <option value="fr">الفرنسية</option>
    <option value="de">الألمانية</option>
    <option value="es">الإسبانية</option>
  </select>
  
  <button onclick="translateText()">ترجم</button>
  <div id="outputText">🔎 الترجمة ستظهر هنا...</div>

  <script>
    async function translateText() {
      const text = document.getElementById("inputText").value;
      const lang = document.getElementById("targetLang").value;
      const outputDiv = document.getElementById("outputText");

      if (!text) {
        outputDiv.textContent = "⚠️ الرجاء إدخال نص للترجمة.";
        return;
      }

      try {
        const response = await fetch("https://libretranslate.com/translate", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            q: text,
            source: "auto",
            target: lang,
            format: "text"
          })
        });

        const data = await response.json();

        if (data.translatedText) {
          outputDiv.textContent = data.translatedText;
        } else {
          outputDiv.textContent = "❌ لم يتم الحصول على ترجمة.";
          console.log(data);
        }
      } catch (error) {
        outputDiv.textContent = "🚨 خطأ: " + error.message;
      }
    }
  </script>
</body>
</html>
