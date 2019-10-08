1. 175. combine two tables
```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;
```
