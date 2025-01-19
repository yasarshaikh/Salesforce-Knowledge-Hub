# Exception: `System.QueryException: List has multiple rows for assignment to SObject`

## Reason
Occurs when a SOQL query that is expected to return a single sObject returns multiple rows.

---

## Way(s) to Reproduce
Execute the following code in the Developer Console's anonymous window, ensuring the query matches multiple records:

```apex
// Attempting to assign a query result to a single sObject when multiple records match
Account acc = [SELECT Id, Name FROM Account WHERE Id != null];
```

## Possible Fix
Use a LIMIT clause to ensure only one record is returned, or assign the query result to a list and process multiple records as needed.

---

## Example Code

```apex
// Using LIMIT to restrict results to one record
Account acc = [SELECT Id, Name FROM Account WHERE Industry = 'Technology' LIMIT 1];
System.debug('Single Account Found: ' + acc.Name);

// Using a list to handle multiple records
List<Account> accList = [SELECT Id, Name FROM Account WHERE Industry = 'Technology'];
for (Account acc : accList) {
    System.debug('Account Found: ' + acc.Name);
}
```
