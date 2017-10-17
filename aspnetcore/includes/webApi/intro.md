## <a name="overview"></a>Omówienie

Oto interfejsu API, które zostaną utworzone:

|interfejs API | Opis    | Treść żądania    | Treść odpowiedzi   |
|--- | ---- | ---- | ---- |
|Pobierz /api/todo  | Pobierz wszystkie elementy zadań do wykonania | Brak | Tablica elementów do wykonania|
|Pobierz /api/zadania / {id}  | Pobierz element według Identyfikatora | Brak | Zadania do wykonania|
|POST/api/todo | Dodaj nowy element | Zadania do wykonania  | Zadania do wykonania |
|Umieść /api/zadania / {id} | Aktualizuj istniejący element&nbsp;  | Zadania do wykonania |  Brak |
|Usuń /api/zadania / {id}&nbsp;  &nbsp; | Usuń element&nbsp;  &nbsp;  | Brak  | Brak|

<br>

Na poniższym diagramie przedstawiono podstawowy projekt aplikacji.

![Klient jest reprezentowany jako pole po lewej stronie i przesyła żądanie i odbiera odpowiedzi z aplikacji, okno rysowane po prawej stronie. W polu aplikacji trzy pola reprezentują kontrolera, modelu i warstwa dostępu do danych. Żądanie wejścia kontrolera aplikacji, a operacje odczytu/zapisu są wykonywane między kontrolerem i warstwa dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](../../tutorials/first-web-api/_static/architecture.png)

* Klient znajduje się niezależnie od zużywa web API (aplikacji mobilnej, przeglądarki itp.). Firma Microsoft nie są zapisywanie klienta, w tym samouczku. Użyjemy [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) do testowania aplikacji.

* A *modelu* jest obiekt, który reprezentuje dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania. Modele są reprezentowane jako klas C#, znanej także jako **P**zwykły **O**ld **C**# **O**obiektu (POCOs).

* A *kontrolera* jest obiekt, który obsługuje HTTP żądania i tworzy odpowiedzi HTTP. Ta aplikacja będzie mieć pojedynczy kontroler.

* Aby zachować samouczka proste, aplikacji nie są używane trwałe bazy danych. Przykładowa aplikacja przechowuje elementy zadań do wykonania w bazie danych w pamięci.
