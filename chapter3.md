# Kotlin in Action
## Part3. 함수 정의와 호출

이번 파트에서는 함수 정의와 호출 기능이 코틀린에서 어떻게 개선되었는지 알아보려고 합니다. 추상적인 내용보다는 코틀린 컬렉션, 문자열, 정규식 관련 영역을 살펴볼 수 있었습니다. 

### 1.  코틀린에서 컬렉션 만들기
코틀린에서는 별도로 컬렉션 기능을 제공하지 않고, 기존 자바 컬렉션을 활용합니다. 
* 자바 코드와의 상호작용이 쉬워짐
* 별도의 변환 과정 필요 없음
* 자바보다 더 많은 기능 제공
``` kotlin
val strings = listOf("first", "second", "fourteenth")
println(strings.last())

val numbers = setOf(1,14,2)
println(numnbers.max())
```

### 2. 함수를 호출하기 쉽게 만들기
#### 이름 붙인 인자
코틀린에서는 함수에 전달하는 인자 중 일부의 이름을 명시할 수 있습니다.  특히, 하나라도 이름을 명시하면 뒤에 오는 모든 인자는 이름을 꼭 명시해야 합니다. 파이썬 규칙과 유사한 것 같습니다.
``` kotlin
println(joinToString(list,";","(",")")) // ok
println(joinToString(list, ";", prefix = "(", ")")) // ok
println(joinToString(list, postfix = ")", ";", "(")) // 컴파일에러
```

#### 디폴트 파라미터 값
기존 자바에서는, 하위 호환성을 유지하거나 API 사용자에게 편의성을 더하는 등의 이유로 오버로딩이 자주 사용됩니다. 그러다보니. 오버로딩된 메서드가 너무 많아져서 **중복이 발생합니다.** 코틀린에서는 **함수 선언부의 파라미터에 디폴트 값을 사용할 수 있어, 위와 같은 중복을 줄일 수 있습니다.**

``` kotlin
fun<T> joinToString(collection : Collection<T>, 
seperator : String = ",", 
prefix : String = "", 
postfix :String = "") : String 
```

> 단, 자바에서 코틀린 함수를 호출하는 경우에는 모든 파라미터를 명시해줘야 합니다. 만약, 자바에서 코틀린 함수를 자주 호출한다면 @JvmOverloads 어노테이션을 추가합니다. 해당 어노테이션을 추가하면, 코틀린 컴파일러가 자동으로 생략된 오버로딩 자바 메서드들을 생성합니다.

#### 정적 유틸리티 클래스 없애기 : 최상위 함수와 프로퍼티
자바에서는 모든 코드가 클래스 안에 들어가야 하기 때문에, 여러 정적 메서드를 모아두는 역할만 하면서 어떤 상태나 인스턴스는 없는 ‘util’ 클래스를 많이 사용합니다. 코틀린에서는 이러한 클래스를  만들 필요 없이, 소스파일의 최상위 수준에 위치시키면 됩니다.

* 소스파일의 최상위에 .kt 파일을 생성
* JVM에서는 해당 파일을 컴파일 할때, 해당 파일의 이름에 맞는 클래스를 생성
* 최상위 함수들은 이 클래스의 정적 메서드로 생성
* 단, 클래스 이름을 바꾸고 싶다면 @JvmName 어노테이션 사용

#### 최상위 프로퍼티
함수와 마찬가지로 프로퍼티도 최상위 수준에 넣을 수 있습니다. 마찬가지로 정적 필드에 저장됩니다.

* 단, 다른 프로퍼티처럼 getter를 통해 자바코드에 노출되기 때문에, const 를 추가
* const val -> public static final 

### 메서드를 다른 클래스에 추가 : 확장 함수와 확장 프로퍼티
기존 자바 코드가 코틀린으로 직접 변경이 안되거나 혹은 미처 변환되지 못한 코드를 처리할 때? **확장함수**의 기능을 사용할 수 있습니다.

확장함수??
* 어떤 클래스의 멤버 매서드인 것 처럼 호출할 수 있으나, 그 클래스의 **밖에 선언된 함수**
* 수신 객체 타입 : 클래스의 이름 (클래스 타입)
* 수신 객체 : 확장함수가 호출되면 대상이 되는 값 (인스턴스 객체)
```kotlin
fun String.lastChar() : Char = this.get(this.length-1)
//String : 수신 객체 타입
//this : 수신 객체

println("Kotlin".lastChar()) // 멤버 매서드인 것 처럼 사용. 구분 불가, 구분이 중요하지도 않음
```

* 클래스에 메서드를 추가하는 것과 동일
* 소스코드가 없어도 추가가 가능
* 심지어  Javar가 아닌 다른 JVM 언어면 확장 가능
* this 생략 가능 , 다른 property 사용 가능
* **단, private, protected 사용 불가능** -> 캡슐화 

####  임포트와 확장 함수
확장 함수를 사용하려면, 해당 함수를 임포트 해야 합니다. 중복된 이름의 확장 함수의 충돌을 막기 위함입니다.

as 키워드를 사용해 임포트한 클래스나 함수를 다른 이름으로 사용할 수 있습니다. 

#### 자바에서 확장 함수 호출
확장함수는 내부적으로 **수신 객체를 첫 번째 인자로 받는 정적 메서드** 입니다.  최상위 함수처럼, 확장함수가 들어있는 파일 이름으로 자바 클래스가 결졍됩니다.

lastChar 함수가 StringUtil.kt 파일에 정의된 경우, 

``` java
//java
char c = StringUtilKt.lastChar("Java");
```

와 같이 사용합니다.

#### 확장 함수로 유틸리티 함수 정의
확장 함수는 단지 **정적 메서드 호출에 대한 문법적 편의** 일 분이기 때문에, 구체 타입을 수신 객체 타입으로 지정할 수도 있습니다. 

String 타입의 컬렉션에만 적용하고 싶다면, collection<String> 으로 정의하면 되겠습니다.

#### 확장 함수는 오버라이드 할 수 없다. 
확장 함수는 정적 메서드와 같은 특징을 갖기 때문에, 하위 클래스에서 오버라이드 할 수 없습니다.

확장 함수는 클래스의 일부가 아니고, 확장 함수를 호출할 때 **수신 객체로 지정한 변수의 정적 타입에 의해 어떤 확장 함수가 호출될 지 결정되기** 때문입니다.

Ex)  View <- Button 상속

```kotlin
fun View.showOff() = println("view")
fun Button.showOff() = println("button")

val view:View = Button()
view.showoff()
// "view"

//Java
View view = new Button();
ExtensionsKt.showOff(view);
```

수신 객체로 View가 설정되어 있기 때문에, View의 정적 타입에 따리 showoff 메서드가 호출되는걸 볼 수 있습니다. Static 메서드로 컴파일되는 점을 이해하면 될 것 같습니다.

> 확장함수의 시그니처와 멤버 함수의 시그니처가 동일하다면, 멤버 함수가 높은 우선순위를 갖고 호출되기 때문에 이를 고려해야 합니다.

#### 확장 프로퍼티
확장함수처럼 클래스 객체에 프로퍼티 형식의 구문으로 사용할 수 있는 방식이지만, 실제로 상태를 저장할 방법은 없기 때문에 상태를 가지는 것은 아니기는 합니다. 단 이 문법을 통해 더 짧게 코드를 작성할 수 있습니다.

```kotlin
val String.lastChar = Char
	get() = get(length-1)
	set(value:Char) {
	this.setCharAt(length-1, value)
}

//동일하게 사용
println ("Kotlin".lastChar)
val sb = StringBuilder("Kotlin?")
sb.lastChar = '!'
```

* 프로퍼티와 동일하나 수신 객체 클래스가 추가
* backing filed (프로퍼티의 값을 저장하는 필드)가 없기 때문에 최소한 게터 정의 필요, 초기화도 불가능 
* Java에서는 마찬가지로 해당 파일명의 클래스를 통해 사용 가능. 게터, 세터 명시적 호출해야 함

### 컬렉션 처리 : 가변 길이 인자, 중위 함수 호출, 라이브러리 지원
컬렉션을 처리할 때 쓸 수 있는 코틀린 표준 라이브러리 함수를 살펴봅니다. 해당 과정에서 다음과 같은 코틀린의 언어 특성을 살펴볼 수 있습니다.

* vararg 키워드 사용하면 호출 시 인자 개수가 달라지는 함수 정의 가능
* infix 함수 호출 구문 사용시 인자가 하나뿐인 메서드 간편하게 호출 가능
* 구조 분해 선언을 사용하면 복합 값을 분해해 여러 변수에 나눠 담음

### 자바 컬렉션 API 확징
자바 컬렉션을 사용하는데 더 확장된 API를 제공하는 것은, 바로 위의 확장 함수를 통해 구현되었습니다.

가령, max(), last() 함수도 사실 까보면

```kotlin
fun <T> List<T>.last() : T {/* 마지막 원소 반환 */}
fun Collection<Int>.max() : Int {/* 최대값 반환 */}
```

#### 가변 인자 함수 : 인자의 개수가 달라질 수 있는 함수 정의
가변 인자를 넘기기 위해서는, vararg 키워드를 사용합니다.
```kotlin
fun listOf<T>(vararg values : T) : List<T> {..}
```

배열에 들어이쓴ㄴ 원소를 가변 길이 인자로 넘길 때는, 자바에서 그냥 넘기던 것과는 다르게 spread 연산자를 사용해야 합니다. 이는 *를 붙이면 됩니다.

```kotlin
val list = listOf("args: ", *args)
```


#### 값의 쌍 다루기 : 중위 호출과 구조 분해 선언
중위호출?
* 객체 {메서드 이름} {유일한 인자} 의 형식으로 메서드를 호출하는 방법
* 인자가 하나뿐인 일반 메서드, 인자가 하나뿐인 확장함수에 중위호출 사용 가능
* infix 선언으로 중위 호출 허용

```kotlin
infix fun Any.to(other:Any) = Pari(this,other)

1.to("one") //일반적인 호출 방식
1 to "one" // 중위 호출
```

구조 분해 선언?
* 위에서 Pair를 두 변수로 즉시 초기화 할 수 있는 기능

```kotlin
val (number,name) = 1 to "one"
```

* key-value, index-element 등, 다른 객체에도 적용 가능

### 문자열과 정규식 다루기
코틀린에서의 문자열은 자바의 문자열과 동일합니다. 다만, 어떤 확장 함수들을 제공하는지 알아봅니다.

#### 문자열 나누기
자바의 split이 정규식으로 인자를 해석하는 반면, 코틀린에서는 여러 조합의 파라미터를 받는 확장함수를 제공하여 혼란스러운 메서드를 감춰버립니다. 

정규식을 파라미터로 받는 경우는 String이 아닌 Regex 타입을 받기 때문에, 인자의 타입으로 구분자가 정규식인지, 문자열인지 알 수 있습니다.

뿐만 아니라, split 함수를 오버로딩하여 구분 문자열을 하나 이상 인자로 받는 함수도 존재하기 때문에 문자열도 당연히 사용 가능합니다. 

```kotlin
println("12.345-6.A".split("\\.|-".toRegex())) 
//toRegex 함수를 통해 정규식으로 명시적으로 만듬

println("12.345-6.A".split(".","-"))
//여러 구분 문자열 지정
```

#### 정규식과 3중 따옴표로 묶은 문자열
코틀린 표준 라이브러리에서는 어떤 구분 문자열이 맨 나중 / 처음에 나타난 곳의 앞/뒤를 반환하는 함수가 있는데, 이러한 함수들을 사용해 정규표현식을 사용하지 않을 수도 있습니다.  따라서, 복잡한 정규표현식을 사용하지 않아 가독성을 높일 수 있습니다.

```kotlin
val directory = path.substringBeforeLast("/")
val fullName = path.substringAfterLast("/")
...
```

정규식을 사용할 때도, 코틀린 라이브러리를 사용할 수 있습니다.

```kotlin
val regex = """(.+)/(.+)\.(.+)""".toRegex()
...
```

3중 따옴표 문자열을 사용하면, \를 포함한 문제도 이스케이프할 필요가 없습니다. 


#### 여러 줄 3중 따옴표 문자열
문자열 이스케이프 뿐만 아니라, 3중 따옴표 문자열에서는 줄 바꿈을 표현하는 아무 문자열이나 그대로 들어갑니다. 즉, 줄바꿈이 들어있는 텍스트를 쉽게 문자열로 바꿀 수 있습니다.  ASCII 아트를 쉽게 만드는걸 상상하면 좋을 것 같습니다.

단, 보기좋게 만들고 싶다면 들여쓰기 하고 특정 문자열을 사용한 다음에 trimMargin을 사용해 그 문자열과 직전의 공백을 제거하면 되겠습니다.

단, **문자열 템플릿**을 사용해야 하기 때문에, 3중 다옴표 문자열 안에 $를 넣어야 한다면  문자열 템플릿 안에 $’ 문자를 사용해야합니다. 

테스트 등에서 html 텍스트를 그대로 넣는다던가 하는 방식으로 사용될 수 있습니다. 

### 코드 다듬기 : 로컬 함수와 확장
중복을 피하기 위한 여러 방법들이 있지만, 보통 메서드 추출하여 긴 메서드를 나누고, 각 부분들을 재활용하곤 합니다.

하지만, 이런 경우 클래스 내에 작은 메서드들이 많아져 관계파악이 어려워지고 가독성이 떨어집니다. 별도의 내부 클래스에 넣을 수도 있지만 이 또한 불필요한 코드가 늘어나는 것이기도 합니다.

코틀린에서는 **로컬 함수**를 통해 이를 해결합니다. 함수에서 추출한 함수를 원 함수 내부에 배치하는 방법입니다. 

이는 예시로 살펴보는게 좋겠습니다.

``` kotlin
class User(val id : Int, val name : String, val address : String)

fun saveUser(user:User) {
    if(user.name.isEmpty()) {
        throw IllegalArgumentException();
    }
    if(user.address.isEmpty()) {
        throw IllegalArgumentException();
    }
    
    //user 저장
}
```

유저를 저장할 때 필드를 검증하는 로직이 중복됩니다. 필드가 늘어날 때마다 코드가 중복될 것입니다. 

이를 로컬 함수를 활용해 **중복을 줄일 수 있습니다.** 또한, 로컬 함수는 원 함수의 모든 파라미터에 접근할 수 있습니다.

``` kotlin
fun saveUser(user:User) {
    
    fun validate(value: String, fieldName : String) {
        if(value.isEmpty()) {
            throw IllegalArgumentException("Cant save user ${user.id} : " + "empty $fieldName")
        }
    }
    
    validate(user.name, "Name")
    validate(user.address, "Address")
    //user 저장
}

```

훨씬 깔끔해진 것 같습니다. 뿐만 아니라 아예 User class의 확장 함수로 검증로직을 빼버릴 수도 있겠습니다.

```kotlin
class User(val id : Int, val name : String, val address : String)

fun User.validateBeforeSave() {
    fun validate(value: String, fieldName : String) {
        if(value.isEmpty()) {
            throw IllegalArgumentException("Cant save user ${user.id} : " + "empty $fieldName")
        }
    }

    validate(name, "Name")
    validate(address, "Address")
}

fun saveUser(user:User) {
    user.validateBeforeSave()
    //user 저장
}
```

User의 내용은 간단하게 유지하면서, 검증 로직이 필요한 곳에서만 사용하도록 할 수 있습니다. (굳이 User를 쓰는 다른 곳에서는 검증 로직이 궁금하지 않을 수도 있으니)

확장 함수를 로컬 함수로 정의할 수도 있지만, (User.validateBeforeSave를 saveUser 내부에) 중첩된 함수의 깊이가 깊어지면 가독성이 떨어집니다.

따라서, **일반적으로 한 단계만 함수를 중첩시킵니다.**



