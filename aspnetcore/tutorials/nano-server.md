---
title: Platformy ASP.NET Core na serwerze Nano
author: shirhatti
description: "Dowiedz się, jak wykonać istniejącej aplikacji platformy ASP.NET Core, a następnie wdrożyć go do wystąpienia Nano Server korzysta z usług IIS."
keywords: Platformy ASP.NET Core nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: f30e911703d5c36d076872f91d4b2fafeefb91f5
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="e22bc-104">Platformy ASP.NET Core z usługami IIS na serwerze Nano</span><span class="sxs-lookup"><span data-stu-id="e22bc-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="e22bc-105">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="e22bc-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="e22bc-106">W tym samouczku możesz podjąć istniejącej aplikacji platformy ASP.NET Core i wdrożyć ją do wystąpienia Nano Server korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e22bc-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="e22bc-107">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="e22bc-107">Introduction</span></span>

<span data-ttu-id="e22bc-108">Nano Server to opcja instalacji, w systemie Windows Server 2016, oferty niewielki wpływ, większe bezpieczeństwo i lepszą obsługę niż instalacja Server Core lub całego serwera.</span><span class="sxs-lookup"><span data-stu-id="e22bc-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="e22bc-109">Przejrzyj urzędnika [dokumentacji Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) bardziej szczegółowe informacje i łącza pobierania wersje ewaluacyjne 180 dni.</span><span class="sxs-lookup"><span data-stu-id="e22bc-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="e22bc-110">Istnieją trzy sposoby łatwo można wypróbować Nano Server.</span><span class="sxs-lookup"><span data-stu-id="e22bc-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="e22bc-111">Zarejestrowanie się przy użyciu swojego konta MS:</span><span class="sxs-lookup"><span data-stu-id="e22bc-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="e22bc-112">Można pobrać plików z systemem Windows Server 2016 ISO i Utwórz obraz Nano Server.</span><span class="sxs-lookup"><span data-stu-id="e22bc-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="e22bc-113">Pobierz Nano Server wirtualnego dysku twardego.</span><span class="sxs-lookup"><span data-stu-id="e22bc-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="e22bc-114">Utwórz maszynę Wirtualną na platformie Azure przy użyciu obrazu Nano Server w galerii Azure.</span><span class="sxs-lookup"><span data-stu-id="e22bc-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="e22bc-115">Jeśli nie masz konta platformy Azure, możesz uzyskać bezpłatną 30-dniową wersję próbną.</span><span class="sxs-lookup"><span data-stu-id="e22bc-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="e22bc-116">W tym samouczku firma Microsoft będzie używać opcji 2 wbudowanych Nano Server wirtualny dysk twardy z systemem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="e22bc-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="e22bc-117">Przed kontynuowaniem w tym samouczku, konieczne będzie [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure) istniejącej aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e22bc-117">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="e22bc-118">Upewnij się, aplikacja korzysta z wbudowanej uruchamiane w **64-bitowych** procesu.</span><span class="sxs-lookup"><span data-stu-id="e22bc-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="e22bc-119">Konfigurowanie wystąpienia Nano Server</span><span class="sxs-lookup"><span data-stu-id="e22bc-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="e22bc-120">[Tworzenie nowej maszyny wirtualnej funkcji Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) na komputerze deweloperskim przy użyciu uprzednio pobranego dysku VHD.</span><span class="sxs-lookup"><span data-stu-id="e22bc-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="e22bc-121">Komputer wymaga ustawienie hasła administratora przed zalogowaniem.</span><span class="sxs-lookup"><span data-stu-id="e22bc-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="e22bc-122">Za pomocą konsoli maszyny Wirtualnej naciśnij klawisz F11, aby ustawić hasło przed pierwszego logowania.</span><span class="sxs-lookup"><span data-stu-id="e22bc-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="e22bc-123">Należy również sprawdzić adres IP nowej maszyny Wirtualnej albo Moje sprawdzanie serwer DHCP rozwiązać IP podany podczas inicjowania obsługi administracyjnej maszyny Wirtualnej lub Nano Server ustawień sieciowych konsoli odzyskiwania.</span><span class="sxs-lookup"><span data-stu-id="e22bc-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="e22bc-124">Załóżmy, że nowa maszyna wirtualna uruchamia się przy użyciu lokalnego adresu IP w wersji 4 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="e22bc-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="e22bc-125">Teraz możesz zarządzać za pomocą usługi zdalne środowiska PowerShell, który jest jedynym sposobem na serwerze Nano w pełni administrować.</span><span class="sxs-lookup"><span data-stu-id="e22bc-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="e22bc-126">Nawiązywanie połączenia z wystąpieniem Nano Server przy użyciu komunikacji zdalnej programu PowerShell</span><span class="sxs-lookup"><span data-stu-id="e22bc-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="e22bc-127">Otwórz okno programu PowerShell z podwyższonym poziomem uprawnień, można dodać do zdalnego wystąpienia Nano Server Twojej `TrustedHosts` listy.</span><span class="sxs-lookup"><span data-stu-id="e22bc-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="e22bc-128">Zastąp zmienną `$nanoServerIpAddress` z poprawnym adresem IP.</span><span class="sxs-lookup"><span data-stu-id="e22bc-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="e22bc-129">Po dodaniu wystąpienia Nano Server do Twojej `TrustedHosts`, możesz nawiązać połączenie przy użyciu komunikacji zdalnej programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e22bc-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="e22bc-130">Udane połączenie wyniki wiersza o formacie wyszukiwania takich jak:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="e22bc-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="e22bc-131">Tworzenie udziału plików</span><span class="sxs-lookup"><span data-stu-id="e22bc-131">Creating a file share</span></span>

<span data-ttu-id="e22bc-132">Utworzyć udział plików na serwerze Nano, dzięki czemu opublikowanej aplikacji mogą zostać skopiowane do niego.</span><span class="sxs-lookup"><span data-stu-id="e22bc-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="e22bc-133">Uruchom następujące polecenia w sesji zdalnej:</span><span class="sxs-lookup"><span data-stu-id="e22bc-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="e22bc-134">Po wykonaniu powyższych poleceń, można uzyskać dostęp do tego udziału odwiedzając `\\192.168.1.10\AspNetCoreSampleForNano` w Eksploratorze Windows na komputerze hosta.</span><span class="sxs-lookup"><span data-stu-id="e22bc-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="e22bc-135">Otwórz port w zaporze</span><span class="sxs-lookup"><span data-stu-id="e22bc-135">Open port in the firewall</span></span>

<span data-ttu-id="e22bc-136">Uruchom następujące polecenia w sesji zdalnej, aby otworzyć port w zaporze, aby umożliwić nasłuchiwania ruch TCP na porcie TCP/8000 usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e22bc-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="e22bc-137">Instalowanie usług IIS</span><span class="sxs-lookup"><span data-stu-id="e22bc-137">Installing IIS</span></span>

<span data-ttu-id="e22bc-138">Dodaj `NanoServerPackage` dostawcy z galerii programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e22bc-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="e22bc-139">Gdy dostawca jest zainstalowany i zaimportować, można zainstalować pakietów systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="e22bc-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="e22bc-140">Uruchom następujące polecenia w sesji programu PowerShell, który został utworzony wcześniej:</span><span class="sxs-lookup"><span data-stu-id="e22bc-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="e22bc-141">Aby szybko sprawdzić usług IIS jest prawidłowo skonfigurowany, możesz odwiedzić adres URL `http://192.168.1.10/` i powinna zostać wyświetlona strona powitalna.</span><span class="sxs-lookup"><span data-stu-id="e22bc-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="e22bc-142">Po zainstalowaniu usług IIS witryna sieci Web o nazwie `Default Web Site` domyślnie zostanie utworzona nasłuchiwanie na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="e22bc-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="e22bc-143">Instalowanie modułu platformy ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="e22bc-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="e22bc-144">Moduł platformy ASP.NET Core jest IIS 7.5 + moduł, który jest odpowiedzialny za zarządzanie procesem odbiorników platformy ASP.NET Core HTTP i żądania serwera proxy do procesów, którymi zarządza.</span><span class="sxs-lookup"><span data-stu-id="e22bc-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="e22bc-145">W tej chwili procesu, aby zainstalować moduł platformy ASP.NET Core dla usług IIS jest wykonywana ręcznie.</span><span class="sxs-lookup"><span data-stu-id="e22bc-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="e22bc-146">Musisz zainstalować [pakietu .NET Core systemu Windows serwer obsługujący](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) na regularne (nie Nano) komputera.</span><span class="sxs-lookup"><span data-stu-id="e22bc-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="e22bc-147">Po zainstalowaniu pakietu na komputerze regularne, należy skopiuj następujące pliki do udziału plików, który utworzony wcześniej.</span><span class="sxs-lookup"><span data-stu-id="e22bc-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="e22bc-148">Na serwerze regularne (nie Nano) z programem IIS uruchom następujące polecenia kopiowania:</span><span class="sxs-lookup"><span data-stu-id="e22bc-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="e22bc-149">Zastąp `C:\windows\system32\inetsrv` z `C:\Program Files\IIS Express` na komputerze z systemem Windows 10.</span><span class="sxs-lookup"><span data-stu-id="e22bc-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="e22bc-150">Na stronie Nano należy skopiować następujące pliki z udziału pliku utworzonej wcześniej prawidłowych lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="e22bc-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="e22bc-151">Tak uruchom następujące polecenia kopiowania:</span><span class="sxs-lookup"><span data-stu-id="e22bc-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="e22bc-152">W sesji zdalnej, uruchom następujący skrypt:</span><span class="sxs-lookup"><span data-stu-id="e22bc-152">Run the following script in the remote session:</span></span>

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
> <span data-ttu-id="e22bc-153">Usuń pliki *aspnetcore.dll* i *aspnetcore_schema.xml* z udziału po kroku powyżej.</span><span class="sxs-lookup"><span data-stu-id="e22bc-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="e22bc-154">Instalowanie programu .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="e22bc-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="e22bc-155">Jeśli aplikacja zostanie opublikowana jako [wdrożenia framework zależne (stacje)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core musi być zainstalowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e22bc-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="e22bc-156">Użyj [skrypt programu PowerShell dotnet install.ps1](https://dot.net/v1/dotnet-install.ps1) w sesji zdalnej programu PowerShell, aby zainstalować oprogramowanie .NET Core na serwerze Nano.</span><span class="sxs-lookup"><span data-stu-id="e22bc-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="e22bc-157">Przekaż wersji interfejsu wiersza polecenia z `-Version` przełącznika:</span><span class="sxs-lookup"><span data-stu-id="e22bc-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="e22bc-158">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e22bc-158">Publishing the application</span></span>

<span data-ttu-id="e22bc-159">Kopiuj opublikowane dane wyjściowe istniejących aplikacji do katalogu głównego udziału plików.</span><span class="sxs-lookup"><span data-stu-id="e22bc-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="e22bc-160">Należy wprowadzić zmiany w Twojej *web.config* wskaż którym została rozpakowana *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="e22bc-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="e22bc-161">Alternatywnie można dodać *dotnet.exe* do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="e22bc-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="e22bc-162">Przykładem *web.config* może wyglądać Jeśli *dotnet.exe* jest **nie** w ŚCIEŻCE:</span><span class="sxs-lookup"><span data-stu-id="e22bc-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

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

<span data-ttu-id="e22bc-163">Uruchom następujące polecenia w sesji zdalnej, aby utworzyć nową lokację w usługach IIS dla opublikowanych aplikacji do innego portu niż domyślna witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e22bc-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="e22bc-164">Należy również otworzyć ten port dostępu do sieci web.</span><span class="sxs-lookup"><span data-stu-id="e22bc-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="e22bc-165">Ten skrypt używa `DefaultAppPool` dla uproszczenia.</span><span class="sxs-lookup"><span data-stu-id="e22bc-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="e22bc-166">Więcej uwagi na uruchomione w ramach puli aplikacji można znaleźć [pul aplikacji](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="e22bc-166">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="e22bc-167">Znany problem działa .NET Core interfejsu wiersza polecenia na serwerze Nano i rozwiązania</span><span class="sxs-lookup"><span data-stu-id="e22bc-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="e22bc-168">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e22bc-168">Running the application</span></span>

<span data-ttu-id="e22bc-169">Opublikowana aplikacja sieci web nie jest dostępny w przeglądarce w `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="e22bc-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="e22bc-170">Jeśli po skonfigurowaniu rejestrowania zgodnie z opisem w [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), można wyświetlać dzienniki na *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="e22bc-170">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
