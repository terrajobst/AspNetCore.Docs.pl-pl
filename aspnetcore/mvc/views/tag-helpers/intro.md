---
title: Pomocnicy tagów w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, czym są pomocnicy tagów i sposobu ich używania w programie ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 870ce2eb28f384b380cc1178842325dc28199f09
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814992"
---
# <a name="tag-helpers-in-aspnet-core"></a>Pomocnicy tagów w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Co to są pomocnicy tagów

Pomocnicy tagów włączyć kodu po stronie serwera wziąć udział w tworzeniu i renderowaniu elementów HTML w plikach Razor. Na przykład wbudowane `ImageTagHelper` można dołączyć numer wersji do nazwy obrazu. Zmianie obrazu serwera generuje unikatowy nową wersję obrazu, dzięki czemu klienci są gwarantowane można pobrać bieżącego obrazu (zamiast przestarzałych obraz pamięci podręcznej). Istnieje wiele wbudowanych pomocników tagów dla typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakiety więcej — i jeszcze bardziej dostępne w publicznych repozytoriach GitHub oraz jak NuGet. Pomocnicy tagów są tworzone w języku C#, a ich celem są elementy HTML na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym. Na przykład wbudowane `LabelTagHelper` mogą kierować HTML `<label>` elementu po `LabelTagHelper` atrybuty są stosowane. Jeśli znasz [pomocników HTML](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), pomocników tagów zmniejszyć jawne przejścia pomiędzy HTML a C# w widokami Razor. W wielu przypadkach pomocników HTML zapewnić alternatywne podejście do określonych Pomocnik tagu, ale ważne jest, aby rozpoznać, czy pomocników tagów nie zastąpić pomocników HTML i nie jest pomocnika tagów dla każdego pomocnika kodu HTML. [W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśnia różnice bardziej szczegółowo.

## <a name="what-tag-helpers-provide"></a>Podaj pomocnicy tagów

**Środowisko programistyczne przyjaznego dla HTML** wygląda w przeważającej części HTML standardowych znaczników Razor za pomocą pomocników tagów. Projektanci frontonu conversant z HTML/CSS/JavaScript edytować Razor bez uczenia składnia Razor języka C#.

**Bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor** to sharp natomiast aby pomocników HTML, poprzednie podejście do tworzenia po stronie serwera, z kodu znaczników w widokami Razor. [W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśnia różnice bardziej szczegółowo. [Obsługa funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) wyjaśnia środowiska funkcji IntelliSense. Nawet deweloperzy używający składni Razor C# są bardziej produktywne, używanie pomocników tagów niż pisanie znaczników języka C# Razor.

**Sposobem, aby zapewnić bardziej efektywne i może wygenerować bardziej niezawodne, niezawodne i kodu łatwego w utrzymaniu, korzystając z informacji dostępna tylko na serwerze** na przykład w przeszłości mantra na temat aktualizowania obrazów była zmiana nazwy obrazu po zmianie obraz. Obrazy, które powinny być agresywnie buforowane ze względu na wydajność i chyba że zmieniasz nazwę obrazu, istnieje ryzyko klientom pobieranie starych kopii. W przeszłości po obraz był edytowany, nazwa ma zostać zmieniony, a każde odwołanie do obrazu w aplikacji sieci web nie trzeba aktualizować. Nie tylko jest to bardzo pracy o znacznym wykorzystaniu, jest również podatne (można pominąć odwołanie, przypadkowo wprowadzić nieprawidłowy ciąg znaków, itp.) Wbudowane `ImageTagHelper` można to zrobić dla Ciebie automatycznie. `ImageTagHelper` Można dołączyć wersji liczb do nazwy obrazu dzięki zmianie obrazu serwera automatycznie generuje unikatowy nową wersję obrazu. Klienci są gwarantowane można pobrać bieżącego obrazu. Ta oszczędności niezawodności i pracy dostępny zasadniczo bezpłatnie przy użyciu `ImageTagHelper`.

Pomocnicy tagów największą liczbą wbudowanych docelową standardowych elementów kodu HTML i udostępniania atrybutów po stronie serwera dla elementu. Na przykład `<input>` element używany w wielu widoków w *widoków/konto* folder zawiera `asp-for` atrybutu. Ten atrybut wyodrębnia nazwę właściwości określonego modelu w postaci HTML. Należy wziąć pod uwagę widoku Razor z modelem następujące:

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

Następujący kod Razor:

```cshtml
<label asp-for="Movie.Title"></label>
```

Generuje poniższy kod HTML:

```html
<label for="Movie_Title">Title</label>
```

`asp-for` Atrybutu jest udostępniana przez `For` właściwość [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0). Zobacz [pomocników tagów Autor](xref:mvc/views/tag-helpers/authoring) Aby uzyskać więcej informacji.

## <a name="managing-tag-helper-scope"></a>Pomocnik tagu zakresu zarządzania

Zakres pomocników tagów jest kontrolowany przy użyciu kombinacji `@addTagHelper`, `@removeTagHelper`i "!" znak zrezygnować.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` udostępnia pomocnicy tagów

Jeśli utworzysz nową aplikację sieci web platformy ASP.NET Core o nazwie *AuthoringTagHelpers*, następujące *Views/_ViewImports.cshtml* plik zostanie dodany do projektu:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` Dyrektywy udostępnia pomocników tagów w widoku. W tym przypadku jest plik widoku *Pages/_ViewImports.cshtml*, która domyślnie jest dziedziczona przez wszystkie pliki w *stron* folderze i jego podfolderach; pomocników tagów udostępnianie. Powyższy kod używa składni symboli wieloznacznych ("\*") można określić, że wszystkie pomocników tagów w określonym zestawie (*Microsoft.AspNetCore.Mvc.TagHelpers*) będą dostępne dla wszystkich plików widoku w *widoków* katalog lub podkatalog. Pierwszy parametr po `@addTagHelper` określa pomocnicy tagów, aby załadować (używamy "\*" dla pomocników tagów), a drugi parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Określa zestaw zawierający pomocników tagów. *Microsoft.AspNetCore.Mvc.TagHelpers* jest zestawem dla wbudowanych pomocników tagów dla platformy ASP.NET Core.

Aby udostępnić wszystkie pomocników tagów w tym projekcie (tworzy zestaw o nazwie *AuthoringTagHelpers*), należy użyć następujących czynności:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Jeśli projekt zawiera `EmailTagHelper` przy użyciu domyślnej przestrzeni nazw (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), możesz podać w pełni kwalifikowaną nazwę (FQN) pomocnika tagów:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie nazwę zestawu (*AuthoringTagHelpers*). Większość programistów wolą używać "\*" Składnia symboli wieloznacznych. Składnia symboli wieloznacznych pozwala wstawić symbol wieloznaczny "\*" jako sufiks w FQN. Na przykład, następujące dyrektywy zostanie wyświetlone `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Jak wspomniano wcześniej, dodając `@addTagHelper` dyrektywę *Views/_ViewImports.cshtml* pliku sprawia, że Pomocnik tagu jest dostępny dla wszystkich plików w widoku w *widoków* katalogu i podkatalogach. Możesz użyć `@addTagHelper` dyrektywy w plikach określonego widoku, aby wyrazić zgodę na udostępnianie Pomocnik tagu tylko tych widoków.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` Usuwa pomocnicy tagów

`@removeTagHelper` Ma ten sam dwa parametry jako `@addTagHelper`, i usuwa pomocnika tagów, który wcześniej został dodany. Na przykład `@removeTagHelper` stosowane w przypadku określonego widoku powoduje usunięcie określonego pomocnika tagów z widoku. Za pomocą `@removeTagHelper` w *Views/Folder/_ViewImports.cshtml* pliku usuwa określony Pomocnik tagu ze wszystkich widoków w *folderu*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Kontrolowanie zakresu Pomocnik tagu za pomocą *_ViewImports.cshtml* pliku

Możesz dodać *_ViewImports.cshtml* dowolny folder widoku oraz widoku aparatu dotyczy dyrektywy z obu tego pliku i *Views/_ViewImports.cshtml* pliku. Jeśli została dodana pusta *Views/Home/_ViewImports.cshtml* plik *Home* widoków, może być brak zmian *_ViewImports.cshtml* plik jest dodatku. Żadnych `@addTagHelper` dyrektywy możesz dodać do *Views/Home/_ViewImports.cshtml* pliku (które nie są w domyślnym *Views/_ViewImports.cshtml* pliku) wystawi tych pomocników tagów do widoków tylko w *Home* folderu.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Rezygnacja z poszczególnych elementów

Można wyłączyć pomocnika tagów znakiem rezygnacji Pomocnik tagu na poziomie elementu ("!"). Na przykład `Email` sprawdzania poprawności jest wyłączona w `<span>` znakiem rezygnacji Pomocnik tagu:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Pomocnik tagu znak rezygnacji muszą dotyczyć otwierający i zamykający tag. (Edytor programu Visual Studio automatycznie dodaje znak rezygnacji z tagu zamykającego po dodaniu do otwierający tag). Po dodaniu znak rezygnacji, elementów i atrybutów Pomocnik tagu już nie są wyświetlane czcionką szczególne.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Za pomocą `@tagHelperPrefix` się jawne użycie pomocnika tagów

`@tagHelperPrefix` Dyrektywy pozwala określić ciąg prefiksu tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego. Na przykład można dodać następujące znaczniki do *Views/_ViewImports.cshtml* pliku:

```cshtml
@tagHelperPrefix th:
```

Na poniższej ilustracji kodu jest równa prefiks Pomocnik tagu `th:`, więc tylko te elementy, które są przy użyciu prefiksu `th:` obsługuje pomocników tagów (włączone Pomocnik tagu elementy mają szczególne czcionki). `<label>` i `<input>` elementy mają prefiks Pomocnik tagu i obsługują Pomocnik tagu, podczas `<span>` nie elementu.

![obraz](intro/_static/thp.png)

Te same reguły hierarchii, które są stosowane do `@addTagHelper` dotyczą również `@tagHelperPrefix`.

## <a name="self-closing-tag-helpers"></a>Samozamykającego pomocnicy tagów

Wiele pomocników tagów nie można użyć jako własny taga zamykającego. Niektóre pomocników tagów są przeznaczone do można samozamykający tagów. Przy użyciu Pomocnika tagów, który nie został zaprojektowany jako samozamykającego pomija wyniku renderowania. Powoduje samozamykający pomocnika tagów w wyniku renderowania tagu samozamykającego. Aby uzyskać więcej informacji, zobacz [tej uwagi](xref:mvc/views/tag-helpers/authoring#self-closing) w [tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring).

## <a name="intellisense-support-for-tag-helpers"></a>Obsługa funkcji IntelliSense dla pomocników tagów

Po utworzeniu nowej aplikacji sieci web platformy ASP.NET Core w programie Visual Studio dodaje pakietu NuGet "Microsoft.AspNetCore.Razor.Tools". To jest pakiet, który dodaje narzędzi pomocnika tagów.

Rozważyć napisanie kodu HTML `<label>` elementu. Zaraz po wprowadzeniu `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:

![obraz](intro/_static/label.png)

Nie tylko pozwala to uzyskać pomocy HTML, ale ikonę ("@" symbol with "<>" pod nim).

![obraz](intro/_static/tagSym.png)

identyfikuje element jako objęte pomocników tagów. Czyste elementy HTML (takie jak `fieldset`) wyświetlana ikona "<>".

Czystym kodem HTML `<label>` tag Wyświetla tagu HTML (przy użyciu programu Visual Studio motyw domyślny) brązowy czcionki, atrybuty w kolorze czerwonym, i atrybutu wartości w kolorze niebieskim.

![obraz](intro/_static/LableHtmlTag.png)

Po wprowadzeniu `<label`, funkcja IntelliSense wyświetla dostępne atrybuty HTML/CSS i atrybuty nakierowane Pomocnik tagu:

![obraz](intro/_static/labelattr.png)

Instrukcji IntelliSense umożliwia wprowadzenie klawisz tab Aby wykonać instrukcję, określając wybraną wartość:

![obraz](intro/_static/stmtcomplete.png)

Zaraz po wprowadzeniu atrybut pomocnika tagów zmienić czcionki tagów i atrybutów. Korzystając z domyślne programu Visual Studio "Niebieski" lub "Jasny" motyw kolorów, czcionka jest pogrubiony purpurowy. Jeśli używasz "ciemny" czcionka jest pogrubiony zielonomodrym. Obrazy w niniejszym dokumencie zostały pobrane przy użyciu domyślnego motywu.

![obraz](intro/_static/labelaspfor2.png)

Możesz użyć programu Visual Studio *CompleteWord* skrót (Ctrl + spacja jest [domyślne](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) w podwójnym cudzysłowie (""), i jesteś teraz w języku C#, tak samo, jak będzie klasy C#. IntelliSense wyświetla wszystkie metody i właściwości w modelu strony. Metody i właściwości są dostępne, ponieważ typ właściwości to `ModelExpression`. Na poniższej ilustracji I jestem edycji `Register` widoku, więc `RegisterViewModel` jest dostępna.

![obraz](intro/_static/intellemail.png)

Funkcja IntelliSense wyświetla właściwości i metody dostępne dla modelu, na stronie. Bogate środowisko funkcji IntelliSense ułatwia wybór klasy CSS:

![obraz](intro/_static/iclass.png)

![obraz](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Pomocnicy tagów w porównaniu do pomocników HTML

Pomocnicy tagów, Dołącz do elementów kodu HTML w widokami Razor podczas [pomocników HTML](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) jest wywołana, zgodnie z metody grupową HTML widokami Razor. Należy wziąć pod uwagę następujące znaczniki Razor, który tworzy element label kodu HTML z klasy CSS "podpis":

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

U (`@`) symbol informuje Razor to początek kodu. Następne dwa parametry ("FirstName" i "imię:") są ciągami, więc [IntelliSense](/visualstudio/ide/using-intellisense) nie będzie mogła pomóc. Ostatni argument:

```cshtml
new {@class="caption"}
```

Anonimowy obiekt służy do reprezentowania atrybutów. Ponieważ `class` jest zastrzeżonym słowem kluczowym w C#, możesz użyć `@` symbolu, aby wymusić C# do interpretacji `@class=` jako symbol separatora (nazwa właściwości). Do frontonu projektanta (ktoś z HTML/CSS/JavaScript i innych technologii klienta, ale nie znają języka C# i Razor), większość wiersza jest obcy. Cały wiersz musi być utworzone przy użyciu pomoc od funkcji IntelliSense.

Za pomocą `LabelTagHelper`, ten sam kod znaczników, może być zapisana jako:

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

Za pomocą wersji pomocnika tagów, zaraz po wprowadzeniu `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:

![obraz](intro/_static/label.png)

Technologia IntelliSense pomaga napisać cały wiersz.

Na poniższej ilustracji kodzie przedstawiono część formularza *Views/Account/Register.cshtml* widoku Razor generowany na podstawie szablonu ASP.NET MVC 4.5.x, w programie Visual Studio.

![obraz](intro/_static/regCS.png)

Edytor programu Visual Studio Wyświetla kod C# na szarym tle. Na przykład `AntiForgeryToken` pomocnika kodu HTML:

```cshtml
@Html.AntiForgeryToken()
```

zostanie wyświetlona na szarym tle. Większość kodu znaczników w widoku rejestr jest C#. Dla porównania, równoważne podejście, używanie pomocników tagów:

![obraz](intro/_static/regTH.png)

Znaczniki jest znacznie bardziej przejrzyste i łatwiejsze do odczytania, edytowanie i obsługa niż metody pomocników HTML. Kod C# jest ograniczona do minimum, który serwer musi wiedzieć o. Edytor programu Visual Studio Wyświetla znaczników objęte pomocnika tagów w szczególne czcionki.

Należy wziąć pod uwagę *E-mail* grupy:

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

Każdego z atrybutów "asp-" ma wartość "Email", ale "Email" nie jest ciąg. W tym kontekście, "Email" jest C# wyrażenie właściwości modelu dla `RegisterViewModel`.

Edytor programu Visual Studio ułatwia pisanie **wszystkich** z kodu znaczników w ujęciu Pomocnik tagu formularza Zarejestruj się, gdy program Visual Studio udostępnia pomoc dla większości kod w metody pomocników HTML. [Obsługa funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) przechodzi do szczegółów na temat pracy z pomocników tagów w edytorze programu Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Pomocnicy tagów w porównaniu do formantów serwera sieci Web

* Pomocnicy tagów nie jest własnością elementu, z którymi są one związane. po prostu uczestniczą w czasie renderowania elementu, a zawartość. ASP.NET [kontrolki serwera sieci Web](https://msdn.microsoft.com/library/7698y1f0.aspx) zadeklarowana i wywoływane na stronie.

* [Sieci Web formanty serwera](https://msdn.microsoft.com/library/zsyt68f1.aspx) mają nietrywialnymi cykl życia, które mogą utrudnić opracowywania i debugowania.

* Formanty serwera sieci Web umożliwiają dodawanie funkcji do elementów modelu DOM (Document Object) klienta za pomocą formantu klienta. Pomocnicy tagów nie ma żadnych modelu DOM.

* Formanty serwera sieci Web obejmują wykrywania automatycznego przeglądarki. Pomocnicy tagów nie znają przeglądarki.

* Wiele pomocników tagów może działać na tym samym elemencie (zobacz [Pomocnik tagu unikanie konfliktów](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) podczas zwykle nie można utworzyć formanty serwera sieci Web.

* Pomocnicy tagów można zmodyfikować tagów i zawartości elementów kodu HTML, które są do zakresu, ale nie bezpośrednio modyfikować dowolne inne na stronie. Formanty serwera sieci Web mają szerszym zakresie i mogą wykonywać akcje, które mają wpływ na inne części strony; Włączanie wystąpienie niezamierzonych skutków ubocznych.

* Formanty serwera sieci Web umożliwia konwertowanie ciągów na obiektach konwerterów typów. Dzięki pomocnicy tagów możesz podjąć natywnie w języku C#, dzięki czemu nie trzeba wpisywać konwersji.

* Serwer sieci Web steruje użyciem [System.ComponentModel](/dotnet/api/system.componentmodel) Aby zaimplementować to zachowanie w czasie wykonywania oraz w czasie projektowania składników i formantów. `System.ComponentModel` zawiera klasy podstawowe i interfejsy do implementacji atrybutów i typy konwerterów, powiązanie z danymi źródeł i licencjonowanie składników. Natomiast, aby pomocnicy tagów, które zwykle dziedziczyć `TagHelper`i `TagHelper` klasy bazowej ujawnia tylko dwie metody, `Process` i `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Dostosowywanie czcionek element pomocnika tagów

Można dostosować, czcionki i kolorowanie z **narzędzia** > **opcje** > **środowiska** > **czcionek Kolory i**:

![obraz](intro/_static/fontoptions2.png)

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring)
* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [TagHelperSamples w serwisie GitHub](https://github.com/dpaquette/TagHelperSamples) zawiera przykłady Pomocnik tagu do pracy z [Bootstrap](https://getbootstrap.com/).
