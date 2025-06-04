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
      font-size: 16px;
      color: #333;
      margin-top: 0.5em;
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
    .inline-box {
      display: flex;
      gap: 1.5em;
      margin-top: 1em;
      flex-wrap: wrap;
    }
    .inline-box label {
      display: flex;
      align-items: center;
      gap: 0.5em;
      background: #eef3fb;
      padding: 0.5em 1em;
      border-radius: 8px;
      cursor: pointer;
    }
    .horizontal-box {
      display: flex;
      flex-wrap: wrap;
      gap: 1em 2em;
    }
    .horizontal-box label {
      display: flex;
      align-items: center;
      gap: 0.4em;
      white-space: nowrap;
    }
    label::before, label::after {
      content: none !important;
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
    <div class="inline-box">
      <label><input type="checkbox" id="smoking"> 흡연</label>
      <label><input type="checkbox" id="drinking"> 음주</label>
    </div>
  </div>

  <div class="section">
    <h3>가족력</h3>
    <div class="horizontal-box">
      <label><input type="checkbox" value="간장질환"> 간장질환</label>
      <label><input type="checkbox" value="고혈압"> 고혈압</label>
      <label><input type="checkbox" value="뇌졸중"> 뇌졸중</label>
      <label><input type="checkbox" value="심장병"> 심장병</label>
      <label><input type="checkbox" value="당뇨병"> 당뇨병</label>
    </div>
  </div>

  <div class="section">
    <h3>본인 병력</h3>
    <div class="horizontal-box">
      <label><input type="checkbox" value="결핵"> 결핵</label>
      <label><input type="checkbox" value="간염"> 간염</label>
      <label><input type="checkbox" value="간장질환"> 간장질환</label>
      <label><input type="checkbox" value="고혈압"> 고혈압</label>
      <label><input type="checkbox" value="심장병"> 심장병</label>
      <label><input type="checkbox" value="뇌졸중"> 뇌졸중</label>
      <label><input type="checkbox" value="당뇨병"> 당뇨병</label>
      <label><input type="checkbox" value="위염"> 위염</label>
    </div>
  </div>

  <button onclick="recommendTests()">📋 검사 추천받기</button>

  <div id="recommendResult" class="result-box"></div>

  <script>
    const diseaseTestMap = {
      '간장질환': ['간기능 혈액검사', '상복부초음파'],
      '고혈압': ['심장초음파'],
      '뇌졸중': ['뇌 MRA', '뇌 MRI'],
      '심장병': ['심장초음파', '관상동맥CT'],
      '당뇨병': ['공복혈당 검사', '당화혈색소'],
      '간염': ['간기능 혈액검사', '상복부초음파'],
      '결핵': ['흉부 X선'],
      '위염': ['위내시경']
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
    </div>
    <footer style="margin-top: 3em; text-align: center;">
      <p style="color: #555; font-size: 1.8em; font-weight: bold;">종합검진 예약은 검진라인에서 간편하게!</p>
      <a href="https://www.sjcore.co.kr" target="_blank" style="display: inline-block; padding: 10px 20px; background-color: #0059ff; color: white; border-radius: 6px; text-decoration: none; font-weight: bold; margin-top: 0.5em;">검진라인 바로가기</a>
    </footer>


