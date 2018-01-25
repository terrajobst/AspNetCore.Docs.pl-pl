---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 8 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET - część 8
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Za pomocą funkcji danych dynamicznych sformatowanie i sprawdzanie poprawności danych

W poprzednich instrukcji zaimplementowano procedur składowanych. Ten samouczek przedstawia sposób funkcji dynamicznych danych zapewniają następujące korzyści:

- Pola są automatycznie formatowane do wyświetlania na podstawie ich typu danych.
- Pola są automatycznie weryfikowane, na podstawie ich typu danych.
- Metadane można dodawać do modelu danych, aby dostosować zachowanie formatowania i walidacji. Gdy to zrobisz, można dodawać reguły formatowania i walidacji w jednym miejscu i są automatycznie stosowane wszędzie uzyskujesz dostęp za pomocą formantów dynamicznych danych pola.

Aby zobaczyć, jak to działa, zostanie zmieniona formanty używać do wyświetlania i edytowania pól w istniejących *Students.aspx* strony, a będzie dodawać metadane formatowania i walidacji do pola nazwy i daty `Student` typu jednostki.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Przy użyciu DynamicField i formant DynamicControl formantów

Otwórz *Students.aspx* strony i w `StudentsGridView` zamienianie kontrola **nazwa** i **Data rejestracji** `TemplateField` elementy o następujący kod:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Używa tego znacznika `DynamicControl` steruje zamiast `TextBox` i `Label` formantów w student nazwy szablonu i używa `DynamicField` kontroli dla daty rejestracji. Ciągi formatu nie zostały określone.

Dodaj `ValidationSummary` kontrolowanie po `StudentsGridView` formantu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

W `SearchGridView` kontroli Zastąp kod znaczników dla **nazwa** i **Data rejestracji** kolumn jako użytkownik, jak w `StudentsGridView` kontrolować, z wyjątkiem Pomiń `EditItemTemplate` elementu. `Columns` Elementu `SearchGridView` formant zawiera teraz następujący kod:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Otwórz *Students.aspx.cs* i dodaj następującą `using` instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Dodaj obsługę strony `Init` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Ten kod określa, że dane dynamiczne zapewni formatowania i walidacji w tych kontrolek powiązanych z danymi dla pól `Student` jednostki. Jeśli zostanie wyświetlony komunikat o błędzie, jak w następującym przykładzie, po uruchomieniu strony, zwykle oznacza to, pamiętasz do wywołania `EnableDynamicData` metody w `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Uruchom strony.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

W **Data rejestracji** kolumny, ponieważ typ właściwości jest wraz z datą jest wyświetlany czas `DateTime`. Który będzie rozwiązać później.

Obecnie Zauważ, że dane dynamiczne automatycznie zawiera podstawowe dane weryfikacji. Na przykład kliknij pozycję **Edytuj**, wyczyść pole daty, kliknij przycisk **aktualizacji**, i sprawdź, czy danych dynamicznych automatycznie sprawia, że to pole wymagane, ponieważ wartość nie jest dopuszczalna w modelu danych. Strona wyświetla gwiazdkę po pole i komunikat o błędzie w `ValidationSummary` sterowania:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Można pominąć `ValidationSummary` kontroli, ponieważ wciśnij wskaźnik myszy nad gwiazdki, aby wyświetlić komunikat o błędzie:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dane dynamiczne będzie zweryfikować dane wprowadzone w **Data rejestracji** pole jest prawidłową datą:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Jak widać, to ogólny komunikat o błędzie. W następnej sekcji pojawi się, jak dostosować wiadomości, a także sprawdzania poprawności i reguły formatowania.

## <a name="adding-metadata-to-the-data-model"></a>Dodawanie metadanych do modelu danych

Zwykle który chcesz dostosować funkcje udostępniane przez dane dynamiczne. Na przykład możesz zmienić sposób wyświetlania danych i zawartość komunikaty o błędach. Zwykle również dostosowywania reguły sprawdzania poprawności danych, aby zapewnić więcej funkcji niż danych dynamicznych udostępnia automatycznie na podstawie danych typów. Aby to zrobić, należy utworzyć częściowej klasy, które odpowiadają typów jednostek.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, zaznacz **Dodaj odwołanie**i Dodaj odwołanie do `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

W *DAL* folder, Utwórz nowy plik klasy, nadaj jej nazwę *Student.cs*i Zastąp następujący kod w niej kod szablonu.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Ten kod tworzy klasę częściową dla `Student` jednostki. `MetadataType` Atrybut zastosowany do tej klasy częściowej identyfikuje klasy używanego do określania metadanych. Klasa metadanych może mieć dowolną nazwę, ale za pomocą nazwy podmiotu oraz "Metadanych" jest typowym rozwiązaniem.

Atrybuty stosowane do właściwości klasy metadanych określ formatowania wiadomości sprawdzania poprawności, reguł i błędów. Atrybuty pokazane mają następujące wyniki:

- `EnrollmentDate`zostanie wyświetlona jako Data (bez time).
- Zarówno nazwa pola musi być 25 znaków lub mniej długości i niestandardowy komunikat o błędzie jest dostępne.
- Zarówno nazwa pola, które są wymagane, a podano niestandardowy komunikat o błędzie.

Uruchom *Students.aspx* ponownie strony i zobacz, czy dane są teraz wyświetlane bez razy:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Edytuj wiersz i spróbuj wyczyścić wartości w polach Nazwa. Gwiazdek wskazującą błędy pola są wyświetlane, jak pozostawić pole, przed kliknięciem przycisku **aktualizacji**. Po kliknięciu **aktualizacji**, zostaje wyświetlona strona tekst komunikatu o błędzie określony.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Spróbuj wprowadzić nazwy, które są dłuższe niż 25 znaków, kliknij przycisk **aktualizacji**, i zostaje wyświetlona strona tekst komunikatu o błędzie określony.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Teraz, po skonfigurowaniu tych reguł formatowania i walidacji w metadanych modelu danych, zasady zostaną automatycznie zastosowane na każdej stronie, która wyświetla lub pozwalają na zmiany w tych polach tak długo, jak używasz `DynamicControl` lub `DynamicField` kontrolki. Zmniejsza to ilość nadmiarowy kod, który trzeba napisać, co sprawia, że programowanie i testowanie, i zapewnia formatowania danych i weryfikacja są spójne w całej aplikacji.

## <a name="more-information"></a>Więcej informacji

Zakończenie tej serii samouczków na wprowadzenie do korzystania z programu Entity Framework. Aby uzyskać więcej zasobów, aby ułatwić korzystanie z programu Entity Framework, kontynuuj [pierwszy samouczek w następnej serii samouczka programu Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) lub odwiedź następującą witrynę:

- [Entity Framework często zadawane pytania](http://www.ef-faq.org/introduction.html)
- [Blog zespołu programu Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework w bibliotece MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework w Centrum deweloperów MSDN danych](https://msdn.microsoft.com/data/ef.aspx)
- [Informacje o formancie serwera sieci Web obiektu EntityDataSource w bibliotece MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Formant EntityDataSource dokumentacja interfejsu API w bibliotece MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework forum w witrynie MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog Julie Lerman](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[Poprzednie](the-entity-framework-and-aspnet-getting-started-part-7.md)
