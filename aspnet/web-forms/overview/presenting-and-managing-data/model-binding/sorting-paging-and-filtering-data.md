---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757363"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób dodawania, sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu.
> 
> Ten samouczek opiera się na projekt utworzony w pierwszym [część](retrieving-data.md) serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

1. Włączyć sortowanie i stronicowanie danych
2. Włącz filtrowanie danych na podstawie wybranych przez użytkownika

## <a name="add-sorting"></a>Dodaj sortowanie

Włączanie sortowania w widoku GridView jest bardzo proste. W pliku Student.aspx, po prostu ustaw **AllowSorting** do **true** w widoku GridView. Nie należy ustawić **SortExpression** wartość dla każdej kolumny, jak DataField jest stosowana automatycznie. Kontrolki GridView modyfikuje zapytanie, aby uwzględnić porządkowanie danych przez wybraną wartość. Wyróżniony kod poniżej przedstawiono dodanie należy wprowadzić włączyć sortowanie.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Uruchamianie aplikacji sieci web i testowanie sortowania dla uczniów rekordy według wartości w różnych kolumnach.

![Studenci sortowania](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Dodawanie stronicowania

Włączanie stronicowania również jest bardzo proste. W widoku GridView, ustaw **właściwość AllowPaging** właściwości **true** i ustaw **PageSize** liczbę rekordów, które mają zostać wyświetlone na każdej stronie. W tym samouczku można ustawić go do 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Uruchom aplikację sieci web i zwróć uwagę, że teraz rekordy są podzielone na kilka stron z nie więcej niż 4 wyświetlanych na jednej stronie rekordów.

![Dodawanie stronicowania](sorting-paging-and-filtering-data/_static/image4.png)

Wykonanie odroczone zapytanie poprawia wydajność aplikacji. Zamiast pobierania całego zestawu danych, widoku GridView modyfikuje zapytanie, aby pobrać tylko te rekordy, dla bieżącej strony.

## <a name="filter-records-by-user-selection"></a>Filtrowanie rekordów na podstawie wyboru użytkownik

Wiązanie modelu dodaje kilka atrybutów, które pozwalają na określenie sposobu ustawiania wartości dla parametru metody wiązania modelu. Te atrybuty **System.Web.ModelBinding** przestrzeni nazw. Obejmują one:

- Formant
- Plik cookie
- Formularz
- Profil
- Ciąg zapytania
- RouteData
- Sesja
- UserProfile
- Stan widoku

W tym samouczku wartość kontrolki zostanie użyta, aby filtrować rekordy, które są wyświetlane w widoku GridView. Należy dodać **kontroli** atrybutu do metody zapytania została utworzona wcześniej. W [później](using-query-string-values-to-retrieve-data.md) samouczek, zostaną zastosowane **QueryString** atrybutu do parametru do określenia, czy wartość parametru pochodzą od wartości ciągu zapytania.

Po pierwsze powyżej podsumowania walidacji, dodać listy rozwijanej w celu filtrowania listy, studentów, które są wyświetlane.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

W pliku związanym z kodem zmodyfikować wybierz metodę, aby otrzymać wartość z kontrolki, a następnie ustaw nazwę parametru nazwę kontrolki, która zawiera wartość.

Należy dodać **przy użyciu** poufności informacji dotyczące **System.Web.ModelBinding** przestrzeni nazw, aby rozwiązać atrybut kontrolki.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Poniższy kod przedstawia metody select zlikwidować filtrującą dane zwrócone dane na podstawie wartości na liście rozwijanej. Dodawanie atrybutu kontroli, zanim parametr określa, że wartość tego parametru pochodzi z formantu o takiej samej nazwie.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Uruchom aplikację sieci web i wybierz różne wartości z listy rozwijanej listy, aby filtrować listę uczniów.

![Studenci filtru](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku włączone sortowanie i stronicowanie danych. Możesz również włączone filtrowania danych według wartości kontrolki.

W ciągu następnych [samouczek](integrating-jquery-ui.md) ulepszenie interfejsu użytkownika przez integrowanie widżetów interfejsu użytkownika JQuery szablonu dynamicznego danych.

> [!div class="step-by-step"]
> [Poprzednie](updating-deleting-and-creating-data.md)
> [dalej](integrating-jquery-ui.md)
