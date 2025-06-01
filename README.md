<!DOCTYPE html>

<html lang="ar" dir="rtl">

<head>

  <meta charset="UTF-8">

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
  padding: 15px 30px;
  background-color: #fff;
  border-bottom: 1px solid #ccc;
  direction: rtl;
}

.header-title {
  font-size: 30px;
  font-weight: bold;
  color: #333;
}

.header-logo {
  height: 50px;
}
    

    .content {

      text-align: center;
      padding: 30px;

    }

    .box {

      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 15px;
      padding: 20px;
      display: inline-block;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);

    }

    .status {

      display: flex;
      justify-content: center;
      align-items: center;
      margin: 10px 0;

    }

    .status span {

      background-color: #f0f0f0;
      border-radius: 10px;
      padding: 5px 10px;
      margin-right: 10px;

    }

    .status .orange {

      background-color: orange;
      color: white;

    }

    button {

      padding: 10px 20px;
      margin-top: 20px;
      font-size: 18px;
      border: none;
      border-radius: 10px;
      background-color: #ddd;
      cursor: pointer;

    }



    #exam-section {

      display: none;
      padding: 40px;

    }



    .question {

      font-size: 60px;
      font-weight: bold;

    }

    label {

      display: block;
      margin: 20px 0;

    }

    .hidden {

      display: none;

    }

    #timer {

      font-weight: bold;
      color: red;
      margin: 10px 0;

    }
    #nav-buttons {
      margin-top: 15px;
    }
  </style>

</head>

<body>

  <!-- الهيدر -->
  <div class="header">
    <!-- شعار + اسم المركز -->
    <div class="logo-title">
      <img src="https://i.postimg.cc/qv9RbZRT/images-15.jpg" alt="Logo" class="logo">
      <div class="center-name">مركز الامتحان</div>
    </div>

    <!-- زر الإغلاق -->
    <button class="close-btn" onclick="window.close()">×</button>
  </div>
  
  <div class="content" id="welcome-section">
    <h2 id="username">مرحبا المترشح</h2>
    <div class="box">
      <h3> مكلف بالزبائن</h3>
      <div class="status">
        <span>55 الأسئلة</span>
        <span class="orange">لم يبدأ بعد</span>
      </div>
      <p>امتحان مكلف بالزبائن</p>
      <button onclick="startExam()">بدء الامتحان</button>
    </div>
  </div>

  <div id="exam-section">
    <div class="question" id="question-text"></div>
    <div id="timer"></div>
    <div id="choices"></div>
    <div id="nav-buttons">
      <button id="prevBtn" onclick="prevQuestion()">السابق</button>
      <button onclick="nextQuestion()">التالي</button>
    </div>
    <div id="result" class="hidden"></div>
  </div>

  <script>
    function changeLanguage(lang) {
      alert("تم تغيير اللغة إلى: " + lang);
      // هنا يمكنك تنفيذ منطق تغيير اللغة الحقيقي حسب الحاجة
    }
    // التقاط الاسم من الرابط
    function getNameFromURL() {
      const params = new URLSearchParams(window.location.search);
      return params.get("name") || "المترشح";
    }

    // عرض الاسم
    document.addEventListener("DOMContentLoaded", function() {
      const name = getNameFromURL();
      document.getElementById("username").textContent = `مرحبا ${name.replace('.', ' ')}`;
    });

    // بدء الامتحان
    function startExam() {
      document.getElementById("welcome-section").style.display = "none";
      document.getElementById("exam-section").style.display = "block";
      showQuestion();
    }

    const questions = [];

    function addQuestion(q, options, correct) {
      questions.push({ q, choices: options, correct });
    }

    // إضافة الأسئلة
    addQuestion("ما هي الوثائق المطلوبة لفتح حساب بريدي جاري؟", ["نسخة من شهادة الميلاد", "الوثيقة CH1 + نسخة من بطاقة الهوية", "بطاقة إقامة", "شهادة عمل"], 1);
    addQuestion("ما هو الحد الأقصى اليومي للتحويل من حساب إلى حساب عبر الصراف؟", ["20,000 دج", "30,000 دج", "50,000 دج", "100,000 دج"], 2);
    addQuestion("ما هي الخدمة التي تسمح بالسحب بدون بطاقة؟", ["Hawalatic", "Cardless", "Flexy", "Edahabia"], 1);
    addQuestion("في أي سنة تم إطلاق البطاقة الذهبية؟", ["2014", "2016", "2017", "2019"], 1);
    addQuestion("ما هو الرقم الأخضر لبريد الجزائر؟", ["1530", "1055", "1500", "1020"], 0);
    addQuestion("ما نوع الحساب الذي يقدّمه بريد الجزائر لتلقي الأجور؟", ["حساب تجاري", "حساب توفير", "حساب بريدي جاري (CCP)", "حساب مشترك"], 2);
    addQuestion("كم هو الحد الأدنى للرصيد اللازم لفتح حساب CCP؟", ["0 دج", "1000 دج", "500 دج", "100 دج"], 0);
    addQuestion("ما هو الرمز الذي يُستخدم لتحويل الأموال إلى حساب CCP؟", ["RIB", "RIP", "IBAN", "SWIFT"], 1);
    addQuestion("ما هي الرسوم السنوية للبطاقة الذهبية؟", ["مجانية", "350 دج", "500 دج", "1000 دج"], 1);
    addQuestion("ما هي مدة صلاحية البطاقة الذهبية؟", ["سنة واحدة", "سنتان", "ثلاث سنوات", "خمس سنوات"], 2);
    addQuestion("أي من هذه الخدمات يمكن إجراؤها عبر البطاقة الذهبية؟", ["دفع الفواتير", "السحب من الصراف", "شراء عبر الإنترنت", "جميع ما سبق"], 3);
    addQuestion("ما هي الجهة المخوّلة بإصدار دفتر الشيكات البريدي؟", ["وزارة المالية", "البنك المركزي", "بريد الجزائر", "الخزينة العمومية"], 2);
    addQuestion("كيف يتم تفعيل البطاقة الذهبية بعد استلامها؟", ["عبر الهاتف", "عبر تطبيق BaridiMob", "من مكتب البريد", "عبر الصراف الآلي"], 3);
    addQuestion("ما هو المبلغ الأقصى للسحب اليومي من البطاقة الذهبية؟", ["20,000 دج", "30,000 دج", "50,000 دج", "لا يوجد حد"], 2);
    addQuestion("كيف يُمكن للزبون إيقاف البطاقة الذهبية في حالة الضياع؟", ["عبر إرسال بريد إلكتروني", "عبر الاتصال بالرقم 1530", "بزيارة الموقع", "لا يمكن ذلك"], 1)                                                                    // الأسئلة (1–55)
    addQuestion("ما هو البريد؟", ["نقل الرسائل", "نقل الأموال", "نقل الأشخاص", "كل ما سبق"], 0);
    addQuestion("متى تأسس بريد الجزائر؟", ["1962", "1975", "1990", "2000"], 0);
    addQuestion("ما هي الخدمات التي يقدمها بريد الجزائر؟", ["خدمات البريد فقط", "خدمات مالية فقط", "خدمات بريدية ومالية", "خدمات سياحية"], 2);
    addQuestion("ما هو رقم الطوارئ في بريد الجزائر؟", ["15", "19", "14", "17"], 1);
    addQuestion("ما هو البريد السريع؟", ["خدمة توصيل الرسائل خلال يوم", "خدمة توصيل الطرود الدولية", "خدمة توصيل الطرود بسرعة عالية", "خدمة إرسال الرسائل الإلكترونية"], 2);
    addQuestion("أي من التالي هو نوع من الخدمات المالية في بريد الجزائر؟", ["القروض", "الحسابات البريدية", "التأمين", "الاستثمار العقاري"], 1);
    addQuestion("ما هي العملات التي يمكن استخدامها في البريد؟", ["الدينار الجزائري فقط", "الدولار فقط", "الدينار واليورو", "أي عملة"], 2);
    addQuestion("ما هو الهدف من خدمة الحساب البريدي الجاري؟", ["حفظ الأموال", "السحب والإيداع فقط", "إجراء المعاملات المالية", "نقل الأموال فقط"], 2);
    addQuestion("أي من هذه الخدمات متعلقة بالطرود؟", ["إرسال الرسائل", "تتبع الطرود", "خدمة الهاتف", "الإنترنت"], 1);
    addQuestion("ما هو البريد الإلكتروني؟", ["رسالة إلكترونية", "خدمة الطرود", "خدمة البريد السريع", "نوع من الطرود"], 0);
    addQuestion("كيف يمكن للزبون تتبع طرده؟", ["عن طريق رقم التتبع", "عن طريق الهاتف فقط", "عن طريق البريد الإلكتروني", "لا يمكن تتبع الطرد"], 0);
    addQuestion("ما هو عمل موظف البريد؟", ["نقل الرسائل فقط", "تقديم الخدمات المالية فقط", "تقديم الخدمات البريدية والمالية", "خدمة العملاء فقط"], 2);
    addQuestion("ما هي الوثائق المطلوبة لفتح حساب بريدي؟", ["بطاقة التعريف فقط", "شهادة ميلاد فقط", "بطاقة التعريف وشهادة الميلاد", "لا حاجة لوثائق"], 0);
    addQuestion("ما هو البريد المالي؟", ["خدمة تقديم القروض", "خدمة التحويلات المالية", "خدمة التوظيف", "خدمة الطرود"], 1);
    addQuestion("ما هي ساعات عمل بريد الجزائر؟", ["من 8 صباحا إلى 4 مساء", "من 9 صباحا إلى 5 مساء", "24 ساعة", "من 7 صباحا إلى 3 مساء"], 0);
    addQuestion("من يحدد سياسة الأسعار في بريد الجزائر؟", ["الدولة", "المدير العام", "الوزارة الوصية", "مجلس الإدارة"], 2);
    addQuestion("ما هي اللغة الرسمية للمراسلات الإدارية؟", ["العربية", "الفرنسية", "الإنجليزية", "كل ما سبق"], 0);
    addQuestion("أي من هذه التصرفات يُعد مخالفًا في الإدارة؟", ["العمل في الوقت المحدد", "تقديم الخدمات للزبائن", "استغلال المنصب لمصالح شخصية", "إبلاغ الرؤساء عن الأخطاء"], 2);
    addQuestion("ما هو السلم الإداري الصحيح للتظلم داخل المؤسسة؟", ["الإدارة المباشرة ثم المفتشية", "المفتشية ثم القضاء", "الزملاء ثم الإدارة", "الإعلام فقط"], 0);
    addQuestion("ما معنى التوقيت الإداري؟", ["توقيت دخول الزبائن", "توقيت عمل الموظفين", "توقيت تسليم الطرود", "توقيت البريد السريع"], 1);
    addQuestion("ما هو اللباس المهني المطلوب في بريد الجزائر؟", ["غير مهم", "رسمي ومحترم", "حسب الطقس", "رياضي"], 1);
    addQuestion("ما المقصود بالهيكل التنظيمي؟", ["ترتيب المكاتب", "خريطة توضح تسلسل المسؤوليات", "عدد الموظفين", "قائمة العطل"], 1);
    addQuestion("من هو المسؤول عن الانضباط داخل المؤسسة؟", ["المدير", "الموظف نفسه", "المفتش", "كل ما سبق"], 3);
    addQuestion("ما هي الجهة التي تراقب جودة الخدمات؟", ["مفتشية العمل", "المديرية العامة", "مكتب الشكاوي", "كل ما سبق"], 3);
    addQuestion("ما هو التصرف المناسب عند تأخر الزبون؟", ["تجاهله", "مساعدته بسرعة واحترام", "مجادلته", "رفض خدمته"], 1);
    addQuestion("ما هو السلوك المطلوب من الموظف تجاه كبار السن؟", ["السرعة فقط", "الاهتمام والاحترام", "طلب مساعدة آخرين", "تجاهل"], 1);
    addQuestion("من المسؤول عن تسجيل العطل السنوية؟", ["الموظف", "مصلحة المستخدمين", "المدير فقط", "لا أحد"], 1);
    addQuestion("ما هي الوثيقة التي تُقدّم عند المرض؟", ["عذر شفهي", "شهادة طبية", "بطاقة مريض", "وصفة دواء"], 1);
    addQuestion("ما هو الهدف من التكوين داخل المؤسسة؟", ["التسلية", "رفع الأداء والكفاءة", "مراقبة الموظفين", "تحديد العقوبات"], 1);
    addQuestion("من يتخذ القرار في نقل الموظف؟", ["رئيس المصلحة", "الموظف نفسه", "المديرية العامة", "الزملاء"], 2);
    addQuestion("ما هو التصرف الصحيح عند حدوث خطأ في العمل؟", ["إخفاؤه", "إبلاغ المسؤول فوراً", "إلقاء اللوم على الزملاء", "ترك العمل"], 1);
    addQuestion("من يقيّم أداء الموظف؟", ["زميله", "الزبائن", "المسؤول المباشر", "المدير العام"], 2);
    addQuestion("ما المقصود بالأمانة المهنية؟", ["احترام الزملاء", "المحافظة على ممتلكات المؤسسة والسرية", "اللباس الرسمي", "الرد على المكالمات"], 1);
    addQuestion("متى تُعتبر الغيابات غير مبررة؟", ["عند تقديم شهادة طبية", "بعد العطلة السنوية", "بدون تبرير أو وثيقة رسمية", "عند الوصول متأخراً"], 2);
    addQuestion("ما هو السلوك الإداري غير المقبول؟", ["العمل بروح الفريق", "استقبال الزبائن بأدب", "الرد بعنف على الزبائن", "الالتزام بالمهام"], 2);
    addQuestion("ما المقصود بالاتصال الإداري؟", ["مكالمات شخصية", "مراسلات بين الأعوان", "نقل المعلومات داخل المؤسسة", "البريد الإلكتروني فقط"], 2);
    addQuestion("أي من التالي يُعد وسيلة تواصل داخلية؟", ["الصحف الوطنية", "الإذاعة", "المذكرة الإدارية", "الإعلانات التجارية"], 2);
    addQuestion("كيف يُحسن الموظف علاقته مع زملائه؟", ["التعاون والاحترام", "العمل الفردي فقط", "الغيرة والمنافسة", "التحدث كثيرًا"], 0);
    addQuestion("ما هي العقوبة الإدارية الأخف؟", ["الطرد النهائي", "التوبيخ", "الخصم من الأجر", "التوقيف المؤقت"], 1);
    addQuestion("كيف يمكن تطوير الكفاءة المهنية؟", ["التكوين المستمر", "الراحة فقط", "العمل الروتيني", "تجاهل التعليمات"], 0);
    
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
        input.disabled = (timers[currentQuestion] <= 0 || answers[currentQuestion] !== null);
        if (answers[currentQuestion] === index) input.checked = true;

        input.addEventListener("change", () => {
          if (answers[currentQuestion] === null) {
            answers[currentQuestion] = index;
            disableChoices(); // غلق الاختيارات بعد الإجابة
          }
        });

        label.appendChild(input);
        label.appendChild(document.createTextNode(" " + choice));
        choicesDiv.appendChild(label);
      });

      // إخفاء زر السابق إذا كنا في السؤال الأول
      document.getElementById("prevBtn").style.display = currentQuestion === 0 ? "none" : "inline";

      startTimer();
    }

    function startTimer() {
      const timerElement = document.getElementById("timer");

      if (timers[currentQuestion] <= 0) {
        timerElement.textContent = "انتهى الوقت لهذا السؤال.";
        return;
      }

      timerElement.textContent = `الوقت المتبقي: ${timers[currentQuestion]} ثانية`;

      timerInterval = setInterval(() => {
        timers[currentQuestion]--;
        timerElement.textContent = `الوقت المتبقي: ${timers[currentQuestion]} ثانية`;

        if (timers[currentQuestion] <= 0) {
          clearInterval(timerInterval);
          timerElement.textContent = "انتهى الوقت لهذا السؤال.";
          disableChoices();
          setTimeout(nextQuestion, 1000); // ينتقل تلقائيًا بعد ثانية
        }
      }, 1000);
    }

    function disableChoices() {
      const inputs = document.querySelectorAll("input[name='choice']");
      inputs.forEach(input => input.disabled = true);
    }

    function nextQuestion() {
      if (currentQuestion < questions.length - 1) {
        currentQuestion++;
        showQuestion();
      } else {
        finishQuiz();
      }
    }

    function prevQuestion() {
      if (currentQuestion > 0 && answers[currentQuestion - 1] === null) {
        currentQuestion--;
        showQuestion();
      }
    }

    function finishQuiz() {
  clearInterval(timerInterval);
  document.getElementById("question-text").classList.add("hidden");
  document.getElementById("choices").classList.add("hidden");
  document.getElementById("timer").classList.add("hidden");
  document.getElementById("nav-buttons").classList.add("hidden");

  let correct = 0;
  let wrong = 0;

  questions.forEach((q, i) => {
    if (answers[i] !== null && timers[i] > 0) {
      if (answers[i] === q.correct) {
        correct++;
      } else {
        wrong++;
      }
    }
  });

  let finalScore = correct - wrong;
  if (finalScore < 0) finalScore = 0;

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
  `;
}

  </script>
</body>
</html>
