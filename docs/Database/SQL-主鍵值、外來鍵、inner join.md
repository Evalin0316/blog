
import Highlight from '@site/src/components/helper/HighLight';

### 主鍵和外來鍵的差異

| 主鍵 ( Primary Key，簡稱 PK ) | 外來鍵 ( Foreign Key，簡稱 FK ) |
|  ----  | ----  | 
| 每個資料表都要有一個主鍵 | 當資料需要關聯到其他表格時才會用到 |
| 主鍵的值必須是唯一的，不能重複 | 同一個資料表中，可以有多筆資料使用相同的外來鍵 |
| 主鍵不能是 Null 值，但外來鍵可以是 Null 值 | 命名通常會用 '參考資料表_id' 的格式 |
| 大部分情況會命名為 'id'，使用整數或 UUID 格 | 外來鍵必須對應到被參考資料表的主鍵 |
| 設定後就不應該再更動 |  |


### <Highlight color="#25c2a0">練習</Highlight>

### 班級資料
| 學生編號 | 姓名 | 班級 | 性別 | 年齡 |
|  ----  | ----  |  ----  | ---- | ---- |
| 1 | 小明 | 三年一班 | 男 | 8 |
| 2 | 小華 | 三年二班 | 女 | 9 |
| 3 | 小美 | 三年一班 | 男 | 8 |
| 4 | 小強 | 三年一班 | 女 | 8 |
| 5 | 小智 | 三年二班 | 男 | 9 |


```js
// 建立班級資料庫
CREATE TABLE class(
  id Serial PRIMARY KEY,
  class_name VARCHAR(20),
  teacher VARCHAR(100)
);

INSERT INTO class(class_name, teacher) VALUES
('三年一班', '王美麗'),
('三年二班', '張樂樂');

-----------------------------------------------

// 建立學生資料庫
CREATE Table school (
  id Serial PRIMARY KEY,
  name VARCHAR(100), // 學生姓名
  class_name VARCHAR(20), // 班級名稱
  gender VARCHAR(10),
  age INTEGER,
  class_id INTEGER,
  FOREIGN KEY (class_id) REFERENCES class(id)
);

INSERT INTO school (name, class_name, gender, age, class_id) VALUES
('小明', '三年一班', '男', 8, 1),
('小華', '三年二班', '女', 8, 2),
('小美', '三年一班', '男', 8, 1),
('小強', '三年一班', '女', 8, 1),
('小智', '三年二班', '男', 8, 2);

----------------------------------------------

// 建立學生家庭資料
CREATE TABLE student_family (
  id Serial PRIMARY KEY,
  student_id INTEGER,
  parent_name VARCHAR(20),
  parent_phone VARCHAR(20),
  parent_gender VARCHAR(3),
  FOREIGN KEY(student_id) REFERENCES school(id)
);

INSERT INTO student_family (student_id, parent_name, parent_phone, parent_gender) VALUES
  (1, '王大祥', '0911223344', '男'),
  (2, '王曉茹', '0955667788', '女'),
  (3, '王大祥', '0933445566', '男'),
  (4, '王曉茹', '0966778899', '女'),
  (5, '王大祥', '0922334455', '男');


// 合併學生資料與班導資料
(1)
SELECT school.*, class.teacher
FROM school
INNER JOIN class ON school.class_id = class.id

(2)
// 篩選特定資料及排序
SELECT
  school.id,
  school.name,
  class.class_name AS 班級,
  class.teacher AS 班導,
  school.gender,
  school.age
FROM school
INNER JOIN class ON school.class_id = class.id

---------------------------------------------------

// 合併學生資料與家長資料
SELECT
  school.id AS 小孩編號,
  school.name AS 姓名,
  student_family.parent_name AS 父母姓名,
  student_family.parent_phone AS 父母電話,
  student_family.parent_gender AS 父母性別
FROM student_family
INNER JOIN school ON student_family.student_id = school.id

```