1. Kotlin : What And Why?
챕터1은 아래와 같은 내용에 대해서 설명합니다.
- Kotlin의 기본적인 특징
- Kotlin 언어의 주요 특성
- 안드로이드와 서버사이드에서의 사용 가능성
- 다른 언어와 코틀린의 구별점
- 코틀린 코드를 작성하고 실행하는 방법

1.1. A taste of Kotlin
위와 같은 간단한 코드로 코틀린의 특성을 알 수 있습니다.
위 코드는 Person 클래스의 리스트에서, 가장 나이가 많은 Person 객체를 찾아 출력하는 예제입니다.

이 예제에서는 아래의 기능을 포함하고 있으며, 간단한 코드로 수많은 반복코드가 제거되었음을 알 수 있습니다.
분명 클래스에는 내부에 어떤 정의도 하지않고, 한줄로 클래스를 정의했지만 자바와 달리 많은 기능을 제공하고 있습니다.

- data 클래스
- Nullable type
- Top-level function
- Named argument
- String template
- Lambda expression
- Elvis operator
- toString 자동 생성
자바로 작성했으면, 엄청나게 많은 코드가 필요했을테지만 코틀린은 간단하게 구현이 가능합니다.

1.2. Kotlin’s primary traits
1.2.1. Target platforms : server-side, Android, anywhere Java runs
- 코틀린은 “자바가 현재 사용되고 있는 모든 곳”에서 더 간결하고 생산적이며 안전한 대체 언어를 제공하는 것이 목표입니다.
- 주로 서버 혹은 안드로이드 개발에서 사용됩니다.

1.2.2. Statically typed
- 자바와 같은 정적 타입의 언어이다. 정적타입이기 때문에, 코틀린 프로그램은 컴파일되는 시점에서 모든 표현식이 어떤 타입인지 알 수 있으며, 컴파일러가 검증이 가능합니다.
- 하지만, 자바와 달리 프로그래머가 모든 타입을 직접적으로 명시할 필요는 없습니다. 컴파일러가 문맥을 통해 변수의 타입을 추론할 수 있습니다.
- 코틀린의 이러한 타입 추론은 대부분의 정적 타입 언어에서의 타입 선언을 생략할 수 있게하여, 불편함을 줄여줍니다.
- nullable type의 지원을 통해 컴파일 시점에서 NPE 검사가 가능합니다.

1.2.3. Functional and Object-oriented
- 코틀린은 함수형 프로그래밍과 객체지향 패러다임을 지원합니다.
- 함수형 프로그래밍의 개념
 - 일급 함수
 - 일급함수는 함수를 값과 같이 변수에 저장하거나 다른 함수에 콜백과 같이 전달이 가능합니다.
 - 불변성
 - 내부 상태가 절대로 변경되지 않는 불변 객체를 사용합니다.
 - 사이드 이펙트가 없는 순수함수
 - 함수형 프로그래밍에서의 함수들은 같은 입력에는 항상 같은 출력을 보장해야합니다.
 - 다른 객체의 상태를 변경시키지 않습니다.
 - 함수의 외부와 상호작용하지 않습니다.
- 함수형 프로그래밍의 장점
 - 간결성
 - 순수 함수는 값처럼 활용이 가능하여, 추상화에 강력하게 사용할 수 잇습니다.
 - 코드 중복을 줄이고, 람다를 통해 더 간결한 표현이 가능해집니다.
 - 멀티쓰레드 안전
 - 멀티 쓰레드에서는 보통 동일한 데이터가 동시에 여러곳에서 수정될 때 문제가 생깁니다.
 - 불변성에 의해 멀티쓰레드에서의 안전성을 보장합니다.
 - 테스트 용이
 - 순수함수는 사이드 이펙트가 없어 테스트에 용이합니다.
- 코틀린에서 지원하는 함수형 프로그래밍
 - 함수 타입
 - 함수가 다른 함수의 파라미터로 전달 될 수 있도록, 함수 타입을 사용할 수 있습니다.
 - 람다
 - 아래와 같이, 간결한 함수를 정의할 수 있는 람다식을 제공합니다.
fun findAlice() = findPerson { it.name == "Alice" } 
fun findBob() = findPerson { it.name == "Bob" }
 - Data 클래스
 - 불변 객체를 간편하게 생성할 수 있는 Data Class 문법을 제공합니다.
 - API
 - 객체, 컬렉션을 함수형으로 다룰 수 있는 여러 API를 제공합니다.

1.2.4. Free and Open source
- 코틀린은 언어 자체와 컴파일러 라이브러리 등 모든 관련되어 있는 툴이 무료이며 오픈소스입니다.

1.3. Kotlin Applications
1.3.1. Kotlin on the server side
- 자바 클래스를 코틀린으로 확장해도 문제가 생기지 않고, 자바 어노테이션등을 코틀린과 함께 사용해도 문제를 발생시키지 않습니다.
- 그렇기 때문에, 스프링에도 당장 사용이 가능합니다.
 - 하지만, jpa + hibernate와의 궁합은 좋지 않은 것 같습니다. ( https://techblog.woowahan.com/2675/ , https://blog.junu.dev/37)


1.3.2. Kotlin on Android
- 코틀린은 안드로이드 공식언어로 채택 되었습니다.
- 자바로 개발되던 안드로이드는 이제 코틀린으로 개발이 가능해졌으며, 위에서 설명한 여러 장점을 통해 더 간결한 코드 작성이 가능해졌습니다.

1.4. The philosophy of Kotlin
1.4.1. Pragmatic
- 코틀린은 실제적인 문제를 해결하기 위해 만들어진 실용적인 언어입니다.
- 어떤 프로그래밍 스타일이나 패러다임을 사용하는 것을 강제하지 않습니다.
- 코틀린 컴파일러는 IntelliJ IDEA와 개발이 계속 맞물려 이루어져왔으며, 이로인해 IDE 같은 여러 도구의 활용을 늘 염두에 두고 설계되어 왔습니다.

1.4.2. Concise
- 프로그래머가 작성하는 코드에서 의미 없는 부분을 줄이고 프로그램에 꼭 필요하지만 부수적인 요소를 줄일 수 있도록 노력해왔습니다.
- 이러한 예로, getter, setter, constructor 등의 로직을 코틀린은 기본적으로 제공합니다.
 - 이로 인해, lombok의 Getter, Setter와 같은 어노테이션을 사용하지 않아도 됩니다.
- 코틀린은 기본적으로 반복되거나 길어질 수 있는 코드를 API로 제공하기 때문에, 간결한 코드 작성이 가능하고 이로인해 코드를 읽는 사람의 입장에서도 쉽게 코드를 읽을 수 있습니다.

1.4.3. Safe
- 코틀린은 JVM위에서 동작하기 때문에, 메모리 안전성을 보장하고 buffer overflow 및 dynamic allocation을 잘못해서 발생할 수 있는 문제를 예방 가능합니다.
- Runtime에서 발생할 수 있는 NPE등을 없애기 위해 null 값을 추적하고, NPE가 발생할 수 있는 코드를 금지하여 Runtime 에러를 예방할 수 있습니다.
- Nullable 타입을 핸들링하는 여러 방법을 제공합니다. 이러한 방법을 통해 NPE로 인해 프로그램이 중단되는 경우를 줄여줍니다.

1.4.4. Interoperable
- 코틀린의 클래스나 메소드에서 자바의 클래스와 메소드를 똑같이 사용가능합니다.
- 이러한 상호운용성 덕분에 코틀리은 자바 라이브러리가 어떤 API를 제공하던 사용할 수 있으며, 자바 어노테이션등도 코틀린 코드에서 적용할 수 있습니다.
- 코틀린 언어와 자바등의 소스 파일이 함께 섞여있어도 프로그램 컴파일이 가능합니다.

1.5. Using the Kotlin tools
이 1.5 챕터는 코틀린의 컴파일 과정 및 유용한 툴들에 대해 설명을 하고 있습니다. 1.5.1 정도만 읽고, 1.5.2 ~ 1.5.6 은 이런게 있다 정도만 알아도 될 것 같습니다.

1.5.1. Compiling Kotlin code
- .kt 라는 파일을 사용하며, 자바와 같이 컴파일시 .class 파일을 생성합니다.
- 코틀린의 빌드 과정은 아래와 같습니다.

- 코틀린 컴파일러로 컴파일된 코드는 코틀린 런타임에 의존하며, 코틀린 런타임은 자체 표준과 자바 API 기능을 확장한 내용을 포함합니다.
- 이러한 빌드시스템 덕분에, 자바와 코틀린이 함께 컴파일되어도 문제없이 빌드가 가능합니다. 추가로, 메이븐 및 그레이들은 어플리케이션 빌드시 코틀린 런타임을 포함하여 잘 빌드하도록 해줍니다.

1.5.2. Plug-in for IntelliJ IDEA and Android Studio
- Jetbrains에서 만든 언어이기 때문에 IntelliJ IDEA 및 Android Studio의 지원을 쉽게 받을 수 있습니다.

1.5.4. Interactive Shell
- 파이썬과 같은 언어들에서 제공하는 REPL을 제공하여, 간단하게 언어를 테스트 할 수 있게 해줍니다.

1.5.4. Eclipse plug-in
- 이클립스에서 사용할 수 있는 플러그인도 제공해줍니다.

1.5.5. Online playground
- 코틀린을 작동시켜가며 테스트해볼 수 있는 플레이그라운드를 제공해줍니다. ( https://play.kotlinlang.org/ )

1.5.6. Java-to-Kotlin converter
- 자바를 코틀린 파일로 컨버팅할 수 있는 플러그인을 제공합니다.
- IntelliJ IDEA에서 제공하는 플러그인을 통해 손쉽게 사용가능합니다.

1.6. Summary
- 코틀린은 정적타입 언어이며, 타입추론이 가능하다. 이러한 특징들은 소스 코드의 간결성을 보장하며, 정확성과 퍼포먼스도 유지할 수 있습니다.
- 코틀린은 함수형 프로그래밍과 OOP를 지원합니다.
- 서버사이드, 안드로이드 모두 사용가능합니다.
- 무료이며, 오픈소스이다. 여러 IDE와 Build system에서 도움을 받기 쉽습니다.
- 실용적이고, 안전하며, 간결한 언어입니다.
- 일반적으로 자주 발생하는 NPE 와 같은 문제를 피할 수 있도록 도와주며, 읽기 쉬운 코드를 생산할 수 있게하며 Java와 완전히 상호운용이 가능합니다.

챕터1의 내용은 위와 같이 코틀린을 사용해야하는 이유와 코틀린의 여러 특성에 대해 설명하고 있었습니다. 특히나 간결한 코드를 작성할 수 있게 언어를 설계했다는 것, 자바와의 상호운용성이 좋다는 것 그리고 NPE를 피하기위해 많은 노력을 기울이고 있다는 것을 열심히 설명하는 파트였던 것 같습니다.