# Database Queries for Assignment-A8

## Creating tables

`Library` table:
```sql
CREATE TABLE Library(
    id NUMBER(12),
    title VARCHAR(255),
    dateofissue DATE,
    author VARCHAR(255)
);

```

`Library_Audit` table:
```sql
CREATE TABLE Library_Audit(
    id NUMBER(12),
    title VARCHAR(255),
    dateofaction DATE,
    author VARCHAR(255),
    status VARCHAR(255)
);

```

## Inserting values

`Library` table:
```sql
INSERT INTO Library VALUES (1, 'Berserk', TO_DATE('2024-07-28','YYYY-MM-DD'), 'Prashant');
INSERT INTO Library VALUES (2, 'Dark', TO_DATE('2024-07-15','YYYY-MM-DD'), 'Rajendra');
INSERT INTO Library VALUES (3, 'Hannibal', TO_DATE('2024-07-20','YYYY-MM-DD'), 'Manoj');
INSERT INTO Library VALUES (4, 'AOT', TO_DATE('2024-07-30','YYYY-MM-DD'), 'Rajesh');
INSERT INTO Library VALUES (5, 'GOT', TO_DATE('2024-07-19','YYYY-MM-DD'), 'Anil');

```

## Trigger

```sql
CREATE OR REPLACE TRIGGER library_action
AFTER INSERT OR UPDATE OR DELETE ON Library
FOR EACH ROW
BEGIN
	IF INSERTING THEN
    	INSERT INTO Library_Audit (id, title, dateofaction, author, status) VALUES (:NEW.id, :NEW.title, current_timestamp, :NEW.author, 'Insert');
	ELSIF UPDATING THEN
        INSERT INTO Library_Audit (id, title, dateofaction, author, status) VALUES (:OLD.id, :OLD.title, current_timestamp, :OLD.author, 'Update');
	ELSIF DELETING THEN
        INSERT INTO Library_Audit (id, title, dateofaction, author, status) VALUES (:OLD.id, :OLD.title, current_timestamp, :OLD.author, 'Delete');
	END IF;
END;
/

```

## Testing trigger

### Insert operation

```sql
INSERT INTO Library VALUES (15, 'CREW', TO_DATE('2024-07-22','YYYY-MM-DD'), 'Ramesh');
INSERT INTO Library VALUES (14, 'Ninteen Eighty Four', TO_DATE('2024-07-01','YYYY-MM-DD'), 'Omkar');

SELECT * FROM Library;
SELECT * FROM Library_Audit;

```
### Update operation

```sql
UPDATE Library SET id = 6, title = 'Sherlock', author = 'Deepak' where id = 3;
UPDATE Library SET id = 7, title = 'MR. ROBOT', author = 'Varad' where id = 4;

SELECT * FROM Library;
SELECT * FROM Library_Audit;

```

### Delete operation

```sql
DELETE FROM Library WHERE id = 1;
DELETE FROM Library WHERE id = 5;

SELECT * FROM Library;
SELECT * FROM Library_Audit;

```

---
