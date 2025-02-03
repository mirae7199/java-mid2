## Queue 자료 구조
**Queue에 블록 넣기, 빼기**
![Image](https://github.com/user-attachments/assets/861f637f-dc77-4ea4-9883-7744d31ca927)
- 1 -> 2 -> 3번 순으로 블록넣는다고 가정하자.
- 입구와 출구가 서로 반대방향에 있다고 생각하면 된다.
- Stack 처럼 값을 넣을 때 블록이 쌓이지만, 값을 뺄때는 반대 방향의 출구로 빠진다.

정리하면 (넣기)1 -> 2 -> 3  (빼기)1 -> 2 -> 3 넣는 순서와 빼는 순서가 같다. 처음 넣은 1번 블록이 가장 먼저 나온다. 이렇게 가장 먼저 들어가서 가장 먼저 나오는 것을 **선입 선출(FIFO, First In First Out)** 라고 한다. 또 이러한 자료 구조를 큐(Queue)라고 한다.

**컬렉션 프레임워크 - Queue**

![Image](https://github.com/user-attachments/assets/7c64c618-0bbe-4954-9c2a-ba5bd679dc96)
- Queue는 List나 Set과 같이 Collection의 자식이다.
- 구현체는 ArrayDeque, LinkedList가 있다.

**ArrayDeque으로 Queue 사용해보기**
~~~ java
public class QueueMain {  
    public static void main(String[] args) {  
        Queue<Integer> queue = new ArrayDeque<>();  
  
        // 데이터 추가  
        queue.offer(1);  
        queue.offer(2);  
        queue.offer(3);  
        System.out.println(queue);  
  
        // 단순 조회  
        System.out.println("queue.peek() = " + queue.peek());  
  
        // 데이터 빼기  
        System.out.println("queue.poll() = " + queue.poll());  
        System.out.println("queue.poll() = " + queue.poll());  
        System.out.println("queue.poll() = " + queue.poll());  
        System.out.println(queue);  
    }
}
~~~
- queue.offer(1): queue에 값을 삽입할 때 사용함.
- queue.peek(): 단순 조회할 때 사용함.
- queue.poll(): queue에 있는 값을 뺄때 사용함.

**실행 결과**
~~~
[1, 2, 3]
queue.peek() = 1
queue.poll() = 1
queue.poll() = 2
queue.poll() = 3
[]
~~~
- 가장 먼저 넣은 값 1이 가장 먼저 나온것을 확인할 수 있음.
