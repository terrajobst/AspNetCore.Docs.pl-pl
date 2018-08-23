---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hosta samodzielnego ASP.NET Web API 1 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ASP.NET Web API nie wymaga usług IIS. Interfejs API sieci web można hosta samodzielnego procesu hosta. W tym samouczku pokazano, jak hostować interfejs API sieci web wewnątrz applic konsoli...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755584"
---
<a name="self-host-aspnet-web-api-1-c"></a>Hosta samodzielnego ASP.NET Web API 1 (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API nie wymaga usług IIS. Interfejs API sieci web można hosta samodzielnego procesu hosta. W tym samouczku pokazano, jak hostować interfejs API sieci web w aplikacji konsoli.
> 
> **Nowe aplikacje powinny używać OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web.** Zobacz [korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Utwórz projekt aplikacji konsoli

Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Windows**. Na liście szablonów projektu wybierz **aplikację Konsolową**. Nadaj projektowi nazwę &quot;host własny&quot; i kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Ustaw docelową platformę (Visual Studio 2010)

Jeśli używasz programu Visual Studio 2010, należy zmienić platformę docelową na .NET Framework 4.0. (Domyślnie projekt jest ukierunkowany szablonu [profilu programu .net Framework klienta](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. W **platformę docelową** listy rozwijanej listy, zmień platformę docelową na .NET Framework 4.0. Gdy zostanie wyświetlony monit, aby zastosować zmianę, kliknij przycisk **tak**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Zainstaluj Menedżera pakietów NuGet

Menedżer pakietów NuGet jest najprostszym sposobem dodania zestawy interfejsu API sieci Web do projektu niedotyczący środowiska ASP.NET.

Aby sprawdzić, czy zainstalowano Menedżer pakietów NuGet, kliknij **narzędzia** menu w programie Visual Studio. Jeśli widzisz menu elementu o nazwie **Menedżer pakietów biblioteki**, można skorzystać z Menedżera pakietów NuGet.

Aby zainstalować Menedżera pakietów NuGet:

1. Uruchom program Visual Studio.
2. Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.
3. W **rozszerzenia i aktualizacje** okno dialogowe, wybierz opcję **Online**.
4. Jeśli nie widzisz "Menedżer pakietów NuGet", wpisz "Menedżer pakietów nuget", w polu wyszukiwania.
5. Wybierz pozycję Menedżer pakietów NuGet, a następnie kliknij przycisk **Pobierz**.
6. Po ukończeniu pobierania pojawi do zainstalowania.
7. Po zakończeniu instalacji może być monitowany o ponowne uruchomienie programu Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Dodaj pakiet NuGet interfejsu API sieci Web

Po zainstalowaniu Menedżera pakietów NuGet do projektu należy dodać pakiet Self-Host interfejsu API sieci Web.

1. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**. *Uwaga*: Jeśli nie widzisz tego menu elementu, upewnij się, Menedżer pakietów NuGet, że poprawnie zainstalowane.
2. Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**
3. W **Zarządzaj pakietami NugGet** okno dialogowe, wybierz opcję **Online**.
4. W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Wybierz pakiet hosta Self interfejsu API sieci Web platformy ASP.NET, a następnie kliknij przycisk **zainstalować**.
6. Po zainstalowaniu pakietu kliknij **Zamknij** aby zamknąć okno dialogowe.

> [!NOTE]
> Upewnij się zainstalować pakiet o nazwie Microsoft.AspNet.WebApi.SelfHost, nie AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.

Dodaj publiczny klasę o nazwie `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Dodaj publiczny klasę o nazwie `ProductsController`. Pochodzić z tej klasy z **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Aby uzyskać więcej informacji o kodzie, w tym kontrolerze, zobacz [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka. Ten kontroler definiuje trzy operacje GET:

| Identyfikator URI | Opis |
| --- | --- |
| / api/produktów | Zostanie wyświetlona lista wszystkich produktów. |
| / InterfejsAPI/produkty/*identyfikator* | Pobieranie produktu według identyfikatora. |
| / Interfejs API/produkty /? kategorii =*kategorii* | Zostanie wyświetlona lista produktów według kategorii. |

## <a name="host-the-web-api"></a>Hostowanie interfejsu API sieci Web

Otwórz plik Program.cs i dodaj następujące instrukcje using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Dodaj następujący kod do **Program** klasy.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Opcjonalnie) Dodaj rezerwacji Namespace adresu URL protokołu HTTP

Ta aplikacja nasłuchuje `http://localhost:8080/`. Domyślna nasłuchiwać pod określonym adresem HTTP wymaga uprawnień administratora. Po uruchomieniu tego samouczka, dlatego może być ten błąd: "HTTP nie można zarejestrować adres URL http://+:8080/" istnieją dwa sposoby, aby uniknąć tego błędu:

- Uruchom program Visual Studio z uprawnieniami administratora z podniesionymi uprawnieniami lub
- Użyj Netsh.exe, aby uzyskać uprawnienia do rezerwowania adresu URL konta.

Aby użyć Netsh.exe, otwórz wiersz polecenia z uprawnieniami administratora i wpisz następujące polecenie, polecenie: następujące czynności:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

gdzie *maszyna\nazwa_użytkownika* jest kontem użytkownika.

Po zakończeniu hostingu samodzielnego, upewnij się usunąć zastrzeżenie:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Wywoływanie interfejsu API sieci Web z aplikacji klienta (C#)

Napiszmy prostą aplikację konsolową, która wywołuje interfejs API sieci web.

Dodaj nowy projekt aplikacji konsoli do rozwiązania:

- W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz **Dodaj nowy projekt**.
- Utwórz nową aplikację konsoli o nazwie &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Użyj Menedżera pakietów NuGet Dodaj pakiet biblioteki podstawowych interfejsów API sieci Web platformy ASP.NET:

- Wybierz z menu narzędzia **Menedżer pakietów biblioteki**.
- Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**
- W **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **Online**.
- W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Wybierz pakiet biblioteki klienta interfejsu API sieci Web platformy ASP.NET firmy Microsoft, a następnie kliknij przycisk **zainstalować**.

Dodaj odwołanie w ClientApp do projektu własnego hosta:

- W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.
- Wybierz **Dodaj odwołanie**.
- W **Menadżer odwołań** okna dialogowego, w obszarze **rozwiązania**, wybierz opcję **projektów**.
- Wybierz projekt własnego hosta.
- Kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image6.png)

Otwórz plik Client/Program.cs. Dodaj następujący kod **przy użyciu** instrukcji:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Dodaj statyczną **HttpClient** wystąpienie:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, listę produktów, według Identyfikatora i listę produktów według kategorii.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Każda z tych metod jest zgodna z ten sam wzorcem:

1. Wywołaj **HttpClient.GetAsync** wysłanie żądania GET do odpowiedniego identyfikatora URI.
2. Wywołaj **HttpResponseMessage.EnsureSuccessStatusCode**. Ta metoda zgłasza wyjątek, jeśli kod błędu stanu odpowiedzi HTTP.
3. Wywołaj **ReadAsAsync&lt;T&gt;**  deserializować typu CLR na podstawie odpowiedzi HTTP. Ta metoda jest metodą rozszerzenia, zdefiniowane w **System.Net.Http.HttpContentExtensions**.

**GetAsync** i **ReadAsAsync** metody są asynchroniczne. Zwracają **zadań** obiekty reprezentujące operację asynchroniczną. Wprowadzenie **wynik** właściwość blokuje wątek, aż do zakończenia operacji.

Aby uzyskać więcej informacji na temat za pomocą elementu HttpClient, w tym sposobu wykonywania wywołań, nieblokującą, zobacz [wywołania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Przed wywołaniem metody te, należy ustawić właściwość BaseAddress na wystąpienie klasy HttpClient "`http://localhost:8080`". Na przykład:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

To powinno danych wyjściowych poniżej. (Pamiętaj, aby najpierw uruchom aplikację własnego hosta).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
