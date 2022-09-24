# 캡슐화와 추상화 이해하기

## 캡슐화(Encapsulation)
- 캡슐화 이유 : 캡슐화를 통해 Rich domain model을 정의할 수 있다.
  ```
  Encapsulation = Rich domain model
  ```
  - TODO Rich domain model
- 캡슐화 정의
  - **캡슐화는 데이터 무결성을 보호하는 행위입니다(Protecting data integrity).**
    - [데이터 무결성은 데이터를 보호하여 **항상 정상인 데이터를 유지하는 것**이다.](https://terms.naver.com/entry.naver?docId=826184&cid=42344&categoryId=42344)
    - [데이터 무결성은 데이터에 대한 정확성(Accuracy), 일관성(Consistency), 유효성(), 신뢰성()을 보장하기 위해 데이터 변경 혹은 수정 시 여러가지 제한을 두어 데이터의 정확성을 보증하는 것을 말한다.](https://terms.naver.com/entry.naver?docId=2839810&cid=40942&categoryId=32840)
      - 정확성, 일관성, 유효성, 신뢰성 : 데이터를 보호한다.
      - 데이터의 정확성을 보증하는 것을 말한다 : 항상 정상인 테이트를 유지하는 것이다.
  - 용어 정의
    - `TODO` 정확성(Accuracy)
    - [일관성(Consistency)](https://terms.naver.com/entry.naver?docId=3431261&cid=58430&categoryId=58430)
      - 트랜잭션의 일관성(consistency)은 트랜잭션이 성공적으로 수행된 후에도 데이터베이스가 일관성 있는 상태를 유지해야 함을 의미한다. 즉, 트랜잭션이 수행되기 전에 데이터베이스가 일관된 상태였다면 트랜잭션의 수행이 완료된 후 결과를 반영한 데이터베이스도 또 다른 일관된 상태가 되어야 한다는 의미다.
    - `TODO` 유효성
    - `TODO` 신뢰성
- 캡슐화 기술 : 내부 데이터를 유효하지 않거나 일관성이 없는 상태로 설정할 수 없게 만드는 방법이다.
     - 정보은닉(Information hiding) : 내부 데이터 손상을 최소화시킨다(Less risk of corrupting the class's internals).
     - 상태와 행위를 하나의 단위로 묶는 것[Bundling of data and operations] : 클래스에서 수행할 수 있는 모든 작업에 대한 대한 단일 진입정을 제공한다(Perform integrity checks before modifying data).
- TODO 불변(비즈니스 규칙)
  - 클래스가 항상 참이어야하는 조건인 고유한 불변 집합이 있습니다. 준수하는 것은 개발자의 책임이다.
- 캡슐화 예.
   - 잘못된 캡슐화 예.
     ```cs
     // 불변성 : 삼각형은 모서리가 3개이다.
     public class Triangle
     {
     	public IEnumerable<Edge> Edges { get; }

        // 모서리 개수가 3개 미만 또는 3개 초가할 때도 Triangle 객체를 생성할 수 있다.
     	public Triangle(IEnumerable<Edge> edges)
     	{
     		Edges = edges;
     	}
     }

     var two = new Triangle(twoEdges);		// Line
     var four = new Triangle(fourEdges);	// Square
   - 올바른 캡슐화 예 : 3개보다 적거나 많은 모서리을 갖는 것으로부터 스스로 보호한다(캡슐화한다).
     ```cs
     // 불변성 : 삼각형은 모서리가 3개이다.
     public class Triangle
     {
     	public IEnumerable<Edge> Edges { get; }

        // 모서리 개수가 3개 미만 또는 3개 초가할 때는 Triangle 객체를 생성할 수 없다.
     	public Triangle(IEnumerable<Edge> edges)
     	{
            // Guard clause
     		if (edges.Count() != 3)
     			throw new Exception("Triangle must have 3 edges");

     		Edges = edges;
     	}
     }
     ```

## 추상화(Abstraction)
- 추상화 필요성 : `Code Simplification`
  - 복작함을 단순화게 표현할 수 있다(Allow you to express complex ideas as easily as simple ones).
- 추상화 정의
  ```
  추상화 = Single Responsibility Principle + Hierarchy
  ```
  - 분질적인 것을 증폭하고 관련 없는 것을 제거하는 것이다(Abstraction is the amplification of the essential and the elimination of theirrelevant).
    - the amplification of the essential : the current task
    - the elimination of the irrelevant : all other tasks
- 추상화 기술
  - the new method is an abstraction : the amplification of the essential(WHAT : 메서드 이름)
- 추상화 코드
  - 모든 코드가 추상화이다(All code is abstraction).
- 좋은 추상화
  - 메서드나 클래스가 무엇을 하는지 알 수 있는 코드(You can focus on a single application concern).
- 나쁜 추상화
  - 메서드나 클래스가 무엇을 하는지 알 수 없는 코드(Yout have to think of multiple concerns).
1. 추상화 예.
   - 리팩토링 전
     ```cs
     string trimmedName = customerName
     	.Replace(" ", "-")
     	.Trim();

     if (trimmedName.Length > 50)
     {
     	trimmedName = trimmedName.Substring(0, 50);
     }
     ```
   - 리팩토링 후
     ```cs
	 string normalizedName = NormalizeCustomerName(customerName);

     private string NormalizeCustomerName(string name)
     {
     	string trimmedName = name
     		.Replace(" ", "-")
     		.Trim();

     	if (trimmedName.Length > 50)
     	{
     		trimmedName = trimmedName.Substring(0, 50);
     	}

     	return trimmedName;
     }
     ```
	 - the new method is an abstraction
	   - amplifies the essential : 메서드 이름(비즈니스 규칙 WHAT : 이름 정규화, `NormalizeCustomerName`)
	   - eliminates the irrelevant : 메서드 구현(비즈니스 규칙 HOW : 이름 정규화, `NormalizeCustomerName`) 