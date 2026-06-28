# 🌟 Sha8lny (شغلني) — Showcase Repository

Welcome to the public showcase repository for **Sha8lny**, a smart digital platform connecting university students with freelancing opportunities and verified internships. 

This repository is designed to showcase the platform's **system complexity, architectural integrity, and engineering standards**. 

> [!NOTE]
> **Showcase Notice**: To protect proprietary business logic and student data, this repository does not host the project's raw source code, production API keys, or database credentials. Instead, it serves as an architectural blueprint and documentation portal.

---

## 🏛️ System & Architectural Highlights

Sha8lny is built on modern software engineering design principles, prioritizing maintainability, testability, and decoupling.

* **Clean Architecture**: Strictly isolates Core Business Rules (Domain), Data Adaptors (Data), and UI Elements (Presentation).
* **Dual-Backend Authentication Engine**: Features a compile-time toggleable authentication engine, allowing the app to run on either **Firebase** or **Supabase** via custom dependency inversion.
* **WebSocket Real-time Synchronization**: Implements direct streams to PostgreSQL database triggers via WebSockets for real-time chat, notifications, and status monitoring.
* **BLoC/Cubit State Pattern**: Drives a unidirectional, predictable state architecture throughout all 12+ functional modules.

---

## 📂 Documentation Portal

Explore the technical design of the Sha8lny platform through our detailed documentation files:

* 🏗️ **[System Architecture](docs/system_architecture.md)**: Deep dive into the system topology, client-server design, the dual-auth abstraction mechanics, and reactive data streams.
* 🚀 **[Core Features Breakdown](docs/core_features.md)**: Detailed breakdown of the multi-party completion handshake, chat triggers, wallet QR scanning, and progress calculations.
* 🛠️ **[Tech Stack & Dependencies](docs/tech_stack.md)**: Review of selected libraries (`flutter_bloc`, `supabase_flutter`, `dio`, `get_it`, `dartz`) and the architectural rationale for each.
* 📁 **[Folder Structure & Clean Layers](docs/folder_structure.md)**: Structural mapping showing how files are organized according to Clean Architecture guidelines.

---

## 🔑 Core Feature Set

| Feature Module | Tech Highlights | Primary Tables / Integrations |
| :--- | :--- | :--- |
| **Dual-Provider Auth** | Abstraction interfaces, runtime toggling | `user_table`, `FirebaseAuth`, `SupabaseAuth` |
| **Verification Handshake** | Multi-party escrow verification flow | `completed_opportunity_table`, payment triggers |
| **Real-time Messaging** | WebSockets streaming, offline push notification fallbacks | `chats_table`, `messages_table`, FCM |
| **Digital Wallet & Scanner** | Camera hardware access, QR parsing | `payment_table`, `mobile_scanner` |
| **Progress Metrics** | Dynamic timeline computation | `assignment_table`, `modules_table` |

---

## 🧑‍💻 Technical Leadership & Roles (Team Lead)

As the **Technical Team Lead and Architect** of this project, my responsibilities and contributions included:

1. **System & Database Architecture**: Designed and deployed the secure relational PostgreSQL schemas on Supabase, establishing clean entity relations for students, companies, applications, and payments.
2. **Framework Decoupling**: Architected the abstract authentication boundary, enabling compile-time switches between Supabase and Firebase.
3. **Clean Architecture Guidelines**: Established coding standards for the development team, enforcing strict Clean Architecture boundaries (Domain, Data, Presentation) to maintain a decoupled codebase.
4. **Developer Tooling & Automation**: Created custom code-generation tools (`dart tool/generate_feature.dart`) to automate feature folder and boilerplate file creation, reducing developer onboarding time.
5. **State Management Standards**: Standardized the use of `Cubit` and functional programming patterns (`dartz`'s `Either`) to handle runtime exceptions.

---

## 🛠️ Main Tech Stack

* **Client**: Flutter, Dart, BloC, Cubit
* **Backends**: Supabase, Firebase
* **Realtime**: WebSockets Stream Listener
* **Local Storage**: SharedPreferences (Cached sessions and preferences)
* **Networking**: Dio (HTTP client with custom Interceptors)

---

## 👥 Contributors & Contact

- **Omar Khaled** — Technical Team Lead & Software Architect — [@omarek386](https://github.com/omarek386)
