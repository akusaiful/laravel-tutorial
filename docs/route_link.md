# Routing

## Basic

```html
<a
  href="{{ route('company.index') }}"
  title="Theme Modes (beta)"
  data-filter-tags="theme settings theme modes (beta)"
>
  <span class="nav-link-text" data-i18n="nav.theme_settings_theme_modes_(beta)"
    >Senarai Syarikat</span
  >
</a>
```

Example using x-component

```html
<x-nav-link
  href="{{ route('counter') }}"
  :active="request()->routeIs('counter')"
>
  {{ __('Counter') }}
</x-nav-link>
```

