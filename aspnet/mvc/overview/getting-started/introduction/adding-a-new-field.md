---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodanie nowego pola | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10eb343259b58052fd1f2411dbdc2196eafc858
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/19/2017
---
<a name="adding-a-new-field"></a>Dodanie nowego pola
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tej sekcji użyjesz migracje Code First Framework jednostki migrację pewne zmiany do klasy modeli, aby zmiana została zastosowana do bazy danych.

Domyślnie korzystając z programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany. Jeśli nie są zsynchronizowane, Entity Framework zgłasza błąd. Ułatwia to śledzenie problemów w czasie opracowywania, znajdującego się w przeciwnym razie tylko (przez zasłoniętej błędy) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie migracje Code First zmiany modelu

Przejdź do Eksploratora rozwiązań. Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **usunąć** usuwania filmy bazy danych. Jeśli nie widzisz *Movies.mdf* plików, kliknij na **Pokaż wszystkie pliki** ikona przedstawionym poniżej na czerwone obramowanie.

![](adding-a-new-field/_static/image1.png)

Tworzenie aplikacji, aby upewnić się, że nie ma żadnych błędów.

Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** , a następnie **Konsola Menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

W **Konsola Menedżera pakietów** okno w `PM>` wierszu wprowadź

Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-Migrations** polecenie (pokazanym powyżej) tworzy *Configuration.cs* plik w nowym *migracje* folderu.

![](adding-a-new-field/_static/image4.png)

Visual Studio otworzy *Configuration.cs* pliku. Zastąp `Seed` metody w *Configuration.cs* pliku następującym kodem:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Umieść kursor nad dowolnym kształcie czerwoną linię w obszarze `Movie` i kliknij przycisk `Show Potential Fixes` , a następnie kliknij przycisk **przy użyciu** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Dzięki temu dodaje następujące instrukcję using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> Kod wywoła pierwszy migracji `Seed` metody po każdym migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizacji wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieje.
> 
> [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody w poniższym kodzie wykonuje operację "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Ponieważ [inicjatora](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) metody uruchamiany przy każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy chcesz dodać już będą dostępne po pierwszej migracji, które utworzy bazę danych. "[Upsert](http://en.wikipedia.org/wiki/Upsert)" operację zapobiega błędom, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale zastępuje wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Z danych testowych w niektórych tabel nie można się zdarzyć, że: w niektórych przypadkach po zmianie danych podczas testowania ma zmiany po aktualizacji bazy danych. W takim przypadku chcesz wykonać operację wstawiania warunkowe: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje.   
>   
> Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć do sprawdzenia, czy wiersz już istnieje. Dla danych filmu testowych, które udostępniasz `Title` właściwość może być używana w tym celu, ponieważ jest unikatowa w ramach tytułów poszczególnych na liście:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Ten kod przyjęto założenie, że titiles są unikatowe. Jeśli ręcznie dodać zduplikowany tytuł, zostanie wyświetlony następujący wyjątek podczas następnego przeprowadzić migrację.   
>   
>  *Sekwencja zawiera więcej niż jeden element*  
>   
> Aby uzyskać więcej informacji na temat [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujące kroki zakończy się niepowodzeniem w przypadku tworzenia nie w tym momencie.)

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Ta migracja tworzy nową bazę danych, dlatego należy usunąć *movie.mdf* pliku w poprzednim kroku.

W **Konsola Menedżera pakietów** okna, wprowadź polecenie `add-migration Initial` do utworzenia początkowej migracji. Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzony.

![](adding-a-new-field/_static/image6.png)

Migracje Code First tworzy innego pliku klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Aby pomóc w kolejności z sygnaturą czasową wstępnie ustala się filename migracji. Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje dotyczące tworzenia `Movies` tabeli bazy danych filmu. Po zaktualizowaniu bazy danych w instrukcji poniżej, *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych. Następnie przy użyciu **inicjatora** metoda zostaną uruchomione, aby wypełnić bazę danych z danych testowych.

W **Konsola Menedżera pakietów**, wprowadź polecenie `update-database` do tworzenia bazy danych i uruchamiania `Seed` metody.

![](adding-a-new-field/_static/image7.png)

Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych i należy wykonać `update-database`. W takim przypadku usuń *Movies.mdf* ponownie, a następnie spróbuj ponownie `update-database` polecenia. Jeśli nadal występuje błąd, Usuń migrations folder i zawartość, a następnie uruchomić z instrukcjami w górnej części strony (który jest delete *Movies.mdf* pliku, a następnie przejdź do Enable-Migrations). Jeśli nadal eror, Otwórz Eksplorator obiektów SQL Server i usunąć bazę danych z listy.

Uruchom aplikację i przejdź do */Movies* adresu URL. Są wyświetlane dane inicjatora.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Rozpocznij od dodania nowej `Rating` właściwości do istniejącej `Movie` klasy. Otwórz *Models\Movie.cs* plik i dodać `Rating` właściwości podobne do następującego:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Pełną `Movie` klasy teraz wygląda podobnie do następującego kodu:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Tworzenie aplikacji (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania *białą listę* dzięki tej nowej właściwości zostaną uwzględnione. Aktualizacja `bind` atrybutu dla `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Otwórz *\Views\Movies\Index.cshtml* plik i dodać `<th>Rating</th>` tuż po nagłówek kolumny **cen** kolumny. Następnie dodaj `<td>` kolumny zbliża się koniec szablon do renderowania `@item.Rating` wartość. Poniżej znajduje się jakie zaktualizowane *Index.cshtml* Wyświetl szablon wygląda następująco:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Następnie otwórz folder *\Views\Movies\Create.cshtml* plik i dodać `Rating` pole z następujących znaczników highlighed. Renderuje pola tekstowego, tak aby po utworzeniu nowego movie, można określić klasyfikację.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Użytkownik zaktualizował teraz kodu aplikacji do obsługi nowej `Rating` właściwości.

Uruchom aplikację i przejdź do */Movies* adresu URL. Po wykonaniu tej czynności, zostanie wyświetlony jeden z następujących błędów:

![](adding-a-new-field/_static/image9.png)  
  
Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First można zaktualizować bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Ten błąd jest wyświetlane, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)


Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework. Ta metoda jest bardzo wygodny wczesnym etapie cyklu programowanie podczas wprowadzania active Programowanie w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych. Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych — dlatego możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych! Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji. Aby uzyskać więcej informacji o inicjatorach bazy danych programu Entity Framework, zobacz [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.
3. Użyj migracje Code First, aby zaktualizować schemat bazy danych.


W tym samouczku użyjemy migracje Code First.

Seed — metoda aktualizacji, dzięki czemu zapewnia wartość dla nowej kolumny. Otwórz plik Migrations\Configuration.cs i Dodaj pole klasyfikacji do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenie:

`add-migration Rating`

`add-migration` Polecenie informuje framework migracji do badania bieżącego modelu film z bieżącego schematu filmu bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu. Nazwa *klasyfikacji* dowolnej i jest używany do nazywania plików migracji. Warto użyć opisową nazwę krok migracji.

Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Skompiluj rozwiązanie, a następnie wprowadź `update-database` w **Konsola Menedżera pakietów** okna.

Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (dołączanie sygnaturę daty *klasyfikacji* będą inne.)

![](adding-a-new-field/_static/image11.png)

Ponownie uruchom aplikację i przejdź do adresu URL /Movies. Widać nowe pole klasyfikacji.

![](adding-a-new-field/_static/image12.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu. Należy pamiętać, że można dodać ocenę.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Kliknij przycisk **Utwórz**. Nowe film, w tym ocenę, teraz wyświetlone w filmy wyświetlania:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Teraz, gdy projekt używa migracji, nie musisz porzucić bazy danych podczas dodawania nowego pola lub w przeciwnym razie aktualizacji schematu. W następnej sekcji możemy wprowadzić dodatkowe zmiany schematu i użyj migracji do aktualizacji bazy danych.

Należy również dodać `Rating` pole do widoku szablonów edycji, szczegóły i Delete.

Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i żaden kod migracji może działać, ponieważ schemat zgodny z modelem. Jednak uruchomiona "update-database" zostanie uruchomiony `Seed` metody ponownie, i jeśli zmieniono żadnych danych inicjatora, zmiany zostaną utracone, ponieważ `Seed` metody upserts danych. Możesz przeczytać dodatkowe informacje `Seed` metody w Dykstra Tomasz popularnych [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

W tej sekcji przedstawiono sposób modyfikowania modelu obiektów i zachować synchronizację ze zmianami bazy danych. Przedstawiono również sposób wypełnienia nowo utworzonej bazy danych z przykładowymi danymi, więc można wypróbować scenariuszy. To właśnie szybkie wprowadzenie do Code First, zobacz [tworzenia modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) bardziej szczegółowy samouczek na temat. Następnie Oto jak można dodać bardziej rozbudowane logikę weryfikacji dla klasy modelu i włączyć niektóre reguły biznesowe są wymuszane.

>[!div class="step-by-step"]
[Poprzednie](adding-search.md)
[dalej](adding-validation.md)
