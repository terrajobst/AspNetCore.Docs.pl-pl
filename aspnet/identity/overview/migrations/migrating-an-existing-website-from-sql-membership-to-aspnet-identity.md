---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrowanie istniejącej witryny sieci Web z członkostwa SQL do produktu ASP.NET Identity | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku przedstawiono kroki, aby migrować istniejącą aplikację sieci web z użytkownika i dane roli utworzone za pomocą członkostwa SQL do nowego produktu ASP.NET Identity s...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 393d14799973e9126379743f63f79a7131206f38
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577616"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrowanie istniejącej witryny sieci Web z członkostwa SQL do produktu ASP.NET Identity
====================
przez [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku przedstawiono kroki, aby migrować istniejącą aplikację sieci web z użytkownika i dane roli utworzone za pomocą członkostwa SQL do nowego systemu produktu ASP.NET Identity. Ta strategia polega na Zmienianie istniejącego schematu bazy danych do jednej wymaganej przez punkt zaczepienia w starym/nowe klasy i tożsamości ASP.NET. Po przyjmiesz takie podejście, po migracji bazy danych, bez wysiłku obsługi przyszłe aktualizacje tożsamości.


W tym samouczku podejmujemy szablonu aplikacji sieci web (formularze sieci Web) utworzone za pomocą programu Visual Studio 2010, aby utworzyć dane użytkownika i roli. Następnie użyjemy skrypty SQL przeprowadzić migrację istniejącej bazy danych do tabel wymaganych przez system tożsamości. Następnie utworzymy zainstalowania niezbędnych pakietów NuGet i dodawać nowe strony zarządzania konta, które używają systemu tożsamości do zarządzania członkostwa. Jako test migracji użytkownicy utworzeni za pomocą członkostwa SQL powinna być mogli się zalogować i nowych użytkowników powinny mieć możliwość rejestrowania. Pełny przykład można znaleźć [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Zobacz też [migracji z członkostwa ASP.NET do produktu ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Wprowadzenie

### <a name="creating-an-application-with-sql-membership"></a>Tworzenie aplikacji z członkostwa SQL

1. Należy uruchomić z istniejącą aplikacją, która korzysta z członkostwa SQL i zawiera dane użytkownika i roli. Na potrzeby tego artykułu Utwórz aplikację sieci web w programie Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Za pomocą narzędzia Konfiguracja programu ASP.NET, Utwórz użytkowników 2: **oldAdminUser** i **użytkownika oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Utwórz rolę o nazwie administratora i Dodaj "oldAdminUser" jako użytkownik w tej roli.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Utwórz sekcję administratora witryny za pomocą Default.aspx. Ustaw znacznik autoryzacji w pliku web.config, aby umożliwić dostęp tylko do użytkowników w rolach administratora. Więcej informacji można znaleźć tutaj [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Wyświetl bazy danych w Eksploratorze serwera, aby zrozumieć tabele utworzone przez system członkostwa SQL. Dane logowania użytkownika są przechowywane w aspnet\_użytkowników i aspnet\_tabele członkostwa, gdy rola dane są przechowywane w aspnet\_tabeli ról. Informacje o tym, które użytkownicy są, w które role są przechowywane w aspnet\_UsersInRoles tabeli. Do zarządzania członkostwa podstawowego jest wystarczające do portu dane w tabelach powyżej w systemie produktu ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migracja do programu Visual Studio 2013

1. Instalowanie programu Visual Studio Express 2013 for Web lub Visual Studio 2013, wraz z [najnowsze aktualizacje](https://www.microsoft.com/download/details.aspx?id=44921).
2. Otwórz projekt powyżej w zainstalowanej wersji programu Visual Studio. Jeśli na maszynie nie zainstalowano programu SQL Server Express, monit jest wyświetlany po otwarciu projektu, ponieważ ciąg połączenia używany program SQL Express. Można albo zainstalować program SQL Express lub jako obejścia Zmień parametry połączenia do LocalDb. W tym artykule zmienimy je do LocalDb.
3. Otwórz plik web.config, a następnie zmień parametry połączenia z. SQLExpess do (LocalDb) w wersji 11.0. Usuń "wystąpienie użytkownika = true" z ciągu połączenia.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Otwórz Eksploratora serwera i sprawdź, czy można zaobserwować schematu tabeli i danych.
5. System tożsamości ASP.NET działa w wersji 4.5 lub nowszej Framework. Przekierować aplikację do wersji 4.5 lub nowszej.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy.

### <a name="installing-the-nuget-packages"></a>Instalowanie pakietów Nuget

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt &gt; **Zarządzaj pakietami NuGet**. W polu wyszukiwania wprowadź "Produktu Asp.net Identity". Wybierz pakiet, na liście wyników, a następnie kliknij przycisk Instaluj. Zaakceptuj Umowę licencyjną, klikając przycisk "Zgadzam się". Należy pamiętać, że ten pakiet zainstaluje pakiety zależności: EntityFramework i tożsamość Microsoft ASP.NET w wersji podstawowej. Podobnie, zainstaluj następujące pakiety (Pomiń ostatnich 4 pakietów OWIN, jeśli nie chcesz włączyć logowania OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migracja bazy danych do nowego systemu tożsamości

Następnym krokiem jest przeprowadzić migrację istniejącej bazy danych do schematu wymagane przez system tożsamości ASP.NET. Do osiągnięcia to przeprowadzamy SQL skrypt, który zawiera zestaw poleceń, aby utworzyć nowe tabele i przeprowadzić migrację istniejących informacji o użytkowniku do nowych tabel. Można znaleźć pliku skryptu [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Ten przykład dotyczy tego pliku skryptu. Jeśli schemat dla tabel utworzony przy użyciu członkostwa SQL został dostosowany lub zmodyfikowane konieczności skryptów można odpowiednio zmienić.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Jak można wygenerować skryptu SQL do migracji schematu

Dla produktu ASP.NET Identity klas do pracy z usługą danych istniejących użytkowników należy migrować schemat bazy danych do tego wymagane przez tożsamości ASP.NET. Możemy to zrobić, dodając nowe tabele i skopiowanie istniejących informacji tych tabel. Domyślnie tożsamości ASP.NET używa EntityFramework do mapowania klas modelu tożsamości w bazie danych do przechowywania/pobierania informacji. Te klasy modelu implementować interfejsy tożsamości podstawowe, definiowania użytkownika i obiektów roli. Tabele i kolumny w bazie danych są oparte na te klasy modelu. Klasy modeli EntityFramework v2.1.0 tożsamości i ich właściwości są zdefiniowane poniżej

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | string | Id | RoleId | ProviderKey | Id |
| Nazwa użytkownika | string | Nazwa | Identyfikator użytkownika | Identyfikator użytkownika | Typ oświadczenia |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Użytkownik\_identyfikator |
| Adres e-mail | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Numer telefonu | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DataGodzina |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Musimy mieć tabel dla każdej z tych modeli z kolumnami odpowiadające właściwości. Mapowanie między klasami i tabelami jest zdefiniowany w `OnModelCreating` metody `IdentityDBContext`. Jest to określane jako metoda interfejsu API fluent, konfiguracji i więcej informacji można znaleźć [tutaj](https://msdn.microsoft.com/data/jj591617.aspx). Konfiguracja dla klasy jest zagadnień wymienionych poniżej.

| **Class** | **Tabela** | **Klucz podstawowy** | **Klucz obcy** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | Nazwa użytkownika i roli o identyfikatorze | Użytkownik\_identyfikator -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | Identyfikator użytkownika —&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Użytkownik\_identyfikator -&gt;AspnetUsers |

Dzięki tym informacjom można utworzyć instrukcje SQL, aby tworzyć nowe tabele. Firma Microsoft indywidualnie zapis każdej instrukcji lub generowanie cały skrypt za pomocą poleceń programu PowerShell platformy EntityFramework, które możemy następnie poddać edycji zgodnie z potrzebami. W tym celu w otwartym VS **Konsola Menedżera pakietów** z **widoku** lub **narzędzia** menu

- Aby uruchomić polecenie "Enable-Migrations" Włączanie funkcji migracje platformy EntityFramework.
- Uruchom polecenie "Dodaj migracji początkowy", który tworzy kod początkowej konfiguracji, aby utworzyć bazę danych w języku C# / VB.
- Ostatnim krokiem jest uruchomienie "Update-Database — skrypt" polecenie, które generuje skrypt SQL oparte na klasy modelu.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Ten skrypt generowanie bazy danych może służyć jako początek, w którym firma Microsoft będzie wykonywać dodatkowe zmiany, aby dodać nowe kolumny, a następnie skopiować dane. Dzięki temu jest możemy wygenerować `_MigrationHistory` tabeli, który jest używany przez EntityFramework do modyfikacji schematu bazy danych, gdy model klasy zmiany w przyszłych wersjach programu wersje tożsamości. 

Informacje o użytkowniku członkostwa SQL ma inne właściwości, oprócz tych, które w klasie modelu tożsamości użytkownika: wiadomości e-mail, prób podania hasła, Data ostatniego logowania, Data ostatniej blokady itp. Jest to przydatne informacje, a następnie prosimy o poświęcenie jej mają zostać przeniesione do systemu tożsamości. Można to zrobić przez dodanie dodatkowych właściwości do modelu użytkowników i ich mapowanie do kolumny tabeli w bazie danych. Możemy to zrobić, dodając klasę tej podklasy `IdentityUser` modelu. Firma Microsoft można dodać właściwości do klasy niestandardowej i Edytuj skrypt SQL w celu dodania odpowiednich kolumn podczas tworzenia tabeli. Kod dla tej klasy jest dokładniejszym opisem zawartym w artykule. Skrypt SQL do tworzenia `AspnetUsers` tabeli po dodając nowe właściwości będą

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Następnie należy skopiować informacje o istniejącym z członkostwa w bazie danych SQL do nowo dodanego tabel dla tożsamości. Można to zrobić za pomocą języka SQL przez skopiowanie danych bezpośrednio z jednej tabeli. Aby dodać dane w wiersze tabeli, użyjemy `INSERT INTO [Table]` konstruowania. Aby skopiować z innej tabeli, możemy użyć `INSERT INTO` instrukcji wraz z `SELECT` instrukcji. Aby uzyskać wszystkie informacje o użytkowniku musimy zbadać *aspnet\_użytkowników* i *aspnet\_członkostwa* tabele, a następnie skopiuj dane do *AspNetUsers*tabeli. Używamy `INSERT INTO` i `SELECT` wraz z `JOIN` i `LEFT OUTER JOIN` instrukcji. Aby uzyskać więcej informacji na temat tworzenia zapytań i kopiowanie danych między tabelami, zobacz [to](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) łącza. Ponadto w tabelach AspnetUserLogins i AspnetUserClaims są puste rozpoczynać się, ponieważ nie ma dostępnych informacji w członkostwa SQL, która mapuje do tego domyślnie. Wszystkie informacje kopiowany jest dla użytkowników i ról. Projekt utworzony w poprzednich krokach będzie zapytanie SQL, aby kopiowanie informacji do tabeli użytkowników

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

W instrukcji SQL z powyższych informacji na temat każdego użytkownika z *aspnet\_użytkowników* i *aspnet\_członkostwa* tabele są kopiowane do kolumn  *AspnetUsers* tabeli. Modyfikacja tylko wykonywane na w tym miejscu jest, gdy firma Microsoft Skopiuj hasło. Ponieważ algorytm szyfrowania haseł w członkostwa SQL używany "PasswordSalt" i "PasswordFormat", możemy skopiować, zbyt wraz ze skrótem hasła, aby może służyć do odszyfrowywania hasła przez tożsamość. Jest to wyjaśnione bardziej szczegółowo w artykule, gdy Podłączanie hasher hasła niestandardowego. 

Ten przykład dotyczy tego pliku skryptu. W przypadku aplikacji, które posiada dodatkowe tabele deweloperów wykonać podejście podobne do dodawania dodatkowych właściwości klasy modelu użytkownika i zamapować do kolumny w tabeli AspnetUsers. Aby uruchomić skrypt,

1. Otwieranie Eksploratora serwera. Rozwiń połączenie "ApplicationServices" do wyświetlania w tabelach. Kliknij prawym przyciskiem myszy węzeł tabele i wybierz opcję "Nowe zapytanie"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. W oknie zapytania skopiuj i Wklej cały skrypt SQL z pliku Migrations.sql. Uruchom plik skryptu, naciskając przycisk ze strzałką "Execute".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Odśwież okno Eksploratora serwera. Pięć nowych tabel są tworzone w bazie danych.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Poniżej przedstawiono, jak dane w tabelach członkostwa SQL są mapowane do nowego systemu tożsamości.

    ASPNET\_ról —&gt; AspNetRoles

    ASP\_netUsers i asp\_netMembership —&gt; AspNetUsers

    ASPNET\_UserInRoles —&gt; AspNetUserRoles

    Zgodnie z opisem w powyższej sekcji, w tabelach AspNetUserClaims i AspNetUserLogins są puste. Pole "Dyskryminatora" w tabeli AspNetUser powinny odpowiadać Nazwa klasy modelu, która jest zdefiniowana jako następnego kroku. Również w kolumnie PasswordHash ma postać "zaszyfrowane hasło | wartość zaburzająca hasła | format hasła". Dzięki temu można używać specjalnych logiki kryptograficznego członkostwa SQL, ponowne użycie starego hasła. Który jest szczegółowo opisane w dalszej części tego artykułu.

### <a name="creating-models-and-membership-pages"></a>Tworzenie modeli i stron członkostwa

Jak wspomniano wcześniej, funkcja tożsamości używa programu Entity Framework na komunikowanie się z bazy danych do przechowywania informacji o koncie domyślnie. Aby pracować z istniejącymi danymi w tabeli, należy utworzyć klasy modelu, które mapowania tabel i obsługiwać je w systemie tożsamości. W ramach umowy tożsamości klasy modelu albo powinny implementować interfejsy zdefiniowane w pliku dll Identity.Core lub rozszerzyć istniejącą implementację tych interfejsów, które są dostępne w Microsoft.AspNet.Identity.EntityFramework.

W naszym przykładzie tabele AspNetRoles, AspNetUserClaims, AspNetLogins i AspNetUserRole mieć kolumn, które są podobne do istniejącego wdrożenia systemu tożsamości. Dlatego można ponownie użyć istniejących klas do mapowania tych tabel. Tabela AspNetUser zawiera pewne dodatkowe kolumny, które są używane do przechowywania dodatkowych informacji z tabele członkostwa SQL. Mogą to być mapowane, tworząc klasę modelu, Rozszerz istniejącą implementację programu "IdentityUser" i Dodaj dodatkowe właściwości.

1. Modele skojarzonym folderu projektu i dodawanie klasy użytkownika. Nazwa klasy powinna być zgodna dane dodane w kolumnie "Dyskryminatora" w tabeli "AspnetUsers".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Klasa użytkownika powinny rozszerzać klasy IdentityUser w *Microsoft.AspNet.Identity.EntityFramework* biblioteki dll. Deklarowanie właściwości w klasie, mapowania kolumn AspNetUser. Właściwości Identyfikatora i nazwy użytkownika, PasswordHash SecurityStamp są zdefiniowane w IdentityUser i dlatego są pomijane. Poniżej przedstawiono kod dla klasy użytkownika, który ma wszystkie właściwości

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Klasy Entity Framework DbContext jest wymagany do utrwalenia danych w tabelach modele i pobierać dane z tabel, aby wypełnić te modele. *Microsoft.AspNet.Identity.EntityFramework* dll definiuje klasę kontekst IdentityDbContext użytkowników, która współdziała z tabel tożsamości do pobierania i przechowywania informacji. Kontekst IdentityDbContext&lt;tuser&gt; przyjmuje klasy "TUser", który może być każdej klasy, która rozszerza klasę IdentityUser.

    Utwórz nową klasę ApplicationDBContext, która rozszerza kontekst IdentityDbContext użytkowników w folderze "Modele" w klasie "User" utworzonego w kroku 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Zarządzanie użytkownikami w nowym systemie tożsamości odbywa się za pomocą Menedżera UserManager&lt;tuser&gt; klasy zdefiniowanej w *Microsoft.AspNet.Identity.EntityFramework* biblioteki dll. Należy utworzyć klasę niestandardową, która rozszerza interfejs UserManager, przekazując w klasie "User" utworzonego w kroku 1.

    W folderze modeli Utwórz nową klasę interfejs UserManager, która rozszerza interfejs UserManager&lt;użytkownika&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Hasła użytkowników aplikacji są szyfrowane i przechowywane w bazie danych. Algorytmu kryptograficznego używane w członkostwa SQL jest inny niż ten w nowym systemie tożsamości. Ponowne użycie starego hasła, musimy selektywnie odszyfrować haseł, gdy stary użytkownicy logują się przy użyciu algorytmu członkostwa SQL podczas korzystania z algorytmu kryptograficznego w tożsamości dla nowych użytkowników.

    Klasa Menedżera UserManager ma właściwość PasswordHasher, która przechowuje wystąpienia klasy, która implementuje interfejs "Funkcji IPasswordHasher". Służy do szyfrowania/odszyfrowywania hasła podczas transakcji uwierzytelniania użytkownika. W klasie interfejs UserManager zdefiniowane w kroku 3, Utwórz nową klasę SQLPasswordHasher i skopiuj poniższego kodu.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Napraw błędy kompilacji, importując przestrzenie nazw System.Text i System.Security.Cryptography.

    Metoda EncodePassword szyfruje hasło zgodnie z domyślną implementację kryptografii członkostwa SQL. To jest pobierana z biblioteki dll System.Web. Jeśli stary aplikacji używano implementację niestandardową powinien być zostaną uwzględnione w tym miejscu. Należy zdefiniować dwa inne metody *HashPassword* i *VerifyHashedPassword* używające *EncodePassword* metodę wyznaczania wartości skrótu podanym hasłem lub upewnij się, jako zwykły tekst hasło z jedną istniejącą w bazie danych.

    System członkostwa SQL umożliwia PasswordHash, PasswordSalt i PasswordFormat wyznaczania wartości skrótu hasła wprowadzonego przez użytkowników podczas rejestrowania, lub zmienić swoje hasło. Podczas migracji te trzy pola są przechowywane w tabeli AspNetUser oddzielone w kolumnie PasswordHash "|" znaków. Po zalogowaniu użytkownika i hasło zawiera te pola, używamy crypto członkostwa SQL do sprawdzenia hasło. w przeciwnym razie firma Microsoft umożliwia crypto domyślne systemu tożsamości weryfikacji hasła. Dzięki temu użytkownicy nie musiałby zmienić swoje hasła, gdy aplikacja jest migrowana.
5. Zadeklaruj Konstruktor dla klasy interfejs UserManager i przekazać go jako SQLPasswordHasher do właściwości w konstruktorze.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Utwórz nowe konto strony zarządzania

Następnym krokiem w procesie migracji jest można dodać strony zarządzania konta, które poinformuje użytkownika, zarejestruj się i zaloguj się. Stron stare konto z członkostwa SQL, formanty, które nie działają z nowym systemem tożsamości. Aby dodać nowego użytkownika strony zarządzania postępuj zgodnie z samouczkiem łącze [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) od kroku "Dodawanie formularzy sieci Web rejestracji użytkowników w aplikacji" ponieważ mamy już utworzono projekt i dodać pakietu NuGet pakiety.

Należy wprowadzić pewne zmiany, na przykład do pracy z projektem, które mamy tutaj.

- Register.aspx.cs i Login.aspx.cs kodzie Użyj klasy `UserManager` z pakietów tożsamości, aby utworzyć użytkownika. W tym przykładzie należy użyć Menedżera UserManager dodawane w folderze modeli, wykonując kroki opisane wcześniej.
- Użyj klasy użytkownika utworzone zamiast IdentityUser w kodzie Register.aspx.cs i Login.aspx.cs klasy. Przechwytuje to naszym klasy użytkownika niestandardowego systemu tożsamości.
- Możesz pominąć część, aby utworzyć bazę danych.
- Deweloper musi ustawić identyfikator aplikacji dla nowego użytkownika, aby dopasować bieżący identyfikator aplikacji. Można to zrobić podczas badania ApplicationId dla tej aplikacji, zanim obiekt użytkownika zostanie utworzony w klasie Register.aspx.cs i ustawiając go przed utworzeniem użytkownika. 

    Przykład:

    Definiowanie metody na stronie Register.aspx.cs kwerendy aspnet\_aplikacji tabeli i Uzyskaj identyfikator aplikacji według nazwy aplikacji

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Teraz Przygotuj tego obiektu użytkownika

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Użyj stara nazwa użytkownika i hasło do logowania istniejącego użytkownika. Strona rejestru służy do tworzenia nowego użytkownika. Sprawdź także, że użytkownicy są w rolach zgodnie z oczekiwaniami.

Eksportowanie do systemu tożsamości pozwala użytkownikowi na dodanie otwarte uwierzytelnianie (OAuth) do aplikacji. Zapoznaj się do przykładu [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) mającego z włączonym uwierzytelnianiem OAuth.

## <a name="next-steps"></a>Następne kroki

W tym samouczku będziemy pokazano, jak port użytkowników z członkostwa SQL do produktu ASP.NET Identity, ale firma Microsoft nie przenieść dane profilu. W następnym samouczku Zapoznamy do przenoszenia danych profilu z członkostwa SQL do nowego systemu tożsamości.

Możesz pozostawić opinię w dolnej części tego artykułu.

*Dziękujemy za Tom Dykstra i Rick Anderson przeglądania tego artykułu.*
