# Timer
Timertodo
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-scale, initial-scale=1.0">
  <title>iOS-Style Timer with To-Do List</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f3f4f6;
      margin: 0;
    }

    .container {
      background-color: #fff;
      border-radius: 20px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      padding: 40px;
      text-align: center;
      width: 350px;
    }

    .timer-display {
      font-size: 48px;
      font-weight: bold;
      color: #333;
      margin-bottom: 20px;
      padding: 20px;
      background-color: #f1f1f1;
      border-radius: 10px;
    }

    .button {
      background-color: #4CAF50;
      color: white;
      font-size: 18px;
      padding: 15px 30px;
      border-radius: 30px;
      border: none;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    .button:hover {
      background-color: #45a049;
    }

    .count-container {
      margin-top: 20px;
      font-size: 16px;
      color: #666;
    }

    .time-picker {
      font-size: 22px;
      margin: 10px 0;
      padding: 10px;
      width: 100px;
      border-radius: 10px;
      border: 1px solid #ddd;
      background-color: #f9f9f9;
    }

    .todo-table {
      margin-top: 30px;
      width: 100%;
      border-collapse: collapse;
    }

    .todo-table th, .todo-table td {
      padding: 10px;
      text-align: left;
      border: 1px solid #ddd;
    }

    .todo-table th {
      background-color: #4CAF50;
      color: white;
    }

    .todo-table td {
      background-color: #f9f9f9;
    }

    .todo-table tr:nth-child(even) td {
      background-color: #f1f1f1;
    }

    .emoji {
      font-size: 30px;
      margin-right: 10px;
    }

  </style>
</head>
<body>

<div class="container">
  <div class="timer-display" id="timerDisplay">00:00</div>

  <div>
    <input type="number" id="minutes" class="time-picker" placeholder="Minutes" min="0" max="60">
    <input type="number" id="seconds" class="time-picker" placeholder="Seconds" min="0" max="59">
  </div>

  <button class="button" id="startStopButton" onclick="startStopTimer()">⏳ Start Timer</button>

  <div class="count-container" id="countDisplay">
    Timer Set: 0 times 🕑
  </div>

  <!-- To-Do List Table -->
  <h3>📝 To-Do List</h3>
  <table class="todo-table">
    <thead>
      <tr>
        <th>Task</th>
        <th>Status</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Complete Timer Project</td>
        <td>✔️ Done</td>
      </tr>
      <tr>
        <td>Start studying for exams</td>
        <td>❌ Pending</td>
      </tr>
      <tr>
        <td>Go for a walk</td>
        <td>❌ Pending</td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Tone Sound -->
<audio id="timerTone" src="https://www.soundjay.com/button/beep-07.wav"></audio>

<script>
  let timer;
  let isRunning = false;
  let timerCount = 0;
  let remainingTime = 0;

  // Start/Stop timer function
  function startStopTimer() {
    const minutesInput = document.getElementById('minutes');
    const secondsInput = document.getElementById('seconds');
    const startStopButton = document.getElementById('startStopButton');

    if (!isRunning) {
      const minutes = parseInt(minutesInput.value) || 0;
      const seconds = parseInt(secondsInput.value) || 0;

      if (minutes > 0 || seconds > 0) {
        remainingTime = (minutes * 60) + seconds;
        updateTimerDisplay();

        // Start the timer
        timer = setInterval(function() {
          if (remainingTime > 0) {
            remainingTime--;
            updateTimerDisplay();
          } else {
            clearInterval(timer);
            playTone(); // Play tone when timer finishes
            alert("⏱ Timer Finished!");
            isRunning = false;
            startStopButton.textContent = "⏳ Start Timer";
          }
        }, 1000);

        // Update the button text and set the timer state
        isRunning = true;
        startStopButton.textContent = "⛔ Stop Timer";

        // Increment timer set count
        timerCount++;
        updateCountDisplay();
      }
    } else {
      clearInterval(timer);
      isRunning = false;
      startStopButton.textContent = "⏳ Start Timer";
    }
  }

  // Update the timer display (MM:SS)
  function updateTimerDisplay() {
    const minutes = Math.floor(remainingTime / 60);
    const seconds = remainingTime % 60;
    document.getElementById('timerDisplay').textContent = formatTime(minutes) + ':' + formatTime(seconds);
  }

  // Format time with leading zeros
  function formatTime(time) {
    return time < 10 ? '0' + time : time;
  }

  // Update the timer set count display
  function updateCountDisplay() {
    document.getElementById('countDisplay').textContent = `Timer Set: ${timerCount} times 🕑`;
  }

  // Function to play the tone
  function playTone() {
    const tone = document.getElementById('timerTone');
    tone.play(); // Play the tone when the timer finishes
  }
</script>

</body>
</html>
