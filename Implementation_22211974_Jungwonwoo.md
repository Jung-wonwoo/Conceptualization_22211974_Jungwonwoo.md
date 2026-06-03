# 💡 Denden Lamp (음식 추천 시스템)

사용자의 취향(질문 답변)을 분석하여 맞춤형 음식을 추천해 주는 **Java Swing 기반의 GUI 토이 프로젝트**입니다.

## ✨ 주요 기능
- **사용자 로그인:** 실행 시 회원/비회원 로그인 기능 제공
- **취향 분석 알고리즘:** 10가지 카테고리 질문을 통해 실시간 음식 필터링
- **스마트 백업:** 조건에 맞는 음식이 없더라도 직전의 유효한 후보군을 유지하여 추천 보장
- **기록 및 필터:** 이전 추천 음식 기록 보기 기능 및 알레르기 성분 제외 기능
- **부드러운 UX:** 파스텔톤 하늘색 배경 테마 및 마스코트 이미지 배치

## 🛠️ 실행 방법
1. 본 저장소를 클론합니다.
2. `src` 폴더 내부에 `mascot.png` 파일이 있는지 확인합니다.
3. `Main.java` 파일을 컴파일 후 실행합니다.

## 💻 전체 소스 코드 (Main.java)
```java
import javax.swing.*;
import java.awt.*;
import java.util.*;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        new UIManager();
    }
}

/////////////////////////////////////////////////////

class UIManager {
    JFrame frame;
    JLabel label;
    JPanel panel; // 하단 버튼용 패널
    JPanel mainMenuPanel; // 메인 메뉴 전용(글자+이미지) 패널

    // 🌟 예쁜 파스텔톤 하늘색 정의 (RGB: 210, 233, 255)
    Color skyColor = new Color(210, 233, 255);

    FoodDB db = new FoodDB();
    RecommendationEngine engine = new RecommendationEngine();
    Session session = new Session();
    History history = new History();

    List<String> questions = Arrays.asList(
            "spicy", "cheese", "fish", "soup",
            "rice", "meat", "noodle", "sweet",
            "bread", "vegetable"
    );

    int index = 0;

    UIManager() {
        frame = new JFrame("Denden Lamp");
        frame.setSize(500, 400);
        frame.setLayout(new BorderLayout());

        // 🌟 1. 메인 창 전체 배경을 하늘색으로 설정
        frame.getContentPane().setBackground(skyColor);

        // 중앙에 배치될 레이블
        label = new JLabel("Denden Lamp", SwingConstants.CENTER);
        label.setFont(new Font("맑은 고딕", Font.BOLD, 20));

        // 하단 버튼용 패널 생성 및 배경색 설정
        panel = new JPanel();
        panel.setBackground(skyColor); // 🌟 2. 하단 패널 하늘색 적용

        // 메인 메뉴 레이아웃을 담당할 패널 초기화 및 배경색 설정
        mainMenuPanel = new JPanel(new BorderLayout());
        mainMenuPanel.setBackground(skyColor); // 🌟 3. 메인 패널 하늘색 적용

        frame.add(panel, BorderLayout.SOUTH);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);

        // 메인 메뉴 표시
        frame.setVisible(true);
        showLogin();
    }
    void showLogin() {
        // ID 입력창 띄우기 (부모 창을 frame으로 지정하여 중앙에 예쁘게 배치)
        String id = JOptionPane.showInputDialog(frame, "아이디 입력");

        if (id == null || id.trim().isEmpty()) {
            JOptionPane.showMessageDialog(frame, "비회원으로 시작합니다");
        } else {
            JOptionPane.showMessageDialog(frame, id + "님 환영합니다");
        }

        // 로그인이 끝나면 메인 메뉴판을 프레임에 그리도록 호출
        showMainMenu();
    }
    /////////////////////////////////////////////////////

    JButton createButton(String text) {
        JButton btn = new JButton(text);
        btn.setFont(new Font("맑은 고딕", Font.BOLD, 14));
        btn.setFocusPainted(false);
        // 버튼 자체의 색상도 깔끔하게 하얀색 바탕으로 지정하면 하늘색 배경과 잘 어울립니다.
        btn.setBackground(Color.WHITE);
        return btn;
    }

    /////////////////////////////////////////////////////

    void showMainMenu() {
        // 기존 프레임과 패널에 붙은 것들을 깔끔하게 청소
        frame.remove(label);
        mainMenuPanel.removeAll();
        panel.removeAll();

        // 메인 메뉴용 글자 셋팅
        label.setText("메뉴를 선택하세요");
        mainMenuPanel.add(label, BorderLayout.NORTH);

        // 이미지 로드 및 추가
        ImageIcon icon = new ImageIcon("src/mascot.png");
        Image img = icon.getImage().getScaledInstance(250, 250, Image.SCALE_SMOOTH);
        JLabel imageLabel = new JLabel(new ImageIcon(img), SwingConstants.CENTER);
        mainMenuPanel.add(imageLabel, BorderLayout.CENTER);

        // 프레임 중앙에 메인 메뉴 패널 배치
        frame.add(mainMenuPanel, BorderLayout.CENTER);

        // 하단 버튼 배치
        JButton start = createButton("추천 시작");
        JButton historyBtn = createButton("이전 기록");
        JButton allergy = createButton("알레르기 설정");

        panel.add(start);
        panel.add(historyBtn);
        panel.add(allergy);

        start.addActionListener(e -> startRecommendation());
        historyBtn.addActionListener(e -> showHistory());
        allergy.addActionListener(e -> setAllergy());

        // Swing 화면 새로고침 강제 집행
        frame.revalidate();
        frame.repaint();
    }

    /////////////////////////////////////////////////////

    void startRecommendation() {
        engine.initialize(db.getAllFoods());
        session = new Session();
        index = 0;

        showQuestion();
    }

    /////////////////////////////////////////////////////

    void showQuestion() {
        // 질문 화면으로 전환할 때는 메인 패널을 숨기고 label만 중앙에 배치
        frame.remove(mainMenuPanel);
        frame.add(label, BorderLayout.CENTER);

        panel.removeAll();

        if (index >= questions.size()) {
            showResult();
            return;
        }

        String q = questions.get(index);
        label.setText(q + " 좋아하나요?");

        JButton yes = createButton("YES");
        JButton no = createButton("NO");
        JButton skip = createButton("SKIP");
        JButton back = createButton("BACK");

        panel.add(yes);
        panel.add(no);
        panel.add(skip);
        panel.add(back);

        yes.addActionListener(e -> handleAnswer(true));
        no.addActionListener(e -> handleAnswer(false));
        skip.addActionListener(e -> {
            index++;
            showQuestion();
        });
        back.addActionListener(e -> handleBack());

        frame.revalidate();
        frame.repaint();
    }

    /////////////////////////////////////////////////////

    void handleAnswer(boolean answer) {
        String q = questions.get(index);

        session.addAnswer(new Answer(q, answer));
        engine.filterByAnswer(q, answer);

        index++;
        showQuestion();
    }

    /////////////////////////////////////////////////////

    void handleBack() {
        if (index > 0) {
            index--;
            session.rollback();

            engine.initialize(db.getAllFoods());

            for (Answer a : session.answers) {
                engine.filterByAnswer(a.attribute, a.response);
            }
            showQuestion();
        } else {
            showMainMenu(); // 첫 질문에서 BACK 누르면 메인메뉴로 안전하게 복귀
        }
    }

    /////////////////////////////////////////////////////

    void showResult() {
        panel.removeAll();

        Food result = engine.getResult();
        label.setText("추천 음식: " + result.name);
        history.addHistory(result);

        JButton restart = createButton("다시 시작");
        JButton menu = createButton("메인으로");

        panel.add(restart);
        panel.add(menu);

        restart.addActionListener(e -> startRecommendation());
        menu.addActionListener(e -> showMainMenu());

        frame.revalidate();
        frame.repaint();
    }

    /////////////////////////////////////////////////////

    void showHistory() {
        List<Food> list = history.getHistory();
        StringBuilder sb = new StringBuilder();

        if (list.isEmpty()) {
            sb.append("기록 없음");
        } else {
            for (Food f : list) {
                sb.append(f.name).append("\n");
            }
        }
        JOptionPane.showMessageDialog(frame, sb.toString());
    }

    /////////////////////////////////////////////////////

    void setAllergy() {
        String input = JOptionPane.showInputDialog("알레르기 입력 (예: fish)");

        if (input != null && !input.isEmpty()) {
            engine.applyAllergyFilter(Arrays.asList(input));
        }
    }
}
/////////////////////////////////////////////////////

class Food {
    String name;
    List<String> attributes;

    Food(String name, List<String> attributes) {
        this.name = name;
        this.attributes = attributes;
    }

    boolean hasAttribute(String attr) {
        return attributes.contains(attr);
    }
}

/////////////////////////////////////////////////////

class FoodDB {
    List<Food> foods = new ArrayList<>();

    FoodDB() {
        foods.add(new Food("김치찌개", Arrays.asList("spicy", "soup", "rice")));
        foods.add(new Food("된장찌개", Arrays.asList("soup", "rice")));
        foods.add(new Food("순두부찌개", Arrays.asList("spicy", "soup")));
        foods.add(new Food("불고기", Arrays.asList("meat", "rice")));
        foods.add(new Food("삼겹살", Arrays.asList("meat")));
        foods.add(new Food("갈비", Arrays.asList("meat")));
        foods.add(new Food("라면", Arrays.asList("spicy", "noodle", "soup")));
        foods.add(new Food("짜장면", Arrays.asList("noodle")));
        foods.add(new Food("짬뽕", Arrays.asList("spicy", "noodle")));
        foods.add(new Food("초밥", Arrays.asList("fish")));
        foods.add(new Food("연어덮밥", Arrays.asList("fish", "rice")));
        foods.add(new Food("피자", Arrays.asList("cheese", "meat")));
        foods.add(new Food("파스타", Arrays.asList("cheese", "noodle")));
        foods.add(new Food("햄버거", Arrays.asList("meat", "bread")));
        foods.add(new Food("샌드위치", Arrays.asList("bread")));
        foods.add(new Food("케이크", Arrays.asList("sweet")));
        foods.add(new Food("아이스크림", Arrays.asList("sweet")));
        foods.add(new Food("샐러드", Arrays.asList("vegetable")));
        foods.add(new Food("비빔밥", Arrays.asList("rice", "vegetable")));
        foods.add(new Food("떡볶이", Arrays.asList("spicy", "sweet")));
    }

    List<Food> getAllFoods() {
        return new ArrayList<>(foods);
    }
}

/////////////////////////////////////////////////////

class Answer {
    String attribute;
    boolean response;

    Answer(String attribute, boolean response) {
        this.attribute = attribute;
        this.response = response;
    }
}

/////////////////////////////////////////////////////

class Session {
    List<Answer> answers = new ArrayList<>();

    void addAnswer(Answer answer) {
        answers.add(answer);
    }

    void rollback() {
        if (!answers.isEmpty()) {
            answers.remove(answers.size() - 1);
        }
    }
}

/////////////////////////////////////////////////////

class History {
    List<Food> historyList = new ArrayList<>();

    void addHistory(Food f) {
        historyList.add(f);
    }

    List<Food> getHistory() {
        return historyList;
    }
}

/////////////////////////////////////////////////////

class RecommendationEngine {
    List<Food> candidates;
    List<Food> previous;

    void initialize(List<Food> foods) {
        candidates = new ArrayList<>(foods);
        previous = new ArrayList<>(foods);
    }

    void filterByAnswer(String attr, boolean answer) {

        previous = new ArrayList<>(candidates);

        Iterator<Food> it = candidates.iterator();

        while (it.hasNext()) {
            Food f = it.next();

            if (answer && !f.hasAttribute(attr)) it.remove();
            if (!answer && f.hasAttribute(attr)) it.remove();
        }

        // ✅ 교집합 유지 (0 되면 이전 상태 유지)
        if (candidates.isEmpty()) {
            candidates = previous;
        }
    }

    void applyAllergyFilter(List<String> allergyList) {
        Iterator<Food> it = candidates.iterator();

        while (it.hasNext()) {
            Food f = it.next();

            for (String a : allergyList) {
                if (f.attributes.contains(a)) {
                    it.remove();
                    break;
                }
            }
        }
    }

    Food getResult() {
        return candidates.get(new Random().nextInt(candidates.size()));
    }
}
```

