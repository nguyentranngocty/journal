# Database

## Thiết kế và SQl Server

[Database Design](assets\db_images)

# PostgreSQL

## CREATE TABLE

- Tạo bảng cơ bản

  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      position VARCHAR(50),
      salary NUMERIC(10, 2)
  );
  ```

  **Chú ý**

  -`SERIAL` : kiểu dữ liệu tự tăng

  -`NUMERIC(10, 2)` : kiểu dữ liệu số, có tổng cộng 10 chữ số và 2 chữ số thập phân.
  
- Tạo bảng với ràng buộc (`UNIQUE`, `CHECK`)

  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      position VARCHAR(50),
      salary NUMERIC(10, 2),
      email VARCHAR(100) UNIQUE,
      CHECK (salary > 0)
  );
  ```

  **Chú ý**
  - `VARCHAR(100) UNIQUE` : kiểu dữ liệu chuỗi
  ký tự phải là duy nhất.
  - `CHECK (salary > 0)`: Đảm bảo rằng giá trị của `salary` phải lớn hơn 0.
- Tạo bảng với khóa ngoại (`FOREIGN KEY`)

  ```sql
  CREATE TABLE departments (
      dept_id SERIAL PRIMARY KEY,
      dept_name VARCHAR(100) NOT NULL
  );

  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      position VARCHAR(50),
      salary NUMERIC(10, 2),
      dept_id INTEGER,
      FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
  );
  ```
- Tạo bảng với các tùy chọn nâng cao (`DEFAULT`, `ON DELETE CASCADE`, `ON UPDATE CASCADE`)

  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      position VARCHAR(50),
      salary NUMERIC(10, 2) DEFAULT 0.00,
      dept_id INTEGER,
      FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
          ON DELETE CASCADE
          ON UPDATE CASCADE
  );
  ```

  **Chú ý**
  - `ON DELETE CASCADE`: Khi một bản ghi trong bảng `departments`
  bị xóa, các bản ghi tương ứng trong bảng `employees` cũng sẽ bị xóa.
  - `ON UPDATE CASCADE`: Khi một giá trị `dept_id` trong bảng `departments`
  được cập nhật, các giá trị tương ứng trong bảng `employees` cũng sẽ được cập nhật.

## KEY

- Các loại key
  ```sql
  -- Tạo bảng departments với khóa chính
  CREATE TABLE departments (
      dept_id SERIAL PRIMARY KEY,
      dept_name VARCHAR(100) NOT NULL
  );

  -- Tạo bảng employees với khóa chính và khóa ngoại
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      position VARCHAR(50),
      salary NUMERIC(10, 2),
      email VARCHAR(100) UNIQUE,
      dept_id INTEGER,
      FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
  );

  -- Tạo bảng project_assignments với khóa tổng hợp
  CREATE TABLE project_assignments (
      project_id INTEGER,
      employee_id INTEGER,
      assignment_date DATE,
      PRIMARY KEY (project_id, employee_id),
      FOREIGN KEY (employee_id) REFERENCES employees(id)
  );
  ```
- Lưu ý
  - Khóa chính (`PRIMARY KEY`) đảm bảo mỗi hàng trong bảng là duy nhất và không null.
  - Khóa duy nhất (`UNIQUE`) đảm bảo rằng các giá trị trong cột hoặc tập hợp các
    cột là duy nhất.
  - Khóa ngoại (`FOREIGN KEY`) tạo liên kết giữa các bảng để duy trì tính
    toàn vẹn dữ liệu.
  - Khóa tổng hợp (`COMPOSITE KEY`) là khóa chính hoặc khóa duy nhất bao gồm nhiều cột.

## INSERT

- Cú pháp

  ```sql
  INSERT INTO employees (name, position, salary)
  VALUES ('John Doe', 'Manager', 75000.00);

  -- Nhiều bản ghi
  INSERT INTO employees (name, position, salary)
  VALUES
      ('Jane Smith', 'Developer', 60000.00),
      ('Emily Johnson', 'Designer', 55000.00),
      ('Michael Brown', 'Analyst', 65000.00);

  -- Thêm dữ liệu với tất cả các cột
  INSERT INTO employees (id, name, position, salary)
  VALUES (1, 'Alice Cooper', 'CEO', 150000.00);

  -- Thêm dữ liệu không chỉ rỗ cột => giá trị cung cấp đúng thứ tự các cột trong bảng, có thể bỏ qua phần liệt kê các cột:
  INSERT INTO employees
  VALUES (2, 'Bob Marley', 'CTO', 140000.00);

  -- Nhập tiếng Việt thì thêm N đằng trước
  INSERT INTO teacher
  VALUES ('GV01', N'Nguyễn Văn A')
  ```
- Sử dụng `RETURNING` để lấy dữ liệu sau khi thêm

  ```sql
  INSERT INTO employees (name, position, salary)
  VALUES ('Charlie Parker', 'CFO', 130000.00)
  RETURNING id, name;
  ```

## UPDATE

- Cú pháp
  ```sql
  UPDATE employees
  SET salary = 80000.00
  WHERE id = 1;

  -- Nhiều cột
  UPDATE employees
  SET position = 'Senior Developer', salary = 70000.00
  WHERE id = 2;

  -- Điều kiện phức tạp
  UPDATE employees
  SET salary = salary * 1.10
  WHERE position = 'Developer' AND salary < 70000.00;

  -- Cập nhất tất cả bản ghi => bỏ WHERE
  UPDATE employees
  SET salary = salary + 5000.00;
  ```
- Sử dụng `RETURNING` để lấy dữ liệu sau khi cập nhật
  ```sql
  UPDATE employees
  SET salary = 85000.00
  WHERE id = 3
  RETURNING id, name, salary;
  ```

## DROP

### DROP TABLE

- Cú pháp
  ```sql
  DROP TABLE employees;

  -- Để tránh lỗi khi bảng không tồn tại
  DROP TABLE IF EXISTS employees;

  -- Xóa nhiều bảng cùng lúc
  DROP TABLE IF EXISTS employees, departments, projects;
  ```

  **Lưu ý**
  - Khi bạn xóa một bảng, tất cả dữ liệu trong bảng đó sẽ bị mất
  và không thể khôi phục lại, trừ khi bạn có bản sao lưu.
  - Việc xóa bảng cũng sẽ xóa tất cả các ràng buộc, chỉ mục, trigger và các đối tượng khác liên quan đến bảng đó.

### DROP Column

- Cú pháp
  ```sql
  ALTER TABLE employees
  DROP COLUMN position;

  -- xóa nhiều cột
  ALTER TABLE employees
  DROP COLUMN position,
  DROP COLUMN salary;
  ```

  **Lưu ý**
  - Xóa cột sẽ xóa tất cả dữ liệu trong cột đó và không thể khôi phục lại nếu không có bản sao lưu.

## DELETE

- Cú pháp
  ```sql
  -- xóa 1 hàng
  DELETE FROM employees
  WHERE id = 1;

  -- xóa nhiều hàng
  DELETE FROM employees
  WHERE position = 'Developer' AND salary < 60000.00;

  -- xóa tất cả hàng
  DELETE FROM employees;
  ```
- Sử dụng `RETURNING` để lấy dữ liệu sau khi xóa
  ```sql
  DELETE FROM employees
  WHERE id = 2
  RETURNING id, name;
  ```

## Example

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    salary NUMERIC(10, 2)
);

INSERT INTO employees (name, position, salary)
VALUES
    ('John Doe', 'Manager', 75000.00),
    ('Jane Smith', 'Developer', 60000.00),
    ('Emily Johnson', 'Designer', 55000.00);

-- Cập nhật thông tin của John Doe
UPDATE employees
SET salary = 80000.00
WHERE id = 1;

-- Cập nhật thông tin của Jane Smith
UPDATE employees
SET position = 'Senior Developer', salary = 70000.00
WHERE id = 2;

-- Cập nhật lương cho tất cả các Developer có lương dưới 70000.00
UPDATE employees
SET salary = salary * 1.10
WHERE position = 'Developer' AND salary < 70000.00;

-- Xóa một hàng với id = 1
DELETE FROM employees
WHERE id = 1;

-- Xóa tất cả các Developer có lương dưới 60000.00
DELETE FROM employees
WHERE position = 'Developer' AND salary < 60000.00;

-- Xóa tất cả các hàng trong bảng (cẩn thận khi sử dụng)
DELETE FROM employees;

-- Xóa cột 'position'
ALTER TABLE employees
DROP COLUMN position;

-- Xóa nhiều cột 'position' và 'salary'
ALTER TABLE employees
DROP COLUMN salary;

-- Xóa nhiều bảng cùng lúc
DROP TABLE IF EXISTS employees, departments, projects;
```

## Note

Trình tự khi tạo database :
    - create table
    - create primary key
    - insert value **NÊN DÙNG CÂU LỆNH `SET DATEFORMAT DMY` ĐỂ INSERT DỮ LIỆU DATE KHÔNG BỊ LỖI**
    - create foreign key

## Query

- `SELECT`
- `FROM`
- `WHERE`
- `ORDER BY`
- `LIMIT`
  ```sql
  -- chọn hết các cột
  SELECT *
  FROM employees -- table

  -- chọn cột name và salary
  SELECT 
      name 
      , salary 
  FROM employees
  WHERE
      department = 'IT' 
      AND salary > 50000 -- condition
  ORDER BY 
      salary DESC
      , name ASC
  LIMIT 5; -- limit số lượng kết quả select ra
  ```

### AS

- Dùng để đặt bí danh (alias) cho các cột, bảng, aggregate function, subquery,... trong truy vấn.
- Ví dụ

  ```sql
  -- cột
  SELECT first_name || ' ' || last_name AS full_name
  FROM employees;

  -- hàm
  SELECT SUM(amount) AS total_sales
  FROM orders;

  -- bảng
  SELECT o.order_id, o.amount, c.customer_name
  FROM orders AS o
  JOIN customers AS c ON o.customer_id = c.customer_id;
  ```
- Chú ý :

  - Có thể đặt bí danh mà không cần sử dụng từ khóa `AS`
  - Đặt alias phù hợp cho việc tham chiếu đến các cột
- Truy xuất qua toán tử (.)

  - Chỉ định cột của một bảng

  ```sql
  SELECT employees.first_name, employees.last_name
  FROM employees;
  ```

  - Tên thay thế ngắn gọn cho bảng hoặc cột

  ```sql
  SELECT e.first_name, e.last_name
  FROM employees e;
  ```

  - Truy cập các bảng trong một cơ sở dữ liệu hoặc lược đồ cụ thể

  ```sql
  SELECT sales_db.public.orders.order_id
  FROM sales_db.public.orders;
  ```

### LIKE

- `LIKE` : tìm kiếm một mẫu cụ thể trong một cột dùng `WHERE`
- Các ký tự đại diện phổ biến:
  - `%`: Đại diện cho bất kỳ chuỗi ký tự nào (bao gồm cả chuỗi rỗng).
  - `_`: Đại diện cho một ký tự duy nhất.
- Kết hợp `LIKE` với các mệnh đề khác như `AND`, `OR`
- Ví dụ
  ```sql
  -- Tìm các bản ghi có tên bắt đầu bằng 'A':
  SELECT * FROM Employees
  WHERE Name LIKE 'A%';

  -- Tìm các bản ghi có tên kết thúc bằng 'n':
  SELECT * FROM Employees
  WHERE Name LIKE '%n';

  -- Tìm các bản ghi có tên chứa 'an' ở bất kỳ vị trí nào:
  SELECT * FROM Employees
  WHERE Name LIKE '%an%';
  -- Tìm các bản ghi có tên có đúng 4 ký tự và bắt đầu bằng 'A':
  SELECT * FROM Employees
  WHERE Name LIKE 'A___';

  SELECT * FROM Employees
  WHERE Name LIKE 'A%' AND Age > 25;
  ```
- Lưu ý
  - `LIKE` không phân biệt chữ hoa chữ thường trong MySQL, nhưng có phân biệt chữ hoa chữ thường trong PostgreSQL.
  - Dữ liệu lớn dùng `LIKE` với ký tự đại diện `%` ở đầu chuỗi tìm kiếm có thể ảnh hưởng đến hiệu suất, vì không thể sử dụng các chỉ mục một cách hiệu quả.

### OFFSET

- Bỏ qua 1 số hàng đầu tiên
- Ví dụ
  ```sql
  -- Muốn lấy 5 nhân viên tiếp theo sau khi bỏ qua 10 nhân viên đầu tiên:
  SELECT name, salary
  FROM employees
  ORDER BY salary DESC
  LIMIT 5 OFFSET 10;
  ```

### IN

- `IN` : lọc các giá trị từ một tập hợp cụ thể.
- Ví dụ
  ```sql
  -- chuỗi
  SELECT employee_id, first_name, last_name, department
  FROM employees
  WHERE department IN ('Sales', 'Marketing', 'HR');

  -- số
  SELECT product_id, product_name, price
  FROM products
  WHERE price IN (10, 20, 30);

  -- subquery
  SELECT employee_id, first_name, last_name
  FROM employees
  WHERE department_id IN (
      SELECT department_id 
      FROM departments 
      WHERE location = 'New York'
  );
  ```
- Chú ý : có thể sử dụng `NOT IN` với công dụng ngược lại `IN`

### SIMILAR TO

- Lọc các hàng dựa trên các mẫu phức tạp bằng cách sử dụng regular expressions
- Ví dụ
  ```sql
  -- Giả sử bạn có một bảng `products` với cột `product_name`, và bạn muốn lọc ra các tên sản phẩm kết thúc bằng "cot", "goat", "nylon", hoặc "mewn".
  SELECT product_name
  FROM products
  WHERE product_name SIMILAR TO '%(cot|goat|nylon|mewn)';
  ```

### Operators

#### 1. Toán tử số học (Arithmetic Operators)

- `+` : Phép cộng
- `-` : Phép trừ
- `*` : Phép nhân
- `/` : Phép chia
- `%` : Phép chia lấy dư

#### 2. Toán tử so sánh (Comparison Operators)

- `=`  : Bằng
- `<>` hoặc `!=` : Khác
- `>`  : Lớn hơn
- `<`  : Nhỏ hơn
- `>=` : Lớn hơn hoặc bằng
- `<=` : Nhỏ hơn hoặc bằng

#### 3. Toán tử logic (Logical Operators)

- `AND` : Và
- `OR`  : Hoặc
- `NOT` : Phủ định

#### 4. Toán tử chuỗi (String Operators)

- `||` hoặc `+` (trong một số DBMS) : Nối chuỗi
- Ví dụ
  ```sql
  SELECT 'Hello' || ' ' || 'World';
  -- Kết quả là 'Hello World' trong PostgreSQL và Oracle

  SELECT 'Hello' + ' ' + 'World'; 
  -- Kết quả là 'Hello World' trong SQL Server
  ```

#### 5. Toán tử tập hợp (Set Operators)

- `UNION` : Hợp
- `UNION ALL` : Hợp tất cả
- `INTERSECT` : Giao
- `EXCEPT` hoặc `MINUS` : Trừ
- Ví dụ
  ```sql
  SELECT column1 FROM table1
  UNION / INTERSECT / EXCEPT
  SELECT column1 FROM table2;
  ```

#### 6. Toán tử khác

- `BETWEEN ... AND ...` : Trong khoảng
- `IN` : Trong danh sách
- `LIKE` : Tìm kiếm mẫu
- `IS NULL` : Kiểm tra giá trị null
- Ví dụ
  ```sql
  SELECT *
  FROM Employees
  WHERE Age BETWEEN 30 AND 40;

  SELECT *
  FROM Employees
  WHERE Department IN ('HR', 'Engineering', 'Sales');

  SELECT *
  FROM Employees
  WHERE Name LIKE 'A%';

  SELECT *
  FROM Employees
  WHERE Address IS NULL;
  ```

### CASE WHEN

- `CASE WHEN` : cấu trúc điều kiện thực hiện các phép toán dựa trên các điều kiện để chọn ra hoặc phân loại dữ liệu bằng cách thêm một cột or nhiều cột
- Phân loại hạng mục :
  ```sql
  SELECT 
      student_id
      , name
      , score
      , CASE
          WHEN score < 50 THEN 'Fail'
          WHEN score >= 50 AND score < 75 THEN 'Pass'
          WHEN score >= 75 THEN 'Excellent'
          ELSE 'No Grade'
      END AS grade
  FROM students;
  ```
- Lọc dữ liệu trên các điều kiện
  ```sql
  SELECT 
      student_id
      , name
      , score
  FROM students
  WHERE 
      CASE
          WHEN score < 50 THEN 'Fail'
          WHEN score >= 50 AND score < 75 THEN 'Pass'
          WHEN score >= 75 THEN 'Excellent'
          ELSE 'No Grade'
      END = 'Pass';
  ```
- Xếp dữ liệu theo điều kiện
  ```sql
  SELECT 
      student_id
      , name
      , score
  FROM  students
  ORDER BY 
      CASE
          WHEN score < 50 THEN 3
          WHEN score >= 50 AND score < 75 THEN 2
          WHEN score >= 75 THEN 1
          ELSE 4
      END;
  ```

### GROUP BY

- `GROUP BY` dùng để nhóm các bản ghi có cùng giá trị trong một hoặc nhiều cột lại với nhau
- Sử dụng cùng với các Aggregate Function (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)
- `HAVING` dùng để lọc các nhóm dựa trên các Agreegate Function sau `GROUP BY`
- Ví dụ
  ```sql
  SELECT 
      product_id
      , COUNT(*) AS total_sales
      , SUM(sale_amount) AS total_amount
      , AVG(sale_amount) AS average_amount
      , MAX(sale_amount) AS max_sale
      , MIN(sale_amount) AS min_sale
  FROM sales
  GROUP BY product_id
  HAVING SUM(sale_amount) > 500
  ORDER BY total_amount DESC;
  ```
- `GROUP BY` kết hợp với `JOIN` để lấy dữ liệu từ nhiều bảng
  ```sql
  SELECT 
      c.customer_name
      , SUM(o.amount) AS total_amount
  FROM customers AS c
  JOIN orders AS o 
      ON c.customer_id = o.customer_id
  GROUP BY c.customer_name;
  ```
- Chú ý : trên SELECT ghi gì thì dưới group by nhóm cái đó, ngoại trừ
  mấy cái hàm được định nghĩa

### JOIN

- `INNER JOIN` / `JOIN`: Trả về các hàng khi có ít nhất một sự khớp nhau giữa các bảng.
- `LEFT JOIN`: Trả về tất cả các hàng từ bảng bên trái và các hàng khớp nhau từ bảng bên phải. Nếu không có sự khớp nhau, giá trị của bảng bên phải sẽ là `NULL`.
- `RIGHT JOIN`: Trả về tất cả các hàng từ bảng bên phải và các hàng khớp nhau từ bảng bên trái. Nếu không có sự khớp nhau, giá trị của bảng bên trái sẽ là `NULL`.
- `FULL JOIN`: Trả về tất cả các hàng khi có một sự khớp nhau trong một trong các bảng. Nếu không có sự khớp nhau, giá trị của bảng không khớp sẽ là `NULL`.
- `CROSS JOIN`: Trả về tích Descartes của hai bảng, nghĩa là tất cả các kết hợp có thể có giữa các hàng của hai bảng. Dùng trong các bài toán xác suất, phân tích tổ hợp, hoặc khi bạn cần tạo ra một bảng đối chiếu giữa hai tập hợp.
- `SELF JOIN` : Bảng tự nối với chính nó. Dùng để so sánh các hàng trong cùng một bảng, xác định các mối quan hệ phân cấp trong dữ liệu như quản lý nhân viên, tìm các cặp giá trị trong một bảng thỏa mãn một điều kiện.
- Ví dụ
  ```sql
  -- JOIN / INNER JOIN
  SELECT 
      s.StudentName
      , c.CourseName
  FROM Students s
  INNER JOIN Enrollments e
      ON s.StudentID = e.StudentID
  JOIN Courses 
      ON e.CourseID = c.CourseID;

  -- LEFT JOIN
  SELECT 
      c.CustomerID
      , c.CustomerName
      , o.OrderID
      , o.OrderDate
  FROM Customers c
  LEFT JOIN Orders o
      ON c.CustomerID = o.CustomerID;

  -- RIGHT JOIN
  SELECT 
      e.EmployeeID
      , e.EmployeeName
      , d.DepartmentName
  FROM Employees e
  RIGHT JOIN Departments d USING(DepartmentID);

  -- FULL JOIN
  SELECT 
      e.EmployeeID
      , e.EmployeeName
      , d.DepartmentID
      , d.DepartmentName
  FROM Employees e
  FULL JOIN Departments d
  ON e.DepartmentID = d.DepartmentID;

  -- CROSS JOIN
  SELECT 
      s.StudentName
      , c.CourseName
  FROM Students s
  CROSS JOIN Courses c;

  -- SELF JOIN
  SELECT 
      a.name AS "Employee"
  FROM Employee a
  JOIN Employee b
      ON a.managerId = b.id AND a.salary > b.salary;
  -- cách viết khác của SELF JOIN
  SELECT a.name AS "Employee" 
  FROM 
      Employee a
      , Employee b
  WHERE
      a.managerId = b.id
      AND a.salary > b.salary;
  ```

### Thứ tự thực hiện câu lệnh SQL

- Thứ tự

  - 1. **FROM**: Xác định bảng hoặc các bảng mà dữ liệu sẽ được lấy.
  - 2. **JOIN**: Kết hợp các bảng dựa trên điều kiện đã chỉ định.
  - 3. **WHERE**: Lọc dữ liệu dựa trên điều kiện đã đưa ra.
  - 4. **GROUP BY**: Gom nhóm dữ liệu dựa trên một hoặc nhiều cột.
  - 5. **HAVING**: Lọc các nhóm đã tạo ra bởi GROUP BY dựa trên điều kiện đã đưa ra.
  - 6. **SELECT**: Chọn các cột hoặc biểu thức để trả về trong kết quả.
  - 7. **DISTINCT**: Loại bỏ các bản ghi trùng lặp từ kết quả.
  - 8. **ORDER BY**: Sắp xếp kết quả theo một hoặc nhiều cột.
  - 9. **LIMIT**: Giới hạn số lượng bản ghi được trả về.
- Tóm tắt

  - **FROM** và **JOIN**: Liên kết các bảng với nhau.
  - **WHERE**: Lọc các hàng dựa trên điều kiện.
  - **GROUP BY**: Gom nhóm các hàng.
  - **HAVING**: Lọc các nhóm đã gom.
  - **SELECT**: Chọn các cột để hiển thị.
  - **DISTINCT**: Loại bỏ các hàng trùng lặp.
  - **ORDER BY**: Sắp xếp kết quả.
  - **LIMIT**: Giới hạn số lượng kết quả.

### Xử lý thời gian

- `DATETIME` : Từ `'1000-01-01 00:00:00'` đến `'9999-12-31 23:59:59'`
- `TIMESTAMP` : Từ `'1970-01-01 00:00:01'` UTC đến `'2038-01-19 03:14:07'` UTC
- Sử dung
  - `DATETIME` : lưu trữ ngày giờ mà không cần quan tâm đến múi giờ, hoặc khoảng thời gian quản lý vượt ngoài phạm vi của `TIMESTAMP`.
  - `TIMESTAMP` : làm việc với dữ liệu có múi giờ và cần chuyển đổi
    giữa các múi giờ khác nhau.
- Hàm
  - `CURRENT_DATE` : Ngày hiện tại
  - `CURRENT_TIME` : Giờ hiện tại
  - `NOW()` : Ngày giờ hiện tại
  - `DATE_ADD()` và `DATE_SUB()` : Thêm hoặc trừ ngày tháng
  - `DATEDIFF()` : Tính khoảng cách giữa hai ngày
  - `DATE_FORMAT()` : Định dạng ngày tháng
  - `DATE_TRUNC()` : Cắt bớt một giá trị ngày/giờ tới một đơn vị thời gian cụ thể (dùng để sắp xếp các tháng, ngày, giờ theo thứ tự)
  - `EXTRACT()` : Trích xuất một phần cụ thể của ngày tháng hoặc thời gian (ra số)
  - `TO_CHAR()` : chuyển đổi về dạng chuỗi theo định dạng bất kì
- Ví dụ
  ```sql
  SELECT CURRENT_DATE;
  SELECT CURRENT_TIME;
  SELECT NOW();

  SELECT DATE_ADD('2024-06-29', INTERVAL 10 DAY);
  SELECT DATE_SUB('2024-06-29', INTERVAL 10 DAY);
  SELECT '2024-06-29'::date + INTERVAL '10 days';
  -- Kết quả: '2024-07-09'
  SELECT '2024-06-29'::date - INTERVAL '10 days';
  -- Kết quả: '2024-06-19'
  SELECT '14:30:00'::time + INTERVAL '1 hour 30 minutes';
  -- Kết quả: '16:00:00'
  SELECT '14:30:00'::time - INTERVAL '1 hour 30 minutes';
  -- Kết quả: '13:00:00'

  SELECT DATEDIFF('2024-07-09', '2024-06-29');
  SELECT '2024-07-09'::date - '2024-06-29'::date;
  -- Kết quả: 10
  SELECT '16:00:00'::time - '14:30:00'::time;
  -- Kết quả: '01:30:00`

  SELECT DATE_FORMAT('2024-06-29', '%d/%m/%Y');
  SELECT DATE_TRUNC('year', TIMESTAMP '2024-06-29 14:30:00');
  -- Kết quả: '2024-01-01 00:00:00'
  SELECT DATE_TRUNC('month', TIMESTAMP '2024-06-29 14:30:00');
  -- Kết quả: '2024-06-01 00:00:00'
  SELECT DATE_TRUNC('day', TIMESTAMP '2024-06-29 14:30:00');
  -- Kết quả: '2024-06-29 00:00:00'
  SELECT DATE_TRUNC('hour', TIMESTAMP '2024-06-29 14:30:00');
  -- Kết quả: '2024-06-29 14:00:00'
  SELECT 
      DATE_TRUNC('month', order_date) AS month
      , SUM(amount) AS total_sales
  FROM orders
  WHERE DATE_TRUNC('QUARTER', order_date) = '2020-01-01' -- QUÝ 1 2020
  GROUP BY month
  ORDER BY month;

  SELECT 
  EXTRACT(YEAR FROM created_at) AS "created_year_no"
  , EXTRACT(MONTH FROM created_at) AS "created_month_no"
  , EXTRACT(DAY FROM created_at) 
  , EXTRACT(HOUR FROM created_at)
  FROM sales
  SELECT 
      EXTRACT(MONTH FROM event_date) AS event_month
      , COUNT(*) AS event_count
  FROM events
  WHERE EXTRACT(YEAR FROM event_date) = 2023
  GROUP BY EXTRACT(MONTH FROM event_date)
  ORDER BY event_month;

  SELECT
      TO_CHAR(created_at, 'Mon')
      , TO_CHAR(created_at, 'Day')
      , TO_CHAR(created_at, 'YYYY')
      , TO_CHAR(created_at, 'HH24')
  FROM sales
  ```

### Xử lý chuỗi

- `LENGTH()` : Độ dài xâu
- `STRCMP()` : So sánh xâu theo thứ tự từ điển > 0 là true
- `lEFT()` : Lọc ra n chữ cái tính từ bên trái
- `RIGHT()` : Lọc ra n chữ cái tính từ bên phải
- `SUBSTR()` : Lọc ra xâu con vị bắt đầu n kí tự
- `LIKE` : lọc theo mẫu
- Ví dụ
  ```sql
  USE northwind;
  SELECT * FROM customers
  WHERE LENGTH(CustomerName) >= 25;
  WHERE STRCMP(CustomerName, 'H') > 0;
  WHERE LEFT(CustomerName, 3) = "Hun";
  WHERE RIGHT(CustomerName, 2) = "en";
  WHERE SUBSTR(CustomerName, 2, 2) = "au";
  WHERE CustomerName LIKE "b%"; -- bắt đầu từ kí từ nào đấy (char/string%)
  WHERE CustomerName LIKE "%er"; -- kết thúc bằng kí từ nào đấy (%char/string)
  WHERE CustomerName LIKE "%ta%"; -- cụm từ bất kì (%string%)
  WHERE CustomerName LIKE "_t%"; -- kí từ thứ n (_char%)
  WHERE CustomerName LIKE "t%e"; -- bắt đầu -> kết thúc (char%char)
  ```

### Subquery

- Phân loại

  1. **Single-row subquery**: Trả về một hàng duy nhất.
  2. **Multiple-row subquery**: Trả về nhiều hàng.
  3. **Correlated subquery**: Phụ thuộc vào truy vấn bên ngoài và được thực
     thi lặp lại cho mỗi hàng được xử lý bởi truy vấn bên ngoài.
  4. **Non-correlated subquery**: Không phụ thuộc vào truy vấn bên ngoài và có thể được thực thi độc lập.
- Subquery có thể được sử dụng trong các câu lệnh như `SELECT`, `INSERT`, `UPDATE`, và `DELETE`, cũng như trong các
  mệnh đề như `WHERE`, `FROM`, và `HAVING`.
- **Subquery trong mệnh đề `WHERE`**:

  ```sql
    --Tìm tất cả các nhân viên có mức lương cao hơn mức lương trung bình của toàn công ty.
   SELECT *
   FROM Employees
   WHERE Salary > (
        SELECT AVG(Salary) 
        FROM Employees
    );
  ```
- **Subquery trong mệnh đề `FROM`**:

  ```sql
  -- Tìm số lượng nhân viên trong mỗi phòng ban
   SELECT Department, COUNT(*)
   FROM (
        SELECT Department
        FROM Employees
    ) AS DeptTable
   GROUP BY Department;
  ```
- **Subquery trong mệnh đề `SELECT`**:

  ```sql
  -- Tìm tên của mỗi nhân viên và số lượng dự án họ đang làm việc
   SELECT 
        Name
        , (
            SELECT COUNT(*)
            FROM Projects
            WHERE Projects.EmployeeID = Employees.EmployeeID
        ) AS ProjectCount
   FROM Employees;
  ```
- **Subquery với `EXISTS`**:

  ```sql
  -- Tìm tất cả các nhân viên có tham gia ít nhất một dự án
   SELECT *
   FROM Employees e
   WHERE EXISTS (
        SELECT 1
        FROM Projects p
        WHERE p.EmployeeID = e.EmployeeID
    );
  ```
- Ví dụ

```sql
WITH revenue AS(
    SELECT 
        c.customer_state
        , SUM(net_sales) AS net_sales
    FROM sales s
    LEFT JOIN customer c USING(customer_id)
    GROUP BY 1
    LIMIT 10
)
SELECT
    *
    , CASE
        WHEN customer_state IN(SELECT customer_state FROM revenue) THEN 'Big 10'
        ELSE 'Others'
        END AS revenue_segment
FROM customer
```

### CTE

- Bảng tạm thời mà có thể được tham chiếu trong câu lệnh SQL chính giúp cải thiện khả năng đọc hiểu và tổ chức mã SQL phức tạp
- Cú pháp 1 CTE
  ```sql
  WITH avg_salary_per_department AS (
      SELECT 
          department_id
          , AVG(salary) AS avg_salary
      FROM employees
      GROUP BY department_id
  )
  SELECT 
      e.employee_id 
      , e.first_name 
      , e.last_name 
      , e.salary
      , a.avg_salary
  FROM employees e
  JOIN avg_salary_per_department a
      ON e.department_id = a.department_id
  WHERE e.salary > a.avg_salary;
  ```
- Cú pháp nhiều CTE
  ```sql
  WITH orders_2023 AS (
      SELECT 
          customer_id 
          , SUM(total_amount) AS total_spent
      FROM orders
      WHERE YEAR(order_date) = 2023
      GROUP BY customer_id
  ),
  customer_totals AS (
      SELECT 
          c.customer_id
          , c.customer_name 
          , o.total_spent
      FROM customers c
      JOIN orders_2023 o
          ON c.customer_id = o.customer_id
  )
  SELECT 
      customer_id 
      , customer_name 
      , total_spent
  FROM customer_totals
  ORDER BY total_spent DESC;
  ```
- Cú pháp đệ quy
  ```sql
  WITH RECURSIVE subordinates AS (
      SELECT 
          employee_id
          , manager_id 
          , first_name 
          , last_name
      FROM employees
      WHERE manager_id = 1  -- Giả sử 1 là ID của nhân viên cấp cao nhất
      UNION ALL
      SELECT 
          e.employee_id 
          , e.manager_id 
          , e.first_name 
          , e.last_name
      FROM employees e
      INNER JOIN subordinates s ON s.employee_id = e.manager_id
  )
  SELECT 
      employee_id
      , manager_id 
      , first_name 
      , last_name
  FROM subordinates;
  ```

### SUMIF và COUNTIF

- Sử dụng các `COUNT()` hoặc `SUM()` với `CASE WHEN` để gom nhóm theo cột -> ứng dụng làm pivot table
- Cú pháp :
  ```sql
  WITH summary AS(
      SELECT 
          c.customer_source
          , SUM(net_sales) AS total
          , SUM(CASE WHEN EXTRACT(YEAR FROM created_at) = 2018 THEN s.net_sales END) AS "2018"
          , SUM(CASE WHEN EXTRACT(YEAR FROM created_at) = 2019 THEN s.net_sales END) AS "2019"
      FROM sales s
      LEFT JOIN customer c USING(customer_id)
      GROUP BY 1
  )
  SELECT 
      *
      , "2019" / "2018" - 1 AS yoy
  FROM summary
  ```

### Window Function

- Tính toán hoặc xếp hạng, so sánh trên 1 tập con,thay vì cả bảng
- `RANK()` : xếp hạng các record
  - Cú pháp

  ```sql
  WITH net_sales_category AS(
      SELECT
          customer_source
          , category
          , SUM(net_sales) AS total
      FROM sales 
      GROUP BY 1, 2
  )
  -- Window function trên cả bảng
  SELECT
      *
      , RANK() OVER(ORDER BY total DESC) AS ranking -- Order by dùng cho window function sắp xếp theo
  FROM net_sales_category 
  ORDER BY customer_source -- Order by dùng để sắp xếp kq của bảng hiển thị ra

  -- Window function trên từng customer_source
  SELECT
      *
      , RANK() OVER(PARTITION BY customer_source ORDER BY total DESC) AS ranking -- Partition by chia từng customer_source khác nhau
  FROM net_sales_category 

  -- Window Function tính tỉ lệ đóng góp của từng customer_source 
  SELECT
      *
      , total / SUM(net_sales) OVER(PARTITION BY customer_source) AS share_of_source
  FROM net_sales_category
  ```
- `Moving Average()` : Trung bình động -> làm dịu đi những biến động quá lớn -> nhìn rõ được xu hướng của biểu đồ hơn
- Dùng `ROWS/GROUP/RANGE BETWEEN ... AND ...` để thực hiện trung bình động cũng như kiểm soát các dòng của window frame
  - Cú pháp

  ```sql
  WITH by_day AS(
      SELECT
          DATE_TRUNC('DAY', created_at) AS created_day
          , SUM(net_sales) AS net_sales
      FROM sales 
      GROUP BY 1
  )
  SELECT
      *
      , AVG(net_sales) OVER(ORDER BY created_at ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
      , AVG(net_sales) OVER(ORDER BY created_at ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      , AVG(net_sales) OVER(ORDER BY created_at ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING)
      , AVG(net_sales) OVER(ORDER BY created_at ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
  FROM by_day
  ORDER BY 1
  ```

## Keywords

- `ADD` : Adds a column in an existing table
- `ADD CONSTRAINT` : Adds a constraint after a table is already created
- `ALL` : Returns true if all of the subquery values meet the condition
- `ALTER` : Adds, deletes, or modifies columns in a table, or changes the data type of a column in a table
  - `ALTER COLUMN`
  - `ALTER TABLE`
- `ANY` : Returns true if any of the subquery values meet the condition
- `BACKUP DATABASE` : Creates a back up of an existing database
- `CREATE INDEX` : Creates an index on a table (allows duplicate values)
  - `CREATE UNIQUE INDEX` : Creates a unique index on a table (no duplicate values)
- ` CREATE VIEW` : Creates a view based on the result set of a SELECT statement
  - `CREATE OR REPLACE VIEW` : Updates a view
- `CREATE PROCEDURE` : Creates a stored procedure
- `DEFAULT` : A constraint that provides a default value for a column
- `EXEC` : Executes a stored procedure
- `EXISTS` : Tests for the existence of any record in a subquery
