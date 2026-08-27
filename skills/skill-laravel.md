---
name: skill-laravel
description: Laravel/PHP coding standards following Spatie guidelines, PSR standards, and Laravel conventions
when_to_use: Detected when working with .php files, composer.json, artisan commands, or Laravel projects
version: 1.2
---

# Laravel & PHP Development Skill

## Core Principle
**Follow Laravel conventions first.** If Laravel has a documented way to do something, use it. Only deviate when you have a clear justification.

## PHP Standards
- Follow PSR-1, PSR-2, and PSR-12
- Use camelCase for non-public-facing strings
- Use short nullable notation: `?string` not `string|null`
- Always specify `void` return types when methods return nothing

## Class Structure
- Use typed properties, not docblocks
- Constructor property promotion when all properties can be promoted
- One trait per line

## Type Declarations & Docblocks
- Use typed properties over docblocks
- Specify return types including `void`
- Use short nullable syntax: `?Type` not `Type|null`
- Document iterables with generics:
  ```php
  /** @return Collection<int, User> */
  public function getUsers(): Collection
  ```

### Docblock Rules
- Don't use docblocks for fully type-hinted methods (unless description needed)
- **Always import classnames in docblocks** - never use fully qualified names:
  ```php
  use \Spatie\Url\Url;
  /** @return Url */
  ```
- Use one-line docblocks when possible: `/** @var string */`
- Most common type should be first in multi-type docblocks:
  ```php
  /** @var Collection|SomeWeirdVendor\Collection */
  ```
- If one parameter needs docblock, add docblocks for all parameters
- For iterables, always specify key and value types:
  ```php
  /**
   * @param array<int, MyObject> $myArray
   * @param int $typedArgument
   */
  function someFunction(array $myArray, int $typedArgument) {}
  ```
- Use array shape notation for fixed keys, put each key on it's own line:
  ```php
  /** @return array{
     first: SomeClass,
     second: SomeClass
  } */
  ```

## Control Flow
- **Happy path last**: Handle error conditions first, success case last
- **Avoid else**: Use early returns instead of nested conditions
- **Separate conditions**: Prefer multiple if statements over compound conditions
- **Always use curly brackets** even for single statements
- **Ternary operators**: Each part on own line unless very short

```php
// Happy path last
if (! $user) {
    return null;
}

if (! $user->isActive()) {
    return null;
}

// Process active user...

// Short ternary
$name = $isFoo ? 'foo' : 'bar';

// Multi-line ternary
$result = $object instanceof Model ?
    $object->name :
    'A default value';

// Ternary instead of else
$condition
    ? $this->doSomething()
    : $this->doSomethingElse();
```

## Method Design (Composed Method / Step-down)

**Universal principle — applies to any class, not just controllers.**

Every public method reads like prose: a high-level narrative at a single level of abstraction. Details live in private helpers below. The public method answers **WHAT**; private helpers answer **HOW**.

Also known as: Extract Method (Fowler), Composed Method (Beck), Step-down rule (Clean Code), SLAP (Single Level of Abstraction Principle), SRP at method level.

- One public method = one identifiable thing
- All statements in a method at the same level of abstraction
- Read top-to-bottom: public method first, helpers below in call order
- Prefer descriptive private method names over comments

```php
public function store(StorePostRequest $request): RedirectResponse
{
    $post = $this->createPost($request->validated());

    $this->notifySubscribers($post);

    return redirect()->route('posts.show', $post);
}

private function createPost(array $data): Post
{
    return Post::create($data);
}

private function notifySubscribers(Post $post): void
{
    SubscriberNotifier::dispatch($post);
}
```

Applies equally to Models, Services, Actions, Jobs, Commands, Listeners, Livewire components, etc.

### When to extract a private helper
Extract when **ANY** of these is true:
- Public method exceeds ~15 lines
- Method has 3+ distinct phases with nameable intent
- The same block repeats elsewhere in the class (DRY)
- A comment was about to be written — replace it with a descriptive helper name

### When NOT to extract (anti-patterns)
- **Single-use, <5-line helpers** that add pure indirection. Inline reads better.
- **Linear, obvious code** with no distinguishable phases. Prose doesn't need chapters for 5 sentences.
- **Helpers that share mutable instance state** between each other (temporal coupling) — that's a smell, not a step-down.
- **"I want to test this helper"** → **extract to another class** (Action/Service), not a private method. Private helpers are implementation detail, not test units.

### Class-level smell
15 private helpers in one class ≠ SRP achieved. It usually means the class is doing too many things with prettier names. If Method Design starts feeling forced, the fix is **split the class**, not extract more helpers.

**Default bias: aggressive step-down in Controllers (fat-controller antidote), conservative elsewhere.** Indirection has a cost — pay it only when it buys real readability.

## Laravel Conventions

### Routes
- URLs: kebab-case (`/open-source`)
- Route names: camelCase (`->name('openSource')`)
- Parameters: camelCase (`{userId}`)
- Use tuple notation: `[Controller::class, 'method']`

### Controllers
- Plural resource names (`PostsController`)
- **Only resource verbs**: `index`, `create`, `store`, `show`, `edit`, `update`, `destroy`. Nothing else.
- **Non-CRUD actions → new controller.** Don't pollute a resource controller with extra methods.
- **Single Action Controllers** use `__invoke`:
  ```php
  class PublishPostController extends Controller
  {
      public function __invoke(Post $post): RedirectResponse
      {
          // ...
      }
  }
  ```
- **Apply Composed Method / Step-down rule** (see *Method Design* section below). Controllers are where this matters most, but the principle is universal.

### Private Helper vs Action Class
- **Default: private helper in the controller.** Don't create an Action "just in case".
- **Extract to Action class when ANY of these is true:**
  1. A second caller needs the same logic (Command, Job, another Controller, Listener)
  2. It manages a DB transaction, dispatches events, or calls external APIs
  3. It encodes business rules that must be tested without HTTP
  4. It needs to be dispatchable (queueable/async)
- **Stays as helper**: request-shaping, response formatting, param mapping, trivial orchestration.
- **Naming**: `CreatePostAction`, `PublishPostAction` (verb + noun + `Action`).
- **Rule of thumb**: YAGNI. Private helper until it hurts — then extract.

### Configuration
- Files: kebab-case (`pdf-generator.php`)
- Keys: snake_case (`chrome_path`)
- Add service configs to `config/services.php`, don't create new files
- Use `config()` helper, avoid `env()` outside config files

### Artisan Commands
- Names: kebab-case (`delete-old-records`)
- Always provide feedback (`$this->comment('All ok!')`)
- Show progress for loops, summary at end
- Put output BEFORE processing item (easier debugging):
  ```php
  $items->each(function(Item $item) {
      $this->info("Processing item id `{$item->id}`...");
      $this->processItem($item);
  });

  $this->comment("Processed {$items->count()} items.");
  ```

## Strings & Formatting
- **String interpolation** over concatenation

## Enums
- Use PascalCase for enum values

## Comments
- **Avoid comments** - write expressive code instead
- Refactor comments into descriptive function names
- When needed, follow **Taylor Otwell's comment style** (see [calebporzio.com/laravel-comments](https://calebporzio.com/laravel-comments)):
  - Single line: `// Space after slashes`
  - Multi-line: use `/* */` block with `|` pipe borders, a title with dashed separators, and a **three-line body** where each line is ~3 chars shorter:
    - Line 1: ~74 characters
    - Line 2: ~71 characters
    - Line 3: ~68 characters
    - Trailing periods are optional
    ```php
    /*
    |--------------------------------------------------------------------------
    | Title of the comment section
    |--------------------------------------------------------------------------
    |
    | In Laravel, there are 598 three-line code comments. The first line of each
    | is ~74 characters long. Each subsequent line has three characters fewer
    | than the one above it. Whether trailing periods count is your choice.
    |
    */
    ```

## Whitespace
- Add blank lines between statements for readability
- Exception: sequences of equivalent single-line operations
- No extra empty lines between `{}` brackets
- Let code "breathe" - avoid cramped formatting

## Validation
- Use array notation for multiple rules (easier for custom rule classes):
  ```php
  public function rules() {
      return [
          'email' => ['required', 'email'],
      ];
  }
  ```
- Custom validation rules use snake_case:
  ```php
  Validator::extend('organisation_type', function ($attribute, $value) {
      return OrganisationType::isValid($value);
  });
  ```

## Blade Templates
- Indent with 4 spaces
- No spaces after control structures:
  ```blade
  @if($condition)
      Something
  @endif
  ```

## Authorization
- Policies use camelCase: `Gate::define('editPost', ...)`
- Use CRUD words, but `view` instead of `show`

## Translations
- Use `__()` function over `@lang`

## API Routing
- Use plural resource names: `/errors`
- Use kebab-case: `/error-occurrences`
- Limit deep nesting for simplicity:
  ```
  /error-occurrences/1
  /errors/1/occurrences
  ```

## Testing
- Keep test classes in same file when possible
- Use descriptive test method names
- Follow the arrange-act-assert pattern
- **Pest 4 Preference**: Use Pest 4 instead of PHPUnit for creating tests

## Naming Conventions Quick Reference
- **Classes**: PascalCase (`UserController`, `OrderStatus`)
- **Methods/Variables**: camelCase (`getUserName`, `$firstName`)
- **Routes**: kebab-case (`/open-source`, `/user-profile`)
- **Config files**: kebab-case (`pdf-generator.php`)
- **Config keys**: snake_case (`chrome_path`)
- **Artisan commands**: kebab-case (`php artisan delete-old-records`)

## File Structure
- Controllers: plural resource name + `Controller` (`PostsController`)
- Views: camelCase (`openSource.blade.php`)
- Jobs: action-based (`CreateUser`, `SendEmailNotification`)
- Events: tense-based (`UserRegistering`, `UserRegistered`)
- Listeners: action + `Listener` suffix (`SendInvitationMailListener`)
- Commands: action + `Command` suffix (`PublishScheduledPostsCommand`)
- Mailables: purpose + `Mail` suffix (`AccountActivatedMail`)
- Resources/Transformers: plural + `Resource`/`Transformer` (`UsersResource`)
- Enums: descriptive name, no prefix (`OrderStatus`, `BookingType`)

## Migrations
- Do not write down methods in migrations, only up methods

## Code Quality Checklist
- Use typed properties over docblocks
- Prefer early returns over nested if/else
- Use constructor property promotion when all properties can be promoted
- Avoid `else` statements when possible
- Use string interpolation over concatenation
- Always use curly braces for control structures

---
*Based on Spatie's Laravel & PHP guidelines*
