# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację. Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`. Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.
 
# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.

  Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`. Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`. Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.

  
# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* W programie Visual Studio naciśnij pozycję **opt-cmd-Return** , aby uruchomić bez debugera. Alternatywnie przejdź do paska menu i przejdź do pozycji **uruchom > Uruchom bez debugowania**.

  Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.

<!-- End of VS tabs -->

---