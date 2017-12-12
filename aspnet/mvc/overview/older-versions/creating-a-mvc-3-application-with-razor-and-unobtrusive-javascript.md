---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Tworzenie MVC 3 aplikacji Razor i dyskretny kod JavaScript | Dokumentacja firmy Microsoft
author: microsoft
description: "Przykładową aplikację sieci web listy użytkowników pokazano, jak łatwo jest tworzenie aplikacji ASP.NET MVC 3, za pomocą aparatu widoku Razor. Przykładowe s aplikacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 68870caf1608e596962650cf653e5b455b82382a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Tworzenie MVC 3 aplikacji Razor i dyskretny kod JavaScript
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Przykładową aplikację sieci web listy użytkowników pokazano, jak łatwo jest tworzenie aplikacji ASP.NET MVC 3, za pomocą aparatu widoku Razor. Przykładowa aplikacja przedstawia sposób użycia nowy aparat widoku Razor z platformą ASP.NET MVC w wersji 3 i Visual Studio 2010 do utworzenia fikcyjnej listy użytkowników witryny sieci Web, która obejmuje funkcje, takie jak tworzenie, wyświetlanie, edytowanie i usuwanie użytkowników.
> 
> W tym samouczku opisano kroki, które zostały pobrane do zbudowania przykładowej aplikacji ASP.NET MVC 3 listy użytkowników. Projektu programu Visual Studio C# i VB kod źródłowy jest dostępny powiązany z tym tematem: [Pobierz](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Jeśli masz pytania dotyczące tego samouczka, opublikuj je, aby [MVC forum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Omówienie

Aplikację, która będzie zbudować jest witryną internetową listy proste użytkownika. Użytkownicy mogą wprowadzać, wyświetlanie i aktualizowanie informacji o użytkowniku.

![Przykład lokacji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Można pobrać projektu zakończone VB i C# [tutaj](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby uruchomić samouczek, Otwórz program Visual Studio 2010 i utworzyć nowy projekt, korzystając *aplikacji sieci Web programu ASP.NET MVC 3* szablonu. Nazwa aplikacji &quot;Mvc3Razor&quot;.

[![Nowy projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

W **nowy projekt programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**wybierz aparatu widoku Razor, a następnie kliknij przycisk **OK**.

![Okno dialogowe nowego projektu programu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

W tym samouczku nie używana będzie dostawcy członkostwa ASP.NET, więc można usunąć wszystkie pliki skojarzone z logowania i członkostwa. W **Eksploratora rozwiązań**, Usuń następujące pliki i katalogi:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (i wszystkie pliki w tym katalogu)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edytuj  *\_Layout.cshtml* plików i Zastąp znaczników wewnątrz `<div>` elementu o nazwie `logindisplay` z komunikatem  *&quot;* wyłączone logowania&quot;. W poniższym przykładzie przedstawiono nowy kod znaczników:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Dodawanie modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie kliknij przycisk **klasy**.

![Nowa klasa Mdl użytkownika](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nazwa klasy `UserModel`. Zastąp zawartość *UserModel* pliku następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Klasa reprezentuje użytkowników. Każdy element członkowski klasy jest oznaczony za pomocą [wymagane](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutu z [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw. Atrybuty w [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw udostępnienia weryfikacji automatyczne klienta — i po stronie serwera dla aplikacji sieci web.

Otwórz `HomeController` klasy i Dodaj `using` dyrektywy, dzięki czemu można uzyskać dostępu do `UserModel` i `Users` klasy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Tuż po `HomeController` deklaracji, Dodaj poniższy komentarz i odwołanie do `Users` klasy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Klasy jest magazynem danych uproszczoną, w pamięci, które będzie używane w tym samouczku. W rzeczywistej aplikacji należy użyć bazy danych, aby przechowywać informacje użytkownika. W pierwszym wierszu kilka `HomeController` w poniższym przykładzie przedstawiono plik:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Utworzenie aplikacji, dzięki czemu modelu użytkownika będzie szkieletów kreatora w następnym kroku.

## <a name="creating-the-default-view"></a>Tworzenie widok domyślny

Następnym krokiem jest, aby dodać metody akcji i widok, aby wyświetlić użytkowników.

Usuń istniejącą *Views\Home\Index* pliku. Utworzy nowy *indeksu* plik, aby wyświetlić użytkowników.

W `HomeController` klasy, Zastąp zawartość `Index` metodę z następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Kliknij prawym przyciskiem myszy wewnątrz `Index` metody, a następnie kliknij przycisk **Dodaj widok**.

![Dodaj widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wybierz **utworzyć widok jednoznacznie** opcji. Aby uzyskać **wyświetlić klasy danych**, wybierz pozycję **Mvc3Razor.Models.UserModel**. (Jeśli nie widzisz **Mvc3Razor.Models.UserModel** w **wyświetlić klasy danych** pole, należy skompilować projekt.) Upewnij się, że aparat widoku jest ustawiona na **Razor**. Ustaw **wyświetlania zawartości** do **listy** , a następnie kliknij przycisk **Dodaj**.

![Dodaj widok indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Nowy widok automatycznie scaffolds danych użytkownika, który jest przekazywany do `Index` widoku. Sprawdź nowo utworzonego *Views\Home\Index* pliku. **Utwórz nowy**, **Edytuj**, **szczegóły**, i **usunąć** łącza nie działają, ale pozostałe strony będzie działać. Uruchom strony. Zostanie wyświetlona lista użytkowników.

![Strona indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Otwórz *Index.cshtml* plików i Zastąp `ActionLink` kod znaczników dla **Edytuj**, **szczegóły**, i **usunąć** następującym kodem :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Nazwa użytkownika jest używana jako identyfikator można znaleźć wybranego rekordu w **Edytuj**, **szczegóły**, i **usunąć** łącza.

## <a name="creating-the-details-view"></a>Tworzenie widoku szczegółów

Następnym krokiem jest dodanie `Details` metody akcji i widok, aby wyświetlić szczegóły użytkownika.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Dodaj następujące `Details` metody kontrolerowi macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Kliknij prawym przyciskiem myszy wewnątrz `Details` metody, a następnie wybierz **Dodaj widok**. Sprawdź, czy **wyświetlić klasy danych** zawiera pole **Mvc3Razor.Models.UserModel***.* Ustaw **wyświetlania zawartości** do **szczegóły** , a następnie kliknij przycisk **Dodaj**.

![Dodaj widok szczegółów](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Uruchom aplikację, a następnie wybierz łącze Szczegóły. Automatyczne szkieletów pokazuje każdej właściwości w modelu.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Tworzenie widoku edycji

Dodaj następujące `Edit` metody do macierzystego kontrolera.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Dodaj widok, jak w poprzednich krokach, ale ustawiona **wyświetlania zawartości** do **Edytuj**.

![Dodaj widok edycji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Uruchom aplikację i edytowanie imię i nazwisko jednego użytkowników. Jeśli użytkownik narusza `DataAnnotation` ograniczenia, które zostały zastosowane do `UserModel` klasy, po przesłaniu formularza, zostanie wyświetlone błędy sprawdzania poprawności, które są tworzone przez kod serwera. Na przykład, jeśli zmienisz nazwę pierwszego &quot;pods&quot; do &quot;A&quot;po przesłaniu formularza na formularzu jest wyświetlany następujący błąd:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

W tym samouczku nazwa użytkownika jest traktowanie jako klucz podstawowy. W związku z tym nie można zmienić właściwości nazwy użytkownika. W *Edit.cshtml* pliku tuż po `Html.BeginForm` ustawić instrukcji, nazwa użytkownika, który ma zostać ukryte pole. Powoduje to właściwości, które mają być przekazane w modelu. Poniższy fragment kodu przedstawia położenie `Hidden` instrukcji:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Zastąp `TextBoxFor` i `ValidationMessageFor` kodu znaczników dla nazwy użytkownika z `DisplayFor` wywołania. `DisplayFor` Metoda Wyświetla właściwość jako element tylko do odczytu. W poniższym przykładzie przedstawiono ukończone znaczników. Oryginalna `TextBoxFor` i `ValidationMessageFor` wywołania są oznaczone jako komentarz ze znakami komentarz rozpoczęcia i zakończenia komentarza Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Włączenie weryfikacji po stronie klienta

Aby włączyć weryfikację po stronie klienta w programie ASP.NET MVC 3, należy ustawić dwie flagi i musi zawierać trzy pliki JavaScript.

Otwórz aplikację *Web.config* pliku. Sprawdź `that ClientValidationEnabled` i `UnobtrusiveJavaScriptEnabled` są ustawione na wartość true w ustawieniach aplikacji. Poniższy fragment z katalogu głównego *Web.config* plików są wyświetlane poprawne ustawienia:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Ustawienie `UnobtrusiveJavaScriptEnabled` na wartość true włącza dyskretny kod technologii Ajax i sprawdzania poprawności dyskretnego kodu klienta. Gdy używasz sprawdzania poprawności dyskretnego kodu reguły sprawdzania poprawności są przekształcane w atrybuty HTML5. Nazwy atrybutów HTML5 może zawierać tylko małe litery, cyfry i łączniki.

Ustawienie `ClientValidationEnabled` na wartość true włącza weryfikacji po stronie klienta. Przez ustawienie tych kluczy w aplikacji *Web.config* pliku, Włącz sprawdzanie poprawności klienta i dyskretny kod JavaScript dla całej aplikacji. Można również włączyć lub wyłączyć te ustawienia w poszczególnych widoków lub metod kontrolera, używając następującego kodu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Należy również uwzględnić w widoku renderowanym kilka plików JavaScript. Prosty sposób obejmują JavaScript we wszystkich widokach jest dodanie ich do *Views\Shared\\_Layout.cshtml* pliku. Zastąp `<head>` elementu  *\_Layout.cshtml* pliku następującym kodem:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Pierwsze dwa skrypty jQuery są obsługiwane przez Microsoft Ajax sieci dostarczania zawartości (CDN). Korzystając z usługi Microsoft Ajax CDN, może znacznie poprawić wydajność trafień pierwszej aplikacji.

Uruchom aplikację, a następnie kliknij łącze edycji. Wyświetl źródło strony w przeglądarce. Źródła przeglądarki zawiera wiele atrybutów w postaci `data-val` (do sprawdzania poprawności danych). Gdy włączone jest sprawdzanie poprawności klienta i dyskretny kod JavaScript, zawierać pól wejściowych przy użyciu reguły weryfikacji klienta `data-val="true"` atrybut do wyzwolenia sprawdzania poprawności dyskretnego kodu klienta. Na przykład `City` pola w modelu zostało oznaczone [wymagane](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybut, który powoduje HTML pokazano w poniższym przykładzie:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Dla każdej reguły weryfikacji klienta dodaje się atrybut, który ma postać `data-val-rulename="message"`. Przy użyciu `City` pola przykładzie wcześniej, generuje reguły weryfikacji klienta wymagane `data-val-required` atrybut i komunikat &quot;Miasto pole jest wymagane&quot;. Uruchom aplikację, edytować jeden z użytkowników, a następnie wyczyść `City` pola. Gdy karcie poza pole zobaczysz komunikat o błędzie weryfikacji po stronie klienta.

![Miasto wymagane](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Podobnie, dla każdego parametru reguły weryfikacji klienta jest dodawany atrybut mający formularza `data-val-rulename-paramname=paramvalue`. Na przykład `FirstName` właściwość jest oznaczona przy [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu i określa minimalną długość wynosi 3 i maksymalnie 8. Reguły sprawdzania poprawności danych o nazwie `length` ma nazwę parametru `max` i wartość parametru 8. Oto kod HTML, który zostanie wygenerowany dla `FirstName` podczas edycji jednego użytkowników:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Aby uzyskać więcej informacji na temat sprawdzania poprawności dyskretnego kodu klienta, zobacz wpis [dyskretny kod weryfikacji klienta w programie ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) w blogu Brada Wilsona.

> [!NOTE]
> W programie ASP.NET MVC 3 w wersji Beta czasami trzeba przesłać formularza, aby rozpocząć weryfikację po stronie klienta. To może ulec zmianie w ostatecznej wersji.


## <a name="creating-the-create-view"></a>Tworzenie widoku Create

Następnym krokiem jest dodanie `Create` metody akcji i widoku w celu umożliwienia użytkownikowi utworzenie nowego użytkownika. Dodaj następujące `Create` metody kontrolerowi macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Dodaj widok, jak w poprzednich krokach, ale ustawiona **wyświetlania zawartości** do **Utwórz**.

![Tworzenie widoku](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Uruchom aplikację, wybierz **Utwórz** łącza, a następnie dodaj nowego użytkownika. `Create` — Metoda korzysta automatycznie weryfikacji po stronie klienta i po stronie serwera. Spróbuj wprowadzić nazwę użytkownika, który zawiera biały znak, takie jak &quot;Ben X&quot;. Kiedy karta poza pole nazwy użytkownika, błąd weryfikacji po stronie klienta (`White space is not allowed`) jest wyświetlany.

## <a name="add-the-delete-method"></a>Dodaj metodę Delete

Aby ukończyć samouczek, Dodaj następujący `Delete` metody kontrolerowi macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Dodaj `Delete` widoku, jak w poprzednich krokach, ustawienie **wyświetlania zawartości** do **usunąć**.

![Usuń widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Masz teraz prostą, ale funkcjonalnej aplikacji ASP.NET MVC 3 z weryfikacji.
