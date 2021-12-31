# 영속성 전이(CASCADE) 와 고아(ORPHAN) 객체

## 1. 영속성 전이(CASCADE)
* 특정 엔티티를 영속 상태로 만들때 관련된 엔티티도 함께 영속 상태로 만들고 싶을 때 사용
  예) 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장.   

    ![](imgs/cascade-and-orphan/2022-01-01-02-01-36.png)

<br>

### 1) 저장

```java
@OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
private List<Child> children = new ArrayList<>();
```

![](imgs/cascade-and-orphan/2022-01-01-02-03-15.png)


```java
Child child1 = new Child();
Child child2 = new Child();

Parent parent = new Parent();
parent.addChild(child1);
parent.addChild(child2);

em.persist(parent);
// em.persist(child1);
// em.persist(child2);

tx.commit();
```

cascade 를 Parent 엔티티에 설정 후 Parent 엔티티만 persist 해도 Child 엔티티가 같이 저장되는 것을 확인할 수 있다.

![](imgs/cascade-and-orphan/2022-01-01-02-04-30.png)

<br>

### 2) 영속성 전이: CASCADE - 주의!

* 영속성 전이는 연관관계를 매핑하는 것과 아무 관련이 없음
* 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 편리함을 제공할 뿐
* **다른 엔티티에서도 cascade에 대상이 되는 객체를 연관관계 매핑을 통하여 사용할때는 cascade를 사용하면 안 된다.**

<br>

### 3) CASCADE의 종류

* ALL: 모두 적용
* PERSIST: 영속
* REMOVE: 삭제
* MERGE: 병합
* REFRESH: REFRESH
* DETACH: DETACH


<br><br>


## 2. 고아(ORPHAN) 객체


```java:Parent.java
/* Parent.java */

@OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
private List<Child> childList = new ArrayList<>();
```

```java
/* JpaMain.java */

Child child1 = new Child();
Child child2 = new Child();

Parent parent = new Parent();
parent.addChild(child1);
parent.addChild(child2);

em.persist(parent);
// em.persist(child1);
// em.persist(child2);

em.flush();
em.clear();

Parent findParent = em.find(Parent.class, parent.getId());
findParent.getChildList().remove(0);
```