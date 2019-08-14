* <span data-ttu-id="df2d8-101">Aby ufać certyfikatowi programistycznemu HTTPS, należy uruchomić następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="df2d8-101">Trust the HTTPS development certificate by running the following command:</span></span>

  ```console
  dotnet dev-certs https --trust
  ```
  
  <span data-ttu-id="df2d8-102">Poprzednie polecenie nie działa w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="df2d8-102">The preceding command doesn't work on Linux.</span></span> <span data-ttu-id="df2d8-103">Zapoznaj się z dokumentacją dystrybucji systemu Linux w celu zaufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="df2d8-103">See your Linux distribution's documentation for trusting a certificate.</span></span>

  <span data-ttu-id="df2d8-104">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="df2d8-104">The preceding command displays the following dialog:</span></span>

  ![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

* <span data-ttu-id="df2d8-106">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="df2d8-106">Select **Yes** if you agree to trust the development certificate.</span></span>

  <span data-ttu-id="df2d8-107">Aby uzyskać więcej informacji, zobacz artykuł [Ufaj certyfikatowi deweloperskim protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) .</span><span class="sxs-lookup"><span data-stu-id="df2d8-107">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>
  
