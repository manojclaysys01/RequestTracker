# 📡 Request Tracker — Android App

A full Android app to monitor, log, filter, and export HTTP API requests and App-to-App (IPC) communications from your mobile device.

---

## 🏗 Architecture

| Layer | Technology |
|-------|-----------|
| UI | Fragments + ViewBinding + Navigation Component |
| ViewModel | Hilt + Kotlin Coroutines + StateFlow |
| Database | Room (SQLite) |
| HTTP Tracking | OkHttp Interceptor |
| IPC Tracking | AppToAppTracker utility |
| DI | Hilt |
| Export | JSON / CSV via FileProvider |

---

## 📂 Project Structure

```
app/src/main/java/com/requesttracker/
├── data/
│   ├── db/              # Room database, DAO, TypeConverters
│   ├── model/           # RequestLog entity
│   └── repository/      # RequestLogRepository
├── di/                  # Hilt modules
├── service/
│   ├── RequestTrackingInterceptor.kt   # OkHttp interceptor
│   ├── AppToAppTracker.kt              # App-to-App tracker
│   └── RequestMonitorService.kt        # Foreground service
├── ui/
│   ├── MainActivity.kt
│   ├── dashboard/       # Stats overview screen
│   ├── logs/            # Full log list with filters
│   └── detail/          # Request detail view
└── util/
    └── ExportUtil.kt    # CSV + JSON export
```

---

## 🚀 Getting Started

### 1. Open in Android Studio

1. Open **Android Studio** (Hedgehog or newer recommended)
2. `File → Open → Select the RequestTracker folder`
3. Wait for Gradle sync
4. Run on device/emulator (`Shift+F10`)

### 2. Track HTTP Requests

Add the interceptor to your OkHttpClient:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    @Singleton
    fun provideOkHttpClient(
        trackingInterceptor: RequestTrackingInterceptor
    ): OkHttpClient = OkHttpClient.Builder()
        .addInterceptor(trackingInterceptor)  // ← Add this
        .build()
}
```

Every network call made through this client is automatically logged.

### 3. Track App-to-App Requests

Inject `AppToAppTracker` and call it around your IPC:

```kotlin
@AndroidEntryPoint
class MyActivity : AppCompatActivity() {

    @Inject lateinit var tracker: AppToAppTracker

    fun sendToAnotherApp() {
        val intent = Intent("com.other.app.ACTION_DO_THING").apply {
            `package` = "com.other.app"
            putExtra("key", "value")
        }

        // Track the outgoing request
        tracker.trackBroadcast(this, intent, direction = "OUTGOING")

        startActivity(intent)
    }
}
```

---

## 📱 App Screens

### Dashboard
- Total requests, success count, error count, average duration
- Error rate progress bar
- Last 5 requests preview

### Logs
- Full paginated list with color-coded status indicators
- Filter chips: All / HTTP / App-to-App / Success / Error
- Search by URL, request body, response body
- Swipe to refresh

### Request Detail
- Full request & response headers
- Pretty-printed JSON body
- Source/target app for IPC calls
- Copy request or response to clipboard

---

## 📤 Export

Tap the **⋮ menu → Export CSV** or **Export JSON** to share logs via:
- Email, Slack, Drive, Airdrop, etc.
- CSV is formatted for spreadsheets
- JSON preserves all fields including headers & bodies

---

## 🔔 Foreground Service

The `RequestMonitorService` runs as a foreground service with a persistent notification, ensuring tracking continues while the app is in the background. Tap "Stop" in the notification to pause.

---

## 🔒 Permissions

| Permission | Purpose |
|-----------|---------|
| `INTERNET` | Required to make HTTP requests |
| `FOREGROUND_SERVICE` | Background monitoring |
| `POST_NOTIFICATIONS` | Show monitoring notification |
| `WRITE_EXTERNAL_STORAGE` | Export on older Android (<9) |

---

## 🗄 Database Schema

```sql
CREATE TABLE request_logs (
    id              INTEGER PRIMARY KEY AUTOINCREMENT,
    type            TEXT NOT NULL,      -- HTTP | APP_TO_APP
    method          TEXT NOT NULL,      -- GET, POST, SEND, RECEIVE...
    url             TEXT NOT NULL,
    requestHeaders  TEXT NOT NULL,      -- JSON
    requestBody     TEXT,
    responseCode    INTEGER,
    responseHeaders TEXT,               -- JSON
    responseBody    TEXT,
    status          TEXT NOT NULL,      -- SUCCESS | ERROR | PENDING
    durationMs      INTEGER,
    sourceApp       TEXT,
    targetApp       TEXT,
    timestamp       INTEGER NOT NULL,
    errorMessage    TEXT
);
```

---

## 🧩 Extending the App

- **Add Retrofit tracking**: Pair with the OkHttp interceptor (already compatible)
- **WebSocket tracking**: Add a custom WebSocket listener that calls `repository.insert()`
- **Real-time dashboard**: Replace the RecyclerView with a LiveChart using the `avgDuration` flow
- **Alerts**: Add threshold-based notifications for high error rates

---

## 📋 Requirements

- Android 7.0+ (API 24+)
- Android Studio Hedgehog (2023.1.1) or newer
- JDK 17
