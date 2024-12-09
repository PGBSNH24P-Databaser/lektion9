---
author: Lektion 9
date: MMMM dd, YYYY
paging: "%d / %d"
---

# Lektion 9

Hej och välkommen!

## Agenda

1. Frågor och repetition
2. Introduktion till SQL transaktioner
3. Fortsättning på Reddit klon
4. Design övning i helklass
5. Eget arbete med handledning

---

# Introduktion till SQL transaktioner

```sql
CREATE TABLE accounts (
  name TEXT PRIMARY KEY,
  balance DECIMAL CHECK (balance >= 0)
);

BEGIN;
UPDATE accounts SET balance = balance - 500.0 WHERE name = 'Ironman';
UPDATE accounts SET balance = balance + 500.0 WHERE name = 'Superman';
COMMIT;

-- ROLLBACK kan användas för att manuellt gå tillbaka
-- COMMIT kan användas flera gånger ifall man vill ha "save points"
```

---

# Isolation nivåer

## Read committed

Data är endast tillgänglig om "committed". Om två transaktioner körs samtidigt så vet de inte om varandra.

## Repeatable read

Tar en "snapshot" av databasen i början och använder endast den datan. Om två transaktioner körs samtidigt kan en "faila".

## Serializable

Endast en transaction kan köras i taget. De "köas" för att köras efter varandra.

---

# Design övning: Discord klon
