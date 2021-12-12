# Spring Container에 등록된 Bean 출력해보기
출처 : [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)

~~~java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberrepository());
    }

    @Bean
    public MemoryMemberRepository memberrepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberrepository(), discountPolicy());
    }

    @Bean
    public DiscountPolicy discountPolicy() {
        return new RateDiscountPolicy();
    }

}
~~~


~~~java
    @Test
    @DisplayName("모든 빈 출력하기")
    void findAllBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("name = " + beanDefinitionName + " object = " + bean);
        }
    }
~~~

getBeanDefinition 으로 bean에 대한 metaData 정보들을 조회할 수 있다.

테스트를 실행해보면 

![](./imgs/checkBeanDefinition_1.JPG)

위와 같이 AppConfig 클래스에서 등록한 빈과 스프링FrameWork 내에 기본으로 등록된 bean을 확인할 수 있다.

AppConfig 클래스에서 등록한 빈만 확인하고 싶다면 아래와 같이 BeanDefinition의 Role에 대한 조건식을 추가하여 확인할 수 있다.

~~~java
    @Test
    @DisplayName("애플리케이션 빈 출력하기")
    void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                Object bean = ac.getBean(beanDefinitionName);
                System.out.println("name = " + beanDefinitionName + " object = " + bean);
            }
        }
    }
~~~

출력해보면

![](./imgs/checkBeanDefinition_2.JPG)