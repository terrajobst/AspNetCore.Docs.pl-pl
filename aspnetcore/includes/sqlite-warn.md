> [!NOTE]
> 
> **Ograniczenia oprogramowania SQLite**
>
> Ten samouczek używa funkcji *migracji* Entity Framework Core, jeśli jest to możliwe. Migracja aktualizuje schemat bazy danych, aby pasował do zmian w modelu danych. Jednak tylko te zmiany są obsługiwane przez aparat bazy danych, a możliwości zmiany schematu oprogramowania SQLite są ograniczone. Na przykład, Dodawanie kolumny jest obsługiwane, ale usuwanie kolumny nie jest obsługiwane. Jeśli migracja zostanie utworzona w celu usunięcia kolumny, polecenie `ef migrations add` zakończy się pomyślnie, ale polecenie `ef database update` kończy się niepowodzeniem. 
>
> Obejście ograniczeń oprogramowania SQLite polega na ręcznym pisaniu kodu migracji w celu przetworzenia odbudowy tabeli, gdy coś w tabeli ulegnie zmianie. Kod przejdzie do `Up` i `Down` metod migracji i będzie obejmował:
>
> * Tworzenie nowej tabeli.
> * Kopiowanie danych ze starej tabeli do nowej tabeli.
> * Porzucenie starej tabeli.
> * Zmiana nazwy nowej tabeli.
>
> Pisanie kodu specyficznego dla bazy danych tego typu jest poza zakresem tego samouczka. Zamiast tego ten samouczek opuszcza i ponownie tworzy bazę danych za każdym razem, gdy próba zastosowania migracji zakończy się niepowodzeniem. Więcej informacji zawierają następujące zasoby:
>
> * [Ograniczenia dostawcy bazy danych EF Core SQLite](/ef/core/providers/sqlite/limitations)
> * [Dostosowywanie kodu migracji](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Rozmieszczanie danych](/ef/core/modeling/data-seeding)
> * [Instrukcja ALTER TABLE w programie SQLite](https://sqlite.org/lang_altertable.html)