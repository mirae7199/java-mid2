## 리스트 추상화2 - 의존관계 주입

MyArrayList를 활용해서 BatchProcessor 클래스를 개발하고 있다고 가정. 개발하다 보니 프로그램에 추가할 데이터가 앞에서 추가할 때가 더 많다고 가정해보자. 그러면 MyArrayList는 앞에서 추가하는 것이 더 성능이 느리기 때문에 MyLinkedList로 바꿔야 한다.

**구체적인 MyArrayList\<E>에 의존하는 BatchProcessor**
![[Pasted image 20250109190018.png]]
이렇게 BatchProcessor 클래스(BP)가 MyArrayList를 구체적으로 의존하게 되면 나중에 코드를 바꿀 필요가 있을 때  Bp 클래스의 코드를 모두 바꿔야 할 경우가 생긴다. 이런 경우를 대비하여 추상화에서 기능을 뽑아서 쓴다면 코드를 싹다 바꾸는 일은 없을 것이다.


**추상적인 MyList에 의존하는 BatchProcessor**
![[Pasted image 20250109190607.png]]

![[Pasted image 20250109190928.png]]
BP를 생성하는 시점에 생성자를 통해서 원하는 리스트 전략(알고리즘)을 선택해서 전달하면 된다.
이렇게 하면 BP를 전혀 바꾸지 않고 원하는 리스트를 사용할 수 있다.

정리하면 추상화와 다형성을 통해서 코드를 전혀 바꾸지 않고 손쉽게 ArrayList 또는 LinkedList를 바꾸어 쓸 수 있다는 것이다.

**BatchProcessor 클래스의 실제 코드**
![[Pasted image 20250109192056.png]]
- logic(int size)는 매우 복잡한 기능을 다루는 메서드라고 가정하고 list 앞 부분에 데이터를 추가한다.
- 어떤 MyList의 구현체를 쓸 것인지는 실행 시점에 생성자를 통해서 결정한다.
	- 생성자에는 MyArrayList 또는 MyLinkedList가 들어 올 수 있다.
- 이것은 BatchProcessor의 외부에서 의존관계가 결정되어서 BatchProcessor 인스턴스에 들어오는 것 같다. 마치 의존관계가 외부에서 주입되는 것 같다고 해서 **의존관계 주입**이라고 한다.
- 참고로 여기서는 생성자를 통해 주입했으므로 생성자 의존관계 주입이라고 한다.
- **System.currentTimeMillis();**
	- 메서드 실행 시간을 알기 위해 (endTime - startTime) 코드를 넣었다.

#### 의존관계 주입
- Dependency Injection 줄여서 DI라고 부른다. 의존성 주입라고도 부름


**BatchProcessorMain 코드**
![[Pasted image 20250109192206.png]]
- 생성자의 매개변수를 넣을 때 MyList의 어떤 구현체를 쓸 것인지 결정할 수 있음

**MyArrayList 실행 결과 O(n)**
> 크기: 50000, 계산 시간: 2820ms

**MyLinkedList 실행 결과 O(1)**
> 크기: 50000, 계산 시간: 5ms

MyLinkedList를 사용한 덕분에 O(n) -> O(1)로 훨씬 더 성능이 개선 된 것을 확인할 수 있다. 데이터가 증가할수록 성능의 차이는 더 벌어진다.


