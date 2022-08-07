---
title: "[강의] 코딩 훈련 교본 (코틀린편) - 12. 조건문"
categories:
  - Kotlin 
  - Training
tags:
  - Kotlin
date: 2021-12-19
last_modified_at: 2021-12-19
toc_label: 목차
---
> 본 강의는 코틀린(Kotlin)의 문법강의가 아닌, 프로그래밍 훈련을 위한 강의입니다.
> 따라서, 본 강의에서 알려드리는 내용만으로 코딩훈련에 임하시기 바랍니다.

# 조건문

배열도 익숙해졌으니 조금 더 복잡한 단계로 들어가 보겠습니다.
프로그램이 구동되는데에 있어서 가장 중요한 부분인 반복문을 배웠습니다.
이제는 반복문 못지 않게 중요한 조건문을 배워보겠습니다. 
문법책을 보신분들은 아시겠지만 if문입니다. 먼저 코드를 보겠습니다.

``` kotlin
fun main(args: Array<String>){
    var n = 5
    var arr = Array(n) { Array(n) { 0 } }

    for(i in 0 until n){
        for(j in 0 until n){
            if(i % 2 == 0){
                arr[i][j] = 1
            }
        }
    }

    println("n = $n")
    for(i in 0 until n) {
        for(j in 0 until n) {
            print("%4d".format(arr[i][j]))
        }
        println()
    }
}
```
결과 :
```
n = 5
   1   1   1   1   1
   0   0   0   0   0
   1   1   1   1   1
   0   0   0   0   0
   1   1   1   1   1
```

새로운 연산자가 나왔습니다. **"i % 2"** 이 구문에서 %연산자입니다. %연산자는 i를 2로 나눈 나머지
라는 뜻입니다. 예를 들면 "3 % 2"는 "1" 입니다. 또, "4 % 2"는 0입니다. 
이걸 사용하면 홀수인지 짝수인지 구분해서 처리하는 프로그램을 만들 수 있습니다.
뿐만아니라 %연산자는 나누기연산자인 "/"보다도 많이 쓰이기 때문에 잘 알아두실 필요가 있습니다.

이제 if문 전체를 해석해보면, "만약 i를 2로 나눈 나머지가 0과 같으면,"입니다. 이걸로 코드를 짜보겠습니다.

``` kotlin
만약(i % 2 == 0)이면, {
    코드 실행
}
```

생각보다 문법 자체는 단순합니다. 그렇다면, 조건에 맞지 않았을때도 실행을 해야할때가 있습니다.
그건 아래처럼 사용합니다.

``` kotlin
if(i % 2 == 0) {
    // 조건이 참일때
    짝수일때 코드
} else {
    // 조건이 거짓일때
    홀수일때 코드
}
```

다른 코드를 좀더 실행해보겠습니다.

``` kotlin
fun main(args: Array<String>){
    var n = 5
    var arr = Array(n) { Array(n) { 0 } }

    for(i in 0 until n){
        for(j in 0 until n){
            if(i == j) {
                arr[i][j] = 5
            }else{
                arr[i][j] = 3
            }
        }
    }

    println("n = $n")
    for(i in 0 until n) {
        for(j in 0 until n) {
            print("%4d".format(arr[i][j]))
        }
        println()
    }
}
```
결과 :
``` 
n = 5
   5   3   3   3   3
   3   5   3   3   3
   3   3   5   3   3
   3   3   3   5   3
   3   3   3   3   5
```

``` kotlin
fun main(args: Array<String>){
    var n = 5
    var arr = Array(n) { Array(n) { 0 } }

    for(i in 0 until n){
        for(j in 0 until n){
            if(i > j) {
                arr[i][j] = 5
            }else{
                arr[i][j] = 1
            }
        }
    }

    println("n = $n")
    for(i in 0 until n) {
        for(j in 0 until n) {
            print("%4d".format(arr[i][j]))
        }
        println()
    }
}
```
결과 :
``` 
n = 5
   1   1   1   1   1
   5   1   1   1   1
   5   5   1   1   1
   5   5   5   1   1
   5   5   5   5   1
```

이것으로 다음 장에서는 몇가지 퀴즈를 풀어보겠습니다. 이제 좀 어려워졌으니 좀더 재미있게 푸실 수 있으리라 생각됩니다.
고민하는 과정을 힘들다 생각하지 마시고, 이렇게 해봐도 틀리고, 저렇게 해봐도 틀리는 과정은 무의미한 과정이 아닌,
모든 과정이 실력이 늘고있는 과정입니다. 용기를 내어서 풀어보시기 바랍니다.
