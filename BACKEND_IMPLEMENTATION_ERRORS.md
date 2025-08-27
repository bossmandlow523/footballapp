# Backend Implementation Errors Summary

## Critical Database Schema Issues

**1. Table Creation Order**
- Created `profiles` table before `teams` table, causing FK constraint failure
- **Fix**: Reordered table creation, added FK constraint after both tables exist

**2. Missing Extension**
- Used `gen_random_uuid()` and `gen_random_bytes()` without enabling `pgcrypto` extension
- **Fix**: Added `CREATE EXTENSION IF NOT EXISTS "pgcrypto";` at start

**3. Data Type Inconsistency**
- Mixed `JSON` and `JSONB` types across tables
- **Fix**: Standardized to `JSONB` for all JSON columns (better performance, indexing)

**4. Column Modification Issues**
- Created `question_type` with CHECK constraint, then dropped and recreated it
- **Fix**: Created column correctly once with proper CHECK constraint

**5. Missing Auto-Update Mechanism**
- `updated_at` columns had no trigger to automatically update timestamps
- **Fix**: Added trigger function and triggers for all tables with `updated_at`

## Row Level Security (RLS) Policy Issues

**6. Incomplete Policy Permissions**
- Used `FOR ALL USING` without `WITH CHECK` clauses for INSERT/UPDATE operations
- **Result**: Users could read but not write/modify data
- **Fix**: Split into separate policies with proper `WITH CHECK` clauses for each operation

**7. Admin Policy Failures**
- Admin management policies on global tables (`routes`, `formations`) lacked `WITH CHECK`
- **Result**: Admin users couldn't insert/update global data
- **Fix**: Added proper `WITH CHECK` clauses for admin operations

**8. Role-Based Write Operations**
- Coach/admin write operations failed due to missing `WITH CHECK` on team-scoped tables
- **Result**: Role-based access control non-functional for writes
- **Fix**: Added comprehensive INSERT/UPDATE/DELETE policies with proper checks

## Key Lessons Learned

1. **Table Dependencies**: Always create tables in dependency order
2. **Extensions First**: Enable required PostgreSQL extensions before using functions
3. **RLS Completeness**: Every operation (SELECT/INSERT/UPDATE/DELETE) needs explicit policies
4. **WITH CHECK Mandatory**: INSERT and UPDATE operations require `WITH CHECK` clauses
5. **Data Type Consistency**: Stick to one JSON type (`JSONB`) for consistency and performance
6. **Triggers for Timestamps**: Auto-updating timestamp columns need triggers

## Files Created to Fix Issues

- `database_schema_fixed.sql` - Corrected table creation with proper order and types
- `rls_policies_fixed.sql` - Complete RLS policies with all operations and WITH CHECK clauses

These fixes ensure the database schema works correctly with proper security and functionality.