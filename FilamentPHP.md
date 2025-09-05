# Filament Development Guidelines

This file contains specific guidelines for Filament development, focusing on the admin panel framework for Laravel.

## Core Filament Principles

**Follow Filament conventions first.** If Filament has a documented way to do something, use it. Only deviate when you have a clear justification.

## Component Architecture

- Follow Filament's component-based architecture patterns
- Use Filament's built-in form components and validation
- Implement proper resource management using Filament resources
- Follow Filament's naming conventions for pages, resources, and components
- Use Filament's relationship management features
- Implement proper authorization using Filament's policy system

## Resource Management

### Resource Classes
- Use singular resource names (`UserResource`, `PostResource`)
- Follow Filament's resource structure with proper methods
- Implement proper form and table configurations
- Use Filament's built-in actions and filters

### Pages
- Create dedicated pages for custom functionality
- Follow Filament's page naming conventions
- Use Filament's page lifecycle hooks appropriately

## Forms & Validation

- Use Filament's form components (`TextInput`, `Select`, `FileUpload`, etc.)
- Implement proper validation using Filament's validation system
- Use Filament's form sections and groups for organization
- Follow Filament's form field naming conventions

## Tables & Data Display

- Use Filament's table components and features
- Implement proper sorting and filtering
- Use Filament's built-in column types
- Follow Filament's table action patterns

## Authorization & Policies

- Implement proper authorization using Filament's policy system
- Use Filament's built-in authorization features
- Follow Filament's permission and role management patterns

## Styling & UI

- Use Filament's built-in styling system
- Follow Filament's design patterns and components
- Implement consistent UI using Filament's theme system

## Best Practices

1. **Resource-First Approach**: Structure your admin around Filament resources
2. **Component Reusability**: Create reusable Filament components when needed
3. **Performance**: Use Filament's built-in optimization features
4. **Security**: Implement proper authorization and validation
5. **Consistency**: Follow Filament's conventions throughout the application

## Quick Reference

### Naming Conventions
- **Resources**: Singular + `Resource` (`UserResource`, `PostResource`)
- **Pages**: Descriptive + `Page` (`CreateUserPage`, `EditPostPage`)
- **Components**: Descriptive + `Component` (`UserStatsComponent`)

### File Structure
- Resources: `app/Filament/Resources/`
- Pages: `app/Filament/Pages/`
- Components: `app/Filament/Components/`
- Widgets: `app/Filament/Widgets/`

---

*These guidelines are specific to Filament development and should be used alongside the main Laravel guidelines.*
