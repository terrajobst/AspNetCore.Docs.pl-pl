---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Dodanie nowego pola do filmu modelu i tabeli bazy danych (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877323"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Dodanie nowego pola do filmu modelu i tabeli bazy danych (VB)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-new-field.md) tego samouczka.


W tej sekcji możesz wprowadzić kilka zmian dla klasy modelu i Dowiedz się, jak można zaktualizować schemat bazy danych, aby dopasować zmiany modelu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Rozpocznij od dodania nowej `Rating` właściwości do istniejącej `Movie` klasy. Otwórz *Movie.cs* plik i dodać `Rating` właściwości podobne do następującego:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Pełną `Movie` klasy teraz wygląda podobnie do następującego kodu:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Ponowne skompilowanie aplikacji przy użyciu **debugowania** &gt; **kompilacji filmu** polecenia menu.

Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.vbhtml* i *\Views\Movies\Create.vbhtml* wyświetlania szablonów w celu zapewnienia obsługi nowych `Rating`właściwości.

Otwórz<em>\Views\Movies\Index.vbhtml</em> plik i dodać `<th>Rating</th>` tuż po nagłówek kolumny <strong>cen</strong> kolumny. Następnie dodaj `<td>` kolumny zbliża się koniec szablon do renderowania `@item.Rating` wartość. Poniżej znajduje się jakie zaktualizowane <em>Index.vbhtml</em> Wyświetl szablon wygląda następująco:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Następnie otwórz folder *\Views\Movies\Create.vbhtml* i Dodaj następujący kod pod koniec formularza. Renderuje pola tekstowego, tak aby po utworzeniu nowego movie, można określić klasyfikację.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Zarządzanie modelu i różnice schematu bazy danych

Użytkownik zaktualizował teraz kodu aplikacji do obsługi nowej `Rating` właściwości.

Teraz uruchom aplikację i przejdź do */Movies* adresu URL. Po wykonaniu tej czynności, zostanie wyświetlony następujący błąd:

![](adding-a-new-field/_static/image1.png)

Ten błąd jest wyświetlane, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Domyślnie korzystając z programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany. Jeśli nie są zsynchronizowane, Entity Framework zgłasza błąd. Ułatwia to śledzenie problemów w czasie opracowywania, znajdującego się w przeciwnym razie tylko (przez zasłoniętej błędy) w czasie wykonywania. Funkcja sprawdzania synchronizacji jest, co powoduje, że ma być wyświetlany komunikat o błędzie, który został wyświetlony.

Istnieją dwa podejścia do rozwiązania problemu:

1. Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework. Ta metoda jest bardzo wygodny w trakcie rozwoju active w testowej bazy danych, ponieważ pozwala szybko razem sformułować schematu modelu i bazy danych. Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych — dlatego możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!
2. Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.

W tym samouczku, użyjemy pierwszego podejścia — będziesz mieć Entity Framework Code First automatycznie ponownie utworzyć bazę danych, w dowolnym momencie zmiany modelu.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automatyczne ponowne tworzenie bazy danych na zmiany modelu

Teraz należy zaktualizować aplikacji, tak aby Code First automatycznie porzuca i ponownie utworzy bazę danych, w dowolnym momencie zmienić modelu dla aplikacji.

> [!NOTE] 
> 
> **Ostrzeżenie** należy włączyć takie podejście automatycznie porzucenie i ponowne utworzenie bazy danych tylko wtedy, gdy używasz deweloperskich lub testowania bazy danych i *nigdy nie* na produkcyjną bazę danych, który zawiera dane. Używanie go na serwerze produkcyjnym może prowadzić do utraty danych.


W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.

![](adding-a-new-field/_static/image2.png)

Nazwa klasy &quot;MovieInitializer&quot;. Aktualizacja `MovieInitializer` klasa może zawierać następujący kod:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer` Klasa określa, czy porzucić i automatycznie ponownie utworzyć klasy modelu kiedykolwiek zmiany bazy danych używanej przez model. Kod zawiera `Seed` metodę, aby określić, niektóre dane domyślne automatyczne dodawanie do bazy danych dowolnej czasu utworzony (lub odtwarzaniu). Zapewnia to wygodny sposób na wypełnianie bazy danych z przykładowymi danymi, bez konieczności ręcznie wypełnić je po każdym wprowadzeniu zmiany modelu.

Teraz, gdy zdefiniowano `MovieInitializer` klasy, należy połączenie go z się tak, że w każdym uruchomieniu aplikacji sprawdza czy klasy modelu różnią się od schematu w bazie danych. W takim przypadku można uruchomić inicjatora ponownie utworzyć bazę danych do zgodny z modelem, a następnie wypełnij bazy danych z przykładowymi danymi.

Otwórz *Global.asax* plik, który znajduje się w katalogu głównym `MvcMovies` projektu:

*Global.asax* plik zawiera klasę, która definiuje całej aplikacji dla projektu i zawiera `Application_Start` obsługi zdarzeń, który jest uruchamiany po pierwszym uruchomieniu aplikacji.

Znajdź `Application_Start` — metoda i dodaj wywołanie do `Database.SetInitializer` na początku metody, jak pokazano poniżej:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer` Instrukcji dodaną wskazuje, że bazy danych używane przez `MovieDBContext` wystąpienia powinien zostać automatycznie usunięta i utworzona ponownie, jeśli schemat i bazy danych nie są zgodne. I jak widać, zostanie również wypełnić bazę danych przykładowych danych, które jest określone w `MovieInitializer` klasy.

Zamknij *Global.asax* pliku.

Ponownie uruchom aplikację i przejdź do */Movies* adresu URL. Podczas uruchamiania aplikacji, wykryje, że struktura modelu nie jest już zgodny schemat bazy danych. Automatycznie ponownie utworzy bazę danych do dopasowania nową strukturę modelu i wypełnienie bazy danych o przykładowe filmy:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu. Należy pamiętać, że można dodać ocenę.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Kliknij przycisk **Utwórz**. Nowe film, w tym ocenę, teraz wyświetlone w filmy wyświetlania:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

W tej sekcji przedstawiono sposób modyfikowania modelu obiektów i zachować synchronizację ze zmianami bazy danych. Przedstawiono również sposób wypełnienia nowo utworzonej bazy danych z przykładowymi danymi, więc można wypróbować scenariuszy. Następnie Oto jak można dodać bardziej rozbudowane logikę weryfikacji dla klasy modelu i włączyć niektóre reguły biznesowe są wymuszane.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)
