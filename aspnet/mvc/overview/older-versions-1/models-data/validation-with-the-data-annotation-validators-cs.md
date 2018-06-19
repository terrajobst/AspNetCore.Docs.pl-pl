---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Weryfikowanie przy użyciu adnotacji danych modułów sprawdzania poprawności (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Skorzystaj z integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów modułu sprawdzania poprawności...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 0aca9472094e6a54c7b7cb4ad4f12df64fe12db2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870196"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="db35b-104">Weryfikowanie przy użyciu adnotacji danych modułów sprawdzania poprawności (C#)</span><span class="sxs-lookup"><span data-stu-id="db35b-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="db35b-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="db35b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="db35b-106">Skorzystaj z integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db35b-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="db35b-107">Dowiedz się, jak używać różnych typów atrybutów modułu sprawdzania poprawności i pracować z nimi w ramach jednostki firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="db35b-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="db35b-108">Z tego samouczka dowiesz sposobu używania adnotacji danych modułów sprawdzania poprawności do przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db35b-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="db35b-109">Zaletą używania adnotacji danych modułów sprawdzania poprawności jest, czy umożliwiają one sprawdzania poprawności, po prostu dodając jeden lub więcej atrybutów — na przykład wymagane lub atrybut StringLength — do właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="db35b-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="db35b-110">Przed użyciem moduły weryfikacji adnotacji danych, należy pobrać integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="db35b-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="db35b-111">Możesz pobrać próbki integratora modelu adnotacji danych w witrynie CodePlex, klikając [tutaj](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="db35b-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="db35b-112">Należy zrozumieć, że integratora modelu adnotacji danych nie jest oficjalną częścią struktury Microsoft ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db35b-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="db35b-113">Mimo że integratora modelu adnotacji danych została utworzona przez zespół Microsoft ASP.NET MVC, Microsoft nie oferuje oficjalną pomoc techniczna dla integratora modelu adnotacji danych opisane i używane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="db35b-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="db35b-114">Za pomocą integratora modelu adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="db35b-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="db35b-115">Aby można było używać integratora modelu adnotacji danych w aplikacji platformy ASP.NET MVC, należy najpierw dodać odwołanie do zestawu Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="db35b-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="db35b-116">Wybierz opcję menu **projekt, Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="db35b-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="db35b-117">Następnie kliknij przycisk **Przeglądaj** karcie i przejdź do lokalizacji, do którego pobrano (i rozpakowane) próbki integratora modelu adnotacji danych (zobacz **rysunek 1**).</span><span class="sxs-lookup"><span data-stu-id="db35b-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="db35b-118">**Rysunek 1**: Dodawanie odwołania do integratora modelu adnotacji danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="db35b-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="db35b-119">Zaznacz zestawu Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll, a następnie kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="db35b-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="db35b-120">Nie można użyć zestawu System.ComponentModel.DataAnnotations.dll dołączone do programu .NET Framework Service Pack 1 z integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="db35b-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="db35b-121">Należy użyć wersji zestawu System.ComponentModel.DataAnnotations.dll dołączone do pobierania próbki integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="db35b-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="db35b-122">Ponadto należy zarejestrować DataAnnotations integratora modelu w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="db35b-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="db35b-123">Dodaj następujący wiersz kodu do aplikacji\_Start() obsługi zdarzeń, aby aplikacja\_metodę Start() wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="db35b-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="db35b-124">Ten wiersz kodu rejestruje ataAnnotationsModelBinder jako domyślnego integratora modelu dla całej aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db35b-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="db35b-125">Przy użyciu modułu sprawdzania poprawności atrybutów adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="db35b-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="db35b-126">Gdy używasz integratora modelu adnotacji danych służy do sprawdzania poprawności atrybutów modułu walidacji.</span><span class="sxs-lookup"><span data-stu-id="db35b-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="db35b-127">Przestrzeń nazw System.ComponentModel.DataAnnotations zawiera następujące atrybuty modułu sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="db35b-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="db35b-128">Zakres — umożliwia sprawdzanie, czy wartość właściwości należące do określonego zakresu wartości.</span><span class="sxs-lookup"><span data-stu-id="db35b-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="db35b-129">Wyrażenia regularnego — umożliwia sprawdzanie, czy wartość właściwości odpowiada wzorcowi określonemu wyrażeniu regularnemu.</span><span class="sxs-lookup"><span data-stu-id="db35b-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="db35b-130">Wymagane — można oznaczać właściwość jako wymaganą.</span><span class="sxs-lookup"><span data-stu-id="db35b-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="db35b-131">StringLength — umożliwia określenie maksymalnej długości dla właściwości ciągu.</span><span class="sxs-lookup"><span data-stu-id="db35b-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="db35b-132">Sprawdzanie poprawności — klasa podstawowa dla wszystkich atrybutów modułu walidacji.</span><span class="sxs-lookup"><span data-stu-id="db35b-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db35b-133">Jeśli Twoje potrzeby sprawdzania poprawności nie są spełnione przez standardowe moduły weryfikacji należy zawsze wybrać opcję tworzenia atrybutu niestandardowego modułu weryfikacji przez dziedziczenie nowy atrybut modułu sprawdzania poprawności z atrybutu podstawowego sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="db35b-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="db35b-134">Klasa produktu w **wyświetlania 1** ilustruje sposób używania tych atrybutów modułu walidacji.</span><span class="sxs-lookup"><span data-stu-id="db35b-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="db35b-135">Właściwości nazwę, opis i UnitPrice są oznaczone jako wymagane.</span><span class="sxs-lookup"><span data-stu-id="db35b-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="db35b-136">Właściwość Name musi mieć długość ciągu, która jest mniejsza niż 10 znaków.</span><span class="sxs-lookup"><span data-stu-id="db35b-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="db35b-137">Na koniec właściwość UnitPrice musi być zgodna wzorzec wyrażenia regularnego, który reprezentuje kwotę waluty.</span><span class="sxs-lookup"><span data-stu-id="db35b-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="db35b-138">**Wyświetlanie listy 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="db35b-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="db35b-139">Klasa produktu ilustruje sposób używania jeden atrybut dodatkowe: atrybut Nazwa wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="db35b-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="db35b-140">Atrybut DisplayName umożliwia zmianę nazwy właściwości, gdy właściwość jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="db35b-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="db35b-141">Zamiast wyświetlania komunikatu o błędzie "UnitPrice pole jest wymagane" można wyświetlić komunikat o błędzie "cena pole jest wymagane".</span><span class="sxs-lookup"><span data-stu-id="db35b-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db35b-142">Aby dostosować komunikat o błędzie wyświetlany przez moduł weryfikacji można przypisać niestandardowy komunikat o błędzie do modułu sprawdzania poprawności właściwości komunikat o błędzie podobny do tego: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="db35b-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="db35b-143">Można użyć klasy produktu w **wyświetlania 1** z akcji kontrolera Create() w **wyświetlania 2**.</span><span class="sxs-lookup"><span data-stu-id="db35b-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="db35b-144">Ta akcja kontrolera zostanie ponownie Utwórz widok, gdy stanu modelu zawiera błędy.</span><span class="sxs-lookup"><span data-stu-id="db35b-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="db35b-145">**Wyświetlanie listy 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="db35b-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="db35b-146">Na koniec można utworzyć widoku w **wyświetlania 3** prawym przyciskiem myszy działanie Create() i wybierając opcję menu **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="db35b-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="db35b-147">Utwórz widok jednoznacznie przy użyciu klasy produktu jako klasa modelu.</span><span class="sxs-lookup"><span data-stu-id="db35b-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="db35b-148">Wybierz **Utwórz** z listy rozwijanej zawartości widoku (zobacz **na rysunku 2**).</span><span class="sxs-lookup"><span data-stu-id="db35b-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="db35b-149">**Rysunek 2**: Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="db35b-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="db35b-150">**Wyświetlanie listy 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="db35b-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db35b-151">Usuń pola Identyfikator w formularzu Utwórz generowane przez **Dodaj widok** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="db35b-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="db35b-152">Ponieważ w polu identyfikatora odnosi się do kolumny tożsamości, nie chcesz zezwolić użytkownikom na wprowadzanie wartości dla tego pola.</span><span class="sxs-lookup"><span data-stu-id="db35b-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="db35b-153">Jeśli Prześlij formularz służący do tworzenia produktu i nie należy wprowadzać wartości dla pól wymaganych, a następnie w wiadomości błąd sprawdzania poprawności **rysunek 3** są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="db35b-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="db35b-154">**Rysunek 3**: Brak pola wymaganego</span><span class="sxs-lookup"><span data-stu-id="db35b-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="db35b-155">Jeśli wprowadzisz nieprawidłowy waluty, a następnie komunikat o błędzie w **4 rysunek** jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="db35b-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="db35b-156">**Rysunek 4**: nieprawidłowy waluty.</span><span class="sxs-lookup"><span data-stu-id="db35b-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="db35b-157">Używanie modułów weryfikacji adnotacji danych z programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="db35b-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="db35b-158">Jeśli używasz programu Entity Framework firmy Microsoft można wygenerować klas modelu danych nie można zastosować atrybutów modułu walidacji bezpośrednio do klas.</span><span class="sxs-lookup"><span data-stu-id="db35b-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="db35b-159">Ponieważ Entity Framework Designer generuje klasy modeli, wszystkie zmiany wprowadzone w klasach modelu zostaną zastąpione podczas następnego wprowadzenia zmian w projektancie.</span><span class="sxs-lookup"><span data-stu-id="db35b-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="db35b-160">Jeśli chcesz użyć moduły weryfikacji z klasy generowane przez program Entity Framework następnie musisz utworzyć meta klasy danych.</span><span class="sxs-lookup"><span data-stu-id="db35b-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="db35b-161">Moduły weryfikacji dotyczą meta klasy danych zamiast stosować moduły weryfikacji, do rzeczywistych klasy.</span><span class="sxs-lookup"><span data-stu-id="db35b-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="db35b-162">Na przykład, załóżmy, że utworzono klasy filmu przy użyciu programu Entity Framework (zobacz **rysunek 5**).</span><span class="sxs-lookup"><span data-stu-id="db35b-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="db35b-163">Załóżmy, ponadto, czy mają być tytuł filmu i dyrektora wymagane właściwości.</span><span class="sxs-lookup"><span data-stu-id="db35b-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="db35b-164">W takim przypadku można utworzyć klasy częściowe i meta klasy danych w **listę 4**.</span><span class="sxs-lookup"><span data-stu-id="db35b-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="db35b-165">**Rysunek 5**: klasa filmu generowane przez program Entity Framework</span><span class="sxs-lookup"><span data-stu-id="db35b-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="db35b-166">**Wyświetlanie listy 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="db35b-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="db35b-167">Plik w **listę 4** zawiera dwie klasy o nazwie filmów oraz MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="db35b-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="db35b-168">Klasa film jest klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="db35b-168">The Movie class is a partial class.</span></span> <span data-ttu-id="db35b-169">Odnosi się do klasy częściowej generowane przez program Entity Framework, który jest zawarty w pliku DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="db35b-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="db35b-170">.NET framework nie obsługuje obecnie, właściwości częściowej.</span><span class="sxs-lookup"><span data-stu-id="db35b-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="db35b-171">W związku z tym nie istnieje sposób się atrybutów modułu sprawdzania poprawności do właściwości klasy filmu zdefiniowane w pliku DataModel.Designer.vb przez zastosowanie atrybutów modułu sprawdzania poprawności do właściwości klasy filmu zdefiniowane w pliku w **listę 4**.</span><span class="sxs-lookup"><span data-stu-id="db35b-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="db35b-172">Zwróć uwagę, że klasy częściowej filmu zostanie nadany atrybut MetadataType, który wskazuje klasę MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="db35b-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="db35b-173">Klasa MovieMetaData zawiera właściwości serwera proxy dla właściwości klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="db35b-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="db35b-174">Moduł sprawdzania poprawności atrybutów są stosowane do właściwości klasy MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="db35b-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="db35b-175">Tytuł, dyrektor i DateReleased zostały oznaczone jako wymagane właściwości.</span><span class="sxs-lookup"><span data-stu-id="db35b-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="db35b-176">Właściwość dyrektor musi być przypisana ciąg, który zawiera mniej niż 5 znaków.</span><span class="sxs-lookup"><span data-stu-id="db35b-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="db35b-177">Na koniec atrybut DisplayName jest stosowane do właściwości DateReleased, aby wyświetlić komunikat o błędzie, takie jak "Data wydania pole jest wymagane."</span><span class="sxs-lookup"><span data-stu-id="db35b-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="db35b-178">zamiast błędu "DateReleased pole jest wymagane."</span><span class="sxs-lookup"><span data-stu-id="db35b-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db35b-179">Należy zauważyć, że właściwości serwera proxy w klasie MovieMetaData nie ma potrzeby reprezentują ten sam typ jak odpowiednie właściwości w klasie filmu.</span><span class="sxs-lookup"><span data-stu-id="db35b-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="db35b-180">Na przykład właściwość Dyrektor jest właściwością ciągu w klasie filmu i właściwości obiektu w klasie MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="db35b-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="db35b-181">Strona w **rysunek 6** przedstawiono zwrócony podczas wprowadzania nieprawidłowe wartości dla właściwości filmu komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="db35b-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="db35b-182">**Rysunek 6**: Używanie modułów weryfikacji z programu Entity Framework ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="db35b-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="db35b-183">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="db35b-183">Summary</span></span>

<span data-ttu-id="db35b-184">W tym samouczku przedstawiono sposób wykorzystać integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="db35b-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="db35b-185">Przedstawiono sposób użycia różnych typów atrybutów modułu sprawdzania poprawności, takich jak wymaganych i atrybuty StringLength.</span><span class="sxs-lookup"><span data-stu-id="db35b-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="db35b-186">Przedstawiono również sposób używania tych atrybutów, podczas pracy z programu Entity Framework firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="db35b-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="db35b-187">[Poprzednie](validating-with-a-service-layer-cs.md)
> [dalej](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="db35b-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
