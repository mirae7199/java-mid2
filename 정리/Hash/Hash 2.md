## 해시 알고리즘 1 - 시작
해시(Hash) 알고리즘을 사용하면 데이터를 찾는 검색 성능을 평균 O(1)로 비약적으로 끌어올릴 수 있다.

**문제**
- 입력: 0~9 사이의 여러 값이 입력된다. 중복된 값은 입력되지 않는다.
- 찾기: 0~9 사이의 값이 하나 입력된다. 입력된 값 중에 찾는 값이 있는지 확인하기.

~~~ java
public class HashStart1 {  
    public static void main(String[] args) {  
        Integer[] inputArray = new Integer[4];  
        inputArray[0] = 1;  
        inputArray[1] = 2;  
        inputArray[2] = 5;  
        inputArray[3] = 8;  
        System.out.println("inputArray = " + Arrays.toString(inputArray));  
  
        int searchValue = 8;  
        // 4번 반복 O(n)        
        for (Integer inputValue : inputArray) {  
            if(inputValue == searchValue){  
                System.out.println(inputValue);  
            }        
        }    
    }
}
~~~

**실행 결과**
~~~
inputArray = [1, 2, 5, 8]
8
~~~

검색 데이터는 8이다. 배열에 8이라는 요소를 찾으려면 모든 배열의 요소를 검색한 후에 비교해야 한다.
따라서 성능이 O(n)으로 매우 느리다. 여기서 핵심은 찾기 성능이 O(n)으로 느리다는 점이다.

## 해시 알고리즘 2 - index 사용
배열은 index를 활용해서 데이터를 찾을 때 성능이 O(1)로 빠르다. 반면에 데이터를 검색할 때는 배열을 처음부터 끝까지 하나하나 비교를 해야 하기 때문에 index를 사용할 수 없어 O(n)으로 느리다. 만약에 데이터를 검색할 때도 이 index를 활용할 수 있다면 O(n) -> O(1)로 성능이 빨라질 수 있지 않을까?

데이터의 값을 인덱스에 맞추어 사용해보자.
![[Pasted image 20250115120506.png]]
- 인덱스 1  -> 데이터 1
- 인덱스 2 -> 데이터 2
- 인덱스 5 -> 데이터 5
- 인덱스 8 -> 데이터 8

저장할 데이터와 인덱스를 맞추어 넣었다. 이제 인덱스 번호는 데이터와 같다. 

**실제 코드 구현**
~~~ java
public class HashStart2 {  
    public static void main(String[] args) {  
        Integer[] inputArray = new Integer[10];  
        inputArray[1] = 1;  
        inputArray[2] = 2;  
        inputArray[5] = 5;  
        inputArray[8] = 8;  
        System.out.println("inputArray = " + Arrays.toString(inputArray));  
		int searchValue = 8;  
		Integer result = inputArray[searchValue]; // O(1)
		System.out.println("result = " + result); 
     }  
}
~~~

**실행 결과**
~~~
inputArray = [null, 1, 2, null, null, 5, null, null, 8, null]
searchValue = 8
~~~

데이터를 추가하는 순서대로 넣은 것이 아니라 데이터를 값을 index로 사용해서 저장했다.

데이터를 검색할 때 for문(iter)를 돌려 모든 배열의 요소를 비교하는 것이 아니라 데이터 값을 index에 넣어 검색하였다.
~~~ java
int searchValue = 8;
Integer result = inputArray[searchValue];
~~~
![[Pasted image 20250115121428.png]]
배열에서 인덱스로 조회하는 것은 O(1)로 매우 빠르다.

**정리**
데이터 값을 인덱스로 저장하여 데이터를 검색할 때 매우 빠른 성능(O(1))을 보여주었다.

**문제**
입력 값의 범위 만큼 큰 배열을 사용해야 하는데 낭비되는 공간이 너무 많다.

