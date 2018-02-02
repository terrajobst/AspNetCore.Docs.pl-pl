# <a name="custom-webhost-service-sample"></a><span data-ttu-id="122ce-101">Przykład usługi WebHost niestandardowych</span><span class="sxs-lookup"><span data-stu-id="122ce-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="122ce-102">W tym przykładzie przedstawiono zalecany sposób hostowania aplikacji platformy ASP.NET Core w systemie Windows bez jako usługa systemu Windows za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="122ce-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="122ce-103">W tym przykładzie pokazano funkcje opisane w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="122ce-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="122ce-104">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="122ce-104">Instructions</span></span>

<span data-ttu-id="122ce-105">Przykładowa aplikacja jest prostej aplikacji sieci web MVC zmodyfikować zgodnie z instrukcjami wyświetlanymi w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="122ce-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="122ce-106">Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="122ce-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="122ce-107">Utwórz folder na *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="122ce-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="122ce-108">Publikowanie aplikacji w folderze `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="122ce-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="122ce-109">Polecenie spowoduje przeniesienie zasobów aplikacji do folderu, w tym wymaganych `appsettings.json` pliku i `wwwroot` folder z zawartością.</span><span class="sxs-lookup"><span data-stu-id="122ce-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="122ce-110">Otwórz **administrator** polecenia powłoki.</span><span class="sxs-lookup"><span data-stu-id="122ce-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="122ce-111">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="122ce-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="122ce-112">W przeglądarce przejdź do `http://localhost:5000` Aby sprawdzić, czy usługa jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="122ce-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="122ce-113">Aby zatrzymać usługę, należy użyć polecenia:</span><span class="sxs-lookup"><span data-stu-id="122ce-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="122ce-114">Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami uruchomionej w usłudze, szybko udostępnić komunikaty o błędach jest dodanie dostawcy rejestrowania, takich jak [Dostawca dziennika zdarzeń systemu Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="122ce-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="122ce-115">Inną możliwością jest wykonanie Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie.</span><span class="sxs-lookup"><span data-stu-id="122ce-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="122ce-116">Na przykład w tym miejscu jest nieobsługiwany wyjątek błędu FileNotFound w dzienniku zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="122ce-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
