# mermaid-playground

Playground to see what diagram types render in markdown, for testing markdown editors and GitHub.

## Category 1: Diagramming Tools

### 1.1 Mermaid.js

Renders natively on GitHub, GitLab, Notion, Obsidian, JetBrains, and via VS Code extensions.

```mermaid
flowchart TD
    A([Start]) --> B{Decision}
    B -->|Yes| C[Option A]
    B -->|No| D[Option B]
    C --> E([End])
    D --> E
```

```mermaid
sequenceDiagram
    participant User
    participant API
    participant DB
    User->>API: POST /diagrams
    API->>DB: INSERT diagram
    DB-->>API: OK
    API-->>User: 201 Created
```

```mermaid
classDiagram
    class DiagramRenderer {
        +render(source: string) SVG
        +parse(source: string) Graph
    }
    class MiroTheme {
        +colors: Map
        +shapes: Map
        +apply(graph: Graph) StyledGraph
    }
    DiagramRenderer --> MiroTheme : uses
```

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> InReview : submit
    InReview --> Approved : approve
    InReview --> Draft : request changes
    Approved --> Published : publish
    Published --> [*]
```

```mermaid
erDiagram
    BOARD ||--o{ DIAGRAM : contains
    DIAGRAM ||--|{ NODE : has
    DIAGRAM ||--|{ EDGE : has
    NODE {
        string id
        string label
        string type
    }
    EDGE {
        string source
        string target
        string label
    }
```

```mermaid
gantt
    title Phase 1 Implementation
    dateFormat  YYYY-MM-DD
    section SVG Renderer
    Parser integration    :a1, 2026-04-07, 5d
    Layout engine         :a2, after a1, 5d
    SVG rendering         :a3, after a2, 5d
    Public API            :a4, after a3, 3d
    section VS Code Extension
    Scaffolding           :b1, after a3, 3d
    markdown-it plugin    :b2, after b1, 5d
    Push command          :b3, after b2, 3d
    section MCP Server
    Scaffolding           :c1, 2026-04-07, 3d
    list tool             :c2, after c1, 3d
    push tool             :c3, after c2, 3d
    pull tool             :c4, after c3, 3d
    OAuth                 :c5, after c4, 3d
```

```mermaid
pie title Markdown Editor Market Share (Engineers)
    "VS Code + Cursor + Windsurf" : 55
    "JetBrains" : 25
    "Other" : 20
```

```mermaid
gitGraph
    commit
    commit
    branch feature/miro-renderer
    checkout feature/miro-renderer
    commit id: "parser"
    commit id: "layout"
    commit id: "svg-render"
    checkout main
    merge feature/miro-renderer
    commit
```

Mermaid supports icons via the `iconify` integration (requires Mermaid v11+). Icons are referenced with `::icon()` syntax or the newer architecture diagram syntax:

```mermaid
architecture-beta
    group cloud(cloud)[Cloud Infrastructure]

    service api(server)[API Server] in cloud
    service db(database)[Database] in cloud
    service disk(disk)[Storage] in cloud
    service users(internet)[Users]

    users:R --> L:api
    api:R --> L:db
    api:B --> T:disk
```

```mermaid
flowchart LR
    A[fa:fa-code Engineer writes Mermaid] --> B[fa:fa-eye Preview in VS Code]
    B --> C[fa:fa-share Push to Miro]
    C --> D[fa:fa-users Team collaborates]
    D --> E[fa:fa-download Pull back to code]
    E --> A
```

```mermaid
flowchart TD
    A[fa:fa-laptop VS Code + Cursor + Windsurf] --> R[fa:fa-cube SVG Renderer Library]
    B[fa:fa-code JetBrains] --> R
    C[fa:fa-book Docs Sites] --> R
    R --> D[fa:fa-image Miro-styled SVG]
    R --> E[fa:fa-link MCP Server]
    E --> F[fa:fa-columns Miro Board]
```

Note: Icon support varies by platform. Font Awesome icons (`fa:fa-*`) work in most Mermaid renderers. The `architecture-beta` diagram type is newer and may not be supported everywhere yet.

### 1.2 PlantUML

Requires a PlantUML server or VS Code extension (jebbs.plantuml). Does NOT render on GitHub.

```plantuml
@startuml
actor User
participant "VS Code" as VSCode
participant "Miro MCP" as MCP
participant "Miro Board" as Board

User -> VSCode : Write ```miro block
VSCode -> VSCode : Render SVG preview
User -> VSCode : "Open in Miro Board"
VSCode -> MCP : push_syntax_diagram
MCP -> Board : Create widget
Board --> MCP : Board URL
MCP --> VSCode : Board URL
VSCode --> User : Opens board in browser
@enduml
```

```plantuml
@startuml
class MiroDiagramRenderer {
  +renderMiroSvg(source: string): Promise<string>
  -parseMermaid(source: string): ParsedGraph
  -layoutGraph(graph: ParsedGraph): LayoutResult
  -renderSvg(layout: LayoutResult): string
}

class DesignRequirements {
  +nodes: Map<string, NodeStyle>
  +edges: EdgeStyle
  +load(path: string): void
}

MiroDiagramRenderer --> DesignRequirements
@enduml
```

### 1.3 Graphviz / DOT

Renders in VS Code via extensions (e.g. Graphviz Markdown Preview). Does NOT render on GitHub.

```dot
digraph architecture {
    rankdir=LR
    node [shape=box, style=rounded]

    subgraph cluster_editor {
        label="Editor (VS Code/Cursor)"
        markdown [label="Markdown File\n(```miro block)"]
        plugin [label="markdown-it\nplugin"]
        preview [label="SVG Preview"]
    }

    subgraph cluster_lib {
        label="npm package"
        parser [label="Mermaid\nParser"]
        layout [label="Layout\nEngine"]
        renderer [label="SVG\nRenderer"]
    }

    subgraph cluster_miro {
        label="Miro"
        mcp [label="MCP Server"]
        board [label="Miro Board\n(Mermaid Widget)"]
    }

    markdown -> plugin -> parser -> layout -> renderer -> preview
    plugin -> mcp [label="push/pull" style=dashed]
    mcp -> board
}
```

```dot
digraph phasing {
    rankdir=TB
    node [shape=box, style="filled,rounded"]

    phase1 [label="Phase 1\nVS Code + MCP Server" fillcolor="#B8E6C8"]
    phase2 [label="Phase 2\nremark/rehype + JetBrains" fillcolor="#C8D8F0"]
    phase3 [label="Phase 3\nConfluence + Browser Ext" fillcolor="#FFF4B8"]
    phase4 [label="Phase 4\nGitHub/GitLab/Notion\n(Partnerships)" fillcolor="#FFD4D4"]

    phase1 -> phase2 -> phase3 -> phase4
}
```

### 1.4 D2 (Terrastruct)

Renders in VS Code via the D2 extension. Does NOT render on GitHub.

```d2
direction: right

editor: VS Code {
  md: Markdown File
  plugin: markdown-it plugin
  preview: SVG Preview
}

lib: Renderer Library {
  parser: Mermaid Parser
  layout: Layout Engine
  svg: SVG Renderer
}

miro: Miro {
  mcp: MCP Server
  board: Board (Widget)
}

editor.md -> editor.plugin -> lib.parser -> lib.layout -> lib.svg -> editor.preview
editor.plugin -> miro.mcp: push/pull {style.stroke-dash: 3}
miro.mcp -> miro.board
```

### 1.5 Nomnoml

Renders via Kroki or dedicated extensions. Does NOT render on GitHub.

```nomnoml
[<frame>Miro Diagram Rendering|
  [Editor] -> [markdown-it plugin]
  [markdown-it plugin] -> [Mermaid Parser]
  [Mermaid Parser] -> [Layout Engine]
  [Layout Engine] -> [SVG Renderer]
  [SVG Renderer] -> [Preview Pane]
  [markdown-it plugin] --> [MCP Server]
  [MCP Server] --> [Miro Board]
]
```

### 1.6 WaveDrom (Digital Timing Diagrams)

Niche - for hardware/digital design. Renders via Kroki.

```wavedrom
{ "signal": [
  { "name": "push",  "wave": "0.1..0.1..0" },
  { "name": "render","wave": "0..1..0.1.0" },
  { "name": "pull",  "wave": "0....1..0.." },
  { "name": "sync",  "wave": "0.......1.0" }
]}
```

---

## Category 2: Non-Diagramming Rendered Content

### 2.1 Math / LaTeX

Renders natively on GitHub (since May 2022), Obsidian, Jupyter, and most docs generators.

Inline math: The rendering time complexity is $O(n \log n)$ where $n$ is the number of nodes.

Block math:

$$
SVG_{output} = \sum_{i=1}^{N} \text{renderNode}(n_i) + \sum_{j=1}^{M} \text{renderEdge}(e_j)
$$

$$
\text{Layout}(G) = \arg\min_{pos} \sum_{(u,v) \in E} \|pos(u) - pos(v)\|^2
$$

### 2.2 GeoJSON

Renders natively on GitHub (code fence since March 2022).

```geojson
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": { "name": "Miro HQ (Amsterdam)", "marker-color": "#FFD02F" },
      "geometry": { "type": "Point", "coordinates": [4.9041, 52.3676] }
    },
    {
      "type": "Feature",
      "properties": { "name": "San Francisco Office", "marker-color": "#FFD02F" },
      "geometry": { "type": "Point", "coordinates": [-122.4194, 37.7749] }
    }
  ]
}
```

### 2.3 TopoJSON

Renders natively on GitHub (code fence since March 2022).

```topojson
{
  "type": "Topology",
  "objects": {
    "example": {
      "type": "GeometryCollection",
      "geometries": [
        {
          "type": "Point",
          "coordinates": [4.9041, 52.3676],
          "properties": { "name": "Amsterdam" }
        }
      ]
    }
  }
}
```

### 2.4 STL (3D Models)

Renders natively on GitHub (code fence since March 2022). Only ASCII STL, not binary.

```stl
solid cube
  facet normal 0 0 -1
    outer loop
      vertex 0 0 0
      vertex 1 0 0
      vertex 1 1 0
    endloop
  endfacet
  facet normal 0 0 -1
    outer loop
      vertex 0 0 0
      vertex 1 1 0
      vertex 0 1 0
    endloop
  endfacet
  facet normal 0 0 1
    outer loop
      vertex 0 0 1
      vertex 1 1 1
      vertex 1 0 1
    endloop
  endfacet
  facet normal 0 0 1
    outer loop
      vertex 0 0 1
      vertex 0 1 1
      vertex 1 1 1
    endloop
  endfacet
  facet normal 0 -1 0
    outer loop
      vertex 0 0 0
      vertex 1 0 1
      vertex 1 0 0
    endloop
  endfacet
  facet normal 0 -1 0
    outer loop
      vertex 0 0 0
      vertex 0 0 1
      vertex 1 0 1
    endloop
  endfacet
  facet normal 1 0 0
    outer loop
      vertex 1 0 0
      vertex 1 0 1
      vertex 1 1 1
    endloop
  endfacet
  facet normal 1 0 0
    outer loop
      vertex 1 0 0
      vertex 1 1 1
      vertex 1 1 0
    endloop
  endfacet
  facet normal 0 1 0
    outer loop
      vertex 0 1 0
      vertex 1 1 0
      vertex 1 1 1
    endloop
  endfacet
  facet normal 0 1 0
    outer loop
      vertex 0 1 0
      vertex 1 1 1
      vertex 0 1 1
    endloop
  endfacet
  facet normal -1 0 0
    outer loop
      vertex 0 0 0
      vertex 0 1 0
      vertex 0 1 1
    endloop
  endfacet
  facet normal -1 0 0
    outer loop
      vertex 0 0 0
      vertex 0 1 1
      vertex 0 0 1
    endloop
  endfacet
endsolid cube
```

### 2.5 Markmap (Mind Maps from Markdown)

Renders in VS Code via the Markmap extension. Does NOT render on GitHub.

```markmap
# Miro Markdown Tag

## Architecture
### SVG Renderer Library
- Mermaid parser
- Layout engine
- SVG generation
### Widget Renderer
- Miro shapes
- Bi-directional editing
### Platform Plugins
- VS Code
- JetBrains
- remark/rehype
### MCP Server
- push_syntax_diagram
- pull_syntax_diagram
- list_syntax_diagrams

## Phasing
### Phase 1
- VS Code extension
- MCP server
### Phase 2
- remark/rehype
- JetBrains
### Phase 3
- Confluence
- Browser extension
### Phase 4
- GitHub partnership
- GitLab partnership
- Notion partnership
```

---

## Rendering Support Matrix

Which examples render where:

| Example | GitHub | VS Code (with extensions) | GitLab | Obsidian |
|---|---|---|---|---|
| Mermaid | Yes | Yes | Yes | Yes |
| PlantUML | No | Yes (jebbs.plantuml) | Yes | Via plugin |
| Graphviz/DOT | No | Yes (graphviz-markdown-preview) | No | Via plugin |
| D2 | No | Yes (terrastruct.d2) | No | Via plugin |
| Nomnoml | No | Via Kroki | No | No |
| WaveDrom | No | Via Kroki | No | No |
| Math/LaTeX | Yes | Yes (KaTeX extensions) | Yes | Yes |
| GeoJSON | Yes | No | No | No |
| TopoJSON | Yes | No | No | No |
| STL | Yes | No | No | No |
| Markmap | No | Yes (markmap extension) | No | Via plugin |
