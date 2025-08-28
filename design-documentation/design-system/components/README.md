---
title: FootballIQ Component Library - shadcn/ui Integration
description: Complete UI component specifications with football-specific customizations
feature: component-library
last-updated: 2024-12-28
version: 1.0
related-files: 
  - ../style-guide.md
  - ../tokens/colors.md
  - ../tokens/typography.md
  - ../tokens/spacing.md
  - ../tokens/animations.md
dependencies:
  - shadcn/ui component library
  - Tailwind CSS v4
  - React 19
status: draft
---

# FootballIQ Component Library - shadcn/ui Integration

## Overview
The FootballIQ component library builds upon shadcn/ui's foundation with football-specific customizations optimized for mobile-first usage. Each component is designed for the unique contexts of football education: quick recognition during practice, clear hierarchy for complex playbook information, and touch-friendly interactions for teenage athletes.

## Table of Contents
1. [Library Architecture](#library-architecture)
2. [Base Components](#base-components)
3. [Form Components](#form-components)
4. [Navigation Components](#navigation-components)
5. [Data Display Components](#data-display-components)
6. [Feedback Components](#feedback-components)
7. [Football-Specific Components](#football-specific-components)
8. [Implementation Guidelines](#implementation-guidelines)

## Library Architecture

### shadcn/ui Foundation
FootballIQ leverages shadcn/ui's component architecture while applying our design system tokens for consistent branding and functionality.

**Integration Benefits:**
- **Copy-Paste Simplicity**: Components can be copied and customized as needed
- **TypeScript Native**: Full type safety and IntelliSense support
- **Tailwind Integration**: Seamless integration with our Tailwind configuration
- **Accessibility First**: Built-in WCAG compliance and keyboard navigation
- **Minimal Bundle Size**: Tree-shakable components with no runtime dependencies

### Component Hierarchy
```
Base Components (Primitive Building Blocks)
├── Button - All button variants and states
├── Input - Form input fields and validation
├── Card - Content containers and information display
└── Typography - Text elements and hierarchy

Form Components (Data Collection)
├── Form Field - Complete form field with validation
├── Select - Dropdown selection components
├── Checkbox/Radio - Selection controls
└── File Upload - File handling for playbooks/videos

Navigation Components (User Flow)
├── Navigation Menu - Primary and mobile navigation
├── Breadcrumbs - Hierarchical navigation
├── Tabs - Content organization
└── Pagination - Data navigation

Data Display Components (Information Presentation)
├── Table - Data tables for statistics and rosters
├── List - Organized content display
├── Badge - Status and category indicators
└── Avatar - User and player representation

Feedback Components (System Communication)
├── Alert - Important messages and notifications
├── Toast - Temporary feedback messages
├── Loading - Progress and processing indicators
└── Empty State - No content scenarios

Football-Specific Components (Domain-Specific)
├── Signal Card - Video signal display and recognition
├── Play Diagram - Interactive playbook visualizations
├── Player Position - Position-specific content display
├── Progress Ring - Circular progress for learning paths
└── Stats Display - Performance metrics and analytics
```

### Component Standards
Every component includes:
- **Multiple Variants**: Primary, secondary, ghost, and destructive options
- **All Interactive States**: Default, hover, active, focus, disabled, loading
- **Size Variations**: Small, medium, large for different contexts
- **Mobile Optimization**: Touch-friendly sizing and spacing
- **Dark Mode Support**: Consistent appearance across themes
- **Accessibility Compliance**: WCAG 2.1 AA standards with screen reader support

## Base Components

### Button Component

**Variants and Usage**
```tsx
// Primary button - main actions
<Button variant="default" size="default">
  Save Playbook
</Button>

// Secondary button - supporting actions  
<Button variant="secondary" size="default">
  Preview
</Button>

// Ghost button - subtle actions
<Button variant="ghost" size="sm">
  Cancel
</Button>

// Destructive button - dangerous actions
<Button variant="destructive" size="default">
  Delete Signal
</Button>
```

**Visual Specifications**

**Default (Primary) Button**
- **Background**: `var(--color-primary)` with hover to `var(--color-primary-dark)`
- **Text**: White (high contrast)
- **Height**: 44px (touch-friendly)
- **Padding**: `12px 24px` (comfortable internal spacing)
- **Border Radius**: `8px` (modern, approachable)
- **Font**: `Body, Semibold` for clear action communication
- **Shadow**: `0 2px 4px rgba(47, 107, 59, 0.1)` with hover enhancement

**Secondary Button**
- **Background**: Transparent with `2px solid var(--color-primary)` border
- **Text**: `var(--color-primary)` changing to white on hover
- **Hover State**: Background fills with `var(--color-primary)`
- **Same dimensions** as primary for consistent interface rhythm

**Size Variations**
```tsx
<Button size="sm">Small Action</Button>     // 36px height, 8px 16px padding
<Button size="default">Standard</Button>    // 44px height, 12px 24px padding  
<Button size="lg">Large Action</Button>     // 52px height, 16px 32px padding
```

**Interactive States**
```css
.btn-primary {
  /* Default state */
  background: var(--color-primary);
  color: white;
  transform: translateY(0);
  transition: all 150ms var(--timing-explosive);
}

.btn-primary:hover {
  background: var(--color-primary-dark);
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(47, 107, 59, 0.15);
}

.btn-primary:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(47, 107, 59, 0.2);
}

.btn-primary:focus-visible {
  outline: 0;
  box-shadow: 0 0 0 3px rgba(47, 107, 59, 0.4);
}

.btn-primary:disabled {
  background: var(--color-neutral-300);
  color: var(--color-neutral-500);
  transform: none;
  cursor: not-allowed;
}
```

### Card Component

**Card Structure and Hierarchy**
```tsx
<Card className="w-full">
  <CardHeader>
    <CardTitle>Signal Recognition Practice</CardTitle>
    <CardDescription>Review and practice team signals</CardDescription>
  </CardHeader>
  <CardContent>
    {/* Main content area */}
  </CardContent>
  <CardFooter>
    <Button variant="default">Start Practice</Button>
    <Button variant="ghost">View Details</Button>
  </CardFooter>
</Card>
```

**Visual Specifications**
- **Background**: White with subtle `var(--color-neutral-50)` tint
- **Border**: `1px solid var(--color-neutral-200)` for clean separation
- **Border Radius**: `12px` for modern, friendly appearance
- **Shadow**: `0 4px 8px rgba(0, 0, 0, 0.1)` for gentle elevation
- **Padding**: `24px` desktop, `20px` mobile
- **Hover Enhancement**: Shadow intensifies, slight scale (1.02)

**Card Variants**
```tsx
// Default card
<Card>Content</Card>

// Interactive card (clickable)
<Card className="cursor-pointer hover:shadow-lg transition-all duration-250">
  Content
</Card>

// Compact card for lists
<Card className="p-4">
  Compact Content
</Card>

// Featured card with accent
<Card className="border-primary bg-primary/5">
  Featured Content  
</Card>
```

### Input Component

**Input Field Specifications**
```tsx
<div className="form-group">
  <Label htmlFor="playbook-name">Playbook Name</Label>
  <Input 
    id="playbook-name"
    placeholder="Enter playbook name"
    className="mt-2"
  />
  <p className="text-sm text-neutral-600 mt-1">
    This will be visible to all players
  </p>
</div>
```

**Visual Design**
- **Height**: 48px for optimal touch targets
- **Border**: `2px solid var(--color-neutral-300)` with `var(--color-primary)` focus
- **Border Radius**: `6px` for subtle, professional appearance
- **Padding**: `12px 16px` for comfortable text entry
- **Typography**: `Body, Regular` for legible user input
- **Placeholder**: `var(--color-neutral-500)` for clear guidance

**Input States**
```css
.input {
  border: 2px solid var(--color-neutral-300);
  transition: all 200ms var(--timing-explosive);
}

.input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(47, 107, 59, 0.1);
  outline: 0;
}

.input:disabled {
  background: var(--color-neutral-100);
  border-color: var(--color-neutral-200);
  color: var(--color-neutral-500);
  cursor: not-allowed;
}

.input.error {
  border-color: var(--color-error);
  box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.1);
}
```

## Form Components

### Form Field Component

**Complete Form Field Structure**
```tsx
<FormField
  control={form.control}
  name="email"
  render={({ field, fieldState }) => (
    <FormItem>
      <FormLabel required>Email Address</FormLabel>
      <FormControl>
        <Input 
          {...field}
          type="email"
          placeholder="coach@school.edu"
        />
      </FormControl>
      <FormDescription>
        This email will be used for team notifications
      </FormDescription>
      <FormMessage />
    </FormItem>
  )}
/>
```

**Label System**
- **Typography**: `Label` style (14px, Semibold, uppercase, 0.05em letter-spacing)
- **Color**: `var(--color-neutral-800)` for strong identification
- **Spacing**: `8px` margin below label, `4px` above input
- **Required Indicator**: `*` in `var(--color-error)` for mandatory fields

### Select Component

**Dropdown Selection**
```tsx
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Select player position" />
  </SelectTrigger>
  <SelectContent>
    <SelectGroup>
      <SelectLabel>Offense</SelectLabel>
      <SelectItem value="qb">Quarterback</SelectItem>
      <SelectItem value="rb">Running Back</SelectItem>
      <SelectItem value="wr">Wide Receiver</SelectItem>
    </SelectGroup>
    <SelectGroup>
      <SelectLabel>Defense</SelectLabel>
      <SelectItem value="lb">Linebacker</SelectItem>
      <SelectItem value="db">Defensive Back</SelectItem>
    </SelectGroup>
  </SelectContent>
</Select>
```

**Mobile Optimization**
- **Touch Target**: 48px minimum height for comfortable selection
- **Dropdown Positioning**: Intelligent positioning to avoid viewport edges
- **Search Functionality**: Type-ahead search for long lists
- **Native Fallback**: Uses native select on mobile when appropriate

### File Upload Component

**Drag-and-Drop File Upload**
```tsx
<FileUpload
  accept={{
    'video/*': ['.mp4', '.mov', '.avi'],
    'application/pdf': ['.pdf'],
    'image/*': ['.png', '.jpg', '.jpeg']
  }}
  maxSize={100 * 1024 * 1024} // 100MB
  onUpload={handleFileUpload}
>
  <div className="text-center py-12">
    <Upload className="w-12 h-12 text-neutral-400 mx-auto mb-4" />
    <p className="text-lg font-medium mb-2">
      Drop playbook files here
    </p>
    <p className="text-sm text-neutral-600">
      Supports PDF, images, and videos up to 100MB
    </p>
    <Button variant="secondary" className="mt-4">
      Browse Files
    </Button>
  </div>
</FileUpload>
```

**Upload States**
- **Idle State**: Clear instructions and visual cues
- **Drag Active**: Visual feedback during drag operations
- **Uploading**: Progress bar with percentage and file name
- **Success**: Confirmation with file details and next actions
- **Error**: Clear error message with retry options

## Navigation Components

### Navigation Menu

**Primary Navigation Structure**
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Dashboard</NavigationMenuTrigger>
    </NavigationMenuItem>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Playbooks</NavigationMenuTrigger>
      <NavigationMenuContent>
        <div className="grid gap-3 p-6 w-[400px]">
          <NavigationMenuLink href="/playbooks/offense">
            Offensive Playbook
          </NavigationMenuLink>
          <NavigationMenuLink href="/playbooks/defense">
            Defensive Playbook
          </NavigationMenuLink>
        </div>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

**Mobile Navigation**
```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button variant="ghost" size="sm">
      <Menu className="h-6 w-6" />
      <span className="sr-only">Toggle navigation menu</span>
    </Button>
  </SheetTrigger>
  <SheetContent side="left" className="w-[300px]">
    <nav className="flex flex-col gap-4">
      <Link href="/dashboard" className="text-lg font-medium">
        Dashboard
      </Link>
      <Link href="/signals" className="text-lg font-medium">
        Signals
      </Link>
      <Link href="/playbooks" className="text-lg font-medium">
        Playbooks
      </Link>
    </nav>
  </SheetContent>
</Sheet>
```

### Breadcrumbs Component

**Hierarchical Navigation**
```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/dashboard">Dashboard</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/playbooks">Playbooks</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Offensive Plays</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Tabs Component

**Content Organization**
```tsx
<Tabs defaultValue="signals" className="w-full">
  <TabsList className="grid w-full grid-cols-3">
    <TabsTrigger value="signals">Signals</TabsTrigger>
    <TabsTrigger value="plays">Plays</TabsTrigger>
    <TabsTrigger value="progress">Progress</TabsTrigger>
  </TabsList>
  <TabsContent value="signals">
    <SignalPracticeContent />
  </TabsContent>
  <TabsContent value="plays">
    <PlaybookContent />
  </TabsContent>
  <TabsContent value="progress">
    <ProgressTrackingContent />
  </TabsContent>
</Tabs>
```

## Data Display Components

### Table Component

**Responsive Data Table**
```tsx
<Table>
  <TableCaption>Team roster and positions</TableCaption>
  <TableHeader>
    <TableRow>
      <TableHead>Player</TableHead>
      <TableHead>Position</TableHead>
      <TableHead>Progress</TableHead>
      <TableHead className="text-right">Actions</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell className="font-medium">John Smith</TableCell>
      <TableCell>Quarterback</TableCell>
      <TableCell>
        <div className="flex items-center gap-2">
          <Progress value={78} className="w-[100px]" />
          <span className="text-sm">78%</span>
        </div>
      </TableCell>
      <TableCell className="text-right">
        <Button variant="ghost" size="sm">View Details</Button>
      </TableCell>
    </TableRow>
  </TableBody>
</Table>
```

**Mobile Table Adaptation**
```tsx
// Mobile: Convert to card layout
<div className="md:hidden space-y-4">
  {players.map((player) => (
    <Card key={player.id} className="p-4">
      <div className="flex justify-between items-start mb-2">
        <div>
          <h3 className="font-medium">{player.name}</h3>
          <p className="text-sm text-neutral-600">{player.position}</p>
        </div>
        <Badge variant="outline">{player.progress}%</Badge>
      </div>
      <Progress value={player.progress} className="mb-3" />
      <Button variant="ghost" size="sm" className="w-full">
        View Details
      </Button>
    </Card>
  ))}
</div>
```

### Badge Component

**Status and Category Indicators**
```tsx
// Status badges
<Badge variant="default">Active</Badge>
<Badge variant="secondary">Completed</Badge>
<Badge variant="destructive">Needs Review</Badge>
<Badge variant="outline">Draft</Badge>

// Position badges with custom colors
<Badge className="bg-blue-100 text-blue-800">Quarterback</Badge>
<Badge className="bg-green-100 text-green-800">Running Back</Badge>
<Badge className="bg-orange-100 text-orange-800">Wide Receiver</Badge>
```

### Avatar Component

**Player and Coach Representation**
```tsx
<Avatar>
  <AvatarImage src="/players/john-smith.jpg" alt="John Smith" />
  <AvatarFallback>JS</AvatarFallback>
</Avatar>

// Avatar group for team display
<AvatarGroup max={4}>
  <Avatar>
    <AvatarImage src="/player1.jpg" />
    <AvatarFallback>JS</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarImage src="/player2.jpg" />
    <AvatarFallback>MD</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarImage src="/player3.jpg" />
    <AvatarFallback>TW</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarFallback>+2</AvatarFallback>
  </Avatar>
</AvatarGroup>
```

## Feedback Components

### Alert Component

**Important Messages and Notifications**
```tsx
<Alert>
  <AlertCircle className="h-4 w-4" />
  <AlertTitle>Signal Practice Required</AlertTitle>
  <AlertDescription>
    Your team needs to complete signal practice before the next game.
    <Button variant="link" className="px-0 h-auto">
      Start Practice Now
    </Button>
  </AlertDescription>
</Alert>

// Different alert variants
<Alert variant="destructive">
  <ExclamationTriangleIcon className="h-4 w-4" />
  <AlertTitle>Error</AlertTitle>
  <AlertDescription>
    Failed to upload playbook. Please try again.
  </AlertDescription>
</Alert>
```

### Toast Component

**Temporary Feedback Messages**
```tsx
// Success toast
toast({
  title: "Signal saved successfully",
  description: "Players can now practice this signal",
  action: <Button variant="outline" size="sm">View Signal</Button>
});

// Error toast
toast({
  variant: "destructive",
  title: "Upload failed",
  description: "The file format is not supported",
});

// Loading toast with progress
toast({
  title: "Processing video...",
  description: "This may take a few minutes",
  action: <Progress value={progress} className="w-[100px]" />
});
```

### Loading Component

**Progress and Processing Indicators**
```tsx
// Spinner for general loading
<div className="flex items-center justify-center p-8">
  <Loader2 className="h-8 w-8 animate-spin text-primary" />
  <span className="ml-2">Loading playbook...</span>
</div>

// Progress bar for file uploads
<div className="space-y-2">
  <div className="flex justify-between text-sm">
    <span>Uploading video...</span>
    <span>{progress}%</span>
  </div>
  <Progress value={progress} className="h-2" />
</div>

// Skeleton loading for content
<div className="space-y-4">
  <Skeleton className="h-4 w-[250px]" />
  <Skeleton className="h-4 w-[200px]" />
  <Skeleton className="h-4 w-[300px]" />
</div>
```

## Football-Specific Components

### Signal Card Component

**Video Signal Display and Recognition**
```tsx
<SignalCard 
  signal={signal}
  onPractice={() => startPractice(signal.id)}
  showProgress={true}
>
  <div className="aspect-video relative overflow-hidden rounded-lg">
    <video 
      src={signal.videoUrl}
      poster={signal.thumbnailUrl}
      controls
      className="w-full h-full object-cover"
    />
    <div className="absolute top-2 right-2">
      <Badge variant="secondary">{signal.category}</Badge>
    </div>
  </div>
  <div className="p-4">
    <h3 className="font-semibold mb-2">{signal.name}</h3>
    <p className="text-sm text-neutral-600 mb-4">
      {signal.description}
    </p>
    <div className="flex items-center justify-between">
      <div className="flex items-center gap-2">
        <Progress value={signal.masteryLevel} className="w-[100px]" />
        <span className="text-sm">{signal.masteryLevel}% mastered</span>
      </div>
      <Button size="sm">Practice</Button>
    </div>
  </div>
</SignalCard>
```

### Play Diagram Component

**Interactive Playbook Visualizations**
```tsx
<PlayDiagram
  play={play}
  playerPosition="QB"
  interactive={true}
  onPlayerClick={handlePlayerClick}
>
  <div className="relative bg-green-50 border-2 border-green-200 rounded-lg p-4">
    {/* Field background with yard lines */}
    <FieldBackground />
    
    {/* Player positions */}
    {play.positions.map((position) => (
      <PlayerDot
        key={position.id}
        position={position}
        highlighted={position.role === playerPosition}
        onClick={() => onPlayerClick(position)}
      />
    ))}
    
    {/* Route lines */}
    {play.routes.map((route) => (
      <RouteLine
        key={route.id}
        path={route.path}
        color={route.color}
        animated={interactive}
      />
    ))}
    
    {/* Play description overlay */}
    <div className="absolute bottom-4 left-4 bg-white/90 rounded p-2">
      <h4 className="font-semibold text-sm">{play.name}</h4>
      <p className="text-xs text-neutral-600">{play.description}</p>
    </div>
  </div>
</PlayDiagram>
```

### Progress Ring Component

**Circular Progress for Learning Paths**
```tsx
<ProgressRing
  value={78}
  size="large"
  showPercentage={true}
  color="primary"
  className="mb-4"
>
  <div className="text-center">
    <div className="text-2xl font-bold text-primary">78%</div>
    <div className="text-sm text-neutral-600">Complete</div>
  </div>
</ProgressRing>
```

### Stats Display Component

**Performance Metrics and Analytics**
```tsx
<StatsDisplay>
  <StatCard
    title="Signals Mastered"
    value="24"
    change="+3 this week"
    trend="up"
    icon={<Trophy className="h-5 w-5" />}
  />
  <StatCard
    title="Practice Time"
    value="2h 15m"
    change="Average per day"
    trend="neutral"
    icon={<Clock className="h-5 w-5" />}
  />
  <StatCard
    title="Team Ranking"
    value="#3"
    change="Moved up 2 spots"
    trend="up"
    icon={<Target className="h-5 w-5" />}
  />
</StatsDisplay>
```

## Implementation Guidelines

### shadcn/ui Setup

**Installation and Configuration**
```bash
# Install shadcn/ui CLI
npx shadcn-ui@latest init

# Add components as needed
npx shadcn-ui@latest add button
npx shadcn-ui@latest add card
npx shadcn-ui@latest add input
npx shadcn-ui@latest add form
```

**Tailwind Configuration Integration**
```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        // FootballIQ custom colors
        primary: {
          DEFAULT: '#2F6B3B',
          dark: '#1E4525',
          light: '#4A8F5F'
        },
        // ... rest of color system
      },
      // ... rest of design tokens
    },
  },
  plugins: [require("tailwindcss-animate")],
}
```

### Component Customization

**Custom Component Wrapper**
```tsx
// components/ui/button-football.tsx
import { Button as ShadcnButton } from "@/components/ui/button"
import { cn } from "@/lib/utils"

interface FootballButtonProps 
  extends React.ComponentProps<typeof ShadcnButton> {
  footballVariant?: "coach" | "player" | "admin"
}

export function FootballButton({ 
  footballVariant, 
  className, 
  ...props 
}: FootballButtonProps) {
  return (
    <ShadcnButton
      className={cn(
        footballVariant === "coach" && "bg-accent-primary hover:bg-accent-primary/90",
        footballVariant === "player" && "bg-secondary hover:bg-secondary/90",
        footballVariant === "admin" && "bg-neutral-800 hover:bg-neutral-700",
        className
      )}
      {...props}
    />
  )
}
```

### Responsive Component Patterns

**Mobile-First Component Design**
```tsx
export function ResponsiveCard({ children, ...props }) {
  return (
    <Card 
      className={cn(
        // Mobile: Full width with minimal padding
        "w-full p-4",
        // Tablet: Moderate padding increase
        "md:p-6",
        // Desktop: Maximum padding and width constraints
        "lg:p-8 lg:max-w-2xl",
        props.className
      )}
      {...props}
    >
      {children}
    </Card>
  )
}
```

### Accessibility Implementation

**Keyboard Navigation Support**
```tsx
export function AccessibleButton({ children, ...props }) {
  return (
    <Button
      onKeyDown={(e) => {
        // Handle Enter and Space for button activation
        if (e.key === 'Enter' || e.key === ' ') {
          e.preventDefault()
          e.currentTarget.click()
        }
      }}
      {...props}
    >
      {children}
    </Button>
  )
}
```

**Screen Reader Support**
```tsx
export function SignalCard({ signal, ...props }) {
  return (
    <Card {...props}>
      <div 
        role="img" 
        aria-label={`Signal demonstration: ${signal.name}`}
      >
        <video src={signal.videoUrl} />
      </div>
      <div>
        <h3 id={`signal-${signal.id}`}>{signal.name}</h3>
        <p aria-describedby={`signal-${signal.id}`}>
          {signal.description}
        </p>
      </div>
    </Card>
  )
}
```

## Related Documentation
- [Style Guide](../style-guide.md) - Complete design system specifications
- [Color System](../tokens/colors.md) - Color usage in components
- [Typography System](../tokens/typography.md) - Typography implementation
- [Animation System](../tokens/animations.md) - Component animations and transitions

## Implementation Notes
This component library provides the complete building blocks for the FootballIQ platform, combining shadcn/ui's robust foundation with football-specific customizations. All components are optimized for mobile-first usage while maintaining accessibility and performance standards.

## Last Updated
Created: December 28, 2024  
Version: 1.0  
Next Review: January 15, 2025

---

**Development Ready**: All components include TypeScript definitions, accessibility support, and mobile-first responsive design, ready for immediate implementation in the FootballIQ platform.