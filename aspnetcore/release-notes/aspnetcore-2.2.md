---
title: What's new in ASP.NET Core 2.2
author: tdykstra
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.2.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/03/2018
uid: aspnetcore-2.2
ms.openlocfilehash: 7429cb725a630c080690d81ec75d291bf48be2fc
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862830"
---
# <a name="whats-new-in-aspnet-core-22"></a>What's new in ASP.NET Core 2.2

W tym artykule przedstawiono najbardziej znaczących zmian w programie ASP.NET Core 2.2, wraz z łączami do odpowiedniej dokumentacji.

## <a name="open-api-analyzers--conventions"></a>Analizatory otwartych interfejsów API i konwencje

Otwarty interfejs API (znany także jako struktury Swagger) to specyfikacja niezależny od języka opisu interfejsów API REST. Ekosystem interfejsu Open API zawiera narzędzia, które pozwalają na odnajdywanie, testowania i produkcji kod klienta przy użyciu specyfikacji. Obsługa generowania i wizualizowanie dokumentów otwartych interfejsów API na platformie ASP.NET Core MVC jest oferowana w ramach społeczności, takich jak opartego na projektach [NSwag](https://github.com/RSuter/NSwag), i [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore). ASP.NET Core 2.2 zapewnia ulepszone narzędzia i środowisko uruchomieniowe środowisk do tworzenia dokumentów interfejsu Open API.

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:tutorials/web-api-help-pages-using-swagger>
* [Platforma ASP.NET Core 2.2.0-preview1: Otwórz interfejsów API analizatorów & konwencje](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/).

## <a name="endpoint-routing"></a>Punkt końcowy routingu

Platforma ASP.NET Core 2.2 używa nowego *routingu punkt końcowy* system ulepszone wysyłania żądań. Zmiany obejmują nowy link członków generowania interfejsu API.

Aby uzyskać więcej informacji, zobacz [różnice między routing oparty na IRouter i do endpoint](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing).

## <a name="health-checks"></a>Kontrole kondycji

Nowe kondycji sprawdza, czy usługa sprawia, że łatwiej używać platformy ASP.NET Core w środowiskach, które wymagają kontroli kondycji, takimi jak Kubernetes. Kontrole kondycji zawiera oprogramowanie pośredniczące i zbiór bibliotek, które definiują `IHealthCheck` pozyskiwania i usługi.

Kontrole kondycji są używane przez koordynatora kontenerów lub szybko określić, jeśli system jest zazwyczaj odpowiada na żądania usługi równoważenia obciążenia. Orkiestrator kontenerów może odpowiadać na kondycji niepowodzenie sprawdzenia przez zatrzymywanie stopniowego wdrażania lub ponownego uruchamiania kontenera. Moduł równoważenia obciążenia może odpowiedzieć sprawdzenie kondycji routing ruchu od niepowodzeniem wystąpienie usługi.

Kontrole kondycji są udostępniane przez aplikację jako punktu końcowego HTTP używane przez systemy monitorowania. Kontrole kondycji można skonfigurować dla różnych scenariusze monitorowania w czasie rzeczywistym oraz monitorowanie systemów.

Aby uzyskać więcej informacji, zobacz [kontroli kondycji w programie ASP.NET Core](xref:host-and-deploy/health-checks).

## <a name="http2-in-kestrel"></a>Protokołu HTTP/2 w Kestrel

Platforma ASP.NET Core 2.2 dodaje obsługę protokołu HTTP/2. 

Protokołu HTTP/2 jest główną korektą protokołu HTTP. Niektóre istotne funkcje protokołu HTTP/2 to obsługa kompresja nagłówków i w pełni multiplexed strumieni za pośrednictwem jednego połączenia. Gdy protokołu HTTP/2 zachowuje semantykę przez HTTP (nagłówków HTTP, metod itd.) jest istotną zmianę z HTTP/1.x, w jaki sposób te dane są obramowane i przesyłane przez sieć.

W wyniku tej zmiany w ramek serwery i klienci muszą negocjowania używana wersja protokołu. Negocjowania protokołu warstwy aplikacji (ALPN) to rozszerzenie protokołu TLS, które umożliwia serwera i klienta negocjowania protokołu wersji używany w ramach ich uzgadniania TLS. Mimo że możliwe jest zapewnienie uprzednia między serwerem a klientem przy użyciu protokołu, wszystkie główne przeglądarki obsługują ALPN jako jedynym sposobem, aby ustanowić połączenie HTTP/2.

Aby uzyskać więcej informacji, zobacz [Obsługa protokołu HTTP/2](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## <a name="kestrel-configuration"></a>Konfiguracja kestrel

We wcześniejszych wersjach programu ASP.NET Core, są skonfigurowane opcje Kestrel przez wywołanie metody `UseKestrel`. W 2.2, Kestrel opcje są konfigurowane przez wywołanie metody `ConfigureKestrel` w Konstruktorze hosta. Ta zmiana rozwiązuje problem z kolejność `IServer` rejestracji dla wewnątrzprocesowego hostingu. Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Eliminowanie konfliktów Iisurl](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [Konfigurowanie opcji serwera Kestrel z ConfigureKestrel](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>Hostowanie wewnątrzprocesowe usług IIS

We wcześniejszych wersjach programu ASP.NET Core usługi IIS służy jako zwrotny serwer proxy. W 2.2, modułu ASP.NET Core można uruchomić oprogramowania CoreCLR i hostowanie aplikacji w ramach proces roboczy usług IIS (*w3wp.exe*). Hosting w trakcie zapewnia istotnie poprawiającą wydajność i zyski diagnostycznych podczas korzystania z użyciem usług IIS.

Aby uzyskać więcej informacji, zobacz [IIS wewnątrzprocesowego hostingu](xref:fundamentals/servers/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## <a name="signalr-java-client"></a>Klienta SignalR Java

Platforma ASP.NET Core 2.2 wprowadza klienta Java dla elementu SignalR. Ten klient obsługuje łączenie z serwerem SignalR platformy ASP.NET Core z kodu języka Java, w tym aplikacje dla systemu Android.

Aby uzyskać więcej informacji, zobacz [klienta platformy ASP.NET Core SignalR Java](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## <a name="cors-improvements"></a>Ulepszenia mechanizmu CORS

We wcześniejszych wersjach programu ASP.NET Core, oprogramowanie pośredniczące CORS umożliwia `Accept`, `Accept-Language`, `Content-Language`, i `Origin` nagłówki do wysłania niezależnie od wartości skonfigurowanych w `CorsPolicy.Headers`. W 2.2, dopasowanie zasad oprogramowanie pośredniczące CORS jest możliwe tylko wtedy, gdy nagłówki są wysyłane `Access-Control-Request-Headers` dokładnie pasują do nagłówków w `WithHeaders`.

Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące CORS](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## <a name="response-compression"></a>Kompresja odpowiedzi

Platforma ASP.NET Core 2.2 można kompresować odpowiedzi z [format kompresji Brotli](https://tools.ietf.org/html/rfc7932).

Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące kompresji odpowiedzi obsługuje kompresję Brotli](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## <a name="project-templates"></a>Szablony projektów

Szablony projektów internetowych platformy ASP.NET Core zostały zaktualizowane do [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) i [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4). Nowy wygląd jest wizualnie przyspiesza i ułatwia zobacz ważne struktury aplikacji.

![Strona główna lub indeks](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>Sprawdzanie poprawności wydajności

System sprawdzania poprawności MVC jest przeznaczony do być rozszerzalne i elastyczne, dzięki czemu można określić na podstawie danego żądania, które moduły mają zastosowanie do danego modelu. Jest to idealne narzędzie do tworzenia dostawców złożoną walidację. Jednak w najbardziej typowych zamierzone, aplikacja używa tylko wbudowanych modułów weryfikacji i nie wymagają ta dodatkowa elastyczność. Wbudowane moduły weryfikacji obejmują takie jak DataAnnotations [wymagane] i [StringLength] i `IValidatableObject`.

W programie ASP.NET Core 2.2 MVC można zwarcie sprawdzania poprawności, gdy ustali, że wykres podanego modelu nie wymaga weryfikacji. Pomijanie wyniki sprawdzania poprawności w znaczne ulepszenia, podczas sprawdzania poprawności modeli, które nie może lub nie masz żadnych modułów weryfikacji. Obejmuje to obiekty, takie jak kolekcje elementów podstawowych (takie jak `byte[]`, `string[]`, `Dictionary<string, string>`), lub wykresów złożonych obiektów bez wiele modułów weryfikacji.

## <a name="http-client-performance"></a>Wydajność klienta HTTP

W ASP.NET Core 2.2, wydajność `SocketsHttpHandler` zostało ulepszone dzięki zmniejszeniu puli połączeń blokowania rywalizacji o zasoby. W przypadku aplikacji, dzięki wielu wychodzące żądania HTTP, takie jak niektóre architektury mikrousług zwiększona przepływność. Pod obciążeniem `HttpClient` nawet o 60% w systemie Linux, a 20% na Windows można zwiększyć przepływność.

Aby uzyskać więcej informacji, zobacz [żądania ściągnięcia, który zgłosił to ulepszenie](https://github.com/dotnet/corefx/pull/32568).

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core 2.2 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.2.0).
