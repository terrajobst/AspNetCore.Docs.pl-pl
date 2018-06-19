---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885848"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualizowanie, usuwanie i tworzenie danych z wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób tworzenia, aktualizacji i usuwania danych z wiązania modelu. Ustawi następujące właściwości:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Te właściwości odbierać nazwę metody, która obsługuje odpowiadająca mu operacja. W ramach tej metody musisz podać logikę do interakcji z danymi.
> 
> W tym samouczku opiera się na projekcie utworzony w pierwszym [części](retrieving-data.md) serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku będziesz:

1. Dodaj szablony danych dynamicznych
2. Włącz aktualizowania i usuwania danych za pomocą metody wiązania modelu
3. Zastosowanie reguły sprawdzania poprawności danych - Włącz tworzenie nowy rekord w bazie danych

## <a name="add-dynamic-data-templates"></a>Dodaj szablony danych dynamicznych

Aby zapewnić najlepsze środowisko użytkownika i zminimalizować powtarzania kodu, użyje szablonów danych dynamicznych. Można łatwo zintegrować szablonów wstępnie zbudowanych danych dynamicznych istniejącej lokacji, instalując pakiet NuGet.

Z **Zarządzaj pakietami NuGet**, zainstaluj **DynamicDataTemplatesCS**.

![Szablony danych dynamicznych](updating-deleting-and-creating-data/_static/image1.png)

Powiadomienie, że projekt zawiera teraz folder o nazwie **DynamicData**. W tym folderze można znaleźć szablonów, które są automatycznie stosowane do formantów dynamicznych w formularzach sieci web.

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Włącz aktualizowanie i usuwanie

Umożliwienie użytkownikom się aktualizować i usuwać rekordy w bazie danych jest bardzo podobny do procesu pobierania danych. W **UpdateMethod** i **DeleteMethod** właściwości, określ nazwy metod, które wykonują te operacje. Za pomocą formantu widoku GridView możesz określić automatyczne generowanie edycji i usuń przyciski. Następujący wyróżniony kod pokazuje dodatki do kodu widoku GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

W pliku CodeBehind dodać za pomocą instrukcji dla **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Następnie dodaj następującą aktualizację i metody zostaną usunięte.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** metoda dotyczy pasujących wartości powiązane z danymi za pomocą formularza sieci web elementu danych. Element danych jest pobierany na podstawie wartości parametru identyfikatora.

## <a name="enforce-validation-requirements"></a>Wymusić wymagania weryfikacji

Atrybuty weryfikacji, które zostały zastosowane do właściwości FirstName, LastName i rok w klasie uczniów automatycznie są wymuszane podczas aktualizowania danych. Formanty DynamicField Dodaj moduły klienta i serwera, na podstawie atrybutów sprawdzania poprawności. Właściwości imię i nazwisko są wymagane. Imię nie może przekraczać 20 znaków długości i nazwisko nie może przekraczać 40 znaków. Rok musi być prawidłową wartością wyliczenia AcademicYear.

Jeśli użytkownik narusza jeden wymagań sprawdzania poprawności, nie kontynuować aktualizację. Aby wyświetlić komunikat o błędzie, Dodaj formant ValidationSummary powyżej widoku GridView. Aby wyświetlić błędy sprawdzania poprawności z wiązania modelu, należy ustawić **ShowModelStateErrors** ustawioną właściwość **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Uruchom aplikację sieci web i Usuń wszystkie rekordy.

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

Zwróć uwagę, że podczas w trybie edycji wartości dla właściwości roku jest automatycznie renderowane jako listy rozwijanej. Właściwość roku jest wartością wyliczenia, a szablon danych dynamicznych dla wartości wyliczenia określa rozwijanej listy do edycji. Tego szablonu można znaleźć, otwierając **wyliczenie\_Edit.ascx** w pliku **DynamicData**/**FieldTemplates** folderu.

Jeśli podasz prawidłowe wartości aktualizacji zakończy się pomyślnie. Jeśli narusza jest jednym z wymagań sprawdzania poprawności, nie kontynuować aktualizację i powyżej siatki jest wyświetlany komunikat o błędzie.

![Komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Dodaj nowe rekordy

Kontrolki widoku siatki nie zawiera **InsertMethod** właściwości i dlatego nie można użyć do dodawania nowego rekordu z wiązania modelu. Można znaleźć właściwości InsertMethod w **FormView**, **widoku DetailsView**, lub **ListView** kontrolki. W tym samouczku użyjesz formancie FormView do dodawania nowego rekordu.

Najpierw dodaj łącze do nowej strony, będzie tworzone na potrzeby dodawania nowego rekordu. Powyżej ValidationSummary należy dodać:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

W górnej części zawartości dla strony studentów pojawi się nowy link.

![Nowy link](updating-deleting-and-creating-data/_static/image5.png)

Następnie należy dodać nowy formularz sieci web za pomocą strony wzorcowej i nadaj jej nazwę **AddStudent**. Wybierz Site.Master jako strony wzorcowej.

Będą zawierały pola do dodawania nowych uczniów za pomocą **Dynamiccontrol** formantu. Formant Dynamiccontrol renderuje tej właściwości można edytować w klasie określony we właściwości ItemType. Właściwość StudentID została oznaczona atrybutem **[ScaffoldColumn(false)]** atrybutu tak nie jest on renderowany. W zastępczym znacznika strony AddStudent Dodaj następujący kod.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

W pliku CodeBehind (AddStudent.aspx.cs), Dodaj **przy użyciu** instrukcji dla **ContosoUniversityModelBinding.Models** przestrzeni nazw.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Następnie należy dodać następujące metody do określenia sposobu wstawić nowy rekord i program obsługi zdarzeń dla przycisku Anuluj.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Zapisz wszystkie zmiany.

Uruchom aplikację sieci web i Utwórz nowe uczniów.

![Dodaj nowe studentów](updating-deleting-and-creating-data/_static/image6.png)

Kliknij przycisk **Wstaw** i zwróć uwagę, nowe uczniów został utworzony.

![Wyświetlanie nowych studentów](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku włączone aktualizowanie, usuwanie i tworzenie danych. Należy zapewnić reguł sprawdzania poprawności są stosowane podczas interakcji z danymi.

W następnej [samouczek](sorting-paging-and-filtering-data.md) w tej serii spowoduje włączenie sortowania, stronicowania i filtrowanie danych.

> [!div class="step-by-step"]
> [Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)
