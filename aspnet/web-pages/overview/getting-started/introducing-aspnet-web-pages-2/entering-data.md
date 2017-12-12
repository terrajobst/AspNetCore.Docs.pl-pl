---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: "Wprowadzenie do składnika ASP.NET Web Pages — wprowadzanie danych bazy danych za pomocą formularzy | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym samouczku przedstawiono sposób tworzenia formularza wpis, a następnie wprowadź dane, które otrzymujesz z formularza do tabeli bazy danych użycia stron ASP.NET Web Pages (...)"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b74eecb16b2c4695bb417816b90f701f724cc9d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Wprowadzenie do składnika ASP.NET Web Pages — wprowadzanie danych bazy danych za pomocą formularzy
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku przedstawiono sposób tworzenia formularza wpis, a następnie wprowadź dane, które otrzymujesz z formularza do tabeli bazy danych użycia stron sieci Web platformy ASP.NET (Razor). Przyjęto założenie, że zostały wykonane serii za pomocą [podstawy formularzy HTML strony ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Zawartość:
> 
> - Więcej informacji na temat sposobu przetwarzania wpisu formularzy.
> - Jak dodać (Wstaw) danych w bazie danych.
> - Jak sprawdzić, czy użytkownicy zostały wprowadzone wymaganej wartości w postaci (sposób sprawdzania poprawności danych wejściowych użytkownika).
> - Jak wyświetlać błędy sprawdzania poprawności.
> - Jak przejść do innej strony z bieżącej strony.
>   
> 
> Funkcje/technologie omówione:
> 
> - `Database.Execute` Metody.
> - SQL `Insert Into` — instrukcja
> - `Validation` Pomocnika.
> - `Response.Redirect` Metody.


## <a name="what-youll-build"></a>Jakie będzie kompilacji

Samouczek wcześniej który pokazano, jak utworzyć bazę danych, należy wprowadzić dane z bazy danych, edytując bazy danych bezpośrednio w programie WebMatrix, pracy w **bazy danych** obszaru roboczego. W przypadku większości aplikacji, która nie jest praktycznym sposobem umieszczanie danych w bazie danych, mimo że. Dlatego w tym samouczku utworzysz opartych na sieci web interfejs, umożliwiający lub wprowadź dane i zapisać ją w bazie danych.

Utworzysz strony, gdzie można wprowadzić nowości. Strona będzie zawierać formularz wpis, który ma pola (pola tekstowe), gdzie można wprowadzić tytuł filmu, genre i roku. Strona będzie wyglądała tej strony:

!["Dodawanie filmu" strony w przeglądarce](entering-data/_static/image1.png)

Pola tekstowe będzie HTML `<input>` elementy, które będą wyglądały jak tego znacznika:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Tworzenie formularza podstawowego

Utwórz stronę o nazwie *AddMovie.cshtml*.

Zastąp nowości w pliku o następujących znaczników. Zastąp wszystkie; wkrótce zostanie dodana blok kodu u góry.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Ten przykład przedstawia typowy HTML podczas tworzenia formularza. Używa `<input>` elementy dla pól tekstowych i przycisku Prześlij. Podpisy dla pola tekstowe są tworzone przy użyciu standardu `<label>` elementów. `<fieldset>` i `<legend>` elementów put nieuprzywilejowany pole wokół formularza.

Zwróć uwagę, że na tej stronie `<form>` element używa `post` jako wartość `method` atrybutu. Poprzednie samouczka utworzone formularza, który jest używany `get` metody. Która jest poprawna, ponieważ mimo że przesłaniem formularza, wartości na serwerze, żądanie nie wprowadzono żadnych zmian. Wszystkie jak zostało pobierania danych na różne sposoby. Jednak w tym stronie *będzie* zmiany — możesz zacząć dodawać nowe rekordy z bazy danych. W związku z tym należy używać tego formularza `post` metody. (Aby uzyskać więcej informacji o różnicach między `GET` i `POST` operacji, zobacz[GET, POST i bezpieczeństwa zlecenie HTTP](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) paska bocznego w samouczku poprzedniej.)

Należy pamiętać, że każde pole tekstowe ma `name` elementu (`title`, `genre`, `year`). Jak przedstawiono w poprzednim samouczek te nazwy są ważne, ponieważ muszą mieć te nazwy, dzięki czemu możesz później uzyskać danych wejściowych użytkownika. Można użyć dowolnej nazwy. Warto Użyj znaczące nazwy, które ułatwiają należy pamiętać, jakie dane, z którymi pracujesz.

`value` Atrybut każdego `<input>` element zawiera bitowego kodu Razor (na przykład `Request.Form["title"]`). Znasz wersja ta procedura poprzedniej samouczka w celu zachowania wartości wprowadzonej w polu tekstowym (jeśli istnieje), po przesłaniu formularza.

## <a name="getting-the-form-values"></a>Pobieranie wartości formularza

Następnie użytkownik dodaje kod, który przetwarza formularza. Konspekt będzie wykonywać następujące czynności:

1. Sprawdź, czy strona jest przesyłana (zostało przesłane). Chcesz kodu do uruchamiania tylko wtedy, gdy użytkownicy kliknięty przycisk, nie po stronie pierwszego uruchomienia.
2. Pobiera wartości, które użytkownik wprowadził do pól tekstowych. W takim przypadku ponieważ używa formularza `POST` zlecenie, możesz uzyskać wartości formularza z `Request.Form` kolekcji.
3. Wstaw wartości jako nowy rekord w *filmy* tabeli bazy danych.

W górnej części pliku Dodaj następujący kod:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Zmienne utworzyć kilka pierwszych wierszy (`title`, `genre`, i `year`) do przechowywania wartości w polach tekstowych. Wiersz `if(IsPost)` upewnia się, że zmienne są ustawione *tylko* po kliknięciu **dodać film** przycisk — to znaczy kiedy formularz została opublikowana.

Jak przedstawiono wcześniej samouczka pobrać wartości pola tekstowego przy użyciu wyrażeń, takich jak `Request.Form["name"]`, gdzie *nazwa* jest nazwą `<input>` elementu.

Nazwy zmiennych (`title`, `genre`, i `year`) są dowolne. Takich jak nazwy, które są przypisywane do `<input>` elementy, możesz je wywołać niczego chcesz. (Nazwy zmiennych nie muszą być zgodne z atrybutami name `<input>` elementów w formularzu.) Jednak w przypadku `<input>` elementy, to warto użyć nazwy zmiennych, które odzwierciedla dane, które zawierają. Podczas pisania kodu spójnych nazw ułatwiają należy pamiętać, jakie dane, z którymi pracujesz.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

W kodzie zablokować właśnie dodanej, po prostu *wewnątrz* zamykający nawias klamrowy ( `}` ) z `if` zablokować (nie tylko wewnątrz bloku kodu), Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

W tym przykładzie jest podobny do kodu, używanego w poprzednich instrukcji do pobierania i wyświetlania danych. Wiersz, który rozpoczyna się od `db =` otwiera bazę danych, takich jak wcześniej i następnego wiersza definiuje instrukcję SQL ponownie jako przedstawiono wcześniej. Jednak teraz definiuje SQL `Insert Into` instrukcji. W poniższym przykładzie przedstawiono ogólna składnia `Insert Into` instrukcji:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Innymi słowy należy określić tabeli, aby wstawić do, wyświetlona lista kolumn do wstawienia do, a następnie listy wartości do wstawienia. (Wspomnianego przed SQL nie jest uwzględniana wielkość liter, ale niektóre osoby wielką słów kluczowych, aby ułatwić odczytu polecenie).

Kolumny, które są wstawianie do już znajdują się w poleceniu — `(Title, Genre, Year)`. Część interesujące jest, jak uzyskać wartości w polach tekstowych w `VALUES` część polecenia. Zamiast wartości rzeczywistych, zobacz `@0`, `@1`, i `@2`, które są oczywiście symbole zastępcze. Po uruchomieniu polecenia (na `db.Execute` wiersza), przekazać wartości, które masz od pól tekstowych.

**Ważne!** Należy pamiętać, że jedynym sposobem kiedykolwiek powinny obejmować dane wprowadzane w trybie online przez użytkownika w instrukcji SQL jest użycie symboli zastępczych, jak widać w tym miejscu (`VALUES(@0, @1, @2)`). Jeśli łączenie danych wejściowych użytkownika do instrukcji SQL, należy otworzyć samodzielnie na atak iniekcji kodu SQL, zgodnie z objaśnieniem w [podstawy formularza stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (samouczek poprzedniej).

Nadal wewnątrz `if` bloków, Dodaj następujący wiersz po `db.Execute` wiersza:

[!code-css[Main](entering-data/samples/sample4.css)]

Po nowych film został wstawiony do bazy danych, ten wiersz przechodzi możesz (przekierowania) do *filmy* strony, dzięki czemu filmu podanej. `~` Operator oznacza "root witryny sieci Web". ( `~` Operator działa tylko w stron ASP.NET nie jest w formacie HTML zazwyczaj.)

Ukończony blok kodu wygląda następująco:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testowanie polecenia wstawiania (wykonanej do tej pory)

Nie jest jeszcze gotowe, lecz teraz jest odpowiedni moment, aby przetestować.

W widoku drzewa plików w programie WebMatrix, kliknij prawym przyciskiem myszy *AddMovie.cshtml* a następnie kliknij przycisk **Uruchom w przeglądarce**.

!["Dodawanie filmu" strony w przeglądarce](entering-data/_static/image2.png)

(Jeśli na końcu innej strony w przeglądarce, upewnij się, że adres URL jest `http://localhost:nnnnn/AddMovie`), gdzie  *nnnnn*  jest numerem portu, którego używasz.)

Czy został wyświetlony stronę błędu Jeśli tak, należy uważnie przeczytać i upewnij się, że kod wygląda dokładnie co był wcześniej.

Wprowadź filmu w postaci &mdash; na przykład użyć "Obywateli Wójcik", "Teatralne" i "1941". (Lub niezależnie od) Następnie kliknij przycisk **dodać film**.

Jeśli wszystko przebiegnie poprawnie, jest przekierowywane do *filmy* strony. Upewnij się, czy wymieniony jest nowy filmu.

![Strona filmy przedstawiający nowo dodana film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

Wróć do *AddMovie* strony lub uruchomić go ponownie. Wprowadź inny film, ale tym razem, wprowadź tylko tytuł &mdash; wprowadź na przykład "Rejstrowanie rejestrowanie logowanie ' w ustaniu". Następnie kliknij przycisk **dodać film**.

Są przekierowywane do *filmy* strony ponownie. Można znaleźć nowe film, ale jest niekompletna.

![Filmy strony przedstawiający nowy film Brak niektórych wartości](entering-data/_static/image4.png)

Podczas tworzenia *filmy* tabeli, należy jawnie powiedzieć, że żadne z pól może mieć wartości null. W tym miejscu masz formularz nowości i jest pozostawienie pustego pola. To jest błąd.

W takim przypadku faktycznie nie podnieść bazy danych (lub *throw*) wystąpił błąd. Użytkownik nie dostarczył genre lub rok, więc kod w *AddMovie* strony traktować te wartości jako tak zwane *puste ciągi*. Gdy SQL `Insert Into` polecenie zadziałało, genre i roku pól nie mają przydatnych danych w nich, ale nie był zerowy.

Oczywiście nie chcesz umożliwić użytkownikom wprowadzanie pusta połowie informacje do bazy danych. Rozwiązanie to sprawdzanie poprawności danych wejściowych użytkownika. Początkowo weryfikacji po prostu zostanie upewnij się, że użytkownik wprowadził wartość dla wszystkich pól (oznacza to, że żadna z nich nie zawiera pusty ciąg).

> [!TIP] 
> 
> **Wartość null i pustych ciągów**
> 
> W programowaniu jest różnica między różnych pojęcia "żadnej wartości." Ogólnie rzecz biorąc, wartość jest *null* Jeśli nigdy nie została ustawiona lub zainicjować w dowolny sposób. Z kolei może mieć wartości zmiennej, która oczekuje na dane znakowe (ciągi) *pusty ciąg*. W takim przypadku wartość nie jest równa null; jest właśnie została jawnie ustawiona na ciąg znaków, którego długość wynosi zero. Te dwie instrukcje wskazują różnice:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Nieco ma bardziej skomplikowane niż ta, ale jest to istotne, że `null` reprezentuje rodzaj nieokreślony stan.
> 
> Teraz, a następnie ważne jest zrozumienie, dokładnie gdy wartość jest równa null i gdy jest tylko pustym ciągiem. W kodzie *AddMovie* strony, należy pobrać wartości w polach tekstowych przy użyciu `Request.Form["title"]` i tak dalej. Po pierwszym uruchomieniu strony (zanim klikniesz przycisk), wartość `Request.Form["title"]` ma wartość null. Jednak po przesłaniu formularza, `Request.Form["title"]` pobiera wartość `title` pola tekstowego. Nie jest jasne, ale puste pole tekstowe nie ma wartości null; go po prostu zawiera pusty ciąg. Dlatego po kod uruchamiany w odpowiedzi na przycisk kliknij przycisk, `Request.Form["title"]` ma pusty ciąg.
> 
> Ta różnica jest ważna Podczas tworzenia *filmy* tabeli, należy jawnie powiedzieć, że żadne z pól może mieć wartości null. W tym miejscu masz formularz nowości a jest pozostawienie pustego pola. Czy spodziewać bazę danych do skarżą się, gdy próbowano zapisać nowe filmów, które nie mają wartości dla rodzaju lub roku. Jednak to punkt &mdash; nawet jeśli te pola tekstowe pozostanie puste, wartości nie ma wartości null; są one puste ciągi. W związku z tym możesz zapisać nowości w bazie danych z tych kolumn pusty &mdash; , ale nie o wartości null! &mdash;wartości. W związku z tym należy upewnić się, że użytkownicy nie przesyłaj pusty ciąg, co można zrobić poprzez sprawdzenie poprawności danych wejściowych użytkownika.


### <a name="the-validation-helper"></a>Pomocnik weryfikacji

Strony sieci Web platformy ASP.NET zawiera pomocnika &mdash; `Validation` Pomocnika &mdash; można się upewnić, że wprowadzania danych, które spełniają wymagania. `Validation` Pomocnika jest jednym z wątków, które korzysta z wbudowanej w do składnika ASP.NET Web Pages, tak aby nie trzeba go zainstalować w pakiecie przy użyciu narzędzia NuGet sposób pomocnika Gravatar jest zainstalowany w samouczku wcześniej.

Aby sprawdzić poprawność danych wejściowych użytkownika, będzie wykonaj następujące czynności:

- Określ, czy wymaga wartości w polach tekstowych na stronie przy użyciu kodu.
- Umieść test do kodu tak, aby informacje filmu są dodawane do bazy danych tylko wtedy, gdy wszystko weryfikuje poprawnie.
- Dodaj kod do znaczników, aby wyświetlić komunikaty o błędach.

W bloku kodu w *AddMovie* prawo do góry u góry przed deklaracjami zmiennych, Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Należy wywołać `Validation.RequireField` raz dla każdego pola (`<input>` element), w którym ma być wymagane wprowadzenie. Można również dodać niestandardowy komunikat o błędzie dla każdego wywołania, tak jak to tutaj. (Możemy zróżnicowane wiadomości tak, aby pokazać, że można umieścić coś, co Ci się podoba).

W przypadku problemu chcesz zapobiec nowe informacje film z wstawiane do bazy danych. W `if(IsPost)` bloków, użyj `&&` (logiczne i), aby dodać inny warunek, który umożliwia sprawdzenie `Validation.IsValid()`. Gdy wszystko będzie gotowe, cała `if(IsPost)` bloku wygląda ten kod:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Jeśli występuje błąd weryfikacji za pomocą dowolnego pola, które są zarejestrowane przy użyciu `Validation` pomocnika, `Validation.IsValid` metoda zwraca wartość false. I w takim przypadku żaden kod w tym bloku zostaną uruchomione, więc żadnych wpisów nieprawidłowy filmu zostaną wstawione do bazy danych. I oczywiście możesz jest nie przekierowana do *filmy* strony.

Ukończony blok kodu, w tym kod weryfikacji teraz wygląda w tym przykładzie:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Wyświetlanie błędów walidacji

Ostatnim krokiem jest, aby wyświetlić komunikaty o błędach. Można wyświetlić poszczególne komunikaty dla każdego błędu weryfikacji, lub możesz wyświetlić podsumowanie lub oba. W tym samouczku należy to zrobić zarówno, dzięki czemu można zobaczyć, jak działa.

Obok każdego `<input>` element, który jest sprawdzanie poprawności, wywołaj `Html.ValidationMessage` — metoda i przekaż go nazwę `<input>` element jest sprawdzania poprawności. Możesz umieścić `Html.ValidationMessage` prawo metody, w którym ma być wyświetlany komunikat o błędzie. Po uruchomieniu strony, `Html.ValidationMessage` metoda renderuje `<span>` elementu, gdzie błąd sprawdzania poprawności. (Jeśli nie było błędu, `<span>` element jest renderowany, ale nie ma żadnego tekstu w nim.)

Zmień kod znaczników na stronie, co zawierają one `Html.ValidationMessage` metody dla każdego z trzech `<input>` elementów na stronie, takich jak w tym przykładzie:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Aby zobaczyć, jak działa podsumowania, również Dodaj następujący kod znaczników i kodu bezpośrednio po `<h1>Add a Movie</h1>` elementu na stronie:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Domyślnie `Html.ValidationSummary` metoda wyświetla na liście wszystkich komunikatów dotyczących sprawdzania poprawności ( `<ul>` element, który znajduje się wewnątrz `<div>` elementu). Jak `Html.ValidationMessage` metody zawsze renderowania kodu znaczników dla podsumowania weryfikacji; Jeśli nie ma żadnych błędów, nie elementy listy są renderowane.

Podsumowanie może być alternatywny sposób wyświetlania komunikatów dotyczących sprawdzania poprawności, a nie za pomocą `Html.ValidationMessage` metodę w celu wyświetlenia każdego błędu określonego pola. Lub można użyć zarówno Podsumowanie i szczegóły. Lub użyć `Html.ValidationSummary` metodę, aby wyświetlić ogólny błąd, a następnie użyj poszczególnych `Html.ValidationMessage` wywołań, aby wyświetlić szczegóły.

Strona ukończyć teraz wygląda w tym przykładzie:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

To wszystko. Teraz możesz przetestować strony przez dodanie filmu, ale pomijając jedną lub więcej pól. Po wykonaniu, zostanie wyświetlony następujący błąd wyświetlania:

![Dodaj stronę Film przedstawiający komunikaty o błędach weryfikacji](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Nadawanie stylu komunikatów o błędach

Widać, że istnieją komunikaty o błędach, ale ich nie naprawdę wyróżniające bardzo dobrze. Jest to prosty sposób do określania stylu komunikatów o błędach, ale.

Do określania stylu poszczególne komunikaty o błędach są wyświetlane przez `Html.ValidationMessage`, Utwórz klasę stylu CSS o nazwie `field-validation-error`. Aby zdefiniować wygląd dla podsumowania weryfikacji, należy utworzyć klasę stylu CSS o nazwie `validation-summary-errors`.

Aby sprawdzić, jak działa ta technika, Dodaj `<style>` element wewnątrz `<head>` części strony. Następnie zdefiniuj styl klasy o nazwie `field-validation-error` i `validation-summary-errors` zawierających następujące reguły:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Zwykle prawdopodobnie spowodowałaby informacji o stylu w oddzielnej *.css* pliku, ale dla uproszczenia należy umieścić ich na stronie teraz. (W dalszej części tego samouczka zestawu następuje przeniesienie reguły CSS do osobnego *.css* pliku.)

Jeśli występuje błąd weryfikacji `Html.ValidationMessage` metoda renderuje `<span>` element, który zawiera `class="field-validation-error"`. Dodanie definicji stylu dla tej klasy, można skonfigurować wygląd wiadomości. Jeśli wystąpią błędy, `ValidationSummary` metoda renderuje również dynamicznie atrybutu `class="validation-summary-errors"`.

Ponownie uruchom strony i celowo Opuść kilka pól. Błędy są teraz bardziej zauważalne. (W rzeczywistości są one overdone, ale jest tak, aby pokazać, co można zrobić).

![Dodaj stronę Film przedstawiający błędy sprawdzania poprawności, które ma zostać wstawiony](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Dodawanie Linku do strony filmy

Jeden ostatnim krokiem jest ułatwia ona na uzyskanie dostępu do *AddMovie* strony z oryginalnej listy filmów.

Otwórz *filmy* strony ponownie. Po upływie `</div>` tag, który następuje `WebGrid` pomocnika, Dodaj następujący kod:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Jak przedstawiono wcześniej, ASP.NET interpretuje `~` operatora jako katalog główny witryny sieci Web. Nie trzeba używać `~` operatora; można użyć znaczników `<a href="./AddMovie">Add a movie</a>` lub w inny sposób do zdefiniował ścieżki, która obsługuje usługę HTML. Ale `~` operator jest dobrym ogólne podejście podczas tworzenia łączy do stron Razor, ponieważ lokacji zapewnia bardziej elastyczne — Jeśli bieżącej strony są przenoszone do podfolderu, link będzie nadal przejdź do *AddMovie* strony. (Należy pamiętać, że `~` operator działa tylko *.cshtml* stron. ASP.NET go rozumieją, ale nie jest to standardowy HTML).

Gdy wszystko będzie gotowe, uruchom *filmy* strony. Ta strona będzie wyglądać:

![Strona filmów z łącze do strony "Dodaj filmy"](entering-data/_static/image7.png)

Kliknij przycisk **Dodawanie filmu** łącze, aby upewnić się, że trafia do *AddMovie* strony.

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku nauczysz się, jak umożliwić użytkownikom edytowanie danych, który jest już w bazie danych.

## <a name="complete-listing-for-addmovie-page"></a>Pełna lista AddMovie strony

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Wstawianie w instrukcji SQL](http://www.w3schools.com/sql/sql_insert.asp) w witrynie W3Schools
- [Walidacja danych wejściowych użytkownika w sieci Web ASP.NET stron witryny](https://go.microsoft.com/fwlink/?LinkId=253002). Więcej informacji na temat pracy z `Validation` pomocnika.

>[!div class="step-by-step"]
[Poprzednie](form-basics.md)
[dalej](updating-data.md)
