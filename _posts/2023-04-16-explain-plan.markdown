---
layout: post
title:  "[DB] EXPLAIN PLAN"
date:   2023-04-16 00:00:00 +0530
categories: Javascript
---
### [DB] EXPLAIN PLAN

<br/>

#### EXPLAIN의 정의
- SQL문의 액세스 경로를 확인하고 튜닝할 수 있도록 SQL문을 분석, 해석해 실행계획을 수립.
- SQL문이 어떻게 실행되고 작동하는지를 점검하기 위해 사용.
- **ANALYZE** : 쿼리를 실제 실행한 후 실행한 쿼리 계획을 보여준다.<br/>
언뜻 보기엔 ANALYZE가 더 유용해 보이지만, 쿼리 실행 시 디비에 부하가 가해질 수 있고, 레코드가 늘어나거나 하면 실행 계획 또한 바뀔 수 있으므로 실 서비스에서 실행시와 동일한 정보를 알려주지는 않음.

<br/>

#### EXPLAIN 사용 방법
- 실행 할 쿼리 앞에 EXPLAIN을 붙여 준다.
![](../../../../../var/folders/8p/bbdkvm910vn6dxs8l7d_pqqh0000gn/T/TemporaryItems/NSIRD_screencaptureui_fhIfQD/스크린샷 2023-04-16 오전 9.50.51.png)

<br/>

#### EXPLAIN 항목
##### id
대상 쿼리문에 JOIN 이 포함되어 있을 때, 어떠한 순서로 테이블이 JOIN 되는지를 나타내는 값
<br/><br/>
##### select_type
각 단계를 실행할 때 어떤 종류의 SELECT 가 실행되었는지를 나타냄.
<br/>
- DEPENDENT SUBQUERY: 서브 쿼리가 외부 쿼리의 결과에 의존할때 표현된다.
- DEPENDENT UNION: UNION, UNION ALL 절이 외부 결과에 의존할때 표현된다.
- DERIVED: FROM 절에서 사용되는 서브쿼리를 의미한다. FROM 절에 사용된 서브쿼리(인라인 뷰라고 표현한다.)의 경우 임시 테이블을 생성해야하므로 파생되었다는 의미의 DERIVED가 표현된다.
- MATERIALIZED: MySQL 5.6 버전에 추가된 셀렉트 타입이다. 그 이전의 버전에서는 IN 절 내에 서브쿼리가 존재할 경우 매 레코드마다 서브쿼리를 실행시키는 형태로 수행되었다. 생각만해봐도 비효율적임을 알 수 있다. 5.6 에서부터 추가된 MATERIALIZED는 IN 절 내의 서브쿼리를 임시테이블로 만들어 조인을 하는 형태로 최적화를 해준다.
- PRIMARY: 서브쿼리가 존재할 때 가장 바깥쿼리를 뜻한다.
- SIMPLE: 단순 구문으로 별다른 조인이나 서브쿼리가 없음
- SUBQUERY: FROM 절 외에서 사용되는 서브쿼리를 뜻한다.
- UNCACHEABLE SUBQUERY: 서브쿼리는 종류에 따라 바깥쿼리 행만큼 수행되어야 하는 경우도있다. 실제로 그렇게 작동한다면 성능에 큰 영향을 끼치게되므로 경우에 따라 쿼리를 캐싱해놓고 캐싱된 데이터를 갖다쓰게끔 최적화가 되어있는데 그런 캐싱이 작동할 수 없는 경우에 표현된다. 즉 캐싱되지못하는 이유가 수정 가능하다면 캐싱되게끔 하는것이 성능에 좋다.
- UNCACHEABLE UNION: UNION과 기본적으로 동일하나 공급되는 모든 값에 대해 UNION 쿼리를 재처리
- UNION: UNION, UNION ALL 절에 사용된 쿼리
- UNION RESULT: UNION, UNION ALL 절로 생성된 임시 테이블을 뜻함.
- LATERAL DERIVED: The SELECT uses a Lateral Derived optimization.
<br/><br/>

##### table
해당 단계에서 접근하는 테이블의 이름
<br/><br/>
##### type
테이블 내에서 접근이 필요한 레코드를 어떻게 찾았는지에 대한 정보
<br/>
- ALL: 전체 데이터 block을 스캔
- const: 옵티마이저가 unique/primary key를 사용하여 매칭되는 row가 1건인 경우
- eq_ref: 1:1의 join 관계. unique/primary key를 사용하여 join을 처리함.
- fulltext: 전문 인덱스(Fulltext Index) 를 이용하여 레코드에 접근함을 뜻함.
- index_merge: 동일한 테이블에서 두개 이상의 인덱스가 동시에 사용됨.
- index_subquery: unique_subquery와 비슷하나 결과값이 unique하지 않은 경우
- index: 인덱스를 사용하긴 하나 전체 인덱스 block을 스캔함. 즉 인덱스를 사용하긴 하나 all 타입과 흡사함.
- range: 주어진 범위내의 row를 스캔함. 범위내의 row가 많으면 많을수록 성능이 저하됨.
- ref_or_null: ref와 동일하나 null 값에 대한 최적화가 되어있음.
- ref: 1:n의 join 관계. non-unique 인덱스가 사용되거나, 복합키로 구성된 인덱스 중, 일부 컬럼만 이용하여 조인될 경우
- system: 테이블에 row가 1건이라 매칭되는 row도 1건인 경우
- unique_subquery: 서브쿼리에서 unique한 값이 생성되는 경우. index lookup function이 사용됨.(서브쿼리 최적화)
<br/><br/>

##### possible_keys
레코드에 접근하기 위해 사용할 수 있는 키, 혹은 인덱스 목록
<br/><br/>
##### key
레코드에 접근하기 위해 어떠한 index를 참조하는지, 인덱스 중 몇 바이트를 참조했는지에 대한 정보
<br/><br/>
##### key_len
사용 된 키의 바이트 수(둘 이상의 컬럼으로 구성된 인덱스를 참조했을 경우에만 의미가 있음)
<br/><br/>
##### ref
인덱스 검색 시 비교 연산 등에 사용되는 기준값
<br/><br/>
##### rows
필요한 레코드들을 추려내는 과정에서 몇 개의 레코드에 접근해야 하는지를 예측하여 보여줌
<br/><br/>
##### Extra
이상의 항목 외의 특이 사항들이 있다면 해당 내용을 표시

<br/><br/><br/>
**_개인적인 이해를 바탕으로 작성한 글 입니다. <br/>
잘못된 내용, 피드백은 언제든 환영합니다!_** 🥺🥺🥺
<br/><br/><br/>

### 참고 사이트
<hr>

[MariaDB에서의 쿼리 계획(Query plan) 활용](https://blog.ifunfactory.com/2017/05/29/mariadb%EC%97%90%EC%84%9C%EC%9D%98-%EC%BF%BC%EB%A6%AC-%EA%B3%84%ED%9A%8Dquery-plan-%ED%99%9C%EC%9A%A9/)
<br/>
[mysql 실행계획 1](https://multifrontgarden.tistory.com/149)
<br/>
[SQL EXPLAIN 정리](https://shlee0882.tistory.com/155)
<br/>