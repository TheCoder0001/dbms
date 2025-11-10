# Database Queries for Assignment-A7

## Creating tables

`O_RollCall` table:
```sql
CREATE TABLE O_RollCall(
name VARCHAR(255),
roll NUMBER(14),
class VARCHAR(255)
);

```

`N_RollCall` table:
```sql
CREATE TABLE N_RollCall(
name VARCHAR(255),
roll NUMBER(14),
class VARCHAR(255)
);

```

## Inserting data

`O_RollCall` table:
```sql
INSERT INTO O_RollCall VALUES ('Stewie', 1, 'Comp 1');
INSERT INTO O_RollCall VALUES ('Edie', 2, 'Comp 2');
INSERT INTO O_RollCall VALUES ('Stomp', 3, 'Comp 3');
INSERT INTO O_RollCall VALUES ('Lara', 4, 'Comp 1');
INSERT INTO O_RollCall VALUES ('Foxy', 5, 'Comp 2');

```

`N_RollCall` table:
```sql
INSERT INTO N_RollCall VALUES ('Stewie', 1, 'Comp 1');
INSERT INTO N_RollCall VALUES ('Edie', 2, 'Comp 2');
INSERT INTO N_RollCall VALUES ('Stomp', 3, 'Comp 3');
INSERT INTO N_RollCall VALUES ('Lara', 4, 'Comp 1');
INSERT INTO N_RollCall VALUES ('Foxy', 5, 'Comp 2');
INSERT INTO N_RollCall VALUES ('Gundeti', 6, 'Comp 3');
INSERT INTO N_RollCall VALUES ('Kalas', 7, 'Comp 2');

```

## Procedure

### Explicit cursor
```sql
DECLARE
  p_name VARCHAR(255);
  p_rollno NUMBER(15);
  p_class VARCHAR(255);
  CURSOR cc1 IS SELECT * FROM N_RollCall WHERE roll NOT IN (SELECT roll FROM O_RollCall);
BEGIN
  OPEN cc1;
    LOOP
      FETCH cc1 INTO p_name, p_rollno, p_class;
      INSERT INTO O_RollCall VALUES (p_name, p_rollno, p_class);
      EXIT WHEN cc1%notfound;
      DBMS_OUTPUT.PUT_LINE(p_name || ' ' || p_rollno || ' ' || p_class);
      END LOOP;
  CLOSE cc1;
END;
/

```

### Parameterized cursor
```sql
DECLARE
  p_name VARCHAR(255);
  p_rollno NUMBER(15);
  p_class VARCHAR(255);
  CURSOR pp1(roll1 NUMBER) IS SELECT * FROM N_RollCall WHERE roll > roll1;

BEGIN
  OPEN pp1(3);
    LOOP
      FETCH pp1 INTO p_name, p_rollno, p_class;
      EXIT WHEN pp1%notfound;
      DBMS_OUTPUT.PUT_LINE(p_name || ' ' || p_rollno || ' ' || p_class);
    END LOOP;
  CLOSE pp1;

END;
/

```

### Implicit Cursor
```sql
DECLARE
  total_rows NUMBER(2);

BEGIN
  UPDATE N_RollCall SET roll = roll + 1;
  IF sql%notfound THEN
    dbms_output.put_line('no roll was updated');
  ELSIF sql%found THEN
    total_rows := sql%rowcount;
    DBMS_OUTPUT.PUT_LINE( total_rows || ' roll calls were affected ');
  END IF;
END;
/

```
