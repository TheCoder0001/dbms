# Queries-B1

## Creation

```mongodb
use empDB
db.createCollection("Employee")

```

## Inserting data

```mongodb
db.Employee.insertMany([
    {
        Empid: 1,
        Name: { FName: "Ayush", LName: "Kalaskar" },
        Company_name: "Oscorp",
        Salary: 50000,
        Designation: "Programmer",
        Age: 28,
        Expertise: ["Java", "Spring", "MongoDB"],
        DOB: "1995-04-15",
        Email_id: "ayush.kalaskar@oscorp.com",
        Contact: "9876543210",
        Address: [{ PAddr: "123, Street A, Pune", LAddr: "Maharashtra" }]
    },
    {
        Empid: 2,
        Name: { FName: "Himanshu", LName: "Patil" },
        Company_name: "Hammer Industries",
        Salary: 60000,
        Designation: "Developer",
        Age: 30,
        Expertise: ["JavaScript", "React", "Node.js"],
        DOB: "1993-06-25",
        Email_id: "himanshu.patil@hammerindustries.com",
        Contact: "9876543211",
        Address: [{ PAddr: "234, Street B, Bangalore", LAddr: "Karnataka" }]
    },
    {
        Empid: 3,
        Name: { FName: "Mehul", LName: "Patil" },
        Company_name: "Lex Corp.",
        Salary: 45000,
        Designation: "Tester",
        Age: 29,
        Expertise: ["Selenium", "Python"],
        DOB: "1994-08-12",
        Email_id: "mehul.patil@lexcorp.org",
        Contact: "9876543212",
        Address: [{ PAddr: "345, Street C, Hyderabad", LAddr: "Telangana" }]
    },
    {
        Empid: 4,
        Name: { FName: "Tanmay", LName: "Machkar" },
        Company_name: "Wayne Industries",
        Salary: 70000,
        Designation: "Project Manager",
        Age: 35,
        Expertise: ["Agile", "Scrum"],
        DOB: "1988-02-20",
        Email_id: "tanmay.machkar@batmobile.com",
        Contact: "9876543213",
        Address: [{ PAddr: "456, Street D, Chennai", LAddr: "Tamil Nadu" }]
    },
    {
        Empid: 5,
        Name: { FName: "Rajendra", LName: "Patil" },
        Company_name: "Stark Industries",
        Salary: 32000,
        Designation: "Programmer",
        Age: 27,
        Expertise: ["Java", "Angular"],
        DOB: "1996-03-30",
        Email_id: "rajendra.patil@starkindustries.com",
        Contact: "9876543214",
        Address: [{ PAddr: "567, Street E, Delhi", LAddr: "Delhi" }]
    },
    {
        Empid: 6,
        Name: { FName: "Rajesh", LName: "Patil" },
        Company_name: "Roxonn",
        Salary: 50000,
        Designation: "Designer",
        Age: 32,
        Expertise: ["Photoshop", "Illustrator"],
        DOB: "1991-11-11",
        Email_id: "rajesh.patil@roxonn.com",
        Contact: "9876543215",
        Address: [{ PAddr: "678, Street F, Kolkata", LAddr: "West Bengal" }]
    },
    {
        Empid: 7,
        Name: { FName: "Prashant", LName: "Kalaskar" },
        Company_name: "Y-Space",
        Salary: 45000,
        Designation: "Tester",
        Age: 26,
        Expertise: ["Selenium", "Java"],
        DOB: "1997-07-07",
        Email_id: "prashant.kalaskar@y-space.com",
        Contact: "9876543216",
        Address: [{ PAddr: "789, Street G, Pune", LAddr: "Maharashtra" }]
    },
    {
        Empid: 8,
        Name: { FName: "Aditya", LName: "Gundeti" },
        Company_name: "SNASA",
        Salary: 80000,
        Designation: "Architect",
        Age: 33,
        Expertise: ["Cloud", "Microservices"],
        DOB: "1990-01-01",
        Email_id: "aditya.gundeti@snasa.org",
        Contact: "9876543217",
        Address: [{ PAddr: "890, Street H, Noida", LAddr: "Uttar Pradesh" }]
    },
    {
        Empid: 9,
        Name: { FName: "Manoj", LName: "Patil" },
        Company_name: "MEPA",
        Salary: 40000,
        Designation: "Developer",
        Age: 31,
        Expertise: ["C#", ".NET"],
        DOB: "1992-05-05",
        Email_id: "manoj.patil@mepa.com",
        Contact: "9876543218",
        Address: [{ PAddr: "901, Street I, Jaipur", LAddr: "Rajasthan" }]
    },
    {
        Empid: 10,
        Name: { FName: "Afan", LName: "Shaikh" },
        Company_name: "Vought",
        Salary: 39000,
        Designation: "HR",
        Age: 29,
        Expertise: ["Recruitment", "Employee Relations"],
        DOB: "1994-09-09",
        Email_id: "afan.shaikh@vought.com",
        Contact: "9876543219",
        Address: [{ PAddr: "1234, Street J, Ahmedabad", LAddr: "Gujarat" }]
    }
])
```

## Queries

1. Select all documents where Designation is "Programmer" and Salary > 30000:
```mongodb
db.Employee.find({ Designation: "Programmer", Salary: { $gt: 30000 } })
```

2. Create a new document if no document contains `{Designation: "Tester", Company_name: "Y-Space", Age: 25}`:
```mongodb
db.Employee.update(
    { Designation: "Tester", Company_name: "Y-Space", Age: 25 },
    { $setOnInsert: { Empid: 11, Name: { FName: "Anil", LName: "Salvi" }, Salary: 30000, DOB: "1998-01-01", Email_id: "anil.salvi@y-space.com", Contact: "9876543220", Address: [{ PAddr: "123 Street", LAddr: "Mumbai" }] } },
    { upsert: true }
)

```

3. Select all documents where Age < 30 or Salary > 40000:
```mongodb
db.Employee.find({ $or: [{ Age: { $lt: 30 } }, { Salary: { $gt: 40000 } }] })

```

4. Match documents where Address contains city "Pune" and Pin_code "411001":
```mongodb
db.Employee.find({ Address: { $elemMatch: { city: "Pune", Pin_code: "411001" } } })

```

5. Find all documents with Company_name "Oscorp" and modify their salary by 2000:
```mongodb
db.Employee.updateMany(
    { Company_name: "Oscorp" },
    { $inc: { Salary: 2000 } }
)

```

6. Find documents where Designation is not equal to "Developer":
```mongodb
db.Employee.find({ Designation: { $ne: "Developer" } })

```

7. Find _id, Designation, Address, and Name where Company_name is "Wayne Industries":
```mongodb
db.Employee.find(
    { Company_name: "Wayne Industries" },
    { _id: 1, Designation: 1, Address: 1, Name: 1 }
)

```

8. Select all documents where Designation is either "Developer" or "Tester":
```mongodb
db.Employee.find({ Designation: { $in: ["Developer", "Tester"] } })

```

9. Find all documents with exact match on Expertise array:
```mongodb
db.Employee.find({ Expertise: { $all: ["Cloud", "Microservices"] } })

```

10. Drop single documents where Designation is "Developer":
```mongodb
db.Employee.deleteMany({ Designation: "Developer" })

```
