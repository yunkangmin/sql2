## GROUPING SETS를 사용하여 ROLLUP 대신하기
```sql
SELECT  TO_CHAR(T1.ORD_DT,'YYYYMM') ORD_YM
        ,T1.CUS_ID
        ,COUNT(*) ORD_CNT
        ,SUM(T1.ORD_AMT) ORD_AMT
FROM    T_ORD T1
WHERE   T1.ORD_DT >= TO_DATE('20170301','YYYYMMDD')
AND     T1.ORD_DT < TO_DATE('20170501','YYYYMMDD')
AND     T1.CUS_ID IN ('CUS_0061','CUS_0062')
GROUP BY GROUPING SETS(
            (TO_CHAR(T1.ORD_DT,'YYYYMM'),T1.CUS_ID)  --GROUP BY기본 데이터
            ,(TO_CHAR(T1.ORD_DT,'YYYYMM'))  --주문년월별 소계
            ,(T1.CUS_ID)  --고객ID별 소계
            ,()   --전체합계
        );
```
<img src="picture/그림69.png" />