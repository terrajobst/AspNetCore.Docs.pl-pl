---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887915"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób dodawania interfejsu użytkownika JQuery [widget selektora daty](http://jqueryui.com/datepicker/) do formularza sieci Web i użyj modelu powiązania do aktualizacji bazy danych przy użyciu wybranej wartości.
> 
> W tym samouczku opiera się na projekcie utworzone w [pierwszy](retrieving-data.md) i [drugi](updating-deleting-and-creating-data.md) części serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku będziesz:

1. Dodawanie właściwości do modelu do rejestrowania studenta Data rejestracji
2. Włącz użytkownikowi na wybranie daty rejestracji za pomocą elementu widget selektora daty interfejsu użytkownika JQuery
3. Wymuś reguły sprawdzania poprawności dla daty rejestracji

Widżet selektora daty interfejsu użytkownika JQuery umożliwia użytkownikom łatwe wybierz datę z kalendarza, która będzie wyświetlana, gdy użytkownik wchodzi w interakcję z polem. Może być wygodniejsze niż data wpisywać ręcznie przy użyciu tego elementu widget. Integrowanie widget selektora daty strony, które jest używane powiązanie modelu danych operacje wymaga małej ilości dodatkowych działań.

## <a name="add-a-new-property-to-the-model"></a>Dodawanie nowych właściwości do modelu

Najpierw należy dodać **Datetime** Twojego uczniów dla właściwości modelu i Migrowanie tej zmiany do bazy danych. Otwórz **UniversityModels.cs**i Dodaj do modelu uczniów wyróżniony kod.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** jest dołączony do wymuszania reguł sprawdzania poprawności dla właściwości. W tym samouczku zakładamy, że czy Contoso University został opiera się na 1 stycznia 2013 i w związku z tym starszych dat rejestracji są nieprawidłowe.

W oknie zarządzania pakietami Dodaj migracji za pomocą polecenia **AddEnrollmentDate dodać migracji**. Zwróć uwagę, że kod migracji dodaje nową kolumnę daty i godziny do tabeli dla użytkowników domowych. Aby odpowiada wartości określonej w RangeAttribute, należy dodać wartość domyślną dla nowej kolumny, jak pokazano w poniższym kodzie wyróżnione.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Zapisz zmiany w pliku migracji.

Nie trzeba ponownie inicjatora danych. W związku z tym Otwórz **Configuration.cs** w folderze migracji i usuń lub komentarz kod w **inicjatora** metody. Zapisz i zamknij plik.

Teraz uruchom polecenie **update-database**. Zauważyć, że kolumna teraz istnieje w bazie danych, a wszystkie istniejące rekordy EnrollmentDate mają wartości domyślne.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Dodawanie formantów dynamicznych dla Data rejestracji

Teraz należy dodać kontrolki do wyświetlania i edytowania Data rejestracji. W tym momencie wartość jest edytowana za pomocą pola tekstowego. Później w samouczku spowoduje zmianę pola tekstowego na elemencie widget selektora daty JQuery.

Po pierwsze, ważne jest, należy pamiętać, że jest konieczne wprowadzenie zmian do **AddStudent.aspx** pliku. Formant Dynamiccontrol automatycznie spowoduje, że nowej właściwości.

Otwórz **Students.aspx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Uruchom aplikację i zwróć uwagę, że można ustawić wartość Data rejestracji, wpisując datę. Podczas dodawania nowego uczniów:

![Ustaw daty](integrating-jquery-ui/_static/image1.png)

Lub edytowanie istniejącej wartości:

![edytowanie daty](integrating-jquery-ui/_static/image2.png)

Wpisywanie działa daty, ale nie może być obsługi klienta, który możesz podać. W następnej sekcji spowoduje włączenie wybranie daty przy użyciu kalendarza.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Zainstaluj pakiet NuGet do pracy z interfejsu użytkownika JQuery

**Interfejsu użytkownika soku** pakietu NuGet umożliwia łatwą integrację widżetów interfejsu użytkownika JQuery do aplikacji sieci web. Aby korzystać z tego pakietu, należy go zainstalować za pośrednictwem pakietu NuGet.

![Dodaj soku interfejsu użytkownika](integrating-jquery-ui/_static/image3.png)

Wersja soku interfejsu użytkownika, który należy zainstalować może spowodować konflikt z wersją JQuery w aplikacji. Przed wykonaniem tego samouczka należy ponownie uruchomić aplikację. Jeśli wystąpi błąd kodu JavaScript, należy uzgodnić z wersją JQuery. Możesz dodać oczekiwanej wersji JQuery do folderu skryptów (wersja 1.8.2 w momencie pisania tego samouczka) lub w Site.master Określ ścieżkę do pliku JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Dostosowywanie szablonu daty/godziny mają zawierać element widget selektora daty

Widżet selektora daty zostaną dodane do szablonu danych dynamicznych do edycji wartości daty i godziny. Przez dodanie elementu widget do szablonu, jest on automatycznie renderowany w formie dodawania nowych uczniów i w widoku siatki dla uczniów lub studentów edycji. Otwórz **DateTime\_Edit.ascx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

W pliku związanym z kodem spowoduje ustawienie daty minimalną i maksymalną dla selektora daty. Przez ustawienie tych wartości, możesz uniemożliwi użytkownikom przechodzenie do nieprawidłowe daty. Pobierze minimalne i maksymalne wartości z **RangeAttribute** dla właściwości DateTime, jeśli zostało ono określone. Otwórz **DateTime\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_załadować metody.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Uruchom aplikację sieci web i przejdź do strony AddStudent. Podaj wartości dla pól i Zauważ, że po kliknięciu w polu tekstowym dla daty rejestracji kalendarza jest wyświetlana.

![Wybór daty](integrating-jquery-ui/_static/image4.png)

Wybierz datę, a następnie kliknij przycisk **Wstaw**. RangeAttribute wymusza sprawdzania poprawności na serwerze. Przez ustawienie właściwości minDate dla selektora daty, należy również zastosować sprawdzania poprawności na kliencie. Kalendarz nie zezwala na użytkownika, przejdź do daty przed wartością minDate.

Podczas edytowania rekordu w widoku siatki kalendarza jest również wyświetlany.

![Selektora daty w widoku GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku przedstawiono sposób włączenia elementu widget JQuery do formularza sieci web, która używa powiązania modelu.

W następnej [samouczek](using-query-string-values-to-retrieve-data.md), wybierając danych użyje wartości ciągu zapytania.

> [!div class="step-by-step"]
> [Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)
