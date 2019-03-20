---
title: Pomocnicy tagów w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, czym są pomocnicy tagów i sposobu ich używania w programie ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 7768dd45bdbe40c16176d57a76823cbb9dd0b91b
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264622"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="f1d7c-103">Pomocnicy tagów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1d7c-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="f1d7c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1d7c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="f1d7c-105">Co to są pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-105">What are Tag Helpers</span></span>

<span data-ttu-id="f1d7c-106">Pomocnicy tagów włączyć kodu po stronie serwera wziąć udział w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="f1d7c-107">Na przykład wbudowane `ImageTagHelper` można dołączyć numer wersji do nazwy obrazu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="f1d7c-108">Zmianie obrazu serwera generuje unikatowy nową wersję obrazu, dzięki czemu klienci są gwarantowane można pobrać bieżącego obrazu (zamiast przestarzałych obraz pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="f1d7c-109">Istnieje wiele wbudowanych pomocników tagów dla typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakiety więcej — i jeszcze bardziej dostępne w publicznych repozytoriach GitHub oraz jak NuGet.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="f1d7c-110">Pomocnicy tagów są tworzone w języku C#, a ich celem są elementy HTML na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="f1d7c-111">Na przykład wbudowane `LabelTagHelper` mogą kierować HTML `<label>` elementu po `LabelTagHelper` atrybuty są stosowane.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="f1d7c-112">Jeśli znasz [pomocników HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), pomocników tagów zmniejszyć jawne przejścia pomiędzy HTML a C# w widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="f1d7c-113">W wielu przypadkach pomocników HTML zapewnić alternatywne podejście do określonych Pomocnik tagu, ale ważne jest, aby rozpoznać, czy pomocników tagów nie zastąpić pomocników HTML i nie jest pomocnika tagów dla każdego pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="f1d7c-114">[W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśnia różnice bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="f1d7c-115">Podaj pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-115">What Tag Helpers provide</span></span>

<span data-ttu-id="f1d7c-116">**Środowisko programistyczne przyjaznego dla HTML** wygląda w przeważającej części HTML standardowych znaczników Razor za pomocą pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="f1d7c-117">Projektanci frontonu conversant z HTML/CSS/JavaScript edytować Razor bez uczenia składnia Razor języka C#.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="f1d7c-118">**Bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor** to sharp natomiast aby pomocników HTML, poprzednie podejście do tworzenia po stronie serwera, z kodu znaczników w widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="f1d7c-119">[W porównaniu do pomocników HTML pomocników tagów](#tag-helpers-compared-to-html-helpers) wyjaśnia różnice bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="f1d7c-120">[Obsługa funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) wyjaśnia środowiska funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="f1d7c-121">Nawet deweloperzy używający składni Razor C# są bardziej produktywne, używanie pomocników tagów niż pisanie znaczników języka C# Razor.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="f1d7c-122">**Sposobem, aby zapewnić bardziej efektywne i może wygenerować bardziej niezawodne, niezawodne i kodu łatwego w utrzymaniu, korzystając z informacji dostępna tylko na serwerze** na przykład w przeszłości mantra na temat aktualizowania obrazów była zmiana nazwy obrazu po zmianie obraz.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="f1d7c-123">Obrazy, które powinny być agresywnie buforowane ze względu na wydajność i chyba że zmieniasz nazwę obrazu, istnieje ryzyko klientom pobieranie starych kopii.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="f1d7c-124">W przeszłości po obraz był edytowany, nazwa ma zostać zmieniony, a każde odwołanie do obrazu w aplikacji sieci web nie trzeba aktualizować.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="f1d7c-125">Nie tylko jest to bardzo pracy o znacznym wykorzystaniu, jest również podatne (można pominąć odwołanie, przypadkowo wprowadzić nieprawidłowy ciąg znaków, itp.) Wbudowane `ImageTagHelper` można to zrobić dla Ciebie automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="f1d7c-126">`ImageTagHelper` Można dołączyć wersji liczb do nazwy obrazu dzięki zmianie obrazu serwera automatycznie generuje unikatowy nową wersję obrazu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="f1d7c-127">Klienci są gwarantowane można pobrać bieżącego obrazu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="f1d7c-128">Ta oszczędności niezawodności i pracy dostępny zasadniczo bezpłatnie przy użyciu `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="f1d7c-129">Pomocnicy tagów największą liczbą wbudowanych docelową standardowych elementów kodu HTML i udostępniania atrybutów po stronie serwera dla elementu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="f1d7c-130">Na przykład `<input>` element używany w wielu widoków w *widoków/konto* folder zawiera `asp-for` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="f1d7c-131">Ten atrybut wyodrębnia nazwę właściwości określonego modelu w postaci HTML.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="f1d7c-132">Należy wziąć pod uwagę widoku Razor z modelem następujące:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-132">Consider a Razor view with the following model:</span></span>

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

<span data-ttu-id="f1d7c-133">Następujący kod Razor:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="f1d7c-134">Generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="f1d7c-135">`asp-for` Atrybutu jest udostępniana przez `For` właściwość [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="f1d7c-136">Zobacz [pomocników tagów Autor](xref:mvc/views/tag-helpers/authoring) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="f1d7c-137">Pomocnik tagu zakresu zarządzania</span><span class="sxs-lookup"><span data-stu-id="f1d7c-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="f1d7c-138">Zakres pomocników tagów jest kontrolowany przy użyciu kombinacji `@addTagHelper`, `@removeTagHelper`i "!" znak zrezygnować.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="f1d7c-139">`@addTagHelper` udostępnia pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="f1d7c-140">Jeśli utworzysz nową aplikację sieci web platformy ASP.NET Core o nazwie *AuthoringTagHelpers*, następujące *Views/_ViewImports.cshtml* plik zostanie dodany do projektu:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers*, the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="f1d7c-141">`@addTagHelper` Dyrektywy udostępnia pomocników tagów w widoku.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="f1d7c-142">W tym przypadku jest plik widoku *Pages/_ViewImports.cshtml*, która domyślnie jest dziedziczona przez wszystkie pliki w *stron* folderze i jego podfolderach; pomocników tagów udostępnianie.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-142">In this case, the view file is *Pages/_ViewImports.cshtml*, which by default is inherited by all files in the *Pages* folder and subfolders; making Tag Helpers available.</span></span> <span data-ttu-id="f1d7c-143">Powyższy kod używa składni symboli wieloznacznych ("\*") można określić, że wszystkie pomocników tagów w określonym zestawie (*Microsoft.AspNetCore.Mvc.TagHelpers*) będą dostępne dla wszystkich plików widoku w *widoków* katalog lub podkatalog.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or subdirectory.</span></span> <span data-ttu-id="f1d7c-144">Pierwszy parametr po `@addTagHelper` określa pomocnicy tagów, aby załadować (używamy "\*" dla pomocników tagów), a drugi parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Określa zestaw zawierający pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="f1d7c-145">*Microsoft.AspNetCore.Mvc.TagHelpers* jest zestawem dla wbudowanych pomocników tagów dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="f1d7c-146">Aby udostępnić wszystkie pomocników tagów w tym projekcie (tworzy zestaw o nazwie *AuthoringTagHelpers*), należy użyć następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="f1d7c-147">Jeśli projekt zawiera `EmailTagHelper` przy użyciu domyślnej przestrzeni nazw (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), możesz podać w pełni kwalifikowaną nazwę (FQN) pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="f1d7c-148">Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie nazwę zestawu (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="f1d7c-149">Większość programistów wolą używać "\*" Składnia symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="f1d7c-150">Składnia symboli wieloznacznych pozwala wstawić symbol wieloznaczny "\*" jako sufiks w FQN.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="f1d7c-151">Na przykład, następujące dyrektywy zostanie wyświetlone `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="f1d7c-152">Jak wspomniano wcześniej, dodając `@addTagHelper` dyrektywę *Views/_ViewImports.cshtml* pliku sprawia, że Pomocnik tagu jest dostępny dla wszystkich plików w widoku w *widoków* katalogu i podkatalogach.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and subdirectories.</span></span> <span data-ttu-id="f1d7c-153">Możesz użyć `@addTagHelper` dyrektywy w plikach określonego widoku, aby wyrazić zgodę na udostępnianie Pomocnik tagu tylko tych widoków.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="f1d7c-154">`@removeTagHelper` Usuwa pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="f1d7c-155">`@removeTagHelper` Ma ten sam dwa parametry jako `@addTagHelper`, i usuwa pomocnika tagów, który wcześniej został dodany.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="f1d7c-156">Na przykład `@removeTagHelper` stosowane w przypadku określonego widoku powoduje usunięcie określonego pomocnika tagów z widoku.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="f1d7c-157">Za pomocą `@removeTagHelper` w *Views/Folder/_ViewImports.cshtml* pliku usuwa określony Pomocnik tagu ze wszystkich widoków w *folderu*.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="f1d7c-158">Kontrolowanie zakresu Pomocnik tagu za pomocą *_ViewImports.cshtml* pliku</span><span class="sxs-lookup"><span data-stu-id="f1d7c-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="f1d7c-159">Możesz dodać *_ViewImports.cshtml* dowolny folder widoku oraz widoku aparatu dotyczy dyrektywy z obu tego pliku i *Views/_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1d7c-160">Jeśli została dodana pusta *Views/Home/_ViewImports.cshtml* plik *Home* widoków, może być brak zmian *_ViewImports.cshtml* plik jest dodatku.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="f1d7c-161">Żadnych `@addTagHelper` dyrektywy możesz dodać do *Views/Home/_ViewImports.cshtml* pliku (które nie są w domyślnym *Views/_ViewImports.cshtml* pliku) wystawi tych pomocników tagów do widoków tylko w *Home* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="f1d7c-162">Rezygnacja z poszczególnych elementów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-162">Opting out of individual elements</span></span>

<span data-ttu-id="f1d7c-163">Można wyłączyć pomocnika tagów znakiem rezygnacji Pomocnik tagu na poziomie elementu ("!").</span><span class="sxs-lookup"><span data-stu-id="f1d7c-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="f1d7c-164">Na przykład `Email` sprawdzania poprawności jest wyłączona w `<span>` znakiem rezygnacji Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="f1d7c-165">Pomocnik tagu znak rezygnacji muszą dotyczyć otwierający i zamykający tag.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="f1d7c-166">(Edytor programu Visual Studio automatycznie dodaje znak rezygnacji z tagu zamykającego po dodaniu do otwierający tag).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="f1d7c-167">Po dodaniu znak rezygnacji, elementów i atrybutów Pomocnik tagu już nie są wyświetlane czcionką szczególne.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="f1d7c-168">Za pomocą `@tagHelperPrefix` się jawne użycie pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="f1d7c-169">`@tagHelperPrefix` Dyrektywy pozwala określić ciąg prefiksu tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="f1d7c-170">Na przykład można dodać następujące znaczniki do *Views/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```

<span data-ttu-id="f1d7c-171">Na poniższej ilustracji kodu jest równa prefiks Pomocnik tagu `th:`, więc tylko te elementy, które są przy użyciu prefiksu `th:` obsługuje pomocników tagów (włączone Pomocnik tagu elementy mają szczególne czcionki).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="f1d7c-172">`<label>` i `<input>` elementy mają prefiks Pomocnik tagu i obsługują Pomocnik tagu, podczas `<span>` nie elementu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![obraz](intro/_static/thp.png)

<span data-ttu-id="f1d7c-174">Te same reguły hierarchii, które są stosowane do `@addTagHelper` dotyczą również `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="self-closing-tag-helpers"></a><span data-ttu-id="f1d7c-175">Samozamykającego pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-175">Self-closing Tag Helpers</span></span>

<span data-ttu-id="f1d7c-176">Wiele pomocników tagów nie można użyć jako własny taga zamykającego.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-176">Many Tag Helpers can't be used as self-closing tags.</span></span> <span data-ttu-id="f1d7c-177">Niektóre pomocników tagów są przeznaczone do można samozamykający tagów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-177">Some Tag Helpers are designed to be self-closing tags.</span></span> <span data-ttu-id="f1d7c-178">Przy użyciu Pomocnika tagów, który nie został zaprojektowany jako samozamykającego pomija wyniku renderowania.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-178">Using a Tag Helper that was not designed to be self-closing suppresses the rendered output.</span></span> <span data-ttu-id="f1d7c-179">Powoduje samozamykający pomocnika tagów w wyniku renderowania tagu samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-179">Self-closing a Tag Helper results in a self-closing tag in the rendered output.</span></span> <span data-ttu-id="f1d7c-180">Aby uzyskać więcej informacji, zobacz [tej uwagi](xref:mvc/views/tag-helpers/authoring#self-closing) w [tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-180">For more information, see [this note](xref:mvc/views/tag-helpers/authoring#self-closing) in [Authoring Tag Helpers](xref:mvc/views/tag-helpers/authoring).</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="f1d7c-181">Obsługa funkcji IntelliSense dla pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-181">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="f1d7c-182">Po utworzeniu nowej aplikacji sieci web platformy ASP.NET Core w programie Visual Studio dodaje pakietu NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="f1d7c-182">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="f1d7c-183">To jest pakiet, który dodaje narzędzi pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-183">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="f1d7c-184">Rozważyć napisanie kodu HTML `<label>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-184">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="f1d7c-185">Zaraz po wprowadzeniu `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-185">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obraz](intro/_static/label.png)

<span data-ttu-id="f1d7c-187">Nie tylko pozwala to uzyskać pomocy HTML, ale ikonę ("@" symbol with "<>" pod nim).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-187">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![obraz](intro/_static/tagSym.png)

<span data-ttu-id="f1d7c-189">identyfikuje element jako objęte pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-189">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="f1d7c-190">Czyste elementy HTML (takie jak `fieldset`) wyświetlana ikona "<>".</span><span class="sxs-lookup"><span data-stu-id="f1d7c-190">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="f1d7c-191">Czystym kodem HTML `<label>` tag Wyświetla tagu HTML (przy użyciu programu Visual Studio motyw domyślny) brązowy czcionki, atrybuty w kolorze czerwonym, i atrybutu wartości w kolorze niebieskim.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-191">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![obraz](intro/_static/LableHtmlTag.png)

<span data-ttu-id="f1d7c-193">Po wprowadzeniu `<label`, funkcja IntelliSense wyświetla dostępne atrybuty HTML/CSS i atrybuty nakierowane Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-193">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![obraz](intro/_static/labelattr.png)

<span data-ttu-id="f1d7c-195">Instrukcji IntelliSense umożliwia wprowadzenie klawisz tab Aby wykonać instrukcję, określając wybraną wartość:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-195">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![obraz](intro/_static/stmtcomplete.png)

<span data-ttu-id="f1d7c-197">Zaraz po wprowadzeniu atrybut pomocnika tagów zmienić czcionki tagów i atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-197">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="f1d7c-198">Korzystając z domyślne programu Visual Studio "Niebieski" lub "Jasny" motyw kolorów, czcionka jest pogrubiony purpurowy.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-198">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="f1d7c-199">Jeśli używasz "ciemny" czcionka jest pogrubiony zielonomodrym.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-199">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="f1d7c-200">Obrazy w niniejszym dokumencie zostały pobrane przy użyciu domyślnego motywu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-200">The images in this document were taken using the default theme.</span></span>

![obraz](intro/_static/labelaspfor2.png)

<span data-ttu-id="f1d7c-202">Możesz użyć programu Visual Studio *CompleteWord* skrót (Ctrl + spacja jest [domyślne](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) w podwójnym cudzysłowie (""), i jesteś teraz w języku C#, tak samo, jak będzie klasy C#.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-202">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="f1d7c-203">IntelliSense wyświetla wszystkie metody i właściwości w modelu strony.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-203">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="f1d7c-204">Metody i właściwości są dostępne, ponieważ typ właściwości to `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-204">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="f1d7c-205">Na poniższej ilustracji I jestem edycji `Register` widoku, więc `RegisterViewModel` jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-205">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![obraz](intro/_static/intellemail.png)

<span data-ttu-id="f1d7c-207">Funkcja IntelliSense wyświetla właściwości i metody dostępne dla modelu, na stronie.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-207">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="f1d7c-208">Bogate środowisko funkcji IntelliSense ułatwia wybór klasy CSS:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-208">The rich IntelliSense environment helps you select the CSS class:</span></span>

![obraz](intro/_static/iclass.png)

![obraz](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="f1d7c-211">Pomocnicy tagów w porównaniu do pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="f1d7c-211">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="f1d7c-212">Pomocnicy tagów, Dołącz do elementów kodu HTML w widokami Razor podczas [pomocników HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) jest wywołana, zgodnie z metody grupową HTML widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-212">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="f1d7c-213">Należy wziąć pod uwagę następujące znaczniki Razor, który tworzy element label kodu HTML z klasy CSS "podpis":</span><span class="sxs-lookup"><span data-stu-id="f1d7c-213">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="f1d7c-214">U (`@`) symbol informuje Razor to początek kodu.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-214">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="f1d7c-215">Następne dwa parametry ("FirstName" i "imię:") są ciągami, więc [IntelliSense](/visualstudio/ide/using-intellisense) nie będzie mogła pomóc.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-215">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="f1d7c-216">Ostatni argument:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-216">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="f1d7c-217">Anonimowy obiekt służy do reprezentowania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-217">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="f1d7c-218">Ponieważ `class` jest zastrzeżonym słowem kluczowym w C#, możesz użyć `@` symbolu, aby wymusić C# do interpretacji `@class=` jako symbol separatora (nazwa właściwości).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-218">Because `class` is a reserved keyword in C#, you use the `@` symbol to force C# to interpret `@class=` as a symbol (property name).</span></span> <span data-ttu-id="f1d7c-219">Do frontonu projektanta (ktoś z HTML/CSS/JavaScript i innych technologii klienta, ale nie znają języka C# i Razor), większość wiersza jest obcy.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-219">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="f1d7c-220">Cały wiersz musi być utworzone przy użyciu pomoc od funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-220">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="f1d7c-221">Za pomocą `LabelTagHelper`, ten sam kod znaczników, może być zapisana jako:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-221">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

<span data-ttu-id="f1d7c-222">Za pomocą wersji pomocnika tagów, zaraz po wprowadzeniu `<l` w edytorze programu Visual Studio, IntelliSense wyświetla zgodnych elementów:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-222">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obraz](intro/_static/label.png)

<span data-ttu-id="f1d7c-224">Technologia IntelliSense pomaga napisać cały wiersz.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-224">IntelliSense helps you write the entire line.</span></span>

<span data-ttu-id="f1d7c-225">Na poniższej ilustracji kodzie przedstawiono część formularza *Views/Account/Register.cshtml* widoku Razor generowany na podstawie szablonu ASP.NET MVC 4.5.x, w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-225">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the ASP.NET 4.5.x MVC template included with Visual Studio.</span></span>

![obraz](intro/_static/regCS.png)

<span data-ttu-id="f1d7c-227">Edytor programu Visual Studio Wyświetla kod C# na szarym tle.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-227">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="f1d7c-228">Na przykład `AntiForgeryToken` pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-228">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="f1d7c-229">zostanie wyświetlona na szarym tle.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-229">is displayed with a grey background.</span></span> <span data-ttu-id="f1d7c-230">Większość kodu znaczników w widoku rejestr jest C#.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-230">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="f1d7c-231">Dla porównania, równoważne podejście, używanie pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-231">Compare that to the equivalent approach using Tag Helpers:</span></span>

![obraz](intro/_static/regTH.png)

<span data-ttu-id="f1d7c-233">Znaczniki jest znacznie bardziej przejrzyste i łatwiejsze do odczytania, edytowanie i obsługa niż metody pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-233">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="f1d7c-234">Kod C# jest ograniczona do minimum, który serwer musi wiedzieć o.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-234">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="f1d7c-235">Edytor programu Visual Studio Wyświetla znaczników objęte pomocnika tagów w szczególne czcionki.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-235">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="f1d7c-236">Należy wziąć pod uwagę *E-mail* grupy:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-236">Consider the *Email* group:</span></span>

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="f1d7c-237">Każdego z atrybutów "asp-" ma wartość "Email", ale "Email" nie jest ciąg.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-237">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="f1d7c-238">W tym kontekście, "Email" jest C# wyrażenie właściwości modelu dla `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-238">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="f1d7c-239">Edytor programu Visual Studio ułatwia pisanie **wszystkich** z kodu znaczników w ujęciu Pomocnik tagu formularza Zarejestruj się, gdy program Visual Studio udostępnia pomoc dla większości kod w metody pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-239">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="f1d7c-240">[Obsługa funkcji IntelliSense dla pomocników tagów](#intellisense-support-for-tag-helpers) przechodzi do szczegółów na temat pracy z pomocników tagów w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-240">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="f1d7c-241">Pomocnicy tagów w porównaniu do formantów serwera sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1d7c-241">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="f1d7c-242">Pomocnicy tagów nie jest własnością elementu, z którymi są one związane. po prostu uczestniczą w czasie renderowania elementu, a zawartość.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-242">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="f1d7c-243">ASP.NET [kontrolki serwera sieci Web](https://msdn.microsoft.com/library/7698y1f0.aspx) zadeklarowana i wywoływane na stronie.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-243">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="f1d7c-244">[Sieci Web formanty serwera](https://msdn.microsoft.com/library/zsyt68f1.aspx) mają nietrywialnymi cykl życia, które mogą utrudnić opracowywania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-244">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="f1d7c-245">Formanty serwera sieci Web umożliwiają dodawanie funkcji do elementów modelu DOM (Document Object) klienta za pomocą formantu klienta.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-245">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="f1d7c-246">Pomocnicy tagów nie ma żadnych modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-246">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="f1d7c-247">Formanty serwera sieci Web obejmują wykrywania automatycznego przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-247">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="f1d7c-248">Pomocnicy tagów nie znają przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-248">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="f1d7c-249">Wiele pomocników tagów może działać na tym samym elemencie (zobacz [Pomocnik tagu unikanie konfliktów](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) podczas zwykle nie można utworzyć formanty serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-249">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="f1d7c-250">Pomocnicy tagów można zmodyfikować tagów i zawartości elementów kodu HTML, które są do zakresu, ale nie bezpośrednio modyfikować dowolne inne na stronie.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-250">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="f1d7c-251">Formanty serwera sieci Web mają szerszym zakresie i mogą wykonywać akcje, które mają wpływ na inne części strony; Włączanie wystąpienie niezamierzonych skutków ubocznych.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-251">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="f1d7c-252">Formanty serwera sieci Web umożliwia konwertowanie ciągów na obiektach konwerterów typów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-252">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="f1d7c-253">Dzięki pomocnicy tagów możesz podjąć natywnie w języku C#, dzięki czemu nie trzeba wpisywać konwersji.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-253">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="f1d7c-254">Serwer sieci Web steruje użyciem [System.ComponentModel](/dotnet/api/system.componentmodel) Aby zaimplementować to zachowanie w czasie wykonywania oraz w czasie projektowania składników i formantów.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-254">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="f1d7c-255">`System.ComponentModel` zawiera klasy podstawowe i interfejsy do implementacji atrybutów i typy konwerterów, powiązanie z danymi źródeł i licencjonowanie składników.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-255">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="f1d7c-256">Natomiast, aby pomocnicy tagów, które zwykle dziedziczyć `TagHelper`i `TagHelper` klasy bazowej ujawnia tylko dwie metody, `Process` i `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="f1d7c-256">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="f1d7c-257">Dostosowywanie czcionek element pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-257">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="f1d7c-258">Można dostosować, czcionki i kolorowanie z **narzędzia** > **opcje** > **środowiska** > **czcionek Kolory i**:</span><span class="sxs-lookup"><span data-stu-id="f1d7c-258">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![obraz](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="f1d7c-260">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1d7c-260">Additional resources</span></span>

* [<span data-ttu-id="f1d7c-261">Tworzenie pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="f1d7c-261">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="f1d7c-262">Praca z formularzami</span><span class="sxs-lookup"><span data-stu-id="f1d7c-262">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="f1d7c-263">[TagHelperSamples w serwisie GitHub](https://github.com/dpaquette/TagHelperSamples) zawiera przykłady Pomocnik tagu do pracy z [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f1d7c-263">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>
