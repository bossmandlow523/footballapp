
# FootballIQ: Complete Tech Stack Documentation

## Executive Summary
This document outlines the complete technology stack for FootballIQ MVP, building on your existing Next.js foundation with carefully selected technologies that prioritize simplicity, rapid development, and scalability.

## Current Foundation Analysis
- ✅ **Next.js 15** with App Router
- ✅ **React 19** with latest features
- ✅ **TypeScript** for type safety
- ✅ **Tailwind CSS v4** for styling
- ✅ **shadcn/ui** component library
- ✅ **Lucide React** for icons

---

## Complete Tech Stack for MVP

### **Frontend Architecture**
```
Next.js 15 (App Router) + React 19 + TypeScript
├── UI Framework: shadcn/ui components
├── Styling: Tailwind CSS v4
├── Icons: Lucide React
├── Forms: React Hook Form + Zod validation
├── State Management: Zustand (lightweight)
├── HTTP Client: Built-in fetch with React Server Components
└── File Upload: react-dropzone + @uploadthing/react
```

### **Backend Architecture**
```
Next.js API Routes (Server Actions + Route Handlers)
├── Database: Supabase (PostgreSQL + Auth + Storage)
├── ORM: Drizzle ORM (lightweight, TypeScript-first)
├── Authentication: Supabase Auth
├── File Storage: Supabase Storage
├── AI Integration: OpenAI API (GPT-4)
└── Email: Resend (simple, developer-friendly)
```

### **Database Schema Design**
```sql
-- Core Tables for MVP
Users (id, email, role, team_id, created_at)
Teams (id, name, coach_id, subscription_tier, created_at)
Plays (id, team_id, name, description, tags, diagram_url)
Signals (id, team_id, name, video_url, meaning, created_at)
Questions (id, play_id, signal_id, question_text, correct_answer)
UserProgress (id, user_id, question_id, is_correct, attempted_at)
```

### **Deployment & Infrastructure**
```
Production: Vercel (seamless Next.js integration)
├── Database: Supabase (managed PostgreSQL)
├── File Storage: Supabase Storage
├── Monitoring: Vercel Analytics + Sentry
├── Domain: Custom domain through Vercel
└── Environment: Edge Runtime where possible
```

---

## Detailed Implementation Plan

### **Phase 1: Core Dependencies (Week 1)**

#### Essential Packages to Add:
```json
{
  "dependencies": {
    "@supabase/supabase-js": "^2.39.0",
    "@supabase/ssr": "^0.1.0",
    "drizzle-orm": "^0.29.0",
    "drizzle-kit": "^0.20.0",
    "postgres": "^3.4.0",
    "zod": "^3.22.0",
    "react-hook-form": "@hookform/resolvers",
    "@hookform/resolvers": "^3.3.0",
    "zustand": "^4.4.0",
    "openai": "^4.20.0",
    "uploadthing": "^6.0.0",
    "@uploadthing/react": "^6.0.0",
    "react-dropzone": "^14.2.0",
    "resend": "^2.0.0"
  },
  "devDependencies": {
    "@types/postgres": "^3.0.0",
    "dotenv": "^16.3.0"
  }
}
```

### **Phase 2: Project Structure (Week 1)**

```
src/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── (dashboard)/
│   │   ├── dashboard/page.tsx
│   │   ├── playbook/page.tsx
│   │   ├── signals/page.tsx
│   │   └── analytics/page.tsx
│   ├── api/
│   │   ├── auth/
│   │   ├── plays/
│   │   ├── signals/
│   │   ├── questions/
│   │   └── uploadthing/
│   └── globals.css
├── components/
│   ├── ui/ (shadcn components)
│   ├── auth/
│   ├── playbook/
│   ├── signals/
│   └── dashboard/
├── lib/
│   ├── db/
│   │   ├── schema.ts
│   │   ├── migrations/
│   │   └── queries.ts
│   ├── auth/
│   ├── ai/
│   └── utils.ts
├── store/
│   └── useStore.ts
└── types/
    └── index.ts
```

### **Phase 3: Environment Configuration**

```env
# .env.local
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# OpenAI
OPENAI_API_KEY=your_openai_api_key

# UploadThing
UPLOADTHING_SECRET=your_uploadthing_secret
UPLOADTHING_APP_ID=your_uploadthing_app_id

# Resend
RESEND_API_KEY=your_resend_api_key

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## Implementation Guide

### **Database Setup (Supabase)**

#### 1. Core Tables SQL:
```sql
-- Users table (extends Supabase auth.users)
CREATE TABLE profiles (
  id UUID REFERENCES auth.users(id) PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  role TEXT CHECK (role IN ('coach', 'player')) NOT NULL,
  team_id UUID REFERENCES teams(id),
  first_name TEXT,
  last_name TEXT,
  position TEXT, -- for players
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Teams table
CREATE TABLE teams (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT NOT NULL,
  coach_id UUID REFERENCES profiles(id) NOT NULL,
  subscription_tier TEXT DEFAULT 'starter',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Plays table
CREATE TABLE plays (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  name TEXT NOT NULL,
  description TEXT,
  tags TEXT[],
  diagram_url TEXT,
  formation TEXT,
  personnel TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Signals table
CREATE TABLE signals (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  name TEXT NOT NULL,
  video_url TEXT NOT NULL,
  meaning TEXT NOT NULL,
  signal_type TEXT, -- formation, route, protection, etc.
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- AI Generated Questions
CREATE TABLE questions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  play_id UUID REFERENCES plays(id),
  signal_id UUID REFERENCES signals(id),
  question_text TEXT NOT NULL,
  question_type TEXT, -- multiple_choice, signal_recognition, scenario
  correct_answer TEXT NOT NULL,
  options JSONB, -- for multiple choice
  difficulty INTEGER DEFAULT 1,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- User Progress Tracking
CREATE TABLE user_progress (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES profiles(id) NOT NULL,
  question_id UUID REFERENCES questions(id) NOT NULL,
  is_correct BOOLEAN NOT NULL,
  time_taken INTEGER, -- seconds
  confidence_level INTEGER, -- 1-5 scale
  attempted_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 2. Row Level Security (RLS):
```sql
-- Enable RLS
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE teams ENABLE ROW LEVEL SECURITY;
ALTER TABLE plays ENABLE ROW LEVEL SECURITY;
ALTER TABLE signals ENABLE ROW LEVEL SECURITY;
ALTER TABLE questions ENABLE ROW LEVEL SECURITY;
ALTER TABLE user_progress ENABLE ROW LEVEL SECURITY;

-- Example policies (team-based access)
CREATE POLICY "Users can view own profile" ON profiles
  FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Team members can view team data" ON plays
  FOR SELECT USING (
    team_id IN (
      SELECT team_id FROM profiles WHERE id = auth.uid()
    )
  );
```

### **Authentication Setup**

#### 1. Supabase Auth Configuration:
```typescript
// lib/auth/supabase.ts
import { createBrowserClient } from '@supabase/ssr'
import { createServerClient } from '@supabase/ssr'

export const createClient = () => {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}

export const createServerSupabaseClient = () => {
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookies().get(name)?.value
        },
      },
    }
  )
}
```

#### 2. Middleware for Auth:
```typescript
// middleware.ts
import { createServerClient } from '@supabase/ssr'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(request: NextRequest) {
  const response = NextResponse.next()
  
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return request.cookies.get(name)?.value
        },
        set(name: string, value: string, options: any) {
          response.cookies.set({ name, value, ...options })
        },
        remove(name: string, options: any) {
          response.cookies.set({ name, value: '', ...options })
        },
      },
    }
  )

  const { data: { session } } = await supabase.auth.getSession()

  // Protect dashboard routes
  if (request.nextUrl.pathname.startsWith('/dashboard') && !session) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  return response
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico).*)'],
}
```

### **AI Integration Setup**

#### 1. OpenAI Service:
```typescript
// lib/ai/openai.ts
import OpenAI from 'openai'

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
})

export async function generateQuestionsFromPlay(playData: {
  name: string
  description: string
  formation: string
  tags: string[]
}) {
  const prompt = `
    Generate 3 football IQ questions based on this play:
    Name: ${playData.name}
    Description: ${playData.description}
    Formation: ${playData.formation}
    Tags: ${playData.tags.join(', ')}
    
    Return JSON array with: question_text, correct_answer, options (for multiple choice), difficulty (1-3)
  `

  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.7,
  })

  return JSON.parse(response.choices[0].message.content || '[]')
}

export async function generateSignalQuestions(signalData: {
  name: string
  meaning: string
  signal_type: string
}) {
  // Similar implementation for signal-based questions
}
```

### **File Upload Setup (UploadThing)**

#### 1. UploadThing Configuration:
```typescript
// app/api/uploadthing/core.ts
import { createUploadthing } from 'uploadthing/next'
import { createServerSupabaseClient } from '@/lib/auth/supabase'

const f = createUploadthing()

export const ourFileRouter = {
  signalVideo: f({ video: { maxFileSize: '32MB', maxFileCount: 1 } })
    .middleware(async ({ req }) => {
      const supabase = createServerSupabaseClient()
      const { data: { session } } = await supabase.auth.getSession()
      
      if (!session) throw new Error('Unauthorized')
      
      return { userId: session.user.id }
    })
    .onUploadComplete(async ({ metadata, file }) => {
      // Store file reference in database
      return { fileUrl: file.url }
    }),
    
  playDiagram: f({ image: { maxFileSize: '4MB', maxFileCount: 1 } })
    .middleware(async ({ req }) => {
      // Similar auth check
    })
    .onUploadComplete(async ({ metadata, file }) => {
      return { fileUrl: file.url }
    }),
}

export type OurFileRouter = typeof ourFileRouter
```

### **State Management (Zustand)**

```typescript
// store/useStore.ts
import { create } from 'zustand'
import { devtools } from 'zustand/middleware'

interface FootballIQStore {
  user: User | null
  team: Team | null
  plays: Play[]
  signals: Signal[]
  setUser: (user: User | null) => void
  setTeam: (team: Team | null) => void
  addPlay: (play: Play) => void
  addSignal: (signal: Signal) => void
}

export const useStore = create<FootballIQStore>()(
  devtools(
    (set) => ({
      user: null,
      team: null,
      plays: [],
      signals: [],
      setUser: (user) => set({ user }),
      setTeam: (team) => set({ team }),
      addPlay: (play) => set((state) => ({ plays: [...state.plays, play] })),
      addSignal: (signal) => set((state) => ({ signals: [...state.signals, signal] })),
    }),
    { name: 'footballiq-store' }
  )
)
```

---

## MVP Feature Implementation Order

### **Sprint 1: Foundation (Week 1-2)**
1. ✅ Setup Supabase project and database schema
2. ✅ Implement authentication (login/register)
3. ✅ Create basic dashboard layout
4. ✅ Setup file upload for videos/images

### **Sprint 2: Core Features (Week 3-4)**
1. ✅ Play management (CRUD operations)
2. ✅ Signal management with video upload
3. ✅ Basic AI question generation
4. ✅ Simple quiz interface

### **Sprint 3: Enhancement (Week 5-6)**
1. ✅ User progress tracking
2. ✅ Basic analytics dashboard
3. ✅ Team management (invite players)
4. ✅ Mobile responsiveness

### **Sprint 4: Polish (Week 7-8)**
1. ✅ Error handling and loading states
2. ✅ Performance optimization
3. ✅ Basic onboarding flow
4. ✅ Deployment and monitoring

---

## Cost Analysis (Monthly)

### **Development Tools (Free Tier)**
- Vercel: Free for MVP (Pro: $20/month when scaling)
- Supabase: Free up to 50MB database (Pro: $25/month)
- UploadThing: Free up to 2GB (Pro: $10/month)
- OpenAI API: ~$50-200/month (based on usage)
- Resend: Free up to 3K emails (Pro: $20/month)

**Total MVP Cost: ~$50-200/month**

### **Scaling Considerations**
- **100 teams**: ~$300-500/month
- **500 teams**: ~$800-1500/month
- **1000+ teams**: Custom enterprise solutions

---

## Performance Optimization Strategy

### **Next.js Optimizations**
- Server Components for data fetching
- Dynamic imports for heavy components
- Image optimization with next/image
- Edge runtime where possible

### **Database Optimizations**
- Proper indexing on frequently queried columns
- Connection pooling through Supabase
- Optimistic updates for better UX

### **Caching Strategy**
- Next.js built-in caching for static content
- Supabase real-time for live updates
- Client-side caching with React Query (if needed)

---

## Security Considerations

### **Data Protection**
- Row Level Security (RLS) in Supabase
- API route protection with middleware
- Input validation with Zod schemas
- Secure file upload with type checking

### **Authentication Security**
- JWT tokens handled by Supabase
- CSRF protection built into Next.js
- Environment variable security
- Rate limiting on API routes

---

## Monitoring & Analytics

### **Error Tracking**
```typescript
// Add to next.config.ts when ready
const withSentry = require('@sentry/nextjs')({
  org: 'your-org',
  project: 'footballiq',
})

module.exports = withSentry({
  // existing config
})
```

### **User Analytics**
```typescript
// Simple event tracking
export const trackEvent = (eventName: string, properties?: any) => {
  // Can integrate with Mixpanel, Amplitude, or custom solution later
  console.log('Event:', eventName, properties)
}
```

---

## Next Steps for Implementation

1. **Week 1**: Set up Supabase project and install core dependencies
2. **Week 1**: Implement authentication and basic database schema
3. **Week 2**: Build play and signal management interfaces
4. **Week 3**: Integrate OpenAI for question generation
5. **Week 4**: Create quiz interface and progress tracking
6. **Week 5**: Add team management and user roles
7. **Week 6**: Polish UI/UX and add analytics
8. **Week 7**: Testing, optimization, and deployment
9. **Week 8**: User feedback integration and iteration

This tech stack provides a solid foundation for rapid MVP development while maintaining the flexibility to scale. The combination of Next.js, Supabase, and carefully selected tools creates a modern, performant platform that can grow with your user base.
