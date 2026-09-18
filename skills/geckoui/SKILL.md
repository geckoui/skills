---
name: geckoui
description: Use this skill when the user asks about "GeckoUI", "geckoui", "@geckoui/geckoui", "Gecko UI components", "Button component", "Input component", "Select component", "Menu component", "Alert component", "Dialog component", "Drawer component", "Calendar component", "Switch component", "Checkbox component", "Radio component", "Tooltip component", "Pagination component", "OTPInput", "DateInput", "DateRangeInput", "CounterInput", "LoadingButton", "Spinner", "Textarea", "Label", "InputError", "ConfirmDialog", "GeckoUIProvider", "Toast", "Badge component", "Tabs component", "RHFInput", "RHFSelect", "RHFCheckbox", "RHFRadio", "RHFSwitch", "RHFTextarea", "RHFDateInput", "RHFFilePicker", "RHFError", "GeckoUI theming", "oklch theme", "--color-primary", "--color-surface", "--color-text", "--color-border", "data-variant", "data-color", "data-size", "module augmentation", or needs to build React UIs with GeckoUI components.
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
          <GeckoUIProvider>
            {children}
          </GeckoUIProvider>
        </AuthProvider>
      </body>
    </html>
  );
}
```

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| `children` | `ReactNode` | — | Your app tree |
| `toastOptions` | `ToasterOptions` | `{}` | Defaults for every toast, and where the stacks sit |

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

Dialog.dismiss(id);  // close that dialog
Dialog.dismiss();    // close the topmost dialog
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
Drawer.dismiss(id);  // close that drawer
Drawer.dismiss();    // close the topmost drawer
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

| Prop      | Type                                                                     | Default     |
| --------- | ------------------------------------------------------------------------ | ----------- |
| `variant` | `"filled" \| "soft" \| "outlined"`                                        | `"soft"`    |
| `color`   | `"default" \| "primary" \| "success" \| "error" \| "warning" \| "info"`   | `"default"` |
| `size`    | `"sm" \| "md" \| "lg"`                                                    | `"md"`      |
| `shape`   | `"rounded" \| "pill" \| "square"`                                         | `"rounded"` |
| `dot`     | `boolean`                                                                | `false`     |
| `icon`    | `ReactNode \| FC`                                                        | -           |

Extends `HTMLAttributes<HTMLSpanElement>`. Uses `data-variant`, `data-color`, `data-size`,
`data-shape`.

### Tabs

```tsx
<Tabs defaultValue="profile" variant="underline">
  <Tab value="profile" label="Profile"><ProfileForm /></Tab>
  <Tab value="billing" label={<>Billing <Badge color="error">2</Badge></>}>
    <BillingForm />
  </Tab>
  <Tab value="team" label="Team" disabled><TeamList /></Tab>
</Tabs>
```

`label` is the tab. `children` is the panel it reveals.

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
| `listClassName`| `string`                               | -              |

**Tab props:** `value` (required), `label` (required, node or render function), `disabled`,
`keepMounted`, `className`, `panelClassName`, `children` (the panel).

The strip scrolls sideways when the tabs outgrow it, centring the selected one. Arrow keys
move focus; Enter or Space selects.

**Navigation tabs.** Tabs that change the URL are not tabs to a screen reader, so pass
`as="nav"`: it renders a `nav` of links with `aria-current="page"` instead of a tablist,
and leaves the arrow keys alone. Your router owns the state:

```tsx
<Tabs as="nav" value={pathname}>
  <Tab
    value="/settings/profile"
    label={({ props }) => (
      <Link href="/settings/profile" {...props} className="GeckoUITabs__tab">Profile</Link>
    )}
  />
</Tabs>
```

Spread `props` onto whatever you render, or the keyboard and aria wiring break.

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

| Prop              | Type                                | Default  |
| ----------------- | ----------------------------------- | -------- |
| `value`           | `string`                            | required |
| `onChange`        | `(value: string) => void`           | required |
| `min`             | `number`                            | -        |
| `max`             | `number`                            | -        |
| `step`            | `number`                            | `1`      |
| `size`            | `"sm" \| "md" \| "lg"` (extensible) | `"md"`   |
| `disabled`        | `boolean`                           | -        |
| `readOnly`        | `boolean`                           | -        |
| `allowTyping`     | `boolean`                           | `false`  |
| `strict`          | `boolean`                           | `true`   |
| `positiveOnly`    | `boolean`                           | `false`  |
| `maxFractionDigits` | `number`                          | -        |
| `maxWholeDigitPlaces` | `number`                        | -        |
| `inputClassName`  | `string`                            | -        |
| `buttonClassName` | `string`                            | -        |

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
toast.dismiss(id);   // one toast
toast.dismiss();     // all of them

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

| Base Component   | RHF Component     | Extra Props                                                                           |
| ---------------- | ----------------- | ------------------------------------------------------------------------------------- |
| Input            | RHFInput          | `transform`, `onChange`, `onBlur`                                                     |
| Textarea         | RHFTextarea       | `onChange`, `onBlur`                                                                  |
| Select           | RHFSelect         | `onChange`                                                                            |
| Checkbox         | RHFCheckbox       | `label`, `labelClassName`, `value`, `uncheckedValue`, `single`, `onChange`, `indeterminate` |
| Radio            | RHFRadio          | `label`, `labelClassName`, `value`, `onChange`                                        |
| Switch           | RHFSwitch         | `value`, `uncheckedValue`, `onChange`                                                 |
| DateInput        | RHFDateInput      | `onChange`                                                                            |
| DateRangeInput   | RHFDateRangeInput | `onChange`                                                                            |
| OTPInput         | RHFOTPInput       | -                                                                                     |
| CounterInput     | RHFCounterInput   | `onChange`                                                                            |
| Input (number)   | RHFNumberInput    | `positiveOnly`, `strict`, `maxFractionDigits`, `maxWholeDigitPlaces`                  |
| Input (currency) | RHFCurrencyInput  | `currency: { symbol, code }`                                                          |
| -                | RHFFileInput      | `multiple`, `render`, `inputClassName`                                                |
| -                | RHFFilePicker     | `render` (drag & drop)                                                                |
| -                | RHFError          | `render`                                                                              |
| -                | RHFInputGroup     | `label`, `labelClassName`, `errorClassName` (wraps label + input + error)             |

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

Anything below fails silently rather than at build time, so check by hand.

| v1 | v2 |
| --- | --- |
| `<GeckoUIPortal />`, self-closing | `<GeckoUIProvider>{children}</GeckoUIProvider>` |
| `<Alert variant="error">` | `<Alert color="error">` |
| `<Drawer handleClose>` | `<Drawer onClose>` |
| `<Checkbox partial>` | `<Checkbox indeterminate>` |
| `<CounterInput editable>` | `<CounterInput allowTyping>` |
| `CounterInput value: number` | `value: string` |
| `dismissOnEsc` | `dismissOnEscape` |
| `AlertVariantMap` | `AlertColorMap` |
| `toast` re-exported from sonner | GeckoUI's own, no sonner |
| `BaseDateRangeInput` exported | removed, use `DateRangeInput` |

Behaviour that changed without a rename:

- `Dialog.dismiss()` and `Drawer.dismiss()` only close their own type. In v1 either closed
  whatever was on top.
- Clicking inside a dialog no longer dismisses it.
- Dialogs sit above drawers, at z-index 2000 rather than 1000.
- Page scroll locks behind Dialog and Drawer, with the scrollbar width paid back as padding
  so nothing shifts.
- `ConfirmDialog` awaits `onConfirm` and `onCancel`, so an async one shows its loading state.
- `Checkbox` `indeterminate` is independent of `checked`; in v1 the dash needed `checked` too.
- Calendars size themselves to the month. Pass `fixedWeeks` for the old fixed height.
- Toast loses `richColors`, `theme` and `expand`; `toastOptions.className` and `.style`
  become `toastClassName` and `toastStyle`.

New in v2: `Badge`, `Tabs`, and the `--color-success` / `--color-error` / `--color-warning`
/ `--color-info` semantic tokens.

## References

For theming details (all CSS variables, oklch values, dark mode, custom theme output format):

- `references/theming.md` — Complete theming reference
