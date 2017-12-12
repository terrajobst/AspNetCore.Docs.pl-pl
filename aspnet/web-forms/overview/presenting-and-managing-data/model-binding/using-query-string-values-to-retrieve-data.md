---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "Przy użyciu wartości ciągu zapytania do filtrowania danych z powiązaniem modelu i sieci web forms | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Przy użyciu wartości ciągu zapytania do filtrowania danych z wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak przekazać wartości w ciągu zapytania i użycie tej wartości można pobrać danych za pośrednictwem wiązania modelu.
> 
> W tym samouczku opiera się na projekcie utworzone w [wcześniej](retrieving-data.md) części serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku będziesz:

1. Dodaj nową stronę do wyświetlenia kursy zarejestrowanych dla użytkowników domowych
2. Pobieranie szkoleń w zarejestrowany dla wybranych dla użytkowników domowych, na podstawie wartości w ciągu zapytania
3. Dodawanie hiperłącza z wartością ciągu zapytania w widoku siatki do nowej strony

Kroki opisane w tym samouczku są dość podobne do zagadnienia omówione w wcześniej [samouczek](sorting-paging-and-filtering-data.md) do filtrowania wyświetlanych studentów, na podstawie zaznaczenia użytkownika na liście rozwijanej. W tym samouczku, użyte **kontroli** atrybutu w metodzie select, aby określić, czy wartość parametru pochodzą z formantu. W tym samouczku użyjesz **QueryString** atrybutu w metodzie select, aby określić, że wartość parametru pochodzi z ciągu zapytania.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Dodaj nową stronę do wyświetlania szkoleń dla użytkowników domowych

Dodaj nowy formularz sieci web używający strony wzorcowej Site.master i nazwa strony **kursów**.

W **Courses.aspx** plików, Dodaj do wyświetlenia kursów dla uczniów wybranego widoku siatki.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Zdefiniuj wybierz — Metoda

W **Courses.aspx.cs**, wybierz metodę o nazwie określonej w widoku siatki spowoduje dodanie **SelectMethod** właściwości. W tej metodzie zostanie Zdefiniuj kwerendę dla pobierania kursy studenta oraz określ, czy parametr pochodzą z wartości ciągu zapytania z taką samą nazwę jak parametr.

Najpierw należy dodać następujące **przy użyciu** instrukcje.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Następnie dodaj następujący kod do Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Atrybutu QueryString oznacza, że wartość ciągu kwerendy o nazwie StudentID zostanie automatycznie przypisany do parametru w ramach tej metody.

## <a name="add-hyperlink-with-query-string-value"></a>Dodawanie hiperłącza z wartością ciągu zapytania

W widoku siatki w Students.aspx spowoduje dodanie pola hiperłącze, który stanowi łącze do nowej strony szkolenia. Hiperłącze będzie zawierać wartość ciągu zapytania o identyfikatorze studenta.

W Students.aspx Dodaj następujące pole do kolumny w widoku siatki poniżej pola do całkowitej środków.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Uruchom aplikację i zwróć uwagę, że widoku siatki zawiera teraz łącze szkolenia.

![Dodawanie hiperłącza](using-query-string-values-to-retrieve-data/_static/image1.png)

Kliknięcie łącza zobaczysz kursy zarejestrowanych tego studenta.

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku po dodaniu łącza z wartością ciągu zapytania. Tej wartości ciągu zapytania jest używany dla wartości parametru w metodzie select.

W następnej [samouczek](adding-business-logic-layer.md), przeniesie kod z kodem — pliki do warstwy logiki biznesowej i warstwy dostępu do danych.

>[!div class="step-by-step"]
[Poprzednie](integrating-jquery-ui.md)
[dalej](adding-business-logic-layer.md)
