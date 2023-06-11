2. 코틀린 기초
 주제 : 코틀린의 기초 문법
 - 함수, 변수, 클래스, enum, when
 - 제어구조
 - 스마트 캐스트

 의견 : 코틀린 = 자바 + 파이썬 느낌이 드는데, 아무튼 팬시함. ( 팬시하다고 느껴지는 부분들에 대해 굵은 글씨로 표현 )

1. 함수
특징을 먼저 정리해보자.
 - 함수를 최상위 수준에 정의할 수 있다. 자바와 달리 클래스 밖에 함수를 뺄 수 있다.
 - 파라미터 이름 뒤에 파라미터 타입을 쓴다. 
 - System.out.println 대신 println으로 대신한다. 물론 자바에 log4j가 있지만, println이 손가락이 훨씬 덜 힘들다. 단순 디버깅용 로깅만큼은 그냥 println으로 제공해줬어도 되지 않았겠나 자바여..
 - 세미콜론을 붙이지 않아도 된다. 이건 좀 문제가 있다. 붙이든 안 붙이든 통일성만 가져가면 상관 없다고 본다. 다만 한 패키지 안에서 나와 다른 스타일의 코드를 보면 조금 킹받을 수 있다. 그렇게 되면 토론을 해야한다. ( 꼭 그렇지 않더라도 사전에 통일성을 위한 토론을 하게 될 것이다. ) 그냥 언어 자체에서 붙이면 에러! 뺶! 혹은 안 붙이면 에러! 빾! 해줬으면 좋겠다. 불필요한 토론 야기가 아닌가 싶다. Call Status 모듈을 코틀린으로 하면 세미콜론 어떻게 할까요? @nick.bae vs @klyne.kim 에 고전학파인 @brain.kim 의 참전을 부탁드립니다.
 - 함수 선언 키워드는 fun을 쓴다. 함수 선언은 재밌으니까 fun이라는 드립을 실제로 저자가 사용했다. 여기서 팬시함 -10

코틀린에서 간단한 함수를 하나 선언하면 다음과 같다.
fun max (a: Int, b: Int) : Int {    
    return if ( a > b ) a else b
}
함수명 이후에 타입을 지정하고, 리턴 타입 그리고 블록을 통해 내용을 표현했다.
코틀린에서는 위 코드를 더 팬시하게 표현할 수 있는데, 그 전에 한 가지 개념을 정리하고 가야한다.

 문(statement) vs 식(expression)
 문 : 값을 만들어내지 않으며 가장 안쪽 블록의 최상위 요소로 존재한다.
 식 : 값을 만들어내며 다른 식의 하위 요소로 계산에 참여 가능하다.
 자바 : 모든 제어구조가 문이다. + 대입문은 식이다.
 코틀린 : 루프를 제외한 모든 제어구조가 식이다. + 대입문은 문이다.

굳이굳이 이렇게 코틀린과 자바가 의미를 다르게 가는 이유가 뭘까...?

일단 위의 코드는 코틀린에서 다음과 같이 표현이 가능하다.
fun max (a: Int, b: Int) : Int = if ( a > b ) a else b
 약간 js 냄새가 난다. 그치만 간단하고 보기 좋다. 그리고 위 코드는 또 아래와 같이 표현이 가능하다.
fun max (a: Int, b: Int) = if ( a > b ) a else b
 반환 타입을 생략할 수 있다. 코틀린의 타입 추론 기능 때문인데, 코틀린은 정적 타입 언어로 컴파일 시점에는 타입이 지정되어야 한다. 코틀린은 식의 값을 분석해서 반환 타입을 추론하는 기능을 가지고 있다. 똑똑하다.

2.변수
코틀린에서 변수를 선언하는 키워드는 2가지가 있다.
 - val : value에서 따온 것으로 변하지 않는 값을 선언할 때 사용한다.
 => value, 값이라는 것은 변하지 않는 값이라는 cs의 통상적인 개념을 잘 차용한 것 같다는 생각이다.
 - var: variable에서 따온 것으로 변하는 값을 선언할 때 사용한다.
 => vairable, 변수 역시 통상적인 개념을 잘 차용한 것 같다.
변수를 통한 선언은 다음과 같은 방법들이 있다.
val answer1 = 42
val answer2 : Int = 42
val answer3 : Int
answer3 = 42
val에 대해 초기화식을 사용하지 않을 경우에는 반드시 타입을 지정해주어야한다.
코틀린에서 변수를 선언할 때 타입을 지정해주지 않아도 타입 추론을 하지만 js에서 ts가 또 나온것과 같은 이유로 굳이굳이 타입을 생략하지는 않았으면 하는 개인적인 바람이다.

문자열 템플릿
js에 비해서도 더 편하다.
fun printName(name : String) {
    println("Hello $name")
}
위와 같이 $만 변수 앞에 붙이면 된다. js처럼 번거롭게 `` 같은 템플릿 전용 따옴표(?)를 쓸 필요가 없다.
fun printName(name : String) {
    println("Hello ${name}")
}
위와 같이 중괄호를 붙일 수도 있는데, 개인적으로 중괄호를 붙이는 것을 습관하는게 좋을 것 같다.
fun printName(name : String) {
    println("$name님 안녕하세요.")
}
위와 같이 표현할 경우 name님 을 하나의 식별자로 봐서 예상치 못한 에러가 발생할 수 있기 때문이다.

어쨌든 이 또한 자바보다는 훨씬 편리하다.

3. 클래스와 프로퍼티
간단한 Value Object를 자바에서 선언하면 다음과 같다.
public class Person {
    private final String name;

    public Person(String name) {
        this.name = name;
    }    

    public String getName() {
        return this.name;
    }
}
Person이라는 클래스 안에 name이라는 상수가 있고, 이에 대한 get 접근만 가능하다. 이 코드를 코틀린에서는 다음과 같이 표현할 수 있다.
class Person ( val name : String )
val 은 상수이기에 name에 대한 getter메소드만 자동으로 제공해준다. 그러면 var로 선언하면 setter까지 제공해줄 것이라는 것을 추측할 수 있다.
class Person ( val name : String, var age : Int )
실제로 위와 같이 선언하면 name에 대해서는 getter만 age에 대해서는 setter, getter를 모두 디폴트로 제공해준다. 물론 age를 만으로 따져야하는 경우에는 커스텀한 getter혹은 setter를 내부에 선언해주면 된다. 아주 멋진 것 같다. 자바에서도 물론 @Getter, @Setter, @Data와 같은 어노테이션을 사용할 수 있지만, 이들을 무지성으로 가져다 쓰기 전에 한 번 고민할 필요가 있다. 변하지 않았으면 하는 프로퍼티가 있는데 @Setter를 붙여도 될까? 그럼 그 하나의 프로퍼티 때문에 나머지 9개의 프로퍼티에 대해 setter를 따로 생성해주어야하나? @Data를 붙이면 편하긴 한데 toString등의 메소드로 인해 예상치 못한 에러가 발생하면 어떡하지? 와 같은 고민을 하게 되는데 코틀린은 그런 고민은 접어두고 비즈니스 로직에만 더 집중해 라고 하는 것 같다.

4. enum과 when
enum은 java와 비슷하다. 하지만 java의 switch와 같은 역할을 하는 when 과 enum을 같이 사용할 경우 그 시너지가 아주 훌륭하다.
enum class Color(val r: Int, val g: Int, val b: Int) { //상수 프로퍼티 정의

    RED(255,0,0),         //상수 생성시 프로퍼티 값 지정
    ORANGE(111,111,111),
    GREEN(222,222,222),
    BLUE(333,333,333); //<- 세미콜린 필수. 구분 역할

    fun rgb() = (r * 256 + g) * 256 + b //함수 정의
}

Color.BLUE.rgb() //rgb 함수호출
enum 클래스는 위와 선언하면 된다. 자바와 달리 코틀린에서 enum은 소프트 키워드로 다른 곳에서 식별자로 쓸 수 있다. 딱히 엄청나다!로 느껴지지는 않지만 쓸 곳이 있겠지...
이제 이 enum 과 when 을 같이 쓰면 다음과 같이 된다.
fun getColorStr(color: Color) =  
when (color) {
    Color.RED -> "red"
    Color.ORANGE -> "orange"
    Color.BLUE, Color.GREEN -> "blue and green" // 블루이거나 그린일 경우를 다음과 같이 표현 가능
    else -> "unknown"
}
아주 멋지다.
break도 안 써도 되고, -> 이후에 반환 값을 그냥 작성하기만 하면 된다.
여기까지만 하면 살짝 섭섭할 수 있다.
자바의 경우 조건에 상수만 넣을 수 있지만, 코틀린은 임의의 객체를 허용한다.
fun getColorMixStr(color1: Color, color2 : Color) =
    when (setOf(color1, color2)) {
        setOf(Color.ORANGE, Color.GREEN) -> "blue and green" //and 임
        else -> "unknown"
}
위와 같이 set 형태로 집어 넣을 수 있다. 그러면 color의 순서에 상관없이 집합의 효과로 원소가 오렌지와 그린이기만 하면 위 조건에 해당하게 된다. 조건에 다양한 형태가 들어감으로써 아주 효율적인 코드를 짤 수 있다. ( 괴물 같은 코드도 나올것 같아 조금 두렵다. )
심지어 조건을 다음과 같이 안 줄 수 도 있다.
fun getColorMixStr(color1: Color, color2 : Color) =
    when {
        (color1 == Color.ORANGE && color2 == Color.GREEN) -> Color.ORANGE
         else -> Color.GREEN
}

5. 스마트 캐스트
여기서부터 이제 슬슬 파이썬의 냄새가 나기 시작한다.
interface Expr 
class Num(val value: Int) : Expr 
class Sum(val left: Expr, val right: Expr) : Expr
Num 클래스는 Int를 인자로 받고 Expr 타입을 리턴한다.
Sum 클래스는 Expr을 인자로 받고 Expr 타입을 리턴한다.
fun eval(e: Expr): Int{
    if (e is Num){ 
        val n = e as Num // 불필요한 타입 변환 => 생략.
        return n.value
    }
    if (e is Sum){ 
        return eval(e.left) + eval(e.right) // 변수 e에 대한 스마트 캐스트 
    } 
    throw IllegalArgumentException("Unknown expression") 
}
함수의 내용은 다음과 같다. 
Expr 타입을 인자로 받는다. 만약 인자가 Num 타입이라면 인자의 value를 반환하고, Sum 타입이라면 다시 eval메소드에 인자의 left, right를 각각 넣어서 합친 값을 반환한다. 

여기서 우린 is, as 를 볼 수 있다. is 는 C#에서도 사용하는데 자바의 instanceOf와 같다. 자바에서는 타입을 확인하고 명시적으로 형변환을 해야하지만 코틀린에서는 그럴 필요가 없다. 형변환할 때 as를 사용할 수 있지만 is로 확인했다면 굳이 해줄 필요가 없다. 스마트 캐스트 기능이 있기 때문이다. is로 확인하고 나면 알아서 해준다. 실제로는 컴파일러가 컴파일 시점에 알아서 해준다.

위와 같이 코드를 짜면 아래와 같은 행동을 할 수 있다.
println(eval(Sum(Sum(Num(3), Num(3)), Num(4))))
+ 함수와 0만 있으면 무엇이든 만들 수 있

팬시하긴 한데 코드가 뭔가 너저분한게 마음에 안든다. 이제 when을 통해 코드를  좀 정갈하게 정리할 필요가 있다.
fun eval(e: Expr): Int = 
    when(e) {
        is Num -> e.value
        is Sum -> eval3(e.left) + eval3(e.right)
        else -> throw IllegalArgumentException("Unknown expression") 
    }
이렇게 정리된다면 개인적으로 매우 만족한다.

6. 이터레이션
코틀린에서는 이터레이션을 다음과 같이 사용한다. 파이썬의 냄새가 여기서도 난다. js인가..
for(i in 1..100) { … } → 100까지 포함
for(i in 1 until 100) { … } → 100은 포함하지 않음
for(x in 2..10 step 2) { … } → 2씩 증가
for(x in 10 downTo 1) { … } → 숫자 감소
in, .., until, step, downTo 와 같이 직관적인 단어를 사용한다. 멋지다.
멋진데,,,
for(i in 1..100) { … } → 100까지 포함
이건 좀 선을 넘은게 아닌가 한다.
마지막 숫자를 포함하다니... 마지막은 생략이 불문율 아닌가.... 코틀린 개발자가 군대를 가봤어야...

맵에 대한 이터레이션도 제공한다.
val binaryCharMap = TreeMap<Char, String>() 

for (c in 'A'..'F') {

    val binary = Integer.toBinaryString(c.toInt())
    binaryCharMap[c] = binary 

} // 아스키 코드 순서 기준 A ~ F의 단어를 맵에 넣는다.

for ((ch, bi) in binaryCharMap) {

    println("C ${ch} : Binary ${bi}" 

} // 맵의 key, value를 이렇게 바로 가져올 수 있다.

이상 코틀린에 대한 기초를 확인해봤다.
내용을 작성하며 아지트에서 글을 2번이나 날린 덕에 3번이나 내용을 다시 볼 수 있었다.
다시 봐도 팬시하고, 지금 하고 있는 프로젝트에 부분부분 적용해보고 싶은 욕구가 치솟는다.
클래스 단위로 좀 적용해봐야겠다.