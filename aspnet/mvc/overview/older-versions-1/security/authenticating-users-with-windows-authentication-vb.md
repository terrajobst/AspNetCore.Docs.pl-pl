---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Uwierzytelnianie użytkowników za pomocą uwierzytelniania Windows (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak używać uwierzytelniania Windows w kontekście aplikacji MVC. Dowiesz się, jak włączyć uwierzytelnianie Windows w ramach Twojej aplikacji sieci web co...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: c3c7f7bfc15483451ede5382bf3fc02db47dea0e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811691"
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Uwierzytelnianie użytkowników za pomocą uwierzytelniania Windows (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak używać uwierzytelniania Windows w kontekście aplikacji MVC. Dowiesz się, jak włączyć uwierzytelnianie Windows w pliku konfiguracji sieci web aplikacji i jak skonfigurować uwierzytelnianie za pomocą programu IIS. Na koniec dowiesz się, jak używać atrybutu [Authorize] do ograniczania dostępu do akcji kontrolera do konkretnych użytkowników Windows lub grup.


Celem tego samouczka jest wyjaśniają, jak możesz korzystać ze zabezpieczeń funkcji wbudowanych w Internet Information Services do hasła ochrony widoków w aplikacji MVC. Dowiesz się, jak umożliwić akcji kontrolera być przywoływane tylko przez określonych użytkowników Windows lub użytkowników, którzy są członkami określonej grupy Windows.

Przy użyciu uwierzytelniania Windows sens, kiedy tworzysz wewnętrznej witryny sieci Web firmy (witryny intranetowej) i chcesz, aby użytkownicy mogli korzystać z ich standardowe Windows użytkownika nazwy i hasła podczas uzyskiwania dostępu do witryny sieci Web. Jeśli tworzysz na zewnątrz połączonego z witryny sieci Web (Internet witrynę sieci Web) uwierzytelnianie formularzy zamiast tego Rozważ użycie.

#### <a name="enabling-windows-authentication"></a>Włączanie uwierzytelniania Windows

Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, uwierzytelnianie Windows nie jest włączone domyślnie. Uwierzytelnianie formularzy jest to domyślny typ uwierzytelniania, który jest włączony dla aplikacji MVC. Należy włączyć uwierzytelnianie Windows, modyfikując plik konfiguracji (web.config) w sieci web aplikacji MVC. Znajdź &lt;uwierzytelniania&gt; sekcji i zmodyfikuj go, aby używać Windows zamiast uwierzytelniania formularzy w następujący sposób:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Po włączeniu uwierzytelniania Windows, serwer sieci web staje się odpowiedzialna za uwierzytelnianie użytkowników. Zazwyczaj istnieją dwa różne typy serwerów sieci web, które można użyć podczas tworzenia i wdrażania aplikacji ASP.NET MVC.

Najpierw podczas tworzenia aplikacji MVC, należy użyć serwera sieci Web platformy ASP.NET Development dołączone do programu Visual Studio. Domyślnie serwer sieci Web programu ASP.NET Development wykonuje wszystkich stron w kontekście bieżącego konta Windows (niezależnie od konto używane do logowania się do Windows).

Serwer sieci Web programu ASP.NET Development obsługuje również uwierzytelnianie NTLM. Kliknij prawym przyciskiem myszy nazwę projektu w oknie Eksploratora rozwiązań i wybierając pozycję Właściwości, można włączyć uwierzytelnianie NTLM. Następnie wybierz kartę sieci Web i zaznacz pole wyboru NTLM (patrz rysunek 1).

**Rysunek 1 — włączenie uwierzytelniania NTLM dla serwera sieci Web programu ASP.NET Development**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

W produkcyjnej aplikacji internetowej strony, należy użyć usług IIS jako serwera sieci web. Usługi IIS obsługują kilka typów uwierzytelniania, w tym:

- Uwierzytelnianie podstawowe — zdefiniowany w ramach protokołu HTTP 1.0. Wysyła nazwy użytkownika i hasła w postaci zwykłego tekstu (kodowanie Base64) przez Internet. — Uwierzytelnianie szyfrowane — wysyła skrót hasła, zamiast hasła, przez internet. — Zintegrowane uwierzytelnianie Windows (NTLM) — najlepszy typ uwierzytelniania do użycia w środowisku sieci intranet przy użyciu systemu windows. -Certyfikat uwierzytelniania — umożliwia użycie uwierzytelniania przy użyciu certyfikatu klienta. Mapuje certyfikat do konta użytkownika Windows.

> [!NOTE] 
> 
> Aby uzyskać bardziej szczegółowym omówieniem tych różnych typów uwierzytelniania, zobacz [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Aby włączyć określonego typu uwierzytelniania, można użyć Menedżera internetowych usług informacyjnych. Należy pamiętać, że wszystkie typy uwierzytelniania nie są dostępne w przypadku każdej wersji systemu operacyjnego. Ponadto jeśli używasz usług IIS 7.0, Windows Vista, należy włączyć różnych typów uwierzytelniania Windows zanim pojawią się w Menedżera internetowych usług informacyjnych. Otwórz **Panelu sterowania, programy, programy i funkcje, Windows, Włącz lub wyłącz funkcje**i rozwiń węzeł Internetowe usługi informacyjne (zobacz rysunek 2).

**Rysunek 2 — funkcje usług IIS Windows włączenie**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Za pomocą programu Internet Information Services, można włączyć lub wyłączyć różnego rodzaju uwierzytelniania. Przykładowo, rysunek 3 ilustruje wyłączenie uwierzytelniania anonimowego i włączanie uwierzytelniania zintegrowanego Windows (NTLM) podczas korzystania z usług IIS 7.0.

**Rysunek 3 — Włączanie uwierzytelniania zintegrowanego Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autoryzowanie Windows użytkowników i grup

Po włączeniu uwierzytelniania Windows, można użyć &lt;Autoryzuj&gt; atrybutu, aby kontrolować dostęp do kontrolerów i akcji kontrolera. Ten atrybut może być stosowane do całego kontrolera MVC lub akcji określony kontroler.

Na przykład kontrolera głównego w ofercie 1 udostępnia trzy czynności o nazwie indeks() CompanySecrets() i StephenSecrets(). Każda osoba może wywołać akcję indeks(). Jednak tylko członkowie lokalnej grupy Menedżerowie Windows wywołać akcję CompanySecrets(). Na koniec tylko użytkownika domeny Windows o nazwie Autor: Stephen (w domenie Redmond) wywołać akcję StephenSecrets().

**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Ze względu na Windows (Kontrola konta), podczas pracy z usługą Windows Vista lub Windows Server 2008, do lokalnej grupy Administratorzy będą zachowywać się inaczej niż inne grupy. &lt;Autoryzuj&gt; atrybutu nie będą rozpoznawane poprawnie członkiem lokalnej grupy administratorów, chyba że zmodyfikujesz ustawień funkcji Kontrola konta użytkownika na komputerze.


Dokładnie co się stanie po użytkownik podejmie próbę wywołania akcji kontrolera, nie będąc odpowiednie uprawnienia, zależy od typu uwierzytelnianie jest włączone. Domyślnie, korzystając z programem ASP.NET Development Server po prostu pobrać pustą stronę. Strona jest obsługiwana przy użyciu **401 nieautoryzowane** stanu odpowiedzi HTTP.

Jeśli z drugiej strony, usług IIS za pomocą wyłączone uwierzytelnianie anonimowe i włączone uwierzytelnianie podstawowe, a następnie nadal jest wyświetlany monit okna dialogowego logowania każdorazowo żądania strony chronione (zobacz rysunek 4).

**Rysunek 4 — okno dialogowe logowania uwierzytelniania podstawowego**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono, jak można użyć uwierzytelniania Windows w kontekście aplikacji ASP.NET MVC. Przedstawiono sposób włączania uwierzytelniania Windows w pliku konfiguracji sieci web aplikacji oraz jak skonfigurować uwierzytelnianie za pomocą programu IIS. Ponadto przedstawiono sposób użycia &lt;Autoryzuj&gt; atrybutu, aby ograniczyć dostęp do akcji kontrolera do konkretnych użytkowników Windows lub grup.

> [!div class="step-by-step"]
> [Poprzednie](authenticating-users-with-forms-authentication-vb.md)
> [dalej](preventing-javascript-injection-attacks-vb.md)
