# Lumora Addon Template

Vorlage für eigene Addons für den **Lumora Client**. Sie enthält die öffentliche **Lumora API 1.7.0** als Quellcode und
ein fertiges Beispiel-Addon (Session Timer: ein HUD-Widget mit der Zeit in der aktuellen Welt, ein Keybind, eine
Einstellung und eine Benachrichtigung).

Die API hat keine Abhängigkeiten und keine Minecraft-Klassen. Ein Addon, das nur die API nutzt, läuft deshalb mit
**derselben JAR in allen Lumora-Versionen** (1.8.9 bis 26.3). Lumora lädt Addons mit seinem eigenen Addon-Loader aus
dem Ordner `addons` – ein Mod-Loader wird nicht gebraucht.

## Was ein Addon erweitern kann

| Bereich | API |
|---|---|
| HUD | eigene Widgets mit Einstellungen, Platz an Ankerzonen wie im HUD-Editor |
| Design | Akzentfarbe und Theme des Spielers, fertige Lumora-Bausteine (`UiKit`), eigene Akzentfarben im Hub |
| Spielermenü | eigene Einträge im Ring (Mausrad auf einen Spieler), mit Kategorien und Bedingungen |
| Chat | eigene Einträge im Menü von Chat-Nachrichten, Event bei neuen Nachrichten |
| Social | Freunde und Party von Lumora Social lesen |
| Menüs | eigene Vollbild-Menüs, Knopf im Lumora-Hub |
| Sonst | Keybinds, Benachrichtigungen, Einstellungen, Spieler und Welt, Plugin-Kanäle, Namensschild-Symbole, Timer |

## Loslegen

1. Oben auf **Use this template → Create a new repository** klicken.
2. Das neue Repository klonen und bauen:

   ```bash
   ./gradlew build
   ```

   Unter Windows `gradlew.bat build`. Benötigt wird ein JDK 21 oder neuer.
3. Die JAR aus `addon/build/libs/` in den Ordner `addons` der Lumora-Instanz kopieren
   (`%APPDATA%\LumoraClient\instances\<version>\addons`, im Spiel: Lumora-Hub → **Addons** → **Addon-Ordner**) und
   den Client starten. Das Addon erscheint im Lumora-Hub unter **Addons**, sein Widget im **HUD-Editor**.

## Eigenes Addon daraus machen

- `addon/src/main/resources/lumora-addon.json`: `id`, `name`, `description`, `authors`, `entrypoint` und die
  benötigten `permissions` anpassen; `api` auf die niedrigste API-Version setzen, deren Dienste du nutzt.
- Paket `dev.lumora.example.timer` umbenennen, `addon.group` in `gradle.properties` und den JAR-Namen in
  `addon/build.gradle.kts` setzen.
- Übersetzungen liegen unter `addon/src/main/resources/assets/<addon-id>/lang/`, Texturen, Schriften und Klänge
  ebenfalls unter `assets/<addon-id>/`.

## Aufbau

| Ordner | Inhalt |
|---|---|
| `lumora-api/` | Öffentliche Lumora API 1.7.0 |
| `addon/` | Beispiel-Addon, nutzt die API nur zur Compile-Zeit (`compileOnly`), zur Laufzeit liefert der Client sie |
| `docs/` | Anleitungen und API-Referenz |
| `.github/workflows/` | Baut das Projekt bei jedem Push und lädt die Addon-JAR als Artefakt hoch |

## Dokumentation

- [Ein Addon erstellen](docs/CREATE_ADDON.md) – mit Beispielen für Design, HUD-Anker, Spielermenü, Chat und Social
- [Ein HUD-Widget erstellen](docs/CREATE_WIDGET.md)
- [HUD-Editor](docs/HUD_EDITOR.md)
- [API-Referenz](docs/API_REFERENCE.md)

## API in anderen Projekten nutzen

Die API lässt sich auch aus dem lokalen Maven-Repository beziehen:

```bash
./gradlew :lumora-api:publishToMavenLocal
```

```groovy
repositories { mavenLocal { content { includeGroup 'dev.lumora' } } }
dependencies { compileOnly 'dev.lumora:lumora-api:1.7.0' }
```

Die API nicht in die eigene JAR einbetten: der Lumora-Client bringt sie mit.

## Lizenz

GNU General Public License v3.0, siehe [LICENSE](LICENSE).
