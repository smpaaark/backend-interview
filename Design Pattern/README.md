## Design Pattern
* [Adapter Pattern]()

[뒤로](https://github.com/smpark1020/backend-interview#back-end-interview)

## Adapter Pattern
호출당하는 메소드를 호출하는 쪽의 코드에 대응되도록 중간에 adapter 변환기를 통해 호출하는 패턴
* 객체를 Service의 속성으로 만들어서 참조합니다.

## Proxy Pattern
* 제어 흐름을 조정하기 위해 중간에 대리자를 두는 패턴
* "자동차 - 타이어" 관계
  * 실제 서비스 전후에 별도 로직을 실행할 수 있습니다.

## Decorator Pattern
메소드 호출의 반환값에 변화를 주기 위해 중간에 decorator를 두는 패턴
* 구현 방법은 Proxy와 같으나, 단 Client가 최종적으로 돌려받는 반환값에 장식을 덧입힙니다.