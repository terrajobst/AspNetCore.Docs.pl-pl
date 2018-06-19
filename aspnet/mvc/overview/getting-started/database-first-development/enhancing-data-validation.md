---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: udoskonalanie sprawdzanie poprawności danych | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879611"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="90fe9-104">Baza danych EF najpierw o platformie ASP.NET MVC: udoskonalanie sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="90fe9-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="90fe9-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="90fe9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="90fe9-106">Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="90fe9-107">Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="90fe9-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="90fe9-109">Ta część serii koncentruje się na dodawanie adnotacji danych do modelu danych, aby określić wymagania weryfikacji i wyświetlenie formatowania.</span><span class="sxs-lookup"><span data-stu-id="90fe9-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="90fe9-110">Zostało ulepszone oparte na opinii użytkowników w sekcji uwag.</span><span class="sxs-lookup"><span data-stu-id="90fe9-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="90fe9-111">Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="90fe9-111">Add data annotations</span></span>

<span data-ttu-id="90fe9-112">Jak przedstawiono w temacie wcześniejszych niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="90fe9-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="90fe9-113">Można na przykład tylko podać numer dla właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="90fe9-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="90fe9-114">Aby określić dalszych reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="90fe9-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="90fe9-115">Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="90fe9-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="90fe9-116">Można także zastosować atrybuty formatowania, które spowodują zmianę sposobu wyświetlania właściwości; takie jak zmiana wartości używane do etykiet tekstowych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="90fe9-117">W tym samouczku spowoduje dodanie adnotacji danych, aby ograniczyć długość wartości określone dla właściwości FirstName, LastName i MiddleName.</span><span class="sxs-lookup"><span data-stu-id="90fe9-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="90fe9-118">W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków aktualnie nie jest wymuszana.</span><span class="sxs-lookup"><span data-stu-id="90fe9-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="90fe9-119">Jeśli użytkownik udostępnia więcej niż 50 znaków dla jednej z tych wartości, strony ulegnie awarii podczas próby zapisania wartość w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="90fe9-120">Klasa będzie również ograniczyć do wartości od 0 do 4.</span><span class="sxs-lookup"><span data-stu-id="90fe9-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="90fe9-121">Otwórz **Student.cs** w pliku **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="90fe9-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="90fe9-122">Dodaj następujący wyróżniony kod do klasy.</span><span class="sxs-lookup"><span data-stu-id="90fe9-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="90fe9-123">W Enrollment.cs Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="90fe9-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="90fe9-124">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="90fe9-124">Build the solution.</span></span>

<span data-ttu-id="90fe9-125">Przejdź do strony do edytowania lub tworzenia studenta.</span><span class="sxs-lookup"><span data-stu-id="90fe9-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="90fe9-126">Jeśli spróbujesz wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="90fe9-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="90fe9-128">Przejdź do strony do edycji rejestracji, a podejmowana próba udostępnienia klasy powyżej 4.</span><span class="sxs-lookup"><span data-stu-id="90fe9-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Błąd zakresu klasy](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="90fe9-130">Pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="90fe9-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="90fe9-131">Dodawanie klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="90fe9-131">Add metadata classes</span></span>

<span data-ttu-id="90fe9-132">Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i potrzebujesz można ponownie wygenerować klasy modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="90fe9-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="90fe9-133">Ta metoda może być bardzo mało wydajne i podatne na utraty reguł sprawdzania poprawności ważne.</span><span class="sxs-lookup"><span data-stu-id="90fe9-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="90fe9-134">Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty.</span><span class="sxs-lookup"><span data-stu-id="90fe9-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="90fe9-135">Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="90fe9-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="90fe9-136">W tej metody klasy modelu można ponownie wygenerować bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="90fe9-137">W **modele** folderu, Dodaj klasę o nazwie **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="90fe9-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Dodaj klasę metadanych](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="90fe9-139">Zastąp kod w Metadata.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="90fe9-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="90fe9-140">Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które zastosowano wcześniej dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="90fe9-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="90fe9-141">**Wyświetlania** atrybut służy do zmiany wartość używaną do tekstu etykiety.</span><span class="sxs-lookup"><span data-stu-id="90fe9-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="90fe9-142">Teraz należy skojarzyć klasy modelu z klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="90fe9-143">W **modele** folderu, Dodaj klasę o nazwie **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="90fe9-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="90fe9-144">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="90fe9-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="90fe9-145">Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każdy odpowiada nazwę i przestrzeń nazw jako klasa, która jest generowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="90fe9-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="90fe9-146">Zastosowanie atrybutu metadanych do klasy częściowej, sprawdź, czy atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="90fe9-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="90fe9-147">Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w częściowej klasy, które nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="90fe9-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="90fe9-148">Aby ponownie wygenerować klasy generowane automatycznie, otwórz plik ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="90fe9-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="90fe9-149">Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz pozycję **modelu aktualizacji z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="90fe9-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="90fe9-150">Mimo że nie zmieniono bazy danych, ten proces spowoduje ponowne wygenerowanie klasy.</span><span class="sxs-lookup"><span data-stu-id="90fe9-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="90fe9-151">W **Odśwież** wybierz opcję **tabel** i **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="90fe9-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![Odśwież tabele](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="90fe9-153">Zapisz plik ContosoModel.edmx w celu zastosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="90fe9-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="90fe9-154">Otwórz plik Student.cs lub plik Enrollment.cs i nie są zastosowane wcześniej atrybuty weryfikacji danych w pliku.</span><span class="sxs-lookup"><span data-stu-id="90fe9-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="90fe9-155">Uruchom aplikację i zwróć uwagę, reguły walidacji nadal są stosowane podczas wprowadzania danych.</span><span class="sxs-lookup"><span data-stu-id="90fe9-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="90fe9-156">[Poprzednie](customizing-a-view.md)
> [dalej](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="90fe9-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
