---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy uruchamiania OWIN | Dokumentacja firmy Microsoft
author: Praburaj
description: "Ten samouczek pokazuje, jak skonfigurować klasy początkowej OWIN, który jest ładowany. Aby uzyskać więcej informacji o OWIN Zobacz Omówienie Katana projektu. W tym samouczku został..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a>Wykrywanie klasy uruchamiania OWIN
====================
przez [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje, jak skonfigurować klasy początkowej OWIN, który jest ładowany. Aby uzyskać więcej informacji o OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md). W tym samouczku zapisał Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Wymagania wstępne
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Wykrywanie klasy uruchamiania OWIN

 Każda aplikacja OWIN ma klasę uruchamiania, której możesz określić składniki do potoku aplikacji. Istnieją różne sposoby uruchamiania klasy mogą łączyć się z środowiska uruchomieniowego, w zależności od modelu hostingu wybierzesz (OwinHost, IIS i usługi IIS Express). Klasa początkowa przedstawiona w tym samouczku można w każdej aplikacji hostingu. Klasa początkowa uzyskuj hostingu środowiska uruchomieniowego, używając jednej z tych zbliża się do:  

1. **Konwencja nazewnictwa**: Katana szuka klasę o nazwie `Startup` w przestrzeni nazw zgodnych z nazwą zestawu lub globalnej przestrzeni nazw.
2. **Atrybut OwinStartup**: jest to metoda potrwa większość deweloperów w celu określenia klasy uruchamiania. Następujący atrybut ustawi Klasa początkowa `TestStartup` klasy w `StartupDemo` przestrzeni nazw. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup` Atrybut zastępuje konwencji nazewnictwa. Można również określić przyjazną nazwę z tego atrybutu, jednak przy użyciu przyjaznej nazwy wymaga również użycia `appSetting` w pliku konfiguracji.
3. **Element appSetting w pliku konfiguracyjnym**: `appSetting` zastępuje element `OwinStartup` atrybut i konwencji nazewnictwa. Może mieć wielu klas uruchamiania (każdego przy użyciu `OwinStartup` atrybut) i skonfigurować, która klasa uruchomienia zostanie załadowany w pliku konfiguracji przy użyciu znaczników podobny do następującego:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Następujący klucz jawnie określa Klasa początkowa i zestaw można również: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Następujący kod XML w pliku konfiguracyjnym określa uruchamiania przyjazną nazwę klasy `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Powyżej znacznika musi być używany z następujących `OwinStartup` atrybut, który określa przyjazną nazwę i powoduje, że `ProductionStartup2` klasy do uruchomienia.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Aby wyłączyć wykrywanie uruchamiania środowiska OWIN Dodaj `appSetting owin:AutomaticAppStartup` o wartości `"false"` w pliku web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Tworzenie aplikacji sieci Web ASP.NET za pomocą uruchamiania OWIN

1. Utwórz pustą aplikację sieci web Asp.Net i nadaj mu nazwę **StartupDemo**. -Zainstaluj `Microsoft.Owin.Host.SystemWeb` za pomocą Menedżera pakietów NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie **Konsola Menedżera pakietów**. Wprowadź następujące polecenie:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- Dodaj klasę początkową OWIN. W programie Visual Studio 2013 kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. — w **Dodaj nowy element** okna dialogowego wprowadź *OWIN* w polu wyszukiwania i Zmień nazwę pliku Startup.cs, a następnie kliknij przycisk **Dodaj**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 Przy następnym, które chcesz dodać *klasy początkowej Owin*, będzie on w dostępnym **Dodaj** menu.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 Alternatywnie kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**, a następnie wybierz **klasy początkowej Owin**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Zastąp wygenerowany kod w *Startup.cs* pliku następującym kodem:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` Wyrażenie lambda jest używane do rejestrowania składnika określonego oprogramowania pośredniczącego do potoku OWIN. W takim przypadku konfigurujemy ustawienia rejestrowania żądań przychodzących przed odpowiada na żądania przychodzącego. `next` Parametr jest delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadań](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) do następnego składnika w potoku. `app.Run` Wyrażenie lambda przechwytuje się potoku na przychodzące żądania i zapewnia mechanizm odpowiedzi.
     > [!NOTE]
     > W powyższym kodzie możemy zostały oznaczone jako komentarz `OwinStartup` atrybut i firma Microsoft jest oparte na Konwencji uruchomionych klasa o nazwie `Startup` .-Naciśnij ***F5*** do uruchomienia aplikacji. Kliknij przycisk Odśwież kilka razy.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Uwaga: Liczby wyświetlanej w obrazach w tym samouczku nie będzie odpowiadała wyświetlona liczba. Ciąg milisekund jest używany do wyświetlenia nową odpowiedź po odświeżeniu strony.  
 Można wyświetlić informacje o śledzeniu w **dane wyjściowe** okna.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Dodaj więcej klasy uruchamiania

W tej sekcji dodamy innej klasy uruchamiania. Można dodać wiele klasy początkowej OWIN do aplikacji. Na przykład można utworzyć klasy startowy do tworzenia, testowania i produkcji.

1. Utwórz nową klasę uruchamiania OWIN i nadaj mu nazwę `ProductionStartup`.
2. Zastąp wygenerowany kod poniżej:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Naciśnij klawisz F5 sterowania, aby uruchomić aplikację. `OwinStartup` Atrybut określa produkcji Klasa początkowa jest uruchamiany.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Utwórz inną klasę uruchamiania OWIN i nadaj mu nazwę `TestStartup`.
5. Zastąp wygenerowany kod poniżej:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup` Określa atrybut przeciążenia powyżej `TestingConfiguration` jako *przyjazną* Nazwa klasy uruchamiania.
6. Otwórz *web.config* pliku, a następnie dodaj klucz uruchomienia aplikacji OWIN, który określa przyjazną nazwę klasy uruchamiania:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Naciśnij klawisz F5 sterowania, aby uruchomić aplikację. Element ustawień aplikacji przyjmuje poprzedzających i badania konfiguracji jest uruchamiana.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Usuń *przyjazną* nazwa z `OwinStartup` atrybutu w `TestStartup` klasy.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Zastąp klucz uruchomienia aplikacji OWIN w *web.config* pliku następującym kodem:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Przywróć `OwinStartup` atrybutu w każdej klasie do atrybutu domyślny kod wygenerowany przez program Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Poniższych kluczy uruchomienia aplikacji OWIN spowoduje, że klasa produkcji do uruchomienia. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 Klucz uruchomienia ostatniego określa uruchamiania metody konfiguracji. Następujący klucz uruchomienia aplikacji OWIN pozwala zmienić nazwę klasy konfiguracji do `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Przy użyciu Owinhost.exe

1. Zastąp następujący kod w pliku Web.config:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 Klucz ostatnio wins, dlatego w tym przypadku `TestStartup` jest określona.
2. Zainstaluj Owinhost z PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Przejdź do folderu aplikacji (folder zawierający *Web.config* pliku) w wierszu polecenia i wpisz: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Wyświetli się okno wiersza poleceń: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost honorowane konwencje uruchamiania wymienionych powyżej.
5. W oknie wiersza polecenia naciśnij klawisz Enter, aby zakończyć OwinHost.
6. W `ProductionStartup` klasy, Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. W wierszu polecenia i wpisz: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Klasa początkowa produkcji została załadowana.  
    ![](owin-startup-class-detection/_static/image9.png)  
 Naszej aplikacji ma wiele klas uruchamiania, a w tym przykładzie mamy mają odłożone która klasa uruchamiania załadować do środowiska wykonawczego.
8. Należy przetestować następujące opcje uruchamiania środowiska uruchomieniowego:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
