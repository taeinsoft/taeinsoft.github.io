---
title: "[강의] 코딩 훈련 교본 (코틀린편) - 03. 출력"
categories:
  - Kotlin
  - Training
tags:
  - Kotlin
date: 2021-11-28
last_modified_at: 2021-11-28
toc_label: 목차
---
> 본 강의는 코틀린(Kotlin)의 문법강의가 아닌, 프로그래밍 훈련을 위한 강의입니다.
> 따라서, 본 강의에서 알려드리는 내용만으로 코딩훈련에 임하시기 바랍니다.

# 문자열 출력

프로그램의 입력, 처리, 출력 중 가장 먼저 출력 기능을 배워보겠습니다.

**절대 복사해서 붙여넣지 마시고, 직접 타이핑 하시기 바랍니다.**

첫번째 출력입니다.

``` kotlin
fun main(args: Array<String>) {
    println("Hello world!!!")
}
```

여러줄 동일하게 출력해보겠습니다.

``` kotlin
fun main(args: Array<String>) {
    println("Hello world!!!")
    println("Hello world!!!")
    println("Hello world!!!")
}
```

println은 프린트 후 라인을 바꿉니다. println을 print로 바꿔보겠습니다.

``` kotlin
fun main(args: Array<String>) {
    print("Hello world!!!")
    print("Hello world!!!")
    print("Hello world!!!")
}
```

라인을 바꿔주기 위해 new line 이라는 의미의 "\n"을 추가해주겠습니다.

``` kotlin
fun main(args: Array<String>) {
    print("Hello world!!!\n")
    print("Hello world!!!\n")
    print("Hello world!!!\n")
}
```

결과값이 같아졌습니다. 똑같은 결과가 나오더라도 다르게 짤 수 있습니다.

# 숫자 출력

다음은 숫자 계산을 해보겠습니다.

``` kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 10
    var c = a + b
    println(c)
}
```

위 코드에서 a, b, c는 **Variable**이라고 부르는 변수입니다. 이 곳에 문자를 넣을 수도, 숫자를 넣을 수도 있습니다.
일단은 숫자만 생각하고 아래 코드를 실행해보겠습니다.

``` kotlin
fun main(args: Array<String>){
    var a = 10
    var b = 10
    var c = a + b

    println("a = $a" + ", b = $b" + ", c = $c")
    println("a = $a, b = $b, c = $c")
}
```

위 코드와 같이 변수의 내용을 출력하려면 **[$기호 + 변수명]** 으로 표기하시면 됩니다.

모든 프로그래밍언어는 문자와 숫자를 구분합니다. 1, 2, 3, 4... 가 숫자일 수도 있고, 문자일 수도 있습니다. 

# 숫자와 문자

다음 코드를 실행해보겠습니다.

``` kotlin
fun main(args: Array<String>) {
    println(1 + 1)
    println("1" + "1")
}
```

거의 모든 프로그래밍 언어가 동일합니다. 숫자만 쓰면 숫자로 인식하여 계산을 수행합니다.
하지만 숫자 계산을 위한 숫자도 있지만, 문자 자체로서의 숫자도 있습니다. 그런 것들은 쌍따옴표로 묶어주면 됩니다.
아래 코드를 더 실행시켜 보겠습니다.

``` kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 20
    var c = a + b
    println("a + b = c")
    println("10 + 20 = 30")
    println("$a + $b = $c")
}
```

위 코드를 실행해보시고, 이것저거 바꿔서 실행해보시기 바랍니다.
문자와 숫자의 개녕에 대해 알아봤습니다. 다음 장에서는 반복문을 배워보도록 하겠습니다.
