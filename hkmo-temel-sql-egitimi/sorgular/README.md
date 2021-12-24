# CREATE

```sql

CREATE TABLE users (
	id INTEGER,
	firstname VARCHAR2(30) NOT NULL,
	lastname VARCHAR2(30) NOT NULL,
	email VARCHAR2(50)
);
```

# ALTER
```sql
ALTER TABLE users ADD phone VARCHAR(11);
```

# DROP
```sql
DROP TABLE users;
```  
  
# INSERT

```sql
INSERT INTO users (firstname, lastname, email) 
	VALUES ('Barış', 'Arıburnu', 'baris.ariburnu@sqltest.net',);

INSERT INTO users (firstname, lastname, email) 
	VALUES ('Görkem Ege', 'Arıburnu', 'gege.ariburnu@sqltest.net');
```

# INSERT  INTO

```sql
INSERT INTO departments (dept_id, dept_name) 
	VALUES (6, "CBS");

INSERT INTO employees 
	VALUES (11, "Barış", 2016-02-15, 1850, 6);

INSERT INTO employees (emp_id, emp_name, hire_date, salary) 
	VALUES (12, "Görkem Ege", 2020-04-15, 10000);
```

  

# UPDATE

```sql
UPDATE employees SET dept_id = 2 WHERE emp_id = 12;
UPDATE employees SET dept_id = 1;
```

# DELETE
```sql
DELETE FROM employees WHERE emp_id = 10;
DELETE FROM employees;
```

# DISTINCT

```sql
SELECT DISTINCT dept_id FROM employees;
```

# WHERE
```sql
SELECT * FROM employees WHERE salary > 5000;
```

# AND
```sql
SELECT * FROM employees WHERE salary > 6000 and dept_id = 1;
```

# OR
```sql
SELECT * FROM employees WHERE salary < 5000 or salary > 7000;
```

# NOT
```sql
SELECT * FROM employees WHERE dept_id != "null";
SELECT * FROM employees WHERE NOT dept_id = "null";
```

# ORDER BY
```sql
SELECT * FROM employees ORDER BY salary;
SELECT * FROM employees ORDER BY salary ASC;
SELECT * FROM employees ORDER BY salary DESC;
```

# NULL
```sql
SELECT * FROM employees WHERE dept_id IS NOT NULL;
SELECT * FROM employees WHERE dept_id IS NULL;
```

# SELECT  TOP
```sql
SELECT TOP 1 * FROM employees; -- SQL Server / MS Access
SELECT * FROM employees LIMIT 1; -- MySQL
SELECT * FROM employees FETCH FIRST 1 ROWS ONLY; -- Oracle
```

# MIN
```sql
SELECT MIN(salary) FROM employees;
```

# MAX
```sql
SELECT MAX(salary) FROM employees;
```

# AVG
```sql
SELECT AVG(salary) FROM employees;
```

# SUM

```sql
SELECT SUM(salary) FROM employees;
```

# COUNT
```sql
SELECT COUNT(1) FROM employees;
```

# LIKE
```sql
SELECT * FROM suppliers WHERE city LIKE 'M%';
SELECT * FROM suppliers WHERE city LIKE '%E';
SELECT * FROM suppliers WHERE city LIKE 'M_L%E';
```

# IN
```sql
SELECT * FROM suppliers WHERE city IN ('Manchester', 'Melbourne');
```

# BETWEEN
```sql
SELECT * FROM employees WHERE salary BETWEEN 4000 and 5000;
```

# ALIASES
```sql
SELECT e.emp_name FROM employees AS e;
```

# INNER JOIN
```sql
SELECT e.emp_name, dept_name FROM employees e 
	INNER JOIN departments d on d.dept_id = e.dept_id;
```

# LEFT JOIN
```sql
SELECT e.emp_name, dept_name FROM employees e 
	LEFT JOIN departments d on d.dept_id = e.dept_id;
```

# RIGHT JOIN
```sql
SELECT e.emp_name, dept_name FROM employees e 
	RIGHT JOIN departments d on d.dept_id = e.dept_id;
```

# FULL OUTER JOIN
```sql
SELECT e.emp_name, dept_name FROM employees e 
	FULL OUTER JOIN departments d on d.dept_id = e.dept_id;
```

# SELF  JOIN
```sql
SELECT e.emp_name, dept_name FROM employees e, departments d 
	WHERE d.dept_id = e.dept_id;
```

# UNION
```sql
SELECT City FROM Customers 
UNION 
SELECT City FROM Suppliers ORDER BY City;
```
 
# UNION ALL

```sql
SELECT City FROM Customers 
UNION ALL 
SELECT City FROM Suppliers ORDER BY City;
```
 
# GROUP BY

```sql
SELECT city, count(1) total 
FROM suppliers 
GROUP BY city;

SELECT city, count(1) total 
FROM suppliers 
GROUP BY city 
ORDER BY COUNT(1) DESC;
```

# HAVING
```sql
SELECT city, count(1) total 
FROM suppliers 
GROUP BY city 
HAVING city LIKE 'M%';
```

# EXISTS

```sql
SELECT supplier_name 
FROM suppliers s 
WHERE EXISTS (
	SELECT product_name FROM products p WHERE p.supplier_id = s.supplier_id AND p.price < 20
);
```

# INSERT  INTO  SELECT
```sql
INSERT INTO customers (cust_name, city, country, address) 
	SELECT supplier_name, city, country, address FROM suppliers;
```
  
# CASE
```sql
SELECT 
	CASE 
		WHEN country IN ('Germany', 'Spain', 'France') THEN 'Europe' 
		ELSE 'Unknown' 
	END; 
FROM suppliers;
```

# NULL

```sql
SELECT emp_name, IFNULL(dept_id, 0) dept_id FROM employees; -- MySQL

SELECT emp_name, ISNULL(dept_id, 0) dept_id FROM employees; -- SQL Server

SELECT emp_name, IsNull(dept_id, 0) dept_id FROM employees; -- MS Access

SELECT emp_name, NVL(dept_id, 0) dept_id FROM employees; -- Oracle
```

# MERGE INTO

```sql
MERGE INTO employees e
    USING hr_records h ON (e.id = h.emp_id)
        WHEN MATCHED THEN
            UPDATE SET e.address = h.address
        WHEN NOT MATCHED THEN
            INSERT (id, address) VALUES (h.emp_id, h.address);
```

## UNION
UNION, UNION ALL, INTERSECT ve MINUS operatörlerini kullanarak birden çok sorguyu birleştirebilirsiniz. Tüm küme operatörleri eşit önceliğe sahiptir. Bir SQL deyimi birden çok küme operatörü içeriyorsa, parantezler açıkça başka bir sıra belirtmedikçe Oracle Database bunları soldan sağa doğru değerlendirir.

```sql
-- Kullanım 1
SELECT OBJECTID, ILCE, MAHALLE, ADA, PARSEL FROM TBL_KADASTRO_01
UNION
SELECT OBJECTID, ILCE, MAHALLE, ADA, PARSEL FROM TBL_KADASTRO_02

-- Kullanım 2
SELECT OBJECTID, ILCE, MAHALLE, ADA, PARSEL FROM TBL_KADASTRO_01
UNION ALL
SELECT OBJECTID, ILCE, MAHALLE, ADA, PARSEL FROM TBL_KADASTRO_02
```


## SDO_CENTROID

Polygon, multipolygon, point veya point cluster için merkezi olan bir nokta geometrisi döndürür. (Centroid aynı zamanda _'ağırlık merkezi'_ olarak da bilinir.)

```sql
SELECT 
	SDO_GEOM.SDO_CENTROID(SDO_CS.TRANSFORM(SHAPE, 4326), 0.005).SDO_POINT.X LATITUDE 
	SDO_GEOM.SDO_CENTROID(SDO_CS.TRANSFORM(SHAPE, 4326), 0.005).SDO_POINT.Y LONGITUDE 
FROM ILCE
```
> 0.005 değeri tolerans değeridir. Tolerans, bir kesinlik düzeyini spatial verilerle ilişkilendirmek için kullanılır. Tolerans, iki noktanın ayrı olabileceği ve yine de aynı kabul edilebileceği mesafeyi yansıtır (örneğin, yuvarlama hatalarını karşılamak için). Tolerans değeri sıfırdan büyük pozitif bir sayı olmalıdır. Tolerans değerinin önemi, mekansal verilerin bir jeodezik koordinat sistemi ile ilişkilendirilip ilişkilendirilmediğine bağlıdır.

## SDO_RELATE

2 spatial nesnenin birbirleriyle olan etkileşimlerini tanımlamak için kullanılır. Etkileşim türlerine [buradan](https://medium.com/@barisariburnu/oracle-spatial-sdo-relate-kullan%C4%B1m%C4%B1-2b9d16c567df) ulaşabilirsiniz.

```sql
-- 1. Kullanım: SELF JOIN ile kullanımı gösterilmiştir.
SELECT * FROM ADA A, PARSEL P 
	WHERE SDO_RELATE(A.GEOMETRI,P.GEOMETRI,'MASK=DISJOINT')='TRUE';

-- 2. Kullanım: INNER JOIN ile kullanımı gösterilmiştir.
SELECT * FROM ADA A 
	INNER JOIN PARSEL P ON SDO_RELATE(A.GEOMETRI,P.GEOMETRI,'MASK=DISJOINT')='TRUE';

-- 3. Kullanım: Birden fazla maskelemenin tek sorguda kullanımı gösterilmiştir.
SELECT * FROM ADA A 
	INNER JOIN PARSEL P ON SDO_RELATE(A.GEOMETRI,P.GEOMETRI,'MASK=INSIDE+COVEREDBY')='TRUE';
```
## SDO_AREA

İki boyutlu bir polygon nesnesinin alanını döndürür.

```sql
SELECT AD, SDO_GEOM.SDO_AREA(SHAPE, 0.005) FROM ILCE
```
> 0.005 değeri tolerans değeridir. Tolerans, bir kesinlik düzeyini spatial verilerle ilişkilendirmek için kullanılır. Tolerans, iki noktanın ayrı olabileceği ve yine de aynı kabul edilebileceği mesafeyi yansıtır (örneğin, yuvarlama hatalarını karşılamak için). Tolerans değeri sıfırdan büyük pozitif bir sayı olmalıdır. Tolerans değerinin önemi, mekansal verilerin bir jeodezik koordinat sistemi ile ilişkilendirilip ilişkilendirilmediğine bağlıdır.

## INITCAP / NLS_INITCAP

INITCAP, her kelimenin ilk harfi büyük, diğer tüm harfleri küçük olacak şekilde char döndürür. Sözcükler, beyaz boşlukla veya alfasayısal olmayan karakterlerle sınırlandırılmıştır. Türkçe karakterlerin dönüşümünü için uygun değildir. Bu sorunu çözmek için NLS_INITCAP fonksiyonu kullanılmaktadır. Detaylı açıklamaya [buradan](https://medium.com/@barisariburnu/oracle-nls-fonksiyonlar%C4%B1-1a7f1e0d257f) ulaşabilirsiniz.

```sql
SELECT INITCAP('ingilizce') FROM dual;
--> Ingilizce

SELECT NLS_INITCAP('ingilizce', 'NLS_SORT = XTURKISH') FROM dual;
--> İngilizce
```
## UPPER/ NLS_UPPER

UPPER, tüm harfleri büyük olacak şekilde char değerini döndürür. Türkçe karakterlerin dönüşümünü için uygun değildir. Bu sorunu çözmek için NLS_UPPER fonksiyonu kullanılmaktadır. Detaylı açıklamaya [buradan](https://medium.com/@barisariburnu/oracle-nls-fonksiyonlar%C4%B1-1a7f1e0d257f) ulaşabilirsiniz.

```sql
SELECT UPPER('ingilizce') FROM dual;
--> INGILIZCE

SELECT NLS_UPPER('ingilizce', 'NLS_SORT = XTURKISH') FROM dual;
--> İNGİLİZCE
```
## CASE

CASE ifadeleri, SQL deyimlerinde IF ... THEN ... ELSE mantığını, prosedürleri çağırmak zorunda kalmadan kullanmanıza izin verir.
```sql
-- 1. Kullanım
SELECT 
     CASE
        WHEN TIP = 1 THEN AD || ' Sk.'
        WHEN TIP = 2 THEN AD || ' Cd.'
        WHEN TIP = 3 THEN AD || ' Blv.'
        WHEN TIP = 4 THEN AD            --Meydan
        WHEN TIP = 5 THEN AD            --Küme Evler
        WHEN TIP = 6 THEN AD            --Karayolu
     END
        AS "YOL ADI",
     SHAPE
FROM YOLORTAHAT ;

-- 2. Kullanım: Grup fonksiyon içerinde CASE kullanılabilmektedir.
SELECT
   COUNT(CASE DEFIN_DURUMU WHEN -1 THEN 0 END) BOS
FROM PARSEL

-- 3. Kullanım: Fonksiyonlar içerisinde CASE yapısı kullanılabilmektedir. Fonksiyon parametreleri girilirken, değer alanına yazılması yeterlidir.
SELECT 
	NLS_INITCAP (
	     CASE
	        WHEN TIP = 1 THEN AD || ' Sk.'
	        WHEN TIP = 2 THEN AD || ' Cd.'
	        WHEN TIP = 3 THEN AD || ' Blv.'
	     END, 'NLS_SORT = XTURKISH') AS "YOL ADI",
     SHAPE
FROM YOLORTAHAT ;

-- 4. Kullanım: Case şartında birden fazla koşul belirtilebilir.
SELECT 
	CASE
		WHEN DEPT_ID = 4 AND SALARY > 3000 THEN 'HKMO' 
	END 
FROM EMPLOYEES;
```
> Bilgi: || ifadesi STRING değerleri birleştirmek için kullanılmaktadır.

### SELECT Sorguları

```sql
-- Her ilçede kaç tane mahalle var?
SELECT 
    I.KIMLIKNO "ILCE KIMLIK NO", 
    I.AD "ILCE ADI", 
    M.KIMLIKNO "MAHALLE KIMLIK NO", 
    M.AD "MAHALLE ADI" 
FROM ILCE I
    INNER JOIN MAHALLE M ON M.ILCEID = I.ID;

-- Her ilçenin yüz ölçümü nedir?
SELECT AD, SDO_GEOM.SDO_AREA(SHAPE, 0.005) "YUZ OLCUMU" FROM ILCE;

-- Bursa'daki tüm ilçelerin yüz ölçümü toplamı nedir?
SELECT SUM(SDO_GEOM.SDO_AREA(SHAPE, 0.005)) "YUZ OLCUMU" FROM ILCE

-- Hangi ilçenin kaç tane mahallesi vardır?
SELECT  
    I.AD "ILCE ADI", 
    COUNT(1)
FROM ILCE I
    INNER JOIN MAHALLE M ON M.ILCEID = I.ID
GROUP BY I.AD;

-- MAKS Yol Gösterim
SELECT 
	YOHY.OBJECTID,
    Y.ID,
    Y.AD,
    Y.TIP,
    I.ID ILCE_ID,
    I.KIMLIKNO ILCE_KIMLIK_NO,
    I.AD ILCE_AD,
    M.ID MAHALLE_ID,
    M.KIMLIKNO MAHALLE_KIMLIK_NO,
    M.AD MAHALLE_AD,
    YOH.SHAPE SHAPE
FROM ILCE I
    INNER JOIN MAHALLE M ON I.ID = M.ILCEID
    INNER JOIN YOLORTAHATYON YOHY ON M.ID = YOHY.MAHALLEID
    INNER JOIN YOLORTAHAT YOH ON YOHY.YOLORTAHATID = YOH.ID
    INNER JOIN YOL Y ON YOH.YOLID = Y.ID;
```