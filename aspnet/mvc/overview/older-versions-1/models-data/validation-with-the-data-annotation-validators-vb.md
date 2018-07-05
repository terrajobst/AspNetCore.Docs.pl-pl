---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Weryfikacja przy użyciu modułów weryfikacji adnotacji danych (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Korzystaj z integratora modelu adnotacji danych do wykonywania sprawdzania poprawności w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów weryfikacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 94410d0e8ad132a4e7a31ac01163663b597c3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391421"
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="5b364-104">Weryfikacja przy użyciu modułów weryfikacji adnotacji danych (VB)</span><span class="sxs-lookup"><span data-stu-id="5b364-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="5b364-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5b364-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5b364-106">Korzystaj z integratora modelu adnotacji danych do wykonywania sprawdzania poprawności w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b364-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="5b364-107">Dowiedz się, jak używać różnych typów atrybutów modułu sprawdzania poprawności i pracować z nimi w programie Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b364-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="5b364-108">W tym samouczku dowiesz się, jak używać modułów weryfikacji adnotacji danych do przeprowadzania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b364-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="5b364-109">Zaletą używania modułów weryfikacji adnotacji danych jest, czy umożliwiają one do wykonywania sprawdzania poprawności, po prostu dodając jeden lub więcej atrybutów — na przykład wymagane lub atrybut StringLength — do właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="5b364-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="5b364-110">Zanim będzie można użyć modułów weryfikacji adnotacji danych, należy pobrać integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5b364-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="5b364-111">Próbka integratora modelu adnotacji danych można pobrać z witryny CodePlex, po kliknięciu [tutaj](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="5b364-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="5b364-112">Jest ważne dowiedzieć się, że integratora modelu adnotacji danych nie jest oficjalną częścią platformę Microsoft ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b364-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="5b364-113">Mimo że integratora modelu adnotacji danych został utworzony przez zespół programu Microsoft ASP.NET MVC, Microsoft nie oferuje oficjalne pomocy technicznej dla integratora modelu adnotacji danych opisano i używanych w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5b364-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="5b364-114">Za pomocą integratora modelu adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="5b364-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="5b364-115">Aby użyć integratora modelu adnotacji danych w aplikacji ASP.NET MVC, należy najpierw dodać odwołanie do zestawu Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="5b364-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="5b364-116">Wybierz opcję menu **projektu, Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="5b364-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="5b364-117">Następnie kliknij przycisk **Przeglądaj** kartę, a następnie przejdź do lokalizacji, w której pobrano (i rozpakowanej) przykładowe integratora modelu adnotacji danych (zobacz **rys.1**).</span><span class="sxs-lookup"><span data-stu-id="5b364-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="5b364-118">**Rysunek 1**: Dodawanie odwołania do integratora modelu adnotacji danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b364-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="5b364-119">Wybierz zestaw Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll, a następnie kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="5b364-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="5b364-120">Nie można użyć zestawu System.ComponentModel.DataAnnotations.dll dołączone do programu .NET Framework z dodatkiem Service Pack 1 za pomocą integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5b364-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="5b364-121">Należy użyć wersji zestawu System.ComponentModel.DataAnnotations.dll dołączone do pobierania próbki integratora modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5b364-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="5b364-122">Na koniec należy zarejestrować DataAnnotations integratora modelu w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="5b364-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="5b364-123">Dodaj następujący wiersz kodu do aplikacji\_Start() obsługi zdarzeń, aby aplikacja\_metody Start() wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="5b364-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="5b364-124">Ten wiersz kodu rejestruje DataAnnotationsModelBinder jako domyślnego integratora modelu dla całej aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b364-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="5b364-125">Przy użyciu atrybutów weryfikacji adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="5b364-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="5b364-126">Gdy używasz integratora modelu adnotacji danych, używasz modułu sprawdzania poprawności atrybutów do wykonywania sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5b364-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="5b364-127">Przestrzeń nazw System.ComponentModel.DataAnnotations zawiera następujące atrybuty weryfikacji:</span><span class="sxs-lookup"><span data-stu-id="5b364-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="5b364-128">Zakres — umożliwia weryfikuje, czy wartość właściwości należące do określonego zakresu wartości.</span><span class="sxs-lookup"><span data-stu-id="5b364-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="5b364-129">ReqularExpression — można sprawdzić, czy wartość właściwości odpowiada wzorcowi określonemu wyrażeniu regularnemu.</span><span class="sxs-lookup"><span data-stu-id="5b364-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="5b364-130">Wymagane — można oznaczać właściwości zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="5b364-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="5b364-131">StringLength — umożliwia określenie maksymalnej długości dla właściwości ciągu.</span><span class="sxs-lookup"><span data-stu-id="5b364-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="5b364-132">Sprawdzanie poprawności — klasa bazowa dla wszystkich atrybutów modułu sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5b364-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5b364-133">Jeśli Twoje potrzeby sprawdzania poprawności nie są spełnione przez żaden standardowych modułów weryfikacji następnie zawsze istnieje możliwość tworzenia atrybutu niestandardowego modułu weryfikacji przez dziedziczenie nowy atrybut weryfikacji z atrybutu podstawowego sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5b364-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="5b364-134">Klasa produktu w **ofercie 1** ilustruje sposób użycia tych atrybutów modułu sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5b364-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="5b364-135">Nazwa, opis i cena jednostkowa właściwości są oznaczone jako wymagane.</span><span class="sxs-lookup"><span data-stu-id="5b364-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="5b364-136">Nazwa właściwości musi mieć długość ciągu, która jest mniejsza niż 10 znaków.</span><span class="sxs-lookup"><span data-stu-id="5b364-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="5b364-137">Na koniec właściwość UnitPrice musi być zgodna z wzorcem wyrażenia regularnego, który reprezentuje kwotę w walucie.</span><span class="sxs-lookup"><span data-stu-id="5b364-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="5b364-138">**Wyświetlanie listy 1**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="5b364-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="5b364-139">Klasa produktu ilustruje sposób użycia jednego dodatkowego atrybutu: atrybut DisplayName.</span><span class="sxs-lookup"><span data-stu-id="5b364-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="5b364-140">Atrybut DisplayName służy do modyfikowania nazwę właściwości, gdy właściwość jest wyświetlana w komunikacie o błędzie.</span><span class="sxs-lookup"><span data-stu-id="5b364-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="5b364-141">Zamiast wyświetlać komunikat o błędzie "wymagane jest pole Cenyjednostkowej" można wyświetlić komunikat o błędzie "wymagane jest pole ceny".</span><span class="sxs-lookup"><span data-stu-id="5b364-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5b364-142">Aby dostosować komunikat o błędzie wyświetlany przez moduł weryfikacji niestandardowy komunikat o błędzie można przypisać do właściwości komunikat o błędzie weryfikacji następująco: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="5b364-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="5b364-143">Można użyć klasy produktu w **ofercie 1** za pomocą akcji kontrolera Create() w **ofercie 2**.</span><span class="sxs-lookup"><span data-stu-id="5b364-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="5b364-144">Ta akcja kontrolera zostanie ponownie Utwórz widok podczas stanu modelu zawiera błędy.</span><span class="sxs-lookup"><span data-stu-id="5b364-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="5b364-145">**Wyświetlanie listy 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="5b364-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="5b364-146">Na koniec możesz utworzyć widok w **ofercie 3** , kliknij prawym przyciskiem myszy działanie Create() i wybierając opcję menu **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="5b364-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="5b364-147">Utwórz widok silnie typizowaną klasą produktu jako klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="5b364-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="5b364-148">Wybierz **Utwórz** z listy rozwijanej zawartości widoku (zobacz **na rysunku 2**).</span><span class="sxs-lookup"><span data-stu-id="5b364-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="5b364-149">**Rysunek 2**: Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="5b364-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="5b364-150">**Wyświetlanie listy 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="5b364-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5b364-151">Usuń pole identyfikatora formularza tworzenia generowane przez **Dodaj widok** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="5b364-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="5b364-152">Ponieważ pole identyfikatora odnosi się do kolumny tożsamości, nie chcesz zezwalać użytkownikom na wprowadzanie wartości dla tego pola.</span><span class="sxs-lookup"><span data-stu-id="5b364-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="5b364-153">Jeśli prześlesz formularz dla tworzenia produktu i nie należy wprowadzać wartości dla wymaganych pól, a następnie komunikaty o błędach weryfikacji w **rysunek 3** są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="5b364-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="5b364-154">**Rysunek 3**: Brak wymaganych pól</span><span class="sxs-lookup"><span data-stu-id="5b364-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="5b364-155">Jeśli wprowadzasz kwotę w walucie nieprawidłowy, a następnie komunikat o błędzie w **rysunek 4** jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="5b364-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="5b364-156">**Rysunek 4**: kwotę w walucie nieprawidłowy</span><span class="sxs-lookup"><span data-stu-id="5b364-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="5b364-157">Przy użyciu modułów weryfikacji adnotacji danych za pomocą programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b364-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="5b364-158">Jeśli używasz Microsoft Entity Framework do generowania klasach modeli danych nie można zastosować atrybuty weryfikacji bezpośrednio do swojej klasy.</span><span class="sxs-lookup"><span data-stu-id="5b364-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="5b364-159">Ponieważ Entity Framework Designer generuje klasy modelu, wszelkie zmiany wprowadzone do klasy modelu zostanie zastąpiony podczas następnego wprowadzisz zmiany w projektancie.</span><span class="sxs-lookup"><span data-stu-id="5b364-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="5b364-160">Jeśli chcesz użyć moduły weryfikacji przy użyciu klas wygenerowanych przez program Entity Framework następnie należy utworzyć meta klasy danych.</span><span class="sxs-lookup"><span data-stu-id="5b364-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="5b364-161">Moduły weryfikacji dotyczą meta klasy danych zamiast stosowania moduły weryfikacji do rzeczywistego klasy.</span><span class="sxs-lookup"><span data-stu-id="5b364-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="5b364-162">Załóżmy, że utworzono klasę filmu używający narzędzia Entity Framework (zobacz **rysunek 5**).</span><span class="sxs-lookup"><span data-stu-id="5b364-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="5b364-163">Wyobraź sobie, ponadto chcesz wprowadzić tytuł filmu i dyrektor ds. właściwości wymaganych właściwości.</span><span class="sxs-lookup"><span data-stu-id="5b364-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="5b364-164">W takim przypadku można utworzyć klasy częściowe i meta klasy danych w **listę 4**.</span><span class="sxs-lookup"><span data-stu-id="5b364-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="5b364-165">**Rysunek 5**: klasa filmu generowane przez program Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b364-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="5b364-166">**Wyświetlanie listy 4**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="5b364-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="5b364-167">Plik w **listę 4** zawiera dwie klasy o nazwie MovieMetaData i filmów.</span><span class="sxs-lookup"><span data-stu-id="5b364-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="5b364-168">Klasa film jest klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="5b364-168">The Movie class is a partial class.</span></span> <span data-ttu-id="5b364-169">Odnosi się do klasy częściowej generowane przez program Entity Framework, który jest zawarty w pliku DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="5b364-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="5b364-170">.NET framework nie obsługuje obecnie częściowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="5b364-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="5b364-171">W związku z tym, nie ma możliwości dotyczą atrybutów weryfikacji właściwości klasy filmu zdefiniowane w pliku DataModel.Designer.vb przez zastosowanie atrybutów weryfikacji właściwości klasy filmu zdefiniowane w pliku w **listę 4**.</span><span class="sxs-lookup"><span data-stu-id="5b364-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="5b364-172">Należy zauważyć, że klasy częściowej film zostanie nadany atrybut MetadataType, który wskazuje na klasy MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="5b364-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="5b364-173">Klasa MovieMetaData zawiera właściwości serwera proxy dla właściwości klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="5b364-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="5b364-174">Atrybuty modułu sprawdzania poprawności są stosowane do właściwości klasy MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="5b364-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="5b364-175">Właściwości tytułu, Dyrektor ds. i DateReleased są oznaczone jako wymagane właściwości.</span><span class="sxs-lookup"><span data-stu-id="5b364-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="5b364-176">Dyrektor ds. właściwość musi być przypisany jest ciąg zawierający mniej niż 5 znaków.</span><span class="sxs-lookup"><span data-stu-id="5b364-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="5b364-177">Na koniec atrybut DisplayName jest stosowany do właściwości DateReleased, aby wyświetlić komunikat o błędzie, takie jak "pole daty wydania jest wymagane."</span><span class="sxs-lookup"><span data-stu-id="5b364-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="5b364-178">zamiast błędu "DateReleased pole jest wymagane."</span><span class="sxs-lookup"><span data-stu-id="5b364-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5b364-179">Należy zauważyć, że właściwości serwera proxy w klasie MovieMetaData nie ma potrzeby reprezentują te same typy jako odpowiednie właściwości w klasie filmu.</span><span class="sxs-lookup"><span data-stu-id="5b364-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="5b364-180">Na przykład właściwość Dyrektor jest właściwością ciągu w klasie film i właściwości obiektu w klasie MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="5b364-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="5b364-181">Na stronie **rysunek 6** ilustruje komunikatów o błędach zwracanych podczas wprowadzania nieprawidłowe wartości dla właściwości filmu.</span><span class="sxs-lookup"><span data-stu-id="5b364-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="5b364-182">**Rysunek 6**: przy użyciu modułów weryfikacji za pomocą programu Entity Framework ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="5b364-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="5b364-183">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5b364-183">Summary</span></span>

<span data-ttu-id="5b364-184">W tym samouczku przedstawiono sposób korzystać z zalet integratora modelu adnotacji danych do wykonywania sprawdzania poprawności w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b364-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="5b364-185">Pokazaliśmy ci, jak używać różnych typów atrybutów StringLength i modułu sprawdzania poprawności atrybutów, takich jak jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="5b364-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="5b364-186">Przedstawiono również sposób użycia tych atrybutów podczas pracy z usługą Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b364-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5b364-187">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5b364-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
