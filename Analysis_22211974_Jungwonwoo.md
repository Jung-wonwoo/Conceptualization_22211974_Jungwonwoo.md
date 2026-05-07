학번:22211974  
이름:정원우  
이메일:wonu8866@naver.com

프로그램 이름:든든램프
<img width="531" height="426" alt="image" src="https://github.com/user-attachments/assets/61ff5bfb-5f00-4ed3-8f56-9b4c21a7e338" />

05/03/2026/1.00/First draft/정원우
05/04/2026/1.01/User Interface prototype 작성/정원우

1.Introduction  
1) Executive Summary  
현대인들에게 점심 메뉴 선택은 매일 반복되는 고민거리 중 하나입니다. "아무거나"라고 답하지만 정작 마음에 드는 메뉴를 고르지 못하는 결정 장애 문제를 해결하기 위해,
본 프로젝트는 사용자에게 재미있고 직관적인 방식으로 최적의 식사를 제안하는 '든든램프' 시스템을 제안합니다. 이 시스템은 유명한 스무고개 게임인 '아키네이터'의 로직을 벤치마킹하여,
몇 가지 질문을 통해 사용자의 취향과 현재 상태를 분석하고 식사 메뉴를 도출합니다.  
2) Prominent Features
의사결정 알고리즘의 활용: 단순 무작위 추천이 아닌, 의사결정 트리 기반의 알고리즘을 사용하여 사용자의 답변에 따라 선택 범위를 좁혀나가는 체계적인 추천 과정을 제공합니다.  
사용자 친화적인 인터페이스: '램프의 요정' 컨셉을 차용하여 자칫 지루할 수 있는 메뉴 선택 과정을 게임처럼 즐길 수 있도록 시각적인 로고와 친근한 UI를 구성합니다.  
유용성 및 확장성: 식사 메뉴뿐만 아니라 향후 사용자의 식습관 데이터 축적을 통한 개인 맞춤형 영양 관리나, 주변 식당의 실시간 데이터와 연동하여 실제 주문까지 이어지는 서비스로 확장할 수 있는 가능성을 지니고 있습니다.  

2.Use case analysis  
<img width="1067" height="848" alt="image" src="https://github.com/user-attachments/assets/cce744ba-4851-45cb-b904-688be3ba875a" />

Use Case #1 : 음식 추천  
GENERAL CHARACTERISTICS  
Summary : 사용자의 응답을 기반으로 최적의 음식 메뉴를 추천하는 기능   
Scope : 든든램프  
Level : User level  
Primary Actor : User
Preconditions : 사용자가 시스템에 접속한 상태여야 한다.  
Trigger : 추천 시작 버튼을 눌렀을 때  
Success Post Condition : 추천 결과가 화면에 출력된다.  
Failed Post Condition : 기본 추천 목록이 출력된다.  
MAIN SUCCESS SCENARIO    
Step | Action  
1 | 사용자가 추천 시작 버튼을 누른다.  
2 | 시스템이 질문을 생성한다.  
3 | 사용자가 응답한다.  
4 | 후보군이 축소된다.  
5 | 최종 음식이 선택된다.  
6 | 결과가 출력된다.  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 질문 생성 실패 시 기본 질문 제공  
3 | 3a. 응답 없음 → 세션 초기화  

Use Case #2 : 질문/응답 처리  
GENERAL CHARACTERISTICS  
Summary : 사용자의 응답을 수집하고 반영하는 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 추천 과정 진행 중  
Trigger : 응답 버튼 클릭  
Success Post Condition : 응답 데이터가 저장된다.  
Failed Post Condition : 데이터가 반영되지 않는다.  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 질문이 출력된다.  
2 | 사용자가 응답한다.  
3 | 데이터 저장  
4 | 후보군 업데이트  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 건너뛰기 선택 시 제외 처리  

Use Case #3 : 알레르기 설정  
GENERAL CHARACTERISTICS   
Summary : 알레르기 음식 제외 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 로그인 상태  
Trigger : 알레르기 설정 메뉴 선택  
Success Post Condition : 해당 음식이 추천에서 제외된다.  
Failed Post Condition : 설정이 반영되지 않는다.  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 설정 화면 진입  
2 | 재료 선택  
3 | 저장  
4 | 후보군에서 제거  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 선택 없음 → 설정 없이 종료   

Use Case #4 : 질문 건너뛰기  
GENERAL CHARACTERISTICS  
Summary : 답변 어려운 질문 생략 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 질문 진행 중  
Trigger : 건너뛰기 버튼 클릭  
Success Post Condition : 다음 질문으로 이동  
Failed Post Condition : 동작 없음  
MAIN SUCCESS SCENARIO   
Step | Action  
1 | 건너뛰기 클릭  
2 | 해당 질문 제외  
3 | 다음 질문 출력  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 연속 건너뛰기 → 기본 추천 제공  

Use Case #5 : 질문 되돌리기  
GENERAL CHARACTERISTICS  
Summary : 이전 질문으로 돌아가는 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 이전 질문 존재  
Trigger : 되돌리기 버튼 클릭  
Success Post Condition : 이전 상태 복원  
Failed Post Condition : 동작 없음  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 되돌리기 클릭  
2 | 스택에서 상태 복원  
3 | 이전 질문 출력  
EXTENSION SCENARIOS  
Step | Branching Action  
1 | 1a. 첫 질문 → 동작 없음  

Use Case #6 : 음식 추천 결과 출력  
GENERAL CHARACTERISTICS  
Summary : 추천된 음식 정보를 보여주는 기능  
Scope : 든든램프  
Level : System level  
Primary Actor : System  
Preconditions : 추천 완료 상태  
Trigger : 추천 종료  
Success Post Condition : 결과 화면 출력  
Failed Post Condition : 기본 리스트 출력  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 결과 계산  
2 | 음식 정보 조회  
3 | 화면 출력  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 데이터 없음 → 기본 메뉴 표시  

Use Case #7 : 이전 내역 조회  
GENERAL CHARACTERISTICS  
Summary : 과거 추천 기록 조회  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 기록 존재  
Trigger : 기록 버튼 클릭  
Success Post Condition : 기록 리스트 출력  
Failed Post Condition : 기록 없음 메시지 출력  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 버튼 클릭  
2 | DB 조회  
3 | 리스트 출력  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 기록 없음 → 메시지 출력  

Use Case #8 : 새 메뉴 등록  
GENERAL CHARACTERISTICS  
Summary : 새로운 음식 추가 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 추천 실패 또는 메뉴 없음  
Trigger : 등록 버튼 클릭  
Success Post Condition : 메뉴 DB 저장  
Failed Post Condition : 저장 실패  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 메뉴 입력  
2 | 특징 입력  
3 | 저장  
4 | DB 반영  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 입력 부족 → 경고 출력  

Use Case #9 : 재시작  
GENERAL CHARACTERISTICS  
Summary : 추천 과정 초기화  
Scope : 든든램프  
Level : User level  
Primary Actor :User  
Preconditions : 진행 중  
Trigger : 재시작 버튼  
Success Post Condition : 초기 상태  
Failed Post Condition : 초기화 실패  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 재시작 클릭  
2 | 데이터 삭제  
3 | 초기 질문 출력  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 삭제 실패 → 오류 출력  

Use Case #10 : 추천 피드백  
GENERAL CHARACTERISTICS  
Summary : 추천 결과 평가 기능  
Scope : 든든램프  
Level : User level  
Primary Actor : User  
Preconditions : 추천 결과 존재  
Trigger : 추천/비추천 버튼 클릭  
Success Post Condition : 데이터 반영  
Failed Post Condition : 저장 실패  
MAIN SUCCESS SCENARIO  
Step | Action  
1 | 결과 확인  
2 | 피드백 선택  
3 | DB 저장  
4 | 알고리즘 반영  
EXTENSION SCENARIOS  
Step | Branching Action  
2 | 2a. 선택 없음 → 종료  

3.Domain analysis  
1) User  
시스템을 사용하는 사용자의 정보를 관리하는 클래스이다.  
사용자의 응답 데이터, 알레르기 정보, 추천 기록 등을 저장한다.  
ex) 응답 리스트, 알레르기 정보, 추천 기록  
2) Question  
사용자에게 제시되는 질문을 관리하는 클래스이다.  
각 질문은 특정 음식 속성과 연결되어 있으며, 후보군을 줄이기 위한 기준으로 사용된다.  
ex) 질문 내용, 속성 (매운 음식 여부, 국물 여부 등)  
3) Answer  
사용자의 응답을 저장하는 클래스이다.  
각 질문에 대해 사용자가 선택한 결과를 기록하며 추천 알고리즘에 반영된다.  
ex) Yes / No 값  
4) Food  
추천 대상이 되는 음식 정보를 저장하는 클래스이다.  
음식의 다양한 속성을 포함하여 추천 알고리즘의 기준 데이터로 활용된다.  
ex) 음식명, 재료, 카테고리, 속성(매운맛, 국물 여부 등)  
5) FoodDB   
음식 데이터를 관리하는 클래스이다.  
전체 음식 리스트를 저장하고, 조건에 맞는 음식들을 검색 및 제공한다.  
ex) 음식 리스트, 검색 함수  
6) RecommendationEngine  
추천 알고리즘을 수행하는 핵심 클래스이다.  
사용자의 응답 데이터를 기반으로 후보군을 줄이고, 최적의 음식을 도출한다.  
ex) 확률 계산, 후보군 필터링  
7) Session  
사용자의 추천 진행 상태를 관리하는 클래스이다.  
현재 질문 단계, 응답 기록, 남은 후보군 등을 저장한다.  
ex) 질문 기록, 후보군 리스트  
8) AllergyFilter  
사용자의 알레르기 정보를 기반으로 음식 후보군을 필터링하는 클래스이다.  
알레르기 유발 음식이 추천되지 않도록 처리한다.  
ex) 제외 음식 리스트  
9) History  
사용자의 과거 추천 결과를 저장하는 클래스이다.  
이전 추천 데이터를 조회하거나 분석하는 데 사용된다.  
ex) 추천 음식, 날짜, 선택 결과  
10) UIManager  
사용자 인터페이스를 관리하는 클래스이다.  
질문 화면, 결과 화면, 설정 화면 등을 출력하고 사용자 입력을 처리한다.  
ex) 화면 출력, 버튼 입력 처리  

4.User Interface prototype  
<img width="247" height="406" alt="image" src="https://github.com/user-attachments/assets/64f06de0-0faf-432a-83c7-b2d202145b2a" />

1) Main Screen (메인 화면)  
chat-image메인 화면에는 "든든램프" 로고와 함께 추천 시작 버튼, 이전 기록 조회 버튼이 표시된다. 화면은 단순한 구조로 구성되어 있으며,
사용자는 추천 시작 버튼을 통해 음식 추천 프로세스를 시작할 수 있다.  
사용자가 앱을 실행하면 가장 먼저 보게 되는 화면이다.  
추천 시작 버튼을 클릭하면 질문 기반 추천 프로세스로 이동하며, 이전 기록 버튼을 통해 과거 추천 결과를 확인할 수 있다.  
<img width="227" height="402" alt="image" src="https://github.com/user-attachments/assets/8a833e73-313f-4ce0-87df-494014c9bdb9" />

2) Question Screen (질문 화면)
chat-image질문 화면에는 중앙에 질문 텍스트가 표시되며, 하단에는 "예", "아니오" 버튼이 있다. 추가로 "건너뛰기", "되돌리기" 버튼이 함께 배치되어 있다.  
시스템이 사용자에게 음식 관련 질문을 제시하는 화면이다.  
사용자는 "예/아니오" 버튼을 통해 응답할 수 있으며, 답변이 어려운 경우 건너뛰기 기능을 사용할 수 있다.  
또한, 이전 답변을 수정하고 싶을 경우 되돌리기 기능을 사용할 수 있다.  
<img width="240" height="406" alt="3" src="https://github.com/user-attachments/assets/6753b8ee-531a-4b07-b3fb-e60750a8d673" />

3) Result Screen (추천 결과 화면)  
chat-image추천 결과 화면에는 추천된 음식의 이미지와 이름, 간단한 설명이 표시된다. 하단에는 재추천 버튼과 피드백 버튼(추천/비추천)이 존재한다.  
모든 질문이 완료되면 최종 추천 결과가 출력되는 화면이다.  
추천된 음식의 이미지 및 정보를 확인할 수 있으며, 사용자는 결과에 대해 만족/불만족 피드백을 제공할 수 있다.  
재추천 버튼을 통해 새로운 추천을 다시 받을 수 있다.  
<img width="212" height="392" alt="4" src="https://github.com/user-attachments/assets/52403b17-dc4d-4751-b459-7329484b3a4f" />

4) Allergy Setting Screen (알레르기 설정 화면)  
chat-image알레르기 설정 화면에는 다양한 재료 목록이 체크박스 형태로 나열되어 있으며, 하단에 저장 버튼이 있다.  
사용자가 특정 음식이나 재료를 제외하기 위한 설정 화면이다.  
사용자는 알레르기 항목을 선택한 후 저장 버튼을 누르면,이후 추천 과정에서 해당 음식이 자동으로 제외된다.  
<img width="210" height="398" alt="5" src="https://github.com/user-attachments/assets/1d0eb1c9-39b8-45f6-8f0a-d37cb0ef592c" />

5) History Screen (이전 내역 조회 화면)  
chat-image이전 추천 기록들이 리스트 형태로 나열되어 있으며, 각 항목에는 추천 음식 이름과 날짜가 표시된다.  
사용자가 과거에 추천받았던 음식들을 확인할 수 있는 화면이다.  
리스트 형태로 제공되며, 사용자는 이전에 만족했던 메뉴를 다시 확인할 수 있다.  
<img width="247" height="413" alt="6" src="https://github.com/user-attachments/assets/167197e1-cd24-4f5e-bf3f-e9ef7d66b5eb" />

6) Restart Flow (재시작 화면 흐름)  
chat-image재시작 버튼을 누르면 확인 팝업창이 표시되며, 확인 시 모든 데이터가 초기화되고 처음 질문 화면으로 돌아간다.  
추천 과정 도중 재시작 버튼을 누르면 현재까지의 모든 응답 데이터가 초기화된다.  
이후 시스템은 처음 질문 단계로 돌아가 새로운 추천을 진행한다.  
<img width="247" height="398" alt="7" src="https://github.com/user-attachments/assets/61e8fa0d-daa0-4625-b2aa-e24e1d8c6264" />

7) New Menu Register Screen (새 메뉴 등록 화면)  
chat-image사용자가 음식 이름과 특징을 입력할 수 있는 입력창이 있으며, 저장 버튼이 하단에 위치한다.  
추천 결과가 사용자의 의도와 다를 경우, 사용자가 직접 새로운 음식을 추가할 수 있는 화면이다.  
음식 이름과 특징을 입력하면 시스템 데이터베이스에 반영된다.  

5.Glossary  
사용자 (User)	: 든든램프 시스템을 사용하는 일반 사용자  
질문 (Question)	: 사용자의 취향을 파악하기 위해 시스템이 제시하는 항목  
응답 (Answer)	: 사용자가 질문에 대해 선택하는 예/아니오 값  
음식 (Food)	: 추천 대상이 되는 메뉴 데이터  
후보군 (Candidate Set)	: 조건에 따라 남아있는 음식들의 집합  
가중치 (Weight)	: 특정 음식이 정답일 확률을 나타내는 값  
엔트로피 (Entropy)	: 데이터의 불확실성을 나타내는 척도  
정보 이득 (Information Gain)	: 질문을 통해 줄어드는 불확실성의 정도  
베이지안 확률 모델	: 새로운 정보를 반영하여 확률을 갱신하는 통계적 방법  
알레르기 설정 (Allergy Setting)	: 특정 음식 또는 재료를 추천에서 제외하기 위한 기능  
추천 결과 (Recommendation Result)	: 시스템이 최종적으로 사용자에게 제안하는 음식  
세션 (Session)	: 사용자의 추천 진행 상태를 저장하는 데이터 단위  
콜드 스타트 (Cold Start)	: 초기 데이터 부족으로 추천 정확도가 낮은 상태  
UI (User Interface)	: 사용자와 시스템 간 상호작용을 위한 화면 구성  
데이터베이스 (Database)	: 음식 정보 및 사용자 데이터를 저장하는 공간  

6.References
1) Information Theory 관련 자료  
2) Bayesian Probability Model 관련 자료  
3) Recommendation System 관련 자료  
