---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Async i procedur składowanych z programu Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async i procedur składowanych z programu Entity Framework w aplikacji platformy ASP.NET MVC
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W starszych samouczkach przedstawiono sposób odczytywanie i aktualizowanie danych za pomocą modelu programowania synchronicznego. W tym samouczku widzisz implementowania model programowania asynchronicznego. Asynchroniczne kod może pomóc w aplikacji działać lepiej, ponieważ zapewnia lepsze wykorzystanie zasobów serwera.

W tym samouczku widoczne jest również sposób użycia procedury składowane insert, update i operacji usuwania w jednostce.

Na koniec będzie ponownie wdrożyć aplikację na platformie Azure, wraz ze wszystkich zmian w bazie danych, które zostały zaimplementowane od czasu pierwszej wdrożonej.

Na poniższych ilustracjach przedstawiono niektóre stron, którymi będzie współpracować.

![Strona działów](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Utwórz działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Dlaczego odblokowane kodem asynchroniczne

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach wysokie obciążenie wszystkich dostępnych wątków może być używana. W takim przypadku serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Z kodem synchroniczne wiele wątków może powiązany, gdy nie są faktycznie wykonują pracę, ponieważ ich oczekiwania operacji We/Wy zakończyć. Asynchroniczne kodu podczas procesu jest oczekiwania operacji We/Wy zakończyć, jego wątku zostanie zwolniona dla serwera na potrzeby przetwarzania innych żądań. W związku z tym kod asynchroniczny umożliwia zasobów serwera można efektywniej przy użyciu, a serwer jest skonfigurowany do obsługi ruchu więcej bez opóźnień.

We wcześniejszych wersjach programu .NET, zapisywanie i testowanie kodu asynchroniczne był zbyt złożony, błąd podatnych na błędy i trudne do debugowania. W programie .NET 4.5 więc znacznie łatwiejsze, że należy zwykle zapisać kod asynchroniczny chyba że nie masz powód jest pisanie, testowanie i debugowanie kodu asynchronicznego. Asynchroniczne kod wprowadzić z małej ilości obciążenie, ale w sytuacjach ruchu niskiej wydajności trafień jest niewielka, podczas w sytuacjach dużego natężenia ruchu sieciowego jest znacząca, potencjalne zwiększenie wydajności.

Aby uzyskać więcej informacji na temat programowania asynchronicznego, zobacz [Użyj .NET 4.5 asynchronicznej obsługi celu unikania blokowania połączeń](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Tworzenie kontrolera działu

Tworzenie kontrolera działu, wybierz ten sam sposób jak starszych kontrolerów, z wyjątkiem teraz **Użyj kontrolera asynchronicznego** pole wyboru akcje.

![Dział szkieletu kontrolera](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Następujące najważniejsze funkcje Pokaż, co zostało dodane do synchroniczne kod `Index` metody dokonanie asynchronicznych:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Cztery zmiany zostały zastosowane umożliwiające kwerendy bazy danych programu Entity Framework do wykonania asynchronicznie:

- Metoda jest oznaczona atrybutem `async` — słowo kluczowe, które informuje kompilator, aby wygenerować wywołań zwrotnych dla części treści metody i do automatycznego tworzenia `Task<ActionResult>` obiekt, który jest zwracany.
- Zwracany typ został zmieniony z `ActionResult` do `Task<ActionResult>`. `Task<T>` Typu reprezentuje trwającej pracy z wyniku typu `T`.
- `await` — Słowo kluczowe została zastosowana do wywołania usługi sieci web. Gdy kompilator widzi to słowo kluczowe, w tle dzieli metody na dwie części. Pierwsza część kończy operację, który jest uruchamiany asynchronicznie. Druga część są umieszczane w metodę wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
- Wersja asynchroniczna elementu `ToList` wywołano metodę rozszerzenie.

Dlaczego jest `departments.ToList` instrukcji zmodyfikowany, ale nie `departments = db.Departments` instrukcji? Przyczyną jest to, że tylko instrukcji, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie. `departments = db.Departments` Instrukcji konfiguruje kwerendy, ale zapytanie nie jest wykonywany do momentu `ToList` metoda jest wywoływana. W związku z tym tylko `ToList` metody jest wykonywane asynchronicznie.

W `Details` — metoda i `HttpGet` `Edit` i `Delete` metod `Find` metoda jest ten, który powoduje, że zapytania do wysłania do bazy danych, w taki sposób, aby metody, który jest wykonywany asynchronicznie:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

W `Create`, `HttpPost Edit`, i `DeleteConfirmed` metod jest `SaveChanges` wywołania metody, która powoduje, że polecenie do wykonania, nie instrukcjach, takich jak `db.Departments.Add(department)` co tylko spowodować jednostek w pamięci, do zmodyfikowania.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Otwórz *Views\Department\Index.cshtml*i Zastąp kod szablonu z następującym kodem:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Ten kod zmienia tytuł od indeksu do działów, przenosi nazwa administratora po prawej stronie i zapewnia pełną nazwę administratora.

Tworzenia, usuwania, uzyskać szczegółowe informacje i edytować widoków, zmienianie podpisu dla `InstructorID` na taki sam sposób jak zmienić pole nazwy działu "Dział" w widokach kursu "Administrator".

Tworzenie i Edycja widoków użyć poniższego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

W widokach Delete i szczegóły użyć poniższego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **działów** kartę.

![Strona działów](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Wszystko, co działa tak samo jak inne kontrolery, ale w tym kontrolerze wszystkie zapytania SQL są wykonywane asynchronicznie.

Należy pamiętać o podczas, używania programowanie asynchroniczne z programu Entity Framework w kilku kwestiach:

- Kod asynchronicznej nie jest bezpieczne dla wątków. Innymi słowy innymi słowy, nie należy próbować wykonać wiele operacji z użyciem tego samego wystąpienia kontekstu.
- Jeśli chcesz skorzystać z zalet wydajności async kodu, upewnij się, że wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), również korzystają asynchronicznego, jeśli wywołują wszystkie metody powodują zapytania do bazy danych programu Entity Framework.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Użyj procedury składowanej wstawiania, aktualizowania i usuwania

Niektóre deweloperów i DBAs preferowane procedur składowanych na potrzeby dostępu do bazy danych. We wcześniejszych wersjach programu Entity Framework można pobrać danych przy użyciu procedury składowanej przez [wykonywania raw zapytania SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nie może nakazać EF procedur składowanych na potrzeby operacji aktualizacji. EF 6 jest łatwa do skonfigurowania Code First, aby użyć procedur składowanych.

1. W *DAL\SchoolContext.cs*, wyróżniony kod, aby dodać `OnModelCreating` metody.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Ten kod nakazuje Entity Framework do procedury przechowywane w operacjach wstawiania, aktualizowania i usuwania operacji na `Department` jednostki.
2. W konsoli Zarządzanie pakietów wprowadź następujące polecenie:

    `add-migration DepartmentSP`

    Otwórz *migracje\&lt; sygnatura czasowa&gt;\_DepartmentSP.cs* wyświetlić kod w `Up` metodę, która tworzy wstawiania, aktualizowania i usuwania procedur składowanych:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- W konsoli Zarządzanie pakietów wprowadź następujące polecenie:

    `update-database`
- Uruchom aplikację w trybie debugowania, kliknij przycisk **działów** , a następnie kliknij pozycję **Utwórz nowy**.
- Wprowadź dane dla nowego działu, a następnie kliknij przycisk **Utwórz**.

    ![Utwórz działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- W programie Visual Studio można znaleźć w dziennikach w **dane wyjściowe** okna, aby zobaczyć, czy procedury składowanej zostało użyte do wstawienia nowego wiersza działu.

    ![SP Insert działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kod najpierw tworzy domyślną przechowywane procedury nazwy. Jeśli korzystasz z istniejącej bazy danych, konieczne może być Dostosowywanie nazw procedury składowanej, aby można było używać procedury składowane już zdefiniowana w bazie danych. Aby dowiedzieć się, jak to zrobić, zobacz [Entity Framework kod pierwszego wstawiania/aktualizacji/usuwania procedur składowanych](https://msdn.microsoft.com/data/dn468673).

Jeśli chcesz dostosować generowane co czy procedury składowane można edytować kod z utworzonym szkieletem migracji `Up` metodę, która tworzy procedury składowanej. W ten sposób zmiany zostaną odzwierciedlone zawsze, gdy zostanie uruchomione migracji, a zostaną zastosowane do produkcyjnej bazy danych podczas migracji automatycznie uruchamia się w środowisku produkcyjnym po zakończeniu wdrożenia.

Jeśli chcesz zmienić istniejącą procedurę składowaną, który został utworzony w poprzedniej migracji, można wygenerować puste migracji za pomocą polecenia Add-Migration i ręcznie napisać kod, który wywołuje [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) — metoda .

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tej sekcji wymaga zakończył opcjonalny **wdrażania aplikacji na platformie Azure** sekcji [migracji i wdrażania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek z tej serii. Jeśli były migracje błędów, które można rozwiązać przez usunięcie bazy danych w projekcie lokalnego, Pomiń tę sekcję.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania** z menu kontekstowego.
2. Kliknij przycisk **publikowania**.

    Program Visual Studio wdroży aplikację na platformie Azure i uruchamiania aplikacji w domyślnej przeglądarce, działające na platformie Azure.
3. Testowanie aplikacji w celu weryfikacji działa.

    Przy pierwszym uruchomieniu strony uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracja `Up` metody wymagane do bazy danych na bieżąco z bieżącym modelem danych. Teraz można używać wszystkich stron sieci web dodanych od czasu ostatniego wdrożone, w tym strony działu, dodane w tym samouczku.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób poprawiania wydajności serwera przez napisanie kodu, który wykonuje asynchronicznie i sposobu użycia procedury składowanej Wstawianie, aktualizowanie i usuwanie operacji. W następnym samouczku zobaczysz sposób zapobiec utracie danych, gdy wielu użytkowników próby edytowania tego samego rekordu w tym samym czasie.

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Poprzednie](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
