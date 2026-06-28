# 🏗️ React Architecture & State Management

This document describes the high-level frontend architecture and state management strategy of the **Sha8alny B2B Portal**. 

---

## 1. High-Level Frontend Architecture

The frontend is built as a single-page application (SPA) leveraging **Vite**, **React 18**, and **TypeScript**. It is designed with a strict separation of concerns, isolating page-level routing, reusable layout templates, global authentication contexts, data integrations, and UI components.

```
┌────────────────────────────────────────────────────────┐
│                      HTTP Clients                      │
│             (Supabase Client / API Layer)              │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│                   State Cache Layer                    │
│             (TanStack Query Client Provider)           │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│                   Global Auth Context                  │
│                    (ProfileProvider)                   │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│                 React Router DOM (SPA)                 │
│              (Guards: RequireAdminAuth /               │
│               RequireProfileCompletion)                │
└───────────────────────────┬────────────────────────────┘
                            ▼
┌────────────────────────────────────────────────────────┐
│                   Components & Pages                   │
│             (Admin Panel / Company Portal)             │
└────────────────────────────────────────────────────────┘
```

---

## 2. Client-Side Routing & Guards

Routing is handled programmatically via `react-router-dom` using declarative layouts. The main router in `App.tsx` divides routes into public gateways, company B2B dashboard operations, and admin overview controls. 

### Protected Route Guards
To prevent unauthorized access, the routing tree uses custom Higher-Order Guard Components:

1.  **`RequireAdminAuth`**:
    *   **Scope**: Protects all paths prefixed with `/admin/*`.
    *   **Logic**: Intercepts routing, checks the loaded `ProfileContext`. If the user's role is not explicitly `admin`, it cancels navigation and redirects to `/auth` with an error toast.
2.  **`RequireProfileCompletion`**:
    *   **Scope**: Protects all company dashboard screens (`/home`, `/dashboard`, `/projects`, `/students`, `/messages`, `/applicants`, `/settings`, `/payment`).
    *   **Logic**: Verifies if the user is authenticated. If the active profile has the `company` role claim, it validates that corporate metadata (`website`, `industry`, `description`) is non-empty. If incomplete, it locks down the app features and redirects the user to the profile setup screen (`/profile`) with a toast notification warning.

---

## 3. State Management Strategy

To ensure optimal performance and code simplicity, state management is split between two separate systems based on the nature of the data: **Global Context State** (for authentication sessions and user roles) and **Server-Side Async Cache State** (for database records).

### A. Global Client State: Context API (`ProfileContext`)
The `ProfileContext` acts as the single source of truth for the active user's credentials and profile data. 

*   **Initialization**: On application mount, it retrieves the active session from the auth client (`supabase.auth.getSession()`).
*   **Active Subscriptions**: It binds a real-time event listener to authentication changes (`supabase.auth.onAuthStateChange`). If a login or logout event triggers, the context automatically re-fetches the user record or resets to default parameters.
*   **Role Identification**: The provider fetches user profiles via database joins, loading basic user details alongside company-specific profiles (e.g. `company_profile` fields like industry or website). It sets roles (`admin` or `company`), which are evaluated immediately by the route guards.
*   **Profile Updates**: Exposes a unified `updateProfile()` mechanism. When modified, the client writes changes back to the database and propagates updates to all listening components.

### B. Server/Async Cache: TanStack React Query
For database operations, rather than maintaining state in manual `useState` or Redux slices, the app utilizes **TanStack React Query** (`@tanstack/react-query`).

*   **Declarative Queries**: Data fetching (e.g., retrieving lists of opportunities, student applicants, or messaging lists) is declared using the `useQuery` hook.
*   **Auto Caching & Background Sync**: Query results are cached. If a user navigates away and returns, the UI renders the cached state immediately, and queries are refreshed silently in the background (stale-while-revalidate).
*   **Cache Invalidation**: On mutations (like publishing an opportunity via `AddProjectDialog` or accepting an applicant), the cache is programmatically marked stale using `queryClient.invalidateQueries({ queryKey: [...] })`. This automatically triggers background refreshes for dependent widgets without requiring a full page reload.
*   **Loading & Error Skeletons**: Standardized loading variables (`isLoading`, `isFetching`) bind directly to UI skeletons and spinners, ensuring a polished user experience during async transactions.
