---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Umożliwia dodanie danych do bazy danych migracje Code First | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 33bc6d82daa9ca5f46452a1adf4e2eebea04fa6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Umożliwia migracje Code First inicjatora bazy danych
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w EF w celu umieszczenia bazy danych z danych testowych.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-3/samples/sample1.cmd)]

To polecenie dodaje folder o nazwie migracji do projektu, a także kod plik o nazwie Configuration.cs w folderze migracji.

![](part-3/_static/image1.png)

Otwórz plik Configuration.cs. Dodaj następujące **przy użyciu** instrukcji.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Następnie dodaj następujący kod, aby **Configuration.Seed** metody:

[!code-csharp[Main](part-3/samples/sample3.cs)]

W oknie Konsola Menedżera pakietów wpisz następujące polecenia:

[!code-console[Main](part-3/samples/sample4.cmd)]

Pierwsze polecenie generuje kod, który utworzy bazę danych, a drugie polecenie wykonuje kodu. Bazy danych jest tworzony lokalnie, przy użyciu [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Eksploruj interfejsu API (opcjonalnie)

Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania. Visual Studio uruchamiania usług IIS Express i uruchamia aplikację sieci web. Visual Studio następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji.

Po uruchomieniu projektu sieci web programu Visual Studio przypisuje numeru portu. Na poniższej ilustracji numer portu to 50524. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.

![](part-3/_static/image3.png)

Strona główna jest implementowane za pomocą platformy ASP.NET MVC. W górnej części strony Brak linku "Interfejsu API". To łącze wprowadzono należy do strony pomocy generowane automatycznie dla interfejsu API sieci web. (Aby dowiedzieć się, jak jest generowany stronę pomocy i jak dodać własne dokumentacji do strony, zobacz [tworzenie pomocy stron ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Możesz kliknąć pomoc łączy strony, aby wyświetlić szczegółowe informacje o interfejsie API, w tym formacie żądań i odpowiedzi.

![](part-3/_static/image4.png)

Interfejs API umożliwia operacje CRUD na bazie danych. Poniżej przedstawiono podsumowanie interfejsu API.

| Autorzy |  |
| --- | -- |
| Pobierz interfejs api/autorów | Pobierz wszystkich autorów. |
| Interfejs api GET/autorów / {id} | Pobierz Autor według identyfikatora. |
| POST/api/autorów | Utwórz nowy autora. |
| Umieść /api/autorów / {id} | Aktualizowanie istniejących autora. |
| Usuń /api/autorów / {id} | Usuń autora. |

| Książki |  |
| --- | -- |
| Pobierz /api/books | Pobierz wszystkie źródłowe. |
| Pobierz /api/books / {id} | Pobierz książkę według identyfikatora. |
| POST/api/książek | Tworzenie nowej książki. |
| Umieść /api/books / {id} | Aktualizowanie istniejącej. |
| Usuń /api/books / {id} | Usuń książkę. |

## <a name="view-the-database-optional"></a>Widok bazy danych (opcjonalnie)

Po uruchomieniu polecenia Update-Database EF baza danych utworzona i wywołać `Seed` metody. Po uruchomieniu aplikacji lokalnie, używa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Bazy danych można wyświetlić w programie Visual Studio. Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server**.

![](part-3/_static/image5.png)

W **Połącz z serwerem** okna dialogowego, w **nazwy serwera** pole edycji, wpisz "(localdb) \v11.0". Pozostaw **uwierzytelniania** opcję "Uwierzytelnianie systemu Windows". Kliknij przycisk **Połącz**.

![](part-3/_static/image6.png)

Visual Studio łączy do LocalDB i przedstawia istniejących baz danych w oknie Eksplorator obiektów SQL Server. Można rozwinąć węzły, aby zobaczyć tabele, które EF utworzone.

![](part-3/_static/image7.png)

Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz **danych widoku**.

![](part-3/_static/image8.png)

Poniższy zrzut ekranu przedstawia wyniki dla tabeli książek. Zwróć uwagę, EF wypełnione w bazie danych inicjatora, czy tabela zawiera klucz obcy tabeli autorów.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Poprzednie](part-2.md)
> [dalej](part-4.md)
