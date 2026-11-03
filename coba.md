Project Suggestion: Personal Expense Tracker

  A simple but architecturally complete app that mirrors the same patterns from both projects. Here's why this works: it naturally
  requires auth, CRUD, forms, charts, camera, and notifications — hitting every major pattern without artificial complexity.

  Tech Stack (matches your existing projects)

  ┌───────────────┬──────────────────────────────────────────────────┬────────────────────────────┐
  │     Layer     │                       Tool                       │          Mirrors           │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Framework     │ Expo SDK 54 + React 19                           │ tgmdp_rn_app               │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Routing       │ Expo Router (file-based)                         │ tgmdp_rn_app               │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ State         │ Zustand 5 + persist middleware                   │ Both projects              │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ API Client    │ Axios with interceptors                          │ Both projects              │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Validation    │ Zod 4                                            │ tgmdp-app-nwis2            │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Styling       │ Tailwind (NativeWind)                            │ tgmdp-app-nwis2            │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Charts        │ react-native-chart-kit or Victory Native         │ tgmdp-app-nwis2 (chart.js) │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Auth          │ expo-secure-store + expo-local-authentication    │ tgmdp_rn_app               │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Camera        │ expo-camera / expo-image-picker                  │ tgmdp_rn_app               │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Notifications │ expo-notifications                               │ tgmdp_rn_app               │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Testing       │ Jest + @testing-library/react-native             │ Both projects              │
  ├───────────────┼──────────────────────────────────────────────────┼────────────────────────────┤
  │ Quality       │ ESLint + Husky + lint-staged + TypeScript strict │ Both projects              │
  └───────────────┴──────────────────────────────────────────────────┴────────────────────────────┘

  Architecture (simplified mirror)

  app/                          # Expo Router (file-based)
  ├── _layout.tsx               # Root layout (providers, auth guard)
  ├── index.tsx                 # Redirect based on auth state
  ├── (auth)/
  │   ├── login.tsx             # Login screen
  │   └── register.tsx          # Register screen
  ├── (tabs)/
  │   ├── _layout.tsx           # Tab navigator
  │   ├── home.tsx              # Recent expenses list
  │   ├── add.tsx               # Add expense form
  │   ├── stats.tsx             # Charts dashboard
  │   └── settings.tsx          # Profile & preferences

  services/
  ├── api.ts                    # Axios instance + interceptors + token refresh
  ├── auth.service.ts           # Login, register, biometric unlock
  └── expense.service.ts        # CRUD for expenses

  stores/
  ├── authStore.ts              # Auth state + session persistence
  ├── expenseStore.ts           # Expense list + filters
  └── uiStore.ts                # Loading, dialogs, theme

  schemas/
  ├── auth.schema.ts            # Zod: LoginRequest, RegisterRequest, Session
  ├── expense.schema.ts         # Zod: Expense, CreateExpenseInput
  └── api.schema.ts             # Zod: ApiResponse<T>

  hooks/
  ├── useAppInit.ts             # App bootstrap (check session, biometric)
  ├── useAuthGuard.ts           # Redirect if not logged in
  ├── useFormGuard.ts           # Dirty-state navigation prevention
  └── useExpenses.ts            # Fetch + cache expense data

  components/
  ├── ui/                       # Button, Input, Select, Dialog
  ├── layout/                   # Header, TabBar, LoadingOverlay
  └── business/                 # ExpenseCard, CategoryPicker, ReceiptPhoto

  types/
  └── index.ts                  # Re-exports from schemas

  utils/
  ├── format.ts                 # Currency, dates
  └── validation.ts             # Shared validators

  4-Week Learning Plan

  Week 1 — Foundation
  - Set up Expo + TypeScript strict + ESLint + Husky + lint-staged
  - Expo Router: layouts, tabs, auth group with redirect
  - Zustand basics: authStore with persist middleware
  - Build login/register screens with controlled form inputs + Zod validation
  - Axios instance with base URL, interceptors, token injection
  - expo-secure-store for token storage

  Week 2 — Core Features
  - Expense CRUD: service layer → Zustand store → screens
  - Form handling: add/edit expense with validation, dirty-state guard
  - Camera integration: expo-image-picker for receipt photos
  - Category system with custom select component
  - Loading/error states with uiStore (busy counter pattern from your projects)

  Week 3 — Polish & Native
  - Stats dashboard with charts (spending by category, monthly trends)
  - expo-local-authentication for biometric app unlock
  - expo-notifications for budget alerts/reminders
  - Pull-to-refresh, empty states, swipe-to-delete
  - Responsive styling with NativeWind

  Week 4 — Quality & Deploy
  - Write tests: unit tests for stores/services, component tests for key screens
  - Hit reasonable coverage on business logic
  - Build APK/IPA with environment configs (dev/prod)
  - Final code cleanup, type safety audit

  Backend Options (keep it simple)

  Pick one — the point is learning RN, not backend:
  - json-server or Mockoon — zero-effort REST mock (week 1-2)
  - Supabase — instant Postgres + Auth + REST API (swap in week 2-3)
  - Your existing tgmdp_backend if you want real integration practice

  What This Covers vs Your Projects

  ┌──────────────────────┬──────────────────────────────┬──────────────────────────┬─────────────────────────────┐
  │       Pattern        │         tgmdp_rn_app         │     tgmdp-app-nwis2      │          Your app           │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ File-based routing   │ Expo Router                  │ react-router             │ Expo Router                 │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Auth + token refresh │ Complex SSO/OTP/biometric    │ Bridge session           │ Simple JWT + biometric      │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Zustand + persist    │ 6+ stores                    │ 15 stores                │ 3 stores                    │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Axios interceptors   │ Full (401 handling, logging) │ Via native bridge        │ Token inject + 401 redirect │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Zod schemas          │ Implicit types               │ Full schema layer        │ Full schema layer           │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Custom hooks         │ 12+ hooks                    │ 10 hooks                 │ 4-5 hooks                   │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Service layer        │ Auth, notification, bridge   │ Bridge, location, camera │ Auth, expense               │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Form validation      │ Custom components            │ Manual + Zod             │ Manual + Zod                │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Camera/native        │ Custom modules               │ Via bridge               │ expo-image-picker           │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Charts               │ —                            │ chart.js                 │ chart-kit                   │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Testing              │ Jest + TL                    │ Vitest + TL              │ Jest + TL                   │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────┼─────────────────────────────┤
  │ Pre-commit hooks     │ Husky + lint-staged          │ Husky + lint-staged      │ Husky + lint-staged         │
  └──────────────────────┴──────────────────────────────┴──────────────────────────┴─────────────────────────────┘

  This gives you the same architectural muscles (service layer, store pattern, hook composition, schema validation, interceptor-based
  auth) without the enterprise complexity (WebView pools, bridge protocols, OTA bundles, 40+ routes). Once you're comfortable, you'll be
  able to read and contribute to the real codebase.