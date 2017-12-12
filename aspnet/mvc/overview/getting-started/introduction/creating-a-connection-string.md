---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Tworzenie parametrów połączenia i pracy z bazy danych LocalDB programu SQL Server | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 41f1f30d86406580ab9fc7278a94d9c291913f9a
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/19/2017
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i pracy z bazy danych LocalDB programu SQL Server
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i pracy z bazy danych LocalDB programu SQL Server

`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się. Faktycznie nie trzeba określać bazę danych do użycia, domyślnie zostanie ustawiona przy użyciu programu Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). W tej sekcji jawnie dodamy parametry połączenia w *Web.config* pliku aplikacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) jest wersja programu SQL Server Express aparat bazy danych rozpoczyna się na żądanie, która działa w trybie użytkownika. LocalDB działa w trybie wykonywania specjalnych programu SQL Server Express, który umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj są przechowywane pliki bazy danych LocalDB w *aplikacji\_danych* folderu projektu sieci web.

SQL Server Express nie jest zalecane do użycia w aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie stosuje się do produkcji z aplikacją sieci web, ponieważ nie jest przeznaczony do pracy z usługami IIS. Niemniej jednak bazy danych LocalDB można łatwo migracji do programu SQL Server lub SQL Azure.

W programie Visual Studio 2017 r. LocalDB jest instalowany domyślnie z programem Visual Studio.

Domyślnie program Entity Framework wyszukuje ciąg połączenia o nazwie takiej jak klasa kontekst obiektu (`MovieDBContext` dla tego projektu). Aby uzyskać więcej informacji, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Otwórz katalog główny aplikacji *Web.config* pliku pokazano poniżej. (Nie *Web.config* w pliku *widoków* folderu.)

![](creating-a-connection-string/_static/image1.png)

Znajdź `<connectionStrings>` elementu:

![](creating-a-connection-string/_static/image2.png)

Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Parametry połączenia dwóch są bardzo podobne. Pierwszy ciąg połączenia o nazwie `DefaultConnection` i jest używana dla bazy danych członkostwa w celu kontrolowania, kto może uzyskać dostęp do aplikacji. Określa ciąg połączenia został dodany, bazy danych LocalDB *Movie.mdf* znajduje się w *aplikacji\_danych* folderu. Firma Microsoft nie będzie używać bazy danych członkostwa w tym samouczku, aby uzyskać więcej informacji na członkostwo, uwierzytelniania i zabezpieczeń, zobacz Moje samouczek [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Nazwa ciągu połączenia musi być zgodna nazwa [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Faktycznie nie trzeba było dodać `MovieDBContext` parametry połączenia. Jeśli nie określisz parametry połączenia programu Entity Framework utworzy bazy danych LocalDB w katalogu użytkowników z w pełni kwalifikowana nazwa [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy (w tym przypadku `MvcMovie.Models.MovieDBContext`). Można określić nazwę bazy danych niczego Ci się podoba, jak długo ma *. MDF* sufiks. Na przykład można nazwę bazy danych *MyFilms.mdf*.

Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.

>[!div class="step-by-step"]
[Poprzednie](adding-a-model.md)
[dalej](accessing-your-models-data-from-a-controller.md)
