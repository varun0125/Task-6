# Task-6

-- Create Departments Table
CREATE TABLE Departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    salary INT,
    FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);

INSERT INTO Departments (department_id, department_name) VALUES
(1, 'Sales'),
(2, 'HR'),
(3, 'Engineering'),
(4, 'Support'),
(5, 'Security');


INSERT INTO Employees (id, name, department_id, salary) VALUES
(101, 'Alice', 1, 60000),
(102, 'Bob', 1, 55000),
(103, 'Charlie', 2, 50000),
(104, 'David', 3, 75000),
(105, 'Eve', 3, 72000),
(106, 'Frank', 4, 48000),
(107, 'Grace', 5, 52000),
(108, 'Heidi', 5, 53000),
(109, 'Ivan', 2, 51000);
SELECT 
    name,
    (SELECT department_name 
     FROM Departments 
     WHERE Departments.department_id = Employees.department_id) AS dept_name
FROM Employees;
SELECT name 
FROM Employees 
WHERE department_id IN (
    SELECT department_id 
    FROM Departments 
    WHERE department_name LIKE 'S%'
);
SELECT dept.department_name, avg_salaries.avg_salary
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM Employees
    GROUP BY department_id
) AS avg_salaries
JOIN Departments AS dept ON dept.department_id = avg_salaries.department_id;
SELECT name 
FROM Employees E1
WHERE salary > (
    SELECT AVG(salary)
    FROM Employees E2
    WHERE E2.department_id = E1.department_id
);
SELECT department_name 
FROM Departments D
WHERE EXISTS (
    SELECT 1
    FROM Employees E
    WHERE E.department_id = D.department_id
);
