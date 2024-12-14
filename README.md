
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatGPT - واجهة تفاعلية بالذكاء الاصطناعي</title>
    <style>
        /* نفس التنسيقات السابقة */
    </style>
</head>
<body>

    <!-- رأس الصفحة -->
    <header>
        <h1>ChatGPT - واجهة تفاعلية بالذكاء الاصطناعي</h1>
    </header>

    <!-- واجهة المحادثة -->
    <div class="chat-container">
        <div class="chat-box" id="chat-box">
            <div class="message">
                <p>مرحبًا! كيف يمكنني مساعدتك اليوم؟</p>
            </div>
        </div>

        <div class="input-container">
            <input type="text" id="user-input" placeholder="اكتب رسالتك هنا...">
            <button onclick="sendMessage()">إرسال</button>
        </div>
    </div>

    <!-- تذييل الصفحة -->
    <footer>
        <p>جميع الحقوق محفوظة &copy; 2024</p>
    </footer>

    <script>
        // وظيفة لإرسال الرسالة
        async function sendMessage() {
            const inputField = document.getElementById('user-input');
            const chatBox = document.getElementById('chat-box');
            const userMessage = inputField.value;

            if (userMessage.trim() !== "") {
                // إضافة رسالة المستخدم إلى واجهة المحادثة
                const userMessageDiv = document.createElement('div');
                userMessageDiv.classList.add('message', 'user');
                const userMessageText = document.createElement('p');
                userMessageText.textContent = userMessage;
                userMessageDiv.appendChild(userMessageText);
                chatBox.appendChild(userMessageDiv);

                // التمرير للأسفل لعرض آخر رسالة
                chatBox.scrollTop = chatBox.scrollHeight;

                // مسح حقل الإدخال
                inputField.value = '';

                // إرسال الطلب إلى OpenAI API للحصول على الرد
                const response = await getAIResponse(userMessage);
                
                console.log("AI Response:", response);  // تحقق من الرد في الكونسول

                if(response) {
                    // إضافة رد الذكاء الاصطناعي إلى واجهة المحادثة
                    const botMessageDiv = document.createElement('div');
                    botMessageDiv.classList.add('message');
                    const botMessageText = document.createElement('p');
                    botMessageText.textContent = response;
                    botMessageDiv.appendChild(botMessageText);
                    chatBox.appendChild(botMessageDiv);

                    // التمرير للأسفل لعرض الرد الجديد
                    chatBox.scrollTop = chatBox.scrollHeight;
                } else {
                    console.log("لم يتم الحصول على رد من OpenAI.");
                }
            }
        }

        // وظيفة للحصول على رد الذكاء الاصطناعي من OpenAI
        async function getAIResponse(userMessage) {
            const apiKey = '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';  // مفتاح API الخاص بك
            const apiUrl = 'https://api.openai.com/v1/completions';

            const requestBody = {
                model: "text-davinci-003",  // نموذج GPT-3 (يمكنك استخدام أي نموذج آخر مثل GPT-4)
                prompt: userMessage,
                max_tokens: 150,
                temperature: 0.7,
            };

            try {
                // إرسال الطلب إلى API
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`,  // استخدام المفتاح في هذا المكان
                    },
                    body: JSON.stringify(requestBody),
                });

                const data = await response.json();

                // تحقق من البيانات التي تم استلامها
                console.log("API Response Data:", data);

                // إرجاع نص الرد من OpenAI
                return data.choices && data.choices[0] ? data.choices[0].text.trim() : null;

            } catch (error) {
                console.error("خطأ في الاتصال بـ OpenAI:", error);
                return null;
            }
        }
    </script>

</body>
</html>
