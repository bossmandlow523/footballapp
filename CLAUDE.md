# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Environment**: Windows (WSL2/PowerShell/Command Prompt compatible)

**Development Server:**
- `npm run dev` - Start development server with Turbopack (http://localhost:3000)
- `npm run build` - Build production application
- `npm start` - Start production server
- `npm run lint` - Run ESLint for code quality

**File Operations:**
- Use Windows-style paths when referencing files outside the project
- Working directory: `/mnt/c/Users/nicks/OneDrive/Desktop/footballapp/footballapp/` (WSL2 path)
- Windows equivalent: `C:\Users\nicks\OneDrive\Desktop\footballapp\footballapp\`

## Project Architecture

This is a **FootballIQ MVP** application - a football coaching platform for team management, playbook creation, signal teaching, and AI-generated quizzes.

### Tech Stack Foundation
- **Next.js 15** with App Router and React 19
- **TypeScript** with strict configuration
- **Tailwind CSS v4** for styling  
- **shadcn/ui** component library (New York style, RSC-enabled)
- **Lucide React** for icons

### Planned Architecture (from TECH_STACK.md)
- **Database**: Supabase (PostgreSQL + Auth + Storage)
- **ORM**: Drizzle ORM
- **AI Integration**: OpenAI GPT-4 for question generation
- **File Upload**: UploadThing for videos/diagrams
- **State Management**: Zustand
- **Forms**: React Hook Form + Zod validation

### Project Structure
```
src/
├── app/              # Next.js App Router
│   ├── layout.tsx    # Root layout with Geist fonts
│   ├── page.tsx      # Home page (currently Next.js default)
│   └── globals.css   # Global Tailwind styles
└── lib/
    └── utils.ts      # Utility functions
```

### Core Features to Implement
Based on TECH_STACK.md, the application will include:
1. **Authentication** - Coach/player roles with team-based access
2. **Play Management** - CRUD operations for football plays with diagrams
3. **Signal Management** - Video upload and meaning assignment
4. **AI Question Generation** - Automated quizzes from plays/signals
5. **Progress Tracking** - User performance analytics
6. **Team Management** - Invite system and role management

### Database Schema Overview
Key entities: Teams, Profiles (extends Supabase auth), Plays, Signals, Questions, UserProgress - all with team-based Row Level Security.

### Development Guidelines
- Use **Server Components** by default for data fetching
- Follow **shadcn/ui patterns** for consistent UI
- Implement **team-based access control** for all features
- Use **TypeScript** strictly - no any types
- Follow **Tailwind utility-first** approach
- Path aliases: `@/` points to `src/`

## Important Notes
- Working directory has nested structure: `/footballapp/footballapp/`
- No existing authentication or database setup yet
- Component library configured with New York style and CSS variables
- Uses Geist font family (Sans and Mono variants)

Every Ui creation should follow the vision of `C:\Users\nicks\OneDrive\Desktop\footballapp\footballapp\MVP_OVERVIEW.md`