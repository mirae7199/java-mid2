## 해시 알고리즘 3 - 메모리 낭비
입력 값의 범위를 0~99로 넓혀보자.

**문제**
- 입력: 0~99 사이의 여러 값이 입력된다. 중복된 값은 입력되지 않는다.
- 찾기: 0~99 사이의 값이 하나 입력된다. 입력된 값 중에 찾는 값이 있는지 확인하기.

검색 속도를 높이기 위해서 앞에서 사용한 데이터의 값을 배열의 인덱스로 사용하자.
~~~ java
public class HashStart3 {  
    public static void main(String[] args) {  
        Integer[] inputArray = new Integer[100];  
        inputArray[1] = 1;  
        inputArray[2] = 2;  
        inputArray[5] = 5;  
        inputArray[8] = 8;  
        inputArray[14] = 14;  
        inputArray[99] = 99;  
        System.out.println("inputArray = " + Arrays.toString(inputArray));  
  
        int searchValue = 99;  
        Integer result = inputArray[searchValue]; // O(1)  
        System.out.println("result = " + result);  
    }
}
~~~

**실행 결과**
~~~
inputArray = [null, 1, 2, null, null, 5, null, null, 8, null, null, null, null, null, 14, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 99]
result = 99
~~~

**한계**
데이터의 값을 인덱스로 사용한 덕분에 성능은 O(1)로 빠르게 나온다. 하지만 비어있는 공간이 너무나 많다. 

- **int 범위 입력**
	- int 범위: -2,147,483,648 ~ 2,147,483,647
	- 약 42억 사이즈의 배열 필요
	- 4byte \* 42억 = 약 17기가바이트 필요

입력할 수 있는 범위가 int라면 최신 컴퓨터들의 메모리가 거의 소모되어 버린다. 만약 1, 10, 9000의 데이터를 입력한다고 하면 그 사이에 낭비되는 공간은 어마어마 할 것이다. 또한 배열을 생성하는데도 시간이 오래 걸릴 것이다. 데이터의 값을 인덱스로 사용하기에는 어려워 보인다.

## 해시 알고리즘 4 - 나머지 연산
앞에서 말한 데이터의 값을 인덱스으로 쓰는 것은 범위가 더 넓어지면 인덱스로 사용하기에는 어려울 것이다. 0~99의 입력 범위에서도 1, 2, 5, 8, 99 사이에 많은 비어있는 공간이 있어 메모리 낭비가 심하다. 

메모리를 효율적으로 쓰면서 넓은 범위의 값을 사용할 수 있는 방법이 있는데 바로 나머지 연산을 사용하는 것이다.  저장할 수 있는 용량(CAPACITY)를 10이라고 하고 입력값을 용량으로 나머지 연산을 한다.

**나머지 연산**
- 1 % 10 = 1
- 2 % 10 = 2
- 5 % 10 = 5
- 8 % 10 = 8
- 14 % 10 = 4
- 99 % 10 = 9

용량이 10이라고 가정하고 나머지 연산을 사용하면 값은 0~9 숫자만 나올 것이다. 용량보다 큰 숫자는 나올 수가 없다. 따라서 연산 결과는 절대 배열의 크기를 넘어가지 않으므로 안전하게 인덱스로 사용할 수 있다.

**해시 인덱스**
이렇게 배열의 인덱스를 사용할 수 있도록 원래의 값을 계산한 인덱스를 해시 인덱스(hashIndex)라고 한다. 14의 해시 인덱스는 4이다, 99의 해시 인덱스는 9이다.

**해시 인덱스를 인덱스로 사용해보기**
![[Pasted image 20250116110927.png]]

**코드로 구현해보기**
~~~ java
public class HashStart4 {  
  
    static final int CAPACITY = 10;  
  
    public static void main(String[] args) {  
        System.out.println("hashIndex(1) = " + hashIndex(1));  
        System.out.println("hashIndex(2) = " + hashIndex(2));  
        System.out.println("hashIndex(5) = " + hashIndex(5));  
        System.out.println("hashIndex(8) = " + hashIndex(8));  
        System.out.println("hashIndex(14) = " + hashIndex(14));  
        System.out.println("hashIndex(99) = " + hashIndex(99));  
  
        Integer[] inputArray = new Integer[CAPACITY];  
        add(inputArray, 1);  
        add(inputArray, 2);  
        add(inputArray, 5);  
        add(inputArray, 8);  
        add(inputArray, 14);  
        add(inputArray, 99);  
  
        System.out.println("inputArray = " + Arrays.toString(inputArray));  
  
        // 검색  
        int searchValue = 99;  
        int hashIndex = hashIndex(99);  
        Integer result = inputArray[hashIndex]; // O(1)  
        System.out.println("result = " + result);  
    }  
    
    static int hashIndex(int value) {  
        return value % CAPACITY;  
    }  
    
    static void add(Object[] array, int value) {  
        int hashIndex = hashIndex(value);  
        array[hashIndex] = value;  
    }
}
~~~

**실행**
~~~
hashIndex(1) = 1
hashIndex(2) = 2
hashIndex(5) = 5
hashIndex(8) = 8
hashIndex(14) = 4
hashIndex(99) = 9
inputArray = [null, 1, 2, null, 14, 5, null, null, 8, 99]
result = 99
~~~

hashIndex 메서드
- 입력된 값을 배열의 용량으로 나머지 연산(%)한다.
- 배열의 인덱스로 사용할 수 있도록 계산하는 것

add 메서드
- 입력된 값으로 hashIndex를 구해서 배열의 인덱스로 사용, 데이터 저장

조회
- 해시 인덱스를 구하고, 배열에 해시 인덱스를 대입해서 값을 구한다.
- inputArray\[hashIndex]

정리
- 입력 값의 범위가 넓어져도 실제 모든 값이 들어오지는 않으므로 배열의 크기를 제한하고 나머지 연산을 통해서 메모리가 낭비되는 문제도 해결했다.
- 해시 인덱스로 배열의 인덱스를 저장해서 O(1)의 성능으로 데이터를 저장하고, 해시 인덱스를 구해서 배열의 인덱스를 구하기 때문에 O(1)의 성능으로 조회할 수 있게 되었다.

한계 - 해시 충돌
- 지금까지 예는 해시 충돌을 피해서 예를 들었다. 하지만 만약 해시 인덱스가 같더라면?
- 1, 11의 두 값을 10으로 나머지 연산을 하면 1의 나머지가 나온다. 둘다 같은 해시 인덱스가 나오는 것이다.

