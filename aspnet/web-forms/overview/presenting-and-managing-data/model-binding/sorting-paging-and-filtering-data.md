---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortowanie, stronicowania i filtrowanie danych przy użyciu wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885409"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortowanie, stronicowania i filtrowanie danych przy użyciu wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób dodawania sortowania, stronicowania i filtrowanie danych za pośrednictwem wiązania modelu.
> 
> W tym samouczku opiera się na projekcie utworzony w pierwszym [części](retrieving-data.md) serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku będziesz:

1. Włącz sortowanie i stronicowanie danych
2. Włącz filtrowanie danych na podstawie zaznaczenia przez użytkownika

## <a name="add-sorting"></a>Dodaj sortowanie

Włączanie sortowania w widoku GridView jest bardzo proste. W pliku Student.aspx wystarczy ustawić **AllowSorting** do **true** w widoku GridView. Nie należy ustawić **SortExpression** wartość dla każdej kolumny, jak automatycznie służy element DataField. Widoku GridView modyfikuje zapytanie zawierało porządkowanie danych przez wybraną wartość. Wyróżniony kod poniżej pokazano sposób dodawania, które należy podjąć włączyć sortowanie.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Uruchamianie aplikacji sieci web i testowanie sortowania rekordów uczniowie według wartości w różnych kolumn.

![Sortowanie uczniów lub studentów](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Dodaj stronicowania

Włączanie stronicowania również jest bardzo proste. W widoku GridView, ustaw **właściwość AllowPaging** właściwości **true** i ustaw **PageSize** właściwości to liczba rekordów, które mają zostać wyświetlone na każdej stronie. W tym samouczku można ustawić go do 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Uruchom aplikację sieci web i zwróć uwagę, że teraz rekordy są podzielone na kilka stron z nie więcej niż 4 rekordów wyświetlanych na jednej stronie.

![Dodaj stronicowania](sorting-paging-and-filtering-data/_static/image4.png)

Wykonywanie zapytań odroczonych poprawia wydajność aplikacji. Zamiast pobierania cały zestaw danych, w widoku GridView modyfikuje zapytanie w celu pobrania tylko rekordów dla bieżącej strony.

## <a name="filter-records-by-user-selection"></a>Filtrowanie rekordów przez określonego użytkownika

Wiązanie modelu dodaje kilka atrybutów, które umożliwiają określanie sposobu wartość dla parametru w metodzie powiązania modelu. Te atrybuty są w **System.Web.ModelBinding** przestrzeni nazw. Obejmują one:

- Formant
- Cookie
- Formularz
- Profil
- Ciąg zapytania
- RouteData
- Sesja
- UserProfile
- Stan widoku

W tym samouczku użyjesz wartość formantu do filtrowania, które rekordy są wyświetlane w widoku GridView. Należy dodać **kontroli** atrybut do metody zapytania została utworzona wcześniej. W [później](using-query-string-values-to-retrieve-data.md) samouczka będą stosowane **QueryString** atrybutu parametru, aby określić, czy wartość parametru pochodzą z wartości ciągu zapytania.

Najpierw powyżej ValidationSummary, Dodaj na rozwijanej liście filtrowania studentów, które są wyświetlane.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

W pliku CodeBehind zmodyfikuj metody select, aby otrzymać wartość z formantu, a następnie ustaw nazwę parametru Nazwa formantu, który zawiera wartość.

Należy dodać **przy użyciu** instrukcji dla **System.Web.ModelBinding** przestrzeni nazw, aby rozpoznać atrybut kontrolki.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Poniższy kod przedstawia metody select zlikwidować do filtrowania danych zwróconych na podstawie wartości z listy rozwijanej. Dodawanie atrybutu kontroli przed parametr określa, że wartość tego parametru pochodzi z formantu o takiej samej nazwie.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Uruchom aplikację sieci web i wybierz różne wartości z listy rozwijanej listy, aby przefiltrować listę uczniów lub studentów.

![Filtr uczniów lub studentów](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku włączone sortowanie i stronicowanie danych. Możesz również włączone filtrowanie danych przez wartość formantu.

W następnej [samouczek](integrating-jquery-ui.md) zwiększy interfejsu użytkownika dzięki integracji elementu widget interfejsu użytkownika JQuery w szablonie danych dynamicznych.

> [!div class="step-by-step"]
> [Poprzednie](updating-deleting-and-creating-data.md)
> [dalej](integrating-jquery-ui.md)
