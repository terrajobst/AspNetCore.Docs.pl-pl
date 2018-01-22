---
title: "Wyłączanie ochrony ładunków, której klucze zostały odwołane."
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak wyłączyć ochronę danych i chronione przy użyciu kluczy, które od chwili zostały odwołane, w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: f2425de3f790cd8dab17940ec52a2a7e170cc630
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="6f213-103">Wyłączanie ochrony ładunków, której klucze zostały odwołane.</span><span class="sxs-lookup"><span data-stu-id="6f213-103">Unprotecting payloads whose keys have been revoked</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="6f213-104">Interfejsy API ochrony danych platformy ASP.NET Core nie są głównie przeznaczone do nieograniczonego trwałość ładunki poufne.</span><span class="sxs-lookup"><span data-stu-id="6f213-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="6f213-105">Innych technologii, takich jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczonego magazynu i mają możliwości odpowiednio silne zarządzania kluczami.</span><span class="sxs-lookup"><span data-stu-id="6f213-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="6f213-106">Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu interfejsy API ochrony danych platformy ASP.NET Core dla długoterminowej ochrony poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="6f213-106">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="6f213-107">Klucze są nigdy nie usuwane z pierścienia klucza, więc `IDataProtector.Unprotect` zawsze można odzyskać istniejących ładunków tak długo, jak klucze są dostępne i prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6f213-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="6f213-108">Jednak problemy występują, gdy developer podejmie próbę wyłączania ochrony danych, który jest chroniony za pomocą klucza odwołane, jako `IDataProtector.Unprotect` spowoduje zgłoszenie wyjątku w takim przypadku.</span><span class="sxs-lookup"><span data-stu-id="6f213-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="6f213-109">Może to być poprawnie dla ładunków krótkim okresie lub przejściowy (na przykład tokeny uwierzytelniania), tego rodzaju ładunki można łatwo odtworzyć przez system, a w najgorszym przypadku użytkownik może być konieczne ponowne zalogowanie.</span><span class="sxs-lookup"><span data-stu-id="6f213-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="6f213-110">Nawet w przypadku utrwalonego ładunków, o `Unprotect` throw może prowadzić do utraty danych można zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="6f213-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="6f213-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="6f213-111">IPersistedDataProtector</span></span>

<span data-ttu-id="6f213-112">Na potrzeby scenariusza umożliwienia ładunków do usunięcia ochrony nawet w wypadku klucze odwołane, system ochrony danych zawiera `IPersistedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="6f213-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="6f213-113">Można pobrać wystąpienia `IPersistedDataProtector`, wystarczy pobrać wystąpienia `IDataProtector` w sposób normalne i spróbuj wykonać rzutowanie `IDataProtector` do `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="6f213-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="6f213-114">Nie wszystkie `IDataProtector` wystąpienia mogą być rzutowane na `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="6f213-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="6f213-115">Programiści powinni używać języka C# jako operator lub podobne, aby uniknąć wyjątki środowiska uruchomieniowego spowodowane przez nieprawidłowy rzutowania i powinny być przygotowane do odpowiednią obsługę w przypadku awarii.</span><span class="sxs-lookup"><span data-stu-id="6f213-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="6f213-116">`IPersistedDataProtector`udostępnia następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="6f213-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="6f213-117">Ten interfejs API przyjmuje chronione obciążenie (w postaci tablicy bajtów) i zwraca niechronione ładunku.</span><span class="sxs-lookup"><span data-stu-id="6f213-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="6f213-118">Nie ma żadnych przeciążenia opartego na ciągach.</span><span class="sxs-lookup"><span data-stu-id="6f213-118">There is no string-based overload.</span></span> <span data-ttu-id="6f213-119">Dostępne są następujące dwa parametry wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="6f213-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="6f213-120">`requiresMigration`: ustawionej na wartość true, jeśli klucz używany do ochrony tego ładunku nie jest już aktywne domyślny klucz, np. klucz używany do ochrony tego ładunku jest stary i ma klucz Wycofanie operacji ponieważ nastąpi miejsce.</span><span class="sxs-lookup"><span data-stu-id="6f213-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="6f213-121">Obiekt wywołujący możesz rozważyć ponownej ochrony ładunku w zależności od ich potrzeb biznesowych.</span><span class="sxs-lookup"><span data-stu-id="6f213-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="6f213-122">`wasRevoked`: zostanie ustawiona na true, jeśli klucz używany do ochrony tego ładunku został odwołany.</span><span class="sxs-lookup"><span data-stu-id="6f213-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="6f213-123">Zachować wyjątkową ostrożność podczas przekazywania `ignoreRevocationErrors: true` do `DangerousUnprotect` metody.</span><span class="sxs-lookup"><span data-stu-id="6f213-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="6f213-124">Jeśli po wywołaniu tej metody `wasRevoked` ma wartość true, a następnie klucz używany do ochrony tego ładunku został odwołany i autentyczności ładunku powinny być traktowane jako podejrzana.</span><span class="sxs-lookup"><span data-stu-id="6f213-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="6f213-125">W takim przypadku tylko kontynuowania działania na niechronionych ładunku, jeśli masz niektórych oddzielne gwarancji czy jest autentyczna, np. jego obiektu pochodzące z bezpiecznej bazie danych, a nie są wysyłane przez klienta niezaufanych sieci web.</span><span class="sxs-lookup"><span data-stu-id="6f213-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]