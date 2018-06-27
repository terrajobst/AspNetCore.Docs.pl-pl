# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Dodaj model do aplikacji platformy ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Dykstra niestandardowy](https://github.com/tdykstra)

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie "**M**odelu" część **M**VC aplikacji.

Użyj tych klas z [Entity Framework Core](/ef/core) (rdzeni EF) do pracy z bazą danych. Podstawowe EF to struktura mapowania obiektów relacyjnych (ORM) upraszczającym trzeba napisać kod dostępu do danych. [Jądro EF obsługuje wiele baz danych](/ef/core/providers/).

Klasy modeli, które zostaną utworzone są określane jako klasy POCO (od "zwykły stare obiekty CLR)", ponieważ nie ma żadnych zależności EF Core. Określają one tylko właściwości danych, które będą przechowywane w bazie danych.

W tym samouczku przedstawiono tworzenie klasy modelu najpierw, a EF Core utworzy bazę danych. Alternatywne podejście nieuwzględnionego w tym miejscu jest do generowania klasy modelu z bazy danych już istnieje. Informacje dotyczące tego podejścia, zobacz [platformy ASP.NET Core - istniejącej bazy danych](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Dodaj klasę modelu danych
