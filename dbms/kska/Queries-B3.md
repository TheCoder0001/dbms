## Queries-B3

> [!NOTE]
> Use Employee database created in Assignment B-01 and perform following aggregation operation
> Refer [Queries-B1](https://git.kska.io/sppu-te-comp-content/DatabaseManagementSystems/src/branch/main/Practical/Assignment-B1/Queries-B1.md)

1. Return the Total Salary of per Company
```mongodb
db.Employee.mapReduce(
    function() {
        emit(this.Company_name, this.Salary);
    },
    function(key, values) {
        return Array.sum(values);
    },
    { out: "total_salary_per_company" }
);

```

2. Return the Total Salary of Company Name:"Oscorp"
```mongodb
db.Employee.mapReduce(
    function() {
        if (this.Company_name === "Oscorp") {
            emit(this.Company_name, this.Salary);
        }
    },
    function(key, values) {
        return Array.sum(values);
    },
    { out: "total_salary_oscorp" }
);

```

3. Return the Avg Salary of Company whose address is “Pune".
```mongodb
db.Employee.mapReduce(
    function() {
        if (this.Address.some(addr => addr.PAddr.includes("Pune"))) {
            emit("Pune", this.Salary);
        }
    },
    function(key, values) {
        return Array.avg(values);
    },
    { out: "avg_salary_pune" }
);

```

4. Return the Total Salary for each Designation of `Wayne Industries`.
```mongodb
db.Employee.mapReduce(
    function() {
        if (this.Company_name === "Wayne Industries") {
            emit(this.Designation, this.Salary);
        }
    },
    function(key, values) {
        return Array.sum(values);
    },
    { out: "total_salary_wayne" }
);

```

5. Return total count for “State=Maharashtra”
```mongodb
db.Employee.mapReduce(
    function() {
        this.Address.forEach(function(addr) {
            if (addr.LAddr === "Maharashtra") {
                emit("Maharashtra", 1);
            }
        });
    },
    function(key, values) {
        return Array.sum(values);
    },
    { out: "count_state_maharashtra" }
);

```

6. Return Count for State Telangana and Age greater than 40.
```mongodb
db.Employee.mapReduce(
    function() {
        if (this.Age > 40) {
            this.Address.forEach(function(addr) {
                if (addr.LAddr === "Telangana") {
                    emit("Telangana_Age_Above_40", 1);
                }
            });
        }
    },
    function(key, values) {
        return Array.sum(values);
    },
    { out: "count_telangana_age_above_40" }
);

```
