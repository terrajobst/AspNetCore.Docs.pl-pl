---
title: Tworzenie pomocników tagów w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzenie pomocników tagów w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 78e5281d109977e8f41fe1f207254d3016f9c569
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244869"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="74056-103">Tworzenie pomocników tagów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74056-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="74056-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74056-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74056-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74056-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="74056-106">Rozpoczynanie pracy z usługą pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="74056-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="74056-107">Ten samouczek zawiera wprowadzenie do programowania pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="74056-108">[Wprowadzenie do pomocników tagów](intro.md) opisuje korzyści, które dają pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="74056-109">Pomocnik tagu jest każda klasa implementująca `ITagHelper` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="74056-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="74056-110">Jednak podczas tworzenia pomocnika tagów ogólnie klasy wyprowadzonej z `TagHelper`, dlatego zapewnia dostęp do wykonywania `Process` metody.</span><span class="sxs-lookup"><span data-stu-id="74056-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="74056-111">Utwórz nowy projekt ASP.NET Core o nazwie **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="74056-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="74056-112">Nie wymaga uwierzytelniania dla tego projektu.</span><span class="sxs-lookup"><span data-stu-id="74056-112">You won't need authentication for this project.</span></span>

1. <span data-ttu-id="74056-113">Utwórz folder do przechowywania pomocnicy tagów, o nazwie *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="74056-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="74056-114">*TagHelpers* folder jest *nie* wymagane, ale jest to uzasadnione Konwencji.</span><span class="sxs-lookup"><span data-stu-id="74056-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="74056-115">Teraz Rozpocznijmy od pisania pomocników kilka prostych tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="74056-116">Minimalny pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="74056-116">A minimal Tag Helper</span></span>

<span data-ttu-id="74056-117">W tej sekcji możesz zapisanie pomocnika tagów, która aktualizuje tag wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="74056-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="74056-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="74056-118">For example:</span></span>

```html
<email>Support</email>
```

<span data-ttu-id="74056-119">Serwer zastosuje nasze Pomocnik tagu wiadomości e-mail do konwertowania tego znacznika do następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="74056-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="74056-120">Oznacza to, że tag kotwicy temu to łącze w wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="74056-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="74056-121">Można to zrobić, jeśli piszesz aparatu blogu i potrzebny do wysyłania wiadomości e-mail, marketing, pomocy technicznej i inne kontakty wszystkie do tej samej domenie.</span><span class="sxs-lookup"><span data-stu-id="74056-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="74056-122">Dodaj następujący kod `EmailTagHelper` klasy *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * <span data-ttu-id="74056-123">Pomocnicy tagów używać konwencji nazewnictwa, który jest przeznaczony dla elementów główna nazwa klasy (minus *pomocnika tagów* część nazwy klasy).</span><span class="sxs-lookup"><span data-stu-id="74056-123">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="74056-124">W tym przykładzie nazwa głównego **wiadomości E-mail**pomocnika tagów jest *wiadomości e-mail*, więc `<email>` zostaną objęte tagu.</span><span class="sxs-lookup"><span data-stu-id="74056-124">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="74056-125">Konwencja nazewnictwa powinny działać dla większości pomocnicy tagów, później zaprezentuję, jak go zastąpić.</span><span class="sxs-lookup"><span data-stu-id="74056-125">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>

   * <span data-ttu-id="74056-126">`EmailTagHelper` Klasa pochodzi od `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="74056-126">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="74056-127">`TagHelper` Klasa udostępnia metody i właściwości do pisania pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-127">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>

   * <span data-ttu-id="74056-128">Zastąpione `Process` metoda kontroluje Pomocnik tagu działania podczas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="74056-128">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="74056-129">`TagHelper` Klasa udostępnia także asynchroniczna wersja (`ProcessAsync`) z tymi samymi parametrami.</span><span class="sxs-lookup"><span data-stu-id="74056-129">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>

   * <span data-ttu-id="74056-130">Parametr kontekstowy do `Process` (i `ProcessAsync`) zawiera informacje związane z wykonywaniem bieżącego tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="74056-130">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>

   * <span data-ttu-id="74056-131">Parametr wyjściowy do `Process` (i `ProcessAsync`) zawiera element HTML stanowych językiem oryginalne źródło, które są używane do generowania tagu HTML i zawartości.</span><span class="sxs-lookup"><span data-stu-id="74056-131">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>

   * <span data-ttu-id="74056-132">Nasze Nazwa klasy ma sufiks **pomocnika tagów**, czyli *nie* wymagane, ale ma on uznawany za Konwencję najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="74056-132">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="74056-133">Można zadeklarować klasy jako:</span><span class="sxs-lookup"><span data-stu-id="74056-133">You could declare the class as:</span></span>

   ```csharp
   public class Email : TagHelper
   ```

1. <span data-ttu-id="74056-134">Aby `EmailTagHelper` klasy dostępne dla wszystkich naszych widokami Razor, należy dodać `addTagHelper` dyrektywę *Views/_ViewImports.cshtml* pliku: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="74056-134">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>

   <span data-ttu-id="74056-135">Powyższy kod używa składni symbolu wieloznacznego do określania, że wszystkie pomocników tagów w naszym zestawie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="74056-135">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="74056-136">Pierwszy ciąg po `@addTagHelper` określa Pomocnik tagu, można załadować (Użyj "\*" dla pomocników tagów), i drugi ciąg "AuthoringTagHelpers" Określa zestaw Pomocnik tagu.</span><span class="sxs-lookup"><span data-stu-id="74056-136">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="74056-137">Należy również zauważyć, że drugi wiersz wiąże pomocników tagów ASP.NET Core MVC za pomocą składni symboli wieloznacznych (pomocników te zostały omówione w [wprowadzenie do pomocników tagów](intro.md).) Jest `@addTagHelper` dyrektywy, która udostępnia Pomocnik tagu w widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="74056-137">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="74056-138">Alternatywnie można podać w pełni kwalifikowana nazwa (FQN) pomocnika tagów, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="74056-138">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

<span data-ttu-id="74056-139">Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie **nazwy zestawu** (*AuthoringTagHelpers*, nie necessarly `namespace`).</span><span class="sxs-lookup"><span data-stu-id="74056-139">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the **assembly name** (*AuthoringTagHelpers*, not necessarly the `namespace`).</span></span> <span data-ttu-id="74056-140">Większość programistów będą najpierw użyj składni symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="74056-140">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="74056-141">[Wprowadzenie do pomocników tagów](intro.md) określa szczegółowo składni dodawania, usuwania, hierarchii i symboli wieloznacznych pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-141">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>

1. <span data-ttu-id="74056-142">Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* plików za pomocą tych zmian:</span><span class="sxs-lookup"><span data-stu-id="74056-142">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="74056-143">Uruchom aplikację i użyj ulubionej przeglądarce, aby wyświetlić źródło HTML, aby można było sprawdzić tagi wiadomości e-mail są zastępowane znaczników zakotwiczenia (na przykład `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="74056-143">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="74056-144">*Pomocy technicznej* i *marketingu* są renderowane jako linki, ale nie mają `href` atrybutu, aby stały się funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="74056-144">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="74056-145">Naprawimy, w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="74056-145">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="74056-146">SetAttribute i SetContent</span><span class="sxs-lookup"><span data-stu-id="74056-146">SetAttribute and SetContent</span></span>

<span data-ttu-id="74056-147">W tej sekcji dodamy `EmailTagHelper` tak, aby go utworzy tag kotwicy prawidłowy do obsługi poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="74056-147">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="74056-148">Zaktualizujemy go, aby pobierał informacje z widoku Razor (w postaci `mail-to` atrybutu) i używać go podczas generowania zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="74056-148">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="74056-149">Aktualizacja `EmailTagHelper` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-149">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* <span data-ttu-id="74056-150">Pascal — z uwzględnieniem wielkości liter nazwy klasy i właściwości pomocników tagów są tłumaczone na ich [obniżyć przypadek kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="74056-150">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="74056-151">W związku z tym Aby użyć `MailTo` użyjemy atrybutu `<email mail-to="value"/>` równoważne.</span><span class="sxs-lookup"><span data-stu-id="74056-151">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="74056-152">Ostatni wiersz ustawia ukończone zawartości dla naszych Pomocnik tagu minimalny zestaw funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="74056-152">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="74056-153">Wyróżniony wiersz pokazuje składnię na dodawanie atrybutów:</span><span class="sxs-lookup"><span data-stu-id="74056-153">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="74056-154">Takie podejście działa dla atrybutu "href", tak długo, jak obecnie nie istnieje w kolekcji atrybutów.</span><span class="sxs-lookup"><span data-stu-id="74056-154">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="74056-155">Można również użyć `output.Attributes.Add` metodę, aby dodać atrybut pomocnika tagów na końcu kolekcji atrybutów tagu.</span><span class="sxs-lookup"><span data-stu-id="74056-155">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="74056-156">Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* plików za pomocą tych zmian: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="74056-156">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

1. <span data-ttu-id="74056-157">Uruchom aplikację i sprawdzić generuje poprawne linki.</span><span class="sxs-lookup"><span data-stu-id="74056-157">Run the app and verify that it generates the correct links.</span></span>

<a name="self-closing"></a>

   > [!NOTE]
   > <span data-ttu-id="74056-158">Gdyby można zapisać wiadomość e-mail tag samozamykający (`<email mail-to="Rick" />`), końcowych danych wyjściowych będą również samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="74056-158">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="74056-159">Aby włączyć możliwość pisania tag z tagu początkowego (`<email mail-to="Rick">`) musi dekoracji klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-159">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   <span data-ttu-id="74056-160">Za pomocą samozamykającego pomocnika tagów poczty e-mail, dane wyjściowe będą `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="74056-160">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="74056-161">Samozamykającego zakotwiczenia tagi nie są prawidłowe HTML, co nie byłoby dobrze, utwórz ją, ale możesz chcieć utworzyć pomocnika tagów, który jest samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="74056-161">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="74056-162">Pomocnicy tagów Ustaw typ `TagMode` właściwości po przeczytaniu tag.</span><span class="sxs-lookup"><span data-stu-id="74056-162">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>

### <a name="processasync"></a><span data-ttu-id="74056-163">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="74056-163">ProcessAsync</span></span>

<span data-ttu-id="74056-164">W tej sekcji będziemy pisać pomocnika asynchronicznego wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="74056-164">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="74056-165">Zastąp `EmailTagHelper` klasy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-165">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="74056-166">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="74056-166">**Notes:**</span></span>

   * <span data-ttu-id="74056-167">Ta wersja wykorzystuje asynchroniczną `ProcessAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="74056-167">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="74056-168">Asynchroniczną `GetChildContentAsync` zwraca `Task` zawierający `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="74056-168">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="74056-169">Użyj `output` parametru, aby pobrać zawartość elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="74056-169">Use the `output` parameter to get contents of the HTML element.</span></span>

1. <span data-ttu-id="74056-170">Wprowadź następującą zmianę w celu *Views/Home/Contact.cshtml* pliku, dzięki czemu pomocnika tagów można pobrać adresu e-mail docelowej.</span><span class="sxs-lookup"><span data-stu-id="74056-170">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="74056-171">Uruchom aplikację i sprawdzić generuje łącza prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="74056-171">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="74056-172">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="74056-172">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="74056-173">Dodaj następujący kod `BoldTagHelper` klasy *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-173">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * <span data-ttu-id="74056-174">`[HtmlTargetElement]` Atrybutu przekazuje parametr atrybutu, który określa, czy dowolnego elementu HTML zawierającego atrybut HTML o nazwie "bold" będą zgodne, a `Process` uruchomi metoda przesłonięcia w klasie.</span><span class="sxs-lookup"><span data-stu-id="74056-174">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="74056-175">W naszym przykładzie `Process` metoda usuwa atrybut "bold" i otacza zawierającego znaczników przy użyciu `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="74056-175">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>

   * <span data-ttu-id="74056-176">Ponieważ nie chcesz zastąpić istniejący znacznik zawartości, należy napisać otwarcia `<strong>` oznaczyć za pomocą `PreContent.SetHtmlContent` metody i zamknięcie `</strong>` oznaczyć za pomocą `PostContent.SetHtmlContent` metody.</span><span class="sxs-lookup"><span data-stu-id="74056-176">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>

1. <span data-ttu-id="74056-177">Modyfikowanie *About.cshtml* widok ma zawierać `bold` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="74056-177">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="74056-178">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="74056-178">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. <span data-ttu-id="74056-179">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74056-179">Run the app.</span></span> <span data-ttu-id="74056-180">Ulubionej przeglądarce służy do kontroli źródła oraz sprawdź znaczników.</span><span class="sxs-lookup"><span data-stu-id="74056-180">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="74056-181">`[HtmlTargetElement]` Atrybut powyżej jest przeznaczony tylko dla znaczników HTML, który zawiera nazwę "bold".</span><span class="sxs-lookup"><span data-stu-id="74056-181">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="74056-182">`<bold>` Element nie został zmodyfikowany przez pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-182">The `<bold>` element wasn't modified by the tag helper.</span></span>

1. <span data-ttu-id="74056-183">Komentarz `[HtmlTargetElement]` wiersza atrybutu i domyślnie przeznaczonych dla `<bold>` tagów, oznacza to, że kod znaczników HTML w formularzu `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="74056-183">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="74056-184">Należy pamiętać, że domyślna konwencja nazw będzie zgodną z nazwą klasy **Bold**pomocnika tagów do `<bold>` tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-184">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

1. <span data-ttu-id="74056-185">Uruchom aplikację i upewnij się, że `<bold>` tag jest przetwarzany przez pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-185">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="74056-186">Klasa z wieloma urządzanie `[HtmlTargetElement]` atrybuty wyniki w logiczne OR elementów docelowych.</span><span class="sxs-lookup"><span data-stu-id="74056-186">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="74056-187">Na przykład przy użyciu poniższego kodu, pogrubienie tag lub atrybut bold będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="74056-187">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="74056-188">Gdy wiele atrybutów zostaną dodane do tej samej instrukcji, środowisko uruchomieniowe traktować je jak logicznego operatora AND.</span><span class="sxs-lookup"><span data-stu-id="74056-188">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="74056-189">Na przykład w poniższym kodzie HTML element musi mieć nazwę "bold" przy użyciu atrybutu o nazwie "bold" (`<bold bold />`) do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="74056-189">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="74056-190">Można również użyć `[HtmlTargetElement]` do zmiany nazwy elementu docelowego.</span><span class="sxs-lookup"><span data-stu-id="74056-190">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="74056-191">Na przykład, jeżeli chcesz `BoldTagHelper` do obiektu docelowego `<MyBold>` tagów, należy użyć następującego atrybutu:</span><span class="sxs-lookup"><span data-stu-id="74056-191">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="74056-192">Przekaż modelu do pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="74056-192">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="74056-193">Dodaj *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-193">Add a *Models* folder.</span></span>

1. <span data-ttu-id="74056-194">Dodaj następujący kod `WebsiteContext` klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="74056-194">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. <span data-ttu-id="74056-195">Dodaj następujący kod `WebsiteInformationTagHelper` klasy *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-195">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * <span data-ttu-id="74056-196">Jak wspomniano wcześniej, pomocników tagów dokonuje translacji wielkości liter Pascal języka C#, nazwy klas i właściwości dla pomocników tagów w [obniżyć przypadek kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="74056-196">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="74056-197">W związku z tym Aby użyć `WebsiteInformationTagHelper` w aparacie Razor, Ty napiszesz `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="74056-197">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>

   * <span data-ttu-id="74056-198">Nie są jawnie identyfikuje element docelowy z `[HtmlTargetElement]` atrybutu, więc domyślną `website-information` docelowe.</span><span class="sxs-lookup"><span data-stu-id="74056-198">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="74056-199">Jeśli zastosowano atrybut (Uwaga nie jest przypadek kebab, ale jest zgodna z nazwą klasy):</span><span class="sxs-lookup"><span data-stu-id="74056-199">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   <span data-ttu-id="74056-200">Niższe tag przypadków kebab `<website-information />` nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="74056-200">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="74056-201">Jeśli chcesz używać `[HtmlTargetElement]` atrybutu, należy użyć kebab przypadek, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="74056-201">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * <span data-ttu-id="74056-202">Elementy, które są samozamykającego nie ma zawartości.</span><span class="sxs-lookup"><span data-stu-id="74056-202">Elements that are self-closing have no content.</span></span> <span data-ttu-id="74056-203">W tym przykładzie znaczników Razor użyje tagu samozamykającego, ale zostanie utworzona Pomocnik tagu [sekcji](http://www.w3.org/TR/html5/sections.html#the-section-element) element (który nie jest samozamykającego i pisania zawartości wewnątrz `section` elementu).</span><span class="sxs-lookup"><span data-stu-id="74056-203">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="74056-204">W związku z tym, należy ustawić `TagMode` do `StartTagAndEndTag` do zapisywania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="74056-204">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="74056-205">Alternatywnie możesz przekształcić w komentarz ustawienia linii `TagMode` i Zapisywanie kodu znaczników przy użyciu tagu zamykającego.</span><span class="sxs-lookup"><span data-stu-id="74056-205">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="74056-206">(Przykład znaczników znajduje się w dalszej części tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="74056-206">(Example markup is provided later in this tutorial.)</span></span>

   * <span data-ttu-id="74056-207">`$` (Znak dolara) w następującym wierszu używa [ciągiem interpolowanym](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="74056-207">The `$` (dollar sign) in the following line uses an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. <span data-ttu-id="74056-208">Dodaj następujący kod do *About.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="74056-208">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="74056-209">Wyróżnione znaczników Wyświetla informacje o witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="74056-209">The highlighted markup displays the web site information.</span></span>

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]

   > [!NOTE]
   > <span data-ttu-id="74056-210">W znacznikach Razor, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="74056-210">In the Razor markup shown below:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   >
   > <span data-ttu-id="74056-211">Wie razor `info` atrybut jest klasą, a nie w ciągu i chcesz pisać kod w języku C#.</span><span class="sxs-lookup"><span data-stu-id="74056-211">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="74056-212">Dowolny atrybut pomocnika tagów niebędących ciągami powinny być zapisywane bez `@` znaków.</span><span class="sxs-lookup"><span data-stu-id="74056-212">Any non-string tag helper attribute should be written without the `@` character.</span></span>

1. <span data-ttu-id="74056-213">Uruchom aplikację, a następnie przejdź do widoku informacje, aby wyświetlić informacje witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="74056-213">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="74056-214">Można użyć następujących znaczników przy użyciu tagu zamykającego i Usuń wiersz z `TagMode.StartTagAndEndTag` w Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="74056-214">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="74056-215">Pomocnik tagu warunku</span><span class="sxs-lookup"><span data-stu-id="74056-215">Condition Tag Helper</span></span>

<span data-ttu-id="74056-216">Pomocnik tagu warunek renderuje dane wyjściowe przy przekazywaniu wartość true.</span><span class="sxs-lookup"><span data-stu-id="74056-216">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="74056-217">Dodaj następujący kod `ConditionTagHelper` klasy *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-217">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. <span data-ttu-id="74056-218">Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-218">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. <span data-ttu-id="74056-219">Zastąp `Index` method in Class metoda `Home` kontrolera, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="74056-219">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. <span data-ttu-id="74056-220">Uruchom aplikację i przejdź do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="74056-220">Run the app and browse to the home page.</span></span> <span data-ttu-id="74056-221">Kod znaczników w warunkowej `div` nie będą renderowane.</span><span class="sxs-lookup"><span data-stu-id="74056-221">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="74056-222">Dołącz ciąg zapytania `?approved=true` do adresu URL (na przykład `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="74056-222">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="74056-223">`approved` jest ustawiona na wartość PRAWDA, a warunkową znaczników, który będzie wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="74056-223">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="74056-224">Użyj [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator, aby określić atrybut docelowy, a nie określając ciąg, jak w przypadku Pomocnik tagu pogrubienie:</span><span class="sxs-lookup"><span data-stu-id="74056-224">Use the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> <span data-ttu-id="74056-225">[Nameof](/dotnet/csharp/language-reference/keywords/nameof) operator ma być chroniony kod powinien on nigdy nie być refaktoryzowany (Firma Microsoft może chcesz zmienić nazwę aby `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="74056-225">The [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="74056-226">Unikaj konfliktów pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="74056-226">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="74056-227">W tej sekcji możesz zapisywać parę automatycznego łączenia pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="74056-227">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="74056-228">Pierwszy spowoduje zastąpienie znaczników zawierający adres URL rozpoczynający się za pośrednictwem protokołu HTTP HTML zakotwiczenia tag zawierających ten sam adres URL (a zatem reaguje łącze do adresu URL).</span><span class="sxs-lookup"><span data-stu-id="74056-228">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="74056-229">Drugi będzie tak samo dla danego adresu URL począwszy od WWW.</span><span class="sxs-lookup"><span data-stu-id="74056-229">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="74056-230">Ponieważ te dwa pomocników są ściśle powiązane i mogą je refaktoryzować w przyszłości, zachowamy je w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="74056-230">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="74056-231">Dodaj następujący kod `AutoLinkerHttpTagHelper` klasy *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="74056-231">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="74056-232">`AutoLinkerHttpTagHelper` Klasy obiektów docelowych `p` elementów i używa [wyrażenia regularnego](/dotnet/standard/base-types/regular-expression-language-quick-reference) utworzyć zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="74056-232">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

1. <span data-ttu-id="74056-233">Dodaj następujący kod na końcu *Views/Home/Contact.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="74056-233">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. <span data-ttu-id="74056-234">Uruchom aplikację i sprawdź Pomocnik tagu poprawnie renderowana zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="74056-234">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

1. <span data-ttu-id="74056-235">Aktualizacja `AutoLinker` klasy, aby uwzględnić `AutoLinkerWwwTagHelper` który przekonwertuje www tekstu zawierającego oryginalny tekst www tag kotwicy.</span><span class="sxs-lookup"><span data-stu-id="74056-235">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="74056-236">Zaktualizowany kod jest wyróżniona poniżej:</span><span class="sxs-lookup"><span data-stu-id="74056-236">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. <span data-ttu-id="74056-237">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74056-237">Run the app.</span></span> <span data-ttu-id="74056-238">Zwróć uwagę, tekst www jest renderowany jako link, ale nie jest tekst HTTP.</span><span class="sxs-lookup"><span data-stu-id="74056-238">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="74056-239">Jeśli umieścisz punkt przerwania w obu klasach widać, że klasa pomocnika tagów HTTP jest uruchamiany pierwszego.</span><span class="sxs-lookup"><span data-stu-id="74056-239">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="74056-240">Problem polega na że buforowania danych wyjściowych pomocnika tagów, a gdy pomocnik tagu WWW jest uruchomiona, zastępuje on buforowane dane wyjściowe z Pomocnik tagu HTTP.</span><span class="sxs-lookup"><span data-stu-id="74056-240">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="74056-241">W dalszej części tego samouczka zobaczymy sposobu kontrolowania przez pomocników tagów uruchamiane w kolejności.</span><span class="sxs-lookup"><span data-stu-id="74056-241">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="74056-242">Naprawimy ten kod z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="74056-242">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="74056-243">W pierwszym wydaniu pomocników tagów automatycznego łączenia stało się zawartość elementu docelowego z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-243">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > <span data-ttu-id="74056-244">Oznacza to, należy wywołać `GetChildContentAsync` przy użyciu `TagHelperOutput` przekazany do `ProcessAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="74056-244">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="74056-245">Jak wspomniano wcześniej, ponieważ dane wyjściowe są buforowane, ostatni Oznacz element pomocniczy służący do uruchamiania usługi wins.</span><span class="sxs-lookup"><span data-stu-id="74056-245">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="74056-246">Naprawiono problem z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="74056-246">You fixed that problem with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > <span data-ttu-id="74056-247">Powyższy kod sprawdza, czy zawartość została zmodyfikowana, a jeśli tak, pobiera zawartość z buforu wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="74056-247">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

1. <span data-ttu-id="74056-248">Uruchom aplikację i sprawdzić, czy dwa linki działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="74056-248">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="74056-249">Chociaż wydaje się, że naszych Pomocnik tagu konsolidator automatycznie jest prawidłowe i kompletne, ma drobny problem.</span><span class="sxs-lookup"><span data-stu-id="74056-249">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="74056-250">Jeśli Pomocnik tagu WWW jest uruchamiany w pierwszy, linki sieci Web nie będą poprawne.</span><span class="sxs-lookup"><span data-stu-id="74056-250">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="74056-251">Aktualizowanie kodu, dodając `Order` przeciążenie, można określić kolejność pracująca w tagu w.</span><span class="sxs-lookup"><span data-stu-id="74056-251">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="74056-252">`Order` Właściwość określa kolejność wykonywania względem innych pomocników tagów przeznaczonych dla tego samego elementu.</span><span class="sxs-lookup"><span data-stu-id="74056-252">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="74056-253">Wartość domyślna kolejności to zero, a wystąpienia o niższych wartościach są wykonywane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="74056-253">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   <span data-ttu-id="74056-254">Powyższy kod gwarantuje, że Pomocnik tagu HTTP jest uruchamiany przed Pomocnik tagu WWW.</span><span class="sxs-lookup"><span data-stu-id="74056-254">The preceding code guarantees that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="74056-255">Zmiana `Order` do `MaxValue` i sprawdź, czy znaczniki wygenerowany dla tagu WWW jest niepoprawny.</span><span class="sxs-lookup"><span data-stu-id="74056-255">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="74056-256">Sprawdzanie i pobrać zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="74056-256">Inspect and retrieve child content</span></span>

<span data-ttu-id="74056-257">Pomocnicy tagów zawierają kilka właściwości, aby pobrać zawartość.</span><span class="sxs-lookup"><span data-stu-id="74056-257">The tag helpers provide several properties to retrieve content.</span></span>

* <span data-ttu-id="74056-258">Wynik `GetChildContentAsync` można dołączyć do `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="74056-258">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
* <span data-ttu-id="74056-259">Możesz sprawdzić wynik `GetChildContentAsync` z `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="74056-259">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
* <span data-ttu-id="74056-260">Jeśli zmodyfikujesz `output.Content`, treść pomocnika tagów nie będą wykonywane lub renderowane, chyba że wywołujesz `GetChildContentAsync` tak jak w naszym przykładzie automatycznie konsolidatora:</span><span class="sxs-lookup"><span data-stu-id="74056-260">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* <span data-ttu-id="74056-261">Wiele wywołań `GetChildContentAsync` zwraca taką samą wartość i nie jest ponownie wykonywana `TagHelper` treści, chyba że przyjmie wartość false parametru, wskazującą, aby nie korzystała z buforowanego zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="74056-261">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
