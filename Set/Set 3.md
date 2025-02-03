### 자바가 제공하는 Set 3 - 예제
~~~ java
public class JavaSet {

    public static void main(String[] args) {

        run(new HashSet<>());

        run(new LinkedHashSet<>());

        run(new TreeSet<>());

    }

  

    private static void run(Set<String> set) {

        set.add("C");

        set.add("D");

        set.add("A");

        set.add("1");

        set.add("2");

        System.out.println("set = " + set.getClass());
  

        Iterator<String> iterator = set.iterator();

        while(iterator.hasNext()) {

            System.out.println(iterator.next());

        }

    }

}
~~~

**실행 결과**
~~~ 
set = class java.util.HashSet
A
1
2
C
D
set = class java.util.LinkedHashSet
C
D
A
1
2
set = class java.util.TreeSet
1
2
A
C
D
~~~
- HashSet, LinkedSet, TreeSet 모두 Set 인터페이스를 구현하기 때문에 구현체를 변경하면서 실행할 수 있다.
- iterator()를 호출하면 컬랙션을 반복해서 출력할 수 있다.
	- iterator.hasNaxt()를 호출하면 다음 출력할 데이터의 유무를 반환한다.
	- iterator.naxt()를 호출하면 다음 데이터를 출력한다.

실행 결과를 보면 
- HashSet은 순서를 보장하지 않는다.
- LinkedHashSet은 입력한 데이터의 순서대로 출력한다.
- TreeSet은 입력한 데이터의 순서에 상관없이 데이터의 값을 기준으로 출력한다.

**참고 -  TreeSet의 정렬 기준**
1, 2, 3 이나 “A”, “B”, “C” 기본 데이터는 크다 작다라는 기준이 명확하다. 하지만 개발자가 직접 만든 클래스 Member라는 클래스는 어떻게 크다 작다는 기준을 어떻게 알 수 있을까? 이런 기준은 Comparable, Comparator 인터페이스를 구현해야한다. (나중에 다룸)

## 자바가 제공하는 Set4 - 최적화
**최적화**
우리가 만든 MyHashSet은 자바가 제공하는 HashSet과 비슷하지만 최적화를 추가로 적용해야 한다.
해시 알고리즘을 사용한 자료구조는 데이터의 수가 배열의 크기를 75% 정도를 넘어가면 해시 충돌이 자주 일어난다. 
- 자바의 HashSet은 데이터의 수가 75% 넘어가면 배열의 크기를 2배로 늘려버리고, 2배로 늘어난 크기를 기준으로 다시 인덱스에 요소를 삽입한다.

**75%를 넘어서고 배열의 크기를 2배로 늘림**
![[Pasted image 20250131142341.png]]
- 데이터의 수가 75%를 넘어가면 배열의 크기를 2배로 늘리고, 모든 데이터를 2배로 늘린 배열의 크기를 기준으로 계산한다. 이 과정을 재해싱(rehashing)이라고 한다.
	- 해시 충돌이 감소한다.
	- 여기서 데이터가 다시 75%를 넘어가면 재해싱을 반복한다.

**정리**
실무에서는 HashSet을 가장 많이 사용하며, 입력 순서 유지, 값 정렬에 따라서 LinkedHashSet, TreeSet을 사용한다.

