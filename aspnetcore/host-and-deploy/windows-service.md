---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758196"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d6afe-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="d6afe-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d6afe-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d6afe-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d6afe-105">Aplikacji ASP.NET Core może być hostowana na Windows bez korzystania z usług IIS jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d6afe-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d6afe-106">Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="d6afe-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6afe-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="d6afe-108">Konwertuj projekt w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="d6afe-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="d6afe-109">Następujące minimalne zmiany są wymagane do skonfigurowania istniejący projekt ASP.NET Core, można uruchomić jako usługę:</span><span class="sxs-lookup"><span data-stu-id="d6afe-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="d6afe-110">W pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="d6afe-110">In the project file:</span></span>

   * <span data-ttu-id="d6afe-111">Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub dodać ją do `<PropertyGroup>` zawierający platforma docelowa:</span><span class="sxs-lookup"><span data-stu-id="d6afe-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="d6afe-112">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="d6afe-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="d6afe-113">Podaj identyfikatorów RID w liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="d6afe-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="d6afe-114">Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).</span><span class="sxs-lookup"><span data-stu-id="d6afe-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="d6afe-115">Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d6afe-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="d6afe-116">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="d6afe-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="d6afe-117">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d6afe-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="d6afe-118">Wywołaj [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="d6afe-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="d6afe-119">Wywołaj [UseContentRoot](xref:fundamentals/host/web-host#content-root) i ścieżka do aplikacji — opublikowane lokalizacji zamiast używania `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="d6afe-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="d6afe-120">Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d6afe-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="d6afe-121">Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d6afe-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="d6afe-122">Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w wierszu polecenia z folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="d6afe-123">Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="d6afe-124">W poniższym przykładzie aplikacja zostanie opublikowana w konfiguracji wydania dla `win7-x64` środowiska uruchomieniowego, aby utworzyć folder w *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="d6afe-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="d6afe-125">Utwórz konto użytkownika dla usługi przy użyciu `net user` polecenia:</span><span class="sxs-lookup"><span data-stu-id="d6afe-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="d6afe-126">Dla przykładowej aplikacji, należy utworzyć konto użytkownika o nazwie `ServiceUser` i hasła.</span><span class="sxs-lookup"><span data-stu-id="d6afe-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="d6afe-127">W poniższym poleceniu zastąp `{PASSWORD}` z [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="d6afe-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="d6afe-128">Jeśli potrzebujesz dodać użytkownika do grupy, użyj `net localgroup` polecenie, gdzie `{GROUP}` to nazwa grupy:</span><span class="sxs-lookup"><span data-stu-id="d6afe-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="d6afe-129">Aby uzyskać więcej informacji, zobacz [kont użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="d6afe-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="d6afe-130">Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) polecenia:</span><span class="sxs-lookup"><span data-stu-id="d6afe-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="d6afe-131">`{PATH}` &ndash; Ścieżka do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6afe-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="d6afe-132">`{USER ACCOUNT}` &ndash; Konto użytkownika (SID).</span><span class="sxs-lookup"><span data-stu-id="d6afe-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="d6afe-133">`(OI)` &ndash; Obiekt dziedziczenia flagi propaguje uprawnienia do podrzędnych plików.</span><span class="sxs-lookup"><span data-stu-id="d6afe-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="d6afe-134">`(CI)` &ndash; Flaga Dziedziczenie kontenera propaguje uprawnienia do folderów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d6afe-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="d6afe-135">`{PERMISSION FLAGS}` &ndash; Ustawia uprawnienia dostępu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6afe-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="d6afe-136">Zapis (`W`)</span><span class="sxs-lookup"><span data-stu-id="d6afe-136">Write (`W`)</span></span>
     * <span data-ttu-id="d6afe-137">Odczyt (`R`)</span><span class="sxs-lookup"><span data-stu-id="d6afe-137">Read (`R`)</span></span>
     * <span data-ttu-id="d6afe-138">Wykonaj (`X`)</span><span class="sxs-lookup"><span data-stu-id="d6afe-138">Execute (`X`)</span></span>
     * <span data-ttu-id="d6afe-139">Pełne (`F`)</span><span class="sxs-lookup"><span data-stu-id="d6afe-139">Full (`F`)</span></span>
     * <span data-ttu-id="d6afe-140">Modyfikowanie (`M`)</span><span class="sxs-lookup"><span data-stu-id="d6afe-140">Modify (`M`)</span></span>
   * <span data-ttu-id="d6afe-141">`/t` &ndash; Rekursywnie dotyczą plików i folderów podrzędnych istniejących.</span><span class="sxs-lookup"><span data-stu-id="d6afe-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="d6afe-142">Dla przykładowej aplikacji opublikowany *c:\\svc* folder i `ServiceUser` konto z uprawnieniami do zapisu/odczytu/wykonania, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d6afe-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="d6afe-143">Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="d6afe-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="d6afe-144">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="d6afe-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="d6afe-145">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="d6afe-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="d6afe-146">**Odstęp między równości i znaku cudzysłowu każdego parametru i wartość jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="d6afe-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="d6afe-147">`{SERVICE NAME}` &ndash; Nazwa do przypisania do usługi w [Menedżera sterowania usługami](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="d6afe-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="d6afe-148">`{PATH}` &ndash; Ścieżka do pliku wykonywalnego usługi.</span><span class="sxs-lookup"><span data-stu-id="d6afe-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="d6afe-149">`{DOMAIN}` (lub jeśli komputer nie jest domeny dołączonych, nazwy komputera lokalnego) i `{USER ACCOUNT}` &ndash; domeny (lub nazwy komputera lokalnego) i konto użytkownika w ramach którego działa usługa.</span><span class="sxs-lookup"><span data-stu-id="d6afe-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="d6afe-150">Czy **nie** pominąć `obj` parametru.</span><span class="sxs-lookup"><span data-stu-id="d6afe-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="d6afe-151">Wartością domyślną dla `obj` jest [konta LocalSystem](/windows/desktop/services/localsystem-account) konta.</span><span class="sxs-lookup"><span data-stu-id="d6afe-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="d6afe-152">Uruchamianie usługi w obszarze `LocalSystem` konto stanowi znaczące zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="d6afe-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="d6afe-153">Zawsze uruchamiaj usługi przy użyciu konta użytkownika z ograniczonymi uprawnieniami na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d6afe-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="d6afe-154">`{PASSWORD}` &ndash; Hasło konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6afe-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="d6afe-155">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d6afe-155">In the following example:</span></span>

   * <span data-ttu-id="d6afe-156">Usługa jest o nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="d6afe-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="d6afe-157">Opublikowana usługa znajduje się w *c:\\svc* folderu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="d6afe-158">Nosi nazwę pliku wykonywalnego aplikacji *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="d6afe-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="d6afe-159">`binPath` Wartość jest ujęta w cudzysłowy proste (").</span><span class="sxs-lookup"><span data-stu-id="d6afe-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="d6afe-160">Usługa jest uruchamiana w ramach `ServiceUser` konta.</span><span class="sxs-lookup"><span data-stu-id="d6afe-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="d6afe-161">Zastąp `{DOMAIN}` przy użyciu konta użytkownika domeny lub nazwy komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d6afe-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="d6afe-162">Ujmij `obj` wartość w cudzysłowy proste (").</span><span class="sxs-lookup"><span data-stu-id="d6afe-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="d6afe-163">Przykład: W przypadku hostowania systemu komputera lokalnego, o nazwie `MairaPC`ustaw `obj` do `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="d6afe-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="d6afe-164">Zastąp `{PASSWORD}` przy użyciu hasła konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6afe-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="d6afe-165">`password` Wartość jest ujęta w cudzysłowy proste (").</span><span class="sxs-lookup"><span data-stu-id="d6afe-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="d6afe-166">Upewnij się, że istnieją spacji między znakami równości parametrów i wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="d6afe-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="d6afe-167">Uruchom usługę za pomocą `sc start {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6afe-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="d6afe-168">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d6afe-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="d6afe-169">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="d6afe-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="d6afe-170">Aby sprawdzić stan usługi, użyj `sc query {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6afe-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="d6afe-171">Stan jest zgłaszany jako jeden z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="d6afe-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="d6afe-172">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d6afe-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="d6afe-173">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="d6afe-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="d6afe-174">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d6afe-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="d6afe-175">Zatrzymaj usługę za pomocą `sc stop {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6afe-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="d6afe-176">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d6afe-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="d6afe-177">Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6afe-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="d6afe-178">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d6afe-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="d6afe-179">Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d6afe-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="d6afe-180">Uruchom aplikację poza usługą</span><span class="sxs-lookup"><span data-stu-id="d6afe-180">Run the app outside of a service</span></span>

<span data-ttu-id="d6afe-181">Znacznie łatwiej testować i debugować podczas uruchamiania spoza niej, więc zwyczajowego, aby dodać kod, który wywołuje `RunAsService` tylko pod pewnymi warunkami.</span><span class="sxs-lookup"><span data-stu-id="d6afe-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="d6afe-182">Na przykład aplikacja może działać jako aplikacji konsoli, za pomocą `--console` argument wiersza polecenia lub jeśli jest dołączony debuger:</span><span class="sxs-lookup"><span data-stu-id="d6afe-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="d6afe-183">Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia `--console` przełącznik został usunięty, zanim argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="d6afe-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="d6afe-184">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d6afe-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d6afe-185">Obsługa zatrzymanie i uruchomienie zdarzenia</span><span class="sxs-lookup"><span data-stu-id="d6afe-185">Handle stopping and starting events</span></span>

<span data-ttu-id="d6afe-186">Aby obsłużyć [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, należy wprowadzić następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="d6afe-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="d6afe-187">Utwórz klasę, która pochodzi od klasy [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="d6afe-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="d6afe-188">Tworzenie metody rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="d6afe-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d6afe-189">W `Program.Main`, wywołanie nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="d6afe-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="d6afe-190">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d6afe-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="d6afe-191">Jeśli niestandardowa `WebHostService` kod wymaga usługi z wstrzykiwanie zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:</span><span class="sxs-lookup"><span data-stu-id="d6afe-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d6afe-192">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d6afe-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d6afe-193">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d6afe-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d6afe-194">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d6afe-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="d6afe-195">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="d6afe-195">Configure HTTPS</span></span>

<span data-ttu-id="d6afe-196">Aby skonfigurować usługę z bezpiecznego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="d6afe-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="d6afe-197">Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.</span><span class="sxs-lookup"><span data-stu-id="d6afe-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="d6afe-198">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="d6afe-199">Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d6afe-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="d6afe-200">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="d6afe-200">Current directory and content root</span></span>

<span data-ttu-id="d6afe-201">Bieżący katalog roboczy zwracany przez wywołanie metody `Directory.GetCurrentDirectory()` dla usługi Windows jest *C:\\WINDOWS\\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="d6afe-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="d6afe-202">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="d6afe-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="d6afe-203">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień za pomocą usługi [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) przy użyciu [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="d6afe-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="d6afe-204">Użyj ścieżki katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="d6afe-204">Use the content root path.</span></span> <span data-ttu-id="d6afe-205">`IHostingEnvironment.ContentRootPath` Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="d6afe-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="d6afe-206">Zamiast używania `Directory.GetCurrentDirectory()` Tworzenie ścieżki do plików ustawień, użyj ścieżki katalogu głównego zawartości i przechowuje pliki w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6afe-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="d6afe-207">Store pliki w odpowiedniej lokalizacji na dysku.</span><span class="sxs-lookup"><span data-stu-id="d6afe-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="d6afe-208">Określ ścieżkę bezwzględną z `SetBasePath` do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="d6afe-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6afe-209">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d6afe-209">Additional resources</span></span>

* <span data-ttu-id="d6afe-210">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="d6afe-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
