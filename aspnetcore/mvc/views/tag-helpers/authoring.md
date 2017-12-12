---
title: "Tworzenie pomocników tagów w platformy ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak tworzyć pomocników tagów w ASP.NET Core."
keywords: "Platformy ASP.NET Core pomocników tagów"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6858b6b8ec89a5e5ffa9e5f8dddb905f38e16603
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="5a82a-104">Tworzenie pomocników tagów w ASP.NET Core wskazówki próbki</span><span class="sxs-lookup"><span data-stu-id="5a82a-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="5a82a-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a82a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a82a-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a82a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="5a82a-107">Wprowadzenie do korzystania z pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="5a82a-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="5a82a-108">Ten samouczek zawiera wprowadzenie do programowania pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="5a82a-109">[Wprowadzenie do pomocników tagów](intro.md) opisuje korzyści, które zapewniają pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="5a82a-110">Obiekt pomocnika tagów jest każda klasa implementująca `ITagHelper` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="5a82a-111">Jednak podczas tworzenia pomocnika tagów można zwykle pochodzi od `TagHelper`, wykonanie tak zapewnia dostęp do `Process` metody.</span><span class="sxs-lookup"><span data-stu-id="5a82a-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="5a82a-112">Utwórz nowy projekt platformy ASP.NET Core o nazwie **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="5a82a-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="5a82a-113">Dla tego projektu nie ma potrzeby uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5a82a-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="5a82a-114">Utwórz folder do przechowywania pomocników tagu o nazwie *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="5a82a-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="5a82a-115">*TagHelpers* folder jest *nie* wymagane, ale jest uzasadnione Konwencję.</span><span class="sxs-lookup"><span data-stu-id="5a82a-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="5a82a-116">Teraz do dzieła pisania pomocników niektórych prostych tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="5a82a-117">Minimalny pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="5a82a-117">A minimal Tag Helper</span></span>

<span data-ttu-id="5a82a-118">W tej sekcji możesz zapisać pomocnika tagów, która aktualizuje tag poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="5a82a-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5a82a-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="5a82a-120">Nasze pomocnika tagów poczty e-mail będzie używany przez serwer programu można przekonwertować tego znacznika do następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5a82a-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="5a82a-121">Oznacza to, że tag kotwicy dzięki temu to łącze w wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="5a82a-122">Można to zrobić, jeśli pisania aparat blogu i potrzebny do wysyłania wiadomości e-mail sprzedaży, pomocy technicznej i inne kontaktów wszystko do tej samej domenie.</span><span class="sxs-lookup"><span data-stu-id="5a82a-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="5a82a-123">Dodaj następujące `EmailTagHelper` klasy do *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="5a82a-124">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="5a82a-124">**Notes:**</span></span>
    
    * <span data-ttu-id="5a82a-125">Pomocników tagów używać konwencji nazewnictwa, która dotyczy elementów Nazwa klasy głównym (minus *pomocnika tagów* część nazwy klasy).</span><span class="sxs-lookup"><span data-stu-id="5a82a-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="5a82a-126">W tym przykładzie nazwa głównego **E-mail**pomocnika tagów jest *e-mail*, więc `<email>` tag będą stosowane.</span><span class="sxs-lookup"><span data-stu-id="5a82a-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="5a82a-127">Konwencja nazewnictwa powinien działać w przypadku większości pomocników tagów, później zostanie omówiony sposób jej zastąpienie.</span><span class="sxs-lookup"><span data-stu-id="5a82a-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="5a82a-128">`EmailTagHelper` Pochodną klasy `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="5a82a-129">`TagHelper` Klasa udostępnia metody i właściwości do pisania pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="5a82a-130">Zastąpione `Process` kontrolki metody pomocnika tagów czego po wykonaniu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="5a82a-131">`TagHelper` Klasa udostępnia także asynchroniczną wersję (`ProcessAsync`) z takimi samymi parametrami.</span><span class="sxs-lookup"><span data-stu-id="5a82a-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="5a82a-132">Parametr kontekstowy do `Process` (i `ProcessAsync`) zawiera informacje związane z wykonywaniem bieżącego tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="5a82a-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="5a82a-133">Parametr wyjściowy do `Process` (i `ProcessAsync`) zawiera element HTML stanowe reprezentatywny z oryginalnego źródła służącego do generowania tagu HTML i zawartości.</span><span class="sxs-lookup"><span data-stu-id="5a82a-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="5a82a-134">Nasze Nazwa klasy zawiera sufiks **pomocnika tagów**, który jest *nie* wymagane, ale uwzględniono Konwencji najlepsze praktyki.</span><span class="sxs-lookup"><span data-stu-id="5a82a-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="5a82a-135">Można zadeklarować tej klasy jako:</span><span class="sxs-lookup"><span data-stu-id="5a82a-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="5a82a-136">Aby `EmailTagHelper` klasy dostępne dla wszystkich naszych widokami Razor, Dodaj `addTagHelper` dyrektywy do *Views/_ViewImports.cshtml* pliku:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="5a82a-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="5a82a-137">Powyższy kod używa składni symboli wieloznacznych do określenia tagów pomocników w naszym zestawie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="5a82a-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="5a82a-138">Pierwszy ciąg po `@addTagHelper` określa pomocnika tagów można załadować (Użyj "*" dla pomocników tagów), a drugi ciąg "AuthoringTagHelpers" Określa zestaw Pomocnik ten tag.</span><span class="sxs-lookup"><span data-stu-id="5a82a-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="5a82a-139">Należy również zauważyć, że drugi wiersz zaimportowanie pomocników tagów platformy ASP.NET Core MVC przy użyciu składni symbolu wieloznacznego (pomocników te zostały omówione w [wprowadzenie do pomocników tagów](intro.md).) Jest `@addTagHelper` dyrektywy, która udostępnia pomocnika tagów do widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="5a82a-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="5a82a-140">Alternatywnie można podać w pełni kwalifikowana nazwa (FQN) pomocniczych znaczników, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a82a-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    <span data-ttu-id="5a82a-141">Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie nazwy zestawu (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="5a82a-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="5a82a-142">Większość deweloperów będzie wolą używać składni symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="5a82a-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="5a82a-143">[Wprowadzenie do pomocników tagów](intro.md) określa szczegółowo składni Dodawanie, usuwanie, hierarchii i symboli wieloznacznych pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="5a82a-144">Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* pliku z tych zmian:</span><span class="sxs-lookup"><span data-stu-id="5a82a-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="5a82a-145">Uruchom aplikację i użyj ulubionej przeglądarce, aby wyświetlić źródło HTML, aby zweryfikować tagi poczty e-mail są zastępowane znacznika zakotwiczenia (na przykład `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="5a82a-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="5a82a-146">*Pomocy technicznej* i *Marketing* są renderowane jako łącza, ale nie mają `href` atrybutu w celu zapewnienia ich funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="5a82a-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="5a82a-147">Firma Microsoft będzie rozwiązać ten problem w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5a82a-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="5a82a-148">Uwaga: Jak tagów HTML i atrybutów, tagi, nazwy klas i atrybutów w Razor i C# nie jest rozróżniana.</span><span class="sxs-lookup"><span data-stu-id="5a82a-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="5a82a-149">SetAttribute i SetContent</span><span class="sxs-lookup"><span data-stu-id="5a82a-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="5a82a-150">W tej sekcji, będziemy informować `EmailTagHelper` tak, aby tworzy tag kotwicy prawidłowy do obsługi poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="5a82a-151">Będziemy informować podjęcia informacji z widoku Razor (w postaci `mail-to` atrybut) i używać go do generowania zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="5a82a-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="5a82a-152">Aktualizacja `EmailTagHelper` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="5a82a-153">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="5a82a-153">**Notes:**</span></span>

* <span data-ttu-id="5a82a-154">Pascal — z uwzględnieniem wielkości liter nazwy klasy i właściwości pomocników tagów są przekształcane na ich [obniżyć przypadku kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="5a82a-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="5a82a-155">W związku z tym Aby użyć `MailTo` użyjesz atrybutu, `<email mail-to="value"/>` równoważne.</span><span class="sxs-lookup"><span data-stu-id="5a82a-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="5a82a-156">Ostatni wiersz ustawia ukończone zawartość dla naszych pomocnika tag minimalny funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="5a82a-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="5a82a-157">Wyróżniony wiersz przedstawiono składnię dodawania atrybutów:</span><span class="sxs-lookup"><span data-stu-id="5a82a-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="5a82a-158">Takie podejście działa dla atrybutu "href", tak długo, jak obecnie nie istnieje w kolekcji atrybutów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="5a82a-159">Można również użyć `output.Attributes.Add` metody, aby dodać atrybut pomocnika tagów do końca kolekcji atrybutów tagu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="5a82a-160">Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* pliku z tych zmian:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="5a82a-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="5a82a-161">Uruchom aplikację i sprawdź generuje poprawne łącza.</span><span class="sxs-lookup"><span data-stu-id="5a82a-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="5a82a-162">Gdyby zapisu poczty e-mail tag własnym zamknięcia (`<email mail-to="Rick" />`), również będą samozamykającego ostateczne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="5a82a-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="5a82a-163">Aby włączyć możliwość zapisu tag z tagiem początkowym (`<email mail-to="Rick">`) musi dekoracji klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="5a82a-164">Z samozamykającego pomocnika tagów poczty e-mail, dane wyjściowe będą `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="5a82a-165">Samozamykającego Tag kotwicy nie są prawidłowe HTML, więc nie chcesz utworzyć, ale warto utworzyć pomocnika tag, który jest samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="5a82a-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="5a82a-166">Pomocników tagów Ustaw typ `TagMode` właściwości po odczytaniu tag.</span><span class="sxs-lookup"><span data-stu-id="5a82a-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="5a82a-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="5a82a-167">ProcessAsync</span></span>

<span data-ttu-id="5a82a-168">W tej sekcji firma Microsoft będzie zapisu pomocnika asynchroniczne poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="5a82a-169">Zastąp `EmailTagHelper` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="5a82a-170">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="5a82a-170">**Notes:**</span></span>

    * <span data-ttu-id="5a82a-171">Ta wersja używa asynchroniczną `ProcessAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="5a82a-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="5a82a-172">Asynchroniczną `GetChildContentAsync` zwraca `Task` zawierający `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="5a82a-173">Użyj `output` parametr, aby pobrać zawartość elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="5a82a-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="5a82a-174">Wprowadź następujące zmiany do *Views/Home/Contact.cshtml* plików, dlatego pomocnika tagów można pobrać docelowych wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="5a82a-175">Uruchom aplikację i sprawdź, czy generuje łącza prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5a82a-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="5a82a-176">RemoveAll, PreContent.SetHtmlContent i PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="5a82a-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="5a82a-177">Dodaj następujące `BoldTagHelper` klasy do *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="5a82a-178">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="5a82a-178">**Notes:**</span></span>
    
    * <span data-ttu-id="5a82a-179">`[HtmlTargetElement]` Atrybutu przekazuje parametr atrybutu, który określa, czy dowolnego elementu HTML zawierającego atrybut HTML o nazwie "bold" jest zgodny, i `Process` metoda przesłonięcia w klasie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="5a82a-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="5a82a-180">W naszym przykładzie `Process` metoda usuwa atrybut "bold" i otaczający zawierającego znaczników z `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="5a82a-181">Ponieważ nie chcesz zastąpić istniejący znacznik zawartości, należy napisać otwarcia `<strong>` tagu z `PreContent.SetHtmlContent` — metoda i zamykania `</strong>` Oznacz za pomocą `PostContent.SetHtmlContent` — metoda.</span><span class="sxs-lookup"><span data-stu-id="5a82a-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="5a82a-182">Modyfikowanie *About.cshtml* widok ma zawierać `bold` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="5a82a-183">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="5a82a-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="5a82a-184">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5a82a-184">Run the app.</span></span> <span data-ttu-id="5a82a-185">Ulubionej przeglądarce służy do kontroli źródła i sprawdź kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="5a82a-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="5a82a-186">`[HtmlTargetElement]` Atrybut powyżej dotyczy tylko kod znaczników HTML, który zawiera nazwę atrybutu "bold".</span><span class="sxs-lookup"><span data-stu-id="5a82a-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="5a82a-187">`<bold>` Element nie został zmodyfikowany przez pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="5a82a-188">Komentarz `[HtmlTargetElement]` wiersza atrybutu i domyślnie przeznaczonych dla `<bold>` tagów, oznacza to, że kod znaczników HTML w formie `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="5a82a-189">Należy pamiętać, że domyślną konwencję nazewnictwa będzie zgodna z nazwą klasy **Bold**pomocnika tagów do `<bold>` tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="5a82a-190">Uruchom aplikację i sprawdź, czy `<bold>` tag są przetwarzane przez pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="5a82a-191">Klasa z wieloma dekoracji `[HtmlTargetElement]` atrybutów wyników w logiczne lub elementy docelowe.</span><span class="sxs-lookup"><span data-stu-id="5a82a-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="5a82a-192">Na przykład przy użyciu poniższy kod, bold tag lub atrybut pogrubienia będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="5a82a-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="5a82a-193">Jeśli wiele atrybutów zostaną dodane do tej samej instrukcji, środowisko uruchomieniowe traktuje je jako logicznego koniunkcji binarnej.</span><span class="sxs-lookup"><span data-stu-id="5a82a-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="5a82a-194">Na przykład w poniższym kodzie HTML element muszą nosić nazwy "bold" z atrybutem o nazwie "bold" (`<bold bold />`) do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="5a82a-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="5a82a-195">Można również użyć `[HtmlTargetElement]` Aby zmienić nazwę elementu docelowego.</span><span class="sxs-lookup"><span data-stu-id="5a82a-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="5a82a-196">Na przykład jeśli potrzebujesz `BoldTagHelper` do docelowego `<MyBold>` tagi, należy użyć następującego atrybutu:</span><span class="sxs-lookup"><span data-stu-id="5a82a-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="5a82a-197">Przekazywanie modelu do pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="5a82a-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="5a82a-198">Dodaj *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="5a82a-199">Dodaj następujące `WebsiteContext` klasy do *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="5a82a-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="5a82a-200">Dodaj następujące `WebsiteInformationTagHelper` klasy do *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="5a82a-201">**Uwagi:**</span><span class="sxs-lookup"><span data-stu-id="5a82a-201">**Notes:**</span></span>
    
    * <span data-ttu-id="5a82a-202">Jak wspomniano wcześniej, pomocników tagów tłumaczy liter Pascal C#, nazwy klas i właściwości dla pomocników tagów do [obniżyć przypadku kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="5a82a-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="5a82a-203">W związku z tym Aby użyć `WebsiteInformationTagHelper` w elemencie Razor, przedstawiono tworzenie `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="5a82a-204">Nie są jawnie identyfikujący element docelowy z `[HtmlTargetElement]` atrybutu to domyślne ustawienie `website-information` będą stosowane.</span><span class="sxs-lookup"><span data-stu-id="5a82a-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="5a82a-205">Po zastosowaniu następującego atrybutu (Uwaga nie jest kebab, ale jest zgodna z nazwą klasy):</span><span class="sxs-lookup"><span data-stu-id="5a82a-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="5a82a-206">Niższe znacznika skrzynki kebab `<website-information />` nie będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="5a82a-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="5a82a-207">Jeśli chcesz używać `[HtmlTargetElement]` atrybutu, należy użyć przypadku kebab, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a82a-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="5a82a-208">Elementy, które są samozamykającego nie ma zawartości.</span><span class="sxs-lookup"><span data-stu-id="5a82a-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="5a82a-209">Na przykład znaczników Razor użyje tagu samozamykającego, ale zostanie utworzony pomocnika tagów [sekcji](http://www.w3.org/TR/html5/sections.html#the-section-element) elementu (który nie jest samozamykającego i pisania zawartości wewnątrz `section` elementu).</span><span class="sxs-lookup"><span data-stu-id="5a82a-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="5a82a-210">W związku z tym należy ustawić `TagMode` do `StartTagAndEndTag` do zapisywania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="5a82a-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="5a82a-211">Alternatywnie komentarz ustawienia linii `TagMode` i pisanie kodu znaczników przy użyciu tagu zamykającego.</span><span class="sxs-lookup"><span data-stu-id="5a82a-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="5a82a-212">(Znaczników przykład znajduje się w dalszej części tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="5a82a-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="5a82a-213">`$` (Dolar) w następującym wierszu używa [interpolowane ciąg](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="5a82a-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="5a82a-214">Dodaj następujący kod do *About.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="5a82a-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="5a82a-215">Wyróżnione znaczników zawiera informacje dotyczące witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="5a82a-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="5a82a-216">W znaczniku Razor, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a82a-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="5a82a-217">Zna razor `info` atrybut jest klasa, nie ciągu i chcesz zapisać kod w języku C#.</span><span class="sxs-lookup"><span data-stu-id="5a82a-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="5a82a-218">Wszelkie atrybut pomocnika tagów innych niż ciąg powinien być zapisywany bez `@` znaków.</span><span class="sxs-lookup"><span data-stu-id="5a82a-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="5a82a-219">Uruchom aplikację i przejdź do widoku informacje, zapoznaj się z informacjami w witrynie sieci web.</span><span class="sxs-lookup"><span data-stu-id="5a82a-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5a82a-220">Można użyć następującego kodu znaczników przy użyciu tagu zamykającego i Usuń wiersz z `TagMode.StartTagAndEndTag` w pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="5a82a-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="5a82a-221">Warunek pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="5a82a-221">Condition Tag Helper</span></span>

<span data-ttu-id="5a82a-222">Warunek pomocnika tagów renderuje dane wyjściowe po upływie wartość true.</span><span class="sxs-lookup"><span data-stu-id="5a82a-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="5a82a-223">Dodaj następujące `ConditionTagHelper` klasy do *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="5a82a-224">Zastąp zawartość *Views/Home/Index.cshtml* pliku z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="5a82a-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="5a82a-225">Zastąp `Index` metoda `Home` kontrolera następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="5a82a-226">Uruchom aplikację i przejdź do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="5a82a-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="5a82a-227">Znaczniki w warunkowej `div` nie będzie renderowany.</span><span class="sxs-lookup"><span data-stu-id="5a82a-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="5a82a-228">Dołącz ciągu zapytania `?approved=true` do adresu URL (na przykład `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="5a82a-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="5a82a-229">`approved`ma ustawioną wartość true a warunkowe zostanie wyświetlony kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="5a82a-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="5a82a-230">Użyj [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatora, aby określić atrybut docelowy zamiast określania ciąg, jak w przypadku Pomocnik bold tagu:</span><span class="sxs-lookup"><span data-stu-id="5a82a-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="5a82a-231">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatora będzie chronić kod powinien on kiedykolwiek można refaktorować (może chcemy zmienić nazwę na `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="5a82a-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="5a82a-232">Unikanie konfliktów pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="5a82a-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="5a82a-233">W tej sekcji należy napisać parę automatyczne łączenie pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="5a82a-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="5a82a-234">Pierwszy spowoduje zastąpienie znaczników zawierający adres URL rozpoczynający się protokołu HTTP z HTML zakotwiczenia tag zawierający ten sam adres URL (i w związku z tym reaguje łącze do adresu URL).</span><span class="sxs-lookup"><span data-stu-id="5a82a-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="5a82a-235">Drugi będzie wykonywać takie same dla danego adresu URL począwszy WWW.</span><span class="sxs-lookup"><span data-stu-id="5a82a-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="5a82a-236">Ponieważ te dwa pomocników są ściśle powiązane i mogą je w przyszłości Refaktoryzuj, firma Microsoft będzie przechowywać w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="5a82a-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="5a82a-237">Dodaj następujące `AutoLinkerHttpTagHelper` klasy do *TagHelpers* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="5a82a-238">`AutoLinkerHttpTagHelper` Klas docelowych `p` elementów i używa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) utworzyć zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="5a82a-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="5a82a-239">Dodaj następujący kod na końcu *Views/Home/Contact.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="5a82a-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="5a82a-240">Uruchom aplikację i sprawdź pomocnika tagów poprawnie renderuje zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="5a82a-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="5a82a-241">Aktualizacja `AutoLinker` klasy, aby uwzględnić `AutoLinkerWwwTagHelper` który przekonwertuje tekst www tag kotwicy zawierający oryginalny tekst www.</span><span class="sxs-lookup"><span data-stu-id="5a82a-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="5a82a-242">Zaktualizowany kod zostanie wyróżniona poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a82a-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="5a82a-243">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5a82a-243">Run the app.</span></span> <span data-ttu-id="5a82a-244">Zwróć uwagę, www tekst jest traktowany jako łącze, ale nie jest tekst HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a82a-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="5a82a-245">Jeśli umieścisz punkt przerwania w obu klasach widać, czy klasa pomocnika tagów HTTP jest uruchamiany pierwszy.</span><span class="sxs-lookup"><span data-stu-id="5a82a-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="5a82a-246">Problem polega na dane wyjściowe pomocnika tagów są buforowane, czy po uruchomieniu pomocnika tagów WWW, zastępuje on dane wyjściowe pamięci podręcznej pomocnika tagów HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a82a-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="5a82a-247">W dalszej części samouczka przedstawiono będzie sterowanie pomocników tagów uruchamiane w kolejności.</span><span class="sxs-lookup"><span data-stu-id="5a82a-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="5a82a-248">Firma Microsoft będzie naprawić kod z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5a82a-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="5a82a-249">W pierwszym wydaniu pomocników tagów automatyczne łączenie masz zawartość elementu docelowego z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="5a82a-250">Oznacza to, należy wywołać `GetChildContentAsync` przy użyciu `TagHelperOutput` przekazany `ProcessAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="5a82a-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="5a82a-251">Jak wspomniano wcześniej, ponieważ dane wyjściowe są buforowane, ostatniego tagu pomocnika do uruchamiania usługi wins.</span><span class="sxs-lookup"><span data-stu-id="5a82a-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="5a82a-252">Został rozwiązany problem z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5a82a-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="5a82a-253">Powyższy kod sprawdza, czy zawartość została zmodyfikowana, a jeśli tak, pobiera zawartość z buforu wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="5a82a-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="5a82a-254">Uruchom aplikację i sprawdzić, czy dwa łącza działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="5a82a-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="5a82a-255">Gdy wydaje się, że nasze pomocnika tagów konsolidatora automatycznie jest prawidłowe i kompletne ma niewielkie problem.</span><span class="sxs-lookup"><span data-stu-id="5a82a-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="5a82a-256">Jeśli pierwszy uruchamia pomocnika tagów WWW, linki sieci Web nie będą poprawne.</span><span class="sxs-lookup"><span data-stu-id="5a82a-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="5a82a-257">Zaktualizuj kod, dodając `Order` przeciążenia można sterować kolejnością działającym w tagu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="5a82a-258">`Order` Właściwość określa kolejność wykonywania względem innych pomocników tagów przeznaczonych dla tego samego elementu.</span><span class="sxs-lookup"><span data-stu-id="5a82a-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="5a82a-259">Wartość domyślna kolejności to zero, a wystąpienia o niższych wartościach jest wykonywany jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="5a82a-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="5a82a-260">Powyższy kod zagwarantuje, że pomocnika tagów HTTP jest uruchamiany przed pomocnika tagów WWW.</span><span class="sxs-lookup"><span data-stu-id="5a82a-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="5a82a-261">Zmień `Order` do `MaxValue` i sprawdź, czy znacznika generowany dla tagu WWW jest niepoprawny.</span><span class="sxs-lookup"><span data-stu-id="5a82a-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="5a82a-262">Sprawdzanie i pobierania zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="5a82a-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="5a82a-263">Pomocników tagów podać kilka właściwości, aby pobrać zawartość.</span><span class="sxs-lookup"><span data-stu-id="5a82a-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="5a82a-264">Wynik `GetChildContentAsync` można dołączać do `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="5a82a-265">Możesz sprawdzić wynik `GetChildContentAsync` z `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="5a82a-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="5a82a-266">Jeśli zmodyfikujesz `output.Content`, treści pomocnika tagów nie zostanie wykonana ani renderowane, chyba że wywołujesz `GetChildContentAsync` jak naszej próbki automatycznie konsolidatora:</span><span class="sxs-lookup"><span data-stu-id="5a82a-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="5a82a-267">Wiele wywołań `GetChildContentAsync` zwróci taką samą wartość i nie zostanie wykonany ponownie `TagHelper` body, chyba że przekazujesz w parametrze false wskazujący używaj buforowanego zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="5a82a-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
