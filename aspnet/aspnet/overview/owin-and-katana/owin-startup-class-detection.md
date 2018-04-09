---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy uruchamiania OWIN | Dokumentacja firmy Microsoft
author: Praburaj
description: Ten samouczek pokazuje, jak skonfigurować klasy początkowej OWIN, który jest ładowany. Aby uzyskać więcej informacji o OWIN Zobacz Omówienie Katana projektu. W tym samouczku został...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="154c8-105">Wykrywanie klasy uruchamiania OWIN</span><span class="sxs-lookup"><span data-stu-id="154c8-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="154c8-106">przez [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="154c8-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="154c8-107">Ten samouczek pokazuje, jak skonfigurować klasy początkowej OWIN, który jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="154c8-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="154c8-108">Aby uzyskać więcej informacji o OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="154c8-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="154c8-109">W tym samouczku zapisał Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="154c8-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="154c8-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="154c8-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="154c8-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="154c8-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="154c8-112">Wykrywanie klasy uruchamiania OWIN</span><span class="sxs-lookup"><span data-stu-id="154c8-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="154c8-113">Każda aplikacja OWIN ma klasę uruchamiania, której możesz określić składniki do potoku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="154c8-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="154c8-114">Istnieją różne sposoby uruchamiania klasy mogą łączyć się z środowiska uruchomieniowego, w zależności od modelu hostingu wybierzesz (OwinHost, IIS i usługi IIS Express).</span><span class="sxs-lookup"><span data-stu-id="154c8-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="154c8-115">Klasa początkowa przedstawiona w tym samouczku można w każdej aplikacji hostingu.</span><span class="sxs-lookup"><span data-stu-id="154c8-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="154c8-116">Klasa początkowa uzyskuj hostingu środowiska uruchomieniowego, używając jednej z tych zbliża się do:</span><span class="sxs-lookup"><span data-stu-id="154c8-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="154c8-117">**Konwencja nazewnictwa**: Katana szuka klasę o nazwie `Startup` w przestrzeni nazw zgodnych z nazwą zestawu lub globalnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="154c8-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="154c8-118">**Atrybut OwinStartup**: jest to metoda potrwa większość deweloperów w celu określenia klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="154c8-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="154c8-119">Następujący atrybut ustawi Klasa początkowa `TestStartup` klasy w `StartupDemo` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="154c8-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="154c8-120">`OwinStartup` Atrybut zastępuje konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="154c8-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="154c8-121">Można również określić przyjazną nazwę z tego atrybutu, jednak przy użyciu przyjaznej nazwy wymaga również użycia `appSetting` w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="154c8-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="154c8-122">**Element appSetting w pliku konfiguracyjnym**: `appSetting` zastępuje element `OwinStartup` atrybut i konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="154c8-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="154c8-123">Może mieć wielu klas uruchamiania (każdego przy użyciu `OwinStartup` atrybut) i skonfigurować, która klasa uruchomienia zostanie załadowany w pliku konfiguracji przy użyciu znaczników podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="154c8-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="154c8-124">Następujący klucz jawnie określa Klasa początkowa i zestaw można również:</span><span class="sxs-lookup"><span data-stu-id="154c8-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="154c8-125">Następujący kod XML w pliku konfiguracyjnym określa uruchamiania przyjazną nazwę klasy `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="154c8-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="154c8-126">Powyżej znacznika musi być używany z następujących `OwinStartup` atrybut, który określa przyjazną nazwę i powoduje, że `ProductionStartup2` klasy do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="154c8-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="154c8-127">Aby wyłączyć wykrywanie uruchamiania środowiska OWIN Dodaj `appSetting owin:AutomaticAppStartup` o wartości `"false"` w pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="154c8-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="154c8-128">Tworzenie aplikacji sieci Web ASP.NET za pomocą uruchamiania OWIN</span><span class="sxs-lookup"><span data-stu-id="154c8-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="154c8-129">Utwórz pustą aplikację sieci web Asp.Net i nadaj mu nazwę **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="154c8-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="154c8-130">-Zainstaluj `Microsoft.Owin.Host.SystemWeb` za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="154c8-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="154c8-131">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="154c8-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="154c8-132">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="154c8-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="154c8-133">Dodaj klasę początkową OWIN.</span><span class="sxs-lookup"><span data-stu-id="154c8-133">Add an OWIN startup class.</span></span> <span data-ttu-id="154c8-134">W programie Visual Studio 2013 kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. — w **Dodaj nowy element** okna dialogowego wprowadź *OWIN* w polu wyszukiwania i Zmień nazwę pliku Startup.cs, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="154c8-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="154c8-135">Przy następnym, które chcesz dodać *klasy początkowej Owin*, będzie on w dostępnym **Dodaj** menu.</span><span class="sxs-lookup"><span data-stu-id="154c8-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="154c8-136">Alternatywnie kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**, a następnie wybierz **klasy początkowej Owin**.</span><span class="sxs-lookup"><span data-stu-id="154c8-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="154c8-137">Zastąp wygenerowany kod w *Startup.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="154c8-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="154c8-138">`app.Use` Wyrażenie lambda jest używane do rejestrowania składnika określonego oprogramowania pośredniczącego do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="154c8-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="154c8-139">W takim przypadku konfigurujemy ustawienia rejestrowania żądań przychodzących przed odpowiada na żądania przychodzącego.</span><span class="sxs-lookup"><span data-stu-id="154c8-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="154c8-140">`next` Parametr jest delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadań](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="154c8-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="154c8-141">`app.Run` Wyrażenie lambda przechwytuje się potoku na przychodzące żądania i zapewnia mechanizm odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="154c8-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="154c8-142">W powyższym kodzie możemy zostały oznaczone jako komentarz `OwinStartup` atrybut i firma Microsoft jest oparte na Konwencji uruchomionych klasa o nazwie `Startup` .-Naciśnij ***F5*** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="154c8-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="154c8-143">Kliknij przycisk Odśwież kilka razy.</span><span class="sxs-lookup"><span data-stu-id="154c8-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="154c8-144">Uwaga: Liczby wyświetlanej w obrazach w tym samouczku nie będzie odpowiadała wyświetlona liczba.</span><span class="sxs-lookup"><span data-stu-id="154c8-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="154c8-145">Ciąg milisekund jest używany do wyświetlenia nową odpowiedź po odświeżeniu strony.</span><span class="sxs-lookup"><span data-stu-id="154c8-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="154c8-146">Można wyświetlić informacje o śledzeniu w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="154c8-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="154c8-147">Dodaj więcej klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="154c8-147">Add More Startup Classes</span></span>

<span data-ttu-id="154c8-148">W tej sekcji dodamy innej klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="154c8-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="154c8-149">Można dodać wiele klasy początkowej OWIN do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="154c8-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="154c8-150">Na przykład można utworzyć klasy startowy do tworzenia, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="154c8-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="154c8-151">Utwórz nową klasę uruchamiania OWIN i nadaj mu nazwę `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="154c8-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="154c8-152">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="154c8-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="154c8-153">Naciśnij klawisz F5 sterowania, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="154c8-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="154c8-154">`OwinStartup` Atrybut określa produkcji Klasa początkowa jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="154c8-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="154c8-155">Utwórz inną klasę uruchamiania OWIN i nadaj mu nazwę `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="154c8-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="154c8-156">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="154c8-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="154c8-157">`OwinStartup` Określa atrybut przeciążenia powyżej `TestingConfiguration` jako *przyjazną* Nazwa klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="154c8-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="154c8-158">Otwórz *web.config* pliku, a następnie dodaj klucz uruchomienia aplikacji OWIN, który określa przyjazną nazwę klasy uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="154c8-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="154c8-159">Naciśnij klawisz F5 sterowania, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="154c8-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="154c8-160">Element ustawień aplikacji przyjmuje poprzedzających i badania konfiguracji jest uruchamiana.</span><span class="sxs-lookup"><span data-stu-id="154c8-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="154c8-161">Usuń *przyjazną* nazwa z `OwinStartup` atrybutu w `TestStartup` klasy.</span><span class="sxs-lookup"><span data-stu-id="154c8-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="154c8-162">Zastąp klucz uruchomienia aplikacji OWIN w *web.config* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="154c8-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="154c8-163">Przywróć `OwinStartup` atrybutu w każdej klasie do atrybutu domyślny kod wygenerowany przez program Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="154c8-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="154c8-164">Poniższych kluczy uruchomienia aplikacji OWIN spowoduje, że klasa produkcji do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="154c8-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="154c8-165">Klucz uruchomienia ostatniego określa uruchamiania metody konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="154c8-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="154c8-166">Następujący klucz uruchomienia aplikacji OWIN pozwala zmienić nazwę klasy konfiguracji do `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="154c8-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="154c8-167">Przy użyciu Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="154c8-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="154c8-168">Zastąp następujący kod w pliku Web.config:</span><span class="sxs-lookup"><span data-stu-id="154c8-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="154c8-169">Klucz ostatnio wins, dlatego w tym przypadku `TestStartup` jest określona.</span><span class="sxs-lookup"><span data-stu-id="154c8-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="154c8-170">Zainstaluj Owinhost z PMC:</span><span class="sxs-lookup"><span data-stu-id="154c8-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="154c8-171">Przejdź do folderu aplikacji (folder zawierający *Web.config* pliku) w wierszu polecenia i wpisz:</span><span class="sxs-lookup"><span data-stu-id="154c8-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="154c8-172">Wyświetli się okno wiersza poleceń:</span><span class="sxs-lookup"><span data-stu-id="154c8-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="154c8-173">Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="154c8-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="154c8-174">OwinHost honorowane konwencje uruchamiania wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="154c8-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="154c8-175">W oknie wiersza polecenia naciśnij klawisz Enter, aby zakończyć OwinHost.</span><span class="sxs-lookup"><span data-stu-id="154c8-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="154c8-176">W `ProductionStartup` klasy, Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="154c8-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="154c8-177">W wierszu polecenia i wpisz:</span><span class="sxs-lookup"><span data-stu-id="154c8-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="154c8-178">Klasa początkowa produkcji została załadowana.</span><span class="sxs-lookup"><span data-stu-id="154c8-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="154c8-179">Naszej aplikacji ma wiele klas uruchamiania, a w tym przykładzie mamy mają odłożone która klasa uruchamiania załadować do środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="154c8-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="154c8-180">Należy przetestować następujące opcje uruchamiania środowiska uruchomieniowego:</span><span class="sxs-lookup"><span data-stu-id="154c8-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
