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

  <div class="tab-section" id="tab-bloodtype">
    <div class="bmi-form-container">
      <h1>혈액형 확률 계산기</h1>
      <label>아버지 혈액형:
        <select id="fatherBlood">
          <option value="A">A형</option>
          <option value="B">B형</option>
          <option value="AB">AB형</option>
          <option value="O">O형</option>
        </select>
      </label>
      <label>어머니 혈액형:
        <select id="motherBlood">
          <option value="A">A형</option>
          <option value="B">B형</option>
          <option value="AB">AB형</option>
          <option value="O">O형</option>
        </select>
      </label>
      <div class="button-group">
        <button onclick="calculateBloodType()">계산하기</button>
      </div>
      <div class="result" id="bloodTypeResult"></div>
    </div>
  </div>

<script>
const bloodTypeMap = { 'A': ['A', 'O'], 'B': ['B', 'O'], 'AB': ['A', 'B'], 'O': ['O'] };
const childBloodPossibilities = { 'AA': ['A'], 'AO': ['A'], 'OA': ['A'], 'BB': ['B'], 'BO': ['B'], 'OB': ['B'], 'AB': ['A', 'B', 'AB'], 'BA': ['A', 'B', 'AB'], 'OO': ['O'] };
function calculateBloodType() {
  const father = document.getElementById("fatherBlood").value;
  const mother = document.getElementById("motherBlood").value;
  const fGenes = bloodTypeMap[father];
  const mGenes = bloodTypeMap[mother];
  const combinations = new Set();
  for (let fg of fGenes) {
    for (let mg of mGenes) {
      const key = fg + mg;
      const possible = childBloodPossibilities[key] || [];
      possible.forEach(p => combinations.add(p));
    }
  }
  const result = Array.from(combinations).sort().join(', ');
  document.getElementById("bloodTypeResult").innerText = `아이의 혈액형 가능성: ${result || '계산 불가'}`;
}
</script>
</body>
</html>
