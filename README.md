<a alt="Coding-Tutor" href="https://coding-tutor.de" target="_blank" rel="noreferrer"><img src="https://avatars.githubusercontent.com/u/199929920" width="245"></a>

# :man_teacher: Teil 1 der Tutorial-Reihe: NestJs/Angular-MonoRepo mit NX :man_teacher:

Anders als bei NestJs erfreut sich Angular bereits einer sehr breiten Community und sollte den meisten Interessenten dieses Tutorials bereits bekannt sein.
Daher starten wir unser kleines Beispiel-Projekt mit einem NX-Monorepo, in das wir direkt zu Beginn eine Angular-App integrieren

**Fangen wir an ... :rocket:**

## Voraussetzungen

Um euch generell in der Typescript-Welt bewegen zu können, müsst ihr zunächst erstmal <a href="https://nodejs.org/en/download">Node.js</a> installieren. Solltet ihr das bereits erledigt haben, müsst ihr sicherstellen, dass ihr mindestens die Version 20+ installiert habt.
Das könnt ihr mit

```
node -v
``` 

in einer Console eure Wahl abfragen.

**Wir werden für die Tutorialstrecke folgende Versionen verwenden:**

:heavy_check_mark: NodeJs v22.0.0  
:heavy_check_mark: NX v20  
:heavy_check_mark: NestJs v11  
:heavy_check_mark: Angular v19

## Neuen NX-Workspace mit Angular-Frontend anlegen

1. Öffnen einer cmd als Administrator
2. Ausführen des Befehls

```
npx create-nx-workspace@latest tut_01
```

(tut_01 kann durch einen beliebigen Namen ersetzt werden)

3. Beim Ausführen des Befehl werden einige Fragen gestellt, die wir für dieses Tutorial wie folgt beantworten:

| Frage                                                                                            | Antwort    | Erklärung                                                                                                                                                         |
|--------------------------------------------------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Which stack do you want to use?                                                                  | angular    | wir wollen direkt mit einer Angular App starten                                                                                                                   |
| Integrated monorepo, or standalone project?                                                      | integrated | es soll ein integrated Monorepo werden, da wir ja auch noch ein NestJs-Backend im weiteren Verlauf hinzufügen möchten                                             |
| Application name                                                                                 | frontend   | Name des Frontends in unserem Fall sehr einfallslos "Frontend"                                                                                                    |
| Which bundler would you like to use?                                                             | esbuild    | Entscheidet darüber welcher Bundler für die Angular-App verwendet werden soll. Da dies nicht scope dieses Tutorials ist, verwenden wir hier unkommentiert esbuild |
| Default stylesheet format                                                                        | scss       | ebenfalls nicht im Scope dieses Tutorials, wir verwenden scss                                                                                                     |
| Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? | No         | ebenfalls nicht im Scope dieses Tutorials, wir verwenden kein SSR                                                                                                 |
| Test runner to use for end to end (E2E) tests                                                    | cypress    | ebenfalls nicht im Scope dieses Tutorials, wir verwenden cypress                                                                                                  |
| Which CI provider would you like to use?                                                         | none       | ebenfalls nicht im Scope dieses Tutorials, wir verwenden none                                                                                                     |

Und damit haben wir ein standardmäßig eingerichtetes NX-Monorepo mit einer Angular-Frontend-App angelegt.

### :call_me_hand: Herzlichen Glückwunsch :call_me_hand:

Damit wir uns im Editor an einheitlich CodingStyle halten, erweitern wir die initial mitgelieferte .prettierrc um folgende Einträge

```json
{
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 180,
  "tabWidth": 4,
  "useTabs": true
}
```

| Eigenschaft		   | Wert		 | Erklärung                                                                                                                                                                                              |
|-----------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| "trailingComma" | "all"  | stellt sicher, das wir an jeder Zeile ein Array, etc. ein Komma am Ende haben                                                                                                                          |
| "printWidth"    | 180		  | erhöht die Zeilenlänge auf 180 Zeichen, dieser Wert ist etwas höher als normal, was aber auf einem 4K Monitor angenehmer zu lesen ist. Der Wert kann auch weggelassen werden, um den Stadard zu nutzen |
| "tabWidth"      | 4			   | die Tab-Size wird auf 4 Zeichen gesetzen, was den Code wesentlich einfacher zu lesen macht                                                                                                             |
| "useTabs"       | true		 | für die Einrückung werden Tabs verwendet anstatts Leerzeichen                                                                                                                                          |

Nach diesen Einstellungen werdet ihr (zumindest bei Webstorm) gefragt, ob ihr diese Einstellungen für das Projekt übernehmen wollt. Das solltet ihr mit "yes" bestätigen.   
Anschließend sollte ein "Reformat Code" die Änderungen anwenden.

### Erster Angular-Anwendungs-Start

Nachdem wir die erste Einrichtung abgeschlossen haben, testen wir jetzt noch, ob sich die Frontend-Anwendung starten lässt. Dafür benutzen wir das nx serve command

Wir öffnen ein Terminal und geben im Root-Verzeichnis

```shell
nx serve frontend -o
``` 

ein. Auf diese Weise wird ein built-in Webserver gestartet, die Frontend-App gebaut und, aufgrund von "-o", in eurem Standard Browser geöffnet.

:tada: Wir haben erfolgreich unsere erste App unseres Monorepos gestartet :tada:

Wir beenden damit dieses Tutorial in dem wir unseren ersten kleinen Erfolg nach Github pushen. Ihr findet den Code sowie diese Anleitung hier bei <a href="https://github.com/coding-tutor-ts/tut01_nx_basics/blob/01_create_nx_monorepo_with_angular_app/README.md">![GitHub](https://img.shields.io/badge/GitHub-100000?logo=github&logoColor=white)
</a>

Für die weitere Einrichtung unseres Monorepos, schau dir <a href="https://github.com/coding-tutor-ts/tut01_nx_basics/blob/02_add_nestjs_backend/README.md">Teil 2 unserer Reihe "Anlage eines Angular/NestJs Monorepos</a> an.

Ihr könnt uns auch gerne auf <a href="https://www.tiktok.com/@codingtutorts">![<a href="https://www.tiktok.com/@codingtutorts">TikTok</a>](https://img.shields.io/badge/TikTok-000000?logo=tiktok&logoColor=white)</a> oder <a href="https://www.youtube.com/channel/UCVTzm0UalCzkoDvjNpCaQlw">![YouTube-Kanal](https://img.shields.io/badge/YouTube-FF0000?logo=youtube&logoColor=white)</a> folgen.



