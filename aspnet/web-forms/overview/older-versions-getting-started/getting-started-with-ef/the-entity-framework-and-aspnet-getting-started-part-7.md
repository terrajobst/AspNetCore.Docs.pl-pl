---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 7 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: cb84f4f3e130fedb3e2f1a17d630767ff65bfa05
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886982"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET - część 7
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>korzystanie z procedur składowanych

W poprzednich instrukcji zaimplementowano wzorzec tabeli na hierarchię dziedziczenia. Ten samouczek przedstawia sposób użycia procedur składowanych, aby uzyskać większą kontrolę nad dostęp do bazy danych.

Entity Framework pozwala określić, że należy używać procedur składowanych na dostęp do bazy danych. Dla każdego typu jednostki można określić procedury składowanej do użycia na potrzeby tworzenia, aktualizowania lub usuwania obiektów tego typu. Następnie w modelu danych, można dodać odwołania do procedur składowanych, które umożliwiają wykonywanie zadań takich jak pobieranie zestawów jednostek.

Korzystanie z procedur składowanych jest typowe wymagania dotyczące dostępu do bazy danych. W niektórych przypadkach administrator bazy danych może wymaga dostępu do wszystkich baz danych za pomocą procedur składowanych ze względów bezpieczeństwa. W innych przypadkach można wbudować logiki biznesowej niektórych procesów, które używa programu Entity Framework w momencie aktualizacji bazy danych. Na przykład gdy usunięciu jednostki można skopiować go do bazy danych archiwum. Lub przy każdej aktualizacji wiersz należy zapisać wiersz do tabeli rejestrowania tego rekordów, którzy dokonali zmian. Następujące rodzaje zadań można wykonać procedury przechowywanej, która jest wywoływana, gdy programu Entity Framework usunięcie jednostki lub aktualizuje jednostkę.

Jak w poprzednich samouczka utworzysz nie nowych stron. Zamiast tego będzie zmienić sposób programu Entity Framework uzyskuje dostęp do bazy danych dla niektórych stron już utworzone.

W tym samouczku utworzysz procedur przechowywanych w bazie danych podczas wstawiania `Student` i `Instructor` jednostek. Zostaną dodane do modelu danych, a następnie określisz, że programu Entity Framework powinny być używane do dodawania `Student` i `Instructor` jednostek w bazie danych. Utworzysz także procedury przechowywanej, która służy do pobierania `Course` jednostek.

## <a name="creating-stored-procedures-in-the-database"></a>Tworzenie procedur przechowywanych w bazie danych

(Jeśli używasz *School.mdf* plik z projektu można pobrać z tego samouczka można pominąć tę sekcję, ponieważ istnieją już procedur składowanych.)

W **Eksploratora serwera**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **procedur składowanych**i wybierz **Dodaj nowe procedury składowanej**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Skopiuj następujące instrukcje SQL i wklej je do okna procedury składowanej, zastępując szkielet procedury składowanej.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` jednostki ma właściwości cztery: `PersonID`, `LastName`, `FirstName`, i `EnrollmentDate`. Bazy danych automatycznie generuje wartość Identyfikatora i procedury składowanej akceptuje parametry dla innych trzech. Procedura składowana ma zwracać wartości klucza rekordu nowego wiersza, aby programu Entity Framework można zachować informacje o który w wersji jednostkę, która zapewnia w pamięci.

Zapisz i zamknij okno procedury składowanej.

Utwórz `InsertInstructor` procedury składowanej w taki sam sposób, przy użyciu następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Utwórz `Update` przechowywane procedury `Student` i `Instructor` jednostek również. (Baza danych już zawiera `DeletePerson` przechowywane procedury, która będzie działać dla obu `Instructor` i `Student` jednostek.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

W tym samouczku będziesz mapować wszystkie trzy funkcje--insert, update i delete — dla poszczególnych typów jednostek. Entity Framework w wersji 4 pozwala na mapowanie co najmniej jeden lub dwa z tych funkcji na procedury składowane bez mapowania innych osób, z jednym wyjątkiem: Jeśli mapujesz funkcji aktualizacji, ale nie funkcji usuwania programu Entity Framework spowoduje zgłoszenie wyjątku podczas możesz Próba usunięcia jednostki. W Entity Framework w wersji 3.5 nie miał tym dużą elastyczność w mapowaniu procedur składowanych: Jeśli zamapowany jednej funkcji zostałby wymagane do mapowania wszystkich trzech.

Aby utworzyć procedury przechowywanej, która odczytuje zamiast aktualizuje dane, należy utworzyć, który wybiera wszystkie `Course` jednostek przy użyciu następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Dodawanie procedur składowanych do modelu danych

Procedury składowane są teraz zdefiniowany w bazie danych, ale muszą zostać dodane do modelu danych, aby udostępnić je do narzędzia Entity Framework. Otwórz *SchoolModel.edmx*, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz **modelu aktualizacji z bazy danych**. W **Dodaj** karcie **wybierz obiekty bazy danych użytkownika** okna dialogowego rozwiń **procedur składowanych**, wybierz nowo utworzony procedur składowanych i `DeletePerson` procedury składowanej, a następnie kliknij przycisk **Zakończ**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapowanie procedur składowanych

W Projektancie modelu danych, kliknij prawym przyciskiem myszy `Student` jednostki i wybierz **mapowania procedury przechowywane**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Szczegóły mapowania** zostanie wyświetlone okno, w którym można określić procedur składowanych, które mają zostać użyte do wstawiania, aktualizowania i usuwania jednostek tego typu Entity Framework.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Ustaw **Wstaw** funkcja **InsertStudent**. Okno Lista parametrów procedury składowanej, z których każdy musi być zamapowana na właściwość jednostki. Dwa pola spośród wymienionych są automatycznie mapowane ponieważ nazwy są takie same. Nie ma właściwości jednostki o nazwie `FirstName`, dlatego należy ręcznie wybrać `FirstMidName` z listy rozwijanej, która zawiera właściwości dostępne jednostki. (Jest to spowodowane zmieniona jego nazwa `FirstName` właściwości `FirstMidName` w pierwszym samouczku.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

W tym samym **szczegóły mapowania** okna, mapy `Update` funkcja `UpdateStudent` procedury składowanej (Upewnij się, że możesz określić `FirstMidName` jako wartość parametru `FirstName`, ponieważ została ona `Insert` procedurę składowaną) i `Delete` funkcja `DeletePerson` procedury składowanej.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Postępuj zgodnie z tą samą procedurą, aby zamapować insert, update i delete procedury składowane instruktorów do `Instructor` jednostki.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Dla procedur składowanych, które zapoznały zamiast danych aktualizacji, możesz użyć **przeglądarki modelu** okna do mapowania procedury składowanej jednostki typu zwraca. W Projektancie modelu danych, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz pozycję **przeglądarki modelu**. Otwórz **SchoolModel.Store** węzeł, a następnie otwórz **procedur składowanych** węzła. Kliknij prawym przyciskiem myszy `GetCourses` przechowywane procedury i wybierz **dodać importu funkcji**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

W **dodać importu funkcji** okna dialogowego, w obszarze **zwraca kolekcję z** wybierz **jednostek**, a następnie wybierz `Course` jako typ jednostki zwracana. Gdy wszystko będzie gotowe, kliknij przycisk **OK**. Zapisz i Zamknij *edmx* pliku.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Za pomocą wstawiania, aktualizowania i usuwania procedur składowanych

Procedury składowane do wstawiania, aktualizowania i usuwania danych są używane automatycznie przez program Entity Framework w po mapować je do odpowiednich jednostek i dodać je do modelu danych. Teraz możesz uruchomić *StudentsAdd.aspx* stronę, i przy tworzeniu nowych uczniów korzysta z programu Entity Framework `InsertStudent` przechowywane procedury, aby dodać nowy wiersz do `Student` tabeli.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Uruchom *Students.aspx* strony i nowych uczniów pojawią się na liście.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Zmień nazwę, aby sprawdzić, czy funkcja aktualizacji działa, a następnie usuń uczniów, aby sprawdzić, czy działa funkcja usuwania.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Korzystanie z procedur składowanych Select

Entity Framework nie uruchamia automatycznie procedur składowanych takich jak `GetCourses`, i nie można używać ich z `EntityDataSource` formantu. Aby korzystać z nich, można skontaktuj się z kodu.

Otwórz *InstructorsCourses.aspx.cs* pliku. `PopulateDropDownLists` Metoda używa zapytania LINQ do jednostek do pobrania wszystkich jednostek kursu tak, aby można przeglądać listę i ustalić, które przypisano instruktora i te, które są nieprzypisane:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Zamień ten element następujący kod:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Strona używa teraz `GetCourses` przechowywane procedury, aby pobrać listę wszystkich kursów. Uruchomić stronę, aby sprawdzić, czy działa tak jak poprzednio.

(Właściwości nawigacji jednostki pobierane przez procedurę składowaną może nie automatycznie wypełniane przy użyciu danych związanych z tymi podmiotami, w zależności od `ObjectContext` ustawienia domyślne. Aby uzyskać więcej informacji, zobacz [ładowanie powiązanych obiektów](https://msdn.microsoft.com/library/bb896272.aspx) w bibliotece MSDN.)

W następnym samouczku nauczysz się, jak ułatwić reguły programu i badanie danych formatowania i walidacji przy użyciu funkcji danych dynamicznych. Zamiast określania na każdej reguły strony sieci web, na przykład ciągi formatu danych i czy pole jest wymagane, takie reguły można określić w metadanych modelu danych i są automatycznie stosowane na każdej stronie.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-8.md)
