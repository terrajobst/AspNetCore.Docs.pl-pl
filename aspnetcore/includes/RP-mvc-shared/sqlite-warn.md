
> [!NOTE]
> W tym samouczku użyjesz programu Entity Framework Core *migracje* funkcji, jeśli jest to możliwe. Migracje aktualizuje schemat bazy danych, aby dopasować zmiany w modelu danych. Jednak migracji można zrobić tylko rodzaje zmian, obsługiwane przez dostawcę programu EF Core i możliwości dostawcy bazy danych SQLite są ograniczone. Na przykład dodawanie kolumny jest obsługiwane, ale usuwanie lub zmienianie kolumny nie jest obsługiwana. Jeśli migracji została utworzona, aby usunąć lub zmienić kolumny, `ef migrations add` polecenie zakończy się pomyślnie, ale `ef database update` polecenie kończy się niepowodzeniem. Ze względu na te ograniczenia w tym samouczku nie używa migracji na zmiany schematu bazy danych SQLite. Zamiast tego po zmianie schematu, należy porzucić i ponownie utworzyć bazy danych.
>
>Obejście problemu w przypadku ograniczenia SQLite jest ręcznie napisać kod migracji do wykonywania odbudowie tabeli, gdy coś, co w przypadku zmiany w tabeli. Wymaga odbudowania indeksu tabeli:
>
>* Zmiana nazwy istniejącej tabeli.
>* Trwa tworzenie nowej tabeli.
>* Kopiowanie danych z tabeli starej do nowej tabeli.
>* Usunięcie starych tabeli.
>
>Aby uzyskać więcej informacji, zobacz następujące zasoby:
>
> * [SQLite EF Core Database Provider Limitations](/ef/core/providers/sqlite/limitations)
> * [Dostosowywanie kodu migracji](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Wstępne wypełnianie danych](/ef/core/modeling/data-seeding)