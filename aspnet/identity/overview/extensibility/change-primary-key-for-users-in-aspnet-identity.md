---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika. ASP.NET Identity umożliwia zmianę typu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika. ASP.NET Identity umożliwia zmianę typu klucza zgodnie z wymaganiami danych. Na przykład można zmienić typu klucza z ciągu na liczbę całkowitą.
> 
> W tym temacie przedstawiono, jak zacząć domyślnej aplikacji sieci web i zmienić wartość klucza konta użytkownika na liczbę całkowitą. Te same zmiany można użyć do wykonania dowolnego typu klucza w projekcie. Demonstracja do wprowadzenia tych zmian w domyślnej aplikacji sieci web, ale można zastosować zmian podobne do niestandardowych aplikacji. Przedstawia on zmiany podczas pracy z MVC i formularzy sieci Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Visual Studio 2013 Update 2 (lub nowszy)
> - ASP.NET Identity 2.1 lub nowszej


Aby wykonać kroki opisane w tym samouczku, musi mieć Visual Studio 2013 Update 2 (lub nowszego) i aplikacji sieci web utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET. Szablon zmienione w aktualizacji Update 3. W tym temacie pokazano, jak zmienić szablonu w Update 2 i Update 3.

Ten temat zawiera następujące sekcje:

- [Zmień typ klucza w klasie tożsamości użytkownika](#userclass)
- [Dodania niestandardowych klas tożsamości, korzystających z typu klucza](#customclass)
- [Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza](#context)
- [Zmień konfigurację uruchamiania, aby użyć klucza typu](#startup)
- [Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza](#mvcupdate2)
- [Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza](#mvcupdate3)
- [Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza](#webformsupdate2)
- [Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza](#webformsupdate3)
- [Uruchamianie aplikacji](#run)
- [Inne zasoby](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Zmień typ klucza w klasie tożsamości użytkownika

W projekcie utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET należy określić, że klasy ApplicationUser używa całkowitą klucza dla konta użytkownika. W IdentityModels.cs, zmień klasy ApplicationUser odziedziczone IdentityUser, który ma typ **int** dla parametru ogólnego TKey. Możesz również przekazać nazwy trzy dostosowane klasy, które nie zostało jeszcze zaimplementowane.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Zmieniono typ klucza, ale domyślnie reszty aplikacji nadal przyjęto założenie, że klucz jest ciąg. Jawnie musi wskazywać typ klucza w kodzie, który przyjmuje ciąg.

W **ApplicationUser** klasy, zmień **GenerateUserIdentityAsync** metodę w celu uwzględnienia int, jak pokazano w poniższym kodzie zaznaczony. Ta zmiana nie jest niezbędna dla projektów formularzy sieci Web przy użyciu szablonu Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Dodania niestandardowych klas tożsamości, korzystających z typu klucza

Inne klasy tożsamości, takie jak IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, magazynie UserStore, elemencie RoleStore, są nadal skonfigurowane do użycia klucza ciągu. Utwórz nowe wersje tych klas, które określają całkowitą dla klucza. Nie trzeba podać kod wiele implementacji w tych klas, przede wszystkim tylko ustawieniu int jako klucz.

Dodaj następujące klasy w pliku IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza

W IdentityModels.cs, Zmień definicję **ApplicationDbContext** klasę, aby użyć nowego dostosowane klas i **int** klucza, jak pokazano w wyróżniony kod.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze. Zmień konstruktora, aby nie zostały spełnione wartości ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Otwórz IdentityConfig.cs i zmień **ApplicationUserManger** klasę, aby użyć nowego użytkownika przechowywania klasy trwałych danych i **int** dla klucza.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

W szablonie Update 3 możesz zmienić klasy ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Zmień konfigurację uruchamiania, aby użyć klucza typu

W Startup.Auth.cs Zastąp kod element OnValidateIdentity, jak wyróżniono poniżej. Zwróć uwagę, że definicja getUserIdCallback analizuje wartość ciągu na liczbę całkowitą.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Jeśli projekt nie rozpoznaje ogólną implementację **metodę GetUserId** metody, konieczne może być pakietu ASP.NET Identity NuGet aktualizacji do wersji 2.1

Wprowadzono wiele zmian dla klasy infrastruktury używane przez program ASP.NET Identity. Podczas kompilowania projektu można zauważyć wiele błędów. Na szczęście pozostałe błędy są podobne. Klasa tożsamości oczekuje liczby całkowitej dla klucza, ale kontrolera (lub formularza sieci Web) jest przekazanie wartości ciągu. W każdym przypadku należy przekonwertować ciąg i liczby całkowitej przez wywołanie metody **metodę GetUserId&lt;int&gt;**. Możesz skorzystać z listy błędów z kompilacji lub wykonaj poniższe zmiany.

Pozostałe zmiany są zależne od typu projektu tworzenia i aktualizacji, które zainstalowano w programie Visual Studio. Można przejść bezpośrednio do odpowiedniej sekcji za pomocą poniższych łączy

- [Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza](#mvcupdate2)
- [Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza](#mvcupdate3)
- [Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza](#webformsupdate2)
- [Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza

Otwórz plik AccountController.cs. Musisz zmienić z następujących metod.

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

Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza

Otwórz plik AccountController.cs. Musisz zmienić następującą metodę.

**ConfirmEmail** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Otwórz plik ManageController.cs. Musisz zmienić z następujących metod.

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

**Element ChangePassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**UstawianieHasła** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** — metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza

Formularze sieci Web z Update 2 należy zmienić następujące strony.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.CS**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza

W przypadku formularzy sieci Web z aktualizacją Update 3 musisz zmienić następujących stron.

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

Zakończono wszystkie wymagane zmiany domyślnego szablonu aplikacji sieci Web. Uruchom aplikację i zarejestrować nowego użytkownika. Po zarejestrowaniu użytkownika, można zauważyć, że tabela AspNetUsers zawiera identyfikator kolumny, która jest liczbą całkowitą.

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Jeśli wcześniej utworzono ASP.NET Identity tabel za pomocą innego klucza podstawowego, należy wprowadzić kilka dodatkowych zmian. Jeśli to możliwe można usunąć istniejącą bazę danych. Bazy danych zostanie ponownie utworzony z poprawny projekt, po uruchomieniu aplikacji sieci web i dodać nowego użytkownika. Jeśli usunięcia nie jest możliwe, uruchom migracje code first, aby zmienić tabel. Jednakże nowy klucz podstawowy całkowitą nie zostanie ustawiona jako właściwość SQL IDENTITY w bazie danych. W kolumnie Identyfikator musi ręcznie Ustaw jako tożsamości.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- [Przegląd dostawców magazynu niestandardowego dla tożsamości ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrowanie istniejącej witryny sieci Web z członkostwa SQL do tożsamości platformy ASP.NET](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrowanie danych uniwersalnych dostawcy członkostwa i profilów użytkownika dla tożsamości ASP.NET](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Przykładowa aplikacja](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) z zmienione klucza podstawowego
