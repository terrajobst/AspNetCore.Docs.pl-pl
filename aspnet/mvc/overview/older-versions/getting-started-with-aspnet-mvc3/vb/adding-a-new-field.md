---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Dodawanie nowego pola do modelu Movie i tabeli bazy danych (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 8fb0fb5c1db24f1961bba08f7b1c2182caca39ca
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577434"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Dodawanie nowego pola do modelu Movie i tabeli bazy danych (VB)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-new-field.md) po ukończeniu tego samouczka.


W tej sekcji możesz wprowadzić pewne zmiany na klasy modeli i Dowiedz się, jak można zaktualizować schematu bazy danych, aby dopasować zmiany modelu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Rozpocznij, dodając nowe `Rating` właściwości do istniejących `Movie` klasy. Otwórz *Movie.cs* pliku i Dodaj `Rating` właściwość podobny do poniższego:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Pełne `Movie` klasy teraz wygląda podobnie do poniższego kodu:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Ponowna kompilacja aplikacji przy użyciu **debugowania** &gt; **kompilacji filmu** polecenia menu.

Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.vbhtml* i *\Views\Movies\Create.vbhtml* wyświetlać szablony w celu zapewnienia obsługi nowych `Rating`właściwości.

Otwórz<em>\Views\Movies\Index.vbhtml</em> pliku i Dodaj `<th>Rating</th>` nagłówek kolumny, tuż za <strong>cena</strong> kolumny. Następnie dodaj `<td>` kolumny w końcowej części szablonu do renderowania `@item.Rating` wartość. Poniżej przedstawiono, jakie zaktualizowane <em>Index.vbhtml</em> Wyświetl szablon wygląda następująco:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Następnie otwórz *\Views\Movies\Create.vbhtml* pliku i Dodaj następujący kod w końcowej części formularza. Renderuje pola tekstowego, tak, aby można było określić klasyfikację, gdy zostanie utworzony nowy film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Zarządzanie modelu i różnice schematu bazy danych

Użytkownik zaktualizował teraz kod aplikacji do obsługi nowej `Rating` właściwości.

Teraz uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Gdy to zrobisz, zobaczysz następujący błąd:

![](adding-a-new-field/_static/image1.png)

Widzisz ten błąd, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Domyślnie gdy używasz programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z. Jeśli nie są zsynchronizowane, platformy Entity Framework zgłasza błąd. Ta funkcja ułatwia śledzenie problemów w czasie programowania, które mogą w przeciwnym razie tylko się okazać (przez zasłoniętej błędy) w czasie wykonywania. Funkcja Sprawdzanie synchronizacji jest o tym, co powoduje, że komunikat o błędzie, który będzie wyświetlany, który został wyświetlony.

Istnieją dwa sposoby rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. To podejście jest bardzo wygodne podczas ustalania active rozwoju w bazie danych testu, ponieważ pozwala szybko się rozwijać, schematu modelu i bazie danych razem. Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych!
2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

W tym samouczku użyjemy pierwszego podejścia — będziesz mieć Entity Framework Code First automatycznie ponownie utworzyć bazę danych, w dowolnym momencie zmiany modelu.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automatyczne ponowne tworzenie bazy danych na zmiany modelu

Zaktualizujmy aplikacji, dzięki czemu Code First automatycznie umieszcza i ponownie tworzy bazę danych w dowolnym momencie możesz zmienić modelu dla aplikacji.

> [!NOTE] 
> 
> **Ostrzeżenie** należy włączyć takie podejście automatycznie porzucenie i ponowne utworzenie bazy danych, tylko wtedy, gdy używasz deweloperskie lub testowe bazy danych i *nigdy nie* na produkcyjnej bazy danych, który zawiera rzeczywiste dane. Używany na serwerze produkcyjnym może prowadzić do utraty danych.


W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.

![](adding-a-new-field/_static/image2.png)

Nazwa klasy &quot;MovieInitializer&quot;. Aktualizacja `MovieInitializer` klasa może zawierać następujący kod:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer` Klasa określa, że usunięty i ponownie tworzone automatycznie w przypadku klasy modelu kiedykolwiek zmiany bazy danych używanej przez model. Kod zawiera `Seed` metodę, aby określić dane domyślne do automatycznego dodawania do bazy danych dowolnej czasu utworzyć (lub odtwarzaniu). Zapewnia to wygodny sposób, aby wypełnić bazę danych z pewnymi przykładowymi danymi bez konieczności ręcznie wypełnić ją po każdym wprowadzeniu zmiany modelu.

Skoro zdefiniowano `MovieInitializer` klasy, będziesz chciał Podłączanie, dzięki czemu przy każdym uruchomieniu aplikacji sprawdza czy klasy modelu różnią się od schematu w bazie danych. W takim przypadku możesz uruchomić inicjator, tak aby ponownie utworzyć bazę danych do zgodny z modelem, a następnie wypełnij bazy danych z przykładowymi danymi.

Otwórz *Global.asax* pliku, który znajduje się w katalogu głównym `MvcMovies` projektu:

*Global.asax* plik zawiera klasę, która definiuje całej aplikacji dla projektu i zawiera `Application_Start` program obsługi zdarzeń, który jest wykonywany po pierwszym uruchomieniu aplikacji.

Znajdź `Application_Start` metody i dodaj wywołanie do `Database.SetInitializer` na początku metody, jak pokazano poniżej:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer` Dodanej instrukcji wskazuje, że bazy danych używane przez `MovieDBContext` wystąpienia powinien zostać automatycznie usunięta i utworzona ponownie, jeśli schemat i bazy danych nie są zgodne. I, co będzie również wypełniać bazy danych z przykładowymi danymi, który jest określony w `MovieInitializer` klasy.

Zamknij *Global.asax* pliku.

Uruchom ponownie aplikację i przejdź do */Movies* adresu URL. Podczas uruchamiania aplikacji, wykrywa, że struktura modelu nie jest już zgodny ze schematem bazy danych. Automatycznie ponownie tworzy bazę danych, aby dopasować nową strukturę modelu i wypełnienie bazy danych o filmy próbki:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film. Należy pamiętać, że można dodawać ocenę.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Kliknij przycisk **Utwórz**. Ten nowy film, w tym klasyfikacji, wyświetlane w filmach, wyświetlanie listy:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

W tej sekcji pokazano, jak można modyfikować obiekty modelu i synchronizację bazy danych przy użyciu zmian. Przedstawiono również sposób wypełnić nowo utworzoną bazę danych z przykładowymi danymi, dzięki czemu możesz wypróbować scenariuszy. Następnie Przyjrzyjmy się jak dodać bogatsze logikę walidacji do klas modelu i włączyć niektóre reguły biznesowe zostaną wymuszone.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)
