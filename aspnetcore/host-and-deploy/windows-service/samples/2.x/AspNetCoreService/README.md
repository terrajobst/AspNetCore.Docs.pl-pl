# <a name="custom-webhost-service-sample"></a><span data-ttu-id="29613-101">Niestandardowe hostem sieci Web usług — przykład</span><span class="sxs-lookup"><span data-stu-id="29613-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="29613-102">Ten przykład pokazuje, jak hostowanie aplikacji ASP.NET Core jako usługę Windows bez użycia usług IIS.</span><span class="sxs-lookup"><span data-stu-id="29613-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="29613-103">W tym przykładzie przedstawiono scenariusz opisany w [hostowanie aplikacji ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="29613-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="29613-104">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="29613-104">Instructions</span></span>

<span data-ttu-id="29613-105">Przykładowa aplikacja jest zmodyfikować zgodnie z instrukcjami wyświetlanymi w aplikacja internetowa ze stronami Razor [hostowanie aplikacji ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="29613-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="29613-106">Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="29613-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="29613-107">Utwórz folder na *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="29613-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="29613-108">Opublikuj aplikację w folderze z `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="29613-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="29613-109">Polecenie przenosi zasoby aplikacji *svc* folderu, w tym wymagane `appsettings.json` pliku i `wwwroot` folderu.</span><span class="sxs-lookup"><span data-stu-id="29613-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="29613-110">Otwórz **administratora** wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="29613-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="29613-111">Wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="29613-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="29613-112">*Odstęp między równości a początkiem ciąg ścieżki jest wymagana.*</span><span class="sxs-lookup"><span data-stu-id="29613-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="29613-113">W przeglądarce przejdź do `http://localhost:5000` i sprawdź, czy usługa jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="29613-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="29613-114">Aplikacja przekierowuje do bezpiecznego punktu końcowego `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="29613-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="29613-115">Aby zatrzymać usługę, użyj polecenia:</span><span class="sxs-lookup"><span data-stu-id="29613-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="29613-116">Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami, szybki sposób, aby udostępnić komunikaty o błędach jest dodanie dostawcy logowania, takie jak [Dostawca dziennika zdarzeń Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="29613-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="29613-117">Innym rozwiązaniem jest Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie.</span><span class="sxs-lookup"><span data-stu-id="29613-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="29613-118">Na przykład w tym miejscu jest nieobsługiwany wyjątek FileNotFound błędu w dzienniku zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="29613-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
