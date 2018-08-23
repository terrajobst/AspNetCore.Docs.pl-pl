---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755257"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> W tym samouczku przedstawiono sposób dodawania interfejsu użytkownika JQuery [widżet Datepicker](http://jqueryui.com/datepicker/) do formularza sieci Web, a model użycie powiązania do aktualizacji bazy danych przy użyciu wybranej wartości.
> 
> Ten samouczek opiera się na projekt utworzony w [pierwszy](retrieving-data.md) i [drugi](updating-deleting-and-creating-data.md) części tej serii.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

1. Dodawanie właściwości do modelu w taki sposób, aby zarejestrować studenta Data rejestracji
2. Włącz użytkownikowi wybranie daty rejestracji za pomocą widżetu selektora daty interfejsu użytkownika JQuery
3. Wymuszanie reguł sprawdzania poprawności dla Data rejestracji

Element widget selektora daty interfejsu użytkownika JQuery umożliwia użytkownikom łatwe wybierz datę z kalendarza, który pojawia się po użytkownik wchodzi w interakcję z polem. Za pomocą tego widżetu można wygodniejsze niż ręcznie wpisując wartość typu date. Integrowanie widżet Datepicker strona, która używa wiązania modelu dla operacji na danych wymaga tylko niewielką ilość pracy dodatkowe.

## <a name="add-a-new-property-to-the-model"></a>Dodaj nową właściwość do modelu

Najpierw należy dodać **daty/godziny** właściwości, aby zainteresować studentów modelu, a następnie przeprowadzić migrację tej zmiany do bazy danych. Otwórz **UniversityModels.cs**i Dodaj wyróżniony kod do modelu dla uczniów.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** jest dołączony do wymuszania reguł sprawdzania poprawności dla właściwości. W tym samouczku zakładamy, że Contoso University opiera się na 1 stycznia 2013 r. i w związku z tym starszych daty rejestracji są nieprawidłowe.

W oknie Zarządzanie pakietami za pomocą polecenia Dodaj migrację **AddEnrollmentDate Dodaj migracji**. Należy zauważyć, że kod migracji dodaje nową kolumnę daty/godziny do tabeli dla uczniów. Do dopasowania wartości, które określiłeś w RangeAttribute, Dodaj wartość domyślną dla nowej kolumny, jak pokazano w poniższym kodzie wyróżnione.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Zapisz zmiany w pliku migracji.

Nie trzeba ponownie umieścić początkowo dane. W związku z tym, otwórz **Configuration.cs** w folderze migracje i usuń lub przekształcić w komentarz kod w **inicjatora** metody. Zapisz i zamknij plik.

Teraz uruchom polecenie **update-database**. Należy zauważyć, że kolumna teraz istnieje w bazie danych, a wszystkie istniejące rekordy mają wartość domyślną dla EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Dodawanie kontrolek dynamicznych dla Data rejestracji

Teraz należy dodać formanty do wyświetlania i edytowania Data rejestracji. W tym momencie wartość jest edytowany za pomocą pola tekstowego. W dalszej części tego samouczka zostanie zmieniony pole tekstowe z walidacją JQuery Datepicker.

Po pierwsze, należy pamiętać, że nie trzeba wprowadzać zmian, aby jest **AddStudent.aspx** pliku. Kontrolka DynamicEntity automatycznie spowoduje, że nowej właściwości.

Otwórz **Students.aspx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Uruchom aplikację i zwróć uwagę, że wartość Data rejestracji można ustawić, wpisując wartość typu date. Podczas dodawania nowego studenta:

![Ustaw datę](integrating-jquery-ui/_static/image1.png)

Lub edytowania istniejącej wartości:

![Edytuj datę](integrating-jquery-ui/_static/image2.png)

Wpisywanie działa daty, ale może nie być środowiska klienta, które chcą udostępnić. W następnej sekcji spowoduje włączenie wybranie daty przy użyciu kalendarza.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Zainstaluj pakiet NuGet do pracy za pomocą interfejsu użytkownika JQuery

**UI soku** pakietu NuGet umożliwia łatwą integrację widżetów interfejsu użytkownika JQuery w aplikacji sieci web. Aby użyć tego pakietu, należy go zainstalować za pośrednictwem pakietu NuGet.

![Dodaj soku interfejsu użytkownika](integrating-jquery-ui/_static/image3.png)

Wersja soku interfejsu użytkownika, które zainstalujesz może spowodować konflikt z wersją JQuery w aplikacji. Przed kontynuowaniem za pomocą tego samouczka spróbuj uruchamiania aplikacji. Jeśli wystąpi błąd kodu JavaScript, należy uzgodnić z wersją JQuery. Możesz dodać oczekiwanej wersji JQuery do folderu skryptów (wersja 1.8.2 w momencie pisania tego samouczka) lub w Site.master Określ ścieżkę do pliku JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Dostosowywanie szablonu daty/godziny do uwzględnienia Datepicker widżetu

Element widget Datepicker doda do szablonu danych dynamicznych do edycji wartości daty/godziny. Przez dodawanie elementu widget do szablonu, jest on automatycznie renderowany w formie dodawania nowego studenta i w widoku siatki dla uczniów i studentów edycji. Otwórz **daty/godziny\_Edit.ascx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

W pliku związanym z kodem ustawi daty minimalne i maksymalne DatePicker. Ustawiając następujące wartości, możesz uniemożliwi użytkownikom przechodzenia do nieprawidłowe daty. Pobierze minimalne i maksymalne wartości z **RangeAttribute** we właściwości daty/godziny, jeśli zostało ono określone. Otwórz **daty/godziny\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_metoda obciążenia.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Uruchom aplikację sieci web i przejdź do strony AddStudent. Podaj wartości dla pól i Zauważ, że po kliknięciu pola tekstowego dla daty rejestracji kalendarz jest wyświetlana.

![Wybór daty](integrating-jquery-ui/_static/image4.png)

Wybierz datę, a następnie kliknij przycisk **Wstaw**. RangeAttribute wymusza sprawdzania poprawności na serwerze. Przez ustawienie właściwości minDate dla selektora daty, również zastosować sprawdzania poprawności na komputerze klienckim. Kalendarz nie zezwala na użytkownika, przejdź do daty przed wartość minDate.

Podczas edytowania rekordu w widoku siatki, kalendarz jest również wyświetlany.

![DatePicker w widoku GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Wniosek

W tym samouczku przedstawiono sposób włączenia elementu widget JQuery do formularza sieci web, który używa wiązania modelu.

W ciągu następnych [samouczek](using-query-string-values-to-retrieve-data.md), będzie używać wartości ciągu zapytania, podczas wybierania danych.

> [!div class="step-by-step"]
> [Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)
