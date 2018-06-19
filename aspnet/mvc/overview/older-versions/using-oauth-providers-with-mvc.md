---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Przy użyciu dostawców uwierzytelniania OAuth z MVC 4 | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 4, która umożliwia użytkownikom logowanie się przy użyciu poświadczeń z zewnętrznego dostawcy, takich jak Facebo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033579"
---
<a name="using-oauth-providers-with-mvc-4"></a>Przy użyciu dostawców uwierzytelniania OAuth z MVC 4
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 4, umożliwiający użytkownikom Zaloguj się przy użyciu poświadczeń z zewnętrznego dostawcy, takich jak Facebook, Twitter, Microsoft lub Google, a następnie zintegruje ją niektórych funkcji tych dostawców do użytkownika Aplikacja sieci Web. Dla uproszczenia ten samouczek koncentruje się na temat pracy z poświadczeń z usługi Facebook.
> 
> Aby użyć zewnętrznych poświadczeń w aplikacji sieci web platformy ASP.NET MVC 5, zobacz [tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Włączanie te poświadczenia w witrynach sieci web zapewnia znaczących korzyści, ponieważ milionów użytkowników mają już konta z tych dostawców zewnętrznych. Ci użytkownicy będą bardziej skłonni zalogowania się do witryny, jeśli nie mają do tworzenia nowego zestawu poświadczeń. Ponadto po zalogowaniu się użytkownika za pomocą jednego z tych dostawców, można zastosować operacji społecznościowych od dostawcy.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku istnieją dwa główne cele:

1. Włączenie użytkownika do logowania się przy użyciu poświadczeń od dostawcy uwierzytelniania OAuth.
2. Pobierz informacje o koncie od dostawcy i zintegrowania z rejestrację konta dla swojej witryny.

Mimo że przykłady w tym samouczku dotyczą za pomocą usługi Facebook jako dostawcy uwierzytelniania, można zmodyfikować kod, aby użyć każdego z dostawców. Kroki w celu wykonania dowolnego dostawcy są bardzo podobne do kroków, które będą wyświetlane w tym samouczku. Można zauważyć tylko istotne różnice bezpośrednie wywołania funkcji API dostawcy ustawić.

## <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) lub [programu Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Lub

- Microsoft Visual Studio 2010 z dodatkiem SP1 lub [programu Visual Web Developer Express 2010 z dodatkiem SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Ponadto w tym temacie założono, że dysponujesz podstawową wiedzą o platformie ASP.NET MVC i Visual Studio. Jeśli potrzebujesz wprowadzenia do platformy ASP.NET MVC 4, zobacz [wprowadzenie do platformy ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio Utwórz nową aplikację sieci Web 4 ASP.NET MVC i nadaj mu nazwę &quot;OAuthMVC&quot;. Możesz zastosować .NET Framework 4.5 lub 4.

![Tworzenie projektu](using-oauth-providers-with-mvc/_static/image1.png)

W oknie Nowy projekt programu ASP.NET MVC 4, wybierz **aplikacji internetowej** i pozostawić **Razor** aparatu widoku.

![Wybierz aplikacji internetowej](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Włącz dostawcę

Po utworzeniu aplikacji sieci web MVC 4 przy użyciu szablonu aplikacji internetowej projekt jest tworzony przy użyciu pliku o nazwie AuthConfig.cs w aplikacji\_folder początkowy.

![Plik AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Plik AuthConfig zawiera kod, aby zarejestrować klientów dla dostawcy uwierzytelniania zewnętrznego. Domyślnie ten kod jest oznaczone jako komentarz, więc żaden z zewnętrznych dostawców nie włączono.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Należy usuń znaczniki komentarza tego kodu w celu użycia klienta uwierzytelniania zewnętrznego. Usuń znaczniki komentarza dostawców, które chcesz uwzględnić w witrynie. W tym samouczku umożliwi tylko poświadczenia usługi Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Zwróć uwagę, w tym przykładzie, że metoda zawiera puste ciągi dla parametrów rejestracji. Jeśli zostanie podjęta próba uruchomienia aplikacji teraz, aplikacji zgłasza wyjątek argumentu, ponieważ puste ciągi nie są dozwolone dla parametrów. Aby zapewnić prawidłowe wartości, należy zarejestrować witryny sieci web z zewnętrznych dostawców, jak pokazano w następnej sekcji.

## <a name="registering-with-an-external-provider"></a>Rejestrowanie przy użyciu zewnętrznego dostawcy

Do uwierzytelniania użytkowników przy użyciu poświadczeń z zewnętrznego dostawcy, należy zarejestrować witryny sieci web z dostawcą. Podczas rejestrowania witryny otrzymasz parametry (takie jak klucz lub identyfikator i klucz tajny) do uwzględnienia podczas rejestrowania klienta. Musisz mieć konto z dostawcami, którego chcesz użyć.

W tym samouczku nie są wyświetlane wszystkie czynności, które należy wykonać, aby zarejestrować z usług tych dostawców. Kroki nie są zwykle trudne. Aby pomyślnie zarejestrować witryny, wykonaj instrukcje podane w tych witrynach. Aby rozpocząć rejestrowanie witryny, zobacz deweloperskiej dla:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Podczas rejestrowania witryny z usługą Facebook, możesz podać &quot;localhost&quot; dla domeny, lokacji i `&quot;http://localhost/&quot;` dla adresu URL, jak pokazano na poniższej ilustracji. Przy użyciu localhost współpracuje z dostawców większości, ale obecnie nie współpracujesz z dostawcy firmy Microsoft. Dla dostawcy usługi Microsoft musi zawierać adres URL witryny sieci web prawidłowe.

![Rejestracja witryny](using-oauth-providers-with-mvc/_static/image4.png)

Na poprzedniej ilustracji wartości identyfikatora aplikacji, klucz tajny aplikacji i kontakt poczty e-mail zostały usunięte. Podczas rejestrowania faktycznie witryny, te wartości będą obecne. Można zauważyć wartości identyfikatora aplikacji i klucz tajny aplikacji, ponieważ spowoduje dodanie ich do aplikacji.

## <a name="creating-test-users"></a>Tworzenie użytkowników testowych

Jeśli nie pamiętać, aby przetestować witryny przy użyciu istniejącego konta usługi Facebook, możesz pominąć tę sekcję.

Można łatwo utworzyć użytkowników testowych dla aplikacji na stronie zarządzania aplikacji Facebook. Te konta do logowania witrynie testowe. Tworzenie użytkowników testowych, klikając **ról** łącza w lewym okienku nawigacji i kliknięcie **Utwórz** łącza.

![Tworzenie użytkowników testowych](using-oauth-providers-with-mvc/_static/image5.png)

Witryny Facebook automatycznie tworzy liczby kont testów, które żądania.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Dodawanie aplikacji identyfikator i klucz tajny od dostawcy

Po odebraniu identyfikator i klucz tajny z usługi Facebook wróć do pliku AuthConfig i dodać je jako wartości parametrów. Poniższe wartości nie są wartości rzeczywistych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Zaloguj się przy użyciu poświadczeń zewnętrznych

To wszystko, co należy zrobić, aby włączyć zewnętrznych poświadczeń w witrynie. Uruchom aplikację i kliknąć link logowania w prawym górnym rogu. Szablon automatycznie rozpoznaje zarejestrowanego dostawcy usługi Facebook i zawiera przycisk dla dostawcy. Jeśli zarejestrujesz wielu dostawców, przycisk dla każdej z nich jest uwzględniana automatycznie.

![Logowanie zewnętrzne](using-oauth-providers-with-mvc/_static/image6.png)

W tym samouczku opisano, jak dostosować Zaloguj przycisków dla zewnętrznych dostawców. Informacje, zobacz [Dostosowywanie interfejsu użytkownika logowania przy użyciu uwierzytelniania OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Kliknij przycisk Facebook, aby zalogować się przy użyciu poświadczeń usługi Facebook. Po wybraniu jednego z zewnętrznych dostawców są przekierowanie do tej witryny i zaloguj się, monit tej usługi.

Na poniższej ilustracji przedstawiono ekran logowania dla usługi Facebook. Zawiera informacje dotyczące logować się do witryny o nazwie oauthmvcexample używasz konta usługi Facebook.

![uwierzytelniania serwisu Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Po zalogowaniu się przy użyciu poświadczeń usługi Facebook, strona informuje użytkownika, czy witryny mają dostęp do podstawowych informacji.

![Żądanie uprawnienia](using-oauth-providers-with-mvc/_static/image8.png)

Po wybraniu **przejdź do aplikacji**, użytkownik musi zarejestrować dla witryny. Na poniższej ilustracji przedstawiono strony rejestracji po użytkownik zalogował się przy użyciu poświadczeń usługi Facebook. Nazwa użytkownika jest zazwyczaj wstępnie wypełnione o nazwie od dostawcy.

![zarejestruj](using-oauth-providers-with-mvc/_static/image9.png)

Kliknij przycisk **zarejestrować** do ukończenia rejestracji. Zamknij przeglądarkę.

Widać, że nowe konto zostało dodane do bazy danych. Otwórz w Eksploratorze serwera **połączenia DefaultConnection** bazy danych, a następnie otwórz **tabel** folderu.

![tabele bazy danych](using-oauth-providers-with-mvc/_static/image10.png)

Kliknij prawym przyciskiem myszy **UserProfile** tabeli i wybierz **Pokaż dane tabeli**.

![Wyświetlanie danych](using-oauth-providers-with-mvc/_static/image11.png)

Zostanie wyświetlone nowe konto, dodane. Sprawdź dane w **strony sieci Web\_OAuthMembership** tabeli. Zostanie wyświetlone więcej danych związanych z zewnętrznego dostawcy dla konta, które właśnie został dodany.

Jeśli chcesz włączyć uwierzytelnianie zewnętrzne, wszystko będzie gotowe. Jednak można dodatkowo zintegrować informacji od dostawcy nowego procesu rejestracji użytkownika, jak pokazano w poniższych sekcjach.

## <a name="create-models-for-additional-user-information"></a>Tworzenie modeli, aby uzyskać dodatkowe informacje dotyczące użytkownika

Jak można zauważyć w poprzednich sekcjach, nie trzeba pobierać żadnych dodatkowych informacji o rejestrację wbudowane konto do pracy. Jednak większość zewnętrznych dostawców Przekaż ponownie dodatkowe informacje o użytkowniku. Poniższe sekcje pokazują, jak zachować tych informacji i zapisać ją w bazie danych. W szczególności zostanie zachowana wartości dla użytkownika Pełna nazwa, identyfikator URI strony sieci web osobiste użytkownika, a wartość, która wskazuje, czy usługi Facebook zweryfikował konta.

Użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) do dodawania tabeli do przechowywania dodatkowe informacje dotyczące użytkownika. Dodajesz tabeli do istniejącej bazy danych, dlatego najpierw należy utworzyć migawkę bieżącej bazy danych. Tworząc migawkę bieżącej bazy danych, można utworzyć później migracji, która zawiera nową tabelę. Aby utworzyć migawkę bieżącej bazy danych:

1. Otwórz **Konsola Menedżera pakietów**
2. Uruchom polecenie **enable-migrations,**
3. Uruchom polecenie **dodać migracji początkowej — IgnoreChanges**
4. Uruchom polecenie **update-database**

Teraz zostaną dodane nowe właściwości. W folderze modeli Otwórz plik AccountModels.cs i znaleźć klasy RegisterExternalLoginModel. Klasa RegisterExternalLoginModel przechowuje wartości, które pochodzą z powrotem od dostawcy uwierzytelniania. Dodaj właściwości o nazwie imię i nazwisko i łącza, jak wyróżniono poniżej.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Również w AccountModels.cs, Dodaj nową klasę o nazwie ExtraUserInformation. Ta klasa reprezentuje nową tabelę, która zostanie utworzona w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

W klasie UsersContext Dodaj wyróżniony kod poniżej, aby utworzyć właściwość DbSet dla nowej klasy.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Teraz można przystąpić do utworzenia nowej tabeli. Otwórz konsolę Menedżera pakietów, ponownie i teraz:

1. Uruchom polecenie **AddExtraUserInformation dodać migracji**
2. Uruchom polecenie **update-database**

Nowa tabela istnieje obecnie w bazie danych.

## <a name="retrieve-the-additional-data"></a>Pobrać dodatkowe dane

Istnieją dwa sposoby można pobrać danych użytkownika dodatkowe. Pierwszym sposobem jest, aby zachować dane użytkownika jest przekazywany, domyślnie podczas żądania uwierzytelniania. Drugim sposobem jest specjalnie wywołanie dostawcy interfejsu API i uzyskać więcej informacji. Wartości imię i nazwisko i łącza są automatycznie przekazywane do tyłu usługi Facebook. Wartość, która wskazuje, czy usługi Facebook zweryfikował konta są pobierane przez wywołanie interfejsu API usługi Facebook. Najpierw wypełnisz wartości imię i nazwisko i łącza, a następnie później, wystąpi zweryfikowano wartość.

Aby pobrać dane dodatkowe, otwórz **AccountController.cs** w pliku **kontrolerów** folderu.

Ten plik zawiera logikę rejestrowania, rejestrowanie i zarządzanie kontami. W szczególności, zwróć uwagę, metody wywołane **ExternalLoginCallback** i **ExternalLoginConfirmation**. W ramach tych metod możesz dodać kod, aby dostosować operacje logowania zewnętrznego dla aplikacji. W pierwszym wierszu **ExternalLoginCallback** metoda zawiera:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Dodatkowe dane są przekazywane w **ExtraData** właściwość **AuthenticationResult** obiekt, który jest zwracany z **VerifyAuthentication** metody. Klient usługi Facebook zawiera następujące wartości w **ExtraData** właściwości:

- identyfikator
- nazwa
- link
- płci
- accesstoken

Innych dostawców ma podobne, lecz nieco inne danych we właściwości ExtraData.

Jeśli użytkownik jest nowy do swojej witryny, możesz pobrać niektóre dodatkowe dane i przekazać dane do widoku potwierdzenia. Ostatni blok kod w metodzie jest uruchamiany tylko wtedy, gdy użytkownik jest nowy do witryny. Zastąp następujący wiersz:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

w tym:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Ta zmiana zawiera tylko wartości właściwości imię i nazwisko i łącza.

W **ExternalLoginConfirmation** metody, zmodyfikuj kod jak wyróżniono poniżej, aby zapisać informacje o użytkowniku dodatkowe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Dostosowanie widoku

Dane użytkownika dodatkowe pobrać od dostawcy będą wyświetlane na stronie rejestracji.

W **widoków**/**konta** folder, otwórz **ExternalLoginConfirmation.cshtml**. Poniżej istniejącego pola dla nazwy użytkownika należy dodać pola Imię i nazwisko, łączy i PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Jesteś teraz już prawie gotowe uruchomić aplikację i zarejestrować nowego użytkownika z dodatkowymi informacjami zapisane. Musi mieć konto, które nie jest już zarejestrowany w lokacji. Możesz użyć konta innego testu, lub usuwania wierszy w **UserProfile** i **stron sieci Web\_OAuthMembership** tabel dla konta chcesz użyć ponownie. Przez usunięcie tych wierszy, będzie upewnij się, czy konto jest zarejestrowana ponownie.

Uruchom aplikację i zarejestrować nowego użytkownika. Należy zauważyć, że teraz stronę potwierdzenia zawiera więcej wartości.

![zarejestruj](using-oauth-providers-with-mvc/_static/image12.png)

Po zakończeniu rejestracji, zamknij przeglądarkę. Szukaj w bazie danych, zwróć uwagę na nowe wartości w **ExtraUserInformation** tabeli.

## <a name="install-nuget-package-for-facebook-api"></a>Instalowanie pakietu NuGet dla interfejsu API usługi Facebook

Udostępnia usługi Facebook [interfejsu API](https://developers.facebook.com/docs/reference/apis/) można wywołać w celu wykonania operacji. Można wywołać interfejsu API usługi Facebook, kierując wysyłania żądań HTTP lub przy użyciu Instalowanie pakietu NuGet, która ułatwia obsługę wysyłania tych żądań. W tym samouczku przedstawiono przy użyciu pakietu NuGet, ale instalowania NuGet pakietu nie jest konieczne. Ten samouczek przedstawia sposób użycia pakietu Facebook C# zestawu SDK. Istnieją inne pakiety NuGet, które ułatwi wywołanie interfejsu API usługi Facebook.

Z **Zarządzaj pakietami NuGet** systemu windows, wybierz pakiet, Facebook C# zestawu SDK.

![Zainstaluj pakiet](using-oauth-providers-with-mvc/_static/image13.png)

Facebook C# SDK użyje do wywołania operacji, która wymaga tokenu dostępu dla użytkownika. Następna sekcja pokazano, jak uzyskać token dostępu.

## <a name="retrieve-access-token"></a>Pobierz token dostępu

Większość zewnętrznych dostawców przesłać token dostępu po zweryfikowaniu poświadczeń użytkownika. Ten token dostępu jest bardzo ważne, ponieważ umożliwia wywołanie operacji, które są dostępne tylko dla użytkowników uwierzytelnionych. W związku z tym pobierania i przechowywania token dostępu jest niezbędne, gdy chcesz zapewnić więcej funkcji.

W zależności od zewnętrznego dostawcy tokenu dostępu może mieć okresu ważności ograniczoną ilość czasu. Aby zapewnić prawidłowy dostęp do tokenu, będą pobierane on zawsze użytkownik loguje się i zapisz go jako wartość sesji zamiast zapisać go do bazy danych.

W **ExternalLoginCallback** metody token dostępu jest również przekazany z powrotem do **ExtraData** właściwość **AuthenticationResult** obiektu. Dodaj wyróżniony kod, aby **ExternalLoginCallback** można zapisać tokenu dostępu w **sesji** obiektu. Ten kod jest uruchamiane za każdym razem, gdy użytkownik zaloguje się za pomocą konta w usłudze Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Mimo że w tym przykładzie pobiera token dostępu z usługi Facebook, token dostępu można pobrać z dowolnego zewnętrznego dostawcy za pomocą tego samego klucza o nazwie &quot;accesstoken&quot;.

## <a name="logging-off"></a>Wylogowanie

Wartość domyślna **wylogowywania** metody logowania użytkownika z aplikacji, ale nie logowania użytkownika z zewnętrznego dostawcy. Aby logowania użytkownika z usługi Facebook i zapobiec token utrwalanie po wylogowaniu użytkownika, można dodać następujące wyróżniony kod do **wylogowywania** metoda elementu AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Wartości podane `next` parametr jest adres URL do użycia po zalogowaniu się z serwisem Facebook. Podczas testowania na komputerze lokalnym, czy Podaj poprawny numer portu witryny lokalnej. W środowisku produkcyjnym zapewni domyślną stronę, np. contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Pobierz informacje o użytkowniku, która wymaga tokenu dostępu

Teraz, przechowywane token dostępu i zainstalować pakiet Facebook C# SDK można je razem żądania dodatkowe informacje dotyczące użytkownika z usługi Facebook. W **ExternalLoginConfirmation** metody, Utwórz wystąpienie **FacebookClient** klasy, przekazując wartość tokenu dostępu. Żądanie wartość **zweryfikować** właściwość dla bieżącego użytkownika uwierzytelnionego. **Zweryfikować** właściwość wskazuje, czy usługi Facebook został zweryfikowany konta przy użyciu innych metod, takich jak wysyłanie wiadomości na telefon komórkowy. Zapisz tę wartość w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Należy ponownie usunąć rekordy w bazie danych użytkownika lub użyj innego konta usługi Facebook.

Uruchom aplikację i zarejestrować nowego użytkownika. Przyjrzyj się **ExtraUserInformation** tabeli, aby wyświetlić wartość właściwości zweryfikowany.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzono lokacji, który jest zintegrowany z usługą Facebook danych rejestracji i uwierzytelniania użytkowników. Znasz już o zachowaniu domyślnym, który jest skonfigurowany dla aplikacji sieci web MVC 4 oraz sposobu dostosowywania to zachowanie domyślne.

## <a name="related-topics"></a>Tematy pokrewne

- [Tworzenie aplikacji platformy ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
