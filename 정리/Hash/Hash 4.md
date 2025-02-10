## 해시 알고리즘 5 - 해시 충돌
9, 99 두 수를 10으로 나누면 9가 된다. 다른 값을 입력했지만 같은 해시 인덱스가 나오는 것을 해시 충돌 이라고 한다.
- 9 % 10 = 9
- 99 % 10 = 9

![[Pasted image 20250116115511.png]]
- 99의 해시 인덱스 9에 저장하고, 9의 해시 인덱스 9에 저장하게 된다.
- 이것이 해시 충돌이다.
- 값은 마지막에 저장한 9가 해시 인덱스 9에 저장하게 된다.

**해시 충돌 해결**
해시 충돌을 인정하면 모든게 해결된다. 그냥 단순하게 같은 배열에 여러 값을 넣을 수 있는 자료구조를 넣고 같은 해시 인덱스에 값을 저장하는 거다. 예를 들면 List\<Integer>\[]  배열에 List\<Integer>만 들어올 수 있게 만들면 된다. 한마디로 배열안에 배열을 만들면 되는 것이다!

![[Pasted image 20250116120206.png]]

**해시 충돌 조회**
- 위 사진과 같이 9번 인덱스에 99, 9 값이 둘다 존재한다.
- 9번 인덱스안에 배열을 찾아서 for문(iter)과 if문을 돌려 조회하려는 값과 비교하면 된다.

**최악의 경우**
![[Pasted image 20250116120551.png]]
- 9번 인덱스안에 배열에 많은 값이 들어가게 된다면?
- 조회 할때 모든 값과 비교해야한다. 따라서 느린 성능 O(n)이 나올 것이다.

**정리**
해시 인덱스를 사용하는 방식은 최악의 경우 O(n)의 성능을 보인다. 하지만 확률적으로 보면 어느 정도 넓게 퍼지기 때문에 평균적으로 대부분 O(1)의 성능을 제공한다. 해시 충돌이 가끔 발생하더라도, 내부에서 값을 몇 번만 비교하는 수준이기 때문에 대부분 빠르게 찾을 수 있다.

## 해시 알고리즘 6 - 해시 충돌 구현
해시 충돌 상황까지 고려해서 코드 구현하기

~~~ java
public class HashStart6 {  
  
    static final int CAPACITY = 10;  
  
    public static void main(String[] args) {  
        //{1, 2, 5, 8, 14, 99 ,9}  
        LinkedList<Integer>[] buckets = new LinkedList[CAPACITY];  
        for (int i = 0; i < CAPACITY; i++) {  
            buckets[i] = new LinkedList<>();  
        }  
        add(buckets, 1);  
        add(buckets, 2);  
        add(buckets, 5);  
        add(buckets, 8);  
        add(buckets, 14);  
        add(buckets, 99);  
        add(buckets, 9); //중복  
  
        System.out.println("buckets = " + Arrays.toString(buckets));  
  
        // 검색  
        int searchValue = 9;  
        boolean contains = contains(buckets, searchValue);  
        System.out.println("buckets.contains(" + searchValue + ") = " + contains);  
    }  
  
    private static void add(LinkedList<Integer>[] buckets, int value) {  
        int hashIndex = hashIndex(value);
          
        // buckets[hashIndex]를 bucket에 할당  
        LinkedList<Integer> bucket = buckets[hashIndex]; 
        
        
        if(!bucket.contains(value)){  
            bucket.add(value);  
        }  
    }
      
    private static boolean contains(LinkedList<Integer>[] buckets, int searchValue) {  
        int hashIndex = hashIndex(searchValue);  
        LinkedList<Integer> bucket = buckets[hashIndex]; // O(1)  
        return bucket.contains(searchValue); // O(n)  
    }  
  
     private static int hashIndex(int value) {  
        return value % CAPACITY;  
    }  
}
~~~

**실행**
~~~
buckets = [[], [1], [2], [], [14], [5], [], [], [8], [99, 9]]
buckets.contains(9) = true
~~~

![[Pasted image 20250116124049.png]]

**배열 선언**
> LinkedList\<Integer>\[] buckets = new LinkedList\[CAPACITY]

해시 충돌을 고려해서 배열 안에 배열이 들어가야 한다. 배열 대신에 여기서는 편리하게 사용할 수 있는 연결 리스트를 사용하였다. LinkedList는 하나의 바구니이다. 바구니를 여러개 모아서 하나의 배열을 선언했다. 쉽게 말하면 배열안에 바구니 여러개가 있고 바구니 안에 여러개의 데이터가 있는 것이다.

**데이터 등록**
~~~ java
private static void add(LinkedList<Integer>[] buckets, int value) {  
    int hashIndex = hashIndex(value);  

	// buckets[hashIndex]를 bucket에 할당  
    LinkedList<Integer> bucket = buckets[hashIndex]; 
    
    if(!bucket.contains(value)){  
        bucket.add(value);  
    }
~~~
add 메서드
- hashIndex 메서드로 입력할 값의 해시 인덱스를 구한다.
- 해시 인덱스로 배열의 인덱스를 찾는다. 배열 안에는 연결 리스트가 있다.
- 값을 저장하기 전에 중복된 데이터가 있는지 확인한다.
	- contains 메서드를 사용해서 중복 데이터 확인
	- 모든 값을 순회하기 때문에 O(n)이다.
	- 단, 값이 하나인 경우는 O(1)

**데이터 검색**
~~~ java
private static boolean contains(LinkedList<Integer>[] buckets, int searchValue) {  
    int hashIndex = hashIndex(searchValue);  
    LinkedList<Integer> bucket = buckets[hashIndex]; // O(1)  
    return bucket.contains(searchValue); // O(n)  
}
~~~
- 해시 인덱스로 배열의 인덱스를 찾는다. 여기에는 연결 리스트가 들어있다.
- 연결 리스트의 contains 메서드를 활용해서 중복 데이터가 있는지 확인한다. -> O(n)

**해시 인덱스 충돌 확률**
해시 충돌이 발생하면 데이터를 추가하거나 조회할 때, 연결 리스트 내부에서 O(n)의 추가 연산을 해야 하므로 성능이 느려진다. 따라서 해시 충돌이 발생하지 않도록 해야 한다.
해시 충돌은 입력하는 데이터의 수와 비교해서 배열의 크기가 클 수록 충돌 확률은 낮아진다.

![[Pasted image 20250116125541.png]]
- **CAPACITY = 1**: 배열의 크기가 하나밖에 없을 때는 모든 해시가 충돌한다.
- **CAPACITY = 5**: 배열의 크기가 입력하는 데이터 수 보다 작은 경우 해시 충돌이 자주 발생한다.
- **CAPACITY = 10**: 저장할 데이터가 7개 인데, 배열의 크기는 10이다. **7/10 약 70% 정도**로 약간 여유있게 데이터가 저장된다. 이 경우 가끔 충돌이 발생한다.
- **CAPACITY = 11**: 저장할 데이터가 7개 인데, 배열의 크기는 11이다. 가끔 충돌이 발생한다. 여기서는 충돌이 발생하지 않았다.
- **CAPACITY = 15**: 저장할 데이터가 7개 인데, 배열의 크기는 15이다. 가끔 충돌이 발생한다. 여기서는 충돌이 하나 발생했다.

상황에 따라 다르겠지만 보통 75%를 적절한 크기로 보고 기준으로 잡는 것이 효과적이다.

**정리**
해시 인덱스를 사용하는 경우
- 데이터 저장
	- 평균: O(1)
	- 최악: O(n)
- 데이터 조회
	- 평균: O(1)
	- 최악: O(n)

해시 인덱스를 사용하는 방식은 사실 최악의 경우는 거의 발생하지 않는다. 배열의 크기만 적절하게 잡아주면 대부분 O(1)에 가까운 매우 빠른 성능을 보여준다.



