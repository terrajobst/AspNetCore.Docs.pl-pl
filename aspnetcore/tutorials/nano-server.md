---
title: Platformy ASP.NET Core na serwerze Nano
author: shirhatti
description: "Dowiedz się, jak wykonać istniejącej aplikacji platformy ASP.NET Core, a następnie wdrożyć go do wystąpienia Nano Server korzysta z usług IIS."
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: d9b55fb42088b447451326b7ee573d9bfa5f5941
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>Platformy ASP.NET Core z usługami IIS na serwerze Nano

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym samouczku możesz podjąć istniejącej aplikacji platformy ASP.NET Core i wdrożyć ją do wystąpienia Nano Server korzysta z usług IIS.

## <a name="introduction"></a>Wprowadzenie

Nano Server to opcja instalacji, w systemie Windows Server 2016, oferty niewielki wpływ, większe bezpieczeństwo i lepszą obsługę niż instalacja Server Core lub całego serwera. Przejrzyj urzędnika [dokumentacji Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) bardziej szczegółowe informacje i łącza pobierania wersje ewaluacyjne 180 dni. 

Istnieją trzy sposoby łatwo można wypróbować Nano Server. Zarejestrowanie się przy użyciu swojego konta MS:

1. Można pobrać plików z systemem Windows Server 2016 ISO i Utwórz obraz Nano Server.

2. Pobierz Nano Server wirtualnego dysku twardego.

3. Utwórz maszynę Wirtualną na platformie Azure przy użyciu obrazu Nano Server w galerii Azure. Jeśli nie masz konta platformy Azure, możesz uzyskać bezpłatną 30-dniową wersję próbną.

W tym samouczku firma Microsoft będzie używać opcji 2 wbudowanych Nano Server wirtualny dysk twardy z systemem Windows Server 2016.

Przed kontynuowaniem w tym samouczku, konieczne będzie [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure) istniejącej aplikacji platformy ASP.NET Core. Upewnij się, aplikacja korzysta z wbudowanej uruchamiane w **64-bitowych** procesu.

## <a name="setting-up-the-nano-server-instance"></a>Konfigurowanie wystąpienia Nano Server

[Tworzenie nowej maszyny wirtualnej funkcji Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) na komputerze deweloperskim przy użyciu uprzednio pobranego dysku VHD. Komputer wymaga ustawienie hasła administratora przed zalogowaniem. Za pomocą konsoli maszyny Wirtualnej naciśnij klawisz F11, aby ustawić hasło przed pierwszego logowania. Należy również sprawdzić adres IP nowej maszyny Wirtualnej albo Moje sprawdzanie serwer DHCP rozwiązać IP podany podczas inicjowania obsługi administracyjnej maszyny Wirtualnej lub Nano Server ustawień sieciowych konsoli odzyskiwania.

> [!NOTE]
> Załóżmy, że nowa maszyna wirtualna uruchamia się przy użyciu lokalnego adresu IP w wersji 4 192.168.1.10.

Teraz możesz zarządzać za pomocą usługi zdalne środowiska PowerShell, który jest jedynym sposobem na serwerze Nano w pełni administrować.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Nawiązywanie połączenia z wystąpieniem Nano Server przy użyciu komunikacji zdalnej programu PowerShell

Otwórz okno programu PowerShell z podwyższonym poziomem uprawnień, można dodać do zdalnego wystąpienia Nano Server Twojej `TrustedHosts` listy.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Zastąp zmienną `$nanoServerIpAddress` z poprawnym adresem IP.

Po dodaniu wystąpienia Nano Server do Twojej `TrustedHosts`, możesz nawiązać połączenie przy użyciu komunikacji zdalnej programu PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Udane połączenie wyniki wiersza o formacie wyszukiwania takich jak:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Tworzenie udziału plików

Utworzyć udział plików na serwerze Nano, dzięki czemu opublikowanej aplikacji mogą zostać skopiowane do niego. Uruchom następujące polecenia w sesji zdalnej:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Po wykonaniu powyższych poleceń, można uzyskać dostęp do tego udziału odwiedzając `\\192.168.1.10\AspNetCoreSampleForNano` w Eksploratorze Windows na komputerze hosta.

## <a name="open-port-in-the-firewall"></a>Otwórz port w zaporze

Uruchom następujące polecenia w sesji zdalnej, aby otworzyć port w zaporze, aby umożliwić nasłuchiwania ruch TCP na porcie TCP/8000 usług IIS.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Instalowanie usług IIS

Dodaj `NanoServerPackage` dostawcy z galerii programu PowerShell. Gdy dostawca jest zainstalowany i zaimportować, można zainstalować pakietów systemu Windows.

Uruchom następujące polecenia w sesji programu PowerShell, który został utworzony wcześniej:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Aby szybko sprawdzić usług IIS jest prawidłowo skonfigurowany, możesz odwiedzić adres URL `http://192.168.1.10/` i powinna zostać wyświetlona strona powitalna. Po zainstalowaniu usług IIS witryna sieci Web o nazwie `Default Web Site` domyślnie zostanie utworzona nasłuchiwanie na porcie 80.

## <a name="installing-the-aspnet-core-module-ancm"></a>Instalowanie modułu platformy ASP.NET Core (ANCM)

Moduł platformy ASP.NET Core jest IIS 7.5 + moduł, który jest odpowiedzialny za zarządzanie procesem odbiorników platformy ASP.NET Core HTTP i żądania serwera proxy do procesów, którymi zarządza. W tej chwili procesu, aby zainstalować moduł platformy ASP.NET Core dla usług IIS jest wykonywana ręcznie. Musisz zainstalować [pakietu .NET Core systemu Windows serwer obsługujący](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) na regularne (nie Nano) komputera. Po zainstalowaniu pakietu na komputerze regularne, należy skopiuj następujące pliki do udziału plików, który utworzony wcześniej.

Na serwerze regularne (nie Nano) z programem IIS uruchom następujące polecenia kopiowania:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Zastąp `C:\windows\system32\inetsrv` z `C:\Program Files\IIS Express` na komputerze z systemem Windows 10.

Na stronie Nano należy skopiować następujące pliki z udziału pliku utworzonej wcześniej prawidłowych lokalizacji. Tak uruchom następujące polecenia kopiowania:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

W sesji zdalnej, uruchom następujący skrypt:

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> Usuń pliki *aspnetcore.dll* i *aspnetcore_schema.xml* z udziału po kroku powyżej.

## <a name="installing-net-core-framework"></a>Instalowanie programu .NET Core Framework

Jeśli aplikacja zostanie opublikowana jako [wdrożenia framework zależne (stacje)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core musi być zainstalowany na serwerze. Użyj [skrypt programu PowerShell dotnet install.ps1](https://dot.net/v1/dotnet-install.ps1) w sesji zdalnej programu PowerShell, aby zainstalować oprogramowanie .NET Core na serwerze Nano. Przekaż wersji interfejsu wiersza polecenia z `-Version` przełącznika:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Publikowanie aplikacji

Kopiuj opublikowane dane wyjściowe istniejących aplikacji do katalogu głównego udziału plików.

Należy wprowadzić zmiany w Twojej *web.config* wskaż którym została rozpakowana *dotnet.exe*. Alternatywnie można dodać *dotnet.exe* do ścieżki.

Przykładem *web.config* może wyglądać Jeśli *dotnet.exe* jest **nie** w ŚCIEŻCE:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

Uruchom następujące polecenia w sesji zdalnej, aby utworzyć nową lokację w usługach IIS dla opublikowanych aplikacji do innego portu niż domyślna witryna sieci Web. Należy również otworzyć ten port dostępu do sieci web. Ten skrypt używa `DefaultAppPool` dla uproszczenia. Więcej uwagi na uruchomione w ramach puli aplikacji można znaleźć [pul aplikacji](xref:host-and-deploy/iis/index#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Znany problem działa .NET Core interfejsu wiersza polecenia na serwerze Nano i rozwiązania
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Uruchamianie aplikacji

Opublikowana aplikacja sieci web nie jest dostępny w przeglądarce w `http://192.168.1.10:8000`. Jeśli po skonfigurowaniu rejestrowania zgodnie z opisem w [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), można wyświetlać dzienniki na *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
