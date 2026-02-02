---
name: skill-react-inertia
description: React + Inertia.js patterns for Laravel SPA development (RILT stack)
when_to_use: Detected when working with .tsx/.jsx files, useForm, Inertia.render, or RILT stack projects
version: 1.2
---

# React + Inertia.js Development Skill (RILT Stack)

## Stack Components

### React Frontend
- Use React 18+ with TypeScript for type safety
- Follow React best practices and modern patterns
- Use functional components with hooks
- Implement proper state management with React hooks
- Follow component-based architecture principles

### Inertia.js Integration
- Use Inertia.js for seamless Laravel-React integration
- Implement proper data sharing between Laravel and React
- Use Inertia's built-in form handling and validation
- Implement proper error handling with Inertia
- Use Inertia's page components and layouts
- Handle navigation and routing with Inertia

## Component Architecture
- Create React components for UI logic and presentation
- Use Inertia.js for data fetching and server communication
- Structure components with clear separation of concerns
- Use TypeScript for type safety and better development experience
- Implement proper component communication patterns

## State Management
- Use React hooks for local component state
- Use Inertia.js for server-side state management
- Implement proper data flow between components
- Use React Context for global state when needed
- Handle form state with Inertia's form handling

## Performance Optimization
- Use React's built-in optimization features (memo, useMemo, useCallback)
- Implement proper code splitting with React.lazy
- Use Inertia's lazy loading for pages
- Use React's concurrent features for better user experience

## Form Handling
- Use Inertia's form handling and validation
- Implement proper error handling with React
- Handle form submissions with Inertia forms
- Implement proper CSRF protection

## Common Patterns

### Inertia + React Component
```tsx
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
└── views/
    └── app.blade.php     # Inertia root template
```

## Development Tools
- Vite for fast development and building
- ESLint for code linting
- Prettier for code formatting
- TypeScript for type safety
- Jest and React Testing Library for testing

## Documentation Access

### Inertia.js Markdown Documentation
When searching for Inertia.js documentation, prefer markdown versions:

- **Root documentation**: Add `/index.md` to the base URL
  - Example: `https://inertiajs.com/index.md`
- **Specific pages**: Add `.md` extension to any documentation URL
  - Example: `https://inertiajs.com/installation.md`

## Dependencies
- Laravel 9+ (latest stable version)
- React 18+ with TypeScript
- Inertia.js for Laravel-React integration

## References
- [React Documentation](https://react.dev/)
- [Inertia.js Documentation](https://inertiajs.com/) | [Markdown Docs](https://inertiajs.com/index.md)
- [TypeScript Documentation](https://www.typescriptlang.org/)

---
*Use alongside skill-laravel.md and skill-tailwind.md for complete RILT stack*
