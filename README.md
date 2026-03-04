Here are the SQL `CREATE TABLE` statements corresponding to the relational schema in the diagram.

# SQL Table Creation

## TYPE

```sql
CREATE TABLE TYPE (
    Type_Id INT PRIMARY KEY,
    Type_Name VARCHAR(100) NOT NULL
);
```

## HOTEL

```sql
CREATE TABLE HOTEL (
    Hotel_Id INT PRIMARY KEY,
    Hotel_Name VARCHAR(150) NOT NULL,
    Type_Id INT NOT NULL,

    CONSTRAINT fk_hotel_type
        FOREIGN KEY (Type_Id)
        REFERENCES TYPE(Type_Id)
        ON UPDATE CASCADE
        ON DELETE RESTRICT
);
```

## CATEGORY

```sql
CREATE TABLE CATEGORY (
    Category_Id INT PRIMARY KEY,
    Category_Name VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    Beds_numbers INT NOT NULL
);
```

## ROOM

```sql
CREATE TABLE ROOM (
    Room_Id INT PRIMARY KEY,
    Floor INT NOT NULL,
    Hotel_Id INT NOT NULL,
    Category_Id INT NOT NULL,

    CONSTRAINT fk_room_hotel
        FOREIGN KEY (Hotel_Id)
        REFERENCES HOTEL(Hotel_Id)
        ON UPDATE CASCADE
        ON DELETE CASCADE,

    CONSTRAINT fk_room_category
        FOREIGN KEY (Category_Id)
        REFERENCES CATEGORY(Category_Id)
        ON UPDATE CASCADE
        ON DELETE RESTRICT
);
```

## EMPLOYEE (with Recursive Relationship)

```sql
CREATE TABLE EMPLOYEE (
    Employee_Id INT PRIMARY KEY,
    Employee_Name VARCHAR(150) NOT NULL,
    Employee_Speciality VARCHAR(100),
    Hotel_Id INT NOT NULL,
    Leader_Id INT,

    CONSTRAINT fk_employee_hotel
        FOREIGN KEY (Hotel_Id)
        REFERENCES HOTEL(Hotel_Id)
        ON UPDATE CASCADE
        ON DELETE CASCADE,

    CONSTRAINT fk_employee_leader
        FOREIGN KEY (Leader_Id)
        REFERENCES EMPLOYEE(Employee_Id)
        ON UPDATE CASCADE
        ON DELETE SET NULL
);
```

# What This Implements

✔ 1:N → TYPE → HOTEL
✔ 1:N → HOTEL → ROOM
✔ 1:N → CATEGORY → ROOM
✔ 1:N → HOTEL → EMPLOYEE
✔ Recursive 1:N → EMPLOYEE leads EMPLOYEE

---

# Optional Improvements (If Required in Exam)

You could also add:

```sql
AUTO_INCREMENT      -- MySQL
SERIAL              -- PostgreSQL
UNIQUE constraints
CHECK constraints (Price > 0, Beds_numbers > 0)
```
