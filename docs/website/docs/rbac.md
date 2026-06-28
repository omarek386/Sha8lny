# 🔑 Role-Based Access Control (RBAC)

This document describes how authorization, route permissions, and UI personalization are handled based on the user's role claim within the **Sha8alny B2B Portal**.

---

## 1. User Roles Overview

The system classifies authenticated accounts into distinct profiles using a `role` flag stored inside the core database `user` table:

| Role Claim | Target Audience | Primary Workspace | Core Responsibilities |
| :--- | :--- | :--- | :--- |
| **`admin`** | Platform Administrators | Admin Portal (`/admin/*`) | User audits, training hours evaluations, site configuration, announcements. |
| **`company`**| Corporate Clients / Employers | Company Portal (`/dashboard`, etc.) | Opportunity posting, student selection, project lifecycle tracking, chat. |
| **`student`**| University Students | Mobile App / External Client | Applying to projects, training document uploads (Restricted on this B2B Portal). |

> [!NOTE]
> Since this application is a dedicated B2B portal targeted specifically at Admins and Companies, any authenticated user with a `student` role claim attempting to log in here is automatically signed out and blocked with an access restriction toast.

---

## 2. Guard Components & Route Protections

Route protection is implemented at the routing layer in `App.tsx` using specialized React wrapper components.

```
                  ┌───────────────────────────────┐
                  │       Navigation Request      │
                  └───────────────┬───────────────┘
                                  │
                  ┌───────────────▼───────────────┐
                  │       Require Auth Guard      │
                  └───────────────┬───────────────┘
                                  │
                 ┌────────────────┴────────────────┐
                 ▼                                 ▼
       [Path: /admin/*]                  [Path: /dashboard/*]
      RequireAdminAuth               RequireProfileCompletion
                 │                                 │
        ┌────────┴────────┐               ┌────────┴────────┐
        ▼                 ▼               ▼                 ▼
   Role = Admin?     Role != Admin?   Profile Complete? Incomplete Profile?
   [Allow Access]    [Redirect /auth] [Allow Access]    [Redirect /profile]
```

### A. Admin Authentication Guard (`RequireAdminAuth`)
*   **Target Routes**: `/admin`, `/admin/table`, `/admin/training-submissions`, `/admin/inbox`, `/admin/opportunities`, `/admin/announcement`, `/admin/settings`, `/admin/profile`.
*   **Logic**:
    1.  Inspects the loading state of `ProfileContext`. If loading is true, displays a full-screen loader.
    2.  Checks `profile.role`. If the active role does not equal `"admin"`, the guard blocks rendering.
    3.  Uses React Router's `<Navigate to="/auth" replace />` to redirect the user to the login screen.

### B. Company Profile Completeness Guard (`RequireProfileCompletion`)
*   **Target Routes**: `/home`, `/dashboard`, `/projects`, `/students`, `/messages`, `/settings`, `/payment`.
*   **Logic**:
    1.  Validates that the user is authenticated (`profile.userId` is set).
    2.  If the role claim is `"company"`, it runs a completeness validation check against the corporate fields:
        $$\text{ProfileComplete} \leftarrow (\text{profile.website} \neq \text{""} \land \text{profile.industry} \neq \text{""} \land \text{profile.description} \neq \text{""})$$
    3.  If any of these fields are missing, the guard prevents access to the core dashboards.
    4.  Displays a warning toast: `"Please complete your company profile to continue"`.
    5.  Redirects the user to the profile setup page (`/profile`), restricting the user from other platform features until the profile is completed.

---

## 3. Dynamic UI and Navigation Adaptation

Layout headers, menus, and actions automatically adapt themselves based on the user's validated profile state.

### A. Sidebar Components Selection
The application serves two entirely distinct layout managers:
*   **`Sidebar` (Company Portal)**: Loaded within `DashboardLayout.tsx` for company users. It exposes links to the company workspace: `HOME`, `Dashboard`, `Projects`, `Student Data`, `Messages`, and `Settings`.
*   **`AdminDashboardLayout` (Admin Portal)**: Exposes options for administrators: `Overview`, `Student Data`, `Training` (submissions pipeline), `Inbox`, `Opportunities` (global view), `Announcement` tool, and `Settings`.

### B. Inline Actions & Layout Adaptations
*   **Header Profile Widgets**: Displays different metadata depending on the user. Company layouts fetch corporate descriptions and logos. Admin headers show user full names and a custom `"Administrator"` subtitle under their profile.
*   **Auth Page Redirections**: Upon successful login, the `Auth` component evaluates the role attribute directly from the user's profile:
    *   If `admin`: redirects to `/admin`.
    *   If `company`: redirects to `/dashboard` (which then checks completeness).
    *   If `student`: logs the user out and displays an access error.
