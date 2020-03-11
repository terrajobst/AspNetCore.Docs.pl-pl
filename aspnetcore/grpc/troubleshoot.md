---
title: Rozwiązywanie problemów z gRPC na platformie .NET Core
author: jamesnk
description: Rozwiązywanie problemów dotyczących błędów podczas korzystania z gRPC na platformie .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 10/16/2019
uid: grpc/troubleshoot
ms.openlocfilehash: c501cda14f3bac9297695ece59cbc4634e4b7895
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664130"
---
# <a name="troubleshoot-grpc-on-net-core"></a>Rozwiązywanie problemów z gRPC na platformie .NET Core

Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)

W tym dokumencie omówiono często występujące problemy podczas tworzenia aplikacji gRPC na platformie .NET.

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>Niezgodność między konfiguracją protokołu SSL klienta i usługi TLS

Szablon gRPC i przykłady używają usługi [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) do domyślnego zabezpieczania usług gRPC. Klienci gRPC muszą używać bezpiecznego połączenia w celu pomyślnego wywołania zabezpieczonych usług gRPC.

Możesz sprawdzić, ASP.NET Core usługa gRPC korzysta z protokołu TLS w dziennikach, które zostały zapisane podczas uruchamiania aplikacji. Usługa nasłuchuje na punkcie końcowym HTTPS:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

Klient .NET Core musi używać `https` w adresie serwera, aby nawiązywać połączenia z bezpiecznym połączeniem:

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

Wszystkie implementacje klienta gRPC obsługują protokół TLS. gRPC klienci z innych języków zwykle wymagają kanału skonfigurowanego przy użyciu `SslCredentials`. `SslCredentials` określa certyfikat, który będzie używany przez klienta i musi być używany zamiast niezabezpieczonych poświadczeń. Przykłady konfigurowania różnych implementacji klienta gRPC do korzystania z protokołu TLS można znaleźć w temacie [GRPC Authentication (uwierzytelnianie](https://www.grpc.io/docs/guides/auth/)).

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a>Wywołaj usługę gRPC z niezaufanym/nieprawidłowym certyfikatem

Klient .NET gRPC wymaga, aby usługa miała zaufany certyfikat. Podczas wywoływania usługi gRPC bez zaufanego certyfikatu zwracany jest następujący komunikat o błędzie:

> Nieobsługiwany wyjątek. System .NET. http. HttpRequestexception: nie można nawiązać połączenia SSL, zobacz wyjątek wewnętrzny.
> ---> System. Security. Authentication. AuthenticationException: certyfikat zdalny jest nieprawidłowy zgodnie z procedurą walidacji.

Ten błąd może pojawić się, jeśli testujesz aplikację lokalnie, a ASP.NET Core certyfikat programistyczny HTTPS nie jest zaufany. Aby uzyskać instrukcje dotyczące rozwiązania tego problemu, zobacz temat [ASP.NET Core ufanie certyfikatowi Deweloperskiemu protokołu HTTPS w systemie Windows i macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).

Jeśli wywołujesz usługę gRPC na innym komputerze i nie można ufać certyfikatowi, można skonfigurować klienta gRPC do ignorowania nieprawidłowego certyfikatu. Poniższy kod używa metody [HttpClientHandler. ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) , aby zezwalać na wywołania bez zaufanego certyfikatu:

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = 
    HttpClientHandler.DangerousAcceptAnyServerCertificateValidator;
var httpClient = new HttpClient(httpClientHandler);

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpClient = httpClient });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> Niezaufanych certyfikatów należy używać tylko podczas opracowywania aplikacji. Aplikacje produkcyjne powinny zawsze używać prawidłowych certyfikatów.

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Wywoływanie niezabezpieczonych usług gRPC z klientem .NET Core

Dodatkowa konfiguracja jest wymagana do wywołania niezabezpieczonych usług gRPC za pomocą klienta .NET Core. Klient gRPC musi ustawić przełącznik `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`, aby `true` i używać `http` w adresie serwera:

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>Nie można uruchomić aplikacji ASP.NET Core gRPC na macOS

Kestrel nie obsługuje protokołu HTTP/2 z protokołem TLS w macOS i starszych wersjach systemu Windows, takich jak Windows 7. ASP.NET Core szablon i przykłady gRPC domyślnie korzystają z protokołu TLS. Podczas próby uruchomienia serwera gRPC zobaczysz następujący komunikat o błędzie:

> Nie można powiązać https://localhost:5001 w interfejsie sprzężenia zwrotnego IPv4: "protokół HTTP/2 za pośrednictwem protokołu TLS nie jest obsługiwany w macOS ze względu na brak pomocy technicznej CLIENTHELLO ALPN".

Aby obejść ten problem, należy skonfigurować Kestrel i klienta gRPC do używania protokołu HTTP/2 *bez* szyfrowania TLS. Należy to zrobić tylko podczas projektowania. Użycie protokołu TLS spowoduje, że komunikaty gRPC są wysyłane bez szyfrowania.

Kestrel musi skonfigurować punkt końcowy HTTP/2 bez protokołu TLS w *program.cs*:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = 
                    HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

Gdy punkt końcowy protokołu HTTP/2 jest skonfigurowany bez protokołu TLS, [ListenOptions. protokoły](xref:fundamentals/servers/kestrel#listenoptionsprotocols) punktu końcowego muszą mieć wartość `HttpProtocols.Http2`. nie można użyć `HttpProtocols.Http1AndHttp2`, ponieważ protokół TLS jest wymagany do negocjowania protokołu HTTP/2. Bez protokołu TLS wszystkie połączenia z punktem końcowym domyślne do protokołu HTTP/1.1 i wywołania gRPC kończą się niepowodzeniem.

Klienta gRPC należy również skonfigurować tak, aby nie korzystał z protokołu TLS. Aby uzyskać więcej informacji, zobacz [wywoływanie niezabezpieczonych usług gRPC przy użyciu programu .NET Core Client](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> Protokołu HTTP/2 bez protokołu TLS należy używać tylko podczas opracowywania aplikacji. Aplikacje produkcyjne powinny zawsze korzystać z zabezpieczeń transportu. Aby uzyskać więcej informacji, zobacz [zagadnienia dotyczące zabezpieczeń w programie gRPC ASP.NET Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>elementy C# zawartości gRPC nie są generowane w kodzie z plików. proto

gRPC generowanie kodu dla konkretnych klientów i klas podstawowych usług wymaga przywoływania plików protobuf i narzędzi z projektu. Należy uwzględnić:

* pliki *. proto* , które mają być używane w grupie elementów `<Protobuf>`. [Zaimportowane pliki *proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) muszą odwoływać się do projektu.
* Odwołanie do pakietu dla pakietu narzędzi gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Aby uzyskać więcej informacji na temat C# generowania zasobów gRPC, zobacz <xref:grpc/basics>.

Domyślnie odwołanie `<Protobuf>` generuje konkretny klient i klasę bazową usługi. Atrybut `GrpcServices` elementu referencyjnego może służyć do ograniczania C# generacji zasobów. Prawidłowe opcje `GrpcServices` są następujące:

* `Both` (wartość domyślna, gdy nie istnieje)
* `Server`
* `Client`
* `None`

ASP.NET Core usługi gRPC Web App hosting Services wymagają tylko wygenerowania klasy bazowej usługi:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

Aplikacja kliencka gRPC wykonująca wywołania gRPC potrzebuje tylko konkretnego wygenerowanego klienta:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a>Projekty WPF nie mogą generować zasobów C# gRPC z plików. proto

Projekty WPF mają [znany problem](https://github.com/dotnet/wpf/issues/810) , który uniemożliwia poprawne działanie generowania kodu gRPC. Wszystkie typy gRPC wygenerowane w projekcie WPF przez odwoływanie się do plików `Grpc.Tools` i *. proto* spowodują błędy kompilacji, gdy są używane:

> błąd CS0246: nie można znaleźć nazwy typu lub przestrzeni nazw "MyGrpcServices" (czy nie brakuje dyrektywy using lub odwołania do zestawu?)

Ten problem można obejść, wykonując następujące:

1. Utwórz nowy projekt biblioteki klas .NET Core.
2. W nowym projekcie Dodaj odwołania, aby włączyć [ C# generowanie kodu z plików *\*. proto* ](xref:grpc/basics#generated-c-assets):
    * Dodaj odwołanie do pakietu do pakietu [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) .
    * Dodaj pliki *\*. proto* do grupy `<Protobuf>` elementów.
3. W aplikacji WPF Dodaj odwołanie do nowego projektu.

Aplikacja WPF może używać typów wygenerowanych przez gRPC z nowego projektu biblioteki klas.

[!INCLUDE[](~/includes/gRPCazure.md)]
