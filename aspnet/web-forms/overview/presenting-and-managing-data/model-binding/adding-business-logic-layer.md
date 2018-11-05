---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 4ba1830ce51f257e18880f752b249dbeb77f504e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021654"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób tworzenia powiązania modelu za pomocą warstwy logiki biznesowej. Ustawi Członkowskie OnCallingDataMethods, aby określić, czy obiekt inny niż bieżąca strona jest używane do wywołania metody dotyczące danych.
> 
> Ten samouczek opiera się na projekt utworzony w [wcześniej](retrieving-data.md) części tej serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.


## <a name="what-youll-build"></a>Będziesz tworzyć

Wiązanie modelu umożliwia umieść kod interakcji danych, w pliku związanym z kodem dla strony sieci web lub w klasie logiki oddzielne biznesowych. Poprzednich samouczków wykazały, jak używać plików z kodem dla danych interakcję kodu. Ta metoda działa w przypadku małych witryn, ale może to prowadzić do kodu powtórzeń i większa trudności podczas obsługi dużej witryny. Może też być bardzo trudne, programowo testować kod, który znajduje się w kodzie plików, ponieważ nie istnieje żadne warstwę abstrakcji.

Można scentralizować dane kod interakcji, można utworzyć warstwy logiki biznesowej, która zawiera całą logikę do interakcji z danymi. Następnie możesz wywołać warstwę logiki biznesowej ze stron sieci web. W tym samouczku pokazano, jak przenieść cały kod, które zostały napisane w poprzednich samouczkach do warstwy logiki biznesowej, a następnie użyj tego kodu na stronach.

W ramach tego samouczka należy:

1. Przenieś kod z plików z kodem do warstwy logiki biznesowej
2. Zmień swoje formanty powiązane z danymi do wywołania metody warstwy logiki biznesowej

## <a name="create-business-logic-layer"></a>Tworzenie warstwy logiki biznesowej

Teraz utworzysz klasę, która jest wywoływana ze stron sieci web. Metody tej klasy wyglądać podobnie do metody, które są używane w poprzednich samouczkach i zawierać atrybuty dostawcy wartości.

Najpierw Dodaj nowy folder o nazwie **LOGIKI**.

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

W folderze LOGIKI, Utwórz nową klasę o nazwie **SchoolBL.cs**. Będzie zawierać wszystkie operacje danych, które pierwotnie znajdowały się w plikach związanym z kodem. Te metody są prawie takie same jak metody w pliku związanym z kodem, ale będzie zawierać pewne zmiany.

Najważniejsze zmiany, należy pamiętać, jest już wykonujesz kodu z w ramach wystąpienia **strony** klasy. Zawiera klasy strony **TryUpdateModel** metody i **ModelState** właściwości. Kiedy ten kod jest przenoszony do warstwy logiki biznesowej, masz już wystąpienie klasy strony, aby wywołać te elementy członkowskie. Aby obejść ten problem, należy dodać **ModelMethodContext** parametru do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState. Użyjesz tego parametru ModelMethodContext można wywołać TryUpdateModel lub pobrać element ModelState. Nie trzeba wprowadzić na stronie sieci web, aby uwzględnić ten nowy parametr zmiany.

Zastąp kod w SchoolBL.cs następującym kodem.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Popraw istniejących stron do pobierania danych z warstwy logiki biznesowej

Na koniec przekonwertuje stron Students.aspx AddStudent.aspx i Courses.aspx korzystania z zapytania w pliku związanym z kodem za pomocą warstwy logiki biznesowej.

Pliki związane z kodem dla uczniów, AddStudent i kursów usunąć lub komentarz następujące metody zapytania:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Możesz teraz powinien mieć żadnego kodu w pliku związanym z kodem, które odnoszą się do operacji na danych.

**OnCallingDataMethods** programu obsługi zdarzeń można określić obiekt ma być używany dla metody dotyczące danych. W Students.aspx Dodaj wartość dla tego programu obsługi zdarzeń i zmieniać nazwy metod danych do nazw metod w klasie logiki biznesowej.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

W pliku związanym z kodem Students.aspx należy zdefiniować program obsługi zdarzeń dla zdarzenia CallingDataMethods. Ta procedura obsługi zdarzeń służy do określenia klasy logiki biznesowej dla operacji na danych.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

W AddStudent.aspx wprowadzić zmiany podobne.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

W Courses.aspx wprowadzić zmiany podobne.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Uruchom aplikację i zwróć uwagę, wszystkie strony działać zgodnie z ich były wcześniej. Logika sprawdzania poprawności działa poprawnie.

## <a name="conclusion"></a>Wniosek

W tym samouczku ponownie strukturę aplikacji przy użyciu warstwy dostępu do danych i warstwy logiki biznesowej. Określono, że formanty danych użyć obiektu, który nie jest stroną bieżące operacje na danych.

> [!div class="step-by-step"]
> [Poprzednie](using-query-string-values-to-retrieve-data.md)
