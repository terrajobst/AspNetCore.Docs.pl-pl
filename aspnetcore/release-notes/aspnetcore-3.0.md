---
title: Co nowego w ASP.NET Core 3,0
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 3,0.
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: aspnetcore-3.0
ms.openlocfilehash: 8c53d8a9fa222ca40f26dc713ec3b70ddde76539
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416121"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="4f260-103">Co nowego w ASP.NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="4f260-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="4f260-104">W tym artykule przedstawiono najbardziej znaczące zmiany w ASP.NET Core 3,0 z linkami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## <a name="blazor"></a><span data-ttu-id="4f260-105">Blazor</span><span class="sxs-lookup"><span data-stu-id="4f260-105">Blazor</span></span>

<span data-ttu-id="4f260-106">Blazor to nowa struktura w ASP.NET Core tworzenia interakcyjnego interfejsu użytkownika sieci Web po stronie klienta przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="4f260-106">Blazor is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="4f260-107">Utwórz rozbudowane interaktywne C# interfejsów użytkownika przy użyciu zamiast języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4f260-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="4f260-108">Udostępnianie logiki aplikacji po stronie serwera i klienta zapisaną w środowisku .NET.</span><span class="sxs-lookup"><span data-stu-id="4f260-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="4f260-109">Renderuj interfejs użytkownika jako HTML i CSS, aby obsługiwał szeroką przeglądarkę, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="4f260-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="4f260-110">Scenariusze obsługiwane przez środowisko Blazor Framework:</span><span class="sxs-lookup"><span data-stu-id="4f260-110">Blazor framework supported scenarios:</span></span>

* <span data-ttu-id="4f260-111">Składniki interfejsu użytkownika wielokrotnego użytku (składniki Razor)</span><span class="sxs-lookup"><span data-stu-id="4f260-111">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="4f260-112">Routing po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="4f260-112">Client-side routing</span></span>
* <span data-ttu-id="4f260-113">Układy składników</span><span class="sxs-lookup"><span data-stu-id="4f260-113">Component layouts</span></span>
* <span data-ttu-id="4f260-114">Obsługa iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="4f260-114">Support for dependency injection</span></span>
* <span data-ttu-id="4f260-115">Formularze i walidacja</span><span class="sxs-lookup"><span data-stu-id="4f260-115">Forms and validation</span></span>
* <span data-ttu-id="4f260-116">Kompiluj biblioteki składników z bibliotekami klas Razor</span><span class="sxs-lookup"><span data-stu-id="4f260-116">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="4f260-117">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="4f260-117">JavaScript interop</span></span>

<span data-ttu-id="4f260-118">Aby uzyskać więcej informacji, zobacz <xref:blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="4f260-118">For more information, see <xref:blazor/index>.</span></span>

### <a name="blazor-server"></a><span data-ttu-id="4f260-119">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="4f260-119">Blazor Server</span></span>

<span data-ttu-id="4f260-120">Blazor oddziela logikę renderowania składników od sposobu stosowania aktualizacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4f260-120">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="4f260-121">Serwer Blazor zapewnia obsługę hostingu składników Razor na serwerze w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f260-121">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="4f260-122">Aktualizacje interfejsu użytkownika są obsługiwane przez połączenie sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="4f260-122">UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="4f260-123">Serwer Blazor jest obsługiwany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4f260-123">Blazor Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="blazor-webassembly-preview"></a><span data-ttu-id="4f260-124">Webassembly Blazor (wersja zapoznawcza)</span><span class="sxs-lookup"><span data-stu-id="4f260-124">Blazor WebAssembly (Preview)</span></span>

<span data-ttu-id="4f260-125">Aplikacje Blazor można również uruchamiać bezpośrednio w przeglądarce przy użyciu środowiska uruchomieniowego .NET opartego na zestawie.</span><span class="sxs-lookup"><span data-stu-id="4f260-125">Blazor apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> <span data-ttu-id="4f260-126">Zestaw webBlazor jest w wersji zapoznawczej i *nie* jest obsługiwany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4f260-126">Blazor WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> <span data-ttu-id="4f260-127">Blazor webassembly będzie obsługiwany w przyszłej wersji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f260-127">Blazor WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="4f260-128">Składniki Razor</span><span class="sxs-lookup"><span data-stu-id="4f260-128">Razor components</span></span>

<span data-ttu-id="4f260-129">Aplikacje Blazor są kompilowane ze składników.</span><span class="sxs-lookup"><span data-stu-id="4f260-129">Blazor apps are built from components.</span></span> <span data-ttu-id="4f260-130">Składniki to samodzielne fragmenty interfejsu użytkownika (UI), takie jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="4f260-130">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="4f260-131">Składniki to normalne klasy .NET, które definiują logikę renderowania interfejsu użytkownika i obsługę zdarzeń po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4f260-131">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="4f260-132">Możesz tworzyć rozbudowane interaktywne aplikacje sieci Web bez języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4f260-132">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="4f260-133">Składniki w Blazor są zwykle tworzone przy użyciu składnia Razor, naturalnej mieszanki HTML i C#.</span><span class="sxs-lookup"><span data-stu-id="4f260-133">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="4f260-134">Składniki Razor są podobne do widoków Razor Pages i MVC, w których oba używają Razor.</span><span class="sxs-lookup"><span data-stu-id="4f260-134">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="4f260-135">W przeciwieństwie do stron i widoków, które są oparte na modelu odpowiedzi na żądanie, składniki są używane do obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4f260-135">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="4f260-136">gRPC</span><span class="sxs-lookup"><span data-stu-id="4f260-136">gRPC</span></span>

<span data-ttu-id="4f260-137">[gRPC](https://grpc.io/):</span><span class="sxs-lookup"><span data-stu-id="4f260-137">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="4f260-138">To popularne środowisko RPC o wysokiej wydajności (zdalne wywołanie procedury).</span><span class="sxs-lookup"><span data-stu-id="4f260-138">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="4f260-139">Oferuje ceniona kontrakt do programowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4f260-139">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="4f260-140">Używa nowoczesnych technologii, takich jak:</span><span class="sxs-lookup"><span data-stu-id="4f260-140">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="4f260-141">HTTP/2 do transportu.</span><span class="sxs-lookup"><span data-stu-id="4f260-141">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="4f260-142">Bufory protokołu jako język opisu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4f260-142">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="4f260-143">Binarny format serializacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-143">Binary serialization format.</span></span>
* <span data-ttu-id="4f260-144">Udostępnia funkcje takie jak:</span><span class="sxs-lookup"><span data-stu-id="4f260-144">Provides features such as:</span></span>

  * <span data-ttu-id="4f260-145">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="4f260-145">Authentication</span></span>
  * <span data-ttu-id="4f260-146">Dwukierunkowe przesyłanie strumieniowe i sterowanie przepływem.</span><span class="sxs-lookup"><span data-stu-id="4f260-146">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="4f260-147">Anulowanie i przekroczenie limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="4f260-147">Cancellation and timeouts.</span></span>

<span data-ttu-id="4f260-148">funkcje gRPC w ASP.NET Core 3,0 obejmują:</span><span class="sxs-lookup"><span data-stu-id="4f260-148">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="4f260-149">[GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; platformę ASP.NET Core do hostowania usług GRPC Services.</span><span class="sxs-lookup"><span data-stu-id="4f260-149">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="4f260-150">gRPC na ASP.NET Core integruje się ze standardowymi funkcjami ASP.NET Core, takimi jak rejestrowanie, iniekcja zależności (DI), uwierzytelnianie i autoryzacja.</span><span class="sxs-lookup"><span data-stu-id="4f260-150">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication, and authorization.</span></span>
* <span data-ttu-id="4f260-151">[GRPC .NET. client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; klienta GRPC dla platformy .NET Core, który kompiluje się na znanym `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="4f260-151">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="4f260-152">[GRPC .NET. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; GRPC z integracją klienta z `HttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="4f260-152">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="4f260-153">Aby uzyskać więcej informacji, zobacz <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="4f260-153">For more information, see <xref:grpc/index>.</span></span>

## <a name="signalr"></a><span data-ttu-id="4f260-154">SignalR</span><span class="sxs-lookup"><span data-stu-id="4f260-154">SignalR</span></span>

<span data-ttu-id="4f260-155">Instrukcje dotyczące migracji można znaleźć w temacie [Aktualizowanie kodu sygnalizującego](xref:migration/22-to-30#signalr) .</span><span class="sxs-lookup"><span data-stu-id="4f260-155">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> <span data-ttu-id="4f260-156">Program sygnalizujący teraz używa `System.Text.Json` do serializacji/deserializacji komunikatów JSON.</span><span class="sxs-lookup"><span data-stu-id="4f260-156">SignalR now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="4f260-157">Aby uzyskać instrukcje dotyczące przywracania serializatora opartego na `Newtonsoft.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson) .</span><span class="sxs-lookup"><span data-stu-id="4f260-157">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="4f260-158">W klientach JavaScript i .NET dla sygnalizującego dodano obsługę automatycznego ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4f260-158">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="4f260-159">Domyślnie klient próbuje ponownie nawiązać połączenie, a następnie ponowić próbę po 2, 10 i 30 sekundach, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="4f260-159">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="4f260-160">Jeśli klient pomyślnie nawiąże połączenie, otrzymuje nowy identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="4f260-160">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="4f260-161">Automatyczne ponowne łączenie jest zgodą:</span><span class="sxs-lookup"><span data-stu-id="4f260-161">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="4f260-162">Interwały ponownego połączenia można określić, przekazując tablicę czasów trwania opartych na milisekundach:</span><span class="sxs-lookup"><span data-stu-id="4f260-162">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="4f260-163">Implementację niestandardową można przekazywać w celu zapewnienia pełnej kontroli nad interwałami ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4f260-163">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="4f260-164">Jeśli ponowne połączenie nie powiedzie się po ostatnim interwale ponownego połączenia:</span><span class="sxs-lookup"><span data-stu-id="4f260-164">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="4f260-165">Klient uważa, że połączenie jest w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="4f260-165">The client considers the connection is offline.</span></span>
* <span data-ttu-id="4f260-166">Klient przestanie próbować ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="4f260-166">The client stops trying to reconnect.</span></span>

<span data-ttu-id="4f260-167">Podczas próby ponownego połączenia zaktualizuj interfejs użytkownika aplikacji, aby powiadomić użytkownika o tym, że trwa próba ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4f260-167">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="4f260-168">Aby zapewnić informacje zwrotne interfejsu użytkownika w przypadku przerwania połączenia, interfejs API klienta sygnalizującego został rozszerzony w celu uwzględnienia następujących programów obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="4f260-168">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="4f260-169">`onreconnecting`: umożliwia deweloperom wyłączenie interfejsu użytkownika lub umożliwienie użytkownikom znajomości aplikacji w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="4f260-169">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="4f260-170">`onreconnected`: daje deweloperom możliwość aktualizowania interfejsu użytkownika po jego nawiązaniu.</span><span class="sxs-lookup"><span data-stu-id="4f260-170">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="4f260-171">Poniższy kod używa `onreconnecting` do aktualizowania interfejsu użytkownika podczas próby nawiązania połączenia:</span><span class="sxs-lookup"><span data-stu-id="4f260-171">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="4f260-172">Poniższy kod używa `onreconnected`, aby zaktualizować interfejs użytkownika w połączeniu:</span><span class="sxs-lookup"><span data-stu-id="4f260-172">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="4f260-173">Sygnał 3,0 i nowsze udostępnia zasób niestandardowy do programów obsługi autoryzacji, gdy metoda centrum wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-173">SignalR 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="4f260-174">Zasób jest wystąpieniem `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="4f260-174">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="4f260-175">`HubInvocationContext` obejmuje:</span><span class="sxs-lookup"><span data-stu-id="4f260-175">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="4f260-176">Nazwa wywoływanej metody centrum.</span><span class="sxs-lookup"><span data-stu-id="4f260-176">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="4f260-177">Argumenty metody centrum.</span><span class="sxs-lookup"><span data-stu-id="4f260-177">Arguments to the hub method.</span></span>

<span data-ttu-id="4f260-178">Rozważmy następujący przykład aplikacji pokoju rozmów, która umożliwia logowanie w wielu organizacjach za pośrednictwem Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f260-178">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="4f260-179">Każda osoba mająca konto Microsoft może zalogować się do programu chat, ale tylko członkowie organizacji będącej właścicielem mogą zakazać użytkowników lub wyświetlać historie rozmów użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4f260-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users’ chat histories.</span></span> <span data-ttu-id="4f260-180">Aplikacja może ograniczyć niektóre funkcje do określonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4f260-180">The app could restrict certain functionality from specific users.</span></span>

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="4f260-181">W poprzednim kodzie `DomainRestrictedRequirement` służy jako `IAuthorizationRequirement`niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="4f260-181">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="4f260-182">Ponieważ `HubInvocationContext` parametr zasobu jest przesyłany, wewnętrzna logika może:</span><span class="sxs-lookup"><span data-stu-id="4f260-182">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="4f260-183">Sprawdź kontekst, w którym jest wywoływany centrum.</span><span class="sxs-lookup"><span data-stu-id="4f260-183">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="4f260-184">Podejmowanie decyzji na umożliwienie użytkownikowi wykonywania poszczególnych metod centrów.</span><span class="sxs-lookup"><span data-stu-id="4f260-184">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="4f260-185">Poszczególne metody centrów mogą być dekoracyjne z nazwą zasad, które kod sprawdza w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4f260-185">Individual Hub methods can be decorated with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="4f260-186">Gdy klienci próbują wywołać poszczególne metody centrów, program obsługi `DomainRestrictedRequirement` uruchamia i kontroluje dostęp do metod.</span><span class="sxs-lookup"><span data-stu-id="4f260-186">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="4f260-187">W zależności od sposobu, w jaki `DomainRestrictedRequirement` kontroluje dostęp:</span><span class="sxs-lookup"><span data-stu-id="4f260-187">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="4f260-188">Wszyscy zalogowani użytkownicy mogą wywoływać metodę `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="4f260-188">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="4f260-189">Historie użytkowników mogą wyświetlać tylko użytkownicy, którzy zalogowali się przy użyciu adresu e-mail `@jabbr.net`.</span><span class="sxs-lookup"><span data-stu-id="4f260-189">Only users who have logged in with a `@jabbr.net` email address can view users’ histories.</span></span>
* <span data-ttu-id="4f260-190">Tylko `bob42@jabbr.net` mogą zakazać użytkowników z pokoju rozmów.</span><span class="sxs-lookup"><span data-stu-id="4f260-190">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

<span data-ttu-id="4f260-191">Tworzenie zasad `DomainRestricted` może wymagać:</span><span class="sxs-lookup"><span data-stu-id="4f260-191">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="4f260-192">W *Startup.cs*Dodaj nowe zasady.</span><span class="sxs-lookup"><span data-stu-id="4f260-192">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="4f260-193">Podaj niestandardowe wymagania `DomainRestrictedRequirement` jako parametr.</span><span class="sxs-lookup"><span data-stu-id="4f260-193">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="4f260-194">Rejestrowanie `DomainRestricted` przy użyciu oprogramowania pośredniczącego autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-194">Registering `DomainRestricted` with the authorization middleware.</span></span>

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

<span data-ttu-id="4f260-195">Centra sygnałów używają [routingu punktu końcowego](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="4f260-195">SignalR hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="4f260-196">Połączenie centrum sygnałów zostało wcześniej wykonane jawnie:</span><span class="sxs-lookup"><span data-stu-id="4f260-196">SignalR hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="4f260-197">W poprzedniej wersji deweloperzy musieli połączyć kontrolery, strony Razor i centra w różnych miejscach.</span><span class="sxs-lookup"><span data-stu-id="4f260-197">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of places.</span></span> <span data-ttu-id="4f260-198">Jawne połączenie daje w wyniku serię niemal identycznych segmentów routingu:</span><span class="sxs-lookup"><span data-stu-id="4f260-198">Explicit connection results in a series of nearly-identical routing segments:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

<span data-ttu-id="4f260-199">Centra sygnalizujące 3,0 mogą być kierowane za pośrednictwem routingu punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4f260-199">SignalR 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="4f260-200">Za pomocą routingu punktów końcowych, zazwyczaj wszystkie Routing można skonfigurować w `UseRouting`:</span><span class="sxs-lookup"><span data-stu-id="4f260-200">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="4f260-201">Dodano ASP.NET Core sygnalizujący 3,0:</span><span class="sxs-lookup"><span data-stu-id="4f260-201">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="4f260-202">Przesyłanie strumieniowe klient-serwer.</span><span class="sxs-lookup"><span data-stu-id="4f260-202">Client-to-server streaming.</span></span> <span data-ttu-id="4f260-203">W przypadku przesyłania strumieniowego klient-serwer metody po stronie serwera mogą przyjmować wystąpienia `IAsyncEnumerable<T>` lub `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="4f260-203">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="4f260-204">W poniższym C# przykładzie metoda `UploadStream` w centrum otrzyma strumień ciągów od klienta:</span><span class="sxs-lookup"><span data-stu-id="4f260-204">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="4f260-205">Aplikacje klienckie platformy .NET mogą przekazać wystąpienie `IAsyncEnumerable<T>` lub `ChannelReader<T>` jako argument `stream` z metody `UploadStream` Hub powyżej.</span><span class="sxs-lookup"><span data-stu-id="4f260-205">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="4f260-206">Po zakończeniu `for` pętli, gdy funkcja lokalna zostanie zakończona, zostanie wysłany strumień:</span><span class="sxs-lookup"><span data-stu-id="4f260-206">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="4f260-207">Aplikacje klienckie języka JavaScript używają `Subject` sygnalizującego (lub [tematu RxJS](https://rxjs.dev/api/index/class/Subject)) dla argumentu `stream` z powyższej metody `UploadStream` Hub.</span><span class="sxs-lookup"><span data-stu-id="4f260-207">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="4f260-208">Kod JavaScript może używać metody `subject.next` do obsługi ciągów, ponieważ są one przechwytywane i gotowe do wysłania na serwer.</span><span class="sxs-lookup"><span data-stu-id="4f260-208">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="4f260-209">Korzystając z kodu, takiego jak dwa poprzednie fragmenty, można utworzyć środowiska przesyłania strumieniowego w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4f260-209">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

## <a name="new-json-serialization"></a><span data-ttu-id="4f260-210">Nowa Serializacja JSON</span><span class="sxs-lookup"><span data-stu-id="4f260-210">New JSON serialization</span></span>

<span data-ttu-id="4f260-211">ASP.NET Core 3,0 obecnie używa <xref:System.Text.Json> domyślnie dla serializacji JSON:</span><span class="sxs-lookup"><span data-stu-id="4f260-211">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="4f260-212">Odczytuje i zapisuje dane JSON asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="4f260-212">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="4f260-213">Jest zoptymalizowany pod kątem tekstu w formacie UTF-8.</span><span class="sxs-lookup"><span data-stu-id="4f260-213">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="4f260-214">Zwykle wyższa wydajność niż `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="4f260-214">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="4f260-215">Aby dodać Json.NET do ASP.NET Core 3,0, zobacz [Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="4f260-215">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="4f260-216">Nowe dyrektywy Razor</span><span class="sxs-lookup"><span data-stu-id="4f260-216">New Razor directives</span></span>

<span data-ttu-id="4f260-217">Poniższa lista zawiera nowe dyrektywy Razor:</span><span class="sxs-lookup"><span data-stu-id="4f260-217">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="4f260-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; dyrektywa `@attribute` stosuje dany atrybut do klasy wygenerowanej strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="4f260-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="4f260-219">Na przykład `@attribute [Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="4f260-219">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="4f260-220">[@implements](xref:mvc/views/razor#implements) &ndash; dyrektywa `@implements` implementuje interfejs dla wygenerowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="4f260-220">[@implements](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="4f260-221">Na przykład `@implements IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="4f260-221">For example, `@implements IDisposable`.</span></span>

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a><span data-ttu-id="4f260-222">Usługi identityserver4 obsługuje uwierzytelnianie i autoryzację dla interfejsów API sieci Web i aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="4f260-222">IdentityServer4 supports authentication and authorization for web APIs and SPAs</span></span>

<span data-ttu-id="4f260-223">ASP.NET Core 3,0 oferuje uwierzytelnianie w aplikacjach jednostronicowych (aplikacji jednostronicowych) przy użyciu obsługi autoryzacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f260-223">ASP.NET Core 3.0 offers authentication in Single Page Apps (SPAs) using the support for web API authorization.</span></span> <span data-ttu-id="4f260-224">ASP.NET Core tożsamość uwierzytelniania i przechowywania użytkowników jest połączona z [usługi identityserver4](https://identityserver.io/) w celu zaimplementowania połączenia Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="4f260-224">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer4](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="4f260-225">Usługi identityserver4 to struktura OpenID Connect Connect i OAuth 2,0 dla ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4f260-225">IdentityServer4 is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core 3.0.</span></span> <span data-ttu-id="4f260-226">Zapewnia następujące funkcje zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="4f260-226">It enables the following security features:</span></span>

* <span data-ttu-id="4f260-227">Uwierzytelnianie jako usługa (AaaS)</span><span class="sxs-lookup"><span data-stu-id="4f260-227">Authentication as a Service (AaaS)</span></span>
* <span data-ttu-id="4f260-228">Logowanie jednokrotne (SSO) dla wielu typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="4f260-228">Single sign-on/off (SSO) over multiple application types</span></span>
* <span data-ttu-id="4f260-229">Kontrola dostępu do interfejsów API</span><span class="sxs-lookup"><span data-stu-id="4f260-229">Access control for APIs</span></span>
* <span data-ttu-id="4f260-230">Brama federacyjna</span><span class="sxs-lookup"><span data-stu-id="4f260-230">Federation Gateway</span></span>

<span data-ttu-id="4f260-231">Aby uzyskać więcej informacji, zapoznaj [się z dokumentacją usługi identityserver4](http://docs.identityserver.io/en/latest/index.html) lub [uwierzytelnianiem i autoryzacją aplikacji jednostronicowych](xref:security/authentication/identity/spa).</span><span class="sxs-lookup"><span data-stu-id="4f260-231">For more information, see [the IdentityServer4 documentation](http://docs.identityserver.io/en/latest/index.html) or [Authentication and authorization for SPAs](xref:security/authentication/identity/spa).</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="4f260-232">Certyfikat i uwierzytelnianie Kerberos</span><span class="sxs-lookup"><span data-stu-id="4f260-232">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="4f260-233">Wymagane jest uwierzytelnianie certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="4f260-233">Certificate authentication requires:</span></span>

* <span data-ttu-id="4f260-234">Konfigurowanie serwera do akceptowania certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="4f260-234">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="4f260-235">Dodawanie oprogramowania pośredniczącego uwierzytelniania w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4f260-235">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="4f260-236">Dodawanie usługi uwierzytelniania certyfikatów w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4f260-236">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="4f260-237">Opcje uwierzytelniania certyfikatów obejmują:</span><span class="sxs-lookup"><span data-stu-id="4f260-237">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="4f260-238">Zaakceptuj certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="4f260-238">Accept self-signed certificates.</span></span>
* <span data-ttu-id="4f260-239">Sprawdź, czy certyfikaty są odwoływane.</span><span class="sxs-lookup"><span data-stu-id="4f260-239">Check for certificate revocation.</span></span>
* <span data-ttu-id="4f260-240">Sprawdź, czy certyfikat proffered ma odpowiednie flagi użycia.</span><span class="sxs-lookup"><span data-stu-id="4f260-240">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="4f260-241">Domyślny podmiot zabezpieczeń użytkownika jest konstruowany ze wszystkich właściwości certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="4f260-241">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="4f260-242">Nazwa główna użytkownika zawiera zdarzenie, które umożliwia uzupełnianie lub zastępowanie podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="4f260-242">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="4f260-243">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/certauth>.</span><span class="sxs-lookup"><span data-stu-id="4f260-243">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="4f260-244">[Uwierzytelnianie systemu Windows](/windows-server/security/windows-authentication/windows-authentication-overview) zostało rozszerzone na system Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="4f260-244">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="4f260-245">W poprzednich wersjach uwierzytelnianie systemu Windows było ograniczone do [usług IIS](xref:host-and-deploy/iis/index) i [HttpSys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="4f260-245">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="4f260-246">W ASP.NET Core 3,0 [Kestrel](xref:fundamentals/servers/kestrel) ma możliwość używania [protokołów](/windows-server/security/kerberos/kerberos-authentication-overview)Negotiate, Kerberos i [NTLM w systemach Windows](/windows-server/security/kerberos/ntlm-overview), Linux i macOS dla hostów przyłączonych do domeny systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="4f260-246">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="4f260-247">Kestrel obsługa tych schematów uwierzytelniania jest udostępniana przez pakiet [NuGet Microsoft. AspNetCore. Authentication. Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) .</span><span class="sxs-lookup"><span data-stu-id="4f260-247">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="4f260-248">Podobnie jak w przypadku innych usług uwierzytelniania, skonfiguruj aplikację uwierzytelniania Wide, a następnie skonfiguruj usługę:</span><span class="sxs-lookup"><span data-stu-id="4f260-248">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="4f260-249">Wymagania dotyczące hosta:</span><span class="sxs-lookup"><span data-stu-id="4f260-249">Host requirements:</span></span>

* <span data-ttu-id="4f260-250">Hosty z systemem Windows muszą mieć dodane [główne nazwy usług](/windows/win32/ad/service-principal-names) (SPN) do konta użytkownika obsługującego aplikację.</span><span class="sxs-lookup"><span data-stu-id="4f260-250">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="4f260-251">Maszyny z systemami Linux i macOS muszą być przyłączone do domeny.</span><span class="sxs-lookup"><span data-stu-id="4f260-251">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="4f260-252">Dla procesu sieci Web należy utworzyć nazwy SPN.</span><span class="sxs-lookup"><span data-stu-id="4f260-252">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="4f260-253">Na komputerze hosta muszą być generowane i skonfigurowane [pliki plik KEYTAB](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) .</span><span class="sxs-lookup"><span data-stu-id="4f260-253">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="4f260-254">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="4f260-254">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="4f260-255">Zmiany szablonu</span><span class="sxs-lookup"><span data-stu-id="4f260-255">Template changes</span></span>

<span data-ttu-id="4f260-256">Szablony interfejsu użytkownika sieci Web (Razor Pages, MVC z kontrolerem i widokami) mają następujące usunięte:</span><span class="sxs-lookup"><span data-stu-id="4f260-256">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="4f260-257">Interfejs użytkownika zgody na plik cookie nie jest już uwzględniony.</span><span class="sxs-lookup"><span data-stu-id="4f260-257">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="4f260-258">Aby włączyć funkcję wyrażania zgody plików cookie w aplikacji wygenerowanej przez szablon ASP.NET Core 3,0, zobacz <xref:security/gdpr>.</span><span class="sxs-lookup"><span data-stu-id="4f260-258">To enable the cookie consent feature in an ASP.NET Core 3.0 template-generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="4f260-259">Skrypty i powiązane zasoby statyczne są teraz przywoływane jako pliki lokalne zamiast używać sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f260-259">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="4f260-260">Aby uzyskać więcej informacji, zobacz [skrypty i powiązane zasoby statyczne są teraz odwołujące się do plików lokalnych zamiast używania sieci CDN w oparciu o bieżące środowisko (ASPNET/AspNetCore. Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span><span class="sxs-lookup"><span data-stu-id="4f260-260">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="4f260-261">Szablon kątowy został zaktualizowany do użycia kątowy 8.</span><span class="sxs-lookup"><span data-stu-id="4f260-261">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="4f260-262">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="4f260-262">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="4f260-263">Nowa opcja szablonu w programie Visual Studio zapewnia obsługę szablonów dla stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="4f260-263">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="4f260-264">Podczas tworzenia RCL z szablonu w powłoce poleceń, należy przekazać opcję `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`).</span><span class="sxs-lookup"><span data-stu-id="4f260-264">When creating an RCL from the template in a command shell, pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="4f260-265">Host ogólny</span><span class="sxs-lookup"><span data-stu-id="4f260-265">Generic Host</span></span>

<span data-ttu-id="4f260-266">Szablony ASP.NET Core 3,0 używają <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="4f260-266">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="4f260-267">Poprzednie wersje używane <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4f260-267">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="4f260-268">Korzystanie z hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) zapewnia lepszą integrację ASP.NET Core aplikacji z innymi scenariuszami serwera, które nie są specyficzne dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f260-268">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that aren't web-specific.</span></span> <span data-ttu-id="4f260-269">Aby uzyskać więcej informacji, zobacz [HostBuilder zastępuje WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4f260-269">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="4f260-270">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="4f260-270">Host configuration</span></span>

<span data-ttu-id="4f260-271">Przed wydaniem ASP.NET Core 3,0 zmienne środowiskowe z prefiksem `ASPNETCORE_` zostały załadowane na potrzeby konfiguracji hosta hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f260-271">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="4f260-272">W 3,0 `AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych poprzedzonych `DOTNET_` na potrzeby konfiguracji hosta z `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4f260-272">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-constructor-injection"></a><span data-ttu-id="4f260-273">Zmiany w iniekcji konstruktorów uruchomieniowych</span><span class="sxs-lookup"><span data-stu-id="4f260-273">Changes to Startup constructor injection</span></span>

<span data-ttu-id="4f260-274">Host generyczny obsługuje tylko następujące typy dla iniekcji konstruktora `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4f260-274">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="4f260-275">Wszystkie usługi można nadal dodawać bezpośrednio jako argumenty do metody `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4f260-275">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="4f260-276">Aby uzyskać więcej informacji, zobacz [host ogólny ogranicza iniekcje konstruktorów uruchamiania (ASPNET/anonse #353)](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="4f260-276">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="4f260-277">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4f260-277">Kestrel</span></span>

* <span data-ttu-id="4f260-278">Konfiguracja Kestrel została zaktualizowana na potrzeby migracji do hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="4f260-278">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="4f260-279">W 3,0 Kestrel jest skonfigurowany w konstruktorze hosta sieci Web dostarczonym przez `ConfigureWebHostDefaults`.</span><span class="sxs-lookup"><span data-stu-id="4f260-279">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="4f260-280">Adaptery połączeń zostały usunięte z usługi Kestrel i zastąpione przez oprogramowanie pośredniczące połączenia, które jest podobne do oprogramowania pośredniczącego HTTP w potoku ASP.NET Core ale dla połączeń niższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="4f260-280">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="4f260-281">Warstwa transportu Kestrel została udostępniona jako interfejs publiczny w `Connections.Abstractions`.</span><span class="sxs-lookup"><span data-stu-id="4f260-281">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="4f260-282">Niejednoznaczność między nagłówkami a przyczepami została rozwiązana przez przeniesienie końcowych nagłówków do nowej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="4f260-282">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="4f260-283">Synchroniczne interfejsy API we/wy, takie jak `HttpRequest.Body.Read`, są typowym źródłem zablokowania wątków prowadzącego do awarii aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-283">Synchronous IO APIs, such as `HttpRequest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="4f260-284">W 3,0 `AllowSynchronousIO` jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="4f260-284">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="4f260-285">Aby uzyskać więcej informacji, zobacz <xref:migration/22-to-30#kestrel>.</span><span class="sxs-lookup"><span data-stu-id="4f260-285">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="4f260-286">Protokół HTTP/2 włączony domyślnie</span><span class="sxs-lookup"><span data-stu-id="4f260-286">HTTP/2 enabled by default</span></span>

<span data-ttu-id="4f260-287">Protokół HTTP/2 jest domyślnie włączony w Kestrel dla punktów końcowych HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4f260-287">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="4f260-288">Obsługa protokołu HTTP/2 dla usług IIS lub HTTP. sys jest włączona, gdy jest ona obsługiwana przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="4f260-288">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="eventcounters-on-request"></a><span data-ttu-id="4f260-289">EventCounters na żądanie</span><span class="sxs-lookup"><span data-stu-id="4f260-289">EventCounters on request</span></span>

<span data-ttu-id="4f260-290">`Microsoft.AspNetCore.Hosting`hosta EventSource, emituje następujące nowe typy <xref:System.Diagnostics.Tracing.EventCounter> związane z żądaniami przychodzącymi:</span><span class="sxs-lookup"><span data-stu-id="4f260-290">The Hosting EventSource, `Microsoft.AspNetCore.Hosting`, emits the following new <xref:System.Diagnostics.Tracing.EventCounter> types related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="4f260-291">Routing punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="4f260-291">Endpoint routing</span></span>

<span data-ttu-id="4f260-292">Routing punktów końcowych, który umożliwia platformom (na przykład MVC) współdziałanie z oprogramowanie pośredniczące, jest ulepszony:</span><span class="sxs-lookup"><span data-stu-id="4f260-292">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="4f260-293">Kolejność programów pośredniczących i punktów końcowych można skonfigurować w potoku przetwarzania żądań `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4f260-293">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="4f260-294">Punkty końcowe i oprogramowanie pośredniczące dobrze tworzą inne technologie ASP.NET Core, na przykład kontrole kondycji.</span><span class="sxs-lookup"><span data-stu-id="4f260-294">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="4f260-295">Punkty końcowe mogą implementować zasady, takie jak CORS lub Authorization, zarówno w oprogramowaniu pośredniczącym, jak i MVC.</span><span class="sxs-lookup"><span data-stu-id="4f260-295">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="4f260-296">Filtry i atrybuty mogą być umieszczane na metodach w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="4f260-296">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="4f260-297">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing#routing-basics>.</span><span class="sxs-lookup"><span data-stu-id="4f260-297">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="4f260-298">Kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="4f260-298">Health Checks</span></span>

<span data-ttu-id="4f260-299">Kontrole kondycji korzystają z routingu punktu końcowego z hostem ogólnym.</span><span class="sxs-lookup"><span data-stu-id="4f260-299">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="4f260-300">W `Startup.Configure` Wywołaj `MapHealthChecks` w konstruktorze punktów końcowych z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="4f260-300">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="4f260-301">Punkty końcowe sprawdzania kondycji mogą:</span><span class="sxs-lookup"><span data-stu-id="4f260-301">Health Checks endpoints can:</span></span>

* <span data-ttu-id="4f260-302">Określ jeden lub więcej dozwolonych hostów/portów.</span><span class="sxs-lookup"><span data-stu-id="4f260-302">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="4f260-303">Wymagaj autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4f260-303">Require authorization.</span></span>
* <span data-ttu-id="4f260-304">Wymagaj mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="4f260-304">Require CORS.</span></span>

<span data-ttu-id="4f260-305">Aby uzyskać więcej informacji, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="4f260-305">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="4f260-306">Potoki w obiekcie HttpContext</span><span class="sxs-lookup"><span data-stu-id="4f260-306">Pipes on HttpContext</span></span>

<span data-ttu-id="4f260-307">Teraz można odczytać treść żądania i napisać treść odpowiedzi przy użyciu interfejsu API <xref:System.IO.Pipelines>.</span><span class="sxs-lookup"><span data-stu-id="4f260-307">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="4f260-308">Program</span><span class="sxs-lookup"><span data-stu-id="4f260-308">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="4f260-309">Właściwość `HttpRequest.BodyReader` zawiera <xref:System.IO.Pipelines.PipeReader>, których można użyć do odczytu treści żądania.</span><span class="sxs-lookup"><span data-stu-id="4f260-309">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="4f260-310">Program</span><span class="sxs-lookup"><span data-stu-id="4f260-310">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="4f260-311">Właściwość `HttpResponse.BodyWriter` zawiera <xref:System.IO.Pipelines.PipeWriter>, których można użyć do zapisania treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4f260-311">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="4f260-312">`HttpRequest.BodyReader` jest analogiczny do strumienia `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="4f260-312">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="4f260-313">`HttpResponse.BodyWriter` jest analogiczny do strumienia `HttpResponse.Body`.</span><span class="sxs-lookup"><span data-stu-id="4f260-313">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="4f260-314">Udoskonalone raportowanie błędów w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="4f260-314">Improved error reporting in IIS</span></span>

<span data-ttu-id="4f260-315">Błędy uruchamiania podczas hostowania aplikacji ASP.NET Core w usługach IIS teraz generują bogatsze dane diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="4f260-315">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="4f260-316">Te błędy są zgłaszane w dzienniku zdarzeń systemu Windows przy użyciu śladów stosu wszędzie tam, gdzie ma to zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="4f260-316">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="4f260-317">Ponadto wszystkie ostrzeżenia, błędy i Nieobsłużone wyjątki są rejestrowane w dzienniku zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="4f260-317">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="4f260-318">Zestaw SDK usługi i procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="4f260-318">Worker Service and Worker SDK</span></span>

<span data-ttu-id="4f260-319">W programie .NET Core 3,0 wprowadzono nowy szablon aplikacji usługi Worker.</span><span class="sxs-lookup"><span data-stu-id="4f260-319">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="4f260-320">Ten szablon zawiera punkt początkowy do pisania długotrwałych usług w programie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f260-320">This template provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="4f260-321">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="4f260-321">For more information, see:</span></span>

* [<span data-ttu-id="4f260-322">Pracownicy .NET Core jako usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="4f260-322">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="4f260-323">Podniesienie — ulepszenia oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="4f260-323">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="4f260-324">W poprzednich wersjach ASP.NET Core wywoływanie <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> i <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> było problematyczne podczas wdrażania w systemie Linux systemu Azure lub za każdym odwrotnym serwerem proxy innym niż IIS.</span><span class="sxs-lookup"><span data-stu-id="4f260-324">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="4f260-325">Poprawka dla poprzednich wersji została udokumentowana w [tym samym schemacie dla systemu Linux i zwrotnych serwerów proxy innych niż IIS](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span><span class="sxs-lookup"><span data-stu-id="4f260-325">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="4f260-326">Ten scenariusz jest ustalony w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4f260-326">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="4f260-327">Host włącza program [pośredniczący z przekazanymi nagłówkami](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) , gdy zmienna środowiskowa `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="4f260-327">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="4f260-328">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true` w naszych obrazach kontenera.</span><span class="sxs-lookup"><span data-stu-id="4f260-328">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="4f260-329">Usprawnienia wydajności</span><span class="sxs-lookup"><span data-stu-id="4f260-329">Performance improvements</span></span>

<span data-ttu-id="4f260-330">ASP.NET Core 3,0 zawiera wiele ulepszeń, które zmniejszają wykorzystanie pamięci i zwiększają przepływność:</span><span class="sxs-lookup"><span data-stu-id="4f260-330">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="4f260-331">Zmniejszenie użycia pamięci w przypadku korzystania z wbudowanego kontenera iniekcji zależności dla usług objętych zakresem.</span><span class="sxs-lookup"><span data-stu-id="4f260-331">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="4f260-332">Zmniejszenie alokacji w całym środowisku, w tym scenariusze i Routing oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4f260-332">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="4f260-333">Zmniejszenie użycia pamięci dla połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4f260-333">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="4f260-334">Zmniejszenie ilości pamięci i ulepszenia przepływności dla połączeń HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4f260-334">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="4f260-335">Nowy, zoptymalizowany i w pełni asynchroniczny serializator JSON.</span><span class="sxs-lookup"><span data-stu-id="4f260-335">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="4f260-336">Zmniejszenie użycia pamięci i ulepszeń przepływności podczas analizowania formularzy.</span><span class="sxs-lookup"><span data-stu-id="4f260-336">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="4f260-337">ASP.NET Core 3,0 działa tylko na platformie .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="4f260-337">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="4f260-338">Począwszy od ASP.NET Core 3,0, .NET Framework nie jest już obsługiwaną platformą docelową.</span><span class="sxs-lookup"><span data-stu-id="4f260-338">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="4f260-339">Projekty ukierunkowane na .NET Framework mogą być nadal w pełni obsługiwane przy użyciu programu [.NET Core 2,1 LTS Release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span><span class="sxs-lookup"><span data-stu-id="4f260-339">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="4f260-340">Większość pakietów pokrewnych ASP.NET Core 2.1. x będzie obsługiwana przez czas dłuższy niż rok LTS dla platformy .NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="4f260-340">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the three-year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="4f260-341">Informacje o migracji znajdują się w temacie [Porting Code from .NET Framework to .NET Core](/dotnet/core/porting/).</span><span class="sxs-lookup"><span data-stu-id="4f260-341">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="4f260-342">Użyj ASP.NET Core udostępnionej platformy</span><span class="sxs-lookup"><span data-stu-id="4f260-342">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="4f260-343">Współdzielona struktura ASP.NET Core 3,0, zawarta w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), nie wymaga już jawnego elementu `<PackageReference />` w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="4f260-343">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="4f260-344">W przypadku korzystania z zestawu SDK `Microsoft.NET.Sdk.Web` w pliku projektu automatycznie występuje odwołanie do udostępnionej struktury:</span><span class="sxs-lookup"><span data-stu-id="4f260-344">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="4f260-345">Zestawy usunięte z ASP.NET Core udostępnionej platformy</span><span class="sxs-lookup"><span data-stu-id="4f260-345">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="4f260-346">Najbardziej istotnymi zestawami usuniętymi ze współdzielonej platformy ASP.NET Core 3,0 są:</span><span class="sxs-lookup"><span data-stu-id="4f260-346">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="4f260-347">[Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/) (JSON.NET).</span><span class="sxs-lookup"><span data-stu-id="4f260-347">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="4f260-348">Aby dodać Json.NET do ASP.NET Core 3,0, zobacz [Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="4f260-348">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="4f260-349">ASP.NET Core 3,0 wprowadza `System.Text.Json` do odczytu i zapisu JSON.</span><span class="sxs-lookup"><span data-stu-id="4f260-349">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="4f260-350">Aby uzyskać więcej informacji, zobacz [Nowa Serializacja kodu JSON](#new-json-serialization) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="4f260-350">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="4f260-351">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4f260-351">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="4f260-352">Aby uzyskać pełną listę zestawów usuniętych z udostępnionej platformy, zobacz [zestawy usuwane z Microsoft. AspNetCore. App 3,0](https://github.com/aspnet/AspNetCore/issues/3755).</span><span class="sxs-lookup"><span data-stu-id="4f260-352">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="4f260-353">Aby uzyskać więcej informacji na temat motywacji dotyczącej tej zmiany, zobacz artykuł [dotyczący zmiany w firmie Microsoft. AspNetCore. App w 3,0](https://github.com/aspnet/Announcements/issues/325) i [pierwsze spojrzenie na zmiany wprowadzone w ASP.NET Core 3,0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="4f260-353">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
