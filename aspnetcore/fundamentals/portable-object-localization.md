---
title: "Konfigurowanie lokalizacji obiektu przenośne"
author: sebastienros
description: "W tym artykule przedstawiono pliki Portable obiektów oraz opisano kroki dotyczące korzystania z nich w aplikacji platformy ASP.NET Core Framework Orchard Core."
keywords: "Platformy ASP.NET Core, lokalizacji, kultury, języka, przenośne obiektu"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="3897f-104">Konfigurowanie lokalizacji obiektu przenośne Orchard podstawowych</span><span class="sxs-lookup"><span data-stu-id="3897f-104">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="3897f-105">Przez [OK Sébastien](https://github.com/sebastienros) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3897f-105">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3897f-106">W tym artykule przedstawiono kroki dotyczące korzystania z plików przenośnych obiektu (PO) w aplikacji platformy ASP.NET Core z [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="3897f-106">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="3897f-107">**Uwaga:** Orchard Core nie jest produktem firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3897f-107">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="3897f-108">W rezultacie Microsoft nie zapewnia obsługi dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="3897f-108">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="3897f-109">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3897f-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="3897f-110">Co to jest plik PO?</span><span class="sxs-lookup"><span data-stu-id="3897f-110">What is a PO file?</span></span>

<span data-ttu-id="3897f-111">PO pliki są dystrybuowane jako pliki tekstowe zawierający przetłumaczone ciągi dla danego języka.</span><span class="sxs-lookup"><span data-stu-id="3897f-111">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="3897f-112">Niektóre zalety funkcji zamiast plikach Arządzania *.resx* pliki obejmują:</span><span class="sxs-lookup"><span data-stu-id="3897f-112">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="3897f-113">Pliki PO obsługują określania liczby mnogiej; *.resx* plików nie obsługuje określania liczby mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-113">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="3897f-114">PO pliki nie są kompilowane jak *.resx* plików.</span><span class="sxs-lookup"><span data-stu-id="3897f-114">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="3897f-115">W efekcie specjalne kroki narzędzi i kompilacji nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="3897f-115">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="3897f-116">Pliki PO działają prawidłowo w przypadku współpracy online narzędzia do edycji.</span><span class="sxs-lookup"><span data-stu-id="3897f-116">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="3897f-117">Przykład</span><span class="sxs-lookup"><span data-stu-id="3897f-117">Example</span></span>

<span data-ttu-id="3897f-118">Oto przykładowy plik PO zawierających Translacja dwa ciągi w języku francuskim, łącznie z jednego z jego mnogiej:</span><span class="sxs-lookup"><span data-stu-id="3897f-118">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="3897f-119">*FR.po*</span><span class="sxs-lookup"><span data-stu-id="3897f-119">*fr.po*</span></span>

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

<span data-ttu-id="3897f-120">W tym przykładzie używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="3897f-120">This example uses the following syntax:</span></span>

- <span data-ttu-id="3897f-121">`#:`: Komentarz wskazujący kontekście ciąg, który ma zostać poddany translacji.</span><span class="sxs-lookup"><span data-stu-id="3897f-121">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="3897f-122">Tych samych parametrach może zostać przetłumaczone inaczej w zależności od tego, gdzie jest używany.</span><span class="sxs-lookup"><span data-stu-id="3897f-122">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="3897f-123">`msgid`: Niezrozumiały ciąg.</span><span class="sxs-lookup"><span data-stu-id="3897f-123">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="3897f-124">`msgstr`: Przetłumaczonego ciąg.</span><span class="sxs-lookup"><span data-stu-id="3897f-124">`msgstr`: The translated string.</span></span>

<span data-ttu-id="3897f-125">W przypadku określania liczby mnogiej pomocy technicznej można zdefiniować więcej wpisów.</span><span class="sxs-lookup"><span data-stu-id="3897f-125">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="3897f-126">`msgid_plural`: Niezrozumiały mnogiej ciąg.</span><span class="sxs-lookup"><span data-stu-id="3897f-126">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="3897f-127">`msgstr[0]`: Przetłumaczonego ciąg w przypadku 0.</span><span class="sxs-lookup"><span data-stu-id="3897f-127">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="3897f-128">`msgstr[N]`: Przetłumaczonego ciąg wielkość N.</span><span class="sxs-lookup"><span data-stu-id="3897f-128">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="3897f-129">Specyfikacja pliku PO można znaleźć [tutaj](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="3897f-129">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="3897f-130">Konfigurowanie obsługi plików PO w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3897f-130">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="3897f-131">Ten przykład jest oparty na aplikacji ASP.NET Core MVC wygenerowany na podstawie szablonu projektu programu Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="3897f-131">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="3897f-132">Odwołanie do pakietu</span><span class="sxs-lookup"><span data-stu-id="3897f-132">Referencing the package</span></span>

<span data-ttu-id="3897f-133">Dodaj odwołanie do `OrchardCore.Localization.Core` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3897f-133">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="3897f-134">Jest ona dostępna na [MyGet](https://www.myget.org/) na następujące źródło pakietu: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="3897f-134">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="3897f-135">*.Csproj* plik zawiera teraz wiersz podobny do następującego (numer wersji może się różnić):</span><span class="sxs-lookup"><span data-stu-id="3897f-135">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="3897f-136">Rejestrowanie usługi</span><span class="sxs-lookup"><span data-stu-id="3897f-136">Registering the service</span></span>

<span data-ttu-id="3897f-137">Dodaj wymagane usługi, aby `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3897f-137">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="3897f-138">Dodaj wymagane oprogramowanie pośredniczące o konieczności `Configure` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3897f-138">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="3897f-139">Dodaj następujący kod do wyboru widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="3897f-139">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="3897f-140">*About.cshtml* jest używany w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3897f-140">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="3897f-141">`IViewLocalizer` Wystąpienia jest wprowadzonym i umożliwia tłumaczenie tekstu "Hello world!".</span><span class="sxs-lookup"><span data-stu-id="3897f-141">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="3897f-142">Tworzenie pliku PO</span><span class="sxs-lookup"><span data-stu-id="3897f-142">Creating a PO file</span></span>

<span data-ttu-id="3897f-143">Utwórz plik o nazwie  *<culture code>po oraz pakiety* w folderze głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3897f-143">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="3897f-144">W tym przykładzie nazwa pliku jest *fr.po* ponieważ jest używana w języku francuskim:</span><span class="sxs-lookup"><span data-stu-id="3897f-144">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="3897f-145">Ten plik z rozszerzeniem zarówno ciąg do tłumaczenia ciąg translacji francuski.</span><span class="sxs-lookup"><span data-stu-id="3897f-145">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="3897f-146">Tłumaczeń przywrócić ich Kultura nadrzędna, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3897f-146">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="3897f-147">W tym przykładzie *fr.po* plik jest używany, jeśli jest żądaną kulturę `fr-FR` lub `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="3897f-147">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="3897f-148">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3897f-148">Testing the application</span></span>

<span data-ttu-id="3897f-149">Uruchom aplikację, a następnie przejdź do adresu URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="3897f-149">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="3897f-150">Tekst **Witaj świecie!**</span><span class="sxs-lookup"><span data-stu-id="3897f-150">The text **Hello world!**</span></span> <span data-ttu-id="3897f-151">zostanie wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="3897f-151">is displayed.</span></span>

<span data-ttu-id="3897f-152">Przejdź do adresu URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3897f-152">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="3897f-153">Tekst **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="3897f-153">The text **Bonjour le monde!**</span></span> <span data-ttu-id="3897f-154">zostanie wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="3897f-154">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="3897f-155">Określania liczby mnogiej</span><span class="sxs-lookup"><span data-stu-id="3897f-155">Pluralization</span></span>

<span data-ttu-id="3897f-156">Pliki PO obsługują formularze określania liczby mnogiej, co jest przydatne, gdy te same parametry musi zostać przetłumaczone inaczej w zależności od mają kardynalność.</span><span class="sxs-lookup"><span data-stu-id="3897f-156">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="3897f-157">To zadanie jest nawiązywane skomplikowane faktem, że każdego języka definiuje reguły niestandardowe, aby wybrać ciąg, który ma być używany w oparciu kardynalność.</span><span class="sxs-lookup"><span data-stu-id="3897f-157">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="3897f-158">Pakiet lokalizacja Orchard udostępnia interfejs API do wywoływania automatycznie tych różnych w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-158">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="3897f-159">Tworzenie określania liczby mnogiej zamówienia zakupu plików</span><span class="sxs-lookup"><span data-stu-id="3897f-159">Creating pluralization PO files</span></span>

<span data-ttu-id="3897f-160">Dodaj następującą zawartość do wymienione wcześniej *fr.po* pliku:</span><span class="sxs-lookup"><span data-stu-id="3897f-160">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="3897f-161">Zobacz [co to jest plik PO?](#what-is-a-po-file) wyjaśnienie reprezentuje każdego wpisu w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3897f-161">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="3897f-162">Dodawanie języka przy użyciu różnych określania liczby mnogiej formularzy</span><span class="sxs-lookup"><span data-stu-id="3897f-162">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="3897f-163">Ciągi w języku angielskim i francuskim były używane w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3897f-163">English and French strings were used in the previous example.</span></span> <span data-ttu-id="3897f-164">Angielskim i francuskim ma tylko dwie formy określania liczby mnogiej i udostępniać te same zasady formularza, czyli że Kardynalność jednej jest mapowany na pierwszym mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-164">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="3897f-165">Inne Kardynalność jest mapowana na drugi mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-165">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="3897f-166">Nie wszystkie języki mają te same zasady stosowania.</span><span class="sxs-lookup"><span data-stu-id="3897f-166">Not all languages share the same rules.</span></span> <span data-ttu-id="3897f-167">Jest to zilustrowane z czeski języka, który ma trzy w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-167">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="3897f-168">Utwórz `cs.po` plików w następujący sposób, zwracając uwagę, jak określania liczby mnogiej wymaga trzech różnych tłumaczenia:</span><span class="sxs-lookup"><span data-stu-id="3897f-168">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="3897f-169">Aby zaakceptować lokalizacje czeski, Dodaj `"cs"` do listy obsługiwanych kultur w `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="3897f-169">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

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

<span data-ttu-id="3897f-170">Edytuj *Views/Home/About.cshtml* pliku do renderowania zlokalizowanego, liczbie mnogiej ciągów dla kilku cardinalities:</span><span class="sxs-lookup"><span data-stu-id="3897f-170">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="3897f-171">**Uwaga:** w scenariuszu rzeczywistych zmiennej będzie służyć do reprezentowania licznika.</span><span class="sxs-lookup"><span data-stu-id="3897f-171">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="3897f-172">W tym miejscu możemy Powtórz ten sam kod z trzech różnych wartości do udostępnienia bardzo konkretnym przypadku.</span><span class="sxs-lookup"><span data-stu-id="3897f-172">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="3897f-173">Po przełączeniu kultury, zostanie wyświetlony poniżej:</span><span class="sxs-lookup"><span data-stu-id="3897f-173">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="3897f-174">Dla `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="3897f-174">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="3897f-175">Dla `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="3897f-175">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="3897f-176">Dla `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="3897f-176">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="3897f-177">Należy pamiętać, że dla kultury czeski, trzy tłumaczenia są różne.</span><span class="sxs-lookup"><span data-stu-id="3897f-177">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="3897f-178">Kultur francuski i angielski udostępnianie tego samego konstrukcji dla dwóch ostatnich przetłumaczone ciągi.</span><span class="sxs-lookup"><span data-stu-id="3897f-178">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="3897f-179">Zaawansowane zadania</span><span class="sxs-lookup"><span data-stu-id="3897f-179">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="3897f-180">Ciągi kontekstem</span><span class="sxs-lookup"><span data-stu-id="3897f-180">Contextualizing strings</span></span>

<span data-ttu-id="3897f-181">Aplikacje często zawierają ciągi do tłumaczenia w kilku miejscach.</span><span class="sxs-lookup"><span data-stu-id="3897f-181">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="3897f-182">Te same parametry mogą mieć różnych tłumaczenia w określonych lokalizacjach w aplikacji (widokami Razor lub klasa plików).</span><span class="sxs-lookup"><span data-stu-id="3897f-182">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="3897f-183">Plik PO obsługuje pojęcie kontekstem pliku, który może służyć do klasyfikowania ciąg reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="3897f-183">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="3897f-184">Przy użyciu kontekstu pliku, ciąg można przekonwertować różnie w zależności od kontekstu pliku (lub brak kontekstu pliku).</span><span class="sxs-lookup"><span data-stu-id="3897f-184">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="3897f-185">Usługi lokalizacji PO Użyj nazwy klasy pełną lub widok, który jest używany podczas tłumaczenia ciąg.</span><span class="sxs-lookup"><span data-stu-id="3897f-185">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="3897f-186">Jest to osiągane przez ustawienie wartości na `msgctxt` wpisu.</span><span class="sxs-lookup"><span data-stu-id="3897f-186">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="3897f-187">Należy wziąć pod uwagę pomocnicza dodanie do poprzedniego *fr.po* przykład.</span><span class="sxs-lookup"><span data-stu-id="3897f-187">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="3897f-188">W lokalizacji widoku Razor *Views/Home/About.cshtml* można zdefiniować jako kontekst plików przez ustawienie zarezerwowanego `msgctxt` wartość wpisu:</span><span class="sxs-lookup"><span data-stu-id="3897f-188">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="3897f-189">Z `msgctxt` tak ustawiona, tłumaczenie tekstu występuje podczas nawigowania do `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3897f-189">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="3897f-190">Tłumaczenie nie występuje podczas nawigowania do `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3897f-190">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="3897f-191">Nie określonego wpisu nie odpowiada z kontekstem danego pliku, mechanizm rezerwowy Orchard Core szuka odpowiedniego pliku PO bez kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3897f-191">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="3897f-192">Zakładając, że nie jest zdefiniowany dla kontekstu określonego pliku *Views/Home/Contact.cshtml*, nawigacyjnego do `/Home/Contact?culture=fr-FR` ładuje plik PO, takich jak:</span><span class="sxs-lookup"><span data-stu-id="3897f-192">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="3897f-193">Zmiana lokalizacji plików PO</span><span class="sxs-lookup"><span data-stu-id="3897f-193">Changing the location of PO files</span></span>

<span data-ttu-id="3897f-194">Domyślna lokalizacja plików PO można zmienić w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3897f-194">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="3897f-195">W tym przykładzie plikach Arządzania są ładowane z *lokalizacja* folderu.</span><span class="sxs-lookup"><span data-stu-id="3897f-195">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="3897f-196">Wdrażanie niestandardowej logiki do znajdowania lokalizacji plików</span><span class="sxs-lookup"><span data-stu-id="3897f-196">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="3897f-197">Gdy wymagany jest bardziej złożonej logice można znaleźć w plikach Arządzania `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfejsu może być wdrożone i zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="3897f-197">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="3897f-198">Jest to przydatne, gdy PO pliki mogą być przechowywane w różnych lokalizacjach, lub jeśli pliki znajdują się w hierarchii folderów.</span><span class="sxs-lookup"><span data-stu-id="3897f-198">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="3897f-199">Przy użyciu języka różne domyślne pluralized</span><span class="sxs-lookup"><span data-stu-id="3897f-199">Using a different default pluralized language</span></span>

<span data-ttu-id="3897f-200">Ten pakiet zawiera `Plural` — metoda rozszerzenia, które są specyficzne dla dwóch w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="3897f-200">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="3897f-201">W przypadku języków wymagających więcej w liczbie mnogiej create — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3897f-201">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="3897f-202">Przy użyciu metody rozszerzenia, nie trzeba udostępnić plik dowolnej lokalizacji w języku domyślnym &mdash; oryginalnego ciągi są już dostępne bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3897f-202">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="3897f-203">Można używać więcej ogólnych `Plural(int count, string[] pluralForms, params object[] arguments)` przepełnienia, które akceptuje tłumaczeń tablicy ciągów.</span><span class="sxs-lookup"><span data-stu-id="3897f-203">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>