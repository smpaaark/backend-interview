## Java
* [Call by value와 Call by reference](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

[뒤로](https://github.com/smpark1020/backend-interview#back-end-interview)

## Call by value와 Call by reference
### Call by value
* 값에 의한 호출
* 메서드 호출 시에 사용한 인자와 메서드 내의 매개변수는 서로 다릅니다.
* 메서드 호출 시에 사용한 인자는 할당된 메모리 주소가 아닌 메모리에 담겨져 있던 값만이 복사되어 메서드 내부 매개변수의 메모리 주소에 담겨지게 됩니다.
* 메서드 내에서 값을 변경하더라도 호출 시에 사용한 인자는 변하지 않습니다.
* 요점
```
Call by value는 메서드 호출 시에 사용되는 인자의 메모리에 저장되어 있는 값(value)을 복사하여 보냅니다.
```

### Call by reference
* 참조에 의한 호출
* 메서드 호출 시에 사용되는 인자가 값이 아닌 주소(Address)를 넘겨줌으로써, 주소를 참조(Reference)하여 데이터를 변경할 수 있습니다.
* 메서드 호출 시에 인자는 메모리에 저장된 주소 값을 복사하여 매개변수의 메모리에 저장합니다.
* 결국 메서드는 호출 시 인자의 주소를 참조하여 연산하기 때문에, 연산 결과에 따라 원본 데이터가 변하게 됩니다.
* 요점
```
Call by reference는 메서드 호출 시 사용되는 인자 값의 메모리에 저장되어있는 주소(Address)를 복사하여 보낸다.
```

### 참조
[[Java] Call by value와 Call by reference](https://re-build.tistory.com/3)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)
