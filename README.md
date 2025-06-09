<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مركز الامتحان - بريد الجزائر</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
      margin: 0;
      padding: 0;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px 10px; /* تقليل البادينغ */
      background-color: #fff;
      border-bottom: 1px solid #ccc;
    }

    .header-content {
      display: flex;
      align-items: center;
      gap: 5px; /* تقليل المسافة بين الشعار والنص */
    }

    .header-content span {
      font-size: 18px; /* تصغير الخط */
      font-weight: bold;
    }

    .header-content img {
      height: 60px; /* تصغير حجم الشعار */
    }

    .content {
      text-align: center;
      padding: 15px; /* تقليل المسافة حول المحتوى */
    }

    .box {
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 10px;
      padding: 15px; /* تقليل الفراغ داخل المربع */
      display: inline-block;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
      width: 70%;
      max-width: 800px;
      min-width: 300px;
      margin: 0 auto;
    }

    .status {
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 5px 0; /* تقليل الهامش العمودي */
    }

    .status span {
      background-color: #f0f0f0;
      border-radius: 10px;
      padding: 3px 8px; /* تقليل الحجم الداخلي */
      margin-right: 5px;
      font-size: 14px;
    }

    .status .orange {
      background-color: orange;
      color: white;
    }

    button {
      padding: 8px 16px;
      margin-top: 10px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      background-color: #ddd;
      cursor: pointer;
    }

    #exam-section {
      display: none;
      padding: 20px; /* تقليل المسافة */
    }

    .question {
      font-size: 28px; /* تقليل حجم السؤال */
      font-weight: bold;
      margin-bottom: 10px;
    }

    label {
      display: block;
      margin: 5px 0; /* تقليل الفراغ بين الخيارات */
      font-size: 15px;
    }

    .hidden {
      display: none;
    }

    #timer {
      font-weight: bold;
      color: red;
      margin: 5px 0; /* تقليل المسافة حول المؤقت */
    }

    #nav-buttons {
      margin-top: 10px;
    }

    .correct {
      color: green;
      font-weight: bold;
    }
    .wrong {
      color: red;
      font-weight: bold;
    }
    .question-block {
      margin-bottom: 20px;
    }

    /* الأنماط لرسالة الإقصاء */
    #disqualification-container {
      background-color: #ffe6e6; /* خلفية حمراء فاتحة */
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      max-width: 600px;
      width: 90%;
      border: 2px solid #ff4d4d; /* حدود حمراء */
      text-align: center;
      margin: 50px auto; /* لتوسيط الرسالة */
    }

    #disqualification-text {
      color: #ff0000; /* نص أحمر للإقصاء */
      font-size: 2em;
      font-weight: bold;
      margin-bottom: 20px;
    }

    #retry-button {
      background-color: #4CAF50; /* خلفية خضراء */
      color: white;
      padding: 15px 30px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1.2em;
      transition: background-color 0.3s ease;
    }

    #retry-button:hover {
      background-color: #45a049; /* أخضر أغمق عند التحويم */
    }
  </style>

</head>

<body>

  <div class="header">
    <div class="header-content">
      <img src="https://i.postimg.cc/qv9RbZRT/images-15.jpg" alt="Logo">
      <span>مركز الامتحان</span>
    </div>
  </div>


  <div class="content" id="welcome-section">
    <h2 id="username">مرحبا المترشح</h2>
    <div class="box">
      <h3> سائق شاحنة وزن ثقيل </h3>
      <div class="status">
        <span>35 الأسئلة</span>
        <span class="orange">لم يبدأ بعد</span>
      </div>
      <p>امتحان سائق شاحنة وزن ثقيل</p>
      <button onclick="startExam()">بدء الامتحان</button>
    </div>
  </div>

  <div id="exam-section" class="hidden">
    <div class="question" id="question-text"></div>
    <div id="timer"></div>
    <div id="choices"></div>
    <div id="nav-buttons">
      <button id="prevBtn" onclick="prevQuestion()">السابق</button>
      <button onclick="nextQuestion()">التالي</button>
    </div>
    <div id="result" class="hidden"></div>
  </div>

  <div id="correction-section" class="hidden"></div>

  <script>
    // 👇 اختر الوضع هنا: "training" أو "official"
    const mode = "training"; // قم بتغيير هذا إلى "official" عند الحاجة
    let examStarted = false; // متغير لتتبع حالة بدء الامتحان

    document.addEventListener("DOMContentLoaded", function () {
        const welcomeSection = document.getElementById("welcome-section");
        const examSection = document.getElementById("exam-section");

        // دالة لعرض رسالة الإقصاء بناءً على الوضع
        function showDisqualificationMessage() {
            // إخفاء كل أقسام الصفحة
            welcomeSection.classList.add("hidden");
            examSection.classList.add("hidden");

            // مسح المحتوى الحالي لـ body بالكامل لضمان عدم ظهور أي شيء آخر
            document.body.innerHTML = '';

            const div = document.createElement('div');
            div.id = 'disqualification-container';
            div.className = 'disqualification-container';

            if (mode === "training") {
                div.innerHTML = `
                    <p id="disqualification-text">لقد تم إقصاؤك من الامتحان!</p>
                    <p>هذا اختبار تجريبي، يمكنك إعادة المحاولة.</p>
                    <button id="retry-button">إعادة المحاولة</button>
                `;
                document.body.appendChild(div);
                document.getElementById("retry-button").addEventListener("click", function () {
                    // مسح علامة الإقصاء والسماح بإعادة المحاولة
                    localStorage.removeItem("disqualified");
                    location.reload(); // إعادة تحميل الصفحة
                });
            } else if (mode === "official") {
                div.innerHTML = `
                    <p id="disqualification-text">لقد تم إقصاؤك من الامتحان!</p>
                    <p>لا يمكنك إعادة الدخول مرة أخرى.</p>
                `;
                document.body.appendChild(div);
            }
        }

        // التحقق عند تحميل الصفحة إذا كان المستخدم قد تم إقصاؤه مسبقًا
        if (localStorage.getItem("disqualified") === "true") {
            showDisqualificationMessage();
        } else {
            // إظهار المحتوى الأصلي للصفحة إذا لم يتم الإقصاء
            welcomeSection.classList.remove("hidden");
            examSection.classList.add("hidden"); // تأكد من إخفاء قسم الامتحان في البداية
        }

        // معالج الحدث عند محاولة المستخدم مغادرة الصفحة أو تحديثها
        window.addEventListener("beforeunload", function (event) {
            // إذا كان الامتحان قد بدأ ولم ينتهِ بعد (أي، لم تظهر صفحة النتائج)
            // هذا هو الشرط الذي يحدد ما إذا كان يجب تسجيل الإقصاء
            if (examStarted && document.getElementById("result").classList.contains("hidden")) {
                 localStorage.setItem("disqualified", "true");
            }
            // تم إزالة event.preventDefault() و event.returnValue = ''; لعدم إظهار الرسالة
        });

        // التقاط الاسم من الرابط
        function getNameFromURL() {
            const params = new URLSearchParams(window.location.search);
            return params.get("name") || "المترشح";
        }

        // عرض الاسم
        const name = getNameFromURL();
        document.getElementById("username").textContent = `مرحبا ${name.replace(/\./g, ' ')}`;


        // بدء الامتحان (تم جعلها دالة عامة ليمكن استدعاؤها من HTML)
        window.startExam = function() {
            examStarted = true; // تعيين حالة بدء الامتحان إلى صحيح
            welcomeSection.style.display = "none";
            examSection.style.display = "block";
            showQuestion();
        };

        const questions = [];

        function addQuestion(q, options, correct) {
            questions.push({ q, choices: options, correct });
        }

        // أسئلتك هنا


 // --- أسئلة سائق مديرية ---

  addQuestion("ما هي الحمولة القصوى المسموح بها لشاحنة ذات وزن ثقيل؟", ["19 طن", "26 طن", "38 طن", "45 طن"], 2);
    addQuestion("ما هي الوثيقة الإلزامية عند نقل الطرود؟", ["رخصة قيادة فقط", "وثيقة إثبات الشحنة", "شهادة تأمين", "بطاقة مهنية فقط"], 1);
    addQuestion("عند استعمال المكابح باستمرار في منحدر، ما هو الخطر؟", ["توفير الوقود", "سخونة الفرامل", "زيادة السرعة", "زيادة الثبات"], 1);
    addQuestion("كيف يتم التعامل مع شحنة قابلة للاشتعال؟", ["تنقل مع باقي الطرود", "تخزن بشكل منفصل", "يتم رفضها", "تنقل بسرعة"], 1);
    addQuestion("ما الإجراء الإلزامي قبل الانطلاق؟", ["وجود زميل", "تشغيل الراديو", "فحص المكابح", "تفريغ الحمولة"], 2);
    addQuestion("السرعة القصوى في المدينة لشاحنة ثقيلة؟", ["80 كم/س", "50 كم/س", "60 كم/س", "70 كم/س"], 1);
    addQuestion("إذا لاحظ السائق خللاً في نظام التوجيه؟", ["متابعة السير", "إعلام المصلحة", "استخدام المكابح", "تفريغ الحمولة"], 1);
    addQuestion("الإجراء عند حادث بسيط بدون إصابات؟", ["المغادرة", "انتظار الشرطة", "معاينة ودية", "ترك الشاحنة"], 2);
    addQuestion("أداة الاستخدام الآمن في منحدر طويل؟", ["دواسة الوقود", "المكابح الهوائية", "فرملة المحرك", "السرعة الأوتوماتيكية"], 2);
    addQuestion("الجهة المسؤولة عن مراقبة الوزن؟", ["بريد الجزائر", "وزارة النقل", "الدرك الوطني", "مؤسسة الطرق"], 2);
    addQuestion("اللون البرتقالي يعني:", ["حمولة سريعة التلف", "قابلة للاشتعال", "طرد إلكتروني", "وثائق بريدية"], 1);
    addQuestion("عند تسرب الوقود؟", ["الاستمرار", "التبليغ", "تغطية التسرب", "ربط الخزان"], 1);
    addQuestion("دفتر متابعة الطريق يُستخدم لـ؟", ["مراقبة الوقود", "تسجيل المكالمات", "تتبع الطرود", "إثبات الحضور"], 2);
    addQuestion("في حالة طقس سيء؟", ["القيادة العادية", "تقليل السرعة", "انتظار توقف المطر", "استخدام الممر الأيسر"], 1);
    addQuestion("ما يجب فحصه دوريًا؟", ["الزجاج", "الزيت والمياه", "الديكور", "لون اللوحة"], 1);
    addQuestion("عند اقتراب حافلة مدرسية؟", ["التجاوز", "التوقف أو الحذر", "البوق", "الانتظار دقيقة"], 1);
    addQuestion("عند سماع صوت غير طبيعي بالمحرك؟", ["زيادة السرعة", "فحص المحرك", "تجاهل الصوت", "التبليغ لاحقًا"], 1);
    addQuestion("نوع الوقود الأكثر استخدامًا؟", ["البنزين", "المازوت", "الغاز", "الكهرباء"], 1);
    addQuestion("أداة التحكم في منحدر؟", ["المكابح", "فرملة المحرك", "السرعة الحرة", "إيقاف المحرك"], 1);
    addQuestion("عند رؤية ممنوع مرور الشاحنات؟", ["المرور", "تغيير المسار", "خفض السرعة", "الاستمرار"], 1);
    addQuestion("عطب في المكابح؟", ["النزول", "فرامل الطوارئ", "إطفاء المحرك", "فتح الأبواب"], 1);
    addQuestion("الغرض من التاكوغراف؟", ["تصفية الهواء", "تسجيل السرعة", "المراقبة الجوية", "المكيف"], 1);
    addQuestion("من مهام السائق؟", ["جمع الطرود", "احترام المواعيد", "توزيع الرسائل", "تسجيل المستخدمين"], 1);
    addQuestion("مدة الراحة بعد 4.5 ساعات قيادة؟", ["15 دقيقة", "لا حاجة", "45 دقيقة", "90 دقيقة"], 2);
    addQuestion("في حال حريق؟", ["الذهاب للبريد", "التوقف ومحاولة الإطفاء", "القماش", "فتح الأبواب"], 1);
    addQuestion("عند تعبئة الوقود؟", ["المحرك مشتغل", "إيقاف المحرك", "الهاتف", "غسل الزجاج"], 1);
    addQuestion("الطرود غير المعلبة؟", ["تجاهل", "إبلاغ", "رمي", "نقلها فقط"], 1);
    addQuestion("توزيع الحمولة؟", ["أي ترتيب", "بالتساوي", "في الأمام", "خفيفة فقط"], 1);
    addQuestion("عطب في الطريق الوطني؟", ["الجلوس", "مثلث الخطر", "الأغاني", "إغلاق الطريق"], 1);
    addQuestion("ضغط الإطارات؟", ["بالنظر", "بالسماع", "جهاز خاص", "السير قليلًا"], 2);
    addQuestion("تجاوز الحمولة؟", ["توفير الوقت", "راحة السائق", "مخالفات وخطر", "سهولة السير"], 2);
    addQuestion("القيادة الليلية؟", ["الموسيقى", "زيادة السرعة", "الحذر والأضواء", "الضوء الداخلي"], 2);
    addQuestion("العواصف الرملية تؤثر على؟", ["العجلات", "الرؤية", "الطريق", "الحمولة"], 1);
    addQuestion("أكثر أسباب الحوادث؟", ["نقص الوقود", "الإرهاق", "خلل الطرود", "الشحن"], 1);
    addQuestion("إجراء يومي؟", ["تنظيف المرايا", "فحص الشاحنة", "ترتيب المكتب", "التوقيع"], 1);



let currentQuestion = 0;
        let answers = Array(questions.length).fill(null);
        let timers = Array(questions.length).fill(120); // 120 ثانية
        let timerInterval;

        function showQuestion() {
            clearInterval(timerInterval);

            const q = questions[currentQuestion];
            document.getElementById("question-text").textContent = `السؤال ${currentQuestion + 1}: ${q.q}`;
            const choicesDiv = document.getElementById("choices");
            choicesDiv.innerHTML = "";

            q.choices.forEach((choice, index) => {
                const label = document.createElement("label");
                const input = document.createElement("input");
                input.type = "radio";
                input.name = "choice";
                input.value = index;
                // تعطيل الخيارات إذا انتهى الوقت أو تم اختيار إجابة
                input.disabled = (timers[currentQuestion] <= 0 || answers[currentQuestion] !== null);
                if (answers[currentQuestion] === index) input.checked = true;

                input.addEventListener("change", () => {
                    if (answers[currentQuestion] === null) { // السماح بالاختيار فقط إذا لم تتم الإجابة بالفعل
                        answers[currentQuestion] = index;
                        disableChoices(); // تعطيل الخيارات بعد الاختيار
                    }
                });

                label.appendChild(input);
                label.appendChild(document.createTextNode(" " + choice));
                choicesDiv.appendChild(label);
            });

            document.getElementById("prevBtn").style.display = currentQuestion === 0 ? "none" : "inline";

            startTimer();
        }

        function startTimer() {
            const timerElement = document.getElementById("timer");

            if (timers[currentQuestion] <= 0) {
                timerElement.textContent = "انتهى الوقت لهذا السؤال.";
                disableChoices(); // التأكد من تعطيل الخيارات إذا انتهى الوقت
                return;
            }

            updateTimerText();

            timerInterval = setInterval(() => {
                timers[currentQuestion]--;
                updateTimerText();

                if (timers[currentQuestion] <= 0) {
                    clearInterval(timerInterval);
                    timerElement.textContent = "انتهى الوقت لهذا السؤال.";
                    disableChoices();
                    // لا تنتقل تلقائيا للسؤال التالي في هذا السيناريو، اترك المستخدم ينتقل بنفسه أو ينهي الامتحان
                    // setTimeout(nextQuestion, 1000); 
                }
            }, 1000);
        }

        function updateTimerText() {
            const timerElement = document.getElementById("timer");
            const seconds = timers[currentQuestion];
            const minutes = Math.floor(seconds / 60);
            const remainingSeconds = seconds % 60;

            const minStr = minutes.toString().padStart(2, '0');
            const secStr = remainingSeconds.toString().padStart(2, '0');

            timerElement.textContent = `الوقت المتبقي: ${minStr}:${secStr}`;
        }

        function disableChoices() {
            const inputs = document.querySelectorAll("input[name='choice']");
            inputs.forEach(input => input.disabled = true);
        }

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.nextQuestion = function() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                showQuestion();
            } else {
                finishQuiz();
            }
        };

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.prevQuestion = function() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion();
            }
        };

        function finishQuiz() {
            clearInterval(timerInterval);
            examStarted = false; // الامتحان انتهى بنجاح، لذا لن يتم تسجيل إقصاء
            localStorage.removeItem("disqualified"); // مسح حالة الإقصاء عند الانتهاء بنجاح

            examSection.classList.add("hidden"); // إخفاء قسم الامتحان
            document.getElementById("question-text").classList.add("hidden");
            document.getElementById("choices").classList.add("hidden");
            document.getElementById("timer").classList.add("hidden");
            document.getElementById("nav-buttons").classList.add("hidden");

            let correct = 0;
            let wrong = 0;

            questions.forEach((q, i) => {
                // التحقق مما إذا تم الإجابة على السؤال
                if (answers[i] !== null) {
                    if (answers[i] === q.correct) {
                        correct++;
                    } else {
                        wrong++;
                    }
                }
            });

            let finalScore = correct - wrong;
            if (finalScore < 0) finalScore = 0; // لا يمكن أن تكون النتيجة سلبية

            const resultDiv = document.getElementById("result");
            resultDiv.classList.remove("hidden");

            let message = "";
            if (finalScore >= Math.ceil(questions.length / 2)) {
                message = `<span style="color: green;">✔️ مبروك! لقد نجحت</span>`;
            } else {
                message = `<span style="color: red;">❌ للأسف، حظ موفق في المرة القادمة</span>`;
            }

            resultDiv.innerHTML = `
                <h2>انتهى الاختبار</h2>
                <p>عدد الإجابات الصحيحة: ${correct}</p>
                <p>عدد الإجابات الخاطئة: ${wrong}</p>
                <p><strong>علامتك النهائية: ${finalScore} من ${questions.length}</strong></p>
                <p>${message}</p>
                <footer style="
                    position: fixed;
                    bottom: 0;
                    width: 85%;
                    text-align: center;
                    font-size: 12px;
                    color: #555;
                    background-color: transparent;
                    padding: 5px 0;
                    direction: rtl;
                ">
                    Created by: Haricha Younes
                </footer>
                <button onclick="showCorrectionPage()">تصحيح الاختبار</button>
            `;
        }

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.showCorrectionPage = function() {
            document.getElementById("result").classList.add("hidden");
            const correctionSection = document.getElementById("correction-section");
            correctionSection.innerHTML = "<h2>تصحيح الاختبار:</h2>";

            questions.forEach((q, index) => {
                const userAnswer = answers[index];
                const correctAnswer = q.correct;

                const div = document.createElement("div");
                div.style.padding = "10px";
                div.style.marginBottom = "10px";
                div.style.borderRadius = "10px";
                div.style.backgroundColor = userAnswer === correctAnswer ? "#d4edda" : "#f8d7da"; // أخضر للفصل، أحمر للخطأ

                div.innerHTML = `
                    <p><strong>سؤال ${index + 1}:</strong> ${q.q}</p>
                    <p>✔️ الإجابة الصحيحة: ${q.choices[correctAnswer]}</p>
                    <p>📝 إجابتك: ${userAnswer !== null ? q.choices[userAnswer] : "لم يجب"}</p>
                `;

                correctionSection.appendChild(div);
            });

            correctionSection.classList.remove("hidden");
        };
    });
  </script>
</body>
</html>
