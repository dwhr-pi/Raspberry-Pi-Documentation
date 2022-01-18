# NPM Hilfe

## Verwendung: npm &lt;Befehl&gt; 

	*[npm install](#npm install) - Installiert alle Abhängigkeiten in Ihrem Projekt.  
	*[npm install &lt;foo&gt;](#npm install &lt;foo&gt;) -  Fügt die &lt;foo(Name)&gt;-Abhängigkeit zu Ihrem Projekt hinzu.  
npm test]() - Führt die Tests dieses Projekts aus.  
npm run &lt;foo&gt; - Führt das Skript mit dem Namen &lt;foo&gt; aus.  
npm &lt;Befehl&gt; -h - Schnelle Hilfe zu &lt;Befehl&gt;.  
npm -l - Zeigt Nutzungsinformationen für alle Befehle an.  
npm help &lt;Begriff&gt; - sucht nach Hilfe zu &lt;Begriff&gt;.  
npm help npm - beteiligter Übersicht.  

Alle Befehle: 

    [access](), [adduser](), [audit](), [bin](), [bugs](), [cache](), [ci](), [completion](), 
	[config](), [dedupe](), [deprecate](), [diff](), [dist-tag](), [docs,](), [doctor](), 
	[edit](), [exec,](), [explain](), [explore](), [find-dupes](), [fund](), [get](), [help](), 
	[hook](), [init](), [install](), [install-ci-test](), [install-test](), [link](), 
	[ll](), [login](), [logout](), [ls](), [org](), [outdated](), [owner](), [pack](), [ping](), 
	[prefix](), [profile](), [prune](), [publish](), [rebuild,](), [repo](), [restart](), 
	[root](), [run-script](), [search](), [set](), [set-script](), [shrinkwrap](), [star](), 
	[stars](), [start](), [stop](), [team](), [test](), [token](), [un]()

[Siehe auch](https://docs.npmjs.com/cli/v7/commands)


Geben Sie die Konfigurationen in der ini-formatierten Datei an:  
```
	/home/dietpi/.npmrc  
```
oder auf der Kommandozeile über: npm &lt;command&gt; --key=value  

Weitere Konfigurationsinformationen: npm help config  
Konfigurationsfelder: npm help 7 config  





|Befehl|Beschreibung|
|:------------------------------------:|------------------------------------|
|npm|[JavaScript-Paketmanager](https://docs.npmjs.com/cli/v7/commands/npm)|
|npm access|[Zugriffsebene für veröffentlichte Pakete festlegen](https://docs.npmjs.com/cli/v7/commands/npm-access)|
|npm adduser|[Registrierungsbenutzerkonto hinzufügen](https://docs.npmjs.com/cli/v7/commands/npm-adduser)|
|npm audit|[Sicherheitsaudit ausführen](https://docs.npmjs.com/cli/v7/commands/npm-audit)|
|npm bin|[Ordner npm bin anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-bin)|
|npm bugs|[Vielleicht Fehler für ein Paket in einem Webbrowser](https://docs.npmjs.com/cli/v7/commands/npm-bugs)|
|npm cache|[Manipuliert den Paket-Cache](https://docs.npmjs.com/cli/v7/commands/npm-cache)|
|npm ci|[Installieren Sie ein Projekt mit einer sauberen Weste](https://docs.npmjs.com/cli/v7/commands/npm-ci)|
|npm completion|[Tab-Vervollständigung für npm](https://docs.npmjs.com/cli/v7/commands/npm-completion)|
|npm config|[Verwalten der npm-Konfigurationsdateien](https://docs.npmjs.com/cli/v7/commands/npm-config)|
|npm dedupe|[Duplizierung reduzieren](https://docs.npmjs.com/cli/v7/commands/npm-dedupe)|
|npm deprecate|[Eine Version eines Pakets verwerfen](https://docs.npmjs.com/cli/v7/commands/npm-deprecate)|
|npm diff|[Der Registry-Diff-Befehl](https://docs.npmjs.com/cli/v7/commands/npm-diff)|
|npm dist-tag|[Paketverteilungs-Tags ändern](https://docs.npmjs.com/cli/v7/commands/npm-dist-tag)|
|npm docs|[Dokumente für ein Paket in einem Webbrowser vielleicht](https://docs.npmjs.com/cli/v7/commands/npm-docs)|
|npm doctor|[Überprüfen Sie Ihre Umgebung](https://docs.npmjs.com/cli/v7/commands/npm-doctor)|
|npm edit|[Ein installiertes Paket bearbeiten](https://docs.npmjs.com/cli/v7/commands/npm-edit)|
|npm exec|[Einen Befehl aus einem npm-Paket ausführen](https://docs.npmjs.com/cli/v7/commands/npm-exec)|
|npm explain|[Installierte Pakete erklären](https://docs.npmjs.com/cli/v7/commands/npm-explain)|
|npm explore|[Ein installiertes Paket durchsuchen](https://docs.npmjs.com/cli/v7/commands/npm-explore)|
|npm find-dupes|[Duplikate im Paketbaum finden](https://docs.npmjs.com/cli/v7/commands/npm-find-dupes)|
|npm fund|[Finanzierungsinformationen abrufen](https://docs.npmjs.com/cli/v7/commands/npm-fund)|
|npm help|[Npm-Hilfedokumentation durchsuchen](https://docs.npmjs.com/cli/v7/commands/npm-help)|
|npm help-search|[Hilfe zu npm erhalten](https://docs.npmjs.com/cli/v7/commands/npm-help-search)|
|npm hook|[Registrierungs-Hooks verwalten](https://docs.npmjs.com/cli/v7/commands/npm-hook)|
|npm init|[Paket.json-Datei erstellen](https://docs.npmjs.com/cli/v7/commands/npm-init)|
|npm install|[Paket installieren](https://docs.npmjs.com/cli/v7/commands/npm-install)|
|npm install-ci-test|[Installieren Sie ein Projekt mit einer sauberen Weste und führen Sie Tests durch](https://docs.npmjs.com/cli/v7/commands/npm-install-ci-test)|
|npm install-test|[Paket(e) installieren und Tests ausführen](https://docs.npmjs.com/cli/v7/commands/npm-install-test)|
|npm link|[Symlink a package folder](https://docs.npmjs.com/cli/v7/commands/npm-link)|
|npm logout|[Von der Registrierung abmelden](https://docs.npmjs.com/cli/v7/commands/npm-logout)|
|npm ls|[Installierte Pakete auflisten](https://docs.npmjs.com/cli/v7/commands/npm-ls)|
|npm org|[Organisationen verwalten](https://docs.npmjs.com/cli/v7/commands/npm-org)|
|npm outdated|[Nach veralteten Paketen suchen](https://docs.npmjs.com/cli/v7/commands/npm-outdated)|
|npm owner|[Paketeigentümer verwalten](https://docs.npmjs.com/cli/v7/commands/npm-owner)|
|npm pack|[Einen Tarball aus einem Paket erstellen](https://docs.npmjs.com/cli/v7/commands/npm-pack)|
|npm ping|[Ping der npm-Registrierung](https://docs.npmjs.com/cli/v7/commands/npm-ping)|
|npm pkg|[Verwaltet Ihr Paket.json](https://docs.npmjs.com/cli/v7/commands/npm-pkg)|
|npm prefix|[Präfix anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-prefix)|
|npm profile|[Einstellungen in Ihrem Registrierungsprofil ändern](https://docs.npmjs.com/cli/v7/commands/npm-profile)|
|npm prune|[Überflüssige Pakete entfernen](https://docs.npmjs.com/cli/v7/commands/npm-prune)|
|npm publish|[Paket veröffentlichen](https://docs.npmjs.com/cli/v7/commands/npm-publish)|
|npm rebuild|[Paket neu erstellen](https://docs.npmjs.com/cli/v7/commands/npm-rebuild)|
|npm repo|[Paket-Repository-Seite im Browser öffnen](https://docs.npmjs.com/cli/v7/commands/npm-repo)|
|npm restart|[Paket neu starten](https://docs.npmjs.com/cli/v7/commands/npm-restart)|
|npm root|[npm-Stamm anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-root)|
|npm run-script|[Beliebige Paketskripte ausführen](https://docs.npmjs.com/cli/v7/commands/npm-run-script)|
|npm search|[Nach Paketen suchen](https://docs.npmjs.com/cli/v7/commands/npm-search)|
|npm set-script|[Aufgaben im Skriptabschnitt von package.json festlegen](https://docs.npmjs.com/cli/v7/commands/npm-set-script)|
|npm shrinkwrap|[Abhängigkeitsversionen zur Veröffentlichung sperren](https://docs.npmjs.com/cli/v7/commands/npm-shrinkwrap)|
|npm star|[Markieren Sie Ihre bevorzugten Pakete](https://docs.npmjs.com/cli/v7/commands/npm-star)|
|npm stars|[Als Favoriten markierte Pakete anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-stars)|
|npm start|[Paket starten](https://docs.npmjs.com/cli/v7/commands/npm-start)|
|npm stop|[Paket stoppen](https://docs.npmjs.com/cli/v7/commands/npm-stop)|
|npm team|[Organisationsteams und Teammitgliedschaften verwalten](https://docs.npmjs.com/cli/v7/commands/npm-team)|
|npm test|[Paket testen](https://docs.npmjs.com/cli/v7/commands/npm-test)|
|npm token|[Ihre Authentifizierungstoken verwalten](https://docs.npmjs.com/cli/v7/commands/npm-token)|
|npm uninstall|[Paket entfernen](https://docs.npmjs.com/cli/v7/commands/npm-uninstall)|
|npm unpublish|[Ein Paket aus der Registrierung entfernen](https://docs.npmjs.com/cli/v7/commands/npm-unpublish)|
|npm unstar|[Element aus Ihren bevorzugten Paketen entfernen](https://docs.npmjs.com/cli/v7/commands/npm-unstar)|
|npm update|[Paket aktualisieren](https://docs.npmjs.com/cli/v7/commands/npm-update)|
|npm version|[Bump für eine Paketversion](https://docs.npmjs.com/cli/v7/commands/npm-version)|
|npm view|[Registrierungsinformationen anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-view)|
|npm whoami|[npm-Benutzernamen anzeigen](https://docs.npmjs.com/cli/v7/commands/npm-whoami)|
|npx|[Einen Befehl aus einem npm-Paket ausführen](https://docs.npmjs.com/cli/v7/commands/npx)|



