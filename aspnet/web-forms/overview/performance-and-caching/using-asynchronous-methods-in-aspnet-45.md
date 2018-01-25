---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "Używanie metod asynchronicznych w języku ASP.NET 4.5 | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "W tym samouczku uczy podstaw konstruowania asynchroniczne aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio Express 2012 for Web, który jest bezpłatny..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 73e46134cfafb9edc4c1888211eab44b8f2bf828
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Używanie metod asynchronicznych w języku ASP.NET 4.5
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku udzieli Ci podstawy asynchroniczne formularzy sieci Web ASP.NET aplikacji za pomocą [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11), który jest bezpłatną wersję programu Microsoft Visual Studio. Można również użyć [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Następujące sekcje są uwzględnione w tym samouczku.
> 
> - [Jak żądania są przetwarzane przez puli wątków](#HowRequestsProcessedByTP)
> - [Wybieranie metody synchronicznego lub asynchronicznego](#ChoosingSyncVasync)
> - [Przykładowa aplikacja](#SampleApp)
> - [Na stronie synchroniczne Gizmos](#GizmosSynch)
> - [Tworzenie stron Gizmos asynchroniczne](#CreatingAsynchGizmos)
> - [Wykonywanie operacji na wielu równolegle](#Parallel)
> - [Przy użyciu Token anulowania](#CancelToken)
> - [Konfiguracja serwera dla wywołania usługi sieci Web opóźnienia wysokiej współbieżności/wysoki](#ServerConfig)
> 
> Kompletnego przykładu znajduje się w tym samouczku w  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) na [GitHub](https://github.com/) lokacji.


Strony sieci Web platformy ASP.NET 4.5 w połączeniu [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) umożliwia rejestrowanie metod asynchronicznych, które zwracają obiekt typu [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 wprowadzono asynchroniczne koncepcji programowania nazywane [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) i obsługuje funkcję ASP.NET 4.5 [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Zadania są reprezentowane przez **zadań** typu i powiązanych typów z [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) przestrzeni nazw. .NET Framework 4.5 opiera się na tym obsługa komunikacji asynchronicznej z [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych, które sprawiają, że praca z [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) znacznie mniej złożona niż poprzednie obiektów metody asynchroniczne. [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe jest syntaktycznych skrócona forma wskazujący, który fragment kodu asynchronicznie ma oczekiwać na innych części kodu. [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) — słowo kluczowe reprezentuje wskazówkę, która służy do oznaczania metod jako opartego na zadaniach asynchronicznej metody. Kombinacja **await**, **async**i **zadań** obiektu ułatwia do pisania kodu asynchronicznego w programie .NET 4.5. Nowy model do metod asynchronicznych jest nazywany *wzorca asynchronicznego opartego na zadaniach* (**wybierz**). Ten samouczek zakłada, masz pewną znajomość asynchronicznych za pomocą programing [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw.

Aby uzyskać więcej informacji na temat używania [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw, zobacz następujące odwołania.

- [Oficjalny dokument: Asynchrony w .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await — często zadawane pytania](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programowanie asynchroniczne programu Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Jak żądania są przetwarzane przez puli wątków

Na serwerze sieci web programu .NET Framework przechowuje pulę wątków, które są używane do obsługi żądań ASP.NET. Gdy żądanie dociera, wątków z puli jest wysyłane do przetwarzania tego żądania. Jeśli żądanie jest przetwarzane synchronicznie, wątku, który przetwarza żądania jest zajęty podczas żądanie jest przetwarzane i że wątek nie może zrealizować innego żądania.   
  
To może nie być problem, ponieważ pula wątków może się wystarczająco duże, aby pomieścić wiele wątków zajęty. Jednak jest ograniczona liczba wątków w puli wątków (domyślna wartość maksymalna .NET 4.5 jest 5000). W dużych aplikacji o wysokiej współbieżności długotrwałe żądania może być zajęta wszystkich dostępnych wątków. Ten stan jest nazywany zablokowania wątku. Po osiągnięciu tego warunku, serwer sieci web kolejki żądań. Jeśli zapełnienia kolejki żądań serwera sieci web odrzuca żądania z stan HTTP 503 (serwer jest zbyt zajęty). Pula wątków CLR ma ograniczenia na nowe iniekcji wątku. Jeśli seryjnym współbieżności (to znaczy witryny sieci web może nagle get dużą liczbę żądań) i wszystkie wątki żądanie dostępne są zajęte ze względu na wywołania zaplecza duże opóźnienie, szybkość iniekcji ograniczone wątku można zwiększyć bezpieczeństwo aplikacji odpowiadać bardzo źle. Ponadto w każdym nowym wątku dodane do puli wątków ma obciążenie (na przykład 1 MB pamięci stosu). Aplikacji sieci web przy użyciu metod synchronicznych wywołań duże opóźnienie usługi, którym puli wątków rozwoju domyślne .NET 4.5 maksymalnie 5 000 wątków będzie używać około 5 GB więcej pamięci niż aplikacja stanie takie same żądania obsługi przy użyciu metody asynchroniczne i wątków tylko 50. Gdy podczas wykonywania zadanie asynchroniczne, nie zawsze korzystasz wątku. Na przykład po wprowadzeniu żądanie usługi sieci web asynchroniczne, program ASP.NET nie będzie korzystać wszystkie wątki między **async** wywołanie metody i **await**. Duże opóźnienie przy użyciu puli wątków do obsługi żądań może prowadzić do dużych pamięci i niską wykorzystanie sprzętu serwera.

## <a name="processing-asynchronous-requests"></a>Przetwarzanie żądań asynchronicznych

W aplikacji sieci web, które Zobacz dużej liczby równoczesnych żądań przy rozruchu lub ma seryjnym obciążenia (gdzie współbieżności zwiększa nagle) wywołania usługi sieci web jest asynchroniczne wydłużają czas odpowiedzi aplikacji. Asynchroniczne żądanie trwa tyle samo czasu na przetwarzanie jako żądań synchronicznych. Na przykład jeśli żądanie sprawia, że usługi sieci web wywołaniu, które wymaga dwóch sekund, przyjmuje żądania dwie sekundy czy jest wykonywane synchronicznie lub asynchronicznie. Jednak podczas wywołania asynchronicznego wątku nie blokuje odpowiedzi na inne żądania podczas oczekiwania na pierwsze żądanie do wykonania. W związku z tym żądań asynchronicznych uniknięcia wzrostu puli usługi kolejkowania wiadomości i wątku żądania, gdy istnieją dużą liczbą jednoczesnych żądań, które wywołują długotrwałej operacji.

## <a id="ChoosingSyncVasync"></a>Wybieranie metody synchronicznego lub asynchronicznego

Ta sekcja zawiera wskazówki dotyczące użycie metody synchroniczna lub asynchroniczna. Są to po prostu wytycznych; zbadać każdą aplikację osobno w celu ustalenia, czy metod asynchronicznych pomocy z wydajnością.

Ogólnie rzecz biorąc Użyj metod synchronicznych następujące warunki:

- Operacje są proste lub wykonywania short.
- Prostota ma większe znaczenie niż wydajność.
- Operacje są głównie operacje Procesora zamiast operacje, które wymagają szeroką gamę dysku lub obciążenie sieci. Używanie metod asynchronicznych operacji procesora zapewnia nie korzyści i narzut powoduje więcej.

 Ogólnie rzecz biorąc można używać metod asynchronicznych następujące warunki:

- W przypadku wywoływania usługi, które mogą być używane za pośrednictwem metod asynchronicznych i używasz .NET 4.5 lub wyższej.
- Operacje są powiązane z siecią lub I/E-granicę zamiast procesora.
- Równoległość ma większe znaczenie niż prostota kodu.
- Chcesz udostępniają mechanizm umożliwiający użytkownikom anulować żądanie długotrwałe.
- Gdy korzyści przełączania wątków limit przeprowadzi koszt przełączania kontekstu. Ogólnie rzecz biorąc należy ustawić metody asynchroniczne Jeśli metoda synchroniczna blokuje wątku żądania ASP.NET podczas wykonywania żadne czynności. Za pomocą wywołania asynchronicznego, wątku żądania programu ASP.NET nie jest blokowane podczas żadne czynności podczas oczekiwania na żądanie usługi sieci web zakończyć.
- Testowanie pokazuje, czy operacji blokowania wąskich gardeł wydajności lokacji i usług IIS może obsłużyć więcej żądań przy użyciu metod asynchronicznych tymi połączeniami blokowania.

 Do pobrania przykładowych pokazano, jak skutecznie używać metod asynchronicznych. Przykładu dostępnego zaprojektowano tak, aby zapewnić prosty pokaz programowania asynchronicznego w programie ASP.NET 4.5. Przykład nie jest przeznaczona do architektury odwołania do programowania asynchronicznego w programie ASP.NET. Wywołuje program przykładowy [ASP.NET Web API](../../../web-api/index.md) metod, które z kolei wymagają [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) do symulowania wywołania usługi sieci web długotrwałe. Większość aplikacji produkcyjnych, nie będą widoczne takie oczywiste korzyści wynikające z używania metod asynchronicznych.   
  
Kilka aplikacji wymaga wszystkich metod asynchronicznych. Często konwertowanie kilka metod synchronicznych do metod asynchronicznych zapewnia najlepsze zwiększenia efektywności ilości pracy wymagane.

## <a id="SampleApp"></a>Przykładowa aplikacja

Możesz pobrać przykładową aplikację z [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) na [GitHub](https://github.com/) lokacji. Repozytorium zawiera trzy projekty:

- *WebAppAsync*: projekt formularzy sieci Web ASP.NET, który wykorzystuje interfejs API sieci Web **WebAPIpwg** usługi. Większość kodu dla tego samouczka jest w tym projekcie.
- *WebAPIpgw*: projekt programu ASP.NET MVC 4 interfejsu API sieci Web, który implementuje `Products, Gizmos and Widgets` kontrolerów. Zapewnia dane dla *WebAppAsync* projektu i *Mvc4Async* projektu.
- *Mvc4Async*: projekt programu ASP.NET MVC 4, który zawiera kod używany w innym samouczka. Wykonywania wywołań interfejsu API sieci Web do **WebAPIpwg** usługi.

## <a id="GizmosSynch"></a>Na stronie synchroniczne Gizmos

 Poniższy kod przedstawia `Page_Load` metoda synchroniczna, który służy do wyświetlania listy gizmos. (W tym artykule gizmo jest urządzenie mechaniczne fikcyjne). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Poniższy kod przedstawia `GetGizmos` metody gizmo usługi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos` Metoda przekazuje identyfikatora URI z usługą HTTP interfejsu API sieci Web programu ASP.NET, która zwraca listę gizmos danych. *WebAPIpgw* projektu zawiera implementację interfejsu API sieci Web `gizmos, widget` i `product` kontrolerów.  
Na poniższej ilustracji przedstawiono stronę gizmos z przykładowy projekt.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Tworzenie stron Gizmos asynchroniczne

Przykład używa nowego [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) i [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) słowa kluczowe (dostępne w .NET 4.5 i programu Visual Studio 2012) pozwala kompilatora odpowiada za utrzymywanie złożone przekształcenia niezbędne do Programowanie asynchroniczne. Kompilator umożliwia pisanie kodu za pomocą, który tworzy przepływ sterowania synchroniczne C# w i kompilator automatycznie stosuje przekształcenia trzeba użyć wywołania zwrotne w celu unikania blokowania wątków.

Asynchroniczne strony ASP.NET należy uwzględnić [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy z `Async` atrybut na wartość "true". Poniższy kod przedstawia [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy z `Async` atrybut na wartość "true" dla *GizmosAsync.aspx* strony.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Poniższy kod przedstawia `Gizmos` synchroniczne `Page_Load` — metoda i `GizmosAsync` asynchroniczne strony. Jeśli przeglądarka obsługuje [HTML 5 &lt;oznaczyć&gt; elementu](http://www.w3.org/wiki/HTML/Elements/mark), zmiany w `GizmosAsync` w Wyróżnij żółty.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Wersja asynchroniczna:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Następujące zmiany zostały zastosowane, aby umożliwić `GizmosAsync` strona była asynchroniczna.

- [Strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywa musi mieć `Async` atrybut na wartość "true".
- `RegisterAsyncTask` Metoda służy do rejestrowania zadania asynchronicznego zawierającego kod, który uruchamia asynchronicznie.
- Nowy `GetGizmosSvcAsync` metoda jest oznaczona atrybutem [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) — słowo kluczowe, które informuje kompilator, aby wygenerować wywołań zwrotnych dla części treści i automatycznie utworzyć `Task` który jest zwracany.
- &quot;Asynchroniczne&quot; został dołączony do nazwy metody asynchronicznej. Dołączanie "Async" nie jest wymagana, ale jest Konwencji podczas zapisywania metod asynchronicznych.
- Zwracany typ nowej nowe `GetGizmosSvcAsync` jest metoda `Task`. Zwracany typ `Task` reprezentuje pracy w toku i dostarcza wywołań metody z dojściem za pośrednictwem której czekać na zakończenie operacji asynchronicznej.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe została zastosowana do wywołania usługi sieci web.
- Interfejs API usługi sieci web asynchroniczne została wywołana (`GetGizmosAsync`).

Wewnątrz `GetGizmosSvcAsync` innej metody asynchroniczne, treści metody `GetGizmosAsync` jest wywoływana. `GetGizmosAsync`natychmiast zwraca `Task<List<Gizmo>>` który ostatecznie zostanie zakończony, gdy dane są dostępne. Ponieważ nie chcesz wykonywanie dodatkowych czynności, aż do uzyskania danych gizmo kod oczekujące na zadanie (przy użyciu **await** — słowo kluczowe). Można użyć **await** — słowo kluczowe tylko w metodach opatrzoną **async** — słowo kluczowe.

**Await** — słowo kluczowe nie blokuje wątek, do czasu ukończenia zadania. Rejestruje się pozostałe metody jako wywołania zwrotnego dla zadania i natychmiast zwraca. Po ukończeniu ostatecznie oczekiwano zadania, spowoduje wywołanie wywołanie zwrotne i w związku z tym wznowić wykonywanie prawa metody, w którym przerwał. Aby uzyskać więcej informacji na temat używania [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw, zobacz [odwołania async](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Poniższy kod przedstawia `GetGizmos` i `GetGizmosAsync` metody.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Asynchroniczne zmiany są podobne do tych wprowadzone **GizmosAsync** powyżej. 

- Podpis metody został oznaczony za pomocą [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) — słowo kluczowe, zwracany typ został zmieniony na `Task<List<Gizmo>>`, i *Async* został dołączony do nazwy metody.
- Asynchroniczną [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) klasa jest używana zamiast synchroniczne [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) klasy.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe została zastosowana do [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) metody asynchronicznej.

Na poniższej ilustracji przedstawiono widok gizmo asynchronicznego.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Przeglądarki prezentację danych gizmos jest identyczny z widoku utworzonego przez wywołanie synchroniczne. Jedyną różnicą jest to wersja asynchroniczna może być więcej wydajności pod dużym obciążeniem.

## <a name="registerasynctask-notes"></a>Informacje o RegisterAsyncTask

Metody podłączonymi z `RegisterAsyncTask` zostanie uruchomiona natychmiast po [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Umożliwia także async void strony zdarzenia bezpośrednio, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Wadą interfejsu async void zdarzeń jest, że deweloperzy już ma pełną kontrolę nad podczas wykonywania zdarzenia. Na przykład jeśli obie aspx oraz. Zdefiniuj wzorzec `Page_Load` zdarzeń i co najmniej jeden z nich są asynchroniczne, kolejność wykonywania nie może zagwarantować. Tej samej kolejności indeterminiate dla programów obsługi zdarzeń z systemem innym niż (takie jak `async void Button_Click` ) ma zastosowanie. Dla większości deweloperów powinna być akceptowane, ale tych, którzy wymagają pełną kontrolę nad kolejność wykonywania należy używać tylko interfejsów API, takich jak `RegisterAsyncTask` który korzystać z metod, które zwraca obiekt zadania.

## <a id="Parallel"></a>Wykonywanie operacji na wielu równolegle

Metod asynchronicznych ma znaczących korzyści za pośrednictwem metod synchronicznych, gdy akcję, należy wykonać kilka operacji niezależnych. W przykładowym podany synchroniczne strony *PWG.aspx*(dla produktów, elementów widget i Gizmos) są wyświetlane wyniki trzy wywołania usługi sieci web w celu uzyskania listy produktów, elementów widget i gizmos. [ASP.NET Web API](../../../web-api/index.md) projektu, który zawiera te usługi używa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) wywołuje w celu symulowania opóźnienia lub powolnej sieci. Jeśli opóźnienie jest równa 500 milisekund asynchroniczną *PWGasync.aspx* strona zajmuje trochę ponad 500 MS, aby ukończyć podczas synchronicznego `PWG` wersji przejmuje 1500 MS. Synchroniczne *PWG.aspx* strony przedstawiono w poniższym kodzie.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asynchroniczną `PWGasync` kodzie przedstawiono poniżej.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Na poniższej ilustracji przedstawiono widok zwrócona z asynchronicznej *PWGasync.aspx* strony.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Przy użyciu Token anulowania

Zwracanie metod asynchronicznych `Task`czy można anulować, będącą podejmują [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametru, gdy zostało ono określone z `AsyncTimeout` atrybutu [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy. Poniższy kod przedstawia *GizmosCancelAsync.aspx* strony z limitem czasu równym na sekundę.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Poniższy kod przedstawia *GizmosCancelAsync.aspx.cs* pliku.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

W przykładowej aplikacji pod warunkiem, wybierając *GizmosCancelAsync* link wywołania *GizmosCancelAsync.aspx* strony i prezentuje anulowanie wywołania asynchronicznego (za pomocą limitu czasu). Ponieważ czas opóźnienia losowego zakresu, może być konieczne odświeżenie strony kilka razy na komunikat o błędzie limitu czasu.

## <a id="ServerConfig"></a>Konfiguracja serwera dla wywołania usługi sieci Web opóźnienia wysokiej współbieżności/wysoki

Aby wykorzystać zalety aplikacji sieci web asynchroniczne, może być konieczne dokonanie pewnych zmian w domyślnej konfiguracji serwera. Należy pamiętać o następujących w uwadze podczas konfigurowania i obciążeniowe testowania aplikacji sieci web asynchronicznego.

- Windows 7, Windows Vista, Windows 8 i wszystkie klienckie systemy operacyjne Windows mieć maksymalnie 10 równoczesnych żądań. Będziesz potrzebować systemu operacyjnego Windows Server, aby można było wykorzystać zalety metod asynchronicznych mocno obciążony.
- Zarejestruj .NET 4.5 z usługami IIS w wierszu polecenia z podwyższonym poziomem uprawnień, za pomocą następującego polecenia:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 Zobacz [narzędzie rejestracji programu ASP.NET usług IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Może być konieczne w celu zwiększenia [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limitu kolejki z domyślnej wartości 1000 do 5000. Jeśli ustawienie jest zbyt niska, mogą pojawić się [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) odrzucanie żądań o stan HTTP 503. Aby zmienić limit kolejki sterownik HTTP.sys:

    - Otwórz Menedżera usług IIS i przejdź do okienka pul aplikacji.
    - Kliknij prawym przyciskiem myszy docelową pulę aplikacji, a następnie wybierz **Zaawansowane ustawienia**.  
        ![Zaawansowane](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - W **Zaawansowane ustawienia** okno dialogowe, zmień *długość kolejki* z 1000 do 5000.  
        ![Długość kolejki](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Należy pamiętać, obrazy powyżej, .NET framework znajduje się w wersji 4.0, mimo że pula aplikacji jest przy użyciu platformy .NET 4.5. Aby poznać tę rozbieżność, zobacz następujące tematy:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Jeśli aplikacja korzysta z usług sieci web lub może być konieczne System.NET do komunikowania się z wewnętrznej bazy danych za pośrednictwem protokołu HTTP należy zwiększyć [connectionmanagement — / maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elementu. Dla aplikacji ASP.NET to jest ograniczona przez funkcję automatycznej konfiguracji do 12 razy liczba procesorów. Oznacza to, że na quad-proc, użytkownik może mieć co najwyżej 12 \* 4 = 48 równoczesnych połączeń punkt końcowy IP. Ponieważ to jest związany z [autokonfiguracji](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), najprostszym sposobem, aby zwiększyć `maxconnection` w programie ASP.NET jest skonfigurowanie aplikacji [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programowo w z `Application_Start` metody w *global.asax* pliku. Zobacz przykład Pobierz przykład.
- W programie .NET 4.5, domyślnie 5000 dla [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) powinna być poprawnie.

## <a name="contributors"></a>Współautorzy

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tomasz Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
