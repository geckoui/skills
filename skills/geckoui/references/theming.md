# GeckoUI Theming Reference

GeckoUI uses CSS custom properties with OKLCH color values. Override with any CSS color format: `oklch()`, `hex`, `rgb()`, `hsl()`.

## How to Create a Custom Theme

Create a CSS file that overrides the `--color-*` variables, import it AFTER `@geckoui/geckoui/styles.css`:

```tsx
import "@geckoui/geckoui/styles.css";

import "./my-theme.css";
```

## Complete Variable Reference

### Primary Colors (brand color scale)

These control buttons, focus rings, active states, and all accent colors.

```css
:root {
  --color-primary-50: oklch(0.9705 0.0142 254.6); /* lightest */
  --color-primary-100: oklch(0.9319 0.0316 255.59);
  --color-primary-200: oklch(0.8823 0.0571 254.13);
  --color-primary-300: oklch(0.8091 0.0956 251.81); /* focus rings */
  --color-primary-400: oklch(0.7137 0.1434 254.62); /* hover borders */
  --color-primary-500: oklch(0.6231 0.188 259.81);
  --color-primary-600: oklch(0.5461 0.2152 262.88); /* primary button bg */
  --color-primary-700: oklch(0.4882 0.2172 264.38); /* primary button hover */
  --color-primary-800: oklch(0.4244 0.1809 265.64);
  --color-primary-900: oklch(0.3791 0.1378 265.52);
  --color-primary-950: oklch(0.2823 0.0874 267.94); /* darkest */
}
```

### Surface Colors (backgrounds)

```css
:root {
  --color-surface-primary: oklch(1 0 none); /* main background */
  --color-surface-secondary: oklch(0.9851 0 none); /* secondary bg */
  --color-surface-tertiary: oklch(0.9702 0 none); /* tertiary bg */
  --color-surface-hover: oklch(0.9702 0 none); /* hover state */
  --color-surface-hover-strong: oklch(0.9401 0 none); /* strong hover (menus) */
  --color-surface-active: oklch(0.8699 0 none); /* active/pressed */
  --color-surface-disabled: oklch(0.9401 0 none); /* disabled bg */
  --color-surface-overlay: oklch(0 0 none); /* backdrop overlay */
  --color-surface-autofill: oklch(0.9702 0 none); /* browser autofill */
  --color-surface-emphasis: oklch(0.4461 0.0263 256.8); /* emphasis bg */
}
```

### Text Colors

```css
:root {
  --color-text-primary: oklch(0.2046 0 none); /* main text */
  --color-text-secondary: oklch(0.3715 0 none); /* secondary text */
  --color-text-tertiary: oklch(0.5555 0 none); /* tertiary text */
  --color-text-disabled: oklch(0.7155 0 none); /* disabled text */
  --color-text-placeholder: oklch(0.7155 0 none); /* placeholder */
  --color-text-inverse: oklch(0.9851 0 none); /* text on dark bg */
  --color-text-muted: oklch(0.5555 0 none); /* muted text */
  --color-text-on-primary: oklch(1 0 none); /* text on primary color */
}
```

### Border Colors

```css
:root {
  --color-border-primary: oklch(0.9401 0 none); /* default border */
  --color-border-secondary: oklch(0.8699 0 none); /* input borders */
  --color-border-focus: oklch(0.7155 0 none); /* focus state */
  --color-border-hover: oklch(0.8699 0 none); /* hover state */
  --color-border-disabled: oklch(0.9401 0 none); /* disabled border */
  --color-border-invalid: oklch(0.6368 0.2078 25.33); /* a field in error */
  --color-border-invalid-hover: oklch(0.5168 0.2178 25.33); /* ...under the pointer */
}
```

### Semantic Colors

Status colours, shared by Toast, Alert and Badge. New in v2.

```css
:root {
  --color-success: oklch(0.6271 0.1699 149.21);
  --color-error: oklch(0.6368 0.2078 25.33);
  --color-warning: oklch(0.6685 0.1626 58.32);
  --color-info: oklch(0.6231 0.188 259.81);
}
```

### Scrollbar Colors

These use the `--gecko-ui-` prefix (not Tailwind utilities):

```css
:root {
  --gecko-ui-scrollbar-track: oklch(0 0 none);
  --gecko-ui-scrollbar-thumb: oklch(0.8717 0.0093 258.34);
  --gecko-ui-scrollbar-thumb-hover: oklch(0.7137 0.0192 261.32);
}
```

## Dark Mode

Apply `.dark` class to root element. Override the same variables:

```css
.dark {
  --color-surface-primary: oklch(0.2046 0 none);
  --color-surface-secondary: oklch(0.2435 0 none);
  --color-surface-tertiary: oklch(0.2972 0 none);
  --color-surface-hover: oklch(0.2972 0 none);
  --color-surface-hover-strong: oklch(0.3715 0 none);
  --color-surface-active: oklch(0.5555 0 none);
  --color-surface-disabled: oklch(0.2972 0 none);
  --color-surface-overlay: oklch(0 0 none);
  --color-surface-autofill: oklch(0.2435 0 none);
  --color-surface-emphasis: oklch(0.7748 0.0054 247.89);

  --color-text-primary: oklch(0.9851 0 none);
  --color-text-secondary: oklch(0.8699 0 none);
  --color-text-tertiary: oklch(0.7155 0 none);
  --color-text-disabled: oklch(0.5555 0 none);
  --color-text-placeholder: oklch(0.5555 0 none);
  --color-text-inverse: oklch(0.2046 0 none);
  --color-text-muted: oklch(0.7155 0 none);

  --color-border-primary: oklch(0.4676 0 none);
  --color-border-secondary: oklch(0.4676 0 none);
  --color-border-focus: oklch(0.5555 0 none);
  --color-border-hover: oklch(0.7155 0 none);
  --color-border-disabled: oklch(0.2972 0 none);

  --gecko-ui-scrollbar-track: oklch(0 0 none);
  --gecko-ui-scrollbar-thumb: oklch(0.4461 0.0263 256.8);
  --gecko-ui-scrollbar-thumb-hover: oklch(0.551 0.0234 264.36);
}
```

**Note:** Primary colors (50-950) are NOT overridden in dark mode by default. Override them if your brand color needs dark mode adjustment.

## Component Variables

Some components expose their own `--gecko-*` variables, so you can retheme them without
touching their rules. Set them on the component's class, or globally on `:root`.

```css
/* Alert */
--gecko-alert-accent   --gecko-alert-bg   --gecko-alert-border   --gecko-alert-radius

/* Badge */
--gecko-badge-accent   --gecko-badge-on-accent   --gecko-badge-radius
--gecko-badge-soft-mix   --gecko-badge-outline-mix

/* Tabs */
--gecko-tabs-accent   --gecko-tabs-muted   --gecko-tabs-indicator   --gecko-tabs-radius
--gecko-tabs-gap   --gecko-tabs-padding-x   --gecko-tabs-padding-y   --gecko-tabs-font-size

/* Toast */
--gecko-toast-accent   --gecko-toast-bg   --gecko-toast-fg   --gecko-toast-border
--gecko-toast-muted   --gecko-toast-radius   --gecko-toast-shadow   --gecko-toast-width
--gecko-toast-gap   --gecko-toast-offset   --gecko-toast-padding   --gecko-toast-duration
--gecko-toast-z
```

```css
.GeckoUITabs {
  --gecko-tabs-accent: rebeccapurple;
  --gecko-tabs-indicator: 3px;
}
```

`--gecko-scrollbar-width` is set by the library while an overlay locks page scroll, so
fixed elements pinned to the right edge can compensate.

## Class Stacking

Some components pass their class to a base component, so multiple `GeckoUI*` classes end up on the **same DOM element**. Styling the base class affects the wrapper too.

| Element                   | Classes on the same element                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------- |
| `<button>`                | `.GeckoUIButton` + `.GeckoUILoadingButton`                                                   |
| `<label>` (Input wrapper) | `.GeckoUIInput` + `.GeckoUIRHFInput`                                                         |
| `<label>` (Input wrapper) | `.GeckoUIInput` + `.GeckoUIRHFInput` + `.GeckoUIRHFNumberInput`                              |
| `<label>` (Input wrapper) | `.GeckoUIInput` + `.GeckoUIRHFInput` + `.GeckoUIRHFNumberInput` + `.GeckoUIRHFCurrencyInput` |
| `<textarea>`              | `.GeckoUITextarea` + `.GeckoUIRHFTextarea`                                                   |
| `<div>` (Select wrapper)  | `.GeckoUISelect` + `.GeckoUIRHFSelect`                                                       |
| `<div>` (Select button)   | `.GeckoUISelectButton` + `.GeckoUIRHFSelectButton`                                           |
| `<label>` (Switch track)  | `.GeckoUISwitch` + `.GeckoUIRHFSwitch`                                                       |
| `<span>` (Switch thumb)   | `.GeckoUISwitch__thumb` + `.GeckoUIRHFSwitch__thumb`                                         |
| `<div>` (OTP grid)        | `.GeckoUIOTPInput` + `.GeckoUIRHFOTPInput`                                                   |
| `<div>` (Counter wrapper) | `.GeckoUICounterInput` + `.GeckoUIRHFCounterInput`                                           |
| `<div>` (DateRange input) | `.GeckoUIDateInput` + `.GeckoUIDateRangeInput`                                               |

**Impact:** Styling `.GeckoUIInput` also affects `RHFInput`, `RHFNumberInput`, and `RHFCurrencyInput` since they share the same element.

## Component Class Reference

Every CSS class, what HTML element it renders on, and what it targets. Use these for component-level overrides.

### Button

| Class            | Element    | Targets           | Data Attrs                                |
| ---------------- | ---------- | ----------------- | ----------------------------------------- |
| `.GeckoUIButton` | `<button>` | The button itself | `data-variant`, `data-color`, `data-size` |

### LoadingButton

| Class                   | Element    | Targets                                       | Data Attrs     |
| ----------------------- | ---------- | --------------------------------------------- | -------------- |
| `.GeckoUILoadingButton` | `<button>` | Stacks on same `<button>` as `.GeckoUIButton` | `data-loading` |

### Input

| Class                  | Element   | Targets                                                             | Data Attrs                    |
| ---------------------- | --------- | ------------------------------------------------------------------- | ----------------------------- |
| `.GeckoUIInput`        | `<label>` | Outer wrapper (border, flex container with prefix + input + suffix) | `data-state`, `data-readonly` |
| `.GeckoUIInput__input` | `<input>` | The actual text input                                               | —                             |

Built-in: `border`, `rounded-md`, `min-h-10`, `px-3`, `gap-2`.

### Textarea

| Class              | Element      | Targets                     |
| ------------------ | ------------ | --------------------------- |
| `.GeckoUITextarea` | `<textarea>` | The textarea element itself |

Built-in: `border`, `rounded-md`, `px-3`, `py-1.5`, `text-sm`.

### Select

| Class                                                          | Element    | Targets                        | Data Attrs                                              |
| -------------------------------------------------------------- | ---------- | ------------------------------ | ------------------------------------------------------- |
| `.GeckoUISelect`                                               | `<div>`    | Outer wrapper                  | —                                                       |
| `.GeckoUISelectButton`                                         | `<div>`    | The trigger/button area        | `data-state`, `data-readonly`                           |
| `.GeckoUISelectButton__content`                                | `<div>`    | Content area inside button     | —                                                       |
| `.GeckoUISelectButton__value`                                  | `<span>`   | Selected value display         | `data-placeholder`, `data-selected`, `data-hidden`      |
| `.GeckoUISelectButton__search`                                 | `<div>`    | Search input wrapper           | `data-focusonly`, `data-multi-selected`, `data-keyword` |
| `.GeckoUISelectButton__search__input`                          | `<input>`  | The actual search input        | `data-initial`, `data-readonly`                         |
| `.GeckoUISelectButton__icons`                                  | `<div>`    | Icons container (clear, arrow) | —                                                       |
| `.GeckoUISelectButton__clear-button`                           | `<button>` | Clear selection button         | `data-disabled`                                         |
| `.GeckoUISelectButton__multiselected-chip`                     | `<div>`    | Multi-select tag/chip          | `data-disabled`                                         |
| `.GeckoUISelectButton__multiselected-chip__clear-button`       | `<button>` | Remove chip button             | `data-disabled`                                         |
| `.GeckoUISelectButton__multiselected-chip__clear-button__icon` | `<span>`   | Remove chip icon               | —                                                       |
| `.GeckoUISelectMenu`                                           | `<div>`    | Floating dropdown panel        | `data-with-search`                                      |
| `.GeckoUISelectMenu__search-container`                         | `<div>`    | Dropdown search input area     | —                                                       |
| `.GeckoUISelectMenu__items`                                    | `<div>`    | Scrollable options container   | —                                                       |
| `.GeckoUISelectOption`                                         | `<div>`    | Individual option row          | `data-state`, `data-focused`, `data-disabled`           |
| `.GeckoUISelectOption__check-icon`                             | `<div>`    | Check icon when selected       | —                                                       |
| `.GeckoUISelectEmpty`                                          | `<div>`    | Empty state message            | —                                                       |
| `.GeckoUISelectDropdownSearch`                                 | `<label>`  | Dropdown search wrapper        | —                                                       |
| `.GeckoUISelectDropdownSearch__icon`                           | `<div>`    | Search icon                    | —                                                       |

### Menu

| Class                  | Element    | Targets                              | Data Attrs      |
| ---------------------- | ---------- | ------------------------------------ | --------------- |
| `.GeckoUIMenu`         | `<div>`    | Outer wrapper                        | —               |
| `.GeckoUIMenu__button` | `<button>` | Default trigger button               | —               |
| `.GeckoUIMenu__items`  | `<div>`    | Floating dropdown panel (scrollable) | —               |
| `.GeckoUIMenu__item`   | `<div>`    | Individual menu action item          | `data-disabled` |

Built-in panel: `border`, `rounded-md`, `p-1`, `shadow-xl`. Built-in item: `px-3`, `py-2`, `text-sm`, `rounded`.

### Alert

| Class                                | Element    | Targets                            | Data Attrs                     |
| ------------------------------------ | ---------- | ---------------------------------- | ------------------------------ |
| `.GeckoUIAlert`                      | `<div>`    | Alert container                    | `data-color`, `data-condensed` |
| `.GeckoUIAlert__icon`                | `<div>`    | Colour icon                        | `data-color`                   |
| `.GeckoUIAlert__body`                | `<div>`    | Header area (icon + title + close) | —                              |
| `.GeckoUIAlert__title`               | `<div>`    | Title text                         | —                              |
| `.GeckoUIAlert__description`         | `<div>`    | Description text                   | —                              |
| `.GeckoUIAlert__remove-button`       | `<button>` | Close/dismiss button               | —                              |
| `.GeckoUIAlert__remove-button__icon` | `<span>`   | Close icon                         | —                              |

Built-in: `border`, `rounded-lg`, `px-4`, `py-3`.

### Accordion

| Class                                | Element    | Targets           | Data Attrs                                   |
| ------------------------------------ | ---------- | ----------------- | -------------------------------------------- |
| `.GeckoUIAccordion`                  | `<div>`    | `Accordion`       | `data-variant`, `data-size`                  |
| `.GeckoUIAccordion__item`            | `<div>`    | `AccordionItem`   | `data-state="open\|closed"`, `data-disabled` |
| `.GeckoUIAccordion__header`          | `<button>` | `AccordionHeader` | `data-state`, `data-disabled`                |
| `.GeckoUIAccordion__header__content` | `<span>`   | Header contents   | —                                            |
| `.GeckoUIAccordion__header__icon`    | `<span>`   | The chevron       | —                                            |
| `.GeckoUIAccordion__panel`           | `<div>`    | `AccordionPanel`  | `data-state`                                 |
| `.GeckoUIAccordion__panel__content`  | `<div>`    | Panel contents    | —                                            |

Variables: `--gecko-accordion-radius`, `--gecko-accordion-gap`,
`--gecko-accordion-padding-x`, `--gecko-accordion-padding-y`,
`--gecko-accordion-font-size`, `--gecko-accordion-duration`.

The open and close animates a grid row from `0fr` to `1fr` rather than a height, so it
reaches the content's real height with nothing measured in JavaScript.

### Badge

| Class                 | Element  | Targets         | Data Attrs                                              |
| --------------------- | -------- | --------------- | ------------------------------------------------------- |
| `.GeckoUIBadge`       | `<span>` | Badge container | `data-variant`, `data-color`, `data-size`, `data-shape` |
| `.GeckoUIBadge__dot`  | `<span>` | Status dot      | —                                                       |

Variables: `--gecko-badge-accent`, `--gecko-badge-on-accent`, `--gecko-badge-radius`,
`--gecko-badge-soft-mix`, `--gecko-badge-outline-mix`.

### Tabs

| Class                 | Element             | Targets    | Data Attrs                                                         |
| --------------------- | ------------------- | ---------- | ------------------------------------------------------------------ |
| `.GeckoUITabs`        | `<div>`             | `Tabs`     | `data-variant`, `data-size`, `data-orientation`, `data-full-width` |
| `.GeckoUITabs__list`  | `<div>` or `<nav>`  | `TabList`  | —                                                                  |
| `.GeckoUITabs__tab`   | `<button>` or yours | `Tab`      | `data-state="selected\|unselected"`, `data-disabled`               |
| `.GeckoUITabs__panel` | `<div>`             | `TabPanel` | —                                                                  |

Variables: `--gecko-tabs-accent`, `--gecko-tabs-muted`, `--gecko-tabs-indicator`,
`--gecko-tabs-radius`, `--gecko-tabs-gap`, `--gecko-tabs-padding-x`,
`--gecko-tabs-padding-y`, `--gecko-tabs-font-size`.

### Toast

| Class                        | Element    | Targets               | Data Attrs                   |
| ---------------------------- | ---------- | --------------------- | ---------------------------- |
| `.GeckoUIToaster`            | `<div>`    | A corner stack        | —                            |
| `.GeckoUIToaster__item`      | `<div>`    | Slot holding a toast  | —                            |
| `.GeckoUIToast`              | `<div>`    | One toast             | `data-variant`, `data-state` |
| `.GeckoUIToast__icon`        | `<div>`    | Variant icon          | —                            |
| `.GeckoUIToast__spinner`     | `<div>`    | Loading spinner       | —                            |
| `.GeckoUIToast__body`        | `<div>`    | Message + description | —                            |
| `.GeckoUIToast__message`     | `<div>`    | Message text          | —                            |
| `.GeckoUIToast__description` | `<div>`    | Description text      | —                            |
| `.GeckoUIToast__actions`     | `<div>`    | Button row            | —                            |
| `.GeckoUIToast__action`      | `<button>` | Action button         | —                            |
| `.GeckoUIToast__cancel`      | `<button>` | Cancel button         | —                            |
| `.GeckoUIToast__close`       | `<button>` | Close button          | —                            |
| `.GeckoUIToast__custom`      | `<div>`    | Fully custom content  | —                            |

Variables: the `--gecko-toast-*` set listed under Component Variables.

### Dialog

| Class                      | Element | Targets                   | Data Attrs   |
| -------------------------- | ------- | ------------------------- | ------------ |
| `.GeckoUIDialog`           | `<div>` | Root fixed overlay        | `data-state` |
| `.GeckoUIDialog__backdrop` | `<div>` | Semi-transparent backdrop | —            |
| `.GeckoUIDialog__dialog`   | `<div>` | The modal panel           | —            |

### ConfirmDialog

| Class                            | Element | Targets                          |
| -------------------------------- | ------- | -------------------------------- |
| `.GeckoUIConfirmDialog__dialog`  | `<div>` | Dialog wrapper                   |
| `.GeckoUIConfirmDialog__title`   | `<div>` | Title (text-base, font-semibold) |
| `.GeckoUIConfirmDialog__content` | `<div>` | Content (text-sm, text-muted)    |
| `.GeckoUIConfirmDialog__actions` | `<div>` | Button row (flex, justify-end)   |

### Drawer

| Class                      | Element | Targets           | Data Attrs                                              |
| -------------------------- | ------- | ----------------- | ------------------------------------------------------- |
| `.GeckoUIDrawer`           | `<div>` | Root container    | —                                                       |
| `.GeckoUIDrawer__backdrop` | `<div>` | Backdrop overlay  | `data-state="visible" \| "hidden"`, `data-clickthrough` |
| `.GeckoUIDrawer__drawer`   | `<div>` | The sliding panel | `data-placement`, `data-state`                          |

### Popover

| Class                      | Element | Targets                       | Data Attrs                  |
| -------------------------- | ------- | ----------------------------- | --------------------------- |
| `.GeckoUIPopover`          | `<div>` | Wraps the trigger             | —                           |
| `.GeckoUIPopover__content` | `<div>` | `PopoverContent`              | `data-state="open\|closed"` |
| `.GeckoUIPopover__arrow`   | `<svg>` | The arrow, when `arrow` is on | —                           |

Variables: `--gecko-popover-bg`, `--gecko-popover-border`, `--gecko-popover-radius`,
`--gecko-popover-padding`, `--gecko-popover-width`, `--gecko-popover-max-width`,
`--gecko-popover-z`.

The trigger is the caller's own element, so it carries no class of ours — style it however
you already style that button.

### Tooltip

| Class                      | Element  | Targets                |
| -------------------------- | -------- | ---------------------- |
| `.GeckoUITooltip__trigger` | `<span>` | Trigger wrapper        |
| `.GeckoUITooltip`          | `<div>`  | Tooltip content bubble |
| `.GeckoUITooltip__arrow`   | `<svg>`  | Arrow element          |

### Calendar

| Class                                    | Element    | Targets                            | Data Attrs                                                                                                                                                                                             |
| ---------------------------------------- | ---------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `.GeckoUICalendar`                       | `<div>`    | Calendar wrapper                   | `data-mode`, `data-selection`                                                                                                                                                        |
| `.GeckoUICalendar__header`               | `<div>`    | Month/year header with arrows      | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__header__title`        | `<button>` | Month/year title (clickable)       | `data-clickable`                                                                                                                                                                                       |
| `.GeckoUICalendar__header__arrow-button` | `<button>` | Navigation arrow                   | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__day-picker-weekdays`  | `<div>`    | Weekday labels row (S M T W T F S) | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__day-picker`           | `<div>`    | Day grid container                 | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__day-picker__button`   | `<button>` | Individual day cell                | `data-today`, `data-selected`, `data-range-start`, `data-range-end`, `data-in-range`, `data-hover-preview`, `data-hover-preview-start`, `data-hover-preview-end`, `data-disabled`, `data-active-month` |
| `.GeckoUICalendar__month-picker`         | `<div>`    | Month grid                         | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__month-picker__button` | `<button>` | Month cell                         | `data-selected`                                                                                                                                                                                        |
| `.GeckoUICalendar__year-picker`          | `<div>`    | Year grid                          | —                                                                                                                                                                                                      |
| `.GeckoUICalendar__year-picker__button`  | `<button>` | Year cell                          | `data-selected`, `data-prev-next`                                                                                                                                                                      |

### DateInput / DateRangeInput

| Class                                     | Element    | Targets                             | Data Attrs                                             |
| ----------------------------------------- | ---------- | ----------------------------------- | ------------------------------------------------------ |
| `.GeckoUIDateInputWrapper`                | `<div>`    | Outer wrapper (includes calendar)   | `data-calendar-open`                                   |
| `.GeckoUIDateInput`                       | `<div>`    | Input container (border, flex)      | `data-state`, `aria-invalid`, `data-empty`, `data-focus` |
| `.GeckoUIDateInput__placeholder`          | `<span>`   | Placeholder text                    | —                                                      |
| `.GeckoUIDateInput__display-container`    | `<div>`    | Date segments container             | —                                                      |
| `.GeckoUIDateInput__segment`              | `<label>`  | Individual segment (day/month/year) | `data-empty`                                           |
| `.GeckoUIDateInput__separator`            | `<span>`   | Separator (/) between segments      | —                                                      |
| `.GeckoUIDateInput__prefix`               | `<div>`    | Prefix icon/text area               | —                                                      |
| `.GeckoUIDateInput__suffix`               | `<div>`    | Suffix icon/text area               | —                                                      |
| `.GeckoUIDateInput__icons`                | `<div>`    | Icons container (clear, calendar)   | —                                                      |
| `.GeckoUIDateInput__clear-button`         | `<button>` | Clear button                        | —                                                      |
| `.GeckoUIDateInput__calendar-icon`        | `<div>`    | Calendar icon                       | —                                                      |
| `.GeckoUIDateInput__hidden-input`         | `<input>`  | Hidden input for each segment       | —                                                      |
| `.GeckoUIDateInput__calendar`             | `<div>`    | Floating calendar panel             | —                                                      |
| `.GeckoUIDateRangeInputWrapper`           | `<div>`    | DateRange outer wrapper             | `data-calendar-open`                                   |
| `.GeckoUIDateRangeInput__range-separator` | `<span>`   | Range separator (-)                 | —                                                      |

### Switch

| Class                   | Element   | Targets                       | Data Attrs      |
| ----------------------- | --------- | ----------------------------- | --------------- |
| `.GeckoUISwitch`        | `<label>` | Visual track (the pill shape) | `data-size`     |
| `.GeckoUISwitch__input` | `<input>` | Hidden checkbox (sr-only)     | `role="switch"` |
| `.GeckoUISwitch__thumb` | `<span>`  | Sliding thumb circle          | `data-size`     |

Checked state: use `.GeckoUISwitch:has(input:checked)` selector.

### Checkbox

| Class                      | Element    | Targets                        |
| -------------------------- | ---------- | ------------------------------ |
| `.GeckoUICheckbox`        | `<div>`   | Outer wrapper                                    |
| `.GeckoUICheckbox__input` | `<input>` | The native checkbox, `appearance-none`, is the box |
| `.GeckoUICheckbox__icon`  | `<svg>`   | Check/indeterminate icon                         |

### Radio

| Class           | Element   | Targets                                              |
| --------------- | --------- | ---------------------------------------------------- |
| `.GeckoUIRadio` | `<input>` | The radio input itself (styled with appearance:none) |

### OTPInput

| Class                              | Element    | Targets                      | Data Attrs   |
| ---------------------------------- | ---------- | ---------------------------- | ------------ |
| `.GeckoUIOTPInput`                 | `<div>`    | Grid container               | `data-state` |
| `.GeckoUIOTPInput__input`          | `<input>`  | Individual digit input cell  | —            |
| `.GeckoUIOTPInput__overlay-button` | `<button>` | Overlay for focus management | —            |

### CounterInput

| Class                          | Element    | Targets                    | Data Attrs                |
| ------------------------------ | ---------- | -------------------------- | ------------------------- |
| `.GeckoUICounterInput`         | `<div>`    | Outer flex container       | `data-size`, `data-state` |
| `.GeckoUICounterInput__button` | `<button>` | Increment/decrement button | `data-action`             |
| `.GeckoUICounterInput__icon`   | `<span>`   | Plus/minus icon            | `data-icon`               |
| `.GeckoUICounterInput__input`  | `<input>`  | The number display input   | —                         |

### Pagination

| Class                              | Element    | Targets                       | Data Attrs                     |
| ---------------------------------- | ---------- | ----------------------------- | ------------------------------ |
| `.GeckoUIPagination`               | `<div>`    | Pagination wrapper            | —                              |
| `.GeckoUIPagination__arrow`        | `<button>` | Prev/next arrow button        | —                              |
| `.GeckoUIPagination__arrow__icon`  | `<span>`   | Arrow icon                    | `data-direction`               |
| `.GeckoUIPagination__page-count`   | `<div>`    | "x / y" display               | —                              |
| `.GeckoUIPagination__page-buttons` | `<div>`    | Page number buttons container | —                              |
| `.GeckoUIPagination__page-button`  | `<button>` | Individual page number        | `data-active`, `data-ellipsis` |

### Label / InputError / Spinner

| Class                               | Element   | Targets            |
| ----------------------------------- | --------- | ------------------ |
| `.GeckoUILabel`                     | `<label>` | Label element      |
| `.GeckoUILabel__required-indicator` | `<span>`  | Red asterisk (\*)  |
| `.GeckoUILabel__tooltip-icon`       | `<div>`   | Help/info icon     |
| `.GeckoUIInputError`                | `<div>`   | Error message text |
| `.GeckoUISpinnerIcon`               | `<svg>`   | Animated spinner   |

### Avatar / AvatarGroup

| Class                             | Element   | Targets                                  | Data Attrs                                                          |
| --------------------------------- | --------- | ---------------------------------------- | ------------------------------------------------------------------- |
| `.GeckoUIAvatar`                  | `<span>`  | One avatar                               | `data-size`, `data-shape`, `data-color`, `data-clickable`           |
| `.GeckoUIAvatar__image`           | `<img>`   | The picture                              | —                                                                    |
| `.GeckoUIAvatar__fallback`        | `<span>`  | Initials or icon when there is no picture | —                                                                   |
| `.GeckoUIAvatarGroup`             | `<div>`   | The group                                | `data-size`, `data-shape`, `data-interactive`                       |
| `.GeckoUIAvatarGroup__list`       | `<ul>`    | The overlapping row                      | —                                                                    |
| `.GeckoUIAvatarGroup__slot`       | `<span>`  | Holds an avatar's place while it lifts   | —                                                                    |
| `.GeckoUIAvatarGroup__list__item` | `<li>`    | One entry in the row                     | —                                                                    |
| `.GeckoUIAvatarGroup__overflow`   | `<span>`  | The `+N` avatar                         | —                                                                    |

Style `.GeckoUIAvatarGroup__slot > .GeckoUIAvatar`, not `.GeckoUIAvatarGroup > .GeckoUIAvatar`.

### Rating

| Class                          | Element  | Targets                       | Data Attrs                                                  |
| ------------------------------ | -------- | ----------------------------- | ------------------------------------------------------------ |
| `.GeckoUIRating`               | `<div>`  | The row of icons              | `data-color`, `data-size`, `data-readonly`, `data-disabled`  |
| `.GeckoUIRating__inputs`       | `<span>` | The visually hidden radios    | —                                                             |
| `.GeckoUIRating__icon`         | `<span>` | One icon                      | `data-filled`                                                |
| `.GeckoUIRating__icon__fill`   | `<span>` | The filled layer, clipped to the value | —                                                   |
| `.GeckoUIRating__icon__empty`  | `<span>` | The empty layer underneath    | —                                                             |

### Slider / RangeSlider

Both render the same classes; a `RangeSlider` has two `__thumb` elements.

| Class                          | Element    | Targets                      | Data Attrs                                                     |
| ------------------------------ | ---------- | ---------------------------- | --------------------------------------------------------------- |
| `.GeckoUISlider`               | `<div>`    | The whole control            | `data-color`, `data-size`, `data-disabled`                      |
| `.GeckoUISlider__track`        | `<div>`    | The unfilled bar             | —                                                                |
| `.GeckoUISlider__fill`         | `<div>`    | The filled part              | —                                                                |
| `.GeckoUISlider__thumb`        | `<div>`    | The handle                   | `data-dragging`, `data-custom`                                  |
| `.GeckoUISlider__label`        | `<span>`   | The value bubble             | —                                                                |
| `.GeckoUISlider__mark`         | `<span>`   | One tick                     | `data-filled`                                                   |
| `.GeckoUISlider__mark-labels`  | `<div>`    | The row of tick labels       | —                                                                |
| `.GeckoUISlider__mark-label`   | `<span>`   | One tick label               | `data-filled`                                                   |

### Stepper

| Class                          | Element  | Targets                      | Data Attrs                                       |
| ------------------------------ | -------- | ---------------------------- | -------------------------------------------------- |
| `.GeckoUIStepper`              | `<nav>`  | The whole stepper            | `data-orientation`, `data-size`, `data-current`   |
| `.GeckoUIStepper__list`        | `<ol>`   | The steps                    | —                                                  |
| `.GeckoUIStepper__step`        | `<li>`   | One step                     | `data-status`                                     |
| `.GeckoUIStepper__button`      | `<button>` | A clickable step           | —                                                  |
| `.GeckoUIStepper__marker`      | `<span>` | The number or tick           | —                                                  |
| `.GeckoUIStepper__body`        | `<span>` | Label and description        | —                                                  |
| `.GeckoUIStepper__label`       | `<span>` | The step title               | —                                                  |
| `.GeckoUIStepper__description` | `<span>` | The step subtitle            | —                                                  |
| `.GeckoUIStepper__separator`   | `<span>` | The gap between steps        | —                                                  |
| `.GeckoUIStepper__line`        | `<span>` | The line inside it           | —                                                  |

`data-status` is `"complete" | "current" | "upcoming"`.

### Breadcrumb

| Class                             | Element   | Targets                        | Data Attrs  |
| --------------------------------- | --------- | ------------------------------ | ----------- |
| `.GeckoUIBreadcrumb`              | `<nav>`   | The whole trail                | `data-size` |
| `.GeckoUIBreadcrumb__list`        | `<ol>`    | The crumbs                     | —           |
| `.GeckoUIBreadcrumb__crumb`       | `<li>`    | One crumb                      | —           |
| `.GeckoUIBreadcrumb__link`        | `<a>` / `<button>` / `<span>` | The crumb's own content | — |
| `.GeckoUIBreadcrumb__separator`   | `<li>`    | Between crumbs                 | —           |
| `.GeckoUIBreadcrumb__expand`      | `<button>` | The `…` shown past `maxItems` | —          |
| `.GeckoUIBreadcrumb__menu`        | `<ul>`    | The collapsed crumbs' dropdown | —           |
| `.GeckoUIBreadcrumb__menu__item`  | `<li>`    | One entry in it                | —           |

### Progress

| Class                      | Element  | Targets             | Data Attrs                                         |
| -------------------------- | -------- | ------------------- | ---------------------------------------------------- |
| `.GeckoUIProgress`         | `<div>`  | The whole control   | `data-color`, `data-size`, `data-indeterminate`     |
| `.GeckoUIProgress__track`  | `<div>`  | The empty bar       | —                                                    |
| `.GeckoUIProgress__bar`    | `<div>`  | The filled part     | —                                                    |
| `.GeckoUIProgress__label`  | `<div>`  | The text beside it  | —                                                    |

### Skeleton

| Class                    | Element  | Targets             | Data Attrs                     |
| ------------------------ | -------- | ------------------- | -------------------------------- |
| `.GeckoUISkeleton`       | `<div>`  | The placeholder     | `data-shape`, `data-animation`  |
| `.GeckoUISkeleton__bar`  | `<span>` | One line of text    | —                                |

### TagInput

| Class                             | Element    | Targets                          | Data Attrs                                    |
| --------------------------------- | ---------- | -------------------------------- | ----------------------------------------------- |
| `.GeckoUITagInputWrapper`         | `<div>`    | Outer wrapper                    | —                                               |
| `.GeckoUITagInput`                | `<div>`    | The field                        | `data-state`, `data-empty`, `data-full`         |
| `.GeckoUITagInput__tags`          | `<div>`    | The tags and the input           | —                                               |
| `.GeckoUITagInput__field`         | `<div>`    | Sizes the input to what is typed | `data-keyword`                                  |
| `.GeckoUITagInput__tag`           | `<span>`   | One tag                          | —                                               |
| `.GeckoUITagInput__tag__remove`   | `<button>` | Its remove button                | —                                               |
| `.GeckoUITagInput__input`         | `<input>`  | What is being typed              | `data-initial`, `aria-invalid`                  |
| `.GeckoUITagInput__placeholder`   | `<span>`   | Shown while empty                | —                                               |
| `.GeckoUITagInput__prefix`        | `<div>`    | Leading slot                     | —                                               |
| `.GeckoUITagInput__suffix`        | `<div>`    | Trailing slot                    | —                                               |
| `.GeckoUITagInput__menu`          | `<div>`    | The options dropdown             | `data-hidden`                                   |
| `.GeckoUITagInput__option`        | `<button>` | One option                       | `data-focused`                                  |

### TimeInput

| Class                              | Element    | Targets                      | Data Attrs                            |
| ---------------------------------- | ---------- | ---------------------------- | --------------------------------------- |
| `.GeckoUITimeInputWrapper`         | `<div>`    | Outer wrapper                | —                                       |
| `.GeckoUITimeInput`                | `<div>`    | The field                    | `data-state`, `data-empty`, `data-focus`, `aria-invalid` |
| `.GeckoUITimeInput__segments`      | `<div>`    | The hh:mm:ss row             | —                                       |
| `.GeckoUITimeInput__segment`       | `<label>`  | One editable segment         | `data-segment`, `data-empty`            |
| `.GeckoUITimeInput__separator`     | `<span>`   | The colon                    | —                                       |
| `.GeckoUITimeInput__placeholder`   | `<span>`   | Shown while empty            | —                                       |
| `.GeckoUITimeInput__prefix`        | `<div>`    | Leading slot                 | —                                       |
| `.GeckoUITimeInput__suffix`        | `<div>`    | Trailing slot                | —                                       |
| `.GeckoUITimeInput__icons`         | `<div>`    | Clock and clear together     | —                                       |
| `.GeckoUITimeInput__clock-icon`    | `<svg>`    | Opens the picker             | —                                       |
| `.GeckoUITimeInput__clear-button`  | `<button>` | Clears the value             | —                                       |
| `.GeckoUITimeInput__hidden-input`  | `<input>`  | Carries the value for a form | —                                       |
| `.GeckoUITimeInput__picker`        | `<div>`    | The dropdown                 | —                                       |
| `.GeckoUITimeInput__columns`       | `<div>`    | The scrolling columns        | —                                       |
| `.GeckoUITimeInput__column`        | `<ul>`     | One column                   | —                                       |
| `.GeckoUITimeInput__cell`          | `<button>` | One value in a column        | `data-state`                            |

### ColorPicker / ColorInput

| Class                                    | Element    | Targets                             | Data Attrs                                      |
| ---------------------------------------- | ---------- | ----------------------------------- | ------------------------------------------------- |
| `.GeckoUIColorPicker`                    | `<div>`    | The panel                           | `data-disabled`                                  |
| `.GeckoUIColorPicker__saturation`        | `<div>`    | The saturation square               | `data-dragging`                                  |
| `.GeckoUIColorPicker__saturation__thumb` | `<div>`    | Its handle                          | `data-custom`                                    |
| `.GeckoUIColorPicker__sliders`           | `<div>`    | Hue and opacity together            | —                                                 |
| `.GeckoUIColorPicker__preview`           | `<div>`    | The current colour beside them      | —                                                 |
| `.GeckoUIColorPicker__slider`            | `<div>`    | One slider                          | `data-kind` (`hue`/`alpha`), `data-dragging`     |
| `.GeckoUIColorPicker__slider__track`     | `<div>`    | Its bar                             | —                                                 |
| `.GeckoUIColorPicker__slider__thumb`     | `<div>`    | Its handle                          | `data-custom`                                    |
| `.GeckoUIColorPicker__controls`          | `<div>`    | Format dropdown, field, eyedropper  | —                                                 |
| `.GeckoUIColorPicker__format`            | `<div>`    | The format dropdown                 | `data-open`                                      |
| `.GeckoUIColorPicker__format__trigger`   | `<button>` | Opens it                            | —                                                 |
| `.GeckoUIColorPicker__format__caret`     | `<span>`   | Its arrow                           | —                                                 |
| `.GeckoUIColorPicker__format__list`      | `<ul>`     | The options                         | —                                                 |
| `.GeckoUIColorPicker__format__option`    | `<li>`     | One format                          | `data-active`                                    |
| `.GeckoUIColorPicker__field`             | `<div>`    | Wraps the text input                | —                                                 |
| `.GeckoUIColorPicker__input`             | `<input>`  | The value field                     | —                                                 |
| `.GeckoUIColorPicker__dropper`           | `<button>` | The eyedropper                      | —                                                 |
| `.GeckoUIColorPicker__swatches`          | `<div>`    | The preset row                      | —                                                 |
| `.GeckoUIColorPicker__swatch`            | `<button>` | One preset                          | `data-active`                                    |
| `.GeckoUIColorInputWrapper`              | `<div>`    | Outer wrapper                       | `data-custom`                                    |
| `.GeckoUIColorInput`                     | `<button>` | The field that opens the picker     | `data-state`, `data-open`, `data-custom`, `aria-invalid` |
| `.GeckoUIColorInput__swatch`             | `<span>`   | The colour square in the field      | —                                                 |
| `.GeckoUIColorInput__value`              | `<span>`   | The formatted value                 | —                                                 |
| `.GeckoUIColorInput__placeholder`        | `<span>`   | Shown while empty                   | —                                                 |
| `.GeckoUIColorInput__caret`              | `<span>`   | The arrow                           | —                                                 |
| `.GeckoUIColorInput__panel`              | `<div>`    | The popover holding the picker      | —                                                 |

### RHF Components

See [Class Stacking](#class-stacking).

| Class                                       | Element      | Targets                                  | Data Attrs                                    |
| ------------------------------------------- | ------------ | ---------------------------------------- | --------------------------------------------- |
| `.GeckoUIRHFInput`                          | `<label>`    | Wraps Input                              | —                                             |
| `.GeckoUIRHFTextarea`                       | `<textarea>` | Wraps Textarea                           | —                                             |
| `.GeckoUIRHFSelect`                         | `<div>`      | Wraps Select outer container             | —                                             |
| `.GeckoUIRHFSelectButton`                   | `<div>`      | Wraps SelectButton                       | —                                             |
| `.GeckoUIRHFOTPInput`                       | `<div>`      | Wraps OTPInput                           | —                                             |
| `.GeckoUIRHFCounterInput`                   | `<div>`      | Wraps CounterInput                       | —                                             |
| `.GeckoUIRHFSwitch`                         | `<label>`    | Wraps Switch                             | —                                             |
| `.GeckoUIRHFSwitch__thumb`                  | `<span>`     | Switch thumb                             | —                                             |
| `.GeckoUIRHFCheckbox`                       | `<label>`    | Checkbox + label wrapper                 | —                                             |
| `.GeckoUIRHFCheckbox__label`                | `<span>`     | Label text                               | —                                             |
| `.GeckoUIRHFRadio`                          | `<label>`    | Radio + label wrapper                    | —                                             |
| `.GeckoUIRHFRadio__label`                   | `<span>`     | Label text                               | —                                             |
| `.GeckoUIFileInputWrapper`                  | `<div>`      | Outer wrapper                            | —                                             |
| `.GeckoUIFileInput`                         | `<div>`      | The field, and the drop target           | `data-state`, `data-dragging`, `data-empty`, `data-custom`   |
| `.GeckoUIFileInput__trigger`                | `<button>`   | Fills the row, opens the dialog          | `aria-invalid`                                |
| `.GeckoUIFileInput__value`                  | `<span>`     | The file name, or the count              | —                                             |
| `.GeckoUIFileInput__placeholder`            | `<span>`     | Shown while empty                        | —                                             |
| `.GeckoUIFileInput__icons`                  | `<div>`      | Clear button and the file icon           | —                                             |
| `.GeckoUIFileInput__clear`                  | `<button>`   | Empties the field                        | —                                             |
| `.GeckoUIRHFCurrencyInput`                  | `<label>`    | Currency input wrapper                   | —                                             |
| `.GeckoUIRHFCurrencyInput__currency-symbol` | `<span>`     | Currency symbol ($ € £)                  | —                                             |
| `.GeckoUIRHFCurrencyInput__currency-code`   | `<span>`     | Currency code (USD, EUR)                 | —                                             |
| `.GeckoUIRHFError`                          | `<div>`      | Error message                            | —                                             |
| `.GeckoUIRHFInputGroup`                     | `<div>`      | Form field group (label + input + error) | —                                             |
