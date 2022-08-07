---
title: "[강의] 코딩 훈련 교본 (코틀린편) - 02. Hello world 프로젝트"
categories:
  - Kotlin
  - Training
tags:
  - Kotlin
date: 2021-11-07
last_modified_at: 2021-11-07
toc: false
---
> 본 강의는 코틀린(Kotlin)의 문법강의가 아닌, 프로그래밍 훈련을 위한 강의입니다.
> 따라서, 본 강의에서 알려드리는 내용만으로 코딩훈련에 임하시기 바랍니다.

처음으로 콘솔창에 "Hello world"를 출력하는 프로그램을 만들어보겠습니다.

**IntelliJ IDEA**를 실행하면 아래와 같은 창이 뜹니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/1.png){: .align-center}

새로운 프로젝트를 시작하기 위해 **[New Project]**를 클릭하면 아래 창으로 넘어갑니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/2.png){: .align-center}

여기서 **[Project SDK]** 를 선택합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/3.png){: .align-center}

**[Download JDK...]** 버튼을 클릭합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/4.png){: .align-center}

아래 그림처럼 **[Azul zulu Community]** 를 선택해줍니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/5.png){: .align-center}

**[Download]** 버튼을 클릭하면 아래와 같이 다운로드가 시작됩니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/6.png){: .align-center}

설치를 마친 뒤, 다음 그림과 같이 생성하고자 하는 프로젝트를 선택합니다.

**[Java] -> [Kotlin/JVM]** 을 선택하고, **[Next]** 버튼을 클릭합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/7.png){: .align-center}

프로젝트 이름은 **"Hello"**로 입력하고, **[Finish]** 버튼을 클릭합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/8.png){: .align-center}

아래 그림처럼 프로젝트가 열리면 왼쪽의 **"src"** 폴더에서 우클릭을 합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/9.png){: .align-center}

우클릭을 한 뒤, 메뉴가 나타나면 **[New] -> [File]**을 선택합니다. 파일 이름은 **"Hello.kt"**로 입력하여 파일을 생성해줍니다.

그리고 다음 소스를 입력해줍니다.

``` kotlin
fun main(args: Array<String>) {
    println("Hello world")
}
```

코드를 입력한 모습은 아래와 같아야 합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/10.png){: .align-center}

코드를 입력하면 **"fun main(...."** 라인 옆에 세모버튼이 있습니다. 이것을 클릭하면 아래와 같은 메뉴가 나타납니다. 여기서 **[Run 'HelloKt']**버튼을 클릭합니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/11.png){: .align-center}

드디어, 아래와 같이 **"Hello world"** 문구가 출력된 것을 확인할 수 있습니다.

![image](/assets/images/2021-11-07-kotlin-02-helloworld/12.png){: .align-center}

이제까지 처음 시작하시는 분들을 위해서 진행단계별로 상세하게 설명을 드렸습니다. 다음장부터 본격적으로 프로그래밍을 배워보도록 하겠습니다.