---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: "Przeciągnij i upuść za pośrednictwem ReorderList (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Bieżąca kolejność listy są..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6afecfc7330647e6f4944c507e308afec6d2401b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="drag-and-drop-via-reorderlist-c"></a>Przeciągnij i upuść za pośrednictwem ReorderList (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Bieżąca kolejność listy jest utrwalony na serwerze.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawie narzędzi programu AJAX formant zawiera listę można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Bieżąca kolejność listy jest utrwalony na serwerze.

## <a name="steps"></a>Kroki

`ReorderList` Sterowanie obsługuje powiązanie danych z bazy danych do listy. Najlepsze obsługuje ona również zapisywanie zmian w kolejności element listy do magazynu danych.

W przykładzie użyto magazynu danych programu Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalne (i wolne) część instalacji programu Visual Studio, w tym wersja express. Jest również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

Najprostszym sposobem konfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Nawiąż połączenie z serwerem, kliknij go dwukrotnie `Databases` i utworzyć nową bazę danych (kliknij prawym przyciskiem myszy i wybierz polecenie `New Database`) o nazwie `Tutorials`.

W tej bazie danych, Utwórz nową tabelę o nazwie `AJAX` następujące cztery kolumny:

- `id`(podstawowego klucza, liczbę całkowitą z zakresu tożsamości, not NULL)
- `char`(char(1), NULL)
- `description`(varchar(50), NULL)
- `position`(int, NULL)


[![Układ tabeli AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image3.png))


Następnie należy wypełnić tabeli kilka wartości. Należy pamiętać, że `position` kolumny posiada kolejność sortowania elementów.


[![Początkowe dane w tabeli AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Następnym krokiem wymaga, aby wygenerować `SqlDataSource` formantu do komunikowania się z nową bazę danych i tabeli. Źródło danych musi obsługiwać `SELECT` i `UPDATE` poleceń SQL. Po zmianie kolejności elementów listy `ReorderList` formant automatycznie przesyła dwóch wartości w źródle danych `Update` polecenia: Nowa pozycja i identyfikator elementu. W związku z tym potrzeb źródła danych `<UpdateParameters>` te dwie wartości w sekcji:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

`ReorderList` Wymaga formantu można ustawić następujące atrybuty:

- `AllowReorder`: Czy można przestawiać elementy listy
- `DataSourceID`: Identyfikator źródła danych
- `DataKeyField`: Nazwa kolumny klucza podstawowego w źródle danych
- `SortOrderField`: Kolumnie źródła danych udostępniający porządek sortowania dla elementów listy

W `<DragHandleTemplate>` i `<ItemTemplate>` sekcje, układ listy można dostosować. Ponadto możliwe jest wiązania z danymi przy użyciu `Eval()` metody, jak pokazano poniżej:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Informacje o stylu CSS następujące (zawartymi w `<DragHandleTemplate>` sekcji `ReorderList` kontroli) upewnia się, że wskaźnik myszy zmienia się odpowiednio po jest przemieszczane nad przeciągnij uchwyt:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Na koniec `ScriptManager` formant inicjuje ASP.NET AJAX dla strony:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Uruchom w tym przykładzie w przeglądarce i nieco rozmieszczanie elementów listy. Następnie ponownie załaduj stronę i/lub przyjrzeć bazy danych. Zmieniony pozycji została zachowana i odzwierciedlenie według wartości w `position` kolumny w bazie danych, a wszystko to bez żadnego kodu tylko za pomocą znacznika.


[![Kolejność elementów danych zgodnie z listą nowych zmian w bazie danych](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Kolejność elementów danych zgodnie z listą nowych zmian w bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image9.png))

>[!div class="step-by-step"]
[Poprzednie](using-postbacks-with-reorderlist-cs.md)
[dalej](using-postbacks-with-reorderlist-vb.md)
