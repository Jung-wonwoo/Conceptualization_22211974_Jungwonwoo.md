학번:22211974  
이름:정원우  
이메일:wonu8866@naver.com

프로그램 이름:든든램프
<img width="434" height="433" alt="image" src="https://github.com/user-attachments/assets/bcde8e00-21ff-488d-b599-fd64892d93f7" />

1.Introduction  
1)Executive Summary  
현대인들에게 점심 메뉴 선택은 매일 반복되는 고민거리 중 하나입니다. "아무거나"라고 답하지만 정작 마음에 드는 메뉴를 고르지 못하는 결정 장애 문제를 해결하기 위해,
본 프로젝트는 사용자에게 재미있고 직관적인 방식으로 최적의 식사를 제안하는 '든든램프' 시스템을 제안합니다. 이 시스템은 유명한 스무고개 게임인 '아키네이터'의 로직을 벤치마킹하여,
몇 가지 질문을 통해 사용자의 취향과 현재 상태를 분석하고 식사 메뉴를 도출합니다.  
2)Prominent Features
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
