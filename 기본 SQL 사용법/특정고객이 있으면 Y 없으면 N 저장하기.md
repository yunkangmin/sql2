## 특정고객이 있으면 Y 없으면 N 저장하기
테스트를 위해 아래와 같이 테이블을 생성한다.

```sql
CREATE TABLE M_CUS_CUD_TEST AS
SELECT  *
FROM    M_CUS T1;

ALTER TABLE M_CUS_CUD_TEST
    ADD CONSTRAINT PK_M_CUS_CUD_TEST PRIMARY KEY(CUS_ID) USING INDEX;

SELECT  NVL(MAX('Y'),'N')
    --INTO    v_EXISTS_YN
FROM    DUAL A
WHERE   EXISTS(
            SELECT  *
            FROM    M_CUS_CUD_TEST T1
            WHERE   T1.CUS_ID = 'CUS_0090'
            );
```
<img src="/picture/그림63.png" />