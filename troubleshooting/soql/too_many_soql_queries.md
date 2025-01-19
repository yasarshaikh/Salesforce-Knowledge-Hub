<p align="center">
  <strong><span style="color: orange;">🚧 DRAFT - Work in Progress 🚧</span></strong>
</p>

---


# Exception: `System.QueryException: Too many SOQL queries: 101` and `System.QueryException: Too many SOQL queries: 201`

## Reason
Occurs when a single transaction exceeds the governor limit for SOQL queries:
- 101 SOQL queries in a synchronous context
- 201 SOQL queries in an asynchronous context (e.g., Batch Apex, Future Methods, Queueable Apex).

---

## Way(s) to Reproduce

### Synchronous Context (101 SOQL Queries)
```apex
// Running SOQL queries inside a loop
for (Integer i = 0; i < 150; i++) {
    Account acc = [SELECT Id FROM Account LIMIT 1];
}
```

### Asynchronous Context (201 SOQL Queries)
```apex
// Batch Apex or Future method context
public class BatchExample implements Database.Batchable<sObject> {
    public Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator('SELECT Id FROM Account');
    }
    public void execute(Database.BatchableContext bc, List<Account> scope) {
        for (Integer i = 0; i < 300; i++) {
            List<Account> accounts = [SELECT Id FROM Account LIMIT 1];
        }
    }
    public void finish(Database.BatchableContext bc) {}
}
```

---

## Possible Fix 
Remove SOQL queries from loops and query all necessary data at once.
Use relationships to retrieve related data in a single query.
In asynchronous contexts, batch your operations effectively to avoid hitting limits.

--- 

## Example Code

### Fix for Synchronous Context
```apex
// Bulk query outside the loop
List<Account> accounts = [SELECT Id FROM Account LIMIT 150];
for (Account acc : accounts) {
    System.debug('Account ID: ' + acc.Id);
}
```

### Fix for Asynchronous Context
```apex
// Use scoped SOQL queries in Batch Apex
public class BatchExampleFixed implements Database.Batchable<sObject> {
    public Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator('SELECT Id FROM Account LIMIT 50000');
    }
    public void execute(Database.BatchableContext bc, List<Account> scope) {
        System.debug('Processing a batch of accounts...');
        // Process scoped data without additional queries
    }
    public void finish(Database.BatchableContext bc) {
        System.debug('Batch processing completed.');
    }
}
```


