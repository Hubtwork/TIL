### Blocking I/O & Non-Blocking I/O

> Block, Non-Block 의 차이는 크게 `호출된 대상` 의 제어가 가능한지 여부에 따라 구분 
>
> **Blocking** - `호출된 대상` 이 제어권을 가져가 작업이 끝날 때까지 대기
>
> **Non-Blocking** - `호출된 대상` 이 작업이 완료되기 전에 제어권을 `호출한 주체` 에게 제어권을 반환함



#### Background

- 프로세스, 스레드 등 실제로 운영체제의 요소들, 작동 원리 등을 익히면서 한번쯤은 정리해볼 필요가 있다고 생각한 부분
- 개발자들이 정확하게 이해가 되지 않은 채 **블로킹** , **논블로킹** 의 용어를 사용하여 커뮤니케이션하고 의미전달이 잘 이루어지지 않는 것을 보고 한번쯤은 정리해보고 싶었음
- 후에 같은 맥락으로 **동기** , **비동기** 에 대해서도 이해하고 살펴볼 것 !



#### Blocking I/O

- `행위의 주체` 가 대기하는 방식

- `호출된 함수` 가 작업을 완료할 때 까지 `호출한 함수` 에게 제어권을 돌려주지 않는 방식 
- **Blocking I/O** - I/O 작업이 진행되는 동안 프로세스가 대기 후 작업이 완료되면 그 결과를 수취하여 작업을 이어감
- **Blocking I/O 서버 Flow**
  - 하나의 클라이언트가 I/O 작업을 진행하는 동안 스레드가 작업을 중지
  - 다수의 사용자가 정상적으로 이용가능하게 하기 위해서 클라이언트 별로 스레드를 생성해 작업을 진행
  - CPU가 컨텍스트 스위칭을 통해 다수의 스레드의 작업을 처리
- **단점**
  - I/O 작업의 경우 CPU 자원의 소모가 거의 없는데 CPU 는 I/O 응답이 올 때까지 대기하므로 **자원 낭비 발생**
  - 클라이언트의 스레드 반환이 작업시간 등 서버측 사유에 의해 길어지거나 동시에 다수의 클라이언트가 접속할 시 **스레드의 수가 비례적으로 증가**
  - 스레드의 증가에 따른 컨텍스트 스위칭 비용에 의해 **비효율적인 자원 운용**



#### Non-Blocking I/O

- `행위의 주체` 가 대기하지 않고 다른 일을 진행하는 방식

- `호출된 함수` 가 작업의 완료여부와 상관없이 바로 `호출한 함수` 에게 제어권을 돌려주는 방식

- **Non-Blocking I/O** - I/O 작업을 진행할 때 유저 프로세스의 중단 없이 결과를 반환해 여러 작업을 병행처리 할 수 있게 함

- **Non-Blocking I/O 서버 Flow**

  - 클라이언트가 I/O 작업을 요청
    - *Polling* ( 요청된 데이터가 준비될 때 까지 )
    - 클라이언트 - I/O 요청
    - 서버 - 에러 ( EWOULDBLOCK ) 반환
  - 서버에 데이터가 준비되면 클라이언트에게 데이터를 반환

  > **Polling** - 지속적으로 데이터가 준비되었는지를 지속적으로 확인. 클라이언트가 서버측에 데이터 준비 여부를 계속 묻는 과정.

- **단점**

  - 데이터가 준비되는데 시간이 오래 걸릴 경우 **Interval 없는 Polling 은 불필요한 CPU 점유**를 야기
  - 발생할 수 있는 다양한 에러 대응 등 **복잡한 설계 및 구현**이 수반됨



### REFERENCE

- https://saimulticorecomputing.wordpress.com/2014/06/25/blocking-and-non-blocking-function-calls/
- https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/



