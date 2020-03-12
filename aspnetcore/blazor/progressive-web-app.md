---
title: Kompilowanie progresywnych aplikacji sieci Web za pomocą ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak tworzyć Blazorprogresywne aplikacje sieci Web (PWAs), aplikacje sieci Web, które używają nowoczesnych funkcji przeglądarki, aby zachować takie działania jak aplikacje klasyczne.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083721"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>Kompilowanie progresywnych aplikacji sieci Web za pomocą ASP.NET Core Blazor webassembly

[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Progresywna aplikacja sieci Web (PWA) to aplikacja oparta na sieci Web, która korzysta z nowoczesnych interfejsów API i funkcji w celu zachowania aplikacji klasycznych. Dostępne są następujące możliwości:

* Praca w trybie offline i zawsze trwa ładowanie niezależnie od szybkości sieci
* Możliwość uruchamiania w osobnym oknie aplikacji, a nie tylko w oknie przeglądarki
* Jest uruchamiany z menu Start systemu operacyjnego hosta, dokowania lub ekranu głównego
* Otrzymywanie powiadomień wypychanych z serwera wewnętrznej bazy danych, nawet jeśli użytkownik nie korzysta z aplikacji
* Automatyczne aktualizowanie w tle

Użytkownik może najpierw odnaleźć i korzystać z aplikacji w swojej przeglądarce internetowej, podobnie jak każda inna aplikacja jednostronicowa (SPA), później postępować w celu zainstalowania jej w systemie operacyjnym i włączenia powiadomień wypychanych. Dlatego używamy warunkowego *postępu*.

Blazor webassembly to prawdziwa oparta na standardach platforma aplikacji sieci Web po stronie klienta, dzięki której można korzystać z dowolnego interfejsu API przeglądarki, w tym interfejsów API programu PWA, które są potrzebne do obsługi wymienionych powyżej. Może działać w trybie offline podobnie jak w przypadku każdej innej technologii sieci Web po stronie klienta.

## <a name="pwa-template"></a>Szablon PWA

Podczas tworzenia nowej aplikacji Blazor webassembly jest oferowana opcja dodawania funkcji PWA. W programie Visual Studio opcja jest określona jako pole wyboru w oknie dialogowym tworzenia projektu:

![obraz](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

Jeśli tworzysz projekt w wierszu polecenia, możesz użyć flagi `--pwa`. Na przykład

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

W obu przypadkach możesz połączyć ten program z opcją "ASP.NET Core Hosted", jeśli chcesz, ale nie musisz tego robić. Funkcje programu PWA są niezależne od modelu hostingu.

## <a name="installation-and-app-manifest"></a>Manifest instalacji i aplikacji

Podczas odwiedzania aplikacji utworzonej przy użyciu opcji szablonu PWA użytkownicy mogą zainstalować aplikację w menu Start, zadokowanym lub na ekranie głównym systemu operacyjnego.

Sposób przedstawiania tej opcji zależy od przeglądarki użytkownika. Na przykład w przypadku korzystania z przeglądarek opartych na formacie chromu w programie Desktop, takich jak Edge lub Chrome, na pasku adresu URL pojawia się przycisk *Dodaj* .

![obraz](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

W systemie iOS osoby odwiedzające mogą zainstalować program PWA przy użyciu przycisku *udostępniania* Safari i jego opcji *Dodaj do homescreen* . W przypadku programu Chrome dla systemu Android użytkownicy powinni nacisnąć przycisk *menu* w prawym górnym rogu, a następnie wybrać pozycję *Dodaj do ekranu głównego*.

Po zainstalowaniu aplikacja pojawia się w osobnym oknie, bez paska adresu.

![obraz](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

Aby dostosować tytuł okna, schemat kolorów, ikonę lub inne szczegóły, zobacz plik `manifest.json` w katalogu *wwwroot* Twojego projektu. Schemat tego pliku jest definiowany przez standardy sieci Web. Aby uzyskać szczegółową dokumentację, zobacz https://developer.mozilla.org/docs/Web/Manifest.

## <a name="offline-support"></a>Obsługa offline

Domyślnie aplikacje utworzone przy użyciu opcji szablonu PWA obsługują uruchamianie w trybie offline. Użytkownik musi najpierw odwiedzić aplikację, gdy jest w trybie online, a następnie automatycznie pobierze i przetworzy pamięć podręczną wszystkich zasobów wymaganych do działania w trybie offline.

> [!IMPORTANT]
> Obsługa w trybie offline jest włączona tylko dla *opublikowanych* aplikacji. Nie jest ona włączona podczas tworzenia. Dzieje się tak, ponieważ mogłoby to zakłócać proces tworzenia zmian i testowania.

> [!WARNING]
> Jeśli zamierzasz wysłać aplikację PWA z obsługą trybu offline, istnieje [kilka ważnych ostrzeżeń i](#caveats-for-offline-pwas) zaleceń, które należy zrozumieć. Są one związane z PWAs w trybie offline i nie są specyficzne dla Blazor. Pamiętaj, aby przeczytać i zrozumieć te zastrzeżenia przed wprowadzeniem założeń dotyczących działania aplikacji z obsługą trybu offline.

Aby zobaczyć, jak działa obsługa w trybie offline, należy najpierw [opublikować aplikację](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app)i hostować ją na serwerze obsługującym protokół https. Po odwiedzeniu aplikacji powinno być możliwe otwarcie narzędzi deweloperskich przeglądarki i sprawdzenie, czy *proces roboczy usługi* został zarejestrowany dla hosta:

![obraz](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

Ponadto w przypadku ponownego załadowania strony na karcie *Sieć* powinna zostać wyświetlona wartość wszystkie zasoby potrzebne do załadowania strony są pobierane z *procesu roboczego usługi* lub pamięci *podręcznej pamięci*:

![obraz](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

Oznacza to, że przeglądarka nie jest zależna od dostępu do sieci w celu załadowania aplikacji. Aby to sprawdzić, możesz zamknąć serwer sieci Web lub poinstruować przeglądarkę, aby zasymulować tryb offline:

![obraz](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

Teraz, nawet bez dostępu do serwera sieci Web, powinno być możliwe ponowne załadowanie strony i sprawdzenie, czy aplikacja jest nadal ładowana i uruchomiona. Podobnie nawet w przypadku symulowania bardzo wolnego połączenia sieciowego strona zostanie załadowana natychmiast po załadowaniu jej niezależnie od sieci.

### <a name="service-worker"></a>Proces roboczy usługi

Obsługa w trybie offline jest realizowana przy użyciu procesu roboczego usługi. Jest to standard sieci Web, który nie jest specyficzny dla Blazor. Aby uzyskać dokumentację dotyczącą pracowników usług, zobacz https://developer.mozilla.org/docs/Web/API/Service_Worker_API. Aby dowiedzieć się więcej o typowych wzorcach użycia dla pracowników usług, zobacz doskonałe informacje o [cyklu życia pracownika usługi](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).

szablon w programie BlazorPWA tworzy dwa pliki procesów roboczych usługi:

* *wwwroot/Service-Worker. js*, który jest używany podczas opracowywania
* *wwwroot/Service-Worker. opublikowano. js*, który jest używany po opublikowaniu aplikacji

Jeśli chcesz udostępnić logikę między tymi dwoma plikami, rozważ dodanie trzeciego pliku JavaScript do przechowywania typowej logiki i Użyj [`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) do załadowania logiki do obu plików.

#### <a name="cache-first-fetch-strategy"></a>Strategia pobierania w pamięci podręcznej

Wbudowany proces roboczy usługi *Service-Worker. opublikowano. js* rozpoznaje żądania przy użyciu strategii *pierwszej pamięci podręcznej* . Oznacza to, że zawsze woli zwrócić zawartość pamięci podręcznej, jeśli jest dostępna, niezależnie od tego, czy użytkownik ma dostęp do sieci, czy nowsza zawartość jest dostępna na serwerze.

Istnieją dwa powody, dla których jest to cenne:

* **Zapewnia niezawodność.** Dostęp do sieci nie jest stanem logicznym. Użytkownik nie jest po prostu "online" lub "offline". W rzeczywistości urządzenie użytkownika może uznać, że jest w trybie online, ale sieć może być zbyt mała, aby oczekiwać. Lub sieć może zwracać nieprawidłowe wyniki dla niektórych adresów URL, na przykład gdy istnieje Portal sieci Wi-Fi, który aktualnie blokuje lub przekierowuje określone żądania. Jest to dlatego, że interfejs API `navigator.onLine` przeglądarki nie jest niezawodny i nie powinien być zależny od tego, czy nie.
* **Zapewnia to poprawność.** Podczas kompilowania pamięci podręcznej zasobów w trybie offline, proces roboczy usługi używa tworzenia skrótów zawartości w celu zagwarantowania, że pobrano kompletną i spójną migawkę zasobów w jednym czasie. Ta pamięć podręczna jest następnie używana jako jednostka niepodzielna. W związku z tym nie ma żadnego punktu, aby uzyskać dostęp do sieci w celu uzyskania nowszych zasobów, ponieważ jedynymi wersjami są te, które zostały już zapisane w pamięci podręcznej. Wszelkie inne zagrożenia niespójnością i niezgodnością (na przykład próba użycia wersji zestawów .NET, które nie zostały skompilowane razem).

#### <a name="background-updates"></a>Aktualizacje w tle

Jako model psychiczny Możesz pomyśleć o tym, że w trybie offline — w pierwszej kolejności, jak w przypadku aplikacji mobilnej, która może być zainstalowana. Zawsze zaczyna się od razu, niezależnie od łączności sieciowej, ale zainstalowana logika aplikacji pochodzi z migawki do punktu w czasie, która może nie być najnowszą wersją.

Szablon Blazor PWA umożliwia tworzenie aplikacji, które automatycznie próbują zaktualizować się w tle zawsze, gdy użytkownik odwiedza i ma działające połączenie sieciowe. Oto jak to działa:

* Podczas kompilacji projekt generuje *manifest zasobów roboczych usługi*. Domyślnie jest to nazwa *Service-Worker-Assets. js*. Zawiera listę wszystkich zasobów statycznych, które aplikacja musi działać w trybie offline, takich jak zestawy .NET, pliki JavaScript, CSS itp., włącznie z ich skrótami zawartości. Ta lista jest załadowana przez proces roboczy usługi, aby uzyskać informację o tym, które zasoby mają być buforowane.
* Za każdym razem, gdy użytkownik odwiedzi aplikację, przeglądarka zażąda w tle *Service-Worker. js* i *Service-Worker-Assets. js* . Jeśli serwer zwróci zmienioną zawartość dla jednego z tych plików (w porównaniu bajtach w bajtach z istniejącym zainstalowanym pracownikiem usługi), proces roboczy usługi próbuje zainstalować nową wersję programu.
* Podczas instalowania nowej wersji, proces roboczy usługi tworzy nową, oddzielną pamięć podręczną dla zasobów w trybie offline i rozpoczyna zapełnianie jej zasobami wymienionymi w *Service-Worker-Assets. js*. Ta logika jest implementowana w funkcji `onInstall` w *Service-Worker. opublikowano. js*.
* Jeśli proces zakończy się pomyślnie (tj. wszystkie zasoby są ładowane bez błędów, a wszystkie wartości skrótów zawartości), nowy proces roboczy usługi przechodzi do stanu "Oczekiwanie na aktywację". Gdy tylko użytkownik zamknie aplikację (tj. nie ma żadnych kart lub okien aplikacji), nowy proces roboczy usługi zmieni się na "aktywny" i będzie używany do kolejnych wizyt w aplikacji. Stary proces roboczy usługi i jego pamięć podręczna są usuwane.
* Jeśli proces nie zakończy się pomyślnie, nowe wystąpienie procesu roboczego usługi zostanie odrzucone. Proces aktualizacji zostanie podjęty ponownie na następnym odwiedzeniu użytkownika, gdy miejmy nadzieję się z lepszym połączeniem sieciowym, które może zakończyć żądania.

Można dostosować dowolny aspekt tego procesu, edytując logikę procesu roboczego usługi. Żadne z powyższych elementów nie jest specyficzne dla Blazor, ale jest tylko sugestią podaną przez opcję szablonu PWA. Aby uzyskać więcej informacji, zobacz [dokumentację procesu roboczego usługi](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.) .

#### <a name="how-requests-are-resolved"></a>Jak są rozwiązywane żądania

Jak opisano powyżej, domyślny proces roboczy usługi korzysta z strategii dotyczącej *pamięci podręcznej* , co oznacza, że próbuje obsłużć zawartość buforowaną, jeśli jest dostępna. W przypadku braku zawartości przechowywanej w pamięci podręcznej dla określonego adresu URL, na przykład podczas żądania danych z interfejsu API zaplecza, proces roboczy usługi powraca do zwykłego żądania sieci, co może zakończyć się powodzeniem tylko wtedy, gdy serwer jest osiągalny. Ta logika jest implementowana w `onFetch` w ramach *Service-Worker. opublikowano. js*.

Jeśli składniki Blazor korzystają z żądania danych z interfejsów API zaplecza i chcesz zapewnić przyjazne środowisko użytkownika w przypadku, gdy takie żądania nie powiodą się z powodu niedostępności sieci, należy zaimplementować logikę w składnikach. Na przykład użyj `try/catch` wokół żądań `HttpClient`.

#### <a name="support-server-rendered-pages"></a>Obsługa stron renderowanych na serwerze

Zastanów się, co się stanie, gdy użytkownik najpierw nawiguje do adresu URL, takiego jak `/counter`, lub dowolnego innego linku bezpośredniego do aplikacji. W takich przypadkach nie chcesz zwracać zawartości w pamięci podręcznej jako `/counter`, ale zamiast tego należy użyć przeglądarki do załadowania zawartości w pamięci podręcznej jako `/index.html`, aby uruchomić aplikację Blazor webassembly. Te początkowe żądania są znane jako żądania *nawigacji* (w przeciwieństwie do żądań dotyczących *zasobów* dla obrazów/CSS/etc lub *pobierania/XHR* żądań dotyczących danych interfejsu API).

Domyślny proces roboczy usługi zawiera logikę przypadków specjalnych dla żądań nawigacji. Rozwiązuje je, zwracając zawartość z pamięci podręcznej dla `/index.html`niezależnie od żądanego adresu URL. Ta logika jest implementowana w funkcji `onFetch` w *Service-Worker. opublikowano. js*.

Jeśli aplikacja zawiera pewne adresy URL, które muszą zwracać kod HTML renderowany przez serwer (i nie obsługują `/index.html` z pamięci podręcznej), należy edytować logikę w procesie roboczym usługi. Na przykład, jeśli wszystkie adresy URL zawierające `/Identity/` muszą być obsługiwane jako regularne żądania tylko online do serwera, zmodyfikuj logikę `onFetch` *Service-Worker. opublikowano. js* . Znajdź następujący kod:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

Zmień kod na następujący:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

Jeśli tego nie zrobisz, proces roboczy usługi przechwytuje żądania dotyczące adresów URL i rozwiąże je przy użyciu `/index.html`.

#### <a name="control-asset-caching"></a>Sterowanie buforowaniem zasobów

Jeśli projekt definiuje Właściwość programu MSBuild o nazwie `ServiceWorkerAssetsManifest`, narzędzia kompilacji Blazorwygenerują manifest zasobów procesów roboczych usługi o podanej nazwie. Domyślny szablon PWA tworzy plik projektu zawierający następujące elementy:

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

Plik zostanie umieszczony w katalogu wyjściowym *wwwroot* , dzięki czemu przeglądarka może pobrać ten plik, żądając `/service-worker-assets.js`. Aby wyświetlić zawartość, Otwórz *YourProject\bin\Debug\netstandard2.1\wwwroot\service-Worker-Assets.js* w edytorze tekstu. Jednak nie należy edytować pliku, ponieważ zostanie on wygenerowany ponownie dla każdej kompilacji.

Domyślnie ten manifest zawiera następujące listę:

* Wszystkie zasoby zarządzane przez Blazor, takie jak zestawy .NET i pliki środowiska uruchomieniowego programu .NET webassembly, które są potrzebne do działania w trybie offline
* Wszystkie zasoby, które zostaną opublikowane w katalogu *wwwroot* , takie jak obrazy, pliki CSS i pliki JavaScript. Obejmuje to statyczne zasoby sieci Web dostarczane przez zewnętrzne projekty i pakiety NuGet.

Można kontrolować, które z tych zasobów będą pobierane i buforowane przez proces roboczy usługi przez edytowanie logiki w `onInstall` w *Service-Worker. opublikowano. js*. Domyślnie pobiera i buforuje pliki zgodne z typowymi rozszerzeniami nazw sieci Web, takimi jak *. html*, *. css*,. *js*, *. wasm*i innych, a także typy plików specyficzne dla Blazor webassembly ( *. dll*, *. pdb*).

Jeśli chcesz uwzględnić dodatkowe zasoby, które nie znajdują się w katalogu *wwwroot* , możesz to zrobić przez zdefiniowanie dodatkowych wpisów w ramach elementu MSBuild. Na przykład w pliku projektu Dodaj:

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

Metadane `AssetUrl` określają podstawowy adres URL, który powinien być używany przez przeglądarkę podczas pobierania zasobu do pamięci podręcznej. Może to być niezależna od oryginalnej nazwy pliku źródłowego na dysku.

> [!IMPORTANT]
> Dodanie `ServiceWorkerAssetsManifestItem` nie powoduje opublikowania pliku w katalogu *wwwroot* . Pozwala ona na kontrolowanie danych wyjściowych publikowanych osobno. `ServiceWorkerAssetsManifestItem` powoduje tylko dodatkowy wpis do wyświetlenia w manifeście zasobów procesu roboczego usługi.

## <a name="push-notifications"></a>Powiadomienia push

Podobnie jak w przypadku wszystkich innych aplikacji PWA, Blazor webassembly PWA może odbierać powiadomienia wypychane z serwera wewnętrznej bazy danych. Serwer może wysłać je w dowolnym momencie, nawet jeśli użytkownik nie korzysta aktywnie z aplikacji (na przykład gdy inny użytkownik wykonuje akcję, która może być istotna).

Mechanizm wysyłania powiadomień wypychanych jest całkowicie niezależny od Blazor webassembly, ponieważ jest zaimplementowany przez serwer wewnętrznej bazy danych, który może korzystać z dowolnej technologii. Jeśli chcesz wysyłać powiadomienia wypychane z serwera ASP.NET Core, rozważ [użycie techniki podobnej do tej w warsztatie niezwykle Pizza](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).

Mechanizm otrzymywania i wyświetlania powiadomień wypychanych na kliencie jest również niezależny od Blazor webassembly, ponieważ jest zaimplementowany w procesie roboczym usługi, który jest plikiem JavaScript. Na przykład możesz ponownie zobaczyć [podejście używane w warsztatie niezwykle Pizza](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).

## <a name="caveats-for-offline-pwas"></a>Ostrzeżenia dotyczące PWAs w trybie offline

Nie wszystkie aplikacje powinny próbować obsługiwać użycie w trybie offline. Zwiększa ona znaczącą złożoność, ale nie zawsze jest istotna.

Obsługa w trybie offline jest zwykle istotna:

* Jeśli podstawowy magazyn danych jest lokalny dla przeglądarki. Na przykład podczas kompilowania interfejsu użytkownika dla urządzenia [IoT](https://en.wikipedia.org/wiki/Internet_of_things) , które przechowuje dane w `localStorage` lub [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).

* Jeśli masz znaczącą służbę do pobierania i buforowania danych interfejsu API zaplecza związanych z każdym użytkownikiem, można poruszać się w trybie offline. W przypadku obsługi edycji należy również utworzyć system do śledzenia zmian i synchronizowania ich z zapleczem.

* Jeśli celem jest zagwarantowanie, że aplikacja zostanie załadowana natychmiast niezależnie od warunków sieci. Następnie należy zaimplementować odpowiednie środowisko użytkownika dla żądań interfejsu API zaplecza, aby pokazać postęp żądań i zachować wrażenie niepowodzenia z powodu niedostępności sieci.

Ponadto PWAs z obsługą trybu offline musi zająć się zakresem dodatkowych komplikacji. Deweloperzy powinni uważnie zapoznać się z poniższymi zastrzeżeniami.

### <a name="offline-support-only-when-published"></a>Obsługa offline tylko po opublikowaniu

szablon aplikacji PWA Blazorumożliwia obsługę offline tylko po opublikowaniu. Jest to spowodowane tym, że podczas opracowywania zwykle chcesz zobaczyć, że zmiany zostaną odzwierciedlone natychmiast w przeglądarce, bez przechodzenia przez proces aktualizacji w tle.

W związku z tym podczas kompilowania aplikacji z obsługą trybu offline jest za mało, aby przetestować aplikację w trybie tworzenia. Należy przetestować aplikację w stanie opublikowanym, aby zrozumieć, jak będzie ona odpowiadać na różne warunki sieci.

### <a name="update-completion-after-user-navigation-away-from-app"></a>Aktualizowanie uzupełniania po nawigacji użytkownika z aplikacji

Aktualizacje nie zostaną ukończone, dopóki użytkownik nie wyjdzie z aplikacji na wszystkich kartach. Zgodnie z opisem w temacie [aktualizacje w tle](#background-updates), po wdrożeniu aktualizacji dla aplikacji Przeglądarka pobierze zaktualizowane pliki procesu roboczego usługi i rozpocznie proces aktualizacji.

Co się stało z wieloma deweloperami, nawet po zakończeniu tej aktualizacji **nie zacznie obowiązywać** , dopóki użytkownik nie przejdzie na wszystkie karty. Odświeżenie karty wyświetlającej aplikację **nie** jest wystarczające, nawet jeśli jest to jedyna karta wyświetlająca aplikację. Do momentu całkowitego zamknięcia aplikacji nowy proces roboczy usługi pozostanie w stanie "Oczekiwanie na aktywację". **Nie jest to specyficzne dla Blazor, ale raczej jest standardowe zachowanie platformy sieci Web.**

Jest to często spowodowane przez deweloperów, którzy próbują przetestować aktualizacje do swoich procesów roboczych lub zasobów w pamięci podręcznej w trybie offline. W przypadku zaewidencjonowania narzędzi deweloperskich w przeglądarce można zobaczyć podobne do następujących:

![obraz](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

W przypadku, gdy lista "klientów" (tj. kart lub okien wyświetlania aplikacji) nie jest pusta, proces roboczy będzie kontynuował oczekiwanie. Przyczyną tego problemu jest zagwarantowanie spójności, tj., że wszystkie zasoby są pobierane z tej samej niepodzielnej pamięci podręcznej.

Podczas testowania zmian może być przydatne kliknięcie linku "skipWaiting", jak pokazano na poniższym zrzucie ekranu, a następnie ponowne załadowanie strony. Jeśli chcesz, Możesz zautomatyzować ten proces dla wszystkich użytkowników, napisanie przez siebie procesu roboczego usługi w celu [pominięcia fazy "oczekiwanie" i natychmiastowej aktywacji przy aktualizacji](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase). Jeśli jednak to zrobisz, nastąpi zagwarantowanie, że zasoby są zawsze pobierane spójnie z tego samego wystąpienia pamięci podręcznej.

### <a name="users-may-run-any-historical-version-of-the-app"></a>Użytkownicy mogą uruchamiać dowolną wersję historyczną aplikacji

Deweloperzy sieci Web zwykle oczekują, że użytkownicy będą uruchamiać tylko najnowszą wdrożoną wersję aplikacji sieci Web, ponieważ jest ona normalna w ramach tradycyjnego modelu dystrybucji sieci Web. Jednak w trybie offline — w pierwszej kolejności program PWA jest bardziej zbliżone w natywnej aplikacji mobilnej, w której użytkownicy nie muszą uruchamiać najnowszej wersji.

Zgodnie z opisem w temacie [aktualizacje w tle](#background-updates), po wdrożeniu aktualizacji aplikacji, **każdy istniejący użytkownik będzie nadal używał poprzedniej wersji dla co najmniej jednej kolejnej odwiedzania** (ponieważ aktualizacja odbywa się w tle i nie jest aktywowana, dopóki użytkownik nie wyjdzie dalej). Ponadto Poprzednia wersja jest wcześniejsza niż poprzednio wdrożona — może to być *dowolna* wersja historyczna, w zależności od tego, kiedy użytkownik ostatnio ukończył aktualizację.

Może to być problemem, jeśli składniki frontonu i zaplecza aplikacji wymagają umowy dotyczącej schematu dla żądań interfejsu API. Nie należy wdrażać zmian schematu interfejsu API bez zgodności z poprzednimi wersjami, dopóki nie będzie można upewnić się, że wszyscy użytkownicy zostali zaktualizowani, lub co najmniej Zablokuj użytkownikom możliwość korzystania ze starszych wersji aplikacji. Jest to podobne do natywnej aplikacji mobilnej. W przypadku wdrożenia istotnej zmiany w interfejsach API serwera aplikacja kliencka zostanie uszkodzona dla osób, które nie zostały jeszcze zaktualizowane.

Jeśli to możliwe, nie Wdrażaj istotnych zmian w interfejsach API zaplecza. Ale jeśli to konieczne, należy rozważyć użycie [standardowych interfejsów API procesów roboczych, takich jak `ServiceWorkerRegistration`](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) , aby określić, czy aplikacja jest aktualna, a jeśli nie, aby zapobiec użyciu.

### <a name="interference-with-server-rendered-pages"></a>Zakłócenia przy użyciu stron renderowanych na serwerze

[Jak opisano powyżej](#support-server-rendered-pages), jeśli chcesz ominąć zachowanie `/index.html` zawartości dla wszystkich żądań nawigacji przez proces roboczy usługi, musisz edytować logikę w procesie roboczym usługi.

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>Zawartość manifestu wszystkich zasobów procesu roboczego usługi jest domyślnie buforowana

[Jak opisano powyżej](#control-asset-caching), plik *Service-Worker-Assets. js* jest generowany podczas kompilacji i zawiera listę wszystkich zasobów, które pracownik usług powinien pobrać i buforować.

Ponieważ ta lista domyślnie zawiera wszystkie dane wyemitowane do *wwwroot* (w tym zawartość dostarczaną przez zewnętrzne pakiety i projekty), należy uważać, aby nie umieścić zbyt dużej ilości zawartości. Jeśli na przykład katalog *wwwroot* zawiera miliony obrazów, proces roboczy usługi spróbuje pobrać i buforować je wszystkie, zużywać nadmierną przepustowość i prawdopodobnie nie zostanie pomyślnie zakończony.

Możliwe jest zaimplementowanie dowolnej logiki w celu kontrolowania, który podzbiór zawartości manifestu powinien być pobierany i buforowany przez edycję funkcji `onInstall` w *Service-Worker. opublikowano. js*.

### <a name="interaction-with-authentication"></a>Interakcja z uwierzytelnianiem

Możliwe jest użycie opcji szablonu PWA w połączeniu z opcjami uwierzytelniania. PWA obsługujący tryb offline może również obsługiwać uwierzytelnianie, gdy użytkownik ma łączność sieciową.

Jeśli jednak użytkownik nie ma łączności sieciowej, nie będzie mógł uwierzytelnić ani uzyskać tokenów dostępu. Próba odwiedzenia strony "login" spowoduje wyświetlenie komunikatu o błędzie "błąd sieci".

Ponieważ jest to zadanie, aby zaprojektować przepływ interfejsu użytkownika, który umożliwia użytkownikowi wykonywanie użytecznych funkcji podczas pracy w trybie offline bez próby uwierzytelnienia lub uzyskania tokenów dostępu, lub co najmniej niepowodzeniem w takich przypadkach. Jeśli nie jest to możliwe w aplikacji, może nie być konieczne włączenie obsługi offline.
