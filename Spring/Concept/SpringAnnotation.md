# 📄 [Spring] Spring Annotation
## **Spring Annotation의 종류와 그 역할**
### 1. ```@Component```
* Component-scan을 선언에 의해 특정 패키지 안의 클래스들을 스캔하고, ```@Component```Annotation이 있는 클래스에 대하여 bean 인스턴스를 생성.

### 2. ```@Controller, @Service, @Repository```
* ```@Component``` 구체화 -> ```@Controller, @Service, @Repository``` 
* bean으로 등록
* 해당 클래스가 Controller/Service/Repository로 사용됨을 Spring Framework에 알린다.

### 3. ```@RequestMapping```
```
@Controller
@RequestMapping("/home") // 1) Class Level
public class HomeController {
    /* an HTTP GET for /home */ 
    @RequestMapping(method = RequestMethod.GET) // 2) Handler Level
    public String getAllEmployees(Model model) {
        ...
    }
    /* an HTTP POST for /home/employees */ 
    @RequestMapping(value = "/employees", method = RequestMethod.POST) 
    public String addEmployee(Employee employee) {
        ...
    }
}
```
* ```@RequestMapping```에 대한 모든 매핑 정보는 Spring에서 제공하는 HandlerMapping Class 가 가지고 있다.
1. Class Level Mapping
   * 모든 메서드에 적용되는 경우
   * "/home"으로 들어오는 모든 요청에 대한 처리를 해당 클래스에서 한다는 것을 의미한다.

2. Handler Level Mapping
   * 요청 url에 대해 해당 메서드에서 처리해야 되는 경우
   * "/home/employees"POST 요청에 대한 처리를 addEmployee()에서 한다는 것을 의미한다.
 * value: 해당 url로 요청이 들어오면 이 메서드가 수행된다.
 * method: 요청 method를 명시한다. 없으면 모든 http method 형식에 대해 수행된다.
