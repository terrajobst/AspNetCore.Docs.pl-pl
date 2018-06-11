# <a name="custom-webhost-service-sample"></a><span data-ttu-id="39ada-101">Przykład usługi WebHost niestandardowych</span><span class="sxs-lookup"><span data-stu-id="39ada-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="39ada-102">W tym przykładzie pokazano, jak na potrzeby hostowania aplikacji platformy ASP.NET Core jako usługa systemu Windows bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="39ada-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="39ada-103">W tym przykładzie przedstawiono scenariusz opisany w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="39ada-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="39ada-104">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="39ada-104">Instructions</span></span>

<span data-ttu-id="39ada-105">Przykładowa aplikacja jest zmodyfikować zgodnie z instrukcjami wyświetlanymi w aplikacji sieci web Razor strony [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="39ada-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="39ada-106">Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="39ada-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="39ada-107">Utwórz folder na *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="39ada-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="39ada-108">Publikowanie aplikacji w folderze `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="39ada-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="39ada-109">Polecenie przenosi zasoby aplikacji do *svc* folderu, w tym wymaganych `appsettings.json` pliku i `wwwroot` folderu.</span><span class="sxs-lookup"><span data-stu-id="39ada-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="39ada-110">Otwórz **administratora** wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="39ada-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="39ada-111">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="39ada-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="39ada-112">*Odstęp między znak równości i początku ciąg ścieżki jest wymagana.*</span><span class="sxs-lookup"><span data-stu-id="39ada-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="39ada-113">W przeglądarce przejdź do `http://localhost:5000` i sprawdź, czy usługa jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="39ada-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="39ada-114">Aplikacja przekierowuje do bezpiecznego punktu końcowego `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="39ada-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="39ada-115">Aby zatrzymać usługę, należy użyć polecenia:</span><span class="sxs-lookup"><span data-stu-id="39ada-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="39ada-116">Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami, szybko udostępnić komunikaty o błędach jest dodanie dostawcy rejestrowania, takich jak [Dostawca dziennika zdarzeń systemu Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="39ada-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="39ada-117">Inną możliwością jest wykonanie Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie.</span><span class="sxs-lookup"><span data-stu-id="39ada-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="39ada-118">Na przykład w tym miejscu jest nieobsługiwany wyjątek błędu FileNotFound w dzienniku zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="39ada-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
