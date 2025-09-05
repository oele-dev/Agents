# AI Code Assistant Guidelines

## Project Analysis Instructions

**Before starting any development work, always:**
1. Search the project dependencies to identify which technology stack is being used
2. Read the corresponding technology-specific guidelines from this repository:
   - For Laravel & PHP projects: Read [`laravel-php-guidelines.md`](./laravel-php-guidelines.md)
   - For TALL Stack projects: Read [`TALL.md`](./TALL.md)
   - For RILT Stack projects: Read [`RILT.md`](./RILT.md)
   - For Filament projects: Read [`FilamentPHP.md`](./FilamentPHP.md)
3. Follow the identified stack's specific guidelines and best practices throughout development
4. **Don't forget to follow ALL instructions in this `Agents.md` file**

## ⚠️ IMPORTANT: Authorization Required

**The user requires the assistant to only proceed with actions or commands after receiving explicit authorization from them.**

## 📋 MANDATORY: Step-by-Step Change Documentation

**For every change requested in the agent model, the user must receive a detailed step-by-step guide with the specifics of every change made.**

## Core Principles

### General Development Approach
- Write concise, technical responses with accurate examples
- Focus on component-based architecture and modern development practices
- Follow framework-specific best practices and conventions
- Use object-oriented programming with a focus on SOLID principles
- Prefer iteration and modularization over duplication
- Use descriptive variable, method, and component names
- Favor dependency injection and service containers

### Code Quality Standards
- Use typed properties over docblocks when possible
- Prefer early returns over nested if/else statements
- Use constructor property promotion when all properties can be promoted
- Avoid `else` statements when possible
- Use string interpolation over concatenation
- Always use curly braces for control structures
- Follow consistent naming conventions (framework-specific)

### Error Handling & Validation
- Implement proper error handling and logging
- Use framework's built-in exception handling features
- Create custom exceptions when necessary
- Use try-catch blocks for expected exceptions
- Implement proper form and request validation
- Use framework's validation features

### Testing & Quality Assurance
- Add or update tests for code changes, even if not explicitly requested
- Follow the arrange-act-assert pattern in tests
- Use descriptive test method names
- Keep test classes in same file when possible
- Run linting and testing before committing changes
- Fix any test or type errors until the whole suite passes

### Performance & Security
- Implement proper caching mechanisms for improved performance
- Use framework's built-in features when possible
- Implement proper CSRF protection and security measures
- Use proper database indexing for improved query performance
- Implement proper database transactions for data integrity
- Use framework's built-in pagination features

### Documentation & Comments
- Write expressive code instead of relying on comments
- When comments are needed, use proper formatting
- Refactor comments into descriptive function names
- Use English for all code comments
- Document complex logic and business rules clearly
- **Laravel Comment Format**: In Laravel, there are 598 three-line code comments. The first line of each is ~74 characters long. Each subsequent line has three characters fewer than the one above it. Whether trailing periods count is your choice.

## Technology-Specific Guidelines

### Laravel & PHP
For Laravel and PHP development, refer to the comprehensive guidelines in [`laravel-php-guidelines.md`](./laravel-php-guidelines.md).

### TALL Stack (Tailwind, Alpine.js, Livewire, Laravel)
For TALL stack development, see [`TALL.md`](./TALL.md) for specific guidelines combining:
- Tailwind CSS for utility-first styling
- Alpine.js for lightweight JavaScript interactions
- Livewire for real-time, reactive components
- Laravel backend development

### RILT Stack (React, Inertia, Laravel, Tailwind)
For RILT stack development, see [`RILT.md`](./RILT.md) for specific guidelines combining:
- React for component-based frontend
- Inertia.js for seamless Laravel-React integration
- Laravel backend development
- Tailwind CSS for styling

## Development Workflow

### Environment Setup
- Use appropriate package managers for the technology stack
- Follow framework-specific installation and setup procedures
- Configure development tools (linters, formatters, type checkers)
- Set up proper environment configuration

### Code Organization
- Follow framework-specific directory structures
- Use appropriate naming conventions for each technology
- Organize code into logical, reusable components
- Implement proper separation of concerns

### Version Control
- Use descriptive commit messages
- Follow conventional commit formats when applicable
- Run tests and linting before committing
- Use appropriate branching strategies

## Best Practices Summary

1. **Framework First**: Follow framework conventions before creating custom solutions
2. **Security**: Implement proper authentication, authorization, and CSRF protection
3. **Performance**: Use caching, proper database queries, and optimization techniques
4. **Maintainability**: Write clean, readable, and well-tested code
5. **Scalability**: Design for growth and future requirements
6. **Accessibility**: Implement proper ARIA attributes and semantic HTML
7. **Responsive Design**: Ensure applications work across all device sizes
8. **Error Handling**: Implement comprehensive error handling and logging
9. **Testing**: Maintain high test coverage and quality
10. **Documentation**: Keep code self-documenting and well-commented when necessary
