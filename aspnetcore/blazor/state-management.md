---
title: Zarządzanie stanem Blazor ASP.NET Core
author: guardrex
description: Dowiedz się, jak utrwalać stan w aplikacjach Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/state-management
ms.openlocfilehash: e8a1959a8fc05ea59362bb5824181a9d2e418811
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218872"
---
# <a name="aspnet-core-opno-locblazor-state-management"></a>Zarządzanie stanem Blazor ASP.NET Core

[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Serwer Blazor jest platformą aplikacji stanowych. W większości przypadków aplikacja utrzymuje ciągłe połączenie z serwerem. Stan użytkownika jest przechowywany w pamięci serwera w ramach *obwodu*. 

Przykłady stanu przechowywanego dla obwodu użytkownika obejmują:

* Renderowany interfejs użytkownika&mdash;hierarchii wystąpień składników i ich najnowszych danych wyjściowych renderowania.
* Wartości wszystkich pól i właściwości w wystąpieniach składników.
* Dane przechowywane w wystąpieniach usługi [wtrysku zależności (di)](xref:fundamentals/dependency-injection) , które są objęte zakresem obwodu.

> [!NOTE]
> Ten artykuł rozwiązuje trwałość stanu w aplikacjach Blazor Server. Blazor aplikacje webassembly mogą korzystać z [trwałości stanu po stronie klienta w przeglądarce,](#client-side-in-the-browser) ale wymagają niestandardowych rozwiązań lub pakietów innych firm wykraczających poza zakres tego artykułu.

## <a name="opno-locblazor-circuits"></a>obwody Blazor

Jeśli użytkownik napotyka chwilową utratę połączenia sieciowego, Blazor próbuje ponownie połączyć użytkownika z oryginalnym obwodem, aby mogli nadal korzystać z aplikacji. Jednak ponowne połączenie użytkownika z oryginalnym obwodem w pamięci serwera nie zawsze jest możliwe:

* Serwer nie może całkowicie zachować rozłączonego obwodu. Serwer musi zwolnić obwód rozłączony po upływie limitu czasu lub w przypadku, gdy na serwerze znajduje się pamięć.
* W środowiskach wieloserwerowych ze zrównoważonym obciążeniem wszystkie żądania przetwarzania serwera mogą stać się niedostępne w danym momencie. Poszczególne serwery mogą kończyć się niepowodzeniem lub być automatycznie usuwane, gdy nie są już wymagane do obsługi ogólnej liczby żądań. Oryginalny serwer może nie być dostępny, gdy użytkownik próbuje ponownie nawiązać połączenie.
* Użytkownik może zamknąć i ponownie otworzyć przeglądarkę lub ponownie załadować stronę, co spowoduje usunięcie wszystkich stanów przechowywanych w pamięci przeglądarki. Na przykład wartości ustawione za poorednictwem wywołań międzyoperacyjnych języka JavaScript są tracone.

Gdy nie można ponownie nawiązać połączenia użytkownika z oryginalnym obwodem, użytkownik otrzymuje nowy obwód z pustym stanem. Jest to równoznaczne z zamknięciem i ponownym otwarciem aplikacji klasycznej.

## <a name="preserve-state-across-circuits"></a>Zachowaj stan między obwodymi

W niektórych scenariuszach jest wymagane zachowanie stanu między obwodami. Aplikacja może zachować ważne dane dla użytkownika, jeśli:

* Serwer sieci Web stał się niedostępny.
* W przeglądarce użytkownika wymuszone jest rozpoczęcie nowego obwodu przy użyciu nowego serwera sieci Web.

Ogólnie rzecz biorąc, utrzymanie stanu między obwodami ma zastosowanie do scenariuszy, w których użytkownicy aktywnie tworzą dane, a nie tylko odczytujące dane, które już istnieją.

Aby zachować stan poza pojedynczym obwodem, *nie należy przechowywać danych w pamięci serwera*. Aplikacja musi przechowywać dane w innej lokalizacji magazynu. Trwałość stanu nie jest automatyczna&mdash;należy wykonać kroki podczas tworzenia aplikacji w celu zaimplementowania trwałości danych stanowych.

Trwałość danych jest zwykle wymagana tylko w przypadku stanu wysokiego poziomu, który użytkownicy wystawią nakłady na tworzenie. W poniższych przykładach stan utrwalania polega na zapisywaniu czasu lub pomocy w działaniach komercyjnych:

* Wieloetapowy formularz WebForm &ndash; czasochłonne, aby użytkownik mógł ponownie wprowadzić dane dla kilku ukończonych kroków procesu wieloetapowego, jeśli ich stan zostanie utracony. Użytkownik utraci stan w tym scenariuszu, jeśli przejdą do formularza wieloetapowego i wrócisz do formularza później.
* Koszyk &ndash; każdy istotny dla nich składnik aplikacji, który reprezentuje potencjalne przychody. Użytkownik, który straci swój stan, a tym samym koszyk, może zakupić mniejszą liczbę produktów lub usług w momencie powrotu do lokacji w przyszłości.

Zwykle nie jest konieczne zachowywanie stanu łatwego ponownego tworzenia, takiego jak wprowadzona nazwa użytkownika do okna dialogowego logowania, które nie zostało przesłane.

> [!IMPORTANT]
> Aplikacja może utrzymywać tylko *stan aplikacji*. Interfejsów użytkownika nie mogą być utrwalane, takie jak wystąpienia składników i ich drzewa renderowania. Składniki i drzewa renderowania nie są generalnie serializowane. Aby zachować coś podobnego do stanu interfejsu użytkownika, na przykład rozwinięte węzły drzewa TreeView, aplikacja musi mieć kod niestandardowy, aby modelować zachowanie jako możliwy do serializacji stan aplikacji.

## <a name="where-to-persist-state"></a>Gdzie będzie trwały stan

Trzy Popularne lokalizacje istnieją dla stanu utrwalania w aplikacji Blazor Server. Każde podejście jest najlepiej dostosowane do różnych scenariuszy i ma inne zastrzeżenia:

* [Po stronie serwera w bazie danych](#server-side-in-a-database)
* [Adres URL](#url)
* [Po stronie klienta w przeglądarce](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a>Po stronie serwera w bazie danych

W przypadku trwałych trwałości danych lub wszelkich danych, które muszą obejmować wiele użytkowników lub urządzeń, niezależna baza danych po stronie serwera prawie najlepiej sprawdza się. Dostępne są następujące opcje:

* Relacyjna baza danych SQL
* Magazyn wartości klucza
* Magazyn obiektów BLOB
* Magazyn tabel

Po zapisaniu danych w bazie danych nowy obwód może być uruchamiany przez użytkownika w dowolnym momencie. Dane użytkownika są przechowywane i dostępne w dowolnym nowym obwodzie.

Aby uzyskać więcej informacji na temat opcji usługi Azure Data Storage, zobacz [dokumentację usługi Azure Storage](/azure/storage/) i [bazy danych platformy Azure](https://azure.microsoft.com/product-categories/databases/).

### <a name="url"></a>Adres URL

W przypadku danych przejściowych reprezentujących stan nawigacji należy modelować dane w ramach adresu URL. Przykłady stanu modelu w adresie URL obejmują:

* Identyfikator wyświetlonej jednostki.
* Numer bieżącej strony w siatce stronicowanej.

Zawartość paska adresu przeglądarki jest zachowywana:

* Jeśli użytkownik ręcznie ponownie załaduje stronę.
* Jeśli serwer sieci Web będzie niedostępny,&mdash;użytkownik musi ponownie załadować stronę, aby można było połączyć się z innym serwerem.

Aby uzyskać informacje na temat definiowania wzorców adresów URL za pomocą dyrektywy `@page`, zobacz <xref:blazor/routing>.

### <a name="client-side-in-the-browser"></a>Po stronie klienta w przeglądarce

W przypadku danych przejściowych, które użytkownik aktywnie tworzy, wspólny magazyn zapasowy to `localStorage` i kolekcje `sessionStorage` przeglądarki. Aplikacja nie jest wymagana do zarządzania lub czyszczenia stanu przechowywanego, jeśli obwód został porzucony, co jest korzystne w porównaniu z magazynem po stronie serwera.

> [!NOTE]
> "Po stronie klienta" w tej sekcji odwołuje się do scenariuszy po stronie klienta w przeglądarce, a nie [modelu hostinguBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly). `localStorage` i `sessionStorage` mogą być używane w aplikacjach Blazor webassembly, ale tylko przez napisanie kodu niestandardowego lub użycie pakietu innej firmy.

`localStorage` i `sessionStorage` różnią się w następujący sposób:

* `localStorage` jest objęty zakresem przeglądarki użytkownika. Jeśli użytkownik ponownie załaduje stronę lub zamknie i otworzy przeglądarkę, stan będzie się utrzymywał. Jeśli użytkownik otworzy wiele kart przeglądarki, stan jest udostępniany na kartach. Dane są zachowywane w `localStorage` do momentu jawnego wyczyszczenia.
* `sessionStorage` należy do zakresu karty przeglądarki użytkownika. Jeśli użytkownik ponownie załaduje kartę, stan będzie się utrzymywał. Jeśli użytkownik zamknie kartę lub przeglądarkę, stan zostanie utracony. Jeśli użytkownik otworzy wiele kart przeglądarki, każda karta ma własną niezależną wersję danych.

Ogólnie rzecz biorąc, `sessionStorage` jest bezpiecznie do użycia. `sessionStorage` pozwala uniknąć ryzyka, że użytkownik otwiera wiele kart i napotyka następujące kwestie:

* Błędy w magazynie Stanów na kartach.
* Zachowanie mylące, gdy karta zastępuje stan innych kart.

`localStorage` jest lepszym rozwiązaniem, jeśli aplikacja musi utrzymywać stan w trakcie zamykania i ponownego otwierania przeglądarki.

Ostrzeżenia dotyczące korzystania z magazynu przeglądarki:

* Podobnie jak w przypadku korzystania z bazy danych po stronie serwera, ładowanie i zapisywanie danych są asynchroniczne.
* W przeciwieństwie do bazy danych po stronie serwera, magazyn nie jest dostępny podczas renderowania wstępnego, ponieważ żądana strona nie istnieje w przeglądarce na etapie renderowania.
* Przechowywanie kilku kilobajtów danych jest rozsądne dla Blazor aplikacji serwera. Po kilku kilobajtach należy wziąć pod uwagę wpływ na wydajność, ponieważ dane są ładowane i zapisywane w sieci.
* Użytkownicy mogą wyświetlać i modyfikować dane. [Ochrona danych](xref:security/data-protection/introduction) ASP.NET Core może ograniczyć ryzyko.

## <a name="third-party-browser-storage-solutions"></a>Rozwiązania do magazynowania przeglądarki innych firm

Pakiety NuGet innych firm zapewniają interfejsy API do pracy z `localStorage` i `sessionStorage`.

Warto rozważać wybór pakietu, który w sposób przezroczysty używa ASP.NET Core [ochrony danych](xref:security/data-protection/introduction). Ochrona danych ASP.NET Core szyfruje przechowywane dane i zmniejsza potencjalne ryzyko naruszenia przechowywanych danych. Jeśli dane serializowane w formacie JSON są przechowywane w postaci zwykłego tekstu, użytkownicy mogą zobaczyć dane przy użyciu narzędzi deweloperskich przeglądarki, a także zmodyfikować przechowywane dane. Zabezpieczanie danych nie zawsze jest problemem, ponieważ dane mogą być proste. Na przykład odczytywanie lub modyfikowanie zapisanego koloru elementu interfejsu użytkownika nie jest istotnym zagrożeniem bezpieczeństwa użytkownika lub organizacji. Unikaj zezwalania użytkownikom na inspekcję i manipulowanie *danymi poufnymi*.

## <a name="protected-browser-storage-experimental-package"></a>Pakiet Eksperymentalny magazynu chronionej przeglądarki

Przykładem pakietu NuGet, który zapewnia [ochronę danych](xref:security/data-protection/introduction) dla `localStorage` i `sessionStorage` jest [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).

> [!WARNING]
> `Microsoft.AspNetCore.ProtectedBrowserStorage` jest nieobsługiwanym pakietem eksperymentalnym nieodpowiedninym do użycia w środowisku produkcyjnym.

### <a name="installation"></a>Instalacja

Aby zainstalować pakiet `Microsoft.AspNetCore.ProtectedBrowserStorage`:

1. W projekcie aplikacji Blazor Server Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).
1. W kodzie HTML najwyższego poziomu (na przykład w pliku *Pages/_Host. cshtml* w domyślnym szablonie projektu) Dodaj następujący tag `<script>`:

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. W metodzie `Startup.ConfigureServices` Wywołaj `AddProtectedBrowserStorage`, aby dodać usługi `localStorage` i `sessionStorage` do kolekcji usług:

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a>Zapisz i Załaduj dane w składniku

W dowolnym składniku wymagającym ładowania lub zapisywania danych w magazynie przeglądarki Użyj [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) , aby wstrzyknąć wystąpienie jednego z następujących elementów:

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

Wybór zależy od tego, który magazyn zapasowy ma być używany. W poniższym przykładzie użyto `sessionStorage`:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

Instrukcję `@using` można umieścić w pliku *_Imports. Razor* zamiast w składniku. Użycie pliku *_Imports. Razor* sprawia, że przestrzeń nazw jest dostępna dla większych segmentów aplikacji lub całej aplikacji.

Aby zachować wartość `_currentCount` w składniku `Counter` szablonu projektu, należy zmodyfikować metodę `IncrementCount`, aby użyć `ProtectedSessionStore.SetAsync`:

```csharp
private async Task IncrementCount()
{
    _currentCount++;
    await ProtectedSessionStore.SetAsync("count", _currentCount);
}
```

W większych, bardziej realistycznych aplikacjach przechowywanie pojedynczych pól jest mało prawdopodobne. Aplikacje są bardziej podobne do przechowywania całych obiektów modelu, które zawierają złożone Stany. `ProtectedSessionStore` automatycznie serializować i deserializacji danych JSON.

W poprzednim przykładzie kodu `_currentCount` dane są przechowywane jako `sessionStorage['count']` w przeglądarce użytkownika. Dane nie są przechowywane w postaci zwykłego tekstu, ale nie są chronione przy użyciu [ochrony danych](xref:security/data-protection/introduction)ASP.NET Core. Zaszyfrowane dane można zobaczyć, jeśli `sessionStorage['count']` jest oceniane w konsoli dewelopera w przeglądarce.

Aby odzyskać dane `_currentCount`, jeśli użytkownik powróci do składnika `Counter` później (w tym, jeśli znajdują się w całkowicie nowym obwodzie), użyj `ProtectedSessionStore.GetAsync`:

```csharp
protected override async Task OnInitializedAsync()
{
    _currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

Jeśli parametry składnika obejmują stan nawigacji, wywołaj `ProtectedSessionStore.GetAsync` i przypisz wynik do `OnParametersSetAsync`, a nie `OnInitializedAsync`. `OnInitializedAsync` jest wywoływana tylko raz podczas pierwszego wystąpienia składnika. `OnInitializedAsync` nie zostanie wywołane ponownie później, jeśli użytkownik przejdzie do innego adresu URL, a pozostałe na tej samej stronie. Aby uzyskać więcej informacji, zobacz <xref:blazor/lifecycle>.

> [!WARNING]
> Przykłady w tej sekcji działają tylko wtedy, gdy serwer nie ma włączonej obsługi przed renderowaniem. Po włączeniu obsługi przed renderowaniem zostanie wygenerowany błąd podobny do:
>
> > W tej chwili nie można wystawić wywołań międzyoperacyjnych języka JavaScript. Dzieje się tak, ponieważ składnik jest wstępnie renderowany.
>
> Wyłącz renderowanie lub Dodaj dodatkowy kod, aby współpracował z renderowaniem. Aby dowiedzieć się więcej na temat pisania kodu, który działa z renderowaniem, zobacz sekcję [Obsługa przed renderowaniem](#handle-prerendering) .

### <a name="handle-the-loading-state"></a>Obsłuż stan ładowania

Ponieważ magazyn przeglądarki jest asynchroniczny (dostęp za pośrednictwem połączenia sieciowego), zawsze jest okres, po upływie którego dane będą ładowane i dostępne do użycia przez składnik. Aby uzyskać najlepsze wyniki, renderowanie komunikatu o stanie ładowania podczas ładowania jest w toku zamiast wyświetlania danych pustych lub domyślnych.

Jednym z metod jest śledzenie, czy dane są `null` (nadal ładowane) czy nie. W domyślnym składniku `Counter` liczba jest utrzymywana w `int`. Wprowadź `_currentCount` wartość null, dodając znak zapytania (`?`) do typu (`int`):

```csharp
private int? _currentCount;
```

Zamiast bezwarunkowo wyświetlać przycisk Count i **Increment** , wybierz, aby wyświetlić te elementy tylko wtedy, gdy dane są ładowane:

```razor
@if (_currentCount.HasValue)
{
    <p>Current count: <strong>@_currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a>Obsługa przed renderowaniem

Podczas renderowania:

* Połączenie interaktywne z przeglądarką użytkownika nie istnieje.
* Przeglądarka nie zawiera jeszcze strony, w której można uruchomić kod JavaScript.

`localStorage` lub `sessionStorage` nie są dostępne podczas renderowania. Jeśli składnik próbuje współdziałać z magazynem, zostanie wygenerowany błąd podobny do:

> W tej chwili nie można wystawić wywołań międzyoperacyjnych języka JavaScript. Dzieje się tak, ponieważ składnik jest wstępnie renderowany.

Jednym ze sposobów na rozwiązanie błędu jest wyłączenie renderowania. Jest to zazwyczaj najlepszym wyborem, jeśli aplikacja znacznie korzysta z magazynu opartego na przeglądarce. Renderowanie zwiększa złożoność i nie korzysta z aplikacji, ponieważ aplikacja nie może przeprowadzić renderowania żadnej przydatnej zawartości, dopóki `localStorage` lub `sessionStorage` nie są dostępne.

Aby wyłączyć renderowanie, Otwórz plik *Pages/_Host. cshtml* i Zmień `render-mode` [pomocnika znacznika składnika](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) , aby <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server>.

Renderowanie może być przydatne w przypadku innych stron, które nie używają `localStorage` lub `sessionStorage`. Aby włączyć renderowanie, odłóż operację ładowania do momentu podłączenia przeglądarki do obwodu. Poniżej przedstawiono przykład przechowywania wartości licznika:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore

... rendering code goes here ...

@code {
    private int? _currentCount;
    private bool _isConnected = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // When execution reaches this point, the first *interactive* render
            // is complete. The component has an active connection to the browser.
            _isConnected = true;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        _currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        _currentCount++;
        await ProtectedSessionStore.SetAsync("count", _currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a>Wyrównać zachowywanie stanu do wspólnej lokalizacji

Jeśli wiele składników korzysta z magazynu opartego na przeglądarce, ponowne implementowanie kodu dostawcy stanu wiele razy powoduje utworzenie duplikacji kodu. Jedną z opcji unikania duplikowania kodu jest utworzenie *składnika nadrzędnego dostawcy stanu* , który hermetyzuje logikę dostawcy stanu. Składniki podrzędne mogą współpracować z trwałymi danymi bez względu na mechanizm trwałości stanu.

W poniższym przykładzie składnika `CounterStateProvider` dane licznika są utrwalane:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (_hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool _hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        _hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

Składnik `CounterStateProvider` obsługuje fazę ładowania, ponieważ nie renderuje jej zawartości podrzędnej do momentu ukończenia ładowania.

Aby użyć składnika `CounterStateProvider`, zawiń wystąpienie składnika wokół dowolnego innego składnika, który wymaga dostępu do stanu licznika. Aby zapewnić dostępność stanu dla wszystkich składników w aplikacji, zawiń składnik `CounterStateProvider` wokół `Router` w składniku `App` (*App. Razor*):

```razor
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

Zapakowane składniki są odbierane i mogą modyfikować stan trwałych liczników. Poniższy składnik `Counter` implementuje wzorzec:

```razor
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

Poprzedni składnik nie jest wymagany do współdziałania z `ProtectedBrowserStorage`ani nie zajmuje się fazą "Ładowanie".

Aby przeprowadzić postępowanie z instrukcją prerenderingu zgodnie z wcześniejszym opisem, `CounterStateProvider` można zmienić tak, aby wszystkie składniki korzystające z danych licznika automatycznie działały z użyciem prerenderowania. Szczegółowe informacje znajdują się w sekcji [Obsługa przed renderowaniem](#handle-prerendering) .

Ogólnie rzecz biorąc, zalecany jest wzorzec *składnika nadrzędnego dostawcy stanu* :

* Aby korzystać z stanu w wielu innych składnikach.
* Jeśli istnieje tylko jeden obiekt stanu najwyższego poziomu, który ma być trwały.

Aby zachować wiele różnych obiektów stanu i korzystać z różnych podzbiorów obiektów w różnych miejscach, lepiej jest unikać obsługi ładowania i zapisywania stanu globalnie.
