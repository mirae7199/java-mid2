## 리스트(List) VS 세트(Set)

**List**
![[Pasted image 20250114191313.png]]
리스트의 요소는 순서가 있고, 같은 요소가 들어올 수 있다.
특징
- 리스트에 추가된 요소는 특정한 순서가 정해져 있다.
- 리스트는 동일한 값이나 객체의 중복을 허용한다.
- 리스트의 각 요소는 인덱스를 통해 접근할 수 있다.

용도
- 순서가 중요하거나 중복된 요소를 허용해야 하는 경우에 주로 사용됨.

**Set**
![[Pasted image 20250114191650.png]]
요소의 순서가 정해져 있지 않고 같은 요소가 들어올 수 없다.
특징
- 세트에 추가된 요소는 순서가 정해져 있지 않다.
- 세트에는 중복된 요소가 존재하지 않는다. 세트에 요소를 추가할 때 이미 존재한다면 무시함.
- 세트의 요소는 유무를 빠르게 확인할 수 있도록 최적화 되어 있다. 이는 데이터의 중복을 방지하고 빠른 조회를 가능하게 한다.

용도
- 중복을 허용하지 않고, 요소의 유무만 중요한 경우에 사용된다.


예시
- List: 장바구니 목록, 순서가 중요한 일련의 이벤트 목록
- Set: 회원 ID 집합, 고유한 항목의 집합

### 직접 구현하는 Set v0.1
- add(value): 세트에 값을 추가한다. 중복 데이터는 저장하지 않는다.
- contains(value): 세트에 값이 있는지 체크한다.
- remove(value): 세트에 있는 값을 제거한다.

**Set 직접 구현하기**
~~~ java
public class MyHashSetV0 {  
    private final int[] elementData = new int[10];  
    private int size = 0;  
  
    // O(n)  
    public boolean add(int value) {  
        if(contains(value)){  
            return false;  
        }        
        elementData[size] = value;  
        size++;  
        return true;  
    }  
  
    // O(n)    
    public boolean contains(int value) {  
        for (int data : elementData) {  
            if(data == value) {  
	            return true;  
            }        
        }       
		return false;  
    }
      
    @Override  
    public String toString() {  
        return "MyHashSetV0{" +  
                "element=" + Arrays.toString(Arrays.copyOf(elementData, size)) + ", size=" + size + '}';  
                
    }
}
~~~
- boolean add(int value): 추가하려는 요소(value)를 중복 체크한 후에 추가한다. 추가 성공이면 true 실패면 false를 반환
- boolean contains(int value): 배열에 같은 요소가 있는지 체크해준다. 중복이 있으면 true 없으면 false 반환

~~~ java
public class MyHashSetV0Main {  
    public static void main(String[] args) {  
        MyHashSetV0 set = new MyHashSetV0();  
        set.add(1); // O(1)  
        set.add(2); // O(n)  
        set.add(3); // O(n)  
        set.add(4); // O(n)  
        System.out.println(set);  
  
        boolean result = set.add(4);// 중복 저장  
        System.out.println("중복 데이터 저장 결과 = " + result);  
        System.out.println(set);  

		// O(n)  
        System.out.println("set.contains(3) = " + set.contains(3)); 
        // O(n)  
        System.out.println("set.contains(99) = " + set.contains(99));
  
    }  
}
~~~

**실행 결과**
~~~
MyHashSetV0{element=[1, 2, 3, 4], size=4}
중복 데이터 저장 결과 = false
MyHashSetV0{element=[1, 2, 3, 4], size=4}
set.contains(3) = true
set.contains(99) = false
~~~
- add() 메서드는 중복 데이터를 검색하기 때문에 O(n)으로 입력 성능이 나쁘다.
	- 중복 데이터 검색 O(n) + 데이터 입력 O(1) -> O(n)
- contains() 메서드로 배열의 있는 모든 데이터를 검색하기 때문에 평균 O(n)이 걸린다.

#### 정리
우리가 만든 Set 자료구조는 데이터 추가, 검색 모두 O(n)으로 성능이 좋지 않다. 특히나 데이터가 많으면 많을 수 록 효율은 나빠진다. 데이터를 추가할 때마다 이렇게 성능이 느린 자료 구조는 사용하기 어렵다.



 