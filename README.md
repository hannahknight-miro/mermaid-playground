# mermaid-playground

```mermaid
flowchart TD
 A[Christmas] -->|Get money| B(Go shopping)
 B --> C{Let me think}
 C -->|One| D[Laptop]
 C -->|Two| E[iPhone]
 C -->|Three| F[Car]
```

## Mermaid diagram with icons
[!IMPORTANT]
Custom Iconify packs require a custom Mermaid configuration. On GitHub, these icons may render as text labels (e.g., "i:logos:react") because GitHub's environment is sandboxed.

```mermaid
architecture-beta
    group api(logos:aws-lambda)[API Layer]
    
    service db(logos:postgresql)[Database] in api
    service server(logos:nodejs)[Server] in api
    
    junction junctionPoint in api
    
    server:R --> junctionPoint
    junctionPoint --> db:L
```

# Other examples

## Latex example
This is an inline equation: $$V_{sphere} = \frac{4}{3}\pi r^3$$,<br>
followed by a display style equation:

$$V_{sphere} = \frac{4}{3}\pi r^3$$


