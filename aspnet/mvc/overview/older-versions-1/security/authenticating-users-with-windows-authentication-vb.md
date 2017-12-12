---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: "Uwierzytelnianie użytkowników przy użyciu uwierzytelniania systemu Windows (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Dowiedz się, jak używać uwierzytelniania systemu Windows w kontekście aplikacji MVC. Możesz dowiedzieć się, jak włączyć uwierzytelnianie systemu Windows w Twojej aplikacji sieci web co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: d4b83d99fcf1247d08ce83364cc00e738b6a16c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Uwierzytelnianie użytkowników przy użyciu uwierzytelniania systemu Windows (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak używać uwierzytelniania systemu Windows w kontekście aplikacji MVC. Dowiesz się, jak włączyć uwierzytelnianie systemu Windows w pliku konfiguracji aplikacji sieci web i jak skonfigurować uwierzytelnianie w usługach IIS. Na koniec należy Dowiedz się, jak użyć atrybutu [Authorize] do ograniczania dostępu do akcji kontrolera do określonych użytkowników systemu Windows lub grupy.


Celem tego samouczka jest wyjaśniają, jak można wykorzystać zabezpieczenia funkcje wbudowane w Internetowe usługi informacyjne hasło ochrony widoków w aplikacji MVC. Sposób umożliwia akcji kontrolera do wywołania tylko przez określonych użytkowników systemu Windows lub użytkowników, którzy są członkami określonych grup systemu Windows.

Przy użyciu uwierzytelniania systemu Windows ma sens, gdy tworzysz wewnętrznej witryny sieci Web firmy (intranecie) i chcesz, aby użytkownicy mogli korzystać z ich standardowe nazwy użytkownika systemu Windows i hasła podczas uzyskiwania dostępu do witryny sieci Web. Jeśli tworzysz na zewnątrz ukierunkowane witryny sieci Web (witryny internetowej) uwierzytelnianie oparte na formularzach zamiast tego Rozważ użycie.

#### <a name="enabling-windows-authentication"></a>Włączanie uwierzytelniania systemu Windows

Podczas tworzenia nowej aplikacji ASP.NET MVC, uwierzytelnianie systemu Windows nie jest włączone domyślnie. Uwierzytelnianie formularzy jest domyślnym typem uwierzytelniania włączone dla aplikacji MVC. Należy włączyć uwierzytelnianie systemu Windows, modyfikując plik konfiguracji (web.config) w sieci web aplikacji MVC. Znajdź &lt;uwierzytelniania&gt; sekcji, a następnie zmodyfikować go do korzystania z systemu Windows zamiast uwierzytelniania formularzy w następujący sposób:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Gdy włączone jest uwierzytelnianie systemu Windows, serwer sieci web staje się odpowiedzialna za uwierzytelnianie użytkowników. Zazwyczaj istnieją dwa typy serwerów sieci web, których można użyć podczas tworzenia i wdrażania aplikacji platformy ASP.NET MVC.

Po pierwsze podczas tworzenia aplikacji MVC, Użyj serwera sieci Web ASP.NET Development uwzględnionych w programie Visual Studio. Domyślnie serwer sieci Web ASP.NET Development wykonuje wszystkich stron w kontekście konta systemu Windows (niezależnie od konto używane do logowania do systemu Windows).

Sieci Web ASP.NET Development Server obsługuje również uwierzytelnianie NTLM. Można włączyć uwierzytelnianie NTLM przez kliknięcie prawym przyciskiem myszy nazwę projektu w oknie Eksploratora rozwiązań i wybierając polecenie Właściwości. Następnie wybierz kartę sieci Web i zaznacz pole wyboru NTLM (zobacz rysunek 1).

**Rysunek 1 — włączenie uwierzytelniania NTLM dla sieci Web ASP.NET Development Server**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

W przypadku aplikacji sieci web produkcyjnej strony, należy użyć usług IIS jako serwera sieci web. Usługi IIS obsługują kilka typów uwierzytelniania, w tym:

- Uwierzytelnianie podstawowe — zdefiniowany w ramach protokołu HTTP 1.0. Wysyła nazwy użytkownika i hasła w postaci zwykłego tekstu (kodowanie Base64) przez Internet. — Uwierzytelnianie szyfrowane wysyła skrót hasła, zamiast hasła, przez internet. — Zintegrowane uwierzytelnianie systemu Windows (NTLM) — najlepszy typ uwierzytelniania do użycia w środowisku sieci intranet przy użyciu systemu windows. -Certyfikatów uwierzytelniania — umożliwia uwierzytelnianie za pomocą certyfikatu klienta. Mapuje certyfikatu na konto użytkownika systemu Windows.

> [!NOTE] 
> 
> Bardziej szczegółowe omówienie różnego typu uwierzytelniania, zobacz [https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx).


Aby włączyć określony typ uwierzytelniania, można użyć Menedżera internetowych usług informacyjnych. Należy pamiętać, że wszystkie typy uwierzytelniania nie są dostępne w przypadku każdej wersji systemu operacyjnego. Ponadto jeśli korzystasz z usług IIS 7.0 z systemem Windows Vista, należy włączyć różne typy uwierzytelniania systemu Windows, zanim pojawią się one w Menedżera internetowych usług informacyjnych. Otwórz **Panelu sterowania, programy, programy i funkcje, Włącz lub wyłącz funkcje systemu Windows**, rozwiń węzeł Internetowe usługi informacyjne (patrz rysunek 2).

**Rysunek 2 — funkcje włączenie systemu Windows w usługach IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Przy użyciu Internetowych usług informacyjnych, można włączyć lub wyłączyć różnego rodzaju uwierzytelniania. Przykładowo, rysunek 3 przedstawia wyłączenie uwierzytelniania anonimowego i włączenie uwierzytelniania zintegrowanego systemu Windows (NTLM) przy użyciu usług IIS 7.0.

**Rysunek 3 – włączenie zintegrowanego uwierzytelniania systemu Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autoryzowanie Windows użytkowników i grup

Po włączeniu uwierzytelniania systemu Windows, można użyć &lt;autoryzacji&gt; atrybutu kontrolować dostęp do kontrolerów i akcji kontrolera. Ten atrybut można stosować do całego kontrolera MVC lub akcji określony kontroler.

Na przykład kontroler głównej w 1 lista przedstawia trzy czynności o nazwie indeks(), CompanySecrets() i StephenSecrets(). Każdy użytkownik może wywołać indeks() akcji. Jednak tylko członkowie grupy kierownicy lokalnego systemu Windows można wywoływać Akcja CompanySecrets(). Ponadto tylko użytkownika domeny systemu Windows o nazwie Stephen (w domenie Redmond) można wywoływać Akcja StephenSecrets().

**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Z powodu systemu Windows (Kontrola konta), podczas pracy z systemem Windows Vista lub Windows Server 2008, do lokalnej grupy Administratorzy będą zachowywać się inaczej niż inne grupy. &lt;Autoryzacji&gt; atrybutu nie będą rozpoznawane poprawnie członkiem lokalnej grupy administratorów, chyba że zmodyfikujesz ustawień kontroli konta użytkownika na komputerze.


Dokładnie co się stanie, jeśli próba wywołania akcji kontrolera bez odpowiednich uprawnień, zależy od typu uwierzytelnianie jest włączone. Domyślnie, używając ASP.NET Development Server wystarczy pobrać pustej strony. Strona jest wyświetlona z **401 nieautoryzowane** stanu odpowiedzi HTTP.

Jeśli, z drugiej strony, usługi IIS za pomocą wyłączone uwierzytelnianie anonimowe i włączone uwierzytelnianie podstawowe, a następnie będzie nadal wyświetlany monit logowania okna dialogowego zawsze żądania strony chronione (patrz rysunek 4).

**Rysunek 4 — okno dialogowe logowania uwierzytelniania podstawowego**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono, jak można użyć uwierzytelniania systemu Windows w kontekście aplikacji platformy ASP.NET MVC. Wiesz, jak włączyć uwierzytelnianie systemu Windows w pliku konfiguracji aplikacji sieci web i jak skonfigurować uwierzytelnianie w usługach IIS. Ponadto przedstawiono sposób używania &lt;autoryzacji&gt; atrybutu, aby ograniczyć dostęp do akcji kontrolera do określonych użytkowników systemu Windows lub grupy.

>[!div class="step-by-step"]
[Poprzednie](authenticating-users-with-forms-authentication-vb.md)
[dalej](preventing-javascript-injection-attacks-vb.md)
