# Queries for Assignment-A5

> [!NOTE]
> Problem Statment: Write a PL/SQL code block to calculate the area of a circle for a value of radius varying from 5 to 9. Store the radius and the corresponding values of calculated area in an empty table named areas, consisting of two columns, radius and area.
> Note: Instructor will frame the problem statement for writing PL/SQL block in line with above statement.

## Creating table

```sql
CREATE TABLE areas (
  radius INT NOT NULL,
  area INT
);

```

## Procedure

```sql
DECLARE
  v_radius NUMBER; -- specify radius here since Live SQL cannot take input from user
  -- Eg. v_radius NUMBER := 5 will take radius 5 as input
  calcedArea NUMBER;
  invalidData EXCEPTION;
  psError EXCEPTION;

BEGIN
  -- if radius is less than 0, raise exception
  IF (v_radius < 0) THEN
    RAISE invalidData;
  ELSIF (v_radius NOT BETWEEN 5 AND 9) THEN
    RAISE psError;
  END IF;

  -- calc area
  calcedArea := 3.14 * v_radius * v_radius;
  DBMS_OUTPUT.PUT_LINE('For radius ' || v_radius || ' cm, the area is ' || calcedArea || ' sq. cm.');

  -- add data to table
  INSERT INTO areas VALUES (v_radius, calcedArea);
  DBMS_OUTPUT.PUT_LINE('Inserted values to areas database.');

EXCEPTION
  WHEN invalidData THEN
    DBMS_OUTPUT.PUT_LINE('Radius cannot be less than 0 cms. Please enter a valid value.');
  WHEN psError THEN
    DBMS_OUTPUT.PUT_LINE('Problem statement requires the radius to be between 5 and 9 cms.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An error occured. Error: ' || SQLERRM);

END;
/

```
