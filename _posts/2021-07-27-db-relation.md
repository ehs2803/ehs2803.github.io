---
title:  "[DB/SQLD] 3층 스키마, 엔터티, 속성 "
date: 2021-07-27 10:26:00
categories:
  - 데이터베이스
  - SQLD 
tags:
  - 데이터베이스
  - 데이터베이스시스템
  - 스키마
  - 엔터티
  - 속성 
---

# 관계
관계는 엔터티 간의 관련성을 의미하며 존재관계와 행위관계로 분류된다.
존재관계는 두 개의 엔터티가 존재 여부의 관계가 있는 것이고, 행위관계는 두 개의 엔터티가 어떤 행위에 의한 관련성이 있는 것이다.

<br>
<br>

# 관계의 종류

1. 존재 관계
엔터티 간의 상태를 의미.
예를 들어 한 학생이 대학교에 입학하면 멘토 교수가 할당되고 그 할당된 멘토 교수가 학생을 관리한다.

2. 행위 관계
엔터티 간에 어떤 행위가 있는 것이다.
예로들어 학생과 멘토교수간에 상담을 하고 그것이 기록된다면 그것은 행위 관계라고 할 수 있다.

<br>
<br>

# 관계 차수(Cardinality)
관계차수는 두개의 엔터티 간에 관계에 참여하는 수를 의미한다.
예를 들어 한 교수가 여러 학생을 멘토할 수 있다. 이러한 경우에 1대N관계가 된다.

<br>
<br>
# 관계 차수 종류

1. 1대1 관계
1대1 관계는 완전 1대1 관계와 선택적 1대1 관계가 있다.
완전1대1 관계는 하나의 엔터티에 관계되는 엔터티의 관계가 하나인 경우로 반드시 존재한다.
선택적1대1 관계는 하나의 엔터티에 관계대는 엔터티의 관계가 하나이거나 없을 수도 있다.

2. 1대N 관계
엔터티에 행이 하나 있을 때 다른 엔터티의 값이 여러개 있는 관계이다.
예를 들어 교수는 여러명의 학생을 멘토할 수 있다.

3. M대N 관계
두개 엔터티가 서로 여러개의 관계를 가지고 있다.
예로 들어 한 명의 학생이 여러개의 과목을 수강할 수 있고, 반대로 한개의 과목은 여러명의 학생이 수강한다. 
관계형 데이터베이스에서 M대N 관계의 조인은 카테시안 곱이 발생한다. 그래서 M대N관계를 1대N, N대1로 해소해야 한다.

<br>
<br>

# 필수적 관계와 선택적 관계
필수적 관계는 반드시 하나는 존재해야 하는 관계이고 선택적 관계는 없을 수도 있는 관계이다.
필수적 관계는 "|"로 표현되고 선택적 관계는 "O"로 표현된다.

<br>
<br>

# 식별 관계와 비식별 관계

강한개체(Strong Entity) : 누구에게도 지배되지 않는 독립적인 개체.
약한개체(Weak Entity) : 개체의 존재가 다른 개체의 존재에 달려 있는 개체.

1. 식별관계(Identification Relationship)
- 고객과 계좌 엔터테에서 고객은 독립적으로 존재할 수 있는 강한개체(String Entity)이다.
- 강한개체는 어떤 다른 엔터티에게 의존하지 않고 독립적으로 존재한다.
- 강한개체는 다른 엔터티와 관계를 가질 때 다른 엔터티에게 기본키를 공유한다.
- 강한 개체는 식별관계로 표현
- 식별관계란 고객 엔터티의 기본키인 회원ID를 계좌 엔터티의 기본키의 하나로 공유한다.
- 강한 개체의 기본키 값이 변경되면 식별관계(기본키를 공유받는)에 있는 엔터티의 값도 변경된다.
- 여기서 계좌 엔터티는 약한 개체가 된다.
- 식별관계는 실선으로 표현된다.

2. 비식별 관계(Non-Identification Relationship)
- 비식별 관계는 강한 개체의 기본키를 다른 엔터티의 기본키가 아닌 일반 칼럼으로 관계를 가지는 것이다.
- 비식별 관계는 점선으로 표현된다.

<br>
<br>

# 엔터티 식별자(Entity Identifier)

식별자는 엔터티를 대표할 수 있는 유일성을 만족하는 속성이다.

**키의 종류**
1. 기본키(Primary key) : 후보키 중에서 엔터티를 대표할 수 있는 키.
2. 후보키(Candidate key) : 후보키는 유일성과 최소성을 만족하는 키.
3. 슈퍼키(Super key) : 슈퍼키는 유일성은 만족하지만 최소성을 만족하지 않는 키.
4. 대체키(Alternate key) : 대체키는 여러 후보키 중에서 기본키를 선정하고 남은 키.
5. 외래키(Foreign key) : 하나 혹은 다수의 다른 테이블의 기본 키 필드를 가리키는 것으로 참조무결성(Referential Integrity)을 확인하기 위해서 사용되는 키.

**주식별자(기본키, primary key)**
- 최소성 : 최소성을 만족한다.
- 대표성 : 엔터티를 대표할 수 있다.
- 유일성 : 엔터티의 인스턴스를 유일하게 식별할 수 있다.
- 불변성 : 자주 변경되지 않아야 된다.

<br>
<br>

# 식별자의 종류
식별자는 대표성, 생성여부, 속성의 수, 대체 여부로 분류된다.

#### 식별자의 대표성
1. 주식별자 : 유일성과 최소성을 만족하면서 엔터티를 만족하는 식별자로 다른 엔터티와 참조 관계로 연결될 수 있다.
2. 보조식별자 : 유일성과 최소성은 만족하지만 대표성을 만족하지 못하는 식별자.

#### 생성 여부
1. 내부 식별자 : 엔터티 내부에서 스스로 생성되는 식별자.
2. 외부 식별자 : 다른 엔터티와의 관계로 인하여 만들어지는 식별자.

#### 속성의 수
1. 단일 식별자 : 하나의 속성으로 구성된다.
2. 복합 식별자 : 두 개 이상의 속성으로 구성된다.

#### 대체 여부
1. 본질 식별자 : 비즈니스 프로세스에서 만들어지는 식별자.
2. 인조 식별자 : 인위적으로 만들어지는 식별자이다. 인조 식별자는 후보 식별자 중에서 주식별자로 선정할 것이 없거나 주식별자가 너무 많은 칼럼으로 되어 있는 경우에 사용한다. 즉 순서번호를 사용해서 식별자를 만드는 것이다.