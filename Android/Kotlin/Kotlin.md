# Kotlin
> 코틀린은 자바 플랫폼에서 돌아가는 새로운 프로그래밍 언어이다.  
> 기존 자바 라이브러리나 프레임워크와 함께 잘 작동하며, 성능도 자바와 같은 수준이다.  
> 안드로이드, 브라우저, 네이티브 앱 개발 등 범용성 많다.  

## Kotlin 특징
> 타입 추론을 지원하는 정적 타입 지정 언어  
> 정확성과 성능을 보장하면서 소스코드를 간결하게 작성하고 유지  

자바의 경우 타입을 일일히 명시해줘야 하고, 정적 타입 지정 언어의 단점 중 하나
코틀린은 타입 추론을 지원하기 때문에, 정확성과 성능을 보장하면서 소스코드를 간결하게 작성하고 유지

> 객체지향, 함수형 프로그래밍 스타일 모두 지원  
> 일급시민함수를 사용하여 수준 높은 추상화  
> 불변 값 지원을 통해 위험한 자원 공유를 막아 다중 스레드 앱 개발을 더욱 편리하고 쉽게 가능  

함수형 프로그래밍 ⇒ 제한 없이 어디에서나 작성
객체지향 프로그래밍 ⇒ 클래스 내부에 있는 함수에만 로직을 작성

**함수형 프로그래밍 핵심 개념**
* 일급시민함수
함수를 일반 값처럼 변수에 저장할 수 있고, 다른 함수의 인자로 넘길 수 있음
* 불변성
생성 이후 절대로 내부 상태가 바뀌지 않는 불변 객체를 사용하여 프로그램 작성
* 사이드 이펙트 X
입력이 같으면 항상 같은 출력을 보장하며, 함수 외부 다른 객체를 건드리지 않음

> 실용성, 간결성, 안전성, 상호운용성이 좋음  
> NPE 등의 오류를 방지  
> 읽기 쉽고 간결한 코드를 지원, 자바와 호환 문제 없이 사용 가능  

- - - -
참고사이트
안드로이드 공식사이트 : https://developer.android.com/kotlin/learn
해로 velog : https://velog.io/@haero_kim/%EC%9A%B0%EB%A6%AC-%EA%B0%99%EC%9D%B4-...-%EC%BD%94%ED%8B%80%EB%A6%B0-%ED%95%A0%EA%B9%8C%EC%9A%94
블로그 : https://smilek1225.tistory.com/7
