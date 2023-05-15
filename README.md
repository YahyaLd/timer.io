# timer.github.io







<!DOCTYPE html>
<html>
<head>
    <title>Online Kronometre</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }

        .timer-container {
            position: relative;
            height: 100vh;
            background-color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .timer {
            font-size: 72px;
            color: #000;
        }

        .button {
            display: inline-block;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: #fff;
            font-size: 18px;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }

        .expand-button {
            position: absolute;
            right: 10px;
            bottom: 10px;
            font-size: 24px;
            background-color: transparent;
            border: none;
            cursor: pointer;
            color: #4CAF50;
        }

        .fullscreen-container {
            background-color: #fff;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .fullscreen-timer {
            font-size: 72px;
            color: #000;
        }
    </style>
</head>
<body>
    <h1>Online Kronometre</h1>
    <p>Kronometrenin kaç saat, dakika ve saniye süreceğini belirtin:</p>
    <input type="number" id="hours" placeholder="Saat" min="0">
    <input type="number" id="minutes" placeholder="Dakika" min="0" max="59">
    <input type="number" id="seconds" placeholder="Saniye" min="0" max="59">
    <button class="button" onclick="startTimer()">Başlat</button>
    <div class="timer-container">
        <div class="timer" id="timer">00:00:00</div>
        <button class="expand-button" onclick="toggleFullScreen()">&#128470;</button>
    </div>
    <button class="button" onclick="stopTimer()">Durdur</button>
    <button class="button" onclick="resetTimer()">Sıfırla</button>
    <script>
        var hoursInput = document.getElementById('hours');
        var minutesInput = document.getElementById('minutes');
        var secondsInput = document.getElementById('seconds');
        var timerContainer = document.querySelector('.timer-container');
        var timerDisplay = document.getElementById('timer');
        var intervalId;
        var endTime;
        var elapsedTime = 0;
        var isRunning = false;
        var isFullScreen = false;

        function startTimer() {
            var hours = parseInt(hoursInput.value);
            var minutes = parseInt(minutesInput.value);
            var seconds = parseInt(secondsInput.value);
            if (isNaN(hours) || isNaN(minutes) || isNaN(seconds) || hours < 0 || minutes < 0 || seconds < 0) {
                alert("Geçerli bir süre girin!");
                return;
            }
            if (isRunning) {
                alert("Kronometre zaten çalışıyor!");
                return;
            }
            isRunning = true;
            var totalSeconds = (hours * 3600) + (minutes * 60) + seconds;
            endTime = Date.now() + (totalSeconds * 1000);
            intervalId = setInterval(updateTimer, 1000);
        }

        function updateTimer() {
            var remainingTime = endTime - Date.now();
            if (remainingTime <= 0) {
                clearInterval(intervalId);
                isRunning = false;
                timerDisplay.textContent = "Süre Doldu!";
            } else {
                var time = calculateTime(remainingTime);
                timerDisplay.textContent = time;
            }
        }

        function calculateTime(duration) {
            var hours = Math.floor(duration / (60 * 60 * 1000));
            var minutes = Math.floor((duration % (60 * 60 * 1000)) / (60 * 1000));
            var seconds = Math.floor((duration % (60 * 1000)) / 1000);
            return formatTime(hours) + ":" + formatTime(minutes) + ":" + formatTime(seconds);
        }

        function formatTime(time) {
            return time < 10 ? "0" + time : time;
        }

        function stopTimer() {
            if (isRunning) {
                clearInterval(intervalId);
                isRunning = false;
                elapsedTime += Date.now() - startTime;
            }
        }

        function resetTimer() {
            clearInterval(intervalId);
            isRunning = false;
            hoursInput.value = "";
            minutesInput.value = "";
            secondsInput.value = "";
            timerDisplay.textContent = "00:00:00";
            elapsedTime = 0;
        }
    </script>
</body>
</html>
    <script>
        function toggleFullScreen() {
            if (!isFullScreen) {
                if (timerContainer.requestFullscreen) {
                    timerContainer.requestFullscreen();
                } else if (timerContainer.mozRequestFullScreen) { // Firefox
                    timerContainer.mozRequestFullScreen();
                } else if (timerContainer.webkitRequestFullscreen) { // Chrome, Safari and Opera
                    timerContainer.webkitRequestFullscreen();
                } else if (timerContainer.msRequestFullscreen) { // IE/Edge
                    timerContainer.msRequestFullscreen();
                }
                isFullScreen = true;
                timerContainer.classList.add('fullscreen-container');
                timerDisplay.classList.add('fullscreen-timer');
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.mozCancelFullScreen) { // Firefox
                    document.mozCancelFullScreen();
                } else if (document.webkitExitFullscreen) { // Chrome, Safari and Opera
                    document.webkitExitFullscreen();
                } else if (document.msExitFullscreen) { // IE/Edge
                    document.msExitFullscreen();
                }
                isFullScreen = false;
                timerContainer.classList.remove('fullscreen-container');
                timerDisplay.classList.remove('fullscreen-timer');
            }
        }
    </script>
</body>
</html>
