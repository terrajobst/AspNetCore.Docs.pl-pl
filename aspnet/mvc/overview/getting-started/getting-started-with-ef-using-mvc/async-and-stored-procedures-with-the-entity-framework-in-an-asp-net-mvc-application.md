---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async i procedur składowanych z platformą Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 211c9c8c66f3eed15121b4dd425fa728c39b7de3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802052"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async i procedur składowanych z platformą Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W samouczkach wcześniej pokazaliśmy ci, jak odczytywanie i aktualizowanie danych za pomocą synchronicznego modelu programowania. W tym samouczku dowiesz się jak zaimplementować asynchronicznego modelu programowania. Kod asynchroniczny może pomóc aplikacji działać lepiej, ponieważ ułatwia lepsze wykorzystanie zasobów serwera.

W tym samouczku zobaczysz również sposób użycia procedury składowane insert, update i operacje usuwania w jednostce.

Na koniec będzie ponownie wdrożyć aplikację na platformie Azure oraz wszystkich zmian w bazie danych, które zostały zaimplementowane od podczas pierwszego wdrażania.

Na poniższych ilustracjach przedstawiono niektóre stron którymi będziesz pracować.

![Działy strony](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Utwórz działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Dlaczego odblokowane za pomocą kodu asynchronicznego

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W rezultacie kod asynchroniczny umożliwia zasobów serwera w celu można użyć bardziej wydajnie i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.

We wcześniejszych wersjach programu .NET, pisanie i testowanie kodu asynchronicznego została skomplikowana, błąd podatne i trudne do debugowania. W .NET 4.5 jest znacznie łatwiejsze, że powinien ogólnie wpisywania kodu asynchronicznego, chyba że istnieje powód, aby nie były pisania, testowania i debugowania kodu asynchronicznego. Kod asynchroniczny wprowadzają niewielkiej ilości obciążenia, ale wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu sytuacjach o niewielkim ruchu, istotne jest potencjalna poprawa wydajności.

Aby uzyskać więcej informacji na temat programowania asynchronicznego, zobacz [Obsługa async Użyj .NET 4.5 w celu unikania blokowania połączeń](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Tworzenie kontrolera działu

Tworzenie kontrolera działu, w taki sam sposób jak w przypadku starszych kontrolerów, jednak tym razem wybierz **Użyj kontrolera asynchronicznego** pole wyboru czynności.

![Tworzenie szkieletu kontrolera działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Następującym głównym funkcjom Pokaż dodanych do kodu synchronicznego `Index` metodę, aby stał się asynchronicznego:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Cztery zmiany zostały zastosowane do Włączanie kwerenda bazy danych programu Entity Framework asynchroniczne wykonywanie:

- Metoda jest oznaczona atrybutem `async` słowo kluczowe, które informuje kompilator, aby wygenerować wywołania zwrotne dla części treści metody, a także automatyczne tworzenie `Task<ActionResult>` obiekt, który jest zwracany.
- Zwracany typ został zmieniony z `ActionResult` do `Task<ActionResult>`. `Task<T>` Typu reprezentuje pracę w toku z wyniku typu `T`.
- `await` — Słowo kluczowe została zastosowana do wywołania usługi sieci web. Gdy kompilator wykryje to słowo kluczowe, w tle dzieli metody na dwie części. Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie. Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
- To wersja asynchroniczna elementu `ToList` wywołano metodę rozszerzenia.

Dlaczego jest `departments.ToList` instrukcji zmodyfikowany, ale nie `departments = db.Departments` instrukcji? Przyczyną jest to, że tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. `departments = db.Departments` Instrukcja ustawia się zapytanie, ale zapytanie nie jest wykonywany do momentu `ToList` metoda jest wywoływana. W związku z tym tylko `ToList` metoda jest wykonywana asynchronicznie.

W `Details` metody i `HttpGet` `Edit` i `Delete` metod `Find` metodą jest ten, który powoduje, że zapytania wysyłane do bazy danych, to metody, która pobiera wykonywany asynchronicznie:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

W `Create`, `HttpPost Edit`, i `DeleteConfirmed` metod jest `SaveChanges` wywołanie metody, która powoduje, że polecenie do wykonania, nie instrukcji takich jak `db.Departments.Add(department)` co spowodować tylko jednostki w pamięci do zmodyfikowania.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Otwórz *Views\Department\Index.cshtml*i Zastąp kod szablonu poniższym kodem:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Ten kod zmienia się tytuł z indeksu działów, przenosi nazwy administratora po prawej stronie i zapewnia pełną nazwę administratora.

W przypadku tworzenia, usuwania, podawania szczegółów i edytowanie widoków, Zmień podpis `InstructorID` pole "Administrator" taki sam sposób, jak zmienić pole nazwy działu "Dział" w widokach kursu.

Tworzenie i edytowanie widoków, użyj następującego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

W widokach Delete i szczegółowe informacje, użyj następującego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **działów** kartę.

![Działy strony](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Wszystko, co działa tak samo, jak w innych kontrolerów, ale w tym kontrolerze wszystkich zapytań SQL są wykonywane asynchronicznie.

Kilka rzeczy, aby wiedzieć, w którym używasz programowania asynchronicznego przy użyciu platformy Entity Framework:

- Kod asynchroniczny nie jest bezpieczny dla wątków. Innymi słowy innymi słowy, nie należy próbować wykonać wiele operacji równolegle za pomocą tego samego wystąpienia kontekstu.
- Jeśli chcesz skorzystać z zalet wydajności kod asynchroniczny, upewnij się, wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), jeśli wywołują wszystkie metody, że zapytania wysyłane do bazy danych programu Entity Framework użyć async.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Używanie procedur składowanych do wstawiania, aktualizowania i usuwania

Niektóre deweloperów i przetwarzający wolą używanie procedur składowanych, aby uzyskać dostęp do bazy danych. We wcześniejszych wersjach programu Entity Framework można pobrać danych za pomocą procedury składowanej przez [wykonywanie pierwotne zapytania SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nie może nakazać EF procedur składowanych na potrzeby operacji aktualizacji. W programów EF 6 jest łatwa do skonfigurowania Code First na używanie procedur składowanych.

1. W *DAL\SchoolContext.cs*, Dodaj wyróżniony kod do `OnModelCreating` metody.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Ten kod powoduje, że platformy Entity Framework do użycia przechowywanych procedur Wstawianie, aktualizowanie i usuwanie operacji na `Department` jednostki.
2. W konsoli Zarządzanie pakietów wprowadź następujące polecenie:

    `add-migration DepartmentSP`

    Otwórz *migracje\&lt; sygnatura czasowa&gt;\_DepartmentSP.cs* Aby wyświetlić kod w `Up` metodę umożliwiającą utworzenie wstawiania, aktualizowania i usuwania procedur składowanych:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. W konsoli Zarządzanie pakietów wprowadź następujące polecenie:

     `update-database`
4. Uruchom aplikację w trybie debugowania, kliknij przycisk **działów** kartę, a następnie kliknij przycisk **Utwórz nowy**.
5. Wprowadź dane dla nowego wydziału, a następnie kliknij przycisk **Utwórz**.

     ![Utwórz działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. W programie Visual Studio, sprawdź dzienniki w **dane wyjściowe** okno, aby wyświetlić, aby wstawić nowy wiersz działu użyto procedury składowanej.

     ![Dział Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kod najpierw tworzy domyślne przechowywane procedury nazwy. Jeśli używasz istniejącej bazy danych może być konieczne Dostosowywanie nazw procedury składowanej, aby można było używać procedury składowane już zdefiniowana w bazie danych. Aby uzyskać informacje o tym, jak to zrobić, zobacz [Entity Framework Code pierwszy wstawiania/aktualizowania/usuwania procedur składowanych](https://msdn.microsoft.com/data/dn468673).

Jeśli chcesz dostosować, co generowane czy procedury składowane, można edytować utworzony szkielet kodu dla migracje `Up` metodę, która tworzy procedurę składowaną. W ten sposób zmiany są odzwierciedlane zawsze, gdy czy migracja jest uruchamiany i zostaną one zastosowane do produkcyjnej bazy danych, podczas migracji automatycznie uruchamia się w środowisku produkcyjnym po zakończeniu wdrożenia.

Jeśli chcesz zmienić istniejącą procedurę składowaną, która została utworzona w poprzedniej migracji, można wygenerować pusty migracji za pomocą polecenia Add-migracji i ręcznie napisać kod, który wywołuje [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) — metoda .

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tej sekcji, musisz ukończyć opcjonalną **wdrażania aplikacji na platformie Azure** sekcji [migracje i wdrożenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek z tej serii. Jeśli użytkownik ma błędy migracji, które rozwiązano problem, usuwając bazy danych w projekcie lokalnym, Pomiń tę sekcję.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.
2. Kliknij przycisk **publikowania**.

    Program Visual Studio wdroży aplikację na platformie Azure, a aplikacja zostanie otwarta w domyślnej przeglądarce, działające na platformie Azure.
3. Testowanie aplikacji w celu zweryfikowania działa.

    Uruchom stronę po raz pierwszy uzyskuje dostęp do bazy danych, platformy Entity Framework umożliwia uruchamianie wszystkich migracjach `Up` metod wymaganych do przełączenia bazy danych na bieżąco z bieżącym modelem danych. Można teraz używać wszystkich stron sieci web, które dodane od czasu ostatniego wdrożenia, w tym stron działu, które zostały dodane w tym samouczku.

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak poprawić wydajność serwera przez napisanie kodu, który jest wykonywany asynchronicznie i jak używanie procedur składowanych dla Wstawianie, aktualizowanie i usuwanie operacji. W następnym samouczku pokazano, jak zapobiec utracie danych, gdy wielu użytkowników próby edytowania tego samego rekordu w tym samym czasie.

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
