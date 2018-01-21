---
title: "Pomocników tagów w platformy ASP.NET Core"
author: rick-anderson
description: "Poznaj pomocników tagów są i sposobu ich używania w ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 003a22d4b0d9400f3e9effe0892d2d7e03704cde
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>Wprowadzenie do pomocników tagów w platformy ASP.NET Core 

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Co to są pomocników tagów?

Pomocników tagów włączyć kod po stronie serwera do tworzenia i renderowania elementów HTML w plikach Razor. Na przykład wbudowana `ImageTagHelper` dołączyć do nazwy obrazu numeru wersji. Przy każdej zmianie obrazu serwera generuje nową wersję unikatowy dla obrazu, więc klienci dotrą do pobrania bieżącego obrazu (zamiast starych obrazu pamięci podręcznej). Istnieje wiele wbudowanych pomocników tagów do wykonywania typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakietów więcej — i jeszcze bardziej dostępne w publicznych repozytoriach usługi GitHub i NuGet. Pomocników tagów są tworzone w języku C# i ich elementami docelowymi na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym elementów HTML. Na przykład wbudowana `LabelTagHelper` celem może być HTML `<label>` elementu podczas `LabelTagHelper` atrybutów są stosowane. Jeśli znasz [pomocników HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), pomocników tagów zmniejszyć jawne przejścia między HTML i C# w widokach Razor. W wielu przypadkach pomocników HTML Podaj informacje o innym podejściu do określonych pomocniczego znacznika, ale ważne jest, aby rozpoznać pomocników tagów nie zastępują pomocników HTML, a nie ma tagu pomocnika dla każdego pomocnika kodu HTML. [W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśniono różnice bardziej szczegółowo.

## <a name="what-tag-helpers-provide"></a>Podaj pomocników tagów

**Środowisko programistyczne HTML przyjaznego** najbardziej części znaczników Razor przy użyciu pomocników tagów wygląda jak standardowy HTML. Projektanci frontonu conversant HTML/CSS/JavaScript można edytować Razor bez uzyskiwania składni Razor C#.

**Bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor** jest natomiast sharp do pomocników HTML, poprzednie podejście do tworzenia znaczników w widokach Razor po stronie serwera. [W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśniono różnice bardziej szczegółowo. [Obsługę funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) opisano środowisko IntelliSense. Deweloperzy nawet ze składni Razor C# są bardziej wydajnej pracy przy użyciu pomocników tagów niż pisanie kodu C# Razor znaczników.

**Można dokonać bardziej wydajną i może wygenerować bardziej niezawodne, niezawodne i kodu za pomocą informacje są dostępne tylko na serwerze** na przykład w przeszłości mantra na aktualizowanie obrazów była zmiana nazwy obrazu po zmianie obraz. Obrazy mają być buforowane agresywnie ze względu na wydajność, a jeśli nie zmienisz nazwę obrazu ryzyka klientom pobieranie starych kopii. W przeszłości po obraz był edytowany, nazwa ma zostać zmienione, a każde odwołanie do obrazu w aplikacji sieci web nie trzeba aktualizować. Nie tylko jest to bardzo pracę znacznym, jest również podatna (można pominąć odwołanie, przypadkowo wprowadź nieprawidłowy ciąg znaków, itp.) Wbudowane `ImageTagHelper` można wykonać tej operacji automatycznie. `ImageTagHelper` Dołączyć wersję numer do nazwy obrazu, więc przy każdej zmianie obrazu serwera automatycznie generuje nową wersję unikatowy dla obrazu. Klienci dotrą do pobrania bieżącego obrazu. Ten oszczędności niezawodności i instalacyjne pochodzą zasadniczo wolnego przy użyciu `ImageTagHelper`.

Większość wbudowanych pomocników tagów target istniejących elementów HTML i udostępniania atrybutów po stronie serwera dla elementu. Na przykład `<input>` element używany w wielu widoków *widoków/konta* folder zawiera `asp-for` atrybut, który wyodrębnia nazwa właściwości określonego modelu do renderowanego kodu HTML. Następujący kod Razor:

```cshtml
<label asp-for="Email"></label>
```

Generuje poniższy kod HTML:

```cshtml
<label for="Email">Email</label>
```

`asp-for` Atrybutu jest udostępniana przez `For` właściwości w `LabelTagHelper`. Zobacz [tworzenia pomocników tagów](authoring.md) Aby uzyskać więcej informacji.

## <a name="managing-tag-helper-scope"></a>Zarządzanie zakresem pomocnika tagów

Zakres pomocników tagów jest kontrolowany przy użyciu kombinacji `@addTagHelper`, `@removeTagHelper`i "!" Wypisz znaków.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`udostępnia pomocników tagów

Jeśli tworzenie nowej aplikacji sieci web platformy ASP.NET Core o nazwie *AuthoringTagHelpers* (z bez uwierzytelniania), następujące *Views/_ViewImports.cshtml* plik zostanie dodany do projektu:

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` Dyrektywy udostępnia pomocników tagów do widoku. W tym przypadku jest plik widoku *Views/_ViewImports.cshtml*, która domyślnie jest dziedziczona przez wszystkie pliki widoku w *widoków* folder i podkatalogów; udostępnianie pomocników tagów. Powyższy kod używa składni symbol wieloznaczny ("\*"), aby określić, że pomocników tagów w określonym zestawie (*Microsoft.AspNetCore.Mvc.TagHelpers*) będą dostępne dla każdego pliku widoku w *widoków* katalogu lub podkatalogu. Pierwszy parametr po `@addTagHelper` określa pomocników tagów, aby załadować (używamy "\*" dla pomocników tagów), a drugi parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Określa zestaw zawierający pomocników tagów. *Microsoft.AspNetCore.Mvc.TagHelpers* jest zestawu dla wbudowanych pomocników tagów Core ASP.NET.

Aby udostępnić wszystkie pomocników tagów w tym projekcie (co powoduje zestawu o nazwie *AuthoringTagHelpers*), należy użyć następujących:

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Jeśli projekt zawiera `EmailTagHelper` z domyślnej przestrzeni nazw (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), można podać w pełni kwalifikowana nazwa (FQN) pomocnika tagów:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie nazwy zestawu (*AuthoringTagHelpers*). Większość deweloperów wolą używać "\*" znaków wieloznacznych. Składnia symbol wieloznaczny umożliwia Wstaw symbol wieloznaczny "\*" jako sufiks w FQN. Na przykład następujące dyrektywy zostanie wyświetlone `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Jak wspomniano wcześniej, dodawanie `@addTagHelper` dyrektywy do *Views/_ViewImports.cshtml* pliku udostępnia pomocnika tagów do wszystkich plików widoku w *widoków* katalogu i podkatalogów. Można użyć `@addTagHelper` dyrektywy w plikach określony widok, aby wyrazić zgodę na udostępnianie pomocnika tagów tylko tych widoków.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Usuwa pomocników tagów

`@removeTagHelper` Ma takie same parametry dwóch jako `@addTagHelper`, i usuwa pomocnika Tag, które zostały wcześniej dodane. Na przykład `@removeTagHelper` stosowane do określonego widoku powoduje usunięcie określonego pomocnika tagów z widoku. Przy użyciu `@removeTagHelper` w *Views/Folder/_ViewImports.cshtml* pliku usuwa określonego pomocnika Tag ze wszystkich widoków *folderu*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Kontrolowanie zakresu pomocnika tagów z *_ViewImports.cshtml* pliku

Możesz dodać *_ViewImports.cshtml* do dowolnego folderu widoku oraz widoku aparat stosuje dyrektywy z obu tego pliku i *Views/_ViewImports.cshtml* pliku. Jeśli dodano pustą *Views/Home/_ViewImports.cshtml* plik *Home* widoków, nie może istnieć bez zmian, ponieważ *_ViewImports.cshtml* plik jest dodatek. Wszelkie `@addTagHelper` dyrektywy dodać do *Views/Home/_ViewImports.cshtml* pliku (które nie są w domyślnym *Views/_ViewImports.cshtml* pliku) wystawi tych pomocników tagów do widoków tylko w *Home* folderu.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Rezygnacja z poszczególne elementy

Możesz wyłączyć pomocnika tagów znakiem Wypisz pomocnika tagów na poziomie elementu ("!"). Na przykład `Email` Weryfikacja jest wyłączona w `<span>` znakiem Wypisz pomocnika tagów:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Należy zastosować znak Wypisz pomocnika tagów do otwarcia i zamknięcia tagu. (Edytorze programu Visual Studio automatycznie dodaje znak rezygnacji tag zamykający podczas dodawania do tagu otwierającego). Po dodaniu znak rezygnacji z elementem i atrybuty pomocnika tagów nie są już wyświetlani charakterystyczne czcionki.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Przy użyciu `@tagHelperPrefix` dokonanie jawnego użycia Pomocnika tagów

`@tagHelperPrefix` Dyrektywy pozwala na określenie Ciąg prefiksu tagu, aby włączyć obsługę pomocnika tagów i użycia Pomocnika tagów jawnego. Na przykład można dodać następujących znaczników *Views/_ViewImports.cshtml* pliku:

```cshtml
@tagHelperPrefix th:
```
Na poniższej ilustracji kodu ustawiono prefiks tagu Pomocnika `th:`, dlatego tylko tych elementów przy użyciu prefiksu `th:` obsługuje pomocników tagów (włączone pomocnika tagów elementy mają charakterystyczne czcionki). `<label>` i `<input>` elementy mają prefiks tagu pomocnika i obsługują pomocnika tagów, podczas `<span>` nie ma elementu.

![obraz](intro/_static/thp.png)

Tym samym zasady hierarchii, które dotyczą `@addTagHelper` dotyczą również `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Obsługę funkcji IntelliSense dla pomocników tagów

Podczas tworzenia nowej aplikacji sieci web ASP.NET w programie Visual Studio, dodaje pakiet NuGet "Microsoft.AspNetCore.Razor.Tools". To jest pakiet, który dodaje narzędzia pomocnika tagów.

Należy wziąć pod uwagę pisania kodu HTML `<label>` elementu. Jak wprowadzasz `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:

![obraz](intro/_static/label.png)

Nie tylko możesz uzyskasz pomocy HTML, ale ikony ("@" symbol with "<>" poniżej).

![obraz](intro/_static/tagSym.png)

identyfikuje element zgodnie z celem pomocników tagów. Czysty elementów HTML (takie jak `fieldset`) wyświetlana ikona "<>".

Czystym kodem HTML `<label>` tagów wyświetla tagu HTML (za pomocą domyślnego motywu kolorów Visual Studio) czcionką brązowy atrybutów na czerwono, i atrybutu wartości w kolorze niebieskim.

![obraz](intro/_static/LableHtmlTag.png)

Po wprowadzeniu `<label`, IntelliSense wyświetla dostępne atrybuty HTML/CSS oraz atrybutów docelowe pomocnika tagów:

![obraz](intro/_static/labelattr.png)

Instrukcji IntelliSense pozwala na wprowadzenie klawisz tab, aby wykonać instrukcję, określając wybraną wartość:

![obraz](intro/_static/stmtcomplete.png)

Zaraz po wprowadzeniu atrybut pomocnika tagów zmiana czcionki tagów i atrybutów. Przy użyciu domyślnej programu Visual Studio "Blue" lub "Jasny" Kolor motywu, czcionka jest pogrubiona purpurowy. Jeśli używasz motywu "Ciemny" czcionka jest pogrubiona jest. Obrazy w tym dokumencie zostały pobrane przy użyciu domyślnego motywu.

![obraz](intro/_static/labelaspfor2.png)

Visual Studio można wprowadzić *CompleteWord* skrótów (Ctrl + spacja jest [domyślne](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) wewnątrz podwójnych cudzysłowów (""), i wszystko jest teraz w języku C#, tak jak będzie w klasie C#. IntelliSense wyświetla wszystkie metody i właściwości w modelu strony. Metody i właściwości są dostępne, ponieważ typ właściwości jest `ModelExpression`. Na poniższej ilustracji, 'M I edytowanie `Register` widoku, więc `RegisterViewModel` jest dostępna.

![obraz](intro/_static/intellemail.png)

Funkcja IntelliSense wyświetla listę właściwości i metod modelu na stronie. Bogate środowisko funkcji IntelliSense pomaga wybrać klasę CSS:

![obraz](intro/_static/iclass.png)

![obraz](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>W porównaniu do pomocników HTML pomocników tagów

Dołącz pomocników tagów do elementów HTML w widokach Razor podczas [pomocników HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) są wywoływane jako metody oraz HTML w widoku Razor. Należy wziąć pod uwagę następujące znaczników Razor, który tworzy element label kodu HTML z klasy CSS "podpis":

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

W (`@`) informuje symbolu Razor jest to początek kodu. Następne dwa parametry ("Imię" i "imię:") są ciągami, więc [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) nie może pomóc. Ostatni argument:

```cshtml
new {@class="caption"}
```

Obiektu anonimowego jest używana do reprezentowania atrybutów. Ponieważ **klasy** jest zarezerwowanym słowem kluczowym języka C#, użyj `@` symbolu, aby wymusić C#, aby zinterpretować "@class=" jako symbolu (nazwa właściwości). Do projektanta frontonu (ktoś zapoznać się z kodu HTML/CSS/JavaScript i innych technologii klienta, ale nie znasz języka C# i Razor), większość wiersza jest obca. Cały wiersz musi być utworzony za pomocą pomoc od IntelliSense.

Przy użyciu `LabelTagHelper`, można zapisać znacznika tej samej:

![obraz](intro/_static/label2.png)

Z wersją pomocnika tagów, jak wprowadzasz `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:

![obraz](intro/_static/label.png)

IntelliSense ułatwia pisanie cały wiersz. `LabelTagHelper` Domyślnie ustawienie zawartość `asp-for` atrybutu "Imię"; wartość ("imię") Konwertuje liter formatu właściwości do zdania składa się z nazwy właściwości z miejsca, w którym występuje każdego nowego wielką literą. W znaczniku następujące:

![obraz](intro/_static/label2.png)

generuje:

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

Formatu — z uwzględnieniem wielkości liter liter zdanie zawartości nie jest używany, jeśli Dodawanie zawartości do `<label>`. Na przykład:

![obraz](intro/_static/1stName.png)

generuje:

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

Na poniższej ilustracji kodu przedstawiono formularz część *Views/Account/Register.cshtml* widoku Razor wygenerowane z szablonu MVC starszych 4.5.x ASP.NET uwzględnionych w programie Visual Studio 2015.

![obraz](intro/_static/regCS.png)

Edytor programu Visual Studio Wyświetla kodu C# za pomocą szare tła. Na przykład `AntiForgeryToken` pomocnika kodu HTML:

```cshtml
@Html.AntiForgeryToken()
```

zostanie wyświetlony z szare tła. Większość znaczników w widoku rejestru jest C#. Porównać równoważne podejścia, przy użyciu pomocników tagów:

![obraz](intro/_static/regTH.png)

Kod znaczników jest znacznie bardziej przejrzysty i łatwiejsze do odczytu, edytowanie i obsługa niż metody pomocników HTML. Kod C# jest ograniczone do minimum, który serwer musi znać. W edytorze programu Visual Studio są wyświetlane znaczników objęci pomocnika tagów charakterystyczne czcionki.

Należy wziąć pod uwagę *E-mail* grupy:

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

Każdego z atrybutów "asp —" ma wartość "E-mail", ale "E-mail" nie jest ciągiem. W tym kontekście "E-mail" jest C# wyrażenie właściwości modelu dla `RegisterViewModel`.

Edytor programu Visual Studio ułatwia pisanie **wszystkie** z kodu znaczników w ujęciu pomocnika Tag formularza rejestru, podczas gdy program Visual Studio udostępnia pomoc dla większości kodu w ujęciu pomocników HTML. [Obsługę funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) przechodzi do szczegółów na temat pracy z pomocników tagów w edytorze programu Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Pomocników tagów w porównaniu do kontrolki serwera sieci Web

* Pomocników tagów nie należy do użytkownika element, do którego są one związane z; po prostu uczestniczą w czasie renderowania elementów i zawartości. ASP.NET [kontrolki serwera sieci Web](https://msdn.microsoft.com/library/7698y1f0.aspx) zadeklarowany i wywoływane na stronie.

* [Formanty serwera sieci Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) mają cykl nieuproszczony może utrudnić opracowywania i debugowania.

* Formanty serwera sieci Web umożliwiają dodawanie funkcji do elementów modelu DOM (Document Object) klienta za pomocą formantu klienta. Pomocników tagów nie ma żadnych modelu DOM.

* Formanty serwera sieci Web obejmują wykrywanie automatyczne przeglądarki. Pomocników tagów nie korzystają z nie przeglądarki.

* Wiele pomocników tagów może działać na tym samym elemencie (zobacz [pomocnika tagów unikanie konfliktów](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) podczas zwykle nie można utworzyć kontrolki serwera sieci Web.

* Pomocników tagów można tagu i zawartość elementów HTML, które jest zakresem, ale nie bezpośrednio modyfikować dowolne inne na stronie. Formanty serwera sieci Web mają szerszym zakresie i mogą wykonywać akcje, które mają wpływ na inne części strony; Włączanie niezamierzone skutki uboczne.

* Formanty serwera sieci Web umożliwia konwertowanie ciągów na obiekty konwertery typu. Z pomocników tagów pracy natywnie w języku C#, dzięki czemu nie trzeba konwersja typu.

* Serwer sieci Web steruje użyciem [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) do zaimplementowania zachowania czasu wykonywania i czasu projektowania, składników i kontrolek. `System.ComponentModel`zawiera klasy podstawowe i interfejsy dla wykonania atrybutów i typy konwerterów, powiązanie z danymi źródeł i licencjonowania składników. Natomiast który do pomocników tagów, które zwykle pochodzi od `TagHelper`i `TagHelper` klasy podstawowej przedstawia tylko dwie metody `Process` i `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Dostosowywanie czcionki element pomocnika tagów

Możesz dostosować czcionki i kolorowania z **narzędzia** > **opcje** > **środowiska** > **czcionek Kolory i**:

![obraz](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Tworzenie pomocników tagów](authoring.md)
* [Praca z formularzy](../working-with-forms.md)
* [TagHelperSamples w serwisie GitHub](https://github.com/dpaquette/TagHelperSamples) zawiera przykłady pomocnika tagów do pracy z [Bootstrap](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
