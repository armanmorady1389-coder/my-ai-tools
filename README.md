<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>هوش مصنوعی هوشمند | AI Tools</title>
    <style>
        :root { --primary: #4361ee; --dark: #1e1e26; }
        body { font-family: 'Tahoma', sans-serif; background: #f4f7fe; margin: 0; padding: 20px; color: var(--dark); text-align: center; }
        .container { max-width: 500px; margin: auto; background: white; padding: 20px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); }
        textarea { width: 90%; height: 100px; padding: 10px; border-radius: 10px; border: 1px solid #ddd; margin: 10px 0; font-family: Tahoma; font-size: 14px; }
        button { width: 100%; background: var(--primary); color: white; border: none; padding: 15px; border-radius: 12px; font-size: 16px; cursor: pointer; font-weight: bold; }
        #resultBox { margin-top: 20px; padding: 15px; background: #f0f4ff; border-radius: 10px; display: none; text-align: right; line-height: 1.8; white-space: pre-wrap; border-right: 5px solid var(--primary); }
        .loader { display: none; color: var(--primary); margin: 10px 0; font-weight: bold; }
    </style>
</head>
<body>

<div class="container">
    <h2>🤖 دستیار هوشمند AI</h2>
    <p>موضوع رو بنویس، با کلید جدید برات جادو می‌کنم!</p>
    
    <textarea id="inputText" placeholder="مثلاً: یه کپشن خفن برای فروش ساعت هوشمند بنویس..."></textarea>
    
    <div id="loader" class="loader">در حال پردازش با هوش مصنوعی... 🧠</div>
    <button id="sendBtn" onclick="askAI()">تولید محتوا 🚀</button>

    <div id="resultBox">
        <strong>نتیجه تولید شده:</strong>
        <div id="aiResponse" style="margin-top: 10px;"></div>
    </div>
</div>

<script>
    async function askAI() {
        const text = document.getElementById('inputText').value;
        const resultBox = document.getElementById('resultBox');
        const aiResponse = document.getElementById('aiResponse');
        const loader = document.getElementById('loader');
        const btn = document.getElementById('sendBtn');

        if(!text) { alert("داداش اول یه چیزی بنویس! 😂"); return; }

        loader.style.display = "block";
        resultBox.style.display = "none";
        btn.disabled = true;

        // این کلید جدید تو هست:
        const API_KEY = "AIzaSyCblSPH0y3MSvgZLpGP6Myp4PrKUKf1_D4"; 
        const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;

        try {
            const response = await fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    contents: [{ parts: [{ text: "به عنوان یک نویسنده محتوای حرفه‌ای، یک متن جذاب و خلاقانه به زبان فارسی درباره این موضوع بنویس: " + text }] }]
                })
            });

            const data = await response.json();
            
            if (data.error) {
                aiResponse.innerText = "خطای گوگل: " + data.error.message;
            } else {
                const output = data.candidates[0].content.parts[0].text;
                aiResponse.innerText = output;
            }
            
            resultBox.style.display = "block";
        } catch (error) {
            aiResponse.innerText = "خطای شبکه! 🚨\nاحتمالاً فیلترشکنت خاموشه. روشن کن و دوباره بزن.\nپیام خطا: " + error.message;
            resultBox.style.display = "block";
        } finally {
            loader.style.display = "none";
            btn.disabled = false;
        }
    }
</script>

</body>
</html>
