## 컬렉션 프레임워크 - Map 구현체
Map의 대표적인 구현체는 HashMap, LinkedHashMap, TreeMap이 있다.

Map의 Key는 중복을 허용하지 않고, 순서를 보장하지 않는다. 어디서 많이 본 자료구조이지 않는가??
중복 X, 순서 보장 X  -> Set 자료구조이다. Map은 Key를 중심으로 동작한다. Value의 값이 없다고 생각하면 Set 자료구조와 거의 같다. 쉽게 말하면 Set 자료구조에 Value를 붙여둔것이 Map 자료구조이다.

**Set 자료구조**

| Key  |     |     |  Key   |     |
| :--: | :-: | --- | :----: | :-: |
| 학생 1 |     |     | "APPL" |     |
| 학생 2 |     |     | "TSLA" |     |
| 학생 3 |     |     | "CPNG" |     |
| 학생 4 |     |     | "NVDA" |     |

**Map 자료 구조**

| Key  | Value |     |  Key   | Value  |
| :--: | :---: | --- | :----: | :----: |
| 학생 1 |  90   |     | "APPL" |  "얘플"  |
| 학생 2 |  80   |     | "TSLA" | "톄슬라"  |
| 학생 3 |  80   |     | "CPNG" |  "쿠팡"  |
| 학생 4 |  100  |     | "NVDA" | "엔비디아" |

이런 이유로 Set과 Map의 구현체는 거의 같다.
- HashSet -> HashMap
- LinkedHashSet -> LinkedHashMap
- TreeSet -> TreeMap

Set을 배워본 입장으로 뭔가 예측이 되지 않는가??
HashMap은 순서를 보장하지 않고
LinkedHashMap은 순서를 보장하고
TreeMap은 값을 기준으로 정렬된 순서로 저장

### 1.HashMap
- HashMap은 해시 알고리즘을 사용해서 저장한다. 주어진 key값은 해시 함수를 통해서 해시 코드를 생성하고, 이 해시 코드로 데이터를 저장/검색 하는데 사용한다.
- 해시 자료구조를 사용하기 때문에 평균적으로 O(1)의 시간 복잡도를 가진다.
- 순서를 보장하지 않는다.

### 2.LinkedHashMap
- LinkedHashMap은 HashMap에 연결 리스트를 넣은 것이다. HashMap과 비슷하지만 연결 리스트로 다음 데이터를 참조하기 때문에 추가된 순서로 순서를 보장한다.
- 추가된 순서로 데이터를 조회(순회)한다. 예를 들면 A->1, D->2, C->3 이 순서로 추가되면 그대로      A->1, D->2, C->3 순으로 출력된다는 것이다.
- HashMap과 비슷하고 해시 자료구조를 사용하기 떄문에 평균적으로 O(1)의 시간 복잡도를 가진다.
- 추가된 순서를 보장한다.

### 3.TreeMap
- TreeMap은 레드-블랙 트리를 기반으로 한 구현이다.
- Comparator에 의해 모든 key가 정렬된다.
- 추가/검색/삭제 등 주요 작업들은 O(log n)의 시간 복잡도를 가진다.
- key 값을 기준으로 정렬된 순서를 보장한다.

**예제 코드**
~~~ java
public class JavaMapMain {  
    public static void main(String[] args) {  
	    System.out.println("HashMap 호출");  
		run(new HashMap<>());  
		System.out.println("LinkedHashMap 호출");  
		run(new LinkedHashMap<>());  
		System.out.println("TreeMap 호출");  
		run(new TreeMap<>());
    }
      
    static void run(Map<String, Integer> map) {  
        map.put("C", 10);  
        map.put("B", 20);  
        map.put("A", 30);  
        map.put("1", 40);  
        map.put("2", 50);  
        System.out.println(map);  
  
        Set<String> keySet = map.keySet();  
        for (String key : keySet) {  
            System.out.println(key + " = " + map.get(key));  
        }        System.out.println();  
  
    }
}
~~~
- run()메서드는 Map의 구현체라면 모두 쓸 수 있다.

**실행 결과**
~~~
HashMap 호출
{A=30, 1=40, B=20, 2=50, C=10}
A = 30
1 = 40
B = 20
2 = 50
C = 10

LinkedHashMap 호출
{C=10, B=20, A=30, 1=40, 2=50}
C = 10
B = 20
A = 30
1 = 40
2 = 50

TreeMap 호출
{1=40, 2=50, A=30, B=20, C=10}
1 = 40
2 = 50
A = 30
B = 20
C = 10
~~~
실행 결과를 보면 
HashMap은 순서를 보장하지 않는다.
LinkedHashMap은 입력된 순서를 보장한다.
TreeMap은 key값을 기준으로 정렬된 것을 확인할 수 있다.

### 자바의 HashMap 작동 원리
HashSet과 HashMap은 작동 원리가 같다.
차이점이라면 
- Key를 사용해서 해시 코드를 생성한다.
- Key-Value를 쌍으로 저장하기 위해서 Entry 객체를 사용한다.

![Image](https://github.com/user-attachments/assets/5fde3ec9-dc39-4641-9c89-a523abfb04ba)

해시를 사용해서 키와 값을 저장하는 자료구조를 해시 테이블이라고 한다.

직접 만든 객체일 경우에 hashCode, equals를 반드시 구현해야 정확한 값이 저장/검색/삭제될 것이다.

**정리**
실무에서는 HashMap를 많이 사용한다. 순서를 유지할 경우에 LinkedHashMap을 쓰고 값을 기준으로 정렬하고 싶을 때 TreeMap을 사용하자.
