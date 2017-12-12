---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: "Poprawa szczegóły i metody Delete (VB) | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 24c986f7ec8376bc997f1ebc575338772507cbc9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="improving-the-details-and-delete-methods-vb"></a>Poprawa szczegóły i metody Delete (VB)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/improving-the-details-and-delete-methods.md) tego samouczka.


W tej części samouczka należy podjąć niektórych ulepszeń automatycznie generowanych `Details` i `Delete` metody. Te zmiany nie są wymagane, ale z kilku małe fragmenty kodu, można łatwo rozszerzyć aplikacji.

## <a name="improving-the-details-and-delete-methods"></a>Poprawa szczegóły i metody Delete

Gdy szkieletu `Movie` kontrolera ASP.NET MVC wygenerowanego kodu działający ponosić, ale które można wprowadzić bardziej niezawodne z kilku niewielkich zmian.

Otwórz `Movie` kontrolera i zmodyfikuj `Details` metoda zwróciła `HttpNotFound` po filmu nie zostanie odnaleziony. Należy również zmodyfikować `Details` metodę, aby ustawić wartość domyślną dla Identyfikatora, który jest przekazywany do niego. (Zostały wprowadzone zmiany podobne do `Edit` metody w [część 6](examining-the-edit-methods-and-edit-view.md) tego samouczka.) Jednak należy zmienić typ zwracany `Details` metody z `ViewResult` do `ActionResult`, ponieważ `HttpNotFound` metoda nie zwraca `ViewResult` obiektu. W poniższym przykładzie przedstawiono zmodyfikowanych `Details` metody.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Kod najpierw ułatwia wyszukiwanie danych przy użyciu `Find` metody. Jest to funkcja zabezpieczeń ważne, która budujemy do metody kodu sprawdza, czy `Find` znaleziono metody filmu przed próbuje podejmować żadnych działań z nim kod. Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film). Jeśli nie jest sprawdzanie wartości null film, może to spowodować błąd bazy danych.

Podobnie, zmień `Delete` i `DeleteConfirmed` metod, aby określić wartości domyślnej dla parametru Identyfikatora i powrócić `HttpNotFound` po filmu nie zostanie odnaleziony. Zaktualizowany interfejs `Delete` metod w `Movie` kontrolera są wyświetlane poniżej.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Należy pamiętać, że `Delete` — metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Stephen Walther [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę. Poniżej przedstawiono podpisy dwóch metod:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody mają unikatowe sygnatury (tej samej nazwy, lista różnych parametrów). Jednak w tym miejscu należy Delete sposobów — jeden dla GET - i jeden dla żądania POST że dla obu wymaga takiego samego podpisu. (Oba muszą zaakceptować pojedynczego całkowitą jako parametr.)

Aby posortować tę możliwość, można wykonać kilka rzeczy. Jeden jest zapewniają różne nazwy metody. To robiliśmy w on poprzedzających przykład. Jednak powstaje mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i zmiana metody routingu zwykle nie będą mogli odnaleźć tej metody. Rozwiązanie, to zostanie wyświetlony w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. To skutecznie wykonuje mapowania systemu routingu, aby adres URL, który zawiera */Delete/*POST znajdzie żądanie `DeleteConfirmed` metody.

Inny sposób, aby uniknąć problemów z metod, które mają identyczne nazwy i podpisy jest sztucznie Zmiana podpisu metody POST, aby uwzględnić parametrem nieużywane. Na przykład, niektórzy deweloperzy dodać parametr typu `FormCollection` przekazanego do metody POST, a następnie po prostu nie używaj parametru:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Zawijanie w

Masz teraz kompletna aplikacja platformy ASP.NET MVC, która przechowuje dane w bazie danych programu SQL Server Compact. Można utworzyć, odczytu, aktualizacji, usuwania i wyszukiwać filmy.

![](improving-the-details-and-delete-methods/_static/image1.png)

W tym samouczku podstawowe otrzymano pracę wprowadzania kontrolerów, kojarzenie ich z widokami i przekazywanie wokół ustalony danych. Następnie utworzony i zaprojektowany modelu danych. Entity Framework Code First utworzył bazę danych z modelu danych na bieżąco, a system szkieletów ASP.NET MVC są generowane automatycznie widoków dla podstawowych operacji CRUD i metod akcji. Następnie dodawane formularza wyszukiwania, które umożliwiają użytkownikom przeszukiwać bazę danych. Zmienić bazy danych w celu uwzględnienia nowej kolumny danych, a następnie zaktualizowane dwie strony, aby utworzyć i wyświetlić te nowe dane. Dodane weryfikacji przez oznaczenie modelu danych z atrybutów z `DataAnnotations` przestrzeni nazw. Wynikowa sprawdzania poprawności jest uruchamiany na kliencie i na serwerze.

Jeśli chcesz wdrożyć aplikację, warto pierwszego testu aplikacji na lokalnym serwerze IIS 7. Możesz użyć tej funkcji [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) łącze, aby włączyć ustawienie programu IIS dla aplikacji ASP.NET. Zobacz następujące linki wdrażania:

- [Mapa zawartości wdrożenia programu ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698.aspx)
- [Włączanie usług IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Wdrażanie projektów aplikacji sieci Web](https://msdn.microsoft.com/en-us/library/dd394698.aspx)

Można teraz zachęca do przejdź do naszego poziomu pośredniego [tworzenia modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) i [magazynu utworów muzycznych MVC](../../mvc-music-store/mvc-music-store-part-1.md) samouczki, aby zapoznać się z [ASP.NET artykuły w witrynie MSDN](https://msdn.microsoft.com/en-us/library/gg416514(VS.98).aspx)i zapoznaj się z wielu plików wideo i zasobów w [https://asp.net/mvc](https://asp.net/mvc) nawet więcej informacji o platformie ASP.NET MVC! [ASP.NET MVC forum](https://forums.asp.net/1146.aspx) są doskonałe miejsce, aby zadać pytania.

Owocnej pracy.

— Scott Hanselman ([http://hanselman.com](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter) i Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[Poprzednie](adding-validation-to-the-model.md)
