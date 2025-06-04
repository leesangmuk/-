<!DOCTYPE html>
<html lang="ko" translate="no">
<head>
  <meta charset="UTF-8">
  <title>검사 추천 시스템</title>
  <style>
    body {
      font-family: 'Noto Sans KR', sans-serif;
      padding: 2em;
      max-width: 900px;
      margin: auto;
      background-color: #f2f6fc;
    }
    h1 {
      font-size: 32px;
      margin-bottom: 1em;
      color: #0059a5;
      text-align: center;
    }
    .section {
      background: white;
      padding: 1.5em;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
      margin-bottom: 2em;
    }
    label {
      display: block;
      margin-top: 0.8em;
      font-size: 16px;
      color: #333;
    }
    input[type="number"], select {
      padding: 8px;
      margin-top: 0.3em;
      width: 100%;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }
    .result-box {
      background: #e0f2ff;
      padding: 1.5em;
      border-radius: 10px;
      margin-top: 2em;
      font-weight: bold;
      color: #003366;
      white-space: pre-line;
    }
    button {
      margin-top: 1.5em;
      padding: 12px 20px;
      font-size: 18px;
      background-color: #0059a5;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      display: block;
      width: 100%;
    }
  </style>
</head>
<body>
  <h1>🩺 건강검진 항목 추천 시스템</h1>

  <div class="section">
    <label>나이: <input type="number" id="age" placeholder="예: 45"></label>
    <label>성별:
      <select id="gender">
        <option value="">선택</option>
        <option value="남자">남자</option>
        <option value="여자">여자</option>
      </select>
    </label>
    <label><input type="checkbox" id="smoking"> 흡연</label>
    <label><input type="checkbox" id="drinking"> 음주</label>
  </div>

  <div class="section">
    <h3>가족력</h3>
    <label><input type="checkbox" value="뇌졸중"> 뇌졸중</label>
    <label><input type="checkbox" value="심근경색"> 심근경색</label>
    <label><input type="checkbox" value="고혈압"> 고혈압</label>
    <label><input type="checkbox" value="간염"> 간염</label>
  </div>

  <div class="section">
    <h3>본인 병력</h3>
    <label><input type="checkbox" value="유방암"> 유방암</label>
    <label><input type="checkbox" value="위염"> 위염</label>
    <label><input type="checkbox" value="허리디스크"> 허리디스크</label>
    <label><input type="checkbox" value="무릎관절염"> 무릎관절염</label>
    <label><input type="checkbox" value="고혈압"> 고혈압</label>
    <label><input type="checkbox" value="당뇨"> 당뇨</label>
    <label><input type="checkbox" value="고지혈증"> 고지혈증</label>
  </div>

  <button onclick="recommendTests()">📋 검사 추천받기</button>

  <div id="recommendResult" class="result-box"></div>

  <script>
    const diseaseTestMap = {
      '뇌졸중': ['뇌 MRA', '뇌 MRI'],
      '심근경색': ['심장초음파', '관상동맥CT'],
      '고혈압': ['심장초음파'],
      '간염': ['상복부초음파'],
      '유방암': ['유방초음파'],
      '위염': ['위내시경'],
      '허리디스크': ['요추 MRI'],
      '무릎관절염': ['관절CT'],
      '당뇨': ['공복혈당 검사', '당화혈색소'],
      '고지혈증': ['지질검사']
    };

    function recommendTests() {
      const age = parseInt(document.getElementById("age").value);
      const gender = document.getElementById("gender").value;
      const isSmoking = document.getElementById("smoking").checked;
      const isDrinking = document.getElementById("drinking").checked;

      const checks = document.querySelectorAll("input[type='checkbox']:checked");
      const selected = [...checks]
        .map(cb => cb.value)
        .filter(val => val && val !== "on");

      const recommended = new Set();

      selected.forEach(disease => {
        const tests = diseaseTestMap[disease];
        if (tests) tests.forEach(t => recommended.add(t));
      });

      if (age >= 50) recommended.add("대장내시경");
      if (gender === "여자" && age >= 40) recommended.add("유방초음파");
      if (gender === "남자" && age >= 50) recommended.add("전립선초음파");

      if (isSmoking) recommended.add("저선량 폐CT");
      if (isDrinking) recommended.add("간기능 혈액검사 및 상복부초음파");

      const box = document.getElementById("recommendResult");
      if (recommended.size === 0) {
        box.innerText = "추천할 검사가 없습니다. 조건을 선택해주세요.";
      } else {
        box.innerText = "추천 검사 항목:\n- " + [...recommended].join("\n- ");
      }
    }
  </script>
</body>
</html>

