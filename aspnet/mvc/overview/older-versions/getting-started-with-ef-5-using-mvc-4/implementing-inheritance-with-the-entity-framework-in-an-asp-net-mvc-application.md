---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Wdrażanie dziedziczenia z programu Entity Framework w aplikacji ASP.NET MVC (8, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b39fc609007d437dd0845f9087dd5a32272cebe9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821279"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Wdrażanie dziedziczenia z programu Entity Framework w aplikacji ASP.NET MVC (8, 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku obsługiwane są wyjątki współbieżności. Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo, można użyć dziedziczenia, aby wyeliminować nadmiarowy kod. W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów. Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela wg hierarchii i tabeli na typ dziedziczenia

W programowanie zorientowane obiektowo, można użyć dziedziczenia w celu ułatwienia pracy z powiązanymi klasami. Na przykład `Instructor` i `Student` klas w `School` modelu danych udostępnianie kilka właściwości, które powoduje nadmiarowy kod:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Można utworzyć `Person` klasy, która zawiera tylko te właściwości udostępnionego podstawowej, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych. Może mieć `Person` tabelę, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli. Niektóre kolumny można zastosować tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), niektóre na wartość oba (`LastName`, `FirstName`). Zazwyczaj trzeba *dyskryminatora* kolumny, aby wskazać, jakiego typu każdy wiersz reprezentuje. Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.

![Tabela na hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).

Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych. Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.

![Tabela na type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasie jednostki jest wywoływana *tabeli na typ* dziedziczenia (TPT).

Wzorce dziedziczenia TPH ogólnie zapewniają lepszą wydajność platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań. Ten samouczek demonstruje sposób implementacji TPH dziedziczenia. Należy to zrobić, wykonując następujące czynności:

- Tworzenie `Person` klasy i zmień `Instructor` i `Student` pochodzi od klasy `Person`.
- Dodaj mapowanie modelu w bazie danych kod do klasy kontekstu bazy danych.
- Zmiana `InstructorID` i `StudentID` odwołania w całym projekcie, aby `PersonID`.

## <a name="creating-the-person-class"></a>Tworzenie klasy osoby

 Uwaga: Nie będzie możliwe do skompilowania projektu po utworzeniu klasy poniżej, dopóki nie zaktualizujesz kontrolerów, które korzysta z tych klas. 

W *modeli* folderze utwórz *osoba.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

W *Instructor.cs*, pochodzi `Instructor` klasy z `Person` klasy, a następnie usuń klucza i nazwy pola. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Wprowadzić zmiany podobne do *Student.cs*. `Student` Klasy będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Dodawanie osoby typu jednostki do modelu

W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych jest ponownie utworzyć jej `Person` tabeli zamiast `Student` i `Instructor` tabel.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Zmiana InstructorID i StudentID PersonID

W *SchoolContext.cs*, w instrukcji mapowania kurs przez instruktorów, zmień `MapRightKey("InstructorID")` do `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Ta zmiana nie jest wymagane; po prostu zmienia nazwę kolumny InstructorID w tabeli sprzężenia wiele do wielu. Jeśli nazwa jako InstructorID, aplikacja nadal będzie działać poprawnie. Poniżej przedstawiono ukończoną *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Następnie należy zmienić `InstructorID` do `PersonID` i `StudentID` do `PersonID` w całym projekcie ***z wyjątkiem*** w plikach z sygnaturami czasowymi migracje *migracje* folder. Aby to zrobić można będzie znaleźć otwierać tylko pliki, które muszą zostać zmienione, a następnie wykonać globalne zmiany w otwartych plików. To jedyny plik w *migracje* folder, należy zmienić to *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Rozpocznij od zamyka wszystkie otwarte pliki w programie Visual Studio.
2. Kliknij przycisk **Znajdź i Zamień — wszystkie pliki** w **Edytuj** menu, a następnie wyszukaj wszystkie pliki w projekcie, które zawierają `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Otwórz każdy plik w **Find Results** okna ***z wyjątkiem*** &lt;sygnatura czasowa&gt;*\_.cs* migracji plików w *Migracje* folderu, klikając dwukrotnie jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Otwórz **Zamień w plikach** okno dialogowe i zmień **przeszukania** do **wszystkimi otwartymi dokumentami**.
5. Użyj **Zamień w plikach** okna dialogowego, aby zmienić wszystkie `InstructorID` do `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Znajdź wszystkie pliki w projekcie, który zawiera `StudentID`.
7. Otwórz każdy plik w **Find Results** okna ***z wyjątkiem*** &lt;sygnatura czasowa&gt;*\_\*.cs* plików migracji w *migracje* folderu, klikając dwukrotnie jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Otwórz **Zamień w plikach** okno dialogowe i zmień **przeszukania** do **wszystkimi otwartymi dokumentami**.
9. Użyj **Zamień w plikach** okna dialogowego, aby zmienić wszystkie `StudentID` do `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Skompiluj projekt.

(Należy pamiętać, że w tym przykładzie pokazano *wadą* z `classnameID` wzorzec nazewnictwa kluczy podstawowych. Jeśli klucze podstawowe identyfikator miał nazwę bez poprzedzania ich nazwą klasy, *nie* zmiana nazwy konieczna może być teraz.)

## <a name="create-and-update-a-migrations-file"></a>Tworzenie i aktualizowanie plików migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` polecenia w konsoli zarządzania Pakietami. Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie wie, jak obsługiwać. Zostanie wyświetlony następujący błąd:

*Instrukcja ALTER TABLE konflikt z ograniczeniem klucza OBCEGO "klucza Obcego\_dbo. Dział\_dbo. Osoby\_PersonID ". Konflikt wystąpił w bazie danych "ContosoUniversity" table "dbo. Osoba", kolumna"PersonID".*

Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metoda następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Uruchom `update-database` ponownie polecenie.

> [!NOTE]
> Istnieje możliwość uzyskać inne błędy, podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji nie można rozwiązać, możesz kontynuować samouczek, zmieniając parametry połączenia w *Web.config* pliku lub usunięcie bazy danych. Najprostszą metodą jest zmiana nazwy bazy danych w *Web.config* pliku. Na przykład zmienić nazwę bazy danych do pojemności\_testów, jak pokazano w poniższym przykładzie:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Za pomocą nowej bazy danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenia jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania z bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Jeśli w przypadku zastosowania tego podejścia, aby można było kontynuować z tego samouczka, należy pominąć krok wdrożenia na końcu tego samouczka, ponieważ witrynę wdrożoną otrzymamy ten sam błąd, gdy migracja jest uruchamiany automatycznie. Jeśli chcesz rozwiązać błąd migracji, zostanie najlepszy zasób jest jednym z forów platformy Entity Framework lub StackOverflow.com.


## <a name="testing"></a>Testowanie

Uruchamianie witryny, a następnie spróbuj różnych stronach. Wszystko działa to tak jak poprzednio.

W **Eksploratora serwera** rozwiń **SchoolContext** i następnie **tabel**, i zobaczysz, że **uczniów** i **przez instruktorów**  tabele zostały zastąpione przez **osoby** tabeli. Rozwiń **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniów** i **przez instruktorów** tabel.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Na poniższym diagramie przedstawiono strukturę nową bazę danych School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Podsumowanie

Tabela wg hierarchii dziedziczenia teraz została wdrożona w celu `Person`, `Student`, i `Instructor` klasy. Aby uzyskać więcej informacji na temat tego i innych struktur dziedziczenia, zobacz [Strategie mapowania dziedziczenia](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na blogu Morteza Manavi. W następnym samouczku zobaczysz niektóre sposoby zaimplementowania repozytorium i jednostki pracy.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
