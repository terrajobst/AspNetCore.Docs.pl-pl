---
title: Konfigurowanie lokalizacji obiektu przenośnego w ASP.NET Core
author: sebastienros
description: W tym artykule wprowadzono przenośne pliki obiektów i instrukcje dotyczące korzystania z nich w aplikacji ASP.NET Coreej z podstawową platformą sadu.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 08002564eb68bc04eebaeafed560202d0d69958a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656192"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a>Konfigurowanie lokalizacji obiektu przenośnego w ASP.NET Core

Autorzy [Sébastien ros](https://github.com/sebastienros) i [Scott Addie](https://twitter.com/Scott_Addie)

W tym artykule omówiono procedurę używania plików przenośnych obiektów (PO) w aplikacji ASP.NET Core z podstawową platformą [sadu](https://github.com/OrchardCMS/OrchardCore) .

**Uwaga:** Rdzeń sadu nie jest produktem firmy Microsoft. W związku z tym firma Microsoft nie zapewnia wsparcia dla tej funkcji.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Co to jest plik ZZ?

Pliki do zakupu są dystrybuowane jako pliki tekstowe zawierające przetłumaczone ciągi dla danego języka. Niektóre zalety korzystania z plików *. resx* są następujące:
- Pliki PO serwisie obsługują pluralizacja; pliki *resx* nie obsługują pluralizacja.
- Pliki ZZ nie są kompilowane jak pliki *resx* . W związku z tym wyspecjalizowane narzędzia i kroki kompilacji nie są wymagane.
- Pliki PO pracy działają dobrze z narzędziami do edycji w trybie online.

### <a name="example"></a>Przykład

Poniżej znajduje się przykładowy plik w formacie PO, zawierający tłumaczenie dla dwóch ciągów w języku francuskim, w tym jeden z jego postacią w liczbie mnogiej:

*fr. ZZ*

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

W tym przykładzie użyto następującej składni:

- `#:`: komentarz wskazujący kontekst ciągu do tłumaczenia. Ten sam ciąg może być przetłumaczony inaczej w zależności od tego, gdzie jest używany.
- `msgid`: nieprzetłumaczony ciąg.
- `msgstr`: przetłumaczony ciąg.

W przypadku pomocy technicznej pluralizacja można zdefiniować więcej wpisów.

- `msgid_plural`: nieprzetłumaczony ciąg w liczbie mnogiej.
- `msgstr[0]`: przetłumaczony ciąg dla przypadku 0.
- `msgstr[N]`: przetłumaczony ciąg dla przypadku N.

Specyfikację pliku PO stronie można znaleźć [tutaj](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Konfigurowanie obsługi plików PO zalogowaniu w ASP.NET Core

Ten przykład jest oparty na aplikacji ASP.NET Core MVC wygenerowanej na podstawie szablonu projektu programu Visual Studio 2017.

### <a name="referencing-the-package"></a>Odwoływanie się do pakietu

Dodaj odwołanie do pakietu NuGet `OrchardCore.Localization.Core`. Jest on dostępny w witrynie [MyGet](https://www.myget.org/) pod następującym źródłem pakietu: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

Plik *. csproj* zawiera teraz wiersz podobny do następującego (numer wersji może się różnić):

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Rejestrowanie usługi

Dodaj wymagane usługi do metody `ConfigureServices` *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Dodaj wymagane oprogramowanie pośredniczące do metody `Configure` *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Dodaj następujący kod do wybranego widoku Razor. W tym przykładzie użyto *. cshtml* .

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

Wystąpienie `IViewLocalizer` jest wstrzykiwane i służy do tłumaczenia tekstu "Hello World!".

### <a name="creating-a-po-file"></a>Tworzenie pliku ZZ

Utwórz plik o nazwie *\<Culture code >. po* w folderze głównym aplikacji. W tym przykładzie nazwa pliku to *fr. po* , ponieważ używany jest język francuski:

[!code-text[](localization/sample/POLocalization/fr.po)]

Ten plik przechowuje zarówno ciąg do przetłumaczenia, jak i ciąg przetłumaczony w języku francuskim. W razie potrzeby tłumaczenia są przywracane do ich kultury nadrzędnej. W tym przykładzie plik *fr. ZZ* jest używany, jeśli żądana kultura jest `fr-FR` lub `fr-CA`.

### <a name="testing-the-application"></a>Testowanie aplikacji

Uruchom aplikację i przejdź do adresu URL `/Home/About`. Tekst **Witaj świecie!** jest wyświetlany.

Przejdź do adresu URL `/Home/About?culture=fr-FR`. Tekst **Bonjour Le Monde!** jest wyświetlany.

## <a name="pluralization"></a>Pluralizacja

Pliki ZZ obsługują formularze pluralizacja, co jest przydatne, gdy ten sam ciąg musi być przetłumaczony inaczej w oparciu o Kardynalność. To zadanie jest wykonywane w sposób skomplikowany przez fakt, że każdy język definiuje reguły niestandardowe, aby wybrać ciąg, który ma być używany na podstawie kardynalności.

Pakiet lokalizacji sadu udostępnia interfejs API umożliwiający automatyczne wywoływanie różnych formularzy w liczbie mnogiej.

### <a name="creating-pluralization-po-files"></a>Tworzenie plików pluralizacja PO

Dodaj następującą zawartość do wspomnianego wcześniej pliku *fr. ZZ* :

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Zobacz, [co to jest plik ZZ?](#what-is-a-po-file) , aby uzyskać wyjaśnienie, co reprezentuje każdy wpis w tym przykładzie.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Dodawanie języka przy użyciu różnych formularzy pluralizacja

W poprzednim przykładzie użyto następujących ciągów w języku angielskim i francuskim. W języku angielskim i francuskim istnieją tylko dwa formularze pluralizacja i współdzielą te same reguły formularza, co oznacza, że Kardynalność jednej z nich jest zamapowana na pierwszą formę w liczbie mnogiej. Każda inna Kardynalność jest mapowana na drugą formę w liczbie mnogiej.

Nie wszystkie języki mają te same reguły. Jest to zilustrowane w języku czeskim, który ma trzy formy w liczbie mnogiej.

Utwórz plik `cs.po` w następujący sposób i zwróć uwagę na to, jak pluralizacja potrzebuje trzech różnych tłumaczeń:

[!code-text[](localization/sample/POLocalization/cs.po)]

Aby zaakceptować lokalizacje czeskie, należy dodać `"cs"` do listy obsługiwanych kultur w metodzie `ConfigureServices`:

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

Edytuj plik *views/Home/about. cshtml* , aby renderować zlokalizowane ciągi w liczbie mnogiej dla kilku kardynalnych:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Uwaga:** W świecie rzeczywistym, zmienna byłaby używana do reprezentowania liczby. W tym miejscu powtarzamy ten sam kod z trzema różnymi wartościami, aby uwidocznić konkretny przypadek.

Po przełączeniu kultur widoczne są następujące elementy:

Instrukcję `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Instrukcję `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Instrukcję `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Należy pamiętać, że w przypadku kultury Czeskiej trzy tłumaczenia są różne. Kultury francuskie i angielskie korzystają z tej samej konstrukcji dla dwóch ostatnich przetłumaczonych ciągów.

## <a name="advanced-tasks"></a>Zaawansowane zadania

### <a name="contextualizing-strings"></a>Contextualizing ciągi

Aplikacje często zawierają ciągi do tłumaczenia w kilku miejscach. Ten sam ciąg może mieć inne tłumaczenie w określonych lokalizacjach w aplikacji (widoki Razor lub pliki klas). Plik ZZ obsługuje pojęcie kontekstu pliku, który może służyć do kategoryzowania reprezentowanego ciągu. Przy użyciu kontekstu pliku ciąg może być przetłumaczony inaczej, w zależności od kontekstu pliku (lub braku kontekstu pliku).

Usługi lokalizowania PO stronie używają nazwy pełnej klasy lub widoku, który jest używany podczas tłumaczenia ciągu. W tym celu należy ustawić wartość wpisu `msgctxt`.

Rozważmy drobne dodanie do poprzedniego przykładu *fr. po* . Widok Razor zlokalizowany w *widokach/Home/about. cshtml* można zdefiniować jako kontekst pliku przez ustawienie wartości zastrzeżonego wpisu `msgctxt`:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Po ustawieniu `msgctxt` w taki sposób tłumaczenie tekstu odbywa się podczas przechodzenia do `/Home/About?culture=fr-FR`. Tłumaczenie nie będzie odbywać się podczas nawigowania do `/Home/Contact?culture=fr-FR`.

Gdy żaden konkretny wpis nie jest dopasowany do danego kontekstu pliku, mechanizm rezerwowy elementu sadu rdzeń szuka odpowiedniego pliku. Przy założeniu, że nie określono określonego kontekstu pliku dla *widoków/Home/Contact. cshtml*, przechodzenie do `/Home/Contact?culture=fr-FR` ładowania pliku ZZ, takiego jak:

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Zmiana lokalizacji plików ZZ

Domyślną lokalizację plików PO stronie można zmienić w `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

W tym przykładzie pliki PO załadowaniu są ładowane z folderu *lokalizacji* .

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementowanie logiki niestandardowej do znajdowania plików lokalizacyjnych

Gdy do lokalizowania plików jest wymagana bardziej złożona logika, interfejs `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` może zostać zaimplementowany i zarejestrowany jako usługa. Jest to przydatne, gdy pliki do zakupu mogą być przechowywane w różnych lokalizacjach lub w przypadku, gdy pliki muszą znajdować się w hierarchii folderów.

### <a name="using-a-different-default-pluralized-language"></a>Używanie innego domyślnego języka w liczbie mnogiej

Pakiet zawiera `Plural` metodę rozszerzenia, która jest specyficzna dla dwóch form w liczbie mnogiej. W przypadku języków wymagających większej liczby formularzy w liczbie mnogiej Utwórz metodę rozszerzenia. Przy użyciu metody rozszerzenia nie trzeba podawać żadnych plików lokalizacji dla języka domyślnego, &mdash; oryginalne ciągi są już dostępne bezpośrednio w kodzie.

Można użyć bardziej ogólnego przeciążenia `Plural(int count, string[] pluralForms, params object[] arguments)`, które akceptuje tablicę ciągów tłumaczeń.
