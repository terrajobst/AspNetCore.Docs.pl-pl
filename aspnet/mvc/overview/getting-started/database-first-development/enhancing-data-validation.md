---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Samouczek: Zwiększ sprawdzania poprawności danych dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na temat dodawania adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667625"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="1f0cc-103">Samouczek: Zwiększ sprawdzania poprawności danych dla platformy EF Database First w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1f0cc-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="1f0cc-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="1f0cc-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="1f0cc-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="1f0cc-107">Ten samouczek koncentruje się na temat dodawania adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="1f0cc-108">Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="1f0cc-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="1f0cc-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1f0cc-110">Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="1f0cc-110">Add data annotations</span></span>
> * <span data-ttu-id="1f0cc-111">Dodawanie klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="1f0cc-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f0cc-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1f0cc-112">Prerequisites</span></span>

* [<span data-ttu-id="1f0cc-113">Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="1f0cc-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="1f0cc-114">Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="1f0cc-114">Add data annotations</span></span>

<span data-ttu-id="1f0cc-115">Jak pokazano w poprzednim temacie niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="1f0cc-116">Można na przykład tylko podać numer dla właściwości klasy korporacyjnej.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="1f0cc-117">Aby określić więcej reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="1f0cc-118">Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="1f0cc-119">Można także zastosować atrybutów formatowania, które zmieniają się, jak wyświetlić właściwości; takie jak zmiana wartości używanego do etykiet tekstu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="1f0cc-120">W tym samouczku zostaną dodane adnotacje danych, aby ograniczyć długość wartości podanych dla właściwości FirstName, LastName i MiddleName.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="1f0cc-121">W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków: obecnie nie są wymuszane.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="1f0cc-122">Jeśli użytkownik poda więcej niż 50 znaków, dla jednego z tych wartości, strony ulegnie awarii podczas próby zapisania wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="1f0cc-123">Zostanie również ograniczyć klasy korporacyjnej, do wartości z zakresu od 0 do 4.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="1f0cc-124">Wybierz **modeli** > **ContosoModel.edmx** > **ContosoModel.tt** , a następnie otwórz *Student.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="1f0cc-125">Dodaj następujący wyróżniony kod do klasy.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="1f0cc-126">Otwórz *Enrollment.cs* i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="1f0cc-127">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-127">Build the solution.</span></span>

<span data-ttu-id="1f0cc-128">Kliknij przycisk **listę uczniów** i wybierz **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="1f0cc-129">Jeśli użytkownik podejmie próbę wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="1f0cc-131">Wróć do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-131">Go back to the home page.</span></span> <span data-ttu-id="1f0cc-132">Kliknij przycisk **listę rejestracje** i wybierz **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="1f0cc-133">Podjęto próbę zapewnienie jakości powyżej 4.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="1f0cc-134">Zostanie wyświetlony ten błąd: *Pola, które klasy korporacyjnej musi należeć do zakresu od 0 do 4.*</span><span class="sxs-lookup"><span data-stu-id="1f0cc-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="1f0cc-135">Dodawanie klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="1f0cc-135">Add metadata classes</span></span>

<span data-ttu-id="1f0cc-136">Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i chcesz ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="1f0cc-137">Takie podejście może być bardzo mało wydajne i podatne na utratę reguł sprawdzania poprawności ważne.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="1f0cc-138">Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="1f0cc-139">Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="1f0cc-140">W tym podejściu klasy modelu może być generowany ponownie bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="1f0cc-141">W **modeli** folderu, Dodaj klasę o nazwie *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="1f0cc-142">Zastąp kod w *Metadata.cs* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="1f0cc-143">Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które były zastosowane wcześniej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="1f0cc-144">**Wyświetlania** atrybut jest używany, aby zmienić wartość etykiety tekstowe.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="1f0cc-145">Teraz należy skojarzyć klas modelu za pomocą klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="1f0cc-146">W **modeli** folderu, Dodaj klasę o nazwie *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="1f0cc-147">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="1f0cc-148">Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każda jest zgodna nazwa i nazw jako klasa, która jest generowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="1f0cc-149">Stosowanie atrybutu metadanych do klasy częściowej, gwarantuje, że atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="1f0cc-150">Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w klas częściowych, które nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="1f0cc-151">Aby ponownie wygenerować automatycznie wygenerowane klasy, otwórz *ContosoModel.edmx* pliku.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="1f0cc-152">Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Model aktualizacji z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="1f0cc-153">Mimo że nie uległy zmianie bazy danych, ten proces spowoduje to ponowne wygenerowanie klasy.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="1f0cc-154">W **Odśwież** zaznacz **tabel** i **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="1f0cc-155">Zapisz *ContosoModel.edmx* plik, aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="1f0cc-156">Otwórz *Student.cs* pliku lub *Enrollment.cs* plików i zwróć uwagę, że atrybutów sprawdzania poprawności danych, które są stosowane w starszych nie są już w pliku.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="1f0cc-157">Uruchom aplikację i zwróć uwagę, czy reguły sprawdzania poprawności nadal są stosowane podczas wprowadzania danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="1f0cc-158">Wniosek</span><span class="sxs-lookup"><span data-stu-id="1f0cc-158">Conclusion</span></span>

<span data-ttu-id="1f0cc-159">Ta seria podać prosty przykład sposobu generowania kodu z istniejącej bazy danych, która umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-159">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="1f0cc-160">ASP.NET MVC 5, platformy Entity Framework i ASP.NET tworzenie szkieletów on używany do tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-160">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span> 

<span data-ttu-id="1f0cc-161">Wprowadzający przykład rozwiązania deweloperskiego Code First, zobacz [wprowadzenie do ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="1f0cc-161">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> 

<span data-ttu-id="1f0cc-162">Na przykład bardziej zaawansowanych, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji platformy ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="1f0cc-162">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="1f0cc-163">Pamiętaj, że interfejs API typu DbContext, służące do pracy z danymi w pierwszej bazy danych jest taki sam jak interfejs API, można użyć do pracy z danymi w Code First.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-163">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="1f0cc-164">Nawet wtedy, gdy użytkownik zamierza użyć pierwszej bazy danych, możesz dowiedzieć się, jak do obsługi bardziej złożonych scenariuszy, takich jak odczytywanie i aktualizowanie powiązanych danych, obsługa konfliktów współbieżności, i innych elementów z samouczka Code First.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-164">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="1f0cc-165">Jedyną różnicą jest w sposób tworzenia bazy danych, klasy kontekstu i klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="1f0cc-165">The only difference is in how the database, context class, and entity classes are created.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f0cc-166">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1f0cc-166">Additional resources</span></span>

<span data-ttu-id="1f0cc-167">Aby uzyskać pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f0cc-167">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f0cc-168">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1f0cc-168">Next steps</span></span>

<span data-ttu-id="1f0cc-169">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="1f0cc-169">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1f0cc-170">Dane dodane adnotacje</span><span class="sxs-lookup"><span data-stu-id="1f0cc-170">Added data annotations</span></span>
> * <span data-ttu-id="1f0cc-171">Dodano metadanych klas</span><span class="sxs-lookup"><span data-stu-id="1f0cc-171">Added metadata classes</span></span>

<span data-ttu-id="1f0cc-172">Aby dowiedzieć się, jak wdrożyć aplikację sieci web i bazy danych SQL w usłudze Azure App Service, zobacz w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="1f0cc-172">To learn how to deploy a web app and SQL database to Azure App Service, see this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1f0cc-173">Wdrażanie aplikacji .NET w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1f0cc-173">Deploy a .NET app to Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
