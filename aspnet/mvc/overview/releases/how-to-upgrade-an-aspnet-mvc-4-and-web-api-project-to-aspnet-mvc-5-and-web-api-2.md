---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Jak uaktualnić platformy ASP.NET MVC 4 oraz projektu interfejsu API platformy ASP.NET MVC 5 i Web API 2 w sieci Web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ASP.NET MVC 5 i Web API 2 Przenieś hosta z nowych funkcji, takich jak trasowanie atrybutów, filtry uwierzytelniania i wiele więcej.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d6fb40741c5f7b992e907a462ac92972fe603624
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578370"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Jak uaktualnić platformy ASP.NET MVC 4 i projektu interfejsu Web API platformy ASP.NET MVC 5 i Web API 2
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> ASP.NET MVC 5 i Web API 2 Przenieś hosta z nowych funkcji, takich jak trasowanie atrybutów, filtry uwierzytelniania i wiele więcej. Zobacz [ https://www.asp.net/vnext ](https://www.asp.net/core) Aby uzyskać więcej informacji.
> 
> Ten przewodnik przeprowadzi Cię kroki wymagane do uaktualnienia do najnowszej wersji aplikacji.  
> 
> > [!NOTE]
> > Zobacz [ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) uzyskać informacji na temat przełomowych zmian z MVC 4 i interfejsu API sieci Web do następnej wersji.
> 
>   
> 
> W tym artykule został napisany przez SRA Youngjune i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Procedura uaktualniania

1. Kopie zapasowe projektu. W tym instruktażu będą wymagać zmian w pliku projektu, w konfiguracji pakietu i w plikach web.config.
2. W przypadku uaktualniania z internetowego interfejsu API sieci Web API 2, w pliku global.asax, zmienić:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   na

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Upewnij się, że wszystkie pakiety, korzystających z Twoich projektów są zgodne z MVC 5 i Web API 2. Następujące tabeli przedstawiono MVC 4 i internetowy interfejs API związane z pakietów, nie trzeba zmieniać. Jeśli masz pakiet, który jest zależny od jednego z pakietów wymienionych poniżej, skontaktuj się z wydawcy, aby uzyskać nowsze wersje, które są zgodne z MVC 5 i Web API 2. Jeśli masz kod źródłowy tych pakietów, możesz należy ponownie skompilować je przy użyciu nowych zestawów MVC 5 i Web API 2.   

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
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o:p >< / o:p > | Usunięte |
    | Microsoft.AspNet.WebPages.Administration | < o:p >< / o:p > | Usunięte |
    | Microsoft-Web-Helpers | < o:p >< / o:p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > -Microsoft-pomocnicy Microsoft.AspNet.WebHelpers został zastąpiony. Należy najpierw usunięcie starego pakietu, a następnie zainstaluj nowszą pakietu.   
    >   
    > Nie ma żadnych między wersjami między najważniejszych pakietów platformy ASP.NET. Na przykład MVC 5 jest zgodny z tylko Razor 3 i nie aparatu Razor 2.
4. Otwórz swój projekt w programie Visual Studio 2013.
5. Usuń wszystkie następujące pakiety NuGet programu ASP.NET, które są zainstalowane. Spowoduje usunięcie je za pomocą konsoli Menedżera pakietów (PMC). Aby otworzyć konsolę zarządzania Pakietami, wybierz **narzędzia** menu, a następnie wybierz **Menedżer pakietów biblioteki,** polecenie **Konsola Menedżera pakietów**. Projekt może nie zawierać wszystkich z nich.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Ten pakiet jest zazwyczaj dodawany podczas uaktualniania z MVC 3 do MVC 4. Aby usunąć, uruchom następujące polecenie w konsoli zarządzania Pakietami:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Ten pakiet został przemianowane, tak jak `Microsoft.AspNet.WebHelpers`. Aby usunąć, uruchom następujące polecenie w konsoli zarządzania Pakietami:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Ten pakiet zawiera obejścia dla usterki w MVC 4, który został rozwiązany w MVC 5. Aby usunąć, uruchom następujące polecenie w konsoli zarządzania Pakietami:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Uaktualnij pakiety ASP.NET NuGet za pomocą konsoli zarządzania Pakietami. W konsoli zarządzania Pakietami uruchom następujące polecenie:  
    `Update-Package`  
   `Update-Package` Polecenia bez parametrów spowoduje zaktualizowanie każdego pakietu. Pakietów można zaktualizować osobno za pomocą argumentu identyfikator. Aby uzyskać więcej informacji na temat polecenia aktualizacji uruchom `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Zaktualizuj aplikację *web.config* pliku

Pamiętaj wprowadzić te zmiany w aplikacji *web.config* plik bez *web.config* w pliku *widoków* folderu.

Znajdź `<runtime>/<assemblyBinding>` sekcji, a następnie dokonaj następujących zmian:

1. W elementach z atrybut name "System.Web.Mvc" należy zmienić numer wersji z "4.0.0.0" do "5.0.0.0". (Dwie zmiany w tym elemencie).
2. W elementach z atrybutem nazwy &quot;System.Web.Helpers "i &quot;System.Web.WebPages&quot; zmienić numer wersji z"2.0.0.0"do"3.0.0.0". Cztery mają miejsce zmiany, dwa w poszczególnych elementów.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Znajdź `<appSettings>` sekcji i zaktualizuj webpages:version z 2.0.0.0.0 do 3.0.0.0, jak pokazano poniżej:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Usuń wszystkie poziomy zaufania inne niż pełne. Na przykład:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Aktualizacja *web.config* pliki w folderze Widoki

Jeśli aplikacja używa obszarów, również należy uaktualnić każdy *web.config* w pliku *widoków* podfolder każdego folderu obszaru.

1. Zaktualizuj wszystkie elementy, które zawierają "System.Web.Mvc" z wersji "4.0.0.0" do wersji "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Zaktualizuj wszystkie elementy, które zawierają "System.Web.WebPages.Razor" z wersji "2.0.0.0" do wersji "3.0.0.0". Jeśli ta sekcja zawiera "System.Web.WebPages", należy zaktualizować te elementy z wersji "2.0.0.0" do wersji "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Jeśli usunięto `Microsoft-Web-Helpers` zainstaluj pakiet NuGet w poprzednim kroku `Microsoft.AspNet.WebHelpers` za pomocą następującego polecenia w konsoli zarządzania Pakietami:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Jeśli aplikacja używa [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) metody, Dodaj następujący kod do *Web.config* pliku.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Ostatnie kroki

Tworzenie i testowanie aplikacji.

Usuń identyfikator GUID typu projektu MVC 4 z plików projektu.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **Zwolnij projekt**.
2. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję Edytuj ProjectName.csproj.
3. Znajdź `ProjectTypeGuids` elementu, a następnie usuń MVC 4 projekt identyfikatora GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Zapisz i zamknij plik Otwórz projekt.
5. Kliknij prawym przyciskiem myszy projekt i wybierz **Załaduj ponownie projekt**.
