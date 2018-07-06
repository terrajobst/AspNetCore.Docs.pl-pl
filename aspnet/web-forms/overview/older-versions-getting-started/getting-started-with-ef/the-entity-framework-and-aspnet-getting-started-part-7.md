---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 7 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823410"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 7
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>korzystanie z procedur składowanych

W poprzednim samouczku zaimplementowano wzorzec Tabela wg hierarchii dziedziczenia. Ten samouczek przedstawia sposób użycia procedur składowanych, aby uzyskać większą kontrolę nad dostępem do bazy danych.

Entity Framework pozwala określić, że należy używać procedur składowanych dla dostępu do bazy danych. Dla dowolnego typu jednostki można określić procedury przechowywanej na potrzeby tworzenia, aktualizowania lub usuwania obiektów tego typu. Następnie w modelu danych, można dodać odwołania do procedur przechowywanych, które służy do wykonywania zadań takich jak pobieranie zestawów jednostek.

Korzystanie z procedur składowanych jest typowym wymogiem dla dostępu do bazy danych. W niektórych przypadkach administrator bazy danych może wymagać, że wszystkie dostępu do bazy danych przechodzą przez procedury składowane ze względów bezpieczeństwa. W innych przypadkach możesz chcieć Wbudowywanie logiki biznesowej w niektórych procesów, które używa programu Entity Framework, gdy powoduje zaktualizowanie bazy danych. Na przykład natychmiast po usunięciu jednostki możesz skopiować go do bazy danych archiwum. Lub przy każdej aktualizacji wiersza należy zapisywać wiersz do tabeli rejestrowania, który rejestruje, którzy wprowadziła zmianę. Tego rodzaju zadania można wykonać procedury przechowywanej, która jest wywoływana zawsze wtedy, gdy platforma Entity Framework usunięcie jednostki lub aktualizuje jednostkę.

Tak jak w poprzednim samouczku nie utworzysz nowych stron. Zamiast tego zmienisz sposób Entity Framework uzyskuje dostęp do bazy danych dla niektórych stron już utworzony.

W tym samouczku utworzysz procedur składowanych w bazie danych wstawianiem `Student` i `Instructor` jednostek. Dodasz je do modelu danych, a następnie określisz, że platforma Entity Framework powinny być używane do dodawania `Student` i `Instructor` jednostek w bazie danych. Utworzysz też procedury przechowywanej, która służy do pobierania `Course` jednostek.

## <a name="creating-stored-procedures-in-the-database"></a>Tworzenie procedur składowanych w bazie danych

(Jeśli używasz *School.mdf* plik z projektu dostępne do pobrania w tym samouczku, można pominąć tę sekcję, ponieważ procedury składowane już istnieje.)

W **Eksploratora serwera**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **procedur składowanych**i wybierz **Dodaj nową procedurę przechowywaną**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Skopiuj następujące instrukcje SQL i je wkleić do okna procedury składowanej, zastępując szkielet procedury składowanej.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` jednostki mają cztery właściwości: `PersonID`, `LastName`, `FirstName`, i `EnrollmentDate`. Baza danych automatycznie generuje wartości Identyfikatora i procedury składowanej akceptuje parametry dla pozostałych trzech. Procedura składowana ma zwracać wartości klucza rekordu nowy wiersz, aby Entity Framework można zachować informacje o który w wersji usługi jednostki, który przechowuje w pamięci.

Zapisz i zamknij okno procedury składowanej.

Utwórz `InsertInstructor` procedurę składowaną w taki sam sposób, przy użyciu następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Tworzenie `Update` przechowywane procedury `Student` i `Instructor` jednostek również. (Baza danych już zawiera `DeletePerson` przechowywane procedury, która będzie działać dla obu `Instructor` i `Student` jednostek.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

W tym samouczku będziesz mapować wszystkie trzy funkcje — insert, update i delete — dla każdego typu jednostki. Entity Framework w wersji 4 pozwala na mapowanie jeden lub dwa pola spośród wymienionych funkcji do procedur składowanych bez mapowania inne osoby, z jednym wyjątkiem: Jeśli mapujesz funkcja aktualizacji, ale nie funkcji usuwania programu Entity Framework spowoduje zgłoszenie wyjątku po użytkownik Spróbuj usunąć jednostkę. W programu Entity Framework w wersji 3.5, nie masz tyle samo elastyczność mapowanie procedur składowanych: Jeśli zamapowany jedna funkcja należało do mapowania wszystkich trzech środowiskach.

Aby utworzyć procedurę składowaną, która odczytuje, a nie aktualizuje dane, utwórz taki, który zaznacza wszystkie `Course` jednostki, przy użyciu następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Dodawanie procedur składowanych do modelu danych

Teraz zdefiniowano procedur składowanych w bazie danych, ale muszą zostać dodane do modelu danych, aby były one dostępne w programie Entity Framework. Otwórz *SchoolModel.edmx*, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz **Model aktualizacji z bazy danych**. W **Dodaj** karcie **wybierz obiekty bazy danych** okna dialogowego rozwiń **procedur składowanych**, wybierz nowo utworzony procedur składowanych i `DeletePerson` procedurę składowaną, a następnie kliknij przycisk **Zakończ**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapowanie procedur składowanych

W Projektancie modeli danych, kliknij prawym przyciskiem myszy `Student` jednostki, a następnie wybierz pozycję **mapowanie procedur przechowywanych**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Szczegóły mapowania** pojawi się okno, w którym można określić procedur składowanych, które Wstawianie, aktualizowanie i usuwanie obiektów tego typu należy używać programu Entity Framework.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Ustaw **Wstaw** funkcja **InsertStudent**. Okno Lista parametrów procedury składowanej, z których każdy muszą być zamapowane na właściwości jednostki. Dwa pola spośród wymienionych są automatycznie mapowane ponieważ nazwy są takie same. Nie ma właściwości jednostki o nazwie `FirstName`, dlatego należy ręcznie wybrać `FirstMidName` z listy rozwijanej, która pokazuje właściwości dostępne jednostki. (Jest to spowodowane zmieniona jego nazwa `FirstName` właściwości `FirstMidName` w pierwszym samouczku.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

W tym samym **szczegóły mapowania** okna, mapa `Update` funkcja `UpdateStudent` procedury składowanej (Upewnij się, że `FirstMidName` jako wartość parametru `FirstName`, ponieważ została ona `Insert` procedurę składowaną) i `Delete` funkcja `DeletePerson` procedury składowanej.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Postępuj zgodnie z tą samą procedurą, aby zamapować insert, update i delete procedur składowanych dla instruktorów, aby `Instructor` jednostki.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Procedury składowane, które odczytują zamiast aktualizacji danych, możesz użyć **przeglądarka modeli** okna, aby zamapować procedury składowanej do jednostki typu zwraca. W Projektancie modeli danych, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **przeglądarka modeli**. Otwórz **SchoolModel.Store** węzeł, a następnie otwórz **procedur składowanych** węzła. Kliknij prawym przyciskiem myszy `GetCourses` przechowywane procedury, a następnie wybierz pozycję **Dodaj importowanie funkcji**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

W **Dodaj importowanie funkcji** okno dialogowe, w obszarze **zwraca kolekcję z** wybierz **jednostek**, a następnie wybierz `Course` jako typ jednostki zwracane. Gdy wszystko będzie gotowe, kliknij przycisk **OK**. Zapisz i Zamknij *edmx* pliku.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Za pomocą Wstawianie, aktualizowanie i usuwanie procedur składowanych

Procedur przechowywanych do wstawiania, aktualizowania i usuwania danych używanych przez Entity Framework automatycznie po dodaniu ich do modelu danych i mapowane je do odpowiednich jednostek. Możesz teraz uruchomić *StudentsAdd.aspx* stronę, i za każdym razem, gdy tworzysz nowego Studenta, korzystających z programu Entity Framework `InsertStudent` przechowywane procedury, aby dodać nowy wiersz `Student` tabeli.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Uruchom *Students.aspx* strony i nowego studenta pojawia się na liście.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Zmień nazwę aby sprawdzić, czy funkcja aktualizacji działa, a następnie usuń ucznia, aby sprawdzić, czy funkcja usuwania działa.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Korzystanie z procedur składowanych wybierz

Entity Framework nie jest automatycznie uruchamiany procedur składowanych takich jak `GetCourses`, i nie można używać ich za pomocą `EntityDataSource` kontroli. Z nich korzystać, możesz je wywołać z kodu.

Otwórz *InstructorsCourses.aspx.cs* pliku. `PopulateDropDownLists` Metoda korzysta z zapytania LINQ do jednostek do pobrania wszystkich jednostek kurs, aby go w pętli poprzez listy i określić, które z nich pod kierunkiem instruktora jest przypisany do i te, które są nieprzypisane:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Zamień ten element następujący kod:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Strona używa teraz `GetCourses` przechowywane procedury, aby pobrać listę wszystkich kursów. Uruchom stronę, aby sprawdzić, czy działa tak jak poprzednio.

(Właściwości nawigacji jednostki pobierane przez procedurę składowaną może nie być automatycznie wypełniona dane związane z tych jednostek, w zależności od `ObjectContext` ustawienia domyślne. Aby uzyskać więcej informacji, zobacz [ładowanie powiązanych obiektów](https://msdn.microsoft.com/library/bb896272.aspx) w bibliotece MSDN.)

W następnym samouczku dowiesz się, jak ułatwiają programów i testów formatowania i sprawdzania poprawności reguły danych przy użyciu funkcji danych dynamicznych. Zamiast określania każdej reguły strony sieci web, takie jak ciągi formatów danych i czy pole jest wymagane, można określić te zasady w metadanych modelu danych i są one automatycznie stosowane na każdej stronie.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-8.md)
