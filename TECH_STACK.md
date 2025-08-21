
# FootballIQ: Complete Tech Stack Documentation

## Executive Summary
This document outlines the confirmed technology stack for FootballIQ MVP, optimized for rapid development and time-to-market. This stack prioritizes simplicity, managed services, and proven technologies to get your AI-powered football education platform to market in 4-6 weeks.

## Current Foundation Analysis
- ✅ **Next.js 15** with App Router
- ✅ **React 19** with latest features
- ✅ **TypeScript** for type safety
- ✅ **Tailwind CSS v4** for styling
- ✅ **shadcn/ui** component library
- ✅ **Lucide React** for icons

---

## Confirmed MVP Tech Stack

### **Frontend Architecture**
```
Next.js 15 (App Router) + React 19 + TypeScript
├── UI Framework: shadcn/ui components
├── Styling: Tailwind CSS v4
├── Icons: Lucide React
├── Forms: React Hook Form + Zod validation
├── State Management: Zustand (lightweight, simple)
├── HTTP Client: Built-in fetch with Server Actions
└── Real-time: Supabase Real-time subscriptions
```

### **Backend Architecture**
```
Next.js API Routes + Server Actions (Full-Stack)
├── Database: Supabase PostgreSQL (managed)
├── Authentication: Supabase Auth (integrated)
├── File Storage: Supabase Storage + CDN
├── Real-time: Supabase Real-time subscriptions
├── AI Integration: OpenAI GPT-4 API
└── Row Level Security: Built-in Supabase RLS
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
├── Database: Supabase Cloud (managed PostgreSQL)
├── Authentication: Supabase Auth (managed)
├── File Storage: Supabase Storage + CDN
├── Real-time: Supabase Real-time engine
├── Monitoring: Vercel Analytics
├── Domain: Custom domain through Vercel
└── Environment: Vercel Edge Runtime + Serverless Functions
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
    "zod": "^3.22.0",
    "react-hook-form": "^7.47.0",
    "@hookform/resolvers": "^3.3.0",
    "zustand": "^4.4.0",
    "openai": "^4.20.0",
    "react-dropzone": "^14.2.0"
  },
  "devDependencies": {
    "dotenv": "^16.3.0"
  }
}
```

#### Removed Dependencies (No longer needed):
- ❌ `drizzle-orm` & `drizzle-kit` (Using Supabase client instead)
- ❌ `postgres` (Supabase handles connections)
- ❌ `uploadthing` & `@uploadthing/react` (Using Supabase Storage)
- ❌ `resend` (Email can be added later via Supabase Edge Functions)

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
│   │   ├── teams/
│   │   ├── plays/
│   │   ├── signals/
│   │   ├── learning/
│   │   ├── analytics/
│   │   ├── ai/
│   │   └── files/
│   └── globals.css
├── components/
│   ├── ui/ (shadcn components)
│   ├── auth/
│   ├── playbook/
│   ├── signals/
│   ├── quiz/
│   └── dashboard/
├── lib/
│   ├── supabase/
│   │   ├── client.ts
│   │   ├── server.ts
│   │   └── types.ts
│   ├── auth/
│   ├── ai/
│   └── utils.ts
├── store/
│   └── useStore.ts (Zustand)
└── types/
    └── database.ts (Supabase generated types)
```

### **Phase 3: Environment Configuration**

```env
# .env.local
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# OpenAI API
OPENAI_API_KEY=your_openai_api_key

# App Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_SITE_NAME=FootballIQ

# Optional: Rate Limiting
RATE_LIMIT_REQUESTS_PER_MINUTE=60
```

---

## Implementation Guide

### **Supabase Setup & Configuration**

#### 1. Supabase Project Setup:
1. Create new project at [supabase.com](https://supabase.com)
2. Copy project URL and anon key to `.env.local`
3. Enable Row Level Security (RLS) on all tables
4. Configure Authentication providers (email/password + Google OAuth)

#### 2. Database Schema (SQL to run in Supabase SQL Editor):
```sql
-- Enable RLS on auth.users (if not already enabled)
ALTER TABLE auth.users ENABLE ROW LEVEL SECURITY;

-- Profiles table (extends Supabase auth.users)
CREATE TABLE profiles (
  id UUID REFERENCES auth.users(id) PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  role TEXT CHECK (role IN ('coach', 'player', 'admin')) NOT NULL,
  team_id UUID REFERENCES teams(id),
  first_name TEXT,
  last_name TEXT,
  position TEXT, -- for players
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Teams table
CREATE TABLE teams (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT NOT NULL,
  coach_id UUID REFERENCES profiles(id) NOT NULL,
  subscription_tier TEXT DEFAULT 'starter',
  invite_code TEXT UNIQUE DEFAULT encode(gen_random_bytes(6), 'base64'),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Plays table
CREATE TABLE plays (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  name TEXT NOT NULL,
  description TEXT,
  tags TEXT[],
  diagram_url TEXT, -- Supabase Storage URL
  formation TEXT,
  personnel TEXT,
  created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Signals table
CREATE TABLE signals (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  name TEXT NOT NULL,
  video_url TEXT NOT NULL, -- Supabase Storage URL
  meaning TEXT NOT NULL,
  signal_type TEXT, -- formation, route, protection, etc.
  created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- AI Generated Questions
CREATE TABLE questions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  play_id UUID REFERENCES plays(id),
  signal_id UUID REFERENCES signals(id),
  question_text TEXT NOT NULL,
  question_type TEXT CHECK (question_type IN ('multiple_choice', 'signal_recognition', 'scenario', 'true_false')) NOT NULL,
  correct_answer TEXT NOT NULL,
  options JSONB, -- for multiple choice: ["option1", "option2", "option3", "option4"]
  difficulty INTEGER DEFAULT 1 CHECK (difficulty BETWEEN 1 AND 3),
  ai_generated BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- User Progress Tracking
CREATE TABLE user_progress (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES profiles(id) NOT NULL,
  question_id UUID REFERENCES questions(id) NOT NULL,
  is_correct BOOLEAN NOT NULL,
  time_taken INTEGER, -- seconds
  confidence_level INTEGER CHECK (confidence_level BETWEEN 1 AND 5),
  attempted_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Learning Sessions (for tracking study sessions)
CREATE TABLE learning_sessions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES profiles(id) NOT NULL,
  team_id UUID REFERENCES teams(id) NOT NULL,
  session_type TEXT CHECK (session_type IN ('signal_practice', 'play_study', 'quiz', 'mixed')) NOT NULL,
  questions_attempted INTEGER DEFAULT 0,
  questions_correct INTEGER DEFAULT 0,
  total_time INTEGER, -- seconds
  started_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE
);

-- Team Philosophies (coach's custom terminology and systems)
CREATE TABLE team_philosophies (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  team_id UUID REFERENCES teams(id) NOT NULL,
  philosophy_type TEXT CHECK (philosophy_type IN ('offensive_system', 'defensive_system', 'terminology', 'general')) NOT NULL,
  title TEXT NOT NULL,
  description TEXT,
  content JSONB, -- flexible structure for different types of philosophy data
  created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3. Row Level Security Policies:
```sql
-- Enable RLS on all tables
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE teams ENABLE ROW LEVEL SECURITY;
ALTER TABLE plays ENABLE ROW LEVEL SECURITY;
ALTER TABLE signals ENABLE ROW LEVEL SECURITY;
ALTER TABLE questions ENABLE ROW LEVEL SECURITY;
ALTER TABLE user_progress ENABLE ROW LEVEL SECURITY;
ALTER TABLE learning_sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE team_philosophies ENABLE ROW LEVEL SECURITY;

-- Profiles policies
CREATE POLICY "Users can view own profile" ON profiles
  FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON profiles
  FOR UPDATE USING (auth.uid() = id);

-- Teams policies
CREATE POLICY "Team members can view team" ON teams
  FOR SELECT USING (
    id IN (SELECT team_id FROM profiles WHERE id = auth.uid())
  );

-- Plays policies  
CREATE POLICY "Team members can view team plays" ON plays
  FOR SELECT USING (
    team_id IN (SELECT team_id FROM profiles WHERE id = auth.uid())
  );

CREATE POLICY "Coaches can manage team plays" ON plays
  FOR ALL USING (
    team_id IN (
      SELECT team_id FROM profiles 
      WHERE id = auth.uid() AND role = 'coach'
    )
  );

-- Signals policies
CREATE POLICY "Team members can view team signals" ON signals
  FOR SELECT USING (
    team_id IN (SELECT team_id FROM profiles WHERE id = auth.uid())
  );

CREATE POLICY "Coaches can manage team signals" ON signals
  FOR ALL USING (
    team_id IN (
      SELECT team_id FROM profiles 
      WHERE id = auth.uid() AND role = 'coach'
    )
  );

-- Questions policies
CREATE POLICY "Team members can view team questions" ON questions
  FOR SELECT USING (
    team_id IN (SELECT team_id FROM profiles WHERE id = auth.uid())
  );

-- User progress policies
CREATE POLICY "Users can view own progress" ON user_progress
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own progress" ON user_progress
  FOR INSERT WITH CHECK (auth.uid() = user_id);

-- Learning sessions policies
CREATE POLICY "Users can manage own sessions" ON learning_sessions
  FOR ALL USING (auth.uid() = user_id);
```

### **Supabase Client Setup**

#### 1. Client Configuration:
```typescript
// lib/supabase/client.ts (Browser)
import { createBrowserClient } from '@supabase/ssr'

export const createClient = () => {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}

// lib/supabase/server.ts (Server Components & API Routes)
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export const createServerClient = () => {
  const cookieStore = cookies()
  
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value
        },
        set(name: string, value: string, options: any) {
          cookieStore.set({ name, value, ...options })
        },
        remove(name: string, options: any) {
          cookieStore.set({ name, value: '', ...options })
        },
      },
    }
  )
}

// lib/supabase/types.ts (Database Types)
export type Database = {
  public: {
    Tables: {
      profiles: {
        Row: {
          id: string
          email: string
          role: 'coach' | 'player' | 'admin'
          team_id: string | null
          first_name: string | null
          last_name: string | null
          position: string | null
          created_at: string
          updated_at: string
        }
        Insert: {
          id: string
          email: string
          role: 'coach' | 'player' | 'admin'
          team_id?: string | null
          first_name?: string | null
          last_name?: string | null
          position?: string | null
          created_at?: string
          updated_at?: string
        }
        Update: {
          id?: string
          email?: string
          role?: 'coach' | 'player' | 'admin'
          team_id?: string | null
          first_name?: string | null
          last_name?: string | null
          position?: string | null
          created_at?: string
          updated_at?: string
        }
      }
      // Add other table types here...
    }
  }
}
```

#### 2. Authentication Middleware:
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

  // Protect API routes
  if (request.nextUrl.pathname.startsWith('/api/') && !session) {
    const excludedPaths = ['/api/auth/', '/api/health']
    const isExcluded = excludedPaths.some(path => 
      request.nextUrl.pathname.startsWith(path)
    )
    
    if (!isExcluded) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
    }
  }

  return response
}

export const config = {
  matcher: [
    '/((?!_next/static|_next/image|favicon.ico|public).*)',
    '/api/(.*)'
  ],
}
```

### **OpenAI Integration**

#### 1. AI Service Configuration:
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
  teamPhilosophy?: string
}) {
  const prompt = `
    Generate 3 football IQ questions for this play:
    
    Play: ${playData.name}
    Description: ${playData.description}
    Formation: ${playData.formation}
    Tags: ${playData.tags.join(', ')}
    ${playData.teamPhilosophy ? `Team Philosophy: ${playData.teamPhilosophy}` : ''}
    
    Return a JSON array with exactly this structure:
    [
      {
        "question_text": "Clear, specific question about the play",
        "question_type": "multiple_choice",
        "correct_answer": "The correct answer",
        "options": ["Option 1", "Option 2", "Option 3", "Option 4"],
        "difficulty": 1
      }
    ]
    
    Make questions practical and game-relevant. Difficulty: 1=basic, 2=intermediate, 3=advanced.
  `

  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.7,
    max_tokens: 1000,
  })

  try {
    return JSON.parse(response.choices[0].message.content || '[]')
  } catch (error) {
    console.error('Failed to parse AI response:', error)
    return []
  }
}

export async function generateSignalQuestions(signalData: {
  name: string
  meaning: string
  signal_type: string
  teamPhilosophy?: string
}) {
  const prompt = `
    Generate 2 recognition questions for this football signal:
    
    Signal: ${signalData.name}
    Meaning: ${signalData.meaning}
    Type: ${signalData.signal_type}
    ${signalData.teamPhilosophy ? `Team Philosophy: ${signalData.teamPhilosophy}` : ''}
    
    Return a JSON array with this structure:
    [
      {
        "question_text": "When you see this signal, what should you do?",
        "question_type": "signal_recognition",
        "correct_answer": "The correct response",
        "options": ["Option 1", "Option 2", "Option 3", "Option 4"],
        "difficulty": 1
      }
    ]
  `

  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.7,
    max_tokens: 800,
  })

  try {
    return JSON.parse(response.choices[0].message.content || '[]')
  } catch (error) {
    console.error('Failed to parse AI response:', error)
    return []
  }
}

export async function generateScenarioQuestions(teamData: {
  plays: Array<{ name: string; description: string; formation: string }>
  teamPhilosophy?: string
}) {
  const prompt = `
    Create game scenario questions based on this team's playbook:
    
    Available Plays: ${teamData.plays.map(p => `${p.name} (${p.formation})`).join(', ')}
    ${teamData.teamPhilosophy ? `Team Philosophy: ${teamData.teamPhilosophy}` : ''}
    
    Generate 3 scenario questions like "3rd and 7, red zone, defense shows blitz - which play?"
    
    Return JSON array with same structure as other questions.
  `

  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.8,
    max_tokens: 1200,
  })

  try {
    return JSON.parse(response.choices[0].message.content || '[]')
  } catch (error) {
    console.error('Failed to parse AI response:', error)
    return []
  }
}
```

### **Supabase Storage Setup**

#### 1. Storage Configuration:
```typescript
// lib/storage/supabase-storage.ts
import { createClient } from '@/lib/supabase/client'

export async function uploadSignalVideo(
  file: File, 
  teamId: string, 
  signalName: string
): Promise<string | null> {
  const supabase = createClient()
  
  // Create unique file name
  const fileExt = file.name.split('.').pop()
  const fileName = `${teamId}/${signalName}-${Date.now()}.${fileExt}`
  
  const { data, error } = await supabase.storage
    .from('signal-videos')
    .upload(fileName, file, {
      cacheControl: '3600',
      upsert: false
    })

  if (error) {
    console.error('Upload error:', error)
    return null
  }

  // Get public URL
  const { data: { publicUrl } } = supabase.storage
    .from('signal-videos')
    .getPublicUrl(data.path)

  return publicUrl
}

export async function uploadPlayDiagram(
  file: File, 
  teamId: string, 
  playName: string
): Promise<string | null> {
  const supabase = createClient()
  
  const fileExt = file.name.split('.').pop()
  const fileName = `${teamId}/${playName}-${Date.now()}.${fileExt}`
  
  const { data, error } = await supabase.storage
    .from('play-diagrams')
    .upload(fileName, file, {
      cacheControl: '3600',
      upsert: false
    })

  if (error) {
    console.error('Upload error:', error)
    return null
  }

  const { data: { publicUrl } } = supabase.storage
    .from('play-diagrams')
    .getPublicUrl(data.path)

  return publicUrl
}

export async function deleteFile(bucket: string, path: string): Promise<boolean> {
  const supabase = createClient()
  
  const { error } = await supabase.storage
    .from(bucket)
    .remove([path])

  return !error
}
```

#### 2. Storage Buckets Setup (run in Supabase SQL Editor):
```sql
-- Create storage buckets
INSERT INTO storage.buckets (id, name, public) VALUES 
('signal-videos', 'signal-videos', true),
('play-diagrams', 'play-diagrams', true),
('generated-content', 'generated-content', true);

-- Storage policies for signal videos
CREATE POLICY "Team members can view signal videos" ON storage.objects
  FOR SELECT USING (
    bucket_id = 'signal-videos' AND
    (storage.foldername(name))[1] IN (
      SELECT team_id::text FROM profiles WHERE id = auth.uid()
    )
  );

CREATE POLICY "Coaches can upload signal videos" ON storage.objects
  FOR INSERT WITH CHECK (
    bucket_id = 'signal-videos' AND
    (storage.foldername(name))[1] IN (
      SELECT team_id::text FROM profiles 
      WHERE id = auth.uid() AND role = 'coach'
    )
  );

-- Similar policies for play diagrams
CREATE POLICY "Team members can view play diagrams" ON storage.objects
  FOR SELECT USING (
    bucket_id = 'play-diagrams' AND
    (storage.foldername(name))[1] IN (
      SELECT team_id::text FROM profiles WHERE id = auth.uid()
    )
  );

CREATE POLICY "Coaches can upload play diagrams" ON storage.objects
  FOR INSERT WITH CHECK (
    bucket_id = 'play-diagrams' AND
    (storage.foldername(name))[1] IN (
      SELECT team_id::text FROM profiles 
      WHERE id = auth.uid() AND role = 'coach'
    )
  );
```

### **Zustand State Management**

```typescript
// store/useStore.ts
import { create } from 'zustand'
import { devtools, persist } from 'zustand/middleware'
import type { Database } from '@/lib/supabase/types'

type Profile = Database['public']['Tables']['profiles']['Row']
type Team = Database['public']['Tables']['teams']['Row']
type Play = Database['public']['Tables']['plays']['Row']
type Signal = Database['public']['Tables']['signals']['Row']
type Question = Database['public']['Tables']['questions']['Row']

interface AuthState {
  user: Profile | null
  team: Team | null
  isLoading: boolean
  setUser: (user: Profile | null) => void
  setTeam: (team: Team | null) => void
  setLoading: (loading: boolean) => void
  clearAuth: () => void
}

interface ContentState {
  plays: Play[]
  signals: Signal[]
  questions: Question[]
  setPlays: (plays: Play[]) => void
  setSignals: (signals: Signal[]) => void
  setQuestions: (questions: Question[]) => void
  addPlay: (play: Play) => void
  addSignal: (signal: Signal) => void
  updatePlay: (id: string, updates: Partial<Play>) => void
  updateSignal: (id: string, updates: Partial<Signal>) => void
  removePlay: (id: string) => void
  removeSignal: (id: string) => void
}

interface UIState {
  currentQuiz: Question | null
  quizProgress: {
    currentQuestion: number
    totalQuestions: number
    correctAnswers: number
  }
  sidebarOpen: boolean
  setCurrentQuiz: (quiz: Question | null) => void
  setQuizProgress: (progress: UIState['quizProgress']) => void
  setSidebarOpen: (open: boolean) => void
  resetQuizProgress: () => void
}

// Auth Store (persisted)
export const useAuthStore = create<AuthState>()(
  devtools(
    persist(
      (set) => ({
        user: null,
        team: null,
        isLoading: false,
        setUser: (user) => set({ user }),
        setTeam: (team) => set({ team }),
        setLoading: (isLoading) => set({ isLoading }),
        clearAuth: () => set({ user: null, team: null }),
      }),
      {
        name: 'footballiq-auth',
        partialize: (state) => ({ user: state.user, team: state.team }),
      }
    ),
    { name: 'auth-store' }
  )
)

// Content Store (not persisted - fetched fresh)
export const useContentStore = create<ContentState>()(
  devtools(
    (set, get) => ({
      plays: [],
      signals: [],
      questions: [],
      setPlays: (plays) => set({ plays }),
      setSignals: (signals) => set({ signals }),
      setQuestions: (questions) => set({ questions }),
      addPlay: (play) => set((state) => ({ plays: [...state.plays, play] })),
      addSignal: (signal) => set((state) => ({ signals: [...state.signals, signal] })),
      updatePlay: (id, updates) => 
        set((state) => ({
          plays: state.plays.map((play) => 
            play.id === id ? { ...play, ...updates } : play
          ),
        })),
      updateSignal: (id, updates) =>
        set((state) => ({
          signals: state.signals.map((signal) =>
            signal.id === id ? { ...signal, ...updates } : signal
          ),
        })),
      removePlay: (id) =>
        set((state) => ({
          plays: state.plays.filter((play) => play.id !== id),
        })),
      removeSignal: (id) =>
        set((state) => ({
          signals: state.signals.filter((signal) => signal.id !== id),
        })),
    }),
    { name: 'content-store' }
  )
)

// UI Store (not persisted)
export const useUIStore = create<UIState>()(
  devtools(
    (set) => ({
      currentQuiz: null,
      quizProgress: {
        currentQuestion: 0,
        totalQuestions: 0,
        correctAnswers: 0,
      },
      sidebarOpen: false,
      setCurrentQuiz: (currentQuiz) => set({ currentQuiz }),
      setQuizProgress: (quizProgress) => set({ quizProgress }),
      setSidebarOpen: (sidebarOpen) => set({ sidebarOpen }),
      resetQuizProgress: () =>
        set({
          quizProgress: {
            currentQuestion: 0,
            totalQuestions: 0,
            correctAnswers: 0,
          },
        }),
    }),
    { name: 'ui-store' }
  )
)

// Convenience hooks
export const useAuth = () => useAuthStore()
export const useContent = () => useContentStore()
export const useUI = () => useUIStore()
```

---

## MVP Implementation Timeline

### **Week 1: Foundation Setup**
- [ ] **Supabase Project Setup**
  - Create Supabase project
  - Configure authentication (email + Google OAuth)
  - Run database schema SQL
  - Set up Row Level Security policies
  - Create storage buckets

- [ ] **Next.js Configuration**
  - Install dependencies (`@supabase/supabase-js`, `@supabase/ssr`, `zustand`, `openai`)
  - Set up Supabase client configuration
  - Configure middleware for auth
  - Set up Zustand stores

- [ ] **Basic Authentication**
  - Login/register pages with shadcn/ui components
  - Auth state management
  - Protected routes

### **Week 2: Core Data Management**
- [ ] **API Routes Foundation**
  - `/api/auth/` routes for auth callbacks
  - `/api/teams/` routes for team CRUD
  - `/api/plays/` routes for play management
  - `/api/signals/` routes for signal management

- [ ] **File Upload System**
  - Supabase Storage integration
  - Video upload for signals
  - Image upload for play diagrams
  - File management utilities

- [ ] **Basic Dashboard UI**
  - Coach dashboard layout
  - Player interface layout
  - Navigation and routing

### **Week 3: Content Creation Features**
- [ ] **Play Management**
  - Create/edit plays with diagrams
  - Tag and categorize plays
  - Formation and personnel data

- [ ] **Signal Management**
  - Record signal videos
  - Define signal meanings
  - Signal combinations

- [ ] **Team Management**
  - Invite players via email/code
  - Role-based permissions
  - Team settings

### **Week 4: AI Integration & Quizzes**
- [ ] **OpenAI Integration**
  - Question generation from plays
  - Signal recognition questions
  - Scenario-based questions

- [ ] **Quiz Interface**
  - Multiple choice questions
  - Signal recognition quizzes
  - Progress tracking
  - Real-time feedback

- [ ] **Analytics Dashboard**
  - Individual player progress
  - Team performance metrics
  - Learning analytics

### **Week 5-6: Polish & Enhancement**
- [ ] **Real-time Features**
  - Live progress updates
  - Real-time coach dashboard
  - Supabase real-time subscriptions

- [ ] **Mobile Optimization**
  - Responsive design improvements
  - Touch-friendly interactions
  - Mobile-specific features

- [ ] **Performance & UX**
  - Loading states and error handling
  - Optimistic updates
  - Caching and performance optimization

---

## Cost Analysis (Monthly)

### **MVP Phase (0-100 teams)**
- **Vercel Hobby**: Free (then $20/month for Pro)
- **Supabase Free Tier**: 500MB database, 1GB file storage, 2GB bandwidth
- **OpenAI API**: ~$30-100/month (estimated based on question generation)
- **Domain**: ~$12/year ($1/month)

**Total MVP Cost: ~$30-100/month**

### **Growth Phase (100-500 teams)**
- **Vercel Pro**: $20/month per member
- **Supabase Pro**: $25/month (100GB database, 100GB file storage, 200GB bandwidth)
- **OpenAI API**: ~$150-400/month (increased usage)
- **Custom Domain & Email**: ~$10/month

**Total Growth Cost: ~$200-450/month**

### **Scale Phase (500+ teams)**
- **Vercel Team**: $20/month per member
- **Supabase Team**: $599/month (unlimited everything, priority support)
- **OpenAI API**: ~$500-1500/month (high volume pricing)
- **Additional Services**: ~$100/month (monitoring, analytics, etc.)

**Total Scale Cost: ~$1,200-2,200/month**

### **Cost Per Team Estimates**
- **MVP**: $0.30-1.00 per team per month
- **Growth**: $0.40-0.90 per team per month  
- **Scale**: $1.00-2.00 per team per month

### **Revenue Model Alignment**
With pricing at $500-2,500/year per team, costs remain well under 10% of revenue even at scale.

---

## Performance Optimization Strategy

### **Next.js Optimizations**
- **Server Components**: Use for data fetching from Supabase
- **Dynamic Imports**: Lazy load quiz components and heavy UI elements
- **Image Optimization**: Use `next/image` for play diagrams and profile pics
- **Edge Runtime**: Deploy API routes to edge where possible
- **Static Generation**: Pre-render marketing pages and documentation

### **Supabase Optimizations**
- **Database Indexing**: Index frequently queried columns (team_id, user_id, created_at)
- **Row Level Security**: Efficient policies to minimize query overhead
- **Real-time Subscriptions**: Use sparingly, only for critical live updates
- **Connection Pooling**: Automatic through Supabase's infrastructure

### **Frontend Performance**
- **Zustand State**: Lightweight state management reduces re-renders
- **Optimistic Updates**: Update UI immediately, sync with backend
- **Virtual Scrolling**: For large lists of plays/signals/questions
- **Code Splitting**: Route-based splitting with Next.js automatic optimization

### **AI & External Services**
- **OpenAI Rate Limiting**: Implement proper rate limiting and queuing
- **Caching AI Responses**: Cache generated questions to reduce API calls
- **Background Processing**: Generate questions asynchronously when possible

---

## Security Considerations

### **Authentication & Authorization**
- **Supabase Auth**: Industry-standard JWT tokens with automatic rotation
- **Row Level Security**: Database-level access control for all data
- **Role-Based Access**: Coach vs Player permissions enforced at API level
- **Session Management**: Automatic token refresh and secure cookie handling

### **Data Protection**
- **Input Validation**: Zod schemas for all API inputs and forms
- **SQL Injection**: Protected via Supabase's parameterized queries
- **File Upload Security**: MIME type validation, file size limits, virus scanning readiness
- **Environment Variables**: Sensitive data never exposed to client

### **API Security**
- **Rate Limiting**: Protect against abuse, especially AI endpoints
- **CORS Configuration**: Restrict origins based on environment
- **Request Validation**: Validate all inputs before processing
- **Error Handling**: Don't expose sensitive information in error responses

### **Content Security**
- **Team Isolation**: All data scoped to teams via RLS policies
- **File Access Control**: Storage policies prevent unauthorized access
- **Data Encryption**: All data encrypted at rest and in transit
- **Audit Logging**: Track important actions for security monitoring

---

## Monitoring & Analytics

### **Built-in Monitoring**
- **Vercel Analytics**: Built-in performance and user analytics
- **Supabase Dashboard**: Database performance, query analysis, real-time metrics
- **OpenAI Usage Tracking**: Monitor API costs and usage patterns
- **Next.js Built-in**: Automatic performance monitoring and optimization suggestions

### **Application Monitoring**
```typescript
// lib/analytics/tracking.ts
export const trackEvent = (eventName: string, properties?: any) => {
  // For MVP: Simple console logging
  if (process.env.NODE_ENV === 'development') {
    console.log('📊 Event:', eventName, properties)
  }
  
  // Future: Add Mixpanel, Amplitude, or PostHog
  // analytics.track(eventName, properties)
}

// Key events to track
export const EVENTS = {
  // User actions
  USER_SIGNED_UP: 'User Signed Up',
  USER_JOINED_TEAM: 'User Joined Team',
  
  // Content creation
  PLAY_CREATED: 'Play Created',
  SIGNAL_CREATED: 'Signal Created',
  QUESTIONS_GENERATED: 'Questions Generated AI',
  
  // Learning activities
  QUIZ_STARTED: 'Quiz Started',
  QUIZ_COMPLETED: 'Quiz Completed',
  SIGNAL_PRACTICED: 'Signal Practice Session',
  
  // Coach activities
  TEAM_CREATED: 'Team Created',
  PLAYER_INVITED: 'Player Invited',
  ANALYTICS_VIEWED: 'Analytics Dashboard Viewed',
} as const
```

### **Performance Monitoring**
```typescript
// lib/monitoring/performance.ts
export const measurePerformance = (label: string, fn: () => Promise<any>) => {
  return async (...args: any[]) => {
    const start = performance.now()
    try {
      const result = await fn.apply(this, args)
      const duration = performance.now() - start
      
      if (duration > 1000) {
        console.warn(`⚠️ Slow operation: ${label} took ${duration.toFixed(2)}ms`)
      }
      
      return result
    } catch (error) {
      const duration = performance.now() - start
      console.error(`❌ Failed operation: ${label} failed after ${duration.toFixed(2)}ms`, error)
      throw error
    }
  }
}
```

---

## Next Steps for Implementation

### **Immediate Actions (This Week)**
1. **Create Supabase Project**
   - Sign up at [supabase.com](https://supabase.com)
   - Create new project named "FootballIQ"
   - Save project URL and anon key to `.env.local`

2. **Install Dependencies**
   ```bash
   npm install @supabase/supabase-js @supabase/ssr zustand openai react-hook-form @hookform/resolvers zod react-dropzone
   ```

3. **Database Setup**
   - Run the database schema SQL in Supabase SQL Editor
   - Set up Row Level Security policies
   - Create storage buckets for videos and diagrams

### **Week 1-2: Foundation**
- Set up Supabase client configuration
- Implement authentication flows
- Create basic dashboard layouts
- Set up API routes structure

### **Week 3-4: Core Features**
- Build play and signal management
- Integrate OpenAI for question generation
- Develop quiz interface
- Implement progress tracking

### **Week 5-6: Polish & Deploy**
- Add real-time features
- Optimize performance
- Deploy to Vercel
- Test with pilot users

---

## Architecture Alignment Summary

✅ **Your Confirmed Architecture Perfectly Aligns With:**
- **Rapid MVP Timeline**: 4-6 weeks to market
- **Cost-Effective Scaling**: $30-100/month initially
- **Modern Tech Stack**: Next.js 15 + Supabase + OpenAI
- **Developer Experience**: Type-safe, intuitive development
- **Business Goals**: Focus on features, not infrastructure

✅ **Architecture Diagram Match**: The updated TECH_STACK.md now perfectly matches your `footballiq_backend_architecture.mermaid` diagram with:
- Next.js API Routes + Server Actions
- Supabase for database, auth, storage, and real-time
- Zustand for state management
- OpenAI for AI features
- Vercel for deployment

Your FootballIQ platform is ready for rapid development with this proven, scalable architecture! 🏈
