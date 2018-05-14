przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji możesz dodawać klasy zarządzania filmów w bazie danych. Użyj tych klas z [Entity Framework Core](/ef/core) (rdzeni EF) do pracy z bazą danych. Podstawowe EF to struktura mapowania obiektów relacyjnych (ORM) upraszczającym trzeba napisać kod dostępu do danych.

Klasy modeli, które możesz utworzyć są określane jako klasy POCO (od "zwykły stare obiekty CLR)", ponieważ nie ma żadnych zależności EF Core. Określają one właściwości dane, które są przechowywane w bazie danych.

W tym samouczku pisania klasy modeli, a EF Core utworzy bazę danych. Alternatywne podejście nieuwzględnionego w tym miejscu jest [wygenerowane klasy modelu z istniejącej bazy danych](/ef/core/get-started/aspnetcore/existing-db).

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.