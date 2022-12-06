## Composite Key

복합 키는 두 개 이상의 컬럼을 Key로 지정하는 것을 말한다.

PK는 한 테이블에 한 개만 존재할 수 있다,

하지만 꼭 한 테이블에 한 컬럼만 기본키로 지정할 수 있는 것은 아니다.

아래와 같이 `PRIMARY KEY`를 두 컬럼에 지정하면 에러가 발생한다.

```sql
mysql> create table test (
    -> id bigint primary key,
    -> seq bigint primary key);
ERROR 1068 (42000): Multiple primary key defined
```

하지만 `PRIMARY KEY(column1, column2)` 이런식으로 기본키를 여러 컬럼을 조합하여 설정하는 것은 가능하다.

```sql
mysql> create table test (
    -> id bigint not null,
    -> seq bigint not null,
    -> primary key(id, seq)
    -> );
Query OK, 0 rows affected (0.01 sec)
```

이렇게 컬럼을 조합하여 기본키로 설정할 경우에는 여러 컬럼을 모두 조합해서 UNIQUE해야 한다.

아래와 같이 id와 seq컬럼에 모두 1의 값을 넣고 저장한 뒤 다시 한 번 동일하게 저장하려고 하면 에러가 발생한다. id는 동일하게 1이고, seq의 값만 2로 저장하는 것은 성공한다.

```sql
mysql> insert into test (id, seq)
    -> values(1, 1);
Query OK, 1 row affected (0.00 sec)

mysql> insert into test (id, seq) values(1, 1);
ERROR 1062 (23000): Duplicate entry '1-1' for key 'test.PRIMARY'

mysql> insert into test (id, seq)
    -> values(1, 2);
Query OK, 1 row affected (0.00 sec)
```

인덱스로 설정하는 필드의 속성이 중요하다. title, author 이 순서로 인덱스를 설정한다면 title을 search 하는 경우, index 를 생성한 효과를 볼 수 있지만, author 만으로 search 하는 경우, index 를 생성한 것이 소용이 없어진다. 따라서 SELECT 질의를 어떻게 할 것인가가 인덱스를 어떻게 생성할 것인가에 대해 많은 영향을 끼치게 된다.
