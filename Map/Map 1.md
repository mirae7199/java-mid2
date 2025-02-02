## 컬렉션 프레임워크 - Map 1

| Key  | Value |     |  Key   | Value  |
| :--: | :---: | --- | :----: | :----: |
| 학생 1 |  90   |     | "APPL" |  "얘플"  |
| 학생 2 |  80   |     | "TSLA" | "톄슬라"  |
| 학생 3 |  80   |     | "CPNG" |  "쿠팡"  |
| 학생 4 |  100  |     | "NVDA" | "엔비디아" |

**Map 자료구조**
- Map의 Key는 중복을 허용하지 않는다.
- Value는 중복을 허용한다.
- Key-Value 값의 쌍을 저장하는 자료구조이다.
- 순서를 유지하지 않는다.

![[Pasted image 20250202114851.png]]
- Map은 Collection을 상속하지 않는다.
- Map의 구현체는 HashMap, LinkedHashMap, TreeMap이 있다.

**예제 코드**
~~~ java
public class MapMain {
	public static main(String[] args) {
		Map<String, Integer> studentMap = new HashMap<>();  
  
		// studentMap에 데이터 추가  
		studentMap.put("studentA", 90);  
		studentMap.put("studentB", 80);  
		studentMap.put("studentC", 80);  
		studentMap.put("studentD", 100);  
  
		System.out.println(studentMap);  
  
		// studentMap 특정 값 조회하기  
		Integer result = studentMap.get("studentA");  
		System.out.println("result = " + result);  
  
		// studentMap.keySet() 활용  
		// 모든 key 값을 set으로 반환  
		// 중복 XSystem.out.println("keySet 활용");  
		Set<String> keySet = studentMap.keySet();  
		for (String key : keySet) {  
		    System.out.println("key = " + key + ", Value = " + studentMap.get(key));  
	}  
  
		// studentMap.entrySet 활용  
		System.out.println("entrySet 활용");  
		Set<Map.Entry<String, Integer>> entries = studentMap.entrySet();  
		for (Map.Entry<String, Integer> entry : entries) {  
		    System.out.println("key = " + entry.getKey() + ", value = " + entry.getValue());  
	}  
  
		// studentMap.values() 활용  
		// 모든 value 값을 collection으로 반환  
		System.out.println("values() 활용");  
		Collection<Integer> values = studentMap.values();  
		for (Integer value : values) {  
		    System.out.println("value = " + value);  
		}
	}
}
~~~

**실행 결과**
~~~
{studentB=80, studentA=90, studentD=100, studentC=80}
result = 90

keySet 활용
key = studentB, Value = 80
key = studentA, Value = 90
key = studentD, Value = 100
key = studentC, Value = 80

entrySet 활용
key = studentB, value = 80
key = studentA, value = 90
key = studentD, value = 100
key = studentC, value = 80

values() 활용
value = 80
value = 90
value = 100
value = 80
~~~

**studentMap.keySet()**
~~~ java
Set<String> keySet = studentMap.keySet();  
for (String key : keySet) {  
	System.out.println("key = " + key + ", Value = " + studentMap.get(key));  
	}   
~~~
- Map의 key 값은 중복을 허용하지 않는다. 따라서 중복을 허용하지 않는 Set으로 반환할 수 있다.
	- key의 모든 목록을 set으로 조회할 수 있다.

**studentMap.values()**
~~~ java
System.out.println("values() 활용");  
Collection<Integer> values = studentMap.values();  
for (Integer value : values) {  
	System.out.println("value = " + value);  
}
~~~
- Map의 value 값은 중복을 허용하고, 순서를 유지하지 않는다. 따라서 Collection으로 반환할 수 있다.
	- Set으로 반환하기에는 중복을 허용해서 불가능
	- List으로 반환하기에는 순서를 유지하지 않아서 불가능
	- 따라서 값의 수집품(모음)이라는 상위 인터페이스 Collection으로 반환
- 모든 값의 목록을 Collection으로 조회할 수 있다.

**studentMap.entrySet()**
~~~ java
System.out.println("entrySet 활용");  
Set<Map.Entry<String, Integer>> entries = studentMap.entrySet();  
for (Map.Entry<String, Integer> entry : entries) {  
	System.out.println("key = " + entry.getKey() + ", value = " + entry.getValue());  
	} 
~~~
- Map은 데이터를 저장할 때 키-값을 쌍으로 묶을 수 있는 방법인 Entry을 사용한다.
	- 하나의 Map에는 여러 Entry가 존재할 수 있음.
	- Entry는 Map 내부에 있는 인터페이스임.

![[Pasted image 20250202123942.png]]
그림을 보면 키-값 쌍으로 묶여있는 Entry를 확인할 수 있다.
 