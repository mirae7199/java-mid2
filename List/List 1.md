## 리스트 추상화1 - 인터페이스 도입

#### List 자료 구조
순서가 있고 중복을 허용하는 자료구조를 List라 한다.
지금까지 만든 MyArrayList와 MyLinkedList는 내부 구현만 다를 뿐 같은 기능을 수행한다.
여기서 핵심은 사용자가 볼 때는 같은 기능을 수행하는 코드로 보일 것이다.
하지만 구현에 따라서 어느 정도의 성능의 차이가 있을 수 있다.
이 둘의 공통 기능을 인터페이스에서 뽑아서 추상화하면 다형성을 활용한 다양한 이득을 얻을 수 있다.

**MyList 인터페이스**
![[Pasted image 20250109184628.png]]
같은 기능을 하는 메서드를 MyList 인터페이스로 뽑아보자


**MyList\<E>를 MyArrayList\<E>로 구현한 코드 중 일부**
![[Pasted image 20250109185053.png]]

**MyList\<E>를 MyLinkedList\<E>로 구현한 코드 중 일부**
![[Pasted image 20250109185216.png]]

여기서 전혀 에러가 나지 않는다. MyList\<E>에서 공통 기능들을 뽑아서 구현한게 MyArrayList\<E>, MyLinked\<E>이기 때문이다.