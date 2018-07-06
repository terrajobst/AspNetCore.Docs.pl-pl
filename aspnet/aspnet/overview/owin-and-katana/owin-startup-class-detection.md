---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy początkowej OWIN | Dokumentacja firmy Microsoft
author: Praburaj
description: W tym samouczku pokazano, jak skonfigurować klasy początkowej OWIN, które są ładowane. Aby uzyskać więcej informacji na temat OWIN Zobacz Omówienie projektu Katana. W tym samouczku został...
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0a4b87192296054bf6aef6c9406c64f19677a061
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828296"
---
<a name="owin-startup-class-detection"></a>Wykrywanie klasy początkowej OWIN
====================
przez [projektu Praburaj](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku pokazano, jak skonfigurować klasy początkowej OWIN, które są ładowane. Aby uzyskać więcej informacji na temat OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md). Ten samouczek został napisany przez Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj projektu i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Wymagania wstępne
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Wykrywanie klasy początkowej OWIN

 Każda aplikacja OWIN ma klasę uruchamiania, w których określane są składniki do potoku aplikacji. Istnieją różne sposoby uruchamiania klasy można połączyć ze środowiskiem uruchomieniowym, w zależności od modelu hostingu wybierzesz (OwinHost, IIS i usług IIS Express). Klasa początkowa przedstawiona w tym samouczku można w każdej aplikacji macierzystej. Klasa startowa. Połącz się z hostingu środowiska uruchomieniowego przy użyciu których zbliża się do jednej z tych:  

1. **Konwencje nazewnictwa**: Katana szuka klasę o nazwie `Startup` w przestrzeni nazw, nazwa zestawu lub globalnej przestrzeni nazw.
2. **Atrybut OwinStartup**: jest to podejście, większość programistów zajmie się w celu określenia klasy uruchamiania. Następujący atrybut ustawi klasa startowa `TestStartup` klasy w `StartupDemo` przestrzeni nazw. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup` Konwencji nazewnictwa przesłonięcia atrybutów. Można również określić przyjazną nazwę, z tego atrybutu, jednak przy użyciu przyjaznej nazwy, musisz również użyć `appSetting` elementu w pliku konfiguracji.
3. **Element appSetting w pliku konfiguracyjnym**: `appSetting` zastępuje element `OwinStartup` atrybut i konwencji nazewnictwa. Możliwość posiadania wielu klas uruchamiania (przy każdym użyciu `OwinStartup` atrybutu) i skonfigurować, która klasa uruchamiania zostanie załadowany w pliku konfiguracji, za pomocą znaczników podobny do następującego:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Można także następujący klucz, który jawnie określa klasę uruchamiania i zestawu: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Następujący kod XML w pliku konfiguracyjnym Określa nazwę klasy przyjazne uruchamiania `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Powyższe znaczników musi być używany z następujących `OwinStartup` atrybut, który określa przyjazną nazwę i powoduje, że `ProductionStartup2` klasy do uruchomienia.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Aby wyłączyć Dodaj OWIN uruchamiania odnajdywania `appSetting owin:AutomaticAppStartup` o wartości `"false"` w pliku web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Tworzenie aplikacji internetowej ASP.NET przy użyciu początkowa OWIN

1. Utwórz pustą aplikację sieci web platformy Asp.Net i nadaj mu **StartupDemo**. -Zainstaluj `Microsoft.Owin.Host.SystemWeb` przy użyciu Menedżera pakietów NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie **Konsola Menedżera pakietów**. Wprowadź następujące polecenie:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Dodaj klasę początkową OWIN. W programie Visual Studio 2013 kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. — w **Dodaj nowy element** okna dialogowego wprowadź *OWIN* w polu wyszukiwania, a następnie zmień nazwę pliku Startup.cs, a następnie kliknij przycisk **Dodaj**.  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   Następnym razem, aby dodać *Klasa początkowa Owin*, będzie on w dostępnym **Dodaj** menu.  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   Alternatywnie, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**, a następnie wybierz pozycję **Klasa początkowa Owin**.  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- Zastąp kod wygenerowany w *Startup.cs* pliku następującym kodem:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  `app.Use` Wyrażenie lambda jest używane do rejestrowania składnika określonego oprogramowania pośredniczącego do potoku OWIN. W tym przypadku konfigurujemy rejestrowania żądań przychodzących przed udzieleniem odpowiedzi na żądanie przychodzące. `next` Parametrem jest delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadań](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) na następny składnik w potoku. `app.Run` Wyrażenie lambda przechwytuje się Potok żądań przychodzących i udostępnia mechanizm odpowiedzi.
     > [!NOTE]
     > W powyższym kodzie możemy zostały oznaczone jako komentarz `OwinStartup` atrybutu, a my one polegania na Konwencji uruchamiania klasę o nazwie `Startup` .-Naciśnij ***F5*** do uruchomienia aplikacji. Następnie kliknij przycisk Odśwież kilka razy.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  Uwaga: Liczby wyświetlanej na ilustracjach w tym samouczku nie będą zgodne numer widoczny. Ciąg milisekund jest używany do wyświetlenia nowej odpowiedzi po odświeżeniu strony.  
  Można wyświetlić informacje o śledzeniu w **dane wyjściowe** okna.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Dodaj więcej klasy uruchamiania

W tej sekcji dodamy innej klasy uruchamiania. Możesz dodać wiele klasy początkowej OWIN do aplikacji. Na przykład możesz tworzyć klasy startowy do tworzenia, testowania i produkcji.

1. Utwórz nową klasę początkowa OWIN i nadaj mu nazwę `ProductionStartup`.
2. Zastąp wygenerowany kod następujących czynności:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Naciśnij klawisz F5 sterowania, aby uruchomić aplikację. `OwinStartup` Atrybut określa, klasa początkowa produkcji jest uruchamiany.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Utwórz inny Klasa początkowa OWIN i nadaj mu `TestStartup`.
5. Zastąp wygenerowany kod następujących czynności:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup` Określa atrybut przeciążenia powyżej `TestingConfiguration` jako *przyjazna* Nazwa klasy uruchamiania.
6. Otwórz *web.config* pliku i Dodaj klucz uruchomienia aplikacji OWIN, który określa przyjazną nazwę klasy uruchamiania:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Naciśnij klawisz F5 sterowania, aby uruchomić aplikację. Element ustawień aplikacji zajmuje poprzedzających i test jest uruchamiany w konfiguracji.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Usuń *przyjazna* nazwę z `OwinStartup` atrybutu w `TestStartup` klasy.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Zamień na klucz uruchomienia aplikacji OWIN w *web.config* pliku następującym kodem:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Przywróć `OwinStartup` atrybutu w każdej klasie kodu atrybut domyślny wygenerowany przez program Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Poniższych kluczy uruchamiania aplikacji OWIN spowoduje, że klasa produkcji do uruchomienia. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Klucz uruchomienia ostatniego określa metody konfiguracji uruchamiania. Następujący klucz uruchomienia aplikacji OWIN pozwala zmienić nazwę klasy konfiguracji do `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Za pomocą Owinhost.exe

1. Zastąp plik Web.config następującym kodem:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Klucz ostatnio wygrywa, więc w tym przypadku `TestStartup` jest określony.
2. Zainstaluj Owinhost z konsoli zarządzania Pakietami: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Przejdź do folderu aplikacji (folder zawierający *Web.config* plików) i w wierszu polecenia i wpisz: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   W oknie polecenia będą wyświetlane: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost honorowane konwencje uruchamiania wymienionych powyżej.
5. W oknie wiersza polecenia naciśnij klawisz Enter, aby zakończyć OwinHost.
6. W `ProductionStartup` klasy, Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. W wierszu polecenia i wpisz: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Klasa początkowa produkcji jest ładowany.  
    ![](owin-startup-class-detection/_static/image9.png)  
   Nasza aplikacja ma wiele klas uruchamiania, a w tym przykładzie firma Microsoft mają odłożone której klasy uruchamiania do obciążenia aż do czasu.
8. Przetestuj następujące opcje uruchamiania środowiska uruchomieniowego:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
