<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dr. Martin Luther King, Jr. Elementary House Sorting Spinner</title>
  <style>
    body {
      text-align: center;
      font-family: Arial, sans-serif;
      background-color: #f5f5f5;
      padding: 50px;
    }
    /* Wheel container */
    #wheel {
      position: relative;
      width: 300px;
      height: 300px;
      margin: 30px auto;
      border-radius: 50%;
      overflow: hidden;
      transform: rotate(0deg);
      transition: transform 4s ease-out;
      display: none;
    }
    /* Four segments */
    .segment {
      position: absolute;
      width: 50%;
      height: 50%;
      top: 50%;
      left: 50%;
      transform-origin: 0% 0%;
      background-size: cover;
      background-position: center;
    }
    .segment.amistad   { transform: rotate(0deg)   skewY(-60deg); }
    .segment.isibindi  { transform: rotate(90deg)  skewY(-60deg); }
    .segment.altruismo { transform: rotate(180deg) skewY(-60deg); }
    .segment.reveur    { transform: rotate(270deg) skewY(-60deg); }
    #result, #crest {
      margin-top: 30px;
      font-size: 28px;
      font-weight: bold;
    }
    #crest img {
      width: 200px;
      height: auto;
      display: block;
      margin: 20px auto;
    }
    #next {
      display: none;
      margin-top: 30px;
    }
    select, button {
      font-size: 16px;
      padding: 10px;
      margin: 10px;
    }
  </style>
</head>
<body>
  <h1>Dr. Martin Luther King, Jr. Elementary Staff House Sorting</h1>
  <select id="staff">
    <option value="" disabled selected>Select a staff member</option>
  </select><br>

  <button id="spinBtn">Spin the Wheel</button>
  <div id="wheel">
    <div class="segment amistad"   style="background-image:url('sandbox:/mnt/data/913408A3-0815-4857-8C3F-A307E28FB7C4.jpeg');"></div>
    <div class="segment isibindi"  style="background-image:url('sandbox:/mnt/data/36E493A9-B0E6-4F12-9F19-F4DE5A03770A.jpeg');"></div>
    <div class="segment altruismo" style="background-image:url('sandbox:/mnt/data/D51EF88E-0AD8-4615-9BA2-08B7C755C7FE.jpeg');"></div>
    <div class="segment reveur"    style="background-image:url('sandbox:/mnt/data/AAC92329-D548-4947-8FCA-18268137D92A.jpeg');"></div>
  </div>

  <div id="result"></div>
  <div id="crest"></div>
  <button id="next" onclick="resetAll()">Next Staff Member</button>

  <audio id="tick" src="https://www.soundjay.com/button/beep-07.wav" preload="auto"></audio>
  <audio id="win" src="https://www.soundjay.com/human/cheering-01.wav" preload="auto"></audio>

  <script>
    const staffAssignments = {
      "Hutchings": "Altruismo", "Curry": "Altruismo", /* ... etc ... */
    };
    const houseDetails = {
      "Amistad":   { color: "#e74c3c", crestUrl: 'sandbox:/mnt/data/913408A3-0815-4857-8C3F-A307E28FB7C4.jpeg' },
      "Isibindi":  { color: "#27ae60", crestUrl: 'sandbox:/mnt/data/36E493A9-B0E6-4F12-9F19-F4DE5A03770A.jpeg' },
      "Altruismo": { color: "#2c3e50", crestUrl: 'sandbox:/mnt/data/D51EF88E-0AD8-4615-9BA2-08B7C755C7FE.jpeg' },
      "Reveur":    { color: "#2980b9", crestUrl: 'sandbox:/mnt/data/AAC92329-D548-4947-8FCA-18268137D92A.jpeg' }
    };

    // Populate dropdown
    window.onload = () => {
      const dropdown = document.getElementById('staff');
      Object.keys(staffAssignments).sort().forEach(name => {
        const opt = document.createElement('option');
        opt.value = name;
        opt.textContent = name;
        dropdown.appendChild(opt);
      });
    };

    document.getElementById('spinBtn').addEventListener('click', startSpin);
    
    function startSpin() {
      const select = document.getElementById('staff');
      const staff = select.value;
      if (!staff) return alert('Please select a staff member.');
      const house = staffAssignments[staff];
      const wheel = document.getElementById('wheel');
      const tick = document.getElementById('tick');
      const win = document.getElementById('win');

      wheel.style.display = 'block';
      wheel.style.transform = 'rotate(0deg)';
      document.getElementById('result').textContent = '';
      document.getElementById('crest').innerHTML = '';
      document.getElementById('next').style.display = 'none';

      // Play tick once at start
      tick.volume = 0.5;
      tick.currentTime = 0;
      tick.play();

      // Determine spin angle: 1440deg (4 spins) + target offset
      const offsets = { Amistad: 45, Isibindi: 135, Altruismo: 225, Reveur: 315 };
      const angle = 1440 + offsets[house];
      wheel.style.transition = 'transform 4s ease-out';
      wheel.style.transform = `rotate(${angle}deg)`;

      setTimeout(() => {
        const details = houseDetails[house];
        document.body.style.backgroundColor = details.color;
        document.getElementById('result').textContent = `${staff} → House ${house}!`;
        document.getElementById('crest').innerHTML = `<img src='${details.crestUrl}' alt='${house} crest'>`;
        win.volume = 0.8;
        win.currentTime = 0;
        win.play();
        document.getElementById('next').style.display = 'inline-block';
      }, 4100);
    }

    function resetAll() {
      const wheel = document.getElementById('wheel');
      wheel.style.display = 'none';
      document.body.style.backgroundColor = '#f5f5f5';
      document.getElementById('staff').selectedIndex = 0;
      document.getElementById('result').textContent = '';
      document.getElementById('crest').innerHTML = '';
    }
  </script>
</body>
</html>
