# Exception: `System.LimitException`

## Reason
Occurs when Salesforce governor limits are exceeded, such as:
- Exceeding CPU time limits.
- Exceeding heap size limits.
- Exceeding SOQL/DML row limits.
- Exceeding recursive trigger depth.

## Why It Can't Be Caught
Governor limits are enforced at the system level to maintain the platform’s multitenant architecture. Once a limit is breached, execution halts immediately.

---

# Way(s) to Reproduce

### 1. Exceeding CPU Time Limit
```apex
// Infinite loop consuming CPU time
while (true) {
    System.debug('This will exceed the CPU time limit');
}
```

### 2. Exceeding Heap Size Limit
```apex
// Creating a massive data structure
List<String> largeList = new List<String>();
for (Integer i = 0; i < 1000000; i++) {
    largeList.add('This is a large dataset');
}
```

---
## Possible Solutions
### Exceeding CPU time limits.
- Use Asynchronous Processing: Offload heavy operations to asynchronous Apex (Batch, Queueable).

### Exceeding heap size limits.
- Process in smaller chunks. 
- Use methods creatively, so that post execution heap memory is released. 
- Use `@ReadOnly` annotation wherever possible.  

### Exceeding SOQL/DML row limits.
- Use best practices to handle large data. 
- Use Asynchronous Processing: Offload heavy operations to asynchronous Apex (Batch, Queueable).

### Exceeding recursive trigger depth.
- Monitor Limits: Use the Debug Logs to identify areas that are approaching governor limits.

--- 
## Example Code
### Optimized for CPU Time
```apex
// Avoid infinite loops by setting proper conditions
for (Integer i = 0; i < 100; i++) {
    System.debug('Optimized iteration: ' + i);
}
```

### Optimized for Heap Size
```apex
// Reduce heap usage by processing smaller chunks
List<String> optimizedList = new List<String>{'Small', 'Chunk', 'Data'};
System.debug('Processed data: ' + optimizedList);
```
