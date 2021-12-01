

ACID:

- Atomicity: all actions in the transaction happen or none happen.
- Consistency: if each transaction is consistent and database is consistent in the beginning, it is guaranteed to be consistent when the transaction completes.
- Isolation: the execution of one transaction is isolated from that of other transactions.
- Durability: If a transaction commits, then its effects on the database persist.

For atomicity, there are two ways: shadow paging or logging. DBMS makes copies of pages and transaction make changes to those copies, only become visible when the transaction commits. DBMS could logs all actions so that it can undo the actions of aborted transactions. 

For consistency, DBMS will make sure it return correct results. Database consistency means it can accurately represents the real world entity and the future transaction could see the effect of past committed transaction correctly. Transaction consistency make sure DB consistency after executing transaction.

For Isolation, transaction will not see other concurrent transaction's effect. It is equivalent to a system where transactions are executed in serial order. That's why we need concurrency control. It tells DBMS how to interleaving of operations from multiple transactions. There are two categories of concurrency control: pessimistic and optimistic. Pessimistic assume transactions will conflict, optimistic will assume conflicts are rare.
