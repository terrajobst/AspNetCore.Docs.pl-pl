---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodawanie nowego pola do modelu Movie i tabeli | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366479"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Dodawanie nowego pola do modelu Movie i tabeli
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji użyjesz migracje Code First Framework jednostki migracji pewne zmiany do klasy modelu, aby zmiana została zastosowana do bazy danych.

Domyślnie gdy używasz programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z. Jeśli nie są zsynchronizowane, platformy Entity Framework zgłasza błąd. Ta funkcja ułatwia śledzenie problemów w czasie programowania, które mogą w przeciwnym razie tylko się okazać (przez zasłoniętej błędy) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie funkcji migracje Code First dla zmiany modelu

Jeśli używasz programu Visual Studio 2012, kliknij dwukrotnie *Movies.mdf* plik z Eksploratora rozwiązań, aby otworzyć narzędzie do bazy danych. Visual Studio Express for Web pokaże Eksplorator bazy danych programu Visual Studio 2012 pokaże Eksploratora serwera. Jeśli używasz programu Visual Studio 2010, należy użyć Eksplorator obiektów SQL Server.

W narzędziu bazy danych (Eksplorator bazy danych, Eksplorator serwera lub Eksploratorze obiektów SQL Server), kliknij prawym przyciskiem myszy `MovieDBContext` i wybierz **Usuń** celu porzucenia bazy danych filmów.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Przejdź z powrotem do Eksploratora rozwiązań. Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **Usuń** można usunąć bazy danych filmów.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Utwórz aplikację, aby upewnić się, że nie ma żadnych błędów.

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** i następnie **Konsola Menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

W **Konsola Menedżera pakietów** okna w `PM>` wierszu wprowadź "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-Migrations** polecenie (jak pokazano powyżej) umożliwia utworzenie *Configuration.cs* plik w nowym *migracje* folderu.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Zostanie otwarty program Visual Studio *Configuration.cs* pliku. Zastąp `Seed` method in Class metoda *Configuration.cs* pliku następującym kodem:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy czerwona linia falista pod `Movie` i wybierz **rozwiązać** następnie **przy użyciu** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Spowoduje to dodanie następujących instrukcję using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Kod wywołuje metodę migracji First `Seed` metody po każdej migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizuje wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieją.


**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujących kroków zakończy się niepowodzeniem, jeśli usługi nie kompilacji na tym etapie.)

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Ta migracja do tworzy nową bazę danych, dlatego możesz usunąć *movie.mdf* pliku w poprzednim kroku.

W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" do utworzenia początkowej migracji. Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzone.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migracje Code First tworzy inny plik klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności. Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje tworzenia tabeli filmy dla bazy danych filmów. Kiedy aktualizować bazę danych w instrukcjach poniżej to *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych. A następnie **inicjatora** metoda zostanie uruchomiony do wypełniania bazy danych z danymi.

W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Jeśli otrzymasz komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych oraz zanim zostanie wykonany `update-database`. W takim przypadku usuń *Movies.mdf* ponownie plik, a następnie spróbuj ponownie `update-database` polecenia. Jeśli nadal wystąpi błąd, usuń folder migracje i zawartość, a następnie uruchomić za pomocą instrukcji u góry tej strony (to delete *Movies.mdf* pliku, a następnie przejść do Enable-Migrations).

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Dane inicjatora są wyświetlane.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Rozpocznij, dodając nowe `Rating` właściwości do istniejących `Movie` klasy. Otwórz *Models\Movie.cs* pliku i Dodaj `Rating` właściwość podobny do poniższego:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Pełne `Movie` klasy teraz wygląda podobnie do poniższego kodu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Tworzenie aplikacji przy użyciu **kompilacji** &gt; **kompilacji filmu** menu polecenia lub przez naciśnięcie klawiszy CTRL-SHIFT-B.

Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* wyświetlać szablony, aby wyświetlić nowy `Rating`właściwości w widoku przeglądarki.

Otwórz<em>\Views\Movies\Index.cshtml</em> pliku i Dodaj `<th>Rating</th>` nagłówek kolumny, tuż za <strong>cena</strong> kolumny. Następnie dodaj `<td>` kolumny w końcowej części szablonu do renderowania `@item.Rating` wartość. Poniżej przedstawiono, jakie zaktualizowane <em>Index.cshtml</em> Wyświetl szablon wygląda następująco:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Następnie otwórz *\Views\Movies\Create.cshtml* pliku i Dodaj następujący kod w końcowej części formularza. Renderuje pola tekstowego, tak, aby można było określić klasyfikację, gdy zostanie utworzony nowy film.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Użytkownik zaktualizował teraz kod aplikacji do obsługi nowej `Rating` właściwości.

Teraz uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Gdy to zrobisz, zobaczysz jedną z następujących błędów:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Widzisz ten błąd, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)


Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. To podejście jest bardzo wygodne w przypadku, gdy aktywne tworzenie aplikacji w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych! Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji. Aby uzyskać więcej informacji na temat inicjatory bazy danych programu Entity Framework, zobacz Tom Dykstra [samouczek platformy ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.
3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.


W tym samouczku użyjemy migracje Code First.

Seed — metoda należy zaktualizować tak, aby go oferuje wartości dla nowej kolumny. Otwórz plik Migrations\Configuration.cs i dodać pole klasyfikacji do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wpisz następujące polecenie:

`add-migration AddRatingMig`

`add-migration` Polecenie informuje platformę migracji, aby zbadać bieżącej modelu movie z bieżącym schemacie filmu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu. AddRatingMig jest dowolnego i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę krok migracji.

Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Skompiluj rozwiązanie, a następnie wprowadź polecenie "update-database" w **Konsola Menedżera pakietów** okna.

Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (sygnatura daty AS AddRatingMig będzie się różnić).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Uruchom ponownie aplikację, a następnie przejdź do adresu URL /Movies. Możesz zobaczyć nowe pole klasyfikacji.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film. Należy pamiętać, że można dodawać ocenę.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Kliknij przycisk **Utwórz**. Ten nowy film, w tym klasyfikacji, wyświetlane w filmach, wyświetlanie listy:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Należy również dodać `Rating` pola edycji, szczegóły i SearchIndex Przeglądanie szablonów.

Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żadne zmiany nie nastąpi, ponieważ schemat jest zgodna z modelem.

W tej sekcji pokazano, jak można modyfikować obiekty modelu i synchronizację bazy danych przy użyciu zmian. Przedstawiono również sposób wypełnić nowo utworzoną bazę danych z przykładowymi danymi, dzięki czemu możesz wypróbować scenariuszy. Następnie Przyjrzyjmy się jak dodać bogatsze logikę walidacji do klas modelu i włączyć niektóre reguły biznesowe zostaną wymuszone.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)
