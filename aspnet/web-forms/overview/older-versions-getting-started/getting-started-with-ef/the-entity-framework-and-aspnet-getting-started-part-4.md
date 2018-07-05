---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 4 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 9783e30fdabe17958690d178ef95616cb72a8164
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380331"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 4
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Praca z powiązanych danych

W poprzednim samouczku użyto `EntityDataSource` kontrolki filtrowanie, sortowanie i grupowanie danych. W tym samouczku możesz wyświetlić i aktualizowanie powiązanych danych.

Utworzysz strony instruktorów, który wyświetla listę instruktorów. Po wybraniu pod kierunkiem instruktora, zobaczysz listę kursom prowadzonym przez instruktora tego. Po wybraniu kurs, zobaczysz szczegółowe informacje dotyczące kurs i listę uczniów zarejestrowane w ramach tego kursu. Można edytować nazwę przez instruktorów, data zatrudnienia i przypisania pakietu office. Przypisanie pakietu office to zestaw osobne jednostki, które są dostępne za pośrednictwem właściwości nawigacji.

Szczegółowe dane w znacznikach lub w kodzie można połączyć danych głównych. W tej części samouczka użyjesz obu tych metod.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Wyświetlanie i aktualizowanie powiązanych jednostek w kontrolce GridView

Utwórz nową stronę sieci web o nazwie *Instructors.aspx* , który używa *Site.Master* strony wzorcowej, a następnie dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Ten kod znaczników tworzy `EntityDataSource` formant, który wybiera instruktorów i umożliwia aktualizacji. `div` Element konfiguruje znaczników renderowania po lewej stronie, tak aby później możesz dodać kolumnę po prawej stronie.

Między `EntityDataSource` znaczników i zamknięcie `</div>` tag, Dodaj następujący kod znaczników, który tworzy `GridView` kontroli i `Label` formantu, którego będziesz używać dla komunikatów o błędach:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

To `GridView` kontroli umożliwia wybór wiersza wyróżnia wybranego wiersza przy użyciu koloru światła szarego tła i określa programy obsługi (które utworzysz w dalszej części) `SelectedIndexChanged` i `Updating` zdarzenia. Określa również `PersonID` dla `DataKeyNames` właściwości, tak aby wartość klucza wybranego wiersza, może być przekazywany do innego formantu, który będzie później dodać.

Ostatnia kolumna zawiera przypisania pakietu office przez instruktorów, który jest przechowywany we właściwości nawigacji `Person` jednostki, ponieważ pochodzi on z skojarzona jednostka. Należy zauważyć, że `EditItemTemplate` element Określa `Eval` zamiast `Bind`, ponieważ `GridView` kontroli bezpośrednio nie można powiązać właściwości nawigacji, aby je zaktualizować. Zaktualizujesz biuro w kodzie. Aby to zrobić, należy odwołanie do `TextBox` kontroli, a uzyskasz i zapisać w `TextBox` kontrolki `Init` zdarzeń.

Następujące `GridView` formant jest `Label` formant, który jest używany dla komunikatów o błędach. Kontrolki `Visible` właściwość `false`, a stan widoku jest wyłączona, tak aby etykiety będą wyświetlane tylko kiedy kod sprawia, że widoczny w odpowiedzi na błąd.

Otwórz *Instructors.aspx.cs* pliku i Dodaj następujący kod `using` instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Dodaj pole Klasa prywatna bezpośrednio po deklaracji nazwy częściowej klasy do przechowywania odwołań do pola tekstowego przypisania pakietu office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Dodaj wycinkiem `SelectedIndexChanged` na później dodasz kod procedury obsługi zdarzeń. Również dodać program obsługi do przypisania pakietu office `TextBox` kontrolki `Init` zdarzeń, dzięki czemu można przechowywać odwołanie do `TextBox` kontroli. Użyjesz tego odwołania można pobrać wartości które użytkownik wprowadził w celu zaktualizowania jednostki skojarzony z właściwością nawigacji.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Użyjesz `GridView` kontrolki `Updating` zdarzenie, aby zaktualizować `Location` właściwości skojarzonego `OfficeAssignment` jednostki. Dodaj następujący program obsługi dla `Updating` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Ten kod jest uruchamiany, gdy użytkownik kliknie **aktualizacji** w `GridView` wiersza. Kod używa składnik LINQ to Entities, aby pobrać `OfficeAssignment` jednostki, która jest skojarzona z bieżącą `Person` jednostki przy użyciu `PersonID` wybranego wiersza z argumentu zdarzenia.

Kod bierze pod jedną z następujących czynności w zależności od wartości w `InstructorOfficeTextBox` sterowania:

- Jeśli pole ma wartość i nie `OfficeAssignment` jednostki do zaktualizowania, tworzony jest jeden.
- Jeśli pole ma wartość i `OfficeAssignment` jednostki, aktualizuje `Location` wartości właściwości.
- Jeśli pole tekstowe jest puste i `OfficeAssignment` jednostka istnieje, jej usunięcie jednostki.

Następnie zapisuje zmiany w bazie danych. Jeśli wystąpi wyjątek, wyświetla komunikat o błędzie.

Uruchom stronę.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Kliknij przycisk **Edytuj** i zmiana wszystkich pól do pól tekstowych.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Zmienić dowolne z tych wartości, w tym **Biuro**. Kliknij przycisk **aktualizacji** zmiany zostaną uwzględnione na liście zostaną wyświetlone.

## <a name="displaying-related-entities-in-a-separate-control"></a>Wyświetlanie powiązanych jednostek w oddzielnej kontrolce

Każdy instruktora nauczyć co najmniej jeden kursów, więc należy dodać `EntityDataSource` kontroli i `GridView` kontrolki Lista kursów skojarzone z dowolnego instruktora wybrano Instruktorzy `GridView` kontroli. Aby utworzyć nagłówek i `EntityDataSource` dla jednostek kursów i Dodaj następujący kod między komunikat o błędzie `Label` kontroli i zamknięcie `</div>` tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametru zawiera wartość `PersonID` instruktora, w których wiersz jest zaznaczony w `InstructorsGridView` kontroli. `Where` Właściwość zawiera polecenia wyboru podrzędnego, który pobiera wszystkie skojarzone `Person` jednostek z `Course` jednostki `People` właściwość nawigacji i wybiera `Course` jednostki tylko wtedy, gdy jeden skojarzone `Person`jednostki zawiera wybrane `PersonID` wartość.

Aby utworzyć `GridView` kontroli. Dodaj następujący kod bezpośrednio po `CoursesEntityDataSource` kontroli (przed zamknięciem `</div>` tag):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Ponieważ kursy nie będzie wyświetlana, jeśli wybrano opcję nie przez instruktorów, `EmptyDataTemplate` element jest dołączony.

Uruchom stronę.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wybierz instruktora, który ma co najmniej jeden kursy przypisana, a kursu lub kursy są wyświetlane na liście. (Uwaga: mimo że schemat bazy danych pozwala wielu kursów, w dostarczonych z bazą danych danych test nie instruktora faktycznie zawiera więcej niż jednego kursu. Można dodać kursy w bazie danych samodzielnie przy użyciu **Eksploratora serwera** okna lub *CoursesAdd.aspx* strony, która zostanie dodana później w samouczku.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Kontrolka pokazuje tylko kilka pól kursu. Aby wyświetlić wszystkie szczegóły na kurs, użyjesz `DetailsView` kontroli na kursie, wybierany przez użytkownika. W *Instructors.aspx*, Dodaj następujący kod po zamykającym `</div>` tag (Upewnij się, umieść ten kod znaczników **po** iloraz zamknięciem tagu, nie wcześniej niż jego):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Ten kod znaczników tworzy `EntityDataSource` formant, który jest powiązany z `Courses` zestawu jednostek. `Where` Właściwość wybiera kurs przy użyciu `CourseID` wartość wybranego wiersza w kursach `GridView` kontroli. Znaczniki Określa program obsługi `Selected` zdarzenie, które będą używane później do wyświetlania ocen studentów, który jest inny poziom niżej w hierarchii.

W *Instructors.aspx.cs*, utwórz następujące klasy zastępczej dla `CourseDetailsEntityDataSource_Selected` metody. (Będziesz wypełniania tego wycinka w dalszej części tego samouczka; teraz należy ją tak, aby strona będzie skompilować i uruchomić).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Uruchom stronę.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Ponieważ wybrano nie kursów Brak początkowo szczegółów kursu. Wybierz instruktora, który ma przypisane kurs, a następnie wybierz kurs, aby wyświetlić szczegóły.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Za pomocą EntityDataSource "wybrane" zdarzenie, aby wyświetlić powiązane z nimi dane

Ponadto chcesz pokazać wszystkich uczniów zarejestrowane i ich ocen dla wybranych kursów. Aby to zrobić, użyjesz `Selected` zdarzenia `EntityDataSource` formant powiązany z kursu `DetailsView`.

W *Instructors.aspx*, Dodaj następujący kod po `DetailsView` sterowania:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Ten kod znaczników tworzy `ListView` formant, który wyświetla listę uczniów i ich ocen dla wybranych kursów. Zostanie określone żadne źródło danych, ponieważ należy powiązać z danymi kontrolki w kodzie. `EmptyDataTemplate` Element udostępnia komunikat do wyświetlenia po wybraniu nie kurs — w takim przypadku nie istnieją żadne studentów, aby wyświetlić. `LayoutTemplate` Element tworzy tabelę HTML, aby wyświetlić listę, a `ItemTemplate` Określa kolumny do wyświetlenia. Identyfikator dla uczniów i klasy korporacyjnej dla uczniów pochodzą z `StudentGrade` jednostki i nazwę dla uczniów pochodzi z `Person` jednostki, która udostępnia Entity Framework w `Person` właściwość nawigacji `StudentGrade` jednostki.

W *Instructors.aspx.cs*, Zastąp zastąpić jej metodą zastępczą poza `CourseDetailsEntityDataSource_Selected` metoda następującym kodem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Argument zdarzenia dla tego zdarzenia zawiera wybrane dane w postaci kolekcji, które będą miały zawierały elementów, jeśli nic nie jest zaznaczone, lub jeden element Jeśli `Course` wybrano jednostki. Jeśli `Course` podmiot jest zaznaczone, kod używa `First` metodę, aby przekonwertować kolekcji do pojedynczego obiektu. Następnie pobiera `StudentGrade` jednostek z właściwości nawigacji powoduje ich konwersję do kolekcji i wiąże `GradesListView` sterowania do kolekcji.

Jest to wystarczające, aby wyświetlić klasami, ale chcesz upewnić się, że wiadomości w szablonie pustymi danymi jest wyświetlany, gdy ta strona jest wyświetlana po raz pierwszy, i zawsze, gdy nie wybrano kursu. Aby to zrobić, utwórz następującą metodę wywołasz z dwóch miejsc:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Wywołanie tej nowej metody z `Page_Load` metodę, aby wyświetlić czas pierwszego szablonu pustymi danymi, ta strona jest wyświetlana. I wywołać go z `InstructorsGridView_SelectedIndexChanged` metody, ponieważ to zdarzenie jest wywoływane, gdy wybrano pod kierunkiem instruktora, co oznacza, że nowe kursy są ładowane do kursy `GridView` kontroli i nie wybrano jeszcze. Poniżej przedstawiono dwa wywołania:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Uruchom stronę.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wybierz pod kierunkiem instruktora, która ma przypisane kurs, a następnie wybierz kursu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Teraz wiesz na kilka sposobów do pracy z powiązanych danych. W następującego samouczka, dowiesz się, jak można dodać relacji między jednostkami istniejących, jak usunąć relacje oraz sposób dodawania nowego obiektu, który ma ustanowioną relację do istniejącej jednostki.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-5.md)
