---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodawanie nowego pola | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: a4eb759646fc77bbba687d8b4914465d58cd3aaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819854"
---
<a name="adding-a-new-field"></a>Dodawanie nowego pola
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji użyjesz migracje Code First Framework jednostki migracji pewne zmiany do klasy modelu, aby zmiana została zastosowana do bazy danych.

Domyślnie gdy używasz programu Entity Framework Code First automatycznie utworzyć bazę danych, tak jak wcześniej w tym samouczku rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z. Jeśli nie są zsynchronizowane, platformy Entity Framework zgłasza błąd. Ta funkcja ułatwia śledzenie problemów w czasie programowania, które mogą w przeciwnym razie tylko się okazać (przez zasłoniętej błędy) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie funkcji migracje Code First dla zmiany modelu

Przejdź do Eksploratora rozwiązań. Kliknij prawym przyciskiem myszy *Movies.mdf* plik i wybierz **Usuń** można usunąć bazy danych filmów. Jeśli nie widzisz *Movies.mdf* plików, kliknij na **Pokaż wszystkie pliki** ikonę przedstawionym poniżej na czerwone obramowanie.

![](adding-a-new-field/_static/image1.png)

Utwórz aplikację, aby upewnić się, że nie ma żadnych błędów.

Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** i następnie **Konsola Menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

W **Konsola Menedżera pakietów** okna w `PM>` wierszu wprowadź

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-Migrations** polecenie (jak pokazano powyżej) umożliwia utworzenie *Configuration.cs* plik w nowym *migracje* folderu.

![](adding-a-new-field/_static/image4.png)

Zostanie otwarty program Visual Studio *Configuration.cs* pliku. Zastąp `Seed` method in Class metoda *Configuration.cs* pliku następującym kodem:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Umieść kursor nad czerwona linia falista pod `Movie` i kliknij przycisk `Show Potential Fixes` a następnie kliknij przycisk **przy użyciu** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Spowoduje to dodanie następujących instrukcję using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Kod wywołuje metodę migracji First `Seed` metody po każdej migracji (oznacza to, że wywołanie **update-database** w konsoli Menedżera pakietów), i ta metoda aktualizuje wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieją.
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda poniższy kod wykonuje operację "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Ponieważ [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda uruchamia się za pomocą każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy próbujesz dodać będą już dostępne po pierwszej migracji, który tworzy bazę danych. "[Upsert](http://en.wikipedia.org/wiki/Upsert)" operacja zapobiega błędom, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, lecz zastępuje ona wszelkie zmiany w danych, które mogły zostać wprowadzone podczas testowania aplikacji. Zawierającej dane testowe w niektórych tabel nie można było to możliwe: w niektórych przypadkach po zmianie danych podczas testowania mają zmiany po aktualizacji bazy danych. W takim przypadku chcesz zrobić to operacja wstawiania warunkowego: Wstaw wiersz, tylko wtedy, gdy jeszcze nie istnieje.   
> 
> Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć, aby sprawdzić, czy wiersz już istnieje. Dla danych filmu testów, które są podawane `Title` właściwość może być używana w tym celu, ponieważ w ramach tytułów poszczególnych na liście jest unikatowa:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Ten kod zakłada, że tytuły są unikatowe. Jeśli ręcznie dodasz zduplikowane tytuł, otrzymasz następujący wyjątek podczas następnego przeprowadzić migrację.   
> 
>  *Sekwencja zawiera więcej niż jeden element*  
> 
> Aby uzyskać więcej informacji na temat [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.** (Następujących kroków zakończy się niepowodzeniem, jeśli nie utworzysz w tym momencie.)

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Tej migracji tworzy nową bazę danych, dlatego możesz usunąć *movie.mdf* pliku w poprzednim kroku.

W **Konsola Menedżera pakietów** okna, wprowadź polecenie `add-migration Initial` do utworzenia początkowej migracji. Nazwa "Początkowego" dowolnej i jest używany do nazywania plików migracji utworzone.

![](adding-a-new-field/_static/image6.png)

Migracje Code First tworzy inny plik klasy w *migracje* folderze (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności. Sprawdź *{DateStamp}\_Initial.cs* plik zawiera instrukcje dotyczące tworzenia `Movies` tabeli bazy danych filmów. Kiedy aktualizować bazę danych w instrukcjach poniżej to *{DateStamp}\_Initial.cs* pliku zostaną uruchomione i Utwórz schemat bazy danych. A następnie **inicjatora** metoda zostanie uruchomiony do wypełniania bazy danych z danymi.

W **Konsola Menedżera pakietów**, wprowadź polecenie `update-database` do tworzenia bazy danych i uruchamiania `Seed` metody.

![](adding-a-new-field/_static/image7.png)

Jeśli otrzymasz komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych oraz zanim zostanie wykonany `update-database`. W takim przypadku usuń *Movies.mdf* ponownie plik, a następnie spróbuj ponownie `update-database` polecenia. Jeśli nadal wystąpi błąd, usuń folder migracje i zawartość, a następnie uruchomić za pomocą instrukcji u góry tej strony (to delete *Movies.mdf* pliku, a następnie przejść do Enable-Migrations). Jeśli nadal występują Twoja nazwavstutorialfactory, Otwórz Eksplorator obiektów SQL Server, a następnie usunąć bazę danych z listy.

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Dane inicjatora są wyświetlane.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Rozpocznij, dodając nowe `Rating` właściwości do istniejących `Movie` klasy. Otwórz *Models\Movie.cs* pliku i Dodaj `Rating` właściwość podobny do poniższego:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Pełne `Movie` klasy teraz wygląda podobnie do poniższego kodu:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Utwórz aplikację (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania *białą listę* dzięki tej nowej właściwości zostaną dołączone. Aktualizacja `bind` atrybutu dla `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Należy również zaktualizować Przeglądanie szablonów, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Otwórz *\Views\Movies\Index.cshtml* pliku i Dodaj `<th>Rating</th>` nagłówek kolumny, tuż za **cena** kolumny. Następnie dodaj `<td>` kolumny w końcowej części szablonu do renderowania `@item.Rating` wartość. Poniżej przedstawiono, jakie zaktualizowane *Index.cshtml* Wyświetl szablon wygląda następująco:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Następnie otwórz *\Views\Movies\Create.cshtml* pliku i Dodaj `Rating` pole za pomocą następujących znaczników highlighed. Renderuje pola tekstowego, tak, aby można było określić klasyfikację, gdy zostanie utworzony nowy film.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Użytkownik zaktualizował teraz kod aplikacji do obsługi nowej `Rating` właściwości.

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Gdy to zrobisz, zobaczysz jedną z następujących błędów:

![](adding-a-new-field/_static/image9.png)  
  
Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Widzisz ten błąd, ponieważ zaktualizowanego `Movie` klasy modelu w aplikacji teraz różni się od schematu `Movie` tabeli istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)


Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy robią active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu możesz *nie* chcesz użyć tej metody w produkcyjnej bazie danych! Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji. Aby uzyskać więcej informacji na temat inicjatory bazy danych programu Entity Framework, zobacz [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.
3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.


W tym samouczku użyjemy migracje Code First.

Seed — metoda należy zaktualizować tak, aby go oferuje wartości dla nowej kolumny. Otwórz plik Migrations\Configuration.cs i dodać pole klasyfikacji do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wpisz następujące polecenie:

`add-migration Rating`

`add-migration` Polecenie informuje platformę migracji, aby zbadać bieżącej modelu movie z bieżącym schemacie filmu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu. Nazwa *ocena* dowolnej i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę krok migracji.

Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMigration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Skompiluj rozwiązanie, a następnie wprowadź `update-database` polecenia w pliku **Konsola Menedżera pakietów** okna.

Na poniższej ilustracji przedstawiono dane wyjściowe w **Konsola Menedżera pakietów** okna (dołączenie sygnatura daty *ocena* będzie się różnił.)

![](adding-a-new-field/_static/image11.png)

Uruchom ponownie aplikację, a następnie przejdź do adresu URL /Movies. Możesz zobaczyć nowe pole klasyfikacji.

![](adding-a-new-field/_static/image12.png)

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film. Należy pamiętać, że można dodawać ocenę.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Kliknij przycisk **Utwórz**. Ten nowy film, w tym klasyfikacji, wyświetlane w filmach, wyświetlanie listy:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Teraz, gdy projekt korzysta z migracji, nie musisz porzucić bazy danych, podczas dodawania nowego pola, lub w przeciwnym razie zaktualizowania schematu. W następnej sekcji możemy wprowadzić więcej zmian schematu i użyć migracje aktualizacji bazy danych.

Należy również dodać `Rating` pole Przeglądanie szablonów Edytuj, szczegóły i Delete.

Można wprowadzić polecenie "update-database" w **Konsola Menedżera pakietów** ponownie okno i nie ma kodu migracji uruchomić, ponieważ schemat jest zgodna z modelem. Jednak uruchomiona "Aktualizacja bazy danych" zostanie uruchomiony `Seed` metoda ponownie, i jeśli zmienisz jakiekolwiek danych inicjatora, zmiany zostaną utracone, ponieważ `Seed` danych wykonuje operację UPSERT metody. Przeczytaj więcej o `Seed` method in Class metoda Tom Dykstra popularnych [samouczek platformy ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

W tej sekcji pokazano, jak można modyfikować obiekty modelu i synchronizację bazy danych przy użyciu zmian. Przedstawiono również sposób wypełnić nowo utworzoną bazę danych z przykładowymi danymi, dzięki czemu możesz wypróbować scenariuszy. Jest to tylko krótkie wprowadzenie do Code First, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) bardziej szczegółowy samouczek na temat. Następnie Przyjrzyjmy się jak dodać bogatsze logikę walidacji do klas modelu i włączyć niektóre reguły biznesowe zostaną wymuszone.

> [!div class="step-by-step"]
> [Poprzednie](adding-search.md)
> [dalej](adding-validation.md)
