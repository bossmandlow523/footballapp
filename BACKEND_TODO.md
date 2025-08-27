# Backend Development Todo List - FootballIQ
*Organized by dependencies and priority with detailed subtasks*

## Phase 1: Critical Foundation (Must complete first)

### 1.1 External Services Setup
**Dependencies:** None - Start here
- [x] **Create Supabase Cloud project with PostgreSQL database**
  - [x] Sign up for Supabase account
  - [x] Create new project "FootballIQ"
  - [x] Note project URL and anon key
  - [x] Configure project settings (timezone, region)
- [x] **Set up OpenRouter API account and obtain API keys**
  - [x] Create OpenRouter account at openrouter.ai
  - [x] Generate API key with appropriate limits
  - [x] Set up billing and usage monitoring
  - [x] Test API key with simple request to preferred model
- [x] **Set up environment variables and secrets management**
  - [x] Create `.env.local` file
  - [x] Add Supabase credentials
  - [x] Add OpenRouter API key
  - [x] Set up development environment variables
  - [x] Configure gitignore for secrets

### 1.2 Core Database Schema
**Dependencies:** Supabase project created
- [x] **Create core database tables**
  - [x] Create `profiles` table with RLS enabled
  - [x] Create `teams` table with coach relationships
  - [x] Create `plays` table with team relationships
  - [x] Create `signals` table with team relationships
  - [x] Create `questions` table with play/signal relationships
  - [x] Create `user_progress` table for tracking
  - [x] Create `learning_sessions` table
  - [x] Create `team_philosophies` table
  - [x] Create `signal_combinations` table for complex signals
  - [x] Create `routes` table for standard football routes
  - [x] Create `play_routes` table for many-to-many relationships
  - [x] Create `concepts` table for team-specific concepts
  - [x] Create `scenarios` table for AI-generated scenarios
  - [x] Create `formations` table for standard formations
- [x] **Establish foreign keys and constraints**
  - [x] Add team_id foreign keys across all tables
  - [x] Add user_id foreign keys for ownership
  - [x] Add play_id/signal_id foreign keys to questions
  - [x] Add cascade delete rules
  - [x] Add check constraints for enums
  - [x] Enhanced existing tables with new columns (formation_id, concept_ids, difficulty_level, etc.)
- [x] **Implement Row Level Security policies**
  - [x] Enable RLS on all tables (including new tables)
  - [x] Create team-based access policies
  - [x] Create role-based policies (coach/player/admin)
  - [x] Added policies for signal_combinations, routes, play_routes, concepts, scenarios, formations
  - [x] Created helper functions for team/role checking
- [x] **Create performance indexes**
  - [x] Index team_id columns across all tables (primary access pattern)
  - [x] Index user_id columns for progress tracking
  - [x] Index created_at for time-based queries
  - [x] Composite indexes for common query patterns
  - [x] Functional indexes for text search and JSON operations
  - [x] Partial indexes for performance optimization
  - [x] Analytics indexes for reporting queries

### 1.3 File Storage Setup
**Dependencies:** Database schema complete
- [x] **Set up Supabase Storage buckets**
  - [x] Create `signal-videos` bucket (100MB limit, video formats)
  - [x] Create `play-diagrams` bucket (10MB limit, image formats)
  - [x] Create `generated-content` bucket (50MB limit, mixed formats)
  - [x] Configure CDN settings and cache policies
  - [x] Set up storage policies matching RLS (team-based access)
  - [x] Created helper functions for file path generation and permissions
  - [x] Added storage analytics views and cleanup functions

## Phase 2: Authentication & Security (High Priority)

### 2.1 Supabase Authentication
**Dependencies:** Database schema, storage buckets
- [ ] **Integrate Supabase Auth with JWT token management**
  - [ ] Configure Supabase client for browser
  - [ ] Configure Supabase client for server
  - [ ] Set up auth state management
  - [ ] Configure JWT refresh handling
- [ ] **Set up session management and automatic token refresh**
  - [ ] Implement session persistence
  - [ ] Handle token expiration
  - [ ] Set up refresh token rotation
  - [ ] Test session timeout scenarios

### 2.2 Next.js Security Layer
**Dependencies:** Supabase auth configured
- [ ] **Implement Next.js middleware for route protection**
  - [ ] Create middleware.ts file
  - [ ] Protect dashboard routes
  - [ ] Protect API routes
  - [ ] Handle auth redirects
  - [ ] Test protected route access
- [ ] **Implement role-based access control**
  - [ ] Create role checking utilities
  - [ ] Implement coach-only routes
  - [ ] Implement player access restrictions
  - [ ] Add admin role handling
  - [ ] Test role-based permissions

### 2.3 Input Validation
**Dependencies:** None (can be done in parallel)
- [ ] **Implement input validation with Zod schemas**
  - [ ] Install and configure Zod
  - [ ] Create schemas for user profiles
  - [ ] Create schemas for team data
  - [ ] Create schemas for plays/signals
  - [ ] Create validation middleware
  - [ ] Test validation edge cases

## Phase 3: Core API Layer (High Priority)

### 3.1 Authentication API
**Dependencies:** Auth system complete
- [ ] **Build Auth API endpoints**
  - [ ] `/api/auth/login` - handle email/password login
  - [ ] `/api/auth/register` - user registration with role
  - [ ] `/api/auth/logout` - secure logout
  - [ ] `/api/auth/recover` - password recovery
  - [ ] `/api/auth/callback` - OAuth callback handler
  - [ ] Test all auth flows

### 3.2 Core Business APIs
**Dependencies:** Database, auth API complete
- [ ] **Build Teams API**
  - [ ] `POST /api/teams` - create new team
  - [ ] `GET /api/teams/:id` - get team details
  - [ ] `PUT /api/teams/:id` - update team
  - [ ] `POST /api/teams/:id/invite` - invite players
  - [ ] `POST /api/teams/join` - join team via invite code
  - [ ] Test team CRUD operations
- [ ] **Build Plays API**
  - [ ] `POST /api/plays` - create new play
  - [ ] `GET /api/plays` - list team plays
  - [ ] `GET /api/plays/:id` - get play details
  - [ ] `PUT /api/plays/:id` - update play
  - [ ] `DELETE /api/plays/:id` - delete play
  - [ ] Test play CRUD with file uploads
- [ ] **Build Signals API**
  - [ ] `POST /api/signals` - create new signal
  - [ ] `GET /api/signals` - list team signals
  - [ ] `GET /api/signals/:id` - get signal details
  - [ ] `PUT /api/signals/:id` - update signal
  - [ ] `DELETE /api/signals/:id` - delete signal
  - [ ] Test signal CRUD with video uploads

### 3.3 File Handling API
**Dependencies:** Storage buckets, core APIs
- [ ] **Build Files API**
  - [ ] `POST /api/files/upload` - generic file upload
  - [ ] `POST /api/files/videos` - video upload with processing
  - [ ] `POST /api/files/diagrams` - diagram upload
  - [ ] `DELETE /api/files/:id` - delete file
  - [ ] `GET /api/files/:id/url` - get signed URL
  - [ ] Test file upload limits and validation

## Phase 4: AI Integration (High Priority)

### 4.1 OpenRouter Service Layer
**Dependencies:** OpenRouter API key, core APIs
- [ ] **Build AI API endpoints**
  - [ ] `POST /api/ai/generate-questions` - generate questions from plays
  - [ ] `POST /api/ai/generate-scenarios` - create game scenarios
  - [ ] `POST /api/ai/analyze-philosophy` - learn team philosophy
  - [ ] `POST /api/ai/customize-content` - personalize for players
  - [ ] Implement rate limiting for AI calls
  - [ ] Test AI response quality

### 4.2 Question Generation Services
**Dependencies:** OpenRouter API, plays/signals data
- [ ] **Implement AI Question Generator**
  - [ ] Create question generation prompts for various models
  - [ ] Handle different question types (multiple choice, scenario)
  - [ ] Parse and validate AI responses from OpenRouter
  - [ ] Store generated questions in database
  - [ ] Implement batch generation with model selection
  - [ ] Test question quality and variety across models

## Phase 5: Learning & Progress Systems (Medium Priority)

### 5.1 Progress Tracking
**Dependencies:** User progress table, questions
- [ ] **Build Learning API**
  - [ ] `POST /api/learning/sessions` - start learning session
  - [ ] `POST /api/learning/answer` - submit answer
  - [ ] `GET /api/learning/progress` - get user progress
  - [ ] `PUT /api/learning/sessions/:id` - complete session
  - [ ] Test progress calculation accuracy
- [ ] **Implement Progress Tracker service**
  - [ ] Track correct/incorrect answers
  - [ ] Calculate learning streaks
  - [ ] Identify knowledge gaps
  - [ ] Generate progress reports
  - [ ] Test progress accuracy

### 5.2 Analytics System
**Dependencies:** Progress tracking complete
- [ ] **Build Analytics API**
  - [ ] `GET /api/analytics/team` - team performance metrics
  - [ ] `GET /api/analytics/player/:id` - individual progress
  - [ ] `GET /api/analytics/content` - content effectiveness
  - [ ] `GET /api/analytics/trends` - learning trends
  - [ ] Test analytics calculations
- [ ] **Implement Team Analytics service**
  - [ ] Aggregate team performance data
  - [ ] Identify top performers
  - [ ] Track improvement over time
  - [ ] Generate coach insights
  - [ ] Test analytics accuracy

## Phase 6: State Management (High Priority)

### 6.1 Zustand Stores
**Dependencies:** Core APIs complete
- [ ] **Implement Auth Store**
  - [ ] User authentication state
  - [ ] Team context management
  - [ ] Role-based UI state
  - [ ] Session persistence
  - [ ] Test auth state transitions
- [ ] **Implement Content Store**
  - [ ] Plays data management
  - [ ] Signals data management
  - [ ] Questions cache
  - [ ] Optimistic updates
  - [ ] Test data synchronization
- [ ] **Implement Learning Store**
  - [ ] Quiz progress state
  - [ ] Learning session tracking
  - [ ] Answer history
  - [ ] Progress metrics
  - [ ] Test learning state accuracy
- [ ] **Implement UI Store**
  - [ ] Navigation state
  - [ ] Modal/drawer state
  - [ ] Loading states
  - [ ] Notification state
  - [ ] Test UI state consistency

## Phase 7: Business Logic Services (Medium Priority)

### 7.1 Advanced Content Processing
**Dependencies:** File API, AI services
- [ ] **Implement Play Parser service**
  - [ ] Extract play data from diagrams
  - [ ] Parse formation information
  - [ ] Identify route patterns
  - [ ] Test parsing accuracy
- [ ] **Implement Signal Processor**
  - [ ] Process video metadata
  - [ ] Extract signal timing
  - [ ] Generate thumbnails
  - [ ] Test video processing
- [ ] **Implement Video Handler**
  - [ ] Video compression
  - [ ] Format optimization
  - [ ] CDN integration
  - [ ] Test video delivery

## Phase 8: Real-time Features (Medium Priority)

### 8.1 Real-time Infrastructure
**Dependencies:** Core functionality stable
- [ ] **Set up Supabase Real-time subscriptions**
  - [ ] Configure real-time channels
  - [ ] Set up WebSocket connections
  - [ ] Handle connection errors
  - [ ] Test real-time reliability
- [ ] **Implement live progress updates**
  - [ ] Real-time quiz progress
  - [ ] Live leaderboards
  - [ ] Coach dashboard updates
  - [ ] Test real-time accuracy
- [ ] **Implement team notifications**
  - [ ] New content alerts
  - [ ] Progress milestones
  - [ ] Team announcements
  - [ ] Test notification delivery

## Phase 9: Performance & Security (Medium Priority)

### 9.1 Performance Optimization
**Dependencies:** Core functionality complete
- [ ] **Implement caching strategies**
  - [ ] OpenRouter response caching
  - [ ] Database query caching
  - [ ] Static content caching
  - [ ] Test cache performance
- [ ] **Optimize database performance**
  - [ ] Query optimization
  - [ ] Connection pooling
  - [ ] Index optimization
  - [ ] Test query performance
- [ ] **Implement rate limiting**
  - [ ] API endpoint rate limiting
  - [ ] AI service rate limiting
  - [ ] User action throttling
  - [ ] Test rate limit effectiveness

### 9.2 Security Hardening
**Dependencies:** Core functionality complete
- [ ] **Configure production security**
  - [ ] CORS configuration
  - [ ] Request validation
  - [ ] Security headers
  - [ ] Test security measures
- [ ] **Implement comprehensive error handling**
  - [ ] API error responses
  - [ ] Database error handling
  - [ ] External service errors
  - [ ] Test error scenarios
- [ ] **Set up security logging**
  - [ ] Authentication events
  - [ ] Access control violations
  - [ ] Suspicious activity detection
  - [ ] Test security monitoring

## Phase 10: Testing & Quality (Medium Priority)

### 10.1 Comprehensive Testing
**Dependencies:** All core functionality complete
- [ ] **Write API endpoint tests**
  - [ ] Authentication endpoint tests
  - [ ] CRUD operation tests
  - [ ] Error handling tests
  - [ ] Test API reliability
- [ ] **Write database tests**
  - [ ] Schema validation tests
  - [ ] Constraint tests
  - [ ] RLS policy tests
  - [ ] Test database integrity
- [ ] **Write integration tests**
  - [ ] Auth flow tests
  - [ ] File upload tests
  - [ ] AI service tests
  - [ ] Test system integration

## Phase 11: Deployment & Production (Low Priority)

### 11.1 Production Setup
**Dependencies:** All functionality tested
- [ ] **Configure Vercel deployment**
  - [ ] Production build configuration
  - [ ] Environment variable setup
  - [ ] Domain configuration
  - [ ] Test production deployment
- [ ] **Set up production monitoring**
  - [ ] Performance monitoring
  - [ ] Error tracking
  - [ ] Usage analytics
  - [ ] Test monitoring alerts
- [ ] **Create operational procedures**
  - [ ] Database migration scripts
  - [ ] Backup procedures
  - [ ] Recovery procedures
  - [ ] Test operational readiness

## Critical Path Analysis

### Absolute Prerequisites (Complete First)
1. **Supabase Project Setup** → Everything else depends on this
2. **Database Schema** → All data operations depend on this
3. **Authentication System** → All protected features depend on this
4. **Core API Layer** → Business logic depends on this

### Parallel Development Opportunities
- **Input Validation** can be developed alongside auth
- **Zustand Stores** can be developed alongside APIs
- **Testing** can be written as features are completed
- **AI Services** can be developed after OpenRouter setup

### Feature Dependencies
- **Learning API** → requires **Progress Tracking** → requires **Questions**
- **Analytics** → requires **Progress Data** → requires **Learning Sessions**
- **Real-time Features** → requires **Core Functionality** to be stable
- **Advanced Services** → require **Basic CRUD** operations to be working

### Performance & Security (Continuous)
- **Rate Limiting** should be implemented with APIs
- **Error Handling** should be implemented with each service
- **Caching** should be added as performance bottlenecks are identified
- **Testing** should be written continuously as features are developed