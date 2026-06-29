# Mermaid `linkStyle` Samples

## 1. Basic stroke color and width by index

```mermaid
flowchart LR
  A --> B --> C --> D
  linkStyle 0 stroke:#e74c3c,stroke-width:3px
  linkStyle 1 stroke:#2ecc71,stroke-width:5px
  linkStyle 2 stroke:#3498db,stroke-width:1px
```

## 2. Multiple indexes in one statement

```mermaid
flowchart LR
  A --> B --> C --> D --> E
  linkStyle 0,2 stroke:#e74c3c,stroke-width:4px
  linkStyle 1,3 stroke:#3498db,stroke-width:2px
```

## 3. `default` — applies to all links

```mermaid
flowchart LR
  A --> B --> C --> D
  linkStyle default stroke:#9b59b6,stroke-width:3px
```

## 4. Dashed lines via `stroke-dasharray`

> Note: in some Mermaid versions commas inside `stroke-dasharray` need to be escaped as `\,`

```mermaid
flowchart LR
  A --> B
  A --> C
  linkStyle 0 stroke:#e74c3c,stroke-width:2px,stroke-dasharray:5
  linkStyle 1 stroke:#3498db,stroke-width:2px,stroke-dasharray:10
```

## 5. `fill:none` (prevents filled shape artifacts on edges)

```mermaid
flowchart TD
  A --> B --> C
  linkStyle default fill:none,stroke:#e74c3c,stroke-width:3px
```

## 6. `color` for label text (inconsistent across renderers)

```mermaid
flowchart LR
  A -->|label| B
  A -->|label| C
  linkStyle 0 stroke:#e74c3c,stroke-width:3px,color:#e74c3c
  linkStyle 1 stroke:#2ecc71,stroke-width:3px,color:#2ecc71
```

## 7. `-` shorthand — attach style to the previous link (newer Mermaid only)

> May not work on GitHub, which uses a pinned older Mermaid version.

```mermaid
flowchart TD
  A --> B
  linkStyle - stroke:#e74c3c,stroke-width:3px
  B --> C
  linkStyle - stroke:#2ecc71,stroke-width:3px
  C --> D
  linkStyle - stroke:#3498db,stroke-width:3px
```

## 8. Mixing `default` with per-index overrides

```mermaid
flowchart LR
  A --> B --> C --> D
  linkStyle default stroke:#bdc3c7,stroke-width:1px
  linkStyle 1 stroke:#e74c3c,stroke-width:4px
```

---

## Notes

| Property | Support |
|---|---|
| `stroke` | Generally reliable |
| `stroke-width` | Most commonly broken — renderer may strip it |
| `color` | Label text color; most inconsistent across versions |
| `fill:none` | Worth including in `default` to prevent polygon artifacts |
| `-` shorthand | Newer Mermaid only; not yet on GitHub |
| Multiple indexes (`0,2`) | Works in recent Mermaid versions |