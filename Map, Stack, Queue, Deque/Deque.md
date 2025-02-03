## Deque 자료 구조
Deque는 양쪽 끝에서 값을 삽입하거나 뺄수 있다. Deque는 일반적인 Stack과 Queue의 기능을 모두 포함하고 있어 매우 유연한 자료 구조이다. Deque에 대해 알아보자.

![Image](https://github.com/user-attachments/assets/bf50bb93-daa1-4436-881c-c3e73e909f50)
- Queue를 배우고 왔다면 이 그림이 무슨 뜻인지 단번에 알 것이다.
	- 양쪽에서 값을 삽입: offerFirst(), offerLast()
	- 양쪽에서 값을 빼기: pollFirst(), pollLast()

**ArrayDeque로 Deque 알아 보기**
~~~ java
public class DequeMain {  
    public static void main(String[] args) {  
        Deque<Integer> deque = new ArrayDeque<>();  
  
        // 데이터 추가  
        deque.offerFirst(1);  
        System.out.println(deque);  
        deque.offerFirst(2);  
        System.out.println(deque);  
        deque.offerLast(3);  
        System.out.println(deque);  
        deque.offerLast(4);  
        System.out.println(deque);  
  
        // 단순 조회하기  
        System.out.println("deque.peekFirst() = " + deque.peekFirst());  
        System.out.println("deque.peekLast() = " + deque.peekLast());  
  
        // 데이터 꺼내기  
        System.out.println("deque.pollFirst() = " + deque.pollFirst());  
        System.out.println("deque.pollFirst() = " + deque.pollFirst());  
        System.out.println("deque.pollLast() = " + deque.pollLast());  
        System.out.println("deque.pollLast() = " + deque.pollLast());  
        System.out.println(deque);  
    }
}
~~~

**실행 결과**
~~~
[1]
[2, 1]
[2, 1, 3]
[2, 1, 3, 4]
deque.peekFirst() = 2
deque.peekLast() = 4
deque.pollFirst() = 2
deque.pollFirst() = 1
deque.pollLast() = 4
deque.pollLast() = 3
[]
~~~
- 앞에 삽입 1: \[1]
- 앞에 삽입 2: \[2, 1]
- 뒤에 삽입 3: \[2, 1, 3]
- 뒤에 삽입 4: \[2, 1, 3, 4]

## Deque와 Stack, Queue
위에서 Deque는 Stack, Queue의 기능을 모두 포함한다고 말했는데 Stack을 사용할 때, Queue를 사용할 때의 메서드가 나눠져 있다. Deque가 제공하는 stack과 Queue 메서드에 대해 알아보자.

**Deque - Stack**
![Image](https://github.com/user-attachments/assets/901a5603-b249-4ee8-9e61-a5990489c95e)
- push(): 값을 앞에서 삽입한다.
- pop(): 값을 앞에서 꺼낸다.

**Deque - Queue**
![Image](https://github.com/user-attachments/assets/40315310-a260-4428-b95a-5d3a19565e11)
- offer(): 값을 뒤에서 삽입한다.
- poll(): 값을 앞에서 꺼낸다.

*Deque - Stack, Queue 예제 코드는 똑같기 때문에 생략한다.*

Deque에서 Stack과 Queue을 위한 메서드가 제공된다는 것을 확인할 수 있다. Stack 클래스는 성능이 좋지 않기 때문에 Deque에 ArrayDeque 구현체를 사용하자. Queue의 기능만 필요하다면 Queue 인터페이스를 사용하고 더 많은 기능을 원하다면 Deque 인터페이스를 사용하자. 그리고 성능이 더 좋은 구현체인 ArrayDeque을 사용하자.





