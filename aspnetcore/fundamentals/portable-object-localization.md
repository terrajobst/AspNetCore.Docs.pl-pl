---
title: "Konfigurowanie lokalizacji obiektu przenośne"
author: sebastienros
description: "W tym artykule przedstawiono pliki Portable obiektów oraz opisano kroki dotyczące korzystania z nich w aplikacji platformy ASP.NET Core Framework Orchard Core."
manager: wpickett
ms.author: scaddie
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 6fefbd9b28d481184e358e7d66af68d112c63696
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Konfigurowanie lokalizacji obiektu przenośne Orchard podstawowych

Przez [OK Sébastien](https://github.com/sebastienros) i [Scott Addie](https://twitter.com/Scott_Addie)

W tym artykule przedstawiono kroki dotyczące korzystania z plików przenośnych obiektu (PO) w aplikacji platformy ASP.NET Core z [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Uwaga:** Orchard Core nie jest produktem firmy Microsoft. W rezultacie Microsoft nie zapewnia obsługi dla tej funkcji.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Co to jest plik PO?

PO pliki są dystrybuowane jako pliki tekstowe zawierający przetłumaczone ciągi dla danego języka. Niektóre zalety funkcji zamiast plikach Arządzania *.resx* pliki obejmują:
- Pliki PO obsługują określania liczby mnogiej; *.resx* plików nie obsługuje określania liczby mnogiej.
- PO pliki nie są kompilowane jak *.resx* plików. W efekcie specjalne kroki narzędzi i kompilacji nie są wymagane.
- Pliki PO działają prawidłowo w przypadku współpracy online narzędzia do edycji.

### <a name="example"></a>Przykład

Oto przykładowy plik PO zawierających Translacja dwa ciągi w języku francuskim, łącznie z jednego z jego mnogiej:

*FR.po*

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

- `#:`: Komentarz wskazujący kontekście ciąg, który ma zostać poddany translacji. Tych samych parametrach może zostać przetłumaczone inaczej w zależności od tego, gdzie jest używany.
- `msgid`: Niezrozumiały ciąg.
- `msgstr`: Przetłumaczonego ciąg.

W przypadku określania liczby mnogiej pomocy technicznej można zdefiniować więcej wpisów.

- `msgid_plural`: Niezrozumiały mnogiej ciąg.
- `msgstr[0]`: Przetłumaczonego ciąg w przypadku 0.
- `msgstr[N]`: Przetłumaczonego ciąg wielkość N.

Specyfikacja pliku PO można znaleźć [tutaj](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Konfigurowanie obsługi plików PO w ASP.NET Core

Ten przykład jest oparty na aplikacji ASP.NET Core MVC wygenerowany na podstawie szablonu projektu programu Visual Studio 2017 r.

### <a name="referencing-the-package"></a>Odwołanie do pakietu

Dodaj odwołanie do `OrchardCore.Localization.Core` pakietu NuGet. Jest ona dostępna na [MyGet](https://www.myget.org/) na następujące źródło pakietu: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* plik zawiera teraz wiersz podobny do następującego (numer wersji może się różnić):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Rejestrowanie usługi

Dodaj wymagane usługi, aby `ConfigureServices` metody *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Dodaj wymagane oprogramowanie pośredniczące o konieczności `Configure` metody *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Dodaj następujący kod do wyboru widoku Razor. *About.cshtml* jest używany w tym przykładzie.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer` Wystąpienia jest wprowadzonym i umożliwia tłumaczenie tekstu "Hello world!".

### <a name="creating-a-po-file"></a>Tworzenie pliku PO

Utwórz plik o nazwie  *<culture code>po oraz pakiety* w folderze głównym aplikacji. W tym przykładzie nazwa pliku jest *fr.po* ponieważ jest używana w języku francuskim:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Ten plik z rozszerzeniem zarówno ciąg do tłumaczenia ciąg translacji francuski. Tłumaczeń przywrócić ich Kultura nadrzędna, jeśli to konieczne. W tym przykładzie *fr.po* plik jest używany, jeśli jest żądaną kulturę `fr-FR` lub `fr-CA`.

### <a name="testing-the-application"></a>Testowanie aplikacji

Uruchom aplikację, a następnie przejdź do adresu URL `/Home/About`. Tekst **Witaj świecie!** zostanie wyświetlona.

Przejdź do adresu URL `/Home/About?culture=fr-FR`. Tekst **Bonjour le monde!** zostanie wyświetlona.

## <a name="pluralization"></a>Określania liczby mnogiej

Pliki PO obsługują formularze określania liczby mnogiej, co jest przydatne, gdy te same parametry musi zostać przetłumaczone inaczej w zależności od mają kardynalność. To zadanie jest nawiązywane skomplikowane faktem, że każdego języka definiuje reguły niestandardowe, aby wybrać ciąg, który ma być używany w oparciu kardynalność.

Pakiet lokalizacja Orchard udostępnia interfejs API do wywoływania automatycznie tych różnych w liczbie mnogiej.

### <a name="creating-pluralization-po-files"></a>Tworzenie określania liczby mnogiej zamówienia zakupu plików

Dodaj następującą zawartość do wymienione wcześniej *fr.po* pliku:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Zobacz [co to jest plik PO?](#what-is-a-po-file) wyjaśnienie reprezentuje każdego wpisu w tym przykładzie.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Dodawanie języka przy użyciu różnych określania liczby mnogiej formularzy

Ciągi w języku angielskim i francuskim były używane w poprzednim przykładzie. Angielskim i francuskim ma tylko dwie formy określania liczby mnogiej i udostępniać te same zasady formularza, czyli że Kardynalność jednej jest mapowany na pierwszym mnogiej. Inne Kardynalność jest mapowana na drugi mnogiej.

Nie wszystkie języki mają te same zasady stosowania. Jest to zilustrowane z czeski języka, który ma trzy w liczbie mnogiej.

Utwórz `cs.po` plików w następujący sposób, zwracając uwagę, jak określania liczby mnogiej wymaga trzech różnych tłumaczenia:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Aby zaakceptować lokalizacje czeski, Dodaj `"cs"` do listy obsługiwanych kultur w `ConfigureServices` metody:

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

Edytuj *Views/Home/About.cshtml* pliku do renderowania zlokalizowanego, liczbie mnogiej ciągów dla kilku cardinalities:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Uwaga:** w scenariuszu rzeczywistych zmiennej będzie służyć do reprezentowania licznika. W tym miejscu możemy Powtórz ten sam kod z trzech różnych wartości do udostępnienia bardzo konkretnym przypadku.

Po przełączeniu kultury, zostanie wyświetlony poniżej:

Dla `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Dla `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Dla `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Należy pamiętać, że dla kultury czeski, trzy tłumaczenia są różne. Kultur francuski i angielski udostępnianie tego samego konstrukcji dla dwóch ostatnich przetłumaczone ciągi.

## <a name="advanced-tasks"></a>Zaawansowane zadania

### <a name="contextualizing-strings"></a>Ciągi kontekstem

Aplikacje często zawierają ciągi do tłumaczenia w kilku miejscach. Te same parametry mogą mieć różnych tłumaczenia w określonych lokalizacjach w aplikacji (widokami Razor lub klasa plików). Plik PO obsługuje pojęcie kontekstem pliku, który może służyć do klasyfikowania ciąg reprezentowanego. Przy użyciu kontekstu pliku, ciąg można przekonwertować różnie w zależności od kontekstu pliku (lub brak kontekstu pliku).

Usługi lokalizacji PO Użyj nazwy klasy pełną lub widok, który jest używany podczas tłumaczenia ciąg. Jest to osiągane przez ustawienie wartości na `msgctxt` wpisu.

Należy wziąć pod uwagę pomocnicza dodanie do poprzedniego *fr.po* przykład. W lokalizacji widoku Razor *Views/Home/About.cshtml* można zdefiniować jako kontekst plików przez ustawienie zarezerwowanego `msgctxt` wartość wpisu:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Z `msgctxt` tak ustawiona, tłumaczenie tekstu występuje podczas nawigowania do `/Home/About?culture=fr-FR`. Tłumaczenie nie występuje podczas nawigowania do `/Home/Contact?culture=fr-FR`.

Nie określonego wpisu nie odpowiada z kontekstem danego pliku, mechanizm rezerwowy Orchard Core szuka odpowiedniego pliku PO bez kontekstu. Zakładając, że nie jest zdefiniowany dla kontekstu określonego pliku *Views/Home/Contact.cshtml*, nawigacyjnego do `/Home/Contact?culture=fr-FR` ładuje plik PO, takich jak:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Zmiana lokalizacji plików PO

Domyślna lokalizacja plików PO można zmienić w `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

W tym przykładzie plikach Arządzania są ładowane z *lokalizacja* folderu.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Wdrażanie niestandardowej logiki do znajdowania lokalizacji plików

Gdy wymagany jest bardziej złożonej logice można znaleźć w plikach Arządzania `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfejsu może być wdrożone i zarejestrowany jako usługa. Jest to przydatne, gdy PO pliki mogą być przechowywane w różnych lokalizacjach, lub jeśli pliki znajdują się w hierarchii folderów.

### <a name="using-a-different-default-pluralized-language"></a>Przy użyciu języka różne domyślne pluralized

Ten pakiet zawiera `Plural` — metoda rozszerzenia, które są specyficzne dla dwóch w liczbie mnogiej. W przypadku języków wymagających więcej w liczbie mnogiej create — metoda rozszerzenia. Przy użyciu metody rozszerzenia, nie trzeba udostępnić plik dowolnej lokalizacji w języku domyślnym &mdash; oryginalnego ciągi są już dostępne bezpośrednio w kodzie.

Można używać więcej ogólnych `Plural(int count, string[] pluralForms, params object[] arguments)` przepełnienia, które akceptuje tłumaczeń tablicy ciągów.
