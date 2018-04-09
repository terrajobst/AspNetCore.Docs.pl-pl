---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> Ten samouczek przedstawia sposób użycia wiązania modelu warstwy logiki biznesowej. Element członkowski OnCallingDataMethods, aby określić, który można wywoływać metod danych obiektu innych niż bieżąca strona zostanie ustawiona.
> 
> W tym samouczku opiera się na projekcie utworzone w [wcześniej](retrieving-data.md) części serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.


## <a name="what-youll-build"></a>Będzie kompilacji

Wiązanie modelu umożliwia umieść swój kod interakcji danych, w pliku CodeBehind dla strony sieci web lub w klasie logika biznesowa oddzielne. Samouczki poprzednie wykazały, jak używać plików z kodem kod interakcji danych. Ta metoda działa w przypadku małych witryn, ale może to prowadzić do kodu powtarzania i większa trudności podczas obsługi dużej witryny. Może również być bardzo trudne do testowania programistyczne kod, który znajduje się w kodzie plików, ponieważ nie istnieje żadne warstwy abstrakcji.

Aby scentralizować dane kod interakcji, można utworzyć warstwy logiki biznesowej zawierający wszystkie logiki do interakcji z danymi. Następnie należy wywołać warstwę logiki biznesowej ze stron sieci web. W tym samouczku pokazano, jak przenieść cały kod zapisane w poprzednim samouczków do warstwy logiki biznesowej, a następnie użyj tego kodu ze stron.

W tym samouczku będziesz:

1. Przenieś kod z plików z kodem do warstwy logiki biznesowej
2. Zmień formanty powiązane z danymi do wywołania metody warstwy logiki biznesowej

## <a name="create-business-logic-layer"></a>Tworzenie warstwy logiki biznesowej

Teraz utworzysz klasy, która jest wywoływana ze stron sieci web. Metody tej klasy wyglądać podobnie do metody, które są używane w poprzednich samouczki i zawierać atrybuty dostawcy wartości.

Najpierw Dodaj nowy folder o nazwie **logiki warstwy Biznesowej**.

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

W folderze logiki warstwy Biznesowej, Utwórz nową klasę o nazwie **SchoolBL.cs**. Będzie zawierać wszystkie operacje danych, które pierwotnie znajdowały się w plików z kodem. Te metody są prawie takie same, jak metody w pliku związanym z kodem, ale zawiera pewne zmiany.

Najważniejsze zmiany, należy pamiętać, jest są kodu z wnętrza wystąpienia nie jest już wykonywane **strony** klasy. Zawiera klasy strony **TryUpdateModel** — metoda i **ModelState** właściwości. Gdy ten kod zostanie przeniesiony do warstwy logiki biznesowej, masz już wystąpienie klasy strony do wywołania tych elementów członkowskich. Aby uniknąć problemów tego problemu, należy dodać **ModelMethodContext** parametru do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState. Użyj tego parametru ModelMethodContext aby wywołać TryUpdateModel lub pobrać element ModelState. Nie jest konieczne wprowadzanie zmian w stronie sieci web dla tego nowego parametru.

Zastąp kod w SchoolBL.cs następującym kodem.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Popraw istniejących stron do pobierania danych z warstwy logiki biznesowej

Na koniec przekonwertuje stron Students.aspx, AddStudent.aspx i Courses.aspx z za pomocą zapytań w pliku CodeBehind, aby za pomocą warstwy logiki biznesowej.

Pliki CodeBehind dla uczniów lub studentów, AddStudent i kursów usunąć lub komentarz z następujących metod zapytania:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Teraz wymaga żaden kod w pliku CodeBehind, które odnoszą się do operacji danych.

**OnCallingDataMethods** obsługi zdarzeń można określić obiektu dla metod danych. W Students.aspx należy dodać wartość dla tego programu obsługi zdarzeń i zmieniać nazwy metod danych do nazwy metod w klasie logiki biznesowej.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

W pliku CodeBehind dla Students.aspx zdefiniuj programu obsługi zdarzeń dla zdarzenia CallingDataMethods. Ten program obsługi zdarzeń służy do określenia klasy logiki biznesowej danych operacje.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

W AddStudent.aspx zmiany podobne.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

W Courses.aspx zmiany podobne.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Uruchom aplikację i zwróć uwagę, że wszystkie strony działać zgodnie z ich były wcześniej. Logikę weryfikacji działa poprawnie.

## <a name="conclusion"></a>Wniosek

W tym samouczku ponownie strukturę aplikacji przy użyciu Warstwa dostępu do danych i warstwy logiki biznesowej. Należy określić, czy formanty danych użyć obiektu, który nie jest bieżącej strony danych operacje.

> [!div class="step-by-step"]
> [Poprzednie](using-query-string-values-to-retrieve-data.md)
