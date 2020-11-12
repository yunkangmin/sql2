####* 출처 : SQL Booster
## 기본세팅

#### 1. 연습용 테이블 스페이스 만들기
- SYS 계정으로 접속해 아래 명령어들을 실행한다.
```sql
--'ora_sql_test_ts'라는 테이블 스페이스를 생성한다.
create tablespace ora_sql_test_ts datafile 'c:\ora_sql_test\ora_sql_test.dba' size 10g
extent management local segment space management auto;
```

#### 2. 연습용 사용자 만들기
```sql
create user ora_sql_test identified by "1qaz2wsx" default tablespace ora_sql_test_ts;
```
- 새로운 사용자가 로그인 할 수 있도록 계정 UNLOCK과 접속 및 리소스 권한을 부여
```sql
alter user ora_sql_test account unlock;
grant connect, resource to ora_sql_test;
```
위 sql까지 실행하면 'ORA_SQL_TEST' 사용자로 접속할 수 있다. 

- 추가로 성능관련 내용을 다루기 위해 아래 권한을 추가한다.

```sql
grant alter system to ora_sql_test;
grant select on v_$sql to ora_sql_test;
grant select on v_$sql_plan_statistics_all to ora_sql_test;
grant select on v_$sql_plan to ora_sql_test;
grant select on v_$session to ora_sql_test;
grant execute on dbms_stats to ora_sql_test;
grant select on dba_segments to ora_sql_test;
```

- 대량의 테스트 데이터를 생성할 때 TEMP 사이즈가 부족하면 에러가 날 수 있기 때문에 아래와 같이 TEMP 사이즈를 확인하고 부족하면 늘린다.

```sql
select t1.file_name, (t1.bytes / 1024 / 1024) tmp_mb from dba_temp_files t1;

alter database tempfile 'c:\app\yun\oradata\orcl\temp01.dbf' resize 5000m;
```

#### 3. 연습용 테이블 생성하기
- 'ORA_SQL_TEST' 계정으로 접속해 "sample data/SQLBooster_C1_02_테이블생성.sql"을 실행하여 관련 테이블을 생성한다.
- 테이블 ERD
<img src="picture/테이블 ERD.gif" width="100%" height="100%"/>

#### 4. 연습용 데이터 생성하기 
- "sample data/SQLBooster_C1_03_데이터생성.sql"을 실행하여 샘플 데이터를 생성한다.
