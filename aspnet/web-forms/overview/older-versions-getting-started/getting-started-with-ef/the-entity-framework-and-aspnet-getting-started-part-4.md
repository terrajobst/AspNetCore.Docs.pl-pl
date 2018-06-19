---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 4 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886872"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET - część 4
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Praca z danymi pokrewne

W poprzednich samouczku użyto `EntityDataSource` formant filtru, sortowania i grupowania danych. W tym samouczku możesz wyświetlić i zaktualizować powiązanych danych.

Utworzysz strony instruktorów, który zawiera listę instruktorów. Po wybraniu instruktora, możesz wyświetlić listę kursy nauczanych przy tym instruktora. Po wybraniu kursu możesz wyświetlić szczegóły dla porach i listy studentów zarejestrowane w toku. Można edytować instruktora nazwa, data zatrudnienia i przypisania pakietu office. Przypisanie pakietu office to zestaw niezależna dostępnej za pośrednictwem właściwości nawigacji.

Możesz połączyć dane główne do szczegółowych danych w znaczniku lub w kodzie. W tej części samouczka użyjesz obu metod.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Wyświetlanie i aktualizowanie powiązanych jednostek w kontrolce GridView

Tworzenie nowej strony sieci web o nazwie *Instructors.aspx* używającą *Site.Master* strony wzorcowej i Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Tworzy tego znacznika `EntityDataSource` formant, który wybiera instruktorów i umożliwia aktualizacji. `div` Element konfiguruje znaczników renderowania po lewej stronie, aby można później dodać kolumnę po prawej stronie.

Między `EntityDataSource` znaczników i zamykania `</div>` tagów, Dodaj następujący kod znaczników, który tworzy `GridView` kontroli i `Label` formant, który ma być używany dla komunikatów o błędach:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

To `GridView` kontroli umożliwia zaznaczenie wiersza, wyróżnia wybranego wiersza z kategorii lekkich szary kolor tła i określa obsługę (które należy utworzyć później) `SelectedIndexChanged` i `Updating` zdarzenia. Określa również `PersonID` dla `DataKeyNames` właściwości, dzięki czemu mogą być przekazywane wartości klucza wybranego wiersza inny formant, który będzie później dodać.

Ostatnia kolumna zawiera instruktora przypisanie pakietu office, które są przechowywane we właściwości nawigacji `Person` jednostki, ponieważ pochodzi on z skojarzonej jednostki. Zwróć uwagę, że `EditItemTemplate` określa element `Eval` zamiast `Bind`, ponieważ `GridView` kontroli bezpośrednio nie można powiązać właściwości nawigacji w celu ich aktualizacji. Będzie aktualizowana przypisania pakietu office w kodzie. Aby to zrobić, należy odwołanie do `TextBox` kontrolki, na które będzie uzyskać i zapisać w `TextBox` formantu `Init` zdarzeń.

Po `GridView` formant jest `Label` formant, który jest używany dla komunikatów o błędach. Kontrolki `Visible` właściwość jest `false`, a stan widoku jest wyłączona, dzięki czemu etykiety będą wyświetlane tylko po kodzie ułatwia widoczny w odpowiedzi na błąd.

Otwórz *Instructors.aspx.cs* pliku i dodaj następującą `using` instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Dodawanie pola Klasa prywatna bezpośrednio po deklaracji Nazwa klasy częściowe do przechowywania odwołanie do pola tekstowego przypisania pakietu office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Dodawanie klasy zastępczej dla `SelectedIndexChanged` obsługi zdarzeń do użycia później dodasz kod. Również dodać obsługę przypisania office `TextBox` formantu `Init` zdarzeń, dzięki czemu można przechowywać odwołanie do `TextBox` formantu. Użyjesz tego odwołania można pobrać wartość użytkownika wprowadzona w celu zaktualizowania jednostki skojarzonej z właściwości nawigacji.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Użyjesz `GridView` formantu `Updating` zdarzeń, aby zaktualizować `Location` właściwości skojarzonego `OfficeAssignment` jednostki. Dodaj następujące obsługę `Updating` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Ten kod jest uruchamiany, gdy użytkownik kliknie **aktualizacji** w `GridView` wiersza. W kodzie użyto LINQ to Entities można pobrać `OfficeAssignment` jednostki, który został skojarzony z bieżącym `Person` jednostki, przy użyciu `PersonID` zaznaczonego wiersza w argumencie zdarzeń.

Kod następnie przyjmuje jeden z następujących czynności w zależności od wartości w `InstructorOfficeTextBox` sterowania:

- Jeśli pole ma wartość i jest nie `OfficeAssignment` jednostki do zaktualizowania, tworzy go.
- Jeśli pole ma wartość i jest `OfficeAssignment` jednostki, aktualizuje `Location` wartości właściwości.
- Jeśli pole tekstowe jest pusta i `OfficeAssignment` jednostka istnieje, usuwa jednostki.

Następnie zapisuje zmiany w bazie danych. Jeśli wystąpi wyjątek, wyświetla komunikat o błędzie.

Uruchom strony.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Kliknij przycisk **Edytuj** i zmień wszystkich pól do pól tekstowych.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Zmienić dowolne z tych wartości, w tym **przypisania Office**. Kliknij przycisk **aktualizacji** , a zmiany zostaną uwzględnione na liście.

## <a name="displaying-related-entities-in-a-separate-control"></a>Wyświetlanie w formancie oddzielne powiązanych jednostek

Każdy instruktora nauczyć szkoleń w co najmniej jednego, należy dodać `EntityDataSource` kontroli i `GridView` formantu Lista kursów skojarzone z dowolną wskazaną instruktora wybrano Instruktorzy `GridView` formantu. W celu utworzenia nagłówka i `EntityDataSource` sterowania dla jednostek kursy, Dodaj następujący kod między komunikat o błędzie `Label` kontroli i zamykania `</div>` tagu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametru zawiera wartość `PersonID` instruktora wiersz, którego jest wybierany w `InstructorsGridView` formantu. `Where` Właściwość zawiera polecenie podrzędnego wyboru, które pobiera wszystkie skojarzone `Person` jednostek z `Course` jednostki `People` właściwość nawigacji i wybiera `Course` jednostki tylko wtedy, gdy jeden z skojarzone`Person`jednostek zawiera wybranego `PersonID` wartość.

Aby utworzyć `GridView` kontroli. Dodaj następujący kod bezpośrednio po `CoursesEntityDataSource` kontroli (przed zamknięciem `</div>` tagu):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Ponieważ kursy nie będzie wyświetlana, jeśli instruktora nie jest zaznaczone, `EmptyDataTemplate` element jest dołączony.

Uruchom strony.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wybierz instruktora, który ma co najmniej jeden kursy przypisany, i kursu lub kursy znajdują się na liście. (Uwaga: mimo że schemat bazy danych umożliwia wielu kursy, w danych testowych dostarczony wraz z bazy danych nie instruktora faktycznie zawiera więcej niż jeden plan. Można dodać szkoleń w bazie danych użytkownika za pomocą **Eksploratora serwera** okna lub *CoursesAdd.aspx* strony, która zostanie dodana w samouczku nowsze.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Formant zawiera tylko kilka pól kursu. Aby wyświetlić szczegóły kursu, użyjesz `DetailsView` kontroli dla porach przez użytkownika. W *Instructors.aspx*, Dodaj następujący kod po upływie `</div>` tag (Upewnij się, umieść ten kod znaczników **po** div zamykający tag, nie przed):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Tworzy tego znacznika `EntityDataSource` formant, który jest powiązany z `Courses` zestawu jednostek. `Where` Właściwości wybiera kursu przy użyciu `CourseID` wartość wybranego wiersza, w tym `GridView` formantu. Kod znaczników Określa program obsługi `Selected` zdarzeń, który zostanie później użyty do wyświetlania ocen studentów, który jest inny poziom w dół hierarchii.

W *Instructors.aspx.cs*, utwórz następujące klasy zastępczej dla `CourseDetailsEntityDataSource_Selected` metody. (Będzie wypełniania tej klasy zastępczej później w samouczku; teraz, należy go tak, aby strona zostanie kompilowanie i uruchamianie).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Uruchom strony.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Ponieważ wybrano nie kursu Brak początkowo szczegółów kursu. Wybierz instruktora, który ma przypisane kursu, a następnie wybierz plan, aby wyświetlić szczegóły.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Przy użyciu obiektu EntityDataSource "zaznaczone" zdarzenia, aby wyświetlić powiązane dane

Na koniec mają być wyświetlane wszystkie zarejestrowane studentów i ich klas dla porach wybrane. Aby to zrobić, użyjesz `Selected` zdarzenie `EntityDataSource` formant powiązany z ciągu `DetailsView`.

W *Instructors.aspx*, Dodaj następujący kod po `DetailsView` sterowania:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Tworzy tego znacznika `ListView` kontrolkę wyświetlającą listę studentów i ich klas dla porach wybrane. Zostanie określone żadne źródło danych, ponieważ będzie databind formantu w kodzie. `EmptyDataTemplate` Komunikat do wyświetlenia po wybraniu kursu nie zawiera elementu — w takim przypadku nie istnieją żadne studentów do wyświetlenia. `LayoutTemplate` Elementu tworzy tabeli HTML do wyświetlenia na liście i `ItemTemplate` Określa kolumny do wyświetlenia. Identyfikator dla użytkowników domowych i uczniów klasy pochodzą z `StudentGrade` oraz typy jednostek i nazwa uczniów pochodzi z `Person` jednostki, która udostępnia programu Entity Framework w `Person` właściwość nawigacji `StudentGrade` jednostki.

W *Instructors.aspx.cs*, Zastąp zastąpić jej metodą zastępczą poza `CourseDetailsEntityDataSource_Selected` metodę z następującym kodem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Argument zdarzenia dla tego zdarzenia zawiera wybranych danych w formularzu kolekcji, które będą miały zawierały elementów, jeśli nic nie zaznaczono lub jeden element Jeśli `Course` wybrano jednostki. Jeśli `Course` wybrano jednostki, w kodzie użyto `First` do przekonwertowania kolekcji do pojedynczego obiektu. Następnie pobiera `StudentGrade` jednostek z właściwości nawigacji konwertuje je do kolekcji i wiąże `GradesListView` sterowania do kolekcji.

Takie ustawienie wystarczy do wyświetlania klasami, ale chce mieć pewność, że ten komunikat w szablonie pusty danych jest wyświetlany po raz pierwszy ta strona jest wyświetlana i zawsze, gdy nie wybrano kursu. W tym celu należy utworzyć następujące metody, która będzie wywoływana z dwóch miejscach:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Wywołanie tej nowej metody z `Page_Load` metodę w celu wyświetlenia czas szablonu pierwszy danych pusta strona jest wyświetlana. I wywołać go z `InstructorsGridView_SelectedIndexChanged` metody ponieważ to zdarzenie jest zgłaszane, gdy wybrano instruktora, co oznacza, że nowe kursy są ładowane do kursów `GridView` kontroli, a nie wybrano jeszcze. Poniżej przedstawiono dwa wywołania:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Uruchom strony.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wybierz instruktora, który ma przypisane kursu, a następnie wybierz kursu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Ma teraz widoczna na kilka sposobów, aby pracować z danymi powiązane. Następujące samouczka, dowiesz się, jak można dodać relacji między obiektami istniejących, jak usunąć relacje i jak dodać nową jednostkę, która ma relację do istniejącego obiektu.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-5.md)
