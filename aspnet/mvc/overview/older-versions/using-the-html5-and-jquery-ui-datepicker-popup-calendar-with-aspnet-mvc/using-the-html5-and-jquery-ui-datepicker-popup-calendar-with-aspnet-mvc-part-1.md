---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 1 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: a4cd6e9adfcd85503b9843232903a243bc07c959
ms.sourcegitcommit: 392a36ed269b88899d6bb652aa7f4dfb72e43e7f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50220664"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 1
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web platformy ASP.NET MVC.


Ta seria samouczków obejmuje podstawy pracy z edytora szablonów, szablony i jQuery [kalendarza podręcznego selektora daty interfejsu użytkownika](http://plugins.jquery.com/project/datepicker) w aplikacji sieci Web platformy ASP.NET MVC. W tym samouczku użyjesz programu Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), która jest bezpłatna wersja programu Microsoft Visual Studio lub Visual Studio 2010 SP1 można użyć, jeśli masz już.

Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymaganego oprogramowania, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)

Jeśli używasz programu Visual Studio 2010 dla programu Visual Web Developer, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

W tym samouczku założono, że zostały wykonane [wprowadzenie do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczka lub że znasz rozwoju platformy ASP.NET MVC. Ten samouczek rozpoczyna się od ukończonych projektu z [wprowadzenie do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczka.

Ten samouczek pokazuje kod w języku C#. Jednak [projekt startowy](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) i ukończone projektu są również dostępne w języku Visual Basic.

Projekt programu Visual Studio przy użyciu języka C# i Visual Basic kod źródłowy jest dostępny powiązany z tym tematem: [Pobierz](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Jakie będziesz tworzyć

Należy dodać szablony (w szczególności, Edytuj i Wyświetl szablony) do prostej aplikacji listy filmów, która została utworzona w [wprowadzenie do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczka. Dodasz również [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego w celu uproszczenia procesu wprowadzania daty. Poniższy zrzut ekranu przedstawia modyfikacji aplikacji za pomocą kalendarza podręcznego selektora daty interfejsu użytkownika jQuery wyświetlane.

![Zakończono jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak używać atrybutów z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeń nazw do sterowania format danych, gdy jest on wyświetlany i gdy jest on w trybie edycji.
- Jak utworzyć szablony (Edytuj i Wyświetl szablony) aby kontrolować sposób formatowania danych.
- Jak dodać [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) jako sposób Wprowadź pola daty.

### <a name="getting-started"></a>Wprowadzenie

Jeśli nie masz jeszcze aplikacji listy filmów z projektu startowego, pobierz go za pomocą następującego linku: [Pobierz](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Następnie w Eksploratorze Windows, kliknij prawym przyciskiem myszy *MvcMovie.zip* plik i wybierz **właściwości**. W **właściwości MvcMovie.zip** okno dialogowe, wybierz opcję **odblokowanie**. (Odblokowywania zapobiega ostrzeżenie zabezpieczeń, który występuje podczas próby użycia *zip* plik pobrany z sieci web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Kliknij prawym przyciskiem myszy *MvcMovie.zip* plik i wybierz **Wyodrębnij wszystkie** aby rozpakować plik. W Visual Web Developer lub Visual Studio 2010, należy otworzyć *MvcMovieCS\_TU.sln* pliku.

W **Eksploratora rozwiązań**, kliknij dwukrotnie *Views\Shared\\_Layout.cshtml* aby go otworzyć. Zmiana `H1` nagłówek z **aplikacja filmu MVC** do **jQuery filmu**. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie kliknij przycisk **Home** kartę, która spowoduje przejście do `Index` metody kontrolera filmu. Aby wypróbować aplikację, wybierz **Edytuj** łącze i **szczegóły** link dla jednego z filmów. Należy zauważyć, że w indeksie, edytować i widoków szczegółów, Data wydania i ceny są poprawnie sformatowane:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Formatowanie daty i ceny jest wynikiem użycia [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybut właściwości `Movie` klasy.

Otwórz *Movie.cs* pliku i Skomentuj `DisplayFormat` atrybutu na `ReleaseDate` i `Price` właściwości. Wartość wynikowa `Movie` klasy wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Naciśnij klawisze CTRL + F5, ponownie, aby uruchomić aplikację, a następnie wybierz **Home** kartę, aby wyświetlić listę filmów. Tym razem daty wydania Pokazuje datę i godzinę, a pole Cena nie jest już wyświetlany symbol waluty. Zmiany w `Movie` klasy wycofywania nieuprzywilejowany formatowania, które wcześniej, ale można to naprawić za chwilę.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Za pomocą atrybutu typu danych DataAnnotations do określania typu danych

Zastąp skomentowana `DisplayFormat` atrybutu dla `ReleaseDate` właściwość o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu, za pomocą `Date` wyliczenia. Zastąp `DisplayFormat` atrybutu dla `Price` właściwość o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu ponownie, przy użyciu tego czasu `Currency` wyliczenia. Jest to kompletny kod wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Uruchom aplikację. Teraz daty wydania i właściwości ceny są poprawnie sformatowany (które przy użyciu odpowiednich formaty daty i waluty). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybut udostępnia metadanych typu wbudowanego ASP.NET MVC szablonów, aby pola renderowania w poprawnym formacie. Za pomocą `DataType` atrybut zalecane jest stosowanie `DisplayFormat` atrybut, który został pierwotnie w kodzie, ponieważ `DataType` atrybutu sprawia, że model bardziej przejrzyste i bardziej elastycznych dla celów, takich jak internacjonalizacji.

W następnej sekcji zobaczysz się, jak utworzyć szablony niestandardowe do wyświetlania pól daty.

> [!div class="step-by-step"]
> [Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
