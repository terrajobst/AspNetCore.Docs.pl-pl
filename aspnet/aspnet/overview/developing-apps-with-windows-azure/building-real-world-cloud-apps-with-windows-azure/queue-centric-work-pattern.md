---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Wzorzec pracy skoncentrowany na kolejkach (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c454023c26d73df174a43b3a8ad6745ef8654fd2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751942"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Wzorzec pracy skoncentrowany na kolejkach (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Wcześniej widzieliśmy, że przy użyciu wielu usług może spowodować "złożona" umowa SLA, gdzie jest umowa SLA skutecznych aplikacji *produktu* indywidualnych umów SLA. Na przykład aplikacji naprawić używa, Web Sites, Storage i bazy danych SQL. Jeśli dowolny z tych usług nie powiedzie się, aplikacja zwróci błąd do użytkownika.

Buforowanie jest dobrym sposobem obsługi błędów przejściowych dla zawartości tylko do odczytu. Ale co zrobić, jeśli Twoja aplikacja potrzebuje do pracy? Na przykład, gdy użytkownik przesyła nowe rozwiązać go zadanie, aplikacja nie można po prostu umieścić zadania w pamięci podręcznej. Aplikacja musi można zapisać zadania naprawić w trwałym magazynie danych, dzięki czemu mogą być przetwarzane.

To, skąd pochodzą wzorzec pracy skoncentrowany na kolejkach. Ten wzorzec umożliwia tym luźne powiązanie warstwa sieci web i usługi zaplecza.

Oto, jak działa wzorzec. Gdy aplikacja odbiera żądanie, umieszcza element roboczy do kolejki i natychmiast zwraca odpowiedź. Następnie proces zaplecza oddzielne ściąga elementy robocze z kolejki i wykonuje pracę.

Wzorzec pracy skoncentrowany na kolejkach przydaje się do:

- Praca jest czasochłonne (duże opóźnienie).
- Pracy, która wymaga usługi zewnętrznej, która może być niedostępne.
- Oznacza to praca dużej ilości zasobów (wysokie użycie procesora CPU).
- Praca używającym współczynnik wyrównywanie (zależnie od obciążenia nagłe wzrosty).

## <a name="reduced-latency"></a>Zmniejszenie opóźnienia

Kolejki są przydatne w każdym razem, gdy robią czasochłonne. Jeśli zadanie trwa kilka sekund lub dłużej, zamiast blokowanie użytkownika końcowego, umieść element roboczy do kolejki. Monituj użytkownika, "Pracujemy nad jej", a następnie użyj odbiornika kolejki, do przetwarzania zadań w tle.

Na przykład podczas dokonywania zakupów w sklepie internetowym, witryny sieci web potwierdza natychmiast zamówienia. Ale nie oznacza to, że ciężarówki dostarczany jest już Twoich rzeczy. Umieszczają zadania w kolejce, a w tle są podczas sprawdzania kredytu przygotowywanie elementów do wysyłki i tak dalej.

W scenariuszach z krótkim czasem oczekiwania całkowity czas end-to-end może być dłużej, przy użyciu kolejki, w porównaniu z synchronicznie wykonywania zadania. Ale nawet wówczas, inne korzyści mogą być większe niż tej wady.

## <a name="increased-reliability"></a>Większa niezawodność

W wersji rozwiązać go, że firma Microsoft już został patrząc do tej pory frontonu sieci web jest ściśle powiązany z zapleczem bazy danych SQL. Jeśli usługa SQL database jest niedostępna, użytkownik pobiera błąd. Jeśli ponownych prób nie działa (oznacza to, że błąd jest więcej niż przejściowe), jedyną czynnością, możesz zrobić, jest wyświetlany błąd i poprosić użytkownika, aby spróbować ponownie później.

![Diagram przedstawiający frontonu sieci web w przypadku braku Jeśli wewnętrznej bazy danych SQL nie powiodło się](queue-centric-work-pattern/_static/image1.png)

Korzystanie z kolejek, gdy użytkownik przesyła zadanie napraw go, aplikacja zapisuje komunikat do kolejki. Ładunek komunikatu jest [JSON](http://json.org/) reprezentację zadania. Gdy tylko zostanie napisany komunikat do kolejki, aplikacja zwraca i natychmiast pokazuje komunikat o powodzeniu użytkownikowi.

Dowolną z usług wewnętrznej bazy danych — takich jak bazy danych SQL lub odbiornik kolejki — przejdą w tryb offline, użytkownicy nadal możesz przesłać nowe rozwiązać go zadania. Komunikaty po prostu może umieścić w kolejce do czasu ponownie usługi wewnętrznej bazy danych są dostępne. W tym momencie usługi wewnętrznej bazy danych będzie nadrób zaległości w zaległości.

![Diagram przedstawiający sieci web frontonu w dalszym ciągu działać, gdy występuje błąd bazy danych SQL](queue-centric-work-pattern/_static/image2.png)

Ponadto teraz możesz dodać więcej logikę zaplecza bez martwienia się o odporności frontonu. Na przykład można wysłać wiadomości e-mail lub wiadomości SMS do właściciela, zawsze wtedy, gdy nowe rozwiązać je przypisano. Adres e-mail lub usługa programu SMS jest niedostępny, można przetwarzać wszystkie inne elementy i następnie umieścić komunikatu w oddzielnej kolejki dla wysyłania wiadomości e-mail/SMS.

Wcześniej był aplikacji sieci Web w umowach SLA skuteczne &times; magazynu &times; bazy danych SQL Database = 99.7%. (Zobacz [projektowanie pod kątem przetrwania awarii](design-to-survive-failures.md).)

Gdy zmienimy aplikacji na używanie kolejki, fronton sieci web zależy od tylko aplikacje sieci Web i pamięci masowej, dla złożonego SLA 99,8%. (Zwróć uwagę, czy kolejek są częścią usługi Azure storage, dzięki czemu są one uwzględnione w tej samej umowy SLA jako magazynu obiektów blob).

Jeśli potrzebujesz jeszcze lepsze niż 99,8%, można utworzyć dwie kolejki w dwóch różnych regionach. Wyznaczyć jeden jako podstawowy, a drugi jako pomocniczy. W aplikacji nie za pośrednictwem kolejki dodatkowej Jeśli kolejki głównej nie jest dostępna. Ryzyko jest niedostępna w tym samym czasie jest bardzo mały.

## <a name="rate-leveling-and-independent-scaling"></a>Wyrównywanie szybkości i niezależne skalowanie

Kolejki są także przydatne coś, co jest nazywane *szybkości wyrównywanie* lub *wyrównywanie obciążenia*.

Aplikacje sieci Web często są podatne na nagłe wzrosty ruchu. Podczas skalowania automatycznego można użyć do automatycznego dodawania serwerów sieci web do obsługi ruchu w sieci web zwiększona, skalowanie automatyczne może nie móc wystarczająco szybko reagować na obsługi nagłych wzrostów obciążenia. Jeśli na serwerach sieci web można odciążyć niektóre czynności, które muszą zrobić przez napisanie wiadomości do kolejki, ich obsługi większego ruchu. Usługa zaplecza można odczytywać komunikaty z kolejki i przetwarzać je. Głębokość kolejki będzie rosnąć lub maleć, jak w zależności od zmian obciążenia przychodzącego.

Za pomocą wielu czasochłonne zadania rozładowany do usługi zaplecza warstwa sieci web można łatwiej odpowiedzieć skokami ruchu. I zaoszczędź pieniądze, ponieważ wszelkie określoną ilość ruchu sieciowego może zostać obsłużony przez mniejszej liczby serwerów sieci web.

Możesz skalować warstwa sieci web i usługi wewnętrznej bazy danych niezależnie od siebie. Na przykład możesz potrzebować trzech serwerów sieci web, ale tylko jeden serwer przetwarzania kolejki komunikatów. Lub Jeśli uruchamiasz zadanie wymagające wielu obliczeń w tle, możesz potrzebować więcej serwerów wewnętrznej bazy danych.

![](queue-centric-work-pattern/_static/image3.png)

Skalowanie automatyczne działa z usługami zaplecza, a także od warstwy sieci web. Możesz skalować w górę lub Skaluj w dół liczbę maszyn wirtualnych, które są przetwarzania zadań w kolejce na podstawie użycia procesora CPU zaplecza maszyn wirtualnych. Można też skalowania automatycznego w oparciu o liczbę elementów znajdują się w kolejce. Na przykład można stwierdzić, funkcja skalowania automatycznego ma próbować zachować co najwyżej 10 elementów w kolejce. Jeśli więcej niż 10 elementów do kolejki, skalowanie automatyczne spowoduje dodanie maszyn wirtualnych. Oni zapoznać się z nimi, automatycznego skalowania będzie zatrzymywania dodatkowe maszyny wirtualne.

## <a name="adding-queues-to-the-fix-it-application"></a>Dodawanie do poprawki umieszcza je w kolejce aplikacji

Aby zaimplementować wzorzec kolejki, musimy upewnić dwie zmiany do aplikacji naprawić.

- Gdy użytkownik przesyła nowe rozwiązać go zadanie, należy umieścić zadania w kolejce, zamiast zapisuje je w bazie danych.
- Tworzenie usługi zaplecza, która przetwarza wiadomości w kolejce.

W kolejce, użyjemy [usługi Azure Queue Storage](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Innym rozwiązaniem jest użycie [usługi Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Aby zdecydować, której usługi kolejki użyć, należy wziąć pod uwagę sposób Twoja aplikacja wymaga do wysyłania i odbierania wiadomości w kolejce:

- Jeśli masz, współpracujących producentów i odbiorców konkurencyjnych, należy wziąć pod uwagę przy użyciu usługi Azure Queue Storage. "Współpracujących producentów" oznacza, że wiele procesów dodawania komunikatów do kolejki. "Konkurujących odbiorców" oznacza wiele procesów ściągają wiadomości w kolejce do ich przetworzenia, ale każdy komunikat danego mogą być przetwarzane tylko przez jednego "użytkownika". Jeśli potrzebujesz więcej przepływności, nie można uzyskać z pojedynczą kolejką, użyj dodatkowe kolejki i/lub dodatkowych kont magazynu.
- Jeśli potrzebujesz [modelu publikowania/subskrybowania](http://en.wikipedia.org/wiki/Publish/subscribe), rozważ użycie kolejek usługi Azure Service Bus.

Aplikacja naprawić dostosowane do potrzeb współpracujących producentów i konkurujących konsumentów modelu.

Kolejna kwestia jest związana dostępności aplikacji. Usługi Queue Storage jest częścią tej samej usługi, które firma Microsoft korzysta z usługi blob storage, więc za jego pomocą nie ma wpływu na umowie SLA. Usługa Azure Service Bus jest oddzielną usługą za pomocą swoje własne umowy SLA. Jeśli użyto kolejek usługi Service Bus, firma Microsoft musiałaby wziąć pod uwagę dodatkową wartość procentowa umów SLA i naszych złożona umowa SLA będzie niższa. Wybierając usługi kolejki, upewnij się, że rozumiesz wpływ wybranego na dostępność aplikacji. Aby uzyskać więcej informacji, zobacz [zasobów](#resources) sekcji.

## <a name="creating-queue-messages"></a>Tworzenie wiadomości w kolejce

Aby umieścić zadanie poprawka w kolejce, fronton sieci web wykonuje następujące czynności:

1. Tworzenie [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) wystąpienia. `CloudQueueClient` Wystąpienia jest używana do wykonywania żądań dotyczących usługi kolejki.
2. Utworzenie kolejki, jeśli go jeszcze nie istnieje.
3. Serializacja zadań naprawić.
4. Wywołaj [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) umieścić komunikat do kolejki.

Wykonamy tę pracę w Konstruktorze i `SendMessageAsync` nową metodę `FixItQueueManager` klasy.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Tutaj używamy [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do serializacji automatyczne do formatu JSON. Możesz użyć dowolnego podejście serializacji, użytkownik sobie tego życzy. JSON ma tę zaletę jest czytelny dla człowieka, będąc mniej szczegółowe informacje, niż XML.

Kodu o jakości produkcyjnej będzie dodać logikę obsługi błędu, zatrzymać, gdy baza danych stała się niedostępna, bardziej obsługi odzyskiwania, utworzyć kolejkę przy uruchamianiu aplikacji i zarządzanie "[skażone" wiadomości](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Zarządzanie skażonymi komunikatami jest komunikat, który nie można przetworzyć przyczyny. Nie chcesz skażone komunikaty znajdują się w kolejce, w którym rola procesu roboczego stale spróbuje je przetworzyć, zakończyć się niepowodzeniem, spróbuj ponownie, zakończyć się niepowodzeniem i tak dalej.)

W aplikacji MVC frontonu należy zaktualizować kod, który tworzy nowe zadanie. Zamiast umieszczać zadania do repozytorium, należy wywołać `SendMessageAsync` metod przedstawionych powyżej.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Przetwarzanie komunikatów w kolejce

Przetwarzanie komunikatów w kolejce, utworzymy usługi zaplecza. Usługa zaplecza zostanie uruchomiona nieskończoną pętlę, która wykonuje następujące czynności:

1. Pobieranie następnego komunikatu z kolejki.
2. Deserializacji komunikatu do zadania rozwiązać go.
3. Zadanie naprawić zapisu w bazie danych.

Aby hostować usługi zaplecza, utworzymy usługi Azure Cloud Service, która zawiera *roli procesu roboczego*. Rola procesu roboczego składa się z co najmniej jeden maszyn wirtualnych, które można wykonać przetwarzanie zaplecza. Kod, który jest uruchamiany na tych maszynach wirtualnych każda będzie ściągać komunikaty z kolejki w miarę ich udostępniania. Dla każdego komunikatu utworzymy deserializacji ładunek w formacie JSON i zapisywać wystąpienie jednostki rozwiązać go zadania w bazie danych przy użyciu tego samego repozytorium, której użyliśmy we wcześniejszej części warstwa sieci web.

Poniższe kroki pokazują jak dodać procesu roboczego projektu roli do rozwiązania, które ma projekt sieci web standard. Napraw go projektu, którą można pobrać już zostały wykonane następujące kroki.

Najpierw Dodaj projekt usługi w chmurze do rozwiązania Visual Studio. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj**, następnie **nowy projekt**. W okienku po lewej stronie rozwiń **Visual C#** i wybierz **chmury**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

W **nową usługę w chmurze Azure** okna dialogowego, rozwiń węzeł **Visual C#** węzła w okienku po lewej stronie. Wybierz **roli procesu roboczego** i kliknij ikonę strzałki w prawo.

![](queue-centric-work-pattern/_static/image6.png)

(Zwróć uwagę, że można również dodać *roli sieci web*. Firma Microsoft może rozwiązać ją uruchomić frontonu w tej samej usłudze w chmurze zamiast uruchamiać go w witrynie sieci Web systemu Azure. Który ma kilka zalet w ułatwianie koordynowania połączenia między aplikacją frontonu i zaplecza. Jednak w celu uproszczenia tego pokazu, firma Microsoft jest przechowywanie frontonu w usłudze Azure App Service Web Apps i uruchamiania tylko serwer zaplecza w usłudze w chmurze.)

Nazwa domyślna jest przypisany do roli procesu roboczego. Aby zmienić nazwę, umieść kursor myszy nad roli procesu roboczego w okienku po prawej stronie, a następnie kliknij ikonę ołówka.

![](queue-centric-work-pattern/_static/image7.png)

Kliknij przycisk **OK** do ukończenia okna dialogowego. Spowoduje to dodanie dwa projekty do rozwiązania Visual Studio.

- Projekt platformy Azure, który definiuje usługę w chmurze, w tym informacje o konfiguracji.
- Projekt roli proces roboczy, który definiuje roli procesu roboczego.

![](queue-centric-work-pattern/_static/image8.png)

Aby uzyskać więcej informacji, zobacz [Tworzenie projektu platformy Azure z programem Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

W roli procesu roboczego, możemy sondowania pod kątem komunikatów przez wywołanie metody `ProcessMessageAsync` metody `FixItQueueManager` klasy, którą widzieliśmy wcześniej.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Metoda sprawdza, czy istnieje komunikat oczekujący. Jeśli istnieje, jej deserializuje wiadomość do `FixItTask` jednostki i zapisuje jednostkę w bazie danych. Go w pętli do momentu kolejka jest pusta.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondowania komunikatów w kolejce jest naliczana małych transakcji jest opłata w wysokości, więc jeśli nie ma oczekujących na przetworzenie, rola procesu roboczego `RunAsync` metoda oczekuje chwilę przed sondowania ponownie przez wywołanie metody `Task.Delay(1000)`.

W projekcie sieci web dodając kod asynchroniczny automatycznie poprawić wydajność ponieważ usługi IIS zarządza z puli wątków ograniczone. To nie jest w przypadku projektu roli proces roboczy. Aby poprawić skalowalność roli procesu roboczego, pisania kodu wielowątkowego lub użycie kodu asynchronicznego w celu zaimplementowania [programowania równoległego](https://msdn.microsoft.com/library/ff963553.aspx). Przykład nie implementuje programowania równoległego, ale pokazuje, jak kod asynchroniczny, dzięki czemu można zaimplementować programowania równoległego.

## <a name="summary"></a>Podsumowanie

W tym rozdziale przedstawiono jak poprawić czas odpowiedzi aplikacji, niezawodność i skalowalność poprzez implementację wzorzec pracy skoncentrowany na kolejkach.

Jest to ostatnia wzorców 13 omówione w tej książce elektronicznej, ale oczywiście istnieje wiele innych wzorców i praktyk, które ułatwiają tworzenie aplikacji w chmurze pomyślne. [Końcowe rozdziału](more-patterns-and-guidance.md) zawiera linki do zasobów dla tematów, które nie zostały omówione w tych wzorców 13.

<a id="resources"></a>
## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat kolejek zobacz następujące zasoby.

Dokumentacja:

- [Microsoft Azure Storage kolejek część 1: Wprowadzenie do](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artykuł przez Schacherl rzymskie.
- [Wykonywanie zadań w tle](https://msdn.microsoft.com/library/ff803365.aspx), rozdział 5 [przenoszenie aplikacji do chmury, wersja 3](https://msdn.microsoft.com/library/ff728592.aspx) z Microsoft Patterns and Practices. (W szczególności i sekcji ["Za pomocą kolejek usługi Azure Storage"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Najlepsze praktyki w maksymalnie wykorzystać skalowalność i ekonomiczność kolejki komunikatów rozwiązań na platformie Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Oficjalny dokument przez Valery Mizonov.
- [Porównanie kolejek platformy Azure i kolejek usługi Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Artykuł w MSDN Magazine zawiera dodatkowe informacje, które mogą pomóc w wyborze odpowiedniej usługi kolejki. Artykuł wspomniany, że usługi Service Bus jest zależna od usługi ACS do uwierzytelniania, co oznacza, że Twoje kolejek SB byłyby niedostępne, gdy usługa ACS jest niedostępny. Jednak ponieważ artykuł został napisany, SB został zmieniony w celu umożliwienia używania [tokeny sygnatur dostępu Współdzielonego](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) jako alternatywę dla usługi ACS.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz podstawy asynchronicznej obsługi komunikatów, wzorzec potoków i filtrów, wzorzec transakcji wyrównującej, wzorzec konkurujących odbiorców, wzorca CQRS.
- [Podróż CQRS](https://msdn.microsoft.com/library/jj554200). Książka elektroniczna o CQRS przez Microsoft Patterns and Practices.

Wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Wprowadzenie do usługi Azure Storage i kolejki Zobacz odcinek 5 zaczynając od 35:13.

> [!div class="step-by-step"]
> [Poprzednie](distributed-caching.md)
> [dalej](more-patterns-and-guidance.md)
