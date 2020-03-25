---
title: Wywoływanie metod .NET z funkcji języka JavaScript w ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać metody .NET z funkcji języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: dbf44fe7923998c65119e42d97c304890fa95523
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218794"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="a95a1-103">Wywoływanie metod .NET z funkcji języka JavaScript w ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a95a1-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="a95a1-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="a95a1-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a95a1-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z metod .NET i metod .NET z funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a95a1-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="a95a1-106">Te scenariusze nazywa się *współdziałaniem JavaScript* (w programie*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="a95a1-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="a95a1-107">W tym artykule opisano wywoływanie metod .NET z języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a95a1-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="a95a1-108">Aby uzyskać informacje na temat wywoływania funkcji JavaScript z platformy .NET, zobacz <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="a95a1-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="a95a1-109">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a95a1-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="a95a1-110">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="a95a1-110">Static .NET method call</span></span>

<span data-ttu-id="a95a1-111">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="a95a1-112">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="a95a1-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="a95a1-113">Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="a95a1-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="a95a1-114">Metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-114">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="a95a1-115">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a95a1-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="a95a1-116">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="a95a1-117">Atrybut `JSInvokable` jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="a95a1-117">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="a95a1-118">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-118">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="a95a1-119">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="a95a1-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="a95a1-120">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-120">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="a95a1-121">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a95a1-121">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="a95a1-122">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="a95a1-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="a95a1-123">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="a95a1-124">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="a95a1-124">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="a95a1-125">W pliku JavaScript po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="a95a1-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="a95a1-126">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="a95a1-126">Instance method call</span></span>

<span data-ttu-id="a95a1-127">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a95a1-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="a95a1-128">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="a95a1-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="a95a1-129">Przekaż wystąpienie platformy .NET przez odwołanie do języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="a95a1-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="a95a1-130">Utwórz wywołanie statyczne do `DotNetObjectReference.Create`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-130">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="a95a1-131">Zawiń wystąpienie w wystąpieniu `DotNetObjectReference` i Wywołaj `Create` w wystąpieniu `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-131">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="a95a1-132">Usuwanie `DotNetObjectReference` obiektów (przykład pojawia się w dalszej części tej sekcji).</span><span class="sxs-lookup"><span data-stu-id="a95a1-132">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="a95a1-133">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="a95a1-134">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a95a1-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="a95a1-135">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a95a1-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="a95a1-136">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a95a1-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="a95a1-137">Po wybraniu przycisku **Wyzwalaj metodę wystąpienia .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="a95a1-137">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="a95a1-138">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-138">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="a95a1-139">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="a95a1-140">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

<span data-ttu-id="a95a1-141">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-141">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="a95a1-142">Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="a95a1-143">Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwraca komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a95a1-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="a95a1-144">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-144">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="a95a1-145">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="a95a1-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="a95a1-146">Aby uniknąć przecieków pamięci i zezwolić na wyrzucanie elementów bezużytecznych w składniku, który tworzy `DotNetObjectReference`, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="a95a1-146">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, adopt one of the following approaches:</span></span>

* <span data-ttu-id="a95a1-147">Metoda Dispose obiektu w klasie, która utworzyła wystąpienie `DotNetObjectReference`:</span><span class="sxs-lookup"><span data-stu-id="a95a1-147">Dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime _jsRuntime;
      private DotNetObjectReference<HelloHelper> _objRef;

      public ExampleJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return _jsRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

  <span data-ttu-id="a95a1-148">Poprzedni wzorzec przedstawiony w klasie `ExampleJsInterop` można również zaimplementować w składniku:</span><span class="sxs-lookup"><span data-stu-id="a95a1-148">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>

  ```razor
  @page "/JSInteropComponent"
  @using BlazorSample.JsInteropClasses
  @implements IDisposable
  @inject IJSRuntime JSRuntime

  <h1>JavaScript Interop</h1>

  <button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
      Trigger .NET instance method HelloHelper.SayHello
  </button>

  @code {
      private DotNetObjectReference<HelloHelper> _objRef;

      public async Task TriggerNetInstanceMethod()
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JSRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

* <span data-ttu-id="a95a1-149">Gdy składnik lub Klasa nie usuwa `DotNetObjectReference`, Usuń obiekt na kliencie, wywołując `.dispose()`:</span><span class="sxs-lookup"><span data-stu-id="a95a1-149">When the component or class doesn't dispose of the `DotNetObjectReference`, dispose of the object on the client by calling `.dispose()`:</span></span>

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a><span data-ttu-id="a95a1-150">Wywołanie metody wystąpienia składnika</span><span class="sxs-lookup"><span data-stu-id="a95a1-150">Component instance method call</span></span>

<span data-ttu-id="a95a1-151">Aby wywołać metody .NET składnika:</span><span class="sxs-lookup"><span data-stu-id="a95a1-151">To invoke a component's .NET methods:</span></span>

* <span data-ttu-id="a95a1-152">Użyj funkcji `invokeMethod` lub `invokeMethodAsync`, aby wykonać wywołanie metody statycznej do składnika.</span><span class="sxs-lookup"><span data-stu-id="a95a1-152">Use the `invokeMethod` or `invokeMethodAsync` function to make a static method call to the component.</span></span>
* <span data-ttu-id="a95a1-153">Metoda statyczna składnika zawija wywołanie metody instancji jako wywołane `Action`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-153">The component's static method wraps the call to its instance method as an invoked `Action`.</span></span>

<span data-ttu-id="a95a1-154">W języku JavaScript po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="a95a1-154">In the client-side JavaScript:</span></span>

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

<span data-ttu-id="a95a1-155">*Strony/JSInteropComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-155">*Pages/JSInteropComponent.razor*:</span></span>

```razor
@page "/JSInteropComponent"

<p>
    Message: @_message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action _action;
    private string _message = "Select the button.";

    protected override void OnInitialized()
    {
        _action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

<span data-ttu-id="a95a1-156">Jeśli istnieje kilka składników, z których każda wywołuje metody wystąpienia, użyj klasy pomocnika, aby wywołać metody wystąpienia (jako `Action`s) każdego składnika.</span><span class="sxs-lookup"><span data-stu-id="a95a1-156">When there are several components, each with instance methods to call, use a helper class to invoke the instance methods (as `Action`s) of each component.</span></span>

<span data-ttu-id="a95a1-157">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a95a1-157">In the following example:</span></span>

* <span data-ttu-id="a95a1-158">Składnik `JSInterop` zawiera kilka składników `ListItem`.</span><span class="sxs-lookup"><span data-stu-id="a95a1-158">The `JSInterop` component contains several `ListItem` components.</span></span>
* <span data-ttu-id="a95a1-159">Każdy składnik `ListItem` składa się z komunikatu i przycisku.</span><span class="sxs-lookup"><span data-stu-id="a95a1-159">Each `ListItem` component is composed of a message and a button.</span></span>
* <span data-ttu-id="a95a1-160">Po wybraniu przycisku składnik `ListItem`, `ListItem``UpdateMessage` Metoda zmienia tekst elementu listy i ukrywa przycisk.</span><span class="sxs-lookup"><span data-stu-id="a95a1-160">When a `ListItem` component button is selected, that `ListItem`'s `UpdateMessage` method changes the list item text and hides the button.</span></span>

<span data-ttu-id="a95a1-161">*MessageUpdateInvokeHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-161">*MessageUpdateInvokeHelper.cs*:</span></span>

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action _action;

    public MessageUpdateInvokeHelper(Action action)
    {
        _action = action;
    }

    [JSInvokable("BlazorSample")]
    public void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

<span data-ttu-id="a95a1-162">W języku JavaScript po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="a95a1-162">In the client-side JavaScript:</span></span>

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

<span data-ttu-id="a95a1-163">*Shared/ListItem. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-163">*Shared/ListItem.razor*:</span></span>

```razor
@inject IJSRuntime JsRuntime

<li>
    @_message
    <button @onclick="InteropCall" style="display:@_display">InteropCall</button>
</li>

@code {
    private string _message = "Select one of these list item buttons.";
    private string _display = "inline-block";
    private MessageUpdateInvokeHelper _messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        _messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JsRuntime.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(_messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        _display = "none";
        StateHasChanged();
    }
}
```

<span data-ttu-id="a95a1-164">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a95a1-164">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a><span data-ttu-id="a95a1-165">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a95a1-165">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="a95a1-166">InteropComponent. Razor — przykład (repozytorium dotnet/AspNetCore w witrynie GitHub, 3,1 gałąź wydania)</span><span class="sxs-lookup"><span data-stu-id="a95a1-166">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="a95a1-167">[Wykonywanie dużych transferów danych w aplikacjach Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="a95a1-167">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
