# Exception: `System.QueryException: List has no rows for assignment to SObject`

## Reason
Occurs when a SOQL query that is expected to return a single sObject returns no records.

---

## Way(s) to Reproduce
Execute the following code in the Developer Console's anonymous window:

```apex
// Attempting to assign a query result to a single sObject when no records match
Account acc = [SELECT Id, Name FROM Account WHERE Name = 'NonExistentName'];
```

---

## Possible Fix

Use a try-catch block to handle the exception or assign the query result to a list and check if it's empty.

---

## Example Code

```apex
// Using try-catch
try {
    Account acc = [SELECT Id, Name FROM Account WHERE Name = 'NonExistentName'];
    System.debug('Account Found: ' + acc.Name);
} catch (System.QueryException e) {
    System.debug('No Account Found.');
}

// Using a list
List<Account> accList = [SELECT Id, Name FROM Account WHERE Name = 'NonExistentName'];
if (!accList.isEmpty()) {
    Account acc = accList[0];
    System.debug('Account Found: ' + acc.Name);
} else {
    System.debug('No Account Found.');
}
```
