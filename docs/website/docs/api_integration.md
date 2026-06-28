# 🔗 API Integration Strategy & Future Backend Migration

This document outlines the design patterns used by the B2B portal to communicate with database APIs, manage data caching, and explains the roadmap for migrating the current client-side Supabase model to an enterprise-grade ASP.NET Web API backend.

---

## 1. Current Client-Side Data Integration

In its current state, the frontend communicates directly with a cloud database using the **Supabase JavaScript SDK** (`@supabase/supabase-js`). 

### Key Integration Patterns
*   **Unified Client Initialization (`supabase/client.ts`)**: A single, global client instance configuration handles JWT session persistence using `localStorage`. It manages token refresh operations in the background.
*   **Relational Joins**: The application retrieves nested relational resources directly through client-side query formulation. For example, student details alongside their related user information and certificates are loaded in a single query:
    ```typescript
    // Example of client-side join formatting
    const { data } = await supabase
      .from('student_profile')
      .select('*, user(full_name, email, phone_number), student_skills(skill_name)');
    ```
*   **TanStack Query Caching & Mutations**: To prevent excessive API calls and table locks:
    *   **Data Fetching**: Queries are wrapped in React Query's `useQuery` hooks. This ensures data is cached, reused across pages, and re-fetched only when stale.
    *   **Mutations**: Writing and editing operations are performed asynchronously. Upon success, React Query's cache keys are invalidated, prompting pages to pull fresh data in the background.

---

## 2. Enterprise Backend Migration Roadmap (.NET 9.0 & SQL Server)

To scale operations, secure business logic, and transition away from client-side database queries, a migration plan has been designed to replace Supabase with an **ASP.NET Core Web API 9.0** backend and **Entity Framework (EF) Core** targeting **Microsoft SQL Server**.

```
   [Legacy Client-Side Querying]                [Target Enterprise Backend API]
     ┌───────────────────────┐                    ┌─────────────────────────┐
     │  React SPA Frontend   │                    │   React SPA Frontend    │
     └───────────┬───────────┘                    └────────────┬────────────┘
                 │ (Direct SQL Filters)                        │ (JSON Payload)
                 ▼                                             ▼
     ┌───────────────────────┐                    ┌─────────────────────────┐
     │ Supabase Client / DB  │                    │ ASP.NET Web API Controller
     └───────────────────────┘                    └────────────┬────────────┘
                                                               │ (EF Core Queries)
                                                               ▼
                                                  ┌─────────────────────────┐
                                                  │   SQL Server Database   │
                                                  └─────────────────────────┘
```

### A. Core Architecture Changes
1.  **Authentication**: Direct JWT generation by Supabase Auth will be replaced by **ASP.NET Core Identity** and JWT Bearer authentication, securing endpoints using role-based policies.
2.  **API Handoff**: All client-side `supabase.from(...)` queries will be refactored into standard RESTful HTTP requests using an custom `apiClient` (Axios wrapper).
3.  **Real-Time Subscriptions**: Messaging and notification listener bindings will transition from PostgreSQL triggers to **SignalR Hubs**, providing low-latency duplex communication.

### B. Mapped Endpoint Specifications
A target REST API interface has been designed to mirror the current data structure. Below is the mapping from the frontend page requirements to the target .NET endpoints:

| Frontend View / Component | Legacy Supabase Operations | Target .NET REST Endpoint | Http Method |
| :--- | :--- | :--- | :--- |
| **Auth Gate (`Auth.tsx`)** | `supabase.auth.signInWithPassword` | `/api/auth/login` | `POST` |
| **B2B Stats (`Dashboard.tsx`)** | Multiple `supabase.from().select(..., {count: 'exact'})` | `/api/dashboard/stats` | `GET` |
| **Applicant Lists (`Applicants.tsx`)** | `supabase.from('application').select('*, user(*)')` | `/api/applications/company` | `GET` |
| **Student Directory (`Students.tsx`)** | `supabase.from('student_profile').select(...)` | `/api/students` | `GET` |
| **Announcements (`AdminIndex.tsx`)** | `supabase.from('announcement').select(...)` | `/api/announcements` | `GET` |
| **Opportunity Form (`Projects.tsx`)** | `supabase.from('opportunity').insert(...)` | `/api/opportunities` | `POST` |
| **Training Audits (`AdminIndex.tsx`)** | `supabase.from('training_submissions').update(...)` | `/api/admin/training-submissions/{id}/evaluate` | `PUT` |

### C. EF Core Transactional Workflows
Complex operations that alter multiple database records (e.g. marking a student's training certificate as Approved and updating their completed training hours) will run inside transactional database contexts:

1.  **Verify Submissions**: Audits training submissions records (`dbo.TrainingSubmissions.Status = 'approved'`).
2.  **Update Candidate Profiles**: Adds opportunity durations to student profiles (`dbo.StudentProfiles.TrainingDays += opp.Duration`).
3.  **Progress Applications**: Transitions application records from `completed_by_company` to `completed`.
4.  **Audit Ledger Actions**: Deletes records from active assignments (`dbo.Assignments`) and appends ledgers inside completed opportunities (`dbo.CompletedOpportunities`).
