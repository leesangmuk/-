# -import streamlit as st

def recommend_equipment_tests(age, gender, diseases, family_history):
    recommendations = []

    # 나이 기반 추천
    if age >= 50:
        recommendations.append("대장내시경")
    if age >= 40:
        recommendations.append("위내시경")
    if age >= 55:
        recommendations.append("저선량 폐CT")

    # 성별 및 가족력 기반
    if gender == "여성" and ("자궁경부암" in family_history or age >= 20):
        recommendations.append("자궁경부세포검사")
        recommendations.append("골반 초음파")
    if gender == "남성" and age >= 50:
        recommendations.append("전립선 초음파")

    # 질환 기반
    if "고지혈증" in diseases or "심장질환" in family_history:
        recommendations.append("심장초음파")
        recommendations.append("심장 CT")

    if "간질환" in diseases:
        recommendations.append("복부 초음파")
        recommendations.append("간 MRI")

    if "폐질환" in diseases or "폐암" in family_history:
        recommendations.append("흉부 CT")

    if "당뇨" in diseases:
        recommendations.append("안과검사(망막 OCT)")

    return list(set(recommendations))

# 웹 UI 구성
st.title("종합검진 선택검사 항목 추천 (장비검사 중심)")

age = st.number_input("나이", min_value=0, max_value=120, step=1)
gender = st.selectbox("성별", ["남성", "여성"])
diseases = st.multiselect(
    "본인이 알고 있는 질환",
    ["고혈압", "당뇨", "고지혈증", "간질환", "폐질환", "기타"]
)
family_history = st.multiselect(
    "가족력",
    ["심장질환", "폐암", "자궁경부암", "간암", "기타"]
)

if st.button("추천 검사 보기"):
    results = recommend_equipment_tests(age, gender, diseases, family_history)
    if results:
        st.subheader("추천 장비검사 항목:")
        for r in results:
            st.markdown(f"- {r}")
    else:
        st.info("현재 입력 조건에 맞는 추가 장비검사는 없습니다. 기본검진을 참고하세요.")
