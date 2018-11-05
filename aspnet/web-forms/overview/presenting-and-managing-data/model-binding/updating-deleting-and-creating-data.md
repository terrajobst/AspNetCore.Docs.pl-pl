---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych za pomocą wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 127543b0696b01f136b340d07f6f806b6a6fb1eb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021576"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualizowanie, usuwanie i tworzenie danych za pomocą wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak tworzenie, aktualizowanie i usuwanie danych przy użyciu wiązania modelu. Będzie można ustawić następujące właściwości:
> 
> - DeleteMethod
> - Operacji InsertMethod
> - Operacji UpdateMethod
> 
> Te właściwości otrzymują nazwę metody, która obsługuje odpowiednia operacja. W ramach tej metody możesz podać logikę do interakcji z danymi.
> 
> Ten samouczek opiera się na projekt utworzony w pierwszym [część](retrieving-data.md) serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

1. Dodawanie szablonów danych dynamicznych
2. Włącz aktualizowanie i usuwanie danych za pośrednictwem metody wiązania modelu
3. Zastosuj reguły sprawdzania poprawności danych — Włączanie, tworząc nowy rekord w bazie danych

## <a name="add-dynamic-data-templates"></a>Dodawanie szablonów danych dynamicznych

Aby zapewnić najlepszą jakość obsługi i zminimalizować powtarzających się kodów, użyje danych dynamicznych szablonów. Można łatwo zintegrować dane dynamiczne wstępnie utworzonych szablonów do istniejącej lokacji, instalując pakiet NuGet.

Z **Zarządzaj pakietami NuGet**, zainstaluj **DynamicDataTemplatesCS**.

![Szablony danych dynamicznych](updating-deleting-and-creating-data/_static/image1.png)

Zwróć uwagę, że projekt obecnie zawiera folder o nazwie **modelu DynamicData**. W tym folderze znajdzie się szablony, które są automatycznie stosowane do kontrolek dynamicznych w formularzach sieci web.

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Włącz, aktualizowanie i usuwanie

Umożliwienie użytkownikom aktualizowanie i usuwanie rekordów bazy danych jest bardzo podobny do procesu pobierania danych. W **operacji UpdateMethod** i **DeleteMethod** właściwościami, można określić nazwy metody, które wykonują te operacje. Przy użyciu kontrolki GridView można również określić automatycznego generowania edycji i usuń przyciski. Następujący wyróżniony kod pokazuje dodatki do kodu kontrolki GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

W pliku związanym z kodem, Dodaj nazwę za pomocą instrukcji for **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Następnie należy dodać następującą aktualizację i usunięcie metod.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** metoda stosowana pasujących wartości powiązanych z danymi za pomocą formularza sieci web do elementu danych. Element danych jest pobierany na podstawie wartości parametru identyfikatora.

## <a name="enforce-validation-requirements"></a>Wymuszanie wymagań dotyczących sprawdzania poprawności

Atrybuty weryfikacji, które został zastosowany do właściwości FirstName, LastName i rok w klasie ucznia jest automatycznie wymuszanych podczas aktualizowania danych. Formanty DynamicField Dodaj moduły klienta i serwera, na podstawie atrybutów sprawdzania poprawności. Właściwości imię i nazwisko są wymagane. Imię nie może przekraczać 20 znaków i nazwisko nie może przekraczać 40 znaków. Rok musi być prawidłową wartością wyliczenia AcademicYear.

Jeśli użytkownik narusza jedno z wymagań sprawdzania poprawności, aktualizacja nie kontynuować. Aby wyświetlić komunikat o błędzie, należy dodać kontrolki podsumowania walidacji powyżej widoku GridView. Aby wyświetlić błędy sprawdzania poprawności z wiązania modelu, należy ustawić **ShowModelStateErrors** właściwością **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Uruchom aplikację sieci web i Usuń wszystkie rekordy.

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

Należy zauważyć, że jeśli w trybie edycji wartości dla właściwości roku automatycznie jest renderowane jako listy rozwijanej. Właściwość Year jest wartością wyliczenia i dane dynamiczne szablon wyliczenia wartości służy do listy rozwijanej listy do edycji. Tego szablonu można znaleźć, otwierając **wyliczenie\_Edit.ascx** w pliku **modelu DynamicData**/**FieldTemplates** folderu.

Jeśli podano prawidłowe wartości aktualizacji zakończy się pomyślnie. Jeśli naruszają jest jedno z wymagań sprawdzania poprawności, będzie można kontynuować aktualizacji i powyżej siatki jest wyświetlany komunikat o błędzie.

![komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Dodawanie nowych rekordów

Nie ma w kontrolce GridView **operacji InsertMethod** właściwości i dlatego nie można używać do dodawania nowego rekordu przy użyciu wiązania modelu. Możesz znaleźć właściwość operacji InsertMethod **FormView**, **DetailsView**, lub **ListView** kontrolki. W tym samouczku użyjesz kontrolce FormView do dodawania nowego rekordu.

Najpierw dodaj łącze do nowej strony, która zostanie utworzona dla dodawania nowego rekordu. Powyżej podsumowania walidacji należy dodać:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Nowe łącze pojawi się w górnej części zawartości dla strony studentów.

![Nowy link](updating-deleting-and-creating-data/_static/image5.png)

Następnie dodaj nowy formularz sieci web za pomocą strony wzorcowej i nadaj mu nazwę **AddStudent**. Wybierz Site.Master jako strony wzorcowej.

Renderowanie zostanie przeprowadzone pola, aby dodać nowego studenta przy użyciu **DynamicEntity** kontroli. Kontrolka DynamicEntity renderuje tej właściwości można edytować w klasie określony we właściwości ItemType. Właściwość StudentID została oznaczona **[ScaffoldColumn(false)]** atrybutu, dzięki czemu nie jest renderowany. W zastępczym MainContent strony AddStudent Dodaj następujący kod.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

W pliku związanym z kodem (AddStudent.aspx.cs) Dodaj **przy użyciu** poufności informacji dotyczące **ContosoUniversityModelBinding.Models** przestrzeni nazw.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Następnie dodaj następujące metody, aby określić sposób wstawiania nowego rekordu i program obsługi zdarzeń dla przycisku Anuluj.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Zapisz wszystkie zmiany.

Uruchom aplikację sieci web i utworzyć nowego studenta.

![dodać nowego studenta](updating-deleting-and-creating-data/_static/image6.png)

Kliknij przycisk **Wstaw** i zwróć uwagę, utworzono nowego studenta.

![Wyświetlanie nowego studenta](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku włączone aktualizowanie, usuwanie i tworzenie danych. Możesz zapewnić reguł sprawdzania poprawności są stosowane podczas interakcji z danymi.

W ciągu następnych [samouczek](sorting-paging-and-filtering-data.md) w tej serii umożliwi sortowanie, stronicowanie i filtrowanie danych.

> [!div class="step-by-step"]
> [Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)
