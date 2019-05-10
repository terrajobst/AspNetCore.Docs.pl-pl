# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="bf578-101">Jak kompilacji/uruchomić próbki danych bezpiecznego użytkownika</span><span class="sxs-lookup"><span data-stu-id="bf578-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="bf578-102">Ustaw hasło, za pomocą narzędzia menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="bf578-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="bf578-103">Aktualizacja bazy danych:</span><span class="sxs-lookup"><span data-stu-id="bf578-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="bf578-104">Włączanie obsługi protokołu HTTPS w projekcie</span><span class="sxs-lookup"><span data-stu-id="bf578-104">Enable HTTPS in the project</span></span>
