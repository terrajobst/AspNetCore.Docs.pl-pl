---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Wdrażanie dziedziczenia z programu Entity Framework w aplikacji platformy ASP.NET MVC (8, 10) | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 54e46c6f996b6fe86a227c851562e61678b02780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Wdrażanie dziedziczenia z programu Entity Framework w aplikacji platformy ASP.NET MVC (8, 10)
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Samouczek poprzedniej służy do obsługi wyjątków współbieżności. Ten samouczek przedstawia sposób wykonania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo można użyć dziedziczenia, aby wyeliminować nadmiarowy kod. W tym samouczku zostanie zmieniona `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowa klasa, która zawiera właściwości, takie jak `LastName` które są wspólne dla uczniów lub studentów i instruktorów. Nie dodać lub zmienić żadnych stron sieci web, ale zostanie zmieniona części kodu i te zmiany są automatycznie odzwierciedlane w bazie danych.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela na hierarchii i dziedziczenie tabeli według typu

W programowanie zorientowane obiektowo dziedziczenia służy do ułatwienia pracy z powiązanymi klasami. Na przykład `Instructor` i `Student` klas w `School` modelu danych udostępniać kilka właściwości, które powoduje nadmiarowy kod:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są udostępniane przez `Instructor` i `Student` jednostek. Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy podstawowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Istnieje kilka metod, które ta struktura dziedziczenia może być reprezentowane w bazie danych. Może mieć `Person` tabeli, która zawiera informacje o studentów i instruktorów w jednej tabeli. Niektóre kolumny można zastosować tylko do instruktorów (`HireDate`), niektórych tylko dla uczniów lub studentów (`EnrollmentDate`), zarówno niektóre do (`LastName`, `FirstName`). Zazwyczaj konieczne będzie *rozróżniacza* reprezentuje kolumny, która wskazuje typ każdego wiersza. Na przykład kolumny rozróżniacza mogą mieć "Instruktora" dla szkolenia i "Uczniów" dla uczniów lub studentów.

![Tabela na hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Ten wzorzec generowania strukturę jednostek dziedziczenia z tabeli pojedynczej bazy danych jest nazywany *tabeli na hierarchii* dziedziczenia (TPH).

Alternatywą jest wyglądu struktury dziedziczenia bazę danych. Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.

![Tabela na type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Ten wzorzec dokonywania tabeli bazy danych w każdej klasie jednostki jest nazywana *tabel na typ* dziedziczenia (TPT).

Wzorce dziedziczenia TPH zazwyczaj zapewniają lepszą wydajność w ramach jednostki niż w przypadku wzorców dziedziczenia TPT wzorce TPT może powodować sprzężenia złożonych zapytań. Ten samouczek pokazuje, jak zaimplementować dziedziczenia TPH. Należy to zrobić, wykonując następujące czynności:

- Utwórz `Person` klasy i zmień `Instructor` i `Student` pochodzi od klasy `Person`.
- Dodaj mapowanie modelu w bazie danych kod do klasy kontekstu bazy danych.
- Zmień `InstructorID` i `StudentID` odwołań w projekcie do `PersonID`.

## <a name="creating-the-person-class"></a>Tworzenie klasy osoby

 Uwaga: Nie można skompilować projekt po utworzeniu klasy poniżej, dopóki nie zostanie zaktualizowany kontrolery, które korzysta z tych klas. 

W *modele* folderu, Utwórz *Person.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

W *Instructor.cs*, pochodzi `Instructor` klasę z `Person` klasy i usunąć pola klucza i nazwę. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Podobne zmiany do *Student.cs*. `Student` Klasa będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Dodanie osoby typu jednostki do modelu

W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, który programu Entity Framework wymaga, aby skonfigurować tabelę dla hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych jest ponownie utworzyć jej `Person` tabeli zamiast `Student` i `Instructor` tabel.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Zmiana InstructorID i StudentID PersonID

W *SchoolContext.cs*, w instrukcji mapowania instruktora kursu, zmień `MapRightKey("InstructorID")` do `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Ta zmiana nie jest wymagane; zmienia jedynie nazwę kolumny InstructorID w tabeli sprzężenia wiele do wielu. Jeśli nazwa jako InstructorID, aplikacja będzie nadal działać poprawnie. W tym miejscu jest wypełniony *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Następnie należy zmienić `InstructorID` do `PersonID` i `StudentID` do `PersonID` w projekcie ***z wyjątkiem*** w plikach z sygnaturami czasowymi migracje *migracje* folder. W tym celu można będzie znaleźć otwierać tylko pliki, które muszą zostać zmienione, a następnie wykonać globalne zmiany w otwartych plików. To jedyny plik w *migracje* należy zmienić folder jest *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
 > Rozpocznij od zamknięcie wszystkich otwartych plików w programie Visual Studio.
2. Kliknij przycisk **Znajdź i Zamień — Znajdź wszystkie pliki** w **Edytuj** menu, a następnie wyszukaj wszystkie pliki w projekcie, które zawierają `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Otwórz każdy plik w **znaleźć wyników** okna ***z wyjątkiem*** &lt;sygnatury czasowej&gt;*\_.cs* migracji plików w *Migracje* folderu, klikając jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Otwórz **Zastąp w plikach** okno dialogowe i zmień **Szukaj w** do **wszystkie otwarte dokumenty**.
5. Użyj **Zastąp w plikach** okna dialogowego, aby zmienić wszystkie `InstructorID` do`PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Znajdź wszystkie pliki w projekcie, który zawiera `StudentID`.
7. Otwórz każdy plik w **znaleźć wyników** okna ***z wyjątkiem*** &lt;sygnatury czasowej&gt;*\_\*.cs* plików migracji w *migracje* folderu, klikając jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Otwórz **Zastąp w plikach** okno dialogowe i zmień **Szukaj w** do **wszystkie otwarte dokumenty**.
9. Użyj **Zastąp w plikach** okna dialogowego, aby zmienić wszystkie `StudentID` do `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Skompiluj projekt.

(Należy pamiętać, że oznacza to *wadą* z `classnameID` wzorzec nazewnictwa kluczy podstawowych. Jeśli klucze podstawowe identyfikator ma nazwę bez prefiksu nazwy klasy *nie* zmiana nazwy konieczna może być teraz.)

## <a name="create-and-update-a-migrations-file"></a>Tworzenie i aktualizowanie plików migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` w kryterium. Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie może ustalić sposób obsługi. Otrzymasz następujący błąd:

*Instrukcja ALTER TABLE jest w konflikcie z ograniczenie FOREIGN KEY "klucz OBCY\_dbo. Dział\_dbo. Osoba\_PersonID ". Konflikt wystąpił w bazie danych "ContosoUniversity" tabeli "dbo. Osoba", kolumna"PersonID".*

Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metodę z następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Uruchom `update-database` polecenie ponownie.

> [!NOTE]
> Istnieje możliwość pobrania inne błędy podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji nie można rozwiązać, zmieniając w ciągu połączenia można kontynuować samouczka *Web.config* pliku lub usuwania bazy danych. Najprostsza metoda jest można zmienić nazwy bazy danych w *Web.config* pliku. Na przykład zmienić nazwę bazy danych do aktualizacji zbiorczej\_testu, jak pokazano w poniższym przykładzie:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Z nową bazę danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenie jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). W przypadku zastosowania tego podejścia dalszego z tego samouczka należy pominąć krok wdrożenia na końcu tego samouczka, ponieważ wdrożonej witrynie otrzyma ten sam błąd podczas automatycznego uruchamiania migracji. Jeśli chcesz rozwiązać problem migracje najlepsze zasobu jest jednym z fora programu Entity Framework lub StackOverflow.com.


## <a name="testing"></a>Testowanie

Uruchom witrynę i spróbuj różnych stron. Wszystko, co działa tak samo, tak jak poprzednio.

W **Eksploratora serwera** rozwiń **SchoolContext** , a następnie **tabel**, i zobaczysz, że **uczniowie** i **instruktora**  tabele zostały zastąpione przez **osoby** tabeli. Rozwiń węzeł **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniowie** i **instruktora** tabel.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** wyświetlić kolumny rozróżniacza.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Na poniższym diagramie przedstawiono struktury nowej bazy danych służbowych:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Podsumowanie

Tabela na hierarchii dziedziczenia teraz została wdrożona w celu `Person`, `Student`, i `Instructor` klasy. Aby uzyskać więcej informacji dotyczących tego i innych konstrukcji dziedziczenia, zobacz [Strategie mapowania dziedziczenia](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na blogu Morteza Manavi. W następnym samouczku zobaczysz kilka sposobów, aby zaimplementować repozytorium i jednostki pracy.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[dalej](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
