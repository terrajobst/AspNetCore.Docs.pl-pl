---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Wzorzec skoncentrowane kolejki pracy (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 124e673206ecea2eac5efb8c2802a32a690fa104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875438"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Wzorzec skoncentrowane kolejki pracy (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Wcześniej widzieliśmy, że przy użyciu wielu usług może spowodować SLA "złożonego", gdzie jest skuteczne SLA aplikacji *produktu* z poszczególnych umów SLA. Na przykład aplikacja napraw korzysta z witryny sieci Web, magazynu i bazy danych SQL. W przypadku niepowodzenia któregokolwiek z tych usług aplikacji spowoduje zwrócenie błędu dla użytkownika.

Buforowanie jest sposób obsługi błędów przejściowych dla zawartości tylko do odczytu. Ale co zrobić, jeśli aplikacja wymaga do pracy? Na przykład gdy użytkownik przesyła nowego poprawka zadania, aplikacji nie można po prostu umieścić zadania w pamięci podręcznej. Aplikacja musi zapisać zadań Usuń do magazynu danych, więc mogą być przetwarzane.

To, gdzie wzorzec skoncentrowane kolejki pracy składa się z. Ten wzorzec umożliwia luźne powiązanie między warstwą sieci web i usługi wewnętrznej bazy danych.

Oto, jak działa wzorzec. Gdy aplikacja pobiera żądanie, umieszcza elementu roboczego do kolejki i natychmiast zwraca odpowiedź. Następnie proces zaplecza oddzielne ściąga elementy robocze z kolejki i wykonuje pracę.

Wzorzec skoncentrowane kolejki pracy jest przydatne w przypadku:

- Praca jest czasochłonne (duże opóźnienie).
- Praca wymaga zewnętrznej usługi, które mogą być niedostępne.
- Działa to znaczy obciążający zasoby (wysoka Procesora).
- Praca będzie korzystać z szybkość bilansowanie (może ulec seria nagłym obciążenia).

## <a name="reduced-latency"></a>Mniejsze opóźnienia

Kolejki są przydatne robią czasochłonne za każdym razem. Jeśli zadanie zajmuje kilka sekund lub już, zamiast blokuje użytkownikowi końcowemu, umieść elementu roboczego do kolejki. Poinformuj użytkownika "Pracujemy nad jego", a następnie użyj odbiornika kolejki przetwarzania zadania w tle.

Na przykład gdy podczas zakupów w sklepie internetowym, witryna sieci web potwierdza natychmiast zamówienia. Ale nie oznacza to, iż Twoje pliki jest już ciężarówka dostarczona. One umieszczone w kolejce zadania, a w tle są podczas sprawdzania zdolności kredytowej przygotowywanie elementów do wysyłki i tak dalej.

W scenariuszach z krótkim czasem oczekiwania może być dłużej przy użyciu kolejki, w porównaniu z synchronicznie wykonanie zadania całkowity czas na trasie. Ale nawet wówczas, inne korzyści mogą być większe niż wadą tego.

## <a name="increased-reliability"></a>Zwiększenie niezawodności

W wersji napraw go, że firma Microsoft już został patrzeć wykonanej do tej pory frontonu sieci web jest ściśle powiązana z zaplecza bazy danych SQL. Jeśli usługa bazy danych SQL jest niedostępny, wówczas użytkownik otrzymuje wystąpił błąd. Jeśli nie działają ponownych prób (błędu jest więcej niż przejściowy), jedyną operacją można wykonać, jest wyświetlany błąd, a następnie poprosić użytkownika o. Spróbuj ponownie później.

![Diagram przedstawiający frontonu sieci web kończą się niepowodzeniem, jeśli wewnętrznej bazy danych SQL nie powiodło się](queue-centric-work-pattern/_static/image1.png)

Korzystanie z kolejek, gdy użytkownik prześle zadanie napraw go, aplikacja zapisuje komunikat do kolejki. Ładunek komunikatu jest [JSON](http://json.org/) reprezentację zadania. Jak jest zapisywany komunikat do kolejki, aplikacja zwraca i natychmiast zawiera komunikat z potwierdzeniem dla użytkownika.

Jeśli którakolwiek z nich wewnętrznej bazy danych — na przykład baza danych SQL lub odbiornika kolejki — przejście do trybu offline, użytkownicy mogą nadal przesyłać nowe poprawka zadania. Komunikaty po prostu zostanie kolejki, dopóki ponownie usługi wewnętrznej bazy danych są dostępne. W tym momencie usługi wewnętrznej bazy danych będzie przechwytywać na liście prac.

![Diagram przedstawiający sieci web frontonu kontynuowanie działania podczas błąd bazy danych SQL](queue-centric-work-pattern/_static/image2.png)

Ponadto teraz możesz dodać więcej logiki zaplecza bez obaw o odporności frontonu. Na przykład można wysłać wiadomości e-mail lub wiadomości SMS do właściciela zawsze, gdy jest przypisany nowy napraw go. Jeśli adres e-mail lub usługa programu SMS jest niedostępny, należy przetworzyć wszystkie inne, a następnie umieść komunikat do oddzielnych kolejki przy wysyłaniu wiadomości e-mail/SMS.

Wcześniej, naszych skuteczne umowy dotyczącej poziomu usług został aplikacje sieci Web &times; magazynu &times; SQL Database = 99.7%. (Zobacz [projektu po awarii](design-to-survive-failures.md).)

Zmianach aplikacji korzystanie z kolejki, tylko w aplikacji sieci Web i magazynu, zależy od frontonu sieci web dla złożone SLA 99.8%. (Pamiętaj, że kolejek jest należącej do usługi Magazyn Azure, tak że znajdują się w tej samej SLA jako magazynu obiektów blob).

Należy jeszcze lepiej niż 99.8% można utworzyć dwie kolejki w dwóch różnych regionach. Wyznaczyć jako podstawowy, a drugi jako pomocniczy. W aplikacji przełączyć kolejki dodatkowej Jeśli podstawowy kolejka jest niedostępna. Ryzyko jest niedostępna w tym samym czasie jest bardzo mała.

## <a name="rate-leveling-and-independent-scaling"></a>Wyrównywanie szybkość i niezależne skalowanie

Kolejki są również przydatne w przypadku coś o nazwie *szybkości bilansowanie* lub *wyrównywanie obciążenia*.

Aplikacje sieci Web często są podatne na gwałtowny seria w ruchu. Za pomocą Skalowanie automatyczne automatycznie dodać serwery sieci web do obsługi ruchu w sieci web zwiększona, skalowanie automatyczne może nie móc wystarczająco szybko reagować na obsługi niespodziewane największego obciążenia. Jeśli na serwerach sieci web można odciążyć niektóre czynności, które mają zrobić przez napisanie wiadomości do kolejki, ich obsługi więcej ruchu. Usługi wewnętrznej bazy danych można odczytywać wiadomości z kolejki i ich przetworzenia. Głębokość kolejki będą zwiększać i zmniejszać jako zależności od zmian obciążenia przychodzącego.

Z wielu jego czasochłonne rozładowany do usługi zaplecza warstwa sieci web można łatwiej odpowiedzieć nagłym największego ruchu. I Zapisz pieniądze, ponieważ wszystkie danego ilość ruchu sieciowego są obsługiwane przez mniejszej liczby serwerów sieci web.

Niezależnie można skalować warstwa sieci web i usługi wewnętrznej bazy danych. Na przykład może być konieczne trzech serwerów sieci web, ale tylko jeden serwer przetwarzania kolejki wiadomości. Lub jeśli korzystasz z zadań obliczeniowych w tle, mogą wymagać więcej serwerów wewnętrznej bazy danych.

![](queue-centric-work-pattern/_static/image3.png)

Skalowanie automatyczne działa z usługami zaplecza, a także z warstwą sieci web. Można skalowanie w górę lub w dół liczbę maszyn wirtualnych, które są przetwarzania zadań w kolejce, na podstawie użycia procesora CPU zaplecza maszyn wirtualnych. Można też automatycznego skalowania w oparciu o liczbę elementów znajdują się w kolejce. Na przykład można podać skalowania automatycznego, aby spróbować Zachowaj nie więcej niż 10 elementów w kolejce. Jeśli kolejka ma więcej niż 10 elementów, skalowania automatycznego doda maszyn wirtualnych. Po ich nadążyć, skalowania automatycznego spowoduje zerwanie dodatkowych maszyn wirtualnych.

## <a name="adding-queues-to-the-fix-it-application"></a>Dodawanie do poprawki umieszcza je w kolejce aplikacji

Aby zaimplementować wzorzec kolejki, musimy upewnić dwie zmiany do aplikacji rozwiązać.

- Gdy użytkownik prześle nowego poprawka zadania, należy umieścić zadania w kolejce, zamiast zapisywanie w bazie danych.
- Tworzenie usługi zaplecza, który przetwarza wiadomości w kolejce.

Kolejki, użyjemy [usługi magazynu kolejek Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Inną opcją jest użycie [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Podjęcie decyzji, które usługi kolejki, do korzystania, należy wziąć pod uwagę sposób Twoja aplikacja powinna do wysyłania i odbierania wiadomości w kolejce:

- Jeśli masz producentów współpracujących i konkurencyjnych konsumentów, należy rozważyć użycie usługi magazynu kolejek Azure. "Współpracujących producentów" oznacza, że wiele procesów dodawania wiadomości do kolejki. "Konkurujących konsumentów" oznacza wiele procesów są ściąganie kolejki komunikatów do ich przetworzenia, ale dowolny komunikat o danym mogą być przetwarzane tylko przez jednego "użytkownika". Jeśli potrzebujesz więcej przepustowości, nie można uzyskać z pojedynczej kolejki, użyj dodatkowe kolejki i/lub dodatkowych kont magazynu.
- Jeśli potrzebujesz [publikowania/subskrypcji modelu](http://en.wikipedia.org/wiki/Publish/subscribe), rozważ użycie kolejek usługi Azure Service Bus.

Aplikacji Usuń pasujące do producentów współpracujących i konkurencyjnych konsumentów modelu.

Kolejnym zagadnieniem są dostępności aplikacji. Usługa kolejki magazynu jest częścią tej samej usługi, która używamy dla magazynu obiektów blob, używa go nie ma wpływu na naszych umowy SLA. Usługa Azure Service Bus jest oddzielny usługi za pomocą własne umowy SLA. Użycie kolejek usługi Service Bus, mamy składników dodatkowe wartość procentowa umów SLA, a naszych złożonego SLA będzie niższa. Wybierając usługi kolejki, upewnij się, że zrozumienie wpływu wybraną na dostępność aplikacji. Aby uzyskać więcej informacji, zobacz [zasobów](#resources) sekcji.

## <a name="creating-queue-messages"></a>Tworzenie wiadomości w kolejce

Aby umieścić poprawka zadań w kolejce, frontonu sieci web wykonuje następujące czynności:

1. Utwórz [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) wystąpienia. `CloudQueueClient` Wystąpienia służy do wykonywania żądania do usługi kolejki.
2. Utwórz kolejkę, jeśli go jeszcze nie istnieje.
3. Serializować zadań rozwiązać.
4. Wywołanie [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) umieścić wiadomości do kolejki.

Robimy tej pracy w Konstruktorze i `SendMessageAsync` nową metodę `FixItQueueManager` klasy.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

W tym miejscu używamy [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do serializacji automatyczne do formatu JSON. Można użyć dowolnej metody serializacji preferowane. Format JSON ma prowadzoną zrozumiałą dla użytkownika, przy jednoczesnym zachowaniu mniej szczegółowe niż XML.

Wysokiej jakości kodu czy dodać logikę obsługi błędów, zatrzymać, gdy baza danych stała się niedostępna ulepszono możliwości obsługi odzyskiwania, Utwórz kolejkę przy uruchamianiu aplikacji i zarządzanie "[skażone" wiadomości](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Trująca wiadomość jest komunikatu, którego nie można przetworzyć jakiegoś powodu. Nie chcesz skażone wiadomości do znajdują się w kolejce, gdzie roli procesu roboczego stale spróbuje ich przetwarzania, się nie powieść, spróbuj ponownie, zakończyć się niepowodzeniem i itd.)

W aplikacji MVC frontonu należy zaktualizować kod, który jest utworzenie nowego zadania. Zamiast wprowadzania zadania do repozytorium, wywołaj `SendMessageAsync` metod przedstawionych powyżej.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Przetwarzanie wiadomości w kolejce

Do przetwarzania komunikatów w kolejce, utworzymy usługi wewnętrznej bazy danych. Usługi wewnętrznej bazy danych zostanie uruchomiony nieskończoną pętlę, która wykonuje następujące czynności:

1. Pobierz następny komunikat z kolejki.
2. Deserializacji komunikatu do zadania napraw go.
3. Zadanie napraw zapisu w bazie danych.

Do obsługi usługi zaplecza, utworzymy usługi w chmurze platformy Azure zawierającą *roli procesu roboczego*. Rola proces roboczy składa się z przynajmniej jednej maszyny wirtualnej, które można wykonać przetwarzania wewnętrznej bazy danych. Kod uruchamiany w tych maszyn wirtualnych będzie pobierać wiadomości z kolejki, gdy będą one dostępne. Dla każdego komunikatu możemy deserializacji ładunek JSON i zapisze wystąpienie jednostki napraw go zadań bazy danych, przy użyciu tego samego repozytorium, które zostały użyte we wcześniejszej części warstwa sieci web.

Poniższe kroki przedstawiają sposób Dodaj pracownika projektu roli do rozwiązania, które zawiera projekt sieci web standard. Poprawka projektu, który można pobrać już zostały wykonane następujące kroki.

Najpierw należy dodać projektu usługi w chmurze w rozwiązaniu Visual Studio. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj**, następnie **nowy projekt**. W okienku po lewej stronie rozwiń **Visual C#** i wybierz **chmury**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

W **nową usługę w chmurze Azure** okna dialogowego, rozwiń węzeł **Visual C#** węzła w okienku po lewej stronie. Wybierz **roli procesu roboczego** i kliknij ikonę strzałki w prawo.

![](queue-centric-work-pattern/_static/image6.png)

(Zwróć uwagę, że można również dodać *roli sieci web*. Firma Microsoft może rozwiązać go uruchomić frontonu w tej samej usłudze w chmurze zamiast uruchamiania go w witrynie sieci Web platformy Azure. Niektóre zalety w ułatwiając połączeń między frontonu i zaplecza do koordynowania relacją. Jednak aby zachować ten pokaz proste, firma Microsoft jest utrzymywanie frontonu w aplikacji sieci Web platformy Azure App Service i tylko systemem zaplecza w usłudze w chmurze.)

Domyślna nazwa jest przypisany do roli procesu roboczego. Aby zmienić nazwę, umieść kursor myszy nad roli procesu roboczego w okienku po prawej stronie, a następnie kliknij ikonę ołówka.

![](queue-centric-work-pattern/_static/image7.png)

Kliknij przycisk **OK** przeprowadzenie okna dialogowego. Spowoduje to dodanie dwa projekty w rozwiązaniu Visual Studio.

- Azure projekt, który definiuje usługi w chmurze, w tym informacje o konfiguracji.
- Projekt roli proces roboczy, który definiuje roli procesu roboczego.

![](queue-centric-work-pattern/_static/image8.png)

Aby uzyskać więcej informacji, zobacz [Tworzenie projektu platformy Azure z programem Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

W roli procesu roboczego, możemy sondować wiadomości przez wywołanie metody `ProcessMessageAsync` metody `FixItQueueManager` klasy, którą widzieliśmy wcześniej.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Metody umożliwia sprawdzenie, czy istnieje oczekujących komunikatów. Jeśli istnieje, jej deserializuje komunikat do `FixItTask` jednostki i jednostka są zapisywane w bazie danych. Pętle go, dopóki kolejka jest pusta.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondowanie dla wiadomości w kolejce wiąże się z małych transakcji naliczają opłaty, dlatego jeśli nie ma oczekujących na przetworzenie, roli procesu roboczego `RunAsync` metoda oczekuje sekundę przed sondowania ponownie przez wywołanie metody `Task.Delay(1000)`.

W projekcie sieci web Dodawanie asynchroniczne kodu może automatycznie poprawić wydajność ponieważ usługi IIS zarządza puli wątków ograniczone. To nie jest w przypadku projektu roli proces roboczy. W celu poprawy skalowalności roli procesu roboczego, można zapisać wielowątkowych kodu lub użycie asynchronicznego kodu do zaimplementowania [Programowanie równoległe](https://msdn.microsoft.com/library/ff963553.aspx). Próbki nie implementuje Programowanie równoległe, ale pokazano, jak wprowadzić kod asynchroniczne, można zaimplementować Programowanie równoległe.

## <a name="summary"></a>Podsumowanie

W tym rozdziale przedstawiono sposób poprawiania czas odpowiedzi aplikacji, niezawodności i skalowalności dzięki implementacji wzorca pracy skoncentrowane na kolejki.

Jest to ostatni 13 wzorce omówione w tym Książka elektroniczna, ale oczywiście istnieje wiele innych wzorcach i rozwiązania, które mogą pomóc Ci tworzenie aplikacji w chmurze powiodło się. [Końcowego rozdział](more-patterns-and-guidance.md) zawiera linki do zasobów tematy, które nie zostały omówione w tych wzorców 13.

<a id="resources"></a>
## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat kolejek zobacz następujące zasoby.

Dokumentacja:

- [Microsoft Azure magazynu kolejek część 1: Wprowadzenie](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artykuł przez Schacherl łaciński.
- [Wykonywanie zadania w tle](https://msdn.microsoft.com/library/ff803365.aspx), rozdział 5 [przenoszenie aplikacji w chmurze, w wersji 3](https://msdn.microsoft.com/library/ff728592.aspx) z Microsoft Patterns and Practices. (W szczególności sekcji ["Przy użyciu usługi Azure magazynu kolejek"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Najlepsze rozwiązania dotyczące maksymalizacja Ekonomiczność na podstawie kolejki komunikatów rozwiązań na platformie Azure i skalowalność](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Oficjalny dokument przez Valery Mizonov.
- [Porównanie kolejek platformy Azure i kolejek usługi Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Artykuł MSDN Magazine zawiera dodatkowe informacje, które mogą pomóc Ci wybrać usługi kolejki, która umożliwia. Artykuł uwagi, że magistrali usług jest zależna od ACS do uwierzytelniania, co oznacza, że z kolejki SB byłyby niedostępne, gdy usługi ACS jest niedostępny. Jednak ponieważ artykuł dotyczy, SB został zmieniony w celu umożliwienia używania [tokeny sygnatury dostępu Współdzielonego](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) zamiast ACS.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz Elementarz asynchroniczną obsługę wiadomości, wzorzec potoków i filtry, wzorzec kompensowanie transakcji, wzorzec konkurujących konsumentów, CQRS wzorca.
- [Przebieg CQRS](https://msdn.microsoft.com/library/jj554200). E-book o CQRS przez Microsoft Patterns and Practices.

Wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Aby obejrzeć wprowadzenie do usługi Azure Storage i kolejek Zobacz epizodu 5, zaczynając od 35:13.

> [!div class="step-by-step"]
> [Poprzednie](distributed-caching.md)
> [dalej](more-patterns-and-guidance.md)
