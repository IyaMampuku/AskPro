# AskPro - Question & Answer Platform

## Overview

AskPro is a Q&A platform in transition from a vanilla JavaScript prototype to a modern full-stack application. The project is being migrated from a simple client-side implementation (using localStorage) to a React-based SPA with Express backend, PostgreSQL database (via Neon), and Drizzle ORM. The core functionality enables users to browse questions, post new questions with categories, view answers, like answers, and search/filter content.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React with TypeScript, using Vite as the build tool

**UI Component Library**: Radix UI primitives with shadcn/ui component system
- Design approach follows Stack Overflow/GitHub Primer aesthetic with utility-focused patterns
- Uses Tailwind CSS for styling with custom design tokens
- Component configuration in `components.json` specifies the "new-york" style variant
- Theme system supports both light and dark modes with CSS custom properties

**Routing**: wouter (lightweight client-side routing library)
- Currently only has a 404 fallback route configured in `client/src/App.tsx`
- Structure prepared for page components to be added

**State Management**: TanStack Query (React Query) for server state
- Configured with custom query client in `client/src/lib/queryClient.ts`
- Includes custom API request helpers and error handling
- Default configuration disables automatic refetching and sets infinite stale time

**Design System**:
- Typography: Inter font family with system font fallbacks
- Color palette defined for both light/dark modes following Q&A platform conventions
- Emphasis on content-first hierarchy and scannable layouts
- Design guidelines documented in `design_guidelines.md`

### Backend Architecture

**Server Framework**: Express.js with TypeScript
- Entry point: `server/index.ts`
- Middleware includes JSON parsing, URL encoding, and request logging
- Development-only Vite integration for HMR and SSR
- Route registration separated into `server/routes.ts`

**API Structure**: RESTful endpoints prefixed with `/api` (routes not yet implemented)

**Error Handling**: Centralized error middleware catches all errors and returns JSON responses

**Development Tools**:
- Runtime error overlay via Replit plugin
- Cartographer and dev banner plugins (Replit-specific, development only)
- Custom logger with formatted timestamps

### Data Storage

**Database**: PostgreSQL via Neon serverless driver
- Connection configured through `DATABASE_URL` environment variable
- Schema management with Drizzle ORM

**ORM**: Drizzle ORM
- Schema definition: `shared/schema.ts`
- Migration output directory: `./migrations`
- Currently defines minimal `users` table with id, username, and password fields
- Zod schemas for validation via `drizzle-zod`

**Storage Abstraction**: Interface-based storage layer (`server/storage.ts`)
- `IStorage` interface defines CRUD operations
- `MemStorage` implementation provides in-memory storage for development
- Designed for easy swapping to database-backed implementation

**Legacy Data Layer**: Original prototype used browser localStorage with mock data
- Mock questions array in `script.js` with hardcoded sample data
- Includes questions with nested answers, categories, timestamps, and like counts
- This will be replaced by database-backed API calls

### Authentication & Authorization

**Current State**: Minimal user schema prepared but no authentication implemented
- User table has password field for future bcrypt/argon2 hashing
- No session management, JWT, or auth middleware currently configured

**Planned Approach** (based on legacy design documents):
- Simple session-based or token-based authentication
- Password hashing with comments explaining the security process
- User profile pages showing user-specific questions and answers

### Code Organization

**Monorepo Structure**:
```
/client          # React SPA
  /src
    /components  # shadcn/ui components
    /hooks       # Custom React hooks
    /lib         # Utilities and configuration
    /pages       # Page components (minimal currently)
/server          # Express backend
/shared          # Shared TypeScript types and schemas
/migrations      # Database migrations (Drizzle)
```

**Path Aliases**:
- `@/` → `client/src/`
- `@shared/` → `shared/`
- `@assets/` → `attached_assets/`

**Build Process**:
- Frontend: Vite builds to `dist/public`
- Backend: esbuild bundles server to `dist/index.js`
- Separate development and production configurations

### Migration Status

**Legacy Files** (original vanilla JS prototype):
- `index.html`, `question.html`, `ask.html` - Static HTML pages
- `style.css` - Custom CSS (being replaced by Tailwind)
- `script.js` - Vanilla JavaScript with mock data
- These files represent the original student-friendly implementation

**Modern Stack** (current implementation):
- React components not yet created for Q&A pages
- API routes not yet implemented in `server/routes.ts`
- Database schema minimal (only users table)
- Storage layer uses in-memory implementation

**Key Architectural Decisions**:

1. **Gradual Migration**: Preserving original files while building modern stack allows reference to original functionality
2. **Storage Interface**: Abstraction allows development without database, easy transition to PostgreSQL later
3. **Shared Schemas**: Drizzle schemas in `/shared` enable type-safe client-server communication
4. **Component Library**: shadcn/ui chosen for customizability while maintaining professional Q&A aesthetic
5. **TypeScript Throughout**: Ensures type safety across full stack despite being a student project evolution

## External Dependencies

### Third-Party Services

**Database**: Neon Serverless PostgreSQL
- Requires `DATABASE_URL` environment variable
- Serverless driver: `@neondatabase/serverless`
- Connection pooling handled by Neon infrastructure

**Session Storage** (configured but not actively used):
- `connect-pg-simple` for PostgreSQL-backed sessions
- Prepared for future authentication implementation

### UI Component Libraries

**Radix UI**: Unstyled, accessible component primitives
- Complete suite: accordion, alert-dialog, avatar, checkbox, dialog, dropdown-menu, hover-card, label, menubar, navigation-menu, popover, progress, radio-group, scroll-area, select, separator, slider, switch, tabs, toast, toggle, tooltip
- Provides accessibility and keyboard navigation out of the box

**Additional UI Dependencies**:
- `cmdk` - Command menu component
- `embla-carousel-react` - Carousel/slider functionality
- `lucide-react` - Icon library
- `vaul` - Drawer component (mobile-friendly)
- `input-otp` - OTP input component
- `react-day-picker` - Calendar/date picker
- `recharts` - Chart components (with custom theming)
- `react-resizable-panels` - Resizable panel layouts

### Styling & Utilities

**Tailwind CSS**: Utility-first CSS framework
- Custom configuration in `tailwind.config.ts`
- Design tokens for light/dark themes
- PostCSS for processing

**Utility Libraries**:
- `class-variance-authority` - Component variant management
- `clsx` & `tailwind-merge` - Conditional class composition
- `date-fns` - Date formatting and manipulation

### Form Management

**React Hook Form**: Form state management
- `@hookform/resolvers` for validation integration
- Paired with Zod schemas from Drizzle for type-safe forms

### Development Tools

**Replit Platform Integrations**:
- `@replit/vite-plugin-runtime-error-modal` - Error overlay
- `@replit/vite-plugin-cartographer` - Code navigation (dev only)
- `@replit/vite-plugin-dev-banner` - Development banner (dev only)

**Build Tools**:
- Vite - Frontend bundler and dev server
- esbuild - Backend bundler
- TypeScript - Type checking
- Drizzle Kit - Database migration management

### Runtime Dependencies

- Node.js runtime
- React 18+ with React DOM
- Express.js web framework
- TanStack Query for data fetching