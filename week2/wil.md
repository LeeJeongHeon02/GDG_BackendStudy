![image](https://github.com/user-attachments/assets/e01d49c8-43f5-4b07-a79b-e77973de42a6)

## 어떤걸 배우나?

- **스프링 빈**과 **스프링 컨테이너**의 개념
- 스프링 컨테이너에 빈을 저장하는 방법
- 스프링 컨테이너에서 빈을 받아오는 방법

## 스프링

JAVA진영의 대표적인 백엔드 프레임워크

객체지향 원칙을 지키면서 개발할 수 있게 도와줌.

## 스프링 부트

스프링 프레임워크를 사용하여 개발할 떄, 편의를 더해주는 **도구**

스프링으로 개발할 떄는 스프링 부트를 함께 사용한다.

## 스프링 어플리케이션 구조

![image.png](attachment:4dc406a1-d831-4949-86e7-fd7e6d4cf3c4:image.png)

## 스프링 빈(Spring Bean)

: 어플리케이션 전역에서 사용할 공용 **객체이다.**

- 스프링 컨테이너라고 하는 공용 창고에 빈(객체)을 저장해두고, 필요한 빈을 컨테이너에서 받아 사용한다.
- 필요한 빈은 스프링 프레임워크가 자동으로 가져다 준다.
- 이떄 빈을 요구하는 객체도 스프링 빈이다.
    - (빈이 아닌 객체가 빈을 요구해도 프레임워크가 자동으로 가져다주지는 못한다.)

## 스프링 컨테이너

: 스프링 빈이 저장되는 공간, 어필리케이션 컨텍스트(Application Context)라고도 한다.

### 스프링 빈을 컨테이너에 저장하는 방법

1. 설정 파일 작성 (수동 등록)
2. 컴포넌트 스캔 (자동 등록)

## 1. 설정 파일 작성

- 설정 파일은 자바 클래스로 작성한다.
- 이때 클래스에 @Configuration으로 설정 파일임을 명시한다.
- 클래스에 @Configuration, 메서드에 @Bean 어노테이션을 사용하면 된다

```java
ApplicationContext context = new AnnotationConfigApplicationContext(TestConfig.class);
```

위 코드를 사용하면 스프링 컨테이너를 생성할 수 있다.

(참고) ApplicationContext는 인터페이스이고, AnnotationConfigApplicationContext는 하위 구현체 클래스이다.

- 컨테이너 안에 등록된 모든 빈 조회하기

```java
//J유닛, 테스트용 라이브러리
    @Test
    public void getAllBeanTest() {
    
        // 스프링 컨테이너를 설정 파일 정보를 이용해서 생성하고, 스프링 컨테이너 안에 있는 모든 칸을 조회하는 테스트
				for (String name : context.getBeanDefinitionNames()) {
            System.out.println(name);
        }
        Assertions.assertThat(context.getBeanDefinitionNames()).contains("myBean");
    }
```

- 스프링 빈은 기본적으로 1개의 객체이므로 컨테이너에서 빈을 가져올 때마다 같은 객체가 반환된다.

```java
@Test
    public void getBeanTest() {
        MyBean myBean1 = context.getBean(MyBean.class);
        MyBean myBean2 = context.getBean(MyBean.class);

        System.out.println(myBean1);
        System.out.println(myBean2);

//        MyBean myBean3 = new MyBean();

        Assertions.assertThat(myBean1).isSameAs(myBean2);
        //같은 객체가 반환됨을 확인할 수 있다.
    }
```

## 2. 컨포넌트 스캔

- 빈을 생성할 클래스에 @Component 어노테이션을 사용한다.
- 그러면 @Component 어노테이션이 붙은 클래스를 찾아서 자동으로 빈 등록한다.
- 컴포넌트 스캔은 @ComponentScan 어노테이션 사용한다.

# 의존성 주입

: 내가 의존하는 객체를 직접 생성하지 않고 밖에서 주입 받는 것

- 스프링에서는 컨테이너에 저장된 빈(객체)과 빈(객체)사이의 의존성을
프레임워크가 주입하는 것을 말한다.

## 의존성 주입 방법

1. 생성자 주입
2. 필드 주입
3. (거의사용안함) Setter주입(메서드주입)

### 생성자 주입

- 의존성이 바뀔 일이 없는 경우 안전하게 final로 선언한다.
- 이때 final 필드는 생성자를 통해 초기화되어야 한다
- 생성자에 @Autowired 을 사용하면, 생성자를 통해 빈을 주입한다.(만약 생성자가 하나만 있다면, @Autowired를 생략할 수 있다)
- @RequiredArgsConstructor를 사용하면 모든 final 필드에 대한 생성자를 자동으로 만들어주어 생성자 코드까지 생략할 수 있다.

### 필드 주입

- 필드에 바로 @Autowired 어노테이션을 사용한다. (final은 사용 불가)
    - 이 방식은 주로 테스트 코드에서 사용한다.
    (운영 코드에서 사용하면 IDE에서 경고를 띄운다.)
- 테스트에서 필드 주입을 하려면, 테스트를 실행할 때
이미 스프링 컨테이너가 존재해야 한다.

```java
@SpringBootTest
public class BeenTest2 {

    @Autowired
    private MyBean myBean;
    
    @Autowired
    private MySubBean mySubBean;
    }
}
```

- 클래스에 @SpringBootTest 를 사용하면 어플리케이션에 있는 모든 빈을 컨테이너에 등록한 후 테스트한다.

# **Spring Layered Architecture**

## **[Client] → Controller → Service → Repository → [DB]**

## **컨트롤러**

- 클라이언트의 요청을 받고, 응답을 보내는 계층
- DTO(Data Transfer Object, Controller ↔ Service 사이에 데이터를 주고받을 때 사용하는 전용 객체)
를 사용하여 서비스 계층과 데이터를 주고받음.

## 서비스

- 어플리케이션의 비즈니스 로직이 담기는 계층
- 레포지토리 계층과 소통하며 엔티티, 또는 DTO로 소통한다.

## 레포지토리

- DB와 소통하며 데이터를 조작하는 계층
- 서비스 계층이 결정한 비즈니스 로직을 실제 DB에 적용한다

## 스프링 빈 활용

- 컨트롤러, 서비스, 레포지토리는 스프링 빈으로 등록한다.
- 매번 새로운 객체를 생성할 필요가 없고, 객체지향 원칙을 준수하며 의존성을 관리할 수 있기 때문이다.
