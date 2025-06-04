<html lang="ko" translate="no">
<head>
  <meta charset="UTF-8">
  <title>건강 계산기</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 0 auto; max-width: 900px; padding: 2em; }
    .tab-buttons { display: flex; gap: 10px; margin-bottom: 20px; }
    .tab-btn { padding: 12px 20px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; background-color: #ccc; color: #333; transition: all 0.2s; }
    .tab-btn.active { background-color: #0059a5; color: white; }
    .tab-section { display: none; }
    .tab-section.active { display: block; }
    .vat-container { display: flex; flex-wrap: wrap; gap: 40px; background: #0059a5; padding: 30px; border-radius: 12px; color: white; }
    .vat-box { flex: 1; min-width: 300px; background: #006bbf; padding: 20px; border-radius: 12px; }
    .vat-box h3 { font-size: 18px; margin-bottom: 10px; }
    .vat-box label { display: block; margin-top: 10px; font-size: 14px; }
    .vat-box input { width: 100%; padding: 8px; margin-top: 5px; border-radius: 6px; border: none; font-size: 16px; }
    .vat-box button { margin-top: 15px; padding: 10px 15px; font-weight: bold; border: none; border-radius: 6px; background: white; color: #0059a5; cursor: pointer; }
    .vat-box .result { margin-top: 20px; background: #e0f2ff; padding: 10px; border-radius: 8px; color: #003b71; font-weight: bold; }

    .bmi-form-container {
      background: #f9f9f9;
      padding: 2em;
      border-radius: 12px;
      max-width: 600px;
      margin: auto;
    }
    .bmi-form-container h1 {
      font-size: 24px;
      font-weight: bold;
    }
    .bmi-form-container label {
      display: block;
      margin-top: 1em;
      font-weight: bold;
    }
    .bmi-form-container input,
    .bmi-form-container select {
      margin-top: 0.5em;
      width: 100%;
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }
    .button-group {
      display: flex;
      gap: 10px;
      margin-top: 20px;
    }
    .button-group button {
      flex: 1;
      padding: 10px;
      font-size: 16px;
      background: #eee;
      border: 1px solid #ccc;
      border-radius: 6px;
      cursor: pointer;
    }
    .result {
      margin-top: 1.5em;
      font-size: 1.2em;
      font-weight: bold;
      white-space: pre-line;
    }
    .blood-check-box {
      text-align: center;
      margin-top: 1.5em;
      border: 1px solid #999;
      padding: 1em;
      border-radius: 8px;
    }
    .blood-check-box .title {
      background: #dbe4ff;
      padding: 0.2em;
      font-weight: bold;
    }
    .blood-check-box .status {
      font-size: 3em;
      font-weight: bold;
      margin: 0.5em 0;
    }
    .blood-check-box.normal { background: #e0f0ff; }
    .blood-check-box.overweight { background: #ffffcc; }
    .blood-check-box.obese { background: #ffb3b3; }
  </style>
</head>
<body>

  <div class="tab-buttons">
    <button class="tab-btn active" onclick="switchTab(event, 'vat')">부가세계산기</button>
    <button class="tab-btn" onclick="switchTab(event, 'bmi')">학생 비만도 계산기</button>
    <button class="tab-btn" onclick="switchTab(event, 'bloodtype')">혈액형 확률 계산기</button>
  </div>

  <div class="tab-section active" id="tab-vat">
    <div class="vat-container">
      <div class="vat-box">
        <h3>합계금액을 알고 계신 경우</h3>
        <label>합계금액(총액)</label>
        <input id="totalInput" type="number" placeholder="총금액 입력 (원)">
        <div class="button-group">
          <button onclick="calcFromTotal()">계산하기</button>
          <button onclick="resetVatForm()">다시하기</button>
        </div>
        <div class="result" id="totalResult"></div>
      </div>
      <div class="vat-box">
        <h3>공급가액을 알고 계신 경우</h3>
        <label>공급가액</label>
        <input id="supplyInput" type="number" placeholder="공급가액 입력 (원)">
        <div class="button-group">
          <button onclick="calcFromSupply()">계산하기</button>
          <button onclick="resetVatForm()">다시하기</button>
        </div>
        <div class="result" id="supplyResult"></div>
      </div>
    </div>
    <footer style="margin-top: 3em; text-align: center;">
      <p style="color: #555; font-size: 1.8em; font-weight: bold;">종합검진 예약은 검진라인에서 간편하게!</p>
      <a href="https://www.sjcore.co.kr" target="_blank" style="display: inline-block; padding: 10px 20px; background-color: #0059ff; color: white; border-radius: 6px; text-decoration: none; font-weight: bold; margin-top: 0.5em;">검진라인 바로가기</a>
    </footer>
  </div>
  
  <div class="tab-section" id="tab-bmi">
    <div class="bmi-form-container">
      <h1>BMI 판정기 (학생용)</h1>
      <label>성별:
        <select id="gender">
          <option value="male">남자</option>
          <option value="female">여자</option>
        </select>
      </label>
      <label>생년월일:
        <input type="date" id="birthdate">
      </label>
      <label>키 (cm):
        <input type="number" id="height">
      </label>
      <label>몸무게 (kg):
        <input type="number" id="weight">
      </label>
      <div class="button-group">
        <button onclick="calculateBMI()">계산하기</button>
        <button onclick="resetForm()">다시하기</button>
      </div>
      <div class="result" id="bmiResult"></div>
      <div class="blood-check-box" id="bloodCheckBox" style="display:none">
        <div class="title">혈액검사 여부</div>
        <div class="status" id="bloodStatus"></div>
        <div class="label" id="bloodLabel"></div>
      </div>
    </div>
    <footer style="margin-top: 3em; text-align: center;">
      <p style="color: #555; font-size: 1.8em; font-weight: bold;">종합검진 예약은 검진라인에서 간편하게!</p>
      <a href="https://www.sjcore.co.kr" target="_blank" style="display: inline-block; padding: 10px 20px; background-color: #0059ff; color: white; border-radius: 6px; text-decoration: none; font-weight: bold; margin-top: 0.5em;">검진라인 바로가기</a>
    </footer>
  </div>

<script>
  function switchTab(event, type) {
    const tabs = document.querySelectorAll(".tab-section");
    const buttons = document.querySelectorAll(".tab-btn");
    tabs.forEach(tab => tab.classList.remove("active"));
    buttons.forEach(btn => btn.classList.remove("active"));
    document.getElementById("tab-" + type).classList.add("active");
    event.currentTarget.classList.add("active");
  }

  function formatWon(value) {
    return value.toLocaleString('ko-KR') + ' 원';
  }

  function calcFromTotal() {
    const total = parseFloat(document.getElementById("totalInput").value);
    if (isNaN(total) || total <= 0) {
      document.getElementById("totalResult").innerText = "올바른 총금액을 입력하세요.";
      return;
    }
    const supply = total / 1.1;
    const tax = total - supply;
    document.getElementById("totalResult").innerHTML = `공급가액: ${formatWon(Math.floor(supply))}<br>부가세액: ${formatWon(Math.floor(tax))}`;
  }

  function calcFromSupply() {
    const supply = parseFloat(document.getElementById("supplyInput").value);
    if (isNaN(supply) || supply <= 0) {
      document.getElementById("supplyResult").innerText = "올바른 공급가액을 입력하세요.";
      return;
    }
    const tax = supply * 0.1;
    const total = supply + tax;
    document.getElementById("supplyResult").innerHTML = `부가세액: ${formatWon(Math.floor(tax))}<br>합계금액: ${formatWon(Math.floor(total))}`;
  }

  function resetVatForm() {
    document.getElementById("totalInput").value = "";
    document.getElementById("supplyInput").value = "";
    document.getElementById("totalResult").innerHTML = "";
    document.getElementById("supplyResult").innerHTML = "";
  }

  const bmiTable = {
    male: {6: 18.8, 7: 19.6, 8: 20.7, 9: 22.0, 10: 23.1, 11: 24.2, 12: 25.1, 13: 25.7, 14: 26.0, 15: 26.2, 16: 26.4, 17: 26.6, 18: 26.9},
    female: {6: 18.7, 7: 19.5, 8: 20.4, 9: 21.5, 10: 22.4, 11: 23.3, 12: 24.1, 13: 24.8, 14: 25.2, 15: 25.4, 16: 25.5, 17: 25.5, 18: 25.5}
  };

  function getAge(birthdateStr) {
    const birthdate = new Date(birthdateStr);
    const today = new Date();
    let age = today.getFullYear() - birthdate.getFullYear();
    const m = today.getMonth() - birthdate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthdate.getDate())) {
      age--;
    }
    return age;
  }

  function calculateBMI() {
    const gender = document.getElementById("gender").value;
    const birthdateStr = document.getElementById("birthdate").value;
    const height = parseFloat(document.getElementById("height").value);
    const weight = parseFloat(document.getElementById("weight").value);
    const age = getAge(birthdateStr);

    const resultDiv = document.getElementById("bmiResult");
    const box = document.getElementById("bloodCheckBox");
    const status = document.getElementById("bloodStatus");
    const label = document.getElementById("bloodLabel");

    if (!gender || !birthdateStr || !height || !weight || age < 6 || age > 18) {
      resultDiv.innerText = "입력을 정확히 해주세요 (만나이 6~18세).";
      box.style.display = "none";
      return;
    }

    const heightMeters = height / 100;
    const bmi = Math.round((weight / (heightMeters * heightMeters)) * 10) / 10;
    const threshold = bmiTable[gender][age];

    let resultText = `만나이 ${age}세
BMI: ${bmi} → `;
    box.style.display = "block";

    if (bmi >= threshold) {
      resultText += "비만 (혈액검사 필요)";
      box.className = "blood-check-box obese";
      status.innerText = "○";
      label.innerText = "비만";
    } else if (bmi >= 85 / 100 * threshold) {
      resultText += "과체중";
      box.className = "blood-check-box overweight";
      status.innerText = "×";
      label.innerText = "과체중";
    } else if (bmi >= 5 / 100 * threshold) {
      resultText += "정상";
      box.className = "blood-check-box normal";
      status.innerText = "×";
      label.innerText = "정상";
    } else {
      resultText += "저체중";
      box.className = "blood-check-box normal";
      status.innerText = "×";
      label.innerText = "저체중";
    }

    resultDiv.innerText = resultText;
  }

  function resetForm() {
    document.getElementById("gender").value = "male";
    document.getElementById("birthdate").value = "";
    document.getElementById("height").value = "";
    document.getElementById("weight").value = "";
    document.getElementById("bmiResult").innerText = "";
    document.getElementById("bloodCheckBox").style.display = "none";
  }
</script>
</div>

<!-- 혈액형 확률 계산기 섹션 -->
<div class="tab-section" id="tab-bloodtype">
  <div class="bmi-form-container">
    <h1>혈액형 확률 계산기</h1>
    <label>아버지 혈액형:
      <select id="father">
        <option value="A">A형</option>
        <option value="B">B형</option>
        <option value="AB">AB형</option>
        <option value="O">O형</option>
      </select>
    </label>
    <label>어머니 혈액형:
      <select id="mother">
        <option value="A">A형</option>
        <option value="B">B형</option>
        <option value="AB">AB형</option>
        <option value="O">O형</option>
      </select>
    </label>
    <div class="button-group">
      <button onclick="calcBloodType()">계산하기</button>
      <button onclick="resetBloodForm()">다시하기</button>
    </div>
    <div class="result" id="bloodResult"></div>
  </div>
</div>

<script>
function calcBloodType() {
  const father = document.getElementById("father").value;
  const mother = document.getElementById("mother").value;
  const result = document.getElementById("bloodResult");

  const combinations = {
    'AA': {A: 75, O: 25}, 'AO': {A: 50, O: 50},
    'BB': {B: 75, O: 25}, 'BO': {B: 50, O: 50},
    'AB': {A: 25, B: 25, AB: 50}, 'OO': {O: 100},
    'A+B': {A: 25, B: 25, AB: 25, O: 25},
    'A+AB': {A: 25, B: 25, AB: 50},
    'A+O': {A: 50, O: 50},
    'B+AB': {A: 25, B: 25, AB: 50},
    'B+O': {B: 50, O: 50},
    'AB+O': {A: 50, B: 50},
    'AB+AB': {A: 25, B: 25, AB: 50},
    'O+O': {O: 100},
    'A+A': {A: 75, O: 25},
    'B+B': {B: 75, O: 25},
    'A+B': {A: 25, B: 25, AB: 25, O: 25}
  };

  const key1 = `${father}+${mother}`;
  const key2 = `${mother}+${father}`;
  const output = combinations[key1] || combinations[key2];

  if (!output) {
    result.innerText = "조합을 찾을 수 없습니다.";
    return;
  }

  let text = "자녀의 혈액형 가능성:\n";
  for (const [type, percent] of Object.entries(output)) {
    text += `- ${type}형: ${percent}%\n`;
  }
  result.innerText = text;
}

function resetBloodForm() {
  document.getElementById("father").value = "A";
  document.getElementById("mother").value = "A";
  document.getElementById("bloodResult").innerText = "";
}
</script>

</body>
</html>
