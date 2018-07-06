---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Przy użyciu wartości ciągu zapytania do filtrowania danych przy użyciu wiązania modelu web forms | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: b4615d004a32d00b91635bc321a2d4ea792fddbe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837266"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Za pomocą wartości ciągu zapytania do filtrowania danych z wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak przekazać wartości ciągu zapytania i wartości można używać do pobierania danych przy użyciu wiązania modelu.
> 
> Ten samouczek opiera się na projekt utworzony w [wcześniej](retrieving-data.md) części tej serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

1. Dodaj nową stronę, aby wyświetlić zarejestrowane kursy dla uczniów lub studentów
2. Pobieranie zarejestrowanych kursy dla wybranej uczniów lub studentów, na podstawie wartości w ciągu zapytania
3. Dodać hiperłącze z wartością ciągu zapytania z widoku siatki do nowej strony

Kroki opisane w tym samouczku są podobne do demonstrującego we wcześniejszych przykładach [samouczek](sorting-paging-and-filtering-data.md) celu przefiltrowania uczniów wyświetlane, w oparciu o wybór użytkownika na liście rozwijanej. W tym samouczku użyto **kontroli** atrybutu w metodzie wybierz, aby określić, że wartość parametru pochodzi z formantu. W tym samouczku użyjesz **QueryString** atrybutu w metodzie select, aby określić, że wartość parametru pochodzi z ciągu zapytania.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Dodaj nową stronę do wyświetlania kursy studenta

Dodaj nowy formularz sieci web używający strony wzorcowej Site.master i nazwij stronę **kursów**.

W **Courses.aspx** plików, Dodawanie widoku siatki, aby wyświetlić kursy dla wybranej uczniów lub studentów.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definiowanie metody select

W **Courses.aspx.cs**, dodasz wybierz metodę o nazwie określonej w widoku siatki **metody SelectMethod** właściwości. W tej metodzie będzie zdefiniować zapytanie do pobierania kursy studenta i określić, że parametr pochodzi z ciągiem zapytania z taką samą nazwę jak parametr.

Najpierw należy dodać następujące **przy użyciu** instrukcji.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Następnie dodaj następujący kod do Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Atrybut QueryString oznacza, że wartość ciągu kwerendy o nazwie StudentID jest automatycznie przypisywana do parametru w przypadku tej metody.

## <a name="add-hyperlink-with-query-string-value"></a>Dodawanie hiperlinku z wartością ciągu zapytania

W widoku siatki na Students.aspx zostaną dodane pola hiperłącze, który stanowi łącze do nowej strony kursów. Hiperłącze będzie zawierać wartość ciągu zapytania w identyfikatorze studenta.

W Students.aspx Dodaj następujące pola do kolumny w widoku siatki, tuż poniżej pola, łączna liczba kredytów systemu.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Uruchom aplikację i zwróć uwagę, że w widoku siatki teraz link kursów.

![Dodawanie hiperlinku](using-query-string-values-to-retrieve-data/_static/image1.png)

Po kliknięciu jednego z linków, zobaczysz ten uczniów zarejestrowane kursów.

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku dodano łącze z wartością ciągu zapytania. Ta wartość ciągu zapytania jest używany dla wartości parametru w metodzie select.

W ciągu następnych [samouczek](adding-business-logic-layer.md), kod zostaną przeniesione z plików z kodem, warstwy logiki biznesowej i warstwy dostępu do danych.

> [!div class="step-by-step"]
> [Poprzednie](integrating-jquery-ui.md)
> [dalej](adding-business-logic-layer.md)
