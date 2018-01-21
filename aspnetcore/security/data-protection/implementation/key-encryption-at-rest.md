---
title: Szyfrowanie klucza magazynowane
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core ochrony klucza szyfrowanie danych przechowywanych."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: a0b9ab31264e5cae666a69491bf4a8ee8251a86f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="8af42-103">Szyfrowanie klucza magazynowane</span><span class="sxs-lookup"><span data-stu-id="8af42-103">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="8af42-104">Domyślnie system ochrony danych [wykorzystuje heurystyki](xref:security/data-protection/configuration/default-settings) ustalenie, jak kryptograficznych materiału klucza powinny być szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="8af42-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="8af42-105">Deweloper może zastąpić heurystyki i ręcznie określić, jak klucze powinny być szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="8af42-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="8af42-106">Jeśli określisz jawne klucza szyfrowania w mechanizmu reszta systemu ochrony danych będą wyrejestrowania domyślnego mechanizmu magazynu kluczy heurystyki podane.</span><span class="sxs-lookup"><span data-stu-id="8af42-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="8af42-107">Należy [Określ mechanizmu magazynowania kluczy jawne](key-storage-providers.md#data-protection-implementation-key-storage-providers), w przeciwnym razie system ochrony danych nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="8af42-107">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="8af42-108">System ochrony danych jest dostarczany z trzech mechanizmów szyfrowania w polu.</span><span class="sxs-lookup"><span data-stu-id="8af42-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="8af42-109">DPAPI systemu Windows</span><span class="sxs-lookup"><span data-stu-id="8af42-109">Windows DPAPI</span></span>

<span data-ttu-id="8af42-110">*Ten mechanizm jest dostępna tylko w systemie Windows.*</span><span class="sxs-lookup"><span data-stu-id="8af42-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="8af42-111">W przypadku systemu Windows DPAPI materiału klucza będą szyfrowane za pomocą [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) przed jest utrwalana w magazynie.</span><span class="sxs-lookup"><span data-stu-id="8af42-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="8af42-112">DPAPI to mechanizm szyfrowania odpowiednie dane, które nigdy nie będzie można odczytać poza bieżącym komputerze (mimo że można utworzyć kopię tych kluczy do usługi Active Directory, zobacz [DPAPI i profili mobilnych](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="8af42-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="8af42-113">Na przykład skonfigurować szyfrowanie klucza na rest DPAPI.</span><span class="sxs-lookup"><span data-stu-id="8af42-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="8af42-114">Jeśli `ProtectKeysWithDpapi` jest wywoływana bez parametrów, tylko konta użytkownika systemu Windows można odszyfrować utrwalonego materiału klucza.</span><span class="sxs-lookup"><span data-stu-id="8af42-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="8af42-115">Opcjonalnie możesz określić dowolne konto użytkownika na komputerze (nie tylko bieżące konto użytkownika) należy stanie odszyfrować materiał kluczy, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8af42-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="8af42-116">Certyfikat X.509</span><span class="sxs-lookup"><span data-stu-id="8af42-116">X.509 certificate</span></span>

<span data-ttu-id="8af42-117">*Ten mechanizm nie jest dostępny na `.NET Core 1.0` lub `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="8af42-117">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="8af42-118">Jeśli aplikacja zostanie rozmieszczona na wielu komputerach, może być wygodne rozpowszechniają udostępnionego certyfikatu X.509 maszyn i konfiguracji aplikacji, aby użyć tego certyfikatu do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="8af42-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="8af42-119">Poniżej znajduje się przykład.</span><span class="sxs-lookup"><span data-stu-id="8af42-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="8af42-120">Obsługiwane są tylko certyfikatów z kluczami prywatnymi CAPI ze względu na ograniczenia .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8af42-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="8af42-121">Zobacz [szyfrowania opartego na certyfikatach z DPAPI NG Windows](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) poniżej dla możliwe obejścia tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="8af42-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="8af42-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="8af42-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="8af42-123">*Ten mechanizm jest dostępna tylko w systemie Windows 8 / Windows Server 2012 lub nowszym.*</span><span class="sxs-lookup"><span data-stu-id="8af42-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="8af42-124">Począwszy od systemu Windows 8, system operacyjny obsługuje DPAPI-NG (nazywanych również CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="8af42-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="8af42-125">Microsoft wychodzi poza jego scenariusz użycia w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="8af42-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="8af42-126">Rozwiązań w chmurze, często wymaga tej zawartości zaszyfrowane na jednym komputerze można odszyfrować na innym.</span><span class="sxs-lookup"><span data-stu-id="8af42-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="8af42-127">W związku z tym począwszy od systemu Windows 8, Microsoft rozszerzony pomysł obejmują scenariusze chmury przy użyciu stosunkowo prosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8af42-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="8af42-128">Ten nowy interfejs API, nazywany DPAPI NG, umożliwia bezpieczne udostępnianie kluczy tajnych (klucze, hasła, materiału klucza) i komunikaty, aby chronić je do zestawu podmiotów zabezpieczeń, które mogą służyć do usunięcia ochrony je na różnych komputerach po odpowiedniego uwierzytelnienia i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8af42-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="8af42-129">Z [o CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="8af42-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="8af42-130">Podmiot zabezpieczeń został zakodowany jako regułę ochrony deskryptora.</span><span class="sxs-lookup"><span data-stu-id="8af42-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="8af42-131">Należy wziąć pod uwagę poniższym przykładzie, który szyfruje materiału klucza tak, aby tylko przyłączonych do domeny użytkownika z określonym identyfikatorem SID może odszyfrować materiału klucza.</span><span class="sxs-lookup"><span data-stu-id="8af42-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="8af42-132">Istnieje też bezparametrowy przeciążenia `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="8af42-132">There is also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="8af42-133">Jest to metoda wygody służącą do reguły "identyfikatora SID = min", gdzie analizę jest identyfikator SID konta użytkownika systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="8af42-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="8af42-134">W tym scenariuszu kontroler domeny usługi AD jest odpowiedzialna za dystrybucję kluczy szyfrowania używanych przez operacje DPAPI NG.</span><span class="sxs-lookup"><span data-stu-id="8af42-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="8af42-135">Użytkownik docelowy będzie można odszyfrować zaszyfrowanego ładunku z dowolnego komputera przyłączonych do domeny (pod warunkiem, że proces jest uruchomiony w ramach ich tożsamości).</span><span class="sxs-lookup"><span data-stu-id="8af42-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="8af42-136">Na podstawie certyfikatu szyfrowania z DPAPI NG systemu Windows</span><span class="sxs-lookup"><span data-stu-id="8af42-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="8af42-137">Jeśli pracujesz na Windows 8.1 / Windows Server 2012 R2 lub później, umożliwia Windows DPAPI-NG szyfrowanie oparte na certyfikatach, nawet wtedy, gdy aplikacja jest uruchomiona [.NET Core](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="8af42-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="8af42-138">Aby skorzystać z tego, użyj ciągu deskryptora reguły "certyfikatu = HashId:thumbprint", gdzie odcisk palca jest kodowany w formacie hex SHA1 odcisk palca certyfikatu do użycia.</span><span class="sxs-lookup"><span data-stu-id="8af42-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="8af42-139">Poniżej znajduje się przykład.</span><span class="sxs-lookup"><span data-stu-id="8af42-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="8af42-140">Dowolnej aplikacji, która jest wskazywana w tym repozytorium musi być uruchomiona na Windows 8.1 / Windows Server 2012 R2 lub nowszej, aby być w stanie odszyfrować tego klucza.</span><span class="sxs-lookup"><span data-stu-id="8af42-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="8af42-141">Niestandardowe klucza szyfrowania</span><span class="sxs-lookup"><span data-stu-id="8af42-141">Custom key encryption</span></span>

<span data-ttu-id="8af42-142">Jeśli mechanizmów w polu nie są odpowiednie, deweloper można określić własnych mechanizmów szyfrowania zapewniając niestandardowego `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="8af42-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
