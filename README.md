# BAISM-CW-2

Aplikacja Android (Kotlin, Jetpack Compose) użyta jako źródło kodu w pipeline CI/CD na Bitrise
w ramach ćwiczenia 2 z przedmiotu Bezpieczeństwo aplikacji i systemów mobilnych.

Bazą jest przykładowa aplikacja Google **Tip Time** (kalkulator napiwków) z kursu
[Android Basics with Compose](https://github.com/google-developer-training/basic-android-kotlin-compose-training-tip-calculator),
licencja Apache 2.0 (plik `LICENSE`).

## Zmiany względem oryginału

- `minSdk` podniesione z 24 do 29, `allowBackup=false`, `uses-sdk` w manifeście (wymagania mobsfscan).
- Drugi test jednostkowy `calculateTip_15PercentRoundup`.
- `bitrise.yml` z workflow `safe_build` i plik `.mobsf`.

## Testy

- Jednostkowe: `app/src/test/java/com/example/tiptime/TipCalculatorTests.kt`
- Instrumentalne (UI): `app/src/androidTest/java/com/example/tiptime/TipUITests.kt`

```
./gradlew :app:testDebugUnitTest
```

## Workflow `safe_build`

1. `git-clone` – klonowanie repozytorium
2. testy jednostkowe (`./gradlew :app:testDebugUnitTest`) – błąd przerywa build
3. `mobsfscan . --type android --exit-warning` – błąd lub ostrzeżenie przerywa build
4. `android-build` – APK w wariancie debug
5. `deploy-to-bitrise-io` – artefakt na bitrise.io

Trigger: pull request do gałęzi `main`.
