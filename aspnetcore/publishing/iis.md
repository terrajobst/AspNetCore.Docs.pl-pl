---
title: Host platformy ASP.NET Core w systemie Windows z programem IIS
author: guardrex
description: "Konfiguracja systemu Windows Server Internet informacji usług (IIS) i wdrażanie aplikacji platformy ASP.NET Core."
keywords: "Wdrażanie platformy ASP.NET Core internetowych usług informacyjnych, usługi iis, systemu windows server, hostingu moduł core bundle,asp.net, sieci web"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 7eb1537df47fcf0b24db2a7d843b655a6f6f8f21
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host platformy ASP.NET Core w systemie Windows z programem IIS

Przez [Luke Latham](https://github.com/guardrex) i [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

Obsługiwane są następujące systemy operacyjne:

* Windows 7 i nowsze
* Windows Server 2008 R2 lub nowszym &#8224;

&#8224; Koncepcyjnie, konfiguracji usług IIS, w tym dokumencie opisano odnosi się także do obsługi aplikacji platformy ASP.NET Core na serwerze Nano Server IIS, ale dotyczą [platformy ASP.NET Core z usługami IIS na serwerze Nano](xref:tutorials/nano-server) Aby uzyskać szczegółowe instrukcje.

[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)) nie będzie działać w konfiguracji serwera proxy wstecznego z usługami IIS. Należy użyć [serwera Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Konfiguracja usług IIS

Włącz **serwer sieci Web (IIS)** roli i ustanowienia usług ról.

### <a name="windows-desktop-operating-systems"></a>Systemy operacyjne Windows

Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu). Otwórz grupę, do **Internetowe usługi informacyjne** i **narzędzia zarządzania siecią Web**. Pole wyboru dla **Konsola zarządzania usługami IIS**. Pole wyboru dla **usługi sieci World Wide Web**. Zaakceptuj domyślne funkcje dla **usługi sieci World Wide Web** lub dostosować funkcje usług IIS, w zależności od potrzeb.

![Konsola zarządzania usługami IIS i usługi sieci World Wide Web są zaznaczone w funkcji systemu Windows.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Systemy operacyjne Windows Server

Dla systemów operacyjnych serwera, użyj **Dodaj role i funkcje** za pomocą kreatora **Zarządzaj** menu lub link w **Menedżera serwera**. Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)**.

![W kroku role serwera wybierz wybrano roli Serwer sieci Web IIS.](iis/_static/server-roles-ws2016.png)

Na **usług ról** kroku, wybierz usługi ról usług IIS, konieczna jest lub zaakceptuj wartość domyślną rolę usług pod warunkiem.

![Domyślne usługi ról są zaznaczone w kroku wybierz rolę usług.](iis/_static/role-services-ws2016.png)

Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli serwera sieci web i usług. Ponowne uruchomienie serwera i IIS nie jest wymagane po zainstalowaniu roli serwera sieci Web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalacja pakietu Hosting .NET Core systemu Windows Server

1. Zainstaluj [pakietu .NET Core systemu Windows serwer obsługujący](https://aka.ms/dotnetcore-2-windowshosting) przez system operacyjny. Pakiet instaluje wykonawczym .NET Core .NET Core biblioteki, a [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). Moduł tworzy wstecznego serwera proxy między usługami IIS a Kestrel serwera. Jeśli system nie ma połączenia internetowego, Uzyskaj i zainstaluj [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu Hosting .NET Core systemu Windows Server.

2. Ponowne uruchamianie systemu lub wykonać **net stop została /y** następuje **net start w3svc** z wiersza polecenia, aby pobrać zmiany systemowej PATH.

> [!NOTE]
> Jeśli używasz konfiguracji udostępnionej usług IIS, zobacz [platformy ASP.NET Core modułu z konfiguracji udostępnionej usług IIS](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Zainstaluj pakiet Webdeploy podczas publikowania z programem Visual Studio

Jeśli zamierzasz wdrożyć aplikacji z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) w [programu Visual Studio](https://www.visualstudio.com/vs/), zainstaluj najnowszą wersję narzędzia Web Deploy przez system operacyjny. Aby zainstalować narzędzie Web Deploy, można użyć [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać Instalatora bezpośrednio z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Preferowaną metodą jest używać WebPI. WebPI oferuje autonomicznego Instalatora i konfiguracja dla dostawców hostingu.

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="enabling-the-iisintegration-components"></a>Włączanie składników IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) aby rozpocząć konfigurowanie hosta. `CreateDefaultBuilder`Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako sieci web Integracja serwera i umożliwia usług IIS przez skonfigurowanie ścieżki podstawowej i port [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Obejmują zależności na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakiet w zależności aplikacji. Włączenie oprogramowanie pośredniczące integracji usług IIS do aplikacji przez dodanie *UseIISIntegration* metodę rozszerzenie *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Zarówno `UseKestrel` i `UseIISIntegration` są wymagane. Wywoływanie kodu *UseIISIntegration* nie ma wpływu na przenośność kodu. Jeśli aplikacja nie jest uruchamiana za usług IIS (na przykład aplikacja jest uruchamiana bezpośrednio na Kestrel), `UseIISIntegration` ops nie.

---

Aby uzyskać więcej informacji dotyczących obsługi, zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Opcje usług IIS

Aby skonfigurować *IISIntegration* usługi opcje, obejmują konfigurację usługi *IISOptions* w *ConfigureServices*:

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

### <a name="webconfig"></a>plik Web.config

*Web.config* pliku głównie konfiguruje moduł platformy ASP.NET Core. Opcjonalnie ona dodatkowych ustawień konfiguracji usług IIS. Tworzenie, przekształcanie i publikowanie *web.config* jest obsługiwany przez zestaw SDK programu .NET Core sieci Web (`Microsoft.NET.Sdk.Web`). Zestaw SDK jest ustawiony na początku pliku projektu (*.csproj*), `<Project Sdk="Microsoft.NET.Sdk.Web">`. Aby zapobiec przekształcania zestawu SDK *web.config* plików, dodawanie  **\<IsTransformWebConfigDisabled >** właściwości do pliku projektu z ustawieniem `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Jeśli nie masz *web.config* plik w projekcie publikowania z *publikowania dotnet* lub z programem Visual Studio publikowania, plik jest tworzony w [opublikowane dane wyjściowe](xref:hosting/directory-structure). Jeśli masz *web.config* pliku w projekcie, jest on przekształcany z prawidłowym *processPath* i *argumenty* skonfigurować [platformy ASP.NET Core modułu ](xref:fundamentals/servers/aspnet-core-module) i przenieść do publikowanych danych wyjściowych. Transformacja nie touch ustawień konfiguracji usług IIS, które zostały uwzględnione w pliku.

## <a name="create-the-iis-website"></a>Utwórz witrynę sieci Web usług IIS

1. W celu systemu usług IIS, należy utworzyć folder do zawierają aplikacji opublikowanych pliki i foldery, które są opisane w [struktury katalogów](xref:hosting/directory-structure).

2. W utworzonym folderze utwórz *dzienniki* folder do przechowywania dzienników stdout (Jeśli zamierzasz włączyć rejestrowanie w celu rozwiązywania problemów rozruchu). Jeśli planujesz wdrożyć aplikację z *dzienniki* folderu w ładunku, możesz pominąć ten krok. Brak [Otwórz problem, można automatycznie utworzyć folderu](https://github.com/aspnet/AspNetCoreModule/issues/30). Jeśli chcesz MSBuild, aby utworzyć *dziennika* folderu, Dodaj następujący `Target` do pliku projektu:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. W **Menedżera usług IIS**, Utwórz nową witrynę sieci Web. Podaj **nazwa witryny** i ustaw **ścieżka fizyczna** do utworzonego folderu wdrożenia aplikacji. Podaj **powiązanie** konfigurację i tworzenie witryny sieci Web.

4. Skonfiguruj pulę aplikacji **bez kodu zarządzanego**. Platformy ASP.NET Core działa w oddzielnych procesach i zarządza środowiska uruchomieniowego.

5. Otwórz **Dodawanie witryny sieci Web** okna.

   ![Kliknij przycisk Dodaj witrynę sieci Web z menu kontekstowe witryny.](iis/_static/add-website-context-menu-ws2016.png)

6. Konfigurowanie witryny sieci Web.

   ![Podaj nazwę lokacji, ścieżka fizyczna i nazwy hosta w kroku Dodawanie witryny sieci Web.](iis/_static/add-website-ws2016.png)

7. W **pul aplikacji** panelu, otwórz **edytowanie puli aplikacji** okno prawym przyciskiem myszy w puli aplikacji witryny sieci Web i wybierając **podstawowych ustawień...**  w menu podręcznym.

   ![Wybierz ustawienia podstawowe z menu kontekstowego puli aplikacji.](iis/_static/apppools-basic-settings-ws2016.png)

8. Ustaw **wersja środowiska .NET CLR** do **bez kodu zarządzanego**.

   ![Ustaw żadnego kodu zarządzanego dla wersji środowiska CLR programu .NET.](iis/_static/edit-apppool-ws2016.png)
     
    Uwaga: Ustawienie **wersja środowiska .NET CLR** do **bez kodu zarządzanego** jest opcjonalna. Platformy ASP.NET Core nie zależą od ładowania CLR pulpitu.

9. Upewnij się, że tożsamość procesu modelu ma odpowiednie uprawnienia.

   Jeśli zmienisz tożsamości domyślnej puli aplikacji (**Model procesu** > **tożsamości**) z **puli** tożsamość, sprawdź, czy nową tożsamość ma wymagane uprawnienia dostępu do folderu instalacji aplikacji, bazy danych i innych wymaganych zasobów.
   
## <a name="deploy-the-application"></a>Wdrażanie aplikacji
Wdrażanie aplikacji do folderu, który został utworzony w celu systemu usług IIS. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) to mechanizm zalecane dla wdrożenia. Poniżej przedstawiono alternatywy dla narzędzia Web Deploy.

Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony. Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona. Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.

### <a name="web-deploy-with-visual-studio"></a>Narzędzie Web Deploy w programie Visual Studio
Zobacz [tworzenie profilów publikowania dla programu Visual Studio i MSBuild, jak wdrażać aplikacje platformy ASP.NET Core](xref:publishing/web-publishing-vs#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użytku z narzędzia Web Deploy. Jeśli Twój dostawca hostingu dostarcza profil publikowania lub obsługę utworzenie, Pobierz swój profil i zaimportuj go za pomocą programu Visual Studio **publikowania** okna dialogowego.

![Okno dialogowe strony publikacji](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Narzędzie Web Deploy poza programu Visual Studio
Można również użyć [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) poza Visual Studio z wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Wdrażanie rozwiązań alternatywnych w sieci Web
Jeśli nie chcesz jej użyć narzędzia Web Deploy lub nie korzystają z programu Visual Studio, mogą przy użyciu jednej z kilku metod przeniesienie aplikacji w systemie hostingu, na przykład Xcopy, Robocopy lub programu PowerShell. Visual Studio użytkownicy mogą skorzystać z [przykłady publikowania](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Przeglądaj witryny sieci Web
![Przeglądarka Microsoft Edge załadował strony początkowej usług IIS.](iis/_static/browsewebsite.png)
   
> [!WARNING]
> Aplikacje .NET core są obsługiwane za pośrednictwem serwera proxy wstecznego między usługami IIS a Kestrel serwera. Aby można było utworzyć wstecznego serwera proxy, *web.config* plik musi znajdować się w ścieżce zawartości katalogu głównego (zazwyczaj ścieżki podstawowej aplikacji), aplikacji, która jest ścieżka fizyczna witryny sieci Web do usług IIS. Poufne pliki istnieją na ścieżkę fizyczną aplikacji, w tym podfolderów, takich jak *my_application.runtimeconfig.json*, *my_application.xml* (komentarze dokumentacji XML), a *my_ Application.deps.JSON*. *Web.config* plik jest niezbędny do tworzenia zwrotny serwer proxy do Kestrel, co zapobiega usług IIS obsługujących tych i innych poufnych plików. **W związku z tym ważne jest który *web.config* plik nie jest ani przypadkowo, zmieniono jego nazwę lub usunięte z wdrożenia.**

## <a name="data-protection"></a>Ochrona danych

Aplikacja platformy ASP.NET Core przechowuje zestaw kluczy w pamięci w następujących warunkach:

* Witryna sieci Web jest hostowany za usług IIS.
* Stos ochrony danych nie został skonfigurowany do przechowywania zestaw kluczy w magazynu trwałego.

Jeśli zestaw kluczy są przechowywane w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny uwierzytelniania formularzy zostały unieważnione. 
* Użytkownicy są wymagane do logowania się ponownie ich następnego żądania. 
* Wszystkie dane, które chroniona przez zestaw kluczy nie jest chroniony.

> [!WARNING]
> Ochrona danych jest używana przez kilka middlewares ASP.NET, w tym używane podczas uwierzytelniania. Nawet jeśli nie wywołuj interfejsy API ochrony danych ze swojego kodu, należy skonfigurować ochronę danych przy użyciu skryptu wdrażania lub w kodzie. Jeśli nie skonfigurujesz ochrony danych, domyślnie klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji. Ponowne uruchamianie unieważnia plików cookie napisane przez oprogramowanie pośredniczące uwierzytelniania plików cookie i użytkownicy będą musieli zalogować się ponownie.

Aby skonfigurować ochronę danych w środowisku usług IIS, należy użyć jednej z następujących metod:

* Uruchom [skrypt programu powershell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) można utworzyć wpisów rejestru odpowiednich (na przykład `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). To klucze są przechowywane w rejestrze, chronione za pomocą DPAPI za pomocą klucza komputera.
* Konfigurowanie puli aplikacji usług IIS do załadowania profilu użytkownika. To ustawienie jest **Model procesu** w obszarze **Zaawansowane ustawienia** dla puli aplikacji. Ustaw **Załaduj profil użytkownika** do `True`. To są przechowywane klucze w katalogu profilu użytkownika i ich ochrony za pomocą DPAPI kluczem określone konto użytkownika używane w puli aplikacji.
* Dostosuj kod aplikacji, aby [system plików jako magazyn pierścień klucza](xref:security/data-protection/configuration/overview). Użyj X509 certyfikat, aby chronić zestaw kluczy i upewnij się, jest zaufany certyfikat. Jeśli certyfikat z podpisem własnym, należy umieścić go w magazynie zaufanych głównych.

Korzystając z usług IIS w farmie sieci web:

* Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich maszyn.
* Wdrażanie X509 certyfikatów na każdym komputerze. Skonfiguruj [ochrony danych w kodzie](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Utwórz gałąź rejestru ochrony danych

Klucze ochrony danych używane przez aplikacje ASP.NET są przechowywane w gałęzi rejestru zewnętrzne do aplikacji. Aby zachować kluczy dla danej aplikacji, należy utworzyć gałąź rejestru dla aplikacji w puli aplikacji.

W przypadku instalacji autonomicznej usług IIS można użyć [skrypt programu PowerShell należy AutoGenKeys.ps1 ochrony danych](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) dla każdej puli aplikacji używane w aplikacji platformy ASP.NET Core. Ten skrypt tworzy specjalnego klucza rejestru HKLM, które ma ACLed tylko konta procesu roboczego. Klucze są szyfrowane, gdy przy użyciu DPAPI.

W scenariuszach kolektywu serwerów sieci web aplikację można skonfigurować do użyć ścieżki UNC do przechowywania jego zestaw kluczy ochrony danych. Domyślnie klucze ochrony danych nie są szyfrowane. Należy się upewnić, że uprawnienia udziału takie jest ograniczone do uruchomieniu aplikacji jako konto systemu Windows. Ponadto można wybrać do ochrony kluczy magazynowane przy użyciu X509 certyfikatu. Warto rozważyć mechanizm Zezwalaj użytkownikom na przekazywanie certyfikaty: miejsce certyfikatów do zaufanego certyfikatu użytkownika przechowywania i upewnij się, są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika. Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) szczegółowe informacje.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Konfigurowanie puli aplikacji usług IIS do załadowania profilu użytkownika

To ustawienie jest **Model procesu** w obszarze **Zaawansowane ustawienia** dla puli aplikacji. Ustaw Załaduj profil użytkownika `True`. To są przechowywane klucze w katalogu profilu użytkownika i ich ochrony za pomocą DPAPI kluczem określone konto użytkownika używane w puli aplikacji.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Zasady komputera dla ochrony danych

System ochrony danych ma ograniczoną obsługę ustawienie domyślne [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji, które korzystanie z interfejsów API ochrony danych. Zobacz [ochrony danych](xref:security/data-protection/index) dokumentację, aby uzyskać więcej szczegółowych informacji.

## <a name="configuration-of-sub-applications"></a>Konfiguracja aplikacji podrzędne

Aplikacje Sub dodany w obszarze katalogu głównego aplikacji nie powinna zawierać moduł platformy ASP.NET Core jako program obsługi. Jeśli jako program obsługi w sub aplikacji dodany moduł *web.config* pliku, pojawi się 500.19 (wewnętrzny błąd serwera) odwołuje się do pliku konfiguracji błędny podczas przeglądania aplikacji sub. W poniższym przykładzie przedstawiono zawartości opublikowanej *web.config* pliku dla platformy ASP.NET Core sub aplikacji:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Jeśli planujesz hostowanie - platformy ASP.NET Core sub aplikacji w kontenerze aplikacji platformy ASP.NET Core, należy jawnie usunąć dziedziczonej obsługi w sub-app *web.config* pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji na temat konfigurowania ASP.NET Core moduł *web.config* plików, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) tematu i [konfiguracja platformy ASP.NET Core modułu Odwołanie](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguracja usług IIS z pliku web.config

Konfiguracja programu IIS nadal ma wpływ `<system.webServer>` sekcji *web.config* dla tych funkcji usług IIS, które mają zastosowanie do konfiguracji zwrotnego serwera proxy. Na przykład masz usługi IIS skonfigurowane na poziomie systemu do używania dynamicznej kompresji, ale można wyłączyć to ustawienie dla aplikacji za pomocą `<urlCompression>` element w aplikacji *web.config* pliku. Aby uzyskać więcej informacji, zobacz [konfiguracji odwołania dla `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/), [odwołania do konfiguracji modułu platformy ASP.NET Core](xref:hosting/aspnet-core-module) i [przy użyciu modułów usług IIS z platformy ASP.NET Core](xref:hosting/iis-modules). Jeśli musisz ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w pulach aplikacji izolowanych (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentacji odwołania tematu w usługach IIS.

## <a name="configuration-sections-of-webconfig"></a>Sekcji konfiguracyjnych w pliku Web.config

W przeciwieństwie do aplikacji .NET Framework, które są skonfigurowane przy użyciu `<system.web>`, `<appSettings>`, `<connectionStrings>`, i `<location>` elementów w *web.config*, aplikacje platformy ASP.NET Core są skonfigurowane przy użyciu innych konfiguracji dostawcy. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pule aplikacji

Odnośnie do hostowania wielu witryn sieci Web na jednym komputerze, należy izolowania aplikacji od siebie, uruchamiając każdej aplikacji w odrębnej puli aplikacji. Usługi IIS **Dodawanie witryny sieci Web** okno dialogowe Ustawienia domyślne tego zachowania. Jeśli podasz **nazwa witryny**, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego. Jest tworzona nowa pula aplikacji przy użyciu nazwy lokacji, podczas dodawania witryny sieci Web.

## <a name="application-pool-identity"></a>Tożsamość puli aplikacji

Konto tożsamości puli aplikacji umożliwia uruchamianie aplikacji w obszarze unikatowe konto bez konieczności tworzenia i zarządzania nimi domeny lub lokalnego konta. W programie IIS 8.0 i nowsze proces roboczy administratora usług IIS (WAS) tworzy konto wirtualny o nazwie dla nowej puli aplikacji i aplikacji w puli procesów roboczych na tym koncie są domyślnie uruchamiane. W konsoli zarządzania usług IIS w obszarze **Zaawansowane ustawienia** dla puli aplikacji, upewnij się, że **tożsamości** jest skonfigurowany do używania **puli** jak pokazano na poniższej ilustracji.

![Okno dialogowe Zaawansowane ustawienia puli aplikacji](iis/_static/apppool-identity.png)

Proces zarządzania usług IIS tworzy bezpiecznego identyfikatora o nazwie puli aplikacji w systemie zabezpieczeń systemu Windows. Zasoby mogą być chronione przy użyciu tej tożsamości; Jednak ta tożsamość nie jest rzeczywistego konta użytkownika i nie będą widoczne w konsoli zarządzania użytkownika systemu Windows.

Jeśli musisz przyznać proces roboczy usług IIS procesów z podwyższonym poziomem uprawnień dostępu do aplikacji, należy zmodyfikować listy kontroli dostępu (ACL) dla katalog zawierający aplikację.

1. Otwórz Eksploratora Windows i przejdź do katalogu.

2. Kliknij prawym przyciskiem myszy w katalogu, a następnie kliknij **właściwości**.

3. W obszarze **zabezpieczeń** , kliknij pozycję **Edytuj** przycisk, a następnie **Dodaj** przycisku.

4. Kliknij przycisk **lokalizacje** przycisk i upewnij się, wybierz system.

5. Wprowadź **IIS AppPool\DefaultAppPool** w **wprowadź nazwy obiektów do wybrania** pola tekstowego.

  ![Wybierz użytkowników lub grup okno folderu aplikacji](iis/_static/select-users-or-groups-1.png)

6. Kliknij przycisk **Sprawdź nazwy** przycisk, a następnie kliknij przycisk **OK**.

  ![Wybierz użytkowników lub grup okno folderu aplikacji](iis/_static/select-users-or-groups-2.png)

Można też to zrobić za pomocą wiersza polecenia przy użyciu **ICACLS** narzędzie:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Porady dotyczące rozwiązywania problemów

Diagnozowanie problemów z wdrożeniami usług IIS:

* Badanie wyników przeglądarki.
* Sprawdź system **aplikacji** Zaloguj się za pośrednictwem **Podgląd zdarzeń**.
* Włącz `stdout` rejestrowania. **Moduł platformy ASP.NET Core** dziennika znajduje się w podanej w ścieżce *stdoutLogFile* atrybutu `<aspNetCore>` element *web.config*. Wszystkie foldery w ścieżce w wartości atrybutu musi istnieć w ramach wdrożenia. Należy także ustawić *stdoutLogEnabled = "true"*. Aplikacje używające `Microsoft.NET.Sdk.Web` zestaw SDK, aby utworzyć *web.config* plik domyślny *stdoutLogEnabled* ustawienie *false*, dlatego należy ręcznie wprowadzić *web.config* pliku lub zmodyfikowania pliku, aby włączyć `stdout` rejestrowania.

Niektóre typowe błędy nie są wyświetlane w przeglądarce, dziennik aplikacji i ASP.NET Core modułu dziennika do modułu *startupTimeLimit* (domyślne: 120 sekund) i *startupRetryCount* (domyślne: 2) są spełnione. W związku z tym Zaczekaj pełne 6 minut przed wydedukowania które module nie można uruchomić procesu aplikacji.

Jest szybkim sposobem Ustal, czy aplikacja działa prawidłowo do uruchomienia aplikacji bezpośrednio na Kestrel. Jeśli aplikacja została opublikowana jako wdrożenie framework zależne (stacje), wykonaj `dotnet my_application.dll` w folderze wdrożenia, który jest ścieżka fizyczna programu IIS do aplikacji. Jeśli aplikacja została opublikowana jako pliku wykonywalnego bezpośrednio z wiersza polecenia, niezależne wdrożenia (SCD), uruchom aplikację `my_application.exe`, w folderze wdrożenia. Jeśli Kestrel nasłuchuje na porcie domyślnym 5000, powinno być możliwe do przeglądania aplikacji w `http://localhost:5000/`. Jeśli aplikacja zazwyczaj odpowiada pod adresem punktu końcowego Kestrel, problem dotyczy najprawdopodobniej konfiguracji ASP.NET usług IIS, moduł Core-Kestrel i mniej prawdopodobne w aplikacji

Jednym ze sposobów ustalenia, czy usługi IIS odwrotny serwer proxy do serwera Kestrel działa poprawnie jest wykonanie żądania prostego pliku statycznego do arkusza stylów, skryptów lub obraz z plików statycznych dla aplikacji w *wwwroot* przy użyciu [plików statycznych oprogramowanie pośredniczące](xref:fundamentals/static-files). Jeśli aplikacja może obsługiwać pliki statyczne, ale innych punktów końcowych i widoków MVC kończą się niepowodzeniem, problem jest mniej prawdopodobnie związane z konfiguracją modułu Core ASP.NET usług IIS — Kestrel i prawdopodobnie w aplikacji (na przykład routingu MVC lub 500 Wewnętrzny błąd serwera).

Gdy Kestrel uruchamia się normalnie za usług IIS, ale aplikacja nie mogą być uruchamiane na komputerze po pomyślnym uruchomieniu lokalnie, tymczasowo można dodać zmienną środowiskową *web.config* można ustawić `ASPNETCORE_ENVIRONMENT` do `Development`. Tak długo, jak nie można zastąpić środowiska uruchamiania aplikacji, dzięki temu [strona wyjątek dewelopera](xref:fundamentals/error-handling) się pojawiać, gdy aplikacja jest uruchamiana w systemie. Ustawienie zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` w ten sposób jest zalecane tylko dla systemów przemieszczania/testowania, które nie są dostępne w Internecie. Należy usunąć zmienną środowiskową z *web.config* pliku po zakończeniu. Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych za pośrednictwem *web.config* dla zwrotnego serwera proxy, zobacz [environmentVariables elementem podrzędnym aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).

W większości przypadków Włączanie rejestrowania aplikacji pomaga w rozwiązywaniu problemów z aplikacji lub zwrotnego serwera proxy. Zobacz [rejestrowanie](xref:fundamentals/logging/index) Aby uzyskać więcej informacji.

Nasze ostatniego porady dotyczące rozwiązywania problemów odnosi się do aplikacji, których nie można uruchomić po uaktualnieniu albo .NET Core SDK w wersjach programowanie maszyny lub pakietu w aplikacji. W niektórych przypadkach niespójne pakiety mogą być dzielone aplikacji podczas przeprowadzania uaktualnienia głównych. Większość tych problemów można rozwiązać przez usunięcie `bin` i `obj` foldery w projekcie, czyszcząc pakietu pamięci podręcznych w `%UserProfile%\.nuget\packages\` i `%LocalAppData%\Nuget\v3-cache`, Przywracanie projektu i potwierdzenie, czy został poprzedniego wdrożenia w systemie całkowicie usunięty przed ponownym wdrażaniem aplikacji.

> [!TIP]
> Jest to wygodny sposób czyszczenia pamięci podręcznych pakietu uzyskanie *NuGet.exe* narzędzia z [NuGet.org](https://www.nuget.org/), dodaj go do systemu ścieżki i wykonaj `nuget locals all -clear` z wiersza polecenia. Można również wykonywać `dotnet nuget locals all --clear` polecenie w wierszu polecenia bez uzyskania *NuGet.exe*.

## <a name="common-errors"></a>Typowe błędy

Następujące nie znajduje się pełna lista błędów. Należy w przypadku wystąpienia błędu niewymienione w tym miejscu, pozostaw szczegółowy komunikat o błędzie w poniższej sekcji komentarzy.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>Nie można uzyskać VC ++ pakiet redystrybucyjny Instalatora

* **Instalator wyjątek:** 0x80072efd lub 0x80072f76 — nieokreślony błąd

* **Wyjątek dziennika Instalatora &#8224;:** błąd 0x80072efd lub 0x80072f76: nie można wykonać EXE pakietu

  &#8224; Dziennik znajduje się pod adresem C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Rozwiązywanie problemów:

* Jeśli system nie ma dostępu do Internetu, podczas instalowania serwera obsługującego pakietu, ten wyjątek występuje, gdy Instalator będzie mógł uzyskiwania *Microsoft Visual C++ 2015 Redistributable*. Może uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Jeśli Instalator zakończy się niepowodzeniem, nie może zostać wyświetlony środowiska uruchomieniowego .NET Core wymaganego do obsługi wdrożenia framework zależne (stacje). Jeśli planujesz hostowanie Dyskietki, upewnij się, że środowisko wykonawcze jest instalowana w programach &amp; funkcji. Można uzyskać Instalator środowiska uruchomieniowego, z [pobiera .NET](https://www.microsoft.com/net/download/core). Po zainstalowaniu środowiska uruchomieniowego, ponownie uruchom system, lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Uaktualnienie systemu operacyjnego usunąć moduł 32-bitowej platformy ASP.NET Core

* **Dziennik aplikacji:** modułu DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** nie można załadować. Dane to kod błędu.

Rozwiązywanie problemów:

* Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane podczas OS uaktualnienia. Jeśli program ASP.NET Core moduł został zainstalowany przed uaktualnieniem systemu operacyjnego, a następnie spróbuj uruchomić wszystkie puli aplikacji w trybie 32-bitowym po uaktualnieniu systemu operacyjnego, wystąpi ten problem. Po uaktualnieniu systemu operacyjnego Napraw moduł platformy ASP.NET Core. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](#install-the-net-core-windows-server-hosting-bundle). Wybierz **naprawy** po uruchomieniu Instalatora.

### <a name="platform-conflicts-with-rid"></a>Platforma w konflikcie z identyfikatorów RID

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** "MACHINE/WEBROOT/APPHOST/moja_aplikacja" z fizyczny katalog główny aplikacji "C:\{ścieżki}\' nie można uruchomić procesu z wiersza polecenia" "C:\\\my_application {PATH}. { exe | dll} "", kod błędu = "0x80004005: ff.

* **Dziennik modułu platformy ASP.NET Core:** nieobsługiwany wyjątek: System.BadImageFormatException: nie można załadować pliku lub zestawu "my_application.dll". Próbowano załadować program w niepoprawnym formacie.

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [porady dotyczące rozwiązywania problemów](#troubleshooting-tips).

* Upewnij się, że nie zostały ustawione `<PlatformTarget>` w Twojej *.csproj* który powoduje konflikt z RID. Na przykład określić nie `<PlatformTarget>` z `x86` i publikowanie za pomocą RID `win10-x64`, przy użyciu *dotnet publikowania - c - r wersji win10-x64* lub przez ustawienie `<RuntimeIdentifiers>` w Twojej *.csproj*  do `win10-x64`. Projekt publikuje bez ostrzeżenia lub błędu, ale kończy się niepowodzeniem z powyższych wyjątki zarejestrowane w systemie.

* Jeśli ten wyjątek występuje wdrożenia aplikacji Azure, podczas uaktualniania aplikacji i wdrożenie nowszej zestawy, ręcznie usuń wszystkie pliki z poprzedniego wdrożenia. Pokutujące niezgodne zestawy może spowodować `System.BadImageFormatException` wyjątków w przypadku wdrażania uaktualnionego aplikacji.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>Identyfikator URI punktu końcowego nieprawidłowe lub zatrzymania witryny sieci Web

* **Przeglądarki:** ERR_CONNECTION_REFUSED

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Upewnij się, że używasz właściwego identyfikatora URI punktu końcowego dla aplikacji. Sprawdź powiązania.

* Upewnij się, że witryna sieci Web usług IIS nie jest w *zatrzymane* stanu.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Serwer CoreWebEngine lub W3SVC funkcje wyłączone

* **Wyjątek systemu operacyjnego:** funkcje usług IIS 7.0 CoreWebEngine i W3SVC musi być zainstalowany, aby użyć modułu programu ASP.NET Core.

Rozwiązywanie problemów:

* Upewnij się, że włączono odpowiednie role i funkcje. Zobacz [konfiguracji usług IIS](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Ścieżka fizyczna witryny sieci Web nieprawidłowe lub brakujące aplikacji

* **Przeglądarki:** 403 Zabroniony — odmowa dostępu **--lub--** zabroniony 403.14 — serwer sieci Web jest skonfigurowana do nie listy zawartości tego katalogu.

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Witrynie sieci Web usług IIS **podstawowych ustawień** i folder aplikacji fizycznych. Upewnij się, że aplikacja jest w folderze w witrynie sieci Web usług IIS **ścieżka fizyczna**.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Niepoprawne roli, nie jest zainstalowany moduł lub nieprawidłowe uprawnienia

* **Przeglądarki:** 500.19 wewnętrzny błąd serwera — nie można uzyskać dostępu do żądanej strony, ponieważ jej odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Upewnij się, czy włączono właściwej roli. Zobacz [konfiguracji usług IIS](#iis-configuration).

* Sprawdź **programy &amp; funkcje** i upewnij się, że **modułu programu Microsoft ASP.NET Core** został zainstalowany. Jeśli **modułu programu Microsoft ASP.NET Core** nie występuje na liście zainstalowanych programów, instalowania modułu. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](#install-the-net-core-windows-server-hosting-bundle).

* Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **puli** lub z tożsamości niestandardowej ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Niepoprawne processPath, Brak zmiennej PATH, pakiet hostingu nie jest zainstalowany, system/IIS ponowne uruchomienie nie, VC ++ Redistributable nie jest zainstalowany lub dotnet.exe naruszenie zasad dostępu

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** "MACHINE/WEBROOT/APPHOST/moja_aplikacja" z fizyczny katalog główny aplikacji "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia"".\my_application.exe" ", kod błędu =" 0x80070002: 0.

* **Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [porady dotyczące rozwiązywania problemów](#troubleshooting-tips).

* Sprawdź *processPath* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest *dotnet* wdrożenia framework zależne (stacje) lub *.\my_application.exe* niezależne wdrożenia (SCD).

* Dla Dyskietki *dotnet.exe* może nie być dostępny za pośrednictwem ustawienia ścieżki. Upewnij się, że * C:\Program Files\dotnet\* istnieje w ŚCIEŻCE systemowej ustawienia.

* Dla Dyskietki *dotnet.exe* mogą nie być dostępne dla tożsamości puli aplikacji. Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu. Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.

* Może wdrożeniu Dyskietki i zainstalowane oprogramowanie .NET Core bez ponownego uruchamiania usług IIS. Uruchom ponownie serwer lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

* Które mogły zostać wdrożone Dyskietki bez instalowania środowiska uruchomieniowego .NET Core przez system operacyjny. Jeśli próbujesz wdrożyć Dyskietki, a nie zainstalowano środowiska wykonawczego platformy .NET Core, uruchom **.NET Core systemu Windows serwer obsługujący Instalatora pakietu** w systemie. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](#install-the-net-core-windows-server-hosting-bundle). Jeśli użytkownik próbuje zainstalować środowisko uruchomieniowe .NET Core w systemie bez połączenia z Internetem, uzyskać środowiska uruchomieniowego z [pobiera .NET](https://www.microsoft.com/net/download/core) i uruchom Instalatora pakietu hostingu, aby zainstalować moduł platformy ASP.NET Core. Ukończenie instalacji przez ponowne uruchomienie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

* Może wdrożeniu Dyskietki i zainstalowane oprogramowanie .NET Core bez ponownego uruchamiania systemu/usług IIS. Uruchom ponownie system lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

* Wdrożono Dyskietki i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie. Może uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Nieprawidłowe argumenty \<aspNetCore\> — element

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** "MACHINE/WEBROOT/APPHOST/moja_aplikacja" z fizyczny katalog główny aplikacji "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia"dotnet".\my_application.dll, kod błędu =" 0x80004005: 80008081.

* **Dziennik modułu platformy ASP.NET Core:** wykonywanie aplikacji nie istnieje: "PATH\my_application.dll"

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [porady dotyczące rozwiązywania problemów](#troubleshooting-tips).

* Sprawdź *argumentów* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest ona () *.\my_application.dll* dla framework zależne wdrożenia (stacje); lub brak pustego ciągu (b) (*argumenty = ""*), lub listę argumentów aplikacji (*argumenty = "arg1, arg2..."*) niezależne wdrożenia (SCD).

### <a name="missing-net-framework-version"></a>Brak wersji .NET Framework

* **Przeglądarki:** 502.3 Zła brama — wystąpił błąd połączenia podczas próby trasy żądania.

* **Dziennik aplikacji:** ErrorCode = z fizyczny katalog główny aplikacji "MACHINE/WEBROOT/APPHOST/moja_aplikacja" "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia"dotnet".\my_application.dll, kod błędu =" 0x80004005: 80008081.

* **Dziennik modułu platformy ASP.NET Core:** Brak metody, pliku lub zestawu wyjątku. Metoda, pliku lub zestawu określonego w wyjątek jest metoda, pliku lub zestawu .NET Framework.

Rozwiązywanie problemów:

* Zainstaluj wersję .NET Framework w systemie.

* W przypadku wdrożenia framework zależne (stacje) upewnij się, czy masz poprawne środowiska uruchomieniowego zainstalowane w systemie. Po uaktualnieniu projektu z 1.1 2.0, wdrożyć system obsługujący i odbierać tego wyjątku, upewnij się, że 2.0 framework jest instalowany przez system operacyjny.

### <a name="stopped-application-pool"></a>Zatrzymanej puli aplikacji

* **Przeglądarki:** 503 Usługa jest niedostępna

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów

* Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.

### <a name="iis-integration-middleware-not-implemented"></a>Oprogramowanie pośredniczące integracji usług IIS nie jest zaimplementowana

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** "MACHINE/WEBROOT/APPHOST/moja_aplikacja" z fizyczny katalog główny aplikacji "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\\my_application {PATH}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"

* **Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i przedstawia normalnego działania.

Rozwiązywanie problemów

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [porady dotyczące rozwiązywania problemów](#troubleshooting-tips).

* Potwierdzenia zostały prawidłowo przywoływany oprogramowanie pośredniczące integracji usług IIS przez wywołanie metody *. UseIISIntegration()* metody dla aplikacji *WebHostBuilder()* (platformy ASP.NET Core 1.x) lub użyć `CreateDefaultBuilder` — metoda (platformy ASP.NET Core 2.x). Zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting) szczegółowe informacje.

### <a name="sub-application-includes-a-handlers-section"></a>Podrzędne aplikacja zawiera \<obsługi\> sekcji

* **Przeglądarki:** błąd HTTP 500.19 — wewnętrzny błąd serwera

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i przedstawia normalnego działania głównego aplikacji. Plik dziennika nie został utworzony dla podrzędnej aplikacji.

Rozwiązywanie problemów

* Upewnij się, że aplikacja sub *web.config* nie zawiera pliku `<handlers>` sekcji.

### <a name="application-configuration-general-issue"></a>Ogólne problem z konfiguracją aplikacji

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** "MACHINE/WEBROOT/APPHOST/moja_aplikacja" z fizyczny katalog główny aplikacji "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\\my_application {PATH}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"

* **Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty

Rozwiązywanie problemów

* Ten wyjątek ogólny wskazuje, że proces nie mógł uruchomić, prawdopodobnie z powodu problemów konfiguracji aplikacji. Odwołanie do [struktury katalogów](xref:hosting/directory-structure), upewnij się, że aplikacji wdrożonych pliki i foldery są odpowiednie, a istnieją pliki konfiguracji aplikacji i zawiera poprawne ustawienia aplikacji i środowiska. Aby uzyskać więcej informacji, zobacz [porady dotyczące rozwiązywania problemów](#troubleshooting-tips).

## <a name="resources"></a>Resources

* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Konfiguracja modułu Core programu ASP.NET](xref:hosting/aspnet-core-module)
* [Używanie modułów usług IIS z platformy ASP.NET Core](xref:hosting/iis-modules)
* [Wprowadzenie do platformy ASP.NET Core](../index.md)
* [Witryna oficjalnego Microsoft IIS](https://www.iis.net/)
* [Bibliotece Microsoft TechNet: Serwer systemu Windows](https://docs.microsoft.com/windows-server/windows-server-versions)
