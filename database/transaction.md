## 트랜잭션(Transaction)
### 트랜잭션이란?
* 데이터베이스의 상태를 변환시키는 하나의 논리적 기능 수행 단위이다.
* 트랜잭션 작업이 정상적으로 끝마치면 `commit`을 해야 데이터베이스에 변경 사항이 반영된다.
* 트랜잭션 작업이 비정상적으로 종료되면 `Atomicity`을 위해 이미 수행된 관련 연산을 모두 취소하고 `rollback` 해야 한다.

### 트랜잭션의 성질
* 트랜잭션의 성질을 흔히 `ACID`라고 표현한다.
  * `ACID`는 `Atomicity(원자성)`, `Consistency(일관성)`, `Isolation(독립성)`, `Durability(영속성)`의 앞글자를 딴 것을 말한다.

### Atomicity(원자성)
* 하나의 트랜잭션이 더 작게 나눌 수 없는 최소의 단위라는 것을 말한다.
* 트랜잭션이 모두 반영되거나, 아니면 전혀 반영되지 않아야 하는 특징을 나타낸다.
  * ex) 계좌이체 시, 송금이 정상적으로 이루어지면 내 계좌의 잔액은 출금이 되어야 한다.

### Consistency(일관성)
* 트랜잭션이 완료된 결괏값이 일관적인 데이터베이스 상태를 유지하는 것을 말한다.
* `모든 고객은 이름이 있어야 한다`라는 제약 조건이 있다면 데이터베이스에는 고객의 이름이 반드시 등록되어야 한다.
  * 이름이 없는 고객 X
  * 기존 고객의 이름 삭제 X

### Isolation(독립성)
* 트랜잭션이 수행되고 있을 때, 다른 트랜잭션의 연산작업이 중간에 끼어들어 기존 작업에 영향을 주지 못하도록 하는 것을 말한다.
* ex) 계좌 이체 작업을 진행하면서 계좌 잔액 조회가 동시에 이루어져서는 안된다.

### Durability(지속성)
* 트랜잭션을 성공한 후 데이터베이스에 반영된 것은 영원히 반영되어야 한다는 것을 의미한다.
* 시스템에 문제가 발생하거나 종료되더라도 데이터베이스에 반영된 값은 그대로 유지되어야 한다.

### 트랜잭션의 상태
* Active(활동 상태)
    * 트랜잭션이 수행을 시작하여 현재 수행 중인 상태
* Partially Committed(부분 완료 상태)
    * 마지막 연산이 실행된 직후의 상태. 연산은 모두 처리했지만, 데이터베이스에 반영되지 않았다.
* Commited(완료 상태)
    * 트랜잭션이 성공적으로 완료되어 `commit`을 한 상태
* Failed(실패 상태)
    * 장애가 발생하여 트랜잭션이 중단된 상태
* Aborted(철회 상태)
    * 수행에 실패하여 `rollback` 연산을 실행한 상태
