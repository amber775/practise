CREATE TABLE employees(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL,
	salary VARCHAR(50) NOT NULL,
	department VARCHAR(50) NOT NULL
);

INSERT INTO employees(id,name,salary,department)
VALUES
	(1,'Alice',5000,'HR'),
	(2,'Bob',6000,'IT'),
	(3,'Charile',5500,'Finance');

SELECT * FROM employees

SELECT name,salary
FROM employees
ORDER BY salary DESC;

ALTER TABLE employees
ALTER COLUMN salary TYPE INTEGER USING CAST(salary AS INTEGER);

SELECT name
FROM employees
WHERE department = 'IT'
AND salary>5000;

SELECT department, AVG(salary)
FROM employees
GROUP BY department;

SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'employees';

INSERT INTO employees(id,name,salary,department)
VALUES
	(4,'Iris',10000,'HR'),
	(5,'Tina',8000,'IT'),
	(6,'Fuhao',5000,'Finance');

#查询每个部门工资最高的员工
SELECT department, name, salary as max_salary
FROM employees
WHERE salary in
(SELECT MAX(salary)
FROM employees
GROUP BY department);#感觉有点不对，如果max_salary刚好和某个值相等就不对

#正确答案
SELECT e.department, e.name, e.salary as max_salary
FROM employees e
WHERE e.salary =(
	SELECT MAX(salary)
	FROM employees
	WHERE department=e.department
);#先处理子查询，e.department单独代表每一个department

#查询高于公司平均工资的员工
SELECT name, salary, department
FROM employees
WHERE salary>(
	SELECT AVG(salary)
	FROM employees
)

#查找每个部门工资排名第二高的员工
WITH ranked_employees AS(
	SELECT name,salary,department,
	DENSE_RANK()OVER(
		PARTITION BY department
		ORDER BY salary DESC
		) 
	AS d_rank
	FROM employees)
SELECT name,salary,department
FROM ranked_employees
WHERE d_rank=2;
                   

