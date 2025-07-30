---
# theme: seriph
theme: default
title: What's new in Filament v4
#   Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slides
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
layout: intro
---

# Preparation
- ⚠️ TEST: Do Examples work?
- 💧 Prepare some water
- 🧘🏼 Focus: Do not disturb
- 🕵🏼 Arc: Filament Talk Space
- 📈 Start presentation & <em>Screen Mirror Mode & Whole Window</em>
- 💻 Make both full screen (Toggle with F)
- 👨🏼‍💻 Open Demo Code in PhpStorm

---


---
layout: statement
class: text-center
---

# 🚀<br>What's new in<br><em>Filament v4</em>

<!--
Today I'm talking about all the great stuff you can expect with Filament v4.

Try to demonstrate that with code samples and demos.
-->

---
layout: intro
---

# <strong>Dennis Koch</strong>

<div class="leading-8 mb-6">
  📍 Freiburg, Germany<br>
  💼 Freelance Full-Stack Developer<br>
  👨🏼‍💻 Filament Core Team Member<br>
</div>

<div class="leading-10 mb-6">
<lucide-github class="size-6 translate-y-1" /> <a href="https://github.com/pxlrbt">pxlrbt</a><br>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="inline -ml-0.5 size-6 translate-y-0.25">
  <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5">
    <path d="M7 13.5V9c0-3 5-3 5 0v3m5 1.5V9c0-3-5-3-5 0v3" />
    <path d="M8 17c7.5 1 13 0 13-4V9c0-5.5-4-6.5-6-6.5H9c-3 0-6.067 1-5.863 6.5c.074 1.987.036 4.385.363 7c1 8 10.5 5.5 12 5v-1.5S7.5 21 8 17" />
  </g>
</svg> <a href="https://phpc.social/@denniskoch">phpc.social/@denniskoch</a><br>
<lucide-mail class="size-6 translate-y-0.5" /> <a href="mailto:hey@denniskoch.dev">hey@denniskoch.dev</a><br>
</div>

<img src="./assets/me.jpg" class="absolute size-64 top-16 right-16   rounded-3xl" />

<!--
## Me
- Still quite new here.
- Freiburg across the border – Thanks for meetup asylum
- Filament: Mostly support on Discord

## Social
- pxlrbt: No so smart short version of German company name
- Learned from that => Real name on mastodon
-->

---
layout: intro
---

<div class="grid grid-cols-2 items-center">
<div>

# <b><em>Fila</em>…what?</b>

- Started as a classic admin panel
- Free and open source
- Based on Livewire
- Today: Full-blown app builder
- Server-Driven UI (SDUI) framework

</div>
<div>

```php
public static function table(Table $table): Table
{
    return $table->columns([
        ImageColumn::make('image'),

        TextColumn::make('customer.name')
            ->searchable()
            ->sortable(),

        TextColumn::make('status')
            ->badge(),
    ]);
}
```

</div>
</div>

<!--
# 💧
## Intro
- We had 1 or 2 Filament talks already
- Anyone doesn't know what Filament is?
- Anyone that hasn't used Filament yet?
- Let's just pretend

## Recap
-  Livewire is what makes it great and flexible
-  SDUI: Isn't that what PHP is? Maybe call it Server-configured UI

## Team
- Dan: Mostly Coding
- Rest: Support & Help, Administration, Design, Marketing

## Recent Talks
- Stefan gave a talk 3 months ago.
- Christoph & Luc: 8 months ago.
-->

---
layout: section
---

# 👀<br>Overview

Performance | DX | New Components

<!--
- Tried to categorise

This time no new Livewire version, let's see how long this lasts. Tonight Laracon.
-->

---
layout: section
---

# 🚀<br>Performance

Rendering | Partials | JS Methods

<!--
## Livewire
- Based on Livewire
- We trade a bit of performance for DX

## Old State
- Performance wasn't that bad
- But bad compared to simple Livewire apps and Frontend Frameworks
- Especially big tables and forms
-->

---

# 🚀 Rendering

Rendering is the biggest part of the request.

<div class="grid grid-cols-2 gap-8">
<v-clicks>

<div>

### Tailwind to CSS classes

```css
.fi-badge {}
.fi-wi-stats-overview-stat {}
.fi-ta-actions {}
```
</div>

<div>

###  Reduced Blade components

```php
public function toEmbeddedHtml(): string
{
    <?php ob_start(); ?>

    <div <?= $attributes->toHtml() ?>>
        <?php if (filled($placeholder !== null)) { ?>
            <p class="fi-ta-placeholder">
                <?= e($placeholder) ?>
            </p>
        <?php } ?>
    </div>

    <?php
    return ob_get_clean();
}
```

</div>
</v-clicks>
</div>

<!--
- Ed asked how we handled this in Stefan's talk.
- Rendering takes longest (unless you create fancy DB queries)
- Always thought size of HTML was biggest issue.

[click]
## TW
- Replace inline TW with `fi-` classes
- Use Tailwinds `@apply`
- Use Tailwinds CSS Vars

[click]
## Blade
- Heavily use Blade components
- Components were the biggest bottleneck.
- Now: Render via PHP

## Compare
100 rows table with 7 columns

- Before 3872 views
- After 122 views

[click]
- `view()` takes about 20x longer to render than plain PHP
Every column was a Blade component.
- 7 columns * 100 row => 700 columns
- More with Badges, Icons, ...

-->

---
layout: center
---

# 🚀 Benchmarks

<div>

| Records | Size Before | Size After | %     | Speed Before | Speed After | %   |
|---------:|-------------:|------------:|-------:|--------------:|-------------:|-----:|
| 50      | 53 kB       | 39 kB      | <em>26.5%</em> | 270ms        | 170ms       |  <em>37%</em> |
| 100     | 87 kB       | 63 kB      | <em>27.5%</em> | 450ms        | 250ms       | <em>44%</em> |
| 500     | 362 kB      | 255 kB     | <em>29.5%</em> | 1.77s        | 720ms       | <em>59%</em> |
| 1000    | 704 kB      | 493 kB     | <em>29.9%</em> | 3.6s         | 1.3s        | <em>64%</em> |

<small>*`OrderResource` from Filament Demo on a MacBook Pro M4</small>

</div>

<style>
em {
  font-weight: bold;
  color: #16a34a;
}
</style>

<!--
- Big improvements all over the board.
- The bigger the better
-->
---

# 🚀 Partials

Livewire gives us **flexibility** — but also **limits performance**.

<svg width="100%" height="400" viewBox="0 0 600 400" xmlns="http://www.w3.org/2000/svg" class="border-2 border-color-gray-700 rounded-xl" :class="{'border-color-red-600': $clicks > 2}" v-show="$clicks > 0 && $clicks < 4">

  <rect class="badge-bg" x="-125" y="10" width="70" height="25" />
  <text class="badge-text" x="-90" y="25">Before</text>

  <rect class="form-field" x="50" y="50" width="200" height="36" v-click="1" />
  <rect class="form-field highlighted-src" x="50" y="50" width="200" height="36" v-click="2" />
  <rect class="form-field" x="50" y="104" width="200" height="36"/>
  <rect class="form-field" x="50" y="158" width="200" height="36"/>
  <rect class="form-field" x="50" y="212" width="200" height="36" v-click="1"/>
  <rect class="form-field highlighted-effect" x="50" y="212" width="200" height="36" v-click="2"/>
  <rect class="form-field" x="50" y="266" width="200" height="36"/>

  <rect class="form-field" x="350" y="50" width="200" height="36"/>
  <rect class="form-field" x="350" y="104" width="200" height="36" v-click="1" />
  <rect class="form-field highlighted-effect" x="350" y="104" width="200" height="36" v-click="2"/>
  <rect class="form-field" x="350" y="158" width="200" height="36"/>
  <rect class="form-field" x="350" y="212" width="200" height="36"/>
  <rect class="form-field" x="350" y="266" width="200" height="36"/>

  <rect class="form-field" x="200" y="330" width="200" height="38" rx="19"/>
</svg>


<svg width="100%" height="400" viewBox="0 0 600 400" xmlns="http://www.w3.org/2000/svg" class="border-2 border-color-gray-700 rounded-xl" v-show="$clicks > 3">

  <rect class="badge-bg" x="-125" y="10" width="70" height="25" />
  <text class="badge-text" x="-90" y="25">After</text>

  <rect class="form-field" x="50" y="50" width="200" height="36" v-click="4" />
  <rect class="form-field highlighted-src" x="50" y="50" width="200" height="36" v-click="5" />
  <rect class="form-field" x="50" y="104" width="200" height="36"/>
  <rect class="form-field" x="50" y="158" width="200" height="36"/>
  <rect class="form-field" x="50" y="212" width="200" height="36" v-click="4"/>
  <rect class="form-field highlighted-effect" x="50" y="212" width="200" height="36" v-click="5"/>
  <rect class="form-field highlighted-rerender" x="50" y="212" width="200" height="36" v-click="6"/>
  <rect class="form-field" x="50" y="266" width="200" height="36"/>

  <rect class="form-field" x="350" y="50" width="200" height="36"/>
  <rect class="form-field" x="350" y="104" width="200" height="36" v-click="4" />
  <rect class="form-field highlighted-effect" x="350" y="104" width="200" height="36" v-click="5"/>
  <rect class="form-field highlighted-rerender" x="350" y="104" width="200" height="36" v-click="6"/>
  <rect class="form-field" x="350" y="158" width="200" height="36"/>
  <rect class="form-field" x="350" y="212" width="200" height="36"/>
  <rect class="form-field" x="350" y="266" width="200" height="36"/>

  <rect class="form-field" x="200" y="330" width="200" height="38" rx="19"/>
</svg>

<span v-click="6"></span>

<style>
    .form-field {
      fill: #23272e;
      stroke: #363b42;
      stroke-width: 1.5;
      rx: 10;
      transition: stroke 0.2s;
    }
    .highlighted-src {
      stroke: #3b82f6;
      stroke-width: 3;
    }
    .highlighted-effect {
      stroke: #0d9488;
      stroke-width: 3;
    }
    .highlighted-rerender {
      stroke: #d32f2f;
      stroke-width: 3;
    }
    .badge-bg {
        fill: #3b82f6;
        rx: 10;
        filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
    }
    .badge-text {
      fill: #fff;
      font-size: 14px;
      dominant-baseline: middle;
      text-anchor: middle;
    }
</style>

<!--
# 💧

[click]
## One Component
- Whole Page (containing Table/Form)
- Performance gets worse the bigger it gets

## Separate components
- Communication between needed
- More complexity

[click:4]
## Solution: Partials
- Render single components/views
- Reduce transferred HTML size
- Morph DOM

[click]
## How it works
- `afterStateUpdated()` => Which components to render?
- Register Partial Rendering for component (Partials are extracted into single views)
- Mark request as `skipRender` so Livewire doesn't do a full render
- Add partials as `effect`
- JS: Find `wire:partials` and morph DOM with new HTML

- Hooks into Lifecycle => Skips render

- Automatically used for
  - Action Modals
  - Tables Filter Form
  - Every Form/Infolist component

## Improvements
- Less Rendering
- Less HTML passed over the wire
- Less morphing

## Files
- HasState::callAfterStateUpdated()
- PartialsComponentHook (Livewire uses "Hooks" for all Features)

## Livewire Payload:
- memo: Used to create the component
- state: Stores the current state
- calls: Methods to call with their params
- updates: Which properties to update
-->

---

# 🚀 Partial Render Self

### `->partiallyRenderAfterStateUpdated()`
<br>

<Grid>
<div>

```php{4-13}
Forms\Components\TextInput::make('password')
    ->live()
    ->password()
    ->partiallyRenderAfterStateUpdated()
    ->belowContent(
        fn ($state) => match (true) {
            strlen($state) > 12 => '🎉 Awesome!',
            strlen($state) > 8 => '🙂 Yeah okay',
            strlen($state) > 4 => '😒 Try harder.',
            strlen($state) > 0 => '😢 This sucks!',
            default => ''
        }
    ),
```
</div>
<div>

<Demo page="partially-render-after-state-updated" height="250px" />

</div>
</Grid>

<Docs
  href="https://filamentphp.com/docs/4.x/forms/overview#field-partial-rendering"
  absolute
>
  Field Partial Rendering
</Docs>

<!--
# 💧

- Maybe your password rules are more complex – I hope they are not
-->

---

# 🚀 Partial Render Other Components

#### `->partiallyRenderComponentsAfterStateUpdated()`

<br>

<Grid>
<div>


```php{5-8}
use Filament\Forms\Components\TextInput;

Forms\Components\Select::make('level')
  ->default('error')
  ->live()
  ->partiallyRenderComponentsAfterStateUpdated([
    'logs'
  ])
  ->options([
      'debug' => 'Debug',
      'error' => 'Error',
  ]),

Infolists\Components\CodeEntry::make('logs')
  ->state(
    fn (Get $get) => (new GetLogs)($get('level'))
  ),
```

</div>
<div>
  <Demo
    page="partially-render-components-after-state-updated"
    contained="false"
    height="350px"
  />
</div>
</Grid>

<Docs
  href="https://filamentphp.com/docs/4.x/forms/overview#field-partial-rendering"
  absolute
>
  Field Partial Rendering
</Docs>


---
layout: section
---

# 🚀 <br> JavaScript Methods

No server loop required.

<!--
- We don't want to write a lot of JS, but if it's just some simple one-liners, why not?
-->
---

# Show/Hide via JavaScript

### `->hiddenJs()`/`->visibleJs()`
<br>

<Grid>

```php
Forms\Components\Toggle::make('published'),

Forms\Components\DatePicker::make('published_at')
    ->visibleJs(<<<'JS'
        $get('published')
    JS)
```

<Demo page="visible-js" />

</Grid>

<Docs
  absolute
  href="https://filamentphp.com/docs/4.x/infolists/overview#hiding-an-entry-using-javascript"
>
Hiding an entry using JavaScript
</Docs>

<!--
- Show additional fields based on some condition
- More useful if you show/hide a big section of fields
-->
---

# Update Fields via JavaScript

### `->afterStateUpdatedJs()`

<br>

<Grid>

```php{2,5-6}
Forms\Components\TextInput::make('name')
    ->afterStateUpdatedJs(<<<'JS'
        slugify = str => str.toLowerCase();

        $set('slug', slugify($state));
    JS),

Forms\Components\TextInput::make('slug')
    ->readOnly(),
```

<Demo page="after-state-updated" />

</Grid>

<Docs
  absolute
  href="https://filamentphp.com/docs/4.x/forms/overview#preventing-the-livewire-component-from-rendering-after-a-field-is-updated"
>
Preventing the Livewire component from rendering after a field is updated
</Docs>

<!--
- Type: What's new in Filament v4?
- Livewire version was laggy and might drop characters
-->

---

# Output Content via JavaScript

### `JsContent::make()`
<br>

<Grid>

```php
Forms\Components\TextInput::make('name')
  ->maxLength(20)
  ->hint(JsContent::make(<<<'JS'
      ($state?.length ?? 0) + '/20'
  JS)),
```

<Demo page="js-content" />

</Grid>

<Docs
  href="https://filamentphp.com/docs/4.x/forms/overview#using-javascript-to-determine-text-content"
  absolute
>
  Using JavaScript to determine text content
</Docs>


<!--
- Example: Char Counter
- Works with all methods that accept `Htmlable`.

Renders: `<span x-text="() => eval({$content})"></span>`

```js
window.Alpine.data(
    'filamentSchemaComponent',
    ({ path, containerPath, isLive, $wire }) => ({
        $statePath: path,
        $get: (path, isAbsolute) => {
            return $wire.$get(
                resolveRelativeStatePath(containerPath, path, isAbsolute),
            )
        },
        $set: (path, state, isAbsolute, isUpdateLive = null) => {
            isUpdateLive ??= isLive

            return $wire.$set(
                resolveRelativeStatePath(containerPath, path, isAbsolute),
                state,
                isUpdateLive,
            )
        },
        get $state() {
            return $wire.$get(path)
        },
    }),
)
```
-->

---

# 🚀 Bulk Actions

<div class="grid grid-cols-2 gap-8">
<div v-click>

### Before

```json
"updates": {
    "selectedTableRecords.4": "5",
    "selectedTableRecords.5": "6",
    "selectedTableRecords.6": "7"
    "...",
    "selectedTableRecords.9999": "1000"
}
```

- Send all selected items over the wire
- 0.3 MB for 10.000 records
- Might be declined by your server
- Crashing your browser

</div>
<div v-click>

### After

```json
"updates": {
    "isTrackingDeselectedTableRecords": true,
    "deselectedTableRecords.0": "1",
    "deselectedTableRecords.1": "2",
    "deselectedTableRecords.2": "3",
    "deselectedTableRecords.3": "4"
}
```

- Only send necessary info
- 4 KB for 10.000 records
- Browser Happy
- Server Happy

</div>
</div>

---

# 🚀 Bulk Actions

<Grid>

<div>

### Chunking

```php
BulkAction::make()
    ->chunkSelectedRecords(250)
    ->action(function (LazyCollection $records) {
        // Process the records...
    })
```

</div>
<div v-click>

### No Fetching

```php
BulkAction::make()
    ->fetchSelectedRecords(false)
    ->action(function (Collection $records) {
        // Process the records...
    })
```

</div>
</Grid>

<Docs
  absolute
  href="https://filamentphp.com/docs/4.x/tables/actions#improving-the-performance-of-bulk-actions"
>
  Improving the performance of bulk actions
</Docs>


<!--
Load sets of records instead of all at once

[click]

Just get the IDs. Useful if you want to bulk update via DB anyway
-->

---
layout: section
---

# 👨🏼‍💻 <br> Developer Experience

Unified Actions | Schemas | Static Tables

<!--
# 💧
-->

---

# 👨🏼‍💻 Unified Actions

One class to rule them all.

<Grid class="item-center justify-center">
<v-clicks>
<div>

- `Filament\Tables\Actions`
- `Filament\Pages\Actions`
- `Filament\Notifications\Actions`
- `Filament\GlobalSearch\Actions`
- `Filament\Forms\Components\Actions`
- `Filament\Infolists\Components\Actions`
- `Filament\Actions`

</div>

<Arrow x1="440" y1="240" x2="590" y2="240" />

<div class="grid place-items-center">

- `Filament\Actions`

</div>
</v-clicks>
</Grid>

<!--
- Less duplicate code
- Only `Actions` or `BulkActios`
- No more bad namespace imports (`Tables/Action`, `Pages/Action`, `Components/Action`)
- Reuse them in Tables, Forms, Infolist, ...
- Use them in multiple places: Header Actions, Toolbar (Bulk Actions),
-->

---

# 👨🏼‍💻 Schemas

Mix and match as you like

<v-click>
<Grid>

```php{1,4,8,15,18}
public function form(Schema $schema): Schema
{
  return $schema->components([
    Schemas\Components\Actions::make([
      Action::make('Special')->action(...),
    ]),

    Forms\Components\Select::make('gif')
      ->live()
      ->options([
        asset('noice.gif') => 'Noice 1',
        asset('noice-2.gif') => 'Noice 2',
      ]),

    Infolist\Components\ImageEntry::make('image')
      ->state(fn ($get) => $get('gif'))

    Schemas\Components\Livewire::make(MyComp::class)
  ]);
}
```

<Demo page="combined-schema" />

</Grid>
</v-click>

<Docs
  href="https://filamentphp.com/docs/4.x/schemas/overview"
  absolute
>
  Schemas
</Docs>

<!--
- Started with Tables & Forms
- v3: Infolists
- v4: Combined => Schemas
- Placeholder/View not needed that much anymore
-->

---
layout: full
---

# 👨🏼‍💻 Page Schemas

Mix and match the whole page.

<div style="width: 28rem;">

<div v-show="$clicks == 1">

```php
public function content(Schema $schema) {
    return $schema->components([
        'Some simple content'
    ]);
}
```

</div>

<div v-show="$clicks == 2">

```php
public function content(Schema $schema) {
    return $schema->components([
        new HtmlString(<<<HTML
          <h1 style="font-size: 2rem">
            An HTML String
          </h1>
        HTML),

        svg('heroicon-c-rocket-launch')
    ]);
}
```

</div>

<div v-show="$clicks == 3">

```php
public function content(Schema $schema) {
    return $schema->components([
        view('laravel')
    ]);
}
```

</div>
<div v-show="$clicks == 4">

```php
public function content(Schema $schema) {
    return $schema->components([
        Livewire::make(DemoComponent::class),
    ]);
}
```
</div>

<div v-show="$clicks == 5">

```php
public function content(Schema $schema) {
    return $schema->components([
        EmbeddedTable::make()
    ]);
}
```
</div>

<div v-show="$clicks == 6">

```php
public function content(Schema $schema) {
    return $schema->components([
        Form::make()->schema([
            Forms\Components\TextInput::make('name')
        ])
    ]);
}
```
</div>
</div>

<Demo
    :page="'page-schema?activeTab='+($clicks-1)"
    class="absolute top-0 right-0 bottom-0"
    :contained="false"
    width="500px"
    height="550px"
/>

<span v-click="6"></span>

<Docs
  href="https://filamentphp.com/docs/4.x/schemas/overview"
  absolute
>
  Schemas
</Docs>

<!--
## Flexibility
- This gives full flexibilty
- No need to publish Blade files anymore
- Use anything that can be rendered

## Reuse
- Reuse Filament defined via methods: Forms, Actions, Relation Managers, ...
-->

---

# 👨🏼‍💻 Static Tables

Before: Sushi – Lots of limitations

<Grid>

```php
class SubscriptionApiWrapper extends Model
{
    use Sushi;

    public function getRows(): array
    {
        $resp = Http::get('https://your-api.com');

        return $resp->json('data');
    }
}
```

</Grid>

<!--
Highly requested.
Example: Integrating 3rd Party APIs
So far: Sushi

[click]

Sushi was meant for static data. Not paginated APIs
Filtering, searching, relationships, ... not possible.
-->
---
layout: full
---

# 👨🏼‍💻 Static Tables

Now: Static data via `->records()`.

<div style="width: 29rem">

```php
return $table
  ->records(function ($search, $sortColumn, $sortDirection) {

    return collect(File::json('meetups.json'))
      ->filter(function ($record) use ($search) {
        if (blank($search)) return true;

        return stripos($record['name'], $search) !== false;
      })
      ->sortBy(
          $sortColumn,
          descending: $sortDirection === 'desc'
      );
});
```
</div>

<div v-click="2" style="width: 29rem">

```php
fn (
  $sortColumn, $sortDirection,
  $page, $recordsPerPage
  $search, $filters,
)
```

</div>

<div v-click>
<Demo
    page="static-table"
    :contained="false"
    class="absolute top-0 right-0 bottom-0 width-full"
    width="450px"
    height="550px"
/>
</div>

<Docs
  href="https://filamentphp.com/docs/4.x/tables/custom-data"
  absolute
>
  Tables: Custom Data
</Docs>

<!--
## Use Case
- Static Data (JSON, ...)
- API

## Features
- All table features
- Custom Actions


## Todo
- TODO: Do Summarizers work?
- TODO: How to use Actions?
-->
---
layout: full
---

# 👨🏼‍💻 Nested Resources

#### Relation Manager
<div style="max-width: 30rem">
```php
class MeetupsRelationManager extends RelationManager
{
  protected static $relatedResource = MeetupResource::class;
}
```

#### Nested Resource

```php
class MeetupResource extends Resource
{
    protected static $parentResource = LocationResource::class;
}
```
</div>

<div v-click>
<Demo
    page="locations"
    :contained="false"
    class="absolute top-0 right-0 bottom-0 width-full"
    width="450px"
    height="550px"
/>
</div>

<Docs
  href="https://filamentphp.com/docs/4.x/resources/nesting"
  absolute
>
  Nested Resources
</Docs>

<!--
## Before
- RM limited to 1 level

## Now
- Nest as deep as you want
- Easy to generate
-->

---

# 🔐 2FA

Choose between Authenticator App or Email

<div class="w-2/3">
<div v-click>

```php
$panel
    ->profile()
    ->multiFactorAuthentication([
        AppAuthentication::make(),
    ]);
```

</div>
<br>
<div v-click>

#### Prepare User Model

```php
Schema::table('users', function (Blueprint $table) {
    $table->text('app_authentication_secret')->nullable();
});
```

</div>

<div v-click>

```php
interface HasAppAuthentication
{
    public function getAppAuthenticationSecret(): ?string;
    public function saveAppAuthenticationSecret(?string $secret): void;
    public function getAppAuthenticationHolderName(): string;
}
```

</div>
</div>

<Docs
    href="https://filamentphp.com/docs/4.x/users/multi-factor-authentication"
>
    Multi-Factor Authentication
</Docs>

<!--
## Current
- So far 2FA through Breezy

## New
- Baked in 2FA
- Profile needed for 2FA management
- Add migration and implement interface on User model

## ...
- Optional or required (will redirect after login)
- Only 1 authenticator possible
-->

---
layout: section
---

# ✨<br>New Components

<!--
# 💧
## Quick Walkthrough
-->

---
layout: center
class: text-center
---

# FusedGroup

<Demo
  page="fused-group"
  style="width: 800px"
  width="1100px"
  height="150px"
  />

<Docs
    href="https://filamentphp.com/docs/4.x/forms/overview#fusing-fields-together-into-a-group"
    center
    absolute
>
    Fusing fields together into a group
</Docs>

---
layout: center
class: text-center
---

# Slider

<Demo
    page="slider"
    style="width: 600px; padding: 1rem"
/>

<Docs
    href="https://filamentphp.com/docs/4.x/forms/slider"
    center
    absolute
>
    Forms: Slider
</Docs>
<!--
- 2 Modes: Single or Range
- noUI Slider under the hood
-->

---
layout: center
class: text-center
---

# Code Editor

<Demo page="code-editor" :contained="false" class="w-[40rem]" />

<Docs
    href="https://filamentphp.com/docs/4.x/forms/code-editor"
    center
    absolute
>
    Forms: Code Editor
</Docs>

<!--
## Facts
- Codemirror
- Supports 12 Languages
- One Theme: One Dark

## Support
Supports: C++, CSS, Go, HTML, Java, Javascript, JSON, Markdown, PHP, Python, XML, YAML
Theme: One Dark
-->

---
layout: center
class: text-center
---

# Code Entry

<Demo page="code-entry" :contained="false" style="margin-top: -1rem; width: 40rem;" height="450px" />

<Docs
    href="https://filamentphp.com/docs/4.x/infolists/code-entry"
    center
    absolute
>
    Infolists: Code Entry
</Docs>

<!--
Phiki (Ryans Shiki port)
- Fast rendering
- Lots of Grammars
- Beautiful Themes
-->

---
layout: center
class: text-center
---

# Table Repeater

<Demo page="table-repeater" :contained="false"  style="width: 40rem;" height="360px" />

<Docs
    href="https://filamentphp.com/docs/4.x/forms/repeater#table-repeaters"
    center
    absolute
>
    Forms: Table Repeaters
</Docs>

<!--
- Adams Table Repeater
- No separate component
- Just configure `->table()`
- Reordering, deleting, adding
- Not a separate component
-->

---
layout: center
class: text-center
---

# Modal Table Select

<Demo
    page="modal-table-select"
    :contained="false"
    style="zoom: 0.6; width: 60rem"
    height="35rem"
/>

<Docs
    href="https://filamentphp.com/docs/4.x/forms/select#selecting-options-from-a-table-in-a-modal"
    center
    absolute
>
    Forms: Selecting options from a table in a modal
</Docs>


<!--
## Use Case
- Similar to Select
- But more options (pagination, filtering, ...)
-->

---
layout: center
class: text-center
---

# RichEditor

<Demo
  page="rich-editor"
  :contained="false"
  style="width: 60rem; zoom: 0.8"
  width="1200px"
  height="450px" />

<Docs
    href="https://filamentphp.com/docs/4.x/forms/rich-editor"
    center
    absolute
>
    Forms: Rich Editor
</Docs>

<!--
- Replaced Trix (Good but simple)
- Based on TipTap

## Features
- Store as JSON or HTML
- Custom Blocks
- Merge Tags
- Private Files
-->

---
layout: default
---

# ✨ What else?
- A11Y improvements
- Timezone setting via `FilamentTimezone`
- Sidebar & Topbar as Livewire components
- Vertical Tabs
- Icon Enum
- New file structure
- Better Documentation
- ...

---
layout: intro
---

# 🚧 Current Status

- v4 still in Beta
- Still some issues reported on GitHub
- Probably stable in next 1–2 months

---
layout: intro
---

# 🕵🏼‍♂️ How you can help

- Test the beta
- Report issues on GitHub
- Spread the word
- Sponsor the project

<div class="flex gap-2 justify-center text-sm bottom-8 left-14 absolute">
    <lucide-github class="size-5 translate-y-1" />
    <a class="inline-block mt-0.5" href="https://github.com/sponsors/danharrin">https://github.com/sponsors/danharrin</a>
</div>

<!--
## Sponsor
- You or Employer
-->

---
layout: center
class: text-center
---

# <b><em>Thank you!</em></b>

Questions?

<!--
# 💧
-->
