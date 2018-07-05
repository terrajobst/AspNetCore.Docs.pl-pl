---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (VB) | Dokumentacja firmy Microsoft
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b10ecd4c061082aba5cefe9d92b8616e9f40e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389166"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Bieżący kolejność listy jest utrwalony na serwerze.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Bieżący kolejność listy jest utrwalony na serwerze.

## <a name="steps"></a>Kroki

`ReorderList` Kontrolka obsługuje powiązanie danych z bazy danych do listy. Przede wszystkim obsługuje ona również zapisywanie zmian w kolejności element listy do magazynu danych.

Ta próbka używa programu Microsoft SQL Server 2005 Express Edition do przechowywania danych. Baza danych jest opcjonalny (i bezpłatne) część instalacji programu Visual Studio, w tym wersja express. Jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

Najprostszym sposobem konfigurowania bazy danych jest używać programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Połączenie z serwerem, kliknij go dwukrotnie `Databases` i Utwórz nową bazę danych (kliknij prawym przyciskiem myszy i wybierz polecenie `New Database`) o nazwie `Tutorials`.

W tej bazie danych, Utwórz nową tabelę o nazwie `AJAX` następujące cztery kolumny:

- `id` (podstawowego klucza, liczbę całkowitą z zakresu tożsamości, not NULL)
- `char` (char(1), NULL)
- `description` (varchar(50), NULL)
- `position` (int, NULL)


[![Układ tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image3.png))


Następnie wypełnij tabelę przy użyciu kilku wartości. Należy pamiętać, że `position` kolumna zawiera kolejność sortowania elementów.


[![Początkowe dane w tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image6.png))


Następnym krokiem wymaga, aby wygenerować `SqlDataSource` formantu do komunikowania się z nową bazę danych i jego tabeli. Źródło danych musi obsługiwać `SELECT` i `UPDATE` poleceń SQL. Po zmianie kolejności elementów listy `ReorderList` formant automatycznie przesyła dwie wartości do źródła danych `Update` polecenia: nowe miejsce i identyfikator elementu. W związku z tym, źródła danych, potrzeb `<UpdateParameters>` sekcję, aby te dwie wartości:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList` Kontroli musi ustawić następujące atrybuty:

- `AllowReorder`: Czy można przestawiać elementów listy
- `DataSourceID`: Identyfikator źródła danych
- `DataKeyField`: Nazwa kolumny klucza podstawowego w źródle danych
- `SortOrderField`: Kolumny źródła danych zapewniająca porządek sortowania dla elementów listy

W `<DragHandleTemplate>` i `<ItemTemplate>` sekcjach układ listy można dostosować. Wiązanie danych jest także możliwe przy użyciu `Eval()` metody, jak pokazano tutaj:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Następujące arkusze CSS stylu informacji (zawartymi w `<DragHandleTemplate>` części `ReorderList` kontroli) upewnia się, że wskaźnik myszy zmienia się odpowiednio kiedy jest przemieszczane nad przeciągnij uchwyt:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Na koniec `ScriptManager` kontroli inicjuje ASP.NET AJAX dla strony:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Uruchomić ten przykład w przeglądarce i nieco zmienić kolejność elementów listy. Następnie ponownie załaduj stronę i/lub przyjrzenie się firmie bazy danych. Zmieniony pozycji została zachowana, a także są odzwierciedlane według wartości w `position` kolumny w bazie danych, które wszystko to bez żadnego kodu tylko za pomocą znaczników.


[![Dane w kolejności nowy element listy zmian w bazie danych](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Dane w zmian w bazie danych zgodnie z listą nowych elementów zamówienia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Poprzednie](using-postbacks-with-reorderlist-vb.md)
