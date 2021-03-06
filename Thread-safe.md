## Thread-safe

> 멀티 Thread 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 Thread로부터 동시 접근이 일어나도 프로그램의 실행에 문제가 없음을 뜻함
>
> 하나의 함수가 한 Thread로부터 호출되어 실행중일 때, 다른 스레드가 그 함수를 호출하여 동시에 실행되더라도 각 Thread에서의 함수 수행 결과가 올바르게 나오는 것

- Re-entrancy(재진입성)
  - 동시 접근 하더라도 같은 결과
- TLS(Thread-local storage)
  - 공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근 방지
  - 각 스레드마다 고유한 저장공간 소유
- Mutual exclusion(상호 배제)
  - 공유 자원을 사용할 경우 해당 자원의 접근을 세마포어 등의 락으로 통제
  - 공유 불가능한 자원의 동시 사용 금지
- Atomic operations
  - 공유 자원에 접근 시 원자 연산 or '원자적'으로 정의된 접근 방법을 사용함으로써 상호 배제 구현
    - 원자성의 보증
      - 모두 성공 or 실패함으로써 DB의 부분적인 갱신으로 더 큰 문제를 방지