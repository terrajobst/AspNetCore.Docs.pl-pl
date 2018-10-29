---
title: Konfigurowanie lokalizacji obiektu przenośnego w programie ASP.NET Core
author: sebastienros
description: Ten artykuł wprowadza plików przenośnych obiektów i opisano kroki używania ich w aplikacji ASP.NET Core przy użyciu Orchard Core framework.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: c9f892f5a886d7167b4705595ed2277279495201
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207631"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a><span data-ttu-id="e5681-103">Konfigurowanie lokalizacji obiektu przenośnego w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5681-103">Configure portable object localization in ASP.NET Core</span></span>

<span data-ttu-id="e5681-104">Przez [OK Sébastien](https://github.com/sebastienros) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e5681-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e5681-105">W tym artykule opisano kolejne kroki dotyczące korzystania z plików obiektu przenośnego (PO) w aplikacji platformy ASP.NET Core za pomocą [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="e5681-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="e5681-106">**Uwaga:** Orchard Core nie jest produktem firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e5681-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="e5681-107">W związku z tym Microsoft nie obsługuje tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e5681-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="e5681-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e5681-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="e5681-109">Co to jest plik zamówienia zakupu?</span><span class="sxs-lookup"><span data-stu-id="e5681-109">What is a PO file?</span></span>

<span data-ttu-id="e5681-110">Pliki PO są dystrybuowane jako pliki tekstowe, zawierający przetłumaczone ciągi dla danego języka.</span><span class="sxs-lookup"><span data-stu-id="e5681-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="e5681-111">Niektóre zalety zamiast plików PO *resx* pliki obejmują:</span><span class="sxs-lookup"><span data-stu-id="e5681-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="e5681-112">Pliki PO obsługują pluralizacja; *resx* pliki nie obsługują pluralizacja.</span><span class="sxs-lookup"><span data-stu-id="e5681-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="e5681-113">Pliki zamówienia zakupu nie są kompilowane, takich jak *resx* plików.</span><span class="sxs-lookup"><span data-stu-id="e5681-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="e5681-114">W efekcie wyspecjalizowane narzędzia i kompilacji kroki nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="e5681-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="e5681-115">Pliki PO działać dobrze współpracy online narzędzia do edycji.</span><span class="sxs-lookup"><span data-stu-id="e5681-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="e5681-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="e5681-116">Example</span></span>

<span data-ttu-id="e5681-117">Poniżej przedstawiono przykładowy plik PO zawierających Translacja dwa ciągi w języku francuskim, łącznie z jego liczba mnoga:</span><span class="sxs-lookup"><span data-stu-id="e5681-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="e5681-118">*FR.po*</span><span class="sxs-lookup"><span data-stu-id="e5681-118">*fr.po*</span></span>

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

<span data-ttu-id="e5681-119">W tym przykładzie używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="e5681-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="e5681-120">`#:`: Komentarz wskazujący kontekście ciąg, który ma zostać poddany translacji.</span><span class="sxs-lookup"><span data-stu-id="e5681-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="e5681-121">Te same parametry mogą być tłumaczone różnie w zależności od tego, gdzie jest on używany.</span><span class="sxs-lookup"><span data-stu-id="e5681-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="e5681-122">`msgid`Nieprzetłumaczonych ciągów.</span><span class="sxs-lookup"><span data-stu-id="e5681-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="e5681-123">`msgstr`: Przetłumaczonego ciągu.</span><span class="sxs-lookup"><span data-stu-id="e5681-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="e5681-124">W przypadku obsługi pluralizacja można zdefiniować więcej wpisów.</span><span class="sxs-lookup"><span data-stu-id="e5681-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="e5681-125">`msgid_plural`Ciąg, liczba mnoga nieprzetłumaczony.</span><span class="sxs-lookup"><span data-stu-id="e5681-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="e5681-126">`msgstr[0]`: Przetłumaczonego ciągu w przypadku 0.</span><span class="sxs-lookup"><span data-stu-id="e5681-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="e5681-127">`msgstr[N]`: Przetłumaczonego ciągu przypadków N.</span><span class="sxs-lookup"><span data-stu-id="e5681-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="e5681-128">Specyfikacja pliku zamówienia zakupu można znaleźć [tutaj](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="e5681-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="e5681-129">Konfigurowanie obsługi plików zamówienia zakupu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5681-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="e5681-130">Ten przykład jest oparty na aplikacji ASP.NET Core MVC generowany na podstawie szablonu projektu programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e5681-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="e5681-131">Odwołanie do pakietu</span><span class="sxs-lookup"><span data-stu-id="e5681-131">Referencing the package</span></span>

<span data-ttu-id="e5681-132">Dodaj odwołanie do `OrchardCore.Localization.Core` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5681-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="e5681-133">Jest ona dostępna na [MyGet](https://www.myget.org/) następujące źródła pakietu: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="e5681-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="e5681-134">*.Csproj* plik zawiera teraz linię podobne do następujących (numer wersji może być inna):</span><span class="sxs-lookup"><span data-stu-id="e5681-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="e5681-135">Rejestrowanie usługi</span><span class="sxs-lookup"><span data-stu-id="e5681-135">Registering the service</span></span>

<span data-ttu-id="e5681-136">Dodaj wymagane usługi, aby `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e5681-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="e5681-137">Dodaj wymagane oprogramowanie pośredniczące `Configure` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e5681-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="e5681-138">Dodaj następujący kod do wybranego widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="e5681-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="e5681-139">*About.cshtml* jest używany w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e5681-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="e5681-140">`IViewLocalizer` Wystąpienie jest wprowadzony i umożliwia tłumaczenie tekstu "Hello world!".</span><span class="sxs-lookup"><span data-stu-id="e5681-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="e5681-141">Tworzenie pliku zamówienia zakupu</span><span class="sxs-lookup"><span data-stu-id="e5681-141">Creating a PO file</span></span>

<span data-ttu-id="e5681-142">Utwórz plik o nazwie  *<culture code>je* w folderze głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5681-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="e5681-143">W tym przykładzie nazwa pliku jest *fr.po* ponieważ jest używana w języku francuskim:</span><span class="sxs-lookup"><span data-stu-id="e5681-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="e5681-144">Ten plik przechowuje ciągu do tłumaczenia i ciąg przetłumaczone francuski.</span><span class="sxs-lookup"><span data-stu-id="e5681-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="e5681-145">Tłumaczenia przywrócić ich nadrzędnego kultury, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="e5681-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="e5681-146">W tym przykładzie *fr.po* plik jest używany, jeśli jest żądaną kulturę `fr-FR` lub `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="e5681-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="e5681-147">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5681-147">Testing the application</span></span>

<span data-ttu-id="e5681-148">Uruchom aplikację i przejdź do adresu URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="e5681-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="e5681-149">Tekst **Witaj świecie!**</span><span class="sxs-lookup"><span data-stu-id="e5681-149">The text **Hello world!**</span></span> <span data-ttu-id="e5681-150">zostanie wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="e5681-150">is displayed.</span></span>

<span data-ttu-id="e5681-151">Przejdź do adresu URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="e5681-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="e5681-152">Tekst **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="e5681-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="e5681-153">zostanie wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="e5681-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="e5681-154">Pluralizacja</span><span class="sxs-lookup"><span data-stu-id="e5681-154">Pluralization</span></span>

<span data-ttu-id="e5681-155">Pliki PO obsługuje pluralizacja formularzy, co jest przydatne, gdy ten sam ciąg musi być tłumaczony inaczej w zależności od kardynalności.</span><span class="sxs-lookup"><span data-stu-id="e5681-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="e5681-156">To zadanie wykonywane skomplikowane faktem, że każdy język definiuje reguły niestandardowe, aby wybrać, które parametry do użycia oparte na kardynalność.</span><span class="sxs-lookup"><span data-stu-id="e5681-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="e5681-157">Pakiet lokalizacji witryny systemu Orchard zapewnia interfejs API do wywołania tych różnych postaci mnogiej automatycznie.</span><span class="sxs-lookup"><span data-stu-id="e5681-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="e5681-158">Tworzenie pluralizacja plików zamówienia zakupu</span><span class="sxs-lookup"><span data-stu-id="e5681-158">Creating pluralization PO files</span></span>

<span data-ttu-id="e5681-159">Dodaj następującą zawartość do wymienionych wcześniej *fr.po* pliku:</span><span class="sxs-lookup"><span data-stu-id="e5681-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="e5681-160">Zobacz [co to jest plik PO?](#what-is-a-po-file) wyjaśnienie co reprezentuje każdy wpis, w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e5681-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="e5681-161">Dodawanie języka przy użyciu różnych pluralizacja formularzy</span><span class="sxs-lookup"><span data-stu-id="e5681-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="e5681-162">Ciągi języka angielskiego i francuskiego zostały użyte w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e5681-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="e5681-163">Języka angielskiego i francuskiego ma tylko dwie formy pluralizacja i udostępniać te same zasady formularza, czyli czy Kardynalność jednego jest mapowane na pierwszym mnogiej.</span><span class="sxs-lookup"><span data-stu-id="e5681-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="e5681-164">Inne Kardynalność jest zamapowana na drugim mnogiej.</span><span class="sxs-lookup"><span data-stu-id="e5681-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="e5681-165">Nie wszystkie języki udostępniać te same reguły.</span><span class="sxs-lookup"><span data-stu-id="e5681-165">Not all languages share the same rules.</span></span> <span data-ttu-id="e5681-166">Jest to zilustrowane w języku Czeskiej, która ma trzy w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="e5681-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="e5681-167">Utwórz `cs.po` pliku w następujący sposób i zwróć uwagę, jak pluralizacja wymaga trzech różnych tłumaczeń:</span><span class="sxs-lookup"><span data-stu-id="e5681-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="e5681-168">Aby zaakceptować lokalizacje Czeskiej, Dodaj `"cs"` do listy obsługiwanych kultur w `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="e5681-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

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

<span data-ttu-id="e5681-169">Edytuj *Views/Home/About.cshtml* pliku do renderowania ciągi zlokalizowane, plural dla kilku kardynalności:</span><span class="sxs-lookup"><span data-stu-id="e5681-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="e5681-170">**Uwaga:** w rzeczywistym scenariuszu, zmienna będzie używana do reprezentowania liczby.</span><span class="sxs-lookup"><span data-stu-id="e5681-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="e5681-171">W tym miejscu możemy Powtórz ten sam kod za pomocą trzy różne wartości, aby ujawnić bardzo szczegółowych przypadków.</span><span class="sxs-lookup"><span data-stu-id="e5681-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="e5681-172">Po przełączeniu kultur, zostaną wyświetlone następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e5681-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="e5681-173">Dla programów `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="e5681-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="e5681-174">Dla programów `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="e5681-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="e5681-175">Dla programów `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="e5681-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="e5681-176">Należy pamiętać, że dla kultury Czeskiej, trzy tłumaczenia są różne.</span><span class="sxs-lookup"><span data-stu-id="e5681-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="e5681-177">Kultury francuski i angielski udostępnianie tych samych konstrukcji dla dwóch ostatnich przetłumaczone ciągi.</span><span class="sxs-lookup"><span data-stu-id="e5681-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="e5681-178">Zaawansowane zadania</span><span class="sxs-lookup"><span data-stu-id="e5681-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="e5681-179">Ciągi uczestnika</span><span class="sxs-lookup"><span data-stu-id="e5681-179">Contextualizing strings</span></span>

<span data-ttu-id="e5681-180">Aplikacje często zawierają ciągi, które można przetłumaczyć w kilku miejscach.</span><span class="sxs-lookup"><span data-stu-id="e5681-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="e5681-181">Te same parametry mogą mieć różnych tłumaczeń w określonych lokalizacjach w obrębie aplikacji (widokami Razor lub plików klasy).</span><span class="sxs-lookup"><span data-stu-id="e5681-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="e5681-182">Plik PO obsługuje pojęcie kontekstu pliku, który może służyć do kategoryzowania ciągu reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="e5681-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="e5681-183">Za pomocą kontekstu pliku, ciągu mogą być tłumaczone różnie, w zależności od kontekstu pliku (lub brak kontekstu pliku).</span><span class="sxs-lookup"><span data-stu-id="e5681-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="e5681-184">Usług lokalizacyjnych zamówienia zakupu, użyj nazwy klasy pełną lub widok, który jest używany podczas tłumaczenia ciąg.</span><span class="sxs-lookup"><span data-stu-id="e5681-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="e5681-185">Jest to osiągane przez ustawienie wartości na `msgctxt` wpisu.</span><span class="sxs-lookup"><span data-stu-id="e5681-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="e5681-186">Należy wziąć pod uwagę pomocnicza dodanie do poprzedniego *fr.po* przykład.</span><span class="sxs-lookup"><span data-stu-id="e5681-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="e5681-187">Widok Razor znajdujący się w *Views/Home/About.cshtml* mogą być definiowane jako kontekstu pliku przez ustawienie zarezerwowanego `msgctxt` wartość wpisu:</span><span class="sxs-lookup"><span data-stu-id="e5681-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="e5681-188">Za pomocą `msgctxt` ustawione jako takie, tłumaczenie tekstu występuje podczas przechodzenia do `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="e5681-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="e5681-189">Tłumaczenie nie występują w przypadku przechodzenia do `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="e5681-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="e5681-190">Nie określonego wpisu jest dopasowywany w kontekście danego pliku, mechanizm rezerwowy Orchard Core szuka pliku odpowiednią zamówienia zakupu bez kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e5681-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="e5681-191">Zakładając, że nie jest zdefiniowany dla kontekstu określonego pliku *Views/Home/Contact.cshtml*, po do `/Home/Contact?culture=fr-FR` ładuje plik zamówienia zakupu, takich jak:</span><span class="sxs-lookup"><span data-stu-id="e5681-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="e5681-192">Zmiana lokalizacji plików zamówienia zakupu</span><span class="sxs-lookup"><span data-stu-id="e5681-192">Changing the location of PO files</span></span>

<span data-ttu-id="e5681-193">Można zmienić domyślną lokalizację plików PO w programie `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e5681-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="e5681-194">W tym przykładzie zamówienia zakupu pliki są ładowane z *lokalizacji* folderu.</span><span class="sxs-lookup"><span data-stu-id="e5681-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="e5681-195">Implementowanie logiki niestandardowej do znajdowania lokalizacji plików</span><span class="sxs-lookup"><span data-stu-id="e5681-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="e5681-196">Gdy będzie potrzebny bardziej złożonej logiki, aby zlokalizować pliki PO `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfejsu może być zaimplementowany i zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="e5681-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="e5681-197">Jest to przydatne, gdy pliki PO mogą być przechowywane w różnych lokalizacjach lub gdy pliki znajdują się w hierarchii folderów.</span><span class="sxs-lookup"><span data-stu-id="e5681-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="e5681-198">Za pomocą języka różne domyślne pluralized</span><span class="sxs-lookup"><span data-stu-id="e5681-198">Using a different default pluralized language</span></span>

<span data-ttu-id="e5681-199">Ten pakiet zawiera `Plural` metodę rozszerzenia, które są specyficzne dla dwóch w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="e5681-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="e5681-200">W przypadku języków wymagających więcej w liczbie mnogiej utworzyć metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="e5681-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="e5681-201">Za pomocą metody rozszerzenia, nie należy podać dowolny plik lokalizacji dla domyślnego języka &mdash; oryginalnego ciągi są już dostępne bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="e5681-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="e5681-202">Można użyć więcej ogólny `Plural(int count, string[] pluralForms, params object[] arguments)` przeciążenia, który przyjmuje tablicę ciągów, tłumaczeń.</span><span class="sxs-lookup"><span data-stu-id="e5681-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
