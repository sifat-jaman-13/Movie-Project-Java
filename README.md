# 🎬 ScarZero Movie Store — Java Edition

> A desktop movie store with separate customer and administrator interfaces, built in Java 17 with JavaFX 21 over an in-memory data layer.

[![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?logo=openjdk&logoColor=white)](https://openjdk.org/)
[![JavaFX](https://img.shields.io/badge/JavaFX-21-1F8ACB?logo=java&logoColor=white)](https://openjfx.io/)
[![Maven](https://img.shields.io/badge/Maven-3.6%2B-C71A36?logo=apachemaven&logoColor=white)](https://maven.apache.org/)
[![Course](https://img.shields.io/badge/Course-CSE215-yellow)](https://ece.northsouth.edu/)
[![NSU](https://img.shields.io/badge/NSU-North%20South%20University-1E90FF)](https://www.northsouth.edu/)

[![Platform](https://img.shields.io/badge/platform-windows%20%7C%20linux%20%7C%20macos-brightgreen)](#-quick-start)
[![License](https://img.shields.io/badge/license-custom--non--commercial-orange)](#-license)
[![Repository](https://img.shields.io/badge/GitHub-Movie--Project--Java-333333?logo=github)](https://github.com/sifat-jaman-13/Movie-Project-Java)

---

## 📚 Project Information

| Field | Value |
|-------|-------|
| **Project** | ScarZero Movie Store — Java Edition |
| **Author** | Sifat Jaman |
| **Course** | CSE215 — Programming Language II |
| **Institution** | North South University |
| **Type** | Solo project |
| **Language** | Java 17 |
| **GUI Framework** | JavaFX 21 |
| **Build Tool** | Maven (shade plugin → `MovieStore.jar`) |
| **Size** | 14 Java files, 1,487 lines of code |
| **Repository** | [sifat-jaman-13/Movie-Project-Java](https://github.com/sifat-jaman-13/Movie-Project-Java) |

---

## 🎯 Overview

ScarZero Movie Store is a single JavaFX application with two entry paths: a **customer view** where users register, browse a catalogue of 40 films, add titles to a cart, check out, and keep a library of purchases; and an **admin console** reached through a separate login, where an administrator adds films and reviews registered accounts.

All state is held in memory by a singleton `DataManager`. The catalogue is seeded in code at startup; anything created during a session — accounts, carts, purchases — exists only until the process exits. See [Data Persistence](#-data-persistence) for what this means in practice.

The project is a companion to [GameStore-Python](https://github.com/sifat-jaman-13/GameStore-Python): the same storefront idea, rebuilt to practise Java classes, inheritance, encapsulation, and the singleton pattern.

> **For students:** Treat this repository as a learning reference and a starting point for ideas, not as a project to copy and submit unchanged. Try implementing your own movie store, redesign its workflows, explore different JavaFX layouts, or extend it with features of your own. Make sure you understand the classes and patterns you use, and let your own problem-solving shape the final result.

---

## ✨ Features

### Customer flow

| View | Capability |
|------|-----------|
| **Login** | Sign in with a username and password created during registration |
| **Register** | Create an account; checks for empty fields, password confirmation mismatch, and duplicate usernames |
| **Dashboard** | Browse all 40 films in a list, select one to read its details, add it to the cart |
| **Cart** | Review selected films with a running total, remove items, or check out |
| **Library** | View films already purchased |

Checkout debits the wallet through `User.deductFromWallet(...)` and moves each film into the buyer's library. New accounts open with a $50.00 credit, so a fresh user can buy three or four films; once that is spent, checkout is refused with an "Insufficient Funds" alert. See [Known Limitations](#-known-limitations).

### Admin console

| Tab | Capability |
|-----|-----------|
| **Manage Movies** | Add a film by entering its title, genre, director, price, and release year; the app generates its ID and assigns a default description and stock value of 50 |
| **Manage Users** | View registered accounts and a total-users count |

Admin login allows a maximum of 5 attempts.

### Implemented in the data layer, not yet in the UI

`DataManager` carries complete, working logic for three further features that **no screen currently reaches**:

- **Wallet top-up requests** — `addWalletRequest`, `getPendingWalletRequests`, `approveWalletRequest`, `rejectWalletRequest`
- **Profile change requests** — `addProfileRequest`, `getPendingProfileRequests`, `approveProfileRequest`, `rejectProfileRequest`
- **Support messages** — `addMessage`, `getAllMessages`, `getUserMessages`

The `WalletRequest`, `ProfileRequest`, and `Message` classes exist and are exercised by these methods, but no JavaFX view calls them. They are listed here as scaffolding for future work, not as usable features.

---

## 📋 Requirements

| Requirement | Minimum |
|-------------|---------|
| **OS** | Windows 10+, Linux (Ubuntu 20.04+), macOS 10.13+ |
| **JDK** | 17 or higher |
| **Maven** | 3.6 or higher |
| **RAM** | 512 MB |
| **Disk** | ~200 MB including the Maven repository |

### Dependencies

| Dependency | Version | Scope |
|-----------|---------|-------|
| `org.openjfx:javafx-controls` | 21 | compile |
| `org.openjfx:javafx-graphics` | 21 | compile |
| `junit:junit` | 4.13.2 | test |

Maven resolves these on the first build. No FXML is used — every view is constructed in Java.

### Verify your toolchain

```bash
java -version
```

```bash
mvn -version
```

---

## 🚀 Quick Start

```bash
git clone https://github.com/sifat-jaman-13/Movie-Project-Java.git
```

```bash
cd Movie-Project-Java
```

```bash
mvn clean package
```

For a step-by-step Windows walkthrough including JDK and Maven installation, see [WINDOWS_SETUP.md](WINDOWS_SETUP.md). On Linux and macOS, [setup_linux.sh](setup_linux.sh) checks your toolchain and runs the build:

```bash
chmod +x setup_linux.sh && ./setup_linux.sh
```

---

## 🖥️ Usage

### Run through Maven

```bash
mvn javafx:run
```

### Run the packaged JAR

`mvn clean package` shades dependencies into a single runnable JAR named by the shade plugin's `finalName`:

```bash
java -jar target/MovieStore.jar
```

The application opens at 1000×700. `DataManager.getInstance().initializeData()` seeds the 40-film catalogue before the login view is shown. The main class is `com.moviestore.MovieStoreApp`.

---

## 🔐 Default Credentials

### Admin console

```
Username: Sifat
Password: sifat
```

Hardcoded in [`AdminLoginView.java`](src/main/java/com/moviestore/ui/AdminLoginView.java) as `ADMIN_USERNAME` and `ADMIN_PASSWORD`, with `MAX_ATTEMPTS = 5`.

### Customer accounts

There are **none**. Unlike the Python edition, no demo users are seeded — `initializeData()` populates films only. Register an account on first run.

> ⚠️ **Security note:** the admin credentials are compiled into the source, and user passwords are held as plain strings with no hashing. That is acceptable for a coursework demonstration and unacceptable anywhere else.

---

## 📁 Project Structure

```
Movie-Project-Java/
├── pom.xml                             # Maven build, JavaFX + shade config
├── setup_linux.sh                      # Linux/macOS toolchain check and build
├── WINDOWS_SETUP.md                    # Detailed Windows setup guide
├── TEST_REPORT.md                      # Manual test report
├── LICENSE                             # Proprietary non-commercial use licence
├── ScarZero_Movie_Store_Documentation.docx
└── src/main/java/com/moviestore/
    ├── MovieStoreApp.java              # Application entry point, stage setup
    ├── core/
    │   ├── DataManager.java            # Singleton store for all in-memory state
    │   ├── Movie.java                  # id, title, genre, director, price, year, description, stock
    │   ├── User.java                   # id, username, password, email, walletBalance, registrationDate
    │   ├── Message.java                # Support message record
    │   ├── WalletRequest.java          # Top-up request record
    │   └── ProfileRequest.java         # Profile change request record
    └── ui/
        ├── LoginView.java              # Customer login
        ├── RegisterView.java           # Customer registration and validation
        ├── UserDashboardView.java      # Catalogue browsing and details
        ├── CartView.java               # Cart, total, checkout
        ├── LibraryView.java            # Purchased films
        ├── AdminLoginView.java         # Admin login with attempt limiting
        └── AdminDashboardView.java     # Manage Movies and Manage Users tabs
```

---

## 🏗️ Architecture

Two packages, one responsibility each:

- **`core/`** — the model and the data layer. `DataManager` is a lazily-initialised, `synchronized` singleton holding every collection: `List<User>`, `List<Movie>`, and `Map<String, List<Movie>>` for each user's cart and library, keyed by user id. All reads and writes go through its methods; no view touches a collection directly.
- **`ui/`** — the JavaFX views. Each is a class exposing a `getView()` that returns its node, which the caller swaps into the shared `BorderPane` centre. Navigation is therefore explicit: a view builds its successor and sets it on the parent pane.

The model classes implement `Serializable` and declare a `serialVersionUID`, which prepares them for object serialisation — but no code serialises them today.

### Patterns used

| Pattern | Where |
|---------|-------|
| **Singleton** | `DataManager.getInstance()` — one synchronised instance for all state |
| **Encapsulation** | Private fields with getters and setters across all `core/` classes |
| **Separation of concerns** | `ui/` renders and delegates; `core/` owns state and rules |

---

## 💾 Data Persistence

**There is none.** This is the most important thing to know before running the project.

`DataManager` holds everything in `ArrayList` and `HashMap` fields. There is no file or database I/O anywhere in the codebase — no `java.io` reader or writer, no JDBC, no serialisation call. `saveAllData()` exists and prints a confirmation, but writes nothing:

```java
public void saveAllData() {
    // In production, this would save to database
    System.out.println("Data saved successfully");
}
```

Consequently, on every restart:

| Data | Behaviour on restart |
|------|---------------------|
| Film catalogue | Re-seeded from code — the same 40 films every time |
| Registered accounts | **Lost** |
| Cart contents | **Lost** |
| Purchased library | **Lost** |
| Films added by the admin | **Lost** |
| Wallet balances | **Lost** |

Adding persistence is the first item on the [Roadmap](#-roadmap). Because the model classes are already `Serializable` and all access is funnelled through `DataManager`, it can be done behind the existing method signatures without touching a single view.

> The Python edition takes the other approach and persists to pipe-separated text files. The contrast between the two is deliberate and is discussed in [TEST_REPORT.md](TEST_REPORT.md).

---

## 🐛 Troubleshooting

**`mvn` is not recognised**
Maven is not installed or not on your `PATH`. See [WINDOWS_SETUP.md](WINDOWS_SETUP.md).

**`Error: JavaFX runtime components are missing`**
You ran the class directly instead of through Maven or the shaded JAR. Use `mvn javafx:run`, or `java -jar target/MovieStore.jar`.

**`invalid target release: 17`**
Your JDK is older than 17. Check with `java -version` and confirm `JAVA_HOME` points at a JDK 17+ installation.

**Build fails on the first run with dependency resolution errors**
Maven needs network access to download JavaFX. Retry with `mvn -U clean package`.

**The window opens blank or the UI is scaled oddly on Linux**
Force the software pipeline:

```bash
mvn javafx:run -Dprebuilt.javafx.args="-Dprism.order=sw"
```

**My account is gone after restarting**
Expected — see [Data Persistence](#-data-persistence). Nothing is saved to disk.

---

## ⚠️ Known Limitations

1. **Nothing persists.** All state is in memory and is discarded when the application exits.
2. **A wallet cannot be topped up.** New accounts start with a $50.00 credit (`STARTING_WALLET_BALANCE` in `RegisterView`), which is enough for three or four films. Once it is spent, checkout reports "Insufficient Funds" and there is no way to add more — the top-up logic in `DataManager` is not surfaced by any view.
3. **Stock is never decremented.** `Movie` has `getStock`/`setStock` and stock is captured when a film is added, but no purchase path reduces it.
4. **Wallet requests, profile requests, and messaging are unreachable** from the UI, as described above.
5. **The admin console has two tabs only** — Manage Movies and Manage Users. There is no dashboard, no analytics, no category management, and no edit or delete controls in the UI once a film has been added.
6. **Passwords are plain text** and the admin credentials are compiled in.
7. **No search or filtering.** The catalogue is one flat list; `getMoviesByGenre` exists in `DataManager` but no view calls it.
8. **No automated tests.** JUnit is declared in `pom.xml` but `src/test` does not exist; verification was manual and is recorded in [TEST_REPORT.md](TEST_REPORT.md).

---

## 🗺️ Roadmap

- [ ] Persist state — serialise the `DataManager` collections, or move to SQLite behind the existing method signatures
- [ ] Build the wallet top-up view so a spent balance can be refilled
- [ ] Decrement stock on purchase and block checkout on sold-out titles
- [ ] Build admin screens for the wallet, profile, and message queues already implemented in `core/`
- [ ] Add edit and delete for films, and category management
- [ ] Add catalogue search and a genre filter using `getMoviesByGenre`
- [ ] Hash passwords and move admin credentials out of the source
- [ ] Add a JUnit suite covering `DataManager`
- [ ] Extract inline styling into a stylesheet

---

## 🎓 Learning Outcomes

- **Object-oriented design** — modelling a domain as classes with encapsulated state and behaviour
- **Design patterns** — a singleton data layer, and passing controllers between views
- **JavaFX** — scene graph construction, layout panes, `TableView`/`ListView`, event handlers, and dialogs
- **Collections framework** — `List` and `Map` for relational-style data without a database
- **Build tooling** — Maven lifecycle, dependency scopes, and producing a runnable shaded JAR
- **Cross-platform development** — one codebase running on Windows, Linux, and macOS
- **Reading your own code honestly** — auditing what is actually wired up versus what merely exists

---

## 👤 Author

**Sifat Jaman** — [@sifat-jaman-13](https://github.com/sifat-jaman-13)
North South University, Department of Electrical and Computer Engineering

### Related projects

- [**GameStore-Python**](https://github.com/sifat-jaman-13/GameStore-Python) — the same storefront architecture in Python with CustomTkinter, with file-based persistence, for CSE101
- [**SAT**](https://github.com/sifat-jaman-13/SAT) — phonebook and movie library management systems in C, for CSE115L

---

## 📜 License

This project is available under a **proprietary non-commercial use licence**. Individual personal and educational use is permitted under the terms in [LICENSE](LICENSE). Commercial or organisational use requires prior written permission. Third-party components remain under their respective licences. Students are encouraged to study the project, take inspiration from it, and build a similar or improved application with their own design, code, and ideas rather than copying it wholesale.
