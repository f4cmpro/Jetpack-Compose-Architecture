# Jetpack Compose Architecture Summary

A concise and practical summary of key topics and patterns for building modern Android apps using Jetpack Compose.

## 🏗️ Recommended Architecture

- Follow **MVVM** (Model-View-ViewModel) with **Unidirectional Data Flow (UDF)**.
- `ViewModel` holds UI state via `StateFlow` or `LiveData`.
- `Composable` observes state and renders UI only. All logic should remain in ViewModel.

## 🧠 Managing `uiState`

- Use a single `uiState` data class that includes multiple fields (`listItems`, `isShowButton`, etc.)
- This centralizes state and makes recomposition predictable.
- For optimization: split derived UI with `derivedStateOf` or separate composables.

## 🔁 Recomposition

- Compose automatically recomposes UI when `uiState` changes.
- It's fine if the entire UI recomposes unless it causes performance issues.
- Use `remember`, `key`, or `derivedStateOf` to minimize unnecessary recomposition.

## 🔄 StateFlow vs LiveData

| Feature             | StateFlow                | LiveData                   |
|---------------------|--------------------------|----------------------------|
| Testability         | ✅ Easy to test           | ❌ Needs lifecycle hacks   |
| Lifecycle awareness | ❌ Manual                 | ✅ Built-in                |
| Compose support     | ✅ Great (`collectAsState`) | ✅ Supported               |

- `getOrAwaitValue()` is used in tests to simulate lifecycle for LiveData.

## 🔍 Search with Debounce

### Using LiveData
- Use `Transformations.switchMap` or coroutine with `viewModelScope` and `delay()`.

### Using StateFlow (preferred)
```kotlin
val query = MutableStateFlow("")
query.debounce(300).collect { search(it) }
```

## 📡 SharedFlow for Navigation Events

- Emit one-time navigation events from `ViewModel`.
- Collect using `LaunchedEffect` in composables to prevent repeated triggers.

## 🧱 AppScaffold vs Scaffold

| Component     | `Scaffold` (Jetpack Compose)         | `AppScaffold` (Custom wrapper)        |
|---------------|--------------------------------------|----------------------------------------|
| Purpose       | Provides top-level UI layout         | Reusable layout logic for consistency |
| Flexibility   | ✅ Highly flexible                   | ⚠️ Depends on abstraction level        |
| Use case      | For standalone screens               | For unified app design                 |

---

Generated with ❤️ by ChatGPT for educational use.