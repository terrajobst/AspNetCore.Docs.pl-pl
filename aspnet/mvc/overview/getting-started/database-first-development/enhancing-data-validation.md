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
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236487"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="7df5e-103">Samouczek: Zwiększ sprawdzania poprawności danych dla platformy EF Database First w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7df5e-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="7df5e-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7df5e-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7df5e-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="7df5e-107">Ten samouczek koncentruje się na temat dodawania adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.</span><span class="sxs-lookup"><span data-stu-id="7df5e-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="7df5e-108">Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.</span><span class="sxs-lookup"><span data-stu-id="7df5e-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="7df5e-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="7df5e-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7df5e-110">Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="7df5e-110">Add data annotations</span></span>
> * <span data-ttu-id="7df5e-111">Dodawanie klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="7df5e-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7df5e-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7df5e-112">Prerequisites</span></span>

* [<span data-ttu-id="7df5e-113">Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="7df5e-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="7df5e-114">Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="7df5e-114">Add data annotations</span></span>

<span data-ttu-id="7df5e-115">Jak pokazano w poprzednim temacie niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7df5e-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="7df5e-116">Można na przykład tylko podać numer dla właściwości klasy korporacyjnej.</span><span class="sxs-lookup"><span data-stu-id="7df5e-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="7df5e-117">Aby określić więcej reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="7df5e-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="7df5e-118">Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="7df5e-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="7df5e-119">Można także zastosować atrybutów formatowania, które zmieniają się, jak wyświetlić właściwości; takie jak zmiana wartości używanego do etykiet tekstu.</span><span class="sxs-lookup"><span data-stu-id="7df5e-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="7df5e-120">W tym samouczku zostaną dodane adnotacje danych, aby ograniczyć długość wartości podanych dla właściwości FirstName, LastName i MiddleName.</span><span class="sxs-lookup"><span data-stu-id="7df5e-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="7df5e-121">W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków: obecnie nie są wymuszane.</span><span class="sxs-lookup"><span data-stu-id="7df5e-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="7df5e-122">Jeśli użytkownik poda więcej niż 50 znaków, dla jednego z tych wartości, strony ulegnie awarii podczas próby zapisania wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="7df5e-123">Zostanie również ograniczyć klasy korporacyjnej, do wartości z zakresu od 0 do 4.</span><span class="sxs-lookup"><span data-stu-id="7df5e-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="7df5e-124">Wybierz **modeli** > **ContosoModel.edmx** > **ContosoModel.tt** , a następnie otwórz *Student.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7df5e-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="7df5e-125">Dodaj następujący wyróżniony kod do klasy.</span><span class="sxs-lookup"><span data-stu-id="7df5e-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="7df5e-126">Otwórz *Enrollment.cs* i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="7df5e-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="7df5e-127">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="7df5e-127">Build the solution.</span></span>

<span data-ttu-id="7df5e-128">Kliknij przycisk **listę uczniów** i wybierz **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="7df5e-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="7df5e-129">Jeśli użytkownik podejmie próbę wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7df5e-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="7df5e-131">Wróć do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7df5e-131">Go back to the home page.</span></span> <span data-ttu-id="7df5e-132">Kliknij przycisk **listę rejestracje** i wybierz **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="7df5e-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="7df5e-133">Podjęto próbę zapewnienie jakości powyżej 4.</span><span class="sxs-lookup"><span data-stu-id="7df5e-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="7df5e-134">Zostanie wyświetlony ten błąd: *Pola, które klasy korporacyjnej musi należeć do zakresu od 0 do 4.*</span><span class="sxs-lookup"><span data-stu-id="7df5e-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="7df5e-135">Dodawanie klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="7df5e-135">Add metadata classes</span></span>

<span data-ttu-id="7df5e-136">Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i chcesz ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="7df5e-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="7df5e-137">Takie podejście może być bardzo mało wydajne i podatne na utratę reguł sprawdzania poprawności ważne.</span><span class="sxs-lookup"><span data-stu-id="7df5e-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="7df5e-138">Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty.</span><span class="sxs-lookup"><span data-stu-id="7df5e-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="7df5e-139">Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="7df5e-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="7df5e-140">W tym podejściu klasy modelu może być generowany ponownie bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="7df5e-141">W **modeli** folderu, Dodaj klasę o nazwie *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="7df5e-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="7df5e-142">Zastąp kod w *Metadata.cs* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="7df5e-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="7df5e-143">Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które były zastosowane wcześniej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="7df5e-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="7df5e-144">**Wyświetlania** atrybut jest używany, aby zmienić wartość etykiety tekstowe.</span><span class="sxs-lookup"><span data-stu-id="7df5e-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="7df5e-145">Teraz należy skojarzyć klas modelu za pomocą klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="7df5e-146">W **modeli** folderu, Dodaj klasę o nazwie *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="7df5e-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="7df5e-147">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="7df5e-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="7df5e-148">Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każda jest zgodna nazwa i nazw jako klasa, która jest generowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="7df5e-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="7df5e-149">Stosowanie atrybutu metadanych do klasy częściowej, gwarantuje, że atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="7df5e-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="7df5e-150">Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w klas częściowych, które nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="7df5e-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="7df5e-151">Aby ponownie wygenerować automatycznie wygenerowane klasy, otwórz *ContosoModel.edmx* pliku.</span><span class="sxs-lookup"><span data-stu-id="7df5e-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="7df5e-152">Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Model aktualizacji z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="7df5e-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="7df5e-153">Mimo że nie uległy zmianie bazy danych, ten proces spowoduje to ponowne wygenerowanie klasy.</span><span class="sxs-lookup"><span data-stu-id="7df5e-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="7df5e-154">W **Odśwież** zaznacz **tabel** i **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="7df5e-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="7df5e-155">Zapisz *ContosoModel.edmx* plik, aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="7df5e-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="7df5e-156">Otwórz *Student.cs* pliku lub *Enrollment.cs* plików i zwróć uwagę, że atrybutów sprawdzania poprawności danych, które są stosowane w starszych nie są już w pliku.</span><span class="sxs-lookup"><span data-stu-id="7df5e-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="7df5e-157">Uruchom aplikację i zwróć uwagę, czy reguły sprawdzania poprawności nadal są stosowane podczas wprowadzania danych.</span><span class="sxs-lookup"><span data-stu-id="7df5e-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7df5e-158">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7df5e-158">Additional resources</span></span>

<span data-ttu-id="7df5e-159">Aby uzyskać pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="7df5e-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7df5e-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7df5e-160">Next steps</span></span>

<span data-ttu-id="7df5e-161">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="7df5e-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7df5e-162">Dane dodane adnotacje</span><span class="sxs-lookup"><span data-stu-id="7df5e-162">Added data annotations</span></span>
> * <span data-ttu-id="7df5e-163">Dodano metadanych klas</span><span class="sxs-lookup"><span data-stu-id="7df5e-163">Added metadata classes</span></span>

<span data-ttu-id="7df5e-164">Przejdź do następnego samouczka, aby dowiedzieć się, jak opublikować aplikację sieci web i bazy danych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="7df5e-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7df5e-165">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="7df5e-165">Publish to Azure</span></span>](publish-to-azure.md)