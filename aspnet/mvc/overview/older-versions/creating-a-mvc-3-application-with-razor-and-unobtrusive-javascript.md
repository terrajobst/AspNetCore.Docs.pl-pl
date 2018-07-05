---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: MVC 3 do tworzenia aplikacji przy użyciu Razor i dyskretnym kodem JavaScript | Dokumentacja firmy Microsoft
author: microsoft
description: Przykładową aplikację sieci web listy użytkowników pokazuje, jak łatwo jest tworzyć aplikacje platformy ASP.NET MVC 3, za pomocą aparatu widoku Razor. Przykładowe s aplikacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398016"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>MVC 3 do tworzenia aplikacji przy użyciu Razor i dyskretnym kodem JavaScript
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Przykładową aplikację sieci web listy użytkowników pokazuje, jak łatwo jest tworzyć aplikacje platformy ASP.NET MVC 3, za pomocą aparatu widoku Razor. Przykładowa aplikacja pokazuje, jak używać nowego aparatu widoku Razor z platformą ASP.NET MVC w wersji 3 i Visual Studio 2010 w celu utworzenia fikcyjnej listy użytkowników witryny sieci Web, która zawiera funkcje, takie jak tworzenie, wyświetlanie, edytowanie i usuwanie użytkowników.
> 
> W tym samouczku opisano kroki, które zostały wykonane w celu tworzenia przykładowej aplikacji ASP.NET MVC 3 listy użytkowników. Projekt programu Visual Studio z kodem źródłowym języka C# i VB jest dostępna powiązany z tym tematem: [Pobierz](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Jeśli masz pytania dotyczące tego samouczka, opublikuj je na [MVC forum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Omówienie

Aplikacja, której możesz tworzyć znajduje listy użytkownika prostą witrynę sieci Web. Użytkownicy mogą wprowadzać, wyświetlanie i aktualizowanie informacji o użytkowniku.

![Przykładową witrynę](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Można ściągnąć projekt ukończone VB i C# [tutaj](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby uruchomić tego samouczka, Otwórz program Visual Studio 2010, a następnie utwórz nowy projekt za pomocą *aplikacji sieci Web programu ASP.NET MVC 3* szablonu. Nazwij aplikację &quot;Mvc3Razor&quot;.

[![Nowy projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

W **nowego projektu programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**wybierz aparatu widoku Razor, a następnie kliknij przycisk **OK**.

![Okno dialogowe nowego projektu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

W tym samouczku możesz nie będzie korzystać z dostawcy członkostwa platformy ASP.NET, więc można usunąć wszystkie pliki, które są skojarzone z logowania i członkostwa. W **Eksploratora rozwiązań**, Usuń następujące pliki i katalogi:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (i wszystkie pliki w tym katalogu)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edytuj  <em>\_Layout.cshtml</em> i Zastąp kod znaczników wewnątrz `<div>` elementu o nazwie `logindisplay` z komunikatem <em>&quot;</em>wyłączone logowania&quot;. Poniższy przykład przedstawia nowy kod znaczników:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Dodawanie modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie kliknij przycisk **klasy**.

![Nowa klasa Mdl użytkownika](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nazwa klasy `UserModel`. Zastąp zawartość *UserModel* pliku następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Klasa reprezentuje użytkowników. Każdy członek tej klasy jest oznaczony za pomocą [wymagane](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybut z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw. Atrybuty w [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeń nazw zapewnia automatyczne klienta i serwera-weryfikację po stronie aplikacji sieci web.

Otwórz `HomeController` klasę i Dodaj `using` dyrektywy, dzięki czemu mogą uzyskać dostęp `UserModel` i `Users` klasy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Tuż za `HomeController` deklaracji, Dodaj poniższy komentarz i odwołania do `Users` klasy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Klasy jest magazynem danych uproszczone, w pamięci, które będzie używane w ramach tego samouczka. W rzeczywistej aplikacji należy użyć bazy danych do przechowywania informacji o użytkowniku. Pierwszych kilka wierszy tego `HomeController` plików są wyświetlane w następującym przykładzie:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Skompiluj aplikację tak, aby modelu użytkowników będzie dostępna do Kreatora tworzenia szkieletów w następnym kroku.

## <a name="creating-the-default-view"></a>Tworzenie widoku domyślnego

Następnym krokiem jest dodać metody akcji i widok, aby wyświetlić użytkowników.

Usuń istniejącą *Views\Home\Index* pliku. Zostanie utworzony nowy *indeksu* plik, aby wyświetlić użytkowników.

W `HomeController` klasy, Zastąp zawartość `Index` metoda następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Kliknij prawym przyciskiem myszy wewnątrz `Index` metody, a następnie kliknij przycisk **Dodaj widok**.

![Dodawanie widoku](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wybierz **utworzyć widok silnie typizowane** opcji. Aby uzyskać **wyświetlić klasy danych**, wybierz opcję **Mvc3Razor.Models.UserModel**. (Jeśli nie widzisz **Mvc3Razor.Models.UserModel** w **wyświetlić klasy danych** pole, należy skompilować projekt.) Upewnij się, że aparat widoku jest ustawiona na **Razor**. Ustaw **wyświetlanie zawartości** do **listy** a następnie kliknij przycisk **Dodaj**.

![Dodawanie widoku indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Nowy widok automatycznie scaffolds danych użytkownika, który jest przekazywany do `Index` widoku. Sprawdź nowo wygenerowane *Views\Home\Index* pliku. **Utwórz nowy**, **Edytuj**, **szczegóły**, i **Usuń** łącza nie działają, ale działa pozostałej części strony. Uruchom stronę. Zobaczysz listę użytkowników.

![Strona indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Otwórz *Index.cshtml* i Zastęp `ActionLink` kod znaczników dla **Edytuj**, **szczegóły**, i **Usuń** następującym kodem :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Nazwa użytkownika jest używany jako identyfikator można znaleźć wybranego rekordu w **Edytuj**, **szczegóły**, i **Usuń** łącza.

## <a name="creating-the-details-view"></a>Tworzenie widoku szczegółów

Następnym krokiem jest dodanie `Details` metody akcji i widok, aby wyświetlić szczegóły użytkownika.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Dodaj następujący kod `Details` metody do głównego kontrolera:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Kliknij prawym przyciskiem myszy wewnątrz `Details` metody, a następnie wybierz <strong>Dodaj widok</strong>. Upewnij się, że <strong>wyświetlić klasy danych</strong> zawiera pole <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Ustaw <strong>wyświetlanie zawartości</strong> do <strong>szczegóły</strong> a następnie kliknij przycisk <strong>Dodaj</strong>.

![Dodaj widok szczegółów](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Uruchom aplikację, a następnie wybierz łącze szczegółowe informacje. Automatyczne tworzenie szkieletów pokazuje każdej właściwości w modelu.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Tworzenie widoku edycji

Dodaj następujący kod `Edit` metody do głównego kontrolera.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Dodaj widok, tak jak w poprzednich krokach, ale ustawiony **wyświetlanie zawartości** do **Edytuj**.

![Dodawanie widoku edycji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Uruchom aplikację, a następnie edytuj imię i nazwisko nazwę jednego z użytkowników. Jeśli dowolny naruszają `DataAnnotation` ograniczenia, które zostały zastosowane do `UserModel` klasy, gdy prześlesz formularz, zobaczysz błędy sprawdzania poprawności, które są tworzone przez kod serwera. Na przykład, jeśli zmienisz nazwę pierwszego &quot;pods&quot; do &quot;A&quot;, gdy prześlesz formularz, jest wyświetlany następujący błąd, na formularzu:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

W tym samouczku są traktowanie nazwy użytkownika jako klucz podstawowy. W związku z tym nie można zmienić właściwości nazwy użytkownika. W *Edit.cshtml* pliku tuż za `Html.BeginForm` instrukcji, ustaw nazwę użytkownika, jako ukryte pole. Powoduje to właściwości, które zostaną przekazane w modelu. Poniższy fragment kodu przedstawia położenie `Hidden` instrukcji:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Zastąp `TextBoxFor` i `ValidationMessageFor` kodu znaczników dla nazwy użytkownika z `DisplayFor` wywołania. `DisplayFor` Metoda Wyświetla właściwość jako element tylko do odczytu. Poniższy przykład pokazuje ukończone znaczników. Oryginalny `TextBoxFor` i `ValidationMessageFor` wywołania są oznaczone jako komentarz ze znakami Początek komentarza i koniec komentarza Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Włączanie weryfikacji po stronie klienta

Aby włączyć weryfikację po stronie klienta w programie ASP.NET MVC 3, należy ustawić dwie flagi, i musi zawierać trzy pliki JavaScript.

Otwórz aplikację *Web.config* pliku. Sprawdź `that ClientValidationEnabled` i `UnobtrusiveJavaScriptEnabled` są ustawione na wartość true, w ustawieniach aplikacji. Poniższy fragment z katalogu głównego *Web.config* pliku są wyświetlane prawidłowe ustawienia:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Ustawienie `UnobtrusiveJavaScriptEnabled` na wartość true włącza dyskretny kod w technologii Ajax i sprawdzania poprawności dyskretnego kodu klienta. Użycie opcji sprawdzania poprawności dyskretnego kodu, reguł sprawdzania poprawności są przekształcane w atrybuty HTML5. Nazwy atrybutów HTML5 może zawierać tylko małe litery, cyfry i łączniki.

Ustawienie `ClientValidationEnabled` do weryfikacji po stronie klienta umożliwia wartość true. Przez ustawienie tych kluczy w aplikacji *Web.config* pliku, Włącz weryfikację klienckich i dyskretny kod JavaScript dla całej aplikacji. Można również włączyć lub wyłączyć te ustawienia w poszczególnych widokach, lub w metodach kontrolera, używając następującego kodu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Należy również uwzględnić kilka plików JavaScript w renderowanym widoku. Łatwe uwzględnianie języka JavaScript we wszystkich widokach jest, aby dodać je do *Views\Shared\\_Layout.cshtml* pliku. Zastąp `<head>` elementu  *\_Layout.cshtml* pliku następującym kodem:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Pierwsze dwa skrypty jQuery są hostowane przez Microsoft Ajax Content Delivery Network (CDN). Dzięki wykorzystaniu Microsoft Ajax CDN może znacznie poprawić wydajność trafień pierwszej aplikacji.

Uruchom aplikację, a następnie kliknij link Edytuj. Wyświetl źródło strony w przeglądarce. Źródła przeglądarki zawiera wiele atrybutów w formie `data-val` (na potrzeby weryfikacji danych). Po włączeniu weryfikacji klienckich i dyskretny kod JavaScript pól wejściowych przy użyciu reguły weryfikacji klienta zawierają `data-val="true"` atrybutu, aby wyzwolić sprawdzania poprawności dyskretnego kodu klienta. Na przykład `City` pola w modelu została ozdobione [wymagane](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybut, co skutkuje HTML pokazano w poniższym przykładzie:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Dla każdej reguły weryfikacji klienta jest dodawany atrybut zawierający formularz `data-val-rulename="message"`. Za pomocą `City` pola przedstawionym wcześniej przykładzie obejmującym, generuje reguły weryfikacji klienta wymagane `data-val-required` atrybut i komunikat &quot;Miasto pole jest wymagane&quot;. Uruchom aplikację, edytować jeden z użytkowników, a następnie wyczyść `City` pola. Gdy karcie poza pole zobaczysz komunikat o błędzie weryfikacji po stronie klienta.

![Miasto wymagane](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Podobnie, dla każdego parametru reguły weryfikacji klienta jest dodawany atrybut zawierający formularz `data-val-rulename-paramname=paramvalue`. Na przykład `FirstName` właściwość jest oznaczona za pomocą [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu i określa minimalną długość 3 i maksymalna długość 8. Reguły sprawdzania poprawności danych o nazwie `length` ma nazwę parametru `max` i wartość parametru 8. Poniżej pokazano kod HTML, który jest generowany dla `FirstName` pola, gdy edytujesz jednego z użytkowników:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Aby uzyskać więcej informacji na temat sprawdzania poprawności dyskretnego kodu klienta, zobacz wpis [dyskretny kod weryfikacji klienta w programie ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) w Brada Wilsona.

> [!NOTE]
> W programie ASP.NET MVC 3 w wersji Beta czasami musisz przesłać formularza w celu uruchamiania funkcji weryfikacji po stronie klienta. To może ulec zmianie w ostatecznej wersji.


## <a name="creating-the-create-view"></a>Tworzenie widoku Create

Następnym krokiem jest dodanie `Create` metody akcji i widoku w celu umożliwienia użytkownikowi utworzenie nowego użytkownika. Dodaj następujący kod `Create` metody do głównego kontrolera:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Dodaj widok, tak jak w poprzednich krokach, ale ustawiony **wyświetlanie zawartości** do **Utwórz**.

![Utwórz widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Uruchom aplikację, wybierz **Utwórz** łącze i Dodaj nowego użytkownika. `Create` Metoda wykorzystuje automatycznie Walidacja po stronie klienta i po stronie serwera. Wprowadź nazwę użytkownika, który zawiera biały znak, takich jak &quot;Ben X&quot;. Gdy karcie poza pole nazwy użytkownika, błąd weryfikacji po stronie klienta (`White space is not allowed`) jest wyświetlana.

## <a name="add-the-delete-method"></a>Dodaj metodę Delete

Do ukończenia tego samouczka, Dodaj następujący kod `Delete` metody do głównego kontrolera:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Dodaj `Delete` widok, tak jak w poprzednich krokach, ustawienie **wyświetlanie zawartości** do **Usuń**.

![Usuń widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Masz teraz prostą, ale w pełni funkcjonalnych aplikacji ASP.NET MVC 3 ze sprawdzaniem poprawności.
