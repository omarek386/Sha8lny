# Tech Stack & Dependencies

This document provides a comprehensive breakdown of the technologies, libraries, and core Flutter packages used in **Sha8lny**, along with the architectural justification for their selection.

---

## 🛠️ Core Technology Stack

- **Framework**: [Flutter](https://flutter.dev/) (SDK `>=3.0.0 <4.0.0`)
- **Programming Language**: [Dart](https://dart.dev/)
- **Primary Backend**: [Supabase](https://supabase.com/) (PostgreSQL Database, Storage, Realtime Engine)
- **Secondary Cloud Services**: [Firebase](https://firebase.google.com/) (Auth, Cloud Messaging, Firestore)

---

## 📦 Key Packages & Architectural Justifications

The project's dependencies are chosen to enforce Clean Architecture boundaries, optimize performance, and deliver a smooth user experience.

### 1. State Management & Architecture
| Package | Purpose | Architectural Justification |
| :--- | :--- | :--- |
| [`flutter_bloc`](https://pub.dev/packages/flutter_bloc) | BLoC & Cubit State Management | Decouples business logic from presentation layer. Ensures unidirectional data flow and highly predictable UI states. |
| [`equatable`](https://pub.dev/packages/equatable) | Value-based Object Comparisons | Simplifies comparisons of models and states by value rather than memory address. Minimizes redundant widget rebuilds. |
| [`get_it`](https://pub.dev/packages/get_it) | Service Locator for Dependency Injection | Facilitates inversion of control (IoC). Enables lazy singleton registrations, reducing memory overhead and improving testability. |
| [`dartz`](https://pub.dev/packages/dartz) | Functional Programming Helpers | Provides standard functional types like `Either<L, R>` for error handling. Prevents exception propagation, keeping use cases pure and clean. |

### 2. Networking & Cloud Services
| Package | Purpose | Architectural Justification |
| :--- | :--- | :--- |
| [`supabase_flutter`](https://pub.dev/packages/supabase_flutter) | Backend Integration | Provides high-performance APIs for database CRUD operations, user session streams, file storage bucket uploads, and WebSockets. |
| [`dio`](https://pub.dev/packages/dio) | Custom HTTP Client | Chosen over standard Http client for its support for global interceptors, custom timeout boundaries, file download streams, and centralized error parsing. |
| [`firebase_core` / `auth`](https://pub.dev/packages/firebase_core) | Secondary Cloud Provider | Enables integration with Google cloud services, supporting the platform's dual-authentication design. |
| [`firebase_messaging` & `flutter_local_notifications`](https://pub.dev/packages/firebase_messaging) | Push Notifications Engine | Connects the device with Firebase Cloud Messaging (FCM) to handle offline notification alerts and local background alerts. |

### 3. Responsive Layouts & Design System
| Package | Purpose | Architectural Justification |
| :--- | :--- | :--- |
| [`flutter_screenutil`](https://pub.dev/packages/flutter_screenutil) | Layout Adaptation | Scales font sizes, margins, and component dimensions across various device viewport aspect ratios. |
| [`google_fonts`](https://pub.dev/packages/google_fonts) | Custom Typography | Dynamically loads and renders modern typefaces (e.g., *Inter*, *Outfit*), ensuring consistent layout appearances across OS platforms. |
| [`flutter_svg`](https://pub.dev/packages/flutter_svg) | Vector Graphic Rendering | Renders SVG files without pixelation, reducing binary asset footprints. |
| [`lottie` / `animate_do` / `flutter_animate`](https://pub.dev/packages/flutter_animate) | Animation Engines | Renders micro-animations and screen transitions, improving user retention and UI feedback loops. |

### 4. Special Features & Hardware Integrations
| Package | Purpose | Architectural Justification |
| :--- | :--- | :--- |
| [`mobile_scanner`](https://pub.dev/packages/mobile_scanner) | Camera Barcode/QR Scanning | Leverages native camera integrations to scan QR codes for wallet payment cards. |
| [`file_picker`](https://pub.dev/packages/file_picker) | System Document Selector | Allows users to browse and select local PDF resumes and assignment files for upload. |
| [`syncfusion_flutter_pdfviewer`](https://pub.dev/packages/syncfusion_flutter_pdfviewer) | PDF Renderer | Integrates an in-app viewer for PDF resumes, eliminating the need to launch external applications. |
| [`youtube_player_flutter`](https://pub.dev/packages/youtube_player_flutter) | Embedded Media Player | Plays embedded video tutorial assignments directly inside the training modules page. |
| [`pinput`](https://pub.dev/packages/pinput) | Custom PIN/OTP Input fields | Renders specialized security PIN and OTP input boxes with validation behaviors. |

---

## 🛠️ Code Generation & Tooling

To accelerate development and prevent human errors in JSON parsing or model generation, the project utilizes compile-time code generation tools:

1. **`build_runner`**: Task executor for compile-time generators.
2. **`freezed`**: Generates type-safe, immutable data classes with union types, pattern matching, and automatic deep copy mechanisms.
3. **`json_serializable`**: Generates boilerplate serialization methods (`fromJson` / `toJson`), ensuring reliable object-to-JSON mappings.
