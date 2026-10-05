# Academic Planner

🌐 **English** · [Português (Brasil)](README.pt-BR.md)

Two small, dependency-free browser tools for organizing a degree: **what you still have to take** and **when and where your classes happen**. Each tool is a single HTML file, and a shared landing page links to both.

> The tools' interface is in Brazilian Portuguese. This README is the official documentation.

## Tools

| Tool | Path | What it does |
| --- | --- | --- |
| **Curriculum grid** | [`curriculum/`](curriculum/) | Tracks every course by cycle with a status (completed, waived, proficiency, in progress, pending), shows overall progress, and lets you reorder courses. Exports to JSON, PDF and PNG. |
| **Class schedule** | [`schedule/`](schedule/) | Builds a weekly timetable (subject, professor, room, cycle), with extracurricular and tutoring ("monitoria") slots that can overlap regular classes. Exports to JSON, PDF, PNG and DOCX. |

Both tools support undo/redo, import from JSON, and a light/dark/auto theme shared between them and the landing page.

## Repository structure

```text
academic-planner/
├── index.html                 # Landing page (EN/PT) linking to both tools
├── README.md                  # Official documentation (English)
├── README.pt-BR.md            # Portuguese (Brazil) translation
├── curriculum/
│   ├── index.html             # Curriculum grid
│   └── examples/
│       ├── ads-2019-2021.json
│       └── data-science-2024-present.json
└── schedule/
    ├── index.html             # Class schedule builder
    └── examples/
        └── schedule-2026-2.json
```

## Getting started

There is no build step.

```bash
git clone <repository-url>
cd academic-planner
```

Then open `index.html` in a browser, or serve the folder with any static server, for example:

```bash
python3 -m http.server 8000
```

The repository can also be published as-is with GitHub Pages (deploy from the root of the default branch).

**Note:** PDF, PNG and DOCX export rely on [html2canvas](https://html2canvas.hertzen.com/), [jsPDF](https://github.com/parallax/jsPDF) and [JSZip](https://stuk.github.io/jszip/), loaded from cdnjs, so exporting needs an internet connection. Everything else works offline.

## Data and privacy

Everything runs in the browser. Nothing is sent to a server; state is kept in `localStorage` on your device.

| Key | Used by | Content |
| --- | --- | --- |
| `gcurr` | Curriculum grid | Current grid |
| `grade`, `gradeOrd` | Class schedule | Current schedule and course list ordering |
| `gtheme` | All pages | Theme: `auto`, `light` or `dark` |
| `glang` | Landing page | Language: `en` or `pt` |

Use **Export JSON** to back up your data, since clearing browser data erases it.

## Using the example files

Open a tool and use **Importar JSON** (Import JSON) to load a file from the `examples/` folder.

## JSON formats

### Curriculum grid

```json
{
  "titulo": "Course title",
  "ciclos": [
    {
      "nome": "1º Ciclo",
      "modo": "d",
      "itens": [{ "t": "Course name", "p": "2026/1", "s": 1 }]
    }
  ]
}
```

| Field | Meaning |
| --- | --- |
| `ciclos[].modo` | Ordering: `d` default, `p` custom, `a` A→Z, `s` by progress |
| `itens[].t` | Course name |
| `itens[].p` | Completion term (`year/semester`), actual or planned |
| `itens[].s` | Status: `0` pending, `1` completed, `2` waived, `3` proficiency, `4` in progress |

### Class schedule

```json
{
  "titulo": "Grade de aulas",
  "dias": ["Segunda-feira", "Terça-feira"],
  "horarios": [{ "inicio": "07:40", "fim": "09:20" }],
  "aulas": [
    {
      "dia": 0, "de": 0, "ate": 0,
      "disciplina": "Subject", "professor": "Name", "sala": "Room",
      "ciclo": "1º Ciclo", "extracurricular": false, "monitoria": false
    }
  ]
}
```

`dia`, `de` and `ate` are zero-based indexes into `dias` and `horarios` (`de`/`ate` are the first and last time slot of the class).

## Contributing

Issues and pull requests are welcome. Commits follow [Conventional Commits](https://www.conventionalcommits.org/).
