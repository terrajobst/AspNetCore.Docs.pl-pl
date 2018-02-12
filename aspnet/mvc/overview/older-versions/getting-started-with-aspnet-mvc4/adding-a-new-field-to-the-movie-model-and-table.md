---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodanie nowego pola filmu modelu i tabeli | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 9965c8a755857a8e8cb8ecbc6c467a6c856aa83d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Dodanie nowego pola filmu modelu i tabeli
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji użyjesz migracje Code First Framework jednostki migrację pewne zmiany do klasy modeli, aby zmiana została zastosowana do bazy danych.

Domyślnie korzystając z programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany. Jeśli nie są zsynchronizowane, Entity Framework zgłasza błąd. Ułatwia to śledzenie problemów w czasie opracowywania, znajdującego się w przeciwnym razie tylko (przez zasłoniętej błędy) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie migracje Code First zmiany modelu

Jeśli używasz programu Visual Studio 2012, kliknij dwukrotnie *Movies.mdf* plików w Eksploratorze rozwiązań, aby otworzyć narzędzie bazy danych. Visual Studio Express for Web wyświetli Eksploratora bazy danych programu Visual Studio 2012 wyświetli Eksploratora serwera. Jeśli używasz programu Visual Studio 2010, należy użyć Eksplorator obiektów SQL Server.

W narzędziu bazy danych (Eksploratora bazy danych, w Eksploratorze serwera lub Eksploratorze obiektów SQL Server), kliknij prawym przyciskiem myszy `MovieDBContext` i wybierz **usunąć** można usunąć bazy danych filmów.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Przejdź z powrotem do Eksploratora rozwiązań. Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **usunąć** usuwania filmy bazy danych.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Tworzenie aplikacji, aby upewnić się, że nie ma żadnych błędów.

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

W **Konsola Menedżera pakietów** okno w `PM>` wierszu wprowadź "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-Migrations** polecenie (pokazanym powyżej) tworzy *Configuration.cs* plik w nowym *migracje* folderu.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio otworzy *Configuration.cs* pliku. Zastąp `Seed` metody w *Configuration.cs* pliku następującym kodem:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy w dowolnym kształcie wierszu red w obszarze `Movie` i wybierz **rozwiązać** następnie **przy użyciu** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Dzięki temu dodaje następujące instrukcję using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Kod wywoła pierwszy migracji `Seed` metody po każdym migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizacji wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieje.


**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujące kroki zakończy się niepowodzeniem, jeśli Twoje nie kompilacji w tym momencie.)

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Migracja do tworzy nową bazę danych, dlatego należy usunąć *movie.mdf* pliku w poprzednim kroku.

W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" do utworzenia początkowej migracji. Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzony.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migracje Code First tworzy innego pliku klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Aby pomóc w kolejności z sygnaturą czasową wstępnie ustala się filename migracji. Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje tworzenia tabeli filmy dla filmu bazy danych. Po zaktualizowaniu bazy danych w instrukcji poniżej, *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych. Następnie przy użyciu **inicjatora** metoda zostaną uruchomione, aby wypełnić bazę danych z danych testowych.

W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych i należy wykonać `update-database`. W takim przypadku usuń *Movies.mdf* ponownie, a następnie spróbuj ponownie `update-database` polecenia. Jeśli nadal występuje błąd, Usuń migrations folder i zawartość, a następnie uruchomić z instrukcjami w górnej części strony (który jest delete *Movies.mdf* pliku, a następnie przejdź do Enable-Migrations).

Uruchom aplikację i przejdź do */Movies* adresu URL. Są wyświetlane dane inicjatora.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Rozpocznij od dodania nowej `Rating` właściwości do istniejącej `Movie` klasy. Otwórz *Models\Movie.cs* plik i dodać `Rating` właściwości podobne do następującego:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Pełną `Movie` klasy teraz wygląda podobnie do następującego kodu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Tworzenie aplikacji przy użyciu **kompilacji** &gt; **kompilacji filmu** menu polecenie lub naciskając klawisz CTRL-SHIFT-B.

Teraz, gdy użytkownik zaktualizował `Model` klasy, należy również zaktualizować *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* wyświetlania szablonów, aby wyświetlić nowy `Rating`właściwości w widoku przeglądarki.

Otwórz*\Views\Movies\Index.cshtml* plik i dodać `<th>Rating</th>` tuż po nagłówek kolumny **cen** kolumny. Następnie dodaj `<td>` kolumny zbliża się koniec szablon do renderowania `@item.Rating` wartość. Poniżej znajduje się jakie zaktualizowane *Index.cshtml* Wyświetl szablon wygląda następująco:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Następnie otwórz folder *\Views\Movies\Create.cshtml* i Dodaj następujący kod pod koniec formularza. Renderuje pola tekstowego, tak aby po utworzeniu nowego movie, można określić klasyfikację.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Użytkownik zaktualizował teraz kodu aplikacji do obsługi nowej `Rating` właściwości.

Teraz uruchom aplikację i przejdź do */Movies* adresu URL. Po wykonaniu tej czynności, zostanie wyświetlony jeden z następujących błędów:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Ten błąd jest wyświetlane, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)


Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework. Ta metoda jest bardzo wygodny w trakcie rozwoju active w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych. Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych — dlatego możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych! Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji. Aby uzyskać więcej informacji na inicjatory bazy danych programu Entity Framework, zobacz Tomasz Dykstra [samouczek platformy ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.
3. Użyj migracje Code First, aby zaktualizować schemat bazy danych.


W tym samouczku użyjemy migracje Code First.

Seed — metoda aktualizacji, dzięki czemu zapewnia wartość dla nowej kolumny. Otwórz plik Migrations\Configuration.cs i Dodaj pole klasyfikacji do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenie:

`add-migration AddRatingMig`

`add-migration` Polecenie informuje framework migracji do badania bieżącego modelu film z bieżącego schematu filmu bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu. AddRatingMig dowolnej i jest używany do nazywania plików migracji. Warto użyć opisową nazwę krok migracji.

Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Skompiluj rozwiązanie, a następnie wprowadź polecenie "update-database" w **Konsola Menedżera pakietów** okna.

Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (sygnaturę daty dołączanie AddRatingMig może się różnić).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Ponownie uruchom aplikację i przejdź do adresu URL /Movies. Widać nowe pole klasyfikacji.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu. Należy pamiętać, że można dodać ocenę.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Kliknij przycisk **Utwórz**. Nowe film, w tym ocenę, teraz wyświetlone w filmy wyświetlania:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Należy również dodać `Rating` pola edycji, szczegóły i SearchIndex Wyświetl szablony.

Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żadne zmiany nie nastąpi, ponieważ schemat zgodny z modelem.

W tej sekcji przedstawiono sposób modyfikowania modelu obiektów i zachować synchronizację ze zmianami bazy danych. Przedstawiono również sposób wypełnienia nowo utworzonej bazy danych z przykładowymi danymi, więc można wypróbować scenariuszy. Następnie Oto jak można dodać bardziej rozbudowane logikę weryfikacji dla klasy modelu i włączyć niektóre reguły biznesowe są wymuszane.

>[!div class="step-by-step"]
[Poprzednie](examining-the-edit-methods-and-edit-view.md)
[dalej](adding-validation-to-the-model.md)
