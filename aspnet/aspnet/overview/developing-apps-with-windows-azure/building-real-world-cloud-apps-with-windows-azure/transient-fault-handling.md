---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "Wystąpienia błędu przejściowego obsługi (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: b743b04789c5e5ebf5ab922cf34a516a16a6d356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Obsługa (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure) wystąpienia błędu przejściowego
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Podczas projektowania aplikacji w chmurze rzeczywistych, jeden czynności, które trzeba myśleć o jest sposób obsługi przerw w działaniu usługi tymczasowego. Ten problem jest unikatowo ważne w aplikacji w chmurze, ponieważ jesteś tak zależny od połączenia sieciowego i usług zewnętrznych. Często można uzyskać małego błędami występującymi, które są zazwyczaj samonaprawiania i jeśli nie jest przygotowany do obsługi inteligentnie, będzie skutkować zły środowisko w dla klientów.

## <a name="causes-of-transient-failures"></a>Przyczyny błędów przejściowych

W środowisku chmury, które można znaleźć, nie powiodło się i usunąć połączenia z bazą danych nastąpić okresowo. Wynika to z częściowo jest przechodzenia przez więcej usług równoważenia obciążenia w porównaniu do środowiska lokalnego, w której mają bezpośredniego połączenia fizycznego serwera sieci web i serwera bazy danych. Ponadto czasami w przypadku, gdy użytkownik jest zależna od usługi wielodostępne zobaczysz wywołania get service wolniej lub upłynął limit czasu ponieważ ktoś inny, który korzysta z usługi jest naciśnięcie silnie. W innych przypadkach może być użytkownik, który jest zbyt często naciśnięcie usługi, a usługa celowo ogranicza możesz — nie zezwala na połączenia — Aby uniknąć negatywnego wpływu na innych dzierżaw usługi.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Użyj inteligentne logiki ponawiania/wycofania celu złagodzenia skutków błędów przejściowych

Zamiast generowania wyjątku i wyświetlanie nie jest dostępna lub błąd strony do klienta, może rozpoznać błędów, które mają zwykle charakter przejściowy, i automatycznie ponów próbę wykonania operacji, które spowodowały błąd, w nadzieję który przed długo będzie pomyślnie. W większości przypadków operacji powiedzie się w drugim spróbuj i będzie można odzyskać z błąd bez klientów mających dotąd pamiętać, że wystąpił problem.

Istnieje kilka metod, które można zaimplementować Logika ponawiania inteligentne.

- Microsoft Patterns &amp; grupa rozwiązań ma [bloku aplikacji obsługi błędów przejściowych](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) wykonuje wszystko dla Ciebie Jeśli używasz ADO.NET dla dostępu do bazy danych SQL (nie za pośrednictwem programu Entity Framework). Wystarczy ustawić zasady dla ponownych prób — ile razy, aby ponowić próbę wykonania kwerendy lub polecenia i jak długo czekać między prób — a zawijania programu SQL kodu w *przy użyciu* bloku.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Obsługuje również TFH [pamięci podręcznej na roli Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) i [usługi Service Bus](https://azure.microsoft.com/services/service-bus/).
- Korzystając z programu Entity Framework zwykle nie są pracy bezpośrednio z połączeniami SQL, więc nie można użyć tego pakietu wzorców i rozwiązania, ale Entity Framework 6 kompilacje tego rodzaju Logika ponawiania bezpośrednio w ramach. W podobny sposób określić strategii ponownych prób, a następnie EF używa tej strategii zawsze, gdy uzyskuje dostęp do bazy danych.

    Aby użyć tej funkcji w aplikacji rozwiązać, musimy wykonać jest dodać klasę pochodzącą z *DbConfiguration* i Włącz logiki ponawiania próby.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Wyjątki bazy danych SQL, identyfikujące w ramach jako błędy zwykle charakter przejściowy kodem przedstawionym nakazuje EF do maksymalnie 3 razy, ponów operację z wykładniczego wycofywania opóźnienia między ponownych prób i maksymalne opóźnienie 5 sekund. Wykładniczej wycofania oznacza, że po każdym nie powiodło się ponownej próbie go będzie oczekiwał na dłuższy okres czasu przed podjęciem ponownej próby. W przypadku awarii trzech prób w wierszu, zgłosi wyjątek. Poniższej sekcji o podziałów obwodu wyjaśnia, dlaczego chcesz wykładniczej wycofania i ograniczoną liczbę ponownych prób.

    Może utrzymać podobne problemy, jeśli używasz usługi Azure Storage, zgodnie z aplikacji Usuń nie dla obiektów blob i interfejsu API programu .NET magazynu klienta już implementuje ten sam rodzaj logiki. Wystarczy określić zasady ponawiania lub jeszcze nie masz to zrobić w przypadku modyfikowania ustawień domyślnych.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Podziałów obwodu

Istnieje kilka przyczyn, dlaczego nie chcesz, aby ponowić próbę zbyt wiele razy za pośrednictwem zbyt długiego okresu:

- Za dużo użytkowników trwale ponawianie żądań zakończonych niepowodzeniem może zostać obniżona obsługi innych użytkowników. Jeśli miliony osób są wszystkie wprowadzeniem powtarzane ponawiania żądań może być zajmowania kolejki wysyłania usług IIS i uniemożliwia obsługę żądań, które go w przeciwnym razie może obsłużyć pomyślnie aplikacji.
- Jeśli ponawia Wszyscy z powodu błędu usługi może być tak wiele żądań w kolejce Usługa pobiera rozpływową, podczas jej uruchamiania odzyskać.
- Jeśli istnieje okno czasu usługa używa do ograniczania jest błąd z powodu dławienia, ciągłego ponownych prób można Przenieś okno i spowodować, ograniczenie kontynuować.
- Konieczne może być użytkownikiem oczekiwanie na stronie sieci web do renderowania. Wprowadzania osób oczekiwania zbyt długo może więcej irytujących tego stosunkowo szybko udzielanie porad je, aby spróbować ponownie później.

Wykładniczej wycofania rozwiązuje część tego problemu przez ograniczenie częstotliwości ponownych prób, które usługi można uzyskać z aplikacji. Ale również musi być *podziałów obwodu*: oznacza to, że w pewnym ponów próg aplikacji zatrzymuje ponawianie próby i przejście innych działań, takich jak jedną z następujących czynności:

- Niestandardowe rezerwowego. Jeśli nie można pobrać giełdowy z Reuters, może być możesz pobrać go z Bloomberg; lub jeśli nie można pobrać danych z bazy danych, może być możesz pobrać go z pamięci podręcznej.
- Nie w trybie dyskretnym. Jeśli potrzebujesz z usługi nie jest all-or-nothing dla aplikacji, po prostu zwracanie wartości null nie można pobrać danych. Jeśli wyświetlasz zadania napraw go, a usługa Blob nie odpowiada, można wyświetlić szczegóły zadania bez obrazu.
- Szybko się niepowodzeniem. Błąd użytkownika, aby uniknąć przepełnienia usługę za pomocą ponów próbę wykonania żądania, które mogą powodować zakłócenia dla innych użytkowników lub rozszerzyć okna ograniczenia przepustowości. Można wyświetlić przyjazny komunikat ". Spróbuj ponownie później".

Nie ma żadnych zasad ponawiania rozwiązanie. Można ponowić próbę więcej razy, a następnie zaczekaj już w procesie roboczym asynchroniczne tła niż w przypadku w aplikacji sieci web synchronicznego, gdy użytkownik jest oczekiwania na odpowiedź. Można poczekać już między kolejnymi próbami usługa relacyjnej bazy danych niż w przypadku dla usługi pamięć podręczna. Poniżej przedstawiono niektóre przykładowe zalecane ponów zasady dają pogląd, jak numery mogą się różnić. ("Fast pierwszy" oznacza brak opóźnienia przed pierwszym ponów próbę.

![Zasady ponawiania próbki](transient-fault-handling/_static/image1.png)

Wskazówki zasady ponawiania bazy danych SQL, zobacz [Rozwiązywanie błędów przejściowych i błędów połączenia bazy danych SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Podsumowanie

Ponów próbę/wycofania strategii może sprawić, że tymczasowe błędy niewidoczne do odbiorcy w większości przypadków i firma Microsoft udostępnia struktury, których można zminimalizować pracy realizację strategii, czy używasz ADO.NET Entity Framework i platformy Azure Usługa magazynu.

W [następnego rozdziału](distributed-caching.md), wyjaśniono, jak poprawić wydajność i niezawodność przy użyciu rozproszonej pamięci podręcznej.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby:

Dokumentacja

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na usług w chmurze Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument przez moduły SIMM znaku i Michael Thomassy. Podobnie jak seria przed uszkodzeniami, ale przechodzi w szczegółowe instrukcje. Zobacz sekcję Telemetrii i informacji diagnostycznych.
- [Przed uszkodzeniami: Wskazówki dotyczące architektury chmury odporność](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument Mercuri wytłoków, Ulrich Homann i Andrew Townhill. Wersja strony sieci Web seria filmów przed uszkodzeniami.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz ponawiania wzorzec, wzorzec przełożonego agenta harmonogramu.
- [Odporność na uszkodzenia w bazie danych Azure SQL](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Wpis w blogu przez Tony Petrossian.
- [Entity Framework - połączenia odporności / Logika ponawiania próby](https://msdn.microsoft.com/data/dn456835). Jak używać i dostosować błędu przejściowego obsługi funkcji programu Entity Framework 6.
- [Elastyczność połączenia i przechwytywaniu polecenia Entity Framework w aplikacji platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarty w serii dziewięć części samouczka przedstawiono sposób ustawienia odporności funkcji Połączenia programów EF 6 dla bazy danych SQL.

Wideo

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Zawiera omówienie podziałów obwód w epizodu 3, zaczynając od 40:55.
- [Kompilowanie Big: Wnioski uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Moduły SIMM znacznik omówiono projektowania dla błędu przejściowego odporność obsługi i wszystkie instrumentacji.

Przykładowy kod

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzone przez Microsoft Azure zespół doradczy klientów który demonstruje sposób użycia [Enterprise biblioteki przejściowy błąd obsługi bloku](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Aby uzyskać więcej informacji, zobacz [chmury usługi podstawy Warstwa dostępu do danych — Obsługa błędów przejściowych](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH jest zalecane dla dostępu do bazy danych za pomocą ADO.NET bezpośrednio (bez użycia programu Entity Framework).

>[!div class="step-by-step"]
[Poprzednie](monitoring-and-telemetry.md)
[dalej](distributed-caching.md)
