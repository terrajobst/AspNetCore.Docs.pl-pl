---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 73a3149c8c789221baa2e3804ebf15ed5a509ddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752089"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server

`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie. Faktycznie nie trzeba określać bazę danych, która do użycia, będą domyślnie przy użyciu platformy Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). W tej sekcji dodasz jawnie parametrów połączenia w *Web.config* pliku aplikacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) to Uproszczona wersja aparatu programu SQL Server Express bazy danych rozpoczyna się na żądanie, która działa w trybie użytkownika. LocalDB działa w specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj pliki bazy danych LocalDB są przechowywane w *aplikacji\_danych* folderu projektu sieci web.

SQL Server Express nie jest zalecane do użytku w aplikacjach sieci web w środowisku produkcyjnym. LocalDB w szczególności nie należy używać w środowisku produkcyjnym z aplikacją sieci web ponieważ nie jest przeznaczony do pracy z usługami IIS. Jednak bazy danych LocalDB mogą zostać łatwo zmigrowane do programu SQL Server lub SQL Azure.

W programie Visual Studio 2017 LocalDB jest instalowany domyślnie z programem Visual Studio.

Domyślnie platforma Entity Framework szuka parametrów połączenia o nazwie taka sama jak klasa kontekstu obiektu (`MovieDBContext` dla tego projektu). Aby uzyskać więcej informacji, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Otwórz katalog główny aplikacji *Web.config* pliku pokazano poniżej. (Nie *Web.config* w pliku *widoków* folderu.)

![](creating-a-connection-string/_static/image1.png)

Znajdź `<connectionStrings>` elementu:

![](creating-a-connection-string/_static/image2.png)

Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Dwa parametry połączenia są bardzo podobne. Pierwszy ciąg połączenia o nazwie `DefaultConnection` i jest używana dla bazy danych członkostwa w celu kontrolowania, kto może uzyskiwać dostęp do aplikacji. Parametry połączenia zostały dodane Określa nazwę bazy danych LocalDB *Movie.mdf* na terenie *aplikacji\_danych* folderu. Firma Microsoft nie będzie używać bazy danych członkostwa w ramach tego samouczka, aby uzyskać więcej informacji na temat członkostwa, uwierzytelniania i zabezpieczeń, zobacz Moje samouczek [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Nazwa ciągu połączenia musi odpowiadać nazwa [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Nie jest potrzebna do dodania `MovieDBContext` parametry połączenia. Jeśli nie określisz ciąg połączenia programu Entity Framework spowoduje utworzenie bazy danych LocalDB w katalogu użytkowników z w pełni kwalifikowana nazwa [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy (w tym przypadku `MvcMovie.Models.MovieDBContext`). Nazwać w bazie danych dowolny lubisz, tak długo, jak przedstawiono w nim *. MDF* sufiks. Na przykład, użyjemy nazwy bazy danych *MyFilms.mdf*.

Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](accessing-your-models-data-from-a-controller.md)
