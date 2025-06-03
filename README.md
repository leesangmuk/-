<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>종합검진 추천</title>
  <style>
    body { font-family: sans-serif; padding: 20px; max-width: 600px; margin: auto; }
    label { display: block; margin-top: 10px; }
    button { margin-top: 20px; }
    .result { margin-top: 20px; background: #f0f0f0; padding: 10px; border-radius: 6px; }
  </style>
</head>
<body>
  <h1>종합검진 선택검사 추천</h1>

  <label>나이:
    <input type="number" id="age" min="0" max="120">
  </label>

  <label>성별:
    <select id="gender">
      <option>남성</option>
      <option>여성</option>
    </select>
  </label>

  <label>본인 질환:
    <select id="diseases" multiple size="5">
      <option>고혈압</option>
      <option>당뇨</option>
      <option>고지혈증</option>
      <option>간질환</option>
      <option>폐질환</option>
    </select>
  </label>

  <label>가족력:
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
    function getSelectValues(select) {
      return Array.from(select.selectedOptions).map(opt => opt.value);
    }

    function recommend() {
      const age = parseInt(document.getElementById('age').value);
      const gender = document.getElementById('gender').value;
      const diseases = getSelectValues(document.getElementById('diseases'));
      const family = getSelectValues(document.getElementById('family'));

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

      const result = document.getElementById('result');
      if (recommendations.size === 0) {
        result.innerHTML = "추천할 장비 기반 검사가 없습니다.";
      } else {
        result.innerHTML = "<strong>추천 검사 항목:</strong><ul>" +
          [...recommendations].map(item => `<li>${item}</li>`).join("") + "</ul>";
      }
    }
  </script>
</body>
</html>
