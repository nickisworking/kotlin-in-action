4. 클래스, 객체, 인터페이스

4.1 클래스 계층 정의

4.1.1 코틀린 인터페이스

코틀린 인터페이스 사례
interface Clicable {
    fun click()                                // 일반 메서드
    fun showOff() = println("I'm clickable!")  // 디폴트 구현이 있는 메서드
}

interface Focusable {
    fun setFocus(b: Boolean) = println("I ${if (b) "got" else "lost"} focus.")
    fun showOff() = println("I'm focusable!")
}

코틀린 인터페이스 구현 클래스 사례
 - ":" 키워드 통해 클래스 확장과 인터페이스 구현 모두 처리 가능
 - 오버라이드 시 자바의 @Override 어노테이션과 비슷한 "override" 변경자 사용. 하지만 자바와 달리 반드시 써야 함
    ㄴ 실수로 상위 클래스의 메서드를 오버라이드하는 경우 방지
 - 위의 showOff()처럼 디폴트 구현이 있는 메서드는 (1) 오버라이딩 가능, (2) 정의를 생략하고 디폴트 구현 이용도 가능
class Button : Clickable, Focusable {
    override fun click() = println("I was clicked")

    override fun showOff() {       // 상위 타입에 동일 시그니처 메서드 2개이므로 오버라이딩 필수
        super<Clickable>.showOff() // super 후 상위 타입 지정하면, 상위 타입의 메서드 지정 가능
        super<Focusable>.showOff()
    }
}

4.1.2 open, final, abstract 변경자: 기본적으로 final

상속 가능한 클래스 사례
- 자바의 클래스와 메서드는 기본적으로 상속에 대해 열려있지만 코틀린의 그것은 기본적으로 final
    ㄴ 즉 상속과 오버라이드 금지. 개발자가 인지 못하는 사이에 오버라이드 되는 것을 방지
    ㄴ 상속과 오버라이드가 가능하도록 하려면 클래스명 앞에 "open" 변경자 삽입
open class RichButton : Clickable {   // 상속 가능한 클래스
    fun disable() {...}               // 오버라이드할 수 없는 final 메서드
    open fun animate() {...}          // 오버라이드 가능한 메서드
    override fun click() {...}        // 오버라이드한 메서드(기본적으로 상속 가능)
    final override fun click() {...}  // 오버라이드한 메서드이나 상속 불가능한 final 메서드
}

추상 클래스 사례
abstract class Animated {
    abstract fun animate()      // 추상 메서드. 하위 클래스에서 반드시 오버라이드 필요
    open fun stopAnimating() {} // 비 추상 메서드. 오버라이드 가능
    fun animateTwice() {}       // 비 추상 메서드. 오버라이드 불가능
}

클래스 내에서 상속 제어 변경자(access modifier)의 의미
삭제된 콘텐츠입니다.

4.1.3 가시성 변경자: 기본적으로 공개
삭제된 콘텐츠입니다.
internal open class TalkativeButton : Focusable {
    private fun yell() = println("Hey!")
    protected fun whisper() = println("Let's talk!")
}

fun TalkativeButton.giveSpeech() { // 오류: public 메서드가 internal 클래스를 노출시키려 함
    yell()                         // 오류: yell 메서드는 internal 클래스의 private 멤버
    whisper()                      // 오류: yell 메서드는 internal 클래스의 protected 멤버
}

4.1.4 내부 클래스와 중첩된 클래스: 기본적으로 중첩 클래스
코틀린도 자바처럼 클래스 안에 다른 클래스 선언이 가능하나(중첩클래스), 자바와의 차이는 기본적으로 중첩클래스에서 바깥쪽 클래스 인스턴스에 대한 접근권한이 없다는 점임
- 이를 내부 클래스로 변경헤서 바깥쪽 클래스에 접근하고 싶다면 "inner" 변경자 삽입
- 내부 클래스에서 바깥쪽 클래스에 접근하고 싶다면 "this@바깥쪽클래스명" (아래는 예시)
class Outer {
    inner class Inner {
        fun getOuterReference(): Outer = this@Outer
    }
}

4.1.5 봉인된 클래스: 클래스 계층 정의 시 계층 확장 제한
interface Expr
class Num(val value: Int) : Expr
class Sum(val left: Expr, val right: Expr) : Expr

fun eval(e: Expr): Int =
    when (e) {
        is Num -> e.value
        is Sum -> eval(e.right) + eval(e.left)
        else ->                 // 디폴트 분기. 없으면 에러
            throw IllegalArgumentException("Unknown expression")
}
코틀린 컴파일러는 when을 이용해 Expr 타입의 값을 검사할 때 꼭 디폴트 분기인 else 문이 있어야 하나, "sealed" 변경자를 이용해 상위클래스를 상속한 하위 클래스의 정의를 제한 가능
sealed class Expr {                     // sealed 변경자 이용해 기반 클래스를 봉인
    class Num(val value: Int) : Expr()  // 기반 클래스의 모든 하위 클래스를 중첩 클래스로 정의
    class Sum(val left: Expr, val right: Expr) : Expr()
}

fun eval(e: Expr): Int =
    when (e) {                          // when 식이 모든 하위클래스를 검사하므로 else 불필요
        is Expr.Num -> e.value
        is Expr.Sum -> eval(e.right) + eval(e.left)
    }

4.2 뻔하지 않은 생성자와 프로퍼티를 가지는 클래스 선언

4.2.1 클래스 초기화: 주 생성자와 초기화 블록

주생성자
클래스 이름 뒤에 오는 괄호로 둘러싸인 코드
class User(val nickname: String)

초기화 블록
init 키워드로 시작하는 블록
- 초기화 블록은 주 생성자와 함께 사용
    ㄴ 주 생성자는 제한적이며 별도의 코드를 포함할 수 없으므로 초기화 블록 필요
    ㄴ 클래스 안에 여러 초기화 블록 선언 가능
class User(val nickname: String) {  // 파라미터가 하나만 있는 주 생성자
    val nickname: String

    init {                          // 초기화 블록
        nickname = _nickname
    }
}

디폴트 생성자
open class Button           // 인자가 없는 디폴트 생성자가 만들어짐

class RadioButton: Button() // Button 클래스의 하위 클래스는 Button 클래스의 생성자 반드시 호출

4.2.2 부 생성자: 상위 클래스를 다른 방식으로 초기화

open class View {
    constructor(ctx: Context) {                      // 부 생성자
        // some code
    }
    constructor(ctx: Context, attr: AttributeSet) {  // 부 생성자
        // some code
    }
}

class MyButton : View {
    constructor(ctx: Context)
        : super(ctx) {          // 상위 클래스 생성자 호출
        // ...
    }
    constructor(ctx: Context, attr: AttributeSet)
        :  super(ctx, attr) {   // 상위 클래스 생성자 호출
        // ...
    }
}

4.2.3 인터페이스에 선언된 프로퍼티 구현

코틀린에서는 아래와 같이 인터페이스에 추상 프로퍼티 선언이 가능
interface User {
    val nickname: String
}

인터페이스는 아무 상태도 포함할 수 없음
- 상태를 저장할 필요가 있다면, 아래와 같이 구현 클래스에서 nickname의 값을 얻을 수 있는 방법 제공 필요
// 주 생성자로 상위 인터페이스의 추상 프로퍼티 접근
class PrivateUser(override val nickname: String) : User

// 커스텀 게터 
class SubscribingUser(val email: String) : User {
    override val nickname: String
        // 게터 호출될 때마다 연산 후 값 반환
        get() = email.substringBefore('@')
}

// 프로퍼티 초기화 식
class FacebookUser(val accountId: Int) : User {
    // 객체 초기화 시 계산한 데이터를 뒷받침하는 필드에 저장(여기서는 nickname)하고 차후 불러오기 가능
    override val nickname = getFacebookName(accountId)
}

>>> println(PrivateUser("test@kotlinlang.org").nickname)
test@kotlinlang.org
>>> println(SubscribingUser("test@kotlinlang.org").nickname)
test

4.2.4 게터와 세터에서 뒷받침하는 필드에 접근

"field" 키워드 통해 게터와 세터에서는 뒷받침하는 필드에 접근 가능
- 게터는 field 값을 읽기만 가능
- 세터는 field 값을 읽기, 쓰기 가능
class User(val name: String) {
    var address: String = "unspecified"
        set(value: String) {
            println("""
                Address was changed for $name:
                "$field" -> "$value".""".trimIndent()) // 뒷받침하는 필드 값 얻기
            field = value // 뒷받침하는 필드 값 변경
        }
}

4.2.5 접근자의 가시성 변경

접근자의 가시성은 기본적으로는 프로퍼티의 가시성과 같음
- 그러나 원한다면 get or set 앞에 가시성 변경자를 추가해서 가시성 변경 가능
class LengthCounter {
    var counter: Int = 0
        // counter는 기본적으로 public이나 세터를 private로 설정했으므로 클래스 외부에서 변경 불가능
        private set

    fun addWord(word: String) {
        counter += word.length
    }
}

4.3 컴파일러가 생성한 메서드: 데이터 클래스와 클래스 위임

4.3.1 모든 클래스가 정의해야 하는 메서드
자바와 마찬가지로 코틀린 클래스도 toString(), equals(), hashCode() 등을 오버라이드 가능
- toString(): 코틀린에서도 객체의 toString()은 Client@5e9f23b4와 같은 방식임. 필요시 오버라이드 필요
- equals(): 주소가 아닌, 프로퍼티의 값이 같을 경우 동일하다고 보려면 아래와 같이 오버라이드 필요
abstract class Person() {
    abstract val name: String
    abstract val cellphoneNumber: String

    override fun equals(other: Any?): Boolean { // 오버라이드
        if (other == null || other !is Person)
            return false
        return name == other.name && cellphoneNumber == other.cellphoneNumber
    }
}

class User(name: String, cellphoneNumber: String): Person() {
    override val name: String = name
    override val cellphoneNumber: String = cellphoneNumber
}

fun main() {
    val user = User("최기환", "010-9991-9848")
    val user2 = User("최기환", "010-9991-9848")

    println(user == user2)
}

실행결과: true
- hashCode(): 자바에서는(그리고 코틀린에서도) equals()를 오버라이드할 때는 반드시 hashCode()도 오버라이드 필요

4.3.2 데이터 클래스: 모든 클래스가 정의해야 하는 메서드 자동 생성

어떤 클래스가 데이터를 저장하는 역할만을 수행한다면 toString(), equals(), hashCode() 등을 오버라이드하게 되는데 일반적으로 구현이 어렵지 않고, IDE를 통해 자동으로 생성할 수 있음.

코틀린은 더 편리하다 클래스를 데이터 클래스로 선언하면 자바에서 요구하는 모든 메서드를 자동으로 포함시킬 수 있다.
- 인스턴스 간 비교를 위한 equals()
- hashMap과 같은 해시 기반 컨테이너에서 키로 사용할 수 있는 hashCode()
- 클래스의 각 필드를 선언한 순서대로 표시하는 문자열을 만들어주는 toString()
- 아래에서 설명할 copy()
 주의  
equals와 hashCode는 주 생성자에 나열된 프로퍼티를 기준으로 만들어진다.
즉, 주 생성자 밖에 정의된 프로퍼티는 equals와 hashCode를 계산할 때 고려의 대상이 아니다.

데이터 클래스
data 변경자가 앞에 붙은 class
data class Client(val name: String, val postalCode: Int)

데이터 클래스와 불변성: copy() 메서드
데이터 클래스의 프로퍼티가 반드시 val일 필요는 없으나 모든 프로퍼티를 val로 하길 권장
- hashMap 등의 컨테이너에 테이터 클래스 객체를 담는다면 불변성이 필수적
    ㄴ 이유: 객체를 키로 컨테이너에 담은 후 객체의 프로퍼티가 변경된다면 데이터 정합성을 해칠 수 있음
- 불변 객체를 사용하면 프로그램에 대해 훨씬 쉽게 추론이 가능
- 특히 다중쓰레드 프로그램의 경우 불변객체는 다른 쓰레드에서 변경 불가능하므로 동기화 필요성 감소
- copy() 메서드는 객체를 복사하면서 일부 프로퍼티를 변경할 수 있게 해줌
    ㄴ 즉 원본에 영향이 없으며 복사본은 원본과 완전히 다른 Lifecycle을 가짐

4.3.3 클래스 위임: by 키워드 사용

클래스 위임
상속받지 않더라도 상속받은 것과 비슷한 효과를 발휘하는 기법
- 배경: 코틀린은 open 변경자를 삽입하지 않는 한 기본적으로 final이라 상속이 불가능
- 사용이유: 상속이 불가능한 클래스에 기능을 추가하고, 해당 기능을 이용하고 싶은 경우 클래스 위임이 적절
class CountingSet<T>(
    val innerSet: MutableCollection<T> = HashSet<T>()
) : MutableCollection<T> by innerSet { // MutableCollection 구현을 innerSet에 위임
    var objectsAdded = 0

    // 이 메서드는 위임하지 않고 재정의하나, 내부에서 MutableCollection의 add를 이용
    override fun add(element: T): Boolean {
        objectsAdded++
        return innerSet.add(element)
    }

    // 이 메서드는 위임하지 않고 재정의하나, 내부에서 MutableCollection의 addAll을 이용
    override fun addAll(c: Collection<T>): Boolean {
        objectsAdded += c.size
        return innerSet.addAll(c)
    }
}

4.4 object 키워드: 클래스 선언과 인스턴스 생성

4.4.1 객체 선언: 싱글턴을 쉽게 만들기

객체 선언
객체 선언 = 클래스 선언 + 그 클래스에 속한 단일 인스턴스의 선언
object Payroll { // "object" 키워드 통한 객체 선언
    val allEmployees = arrayListOf<Person>()
    fun calculateSalary() {
        for (person in allEmployees) {
            ...
        }
    }
}

// 아래와 같이 이용 가능
Payroll.allEmployees.add(Person(...))
Payroll.calculateSalary()

4.4.2 동반 객체: 팩토리 메서드와 정적 멤버가 들어갈 장소

동반 객체
자바의 정적 메서드(static method) 호출이나 정적 필드 사용 구문과 같음
class A {
    companion object { // "companion" 키워드 통한 동반 객체 선언
        fun bar() {
            println("Companion object called")
        }
    }
}

>>> A.bar()       // 좌측과 같이 호출 가능
Companion object called

부 생성자가 여럿 있는 클래스가 있다고 할 때...
class User {
    val nickname: String

    constructor(email: String) {
        nickname = email.substringBefore('@')
    }

    constructor(facebookAccountId: Int) {
        nickname = getFacebookName(facebookAccountId)
    }
}

동반 객체를 이용하여 팩토리 메서드로 위 클래스 구현
class User private constructor(val nickname: String) {
    companion object {
        fun newSubscribingUser(email: String) = // 팩토리 메서드
            User(email.substringBefore('@'))
        fun newFacebookUser(accountId: Int) =   // 팩토리 메서드
            User(getFacebookName(accountId))
    }
}

>>> val subscribingUser = User.newSubscribingUser("bob@gmail.com")
>>> val facebookUser = User.newFacebookUser(4)
>>> println(subscribingUser.nickname)
bob

팩토리 메서드는 꽤 유용하나 하위 클래스에서 오버라이드할 수 없으므로, 만약 클래스를 확장해야 한다면 차라리 여러 생성자를 사용하는 편이 더 나은 해법이다.

4.4.3 동반 객체를 일반 객체처럼 사용

필요하다면 동반 객체에 이름을 붙일 수 있음
class Person(val name: String) {
    companion object Loader { // 동반 객체에 "Loader"라는 이름 부여
        fun fromJSON(jsonText: String): Person = ...
    }
}

>>> person = Person.Loader.fromJSON("{name: 'Dmitry'}") // 정상 동작
>>> person.name
Dmitry
>>> person2 = Person.fromJSON("{name: 'Brent'}")        // 정상 동작
>>> person2.name
Brent

동반 객체에서 인터페이스 구현도 가능
interface JSONFactory<T> {
    fun fromJSON(jsonText: String): T
}

class Person(val name: String) {
    // 동반 객체에서 인터페이스 구현
    companion object : JSONFactory<Person> {
        override fun fromJSON(jsonText: String): Person = ...
    }
}

// JSON으로부터 각 원소를 다시 만들어내는 함수가 있을 때
fun loadFromJSON<T>(factory: JSONFactory<T>): T {
    ...
}

// Person 객체를 위 함수로 넘길 수 있다.
loadFromJSON(Person) // Person임에 유의

동반 객체 확장
클래스의 확장 함수처럼, 동반 객체도 확장 함수를 적용할 수 있음
// 비즈니스 로직 모듈
class Person(val firstName: String, val lastName: String) {
    // 동반 객체에 대한 확장함수를 작성할 수 있으려면, 원래 클래스에 동반 객체를 꼭 선언해야 한다.
    // (설령 아래처럼 빈 객체라도 있어야 한다.)
    companion object { // 비어있는 동반 객체 선언
    }
}

// client/server communication module
fun Person.Companion.fromJSON(json: String): Person { // 확장 함수 선언
    ...
}

// 아래 함수는 Person의 멤버 함수가 아니지만 동반 객체에 확장 함수를 작성하여 
val p = Person.fromJSON(json)

4.4.4 객체 식: 무명 내부 클래스를 다른 방식으로 지정
fun countClicks(window: Window) {
    var clickCount = 0 // 자바와 달리 final이 아닌 변수도 사용 가능

    window.addMouseListener(object : MouseAdapter() {
        override fun mouseClicked(e: MouseEvent) {
            clickCount++ // 로컬 변수 변경 가능
        }
    })
    // ...
}