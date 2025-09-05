# RILT Stack Guidelines (React, Inertia, Laravel, Tailwind)

## Overview
This document provides specific guidelines for the RILT stack development. For general AI assistant principles, see [`agents.md`](./agents.md). For comprehensive Laravel and PHP guidelines, refer to [`laravel-php-guidelines.md`](./laravel-php-guidelines.md).

## Stack Components

### Laravel Backend
- Follow all guidelines in [`laravel-php-guidelines.md`](./laravel-php-guidelines.md)
- Use Laravel 9+ with PHP 8.1+ features
- Implement proper Eloquent ORM usage
- Use Laravel's built-in authentication and authorization
- Implement proper database migrations and seeders
- Use Inertia.js for seamless Laravel-React integration

### React Frontend
- Use React 18+ with TypeScript for type safety
- Follow React best practices and modern patterns
- Use functional components with hooks
- Implement proper state management with React hooks
- Use React Router for client-side routing (when needed)
- Follow component-based architecture principles

### Inertia.js Integration
- Use Inertia.js for seamless Laravel-React integration
- Implement proper data sharing between Laravel and React
- Use Inertia's built-in form handling and validation
- Implement proper error handling with Inertia
- Use Inertia's page components and layouts
- Handle navigation and routing with Inertia

### Tailwind CSS Styling
- Use Tailwind CSS for styling components, following a utility-first approach
- Follow a consistent design language using Tailwind CSS classes
- Implement responsive design and dark mode using Tailwind utilities
- Optimize for accessibility (e.g., aria-attributes) when using components
- Use Tailwind's component system for reusable UI patterns
- Implement proper spacing, typography, and color schemes

## RILT-Specific Best Practices

### Component Architecture
- Create React components for UI logic and presentation
- Use Inertia.js for data fetching and server communication
- Structure components with clear separation of concerns
- Use TypeScript for type safety and better development experience
- Implement proper component communication patterns

### State Management
- Use React hooks for local component state
- Use Inertia.js for server-side state management
- Implement proper data flow between components
- Use React Context for global state when needed
- Handle form state with Inertia's form handling

### Performance Optimization
- Use React's built-in optimization features (memo, useMemo, useCallback)
- Implement proper code splitting with React.lazy
- Use Inertia's lazy loading for pages
- Optimize Tailwind CSS builds for production
- Use React's concurrent features for better user experience

### Form Handling
- Use Inertia's form handling and validation
- Implement proper error handling with React
- Use Tailwind CSS for form styling and feedback
- Handle form submissions with Inertia forms
- Implement proper CSRF protection

### UI/UX Patterns
- Use Tailwind CSS for consistent design system
- Implement responsive design patterns
- Use React for smooth animations and transitions
- Create accessible components with proper ARIA attributes
- Use Inertia's loading states for better user feedback

## File Structure
```
app/
├── Http/Controllers/       # Laravel controllers
├── Http/Requests/         # Form request validation
└── Http/Resources/        # API resources

resources/
├── js/
│   ├── Components/        # React components
│   ├── Layouts/          # Inertia layouts
│   ├── Pages/            # Inertia pages
│   └── Types/            # TypeScript type definitions
├── views/
│   └── app.blade.php     # Inertia root template
└── css/
    └── app.css           # Tailwind CSS imports
```

## Development Workflow

### Component Creation
1. Create React component with TypeScript
2. Style component with Tailwind CSS
3. Integrate with Inertia.js for data handling
4. Test component functionality and responsiveness
5. Implement proper error handling and validation

### Styling Approach
1. Use Tailwind CSS utility classes for styling
2. Create custom components for repeated patterns
3. Implement responsive design with Tailwind breakpoints
4. Use Tailwind's dark mode features
5. Ensure accessibility compliance

### Testing Strategy
1. Test React components with Jest and React Testing Library
2. Test Inertia integration with Laravel testing tools
3. Test responsive design across different screen sizes
4. Test accessibility with screen readers and keyboard navigation
5. Test form validation and error handling

## Common Patterns

### Inertia + React Integration
```tsx
// React Component with Inertia
import { useForm } from '@inertiajs/react'
import { User } from '@/Types/User'

interface Props {
    user: User
}

export default function UserProfile({ user }: Props) {
    const { data, setData, post, processing, errors } = useForm({
        name: user.name,
        email: user.email,
    })

    const handleSubmit = (e: React.FormEvent) => {
        e.preventDefault()
        post('/profile')
    }

    return (
        <form onSubmit={handleSubmit} className="bg-white rounded-lg shadow p-6">
            <div className="mb-4">
                <label className="block text-sm font-medium text-gray-700">
                    Name
                </label>
                <input
                    type="text"
                    value={data.name}
                    onChange={(e) => setData('name', e.target.value)}
                    className="mt-1 block w-full border border-gray-300 rounded-md px-3 py-2"
                />
                {errors.name && (
                    <p className="mt-1 text-sm text-red-600">{errors.name}</p>
                )}
            </div>

            <button
                type="submit"
                disabled={processing}
                className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 disabled:opacity-50"
            >
                {processing ? 'Saving...' : 'Save'}
            </button>
        </form>
    )
}
```

### Laravel Controller with Inertia
```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Inertia\Inertia;

class UserController extends Controller
{
    public function show(User $user)
    {
        return Inertia::render('Users/Show', [
            'user' => $user->only(['id', 'name', 'email']),
        ]);
    }

    public function update(Request $request, User $user)
    {
        $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email,' . $user->id,
        ]);

        $user->update($request->only(['name', 'email']));

        return redirect()->back()->with('success', 'Profile updated successfully.');
    }
}
```

### Responsive Design with Tailwind
```tsx
export default function Dashboard() {
    return (
        <div className="min-h-screen bg-gray-100">
            <div className="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <div className="bg-white overflow-hidden shadow rounded-lg">
                        <div className="p-5">
                            <div className="flex items-center">
                                <div className="flex-shrink-0">
                                    <div className="w-8 h-8 bg-blue-500 rounded-full"></div>
                                </div>
                                <div className="ml-5 w-0 flex-1">
                                    <dl>
                                        <dt className="text-sm font-medium text-gray-500 truncate">
                                            Total Users
                                        </dt>
                                        <dd className="text-lg font-medium text-gray-900">
                                            1,234
                                        </dd>
                                    </dl>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    )
}
```

## TypeScript Integration

### Type Definitions
```typescript
// resources/js/Types/User.ts
export interface User {
    id: number
    name: string
    email: string
    email_verified_at: string | null
    created_at: string
    updated_at: string
}

// resources/js/Types/Inertia.ts
import { User } from './User'

export interface InertiaPageProps {
    auth: {
        user: User
    }
    flash: {
        success?: string
        error?: string
    }
}
```

### Component Props
```typescript
import { InertiaPageProps } from '@/Types/Inertia'

interface Props extends InertiaPageProps {
    users: User[]
}

export default function UsersIndex({ users, auth }: Props) {
    // Component implementation
}
```

## Dependencies
- Laravel 9+ (latest stable version)
- React 18+ with TypeScript
- Inertia.js for Laravel-React integration
- Tailwind CSS 3+ for utility-first styling
- Composer for dependency management
- NPM/Yarn for frontend dependencies

## Development Tools
- Vite for fast development and building
- ESLint for code linting
- Prettier for code formatting
- TypeScript for type safety
- Jest and React Testing Library for testing

## Documentation Access

### Inertia.js Markdown Documentation
When searching for Inertia.js documentation, prefer markdown versions for better readability and easier parsing. Inertia.js supports markdown access by adding `.md` to the URL:

- **Root documentation**: Add `/index.md` to the base URL
  - Example: `https://inertiajs.com/index.md`
- **Specific pages**: Add `.md` extension to any documentation URL
  - Example: `https://inertiajs.com/installation.md`

This approach provides:
- Cleaner, more readable documentation format
- Better compatibility with AI assistants and development tools
- Easier parsing and reference for automated systems
- Consistent formatting for Inertia.js documentation

## References
- [General AI Assistant Guidelines](./agents.md)
- [Laravel & PHP Guidelines](./laravel-php-guidelines.md)
- [React Documentation](https://react.dev/)
- [Inertia.js Documentation](https://inertiajs.com/) | [Markdown Docs](https://inertiajs.com/index.md)
- [Tailwind CSS Documentation](https://tailwindcss.com/)
- [TypeScript Documentation](https://www.typescriptlang.org/)
