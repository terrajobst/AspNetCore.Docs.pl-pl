---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Wprowadzenie do składnika ASP.NET Web Pages — wprowadzanie danych bazy danych za pomocą formularzy | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku pokazano, jak utworzyć formularz wprowadzania, a następnie wprowadź dane, który można pobrać z formularza do tabeli bazy danych użycia stron ASP.NET Web Pages (...)
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 453ed888bc219175c5a3a25f857dfa79ab22b608
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753625"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Wprowadzenie do składnika ASP.NET Web Pages — wprowadzanie danych bazy danych za pomocą formularzy
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Ten samouczek pokazuje, jak utworzyć formularz wprowadzania, a następnie wprowadź dane, który można pobrać z formularza do tabeli bazy danych użycia stron ASP.NET Web Pages (Razor). Przyjęto założenie, że zostały wykonane serii za pośrednictwem [podstawy formularzy HTML strony ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Zawartość:
> 
> - Więcej informacji o sposobie przetwarzania formularzach wprowadzania.
> - Jak dodawać dane (Wstaw) w bazie danych.
> - Jak upewnić się, że użytkownicy wprowadzono wymaganej wartości w postaci (sposób sprawdzania poprawności danych wejściowych użytkownika).
> - Jak wyświetlić błędy sprawdzania poprawności.
> - Jak przejść do innej strony z bieżącej strony.
>   
> 
> Funkcje/technologie omówione:
> 
> - `Database.Execute` Metody.
> - SQL `Insert Into` — instrukcja
> - `Validation` Pomocnika.
> - `Response.Redirect` Metody.


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

W tym samouczku wcześniej pokazujący, jak utworzyć bazę danych, wprowadzone dane z bazy danych, edytując bazy danych bezpośrednio w programie WebMatrix, pracy w **bazy danych** obszaru roboczego. W przypadku większości aplikacji, która nie jest praktycznym sposobem dane umieszczane w bazie danych, mimo że. Dlatego w tym samouczku utworzysz opartego na sieci web interfejs, umożliwiający Ty, ani wprowadzać dane i zapisz go w bazie danych.

Utworzysz strony, w którym możesz wprowadzić nowe filmy. Strona będzie zawierać formularza zgłoszenia, który ma pola (pola tekstowe), gdzie można wprowadzić tytuł filmu, gatunku i roku. Strona będzie wyglądała podobna do tej strony:

!["Dodawanie filmu" strony w przeglądarce](entering-data/_static/image1.png)

Pola tekstowe będzie HTML `<input>` elementy, które będzie wyglądać jak ten kod znaczników:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Tworzenie formularza podstawowego

Utwórz stronę o nazwie *AddMovie.cshtml*.

Zastąp, co znajduje się w pliku następującym kodem. Zastąp wszystko; wkrótce zostanie dodana blok kodu u góry.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Ten przykład przedstawia typowy HTML podczas tworzenia formularza. Używa ona `<input>` elementów dla pola tekstowe i przycisk Prześlij. Podpisy dla pola tekstowe są tworzone przy użyciu standardu `<label>` elementów. `<fieldset>` i `<legend>` elementy umieść nieuprzywilejowany ramkę wokół formularza.

Należy zauważyć, że na tej stronie `<form>` element używa `post` jako wartość pozycji `method` atrybutu. W poprzednim samouczku utworzono formularz, który jest używany `get` metody. Było poprawne, mimo że przesłać formularza, wartości do serwera, żądanie nie wprowadzone zmiany. Wszystko, co tak było pobierania danych na różne sposoby. Jednak w tym stronie *będzie* zmiany — zamierzamy dodawać nowe rekordy bazy danych. Dlatego należy używać tego formularza `post` metody. (Aby uzyskać więcej informacji o różnicach między `GET` i `POST` operacji, zobacz[GET, POST i bezpieczeństwa zlecenie HTTP](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) paska bocznego w poprzednim samouczku.)

Należy zauważyć, że każde pole tekstowe ma `name` — element (`title`, `genre`, `year`). Jak w poprzednim samouczku, te nazwy są ważne, ponieważ muszą mieć te nazwy, dzięki czemu możesz później uzyskać dane wejściowe użytkownika. Możesz użyć dowolnej nazwy. Pomocne jest użycie znaczące nazwy, które ułatwiają należy pamiętać, jakie dane pracujemy z użyciem.

`value` Atrybut każdego `<input>` element zawiera ilość kodu Razor (na przykład `Request.Form["title"]`). Pokazaliśmy ci, wersja ta procedura w poprzednim samouczku, aby zachować wartość wprowadzona w polu tekstowym (jeśli istnieją) po przesłaniu formularza.

## <a name="getting-the-form-values"></a>Pobieranie wartości formularza

Następnie dodasz kod, który przetwarza formularza. Konspekt artykułu będzie wykonywać następujące czynności:

1. Sprawdź, czy strona jest przesyłana (przesłano). Chcesz w kodzie tylko wtedy, gdy użytkownicy kliknięty przycisk, nie wtedy, gdy strona pierwszego uruchomienia.
2. Pobiera wartości, które użytkownik wprowadził w polach tekstowych. W tym przypadku ponieważ korzysta z formularza `POST` zlecenie, Pobierz wartości formularza z `Request.Form` kolekcji.
3. Wstaw wartości jako nowego rekordu w *filmy* tabeli bazy danych.

W górnej części pliku Dodaj następujący kod:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Pierwszych kilka wierszy tworzenia zmiennych (`title`, `genre`, i `year`) do przechowywania wartości w polach tekstowych. Wiersz `if(IsPost)` upewnia się, że zmienne są ustawiane *tylko* po użytkownik kliknie **dodać film** przycisk — oznacza to, kiedy formularz została opublikowana.

Jak przedstawiono wcześniej samouczka można pobrać wartość w polu tekstowym za pomocą wyrażenia takie jak `Request.Form["name"]`, gdzie *nazwa* nazywa się `<input>` elementu.

Nazwy zmiennych (`title`, `genre`, i `year`) są dowolne. Takie jak nazwy, które można przypisać do `<input>` elementy, można je wywoływać wszystko chcesz. (Nazwy zmiennych nie muszą być zgodne z atrybutami name `<input>` elementów w formularzu.) Ale podobnie jak w przypadku `<input>` elementów, to dobry pomysł, aby użyć nazwy zmiennych, które odzwierciedlają dane, które zawierają. Podczas pisania kodu, spójne nazwy ułatwiają aby zapamiętać dane pracujemy z użyciem.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

W kodzie zablokujemy Cię właśnie dodanej, po prostu *wewnątrz* zamykającego nawiasu klamrowego ( `}` ) z `if` blokuje (nie tylko wewnątrz bloku kodu), Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Ten przykład jest podobny do kodu, którego użyto w poprzednim samouczku do pobierania i wyświetlania danych. Wiersz, który rozpoczyna się od `db =` otwiera bazy danych, takich jak wcześniej i następny wiersz określa instrukcję SQL ponownie jako przedstawiono wcześniej. Jednak tym razem definiuje SQL `Insert Into` instrukcji. Poniższy przykład pokazuje ogólna składnia `Insert Into` instrukcji:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Innymi słowy należy określić tabeli, wstawianie, wyświetlona lista kolumn do wstawienia do, a następnie listę wartości do wstawienia. (Wspomniane wcześniej SQL nie jest uwzględniana wielkość liter, ale niektórzy wielką literą słów kluczowych, aby ułatwić interpretowanie polecenie).

Kolumny, które w przypadku wstawiania do już są wymienione w poleceniu — `(Title, Genre, Year)`. Interesujące części jest, jak pobrać wartości z pól tekstowych do `VALUES` część polecenia. Zamiast wartości faktycznych można zobaczyć `@0`, `@1`, i `@2`, które z kolei są symbolami zastępczymi. Po uruchomieniu polecenia (na `db.Execute` wiersza), możesz przekazać wartości, które stało się z pól tekstowych.

**Ważne!** Należy pamiętać, że jedynie nigdy nie powinna zawierać dane wprowadzone w tryb online przez użytkownika w instrukcji SQL jest użycie symboli zastępczych, jak widać w tym miejscu (`VALUES(@0, @1, @2)`). Jeśli możesz połączyć dane wejściowe użytkownika w instrukcji SQL, należy otworzyć samodzielnie do ataku polegającego na iniekcji SQL zgodnie z objaśnieniem w [podstawy formularzy stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (poprzedniego samouczka).

Nadal wewnątrz `if` zablokować, Dodaj następujący wiersz po `db.Execute` wiersza:

[!code-css[Main](entering-data/samples/sample4.css)]

Po ten nowy film został wstawiony do bazy danych, ten wiersz skacze możesz (przekierowania) do *filmy* strony, aby było widać filmu wprowadzonej. `~` Operator oznacza "root witryny sieci Web". ( `~` Operator działa tylko na stronach ASP.NET, nie w formacie HTML ogólnie.)

Ukończony blok kodu wygląda następująco:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testowanie polecenia Insert (pory)

Nie masz jeszcze zrobione, ale teraz jest odpowiedni moment, aby przetestować.

W plikach w programie WebMatrix w widoku drzewa kliknij prawym przyciskiem myszy *AddMovie.cshtml* strony, a następnie kliknij przycisk **Uruchom w przeglądarce**.

!["Dodawanie filmu" strony w przeglądarce](entering-data/_static/image2.png)

(Jeśli na końcu innej strony w przeglądarce, upewnij się, że adres URL jest `http://localhost:nnnnn/AddMovie`), gdzie *nnnnn* jest numerem portu, którego używasz.)

Czy został wyświetlony strona błędu Jeśli tak, należy uważnie przeczytać i upewnij się, że kod wyglądał dokładnie co zostało wymienione wcześniej.

Wprowadź filmu w postaci &mdash; na przykład użyć "Dla obywateli Wójcik", "Dramat" i "1941". (Lub zależności od potrzeb). Następnie kliknij przycisk **dodać film**.

Jeśli wszystko przebiegnie poprawnie, użytkownik jest przekierowany do *filmy* strony. Upewnij się, że znajduje się nowy film.

![Strona filmy, przedstawiający nowo dodana filmu](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

Wróć do *AddMovie* strony lub uruchomić go ponownie. Wprowadź inny film, ale tym razem, wprowadź tylko tytuł &mdash; na przykład, wprowadź "Rejstrowanie rejestrowanie logowanie ' w Rain". Następnie kliknij przycisk **dodać film**.

Są przekierowywane do *filmy* strony. Możesz znaleźć ten nowy film, ale jest on niepełny.

![Filmy stronie nowy Film przedstawiający, którym brakuje niektórych wartości](entering-data/_static/image4.png)

Podczas tworzenia *filmy* tabeli jawnie wspomniałeś, żadne pole może mieć wartości null. W tym miejscu masz formularz wprowadzania dla nowych filmów, a teraz pozostawić puste pola. To jest błąd.

W tym przypadku faktycznie nie zgłaszać bazy danych (lub *throw*) wystąpił błąd. Nie podasz gatunku lub roku, dlatego ten kod w *AddMovie* strony traktować te wartości jako tak zwane *puste ciągi*. Gdy SQL `Insert Into` polecenie zadziałało, pola gatunku i rok w tamtym przydatnych danych w nich, ale nie zostały one wartość null.

Oczywiście nie chcesz umożliwić użytkownikom wprowadzanie informacji o filmach połowie puste do bazy danych. Jest to rozwiązanie do sprawdzania poprawności danych wejściowych użytkownika. Początkowo sprawdzanie poprawności po prostu zostanie upewnij się, że użytkownik wpisze wartość dla wszystkich pól (oznacza to, że żaden z nich zawiera pusty ciąg znaków).

> [!TIP]
> 
> **Wartość null i pustych ciągów**
> 
> W programowaniu istnieje różnica między różnych pojęcia "braku wartości". Ogólnie rzecz biorąc, wartość jest *null* Jeśli nigdy nie została ustawiona lub zainicjowany w dowolny sposób. Z kolei można ustawić zmiennej, która oczekuje, że dane znakowe (ciągi), aby *pusty ciąg*. W takim przypadku wartość nie jest równa null; po prostu zostały jawnie ustawiana jest na ciąg znaków, którego długość ma wartość zero. Te dwie instrukcje wyświetlenie różnicy:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Nieco jest bardziej skomplikowane niż ta, ale istotną kwestią jest to, że `null` reprezentuje rodzaj nieokreślony stan.
> 
> Teraz, a następnie ważne jest zrozumieć dokładnie gdy ma wartość null, i kiedy jest tylko pustym ciągiem. W kodzie dla *AddMovie* strony, można pobrać wartości pola przy użyciu `Request.Form["title"]` i tak dalej. Po pierwszym uruchomieniu strony (przed kliknięciem przycisku), wartość `Request.Form["title"]` ma wartość null. Jednak gdy prześlesz formularz, `Request.Form["title"]` pobiera wartość `title` pola tekstowego. Nie jest jasne, ale puste pole tekstowe nie jest równa null; ją po prostu zawiera pusty ciąg. Gdy kod jest uruchamiany w odpowiedzi na przycisk kliknij przycisk, `Request.Form["title"]` zawiera pusty ciąg.
> 
> Ta różnica jest ważna Podczas tworzenia *filmy* tabeli jawnie wspomniałeś, żadne pole może mieć wartości null. Jednak w tym miejscu masz formularz wprowadzania dla nowych filmów i jest pozostawienie pustego pola. Można oczekiwać rozsądnie bazy danych, błąd podczas próby zapisania nowe filmy, które nie mają wartości gatunku lub roku. Ale to punkt &mdash; nawet jeśli te pola tekstowe jest puste, wartości nie ma wartość null; są one puste ciągi. W rezultacie, możesz zapisać nowe filmy w bazie danych z tych kolumn pusty &mdash; , ale nie o wartości null! &mdash; wartości. W związku z tym należy upewnić się, że użytkownicy nie przesyłaj pustym ciągiem, co można zrobić, sprawdzając poprawność danych wejściowych użytkownika.


### <a name="the-validation-helper"></a>Pomocnik weryfikacji

ASP.NET Web Pages obejmuje Pomocnika &mdash; `Validation` Pomocnika &mdash; , można użyć, aby upewnić się, że użytkownicy wprowadzają dane, które spełniają Twoje wymagania. `Validation` Pomocnika jest jednym z wątków, które jest kompilowany w do stron ASP.NET Web Pages, więc nie trzeba go zainstalować jako pakiet za pomocą NuGet, sposób pomocnika Gravatar jest zainstalowany w samouczku wcześniej.

Aby sprawdzić poprawność danych wejściowych użytkownika, wykonasz następujące czynności:

- Użyj kodu, aby określić wymaganie wartości w polach tekstowych na stronie.
- Umieść testu kod tak, aby informacje filmu są dodawane do bazy danych, tylko wtedy, gdy wszystko, co sprawdza poprawność prawidłowo.
- Dodaj kod do znaczników, aby wyświetlić komunikaty o błędach.

W bloku kodu w *AddMovie* strony, prawo do góry u góry przed deklaracji zmiennych, Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Należy wywołać `Validation.RequireField` jeden raz dla każdego pola (`<input>` elementu) gdzie chcesz wymagać wpis. Można również dodać niestandardowy komunikat o błędzie dla każdego wywołania, tak jak to tutaj. (Firma Microsoft zróżnicowane wiadomości, aby zobrazować można umieścić coś, co chcesz istnieje).

W przypadku problemu chcesz uniemożliwić nowe informacje film z wstawiane do bazy danych. W `if(IsPost)` blokowania, należy użyć `&&` (operator logiczny oraz), aby dodać inny warunek, który umożliwia sprawdzenie `Validation.IsValid()`. Gdy wszystko będzie gotowe, cały `if(IsPost)` blok będzie wyglądać podobnie do tego kodu:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Jeśli występuje błąd sprawdzania poprawności z żadnym z pól, które są zarejestrowane przy użyciu `Validation` Pomocnika `Validation.IsValid` metoda zwraca wartość false. I w takim przypadku Brak kodu, w tym bloku zostaną uruchomione, dzięki czemu żadnych wpisów nieprawidłowy film zostanie wstawiony do bazy danych. I oczywiście możesz teraz nie przekierowana do *filmy* strony.

Ukończony blok kodu, w tym kod sprawdzania poprawności wygląda teraz następująco:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Wyświetlanie błędów walidacji

Ostatnim krokiem jest, aby wyświetlić komunikaty o błędach. Można wyświetlić pojedyncze wiadomości dla każdego błędu weryfikacji, lub możesz wyświetlić podsumowanie i / lub. W tym samouczku wykonasz zarówno tak, aby zobaczyć, jak to działa.

Obok każdego `<input>` element, który jest sprawdzanie poprawności, wywołanie `Html.ValidationMessage` metody i przekaż mu nazwę z `<input>` element one sprawdzania poprawności. Możesz umieścić `Html.ValidationMessage` metoda po prawej stronie, gdzie ma być wyświetlany komunikat o błędzie. Po uruchomieniu strony, `Html.ValidationMessage` metoda renderuje `<span>` elementu, gdzie błąd sprawdzania poprawności. (Jeśli nie ma błędów, `<span>` element jest renderowany, ale nie ma żadnego tekstu w nim.)

Zmień adiustację tak, na stronie, aby obejmowała `Html.ValidationMessage` metody dla każdego z trzech `<input>` elementów na stronie, takich jak w tym przykładzie:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Aby zobaczyć, jak działa podsumowania, dodać następujący kod znaczników i kodu bezpośrednio po `<h1>Add a Movie</h1>` elementu na stronie:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Domyślnie `Html.ValidationSummary` metoda Wyświetla wszystkie komunikaty sprawdzania poprawności w postaci listy ( `<ul>` element, który znajduje się wewnątrz `<div>` elementu). Podobnie jak w przypadku `Html.ValidationMessage` metody zawsze renderowania kodu znaczników dla podsumowania weryfikacji; Jeśli nie ma żadnych błędów, elementów listy są renderowane.

Podsumowanie może być alternatywny sposób wyświetlania komunikatów dotyczących sprawdzania poprawności, a nie za pomocą `Html.ValidationMessage` metodę w celu wyświetlenia każdego błędu określonego pola. Możesz też podsumowanie i szczegóły. Możesz też `Html.ValidationSummary` metodę, aby wyświetlić ogólny błąd, a następnie użyj poszczególnych `Html.ValidationMessage` wywołania w celu wyświetlenia szczegółowych informacji.

Stronie zakończenia wygląda teraz następująco:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

To wszystko. Teraz możesz przetestować stronę przez dodawanie filmu, ale pomijając jedną lub więcej pól. Po wykonaniu, zostanie wyświetlony następujący błąd wyświetlania:

![Dodaj stronę Film przedstawiający komunikaty o błędach weryfikacji](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Ustawianie stylów komunikatów o błędach weryfikacji

Widać, że istnieją komunikaty o błędach, ale ich nie naprawdę wyróżniały się bardzo dobrze. Jest łatwe do określania stylu komunikatów o błędach, mimo że.

Do określania stylu poszczególne komunikaty o błędach, które są wyświetlane przez `Html.ValidationMessage`, Utwórz klasę style CSS, o nazwie `field-validation-error`. Aby zdefiniować wygląd podsumowania weryfikacji, należy utworzyć klasę style CSS o nazwie `validation-summary-errors`.

Aby zobaczyć, jak działa ta technika, Dodaj `<style>` element wewnątrz `<head>` części strony. Następnie zdefiniuj stylu klasy o nazwie `field-validation-error` i `validation-summary-errors` zawierające następujące reguły:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Zwykle prawdopodobnie umieścić informacje dotyczące stylu w oddzielnych *.css* pliku, ale dla uproszczenia można je umieścić w stronę teraz. (W dalszej części tego samouczka zestawu przeniesienie reguły CSS do osobnego *.css* pliku.)

Jeśli występuje błąd sprawdzania poprawności `Html.ValidationMessage` metoda renderuje `<span>` element, który zawiera `class="field-validation-error"`. Dodanie definicji stylu dla tej klasy, można skonfigurować komunikat wygląda następująco. Jeśli występują błędy, `ValidationSummary` metoda renderuje podobnie dynamicznie atrybut `class="validation-summary-errors"`.

Ponownie uruchom strony i celowo Opuść kilka pól. Błędy są teraz lepiej widoczne. (W rzeczywistości są one overdone, ale jest, że jedynie pokazanie, co można zrobić).

![Dodaj stronę Film przedstawiający błędy sprawdzania poprawności, które zostały różne](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Dodawanie Linku do strony filmy

Jeden ostatnim krokiem jest zapewnienie wygodne przejść do *AddMovie* strony z oryginalnej listy filmów.

Otwórz *filmy* strony. Po zamykającym `</div>` tag, który następuje po `WebGrid` pomocnika, Dodaj następujący kod:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Jak przedstawiono wcześniej ASP.NET interpretuje `~` operatora jako katalogu głównego witryny sieci Web. Nie trzeba stosować `~` operatora; można użyć znaczników `<a href="./AddMovie">Add a movie</a>` lub w inny sposób zdefiniować ścieżkę, która rozumie HTML. Ale `~` operator to dobra metoda ogólne podczas tworzenia łączy do stron Razor, ponieważ sprawia, że witryna bardziej elastyczne — Jeśli przesuniesz bieżącą stronę do podfolderu łącze nadal przejdzie *AddMovie* strony. (Należy pamiętać, że `~` operator działa tylko w *.cshtml* stron. ASP.NET rozpoznaje ją, ale nie jest to standardowy HTML).

Gdy wszystko będzie gotowe, uruchom *filmy* strony. Ta strona będzie wyglądać:

![Strona filmów z linkiem do strony "Dodaj filmy"](entering-data/_static/image7.png)

Kliknij przycisk **Dodawanie filmu** łącze, aby upewnić się, że przejdzie do *AddMovie* strony.

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku dowiesz się, jak umożliwić użytkownikom edytowanie danych, który jest już w bazie danych.

## <a name="complete-listing-for-addmovie-page"></a>Kompletna lista AddMovie strony

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Wstaw do instrukcji SQL](http://www.w3schools.com/sql/sql_insert.asp) witrynie W3Schools
- [Walidacja danych wejściowych użytkownika we wzorcu ASP.NET Web Pages witryn](https://go.microsoft.com/fwlink/?LinkId=253002). Więcej informacji na temat pracy z `Validation` pomocnika.

> [!div class="step-by-step"]
> [Poprzednie](form-basics.md)
> [dalej](updating-data.md)
