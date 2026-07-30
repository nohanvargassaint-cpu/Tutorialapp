# Tres en Raya — Android (Kotlin + Jetpack Compose)

App nativa de Tres en Raya (Tic-Tac-Toe) con arquitectura **MVVM**, 100% en
**Jetpack Compose** (sin XML de layouts), modo **2 Jugadores** y modo
**Jugador vs Bot** con una IA de dificultad media.

## Estructura del proyecto

```
TresEnRaya/
├── build.gradle.kts                 # Configuración raíz (plugins)
├── settings.gradle.kts              # Módulos incluidos
├── gradle.properties
├── app/
│   ├── build.gradle.kts             # Dependencias del módulo app
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── res/values/              # strings.xml, themes.xml
│       └── java/com/tresenraya/game/
│           ├── MainActivity.kt
│           ├── model/
│           │   ├── Player.kt        # Enum X / O / NONE
│           │   ├── GameMode.kt      # PLAYER_VS_PLAYER / PLAYER_VS_BOT
│           │   ├── GameState.kt     # Estado inmutable de la partida
│           │   ├── WinLines.kt      # Las 8 combinaciones ganadoras
│           │   └── BotEngine.kt     # Lógica de la IA (nivel medio)
│           ├── viewmodel/
│           │   └── GameViewModel.kt # Toda la lógica de turnos/puntaje
│           └── ui/
│               ├── theme/           # Color.kt, Type.kt, Theme.kt
│               ├── components/      # GameBoard, ScoreBoard, ModeSelector
│               └── screens/         # GameScreen.kt
```

## Cómo abrir y ejecutar

1. Abre **Android Studio** (Koala o superior recomendado).
2. `File > Open...` y selecciona la carpeta `TresEnRaya/`.
3. Deja que Gradle sincronice (usa AGP 8.5.1 / Kotlin 1.9.24 / compileSdk 34).
4. Ejecuta sobre un emulador o dispositivo físico con Android 7.0 (API 24) o superior.

No hace falta ninguna clave ni configuración adicional: el proyecto no tiene
dependencias externas de red ni servicios de terceros.

## Lógica destacada

- **`GameViewModel`** es la única fuente de verdad del estado (`StateFlow<GameState>`),
  siguiendo flujo de datos unidireccional: la UI solo lee estado y emite eventos
  (`onCellClicked`, `resetRound`, `resetFullScore`, `setGameMode`).
- **`BotEngine`** decide la jugada del bot: gana si puede, bloquea si el rival
  puede ganar, si no prioriza el centro, luego esquinas, luego cualquier casilla libre.
- El bot "piensa" con un `delay(600)` dentro de una corrutina lanzada en
  `viewModelScope`, para que la jugada no se sienta instantánea.
- La línea ganadora se resalta comparando los índices contra las 8 combinaciones
  de `WinLines.ALL` (filas, columnas y diagonales).

## Publicar este proyecto en GitHub

Desde la carpeta `TresEnRaya/`:

```bash
git init
git add .
git commit -m "Tres en Raya: app Android en Kotlin + Jetpack Compose (MVVM)"
git branch -M main
git remote add origin https://github.com/<tu-usuario>/tres-en-raya-android.git
git push -u origin main
```

Sugerencias:
- Crea el repositorio vacío primero en GitHub (sin README, para evitar conflictos al hacer push).
- El `.gitignore` incluido ya excluye `/build`, `.gradle/`, `local.properties`, APKs, etc.
- Si quieres CI, puedes añadir un workflow de GitHub Actions que ejecute
  `./gradlew assembleDebug` en cada push (puedo generarlo si lo necesitas).

## Posibles mejoras futuras

- IA con algoritmo Minimax completo (modo "Difícil", imbatible).
- Animación de confeti o vibración al ganar.
- Persistencia del marcador entre sesiones (DataStore).
- Sonidos al colocar ficha.
