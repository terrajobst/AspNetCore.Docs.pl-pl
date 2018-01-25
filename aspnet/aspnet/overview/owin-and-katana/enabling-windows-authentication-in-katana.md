---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Włączanie uwierzytelniania systemu Windows w kolekcji Katana | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w kolekcji Katana. Obejmuje dwa scenariusze: za pomocą usług IIS do hosta Katana i hosta samodzielnego Kat za pomocą HttpListener..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a>Włączanie uwierzytelniania systemu Windows w kolekcji Katana
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w kolekcji Katana. Obejmuje dwa scenariusze: za pomocą usług IIS do hosta Katana i za pomocą HttpListener hosta samodzielnego Katana niestandardowy proces. Dzięki użyciu Dorrans Marcin Matson Dominik i Krzysztof Roaming weryfikacji w tym artykule.


Katana to implementacja firmy Microsoft [OWIN](http://owin.org/), Otwórz interfejs sieci Web dla platformy .NET. Wprowadzenie do OWIN i Katana może odczytywać [tutaj](an-overview-of-project-katana.md). Architektura OWIN zawiera kilka warstw:

- Host: Zarządza procesem, w której jest uruchamiana potoku OWIN.
- Serwer: Otwiera gniazda sieci i nasłuchuje żądań.
- Oprogramowanie pośredniczące: Przetwarza żądania HTTP i odpowiedzi.

Katana obecnie zawiera dwa serwery, które obsługuje zintegrowane uwierzytelnianie systemu Windows:

- **Microsoft.Owin.Host.SystemWeb**. Korzysta z usług IIS z potoku ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Ten serwer jest obecnie opcją domyślną, gdy hostingu samodzielnego Katana.

> [!NOTE]
> Katana nie jest aktualnie dostępny oprogramowanie pośredniczące OWIN do uwierzytelniania systemu Windows, ponieważ ta funkcja jest już dostępny na serwerach.


## <a name="windows-authentication-in-iis"></a>Uwierzytelnianie systemu Windows w usługach IIS

Używając Microsoft.Owin.Host.SystemWeb, może po prostu włącz uwierzytelnianie systemu Windows w usługach IIS.

Zacznijmy od utworzenia nowej aplikacji ASP.NET przy użyciu szablonu projektu "Aplikacja sieci Web ASP.NET pusty".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Następnie dodaj pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Teraz Dodaj klasę o nazwie `Startup` z następującym kodem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

To wszystko, musisz utworzyć aplikację "Hello world" dla OWIN działającą na serwerze IIS. Naciśnij klawisz F5, aby debugować aplikację. Powinny pojawić się "Witaj świecie!" w oknie przeglądarki.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Następnie firma Microsoft będzie włączyć uwierzytelnianie systemu Windows w usługach IIS Express. Z **widoku** menu, wybierz opcję **właściwości**. Kliknij nazwę projektu w Eksploratorze rozwiązań, aby wyświetlić właściwości projektu.

W **właściwości** ustaw **uwierzytelnianie anonimowe** do **wyłączone** i ustaw **uwierzytelniania systemu Windows** do  **Włączone**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Po uruchomieniu aplikacji w programie Visual Studio, usługi IIS Express wymaga poświadczeń użytkownika systemu Windows. Zobacz ten przy użyciu [Fiddler](http://fiddler2.com/home) lub innego protokołu HTTP, narzędzia debugowania. Oto przykład odpowiedzi HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Nagłówki WWW-Authenticate, w tej odpowiedzi wskazują, że serwer obsługuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokołu, który korzysta z protokołu Kerberos lub NTLM.

Później, podczas wdrażania aplikacji na serwerze, wykonaj [te kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Aby włączyć uwierzytelnianie systemu Windows w usługach IIS na tym serwerze.

## <a name="windows-authentication-in-httplistener"></a>Uwierzytelnianie systemu Windows w HttpListener

Jeśli używasz Microsoft.Owin.Host.HttpListener do hosta samodzielnego Katana, możesz je włączyć uwierzytelnianie systemu Windows bezpośrednio na **HttpListener** wystąpienia.

Najpierw utwórz nową aplikację konsoli. Następnie dodaj pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Teraz Dodaj klasę o nazwie `Startup` z następującym kodem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Ta klasa implementuje w tym samym przykładzie "Hello world" z przed, ale także ustawia uwierzytelniania systemu Windows jako schematu uwierzytelniania.

Wewnątrz `Main` funkcji, Rozpocznij potoku OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

W narzędziu Fiddler, aby upewnić się, że aplikacja używa uwierzytelniania systemu Windows można wysyłać żądania:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Tematy pokrewne

[Omówienie projektu Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Opis uwierzytelniania formularzy OWIN w nazwie wzorca MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
