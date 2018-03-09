---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: "Jak uaktualnić platformy ASP.NET MVC 4 oraz sieci Web projektu interfejsu API platformy ASP.NET MVC 5 i składnika Web API 2 | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "ASP.NET MVC 5 i Web API 2 Przełącz hosta z nowych funkcji, łącznie z trasami atrybutów, filtry uwierzytelniania i o wiele więcej."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 05a3189cf105d1230b96e90b46ea5ab60fef1bf1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Jak uaktualnić do programu ASP.NET MVC 5 i składnika Web API 2 platformy ASP.NET MVC 4 oraz projekt interfejsu API sieci Web
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 i Web API 2 Przełącz hosta z nowych funkcji, łącznie z trasami atrybutów, filtry uwierzytelniania i o wiele więcej. Zobacz [https://www.asp.net/vnext](https://www.asp.net/core) więcej szczegółów.
> 
> Ten przewodnik przeprowadzi Cię kroki wymagane do uaktualnienia do najnowszej wersji aplikacji.  
> 
> > [!NOTE]
> > Zobacz [ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji](../../../visual-studio/overview/2013/release-notes.md) informacji na istotne zmiany z MVC 4 i interfejsu API sieci Web do następnej wersji.
> 
>   
> 
> Ten artykuł dotyczy Youngjune Hong i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Kroki uaktualniania

1. Tworzenie kopii zapasowej projektu. W tym przewodniku wymaga wprowadzić zmiany w pliku projektu, w konfiguracji pakietu i w plikach web.config.
2. W przypadku uaktualniania z interfejsu API sieci Web 2 interfejsu API sieci Web, w pliku global.asax, zmieniać:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

 na

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Upewnij się, że wszystkie pakiety używających swoje projekty są zgodne z MVC 5 i 2 interfejsu API sieci Web. Następujące tabelą MVC 4 i interfejsu API sieci Web związanych z pakietów niż muszą zostać zmienione. Jeśli pakiet, który jest zależny od jednego z wymienionych poniżej pakietów, skontaktuj się z wydawcy, aby uzyskać nowsze wersje, które są zgodne z MVC 5 i 2 interfejsu API sieci Web. Jeśli masz kod źródłowy dla tych pakietów, należy je skompiluj przy nowe zestawy MVC 5 i 2 interfejsu API sieci Web.   

    | **Identyfikator pakietu** | **Stara wersja** | **Nowa wersja** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o: p >< / o: p > | Usunięte |
    | Microsoft.AspNet.WebPages.Administration | < o: p >< / o: p > | Usunięte |
    | Pomocnicy firmy Microsoft | < o: p >< / o: p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Pomocnicy Microsoft zostało zastąpione Microsoft.AspNet.WebHelpers. Należy najpierw usunięcie starego pakietu, a następnie zainstaluj nowszą pakietu.   
    >   
    > Nie ma żadnych zgodność wersji krzyżowego między najważniejszych pakietów platformy ASP.NET. Na przykład MVC 5 jest zgodny z tylko Razor 3, a nie Razor 2.
4. Otwórz projekt w programie Visual Studio 2013.
5. Usuń wszystkie następujące pakiety ASP.NET NuGet, które są zainstalowane. Spowoduje usunięcie tych przy użyciu konsoli Menedżera pakietów (PMC). Aby otworzyć PMC, wybierz **narzędzia** menu, a następnie wybierz **Menedżer pakietów biblioteki,** następnie wybierz **Konsola Menedżera pakietów**. Projekt może nie zawierać wszystkich z nich.

    1. `Microsoft.AspNet.WebPages.Administration`  
 Ten pakiet jest zwykle dodane podczas uaktualniania MVC 3 MVC 4. Aby usunąć go, uruchom następujące polecenie w kryterium:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
 Ten pakiet zawiera zostały rebranded jako `Microsoft.AspNet.WebHelpers`. Aby usunąć go, uruchom następujące polecenie w kryterium:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
 Ten pakiet zawiera obejść dla usterki w MVC 4, który został rozwiązany w MVC 5. Aby usunąć go, uruchom następujące polecenie w kryterium:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Uaktualnij wszystkie pakiety ASP.NET NuGet przy użyciu kryterium. W kryterium uruchom następujące polecenie:  
    `Update-Package`  
 `Update-Package` Polecenia bez żadnych parametrów spowoduje zaktualizowanie każdy pakiet. Indywidualnie aktualizację pakietów za pomocą argumentu identyfikator. Aby uzyskać więcej informacji na temat polecenia aktualizacji, uruchom `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aktualizowanie aplikacji *web.config* pliku

Pamiętaj wprowadzić te zmiany w aplikacji *web.config* plik nie *web.config* w pliku *widoków* folderu.

Zlokalizuj `<runtime>/<assemblyBinding>` , a następnie wprowadź następujące zmiany:

1. W elementach z atrybut name "System.Web.Mvc" należy zmienić numer wersji z "4.0.0.0" do "5.0.0.0". (Dwie zmiany w tym elemencie).
2. W elementach z atrybutem nazwy &quot;System.Web.Helpers "i &quot;System.Web.WebPages&quot; numer wersji z"2.0.0.0"do"3.0.0.0". Cztery zmiany nastąpi, dwa w poszczególnych elementów.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Zlokalizuj `<appSettings>` sekcji i zaktualizuj webpages:version z 2.0.0.0.0 do 3.0.0.0, jak pokazano poniżej:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Usuń wszystkie poziomy zaufania niż pełne. Na przykład:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Aktualizacja *web.config* pliki w folderze Widoki

Jeśli aplikacja używa obszarów, również konieczne będzie zaktualizowanie każdego *web.config* w pliku *widoków* podfolderu każdego folderu obszaru.

1. Zaktualizuj wszystkie elementy, które zawierają "System.Web.Mvc" z wersji "4.0.0.0" do wersji "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Zaktualizuj wszystkie elementy, które zawierają "System.Web.WebPages.Razor" z wersją "2.0.0.0" do wersji "3.0.0.0". Jeśli ta sekcja zawiera "System.Web.WebPages", należy zaktualizować te elementy z wersją "2.0.0.0" do wersji "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Jeśli usunięto `Microsoft-Web-Helpers` instalacja pakietu NuGet w poprzednim kroku, `Microsoft.AspNet.WebHelpers` przy użyciu następującego polecenia w kryterium:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Jeśli aplikacja korzysta z [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) metody, Dodaj następujący kod do *Web.config* pliku.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Końcowe czynności

Tworzenie i testowanie aplikacji.

Usuń identyfikator GUID typu projektu MVC 4 z plików projektu.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **Zwolnij projekt**.
2. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję Edytuj ProjectName.csproj.
3. Zlokalizuj `ProjectTypeGuids` elementu, a następnie usuń MVC 4 projekt identyfikatora GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Zapisz i zamknij plik otwartego projektu.
5. Kliknij prawym przyciskiem myszy projekt i wybierz **Załaduj ponownie projekt**.
