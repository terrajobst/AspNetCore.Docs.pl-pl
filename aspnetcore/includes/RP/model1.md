Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji dodasz klasy zarządzania filmów w bazie danych. Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych. EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.

Klasy modeli, które możesz utworzyć są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core. Mogą określać właściwości danych, które są przechowywane w bazie danych.

W tym samouczku pisania klasy modeli i programem EF Core tworzy bazę danych. Alternatywnym podejściu, nieuwzględnione w tym miejscu jest [wygenerowane klasy modelu z istniejącej bazy danych](/ef/core/get-started/aspnetcore/existing-db).

[Wyświetlanie lub pobieranie](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.
