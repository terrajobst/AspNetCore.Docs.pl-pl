# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Dodawanie modelu do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tom Dykstra](https://github.com/tdykstra)

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie "**M**odelu" wchodzi w skład **M**VC aplikacji.

Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych. EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać. [EF Core obsługuje wiele baz danych](/ef/core/providers/).

Klasy modeli, które zostaną utworzone są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core. Określają one po prostu właściwości danych, które będą przechowywane w bazie danych.

W tym samouczku przedstawiono tworzenie klas modelu najpierw i programem EF Core spowoduje utworzenie bazy danych. Alternatywnym podejściu, nieuwzględnione w tym miejscu jest do generowania klasy modelu z bazy danych już istnieje. Aby uzyskać informacji na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Dodaj klasę modelu danych
