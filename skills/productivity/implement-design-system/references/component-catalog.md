# Component Catalog

컴포넌트별 생성 방법, shadcn registry 유무, DESIGN.md 토큰 패치 포인트.

## shadcn registry 컴포넌트

아래 컴포넌트는 `npx shadcn@latest add <name> --yes`로 추가 후 cva/CSS 클래스를 DESIGN.md 토큰으로 패치한다.

| 컴포넌트 | shadcn add 이름 | 토큰 패치 포인트 |
|---|---|---|
| Button | `button` | `bg-primary`, `text-primary-foreground`, `rounded-[var(--radius)]`, hover variant |
| Input | `input` | `border-input`, `ring-ring`, `rounded-[var(--radius)]` |
| Textarea | `textarea` | Input과 동일 패턴 |
| Label | `label` | `text-foreground`, typography label 클래스 |
| Select | `select` | `border-input`, `bg-background`, `rounded-[var(--radius)]` |
| Checkbox | `checkbox` | `border-primary`, `bg-primary` (checked) |
| RadioGroup | `radio-group` | `border-primary`, `text-primary` |
| Switch | `switch` | `bg-primary` (checked), `rounded-full` |
| Tabs | `tabs` | `border-border`, `text-primary` (active) |
| Tooltip | `tooltip` | `bg-foreground`, `text-background`, `rounded-[var(--radius)]` |
| Popover | `popover` | `bg-background`, `border-border`, `rounded-[var(--radius)]` |
| DropdownMenu | `dropdown-menu` | `bg-background`, `text-foreground`, hover `bg-accent` |
| Dialog | `dialog` | `bg-background`, `border-border`, `rounded-[calc(var(--radius)+4px)]` |
| Drawer | `drawer` | `bg-background`, rounded 상단 or 측면 |
| ScrollArea | `scroll-area` | `bg-muted` (scrollbar thumb) |
| Separator | `separator` | `bg-border` |
| Breadcrumb | `breadcrumb` | `text-muted-foreground`, active `text-foreground` |
| Badge | `badge` | `bg-primary`, `text-primary-foreground`, `rounded-full` |
| Card | `card` | `bg-card`, `border-border`, `rounded-[var(--radius)]` |
| Skeleton | `skeleton` | `bg-muted`, animate-pulse |
| Progress | `progress` | `bg-muted`, indicator `bg-primary` |
| Calendar | `calendar` | `text-primary` (selected day), `bg-accent` (hover) |
| DataTable | `data-table` | `border-border`, header `bg-muted` |
| Alert | `alert` | Callout 베이스로 사용 |

## 커스텀 컴포넌트 (registry 외)

shadcn registry에 없어 직접 생성하는 컴포넌트.

---

### Combobox

```tsx
// components/ui/combobox.tsx
"use client"
import * as React from "react"
import { Check, ChevronsUpDown } from "lucide-react"
import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Command, CommandEmpty, CommandGroup, CommandInput, CommandItem } from "@/components/ui/command"
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover"

interface ComboboxProps {
  options: { label: string; value: string }[]
  value?: string
  onValueChange?: (value: string) => void
  placeholder?: string
}

export function Combobox({ options, value, onValueChange, placeholder = "선택..." }: ComboboxProps) {
  const [open, setOpen] = React.useState(false)
  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <Button variant="outline" role="combobox" aria-expanded={open} className="w-full justify-between">
          {value ? options.find((o) => o.value === value)?.label : placeholder}
          <ChevronsUpDown className="ml-2 h-4 w-4 shrink-0 opacity-50" />
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-full p-0">
        <Command>
          <CommandInput placeholder="검색..." />
          <CommandEmpty>결과 없음</CommandEmpty>
          <CommandGroup>
            {options.map((option) => (
              <CommandItem key={option.value} value={option.value}
                onSelect={(v) => { onValueChange?.(v === value ? "" : v); setOpen(false) }}>
                <Check className={cn("mr-2 h-4 w-4", value === option.value ? "opacity-100" : "opacity-0")} />
                {option.label}
              </CommandItem>
            ))}
          </CommandGroup>
        </Command>
      </PopoverContent>
    </Popover>
  )
}
```

---

### MultiCombobox

Combobox에서 `value: string[]`로 확장. `CommandItem`의 `onSelect`에서 배열 토글 로직 적용.

---

### DatePicker

```tsx
// components/ui/date-picker.tsx
"use client"
import * as React from "react"
import { format } from "date-fns"
import { Calendar as CalendarIcon } from "lucide-react"
import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover"

interface DatePickerProps {
  value?: Date
  onValueChange?: (date: Date | undefined) => void
  placeholder?: string
}

export function DatePicker({ value, onValueChange, placeholder = "날짜 선택" }: DatePickerProps) {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button variant="outline" className={cn("w-full justify-start text-left font-normal",
          !value && "text-muted-foreground")}>
          <CalendarIcon className="mr-2 h-4 w-4" />
          {value ? format(value, "PPP") : <span>{placeholder}</span>}
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-auto p-0">
        <Calendar mode="single" selected={value} onSelect={onValueChange} initialFocus />
      </PopoverContent>
    </Popover>
  )
}
```

---

### TimePicker

Input[type=time] 기반 또는 shadcn Select를 시·분 콤보로 조합.

---

### StepIndicator

```tsx
// components/ui/step-indicator.tsx
import { cn } from "@/lib/utils"

interface Step { label: string; description?: string }
interface StepIndicatorProps { steps: Step[]; current: number; className?: string }

export function StepIndicator({ steps, current, className }: StepIndicatorProps) {
  return (
    <ol className={cn("flex items-center", className)}>
      {steps.map((step, i) => (
        <li key={i} className={cn("flex items-center", i < steps.length - 1 && "flex-1")}>
          <div className="flex flex-col items-center">
            <span className={cn(
              "flex h-8 w-8 items-center justify-center rounded-full border-2 text-sm font-medium",
              i < current  && "border-primary bg-primary text-primary-foreground",
              i === current && "border-primary text-primary",
              i > current  && "border-border text-muted-foreground",
            )}>
              {i + 1}
            </span>
            <span className="mt-1 text-label-sm">{step.label}</span>
          </div>
          {i < steps.length - 1 && (
            <div className={cn("h-px flex-1 mx-2", i < current ? "bg-primary" : "bg-border")} />
          )}
        </li>
      ))}
    </ol>
  )
}
```

---

### EmptyState

```tsx
// components/ui/empty-state.tsx
import { cn } from "@/lib/utils"

interface EmptyStateProps {
  icon?: React.ReactNode
  title: string
  description?: string
  action?: React.ReactNode
  className?: string
}

export function EmptyState({ icon, title, description, action, className }: EmptyStateProps) {
  return (
    <div className={cn("flex flex-col items-center justify-center py-12 text-center", className)}>
      {icon && <div className="mb-4 text-muted-foreground">{icon}</div>}
      <h3 className="text-body-lg font-medium text-foreground">{title}</h3>
      {description && <p className="mt-1 text-body-sm text-muted-foreground max-w-sm">{description}</p>}
      {action && <div className="mt-6">{action}</div>}
    </div>
  )
}
```

---

### Stat

```tsx
// components/ui/stat.tsx
import { cn } from "@/lib/utils"

interface StatProps {
  label: string
  value: React.ReactNode
  delta?: { value: string; positive: boolean }
  className?: string
}

export function Stat({ label, value, delta, className }: StatProps) {
  return (
    <div className={cn("flex flex-col gap-1", className)}>
      <span className="text-label-md text-muted-foreground">{label}</span>
      <span className="text-headline-lg font-semibold text-foreground">{value}</span>
      {delta && (
        <span className={cn("text-label-sm", delta.positive ? "text-green-600" : "text-destructive")}>
          {delta.positive ? "↑" : "↓"} {delta.value}
        </span>
      )}
    </div>
  )
}
```

---

### DefinitionList

```tsx
// components/ui/definition-list.tsx
import { cn } from "@/lib/utils"

interface DefinitionListProps {
  items: { term: string; definition: React.ReactNode }[]
  className?: string
}

export function DefinitionList({ items, className }: DefinitionListProps) {
  return (
    <dl className={cn("divide-y divide-border", className)}>
      {items.map(({ term, definition }) => (
        <div key={term} className="flex gap-4 py-3">
          <dt className="w-40 shrink-0 text-label-md text-muted-foreground">{term}</dt>
          <dd className="text-body-sm text-foreground">{definition}</dd>
        </div>
      ))}
    </dl>
  )
}
```

---

### IndicatorDot

```tsx
// components/ui/indicator-dot.tsx
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

const dotVariants = cva("inline-block rounded-full", {
  variants: {
    status: {
      active:  "bg-green-500",
      warning: "bg-yellow-500",
      error:   "bg-destructive",
      idle:    "bg-muted-foreground",
    },
    size: {
      sm: "h-1.5 w-1.5",
      md: "h-2 w-2",
      lg: "h-3 w-3",
    },
  },
  defaultVariants: { status: "idle", size: "md" },
})

interface IndicatorDotProps extends VariantProps<typeof dotVariants> {
  className?: string
}

export function IndicatorDot({ status, size, className }: IndicatorDotProps) {
  return <span className={cn(dotVariants({ status, size }), className)} />
}
```

---

### Tag

Badge의 variant 확장. shadcn `badge.tsx`에 `tag` variant 추가:

```ts
// badge.tsx의 badgeVariants에 추가
tag: "border border-border bg-background text-foreground hover:bg-accent gap-1",
```

삭제 버튼 포함 버전:
```tsx
interface TagProps { label: string; onRemove?: () => void }
export function Tag({ label, onRemove }: TagProps) {
  return (
    <Badge variant="tag" className="pr-1">
      {label}
      {onRemove && (
        <button onClick={onRemove} className="ml-1 rounded-full opacity-60 hover:opacity-100">
          <X className="h-3 w-3" />
        </button>
      )}
    </Badge>
  )
}
```

---

### Callout

shadcn `alert.tsx`에 `callout` variant 추가:

```ts
// alert.tsx의 alertVariants에 추가
callout: "border-l-4 border-primary bg-primary/5 text-foreground [&>svg]:text-primary",
info:    "border-l-4 border-blue-500 bg-blue-50 text-foreground [&>svg]:text-blue-500",
warning: "border-l-4 border-yellow-500 bg-yellow-50 text-foreground [&>svg]:text-yellow-600",
```

---

### Spinner

```tsx
// components/ui/spinner.tsx
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

const spinnerVariants = cva("animate-spin rounded-full border-2 border-current border-t-transparent", {
  variants: {
    size: {
      sm: "h-4 w-4",
      md: "h-6 w-6",
      lg: "h-8 w-8",
    },
  },
  defaultVariants: { size: "md" },
})

interface SpinnerProps extends VariantProps<typeof spinnerVariants> {
  className?: string
}

export function Spinner({ size, className }: SpinnerProps) {
  return <span className={cn(spinnerVariants({ size }), className)} aria-label="로딩 중" />
}
```

---

### Toast (sonner 래퍼)

```tsx
// components/ui/toast.tsx
export { Toaster } from "sonner"
export { toast } from "sonner"
```

루트 레이아웃에 `<Toaster richColors />` 추가. 색상 커스터마이징:
```tsx
<Toaster
  toastOptions={{
    classNames: {
      toast: "bg-background text-foreground border-border",
      success: "bg-green-50 text-green-900 border-green-200",
      error: "bg-destructive/10 text-destructive border-destructive/20",
    },
  }}
/>
```

---

### Topbar

```tsx
// components/ui/topbar.tsx
import { cn } from "@/lib/utils"

interface TopbarProps {
  left?: React.ReactNode
  center?: React.ReactNode
  right?: React.ReactNode
  className?: string
}

export function Topbar({ left, center, right, className }: TopbarProps) {
  return (
    <header className={cn(
      "sticky top-0 z-50 flex h-14 items-center border-b border-border bg-background px-4",
      className
    )}>
      <div className="flex flex-1 items-center gap-4">{left}</div>
      {center && <div className="absolute left-1/2 -translate-x-1/2">{center}</div>}
      <div className="flex flex-1 items-center justify-end gap-2">{right}</div>
    </header>
  )
}
```

---

### Sidebar

```tsx
// components/ui/sidebar.tsx
import { cn } from "@/lib/utils"

interface SidebarProps {
  children: React.ReactNode
  collapsed?: boolean
  className?: string
}

export function Sidebar({ children, collapsed, className }: SidebarProps) {
  return (
    <aside className={cn(
      "flex h-full flex-col border-r border-border bg-background transition-all duration-200",
      collapsed ? "w-16" : "w-60",
      className
    )}>
      {children}
    </aside>
  )
}
```

---

### StickyActionBar

```tsx
// components/ui/sticky-action-bar.tsx
import { cn } from "@/lib/utils"

interface StickyActionBarProps {
  children: React.ReactNode
  className?: string
}

export function StickyActionBar({ children, className }: StickyActionBarProps) {
  return (
    <div className={cn(
      "sticky bottom-0 z-10 flex items-center justify-end gap-3",
      "border-t border-border bg-background/95 px-6 py-4 backdrop-blur",
      className
    )}>
      {children}
    </div>
  )
}
```

---

### AiChat (선택 시)

```tsx
// components/ui/ai-chat.tsx
"use client"
import * as React from "react"
import { Send } from "lucide-react"
import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Textarea } from "@/components/ui/textarea"
import { ScrollArea } from "@/components/ui/scroll-area"

interface Message { role: "user" | "assistant"; content: string }

interface AiChatProps {
  messages: Message[]
  onSend: (message: string) => void
  loading?: boolean
  className?: string
}

export function AiChat({ messages, onSend, loading, className }: AiChatProps) {
  const [input, setInput] = React.useState("")

  const handleSend = () => {
    if (!input.trim()) return
    onSend(input.trim())
    setInput("")
  }

  return (
    <div className={cn("flex h-full flex-col", className)}>
      <ScrollArea className="flex-1 p-4">
        <div className="flex flex-col gap-3">
          {messages.map((msg, i) => (
            <div key={i} className={cn(
              "max-w-[80%] rounded-[var(--radius)] px-3 py-2 text-body-sm",
              msg.role === "user"
                ? "ml-auto bg-primary text-primary-foreground"
                : "bg-muted text-foreground"
            )}>
              {msg.content}
            </div>
          ))}
        </div>
      </ScrollArea>
      <div className="flex gap-2 border-t border-border p-3">
        <Textarea
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="메시지 입력..."
          className="resize-none"
          rows={1}
          onKeyDown={(e) => { if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); handleSend() } }}
        />
        <Button size="icon" onClick={handleSend} disabled={loading || !input.trim()}>
          <Send className="h-4 w-4" />
        </Button>
      </div>
    </div>
  )
}
```

## 컴포넌트 의존성 요약

| 컴포넌트 | 필요 shadcn 컴포넌트 | 필요 외부 라이브러리 |
|---|---|---|
| Combobox | command, popover, button | lucide-react |
| MultiCombobox | command, popover, button | lucide-react |
| DatePicker | calendar, popover, button | date-fns |
| TimePicker | select | — |
| StepIndicator | — | — |
| EmptyState | — | — |
| Stat | — | — |
| DefinitionList | — | — |
| IndicatorDot | — | class-variance-authority |
| Tag | badge | lucide-react (X icon) |
| Callout | alert | lucide-react |
| Spinner | — | class-variance-authority |
| Toast | — | sonner |
| Topbar | — | — |
| Sidebar | — | — |
| StickyActionBar | — | — |
| AiChat | scroll-area, textarea, button | lucide-react |
