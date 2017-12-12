---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Host samodzielny ASP.NET Web API 1 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Interfejs API sieci Web ASP.NET nie wymaga usług IIS. Interfejs API sieci web można hosta samodzielnego procesu hosta. Ten samouczek pokazuje, jak udostępniać interfejsu API sieci web wewnątrz applic konsoli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a>Host samodzielny ASP.NET Web API 1 (C#)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Interfejs API sieci Web ASP.NET nie wymaga usług IIS. Interfejs API sieci web można hosta samodzielnego procesu hosta. W tym samouczku przedstawiono sposób obsługi interfejsu API sieci web w aplikacji konsoli.
> 
> **Nowe aplikacje powinny używać OWIN do hosta samodzielnego interfejsu API sieci Web.** Zobacz [umożliwia OWIN Web API 2 platformy ASP.NET hosta samodzielnego](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - 1 interfejs API sieci Web
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Utwórz projekt aplikacji konsoli

Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Windows**. Na liście szablony projektów, wybierz **aplikacji konsoli**. Nazwij projekt &quot;SelfHost&quot; i kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Ustaw docelową platformę (Visual Studio 2010)

Jeśli używasz programu Visual Studio 2010, należy zmienić platformę docelową programu .NET Framework 4.0. (Domyślnie elementy docelowe projektu szablonu [.Net Framework Client Profile](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. W **platformy docelowej** listy rozwijanej pozycję zmienić platformę docelową programu .NET Framework 4.0. Po wyświetleniu monitu, aby zastosować zmiany, kliknij przycisk **tak**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Zainstaluj Menedżera pakietów NuGet

Menedżer pakietów NuGet jest najprostszym sposobem, aby dodać zestawy interfejsu API sieci Web do projektu z systemem innym niż ASP.NET.

Aby sprawdzić, czy jest zainstalowany Menedżer pakietów NuGet, kliknij **narzędzia** menu w programie Visual Studio. Jeśli widzisz menu elementu o nazwie **Menedżer pakietów biblioteki**, następnie Menedżer pakietów NuGet.

Aby zainstalować Menedżera pakietów NuGet:

1. Uruchom program Visual Studio.
2. Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.
3. W **rozszerzenia i aktualizacje** okno dialogowe, wybierz opcję **Online**.
4. Jeśli nie widzisz "Menedżer pakietów NuGet", wpisz "Menedżer pakietów nuget", w polu wyszukiwania.
5. Wybierz Menedżera pakietów NuGet, a następnie kliknij przycisk **Pobierz**.
6. Po zakończeniu pobierania, pojawi się monit do zainstalowania.
7. Po zakończeniu instalacji może być monitowany o ponowne uruchomienie programu Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Dodaj pakiet NuGet interfejsu API sieci Web

Po zainstalowaniu Menedżera pakietów NuGet, Dodaj pakiet Self-Host interfejsu API sieci Web do projektu.

1. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**. *Uwaga*: Jeśli użytkownik nie ma tego menu elementu, upewnij się, Menedżer pakietów NuGet, że zainstalowane poprawnie.
2. Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**
3. W **Zarządzaj pakietami NugGet** okno dialogowe, wybierz opcję **Online**.
4. W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Wybierz pakiet ASP.NET Web API Self Host, a następnie kliknij przycisk **zainstalować**.
6. Po zainstalowaniu pakietu, kliknij przycisk **zamknąć** aby zamknąć okno dialogowe.

> [!NOTE]
> Upewnij się zainstalować pakiet o nazwie Microsoft.AspNet.WebApi.SelfHost, nie AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.

Dodaj Klasa publiczna o nazwie `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Dodaj Klasa publiczna o nazwie `ProductsController`. Pochodzi ta klasa z **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka. Ten kontroler definiuje trzy czynności GET:

| Identyfikator URI | Opis |
| --- | --- |
| / api/produktów | Pobranie listy wszystkich produktów. |
| /API/produkty/*id* | Uzyskiwanie produktu według identyfikatora. |
| /API/produkty /? kategorii =*kategorii* | Pobierz listę produktów według kategorii. |

## <a name="host-the-web-api"></a>Interfejs API sieci Web hosta

Otwórz plik Program.cs i dodaj następujące instrukcje using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Dodaj następujący kod do **Program** klasy.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Opcjonalnie) Dodaj Namespace rezerwację adresu URL HTTP

Ta aplikacja nasłuchuje `http://localhost:8080/`. Domyślnie nasłuchuje pod adresem HTTP wymaga uprawnień administratora. Po uruchomieniu tego samouczka, dlatego może ten błąd: "HTTP nie może zarejestrować adresu URL http://+:8080/" istnieją dwa sposoby, aby uniknąć tego błędu:

- Uruchom program Visual Studio z uprawnieniami administratora z podniesionymi uprawnieniami, lub
- Użyj Netsh.exe, aby uzyskać uprawnienia konta do zarezerwowania adresu URL.

Aby użyć Netsh.exe, otwórz wiersz polecenia z uprawnieniami administratora i wprowadź następujące polecenia: następujące polecenie:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

gdzie *machine\username* jest konto użytkownika.

Po zakończeniu hostingu samodzielnego, należy usunąć zastrzeżenie:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Wywołanie interfejsu API sieci Web z aplikacji klienta (C#)

Napisz aplikacji konsoli simple, która wywołuje interfejs API sieci web.

Dodaj nowy projekt aplikacji konsoli do rozwiązania:

- W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz **Dodawanie nowego projektu**.
- Utwórz nową aplikację konsoli o nazwie &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Użyj Menedżera pakietów NuGet można dodać pakietu ASP.NET Web API Core Libraries:

- Wybierz z menu narzędzia **Menedżer pakietów biblioteki**.
- Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**
- W **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **Online**.
- W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Wybierz pakiet Microsoft ASP.NET Web API Client Libraries, a następnie kliknij przycisk **zainstalować**.

Dodaj odwołanie w ClientApp do projektu SelfHost:

- W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.
- Wybierz **Dodaj odwołanie**.
- W **Menedżera odwołań** okna dialogowego, w obszarze **rozwiązania**, wybierz pozycję **projekty**.
- Wybierz projekt SelfHost.
- Kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image6.png)

Otwórz plik Client/Program.cs. Dodaj następujące **przy użyciu** instrukcji:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Dodawanie statycznego **HttpClient** wystąpienie:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, produktu za pomocą Identyfikatora listy i listę produktów według kategorii.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Każda z tych metod następujące tego samego wzorca:

1. Wywołanie **HttpClient.GetAsync** wysłać żądania GET do odpowiedniego identyfikatora URI.
2. Wywołanie **HttpResponseMessage.EnsureSuccessStatusCode**. Ta metoda zgłasza wyjątek, jeśli stan odpowiedzi HTTP jest kod błędu.
3. Wywołanie **ReadAsAsync&lt;T&gt;**  deserializować typu CLR z odpowiedzi HTTP. Ta metoda jest metodą rozszerzenia, zdefiniowane w **System.Net.Http.HttpContentExtensions**.

**GetAsync** i **ReadAsAsync** metody są asynchroniczne. Zwracają **zadań** obiekty reprezentujące operację asynchroniczną. Pobieranie **wynik** właściwości blokuje wątek przed zakończeniem operacji.

Więcej informacji o za pomocą elementu HttpClient, jak wykonać nieblokujące wywołania w tym temacie [wywoływania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Przed wywołaniem metody te, ustaw właściwość BaseAddress na wystąpienie HttpClient "`http://localhost:8080`". Na przykład:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

To wyjścia poniżej. (Pamiętaj, aby uruchomić aplikację SelfHost najpierw).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
