## LAG, LEAD 분석함수 사용법
LAG는 자신의 이전 값을, LEAD는 자신의 이후 값을 가져오는 분석함수이다.

사용법은 아래와 같다.
- LAG(컬럼명, offset) OVER([PARTITION BY ~] ORDER BY ~)
- LEAD(컬럼명, offset) OVER([PARTITION BY ~] ORDER BY ~)
- offset: 현재 로우에서 몇 로우 이전 또는 몇 로우 이후를 뜻한다.

```sql
SELECT  T1.CUS_ID 
        ,SUM(T1.ORD_AMT) ORD_AMT
        ,ROW_NUMBER() OVER(ORDER BY SUM(T1.ORD_AMT) DESC) RNK
        ,LAG(T1.CUS_ID,1) OVER(ORDER BY SUM(T1.ORD_AMT) DESC) LAG_1
        ,LEAD(T1.CUS_ID,1) OVER(ORDER BY SUM(T1.ORD_AMT) DESC) LEAD_1
FROM    T_ORD T1
WHERE   T1.ORD_DT >= TO_DATE('20170301','YYYYMMDD')
AND     T1.ORD_DT < TO_DATE('20170401','YYYYMMDD')
AND     T1.CUS_ID IN ('CUS_0020','CUS_0021','CUS_0022','CUS_0023')
GROUP BY T1.CUS_ID;
```
<img src="/picture/그림7.png" />

#### 주문년월 별 주문금액에, 전월 주문금액을 같이 표시하기
아래 SQL을 활용하여 이전 대비 현재 월의 증가율을 손쉽게 구할 수 있다.
```sql
SELECT  TO_CHAR(T1.ORD_DT,'YYYYMM') ORD_YM
        ,SUM(T1.ORD_AMT) ORD_AMT
        ,LAG(SUM(T1.ORD_AMT), 1) OVER(ORDER BY TO_CHAR(T1.ORD_DT,'YYYYMM') ASC) BF_YM_ORD_AMT
FROM    T_ORD T1
WHERE   T1.ORD_ST = 'COMP'
GROUP BY TO_CHAR(T1.ORD_DT,'YYYYMM');
```
<img src="/picture/그림8.png" height="50%"/>