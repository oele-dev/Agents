---
name: skill-livewire
description: Livewire 3+ component patterns for real-time reactive Laravel applications
when_to_use: Detected when working with wire:model, Livewire components, or TALL stack projects
version: 1.2
---

# Livewire Development Skill

## Overview
Livewire enables real-time, reactive components in Laravel applications without writing JavaScript.

## Component Architecture
- Use Livewire for dynamic components and real-time user interactions
- Favor Livewire's lifecycle hooks and properties
- Use the latest Livewire (3+) features for optimization and reactivity
- Implement Blade components with Livewire directives (e.g., `wire:model`)
- Handle state management and form handling using Livewire properties and actions
- Use `wire:loading` and `wire:target` to provide feedback and optimize user experience
- Apply Livewire's security measures for components
- Use Livewire components to break down complex UIs into smaller, reusable units

## State Management
- Use Livewire properties for server-side state
- Implement proper data binding with wire:model
- Use Livewire events for component communication
- Handle form state with Livewire's built-in form handling

## Performance Optimization
- Use Livewire's lazy loading for components
- Implement proper caching strategies
- Use Livewire's wire:key for proper component re-rendering
- Optimize network requests with debouncing and throttling

## Form Handling
- Use Livewire's form validation and error handling
- Implement real-time validation
- Handle form submissions with Livewire actions
- Implement proper CSRF protection

## Common Patterns

### Basic Livewire Component
```php
namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\User;

class UserProfile extends Component
{
    public User $user;
    public string $name = '';
    public string $email = '';

    public function mount(User $user): void
    {
        $this->user = $user;
        $this->name = $user->name;
        $this->email = $user->email;
    }

    public function updateProfile(): void
    {
        $this->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email,' . $this->user->id,
        ]);

        $this->user->update([
            'name' => $this->name,
            'email' => $this->email,
        ]);

        $this->dispatch('profile-updated');
    }

    public function render()
    {
        return view('livewire.user-profile');
    }
}
```

### Blade View with Livewire
```blade
<div class="bg-white rounded-lg shadow p-6">
    <form wire:submit="updateProfile">
        <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">
                Name
            </label>
            <input
                wire:model="name"
                type="text"
                class="mt-1 block w-full border border-gray-300 rounded-md px-3 py-2"
            />
            @error('name')
                <p class="mt-1 text-sm text-red-600">{{ $message }}</p>
            @enderror
        </div>

        <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">
                Email
            </label>
            <input
                wire:model="email"
                type="email"
                class="mt-1 block w-full border border-gray-300 rounded-md px-3 py-2"
            />
            @error('email')
                <p class="mt-1 text-sm text-red-600">{{ $message }}</p>
            @enderror
        </div>

        <button
            type="submit"
            class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
            wire:loading.attr="disabled"
        >
            <span wire:loading.remove>Save</span>
            <span wire:loading>Saving...</span>
        </button>
    </form>
</div>
```

## UI/UX Patterns
- Use Livewire's loading states for better user feedback
- Implement proper error handling and display
- Use wire:loading for visual feedback during actions
- Create accessible components with proper ARIA attributes

## File Structure
```
app/
├── Http/Livewire/          # Livewire components
│   ├── Components/         # Reusable components
│   └── Pages/             # Page components

resources/
└── views/
    └── livewire/          # Livewire component views
```

## Dependencies
- Laravel 9+ (latest stable version)
- Livewire 3+ for real-time, reactive components

## References
- [Livewire Documentation](https://livewire.laravel.com/)
