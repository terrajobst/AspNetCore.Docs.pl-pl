---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 1 | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 9320c8a2aadb3b3c5bd6cd90b59d8a72db384c0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 1
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web programu ASP.NET MVC.


W tym samouczku udzieli Ci podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i jQuery [kalendarza podręcznego selektora daty interfejsu użytkownika](http://plugins.jquery.com/project/datepicker) w aplikacji sieci Web programu ASP.NET MVC. W tym samouczku, można użyć programu Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), który jest bezpłatną wersję programu Microsoft Visual Studio lub Visual Studio 2010 z dodatkiem SP1 można użyć, jeśli masz już który.

Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagane oprogramowanie, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)

Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Ten samouczek zakłada, zostały ukończone [wprowadzenie MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczek lub że znasz rozwoju platformy ASP.NET MVC. W tym samouczku rozpoczyna się od ukończone projektu z [wprowadzenie MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczka.

W tym samouczku przedstawiono kod w języku C#. Jednak [projektu starter](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) i ukończone projektu są również dostępne w języku Visual Basic.

Projektu programu Visual Studio z kodu źródłowego C# i Visual Basic jest dostępna powiązany z tym tematem: [Pobierz](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Jakie będzie kompilacji

Należy dodać szablony (w szczególności Edytuj i Wyświetl szablony) do prostą aplikację listy filmów, który został utworzony w [wprowadzenie MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczka. Należy również dodać [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego w celu uproszczenia procesu wprowadzania daty. Poniższy zrzut ekranu przedstawia zmodyfikowaną aplikację z kalendarza podręcznego selektora daty interfejsu użytkownika jQuery wyświetlane.

![Zakończono jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Oto dowiesz się:

- Jak używać atrybutów z [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw, aby kontrolować format danych, pojawi się i gdy jest on w trybie edycji.
- Jak utworzyć szablony (Edytuj i Wyświetl szablony) formatowania danych.
- Jak dodać [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) sposób wprowadzania pól daty.

### <a name="getting-started"></a>Wprowadzenie

Jeśli nie masz jeszcze aplikacji listy filmów z projektu starter, pobierz go przy użyciu następującego łącza: [Pobierz](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Następnie w Eksploratorze Windows, kliknij prawym przyciskiem myszy *MvcMovie.zip* plik i wybierz **właściwości**. W **właściwości MvcMovie.zip** okno dialogowe, wybierz opcję **Odblokuj**. (Odblokowywania uniemożliwia ostrzeżenie występuje, gdy użytkownik próbuje użyć *.zip* pliku pobranym z sieci web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Kliknij prawym przyciskiem myszy *MvcMovie.zip* plik i wybierz **Wyodrębnij wszystkie** aby rozpakować plik. W programie Visual Studio 2010 lub Visual Web Developer Otwórz *MvcMovieCS\_TU.sln* pliku.

W **Eksploratora rozwiązań**, kliknij dwukrotnie *Views\Shared\\_Layout.cshtml* go otworzyć. Zmień `H1` nagłówka z **MVC Movie App** do **jQuery film**. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie kliknij przycisk **Home** kartę, która umożliwia przejście do `Index` kontrolera filmu. Wypróbowanie aplikacji, wybierz **Edytuj** łącze i **szczegóły** łącze dla jednego z filmów. Zwróć uwagę, że w indeksie, edycji, i widoki szczegółów, Data wydania i cen dobrze sformatowana:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Formatowanie daty i ceny jest wynikiem przy użyciu [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu dla właściwości `Movie` klasy.

Otwórz *Movie.cs* plików i Oznacz jako komentarz `DisplayFormat` atrybutu `ReleaseDate` i `Price` właściwości. Powstałe w ten sposób `Movie` klasy wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie wybierz **Home** kartę, aby wyświetlić listę filmów. Data wydania teraz przedstawia datę i godzinę, a pola Cena nie jest już wyświetlana symbol waluty. Zmiany w `Movie` klasy wycofywania nieuprzywilejowany formatowania, który był wyświetlany poprzednio, ale później będzie rozwiązać ten problem.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Za pomocą atrybutu DataAnnotations DataType, aby określić typ danych

Zastąp oznaczone jako poza `DisplayFormat` atrybutu dla `ReleaseDate` właściwości o [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu przy użyciu `Date` wyliczenia. Zastąp `DisplayFormat` atrybutu dla `Price` właściwości o [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) ponownie, ten czas użyciu atrybutu `Currency` wyliczenia. To jest kompletny kod wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Uruchom aplikację. Teraz Data wydania i właściwości ceny są prawidłowo sformatowane (które przy użyciu odpowiednich formatów daty i currency). [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu zapewnia typ metadanych dla wbudowanych platformy ASP.NET MVC szablonów pól renderowania w poprawnym formacie. Przy użyciu `DataType` atrybutu zalecane jest stosowanie `DisplayFormat` atrybut, który został pierwotnie w kodzie, ponieważ `DataType` atrybutu sprawia, że model czyszczący i bardziej elastyczne do celów, takich jak internacjonalizacji.

W następnej sekcji pojawi się, jak utworzyć szablony niestandardowe, aby wyświetlić datę pola.

>[!div class="step-by-step"]
[Dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
