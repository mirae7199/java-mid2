## 컬렉션 프레임워크 - Map 2
Map에 같은 key를 저장할 시 데이터는 어떻게 될 것인가?

**예제 코드**
~~~ java
public class MapMain2 {  
    public static void main(String[] args) {  
        Map<String, Integer> studentMap = new HashMap<>();  
  
        // 학생 성적 데이터 추가  
        studentMap.put("studentA", 90);  
        System.out.println(studentMap);  
  
        // 같은 키에 저장 시 기존 값을 교체한다.  
        studentMap.put("studentA", 100);  
        System.out.println(studentMap);  

	// studentA key가 있는지 확인
        boolean containsKey = studentMap.containsKey("studentA");  
        System.out.println("containsKey = " + containsKey);  
  
        // 특정 학생의 값 삭제  
        studentMap.remove("studentA");  
        System.out.println(studentMap);  
    }
}
~~~

**실행 결과**
~~~
{studentA=90}
{studentA=100}
containsKey = true
{}
~~~
같은 key로 저장할 시 기존 값을 새로 추가한 값으로 교체한다. 실행 결과를 보면 {studentA=90}에서 {studentA=100}으로 값이 교체된 것을 확인할 수 있다.

그렇다면, key가 없는 경우에만 저장할려면 어떻게 해야할까?

**예제 코드**
~~~ java
public class MapMain3 {  
    public static void main(String[] args) {  
        Map<String, Integer> studentMap = new HashMap<>();  
  
        // 학생 성적 데이터 추가  
        studentMap.put("studentA", 50);  
        System.out.println(studentMap);  
  
        // 학생이 없는 경우에만 추가  
        if(!studentMap.containsKey("studentA")) {  
            studentMap.put("studnetA", 100);  
        }        
        System.out.println(studentMap);  
  
	// 학생이 없는 경우에만 추가 2     
	studentMap.putIfAbsent("studentA", 100);  
        studentMap.putIfAbsent("studnetB", 100);  
        System.out.println(studentMap);  
  
    }
}
~~~

**실행 결과**
~~~
{studentA=50}
{studentA=50}
{studnetB=100, studentA=50}
~~~

Map에 key가 없을 때 저장하는 방법은 2가지가 있다.
- if(!studentMap.containsKey("studentA")
	- containsKey()는 map에 특정 key의 존재 유무를 확인해준다.
	- 이건 너무 귀찮다..
- studentMap.putIfAbsent("studentA", 100);
	- putIfAbsent()는 map에 특정 key가 없는 경우에만 저장하는 것이다.
	- 이 메서드를 사용하면 if문을 쓰고 containsKey 메서드를 쓰는 방법보다 코드 한줄로 편리하게 저장할 수 있다.
