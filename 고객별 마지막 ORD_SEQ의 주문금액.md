## 고객별 마지막 ORD_SEQ의 주문금액
ORD_SEQ(마지막 주문번호)와 주문금액을 합친뒤 MAX를 하면 마지막 주문번호와 주문금액이 합쳐진 문자열이 나오고 SUBSTR함수로 금액만 자른 뒤 이를 숫자로 변환한뒤 출력한다.

**하지만 아래 SQL은 성능에 안 좋을 수 있다. 고객의 모든 주문을 읽기 때문이다.**

```sql
SELECT  T1.CUS_ID
        ,T1.CUS_NM
        ,(SELECT  TO_NUMBER(
                    SUBSTR(
                        MAX(
                            LPAD(TO_CHAR(A.ORD_SEQ),8,'0')
                            ||TO_CHAR(A.ORD_AMT)
                            ),9))
                    FROM T_ORD A WHERE A.CUS_ID = T1.CUS_ID) LAST_ORD_AMT
FROM    M_CUS T1
ORDER BY T1.CUS_ID;
```
아래와 같이 처리할 수도 있다. 이 또한 성능에 좋지 못하다.
```sql
SELECT  T1.CUS_ID
        ,T1.CUS_NM
        ,(
            SELECT  B.ORD_AMT
            FROM    T_ORD B 
            WHERE   B.ORD_SEQ = 
                        (SELECT MAX(A.ORD_SEQ) FROM T_ORD A WHERE A.CUS_ID = T1.CUS_ID)
            ) LAST_ORD_AMT
FROM    M_CUS T1
ORDER BY T1.CUS_ID;
```

#### 마지막 주문 한 건 조회하기

```sql
SELECT  *
FROM    T_ORD T1
WHERE   T1.ORD_SEQ = (SELECT MAX(A.ORD_SEQ) FROM T_ORD A);

--아래 SQL이 성능면에서 유리하다. 
SELECT  *
FROM    (
        SELECT  *
        FROM    T_ORD T1
        ORDER BY T1.ORD_SEQ DESC
        ) A
WHERE  ROWNUM <= 1;

```