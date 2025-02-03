## Stack 구조
**stack에 블록 넣기**
![[Pasted image 20250203122504.png]]
- 1번, 2번, 3번 블록이 있다고 가정하고 stack이라는 곳에 넣는다고 해보자.
- 입구는 하나이므로 1 -> 2 -> 3 순으로 넣으면 넣은 순서대로 블록은 쌓인다.
- 첫 블록이 제일 아래에 쌓이는 것이다.

**stack에 있는 블록 빼기**
![[Pasted image 20250203123024.png]]
- 블록을 뺄때도 마찬가지로 입구가 하나이므로 마지막에 넣은 블록부터 빼야한다.

정리해보자면 (넣기)1 -> 2 -> 3  (빼기)3-> 2 -> 1 이런식으로 마지막으로 넣은 3번이 가장 먼저 나온다. 이렇게 가장 마지막에 넣은 값이 가장 먼저 나오는 것을 **후입 선출(LIFO, Last In First Out)** 이라고 한다. 또한 이러한 자료구조를 스택(Stack)이라고 한다.

**예제 코드**
~~~ java
public class StackMain {  
    public static void main(String[] args) {  
        Stack<Integer> stack = new Stack<>();  
  
        stack.push(1);  
        stack.push(2);  
        stack.push(3);  
        System.out.println(stack);  
  
        // 다음 꺼낼 요소 확인(꺼내지 않고 단순 조회만)  
        System.out.println("stack.peek() = " + stack.peek());  
  
        System.out.println("stack.pop() = " + stack.pop());  
        System.out.println("stack.pop() = " + stack.pop());  
        System.out.println("stack.pop() = " + stack.pop());  
        System.out.println(stack);  
    }
}
~~~
- stack.push(1): 값을 삽입할 때 쓴다.
- stack.peek(): 값을 꺼내지 않고 단순 조회할 떄 쓴다.
- stack.pop(): 값을 꺼낼때 쓴다.

**실행 결과**
~~~
[1, 2, 3]
stack.peek() = 3
stack.pop() = 3
stack.pop() = 2
stack.pop() = 1
[]
~~~
- 제일 마지막에 넣었던 3이 제일 먼저 나온것을 확인할 수 있었음.

> **주의! - stack 클래스는 사용하지 말것**
> 자바의 stack 내부에는 Vector라는 자료구조를 사용하는데 이 자료구조는 자바 1.0에 개발되었음. 지금은 사용하지 않고, 하위 호환을 위해 존재함. Deque를 사용할 것을 권장함.
