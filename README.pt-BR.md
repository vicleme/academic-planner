# Planejador Acadêmico

🌐 [English](README.md) · **Português (Brasil)**

Duas ferramentas leves, sem dependências de instalação, para organizar a graduação: **o que ainda falta cursar** e **quando e onde acontecem as aulas**. Cada ferramenta é um único arquivo HTML, e uma página inicial em comum leva a ambas.

> A interface das ferramentas está em português do Brasil. A documentação oficial é o [README em inglês](README.md); este arquivo é a tradução.

## Ferramentas

| Ferramenta | Caminho | O que faz |
| --- | --- | --- |
| **Grade curricular** | [`curriculum/`](curriculum/) | Acompanha cada disciplina por ciclo com um status (cursada, dispensada, proficiência, cursando, pendente), mostra o progresso geral e permite reordenar as disciplinas. Exporta em JSON, PDF e PNG. |
| **Grade de aulas** | [`schedule/`](schedule/) | Monta a grade semanal (disciplina, professor, sala, ciclo), com aulas extracurriculares e monitorias que podem se sobrepor às aulas regulares. Exporta em JSON, PDF, PNG e DOCX. |

Ambas têm desfazer/refazer, importação de JSON e tema claro/escuro/automático, compartilhado entre elas e a página inicial.

## Estrutura do repositório

```text
academic-planner/
├── index.html                 # Página inicial (EN/PT) com links para as duas ferramentas
├── README.md                  # Documentação oficial (inglês)
├── README.pt-BR.md            # Tradução em português (Brasil)
├── curriculum/
│   ├── index.html             # Grade curricular
│   └── examples/
│       ├── ads-2019-2021.json
│       └── data-science-2024-present.json
└── schedule/
    ├── index.html             # Construtor de grade de aulas
    └── examples/
        └── schedule-2026-2.json
```

## Como começar

Não há etapa de build.

```bash
git clone <url-do-repositorio>
cd academic-planner
```

Depois, abra o `index.html` no navegador ou sirva a pasta com qualquer servidor estático, por exemplo:

```bash
python3 -m http.server 8000
```

O repositório também pode ser publicado como está no GitHub Pages (publicação a partir da raiz da branch padrão).

**Observação:** a exportação para PDF, PNG e DOCX usa [html2canvas](https://html2canvas.hertzen.com/), [jsPDF](https://github.com/parallax/jsPDF) e [JSZip](https://stuk.github.io/jszip/), carregados do cdnjs, então exportar exige conexão com a internet. O restante funciona offline.

## Dados e privacidade

Tudo roda no navegador. Nada é enviado a servidor; o estado fica no `localStorage` do seu dispositivo.

| Chave | Usada por | Conteúdo |
| --- | --- | --- |
| `gcurr` | Grade curricular | Grade atual |
| `grade`, `gradeOrd` | Grade de aulas | Grade atual e ordenação da lista de aulas |
| `gtheme` | Todas as páginas | Tema: `auto`, `light` ou `dark` |
| `glang` | Página inicial | Idioma: `en` ou `pt` |

Use o botão **JSON** para fazer backup, pois limpar os dados do navegador apaga tudo.

## Usando os arquivos de exemplo

Abra uma ferramenta e use **Importar JSON** para carregar um arquivo da pasta `examples/`.

## Formatos de JSON

### Grade curricular

```json
{
  "titulo": "Título do curso",
  "ciclos": [
    {
      "nome": "1º Ciclo",
      "modo": "d",
      "itens": [{ "t": "Nome da disciplina", "p": "2026/1", "s": 1 }]
    }
  ]
}
```

| Campo | Significado |
| --- | --- |
| `ciclos[].modo` | Ordenação: `d` padrão, `p` personalizada, `a` A→Z, `s` por progresso |
| `itens[].t` | Nome da disciplina |
| `itens[].p` | Semestre de integralização (`ano/semestre`), realizado ou previsto |
| `itens[].s` | Status: `0` pendente, `1` cursada, `2` dispensada, `3` proficiência, `4` cursando |

### Grade de aulas

```json
{
  "titulo": "Grade de aulas",
  "dias": ["Segunda-feira", "Terça-feira"],
  "horarios": [{ "inicio": "07:40", "fim": "09:20" }],
  "aulas": [
    {
      "dia": 0, "de": 0, "ate": 0,
      "disciplina": "Disciplina", "professor": "Nome", "sala": "Sala",
      "ciclo": "1º Ciclo", "extracurricular": false, "monitoria": false
    }
  ]
}
```

`dia`, `de` e `ate` são índices (começando em zero) de `dias` e `horarios`; `de` e `ate` indicam o primeiro e o último horário da aula.

## Contribuindo

Issues e pull requests são bem-vindos. Os commits seguem o padrão [Conventional Commits](https://www.conventionalcommits.org/).
