## 컬렉션 프레임워크 - HashSet

### 직접 구현하는 Set 1 - MyHashSetV1

**해시 알고리즘을 사용하도록 개선된 MyHashSetV1**
~~~ java
public class MyHashSetV1 {  
  
    private final int DEFAULT_INITIAL_CAPACITY = 16;  
  
    LinkedList<Integer>[] buckets;  
  
    private int size = 0;  
    private int capacity = DEFAULT_INITIAL_CAPACITY;  
  
    public MyHashSetV1(int capacity) {  
        this.capacity = capacity;  
        initBuckets();  
    }  
    
	private void initBuckets() {  
	    buckets = new LinkedList[capacity];  
	    for (int i = 0; i < capacity; i++) {  
	        buckets[i] = new LinkedList<>();  
	    }
    }

    public boolean add(int value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Integer> bucket = buckets[hashIndex];  
  
        if(bucket.contains(value)) {  
            return false;  
        } 
         
        bucket.add(value);  
        size++;  
  
        return true;  
  
    }  
    
    public boolean contains(int searchValue) {  
        int hashIndex = hashIndex(searchValue);  
  
        LinkedList<Integer> bucket = buckets[hashIndex];  
  
        return bucket.contains(searchValue);  
    }  
    
    public boolean remove(int value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Integer> bucket = buckets[hashIndex];  
        boolean result = bucket.remove(Integer.valueOf(value));  
        
        if(result) {  
            size--;  
            return true;  
        } else {  
            return false;  
        }    
    }  
    
    private int hashIndex(int value) {  
        return value % capacity;  
    }  
    public int getSize() {  
        return size;  
    }  
    
    @Override  
    public String toString() {  
        return "MyHashSetV1{" +  
                ", buckets=" + Arrays.toString(buckets) +  
                ", size=" + size +  
                '}';  
    }
}
~~~

- buckets: 연결 리스트를 배열로 사용한다.
	- 배열안에 연결 리스트가 들어있고, 연결 리스트에 데이터가 저장된다.
	- 해시 인덱스 충돌이 일어나면 연결 리스트에 여러 데이터가 저장된다.
- initBuckets: 생성자를 호출할 때 capacity만큼 배열안의 연결 리스트를 생성한다. 여기서는 기본값으로 16으로 설정하였다. 따라서  buckets안의 연결 리스트 객체는 16개가 생성된다.
- add: hashIndex를 구해서 그에 해당하는 배열을 bucket에 참조하게 하였다. 그리고 중복이 없을 시 연결 리스트에 데이터를 저장하고 return을 반환한다.
- contains: 데이터 중복 유무를 반환해준다.
- remove: hashIndex에 해당하는 배열에 데이터 요소를 검색하고 있다면 삭제를 해준다.

~~~ java
public class MyHashSetV1Main {  
    public static void main(String[] args) {  
        MyHashSetV1 set = new MyHashSetV1(10);  
        set.add(1);  
        set.add(2);  
        set.add(5);  
        set.add(8);  
        set.add(14);  
        set.add(99);  
        set.add(9); // hashIndex 중복  
        System.out.println(set);  
  
        // 검색  
        int searchValue = 9;  
        boolean result = set.contains(searchValue);  
        System.out.println("set.contains(" + searchValue + ") = " + result);  
  
        // 삭제  
        boolean removeResult = set.remove(searchValue);  
        System.out.println("removeResult = " + removeResult);  
        System.out.println(set);
	}
}
~~~

**실행 결과**
~~~
MyHashSetV1{, buckets=[[], [1], [2], [], [14], [5], [], [], [8], [99, 9]], size=7}
set.contains(9) = true
removeResult = true
MyHashSetV1{, buckets=[[], [1], [2], [], [14], [5], [], [], [8], [99]], size=6}
~~~

- 생성: MyHashSet()의 기본 생성자 값은 capacity가 16이지만 여기서는 10으로 설정했다.
- 저장: add() 메서드를 이용해서 0~99 값의 데이터를 저장하였다. 해시 충돌이 일어나도록 설정 했으며 해시 인덱스 9을 보면 연결 리스트의 데이터가 99, 9가 있는 것을 확인 할 수 있다.
- 검색: 검색할 데이터 9로 해시 인덱스를 구해서 set에 중복 데이터가 있는지 확인하였다. 데이터 9이기 때문에 해시 인덱스는 9이고 연결 리스트에 있는 모든 데이터를 순서대로 비교해서 9를 찾는다.

MyHashSetV1은 해시 알고리즘을 사용한 덕분에 등록, 검색, 삭제 모두 평균 O(1)로 연산 속도가 크게 개선했다.

**해결 해야 할 문제**
지금까지는 주어진 데이터를 해시 인덱스를 구해서 인덱스로 사용했다. 근데 데이터가 숫자가 아니라면 어떻게 해야할까? "A", "B" 같은 문자라면 해시 인덱스를 어떻게 구해서 인덱스로 사용할 수 있는가?

### 문자열 해시 코드
모든 문자는 고유의 숫자로 표현할 수 있다. 예를 들어 'A' -> 65, 'B' -> 66으로 표현된다. 또한 "AB" 같은 연속된 문자는 각각의 고유의 숫자를 더한 값이 나온다. "AB" -> 65 + 66 -> 131이다.

> ASCII 코드를 참고할 것

![[Pasted image 20250125144031.png]]
- hashCode() 메서드를 사용해서 문자열을 해시 코드로 변경한다. 이렇게 해서 나온 고유의 숫자를 해시 코드라고 한다.
- 해시 코드로 해시 인덱스를 생성해서 데이터를 저장한다.

**해시 함수**
- 해시 함수는 임의의 길이의 데이터를 입력받아 고정된 길이의 해시값(해시 코드)을 출력하는 함수이다.
- 다른 데이터를 입력해서 같은 해시 코드가 출력될 수 있는데 이를 해시 충돌이라 한다.
	- "BC" -> 66 + 67 -> 133
	- "AD" -> 65 + 68 -> 133

**해시 코드(Hash Code)**
해시 코드는 데이터를 대표하는 값을 뜻한다.
- A의 해시 코드 65
- B의 해시 코드 66
- C의 해시 코드 67

**해시 인덱스(Hash Index)**
- 해시 인덱스는 데이터의 저장 위치를 결정한다.
- 보통 해시 코드에 배열의 크기를 나누어 구한다.

**정리**
문자 데이터를 사용할 때 해시 함수를 사용해서 해시 코드로 변환한 덕분에 해시 인덱스를 사용할 수 있게 되었다. 따라서 숫자 뿐만 아니라 문자 데이터도 해시 인덱스를 통해서 빠르게 저장, 조회할 수 있다.

그렇다면 내가 직접 만든 객체 Member, User같은 객체들은 어떻게 해시 코드를 정의하는가?

### 자바의 hashCode()

**Object.hashCode()**
자바는 모든 객체가 자신만의 해시 코드를 표현할 수 있는 기능을 제공한다. Object의 hashCode이다.

~~~ java
public class Object{
	public int hashCode();
}
~~~
- 그대로 사용하지 않고 재정의(override)해서 많이 쓴다.
- 이 메서드의 기본 구현은 참조값을 받아오는 것이다.
- 즉, Object의 hashCode()와 재정의 한 hashCode()는 다르다.

~~~ java
public class Member {  
    private String id;  
  
    public String getId() {  
        return id;  
    }  
    
    public void setId(String id) {  
        this.id = id;  
    }  
    
    @Override  
    public boolean equals(Object o) {  
        if (o == null || getClass() != o.getClass()) return false;  
        Member member = (Member) o;  
        return Objects.equals(id, member.id);  
    }  
    
    @Override  
    public int hashCode() {  
        return Objects.hashCode(id);  
    }  
    
    @Override  
    public String toString() {  
        return "Member{" +  
                "id='" + id + '\'' +  
                '}';  
    }
}
~~~
- Member의 id기준으로 quals 비교를 하고, hashCode 생성

~~~ java
public class JavaHashCodeMain {  
    public static void main(String[] args) {  
        // Object의 기본 hashCode는 객체의 참조값을 기반으로 생성  
        Object obj1 = new Object();  
        Object obj2 = new Object();  
        System.out.println("obj1.hashCode() = " + obj1.hashCode());  
        System.out.println("obj2.hashCode() = " + obj2.hashCode());  
  
        // 각 클래스마다 hashCode를 이미 오버라이딩 해두었다.  
        Integer i = 10;  
        String strA = "A";  
        String strAB = "AB";  
        System.out.println("10.hashCode() = " + i.hashCode());  
        System.out.println("'A'.hashCode() = " + strA.hashCode());  
        System.out.println("'AB'.hashCode() = " + strAB.hashCode());  
  
        // hashCode는 마이너스 값이 들어올 수 있다.  
        System.out.println("-1.hashCode() = " + Integer.valueOf(-1).hashCode());  
  
        // 둘은 같을까 다를까?, 인스턴스는 다르지만, equals는 같다.  
        Member member1 = new Member("idA");  
        Member member2 = new Member("idA");  
  
        // equals, hashCode를 오버라이딩 하지 않은 경우와, 한 경우를 비교  
        System.out.println("(member1 == member2) = " + (member1 == member2));  
        System.out.println("member1.equals(member2) = " + member1.equals(member2));  
        System.out.println("member1.hashCode() = " + member1.hashCode());  
        System.out.println("member2.hashCode() = " + member2.hashCode());  
  
    }
}
~~~

**실행 결과**
~~~
obj1.hashCode() = 157627094
obj2.hashCode() = 1811044090
10.hashCode() = 10
'A'.hashCode() = 65
'AB'.hashCode() = 2081
-1.hashCode() = -1
(member1 == member2) = false
member1.equals(member2) = true
member1.hashCode() = 104070
member2.hashCode() = 104070
~~~
- 각 Integer, String .. 등등 의 타입들은 이미 hashCode를 재정의 해두었다. 들어오는 값이 같다면 hashCode의 값도 같다.
- Member의 객체는 다르지만 들어온 id값이 같으면 equals는 같다.
- member1 == member2는 물리적으로 참조값이 같은지를 묻는 것이다. (동일성)
- member1.equals(member2)는 논리적으로 값이 같은지를 묻는 것이다. (동등성)

#### 직접 구현하는 해시 코드
Member의 경우 회원의 id가 같으면 객체가 다르더라도 논리적으로 같은 회원으로 볼 수 있다. 따라서 id을 기준으로 동등성을 비교해야하므로 equals를 재정의해야 한다.
hashCode도 이와 같다. 회원의 id같으면 논리적으로 같은 회원이므로 회원 id 기준으로 해시코드를 생성해야한다.

**Member의 hashCode() 구현**
- Member는 hashCode()를 재정하였다.
- 재정의 하지 않은 hashCode()는 Object에서 기본으로 제공하는 기능이다. 이 기능은 객체의 참조값을 기준으로 해시코드를 생성한다. 회원 id 같더라도 다른 해시코드를 반환한다는 것이다.
- Member에서는 id를 기준으로 hashCode()를 재정의 했기 때문에 회원 id 같으면 같은 해시코드를 반환하게 된 것이다.

**정리**
- Object가 기본으로 제공하는 equals 와 hashCode는 참조값 기준이다.
- 이미 만들어진 타입들 String, int, boolean ,,, 등은 이미 재정의가 되어 있다.
- 개발자가 만든 객체는 직접 equals와 hashCode를 재정의 해야 한다. (IDE에서 제공함!)

### 직접 구현하는 Set2 - MyHashSetV2
모든 타입을 저장할 수 있는 Set을 만들어보자.

~~~ java
public class MyHashSetV2 {  
  
    private final int DEFAULT_INITIAL_CAPACITY = 16;  
  
    LinkedList<Object>[] buckets;  
  
    private int size = 0;  
    private int capacity = DEFAULT_INITIAL_CAPACITY;  
  
    public MyHashSetV2() {  
        initBuckets();  
    }  
    
    public MyHashSetV2(int capacity) {  
            this.capacity = capacity;  
            initBuckets();  
    }  
    
    private void initBuckets() {  
        buckets = new LinkedList[capacity];  
        for (int i = 0; i < capacity; i++) {  
            buckets[i] = new LinkedList<>();  
        }        
    }  
    
    public boolean add(Object value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Object> bucket = buckets[hashIndex];  
  
        if(bucket.contains(value)) {  
            return false;  
        }  
        bucket.add(value);  
        size++;  
  
        return true;  
  
    }  
    
    public boolean contains(Object searchValue) {  
        int hashIndex = hashIndex(searchValue);  
  
        LinkedList<Object> bucket = buckets[hashIndex];  
  
        return bucket.contains(searchValue);  
    }  
    
    public boolean remove(Object value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Object> bucket = buckets[hashIndex];  
        boolean result = bucket.remove(value);  
        if(result) {  
            size--;  
            return true;  
        } else {  
            return false;  
        }
    } 
         
    private int hashIndex(Object value) {  
        return Math.abs(value.hashCode()) % capacity;  
    }  
    
    public int getSize() {  
        return size;  
    }  
    
    @Override  
    public String toString() {  
        return "MyHashSetV1{" +  
                ", buckets=" + Arrays.toString(buckets) +  
                ", size=" + size +  
                '}';  
    }
}
~~~

**private LinkedList\<Object>[] buckets;**
- MyHashSetV1에서는 \<Integer>로 설정해서 int만 들어 올 수 있게 했었다.
- V2에서는 Object 타입으로 바꿔서 모든 타입을 저장할 수 있게 하였다.

**HashIndex() 변경**
- 기존 HashIndex에서는 Int만 저장할 수 있게 설정해서 value % capacity로 했었다.
- 지금은 모든 객체가 들어올 수 있기 때문에 들어온 value의 hashCode를 구한 후에 capacity 나머지 연산을 해주었다.
- Object의 hashCode()를 사용한 덕분에 모든 객체의 HashCode()를 구할 수 있다. 물론 만약 개발자가 만든 타입이 들어온다 해도 재정의한 hashCode()가 들어갈 것이다. (다형성에 의해)
- hashCode()는 음수의 값이 나올 수 있기 때문에 Math.abs() 절대값 함수를 사용하였다.

**MyHashSetV2Main**
~~~ java
public class MyHashSetV2Main {  
    public static void main(String[] args) {  
        MyHashSetV2 set = new MyHashSetV2(10);  
  
        set.add("A");  
        set.add("B");  
        set.add("C");  
        set.add("D");  
        set.add("AB");  
        set.add("SET");  
        System.out.println(set);  
  
        System.out.println("\"A\".hashCode() = " + "A".hashCode());  
        System.out.println("\"B\".hashCode() = " + "B".hashCode());   
        System.out.println("\"AB\".hashCode() = " + "AB".hashCode());  
        System.out.println("\"SET\".hashCode() = " + "SET".hashCode());  
  
        // 검색  
        String searchValue = "SET";  
        boolean result = set.contains(searchValue);  
        System.out.println("set.contains(" + searchValue + ") = " + result);  
  
    }
}
~~~

**실행 결과**
~~~
MyHashSetV2{, buckets=[[], [AB], [], [], [], [A], [B, SET], [C], [D], []], size=6}
"A".hashCode() = 65
"B".hashCode() = 66
"AB".hashCode() = 2081
"SET".hashCode() = 81986
set.contains(SET) = true
~~~

![[Pasted image 20250127095851.png]]
- 자바에서 기본으로 제공하는 클래스들은 hashCode가 재정의 되어 있다. 따라서 String도 마찬가지이다.
- hashIndex(Object value)를 호출하면 String "A" 값이 들어가면서 String에 재정의된 hashCode()가 호출된다. 해시코드가 생성되면 CAPACITY와 나머지 연산을 해서 해시 인덱스가 반환된다.

### 직접 구현하는 Set3 - 직접 만든 객체 보관
MyHashSetV2는 모든 객체를 받아서 저장할 수 있다. 직전에 만든 Member도 저장해보자.
단, 직접 만든 클래스의 경우 equals와 hashCode를 재정의해야 한다.

~~~ java
public class MyHashSetV2Main2 {  
    public static void main(String[] args) {  
        MyHashSetV2 set = new MyHashSetV2(10);  
        Member hi = new Member("hi");  
        Member jpa = new Member("JPA");  
        Member java = new Member("java");  
        Member spring = new Member("spring");  
  
        System.out.println("hi.hashCode() = " + hi.hashCode());  
        System.out.println("jpa.hashCode() = " + jpa.hashCode());  
        System.out.println("java.hashCode() = " + java.hashCode());  
        System.out.println("spring.hashCode() = " + spring.hashCode());  
  
        set.add(hi);  
        set.add(jpa);  
        set.add(java);  
        set.add(spring);  
        System.out.println(set);  
  
        // 검색  
        Member searchValue = new Member("JPA");  
        boolean result = set.contains(searchValue);  
        System.out.println("set.contains(" + searchValue + ") = " + result);  
    }
}
~~~

**실행 결과**
~~~
hi.hashCode() = 3329
jpa.hashCode() = 73659
java.hashCode() = 3254818
spring.hashCode() = -895679987
MyHashSetV2{, buckets=[[], [], [], [], [], [], [], [Member{id='spring'}], [Member{id='java'}], [Member{id='hi'}, Member{id='JPA'}]], size=4}
set.contains(Member{id='JPA'}) = true
~~~

![[Pasted image 20250127101318.png]]
*코드와 그림은 다를 수 있다.

- Member 객체는 hashCode()를 재정의 했었다.
- hashIndex(Object value)에서 value.hashCode()를 호출하면 Member에서 재정의한 hashCode()가 호출된다.

**Equals의 사용**
- 그렇다면 equals는 어디서 사용되는 걸까?
	- contains(Object value)가 호출되면 검색할 값 searchValue의 hashIndex()를 호출하고 해시 인덱스를 생성한다. (searchValue = new Member("JPA") 라 가정)
	- 해시 인덱스로 해당하는 인덱스 \[0]를 검색한다. 연결 리스트 안 모든 데이터 \[hi, JPA]를 검색하는데 이 때 데이터를 하나하나 equals()를 사용해서 비교한다.

따라서 해시 자료 구조를 사용할 때는 equals와 hashCode도 반드시 재정의 해야 한다.

#### hashCode와 equals를 재정의 하지 않으면 어떻게 될까?
![[Pasted image 20250127103046.png]]
- 각각 m1 -> "A" , m2 -> "A" 를 넣었다.
- 예상이 가겠지만  hashCode()를 재정의 하지 않아서 Object에서 제공하는 기능으로 해시 코드가 반환된다. 이 때 해시코드는 참조값을 기준으로 하기 때문에 값이 다르다.
- 해시 코드 값이 다르기 때문에 해시 인덱스도 다르게 나온다.
- 또 "A" 라는 회원의 id 값이 중복 저장하게 된다.

**검색 문제**
![[Pasted image 20250127103505.png]]
- MemberNoHashNoEq searchValue = new MemberNoHashNoEq("A");
- "A" 값을 검색한다고 가정했을 때 해시 코드를 구하지만 참조값이 기준이기 때문에 값이 다르게 나온다.
- 엉뚱한 해시 인덱스가 나와서 검색에 실패된다.

### [[직접 구현하는 Set4 - 제네릭과 인터페이스 도입]]
제네릭을 도입해서 안전성을 높이기

~~~ java
public class MyHashSetV3<E> implements MySet<E> {  
  
    private final int DEFAULT_INITIAL_CAPACITY = 16;  
  
    LinkedList<E>[] buckets;
	,,,
~~~
- 이전 코드에서 Object였던 부분을 제네릭의 타입 매개변수 E로 변경했다.

~~~ java
public class MyHashSetV3Main {  
    public static void main(String[] args) {  
        MySet<String> set = new MyHashSetV3<>(10);  
        set.add("A");  
        set.add("B");  
        set.add("C");  
        System.out.println(set);  
  
        // 검색  
        String searchValue = "A";  
        boolean result = set.contains(searchValue);  
        System.out.println("set.contains(" + searchValue + ") = " + result);  
    }
}
~~~

**실행 결과**
~~~
MyHashSetV3{, buckets=[[], [], [], [], [], [A], [B], [C], [], []], size=3, capacity=10}
set.contains(A) = true
~~~

제네릭 덕분에 타입 안정성이 높은 자료 구조를 만들 수 있었다.
