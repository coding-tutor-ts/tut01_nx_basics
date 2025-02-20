<a alt="Coding-Tutor" href="https://coding-tutor.de" target="_blank" rel="noreferrer"><img src="https://avatars.githubusercontent.com/u/199929920" width="245"></a>

# :man_teacher: Teil 2 der Tutorial-Reihe: NestJs/Angular-MonoRepo mit NX :man_teacher:

In Teil 1 haben wir ein NX-Monorepo mit einer Angular-Frontend-App angelegt. In diesem Teil werden wir ein NestJs-Backend hinzufügen und
per Get-Request erreichbar machen. Dafür führen wir folgende Schritte durch:

1. [NestJs-Plugin für NX installieren](#1-nestjs-plugin-für-nx-installieren)
2. [Anlegen eines NestJs-Backends](#2-anlegen-eines-nestjs-backends)
3. [NestJs: Basics](#3-nestjs-basics)
4. [Testen der vordefinierten Route](#4-testen-der-vordefinierten-route)

**Fangen wir an ... :rocket:**

## Voraussetzungen

Um dieses Tutorial durcharbeiten zu können, ist es ratsam, Teil 1 abgeschlossen zu haben. Ihr könnt aber auch direkt hier einsteigen, wenn ihr bereits ein NX-Monorepo mit einer Angular-Frontend-App angelegt habt.

## 1. NestJs-Plugin für NX installieren

Um in NX mit NestJs arbeiten zu können, müssen wir zunächst ein NestJs-Plugin für NX installieren. Dafür geben wir im Terminal folgenden Befehl ein:

```shell
npm install --save-dev @nx/nest
```

## 2. Anlegen eines NestJs-Backends

Nachdem wir das Plugin installiert haben, können wir ein NestJs-Backend anlegen. Dafür geben wir im Terminal folgenden Befehl ein:

```shell
nx generate @nx/nest:app --directory=apps/backend --strict=true --frontendProject=frontend
```

Dieser Befehl legt ein NestJs-Backend mit dem Namen "backend" an. Der Parameter `--strict=true` sorgt dafür, dass das Backend im Strict-Mode läuft, dieser Modus sollte generell genutzt werden, da er die Code-Qualität erhöht.
Der Parameter `--frontendProject=frontend` verbindet das Backend mit dem Frontend-Projekt "frontend", in dem es im Angular-Frontend eine Proxy-Config anlegt. Das Argument `--directory=apps/backend` legt das Backend im Ordner "apps/backend" an.

Beim Ausführen des Befehls wird der Benutzer bezüglich der weiteren Konfiguration des Backends befragt, die wir für dieses Tutorial wie folgt beantworten:

| Frage                                         | Antwort | Erklärung                                                              |
|-----------------------------------------------|---------|------------------------------------------------------------------------|
| Which linter would you like to use?           | eslint  | Für das Linting möchten wir, wie im Frontend, ESLint verwenden         |
| Which unit test runner would you like to use? | jest    | Das Unit-Testing soll im späteren Verlauf mit Jest durchgeführt werden |

Mit dem Argument --dry-run können wir uns das Ergebnis des Befehls anzeigen lassen, ohne dass die Dateien tatsächlich angelegt werden.
Auf diese Weise stellt ihr sicher, das alles korrekt konfiguriert ist und die Namen und Pfade so aussehen wie ihr es erwartet.

**Und damit haben wir erfolgreich ein NestJs-Backend zu unserem Nx-Monorepo hinzugefügt.**

### :call_me_hand: Herzlichen Glückwunsch :call_me_hand:

## 3. NestJs: Basics

Schauen wir uns zunächst die Struktur des neu angelegten NestJs-Backends an.

### Root-Verzeichnis des Backend

Im Root-Verzeichnis des Backends finden wir folgende Dateien und Ordner:

**eslint.config.mjs**  
Die ESLint-Konfiguration für das Backend (leitet von der eslint-config im Projekt-Root ab). Hier kannst du spezielle EsLint-Regeln für das Backend definieren.  
Eine Anleitung zur ESLint-Config findest du [hier](https://eslint.org/docs/latest/use/configure/).

**jest.config.js**  
Die Jest-Konfiguration für das Backend (leitet von der Jest-Konfiguration im Projekt-Root ab). Du erinnerst dich vielleicht, dass wir bei der Erstellung des Backends angegeben haben, dass wir das Unit-Testing mit Jest durchführen wollen.
Hier kannst du spezielle Jest-Regeln für das Backend definieren.  
Eine Anleitung zur Jest-Config findest du [hier](https://jestjs.io/docs/configuration).

**tsconfig.app.json**  
Die TypeScript-Konfiguration für das Backend (leitet von der tsconfig.json ab). Hier kannst du spezielle TypeScript-Regeln für das Backend definieren.

**tsconfig.json**  
Die Haupt-TypeScript-Konfiguration für das Backend (leitet von der tsconfig.base.json im Projekt-Root ab). Hier kannst du allgemeine TypeScript-Regeln für das Backend definieren.

**tsconfig.spec.json**  
Die Typescript-Konfiguration für die Tests des Backends (leitet von der tsconfig.spec.json im Projekt-Root ab). Hier kannst du spezielle TypeScript-Regeln für die Tests des Backends definieren.

Eine Anleitung zur TypeScript-Config findest du [hier](https://www.typescriptlang.org/tsconfig).

**webpack.config.js**  
Die Webpack-Konfiguration für das Backend. Hier kannst du Webpack-Regeln für das Backend definieren und damit beeinflussen, wie die Backend-Anwendung gebaut wird.  
Eine Anleitung zur Webpack-Config findest du [hier](https://webpack.js.org/guides/getting-started/).

### src

In diesem Verzeichnis liegt ähnlich wie bei Angular die Sourcen des Backends. Hier finden wir die Datei `main.ts` sowie initial die Ordner `app` (in dem sich die eigentliche Anwendung, also dein Code, befindet) und `assets`
(hier liegen Dateien, die nicht kompiliert werden, sondern eins zu eins in den Build der Anwendung übernommen werden).  
Im Weiteren Verlauf dieser Tutorial-Reihe werden wir hier noch das Verzeichnis `libs` finden, dass unsere Anwendungs-Libraries (z.B. für Features) beinhaltet.

**main.ts**  
Diese Datei ist der Einstiegspunkt in die Backend-Anwendung. Hier wird die Anwendung initialisiert und gestartet. Schauen wir uns diese mal im Detail an

```typescript
import { Logger } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app/app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule); // Erstellt unsere NestJs-App, die wir in app.module.ts definiert haben
  const globalPrefix = 'api'; // Definiert den globalen Prefix für alle Routen, bedeutet in diesem Fall: http://localhost:3000/api
  app.setGlobalPrefix(globalPrefix);
  const port = process.env.PORT || 3000; // Definiert den Port, auf dem die Anwendung laufen soll
  await app.listen(port); // Startet die Anwendung auf dem definierten Port
  Logger.log(`🚀 Application is running on: http://localhost:${port}/${globalPrefix}`); // Loggt die URL, unter der die Anwendung erreichbar ist
}

bootstrap();
```

### app

In diesem Verzeichnis verstauen wir sämtlichen Code, der unsere Backend-Anwendung ausmacht.

**app.module.ts**  
NestJs ist ähnlich wie Angular (vor v17) modulbasiert. Das bedeutet, dass wir unsere Anwendung in Module aufteilen, die jeweils bestimmte Funktionalitäten kapseln.
In `app.module.ts` definieren wir das Hauptmodul unserer Anwendung. Hier importieren wir alle Module, die unsere Anwendung benötigt und registrieren sie in der `imports`-Property des Moduls.

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [], // Hier importieren wir alle Module, die unsere Anwendung benötigt
  controllers: [AppController], // Hier können Controller registiert werden
  providers: [AppService], // Hier werden alle Klasse für die Dependency Injection registriert. Diese Klasse sind mit Injectable() dekoriert
})
export class AppModule {
}

```

**app.service.ts**
In diesem Service definieren wir die Logik unserer Anwendung. Ein Service ist eine Klasse, die mit dem `@Injectable`-Decorator dekoriert ist und die Business-Logik unserer Anwendung enthält.
Alle Klasse, die mit @Injectable() dekoriert sind, werden in NestJs als Singleton-Instanz behandelt und werden durch die Dependency-Injection in andere Klassen injiziert. Damit die Dependency Injection
diesen Service kennt, muss er in der `providers`-Property des Moduls registriert werden.  
In diesem beispielhaften Service wird eine getData() Methode definiert, die eine Objekt

```typescript
{
  message: 'Hello API'
}
```

zurückgibt.

**app.controller.ts**  
In diesem Controller definieren wir die Routen unserer Anwendung. Ein Controller ist eine Klasse, die mit dem `@Controller`-Decorator dekoriert ist und Routen definiert, die auf HTTP-Requests reagieren.
In unserem Beispiel-Controller zum einen im Constructor uns AppService per Dependency Injection injiziert und zum anderen wird eine Route definiert, die auf einen **Get-Request** auf die Route `/api` reagiert.
In dieser Methode wird die getData()-Methode des AppService aufgerufen und das Ergebnis zurückgegeben.

***.spec.ts**
In diesen Dateien sind die Unit-Tests zu den jeweiligen Dateien enthalten. Diese Dateien werden von Jest ausgeführt, wenn wir die Tests starten.

### Starten der Backend-Anwendung

Um die Backend-Anwendung zu starten, geben wir im Terminal folgenden Befehl ein:

```shell
nx serve backend
```

Danach läuft unsere Anwendung erfolgreich unter http://localhost:3000. Jetzt können wir per Get-Request (z.B. mit Postman) auf die Anwendung zugreifen.
Eine Get-Abfrage auf http://localhost:3000/api sollte eine Antwort mit Status 200 und dem Inhalt

```json
{
  "message": "Hello API"
}
```

zurückgeben.

Neben dem manuellen Test per Postman (oder ähnlichem Tool). Kann die Route auch per Unit-Test mithilfe von Jest getestet werden

## 4. Testen der vordefinierten Route

Um die vordefinierte Route zu testen, betrachten wir uns die ebenfalls initial generierte Test-Datei `app.controller.spec.ts` im Ordner `src/app`.

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { AppController } from './app.controller';
import { AppService } from './app.service';

describe('AppController', () => {
  let app: TestingModule;   // Initialisierung des TestingModules

  beforeAll(async () => { // wird jeweils vor der Ausführung der Tests ausgeführt
    app = await Test.createTestingModule({ // erzeugt unsere Test-Instanz mit unserem Controller und Service
      controllers: [AppController],
      providers: [AppService],
    }).compile();
  });

  describe('getData', () => { // Implementierung des Tests für die getData-Methode des AppControllers
    it('should return "Hello API"', () => {
      const appController = app.get<AppController>(AppController); // Instanzierung des AppControllers
      expect(appController.getData()).toEqual({ message: 'Hello API' }); // Erwartet, dass die getData-Methode des AppControllers das Objekt { message: 'Hello API' } zurückgibt
    });
  });
});

```

führen wir einen Unit-Test aus. Dafür geben wir im Terminal folgenden Befehl ein:

```shell
nx test backend
```

Solltet ihr bei diesem Aufruf eine Fehlermeldung erhalten, dass das Target backend:test nicht bekannt ist, dann müsst ihr eure `apps/backend/project.json` auf folgenden Eintrag prüfen:

```json
{
  "targets": {
    ...
    "test": {
      "executor": "@nx/jest:jest",
      "options": {
        "jestConfig": "apps/backend/jest.config.ts",
        "passWithNoTests": true
      }
    },
    ...
  }
}
```

:tada: Wir haben erfolgreich unsere NestJs-App unseres Monorepos gestartet und erfolgreich eine erste Route eingerichtet :tada:

Wir beenden damit dieses Tutorial in dem wir unseren Fortschritt nach Github pushen. Ihr findet den Code sowie diese Anleitung hier bei <a href="https://github.com/coding-tutor-ts/tut01_nx_basics/blob/02_add_nestjs_backend_dev/README.md">![GitHub](https://img.shields.io/badge/GitHub-100000?logo=github&logoColor=white)
</a>

Für die weitere Einrichtung der Angular-App folge uns und schau dir Teil 3 unserer Reihe "Anlage eines Angular/NestJs" Monorepos an.

Ihr könnt uns auch gerne auf <a href="https://www.tiktok.com/@codingtutorts">![<a href="https://www.tiktok.com/@codingtutorts">TikTok</a>](https://img.shields.io/badge/TikTok-000000?logo=tiktok&logoColor=white)</a> oder <a href="https://www.youtube.com/channel/UCVTzm0UalCzkoDvjNpCaQlw">![YouTube-Kanal](https://img.shields.io/badge/YouTube-FF0000?logo=youtube&logoColor=white)</a> folgen.



