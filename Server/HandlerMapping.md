🔹 HandlerMapping
📘 개념
클라이언트의 요청 URL에 따라 어떤 Controller의 어떤 메서드가 호출될지 결정하는 인터페이스.

DispatcherServlet이 요청을 받으면 가장 먼저 이 컴포넌트를 통해 핸들러(컨트롤러 메서드)를 찾음.

🧠 왜 사용하는가?
URL을 매핑하는 로직을 분리함으로써 코드 유지보수성과 유연성을 확보하기 위해 사용.

🕰️ 발전 배경
초창기에는 web.xml에 직접 servlet이나 URL 패턴을 등록하거나, Controller 안에서 if/else나 switch로 직접 분기했음.

→ 코드가 복잡해지고 URL 관리가 어려워짐.

📌 어디서 사용되는가?
Spring MVC의 핵심 컴포넌트.

@RequestMapping, @GetMapping, @PostMapping과 함께 사용.

📎 주요 구현체
RequestMappingHandlerMapping (가장 많이 사용됨, @RequestMapping 기반)

SimpleUrlHandlerMapping (XML 기반 URL 매핑)

🧑‍💻 코드 예시
java
복사
편집
@Controller
public class MyController {

    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("msg", "안녕하세요!");
        return "hello"; // View 이름 반환
    }
}
🔎 여기서 RequestMappingHandlerMapping이 "/hello" 요청을 hello() 메서드로 매핑함.