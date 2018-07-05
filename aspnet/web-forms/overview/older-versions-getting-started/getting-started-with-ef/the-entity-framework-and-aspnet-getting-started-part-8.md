---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 8 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374055"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 8
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Za pomocą funkcji dynamicznego danych do formatu i sprawdzanie poprawności danych

W poprzednim samouczku wdrożono procedury składowane. W tym samouczku opisano, jak dane dynamiczne, funkcje może zapewnić następujące korzyści:

- Pola są automatycznie formatowane do wyświetlania na podstawie ich typu danych.
- Pola są automatycznie zweryfikowana, na podstawie ich typu danych.
- Metadane można dodawać do modelu danych, aby dostosować zachowanie formatowania i sprawdzania poprawności. Gdy to zrobisz, możesz dodać reguły formatowania i sprawdzania poprawności w jednym miejscu i są automatycznie stosowane wszędzie, gdzie możesz uzyskać dostęp pola za pomocą formantów danych dynamicznych.

Aby zobaczyć, jak to działa, możesz zmienić formantów, użyj do wyświetlania i edytowania pól w istniejącym *Students.aspx* strony i dodasz metadanych formatowania i sprawdzania poprawności do pola Nazwa i data `Student` typu jednostki.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Przy użyciu DynamicField i DynamicControl formantów

Otwórz *Students.aspx* strony i `StudentsGridView` Zastąp formant **nazwa** i **Data rejestracji** `TemplateField` elementów za pomocą następujących znaczników:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Ten kod znaczników używa `DynamicControl` kontroluje zamiast `TextBox` i `Label` kontrolek w student nazwy szablonu, przy czym `DynamicField` kontrola Data rejestracji. Ciągi formatów, nie są określone.

Dodaj `ValidationSummary` kontrolowanie po `StudentsGridView` kontroli.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

W `SearchGridView` kontrolka zastąpić kod znaczników dla **nazwa** i **Data rejestracji** kolumn, co Ty, jak w `StudentsGridView` kontrolować, z wyjątkiem pominąć `EditItemTemplate` elementu. `Columns` Elementu `SearchGridView` kontrolka zawiera teraz następujące znaczniki:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Otwórz *Students.aspx.cs* i dodaj następującą `using` instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Dodawanie obsługi dla strony `Init` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Ten kod określa, że dane dynamiczne zapewni formatowania i sprawdzania poprawności w tych formantów powiązanych z danymi dla pól `Student` jednostki. Jeśli otrzymasz komunikat o błędzie, jak w poniższym przykładzie, po uruchomieniu strony, zwykle oznacza to, pamiętasz wywołania `EnableDynamicData` method in Class metoda `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Uruchom stronę.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

W **Data rejestracji** kolumnie czasu jest wyświetlany wraz z datą, ponieważ typ właściwości to `DateTime`. Można to naprawić później.

Na razie Zwróć uwagę, dane dynamiczne zapewnia automatycznie Walidacja danych podstawowych. Na przykład kliknij pozycję **Edytuj**, wyczyść pole daty, kliknij przycisk **aktualizacji**, i zobaczysz, że dane dynamiczne automatycznie sprawia, że to pole wymagane ponieważ wartości nie dopuszcza wartości null w modelu danych. Zostanie wyświetlona strona gwiazdkę po pola i komunikat o błędzie w `ValidationSummary` sterowania:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Można pominąć `ValidationSummary` kontrolować, ponieważ można również przytrzymasz wskaźnik myszy nad gwiazdki, aby wyświetlić komunikat o błędzie:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dane dynamiczne również zostanie przeprowadzona Weryfikacja dane wprowadzone w **Data rejestracji** pole jest prawidłową datą:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Jak widać, to ogólny komunikat o błędzie. W następnej sekcji pokazano, jak dostosować komunikaty, a także sprawdzania poprawności i reguły formatowania.

## <a name="adding-metadata-to-the-data-model"></a>Dodawanie metadanych do modelu danych

Zazwyczaj który chcesz dostosować funkcje udostępniane przez dane dynamiczne. Na przykład możesz zmienić sposób wyświetlania danych i zawartość komunikaty o błędach. Użytkownik zwykle również dostosować reguły sprawdzania poprawności danych, aby zapewnić więcej funkcji niż zawiera dane dynamiczne automatycznie oparte na typach danych. Aby to zrobić, należy utworzyć klas częściowych, które odnoszą się do typów jednostek.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, wybierz opcję **Dodaj odwołanie**i Dodaj odwołanie do `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

W *DAL* folderu, Utwórz nowy plik klasy, nadaj jej nazwę *Student.cs*i Zastąp kod szablonu w nim poniższy kod.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Ten kod tworzy klasę częściową dla `Student` jednostki. `MetadataType` Zastosowany do klasy częściowej identyfikuje klasę, która za pomocą określić metadane. Klasa metadanych może mieć dowolną nazwę, ale przy użyciu nazwy podmiotu oraz "Metadane" jest powszechną praktyką.

Atrybuty stosowane do właściwości w klasie metadanych określ, formatowania i sprawdzania poprawności, zasady i komunikaty o błędach. Atrybuty pokazane tutaj będzie miała następujące wyniki:

- `EnrollmentDate` zostanie wyświetlona jako datę (bez czasu).
- Zarówno nazwa pola musi być 25 znaków lub mniej długości i niestandardowy komunikat o błędzie jest dostarczany.
- Zarówno nazwa pola, które są wymagane, a podano niestandardowy komunikat o błędzie.

Uruchom *Students.aspx* strony ponownie, a zobaczysz, że dane są teraz wyświetlane bez godzin:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Edytuj wiersz i spróbuj wyczyścić wartości w polach Nazwa. Gwiazdek wskazującą błędy pola są wyświetlane tak szybko, jak pozostawić pole, zanim klikniesz pozycję **aktualizacji**. Po kliknięciu **aktualizacji**, zostanie wyświetlona strona tekst komunikatu o błędzie określony.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Wprowadź nazwy, które są dłuższe niż 25 znaków, kliknij przycisk **aktualizacji**, i zostanie wyświetlona strona tekst komunikatu o błędzie określony.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Teraz, gdy skonfigurowano te reguły formatowania i sprawdzania poprawności metadanych modelu danych, zasady zostaną automatycznie zastosowane na każdej stronie, który wyświetla lub zezwala na zmiany w tych polach, tak długo, jak długo używasz `DynamicControl` lub `DynamicField` kontrolki. Zmniejsza to ilość nadmiarowy kod, który trzeba napisać, co sprawia, że programowanie i testowanie, i zapewnia, że formatowanie danych i sprawdzania poprawności są spójne w całej aplikacji.

## <a name="more-information"></a>Więcej informacji

To już koniec tej serii samouczków na temat rozpoczynania pracy z platformą Entity Framework. Aby uzyskać więcej zasobów, aby pomóc nauczyć się, jak używać programu Entity Framework, kontynuuj [pierwszym samouczku dalej serii samouczków Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) lub można znaleźć w następujących witrynach:

- [Entity Framework — często zadawane pytania](http://www.ef-faq.org/introduction.html)
- [Blog zespołu programu Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework w bibliotece MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework w Centrum deweloperów MSDN danych](https://msdn.microsoft.com/data/ef.aspx)
- [Omówienie kontrolki serwera sieci Web EntityDataSource w bibliotece MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Kontrolka EntityDataSource dokumentacja interfejsu API w bibliotece MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework fora w witrynie MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-7.md)
