---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrowanie danych uniwersalnego dostawcy dotyczących członkostwa i profilów użytkowników do produktu ASP.NET Identity (C#) | Dokumentacja firmy Microsoft
author: rustd
description: W tym samouczku opisano kroki, które są niezbędne przeprowadzić migrację użytkowników i dane roli i danych profilów użytkowników utworzone przy użyciu dostawców uniwersalnych z istniejącej aplikacji...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756252"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrowanie danych uniwersalnego dostawcy dotyczących członkostwa i profilów użytkowników do produktu ASP.NET Identity (C#)
====================
przez [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku opisano kroki, które są niezbędne przeprowadzić migrację użytkowników i dane roli i danych profilów użytkowników utworzone przy użyciu dostawców uniwersalnych istniejącej aplikacji do modelu tożsamości ASP.NET. Podejście wspomnianych tutaj, aby przeprowadzić migrację danych profilu użytkownika mogą być używane w aplikacji przy użyciu członkostwa SQL, jak również.


Wraz z wydaniem programu Visual Studio 2013, zespół programu ASP.NET wprowadzono nowy system tożsamości ASP.NET, i możesz dowiedzieć się więcej o tej wersji [tutaj](../../index.md). Po tego artykułu, aby przeprowadzić migrację aplikacji sieci web z [członkostwa SQL do nowego systemu tożsamości](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), w tym artykule przedstawiono kroki, aby migrować istniejące aplikacje, które należy wykonać model dostawców dla użytkownika i roli zarządzania do nowego modelu tożsamości. Celem tego samouczka będą się głównie na temat migracji danych profilu użytkownika, aby bezproblemowo podłączyć ją do nowego systemu. Migrowanie informacji o użytkowniku i roli jest podobny do członkostwa SQL. Podejście do migracji danych profilu może służyć w aplikacji za pomocą także członkostwa SQL.

Na przykład Zaczniemy z aplikacją sieci web utworzonych za pomocą programu Visual Studio 2012, który używa modelu dostawców. Firma Microsoft będzie, a następnie dodać kod do zarządzania profilami, zarejestruj użytkownika, dodać dane profilowe dla użytkowników, migrować schemat bazy danych, a następnie zmień aplikacji do systemu tożsamości użytkownika i roli zarządzania. Jako test migracji użytkownicy utworzeni za pomocą dostawców uniwersalnych powinien móc zalogować się i nowych użytkowników powinny mieć możliwość rejestrowania.

> [!NOTE]
> Możesz znaleźć pełny przykład o [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Podsumowanie migracji danych profilu

Przed rozpoczęciem z migracje, Przyjrzyjmy się środowisko przechowywania danych profilu w dostawcy modelu. Dane profilu dla aplikacji, które użytkownicy mogą być przechowywane na wiele sposobów, najczęściej występujące między nimi są przy użyciu dostawców wbudowanych profilu dostarczany wraz z dostawców uniwersalnych. Obejmuje kroki

1. Dodaj klasę, która ma właściwości, używany do przechowywania danych profilu.
2. Dodaj klasę, która rozszerza "ProfileBase" i implementuje metody w celu uzyskania powyższe dane profilu dla użytkownika.
3. Włącz przy użyciu domyślnych dostawców profilu w *web.config* plików i definiowania klasy zadeklarowanej w kroku 2 # ma być używany podczas uzyskiwania dostępu do informacji o profilu.

Informacje o profilu są przechowywane jako serializacji xml i danych binarnych w tabeli "Profile" w bazie danych.

Po przeprowadzeniu migracji aplikacji do korzystania z nowego systemu tożsamości ASP.NET, informacje o profilu jest przeprowadzona i przechowywane jako właściwości klasy użytkownika. Każda właściwość następnie mogą być mapowane na kolumny w tabeli użytkownika. Korzyści, w tym miejscu jest, że właściwości mogą pracować na bezpośrednio przy użyciu klasy użytkownika oprócz nie ma potrzeby serializowania/deserializowania danych informacji czas podczas dostępu do niego.

## <a name="getting-started"></a>Wprowadzenie

1. Utwórz nową aplikację ASP.NET 4.5 Web Forms w programie Visual Studio 2012. Bieżąca przykładowa używa szablonu formularzy sieci Web, ale można również użyć aplikacji MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Utwórz nowy folder modeli, do przechowywania informacji o profilu  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Na przykład przechowywać Daj nam daty urodzenia, Miasto, wysokość i wagi użytkownika w profilu. Wysokość i wagi są przechowywane jako klasa niestandardowa o nazwie "PersonalStats". Do przechowywania i pobierania profilu, potrzebujemy klasę, która rozszerza "ProfileBase". Utwórz nową klasę "AppProfile", aby pobrać i zapisać informacje o profilu.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Włącz profil w *web.config* pliku. Wprowadź nazwę klasy, która ma być używany do przechowywania/pobierania informacji o użytkowniku, utworzonego w kroku #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Dodaj strony formularzy sieci web w folderze "Konto", aby pobrać dane profilu użytkownika i zapisz go. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję "Dodaj nowy element". Dodawanie nowej strony formularzy sieci Web za pomocą strony wzorcowej "AddProfileData.aspx". W sekcji "MainContent", należy skopiować następujące czynności:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Dodaj następujący kod w kodzie:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Dodaj przestrzeń nazw, w ramach której AppProfile klasa jest zdefiniowana, aby usunąć błędy kompilacji.
6. Uruchom aplikację i Utwórz nowego użytkownika przy użyciu nazwy użytkownika "**użytkownika olduser".** Przejdź do strony "AddProfileData" i Dodaj informacje o profilu użytkownika.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Aby sprawdzić, czy dane są przechowywane jako serializacji xml w tabeli "Profile" w oknie Eksploratora serwera. W programie Visual Studio z menu "Widok" Wybierz "Eksploratora serwera". Powinna istnieć połączenie danych dla bazy danych zdefiniowanych w *web.config* pliku. Kliknięcie na połączenie danych pokazuje pod różnych kategorii. Rozwiń węzeł "Tabele", aby pokazać różnych tabel w bazie danych, a następnie kliknij prawym przyciskiem "Profile" i wybierz pozycję "Pokaż dane tabeli" Aby wyświetlić dane profilu, przechowywane w tabeli profilów.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrowanie schematu bazy danych

Aby istniejącą bazę danych, pracy z systemem obsługi tożsamości, musimy zaktualizować schematu w bazie danych tożsamości do obsługi pól, zapoczątkowany w oryginalnej bazie danych. Można to zrobić za pomocą skryptów SQL, aby tworzyć nowe tabele i skopiować informacje o istniejącym. W oknie "Eksploratora serwera" rozwiń "DefaultConnection" do wyświetlania w tabelach. Kliknij prawym przyciskiem myszy tabele, a następnie wybierz pozycję "Nowe zapytanie"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Wklej skrypt SQL z [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) i uruchomimy ją. "DefaultConnection" jest odświeżana, widać, są dodawane nowe tabele. Możesz sprawdzić danych używany wewnątrz tabel, aby zobaczyć, że dane zostały poddane migracji.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrowanie aplikacji do korzystania z produktu ASP.NET Identity

1. Zainstaluj pakiety Nuget służące do produktu ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Więcej informacji na temat zarządzania pakietami Nuget można znaleźć [tutaj](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Aby pracować z istniejącymi danymi w tabeli, należy utworzyć klasy modelu, które mapowania tabel i obsługiwać je w systemie tożsamości. W ramach umowy tożsamości klasy modelu albo powinny implementować interfejsy zdefiniowane w pliku dll Identity.Core lub rozszerzyć istniejącą implementację tych interfejsów, które są dostępne w Microsoft.AspNet.Identity.EntityFramework. Firma Microsoft będzie używać istniejących klas dla roli, danymi logowania użytkowników i oświadczeń użytkowników. Należy użyć niestandardowych użytkownika w naszym przykładzie. Kliknij prawym przyciskiem myszy projekt i Utwórz nowy folder "IdentityModels". Dodaj nową klasę "User", jak pokazano poniżej:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Zauważ, że ProfileInfo teraz właściwość klasy użytkownika. Dlatego możemy użyć klasy użytkownika bezpośrednio pracować z danymi profilu.

Skopiuj pliki z **IdentityModels** i **IdentityAccount** foldery ze źródła pobierania ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Muszą one być pozostałe klasy modelu i nowe strony, które są wymagane przez użytkownika i zarządzanie rolami przy użyciu interfejsów API tożsamości ASP.NET. Metoda, używana jest podobny do członkostwa SQL oraz szczegółowy opis można znaleźć [tutaj](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopiowanie danych z profilu do nowych tabel

Jak wspomniano wcześniej, należy zdeserializować danych xml w tabelach profile i zapisz go w kolumnach tabeli AspNetUsers. Nowe kolumny zostały utworzone w tabeli użytkowników w poprzednim kroku, więc wszystko, co zostało do wypełniania tych kolumn niezbędnych danych. W tym celu użyjemy aplikację konsolową, która jest uruchamiane jeden raz, aby wypełnić nowo utworzonej kolumny w tabeli użytkowników.

1. Utwórz nową aplikację konsoli w istniejącej rozwiązania.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Zainstaluj najnowszą wersję pakietu programu Entity Framework.
3. Dodaj aplikację sieci web utworzonego powyżej jako odwołanie do aplikacji konsoli. Aby zrobić to kliknij prawym przyciskiem myszy projekt, a następnie "Dodaj odwołania do", następnie rozwiązania, następnie kliknij przyciskiem myszy projekt i kliknij przycisk OK.
4. Kopiuj poniższego kodu w klasie pliku Program.cs. Tę logikę odczytuje dane profilu dla każdego użytkownika, serializuje go jako obiekt "ProfileInfo" i zapisuje je w bazie danych.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Niektóre modele są definiowane w folderze "IdentityModels" projektu aplikacji sieci web, więc należy dołączyć odpowiednie przestrzenie nazw.
5. Powyższy kod działa na plik bazy danych w aplikacji\_folderu danych projektu aplikacji sieci web utworzony w poprzednich krokach. Aby odwołać do tego, należy zaktualizować parametry połączenia w pliku app.config aplikacji konsolowej przy użyciu parametrów połączenia w pliku web.config aplikacji sieci web. Zapewniają również Pełna fizyczna ścieżka we właściwości "AttachDbFilename".
6. Otwórz wiersz polecenia i przejdź do folderu bin powyżej aplikacji konsoli. Uruchom plik wykonywalny i przejrzyj dane wyjściowe dziennika, jak pokazano na poniższej ilustracji.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Otwórz w tabeli "AspNetUsers" w Eksploratorze serwera i sprawdź dane w nowych kolumn, które posiadają właściwości. One powinny być aktualizowane przy użyciu wartości odpowiednich właściwości.

## <a name="verify-functionality"></a>Sprawdzenie działania

Użyj stron członkostwa nowo dodane, które są implementowane za pomocą tożsamości ASP.NET można zalogować użytkownika ze starej bazy danych. Użytkownik powinien móc logować się przy użyciu tych samych poświadczeń. Wypróbuj inne funkcje, takie jak dodawanie uwierzytelniania OAuth, tworzenie nowego użytkownika, zmiana haseł, Dodawanie ról, dodawanie użytkowników do ról, itp.

Dane profilu dla użytkownika starych i nowych użytkowników, należy pobrać i przechowywane w tabeli użytkowników. Stary tabeli nie powinny istnieć odwołania.

## <a name="conclusion"></a>Wniosek

Tego artykułu opisano proces migracji aplikacji sieci web, które są używane modelu Dostawca członkostwa do produktu ASP.NET Identity. Tego artykułu opisano dodatkowo migrowanie danych profilu dla użytkowników być dołączane do systemu tożsamości. Zostaw komentarz poniżej pytania i problemy występujące podczas migracji aplikacji.

*Dziękujemy za Rick Anderson i Robert McMurray przeglądania tego artykułu.*
