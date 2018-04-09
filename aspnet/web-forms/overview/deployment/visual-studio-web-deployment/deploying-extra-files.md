---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak rozszerzyć potoku do wykonywania dodatkowych zadań podczas wdrażania programu Visual Studio publikowania w sieci web. Zadanie służy do kopiowania dodatkowe pliki, które nie znajdują się w folderze projektu do docelowej witryny sieci web.

W tym samouczku będzie skopiować jeden dodatkowy plik: *robots.txt*. Chcesz wdrożyć ten plik do etapu przemieszczania, ale nie do środowiska produkcyjnego. W [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) samouczek, dodać ten plik do projektu i skonfigurować produkcji profilu, aby wykluczyć go opublikować. W tym samouczku zobaczysz alternatywną metodą aby obsłużyć taką sytuację, który będą przydatne w przypadku wszystkie pliki, które mają zostać wdrożone, ale nie mają zostać uwzględnione w projekcie.

## <a name="move-the-robotstxt-file"></a>Przenieś plik robots.txt

Aby przygotować się do innej metody obsługi *robots.txt*, w tej części samouczka plik zostanie przeniesiony do folderu, który nie jest dołączony do projektu i usunięciu *robots.txt* z obszaru przemieszczania środowisko. Należy usunąć go z tymczasowej tak, aby sprawdzić, czy nowych metod wdrażania pliku w tym środowisku działa poprawnie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *robots.txt* plik i kliknij przycisk **wykluczyć z projektu**.
2. Za pomocą Eksploratora plików systemu Windows, Utwórz nowy folder w folderze rozwiązania i nadaj jej nazwę *ExtraFiles*.
3. Przenieś *robots.txt* plik z *ContosoUniversity* folderu projektu do *ExtraFiles* folderu.

    ![ExtraFiles folder](deploying-extra-files/_static/image1.png)
4. Za pomocą własnych narzędzi FTP usunąć *robots.txt* plików tymczasowych witryny sieci web.

    Alternatywnie, można wybrać **Usuń dodatkowe pliki w miejscu docelowym** w obszarze **opcji publikowania pliku** na **ustawienia** kartę przemieszczania profil publikowania, i ponownie opublikować do etapu przemieszczania.

## <a name="update-the-publish-profile-file"></a>Zaktualizuj profil publikowania

Wystarczy *robots.txt* w przejściowym, więc przemieszczania jest tylko profil publikowania, musisz zaktualizować w celu jej wdrożenia.

1. W programie Visual Studio Otwórz *Staging.pubxml*.
2. Na koniec pliku przed tagiem zamykającym `</Project>` tagów, Dodaj następujący kod:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Ten kod tworzy nową *docelowej* który będzie zbierać dodatkowe pliki do wdrożenia. Element docelowy składa się z jednego lub więcej zadań, które wykona MSBuild na podstawie warunków przez użytkownika.

    `Include` Atrybut określa, że folder, w którym można znaleźć plików *ExtraFiles*, który znajduje się na tym samym poziomie co folder projektu. MSBuild zbierze wszystkie pliki z tego folderu i rekursywnie z podfoldery (podwójną gwiazdką określa cykliczne podfolderów). Ten kod może narażać wielu plików i plików w podfolderach wewnątrz *ExtraFiles* folder i wszystkie zostanie wdrożony.

    `DestinationRelativePath` Element określa, że plików i folderów powinien zostać skopiowany do folderu głównego w witrynie sieci web, w tej samej strukturze plików i folderów, ponieważ występują w *ExtraFiles* folderu. Jeśli chcesz skopiować *ExtraFiles* sam folder, `DestinationRelativePath` wartość będzie *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Na koniec pliku przed tagiem zamykającym `</Project>` tagów, Dodaj następujący kod znaczników, który określa, kiedy można wykonać nowy element docelowy.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Ten kod powoduje, że nowe `CustomCollectFiles` docelowych i wykonywane przy każdym wykonaniu docelowego, który kopiuje pliki do folderu docelowego. Brak oddzielnych cel publikowania i tworzenia pakietu wdrożenia i nowy obiekt docelowy jest wprowadzonym w obu elementów docelowych w przypadku, gdy użytkownik chce wdrożyć przy użyciu pakietu wdrożeniowego zamiast publikowania.

    *.Pubxml* pliku teraz wygląda następująco:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Zapisz i Zamknij *Staging.pubxml* pliku.

## <a name="publish-to-staging"></a>Publikowanie do etapu przemieszczania

Publikowanie za pomocą jednego kliknięcia lub wiersza polecenia, publikowanie aplikacji przy użyciu profilu tymczasowego.

Jeśli korzystasz z jednym kliknięciem opublikować, można sprawdzić w **Podgląd** okna który *robots.txt* zostaną skopiowane. W przeciwnym razie użyj narzędzia do FTP do sprawdzenia, czy *robots.txt* plik znajduje się w folderze głównym witryny sieci web po wdrożeniu.

## <a name="summary"></a>Podsumowanie

Na tym kończy się tej serii samouczków dotyczących wdrażania aplikacji sieci web ASP.NET do innego dostawcy hostingu. Aby uzyskać więcej informacji o tych tematów objęte te samouczki, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Więcej informacji

Jeśli wiesz, jak pracować z plikami programu MSBuild, można zautomatyzować wiele innych zadań wdrożeniowych pisanie kodu w *.pubxml* plików (w przypadku zadania specyficzne dla profilu) lub projektu *. wpp.targets* pliku (dla zadań związanych z zarządzaniem Zastosuj do wszystkich profilów). Aby uzyskać więcej informacji na temat *.pubxml* i *. wpp.targets* plików, zobacz [porady: edytowanie ustawień wdrażania w plikach profilu publikacji (.pubxml) i. wpp.targets pliku w Visual Studio Web Projekty](https://msdn.microsoft.com/library/ff398069). Podstawowe wprowadzenie do programu MSBuild kod, zobacz **Anatomia pliku projektu** w [serii wdrożenia przedsiębiorstwa: opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Aby dowiedzieć się, jak pracować z plikami MSBuild do wykonywania zadań własnych scenariuszy, zobacz ten podręcznik: [wewnątrz kompilacji aparatu Microsoft: przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi i Bartholomew łączy.

## <a name="acknowledgements"></a>Potwierdzeń

Chcę Dziękujemy następujących osób istotny wkład do zawartości z tego samouczka serii:

- [Alberto Poblacion, MVP &amp; MCT, (Hiszpania)](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, dane aplikacji na wiele Platform MVP, Stany Zjednoczone
- Harsh Mittal, Microsoft
- [Jan Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italy](http://www.iamraf.net/)
- [Rick Anderson firmy Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott myśliwego, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Poprzednie](command-line-deployment.md)
> [dalej](troubleshooting.md)
