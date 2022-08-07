---
title: "[Java] 복사생성자를 이용한 깊은 복사(Deep copy), 얕은 복사(Shallow copy)"
categories:
  - Java
tags:
  - Java
  - OOP
date: 2018-09-30
last_modified_at: 2021-11-03
toc: false
---

개발하다보면 객체를 똑같이 복사해서 사용해야 할 때가 있습니다. 이때 기존 객체와 새로 복사항 객체가 완전히 분리를 시켜야 할때 어떤 것을 신경써야 하는지 알아보겠습니다. 먼저 예제로 복사할 클래스를 만들어 보겠습니다.
``` java
public class Teacher{
    String name;
    int age;
    List<Student> studentList;
}

public class Student{
    String name;
    int age;
}
```

위와 같이 Teacher 클래스와 Student 클래스를 만들고, Teacher 클래스가 Student의 리스트를 멤버로 가진다고 가정하겠습니다. 그럼 우리는 Teacher를 이렇게 복사를 하고 싶습니다.

``` java
Taecher newTeacher = new Teacher(oldTeacher);
```

그래서 클래스에 구현을 합니다.

``` java
public class Teacher{
    String name;
    int age;
    List<Student> studentList;

    public Teacher(){
    }
 
    public Teacher(Teacher src){
        this.name = src.name;
        this.age = src.age;
        this.studentList = src.studentList;
    }
}
```

하지만 위와 같이 구현할 경우 newTeacher를 생성한 뒤, newTeacher의 name을 다른 이름으로 바꿀 경우, oldTeacher의 name도 변경됩니다. 이유는 값복사를 하지 않고, 참조복사를 하였기 때문입니다. 다른말로는 얕은 복사가 되었기 때문입니다.

이 경우는 쉽게 말해서 실제 데이터는 1개뿐이고, 이름표만 2개를 단 경우와 동일합니다. 그렇다면 객체를 완벽하게 독립적으로 복사하려면 어떻게 해야하는지 살펴보겠습니다.

``` java
public class Teacher{
    String name;
    int age;
    List<Student> studentList;

    public Teacher(){
    }
 
    public Teacher(Teacher src){
        this.name = new String(src.name);
        this.age = src.age;
        this.studentList = new ArrayList<>();
        for(int i = 0; i < src.studentList.size(); i++){
            this.studentList.add(new Student(src.studentList.get(i));
        }
    }
}

public class Student{
    String name;
    int age;

    public Student(){
    }

    public Student(Student src){
        this.name = new String(src.name);
        this.age = src.age;
    }
}
```

int, float, double과 같은 기초 변수 이외의 모든 참조변수는 new 연산자를 이용하여 객체를 새로 생성하고, 내용물을 복사하게 만들어야 합니다. 만약 List와 같은 컨테이너를 멤버로 가지고 있다면 item 하나하나 순회하면서 객체를 생성해서 복사해줘야 합니다. 클래스의 복사가 생각보다 단순하지 않습니다. 이렇게 객체를 복사해주는 생성자를 복사생성자(copy constructor)라고 부릅니다.