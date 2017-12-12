---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 5 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7200899d54585cd09e0a648e3aaaf839db2649e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET — część 5
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Praca z danymi powiązane, nadal

W poprzednich instrukcji rozpoczęcia do użycia `EntityDataSource` formantu do pracy z powiązanych danych. Wyświetlane wiele poziomów hierarchii i edycji danych właściwości nawigacji. W tym samouczku nastąpi przejście do pracy z powiązanych danych przez dodawanie i usuwanie relacji i przez dodanie nowej jednostki, która ma relację do istniejącego obiektu.

Utworzysz strona, która dodaje kursów, które są przypisane do działów. Działy już istnieje, a podczas tworzenia nowego kursu w tym samym czasie będzie ustanowienie relacji między nim a istniejących działu.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Strona, która działa w relacji wiele do wielu, przypisując instruktora do kursu (Dodawanie relacji między dwiema jednostkami, które można wybrać) również zostaną utworzone lub usuwanie instruktora z kursu (usuwanie relacji między dwiema jednostkami które Wybierz). W bazie danych, Dodawanie relacji między instruktora i kursu powoduje dodawany do nowego wiersza `CourseInstructor` tabeli skojarzenia; usuwanie relacji polega na usuwaniu wierszy z `CourseInstructor` tabeli skojarzenia. Jednak możesz to zrobić w programu Entity Framework przez ustawienie właściwości nawigacji, bez odwołujących się do `CourseInstructor` jawnie tabeli.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Dodawanie jednostki w relacji do istniejącego obiektu

Tworzenie nowej strony sieci web o nazwie *CoursesAdd.aspx* używającą *Site.Master* strony wzorcowej i Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Tworzy tego znacznika `EntityDataSource` formant, który wybiera kursów, który umożliwia wstawienie i program obsługi, który określa `Inserting` zdarzeń. Użyjesz programu obsługi aktualizacji `Department` właściwości nawigacji podczas tworzenia nowego `Course` utworzona jednostka.

Tworzy również znaczników `DetailsView` kontrolki używanej do dodawania nowych `Course` jednostek. Kod znaczników używa pola powiązane `Course` właściwości jednostki. Należy wprowadzić `CourseID` wartości, ponieważ nie jest polem Identyfikator generowanych przez system. Zamiast tego jest to liczba kursu, które muszą zostać określone ręcznie, gdy jest tworzona w trakcie.

Użyj pola szablonu dla `Department` właściwość nawigacji, ponieważ nie można używać właściwości nawigacji z `BoundField` formantów. Pole szablonu umożliwia listy rozwijanej, aby wybrać działu. Listy rozwijanej jest powiązany z `Departments` zestawu przy użyciu jednostek `Eval` zamiast `Bind`, ponownie ponieważ bezpośrednio nie można powiązać właściwości nawigacji, aby można było zaktualizować je. Określ program obsługi `DropDownList` formantu `Init` zdarzeń, dzięki czemu można przechowywać odwołanie do formantu do użytku przez kod, który aktualizuje `DepartmentID` klucza obcego.

W *CoursesAdd.aspx.cs* zaraz po deklaracji klasy częściowe, Dodaj pola klasy, aby pomieścić odwołanie do `DepartmentsDropDownList` sterowania:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Dodaj obsługę `DepartmentsDropDownList` formantu `Init` zdarzeń, dzięki czemu można przechowywać odwołanie do formantu. Dzięki temu można uzyskać wartość użytkownik wprowadził i użyj go, aby zaktualizować `DepartmentID` wartość `Course` jednostki.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Dodaj obsługę `DetailsView` formantu `Inserting` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Po kliknięciu przez użytkownika `Insert`, `Inserting` zdarzenie jest wywoływane przed wstawieniem nowym rekordzie. Pobiera kod obsługi `DepartmentID` z `DropDownList` kontroli i używa go do wartości, który będzie używany dla `DepartmentID` właściwość `Course` jednostki.

Entity Framework zajmie się dodawania tego kursu do `Courses` właściwość nawigacji skojarzonego `Department` jednostki. Dodaje również dział do `Department` właściwość nawigacji `Course` jednostki.

Uruchom strony.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Wprowadź identyfikator, tytuł, liczbę środków i wybierz działu, a następnie kliknij przycisk **Wstaw**.

Uruchom *Courses.aspx* strony, a następnie wybierz tego samego działu, aby wyświetlić nowy plan.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Praca z relacji wiele do wielu

Relacja między `Courses` zestaw jednostek oraz `People` zestaw jednostek znajduje relacji wiele do wielu. A `Course` jednostki ma właściwości nawigacji o nazwie `People` zawierających zero, co najmniej jeden powiązane `Person` jednostek (reprezentujący instruktorów przypisane do tego kursu nauki). I `Person` jednostki ma właściwości nawigacji o nazwie `Courses` zawierających zero, co najmniej jeden powiązane `Course` jednostek (reprezentujący kursy przypisane nauczysz się tym instruktora). Jeden instruktora może nauczyć szkoleń w wielu i jednego kursu może organizowane jednocześnie przez wiele instruktorów. W tej sekcji tego przewodnika, będzie dodawanie i usuwanie relacji między `Person` i `Course` jednostek, aktualizując właściwości nawigacji z powiązanych jednostek.

Tworzenie nowej strony sieci web o nazwie *InstructorsCourses.aspx* używającą *Site.Master* strony wzorcowej i Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Tworzy tego znacznika `EntityDataSource` formant, który pobiera nazwę i `PersonID` z `Person` jednostki dla instruktorów. A `DropDrownList` kontrolka jest powiązana z `EntityDataSource` formantu. `DropDownList` Kontroli określa program obsługi `DataBound` zdarzeń. Ten program obsługi do listy rozwijanej databind dwa, które wyświetlają kursy będzie używany.

Kod znaczników tworzy także następującej grupy formantów do przypisywania kursu do wybranego instruktora:

- A `DropDownList` formant wyboru kursu do przypisania. Ten formant zostanie wypełniona kursów, które aktualnie nie są przypisane do wybranych instruktora.
- A `Button` formantu, aby zainicjować przypisania.
- A `Label` formantu, aby wyświetlić komunikat o błędzie, jeśli przypisanie zakończy się niepowodzeniem.

Na koniec znaczników również tworzy grupę kontrolek używanych w celu usunięcia kursu z wybranym instruktorze.

W *InstructorsCourses.aspx.cs*, dodać za pomocą instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Dodaj metodę do zapełniania dwie listy rozwijanej, które wyświetlają kursy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Ten kod pobiera wszystkie kursy z `Courses` jednostki ustawić i pobiera kursów z `Courses` właściwość nawigacji `Person` jednostki dla wybranego instruktora. Następnie określa kursów, które są przypisane do tego instruktora i odpowiednio wypełnia list rozwijanych.

Dodaj obsługę `Assign` przycisku `Click` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Ten kod pobiera `Person` pobiera jednostki dla wybranego instruktora `Course` jednostki dostępne ze i dodaje je do wybranego kursu `Courses` właściwość nawigacji instruktora `Person` jednostki. Następnie zapisuje zmiany w bazie danych i repopulates listy rozwijanej, aby wyniki można zobaczyć natychmiast.

Dodaj obsługę `Remove` przycisku `Click` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Ten kod pobiera `Person` pobiera jednostki dla wybranego instruktora `Course` jednostki dostępne ze i spowoduje usunięcie wybranego kursu z `Person` jednostki `Courses` właściwości nawigacji. Następnie zapisuje zmiany w bazie danych i repopulates listy rozwijanej, aby wyniki można zobaczyć natychmiast.

Dodaj kod, aby `Page_Load` metodę, która sprawdza się, że komunikaty o błędach nie są widoczne, gdy nie było błędu raportu, a następnie dodaj obsługę `DataBound` i `SelectedIndexChanged` zdarzenia z listy rozwijanej instruktorów do wypełnienia listy rozwijanej kursów:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Uruchom strony.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wybierz instruktora. **Przypisać kursu** listy rozwijanej wyświetla kursów, które nie uczy instruktora, i **Usuń kursu** kursów, które instruktora jest już przypisany do wyświetla listy rozwijanej. W **przypisać kursu** sekcji wybierz kursu, a następnie kliknij przycisk **przypisać**. Przenosi kursu **Usuń kursu** listy rozwijanej. Wybierz kursów **Usuń kursu** sekcji, a następnie kliknij przycisk **Usuń***.* Przenosi kursu **przypisać kursu** listy rozwijanej.

Niektóre inne sposoby pracy z powiązanych danych ma teraz widoczna. Poniższe kroki samouczka dowiesz się, jak użycia dziedziczenia w modelu danych do poprawy łatwości konserwacji aplikacji.

>[!div class="step-by-step"]
[Poprzednie](the-entity-framework-and-aspnet-getting-started-part-4.md)
[dalej](the-entity-framework-and-aspnet-getting-started-part-6.md)
