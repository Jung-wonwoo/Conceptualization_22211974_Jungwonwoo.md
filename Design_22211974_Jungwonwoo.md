학번:22211974  
이름:정원우  
이메일:wonu8866@naver.com

프로그램 이름:든든램프  
<img width="531" height="426" alt="image" src="https://github.com/user-attachments/assets/61ff5bfb-5f00-4ed3-8f56-9b4c21a7e338" />

history  
2026.05.16. 1.0.0 First draft 정원우  
2026.05.29. 1.0.1 내용 추가   정원우  
2026.06.02. 1.0.2 내용 추가   정원우

1. Introduction
현대인들은 바쁜 일상 속에서 식사 메뉴를 선택하는 데 많은 어려움을 겪는다. "무엇을 먹을지"에 대한 고민은 일상적으로 반복되며, 이는 선택 피로를 유발하고 불필요한 시간 소모로 이어진다.  
특히 명확한 메뉴를 정하지 못한 상태에서 다양한 선택지 사이에서 결정을 내려야 하는 상황은 사용자에게 부담으로 작용한다.  
이러한 문제를 해결하기 위해 본 프로젝트에서는 질문-응답 기반 의사결정 방식을 활용한 음식 추천 시스템 '든든램프'를 제안한다. 본 시스템은 사용자의 취향을 단계적으로 파악하기 위해 일련의 질문을 제시하고,  
사용자의 응답을 기반으로 음식 후보군을 점진적으로 축소하여 최종적으로 가장 적합한 메뉴를 추천하는 것을 목표로 한다. 또한 알레르기 설정, 질문 건너뛰기, 되돌리기 기능 등을 통해 사용자 편의성과 유연성을 함께 고려하였다.  
본 문서는 Analysis 단계에 이어 작성된 Design 단계의 문서로, 시스템이 수행해야 하는 기능을 실제로 어떻게 구현할 것인지에 초점을 맞춘다. 이를 위해 Class Diagram을 통해 시스템의 정적 구조를 정의하고,  
Sequence Diagram을 통해 객체 간의 상호작용을 시간 흐름에 따라 표현하며, State Machine Diagram을 통해 시스템의 상태 변화 과정을 모델링한다.  
이와 같은 설계를 통해 '든든램프' 시스템의 전체 구조와 동작을 구체화하고, 이후 구현 단계에서 일관된 기준을 제공하는 것을 목적으로 한다.

2. Class diagram
<img width="576" height="387" alt="image" src="https://github.com/user-attachments/assets/e818806f-01f1-4830-af44-7deef7e8fc53" />  
<br>
<img width="218" height="134" alt="image" src="https://github.com/user-attachments/assets/d59fa8d6-fb17-4e0e-bcd6-7752948a79d5" /> 

1) User  
(1) Attributes  
userId: String : 사용자의 고유 식별자  
allergyList: List : 사용자가 설정한 알레르기 정보 목록  
answerList: List : 사용자가 입력한 응답 데이터  
(2) Methods  
setAllergy(list: List): void : 알레르기 정보를 설정  
addAnswer(answer: Answer): void : 사용자의 응답 추가  
getAnswers(): List : 응답 목록 반환  
(3) Others  
사용자의 기본 정보와 입력 데이터를 관리하는 클래스이며, 추천 시스템의 입력 데이터 역할을 수행한다.  
<img width="145" height="121" alt="image" src="https://github.com/user-attachments/assets/184a606c-0e84-4ecf-9594-fa1402eac4eb" />

2) Question  
(1) Attributes  
questionId: int : 질문의 고유 ID  
content: String : 질문 내용  
attribute: String : 음식 속성과 연결된 질문 기준  
(2) Methods  
getQuestion(): String : 질문 내용을 반환  
(3) Others  
사용자의 취향을 파악하기 위한 질문을 관리하며, 추천 알고리즘의 기준이 되는 핵심 요소이다.  
<img width="160" height="93" alt="image" src="https://github.com/user-attachments/assets/e4d4b820-dab3-4f08-9a1c-0754ccb77b9d" />

3) Answer  
(1) Attributes  
questionId: int : 응답이 연결된 질문 ID  
response: boolean : 사용자 응답 (true: Yes, false: No)  
(2) Methods  
Answer(questionId: int, response: boolean) : 생성자  
getResponse(): boolean : 응답 값 반환  
getQuestionId(): int : 질문 ID 반환  
(3) Others  
사용자의 응답을 저장하는 데이터 클래스이며, 추천 알고리즘에서 입력 값으로 활용된다.  
<img width="214" height="122" alt="image" src="https://github.com/user-attachments/assets/94e5bfc9-ebf9-46a5-8f5b-bb7642fd20a2" />

4) Food  
(1) Attributes  
foodId: int : 음식 고유 ID  
name: String : 음식 이름  
ingredients: List : 포함된 재료 목록  
attributes: List : 음식의 특징 속성  
(2) Methods  
hasAttribute(attr: String): boolean : 특정 속성 포함 여부 확인  
(3) Others  
추천 대상이 되는 핵심 데이터 클래스이며, 다양한 속성을 기반으로 필터링 및 추천이 이루어진다.  
<img width="334" height="108" alt="image" src="https://github.com/user-attachments/assets/fcb902ce-f16d-401f-a5f0-b32167e3c28d" />

5) FoodDB  
(1) Attributes  
foodList: List : 전체 음식 데이터 목록  
(2) Methods  
getAllFoods(): List : 전체 음식 반환  
findFoodsByAttribute(attr: String): List : 속성 기반 음식 검색  
removeByAllergy(allergyList: List): List : 알레르기 음식 제거  
(3) Others  
음식 데이터를 저장하고 관리하는 클래스이며, 시스템의 데이터 저장소 역할을 수행한다.  
<img width="294" height="123" alt="image" src="https://github.com/user-attachments/assets/abbdb0fe-9f1b-428a-9a38-74a95ffd0e16" />

6) RecommendationEngine  
(1) Attributes  
candidateFoods: List : 현재 추천 후보 음식 목록  
(2) Methods  
initialize(foodList: List): void : 후보군 초기화  
filterByAnswer(answer: Answer): void : 응답 기반 후보군 필터링    
applyAllergyFilter(allergyList: List): void : 알레르기 필터 적용  
getBestFood(): Food : 최종 추천 음식 반환  
(3) Others  
추천 알고리즘을 수행하는 핵심 클래스이며, 시스템의 주요 로직을 담당한다.  
<img width="217" height="135" alt="image" src="https://github.com/user-attachments/assets/0a1319e0-4901-424f-bc31-c8da92d4bbd8" />

7) Session  
(1) Attributes   
currentQuestionIndex: int : 현재 질문 위치  
answers: List : 사용자 응답 기록  
candidateFoods: List : 현재 후보 음식 목록  
(2) Methods  
addAnswer(answer: Answer): void : 응답 추가  
getNextQuestion(): Question : 다음 질문 반환  
rollback(): void : 이전 상태로 되돌리기  
(3) Others  
추천 과정의 상태를 관리하는 클래스이며, 사용자와 시스템 간의 상호작용 흐름을 제어한다.  
<img width="285" height="78" alt="image" src="https://github.com/user-attachments/assets/8dec2537-b259-48fa-8128-3d617ff40532" />

8) AllergyFilter  
(1) Attributes   
allergyList: List : 알레르기 정보 목록  
(2) Methods  
filterFoods(foodList: List): List : 알레르기 기반 음식 필터링  
(3) Others  
알레르기 정보를 기반으로 후보 음식 목록을 필터링하는 보조 클래스이다.  
<img width="187" height="93" alt="image" src="https://github.com/user-attachments/assets/864f13f8-913b-4bf3-b26d-d6ee04e07570" />

9) History  
(1) Attributes  
historyList: List : 추천 결과 기록  
(2) Methods  
addHistory(food: Food): void : 추천 결과 저장  
getHistory(): List : 기록 반환  
(3) Others  
사용자의 과거 추천 결과를 저장하고 조회하는 클래스이다.
<img width="214" height="111" alt="image" src="https://github.com/user-attachments/assets/7f3bd82a-f375-4cf0-a82d-9790678b87da" />

10) UIManager  
(1) Attributes  
currentScreen: String : 현재 화면 상태  
(2) Methods  
showQuestion(q: Question): void : 질문 화면 출력  
showResult(food: Food): void : 결과 화면 출력  
getUserInput(): Answer : 사용자 입력 반환  
(3) Others  
사용자 인터페이스를 담당하는 클래스이며, 사용자 입력과 출력 처리를 수행한다.  

3. Sequence diagram  
<img width="640" height="275" alt="image" src="https://github.com/user-attachments/assets/bff40ad2-4bf1-4e7c-805a-10f7918f4bd8" />

1) 음식 추천  
위의 Sequence Diagram은 사용자가 음식 추천을 요청하고, 질문-응답 과정을 통해 최종 추천 결과를 도출하는 전체 흐름을 나타낸다.  
사용자가 추천을 시작하면 UIManager를 통해 Session이 생성되고, RecommendationEngine이 초기화된다. 이후 FoodDB로부터 음식 데이터를 가져와 후보군을 구성한다.  
추천 과정은 반복적인 질문-응답 구조로 이루어진다. UIManager는 사용자에게 질문을 출력하고, 사용자는 이에 대해 응답을 입력한다. 해당 응답은 RecommendationEngine으로 전달되어 후보군 필터링에 반영된다.  
이 과정은 후보군이 충분히 줄어들 때까지 반복되며, 이후 RecommendationEngine은 최적의 음식을 선택한다. 최종 결과는 UIManager를 통해 사용자에게 출력된다.  
본 과정에서 RecommendationEngine은 핵심 로직을 수행하며, Session은 상태를 관리하고, UIManager는 사용자와의 인터페이스를 담당한다.  
<img width="524" height="310" alt="image" src="https://github.com/user-attachments/assets/970715c8-129d-4ccd-81a8-3967265e7ab2" />

2) 질문 되돌리기  
본 Sequence Diagram은 사용자가 이전 질문으로 되돌아가 응답을 수정하는 과정을 나타낸다.  
사용자가 되돌리기 버튼을 클릭하면 UIManager를 통해 Session에 요청이 전달된다. Session은 저장된 응답 기록을 기반으로 이전 상태로 복원하며, 마지막 응답을 제거한다.  
이후 RecommendationEngine은 해당 응답을 반영하기 이전 상태로 후보군을 복구한다.   
복원이 완료되면 이전 질문이 다시 사용자에게 출력되며, 사용자는 새로운 응답을 입력할 수 있다.
<img width="508" height="302" alt="image" src="https://github.com/user-attachments/assets/6bc09e7a-2fe6-462c-82d0-46001ed8d872" />

3) 질문 기다리기  
본 Sequence Diagram은 사용자가 질문에 답변하지 않고 건너뛰는 경우의 흐름을 나타낸다.  
사용자가 건너뛰기 버튼을 선택하면 UIManager를 통해 RecommendationEngine에 전달된다. RecommendationEngine은 해당 질문을 후보군 필터링에 반영하지 않고 다음 질문을 선택한다.  
이후 UIManager는 새로운 질문을 사용자에게 출력하며 추천 과정이 계속 진행된다.  
<img width="640" height="381" alt="image" src="https://github.com/user-attachments/assets/c4f40659-4e8a-4b14-b52d-a0202ae88473" />

4) 알레르기 설정
본 Sequence Diagram은 사용자가 알레르기 정보를 설정하고 추천 과정에 반영하는 흐름을 나타낸다.  
사용자가 알레르기 설정 화면에서 특정 재료를 선택하면 UIManager를 통해 User 객체에 저장된다.  
이후 RecommendationEngine은 추천 과정에서 AllergyFilter를 호출하여 해당 재료가 포함된 음식들을 후보군에서 제거한다.  
이 과정을 통해 사용자의 알레르기를 고려한 안전한 추천이 이루어진다.
<img width="487" height="288" alt="image" src="https://github.com/user-attachments/assets/ca00cc57-b5e0-4828-bab6-c67c49bea0f7" />

5) 추천 재시작  
본 Sequence Diagram은 사용자가 추천 과정을 초기화하고 다시 시작하는 흐름을 나타낸다.
사용자가 재시작 버튼을 선택하면 UIManager를 통해 Session에 reset 요청이 전달되어 기존의 응답 데이터와 상태가 초기화된다. 이후 UIManager는 RecommendationEngine에 initialize 메시지를 전달하여 후보군을 다시 설정한다.
RecommendationEngine은 초기 상태에서 첫 질문을 생성하고 이를 UIManager에 전달하며, UIManager는 사용자에게 새로운 질문을 출력한다. 이를 통해 추천 과정은 처음부터 다시 시작된다.
<img width="311" height="251" alt="image" src="https://github.com/user-attachments/assets/47309a5d-a4ad-49d8-ac79-73de0cbe2c05" />

6) 이전 기록 조회
본 Sequence Diagram은 사용자가 이전에 추천받은 음식 목록을 조회하는 과정을 나타낸다.
사용자가 이전 기록 조회를 요청하면 UIManager를 통해 History 객체에 getHistory 메시지가 전달된다. History는 저장된 추천 결과 목록을 반환하며, UIManager는 이를 사용자에게 출력한다.
이를 통해 사용자는 과거 추천 결과를 확인하고 이전에 선택했던 음식을 다시 확인할 수 있다.
<img width="332" height="221" alt="image" src="https://github.com/user-attachments/assets/39950452-e8ae-4139-932a-6e0f547cf72b" />

7) 추천 피드백  
본 Sequence Diagram은 사용자가 추천 결과에 대해 피드백을 제공하고 이를 추천 알고리즘에 반영하는 과정을 나타낸다.
사용자가 추천 결과에 대해 피드백을 입력하면 UIManager를 통해 RecommendationEngine에 전달된다. RecommendationEngine은 updateWeight 메소드를 통해 해당 피드백을 반영하고,
내부적으로 확률 값을 조정하여 이후 추천 정확도를 향상시킨다. 이 과정은 시스템이 사용자 취향을 점진적으로 학습할 수 있도록 한다.
<img width="276" height="275" alt="image" src="https://github.com/user-attachments/assets/c80a5bce-0db0-4993-a660-29e0872e008f" />

8) 로그인  
본 Sequence Diagram은 사용자의 로그인 과정을 나타낸다.
사용자가 로그인 정보를 입력하면 UIManager는 입력된 정보를 검증하기 위해 checkInfo 과정을 수행한다. 이후 UIManager는 System에 validate 메시지를 전달하여 사용자 정보의 유효성을 확인한다.
System은 인증 결과를 UIManager에 반환하며, UIManager는 로그인 성공 또는 실패 여부를 사용자에게 전달한다.
본 시스템에서 로그인 기능은 필수 기능이 아니라 선택적 기능으로, 사용자 기록 관리 및 개인화 기능을 위한 목적으로 설계되었다.

4. State machine diagram  
<img width="640" height="247" alt="image" src="https://github.com/user-attachments/assets/64b18cb2-1edf-4150-b47d-1621714fdfb1" />  

본 State Machine Diagram은 음식 추천 시스템 '든든램프'의 상태 변화 과정을 나타낸다.
시스템은 초기 상태에서 로그인 없이도 음식 추천 기능을 사용할 수 있으며, 로그인 기능은 사용자 기록 저장 및 개인화 기능을 위한 선택적 기능으로 설계되었다. 사용자는 로그인 또는 비회원 상태를 선택한 후 Main 상태로 전이된다.
Main 상태에서는 음식 추천, 과거 내역 조회, 알레르기 설정 기능을 선택할 수 있다. 음식 추천을 선택할 경우 Question 상태로 전이되며, 시스템은 사용자에게 질문을 제시한다. 이후 사용자는 Answer 상태에서 응답을 입력하고, 
Filtering 상태를 거쳐 후보 음식이 갱신된다. 이 과정은 반복적으로 수행되어 최적의 음식이 선택될 때까지 진행된다.
또한 추천 과정 중 사용자는 Skip 기능을 통해 질문을 건너뛸 수 있으며, Rollback 기능을 통해 이전 질문으로 되돌아갈 수 있다. 최종적으로 Result 상태에서 추천 결과가 출력되며, 
사용자는 Restart를 통해 추천 과정을 다시 시작하거나 End 상태로 종료할 수 있다.
본 다이어그램은 사용자 선택에 따른 분기와 반복적인 추천 흐름을 포함하여 시스템의 전체 상태 전이를 표현한다.

5. Implementation requirments  
CPU : Intel Pentium IV 이상  
RAM : 1GB 이상  
HDD / SSD : 10GB 이상의 여유 공간  
Implementation Language : Java (JDK 1.8 이상)  
Network	인터넷 연결 필요 없음 (단독 실행 가능)  
Operating System : Windows 7 이상  

6. Glossary  
Denden Lamp : 질문-응답 기반 음식 추천 시스템으로, 사용자의 취향을 분석하여 최적의 음식을 추천하는 프로그램  
FoodDB : 음식 데이터를 저장하고 관리하는 클래스  
RecommendationEngine : 사용자 응답을 기반으로 음식 후보군을 필터링하고 추천을 수행하는 핵심 클래스  
Session : 추천 과정의 상태 및 사용자 응답을 관리하는 클래스  
UIManager : 사용자 인터페이스를 담당하며 입력과 출력을 처리하는 클래스

7. Reference  
강의자료 : Structural Modeling, Behavioral Modeling  
Android Developers Documentation  
Java SE Documentation  


















