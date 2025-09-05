# TALL Stack Guidelines (Tailwind, Alpine.js, Livewire, Laravel)

## Overview
This document provides specific guidelines for the TALL stack development. For general AI assistant principles, see [`agents.md`](./agents.md). For comprehensive Laravel and PHP guidelines, refer to [`laravel-php-guidelines.md`](./laravel-php-guidelines.md).

## Stack Components

### Laravel Backend
- Follow all guidelines in [`laravel-php-guidelines.md`](./laravel-php-guidelines.md)
- Use Laravel 9+ with PHP 8.1+ features
- Implement proper Eloquent ORM usage
- Use Laravel's built-in authentication and authorization
- Implement proper database migrations and seeders

### Livewire Components
- Use Livewire for dynamic components and real-time user interactions
- Favor Livewire's lifecycle hooks and properties
- Use the latest Livewire (3+) features for optimization and reactivity
- Implement Blade components with Livewire directives (e.g., `wire:model`)
- Handle state management and form handling using Livewire properties and actions
- Use `wire:loading` and `wire:target` to provide feedback and optimize user experience
- Apply Livewire's security measures for components
- Use Livewire components to break down complex UIs into smaller, reusable units

### Alpine.js Integration
- Use Alpine.js for lightweight JavaScript interactions
- Combine Alpine.js with Livewire for enhanced interactivity
- Use Alpine.js for client-side state management when appropriate
- Implement Alpine.js directives for DOM manipulation
- Use Alpine.js for form validation and user feedback
- Keep Alpine.js code minimal and focused on UI interactions

### Tailwind CSS Styling
- Use Tailwind CSS for styling components, following a utility-first approach
- Follow a consistent design language using Tailwind CSS classes
- Implement responsive design and dark mode using Tailwind utilities
- Optimize for accessibility (e.g., aria-attributes) when using components
- Use Tailwind's component system for reusable UI patterns
- Implement proper spacing, typography, and color schemes

## TALL-Specific Best Practices

### Component Architecture
- Create Livewire components for server-side logic and state management
- Use Alpine.js for client-side interactions and animations
- Structure components with clear separation of concerns
- Use Blade components for reusable UI elements
- Implement proper component communication patterns

### State Management
- Use Livewire properties for server-side state
- Use Alpine.js for client-side state when needed
- Implement proper data binding between Livewire and Alpine.js
- Use Livewire events for component communication
- Handle form state with Livewire's built-in form handling

### Performance Optimization
- Use Livewire's lazy loading for components
- Implement proper caching strategies
- Use Alpine.js for lightweight client-side interactions
- Optimize Tailwind CSS builds for production
- Use Livewire's wire:key for proper component re-rendering

### Form Handling
- Use Livewire's form validation and error handling
- Implement real-time validation with Alpine.js
- Use Tailwind CSS for form styling and feedback
- Handle form submissions with Livewire actions
- Implement proper CSRF protection

### UI/UX Patterns
- Use Tailwind CSS for consistent design system
- Implement responsive design patterns
- Use Alpine.js for smooth animations and transitions
- Create accessible components with proper ARIA attributes
- Use Livewire's loading states for better user feedback

## File Structure
```
app/
├── Http/Livewire/          # Livewire components
│   ├── Components/         # Reusable components
│   └── Pages/             # Page components
├── View/Components/        # Blade components
└── View/Composers/         # View composers

resources/
├── views/
│   ├── components/         # Blade components
│   ├── layouts/           # Layout templates
│   └── livewire/          # Livewire component views
├── js/
│   └── alpine/            # Alpine.js components
└── css/
    └── app.css            # Tailwind CSS imports
```

## Development Workflow

### Component Creation
1. Create Livewire component for server-side logic
2. Create corresponding Blade view with Tailwind CSS
3. Add Alpine.js for client-side interactions if needed
4. Test component functionality and responsiveness
5. Implement proper error handling and validation

### Styling Approach
1. Use Tailwind CSS utility classes for styling
2. Create custom components for repeated patterns
3. Implement responsive design with Tailwind breakpoints
4. Use Tailwind's dark mode features
5. Ensure accessibility compliance

### Testing Strategy
1. Test Livewire components with Laravel's testing tools
2. Test Alpine.js interactions with browser testing
3. Test responsive design across different screen sizes
4. Test accessibility with screen readers and keyboard navigation
5. Test form validation and error handling

## Common Patterns

### Livewire + Alpine.js Integration
```php
// Livewire Component
class UserProfile extends Component
{
    public User $user;

    public function updateProfile()
    {
        // Server-side logic
    }
}
```

```blade
<!-- Blade View with Alpine.js -->
<div x-data="{ editing: false }" class="bg-white rounded-lg shadow p-6">
    <div x-show="!editing">
        <h2 class="text-xl font-semibold">{{ $user->name }}</h2>
        <button @click="editing = true" class="mt-4 bg-blue-500 text-white px-4 py-2 rounded">
            Edit Profile
        </button>
    </div>

    <div x-show="editing">
        <form wire:submit.prevent="updateProfile">
            <input wire:model="user.name" class="w-full border rounded px-3 py-2">
            <button type="submit" class="bg-green-500 text-white px-4 py-2 rounded">
                Save
            </button>
        </form>
    </div>
</div>
```

### Responsive Design with Tailwind
```blade
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
    @foreach($items as $item)
        <div class="bg-white rounded-lg shadow p-6 hover:shadow-lg transition-shadow">
            <!-- Item content -->
        </div>
    @endforeach
</div>
```

## Dependencies
- Laravel 9+ (latest stable version)
- Livewire 3+ for real-time, reactive components
- Alpine.js 3+ for lightweight JavaScript interactions
- Tailwind CSS 3+ for utility-first styling
- Composer for dependency management
- NPM/Yarn for frontend dependencies

## References
- [General AI Assistant Guidelines](./agents.md)
- [Laravel & PHP Guidelines](./laravel-php-guidelines.md)
- [Livewire Documentation](https://livewire.laravel.com/)
- [Alpine.js Documentation](https://alpinejs.dev/)
- [Tailwind CSS Documentation](https://tailwindcss.com/)
