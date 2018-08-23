---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Używanie metod asynchronicznych na platformie ASP.NET 4.5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia asynchronicznych aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio Express 2012 for Web, która jest bezpłatna...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 5e5fe83e802980a5783c3d77454d5fb0ee7d5a91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756029"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Używanie metod asynchronicznych na platformie ASP.NET 4.5
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawy tworzenia asynchronicznego wzorca ASP.NET Web Forms aplikacji za pomocą [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11), która jest bezpłatna wersja programu Microsoft Visual Studio. Można również użyć [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Poniższe sekcje są uwzględnione w tym samouczku.
> 
> - [Sposób przetwarzania żądań w puli wątków](#HowRequestsProcessedByTP)
> - [Wybieranie metod synchronicznych lub asynchronicznych](#ChoosingSyncVasync)
> - [Przykładowa aplikacja](#SampleApp)
> - [Na stronie synchroniczne Gizmo](#GizmosSynch)
> - [Tworzenie strony Gizmo asynchroniczne](#CreatingAsynchGizmos)
> - [Wykonywanie operacji na wielu równolegle](#Parallel)
> - [Korzystając z tokena odwołania](#CancelToken)
> - [Konfiguracja serwera dla wywołania usługi sieci Web opóźnienie współbieżności wysoki/wysoka](#ServerConfig)
> 
> Pełny przykład znajduje się w tym samouczku w  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) na [GitHub](https://github.com/) lokacji.


Strony sieci Web platformy ASP.NET 4.5 w połączeniu [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) umożliwia rejestrowanie metod asynchronicznych, które zwracają obiekt typu [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 wprowadzono asynchronicznego koncepcji programowania, nazywane [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) i obsługuje ASP.NET 4.5 [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Zadania są reprezentowane przez **zadań** typu i powiązanych typów [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) przestrzeni nazw. .NET Framework 4.5 jest oparta na tej asynchronicznej obsługi za pomocą [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych, które ułatwić pracę z [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) obiektów znacznie mniej złożone niż wcześniejsza metody asynchroniczne. [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe jest syntaktycznych skrótem wskazująca, że fragment kodu asynchronicznego powinien zaczekać na inne części kodu. [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) — słowo kluczowe reprezentuje wskazówkę, która służy do oznaczania metod jako opartego na zadaniach asynchronicznej metody. Kombinacja **await**, **async**i **zadań** obiektu ułatwia pisanie kodu asynchronicznego w .NET 4.5. Nowy model, w przypadku metod asynchronicznych jest nazywany *wzorca asynchronicznego opartego na zadaniach* (**NACIŚNIJ**). W tym samouczku założono, masz pewną znajomość asynchroniczne przy użyciu programistyczne [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw.

Aby uzyskać więcej informacji na temat korzystania z [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw, zobacz następujące informacje.

- [Oficjalny dokument: Asynchroniczność na platformie .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await — często zadawane pytania](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programowanie asynchroniczne w Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Sposób przetwarzania żądań w puli wątków

Na serwerze sieci web programu .NET Framework obsługuje puli wątków, które są używane do obsługi żądań ASP.NET. Po odebraniu żądania wątków z puli jest wysyłane do przetworzenia tego żądania. Jeśli żądanie jest przetwarzane synchronicznie, wątek, który przetwarza żądanie jest zajęty podczas żądania jest przetwarzany, że wątek nie może obsłużyć inne żądanie.   
  
Może to nie być problemem, ponieważ w puli wątków może pracować z wystarczająco dużą do obsługi wielu zajęte wątki. Jednakże, jest ograniczona liczba wątków w puli wątków (wartość domyślna dla platformy .NET 4.5 to 5000). W dużych aplikacji z wysoką współbieżności długotrwałych żądań wszystkie dostępne wątki może być zajęta. Ten stan jest nazywany zablokowania wątku. Po osiągnięciu tego warunku, serwer sieci web kolejki żądań. Jeśli zapełnieniu kolejki żądań serwera sieci web odrzucanie żądań ze stanem HTTP 503 (serwer jest zbyt zajęty). Pula wątków CLR ma ograniczenia na nowy wątek wstrzyknięć kodu. Jeśli intensywnego współbieżności (oznacza to, że witryny sieci web można nagle pobierać dużą liczbę żądań) i wszystkie wątki żądanie dostępne są zajęte ze względu na wywołania zaplecza dużymi opóźnieniami, szybkość iniekcji ograniczone wątku można uczynić aplikację reagować bardzo źle. Ponadto każdy nowy wątek dodane do puli wątków ma obciążenie (np. 1 MB pamięci stosu). W przypadku aplikacji sieci web przy użyciu synchronicznej metody do wywołania duże opóźnienie usługi, którym puli wątków rozwoju domyślne .NET 4.5 maksymalnie 5 000 wątków będzie korzystać około 5 GB więcej pamięci niż aplikacja może takie same żądania obsługi przy użyciu metody asynchroniczne i tylko 50 wątków. Podczas prowadzenia prac asynchronicznych, nie zawsze jest używany wątek. Na przykład, gdy żądanie usługi sieci web w asynchronicznej, ASP.NET nie będzie korzystać z żadnych wątków między **async** wywołania metody oraz **await**. Przy użyciu puli wątków do obsługi żądań dużymi opóźnieniami może prowadzić do zużycia dużej ilości pamięci i niską wykorzystanie sprzętu serwera.

## <a name="processing-asynchronous-requests"></a>Przetwarzanie żądań asynchronicznych

W aplikacji sieci web, które Zobacz dużą liczbą jednoczesnych żądań przy rozruchu lub ma obciążenia intensywnego (gdzie współbieżności zwiększa nagle) wywołań usługi sieci web jest asynchroniczne zwiększa szybkość reakcji aplikacji. Asynchroniczne żądanie trwa tyle samo czasu na przetworzenie jako żądań synchronicznych. Na przykład jeśli żądanie sprawia, że usługi sieci web, w wywołaniu, które wymaga dwóch sekund, przyjmuje żądania dwóch sekund czy jest wykonywane synchronicznie lub asynchronicznie. Jednak podczas wywołania asynchronicznego wątku nie jest zablokowany z odpowiada na inne żądania podczas oczekiwania na pierwsze żądanie zakończyć. W związku z tym żądań asynchronicznych uniknięcia wzrostu puli żądania usługi kolejkowania i wątku, gdy są dużo współbieżnych żądań, które wywołują długotrwałych operacji.

## <a id="ChoosingSyncVasync"></a>  Wybieranie metod synchronicznych lub asynchronicznych

Ta sekcja zawiera wskazówki dotyczące kiedy należy używać metod synchroniczna lub asynchroniczna. Są to po prostu wytycznych; zbadać każdą aplikację pojedynczo, aby określić, czy metod asynchronicznych poprawić wydajność.

Ogólnie rzecz biorąc należy użyć metod synchronicznych następujące warunki:

- Operacje są proste lub krótko działających.
- Prostota jest ważniejsza niż wydajność.
- Operacje są głównie operacje Procesora zamiast operacje obejmujące rozbudowane dysków lub obciążenie sieci. Używanie metod asynchronicznych na operacje zależne od Procesora CPU zapewnia nie korzyści, a wyniki w większej liczbie obciążenie.

  Ogólnie rzecz biorąc należy używać metod asynchronicznych następujące warunki:

- W przypadku wywoływania usługi, które mogą być wykorzystywane za pomocą metod asynchronicznych i korzystania z platformy .NET 4.5 lub nowszej.
- Operacje są powiązane z siecią lub I/O-granicę zamiast zależne od Procesora CPU.
- Równoległość jest ważniejsza niż uproszczenia kodu.
- Chcesz zapewnić mechanizm, który pozwala użytkownikom na anulowanie długotrwałe żądania.
- Kiedy korzyść przełączania wątków się przeprowadzi koszt przełącznika kontekstu. Ogólnie rzecz biorąc należy upewnić metody asynchronicznej Jeśli metoda synchroniczna blokuje wątek żądania programu ASP.NET, podczas wykonywania żadnej pracy. Za pomocą wywołania asynchronicznego, wątek żądania programu ASP.NET nie jest zablokowany podczas oczekiwania na żądanie usługi sieci web zakończyć, wykonując żadnej pracy.
- Testowanie pokazuje, że blokowania operacji jest wąskim gardłem wydajności witryn i usług IIS może obsługiwać więcej żądań przy użyciu metod asynchronicznych na te wywołania blokowania.

  Do pobrania przykładowych pokazano, jak skutecznie używać metod asynchronicznych. Przykładowym został zaprojektowany w celu zapewnienia prosty pokaz programowania asynchronicznego w programie ASP.NET 4.5. Plik nie ma być architektury referencyjnej dla programowania asynchronicznego w programie ASP.NET. Wywołuje program przykładowy [ASP.NET Web API](../../../web-api/index.md) metod, które z kolei wywołać [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) do symulacji wywołania usługi sieci web długotrwałych. Większości aplikacji produkcyjnych, nie będą widoczne takie oczywiste korzyści wynikające z używania metod asynchronicznych.   
  
Kilka aplikacji wymaga wszystkich metod asynchronicznych. Często konwertowanie kilka metod synchronicznych metod asynchronicznych zapewnia najlepsze zwiększenie wydajności, ilości pracy wymaganej.

## <a id="SampleApp"></a>  Przykładowa aplikacja

Możesz pobrać przykładową aplikację z [ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET) na [GitHub](https://github.com/) lokacji. Repozytorium zawiera trzy projekty:

- *WebAppAsync*: projekt formularzy sieci Web ASP.NET, który korzysta z interfejsu API sieci Web **WebAPIpwg** usługi. Większość kodu, w tym samouczku to w tym projekcie.
- *WebAPIpgw*: interfejs API sieci Web platformy ASP.NET MVC 4 projektu, który implementuje `Products, Gizmos and Widgets` kontrolerów. Zapewnia dane dotyczące *WebAppAsync* projektu i *Mvc4Async* projektu.
- *Mvc4Async*: projekt platformy ASP.NET MVC 4, który zawiera kod używany w innym samouczku. To sprawia, że wywołania interfejsu API sieci Web **WebAPIpwg** usługi.

## <a id="GizmosSynch"></a>  Na stronie synchroniczne Gizmo

 Poniższy kod przedstawia `Page_Load` synchroniczne metody, która jest używana do wyświetlania listy elementy gizmo. (W tym artykule gizmo jest urządzenie mechaniczne fikcyjnej). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Poniższy kod przedstawia `GetGizmos` metoda gizmo usługi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos` Metoda przekazuje identyfikatora URI z usługą HTTP interfejsu API sieci Web programu ASP.NET, która zwraca listę gizmo danych. *WebAPIpgw* projekt zawiera implementację interfejsu API sieci Web `gizmos, widget` i `product` kontrolerów.  
Na poniższej ilustracji przedstawiono na stronie elementy gizmo z przykładowego projektu.

![Elementy gizmo](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Tworzenie strony Gizmo asynchroniczne

W przykładzie użyto nowy [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) i [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) słowa kluczowe (dostępne w .NET 4.5 i programu Visual Studio 2012) aby pozwolić kompilatorowi odpowiada za utrzymywanie niezbędne do przekształcenia skomplikowane Programowanie asynchroniczne. Kompilator pozwala napisać kod, używając konstrukcji języka C# firmy synchroniczne sterowanie przepływem i kompilator automatycznie stosuje przekształcenia, które są niezbędne do korzystania z wywołań zwrotnych w celu unikania blokowania wątków.

Asynchroniczne strony ASP.NET mogą zawierać [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy z `Async` atrybut na wartość "true". Poniższy kod przedstawia [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy z `Async` atrybut na wartość "true" dla *GizmosAsync.aspx* strony.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Poniższy kod przedstawia `Gizmos` synchroniczne `Page_Load` metody i `GizmosAsync` asynchronicznego strony. Jeśli przeglądarka obsługuje [HTML 5 &lt;oznaczyć&gt; elementu](http://www.w3.org/wiki/HTML/Elements/mark), zobaczysz zmiany w `GizmosAsync` w Wyróżnij żółty.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Asynchroniczna wersja:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Następujące zmiany zostały zastosowane, aby umożliwić `GizmosAsync` strony być asynchroniczne.

- [Strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywa musi mieć `Async` atrybut na wartość "true".
- `RegisterAsyncTask` Metoda służy do rejestrowania zadanie asynchroniczne zawierającego kod, który jest uruchamiany asynchronicznie.
- Nowy `GetGizmosSvcAsync` metoda jest oznaczona atrybutem [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słowo kluczowe, które informuje kompilator, aby wygenerować wywołania zwrotne dla części treści, a także automatyczne tworzenie `Task` zwracanym.
- &quot;Asynchroniczne&quot; został dołączony do nazwy metody asynchronicznej. Dołączanie "Async" nie jest wymagana, ale jest do Konwencja, podczas zapisywania metod asynchronicznych.
- Zwracany typ nowej `GetGizmosSvcAsync` metodą jest `Task`. Zwracany typ `Task` reprezentuje pracę w toku i zapewnia obiektów wywołujących metodę z dojściem za pomocą którego można czekać na zakończenie operacji asynchronicznej.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe została zastosowana do wywołania usługi sieci web.
- Wywołano asynchroniczną internetowy interfejs API usługi (`GetGizmosAsync`).

Wewnątrz `GetGizmosSvcAsync` metoda treści innej metody asynchronicznej, `GetGizmosAsync` jest wywoływana. `GetGizmosAsync` natychmiast zwraca `Task<List<Gizmo>>` , zostanie ostatecznie zakończona, gdy dane są dostępne. Ponieważ nie chcesz wykonywanie dodatkowych czynności, aż do uzyskania danych gizmo, kod czeka na zadanie (przy użyciu **await** — słowo kluczowe). Możesz użyć **await** — słowo kluczowe tylko w metodach oznaczony za pomocą **async** — słowo kluczowe.

**Await** — słowo kluczowe nie blokuje wątku, do czasu ukończenia zadania. Rejestruje pozostałą część metody jako wywołania zwrotnego dla zadania i natychmiast powraca. Gdy ostatecznie zakończy się oczekiwane zadanie, spowoduje to wywołanie wywołania zwrotnego i dlatego wznowić wykonywanie metody po prawej stronie, tam, gdzie ją przerwaliśmy. Aby uzyskać więcej informacji na temat korzystania z [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) przestrzeni nazw, zobacz [odwołania async](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Poniższy kod przedstawia `GetGizmos` i `GetGizmosAsync` metody.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Asynchroniczne zmiany są podobne do wprowadzonych **GizmosAsync** powyżej. 

- Podpis metody został oznaczony za pomocą [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) — słowo kluczowe, zwracany typ została zmieniona na `Task<List<Gizmo>>`, i *Async* został dołączony do nazwy metody.
- Asynchroniczną [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) klasa jest używana zamiast synchronicznej [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) klasy.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) — słowo kluczowe została zastosowana do [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) metody asynchronicznej.

Na poniższej ilustracji przedstawiono widok gizmo asynchronicznego.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Prezentacji przeglądarek danych gizmo jest identyczna z widoku utworzonego przez wywołanie synchroniczne. Jedyną różnicą jest to wersja asynchroniczna mogą działać wydajniej pod dużym obciążeniem.

## <a name="registerasynctask-notes"></a>Informacje o RegisterAsyncTask

Metody podłączonymi za pomocą `RegisterAsyncTask` będzie uruchamiać natychmiast po [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Umożliwia także zdarzenia void strony async bezpośrednio, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Wadą void zdarzeń async jest, że deweloperów już ma pełną kontrolę nad podczas wykonywania zdarzenia. Na przykład jeśli oba .aspx, a jeśli tak, to a. Zdefiniuj wzorzec `Page_Load` zdarzeń i jednego lub obu z nich są asynchroniczne, kolejność wykonywania nie można zagwarantować. Takiej samej kolejności indeterminiate dla procedury obsługi zdarzeń innych (takich jak `async void Button_Click` ) ma zastosowanie. Dla większości deweloperów powinna to być akceptowane, ale osób, które wymagają pełną kontrolę nad kolejność wykonywania należy używać tylko interfejsów API, takich jak `RegisterAsyncTask` , korzystanie z metod, które zwracają obiekt zadania.

## <a id="Parallel"></a>  Wykonywanie operacji na wielu równolegle

Metod asynchronicznych mieć znaczącą korzyścią za pośrednictwem metod synchronicznych, gdy akcję, należy wykonać kilka operacji niezależnych. W przykładowym, strona synchroniczne *PWG.aspx*(w przypadku produktów, elementów widget i elementy Gizmo) służy do wyświetlania wyników trzy wywołania usługi sieci web, aby uzyskać listę produktów, elementów widget i elementy gizmo. [ASP.NET Web API](../../../web-api/index.md) projektu, który udostępnia te usługi używa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) do symulowania opóźnienia lub wolno działającą sieć wywołań. Gdy opóźnienie ma wartość 500 milisekund asynchroniczną *PWGasync.aspx* strona zajmuje to nieco ponad 500 milisekund do wykonania podczas synchronicznej `PWG` wersji przejmuje 1500 MS. Synchronicznej *PWG.aspx* strona jest wyświetlana w poniższym kodzie.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asynchroniczną `PWGasync` kodzie przedstawiono poniżej.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Na poniższej ilustracji przedstawiono widok zwrócone w wyniku asynchronicznego *PWGasync.aspx* strony.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  Korzystając z tokena odwołania

Metod asynchronicznych, zwracając `Task`czy można anulować, który jest przyjmują [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametru, jeśli została dostarczona z `AsyncTimeout` atrybutu [strony](https://msdn.microsoft.com/library/ydy4x04a.aspx) dyrektywy. Poniższy kod przedstawia *GizmosCancelAsync.aspx* strony z limitem czasu równym na sekundę.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Poniższy kod przedstawia *GizmosCancelAsync.aspx.cs* pliku.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

W przykładowej aplikacji pod warunkiem, wybierając *GizmosCancelAsync* link wywołania *GizmosCancelAsync.aspx* strony i pokazuje anulowanie wywołania asynchronicznego (przez upływem limitu czasu). Ponieważ czas opóźnienia znajduje się w zakresie losowych, może być konieczne odświeżenie strony kilka razy, aby uzyskać komunikat o błędzie limitu czasu.

## <a id="ServerConfig"></a>  Konfiguracja serwera dla wywołania usługi sieci Web opóźnienie współbieżności wysoki/wysoka

Aby wykorzystać zalety aplikacji sieci web asynchronicznego, konieczne może być pewnych zmian w domyślnej konfiguracji serwera. Pamiętaj o następujących ducha podczas konfigurowania i testowanie aplikacji sieci web asynchronicznego obciążeniowe.

- Windows 7, Windows Vista, Windows 8 i wszystkie systemy operacyjne klienta Windows mieć maksymalnie 10 współbieżnych żądań. Potrzebna będzie systemu operacyjnego Windows Server, aby zobaczyć korzyści wynikające z metod asynchronicznych pod dużym obciążeniem.
- Zarejestrowanie .NET 4.5 programu IIS z wiersza polecenia z podwyższonym poziomem uprawnień, korzystając z następującego polecenia:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
  Zobacz [narzędzie rejestracji usług IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Może być konieczne zwiększenie [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limit kolejki z domyślnej wartości 1000 i 5000. Jeśli ustawienie jest zbyt niska, może zostać wyświetlony [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) odrzucanie żądań ze stanem HTTP 503. Aby zmienić limit kolejki sterownik HTTP.sys:

    - Otwórz Menedżera usług IIS i przejdź do okienka pul aplikacji.
    - Kliknij prawym przyciskiem myszy docelową pulę aplikacji, a następnie wybierz pozycję **Zaawansowane ustawienia**.  
        ![Zaawansowane](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - W **Zaawansowane ustawienia** okno dialogowe, zmiana *długość kolejki* od 1000 do 5000.  
        ![Długość kolejki](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Należy zwrócić uwagę na obrazach powyżej, .NET framework znajduje się w wersji 4.0, mimo że pula aplikacji jest przy użyciu platformy .NET 4.5. Aby poznać tę rozbieżność, zobacz następujące tematy:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Jeśli aplikacja korzysta z usług sieci web lub może być konieczne zwiększenie przestrzeni nazw System.NET nawiązać połączenia z usługą zaplecza za pośrednictwem protokołu HTTP należy [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elementu. Dla aplikacji platformy ASP.NET to jest ograniczona przez funkcję automatycznej konfiguracji do 12 razy liczba procesorów CPU. Oznacza to, że na cztery procesory mogą istnieć maksymalnie 12 \* 4 = 48 jednoczesnych połączeń do punktu końcowego adresu IP. Ponieważ to jest powiązany z [autokonfiguracji](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), najłatwiejszy sposób na zwiększenie `maxconnection` na platformie ASP.NET aplikacja ma ustawiony [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programowo w z `Application_Start` method in Class metoda *global.asax* pliku. Zobacz przykład pobrać przykład.
- W .NET 4.5, wartość domyślna 5000 dla [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) powinny być dobrym stanie.

## <a name="contributors"></a>Współautorzy

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
