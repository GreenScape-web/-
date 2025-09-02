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
    }
  </style>
</head>
<body>
  <h1>🌍 المترجم</h1>
  <textarea id="inputText" placeholder="اكتب النص هنا..."></textarea><br>
  <button onclick="translateText()">ترجم</button>
  <div id="outputText">🔎 الترجمة ستظهر هنا...</div>

  <script>
    const apiKey = "AIzaSyBYB1k-Ba3sQM2rdPF1jb7dsZyQ1IYQns0";

    async function translateText() {
      const text = document.getElementById("inputText").value;
      const outputDiv = document.getElementById("outputText");
      
      if (!text) {
        outputDiv.textContent = "⚠️ الرجاء إدخال نص للترجمة.";
        return;
      }

      try {
        const response = await fetch(
          `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=${apiKey}`,
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              contents: [{ parts: [{ text: `Translate this to Arabic:\n${text}` }] }]
            })
          }
        );

        const data = await response.json();

        if (data.candidates && data.candidates[0].content.parts[0].text) {
          outputDiv.textContent = data.candidates[0].content.parts[0].text;
        } else {
          outputDiv.textContent = "❌ لم يتم الحصول على ترجمة. تحقق من الإعدادات.";
          console.log(data);
        }
      } catch (error) {
        outputDiv.textContent = "🚨 خطأ: " + error.message;
      }
    }
  </script>
</body>
</html>
