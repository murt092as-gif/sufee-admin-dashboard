<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ستوركا | StorKa</title>
    <style>
        :root {
            --primary-color: #6c5ce7;
            --bg-color: #0f0c29;
            --text-color: #ffffff;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(to right, #24243e, #302b63, #0f0c29);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            text-align: center;
        }

        .container {
            background: rgba(255, 255, 255, 0.1);
            padding: 2rem;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            width: 90%;
            max-width: 400px;
        }

        h1 { margin-bottom: 0.5rem; color: var(--primary-color); }
        
        .date-time { font-size: 0.9rem; margin-bottom: 2rem; opacity: 0.8; }

        .timer-box {
            font-size: 2.5rem;
            font-weight: bold;
            background: #ffffff15;
            padding: 1rem;
            border-radius: 15px;
            margin: 1rem 0;
            color: #fab1a0;
        }

        .points-display {
            font-size: 1.2rem;
            margin-top: 1rem;
        }

        .points-number {
            color: #55efc4;
            font-weight: bold;
            font-size: 1.5rem;
        }

        .btn-claim {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 10px 25px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1rem;
            margin-top: 1rem;
            transition: 0.3s;
        }

        .btn-claim:disabled { background-color: #636e72; cursor: not-allowed; }
    </style>
</head>
<body>

<div class="container">
    <h1>ستوركا - StorKa</h1>
    <div id="currentDateTime" class="date-time">جاري تحميل الوقت...</div>

    <hr style="border: 0.5px solid rgba(255,255,255,0.1)">

    <div class="points-display">نقاطك الحالية: <span id="userPoints" class="points-number">0</span></div>
    
    <div class="timer-box" id="countdown">04:00:00</div>
    
    <p>ستحصل على <span style="color:#55efc4">50 نقطة</span> عند انتهاء العداد</p>
    
    <button id="claimBtn" class="btn-claim" disabled>استلام النقاط</button>
</div>

<script>
    // إعدادات العداد (4 ساعات بالثواني)
    let totalSeconds = 4 * 60 * 60;
    let points = 0;

    function updateDateTime() {
        const now = new Date();
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit' };
        document.getElementById('currentDateTime').innerText = now.toLocaleDateString('ar-EG', options);
    }

    function startTimer() {
        const timerElement = document.getElementById('countdown');
        const claimBtn = document.getElementById('claimBtn');

        const interval = setInterval(() => {
            if (totalSeconds <= 0) {
                clearInterval(interval);
                timerElement.innerText = "00:00:00";
                claimBtn.disabled = false;
                claimBtn.innerText = "اضغط لاستلام 50 نقطة";
            } else {
                totalSeconds--;
                let hrs = Math.floor(totalSeconds / 3600);
                let mins = Math.floor((totalSeconds % 3600) / 60);
                let secs = totalSeconds % 60;

                timerElement.innerText = 
                    `${hrs.toString().padStart(2, '0')}:${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
            }
        }, 1000);
    }

    document.getElementById('claimBtn').addEventListener('click', function() {
        points += 50;
        document.getElementById('userPoints').innerText = points;
        this.disabled = true;
        this.innerText = "تم الاستلام!";
        // إعادة العداد (اختياري)
        totalSeconds = 4 * 60 * 60;
        startTimer();
    });

    // تشغيل الدوال عند التحميل
    setInterval(updateDateTime, 1000);
    updateDateTime();
    startTimer();
</script>

</body>
</html>
