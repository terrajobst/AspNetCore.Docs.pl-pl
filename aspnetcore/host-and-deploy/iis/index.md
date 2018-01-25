---
title: Host platformy ASP.NET Core w systemie Windows z programem IIS
author: guardrex
description: "Dowiedz się, jak udostępniać aplikacje platformy ASP.NET Core na systemu Windows Server Internet informacji usług (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host platformy ASP.NET Core w systemie Windows z programem IIS

Przez [Luke Latham](https://github.com/guardrex) i [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Supported operating systems

Obsługiwane są następujące systemy operacyjne:

* Windows 7 i nowsze
* Windows Server 2008 R2 lub nowszym &#8224;

&#8224; Koncepcyjnie, konfiguracji usług IIS, w tym dokumencie opisano odnosi się także do obsługi aplikacji platformy ASP.NET Core na serwerze Nano Server IIS, ale dotyczą [platformy ASP.NET Core z usługami IIS na serwerze Nano](xref:tutorials/nano-server) Aby uzyskać szczegółowe instrukcje.

[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)) nie będzie działać w konfiguracji zwrotny serwer proxy z usługami IIS. Użyj [serwera Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Konfiguracja usług IIS

Włącz **serwer sieci Web (IIS)** roli i ustanowienia usług ról.

### <a name="windows-desktop-operating-systems"></a>Systemy operacyjne Windows

Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu). Otwórz grupę, do **Internetowe usługi informacyjne** i **narzędzia zarządzania siecią Web**. Pole wyboru dla **Konsola zarządzania usługami IIS**. Pole wyboru dla **usługi sieci World Wide Web**. Zaakceptuj domyślne funkcje dla **usługi sieci World Wide Web** lub dostosować funkcje usług IIS.

![Konsola zarządzania usługami IIS i usługi sieci World Wide Web są zaznaczone w funkcji systemu Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Systemy operacyjne Windows Server

Dla systemów operacyjnych serwera, użyj **Dodaj role i funkcje** za pomocą kreatora **Zarządzaj** menu lub link w **Menedżera serwera**. Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)**.

![W kroku role serwera wybierz wybrano roli Serwer sieci Web IIS.](index/_static/server-roles-ws2016.png)

Na **usług ról** krok, wybierz usługi ról usług IIS konieczne lub zaakceptuj domyślną rolę usług pod warunkiem.

![Domyślne usługi ról są zaznaczone w kroku wybierz rolę usług.](index/_static/role-services-ws2016.png)

Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli serwera sieci web i usług. Ponowne uruchomienie serwera i IIS nie jest wymagane po zainstalowaniu roli serwera sieci Web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalacja pakietu Hosting .NET Core systemu Windows Server

1. Zainstaluj [pakietu .NET Core systemu Windows serwer obsługujący](https://aka.ms/dotnetcore-2-windowshosting) przez system operacyjny. Pakiet instaluje wykonawczym .NET Core .NET Core biblioteki, a [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel serwera. Jeśli system nie ma połączenia internetowego, Uzyskaj i zainstaluj [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu Hosting .NET Core systemu Windows Server.

2. Ponowne uruchamianie systemu lub wykonać **net stop została /y** następuje **net start w3svc** z wiersza polecenia, aby pobrać zmiany systemowej PATH.

> [!NOTE]
> Aby uzyskać informacje o konfiguracji udostępnionej usług IIS, zobacz [platformy ASP.NET Core modułu z konfiguracji udostępnionej usług IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Zainstaluj pakiet Webdeploy podczas publikowania z programem Visual Studio

W przypadku wdrażania aplikacji na serwerach z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), zainstaluj najnowszą wersję narzędzia Web Deploy na serwerze. Aby zainstalować narzędzie Web Deploy, użyj [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać Instalatora bezpośrednio z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Preferowaną metodą jest używać WebPI. WebPI oferuje autonomicznego Instalatora i konfiguracja dla dostawców hostingu.

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="enabling-the-iisintegration-components"></a>Włączanie składników IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) aby rozpocząć konfigurowanie hosta. `CreateDefaultBuilder`Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako sieci web Integracja serwera i umożliwia usług IIS przez skonfigurowanie ścieżki podstawowej i port [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Obejmują zależności na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakietu w zależnościach aplikacji. Włączenie oprogramowanie pośredniczące integracji usług IIS do aplikacji przez dodanie *UseIISIntegration* metodę rozszerzenie *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Zarówno `UseKestrel` i `UseIISIntegration` są wymagane. Wywoływanie kodu `UseIISIntegration` nie ma wpływu na przenośność kodu. Jeśli aplikacja nie jest uruchamiana za usług IIS (na przykład aplikacja jest uruchamiana bezpośrednio na Kestrel), `UseIISIntegration` ops nie.

---

Aby uzyskać więcej informacji dotyczących obsługi, zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Opcje usług IIS

Aby skonfigurować opcje usług IIS, obejmują konfigurację usługi `IISOptions` w `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, ustawia oprogramowanie pośredniczące uwierzytelniania `HttpContext.User` i sprostać wymaganiom ogólnego. Jeśli `false`, oprogramowanie pośredniczące uwierzytelniania zapewnia tylko tożsamości (`HttpContext.User`) i sprostać wymaganiom jawnie żądanie `AuthenticationScheme`. Należy włączyć uwierzytelnianie systemu Windows w usługach IIS dla `AutomaticAuthentication` funkcji. |
| `AuthenticationDisplayName`    | `null`  | Ustawia nazwę wyświetlaną pokazywana użytkownikom na stronach logowania. |
| `ForwardClientCertificate`     | `true`  | Jeśli `true` i `MS-ASPNETCORE-CLIENTCERT` nagłówek żądania jest obecny, `HttpContext.Connection.ClientCertificate` jest wypełnione. |

### <a name="webconfig"></a>web.config

*Web.config* tego pliku podstawowego służy do konfigurowania [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). Opcjonalnie ona dodatkowych ustawień konfiguracji usług IIS. Tworzenie, przekształcanie i publikowanie *web.config* jest obsługiwany przez zestaw SDK programu .NET Core sieci Web (`Microsoft.NET.Sdk.Web`). Zestaw SDK jest ustawiony na początku pliku projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`. Aby zapobiec przekształcania zestawu SDK *web.config* plików, dodawanie  **\<IsTransformWebConfigDisabled >** właściwości do pliku projektu z ustawieniem `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Jeśli *web.config* plik znajduje się w projekcie, jest przekształcana z prawidłowym *processPath* i *argumenty* skonfigurować [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i przenieść do [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure). Transformacja nie zmodyfikować ustawień konfiguracji usług IIS w pliku.

### <a name="webconfig-location"></a>web.config location

Aplikacje .NET core są obsługiwane przez zwrotny serwer proxy między usługami IIS a Kestrel serwera. Aby można było utworzyć zwrotny serwer proxy, *web.config* plik musi znajdować się w ścieżce zawartości katalogu głównego (zazwyczaj ścieżki podstawowej aplikacji) wdrożonej aplikacji, to ścieżka fizyczna witryny sieci Web do usług IIS. *Web.config* plik jest wymagany w katalogu głównym aplikacji, aby umożliwić publikowanie wielu aplikacji za pomocą narzędzia Web Deploy.

Poufne pliki istnieją na ścieżkę fizyczną aplikacji, w tym podfolderów, takich jak  *\<nazwa_zestawu >. runtimeconfig.json*,  *\<nazwa_zestawu > .xml* (XML Komentarze dokumentacji) i  *\<nazwa_zestawu >. deps.json*. Gdy *web.config* plik istnieje i skonfiguruje lokację, usług IIS zapobiega obsługiwanej przez te poufnych plików. **W związku z tym ważne jest który *web.config* plik nie jest ani przypadkowo, zmieniono jego nazwę lub usunięte z wdrożenia.**

## <a name="create-the-iis-website"></a>Utwórz witrynę sieci Web usług IIS

1. W celu systemu usług IIS, należy utworzyć folder do zawierają aplikacji opublikowanych pliki i foldery, które są opisane w [struktury katalogów](xref:host-and-deploy/directory-structure).

2. W folderze, Utwórz *dzienniki* folder do przechowywania dzienników stdout, gdy jest włączone rejestrowanie stdout. Jeśli aplikacja jest wdrażana z *dzienniki* folderu w ładunku, Pomiń ten krok. Aby uzyskać instrukcje dotyczące sposobu wprowadzania MSBuild utworzyć *dzienniki* folderu, zobacz [struktury katalogów](xref:host-and-deploy/directory-structure) tematu.

3. W **Menedżera usług IIS**, Utwórz nową witrynę sieci Web. Podaj **nazwa witryny** i ustaw **ścieżka fizyczna** do folderu wdrożenia aplikacji. Podaj **powiązanie** konfigurację i tworzenie witryny sieci Web.

4. Skonfiguruj pulę aplikacji **bez kodu zarządzanego**. Platformy ASP.NET Core działa w oddzielnych procesach i zarządza środowiska uruchomieniowego.

5. Otwórz **Dodawanie witryny sieci Web** okna.

   ![Wybierz "Dodawanie witryny sieci Web" z menu kontekstowe witryny.](index/_static/add-website-context-menu-ws2016.png)

6. Konfigurowanie witryny sieci Web.

   ![Podaj nazwę lokacji, ścieżka fizyczna i nazwy hosta w kroku Dodawanie witryny sieci Web.](index/_static/add-website-ws2016.png)

7. W **pul aplikacji** panelu, otwórz **edytowanie puli aplikacji** okno prawym przyciskiem myszy w puli aplikacji witryny sieci Web i wybierając **podstawowych ustawień...**  w menu podręcznym.

   ![Wybierz ustawienia podstawowe z menu kontekstowego puli aplikacji.](index/_static/apppools-basic-settings-ws2016.png)

8. Ustaw **wersja środowiska .NET CLR** do **bez kodu zarządzanego**.

   ![Ustaw żadnego kodu zarządzanego dla wersji środowiska CLR programu .NET.](index/_static/edit-apppool-ws2016.png)
     
    Uwaga: Ustawienie **wersja środowiska .NET CLR** do **bez kodu zarządzanego** jest opcjonalna. Platformy ASP.NET Core nie zależą od ładowania CLR pulpitu.

9. Upewnij się, że tożsamość procesu modelu ma odpowiednie uprawnienia.

   Jeśli domyślna tożsamość puli aplikacji (**Model procesu** > **tożsamości**) została zmieniona z **puli** tożsamość, sprawdź, czy nową tożsamość ma wymagane uprawnienia dostępu do folderu instalacji aplikacji, bazy danych i innych wymaganych zasobów.
   
## <a name="deploy-the-app"></a>Wdrażanie aplikacji

Wdróż aplikację na folder utworzony w celu systemu usług IIS. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) to mechanizm zalecane dla wdrożenia.

Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony. Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona. Nie można zastąpić zablokowane pliki. Aby zwolnić zablokowane pliki we wdrożeniu, zatrzymać puli aplikacji:

* Ręcznie w Menedżerze usług IIS na serwerze.
* Za pomocą narzędzia Web Deploy i odwołuje się do `Microsoft.NET.Sdk.Web` w pliku projektu. *App_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web. Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania. Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
* Zatrzymanie i ponowne uruchomienie puli aplikacji (wymaga programu PowerShell, 5 lub nowszy) przy użyciu programu PowerShell:
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Narzędzie Web Deploy w programie Visual Studio

Zobacz [profilów publikowania programu Visual Studio dla wdrożenia aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użytku z narzędzia Web Deploy. Jeśli dostawca hostingu dostarcza profil publikowania lub obsługę utworzenie, Pobierz swój profil i zaimportuj go za pomocą programu Visual Studio **publikowania** okna dialogowego.

![Okno dialogowe strony publikacji](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Narzędzie Web Deploy poza programu Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) można również poza Visual Studio z wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Wdrażanie rozwiązań alternatywnych w sieci Web

Przenoszenie aplikacji w systemie hostingu, na przykład Xcopy, Robocopy lub programu PowerShell przy użyciu jednej z kilku metod. Visual Studio użytkownicy mogą skorzystać z [przykłady publikowania](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Przeglądaj witryny sieci Web

![Przeglądarka Microsoft Edge załadował strony początkowej usług IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Ochrona danych

Ochrona danych jest używana przez kilka middlewares ASP.NET, w tym używane podczas uwierzytelniania. Nawet jeśli interfejsy API ochrony danych nie jest wywoływany z kodu użytkownika, ochrony danych należy skonfigurować przy użyciu skryptu wdrożenia lub w kodzie użytkownika, aby utworzyć trwały magazynu kluczy. Jeśli nie jest skonfigurowany do ochrony danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli zestaw kluczy są przechowywane w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny uwierzytelniania formularzy zostały unieważnione. 
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania. 
* Wszystkie dane chronione za pomocą zestaw kluczy nie mogły być odszyfrowane.

Aby skonfigurować ochronę danych w środowisku usług IIS, należy użyć **jeden** z następujących metod:

### <a name="create-a-data-protection-registry-hive"></a>Utwórz gałąź rejestru ochrony danych

Dane ochrony kluczy używanych przez aplikacje ASP.NET są przechowywane w gałęzi rejestru zewnętrzne do aplikacji. Aby zachować kluczy dla danej aplikacji, utwórz gałąź rejestru dla puli aplikacji.

Dla autonomicznej, w przypadku instalacji usług IIS z systemem innym niż farma sieci Web [skrypt programu PowerShell należy AutoGenKeys.ps1 ochrony danych](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) można używać dla każdej puli aplikacji używane w aplikacji platformy ASP.NET Core. Ten skrypt tworzy specjalnego klucza rejestru HKLM, które ma ACLed tylko konta procesu roboczego. Klucze są szyfrowane, gdy przy użyciu DPAPI za pomocą klucza komputera.

W scenariuszach kolektywu serwerów sieci web aplikację można skonfigurować do użyć ścieżki UNC do przechowywania jego zestaw kluczy ochrony danych. Domyślnie klucze ochrony danych nie są szyfrowane. Upewnij się, że uprawnienia udziału takie jest ograniczone do uruchomieniu aplikacji jako konto systemu Windows. Ponadto X509 certyfikatu może służyć do ochrony kluczy w stanie spoczynku. Należy wziąć pod uwagę mechanizm Zezwalaj użytkownikom na przekazywanie certyfikaty: miejsce certyfikatów do zaufanego certyfikatu użytkownika przechowywania i upewnij się, są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika. Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) szczegółowe informacje.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Konfigurowanie puli aplikacji usług IIS do załadowania profilu użytkownika

To ustawienie jest **Model procesu** w obszarze **Zaawansowane ustawienia** dla puli aplikacji. Ustaw Załaduj profil użytkownika `True`. To są przechowywane klucze w katalogu profilu użytkownika i ich ochrony za pomocą DPAPI kluczem określone konto użytkownika używane w puli aplikacji.

### <a name="use-the-file-system-as-a-key-ring-store"></a>System plików jako pierścień klucza magazynu

Kod aplikacji, aby dopasować [system plików jako magazyn pierścień klucza](xref:security/data-protection/configuration/overview). Użyj X509 certyfikatu, aby chronić zestaw kluczy i certyfikat zaufanego certyfikatu. Jeśli certyfikat z podpisem własnym, należy umieścić certyfikatu w magazynie zaufanych głównych.

Korzystając z usług IIS w farmie sieci web:

* Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich maszyn.
* Wdrażanie X509 certyfikatów na każdym komputerze. Skonfiguruj [ochrony danych w kodzie](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Ustawienie zasad komputera w przypadku ochrony danych

System ochrony danych ma ograniczoną obsługę ustawienie domyślne [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji, które korzystanie z interfejsów API ochrony danych. Zobacz [ochrony danych](xref:security/data-protection/index) dokumentację, aby uzyskać więcej szczegółowych informacji.

## <a name="configuration-of-sub-applications"></a>Konfiguracja aplikacji podrzędne

Aplikacje podrzędne dodany w obszarze katalogu głównego aplikacji nie powinna zawierać moduł platformy ASP.NET Core jako program obsługi. Jeśli moduł jest dodawany jako program obsługi w sub-app *web.config* pliku 500.19 (wewnętrzny błąd serwera) odwołuje się do pliku konfiguracji błędny odebraniu podczas próby przeglądania aplikacji sub. W poniższym przykładzie przedstawiono zawartości opublikowanej *web.config* pliku dla platformy ASP.NET Core sub aplikacji:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Odnośnie do hostowania - platformy ASP.NET Core sub aplikacji w kontenerze aplikacji platformy ASP.NET Core, jawnie usunąć dziedziczonej obsługi w aplikacji sub *web.config* pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji na temat konfigurowania moduł platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) tematu i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguracja usług IIS z pliku web.config

Konfiguracja usług IIS ma wpływ  **\<system.webServer >** sekcji *web.config* dla tych funkcji usług IIS, które mają zastosowanie do konfiguracji zwrotnego serwera proxy. Skonfigurowanie usług IIS na poziomie systemu, aby użyć kompresji dynamicznej to ustawienie zostanie wyłączone dla aplikacji za pomocą  **\<urlCompression >** element w aplikacji *web.config* pliku. Aby uzyskać więcej informacji, zobacz [konfiguracji odwołania dla \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i [przy użyciu modułów usług IIS w aplikacji ASP. Podstawowe NET](xref:host-and-deploy/iis/modules). Jeśli trzeba ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w pulach aplikacji izolowanych (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentacji odwołania tematu w usługach IIS.

## <a name="configuration-sections-of-webconfig"></a>Sekcji konfiguracyjnych w pliku Web.config

Sekcje Configruation aplikacji struktury ASP.NET w *web.config* nie są używane przez aplikacje platformy ASP.NET Core dla konfiguracji:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<Lokalizacja >**

Aplikacje platformy ASP.NET Core są skonfigurowane przy użyciu innych dostawców konfiguracji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pule aplikacji

Odnośnie do hostowania wielu witryn sieci Web na jednym komputerze, izolowania aplikacji od siebie, uruchamiając każdej aplikacji w odrębnej puli aplikacji. Usługi IIS **Dodawanie witryny sieci Web** domyślnie okno dialogowe tej konfiguracji. Gdy **nazwa witryny** została podana, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego. Jest tworzona nowa pula aplikacji przy użyciu nazwy lokacji po dodaniu witryny sieci Web.

## <a name="application-pool-identity"></a>Tożsamość puli aplikacji

Konto tożsamości puli aplikacji dzięki czemu aplikacja może działać w ramach unikatowe konto bez konieczności tworzenia i zarządzania nimi domeny lub lokalnego konta. W programie IIS 8.0 i nowsze proces roboczy administratora usług IIS (WAS) tworzy konto wirtualny o nazwie nowej puli aplikacji i aplikacji w puli procesów roboczych na tym koncie są domyślnie uruchamiane. W konsoli zarządzania usług IIS w obszarze **Zaawansowane ustawienia** dla puli aplikacji, upewnij się, że **tożsamości** jest skonfigurowany do używania **puli**:

![Okno dialogowe Zaawansowane ustawienia puli aplikacji](index/_static/apppool-identity.png)

Proces zarządzania usług IIS tworzy bezpiecznego identyfikatora o nazwie puli aplikacji w systemie zabezpieczeń systemu Windows. Zasoby mogą być chronione przy użyciu tej tożsamości; Jednak ta tożsamość nie jest rzeczywistego konta użytkownika i nie będą widoczne w konsoli zarządzania użytkownika systemu Windows.

Jeśli proces roboczy usług IIS wymaga podwyższonego poziomu dostępu do aplikacji, należy zmodyfikować listy kontroli dostępu (ACL) dla katalogu zawierającej aplikację:

1. Otwórz Eksploratora Windows i przejdź do katalogu.

2. Kliknij prawym przyciskiem myszy na katalog i wybierz **właściwości**.

3. W obszarze **zabezpieczeń** wybierz opcję **Edytuj** przycisk, a następnie **Dodaj** przycisku.

4. Wybierz **lokalizacje** przycisk i upewnij się, że system jest zaznaczone.

5. Wprowadź **IIS AppPool\DefaultAppPool** w **wprowadź nazwy obiektów do wybrania** pola tekstowego.

   ![Wybierz użytkowników lub grup okno folderu aplikacji](index/_static/select-users-or-groups-1.png)

6. Wybierz **Sprawdź nazwy** przycisku. Wybierz **OK**.

   ![Wybierz użytkowników lub grup okno folderu aplikacji](index/_static/select-users-or-groups-2.png)

Ponadto można to zrobić za pomocą wiersza polecenia przy użyciu **ICACLS** narzędzie:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot)
* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Używanie modułów usług IIS z platformy ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Wprowadzenie do platformy ASP.NET Core](../index.md)
* [Witryna oficjalnego Microsoft IIS](https://www.iis.net/)
* [Bibliotece Microsoft TechNet: Serwer systemu Windows](https://docs.microsoft.com/windows-server/windows-server-versions)
