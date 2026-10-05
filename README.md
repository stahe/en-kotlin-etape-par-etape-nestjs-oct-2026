# Step-by-Step Introduction to Mobile Programming with Kotlin

📖 **Read the tutorial: [https://stahe.github.io/en-kotlin-etape-par-etape-nestjs-oct-2026/](https://stahe.github.io/en-kotlin-etape-par-etape-nestjs-oct-2026/)**

This course teaches you how to write a **native Android mobile** app using the [Kotlin](https://kotlinlang.org) 2.2 and [Jetpack Compose](https://developer.android.com/compose), Android’s declarative UI library: an app whose screens are rendered **on the phone** based on JSON data from a server.

It follows the progression of the course [Step-by-Step Introduction to the Flutter Mobile Framework](https://stahe.github.io/en-flutter-etape-par-etape-sept-2026/) (which is itself modeled after the [React](https://stahe.github.io/react-etape-par-etape-sept-2026/), [Vue.js](https://stahe.github.io/vuejs-etape-par-etape-sept-2026/), and [Angular](https://stahe.github.io/angular-etape-par-etape-sept-2026/) courses): same structure, same 25 examples, same server, same case study, written in the Android style.

| Flutter Course | Kotlin Course |
|---|---|
| Dart, widgets, a `build()` method | Kotlin, `@Composable` functions |
| `setState`, `StatefulWidget` | `remember`, `rememberSaveable`, `mutableStateOf` |
| Flutter’s engine renders every pixel | Compose, Android’s own toolkit (Material 3) |
| `initState`, `dispose` | effects: `LaunchedEffect`, `DisposableEffect` |
| `InheritedWidget`, provider | `CompositionLocal`, `ViewModel`, `StateFlow` |
| go_router | Compose Navigation (`@Serializable` routes, deep links) |
| `Future`, `Stream` | coroutines, `Flow` |
| JSON dictionaries, `intl` | `strings.xml`, the app’s language (`setApplicationLocales`) — and the same JSON dictionaries |
| the `http` package | OkHttp, and its `CookieJar` for the token cookie |
| `shared_preferences` | DataStore, `SharedPreferences` |
| `flutter_test` | Compose’s instrumented tests (on the emulator) |

The server, however, remains unchanged: it’s the **RdvMedecins** app’s JSON server already used by the React, Vue.js, Angular, and Flutter clients.

## The Approach: Many Short Examples, Then a Case Study

The course is structured around **25 short examples**, each focused on a single concept. They form a single Android Studio project—one activity per example—accessible from a menu.

| Chapter | Content | Examples |
|---|---|---|
| Getting Started | an Android project (Gradle, manifest, activity), `@Composable` functions, state and recomposition, events (gestures, scrolling, focus), all field types, a reducer (`sealed interface`, `when`), validation, fields that can validate themselves, formatting (`java.time`, `NumberFormat`, extensions) | 01–09 |
| Components | parameters, functions as parameters, composition (placements, generic components), lifecycle (effects), `CompositionLocal`, reusable behaviors and animations, confirmation dialog (suspended function), and `Snackbar` | 10–16 |
| Navigation | Compose Navigation: typed routes, parameters, deep links, tab bar, redirection, guards | 17–18 |
| Asynchronous Operations and Shared State | coroutines, `Flow`, anti-bounce, stale responses (`mapLatest`), `produceState`, `ViewModel`, `StateFlow`, DataStore, dark theme, `SaveableStateHolder` | 19–20 |
| Internationalization | `strings.xml` resources, parameters, plurals, dates, amounts, app language, translated calendar | 21 |
| The Server: A Black Box | setting up the JSON server, its API, 48 `curl` examples, what changes for a mobile client | – |
| Communicating with the Server | OkHttp, the server address (emulator, phone, `adb reverse`), the token cookie on the phone, `kotlinx.serialization`, an API access layer, a `ViewModel`, server errors attached to fields | 22–25 |

Each example is presented with its complete, commented code and screenshots of its execution.

## The Server: A Black Box

The server is the NestJS server from previous lessons, whose controllers return **JSON**. This lesson treats it as a **black box**: we set it up, study its API, and query it with `curl`—but we don’t need to read its code (which is provided and commented for the curious).

- All errors have the same format: `{ "statusCode": 409, "key": "ERRORS.LOGIN_TAKEN", "params": {...}, "fields": {...} }` — **keys** for translation, never plain text;
- Authentication via a JWT token in an `httpOnly` cookie: on the phone, it’s the HTTP client (OkHttp and its `CookieJar`) that stores and sends it back;
- A “test mode” for the CAPTCHA so you can query the API with `curl` (and generate screenshots via automated tests).

## The Case Study: The RdvMedecins Android Client

A complete application for **booking appointments at a doctor’s office**, with **all** files listed and commented.

- **Modern Android**: Kotlin 2.2, Jetpack Compose, and Material 3; a single activity; Compose Navigation; `ViewModel`; coroutines; OkHttp; kotlinx.serialization; a layered architecture (data, state, UI).
- **Three roles**: `ADMIN` (manages doctors and clients), `DOCTOR` (schedules and cancels appointments), `USER` (the patient: books appointments for themselves, manages their account).
- **Privacy**: A patient never receives the names of other patients—the server does not send them.
- **The entire state of a screen is contained within its route**: `Ecrans.Agenda(idMedecin = 1, jour = "2026-10-05")`; the booking window is a “dialog” destination, which the Back button closes.
- **Server-side validation**: Forms display errors returned by the API below each field; optimistic locking, duplicate names, login already taken, etc.
- **Session**: Persists between sessions (the cookie is stored on the device), restored on startup (`GET /api/auth/moi`), expiration managed in a single location (401 response), returns to the requested screen after reconnection.
- **Mobile-friendly**: drawer menu (or `NavigationRail` on large screens and in landscape mode), lists instead of tables, floating button, keyboard that doesn’t hide fields, and functionality that survives rotation and language changes.
- **French / English**, with JSON dictionaries from other clients, including the calendar and Android’s own text strings.
- **Automatic screenshots**: an instrumented test navigates the app like a user.
- **Deployment**: the signed production version (APK, Android App Bundle).

## Repository Contents

```
exemples_kotlin/           the 25 short examples (a single Android Studio project)
rdvmedecins-nestjs-json/   the RdvMedecins JSON server (the “black box”)
rdvmedecins_kotlin/        the Android client for the case study
tools/                    the Windows scripts that generate screenshots
```

Each project can be opened in Android Studio (`File / Open`) or compiled from the command line: `gradlew installDebug`. The apps connect to the server at `http://localhost:8080`, after running `adb reverse tcp:8080 tcp:8080` (or another address: `gradlew installDebug -Pserver=http://10.0.2.2:8080`).

## Technologies

Kotlin 2.2 · Jetpack Compose (BOM 2026.02) · Material 3 · Navigation Compose 2.9 · Lifecycle / ViewModel 2.9 · DataStore 1.1 · AppCompat 1.7 · OkHttp 4.12 · kotlinx.serialization 1.9 · AndroidSVG 1.4 · Compose UI Test · AGP 9.4 · Gradle 9.6 · Android 8.0 (API 26) to Android 17 (API 37) · server-side: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Prerequisites

- Basics of the Kotlin language: the [Kotlin](https://stahe.github.io/kotlin-oct-2026/) course is a good preparation, but the concepts used are explained throughout the examples. Basics of the HTTP protocol.
- Android Studio (which provides the JDK, Gradle, the Android SDK, and the emulator) or an Android phone; Node.js and a MySQL server (e.g., Laragon on Windows) for the JSON server. Installation instructions are provided in the course.

## Author

This course, its examples, and the Android client were written by **Claude**, the AI from [Anthropic](https://www.anthropic.com) (October 2026), at the request of **Serge Tahé**.
