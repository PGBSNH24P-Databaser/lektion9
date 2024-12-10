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

- Kontosystem (användare och lösenord)
- Server kanaler (voice + text)
- Servers
- Privata meddelanden
- Text, bild, emojis i text kanal
- Pin messages
- Reagera på inlägg
- Användar roller (per server)
- Svar på andra inlägg, vidarebefodra andra inlägg
- Läsa/markera som oläst
- Notifikationer
- Vänlista

users:

- user_id
- name
- password

server:

- name

server_users (junction):

- user_id FK
- server_id FK

text-channels:

- name
- server_id FK

voice-channels:

- name
- server_id FK

roles:

- name
- server_id FK
- can_delete_channel BOOL
- can_mute_others BOOL
- can_kick BOOL
- can_ban BOOL

user-roles:

- user_id FK
- role_id FK

friends (junction):

- user_id_1 FK
- user_id_2 FK

messages:

- from_user_id FK
- to_user_id FK (NULLABLE) (null if channel)
- text_channel_id FK (NULLABLE) (null if private)
- pinned BOOL
- reply_message_id FK (NULLABLE)
- forward_message_id FK (NULLABLE)
- content TEXT

user_read_messages (junction):

- user_id
- message_id
