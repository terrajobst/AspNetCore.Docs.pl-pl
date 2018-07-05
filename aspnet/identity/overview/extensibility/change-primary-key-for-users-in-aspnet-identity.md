---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Zmiana klucza podstawowego dla użytkowników w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft
author: tfitzmac
description: W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartość ciągu dla klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 20e6b86f50a6ea62f188ae592e0b302c7ef77177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373331"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Zmiana klucza podstawowego dla użytkowników w produkcie ASP.NET Identity
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartość ciągu dla klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu klucza, aby spełnić wymagania dotyczące danych. Na przykład można zmienić typ klucza z ciągu na liczbę całkowitą.
> 
> W tym temacie pokazano, jak zacząć domyślną aplikację sieci web i zmienić klucz konta użytkownika na liczbę całkowitą. Te same modyfikacje można użyć do wdrożenia dowolnego typu klucza w projekcie. Pokazuje sposób wprowadzania tych zmian w domyślnej aplikacji sieci web, ale można zastosować zmiany podobne do niestandardowych aplikacji. Pokazuje zmiany wymagane podczas pracy z MVC i formularzy sieci Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Visual Studio 2013 z aktualizacją Update 2 (lub nowszym)
> - ASP.NET Identity 2.1 lub nowszej


Aby wykonać kroki opisane w tym samouczku, konieczne jest posiadanie programu Visual Studio 2013 Update 2 (lub nowszego) i aplikacji sieci web utworzone za pomocą szablonu aplikacji sieci Web ASP.NET. Szablon zmienione w wersji Update 3. W tym temacie pokazano, jak zmienić szablon w aktualizacji 2 i Update 3.

Ten temat zawiera następujące sekcje:

- [Zmień typ klucza w tożsamości klasy użytkownika](#userclass)
- [Dodaj niestandardowe klasy tożsamości, które używają typu klucza](#customclass)
- [Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia typ klucza](#context)
- [Zmień konfigurację uruchamiania, aby używać kluczy typu](#startup)
- [Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza](#mvcupdate2)
- [Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza](#mvcupdate3)
- [W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza](#webformsupdate2)
- [W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza](#webformsupdate3)
- [Uruchamianie aplikacji](#run)
- [Inne zasoby](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Zmień typ klucza w tożsamości klasy użytkownika

W projekcie, utworzone na podstawie szablonu aplikacji sieci Web ASP.NET należy określić, że klasy ApplicationUser używa całkowitą dla klucza dla kont użytkowników. W IdentityModels.cs, zmień klasy ApplicationUser odziedziczone IdentityUser, który ma typ **int** dla parametru generycznego TKey. Możesz też przekazać nazwy trzy niestandardowe klasy, która nie zostało jeszcze zaimplementowane.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Zmieniono typ klucza, ale domyślnie pozostałe części aplikacji nadal przyjęto założenie, że klucz jest ciąg. Należy jawnie wskazać typ klucza w kodzie, który przyjmuje ciąg.

W **ApplicationUser** klasy, zmienić **GenerateUserIdentityAsync** metodę, aby uwzględnić int, jak pokazano w poniższym kodzie wyróżnione. Ta zmiana nie jest konieczne w przypadku projektów formularzy sieci Web za pomocą szablonu aktualizacji 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Dodaj niestandardowe klasy tożsamości, które używają typu klucza

Inne klasy tożsamości, takich jak IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, magazynie UserStore, elemencie RoleStore, nadal są konfigurowane do użycia klucza typu ciąg. Utwórz nowe wersje tych klas, które określają całkowitą dla klucza. Nie należy podać wiele kod implementacji w ramach tych zajęć, przede wszystkim właśnie przeprowadzasz int jako klucz.

Dodaj następujące klasy do pliku IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia typ klucza

W IdentityModels.cs, należy zmienić definicję **ApplicationDbContext** nowej klasy dostosowane klasy i **int** klucza, jak pokazano na wyróżniony kod.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze. Zmień konstruktora, aby nie mogła przekazywać wartość ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Otwórz IdentityConfig.cs i zmień **ApplicationUserManger** klasy nowy użytkownik przechowywania klasę utrwalanie danych i **int** dla klucza.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

W szablonie Update 3 możesz zmienić klasy ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Zmień konfigurację uruchamiania, aby używać kluczy typu

W Startup.Auth.cs Zastąp kod element OnValidateIdentity, jak wyróżniono poniżej. Należy zauważyć, że definicja getUserIdCallback analizuje wartości ciągu na liczbę całkowitą.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Jeśli projekt nie może rozpoznać ogólną implementację **metodę GetUserId** metody, konieczne może być aktualizacja pakietu NuGet tożsamości ASP.NET do wersji 2.1

Wprowadzono wiele zmian do klas infrastruktury posługują się produktu ASP.NET Identity. Jeśli spróbujesz kompilowania projektu, można zauważyć wiele błędów. Na szczęście pozostałe błędy są podobne. Klasa tożsamości oczekuje całkowitą dla klucza, ale kontroler (lub formularza sieci Web) przekazuje wartość ciągu. W obu przypadkach należy przekonwertować ciąg i liczba całkowita, wywołując **metodę GetUserId&lt;int&gt;**. Możesz pracować za pośrednictwem listy błędów z kompilacji lub wykonaj poniższe zmiany.

Pozostałe zmiany są zależne od typu projektu, tworzenia i aktualizacji, które zostały zainstalowane w programie Visual Studio. Możesz przejść bezpośrednio do odpowiedniej sekcji za pomocą poniższych łączy

- [Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza](#mvcupdate2)
- [Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza](#mvcupdate3)
- [W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza](#webformsupdate2)
- [W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza

Otwórz plik AccountController.cs. Musisz zmienić następujących metod.

**ConfirmEmail** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Usuń skojarzenie** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza

Otwórz plik AccountController.cs. Musisz zmienić następującą metodę.

**ConfirmEmail** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Otwórz plik ManageController.cs. Musisz zmienić następujących metod.

**Indeks** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** metody

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** metody

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza

Formularze sieci Web z aktualizacją Update 2 należy zmienić na następujących stronach.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.CS**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza

Formularze sieci Web z aktualizacją Update 3 należy zmienić na następujących stronach.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.CS**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Uruchamianie aplikacji

Zakończono wszystkie zmiany wymagane w celu domyślnego szablonu aplikacji sieci Web. Uruchom aplikację i zarejestrować nowego użytkownika. Po zarejestrowaniu użytkownika, zauważysz, że tabela AspNetUsers zawiera kolumny identyfikatora, który jest liczbą całkowitą.

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Jeśli wcześniej utworzono produktu ASP.NET Identity tabel przy użyciu innego klucza podstawowego, należy wprowadzić pewne dodatkowe zmiany. Jeśli to możliwe można usunąć istniejącej bazy danych. Po uruchomieniu aplikacji sieci web i dodać nowego użytkownika będzie ponownie utworzony przy użyciu poprawny projekt bazy danych. Jeśli usuwanie nie jest możliwe, uruchom migracje code first, aby zmienić w tabelach. Jednakże nowy klucz podstawowy dla liczby całkowitej nie zostanie ustawiona jako właściwość SQL IDENTITY w bazie danych. Jako tożsamość, należy ręcznie ustawić kolumny identyfikatora.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- [Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrowanie istniejącej witryny internetowej z członkostwa SQL do produktu ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrowanie danych uniwersalnego dostawcy dotyczących członkostwa i profilów użytkowników do produktu ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Przykładowa aplikacja](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) przy użyciu zmienionego klucz podstawowy
