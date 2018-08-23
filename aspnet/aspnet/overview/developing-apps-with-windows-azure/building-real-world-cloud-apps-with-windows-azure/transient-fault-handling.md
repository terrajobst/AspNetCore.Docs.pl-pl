---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Obsługa (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure) błędu przejściowego | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 26f1d040e4857899ddcff79b778d0af62b66134d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754694"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Obsługa (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure) błędu przejściowego
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Podczas projektowania aplikacji w chmurze świata rzeczywistego, jedną z rzeczy, które trzeba się zajmować jest sposób obsługi przerw w świadczeniu usługi tymczasowych. Ten problem jest jednoznacznie ważne w aplikacjach w chmurze, ponieważ jesteś więc zależne od usług zewnętrznych i połączeniem sieciowym. Często znajdziesz niewielkie błędy wyróżnienia, które są zazwyczaj samonaprawiania, a jeśli nie masz przygotowany do obsługi tych inteligentnie, powodują one, będzie złe doświadczenia klientom.

## <a name="causes-of-transient-failures"></a>Przyczyny błędów przejściowych

W środowisku chmurowym, które można znaleźć, które nie powiodło się i porzucić połączenia z bazą danych się zdarzyć, okresowo. Wynika to z częściowo one przechodzenia przez więcej modułów równoważenia obciążenia w porównaniu do środowiska lokalnego, w których serwera sieci web i serwera bazy danych mają bezpośrednie połączenie fizyczne. Ponadto czasami, gdy wszystko jest zależna od usługi wielodostępne zobaczysz wywołania usługi get wolniej lub przekraczają limit czasu ponieważ ktoś inny używa usługi powoduje osiągnięcie intensywnie. W innych przypadkach może być użytkownika, który wykorzystuje usługę zbyt często, a usługa celowo ogranicza możesz — nie zezwala na połączenia — aby zapobiec negatywnego wpływu na innych dzierżaw usługi.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Użyj inteligentne logikę ponawiania/wycofywania, aby zminimalizować wpływ błędów przejściowych

Zamiast zgłaszać wyjątek i wyświetlanie strony błąd lub nie jest dostępna dla klienta, może rozpoznać błędów, które mają zwykle charakter przejściowy, i automatycznie ponów próbę wykonania operacji, które spowodowały błąd, w nadzieję, zanim czasu będzie pomyślnie. W większości przypadków operacja zostanie wykonana przy próbie drugiej, a będzie można odzyskać sprawność po błędzie bez klientów mających kiedykolwiek pamiętać, że wystąpił problem.

Istnieje kilka sposobów, które można zaimplementować logikę ponawiania inteligentne.

- Microsoft Patterns &amp; rozwiązania grupa ma [blok aplikacji obsługi błędów przejściowych](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) wykonujący wszystko, co dla Ciebie, jeśli używasz programu ADO.NET dla dostępu do bazy danych SQL, (nie przy użyciu platformy Entity Framework). Właśnie ustawiono zasadę ponownych prób — ile razy, aby ponowić próbę wykonania kwerendy lub polecenia oraz jak długo należy czekać między prób — i zawijania SQL możesz pisać kod w *przy użyciu* bloku.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Obsługuje również TFH [pamięć podręczna oparta na roli Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) i [usługi Service Bus](https://azure.microsoft.com/services/service-bus/).
- Korzystając z programu Entity Framework można zwykle nie działają bezpośrednio za pomocą połączenia SQL, więc nie można użyć tego pakietu Patterns and Practices, ale tego rodzaju Logika ponawiania platformy Entity Framework 6 opiera się bezpośrednio w ramach. W podobny sposób określić strategia ponawiania prób, a następnie EF używa tej strategii, zawsze wtedy, gdy uzyskuje dostęp do bazy danych.

    Aby użyć tej funkcji w aplikacji naprawić, musimy to zrobić wystarczy dodać klasę, która pochodzi od klasy *DbConfiguration* i Włącz Logika ponawiania.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Wyjątki bazy danych SQL, które w ramach identyfikują jako błędy zwykle charakter przejściowy kod przedstawiony powoduje, że EF, aby ponowić próbę wykonania operacji maksymalnie 3 razy, za pomocą wykładniczego wycofywania opóźnienia między ponownych prób i maksymalnym opóźnieniem of 5 sekund. Wycofywanie wykładnicze oznacza, że po każdym ponawiania nie powiodło się po upływie dłuższy okres czasu przed podjęciem ponownej próby. Maksymalna liczba prób to trzy w wierszu nie powiedzie się, zgłosi wyjątek. Poniższa sekcja dotycząca wyłączników wyjaśnia, dlaczego chcesz wycofywanie wykładnicze i ograniczoną liczbę ponownych prób.

    Może mieć podobne problemy, gdy używasz usługi Azure Storage, jak aplikacja naprawić nie dla obiektów blob, a interfejs API z klienta .NET magazynu implementuje już tego samego rodzaju logiki. Wystarczy określić zasady ponawiania lub nawet nie trzeba tego robić, jeśli jesteś zadowolony z ustawień domyślnych.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Wyłączniki

Istnieje kilka powodów dlaczego użytkownik nie chce ponowienie zbyt wiele razy zbyt długiego okresu:

- Zbyt wielu użytkowników trwale ponawianie żądań zakończonych niepowodzeniem może zostać obniżona innym użytkownikom środowiska. W przypadku milionów osób wszystko co powtarzanych ponawiania żądań można być zajmowania kolejki wysyłania usług IIS i uniemożliwia obsługę żądań, które go w przeciwnym razie może obsługiwać pomyślnie aplikacji.
- Wszyscy ponawia próbę z powodu błędu usługi może być tak wiele żądań w kolejce, usługa pobiera propagowane podczas uruchamiania do odzyskania.
- Ten błąd jest spowodowany ograniczania przepustowości, jeśli ma przedział czasu, w których usługa używa ograniczania, dalsze próby może przenieść to okno w i spowodować ograniczanie kontynuować.
- Konieczne może być użytkownikiem oczekiwanie na stronie sieci web do renderowania. Podejmowanie osób oczekiwania jest za długa może być bardziej irytujących tego względnie szybko wniosku je, aby spróbować ponownie później.

Wycofywanie wykładnicze rozwiązuje część tego problemu, ograniczając częstotliwość ponownych prób, które usługi można uzyskać z poziomu aplikacji. Ale także musiały mieć *wyłączniki*: oznacza to, że w pewnym ponów próg zatrzymuje, ponawianie próby i przyjmuje innych działań, takich jak jeden z następujących aplikacji:

- Niestandardowe rezerwowego. Jeśli cena akcji nie można pobrać z Reuters, być może należy pobrać go ze strony Bloomberg; lub jeśli nie można pobrać danych z bazy danych, może być możesz pobrać go z pamięci podręcznej.
- Niepowodzenie dyskretnym. Jeśli potrzebujesz przy użyciu usługi nie jest sztywnego dla aplikacji, po prostu zwracać wartość null Jeśli nie można uzyskać danych. Jeśli wyświetlasz zadania rozwiązać go, a nie odpowiada na usługę Blob service, można wyświetlić szczegóły zadania bez obrazu.
- Awaria następuje szybko. Błąd użytkownika, aby uniknąć przeciążenia usługi o ponów próbę wykonania żądania, które mogą spowodować przerwy w działaniu usługi dla innych użytkowników lub rozszerzyć oknem ograniczenia przepustowości. Możesz wyświetlić przyjazny komunikat "spróbuj ponownie później".

Nie ma żadnych zasad ponawiania opracowanie. Możesz ponowić próbę razy i poczekać dłużej trwające procesu roboczego tła asynchronicznego mógłbyś w aplikacji sieci web synchroniczne, w którym użytkownik jest oczekiwanie na odpowiedź. Możesz poczekać dłużej między kolejnymi próbami usługa relacyjnej bazy danych niż w przypadku usługi pamięć podręczna. Poniżej przedstawiono niektóre przykładowe zalecane ponów próbę wykonania zasady, które umożliwiają oszacowanie jak numery może się różnić w. ("Szybkie First" oznacza nie opóźnienie przed pierwszym ponowieniem próby.

![Przykładowe zasady ponawiania prób](transient-fault-handling/_static/image1.png)

Wskazówki zasad ponawiania prób bazy danych SQL, zobacz [Rozwiązywanie problemów dotyczących błędów przejściowych i błędów połączenia do bazy danych SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Podsumowanie

Strategia ponawiania prób/wycofywania sprawić, że tymczasowe błędy niewidoczne dla klienta w większości przypadków, a firma Microsoft udostępnia struktur, które można użyć, aby zminimalizować pracę realizację strategii, czy używasz programu ADO.NET Entity Framework i platformy Azure Usługi magazynu.

W [następny rozdział](distributed-caching.md), omówimy sposób zwiększyć wydajność i niezawodność, za pomocą rozproszonej pamięci podręcznej.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby:

Dokumentacja

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usługach Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument — Markiem Simmsem i Michael Thomassy. Podobnie jak przed uszkodzeniami serii, ale zbliża się bardziej szczegółowe informacje z instrukcjami. W sekcji Telemetria i Diagnostyka.
- [Przed uszkodzeniami: Wskazówki dotyczące architektury na temat odporności chmury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument, Marc Mercuri, Ulrich Homann i Andrew Townhill. Wersja strony sieci Web przed uszkodzeniami serii filmów wideo.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz ponawiania wzorzec, wzorca nadzorcy agenta harmonogramu.
- [Odporność na uszkodzenia w usłudze Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Wpis na blogu autorstwa Tony Petrossian.
- [Entity Framework - elastyczność połączenia / logika ponowień](https://msdn.microsoft.com/data/dn456835). Jak dostosować Obsługa błędu przejściowego w funkcji programu Entity Framework 6.
- [Połączeń i przejmowanie poleceń z platformą Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarty w dziewięć części serii samouczków pokazano, jak skonfigurować funkcji odporności połączenia programów EF 6 dla bazy danych SQL.

Wideo

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Dziewięć serii Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Zawiera omówienie wyłączników w odcinek 3 zaczynając od 40:55.
- [Tworzenie Big: Wnioski wyciągnięte z klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). — Markiem Simmsem opowiada o projektowanie pod kątem awarii przejściowych błędów, obsługi i instrumentacji wszystko.

Przykładowy kod

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzona przez program Microsoft Azure zespół doradczy klientów, który demonstruje sposób skorzystania [Enterprise biblioteki przejściowych błędów obsługi bloku](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Aby uzyskać więcej informacji, zobacz [Cloud Service Fundamentals warstwy dostępu do danych — obsługi błędów przejściowych](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH jest zalecane w przypadku dostępu do bazy danych przy użyciu platformy ADO.NET bezpośrednio (bez używający narzędzia Entity Framework).

> [!div class="step-by-step"]
> [Poprzednie](monitoring-and-telemetry.md)
> [dalej](distributed-caching.md)
