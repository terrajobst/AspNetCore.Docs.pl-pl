---
uid: web-api/overview/security/integrated-windows-authentication
title: Zintegrowane uwierzytelnianie systemu Windows | Dokumentacja firmy Microsoft
author: MikeWasson
description: "W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu zintegrowanego uwierzytelniania systemu Windows."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>Zintegrowane uwierzytelnianie systemu Windows
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Zintegrowane uwierzytelnianie systemu Windows umożliwia użytkownikom logowanie się przy użyciu poświadczeń systemu Windows przy użyciu protokołu Kerberos lub NTLM. Klient wysyła poświadczenia w nagłówku autoryzacji. Uwierzytelnianie systemu Windows jest najodpowiedniejsze do użytku w środowisku sieci intranet. Aby uzyskać więcej informacji, zobacz [uwierzytelniania systemu Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Zalety | Wady |
| --- | --- |
| -Wbudowane w usługach IIS. -Nie wysyła poświadczenia użytkownika w żądaniu. — Jeśli komputer klienta należy do domeny (na przykład aplikacja intranetu), użytkownik musi wprowadzić poświadczenia. | — Nie jest zalecane dla aplikacji internetowych. -Wymaga obsługi protokołu Kerberos lub NTLM w kliencie. -Klient musi należeć do domeny usługi Active Directory. |

> [!NOTE]
> Jeśli aplikacja jest hostowana na platformie Azure i masz lokalnej domeny usługi Active Directory, należy wziąć pod uwagę federowania lokalnej usługi AD z usługą Azure Active Directory. W ten sposób użytkownicy mogą Zaloguj się przy użyciu poświadczeń lokalnego, ale uwierzytelnianie jest wykonywane przez usługę Azure AD. Aby uzyskać więcej informacji, zobacz [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Aby utworzyć aplikację, która używa zintegrowanego uwierzytelniania systemu Windows, wybierz szablon "Intranet aplikacji" w Kreatorze projektu MVC 4. Ten szablon projektu umieszcza następujące ustawienie w pliku Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Po stronie klienta zintegrowanego uwierzytelniania systemu Windows współdziała z dowolnej przeglądarki obsługującej [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schemat uwierzytelniania, w tym w większości przeglądarek głównych. Dla aplikacji klienckich, .NET **HttpClient** klasa obsługuje uwierzytelnianie systemu Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Uwierzytelnianie systemu Windows jest narażony na fałszerstwie żądania (CSRF) między lokacjami. Zobacz [zapobieganie Fałszerstwie żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
