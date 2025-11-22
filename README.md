<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>روبوت دردشة مكتبة جامعة الوصل</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
            background: linear-gradient(135deg, #e3f2fd, #bbdefb);
            display: flex;
            justify-content: center;
            padding: 20px;
        }

        .page {
            width: 100%;
            max-width: 1100px;
        }

        header {
            background: rgba(255, 255, 255, 0.8);
            padding: 10px 16px;
            border-radius: 16px;
            margin-bottom: 15px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.6);
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
        }

        .header-text {
            display: flex;
            flex-direction: column;
        }

        header h1 {
            margin: 0;
            font-size: 20px;
        }

        header p {
            margin: 4px 0 0;
            font-size: 13px;
            color: #555;
        }

        .wasl-logo {
            width: 75px;
            height: auto;
            object-fit: contain;
        }

        @media (max-width: 600px) {
            header {
                flex-direction: row-reverse;
            }
            header h1 {
                font-size: 18px;
            }
        }

        main {
            display: grid;
            grid-template-columns: 2fr 1.5fr;
            gap: 15px;
        }

        @media (max-width: 850px) {
            main {
                grid-template-columns: 1fr;
            }
        }

        .chatbot-box, .ai-helper-box {
            background: rgba(255,255,255,0.7);
            padding: 15px;
            border-radius: 16px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.6);
        }

        .chatbot-box h2,
        .ai-helper-box h3 {
            margin-top: 0;
            text-align: center;
        }

        .chat-window {
            background: rgba(255,255,255,0.9);
            border-radius: 14px;
            padding: 10px;
            height: 360px;
            overflow-y: auto;
            border: 1px solid #e0e0e0;
        }

        .message {
            padding: 8px 10px;
            border-radius: 12px;
            margin: 6px 0;
            max-width: 85%;
            font-size: 14px;
            line-height: 1.5;
        }

        .message.user {
            background: #1e88e5;
            color: white;
            margin-left: auto;
            text-align: right;
        }

        .message.bot {
            background: #f5f5f5;
            color: #222;
            margin-right: auto;
        }

        .chat-input-row {
            display: flex;
            gap: 8px;
            margin-top: 10px;
        }

        #userInput {
            flex: 1;
            padding: 10px;
            border-radius: 999px;
            border: 1px solid #b0bec5;
            outline: none;
        }

        .send-btn {
            padding: 0 18px;
            border-radius: 999px;
            border: none;
            background: #1e88e5;
            color: white;
            cursor: pointer;
            font-size: 14px;
        }

        .quick-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            margin: 10px 0;
        }

        .quick-buttons button {
            border-radius: 999px;
            border: 1px solid #bbdefb;
            background: #e3f2fd;
            padding: 6px 10px;
            font-size: 12px;
            cursor: pointer;
        }

        .ai-helper-box textarea {
            width: 100%;
            height: 140px;
            padding: 10px;
            border-radius: 10px;
            border: 1px solid #cfd8dc;
            resize: none;
            background: #fafafa;
        }

        .ai-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
        }

        .ai-buttons button {
            flex: 1 1 calc(50% - 8px);
            padding: 8px;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            background: #1e88e5;
            color: white;
            font-size: 13px;
        }

        #aiOutput {
            background: rgba(255,255,255,0.95);
            margin-top: 10px;
            padding: 10px;
            border-radius: 10px;
            min-height: 60px;
            border: 1px solid #e0e0e0;
            font-size: 13px;
            white-space: pre-line;
        }

        footer {
            text-align: center;
            margin-top: 20px;
            font-size: 12px;
            color: #555;
        }
    </style>
</head>
<body>
<div class="page">
    <header>
        <!-- شعار جامعة الوصل (شفاف) -->
        <img src="logo.png" alt="شعار جامعة الوصل" class="wasl-logo">

        <div class="header-text">
            <h1>روبوت دردشة مكتبة جامعة الوصل</h1>
            <p>مساعد ذكي يسهّل عليك الوصول لخدمات المكتبة، مصادر المعلومات، وقواعد البيانات.</p>
        </div>
    </header>

    <main>
        <!-- صندوق الشات بوت -->
        <section class="chatbot-box">
            <h2>💬 اسأل مكتبة جامعة الوصل</h2>

            <div class="chat-window" id="chatWindow"></div>

            <!-- أزرار سريعة -->
            <div class="quick-buttons">
                <button onclick="quickAsk('ما هي مواعيد عمل المكتبة؟')">مواعيد المكتبة</button>
                <button onclick="quickAsk('أين تقع مكتبة جامعة الوصل؟')">موقع المكتبة</button>
                <button onclick="quickAsk('ما هي سياسة الإعارة؟')">سياسة الإعارة</button>
                <button onclick="quickAsk('ما هي مصادر المعلومات المتاحة؟')">المصادر المتاحة</button>
                <button onclick="quickAsk('ما هي قواعد البيانات مثل المنهل؟')">قواعد البيانات</button>
                <button onclick="quickAsk('ما هي البرامج الأكاديمية؟')">البرامج الأكاديمية</button>
                <button onclick="quickAsk('ما هو نظام التصنيف المستخدم؟')">نظام التصنيف</button>
                <button onclick="quickAsk('كيف أدخل إلى قاعدة المنهل؟')">الوصول للمنهل</button>
                <button onclick="quickAsk('ما هي طرق التواصل مع المكتبة؟')">التواصل</button>
            </div>

            <div class="chat-input-row">
                <input id="userInput" type="text" placeholder="اكتبي سؤالك هنا…">
                <button class="send-btn" onclick="sendMessage()">إرسال</button>
            </div>
        </section>

        <!-- المساعد البحثي الذكي -->
        <section class="ai-helper-box">
            <h3>🤖 المساعد البحثي الذكي</h3>
            <p style="font-size:13px; margin-top:0; color:#555;">ألصقي نصًا أو اكتبي وصفًا لموضوعك، ثم اختاري نوع المساعدة:</p>

            <textarea id="aiInput" placeholder="ألصقي النص هنا…"></textarea>

            <div class="ai-buttons">
                <button onclick="summarize()">تلخيص النص</button>
                <button onclick="keywords()">كلمات مفتاحية</button>
                <button onclick="titleGenerate()">عنوان بحث</button>
                <button onclick="writeEmail()">استفسار رسمي</button>
            </div>

            <div id="aiOutput">ستظهر النتيجة هنا…</div>
        </section>
    </main>

    <footer>
        مكتبة جامعة الوصل – دبي
    </footer>
</div>

<script>
    const chatWindow = document.getElementById("chatWindow");
    const userInput = document.getElementById("userInput");

    window.addEventListener("load", () => {
        addMessage("مرحبًا بك 👋\nأنا مساعد مكتبة جامعة الوصل. اسألني عن أي خدمة مكتبية.", "bot");
    });

    function addMessage(text, sender) {
        const msg = document.createElement("div");
        msg.classList.add("message", sender);
        msg.textContent = text;
        chatWindow.appendChild(msg);
        chatWindow.scrollTop = chatWindow.scrollHeight;
    }

    function sendMessage() {
        const text = userInput.value.trim();
        if (!text) return;
        addMessage(text, "user");
        userInput.value = "";
        setTimeout(() => {
            addMessage(generateReply(text), "bot");
        }, 400);
    }

    function quickAsk(text) {
        userInput.value = text;
        sendMessage();
    }

    function generateReply(t) {
        t = t.toLowerCase();

        if (t.includes("مواعيد")) return "⏰ مواعيد المكتبة: الإثنين–الخميس، 8 صباحًا – 8 مساءً.";
        if (t.includes("موقع")) return "📍 موقع المكتبة: الحرم الجامعي – مبنى المكتبة الرئيسي.";
        if (t.includes("إعارة")) return "📚 سياسة الإعارة: أسبوع–أسبوعين حسب نوع المصدر.";
        if (t.includes("مصادر")) return "📘 المصادر: كتب – مراجع – دوريات – رسائل – قواعد بيانات.";
        if (t.includes("قواعد") || t.includes("منهل")) return "💻 قواعد البيانات: أهمها قاعدة المنهل للكتب والرسائل العربية.";
        if (t.includes("برامج")) return "🎓 البرامج الأكاديمية: العربية وآدابها – علوم المكتبات – الدراسات الإسلامية.";
        if (t.includes("ديوي") || t.includes("تصنيف")) return "🔢 نظام ديوي العشري هو النظام المستخدم لترتيب الكتب في المكتبة.";
        if (t.includes("غرف")) return "🗂 حجز الغرف: يتم عبر أمينة المكتبة أو نموذج الحجز.";
        if (t.includes("تواصل") || t.includes("ايميل")) return "📨 للتواصل: يرجى الرجوع لصفحة المكتبة في موقع الجامعة.";
        
        return "سؤال جميل! تقدرين تسألين عن المواعيد، الإعارة، المصادر، قواعد البيانات، البرامج الأكاديمية، نظام التصنيف، أو التواصل.";
    }

    function summarize() {
        let text = document.getElementById("aiInput").value.trim();
        if (!text) return document.getElementById("aiOutput").innerText="يرجى لصق نص أولاً.";
        document.getElementById("aiOutput").innerText="✨ ملخص:\nالنص يتناول فكرة رئيسية ويوضح أهم العناصر بشكل مختصر يساعد على فهم المحتوى.";
    }

    function keywords() {
        document.getElementById("aiOutput").innerText="🔑 كلمات مقترحة:\nمكتبات – ذكاء اصطناعي – مصادر – قواعد بيانات – بحث علمي – فهرسة.";
    }

    function titleGenerate() {
        document.getElementById("aiOutput").innerText="📝 عنوان مقترح:\nتطوير خدمات المكتبات الجامعية باستخدام تقنيات الذكاء الاصطناعي.";
    }

    function writeEmail() {
        document.getElementById("aiOutput").innerText="📩 نموذج استفسار:\nالسادة مكتبة جامعة الوصل، أود الاستفسار حول… مع الشكر.";
    }
</script>

</body>
</html>
