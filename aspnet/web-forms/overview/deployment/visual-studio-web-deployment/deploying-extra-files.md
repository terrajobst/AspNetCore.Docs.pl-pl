---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820853"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak rozszerzyć potoku w celu dodatkowego zadania podczas wdrażania programu Visual Studio, publikowanie w sieci web. Zadanie jest do skopiowania dodatkowe pliki, które nie znajdują się w folderze projektu do docelowej witryny sieci web.

W tym samouczku będzie skopiować jeden dodatkowy plik: *robots.txt*. Chcesz wdrożyć ten plik do wdrażania przejściowego, ale nie do środowiska produkcyjnego. W [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) samouczków, dodać ten plik do projektu i skonfigurowano produkcji profilu w celu wykluczenia go opublikować. W tym samouczku zobaczysz alternatywną metodę, aby obsłużyć taką sytuację, taki, który będzie przydatna dla wszystkich plików, które mają zostać wdrożone, ale nie powinny zostać uwzględnione w projekcie.

## <a name="move-the-robotstxt-file"></a>Przenieś plik robots.txt

Aby przygotować się do innej metody obsługi *robots.txt*, w tej części samouczka plik zostanie przeniesiony do folderu, który nie znajduje się w projekcie i usuniesz *robots.txt* z obszaru przemieszczania środowisko. Należy usunąć plik z tymczasowej, aby sprawdzić, czy nową metodę wdrażania pliku w tym środowisku działa poprawnie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *robots.txt* plik i kliknij przycisk **Wyklucz z projektu**.
2. Za pomocą Eksploratora plików Windows, Utwórz nowy folder w folderze rozwiązania i nadaj mu nazwę *ExtraFiles*.
3. Przenieś *robots.txt* plik wchodzącej w skład *ContosoUniversity* folderu projektu do *ExtraFiles* folderu.

    ![ExtraFiles folder](deploying-extra-files/_static/image1.png)
4. Za pomocą narzędzia usługi FTP, Usuń *robots.txt* plik tymczasowej witryny sieci web.

    Alternatywnie, możesz wybrać **Usuń dodatkowe pliki w lokalizacji docelowej** w obszarze **opcji publikowania pliku** na **ustawienia** karta profilu publikowania przemieszczania, i ponownie opublikować do wdrażania przejściowego.

## <a name="update-the-publish-profile-file"></a>Aktualizowanie pliku profilu publikowania

Dzięki temu wystarczy *robots.txt* przejściowy, dlatego przemieszczania jest to jedyny profil publikowania należy zaktualizować w celu jej wdrożenia.

1. W programie Visual Studio, otwórz *Staging.pubxml*.
2. Na koniec pliku przed tagiem zamykającym `</Project>` tag, Dodaj następujący kod:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Ten kod tworzy nową *docelowej* który będzie zbierać dodatkowe pliki do wdrożenia. Obiekt docelowy składa się z jednego lub większej liczby zadań, wykonujących MSBuild na podstawie warunków przez użytkownika.

    `Include` Atrybut określa, że folder, w którym można znaleźć plików *ExtraFiles*, który znajduje się na tym samym poziomie jak folder projektu. Program MSBuild będzie zbierać wszystkie pliki z tego folderu i cyklicznie w podfolderach (podwójny gwiazdki określa cykliczne podfolderów). Przy użyciu tego kodu można umieścić wielu plików i pliki w podfolderach wewnątrz *ExtraFiles* folder i wszystkie zostaną wdrożone.

    `DestinationRelativePath` Element określa, że foldery i pliki zostaną skopiowane do folderu głównego w witrynie sieci web, w tej samej strukturze plików i folderów znajdują się one w *ExtraFiles* folderu. Jeśli chcesz skopiować *ExtraFiles* sam folder, `DestinationRelativePath` wartość będzie wynosić *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Na koniec pliku przed tagiem zamykającym `</Project>` tag, Dodaj następujący kod znaczników, który określa, kiedy należy wykonać nowy obiekt docelowy.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Ten kod powoduje, że nowe `CustomCollectFiles` docelowych, które mają być wykonane w każdym przypadku, gdy docelowy, który kopiuje pliki do folderu docelowego jest wykonywany. Ma oddzielne docelowe do publikowania i tworzenie pakietu wdrożenia i nowy obiekt docelowy jest wprowadzony w obu elementów docelowych, w przypadku, gdy użytkownik chce wdrożyć przy użyciu pakietu wdrożeniowego zamiast publikowania.

    *.Pubxml* pliku wygląda teraz następująco:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Zapisz i Zamknij *Staging.pubxml* pliku.

## <a name="publish-to-staging"></a>Publikowanie w środowisku przejściowym

Publikowanie za pomocą jednego kliknięcia lub wiersza polecenia, Opublikuj aplikację, korzystając z profilu tymczasowego.

Jeśli używasz jednym kliknięciem publikować, można sprawdzić w **Podgląd** okna, *robots.txt* zostaną skopiowane. W przeciwnym razie użyj narzędzie FTP, aby sprawdzić, czy *robots.txt* plik znajduje się w folderze głównym witryny sieci web po wdrożeniu.

## <a name="summary"></a>Podsumowanie

Na tym kończy się w tej serii samouczków na temat wdrażania aplikacji sieci web ASP.NET u dostawcy hostingu innych firm. Aby uzyskać więcej informacji na temat dowolny z tematów omówione w tych samouczkach zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Więcej informacji

Jeśli wiesz, jak pracować z plikami programu MSBuild, możesz zautomatyzować wiele zadań wdrażania przez napisanie kodu *.pubxml* plików (w przypadku zadań specyficznych dla danego profilu) lub projektu *. wpp.targets* pliku (dla zadania, mają zastosowanie do wszystkich profilów). Aby uzyskać więcej informacji na temat *.pubxml* i *. wpp.targets* plików, zobacz [porady: edytowanie ustawień wdrażania w plikach profilu publikacji (.pubxml) i. wpp.targets pliku w Visual Studio Web Projekty](https://msdn.microsoft.com/library/ff398069). Podstawowe wprowadzenie do kodu programu MSBuild, zobacz **Anatomia pliku projektu** w [wdrożenia w przedsiębiorstwie, seria: objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Aby dowiedzieć się, jak pracować z plikami programu MSBuild do wykonywania zadań dla własnych scenariuszy, zobacz tę książkę: [wewnątrz aparatu Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi i William Bartholomew.

## <a name="acknowledgements"></a>Potwierdzanie

Chcę otrzymywać podziękować następujących osób, które istotny wkład w zawartości w tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, programowanie na platformie danych MVP, Stany Zjednoczone
- Ostrym Mittal, Microsoft
- [Jan Galloway'em](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Włochy](http://www.iamraf.net/)
- [Rick Anderson firmy Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Poprzednie](command-line-deployment.md)
> [dalej](troubleshooting.md)
