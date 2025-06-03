<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>종합검진 선택검사 추천기</title>
  <style>
    body { font-family: sans-serif; max-width: 600px; margin: auto; padding: 2rem; }
    h1 { color: #2c3e50; }
    label { display: block; margin-top: 1rem; }
    select, input { width: 100%; padding: 0.5rem; margin-top: 0.5rem; }
    button { margin-top: 1.5rem; padding: 0.8rem 1.2rem; background: #27ae60; color: white; border: none; cursor: pointer; }
    .result { margin-top: 2rem; background: #ecf0f1; padding: 1rem; border-radius: 8px; }
  </style>
</head>
<body>
  <h1>종합검진 선택검사 추천기</h1>

  <label>나이:
    <input type="number" id="age" min="0" max="120">
  </label>

  <label>성별:
    <select id="gender">
      <option>남성</option>
      <option>여성</option>
    </select>
  </label>

  <label>본인 질환 (여러 개 선택 가능):
    <select id="diseases" multiple size="5">
      <option>고혈압</option>
      <option>당뇨</option>
      <option>고지혈증</option>
      <option>간질환</option>
      <option>폐질환</option>
    </select>
  </label>

  <label>가족력 (여러 개 선택 가능):
    <select id="family" multiple size="5">
      <option>심장질환</option>
      <option>폐암</option>
      <option>자궁경부암</option>
      <option>간암</option>
    </select>
  </label>

  <button onclick="recommend()">검사 추천 보기</button>

  <div class="result" id="result"></div>

  <script>
    function getSelectedValues(select) {
      return Array.from(select.selectedOptions).map(opt => opt.value);
    }

    function recommend() {
      const age = parseInt(document.getElementById('age').value);
      const gender = document.getElementById('gender').value;
      const diseases = getSelectedValues(document.getElementById('diseases'));
      const family = getSelectedValues(document.getElementById('family'));
      const resultBox = document.getElementById('result');

      const recommendations = new Set();

      if (age >= 50) recommendations.add("대장내시경");
      if (age >= 40) recommendations.add("위내시경");
      if (age >= 55) recommendations.add("저선량 폐CT");

      if (gender === "여성" && (family.includes("자궁경부암") || age >= 20)) {
        recommendations.add("자궁경부세포검사");
        recommendations.add("골반 초음파");
      }

      if (gender === "남성" && age >= 50) {
        recommendations.add("전립선 초음파");
      }

      if (diseases.includes("고지혈증") || family.includes("심장질환")) {
        recommendations.add("심장초음파");
        recommendations.add("심장 CT");
      }

      if (diseases.includes("간질환")) {
        recommendations.add("복부 초음파");
        recommendations.add("간 MRI");
      }

      if (diseases.includes("폐질환") || family.includes("폐암")) {
        recommendations.add("흉부 CT");
      }

      if (diseases.includes("당뇨")) {
        recommendations.add("망막 OCT");
      }

      if (recommendations.size === 0) {
        resultBox.innerHTML = "<strong>추천할 장비 기반 검사가 없습니다.</strong>";
      } else {
        resultBox.innerHTML = "<strong>추천 검사 항목:</strong><ul>" +
          Array.from(recommendations).map(item => `<li>${item}</li>`).join("") +
          "</ul>";
      }
    }
  </script>
</body>
</html>

  </script>
</body>
</html>
