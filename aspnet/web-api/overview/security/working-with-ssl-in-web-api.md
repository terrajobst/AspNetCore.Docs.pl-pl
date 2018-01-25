---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "Praca z protokołem SSL w składniku Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Przedstawia sposób użycia protokołu SSL z interfejsu API sieci Web ASP.NET, w tym o korzystaniu z certyfikatów SSL klienta."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>Praca z protokołem SSL w składniku Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Kilka wspólnych schematów uwierzytelniania nie są bezpieczne za pośrednictwem zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie oparte na formularzach Wyślij niezaszyfrowane poświadczeń. Do zabezpieczenia, te schematy uwierzytelniania *musi* używają protokołu SSL. Ponadto certyfikaty SSL klienta może służyć do uwierzytelniania klientów.

## <a name="enabling-ssl-on-the-server"></a>Włączanie protokołu SSL na serwerze

Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszy:

- Utwórz lub uzyskać certyfikat. Do testowania, można utworzyć certyfikatu z podpisem własnym.
- Dodaj powiązanie HTTPS.

Aby uzyskać więcej informacji, zobacz [jak się protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Do testowania lokalnego, można włączyć protokołu SSL w usługach IIS Express z programu Visual Studio. W oknie właściwości ustaw **włączony protokół SSL** do **True**. Zanotuj wartość ustawienia **adres URL protokołu SSL**; Użyj tego adresu URL do testowania połączenia HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Wymuszanie protokołu SSL w kontrolerze interfejsu API sieci Web

Jeśli masz zarówno HTTPS i powiązanie HTTP, klienci mogą nadal używać HTTP dostęp do witryny. Mogą zezwalać niektórych zasobów był dostępny za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL. W takim przypadku użyj filtru akcji, aby wymagać protokołu SSL do chronionych zasobów. Poniższy kod przedstawia filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Dodaj filtr do wszystkich akcji interfejsu API sieci Web, które wymagają protokołu SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certyfikaty SSL klienta

Protokół SSL umożliwia uwierzytelnianie za pomocą certyfikatów infrastruktury kluczy publicznych. Serwer musi dostarczyć certyfikat, który służy do uwierzytelniania serwera do klienta. Mniej często od klienta zapewnienia certyfikatu na serwerze, ale jest jedną z opcji uwierzytelniania klientów. Aby używać certyfikatów klienta przy użyciu protokołu SSL, należy rozdystrybuować podpisanych certyfikatów użytkowników. Dla wielu typów aplikacji nie będą poprawne działanie, ale w niektórych środowiskach (na przykład dla wersji enterprise) może być możliwe.

| Zalety | Wady |
| --- | --- |
| -Certyfikatu poświadczenia są mocniejszy niż element nazwy użytkownika i hasła. -SSL zawiera pełną bezpiecznego kanału z uwierzytelnianiem, integralności i szyfrowania wiadomości. | -Należy uzyskać i zarządzać certyfikatami infrastruktury kluczy publicznych. Platform klient musi obsługiwać certyfikaty SSL klienta. |

Aby skonfigurować usługi IIS, aby akceptować certyfikaty klienta, otwórz Menedżera usług IIS i wykonaj następujące czynności:

1. Kliknij węzeł witryny w widoku drzewa.
2. Kliknij dwukrotnie **ustawienia protokołu SSL** funkcji w środkowym okienku.
3. W obszarze **certyfikaty klienta**, wybierz jedną z następujących opcji: 

    - **Zaakceptuj**: IIS będzie akceptować certyfikat od klienta, ale nie wymaga jednego.
    - **Wymagaj**: wymagany certyfikat klienta. (Aby włączyć tę opcję, należy również wybrać opcję "Wymagaj protokołu SSL")

W pliku ApplicationHost.config, można również ustawić następujące opcje:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** flagi oznacza, że usługi IIS będzie akceptować certyfikat od klienta, ale nie wymaga jednego (odpowiada opcji "Zaakceptuj" w Menedżerze usług IIS). Aby wymagać certyfikatu, należy ustawić **SslRequireCert** flagi. Do testowania, można również ustawić te opcje w usługach IIS Express, w lokalnym hosta aplikacji. Plik konfiguracji znajduje się w "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Tworzenie certyfikatu klienta do testowania

Do celów testowych, można użyć [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) można utworzyć certyfikatu klienta. Najpierw utwórz urząd główny testu:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert wyświetli monit o wprowadzenie hasła dla klucza prywatnego.

Następnie Dodaj certyfikat do badania serwera "Zaufane główne urzędy certyfikacji" przechowywania w następujący sposób:

1. Otwórz program MMC.
2. W obszarze **pliku**, wybierz pozycję **Dodaj/Usuń przystawkę**.
3. Wybierz **konto komputera**.
4. Wybierz **komputera lokalnego** i Zakończ pracę kreatora.
5. W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".
6. Na **akcji** menu wskaż **wszystkie zadania**, a następnie kliknij przycisk **importu** można uruchomić Kreatora importu certyfikatów.
7. Przejdź do pliku certyfikatu TempCA.cer.
8. Kliknij przycisk **Otwórz**, następnie kliknij przycisk **dalej** i Zakończ pracę kreatora. (Pojawi się monit o ponowne wprowadzenie hasła.)

Teraz należy utworzyć certyfikat klienta, który jest podpisany przez pierwszy certyfikat:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>W składniku Web API przy użyciu certyfikatów klienta

Po stronie serwera można uzyskać certyfikatu klienta, wywołując [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) komunikatu żądania. Metoda zwraca wartość null, jeśli certyfikat klienta. W przeciwnym razie zwraca **X509Certificate2** wystąpienia. Aby uzyskać informacje z certyfikatu, takie jak wystawcy i podmiotu, użyj tego obiektu. Następnie można użyć tych informacji do uwierzytelniania i/lub autoryzacji.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
