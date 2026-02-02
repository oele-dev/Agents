---
name: skill-tailwind
description: Tailwind CSS utility-first styling patterns and responsive design
when_to_use: Detected when working with Tailwind classes, tailwind.config.js, or utility-first CSS
version: 1.2
---

# Tailwind CSS Development Skill

## Core Approach
- Use Tailwind CSS for styling components, following a utility-first approach
- Follow a consistent design language using Tailwind CSS classes
- Implement responsive design and dark mode using Tailwind utilities
- Optimize for accessibility (e.g., aria-attributes) when using components
- Use Tailwind's component system for reusable UI patterns
- Implement proper spacing, typography, and color schemes

## Styling Best Practices
1. Use Tailwind CSS utility classes for styling
2. Create custom components for repeated patterns
3. Implement responsive design with Tailwind breakpoints
4. Use Tailwind's dark mode features
5. Ensure accessibility compliance

## Responsive Design Patterns
```tsx
// Grid layout with responsive columns
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
    {items.map((item) => (
        <div key={item.id} className="bg-white rounded-lg shadow p-6 hover:shadow-lg transition-shadow">
            {/* Item content */}
        </div>
    ))}
</div>
```

```blade
<!-- Responsive container -->
<div class="min-h-screen bg-gray-100">
    <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            @foreach($items as $item)
                <div class="bg-white overflow-hidden shadow rounded-lg">
                    <!-- Item content -->
                </div>
            @endforeach
        </div>
    </div>
</div>
```

## Common Utility Patterns

### Layout & Spacing
- Container: `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`
- Card: `bg-white rounded-lg shadow p-6`
- Flex center: `flex items-center justify-center`
- Grid responsive: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6`

### Typography
- Heading: `text-2xl font-bold text-gray-900`
- Body text: `text-base text-gray-700`
- Small text: `text-sm text-gray-500`

### Forms
- Input: `w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500`
- Button: `bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500`
- Label: `block text-sm font-medium text-gray-700 mb-1`

### States
- Hover: `hover:bg-blue-600 hover:shadow-lg`
- Focus: `focus:outline-none focus:ring-2 focus:ring-blue-500`
- Active: `active:bg-blue-700`
- Disabled: `disabled:opacity-50 disabled:cursor-not-allowed`

## Responsive Breakpoints
- `sm:` - 640px
- `md:` - 768px
- `lg:` - 1024px
- `xl:` - 1280px
- `2xl:` - 1536px

## Dark Mode
```html
<!-- Dark mode support -->
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
    <!-- Content -->
</div>
```

## Accessibility
- Use semantic HTML elements
- Include proper ARIA attributes
- Ensure sufficient color contrast
- Support keyboard navigation
- Add focus states to interactive elements

## Performance
- Optimize Tailwind CSS builds for production
- Use PurgeCSS to remove unused styles
- Enable JIT mode for faster development

## Dependencies
- Tailwind CSS 3+ for utility-first styling

## References
- [Tailwind CSS Documentation](https://tailwindcss.com/)

---
*Use alongside skill-laravel.md, skill-livewire.md, or skill-react-inertia.md*
