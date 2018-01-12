# 数据库
```sql
-- 查看安徽省下面的所有 '市编码'
select UPCNAME as 省名, TS_UP_BM as 省编号, CNAME as 市名, TS_BM as 市编号
  from TB_SYSTEM_CITY 
 where UPCNAME = '安徽省'  -- 安徽省（600301）  合肥市(8340100)

-- 查看安徽省的所有信息
select * from TB_SYSTEM_CITY where TS_BM = '600301' --and TS_UP_BM = '8340100'
select * from TB_SYSTEM_CITY where TS_BM = '600301'  and '8340100'

-- 安徽省
SELECT --KD.TS_UP_BM as 省编号,
       KD.CITY_CODE   as 市编号,
       KD.PRODUCT_NBR as 产品Nbr,
       KD.PRICE       as 金额,
       MK.TS_MK_TITLE as 描述
  FROM GMS_FZY_KDXY KD, TS_MK_INFO12500 MK
where KD.CITY_CODE = '8340100'

select *   FROM GMS_FZY_KDXY KD, TS_MK_INFO12500 MK

-- python 代码中需要的desc
SELECT MK.TS_MK_TITLE
  FROM TS_MK_INFO12500 MK, GMS_FZY_KDXY KD
 WHERE KD.CITY_CODE = '8430600'
   AND KD.PRODUCT_NBR = '7234314310200820'
   and MK.TS_MK_ID = KD.GOODS_ID;

-- python 代码中需要的desc的python代码
sql = "MK.TS_MK_TITLE FROM TS_MK_INFO12500 MK, GMS_FZY_KDXY KD WHERE KD.CITY_CODE =  '%s' AND KD.PRODUCT_NBR =  '%s' and TS_MK_ID = GOODS_ID"  % (city_code, prodNbr)

-- 根据市编码和产品Nbr获取产品Nbr的 price 
 SELECT KD.PRICE
  FROM GMS_FZY_KDXY KD
 WHERE KD.CITY_CODE = '8340100'
   AND KD.PRODUCT_NBR = '7234314310200796'; 

--  python 代码中需要的price  
sql = "SELECT KD.PRICE FROM GMS_FZY_KDXY KD WHERE KD.CITY_CODE = '%s' AND KD.PRODUCT_NBR = '%s'" % (city_code, prodNbr)

-- 安徽省（600301）  合肥市(8340100)
insert into GMS_FZY_KDXY KD
  (ID,
   PROVINCE_CODE,
   PRODUCT_NBR,
   PRODUCT_CODE,
   GOODS_ID,
   GOODS_CODE,
   CITY_CODE,
   PRICE)
values
  ('1233210123',
   '600301',
   '7234314310200796',
   '72343',
   '000000005CE6F3FB5C46788EE053AC1410ACAE3B',
   'C5568',
   '8340100',
   100.00);
    
-- 查看上面被插入的语句     
select * from GMS_FZY_KDXY where id='1233210123'
   
-- （根据市和产品nbr）查看安徽省合肥市的desc,商品ID,市编码,省编码   
SELECT MK.TS_MK_TITLE, KD.Goods_Id, KD.CITY_CODE, KD.PRODUCT_NBR
  FROM TS_MK_INFO12500 MK, GMS_FZY_KDXY KD
 WHERE KD.CITY_CODE = '8340100'
   AND KD.PRODUCT_NBR = '7234314310200796'
   and MK.TS_MK_ID = KD.GOODS_ID;

-- 向表插入一个字段
insert into GMS_FZY_KDXY(Goods_Id) values('12345678909876655')

-- 根据商品ID查询表 
select * from GMS_FZY_KDXY where Goods_Id='12345678909876655'
 
-- python代码 GoodsId
sql = "SELECT KD.Goods_Id FROM TS_MK_INFO12500 MK, GMS_FZY_KDXY KD WHERE KD.CITY_CODE =  '%s' AND KD.PRODUCT_NBR =  '%s' and TS_MK_ID = GOODS_ID"  % (city_code, prodNbr) 

-- 存储过程(校验地区编码是否合法，合法返回0，否则返回1)
SP_DEAL_VERIFY_AREA_CODE

-- LatnID（本地网）
select t.tb_system_qh from tb_system_city t where t.ts_up_bm='600301' and t.ts_bm='8340100'

SP_DEAL_VERIFY_AREA_CODE    -- 600301  8340600
```

-- 存储过程(校验地区编码是否合法，合法返回0，否则返回1)
SP_DEAL_VERIFY_AREA_CODE
```sql
CREATE OR REPLACE PROCEDURE SP_DEAL_VERIFY_AREA_CODE(
    PROV_CODE VARCHAR2, 
    CITY_CODE VARCHAR2,
    IS_OK     OUT VARCHAR2) AS
    V_PROV_COUNT NUMBER;
    V_CITY_COUNT NUMBER;
BEGIN
  /*
  *************************************************
  存储过程名   : SP_DEALVERIFY_AREA_CODE
  建立日期     ：2017-11-24
  作者         ：ZHOUZQ
  模块         ：宽带能力接入协议
  功能描述     ：校验地区编码是否合法，合法返回0，否则返回1
  
  *************************************************
  */
  IS_OK        := '0';
  V_PROV_COUNT := 0;
  V_CITY_COUNT := 0;
  
  IF PROV_CODE = '600103' THEN
    SELECT COUNT(T2.UPCNAME), COUNT(T2.CNAME)
      INTO V_PROV_COUNT, V_CITY_COUNT
      FROM TB_SYSTEM_CITY T2
     WHERE T2.TS_UP_BM = PROV_CODE
       AND T2.TB_SYSTEM_QH = CITY_CODE
       AND ROWNUM < 2;
  ELSE

  SELECT COUNT(T2.UPCNAME), COUNT(T2.CNAME)
    INTO V_PROV_COUNT, V_CITY_COUNT
    FROM TB_SYSTEM_CITY T2
   WHERE T2.TS_UP_BM = PROV_CODE
     AND T2.TS_BM = CITY_CODE
     AND ROWNUM < 2;
  IF (V_PROV_COUNT = 0) OR (V_CITY_COUNT = 0) THEN
    IS_OK := '1';
  END IF;
END IF;
END;

```