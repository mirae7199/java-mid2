## 직접 구현한 리스트의 성능 비교

**실행 결과**
![[Pasted image 20250110200857.png]]
다음은 MyArrayList와 MyLinkedList의 성능을 비교하기 위해 데이터를 앞에 추가, 중간에 추가, 뒤에 추가 한 것이다.

**직접 만든 배열 리스트와 연결 리스트 - 성능 비교 표**

| 기능        | 배열 리스트          | 연결 리스트          |
| --------- | --------------- | --------------- |
| 앞에 추가(삭제) | O(n) - 1369ms   | O(1) - 2ms      |
| 평균 추가(삭제) | O(n) - 651ms    | O(n) - 1112ms   |
| 뒤에 추가(삭제) | O(1) - 2ms      | O(n) - 2196ms   |
| 인덱스 조회    | O(1) - 1ms      | O(n) - 평균 438ms |
| 검색        | O(n) - 평균 115ms | O(n) - 평균 492ms |

정리하면 이론적으로는 연결 리스트가 더 효율적일 수 있지만, 현대 컴퓨터 시스템의 메모리 접근 패턴, CPU 캐시 최적화 등을 고려할 때 MyArrayList가 실제 사용 환경에서 더 나은 성능을 보여주는 경우가 많다.

**배열 리스트 VS 연결 리스트**
대부분의 경우 배열 리스트가 성능상 유리하다. 이런 이유로 실무에서는 주로 배열 리스트를 기본으로 사용한다. 만약 데이터를 앞쪽에서 자주 추가하거나 삭제할 일이 있다면 연결 리스트를 고려하자.