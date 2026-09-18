---
name: geckoui
description: Use this skill when the user asks about "GeckoUI", "geckoui", "@geckoui/geckoui", "Gecko UI components", "Button component", "Input component", "Select component", "Menu component", "Alert component", "Dialog component", "Drawer component", "Calendar component", "Switch component", "Checkbox component", "Radio component", "Tooltip component", "Pagination component", "OTPInput", "DateInput", "DateRangeInput", "CounterInput", "LoadingButton", "Spinner", "Skeleton component", "Progress component", "Textarea", "Label", "InputError", "ConfirmDialog", "GeckoUIProvider", "Toast", "Badge component", "Avatar component", "AvatarGroup", "Tabs component", "RHFInput", "RHFSelect", "RHFCheckbox", "RHFRadio", "RHFSwitch", "RHFTextarea", "RHFDateInput", "RHFFilePicker", "RHFError", "GeckoUI theming", "oklch theme", "--color-primary", "--color-surface", "--color-text", "--color-border", "data-variant", "data-color", "data-size", "module augmentation", or needs to build React UIs with GeckoUI components.
version: "2.0.0"
---

# GeckoUI

React component library with Tailwind CSS v4, OKLCH theming, and React Hook Form integration.

**Covers `@geckoui/geckoui` v2.x.** For v1, use the skill on the `v1` branch:
`npx skills add https://github.com/geckoui/skills/tree/v1/skills/geckoui`.

v2 renamed several props. If code uses `GeckoUIPortal`, `Alert variant`,
`Drawer handleClose`, `Checkbox partial`, `CounterInput editable`, a numeric
`CounterInput value`, or imports `toast` expecting sonner, it is written for v1 — see
Migrating from v1 at the end.

## Setup

```bash
pnpm add @geckoui/geckoui
```

```tsx
import "@geckoui/geckoui/styles.css";
```

For Tailwind CSS v4 projects, import inside `@layer`:

```css
@import "tailwindcss";

@layer components {
  @import "@geckoui/geckoui/styles.css";
}
```

### Dark mode needs the `dark` class

GeckoUI keeps its dark values under a `.dark` class. Nothing switches on its own, so
**add `dark` to an element above the app — normally `<html>` — or every component stays
light no matter what else the app does.**

```tsx
// app/layout.tsx
<html lang="en" className="dark">
```

Toggling it:

```tsx
document.documentElement.classList.toggle("dark", isDark);
```

Or let `next-themes` write it:

```tsx
<ThemeProvider attribute="class">{children}</ThemeProvider>
```

The class sets CSS variables, so everything beneath it inherits. Putting it on a wrapper
instead of the root themes only that subtree, which is occasionally what you want and
usually not.

When a dark themed app renders GeckoUI components in light colours, this class is the
first thing to check.

Wrap your app in `<GeckoUIProvider>` (required for Dialog, Drawer, ConfirmDialog and Toast). Place it **below** your own context providers, so overlays opened imperatively can read them:

```tsx
import { GeckoUIProvider } from "@geckoui/geckoui";

// app/layout.tsx
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <AuthProvider>
          <GeckoUIProvider>{children}</GeckoUIProvider>
        </AuthProvider>
      </body>
    </html>
  );
}
```

| Prop           | Type             | Default | Description                                        |
| -------------- | ---------------- | ------- | -------------------------------------------------- |
| `children`     | `ReactNode`      | —       | Your app tree                                      |
| `toastOptions` | `ToasterOptions` | `{}`    | Defaults for every toast, and where the stacks sit |

`react-hook-form` is an optional peer dependency. Install it only if you use the `RHF*` components.

## Components

### Button

```tsx
<Button variant="filled" color="primary" size="md">Click</Button>
<Button variant="outlined">Cancel</Button>
<Button variant="ghost">Learn More</Button>
<Button variant="icon">X</Button>
<Button disabled>Disabled</Button>
```

| Prop      | Type                                          | Default     |
| --------- | --------------------------------------------- | ----------- |
| `variant` | `"filled" \| "outlined" \| "ghost" \| "icon"` | `"filled"`  |
| `color`   | `"primary"` (extensible)                      | `"primary"` |
| `size`    | `"xs" \| "sm" \| "md" \| "lg" \| "xl"`        | `"md"`      |

Extends `ButtonHTMLAttributes`. Uses `data-variant`, `data-color`, `data-size` attributes.

### LoadingButton

```tsx
<LoadingButton loading loadingText="Saving...">Save</LoadingButton>
<LoadingButton loading spinnerPosition="end">Submit</LoadingButton>
```

| Prop              | Type               | Default   |
| ----------------- | ------------------ | --------- |
| `loading`         | `boolean`          | -         |
| `spinnerPosition` | `"start" \| "end"` | `"start"` |
| `loadingText`     | `string`           | -         |

Extends `ButtonProps`.

### Input

```tsx
<Input placeholder="Email" />
<Input prefix="$" suffix=".00" />
<Input disabled value="readonly" />
```

| Prop             | Type              | Default |
| ---------------- | ----------------- | ------- |
| `prefix`         | `ReactNode \| FC` | -       |
| `suffix`         | `ReactNode \| FC` | -       |
| `className`      | `string`          | -       |
| `inputClassName` | `string`          | -       |

`className` targets the outer wrapper (contains prefix + input + suffix). `inputClassName` targets the actual `<input>` element. Extends `InputHTMLAttributes` (except `prefix`). Uses `data-state="enabled" | "disabled"`.

### Textarea

```tsx
<Textarea placeholder="Message" rows={4} />
<Textarea autoResize />
```

| Prop         | Type      | Default |
| ------------ | --------- | ------- |
| `autoResize` | `boolean` | `false` |

Extends `TextareaAutosizeProps`.

### Select

```tsx
// Single
<Select value={value} onChange={setValue} placeholder="Choose">
  <SelectOption value="a" label="Option A" />
  <SelectOption value="b" label="Option B" />
</Select>

// Multiple
<Select multiple value={values} onChange={setValues} filterable>
  <SelectOption value="red" label="Red" />
  <SelectOption value="blue" label="Blue" />
</Select>
```

| Prop                   | Type                                   | Default                          |
| ---------------------- | -------------------------------------- | -------------------------------- |
| `value`                | `T` or `T[]`                           | -                                |
| `onChange`             | `(v: T) => void` or `(v: T[]) => void` | -                                |
| `multiple`             | `boolean`                              | `false`                          |
| `filterable`           | `boolean \| "inline" \| "dropdown"`    | `false`                          |
| `placeholder`          | `string`                               | -                                |
| `placeholderClassName` | `string`                               | -                                |
| `disabled`             | `boolean`                              | -                                |
| `clearable`            | `boolean`                              | `false`                          |
| `closeMenuOnSelect`    | `boolean`                              | `true` (single), `false` (multi) |
| `placement`            | `Placement`                            | `"bottom-start"`                 |
| `floatingStrategy`     | `Strategy`                             | -                                |
| `hideDefaultEmptyUI`   | `boolean`                              | -                                |
| `prefix`               | `ReactNode \| FC`                      | -                                |
| `suffix`               | `ReactNode \| FC`                      | -                                |
| `wrapperClassName`     | `string`                               | -                                |
| `menuClassName`        | `string`                               | -                                |

`wrapperClassName` targets the outer container. `menuClassName` targets the dropdown panel. `placeholderClassName` targets the placeholder text.

**SelectOption props:** `value` (required), `label` (required), `disabled`, `visibility`, `hideCheckIcon`, `onClick`, `onRemove`, `className` (string or render fn), `children` (ReactNode or render fn).

**A value that matches no option.** The trigger falls back to text worked out from the
value itself: a string or number prints as is, an object uses its `label` key if it has
one, otherwise its first non-nil property. `{ id: null, name: "Ann" }` reads as "Ann".
Anything that would leave the trigger blank — `null`, `undefined`, `""`, an object whose
properties are all nil — shows the placeholder instead, and the library warns in
development. Give object values a `label` key rather than relying on property order.

**SelectTrigger:** render-props for custom trigger, receives `{ keyword, selectedOptions, handleChange, options, toggleMenu, open, openMenu, closeMenu, hasValue, filteredOptions, handleInputChange, handleKeyboardInteraction }`.

**SelectConsumer:** render-props for accessing full Select context: `<SelectConsumer render={(ctx) => ...} />`.

### Menu

```tsx
// Default button
<Menu label="Actions">
  <MenuItem onClick={() => {}}>Edit</MenuItem>
  <MenuItem onClick={() => {}}>Delete</MenuItem>
  <MenuItem disabled>Archived</MenuItem>
</Menu>

// Custom trigger
<Menu>
  <MenuTrigger>
    {({ toggleMenu }) => <Button onClick={toggleMenu}>Options</Button>}
  </MenuTrigger>
  <MenuItem onClick={() => {}}>Edit</MenuItem>
</Menu>
```

**Menu props:** `label`, `disabled`, `placement`, `floatingStrategy`, `className` (outer wrapper), `menuClassName` (dropdown panel), `buttonClassName` (default button).

**MenuItem props:** `onClick`, `disabled`, `children`.

**MenuTrigger:** render function receiving `{ open, toggleMenu, openMenu, closeMenu, disabled }`.

### Accordion

Four parts. The header and the panel sit inside their item, so nothing is paired by hand.

```tsx
<Accordion defaultValue="shipping">
  <AccordionItem value="shipping">
    <AccordionHeader>Shipping</AccordionHeader>
    <AccordionPanel>Ships in two to three working days.</AccordionPanel>
  </AccordionItem>
  <AccordionItem value="returns">
    <AccordionHeader>
      Returns <Badge color="info">free</Badge>
    </AccordionHeader>
    <AccordionPanel>Thirty days, unopened.</AccordionPanel>
  </AccordionItem>
</Accordion>
```

| Prop           | Type                                    | Default   |
| -------------- | --------------------------------------- | --------- |
| `value`        | `string \| string[]`                    | -         |
| `defaultValue` | `string \| string[]`                    | -         |
| `onChange`     | `(value: string \| string[]) => void`   | -         |
| `multiple`     | `boolean`                               | `false`   |
| `collapsible`  | `boolean`                               | `true`    |
| `variant`      | `"plain" \| "separated" \| "contained"` | `"plain"` |
| `size`         | `"sm" \| "md" \| "lg"`                  | `"md"`    |
| `keepMounted`  | `boolean`                               | `true`    |

**AccordionItem props:** `value` (required), `disabled`, `children`.

**AccordionHeader props:** `hideIcon`, `icon`, `children`. Whatever is inside is what the
header shows, so an icon or a badge needs no prop of its own.

**AccordionPanel props:** `keepMounted`, `children`.

One item opens at a time unless `multiple` is set, and `value`, `defaultValue` and
`onChange` then deal in arrays. `onChange` reports an empty string when the last open item
closes, so `setOpen("")` closes everything. `collapsible={false}` keeps one open at all
times.

Closed panels stay in the DOM, which is what makes the open and close animate and keeps a
half filled form alive while its panel is shut. `keepMounted={false}` trades the closing
animation for a lighter DOM.

Arrow up and down move between headers, Home and End jump to the ends, and Enter or Space
opens. Disabled items are skipped, and the arrow keys are left alone inside a panel.

### Alert

```tsx
<Alert color="error" title="Error" description="Something went wrong" />
<Alert color="success" title="Done!" onRemove={() => {}} />
<Alert color="warning" title="Warning" condensed />
```

| Prop            | Type                                                       | Default     |
| --------------- | ---------------------------------------------------------- | ----------- |
| `color`         | `"error" \| "warning" \| "info" \| "success" \| "default"` | `"default"` |
| `title`         | `ReactNode \| FC`                                          | required    |
| `description`   | `ReactNode \| FC`                                          | -           |
| `condensed`     | `boolean`                                                  | `false`     |
| `onRemove`      | `() => void`                                               | -           |
| `icon`          | `ReactNode \| FC`                                          | -           |
| `iconClassName` | `string`                                                   | -           |

`iconClassName` targets the icon element. Uses `data-color`, `data-condensed` attributes.

`variant` meant the visual treatment on Button and the meaning on Alert. v2 settles it:
**`variant` is how a thing looks, `color` is what it means.** Alert took the rename because
it was the odd one out.

### Dialog

`Dialog.show()` pushes onto the overlay stack and returns an `id`. Dialogs stack, and only the topmost responds to Escape or a backdrop click. Dialogs always sit above drawers.

```tsx
const id = Dialog.show({
  content: ({ dismiss }) => (
    <div>
      <h3>Title</h3>
      <p>Content</p>
      <Button onClick={dismiss}>Close</Button>
    </div>
  ),
  className: "max-w-md",
  dismissOnEscape: true,
  dismissOnOutsideClick: true
});

Dialog.dismiss(id); // close that dialog
Dialog.dismiss(); // close the topmost dialog
```

`Dialog.dismiss()` only closes dialogs. Use `Drawer.dismiss()` for drawers.

Dialog content renders inside your tree, so it can read React context provided above
`GeckoUIProvider`:

```tsx
Dialog.show({
  content: () => {
    const { user } = useContext(AuthContext);
    return <p>Hello, {user.name}</p>;
  }
});
```

Clicking inside a dialog never dismisses it. Only a press and release both landing on the
backdrop does, so a `Select` or `Menu` popup inside a dialog is safe.

| Prop                    | Type                           | Default |
| ----------------------- | ------------------------------ | ------- |
| `content`               | `ReactNode \| FC<{ dismiss }>` | -       |
| `className`             | `string`                       | -       |
| `dismissOnEscape`       | `boolean`                      | `true`  |
| `dismissOnOutsideClick` | `boolean`                      | `true`  |

`className` targets the dialog panel. **Built-in styles:** `bg-surface-primary`, `p-6`, `rounded-md`, `shadow-xl`, `max-w-[400px]`. Override via `className`.

### ConfirmDialog

```tsx
ConfirmDialog.show({
  title: "Delete?",
  content: "This cannot be undone.",
  confirmButtonLabel: "Delete",
  cancelButtonLabel: "Cancel",
  onConfirm: async ({ dismiss }) => {
    await deleteItem();
    dismiss();
  },
  onCancel: ({ dismiss }) => {
    dismiss();
  }
});
```

| Prop                    | Type                                       | Default |
| ----------------------- | ------------------------------------------ | ------- |
| `title`                 | `string`                                   | -       |
| `content`               | `ReactNode \| FC`                          | -       |
| `confirmButtonLabel`    | `string`                                   | -       |
| `cancelButtonLabel`     | `string`                                   | -       |
| `onConfirm`             | `(e: { preventDefault, dismiss }) => void` | -       |
| `onCancel`              | `(e: { preventDefault, dismiss }) => void` | -       |
| `className`             | `string`                                   | -       |
| `titleClassName`        | `string`                                   | -       |
| `contentClassName`      | `string`                                   | -       |
| `dismissOnEscape`       | `boolean`                                  | `true`  |
| `dismissOnOutsideClick` | `boolean`                                  | `true`  |

`onConfirm` and `onCancel` are both awaited, so an async callback shows the button's
loading state and a `preventDefault()` inside one still lands in time.

**Built-in styles:** Same dialog panel styles as Dialog. Title is `text-base font-semibold`, content is `text-sm text-muted`, actions are right-aligned.

### Drawer

Controlled JSX usage:

```tsx
<Drawer open={isOpen} onClose={() => setOpen(false)} placement="right">
  <div className="p-6">Drawer content</div>
</Drawer>
```

Imperative API. Returns an `id`, stacks, and the content can read your app's context:

```tsx
const id = Drawer.show(<DrawerContent />, { placement: "right", onClose: () => {} });
Drawer.dismiss(id); // close that drawer
Drawer.dismiss(); // close the topmost drawer
```

**Built-in styles:** `bg-surface-primary`, `shadow-xl`, `overflow-y-auto`, no padding. `max-w-md` for left/right, `max-h-[50%]` for top/bottom. Override via `className`.

| Prop                | Type                                     | Default   |
| ------------------- | ---------------------------------------- | --------- |
| `open`              | `boolean`                                | required  |
| `onClose`           | `() => void`                             | -         |
| `placement`         | `"top" \| "bottom" \| "left" \| "right"` | `"right"` |
| `hideBackdrop`      | `boolean`                                | `false`   |
| `allowClickOutside` | `boolean`                                | `false`   |
| `dismissOnEscape`   | `boolean`                                | `true`    |
| `className`         | `string`                                 | -         |
| `backdropClassName` | `string`                                 | -         |

`className` targets the drawer panel. `backdropClassName` targets the backdrop overlay. Uses `data-placement`, `data-state="open" | "closed"` on the panel.

### Popover

A panel anchored to whatever opens it. `PopoverTrigger` uses its child as the trigger
rather than wrapping it, so the button keeps its own tag, styling and click handler.

```tsx
<Popover placement="bottom-start" arrow>
  <PopoverTrigger>
    <Button>Filters</Button>
  </PopoverTrigger>

  <PopoverContent>
    <FilterForm />
  </PopoverContent>
</Popover>
```

| Prop                    | Type                      | Default          |
| ----------------------- | ------------------------- | ---------------- |
| `open`                  | `boolean`                 | -                |
| `defaultOpen`           | `boolean`                 | `false`          |
| `onOpenChange`          | `(open: boolean) => void` | -                |
| `placement`             | `Placement`               | `"bottom-start"` |
| `offset`                | `number`                  | `6`              |
| `floatingStrategy`      | `Strategy`                | -                |
| `dismissOnEscape`       | `boolean`                 | `true`           |
| `dismissOnOutsideClick` | `boolean`                 | `true`           |
| `arrow`                 | `boolean`                 | `false`          |
| `disabled`              | `boolean`                 | `false`          |

**PopoverTrigger props:** one child, used as the trigger.

**PopoverContent props:** `children`, plus any div attribute.

`offset` is the gap between the trigger and the nearest part of the popover, which is the
arrow's tip when `arrow` is on rather than the panel edge, so the spacing looks the same
either way.

`usePopover()` gives anything inside the panel `open`, `close()`, `toggle()` and
`setOpen()`, so a Cancel button or a form submit can shut it.

Focus moves into the panel when it opens and returns to the trigger when it closes.
Escape closes it even from inside a text field, unlike `Dialog` and `Drawer`, which leave
Escape alone while you type. The panel renders inline rather than in a portal, so it stays
inside a dialog and keeps React context.

**Which one to reach for:** `Tooltip` for a label on hover, `Popover` for a panel you click
open that can hold a form, `Menu` for a list of actions with arrow key navigation.

### Tooltip

```tsx
<Tooltip content="Helpful text" placement="top" triggerAsChild>
  <Button>Hover me</Button>
</Tooltip>
```

| Prop               | Type                        | Default |
| ------------------ | --------------------------- | ------- |
| `content`          | `string \| ReactNode \| FC` | -       |
| `placement`        | `Placement`                 | `"top"` |
| `sideOffset`       | `number`                    | `12`    |
| `triggerAsChild`   | `boolean`                   | `false` |
| `delayDuration`    | `number`                    | `700`   |
| `backgroundColor`  | `string`                    | -       |
| `className`        | `string`                    | -       |
| `triggerClassName` | `string`                    | -       |
| `arrowClassName`   | `string`                    | -       |

`className` targets the tooltip content. `triggerClassName` targets the trigger wrapper. `arrowClassName` targets the arrow `<svg>`.

### Calendar

```tsx
<Calendar selectedDate={date} onSelectDate={setDate} />
<Calendar mode="range" selectedRange={range} onSelectRange={setRange} />
<Calendar fixedWeeks selectedDate={date} onSelectDate={setDate} />
```

A month takes the four to six weeks it needs, so the calendar changes height as you page
between months. Pass `fixedWeeks` to always render six, which `DateInput` and
`DateRangeInput` also accept.

### DateInput / DateRangeInput

```tsx
<DateInput value={date} onChange={setDate} format="MM/DD/YYYY" />
<DateInput value={date} onChange={setDate} disabled />
<DateRangeInput value={range} onChange={setRange} />
```

| Prop                   | Type                                                             | Default          |
| ---------------------- | ---------------------------------------------------------------- | ---------------- |
| `value`                | `string \| null` (DateInput) / `DateRange` (Range)               | -                |
| `onChange`             | `(v: string \| null) => void` / `(v: DateRange \| null) => void` | -                |
| `format`               | `"DD/MM/YYYY" \| "MM/DD/YYYY" \| "YYYY-MM-DD"`                   | -                |
| `separator`            | `string`                                                         | -                |
| `rangeSeparator`       | `string` (DateRangeInput only)                                   | -                |
| `placeholder`          | `string`                                                         | -                |
| `disabled`             | `boolean`                                                        | -                |
| `readOnly`             | `boolean`                                                        | -                |
| `hasError`             | `boolean`                                                        | -                |
| `prefix`               | `ReactNode \| FC`                                                | -                |
| `suffix`               | `ReactNode \| FC`                                                | -                |
| `hideCalendarIcon`     | `boolean`                                                        | -                |
| `hideClearIcon`        | `boolean`                                                        | -                |
| `hideCalendar`         | `boolean`                                                        | `false`          |
| `calendarPlacement`    | `Placement`                                                      | `"bottom-start"` |
| `floatingStrategy`     | `Strategy`                                                       | -                |
| `className`            | `string`                                                         | -                |
| `wrapperClassName`     | `string`                                                         | -                |
| `calendarClassName`    | `string`                                                         | -                |
| `placeholderClassName` | `string`                                                         | -                |

`className` targets the input container. `wrapperClassName` targets the outer wrapper (includes floating calendar). `calendarClassName` targets the calendar popup. `placeholderClassName` targets the placeholder text.

### Switch

```tsx
<label className="flex items-center gap-3">
  <Switch size="md" />
  <span>Enable notifications</span>
</label>
```

| Prop             | Type                         | Default |
| ---------------- | ---------------------------- | ------- |
| `size`           | `"sm" \| "md"` (extensible)  | `"md"`  |
| `checked`        | `boolean`                    | -       |
| `defaultChecked` | `boolean`                    | -       |
| `onChange`       | `(checked: boolean) => void` | -       |
| `className`      | `string`                     | -       |
| `thumbClassName` | `string`                     | -       |

`className` targets the outer `<label>` (the visual track). `thumbClassName` targets the sliding thumb circle. Renders hidden `<input type="checkbox" role="switch">` + visual label. Uses CSS `:has(input:checked)`.

### Checkbox

```tsx
<label className="flex items-center gap-2">
  <Checkbox checked={val} onChange={(e) => setVal(e.target.checked)} />
  <span>Agree</span>
</label>
<Checkbox checked={allChecked} indeterminate={someChecked && !allChecked} />
```

`indeterminate` mirrors the native DOM property and is independent of `checked`, so a
select-all box shows the dash while staying unchecked.

### Badge

```tsx
<Badge>Default</Badge>
<Badge variant="filled" color="error">Failed</Badge>
<Badge variant="soft" color="success" dot>Live</Badge>
<Badge shape="pill" size="lg" icon={<StarIcon />}>Featured</Badge>
```

| Prop      | Type                                                                    | Default     |
| --------- | ----------------------------------------------------------------------- | ----------- |
| `variant` | `"filled" \| "soft" \| "outlined"`                                      | `"soft"`    |
| `color`   | `"default" \| "primary" \| "success" \| "error" \| "warning" \| "info"` | `"default"` |
| `size`    | `"sm" \| "md" \| "lg"`                                                  | `"md"`      |
| `shape`   | `"rounded" \| "pill" \| "square"`                                       | `"rounded"` |
| `dot`     | `boolean`                                                               | `false`     |
| `icon`    | `ReactNode \| FC`                                                       | -           |

Extends `HTMLAttributes<HTMLSpanElement>`. Uses `data-variant`, `data-color`, `data-size`,
`data-shape`.

### Avatar

A picture of someone, falling back to their initials.

```tsx
<Avatar name="Ada Lovelace" src={user.image} />
<Avatar name="Ada Lovelace" size="lg" color="primary" />
<Avatar shape="rounded" fallback={<BotIcon />} />
<Avatar name="Ada Lovelace" onClick={() => open(user)} />
```

| Prop       | Type                                                                    | Default     |
| ---------- | ----------------------------------------------------------------------- | ----------- |
| `src`      | `string`                                                                | -           |
| `alt`      | `string`                                                                | -           |
| `name`     | `string`                                                                | -           |
| `fallback` | `ReactNode \| FC`                                                       | -           |
| `size`     | `"xs" \| "sm" \| "md" \| "lg" \| "xl"`                                  | `"md"`      |
| `shape`    | `"circle" \| "rounded" \| "square"`                                     | `"circle"`  |
| `color`    | `"default" \| "primary" \| "success" \| "error" \| "warning" \| "info"` | `"default"` |

Extends `HTMLAttributes<HTMLSpanElement>`. Uses `data-size`, `data-shape`, `data-color`,
`data-clickable`.

It shows, in order: the image, then `fallback`, then the initials from `name`, then a
person icon. A failed image falls through the same chain, and a new `src` gets a fresh
attempt. `onClick` makes it `role="button"` with a tab stop and Enter/Space.

### AvatarGroup

```tsx
<AvatarGroup max={3}>
  <Avatar name="Ada Lovelace" />
  <Avatar name="Grace Hopper" />
  <Avatar name="Alan Turing" />
  <Avatar name="Katherine Johnson" />
</AvatarGroup>
```

| Prop             | Type                        | Default           |
| ---------------- | --------------------------- | ----------------- |
| `max`            | `number`                    | -                 |
| `size`           | `keyof AvatarSizeMap`       | `"md"`            |
| `shape`          | `keyof AvatarShapeMap`      | `"circle"`        |
| `interactive`    | `boolean`                   | `true`            |
| `renderOverflow` | `({ avatars }) => ReactNode` | the list of names |

`size` and `shape` apply to every avatar inside; an avatar can still set its own. Avatars
overlap, each over the one after it, and `max` counts the rest as `+2`.

Hovering an avatar lifts it and names it. Hovering the count opens an overlay listing the
rest. `renderOverflow` fills that overlay — `avatars` holds the props of everyone past
`max`, so it can show anything, not only avatars. The overlay is placed for you; where and
how it opens is not configurable.

`interactive={false}` drops the lift, the names and the overlay, keeping the count.

Each avatar is wrapped in a `GeckoUIAvatarGroup__slot` that holds its place while it lifts,
so `.GeckoUIAvatarGroup > .GeckoUIAvatar` is not the selector to style against — use
`.GeckoUIAvatarGroup__slot > .GeckoUIAvatar`.

### Tabs

Four parts: `Tabs` holds the state, `TabList` is the strip, each `Tab` is one tab, and each
`TabPanel` is what its tab reveals — paired by `value`.

```tsx
<Tabs defaultValue="profile" variant="underline">
  <TabList>
    <Tab value="profile">Profile</Tab>
    <Tab value="billing">
      Billing <Badge color="error">2</Badge>
    </Tab>
    <Tab value="team" disabled>
      Team
    </Tab>
  </TabList>

  <TabPanel value="profile">
    <ProfileForm />
  </TabPanel>
  <TabPanel value="billing">
    <BillingForm />
  </TabPanel>
  <TabPanel value="team">
    <TeamList />
  </TabPanel>
</Tabs>
```

Whatever sits inside `Tab` is what the tab shows, so an icon needs no prop of its own.

| Prop           | Type                                   | Default        |
| -------------- | -------------------------------------- | -------------- |
| `value`        | `string`                               | -              |
| `defaultValue` | `string`                               | first enabled  |
| `onChange`     | `(value: string) => void`              | -              |
| `variant`      | `"underline" \| "segmented" \| "soft"` | `"underline"`  |
| `size`         | `"sm" \| "md" \| "lg"`                 | `"md"`         |
| `orientation`  | `"horizontal" \| "vertical"`           | `"horizontal"` |
| `fullWidth`    | `boolean`                              | `false`        |
| `as`           | `"div" \| "nav"`                       | `"div"`        |
| `keepMounted`  | `boolean`                              | `false`        |

**TabList props:** `children`, plus any div attribute. It owns the keyboard navigation and
the scrolling, and can sit anywhere in the layout — a sticky header with the panels
scrolling below, say.

**Tab props:** `value` (required), `disabled`, `asChild`, `children`.

**TabPanel props:** `value` (required), `keepMounted`, `children`. A hidden panel unmounts
unless `keepMounted` is set here or on `Tabs`, so a half filled form survives.

The strip scrolls sideways when the tabs outgrow it, centring the selected one. Arrow keys
move focus; Enter or Space selects.

**Navigation tabs.** Tabs that change the URL are not tabs to a screen reader, so pass
`as="nav"`: it renders a `nav` of links with `aria-current="page"` instead of a tablist,
and leaves the arrow keys alone. Your router owns the state, and `asChild` hands the tab
wiring to your own link:

```tsx
<Tabs as="nav" value={pathname}>
  <TabList>
    <Tab value="/settings/profile" asChild>
      <Link href="/settings/profile">Profile</Link>
    </Tab>
    <Tab value="/settings/billing" asChild>
      <Link href="/settings/billing">Billing</Link>
    </Tab>
  </TabList>
</Tabs>
```

There are no panels in that case — the page below is the content.

### Radio

```tsx
<label><Radio name="plan" value="free" /> Free</label>
<label><Radio name="plan" value="pro" /> Pro</label>
```

### OTPInput

```tsx
<OTPInput value={otp} onChange={setOtp} length={6} onOTPComplete={(v) => verify(v)} />
```

| Prop             | Type                      | Default  |
| ---------------- | ------------------------- | -------- |
| `value`          | `string`                  | required |
| `onChange`       | `(value: string) => void` | required |
| `length`         | `number`                  | `6`      |
| `numberOnly`     | `boolean`                 | `true`   |
| `aspectRatio`    | `string \| number`        | `0.94`   |
| `onOTPComplete`  | `(value: string) => void` | -        |
| `disabled`       | `boolean`                 | -        |
| `className`      | `string`                  | -        |
| `inputClassName` | `string`                  | -        |

`className` targets the grid container. `inputClassName` targets each individual input cell.

### CounterInput

```tsx
<CounterInput value={count} onChange={setCount} min={0} max={100} size="md" />
```

| Prop                  | Type                                | Default  |
| --------------------- | ----------------------------------- | -------- |
| `value`               | `string`                            | required |
| `onChange`            | `(value: string) => void`           | required |
| `min`                 | `number`                            | -        |
| `max`                 | `number`                            | -        |
| `step`                | `number`                            | `1`      |
| `size`                | `"sm" \| "md" \| "lg"` (extensible) | `"md"`   |
| `disabled`            | `boolean`                           | -        |
| `readOnly`            | `boolean`                           | -        |
| `allowTyping`         | `boolean`                           | `false`  |
| `strict`              | `boolean`                           | `true`   |
| `positiveOnly`        | `boolean`                           | `false`  |
| `maxFractionDigits`   | `number`                            | -        |
| `maxWholeDigitPlaces` | `number`                            | -        |
| `inputClassName`      | `string`                            | -        |
| `buttonClassName`     | `string`                            | -        |

`inputClassName` targets the number display input. `buttonClassName` targets the increment/decrement buttons.

**The value is a string.** A number cannot hold a half typed "2." or a leading zero, so
the value stays text and you convert at the edge: `Number(value)`, or `z.coerce.number()`
in a schema.

### Pagination

```tsx
<Pagination currentPage={page} totalPages={10} onChange={setPage} />
```

### Spinner

```tsx
<Spinner />
<Spinner className="stroke-red-500" />
```

### Progress

How far along something is.

```tsx
<Progress value={40} />
<Progress value={3} max={7} label={({ value, max }) => `${value} of ${max} files`} />
<Progress />
<Progress value={100} color="success" size="lg" />
```

| Prop    | Type                                                                    | Default     |
| ------- | ----------------------------------------------------------------------- | ----------- |
| `value` | `number`                                                                | -           |
| `max`   | `number`                                                                | `100`       |
| `color` | `"default" \| "primary" \| "success" \| "error" \| "warning" \| "info"` | `"primary"` |
| `size`  | `"sm" \| "md" \| "lg"`                                                  | `"md"`      |
| `label` | `ReactNode \| ({ percent, value, max }) => ReactNode`                    | -           |

Extends `HTMLAttributes<HTMLDivElement>`. Uses `data-size`, `data-color`,
`data-indeterminate`. Carries `role="progressbar"`, so `aria-label` names it.

Leaving `value` out runs the bar end to end until you have a number, and drops
`aria-valuenow` so the value reads as unknown rather than zero. `value={0}` is a known
value and draws an empty bar.

`value` is clamped into range, and the `label` function is handed the clamped number.
`percent` is rounded and drives the bar width too, so the number and the bar never
disagree. A `label` node shows while the value is unknown; a `label` function is only
called when there is a value.

**Which one to reach for:** `Spinner` for work with no length, `Progress` when you can
count it, `Skeleton` when you know the shape of what is coming.

### Skeleton

A placeholder that holds the space content will take while it loads. Size it with
`className` the way you would size the real thing.

```tsx
<Skeleton />
<Skeleton lines={3} />
<Skeleton shape="circle" className="size-12" />
<Skeleton shape="rounded" className="h-24 w-40" />
```

| Prop        | Type                                 | Default    |
| ----------- | ------------------------------------ | ---------- |
| `shape`     | `"text" \| "rounded" \| "circle"`    | `"text"`   |
| `animation` | `"pulse" \| "wave" \| "none"`        | `"pulse"`  |
| `lines`     | `number`                             | `1`        |
| `loading`   | `boolean`                            | `true`     |
| `children`  | `ReactNode`                          | -          |

Extends `HTMLAttributes<HTMLDivElement>`. Uses `data-shape` and `data-animation`.

Size it through `className`. A `text` skeleton takes its height from the current font
size, and `lines` draws a paragraph with the last line short.

`loading` renders `children` in place of the placeholder:

```tsx
<Skeleton loading={isLoading} lines={2}>
  <p>{user.bio}</p>
</Skeleton>
```

Children are never rendered while loading, so nothing inside has to guard against data
that has not arrived. Once loading is over the wrapper is gone too, leaving only your own
markup.

Both animations are dropped under `prefers-reduced-motion`.

**Which one to reach for:** `Spinner` for work with no shape to hold, like a button that is
submitting. `Skeleton` when you know the shape of what is coming.

### Label / InputError

```tsx
<Label required tooltip="Help text">Email</Label>
<InputError>Invalid email</InputError>
```

### Toast

GeckoUI's own toasts — `sonner` is gone, and nothing needs installing.

```tsx
import { toast } from "@geckoui/geckoui";

toast("Plain message");
toast.success("Saved!");
toast.error("Failed");
toast.warning("Careful");
toast.info("Heads up");

const id = toast.success("Saved!", { description: "All changes stored." });
toast.dismiss(id); // one toast
toast.dismiss(); // all of them

toast.promise(save(), {
  loading: "Saving...",
  success: (result) => `Saved ${result.name}`,
  error: (e) => `Failed: ${String(e)}`
});
```

**Per toast options:** `description`, `duration`, `position`, `action`, `cancel`, `id`,
`icon`, `closeButton`, `onDismiss`, `onAutoClose`, `className`, `style`.

**Defaults, on `GeckoUIProvider toastOptions`:** `position`, `duration`, `closeButton`,
`visibleToasts`, `gap`, `offset`, `className`, `toastClassName`, `toastStyle`,
`iconClassName`, `messageClassName`, `descriptionClassName`, `actionClassName`,
`cancelClassName`, `closeClassName`.

Styled through `--gecko-toast-*` variables rather than props.

## React Hook Form

All RHF components accept `name` (required), `rules`, `control`, and `disabled`. Use inside `<FormProvider>` or pass `control` explicitly.

```tsx
import { FormProvider, useForm } from "react-hook-form";

const methods = useForm({ defaultValues: { email: "", country: "" } });

<FormProvider {...methods}>
  <form onSubmit={methods.handleSubmit(onSubmit)}>
    <RHFInput name="email" placeholder="Email" />
    <RHFSelect name="country" placeholder="Select country">
      <SelectOption value="us" label="US" />
      <SelectOption value="uk" label="UK" />
    </RHFSelect>
    <RHFTextarea name="bio" placeholder="Bio" />
    <RHFCheckbox name="terms" value={true} uncheckedValue={false} single label="Agree" />
    <RHFRadio name="plan" value="pro" label="Pro" />
    <RHFSwitch name="notifications" />
    <RHFDateInput name="birthDate" />
    <RHFOTPInput name="otp" length={6} />
    <RHFCounterInput name="quantity" min={1} max={10} />
    <RHFError name="email" />
    <Button type="submit">Submit</Button>
  </form>
</FormProvider>;
```

| Base Component   | RHF Component     | Extra Props                                                                                 |
| ---------------- | ----------------- | ------------------------------------------------------------------------------------------- |
| Input            | RHFInput          | `transform`, `onChange`, `onBlur`                                                           |
| Textarea         | RHFTextarea       | `onChange`, `onBlur`                                                                        |
| Select           | RHFSelect         | `onChange`                                                                                  |
| Checkbox         | RHFCheckbox       | `label`, `labelClassName`, `value`, `uncheckedValue`, `single`, `onChange`, `indeterminate` |
| Radio            | RHFRadio          | `label`, `labelClassName`, `value`, `onChange`                                              |
| Switch           | RHFSwitch         | `value`, `uncheckedValue`, `onChange`                                                       |
| DateInput        | RHFDateInput      | `onChange`                                                                                  |
| DateRangeInput   | RHFDateRangeInput | `onChange`                                                                                  |
| OTPInput         | RHFOTPInput       | -                                                                                           |
| CounterInput     | RHFCounterInput   | `onChange`                                                                                  |
| Input (number)   | RHFNumberInput    | `positiveOnly`, `strict`, `maxFractionDigits`, `maxWholeDigitPlaces`                        |
| Input (currency) | RHFCurrencyInput  | `currency: { symbol, code }`                                                                |
| -                | RHFFileInput      | `multiple`, `render`, `inputClassName`                                                      |
| -                | RHFFilePicker     | `render` (drag & drop)                                                                      |
| -                | RHFError          | `render`                                                                                    |
| -                | RHFInputGroup     | `label`, `labelClassName`, `errorClassName` (wraps label + input + error)                   |

## Development Warnings

The library warns in the console about mistakes only a developer can fix — no provider
mounted, several providers, an invalid calendar date, `RHFInputGroup` misuse, a `Select`
value matching no option. Every message is prefixed `[GeckoUI]`.

They are stripped from production builds, so they cost nothing at runtime. If one appears,
it is pointing at a real wiring problem rather than noise to ignore.

## Module Augmentation

Extend built-in variant/color/size maps:

```tsx
declare module "@geckoui/geckoui" {
  interface ButtonColorMap {
    secondary: unknown;
    danger: unknown;
  }
}
```

Then add styles:

```css
.GeckoUIButton[data-variant="filled"][data-color="secondary"] {
  /* your styles */
}
```

**Extensible interfaces:** `ButtonVariantMap`, `ButtonColorMap`, `ButtonSizeMap`,
`AlertColorMap`, `BadgeVariantMap`, `BadgeColorMap`, `BadgeSizeMap`, `BadgeShapeMap`,
`TabsVariantMap`, `TabsSizeMap`, `SwitchSizeMap`, `CounterInputSizeMap`,
`DrawerPlacementMap`.

## Styling

Components use `data-*` attributes for variants/states. Target with attribute selectors:

```css
.GeckoUIButton[data-variant="filled"][data-color="primary"] {
}
.GeckoUIButton[data-size="lg"] {
}
.GeckoUIAlert[data-color="error"] {
}
.GeckoUIBadge[data-variant="soft"][data-color="success"] {
}
.GeckoUITabs__tab[data-state="selected"] {
}
.GeckoUIDrawer__drawer[data-placement="right"][data-state="open"] {
}
```

## Migrating from v1

If the code predates v2, read `references/migrating.md` before changing anything. Most of
the differences fail silently rather than at build time — renamed props are dropped, not
flagged.

The short version: `GeckoUIPortal` becomes `GeckoUIProvider`, `Alert variant` becomes
`color`, `Drawer handleClose` becomes `onClose`, `Checkbox partial` becomes
`indeterminate`, `CounterInput editable` becomes `allowTyping` and its value becomes a
string, and `toast` is GeckoUI's own rather than sonner's.

## References

- `references/theming.md` — every CSS variable, oklch values, dark mode, component
  class reference, and the output format for a generated theme
- `references/migrating.md` — the full v1 to v2 change list, including the behaviour
  changes that do not fail at build time
