# CLAUDE.md - FootballIQ Project Guide

## Project Overview
FootballIQ is an AI-powered football education platform for coaches and players. Built with Next.js 15 + Supabase + OpenRouter.

## Tech Stack
- **Frontend**: Next.js 15 (App Router) + React 19 + TypeScript + Tailwind CSS v4 + shadcn/ui
- **Backend**: Next.js API Routes + Server Actions
- **Database**: Supabase PostgreSQL with Row Level Security
- **Authentication**: Supabase Auth
- **Storage**: Supabase Storage for videos/diagrams
- **AI**: OpenRouter for multi-model question generation
- **State**: Zustand for client state
- **Deployment**: Vercel

## Key Commands
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run type-check   # Run TypeScript checks
npm run lint         # Run ESLint
```

## Project Structure
```
src/
├── app/
│   ├── (auth)/          # Login/register pages
│   ├── (dashboard)/     # Protected dashboard pages
│   └── api/            # API routes
├── components/
│   ├── ui/             # shadcn components
│   ├── auth/           # Authentication components
│   ├── playbook/       # Play management
│   └── signals/        # Signal management
├── lib/
│   ├── supabase/       # Database client & types
│   ├── ai/             # OpenRouter integration
│   └── utils.ts        # Shared utilities
└── store/
    └── useStore.ts     # Zustand stores
```

## Environment Variables (.env.local)
```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
OPENROUTER_API_KEY=your_openrouter_api_key
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## Database Schema (Core Tables)
- **profiles** - User profiles (extends Supabase auth.users)
- **teams** - Team data with coach assignments
- **plays** - Play diagrams and descriptions
- **signals** - Video signals with meanings
- **questions** - AI-generated quiz questions
- **user_progress** - Learning progress tracking
- **learning_sessions** - Study session data

## Key Features
1. **Authentication**: Email/password + Google OAuth via Supabase
2. **Team Management**: Invite players, role-based permissions
3. **Play Management**: Upload diagrams, categorize with tags
4. **Signal Management**: Upload videos with meanings
5. **AI Quiz Generation**: OpenRouter generates questions from plays/signals using optimal models
6. **Progress Tracking**: Individual and team analytics
7. **Real-time Updates**: Supabase real-time subscriptions

## Development Guidelines
- Use Server Components for data fetching
- Follow existing shadcn/ui patterns
- Implement proper error handling
- Use TypeScript strictly
- Apply Row Level Security for all data access
- Optimize images with next/image
- Use Zustand for client state management

## Security Notes
- All data isolated by team via RLS policies
- File uploads validated for type and size
- API routes protected with authentication middleware
- Environment variables never exposed to client
- Input validation with Zod schemas

## Performance Considerations
- Server Components for database queries
- Dynamic imports for heavy components
- Virtual scrolling for large lists
- Optimistic updates for better UX
- Edge runtime for API routes where possible

## Deployment
- **Production**: Vercel (automatic deployment from git)
- **Database**: Supabase Cloud (managed PostgreSQL)
- **Storage**: Supabase Storage with CDN
- **Domain**: Custom domain through Vercel

## Related Files
- `BACKEND_TODO.md` - Comprehensive backend development task list- AFTER A TASK IS COMPLETED PUT A CHECK MARK NEXT TO IT IN THE TEXT FILE
- `footballapp/TECH_STACK.md` - Detailed technical architecture documentation