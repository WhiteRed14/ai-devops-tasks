Stwórz prosty diagram ASCII Art przedstawiający połączenie klienta, serwera aplikacji i bazy danych.

Oto prosty diagram ASCII Art przedstawiający połączenie między klientem, serwerem aplikacji i bazą danych:

```
+----------+     HTTP/HTTPS     +----------------+     SQL/NoSQL     +-------------+
|          | -----------------> |                | -----------------> |             |
|  Klient  |                    | Serwer Aplikacji|                    | Baza Danych |
|          | <----------------- |                | <----------------- |             |
+----------+     Response       +----------------+     Wyniki        +-------------+
```

W tym diagramie:
- Strzałki pokazują kierunek przepływu danych
- Klient komunikuje się z serwerem aplikacji przez protokół HTTP/HTTPS
- Serwer aplikacji komunikuje się z bazą danych przez protokoły SQL lub NoSQL
- Każda strzałka ma etykietę opisującą typ przesyłanych danych

Dodaj do diagramu opisy elementów i kierunki przepływu danych.

Oto rozszerzony diagram ASCII Art z opisami elementów i kierunkami przepływu danych:

```
+------------------+     HTTP/HTTPS     +----------------------+     SQL/NoSQL     +------------------+
|                  | -----------------> |                      | -----------------> |                  |
|     Klient       |                    |    Serwer Aplikacji  |                    |    Baza Danych   |
| (Przeglądarka,   |                    | (Node.js, Python,    |                    | (MySQL, MongoDB, |
|  Aplikacja)      | <----------------- |  Java, itp.)         | <----------------- |  PostgreSQL)     |
+------------------+     Response       +----------------------+     Wyniki        +------------------+

Kierunek przepływu danych:
1. Klient wysyła żądanie HTTP/HTTPS do serwera
2. Serwer przetwarza żądanie i komunikuje się z bazą danych
3. Baza danych zwraca wyniki do serwera
4. Serwer przetwarza dane i wysyła odpowiedź do klienta
```

W tym rozszerzonym diagramie:
- Dodano przykłady technologii w każdym komponencie
- Dodano opis kierunku przepływu danych
- Zachowano czytelność strzałek pokazujących przepływ
- Dodano etykiety dla typów przesyłanych danych
