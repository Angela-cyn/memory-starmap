# Memory Starmap

A 3D memory visualization rendered as an interactive starfield. Each memory is a star — its size and brightness reflect importance, its color represents type. Related memories cluster together through force-directed layout, and connections animate when you select a star.

Built with Three.js. Zero dependencies, single HTML file.

## Features

- **3D force-directed graph** — memories with shared concepts attract each other
- **Orbital animation** — stars orbit with slightly tilted axes after layout stabilizes
- **Meteor showers** — periodic shooting star events with line-trail rendering and bloom glow
- **Liquid Glass UI** — Apple-inspired frosted glass cards with edge chromatic dispersion and animated shimmer
- **Edge growth animation** — relationship lines grow outward from the selected star
- **Linked star highlighting** — related stars glow when a star is selected
- **Breathing loading animation** — particle ring with quintic ease-in convergence
- **Pinned star breathing** — important memories pulse with independent rhythms

## Quick Start

```bash
# Just open it — demo data is included
npx serve .
# or
python -m http.server 8000
```

Open `http://localhost:8000` and you'll see the demo starmap with 25 sample memories.

## Connect Your Own Data

Set `window.STARMAP_CONFIG` before the script loads:

```html
<script>
  window.STARMAP_CONFIG = {
    nodesUrl: '/api/network?mode=bucket',     // required: returns { nodes, edges }
    conceptsUrl: '/api/network?mode=concept', // optional: concept co-occurrence data
    detailUrl: '/api/bucket/{id}',            // optional: detail endpoint, {id} is replaced
  };
</script>
```

### Data Format

**Nodes endpoint** should return:

```json
{
  "nodes": [
    {
      "id": "unique-id",
      "name": "Memory title",
      "type": "feel | diary | permanent | dynamic",
      "importance": 1-10,
      "score": 0-100,
      "pinned": false,
      "resolved": false,
      "domain": ["tag1", "tag2"]
    }
  ],
  "edges": [
    {
      "source": "id-a",
      "target": "id-b",
      "similarity": 0.0-1.0
    }
  ]
}
```

**Concepts endpoint** (optional) returns concept nodes with bucket lists for computing co-occurrence edges:

```json
{
  "nodes": [
    {
      "id": "concept-id",
      "buckets": ["bucket-id-1", "bucket-id-2"]
    }
  ]
}
```

**Detail endpoint** (optional) returns memory content for the detail card:

```json
{
  "content": "Full text content of the memory..."
}
```

## Node Types & Colors

| Type | Color | Description |
|------|-------|-------------|
| `feel` | Orange | Emotional memories |
| `diary` | Blue | Journal entries |
| `permanent` | Purple | Consolidated long-term memories |
| `dynamic` | Light blue | Active working memories |
| pinned | Gold | Important flagged memories (breathing animation) |
| resolved | Gray | Settled/archived memories (dimmed) |

## Customization

Key parameters you can tweak in the source:

- **Background stars**: `count`, cluster shapes, brightness tiers
- **Force layout**: repulsion (`-1200`), attraction target distance, damping
- **Orbit speed**: `_orbitSpeed` ranges for pinned vs. active nodes
- **Bloom**: threshold, strength, radius in `UnrealBloomPass`
- **Meteors**: shower interval, count, speed, tail length
- **Glass card**: blur, saturation, border colors, shimmer speed

## License

MIT
