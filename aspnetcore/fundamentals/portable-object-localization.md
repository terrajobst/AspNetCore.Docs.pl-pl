---
title: Konfigurowanie lokalizacji obiektu przenośnego w programie ASP.NET Core
author: sebastienros
description: Ten artykuł wprowadza plików przenośnych obiektów i opisano kroki używania ich w aplikacji ASP.NET Core przy użyciu Orchard Core framework.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 466759b30e756a7cac8abab7352025df0462bb6f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210096"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a>Konfigurowanie lokalizacji obiektu przenośnego w programie ASP.NET Core

Przez [OK Sébastien](https://github.com/sebastienros) i [Scott Addie](https://twitter.com/Scott_Addie)

W tym artykule opisano kolejne kroki dotyczące korzystania z plików obiektu przenośnego (PO) w aplikacji platformy ASP.NET Core za pomocą [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Uwaga:** Orchard Core nie jest produktem firmy Microsoft. W związku z tym Microsoft nie obsługuje tej funkcji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Co to jest plik zamówienia zakupu?

Pliki PO są dystrybuowane jako pliki tekstowe, zawierający przetłumaczone ciągi dla danego języka. Niektóre zalety zamiast plików PO *resx* pliki obejmują:
- Pliki PO obsługują pluralizacja; *resx* pliki nie obsługują pluralizacja.
- Pliki zamówienia zakupu nie są kompilowane, takich jak *resx* plików. W efekcie wyspecjalizowane narzędzia i kompilacji kroki nie są wymagane.
- Pliki PO działać dobrze współpracy online narzędzia do edycji.

### <a name="example"></a>Przykład

Poniżej przedstawiono przykładowy plik PO zawierających Translacja dwa ciągi w języku francuskim, łącznie z jego liczba mnoga:

*fr.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

W tym przykładzie używa następującej składni:

- `#:`: Komentarz wskazujący kontekście ciąg, który ma zostać poddany translacji. Te same parametry mogą być tłumaczone różnie w zależności od tego, gdzie jest on używany.
- `msgid`: Nieprzetłumaczonych ciągów.
- `msgstr`: Przetłumaczony ciąg.

W przypadku obsługi pluralizacja można zdefiniować więcej wpisów.

- `msgid_plural`: Liczba mnoga nieprzetłumaczonych ciąg.
- `msgstr[0]`: Przetłumaczonego ciągu w przypadku 0.
- `msgstr[N]`: Przetłumaczony ciąg dla przypadków N.

Specyfikacja pliku zamówienia zakupu można znaleźć [tutaj](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Konfigurowanie obsługi plików zamówienia zakupu w programie ASP.NET Core

Ten przykład jest oparty na aplikacji ASP.NET Core MVC generowany na podstawie szablonu projektu programu Visual Studio 2017.

### <a name="referencing-the-package"></a>Odwołanie do pakietu

Dodaj odwołanie do `OrchardCore.Localization.Core` pakietu NuGet. Jest ona dostępna na [MyGet](https://www.myget.org/) następujące źródła pakietu: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* plik zawiera teraz linię podobne do następujących (numer wersji może być inna):

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Rejestrowanie usługi

Dodaj wymagane usługi, aby `ConfigureServices` metody *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Dodaj wymagane oprogramowanie pośredniczące `Configure` metody *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Dodaj następujący kod do wybranego widoku Razor. *About.cshtml* jest używany w tym przykładzie.

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer` Wystąpienie jest wprowadzony i umożliwia tłumaczenie tekstu "Hello world!".

### <a name="creating-a-po-file"></a>Tworzenie pliku zamówienia zakupu

Utwórz plik o nazwie  *\<kulturze kodu > je* w folderze głównym aplikacji. W tym przykładzie nazwa pliku jest *fr.po* ponieważ jest używana w języku francuskim:

[!code-text[](localization/sample/POLocalization/fr.po)]

Ten plik przechowuje ciągu do tłumaczenia i ciąg przetłumaczone francuski. Tłumaczenia przywrócić ich nadrzędnego kultury, jeśli to konieczne. W tym przykładzie *fr.po* plik jest używany, jeśli jest żądaną kulturę `fr-FR` lub `fr-CA`.

### <a name="testing-the-application"></a>Testowanie aplikacji

Uruchom aplikację i przejdź do adresu URL `/Home/About`. Tekst **Witaj świecie!** zostanie wyświetlona.

Przejdź do adresu URL `/Home/About?culture=fr-FR`. Tekst **Bonjour le monde!** zostanie wyświetlona.

## <a name="pluralization"></a>Pluralizacja

Pliki PO obsługuje pluralizacja formularzy, co jest przydatne, gdy ten sam ciąg musi być tłumaczony inaczej w zależności od kardynalności. To zadanie wykonywane skomplikowane faktem, że każdy język definiuje reguły niestandardowe, aby wybrać, które parametry do użycia oparte na kardynalność.

Pakiet lokalizacji witryny systemu Orchard zapewnia interfejs API do wywołania tych różnych postaci mnogiej automatycznie.

### <a name="creating-pluralization-po-files"></a>Tworzenie pluralizacja plików zamówienia zakupu

Dodaj następującą zawartość do wymienionych wcześniej *fr.po* pliku:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Zobacz [co to jest plik PO?](#what-is-a-po-file) wyjaśnienie co reprezentuje każdy wpis, w tym przykładzie.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Dodawanie języka przy użyciu różnych pluralizacja formularzy

Ciągi języka angielskiego i francuskiego zostały użyte w poprzednim przykładzie. Języka angielskiego i francuskiego ma tylko dwie formy pluralizacja i udostępniać te same zasady formularza, czyli czy Kardynalność jednego jest mapowane na pierwszym mnogiej. Inne Kardynalność jest zamapowana na drugim mnogiej.

Nie wszystkie języki udostępniać te same reguły. Jest to zilustrowane w języku Czeskiej, która ma trzy w liczbie mnogiej.

Utwórz `cs.po` pliku w następujący sposób i zwróć uwagę, jak pluralizacja wymaga trzech różnych tłumaczeń:

[!code-text[](localization/sample/POLocalization/cs.po)]

Aby zaakceptować lokalizacje Czeskiej, Dodaj `"cs"` do listy obsługiwanych kultur w `ConfigureServices` metody:

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

Edytuj *Views/Home/About.cshtml* pliku do renderowania ciągi zlokalizowane, plural dla kilku kardynalności:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Uwaga:** W rzeczywistym scenariuszu zmienna może służyć do reprezentowania liczby. W tym miejscu możemy Powtórz ten sam kod za pomocą trzy różne wartości, aby ujawnić bardzo szczegółowych przypadków.

Po przełączeniu kultur, zostaną wyświetlone następujące czynności:

Dla programów `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Dla programów `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Dla programów `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Należy pamiętać, że dla kultury Czeskiej, trzy tłumaczenia są różne. Kultury francuski i angielski udostępnianie tych samych konstrukcji dla dwóch ostatnich przetłumaczone ciągi.

## <a name="advanced-tasks"></a>Zaawansowane zadania

### <a name="contextualizing-strings"></a>Ciągi uczestnika

Aplikacje często zawierają ciągi, które można przetłumaczyć w kilku miejscach. Te same parametry mogą mieć różnych tłumaczeń w określonych lokalizacjach w obrębie aplikacji (widokami Razor lub plików klasy). Plik PO obsługuje pojęcie kontekstu pliku, który może służyć do kategoryzowania ciągu reprezentowanego. Za pomocą kontekstu pliku, ciągu mogą być tłumaczone różnie, w zależności od kontekstu pliku (lub brak kontekstu pliku).

Usług lokalizacyjnych zamówienia zakupu, użyj nazwy klasy pełną lub widok, który jest używany podczas tłumaczenia ciąg. Jest to osiągane przez ustawienie wartości na `msgctxt` wpisu.

Należy wziąć pod uwagę pomocnicza dodanie do poprzedniego *fr.po* przykład. Widok Razor znajdujący się w *Views/Home/About.cshtml* mogą być definiowane jako kontekstu pliku przez ustawienie zarezerwowanego `msgctxt` wartość wpisu:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Za pomocą `msgctxt` ustawione jako takie, tłumaczenie tekstu występuje podczas przechodzenia do `/Home/About?culture=fr-FR`. Tłumaczenie nie występują w przypadku przechodzenia do `/Home/Contact?culture=fr-FR`.

Nie określonego wpisu jest dopasowywany w kontekście danego pliku, mechanizm rezerwowy Orchard Core szuka pliku odpowiednią zamówienia zakupu bez kontekstu. Zakładając, że nie jest zdefiniowany dla kontekstu określonego pliku *Views/Home/Contact.cshtml*, po do `/Home/Contact?culture=fr-FR` ładuje plik zamówienia zakupu, takich jak:

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Zmiana lokalizacji plików zamówienia zakupu

Można zmienić domyślną lokalizację plików PO w programie `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

W tym przykładzie zamówienia zakupu pliki są ładowane z *lokalizacji* folderu.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementowanie logiki niestandardowej do znajdowania lokalizacji plików

Gdy będzie potrzebny bardziej złożonej logiki, aby zlokalizować pliki PO `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfejsu może być zaimplementowany i zarejestrowany jako usługa. Jest to przydatne, gdy pliki PO mogą być przechowywane w różnych lokalizacjach lub gdy pliki znajdują się w hierarchii folderów.

### <a name="using-a-different-default-pluralized-language"></a>Za pomocą języka różne domyślne pluralized

Ten pakiet zawiera `Plural` metodę rozszerzenia, które są specyficzne dla dwóch w liczbie mnogiej. W przypadku języków wymagających więcej w liczbie mnogiej utworzyć metodę rozszerzenia. Za pomocą metody rozszerzenia, nie należy podać dowolny plik lokalizacji dla domyślnego języka &mdash; oryginalnego ciągi są już dostępne bezpośrednio w kodzie.

Można użyć więcej ogólny `Plural(int count, string[] pluralForms, params object[] arguments)` przeciążenia, który przyjmuje tablicę ciągów, tłumaczeń.
