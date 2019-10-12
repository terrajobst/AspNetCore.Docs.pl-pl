---
title: Obsługa błędów w aplikacjach ASP.NET Core Blazor
author: guardrex
description: Odkryj, jak ASP.NET Core Blazor, jak Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/handle-errors
ms.openlocfilehash: fb4c7cacfe8be2417d6009cfc722595d0d91d530
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288836"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>Obsługa błędów w aplikacjach ASP.NET Core Blazor

[Steve Sanderson](https://github.com/SteveSandersonMS)

W tym artykule opisano sposób, w jaki Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Jak platforma Blazor reaguje na Nieobsłużone wyjątki

Serwer Blazor jest strukturą stanową. Gdy użytkownicy współpracują z aplikacją, utrzymują połączenie z serwerem znanym jako *obwód*. Obwód zawiera aktywne wystąpienia składnika, a także wiele innych aspektów stanu, takich jak:

* Najnowsze renderowane dane wyjściowe składników.
* Bieżący zestaw delegatów obsługi zdarzeń, które mogą być wyzwalane przez zdarzenia po stronie klienta.

Jeśli użytkownik otworzy aplikację na wielu kartach przeglądarki, mają one wiele niezależnych obwodów.

Blazor traktuje większość nieobsłużonych wyjątków jako krytyczne do obwodu, w którym występują. Jeśli obwód zostanie przerwany z powodu nieobsługiwanego wyjątku, użytkownik może nadal korzystać z aplikacji tylko przez ponowne załadowanie strony w celu utworzenia nowego obwodu. Nie ma to wpływu na obwody, które zostały przerwane, które są obwody dla innych użytkowników lub kart przeglądarki. Ten scenariusz jest podobny do aplikacji klasycznej, która uległa awarii @ no__t-0the, należy ponownie uruchomić aplikację, ale nie ma to wpływu na inne aplikacje.

Obwód jest zakończony, gdy wystąpił nieobsługiwany wyjątek z następujących powodów:

* Nieobsługiwany wyjątek często pozostawia obwód w niezdefiniowanym stanie.
* Nie można zagwarantować normalnej operacji aplikacji po wystąpieniu nieobsłużonego wyjątku.
* W przypadku kontynuowania obwodu w aplikacji mogą pojawić się luki w zabezpieczeniach.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Zarządzanie nieobsługiwanymi wyjątkami w kodzie dewelopera

Aby aplikacja kontynuowała działanie po wystąpieniu błędu, aplikacja musi mieć logikę obsługi błędów. W dalszej części tego artykułu opisano potencjalne źródła nieobsłużonych wyjątków.

W środowisku produkcyjnym nie Renderuj komunikatów wyjątków struktury ani śladów stosu w interfejsie użytkownika. Renderowanie komunikatów o wyjątkach lub śladów stosu może:

* Ujawnianie poufnych informacji użytkownikom końcowym.
* Pomóż złośliwemu użytkownikowi wykrywać słabe strony w aplikacji, która może naruszyć bezpieczeństwo aplikacji, serwera lub sieci.

## <a name="log-errors-with-a-persistent-provider"></a>Rejestrowanie błędów przez dostawcę trwałego

Jeśli wystąpi nieobsługiwany wyjątek, wyjątek jest rejestrowany w przypadku wystąpień <xref:Microsoft.Extensions.Logging.ILogger> skonfigurowanych w kontenerze usługi. Domyślnie aplikacje Blazor logują się do danych wyjściowych konsoli za pomocą dostawcy rejestrowania konsoli. Należy rozważyć logowanie do bardziej trwałej lokalizacji z dostawcą, który zarządza rozmiarem dziennika i rotacją dzienników. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

Podczas opracowywania Blazor zazwyczaj wszystkie szczegóły wyjątków do konsoli przeglądarki, aby pomóc w debugowaniu. W środowisku produkcyjnym szczegółowe błędy w konsoli przeglądarki są domyślnie wyłączone, co oznacza, że błędy nie są wysyłane do klientów, ale wszystkie szczegóły wyjątku nadal są rejestrowane po stronie serwera. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.

Należy zdecydować, które zdarzenia mają być rejestrowane, oraz poziom ważności zarejestrowanych zdarzeń. Nieszkodliwi użytkownicy mogą być w stanie wyzwolić błędy w sposób celowy. Na przykład nie należy rejestrować zdarzenia z błędu, gdzie w adresie URL składnika, który wyświetla szczegóły produktu, jest podano nieznanego `ProductId`. Nie wszystkie błędy powinny być traktowane jako zdarzenia o wysokiej ważności do rejestrowania.

## <a name="places-where-errors-may-occur"></a>Miejsca, w których mogą wystąpić błędy

Kod struktury i aplikacji może wyzwolić Nieobsłużone wyjątki w jednej z następujących lokalizacji:

* [Tworzenie wystąpienia składnika](#component-instantiation)
* [Metody cyklu życia](#lifecycle-methods)
* [Logika renderowania](#rendering-logic)
* [Programy obsługi zdarzeń](#event-handlers)
* [Usuwanie składników](#component-disposal)
* [Międzyoperacyjność JavaScript](#javascript-interop)
* [Programy obsługi obwodu](#circuit-handlers)
* [Usuwanie obwodu](#circuit-disposal)
* [Renderowanie prerenderingu](#prerendering)

Poprzednie Nieobsłużone wyjątki zostały opisane w poniższych sekcjach tego artykułu.

### <a name="component-instantiation"></a>Tworzenie wystąpienia składnika

Gdy Blazor tworzy wystąpienie składnika:

* Konstruktor składnika jest wywoływany.
* Konstruktory wszelkich niepojedynczych usług DI dostarczonych do konstruktora składnika za pośrednictwem dyrektywy [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) lub atrybutu [[wstrzyknięcie]](xref:blazor/dependency-injection#request-a-service-in-a-component) są wywoływane. 

Obwód kończy się niepowodzeniem, gdy dowolny wykonany Konstruktor lub setter dla dowolnej właściwości `[Inject]` zgłasza nieobsługiwany wyjątek. Wyjątek jest krytyczny, ponieważ struktura nie może utworzyć wystąpienia składnika. Jeśli logika konstruktora może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.

### <a name="lifecycle-methods"></a>Metody cyklu życia

W trakcie okresu istnienia składnika Blazor wywołuje metody cyklu życia:

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Jeśli jakakolwiek metoda cyklu życia zgłasza wyjątek, synchronicznie lub asynchronicznie, wyjątek jest krytyczny dla obwodu. Aby składniki zajmowały błędy w metodach cyklu życia, Dodaj logikę obsługi błędów.

W poniższym przykładzie, gdzie `OnParametersSetAsync` wywołuje metodę w celu uzyskania produktu:

* Wyjątek zgłoszony w metodzie `ProductRepository.GetProductByIdAsync` jest obsługiwany przez instrukcję `try-catch`.
* Gdy jest wykonywany blok `catch`:
  * `loadFailed` jest ustawiona na `true`, który jest używany do wyświetlania komunikatu o błędzie dla użytkownika.
  * Błąd jest rejestrowany.

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>Logika renderowania

Znaczniki deklaratywne w pliku składnika `.razor` są kompilowane do C# metody o nazwie `BuildRenderTree`. Gdy składnik jest renderowany, `BuildRenderTree` wykonuje i tworzy strukturę danych opisującą elementy, tekst i składniki podrzędne renderowanego składnika.

Logika renderowania może zgłosić wyjątek. Przykład tego scenariusza występuje, gdy `@someObject.PropertyName` jest oceniane, ale `@someObject` jest `null`. Nieobsługiwany wyjątek zgłoszony przez logikę renderowania jest krytyczny dla obwodu.

Aby zapobiec wystąpieniu wyjątku odwołania o wartości null w logice renderowania, przed uzyskaniem dostępu do elementów członkowskich Sprawdź, czy jest to obiekt `null`. W poniższym przykładzie właściwości `person.Address` nie są dostępne, jeśli `person.Address` jest `null`:

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

Poprzedni kod założono, że `person` nie `null`. Często Struktura kodu gwarantuje, że obiekt istnieje w momencie renderowania składnika. W takich przypadkach nie trzeba sprawdzać `null` w logice renderowania. W poprzednim przykładzie można zagwarantować, że istnieje `person`, ponieważ podczas tworzenia wystąpienia składnika jest tworzony `person`.

### <a name="event-handlers"></a>Procedury obsługi zdarzeń

Kod po stronie klienta wyzwala wywołania kodu, C# gdy programy obsługi zdarzeń są tworzone przy użyciu:

* `@onclick`
* `@onchange`
* Inne atrybuty `@on...`
* `@bind`

Kod procedury obsługi zdarzeń może zgłosić nieobsługiwany wyjątek w tych scenariuszach.

Jeśli procedura obsługi zdarzeń zgłasza nieobsługiwany wyjątek (na przykład kwerenda bazy danych kończy się niepowodzeniem), wyjątek jest krytyczny dla obwodu. Jeśli aplikacja wywołuje kod, który może zakończyć się niepowodzeniem z powodów zewnętrznych, należy zastosować wyjątek pułapki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.

Jeśli kod użytkownika nie jest pułapk i nie obsługuje wyjątku, struktura rejestruje wyjątek i kończy obwód.

### <a name="component-disposal"></a>Usuwanie składników

Składnik może zostać usunięty z interfejsu użytkownika, na przykład, ponieważ użytkownik przeszedł do innej strony. Gdy składnik implementujący <xref:System.IDisposable?displayProperty=fullName> jest usuwany z interfejsu użytkownika, struktura wywołuje metodę <xref:System.IDisposable.Dispose*> składnika. 

Jeśli metoda `Dispose` składnika zgłasza nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu. Jeśli logika usuwania może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.

Aby uzyskać więcej informacji na temat usuwania składników, zobacz <xref:blazor/components#component-disposal-with-idisposable>.

### <a name="javascript-interop"></a>Międzyoperacyjność JavaScript

`IJSRuntime.InvokeAsync<T>` umożliwia kodowi .NET wykonywanie wywołań asynchronicznych do środowiska uruchomieniowego JavaScript w przeglądarce użytkownika.

Poniższe warunki dotyczą obsługi błędów z `InvokeAsync<T>`:

* Jeśli wywołanie `InvokeAsync<T>` kończy się synchronicznie, wystąpi wyjątek programu .NET. Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład dlatego, że nie można serializować dostarczonych argumentów. Kod dewelopera musi przechwycić wyjątek. Jeśli kod aplikacji w obsłudze zdarzeń lub metoda cyklu życia składnika nie obsługuje wyjątku, wynikający z nich wyjątek jest krytyczny dla obwodu.
* Jeśli wywołanie `InvokeAsync<T>` zakończy się asynchronicznie, program .NET <xref:System.Threading.Tasks.Task> kończy się niepowodzeniem. Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład ponieważ kod po stronie JavaScript zgłasza wyjątek lub zwraca `Promise`, który został ukończony jako `rejected`. Kod dewelopera musi przechwycić wyjątek. W przypadku użycia operatora [await](/dotnet/csharp/language-reference/keywords/await) Rozważ zapakowanie wywołania metody w instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem. W przeciwnym razie niepowodzenie kodu spowoduje nieobsłużony wyjątek, który jest krytyczny dla obwodu.
* Domyślnie wywołania `InvokeAsync<T>` muszą zostać ukończone w określonym przedziale czasu lub w przeciwnym razie upłynął limit czasu połączenia. Domyślny limit czasu wynosi jedną minutę. Limit czasu chroni kod przed utratą połączenia sieciowego lub kodem JavaScript, który nigdy nie odsyła komunikat uzupełniający. Jeśli wystąpiło przełączenie, wyniki `Task` zakończą się niepowodzeniem z <xref:System.OperationCanceledException>. Zalewka i przetwórz wyjątek z rejestrowaniem.

Podobnie kod JavaScript może inicjować wywołania metod .NET wskazywanych przez [atrybut [JSInvokable]](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions). Jeśli te metody .NET zgłaszają nieobsługiwany wyjątek:

* Wyjątek nie jest traktowany jako krytyczny dla obwodu.
* @No__t po stronie skryptu JavaScript jest odrzucany.

Istnieje możliwość użycia kodu obsługi błędów po stronie .NET lub stronie JavaScript wywołania metody.

Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Programy obsługi obwodu

Blazor umożliwia kodowi Definiowanie *procedury obsługi obwodu*, która odbiera powiadomienia w przypadku zmiany stanu obwodu użytkownika. Używane są następujące stany:

* `initialized`
* `connected`
* `disconnected`
* `disposed`

Powiadomienia są zarządzane przez zarejestrowanie usługi DI, która dziedziczy z abstrakcyjnej klasy bazowej `CircuitHandler`.

Jeśli metody obsługi niestandardowego obwodu zgłaszają nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu. Aby tolerować wyjątki w kodzie programu obsługi lub metodach wywoływanych, zawiń kod w co najmniej jednej instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.

### <a name="circuit-disposal"></a>Usuwanie obwodu

Gdy obwód kończy się, ponieważ użytkownik odłączył się i struktura czyści stan obwodu, struktura usuwa zakres DI obwodu. Oddysponowanie zakresu polega na usunięciu wszelkich usług typu DI w zakresie obwodów, które implementują <xref:System.IDisposable?displayProperty=fullName>. Jeśli jakakolwiek usługa nie zgłasza nieobsłużonego wyjątku podczas usuwania, struktura rejestruje wyjątek.

### <a name="prerendering"></a>Renderowanie prerenderingu

Składniki Blazor mogą być wstępnie renderowane przy użyciu `Html.RenderComponentAsync`, aby ich renderowane znaczniki HTML były zwracane jako część początkowego żądania HTTP użytkownika. Działa to w następujący sposób:

* Tworzenie nowego obwodu zawierającego wszystkie wstępnie renderowane składniki, które są częścią tej samej strony.
* Generowanie początkowego kodu HTML.
* Podtraktowanie obwodu jako `disconnected` do momentu, aż przeglądarka użytkownika ustanowi połączenie sygnalizujące z powrotem do tego samego serwera w celu wznowienia interakcji z obwodem.

Jeśli jakikolwiek składnik zgłasza nieobsłużony wyjątek podczas renderowania pre, na przykład podczas wykonywania metody cyklu życia lub logiki renderowania:

* Wyjątek jest krytyczny dla obwodu.
* Wyjątek jest generowany w stosie wywołań z wywołania `Html.RenderComponentAsync`. W związku z tym całe żądanie HTTP kończy się niepowodzeniem, chyba że wyjątek jest jawnie przechwycony przez kod dewelopera.

W normalnych warunkach w przypadku niepowodzenia wstępnego renderowania kontynuowanie kompilowania i renderowania składnika nie ma sensu, ponieważ nie można renderować składnika roboczego.

Aby tolerować błędy, które mogą wystąpić podczas renderowania prerenderingu, logika obsługi błędów musi być umieszczona wewnątrz składnika, który może zgłaszać wyjątki. Używaj instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem. Zamiast zawijać wywołanie do `RenderComponentAsync` w instrukcji `try-catch`, umieść logikę obsługi błędów w składniku renderowanym przez `RenderComponentAsync`.

## <a name="advanced-scenarios"></a>Scenariusze zaawansowane

### <a name="recursive-rendering"></a>Renderowanie cykliczne

Składniki można cyklicznie zagnieżdżać. Jest to przydatne do reprezentowania struktur danych rekursywnych. Na przykład składnik `TreeNode` może renderować więcej składników `TreeNode` dla każdego elementu podrzędnego węzła.

W przypadku renderowania cyklicznego należy unikać tworzenia wzorców, które powodują nieskończoną rekursję:

* Nie Renderuj rekursywnie struktury danych zawierającej cykl. Na przykład nie Renderuj węzła drzewa, którego elementy podrzędne zawierają sam siebie.
* Nie twórz łańcucha układów zawierających cykl. Na przykład nie twórz układu, którego układ jest sam.
* Nie Zezwalaj użytkownikowi końcowemu na naruszenie nieodmian rekursji (reguł) przy użyciu złośliwego wpisu danych lub wywołań międzyoperacyjnych języka JavaScript.

Nieskończone pętle podczas renderowania:

* Powoduje, że proces renderowania kontynuuje działanie zawsze.
* Jest równoznaczny z tworzeniem niezakończonej pętli.

W tych scenariuszach obwód, którego to dotyczy, zawiesza się, a wątek zwykle próbuje wykonać:

* Zużywaj ilość czasu procesora CPU dozwoloną przez system operacyjny w nieskończoność.
* Korzystaj z nieograniczonej ilości pamięci serwera. Zużywanie nieograniczonej pamięci jest równoważne scenariuszowi, w którym niezakończona pętla dodaje wpisy do kolekcji na każdej iteracji.

Aby uniknąć nieskończonych wzorców rekursji, należy się upewnić, że kod renderowania cyklicznego zawiera odpowiednie warunki zatrzymania.

### <a name="custom-render-tree-logic"></a>Logika drzewa renderowania niestandardowego

Większość składników Blazor jest implementowana jako pliki *Razor* i są kompilowane do tworzenia logiki, która działa na `RenderTreeBuilder` w celu renderowania danych wyjściowych. Deweloper może ręcznie zaimplementować logikę `RenderTreeBuilder` przy użyciu C# kodu procedury. Aby uzyskać więcej informacji, zobacz <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> Korzystanie z logiki konstruktora drzewa renderowania ręcznego jest uznawane za zaawansowane i niebezpieczne scenariusze, które nie są zalecane do ogólnego tworzenia składników.

Jeśli zostanie zapisany kod `RenderTreeBuilder`, Deweloper musi zagwarantować poprawność kodu. Na przykład Deweloper musi upewnić się, że:

* Wywołania `OpenElement` i `CloseElement` są prawidłowo zrównoważone.
* Atrybuty są dodawane tylko w prawidłowych miejscach.

Nieprawidłowa ręczna logika konstruktora drzewa renderowania może spowodować dowolne niezdefiniowane zachowanie, w tym awarie, zawieszenie serwera i luki w zabezpieczeniach.

Rozważ ręczne renderowanie logiki konstruktora drzew drzewa na tym samym poziomie złożoności i z tym samym poziomem *zagrożenia* co ręczne pisanie kodu zestawu lub instrukcji MSIL.
