# Backend QA Fail Report - FootballIQ

- **Task**: Create core database tables
  - **Status**: FAIL
  - **Issues**:
    - `profiles` references `teams` before `teams` is created; invalid execution order.
    - Uses `gen_random_uuid()`/`gen_random_bytes()` without enabling `pgcrypto`.
    - `questions.question_type` created with a CHECK then dropped later; violates spec and breaks dependent logic.
    - Mixed `JSON` and `JSONB` types; inconsistent usage.
    - `updated_at` columns lack automatic update mechanism; values will become stale.

- **Task**: Implement Row Level Security policies
  - **Status**: FAIL
  - **Issues**:
    - "Manage" policies use `FOR ALL USING` without `WITH CHECK`; inserts/updates denied.
    - Admin manage policies on `routes` and `formations` lack `WITH CHECK`; admin inserts blocked.

- **Task**: Create role-based access control
  - **Status**: FAIL
  - **Issues**:
    - Coaches/admins cannot insert/update due to missing `WITH CHECK`; role-based writes non-functional.

- **Task**: Policies for `signal_combinations`, `routes`, `play_routes`, `concepts`, `scenarios`, `formations`
  - **Status**: FAIL
  - **Issues**:
    - Missing `WITH CHECK` on manage policies prevents inserts/updates across these tables.
