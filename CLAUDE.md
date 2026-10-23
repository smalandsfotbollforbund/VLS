<!-- gitbook-agent-instructions:start -->

## GitBook Documentation Editing

This repository contains documentation synced with GitBook via Git Sync.

Before editing GitBook-synced Markdown, YAML, or asset files, make sure the GitBook skill is available and up to date in your local agent environment. Prefer installing or updating it with:

```bash
npx skills add gitbookio/gitbook-skills
```

This command may add or update local agent skill files. Use them only as local agent instructions; do not commit those installed skill files or any tool-generated agent configuration unless the user explicitly asks for it.

If `npx` is unavailable, load the skill from:

https://gitbook.com/docs/skill.md

When making changes, preserve GitBook sync metadata such as frontmatter, `SUMMARY.md`, `gitbook-docs.yaml`, `.gitbook/`, and asset links unless the requested edit explicitly requires changing them.

<!-- gitbook-agent-instructions:end -->

## Innehållsarkitektur och länkregler

VLS-repot innehåller tre separata spaces med täta skott dem emellan.

### Aktiva spaces

| Mapp | Space | Syfte |
|------|-------|-------|
| `modell/` | Styrmodell | Principer, strukturer och roller |
| `dokument/` | Styrdokument | Konkreta dokument: arbetsordningar, instruktioner, delegationsordning, reglemente |
| `varumarke/` | Varumärkesmanual | Visuell identitet, tonalitet och berättelsen |

### Länkregler (ska alltid följas)

1. **`modell/` → `dokument/`**: tillåtet. Styrmodellen får hänvisa till konkreta dokument.
2. **`dokument/` → `modell/`**: INTE tillåtet. Dokument får inte länka tillbaka till styrmodellen.
3. **Intern länkning**: varje space länkar fritt inom sig själv.
4. **`varumarke/`**: komplett silo — inga länkar ut till `modell/` eller `dokument/`, och inga av de andra spacen länkar in till `varumarke/`.
5. **Duplicering i `modell/`**: varje sida beskriver ett koncept. Om samma information behövs på flera ställen används en länk, inte en kopia av texten.

### Kontrollera vid varje ändring

Innan du commitar ändringar i något av dessa spaces: kontrollera att inga nya tvärslänkar bryter mot reglerna ovan. Kör:

```bash
# Hitta otillåtna länkar: dokument/ → modell/
grep -rn "\.\./modell\|styrning\.smaland\.football/\(ledning\|organisation\|demokrati\|grund\|strategi\|arbetssatt\|granskande\|underliggande-organ\)" dokument/ 2>/dev/null

# Hitta otillåtna länkar: varumarke/ → utanför varumarke/
grep -rn "\.\./modell\|\.\./dokument\|styrning\.smaland\.football" varumarke/ 2>/dev/null

# Hitta otillåtna länkar: modell/ eller dokument/ → varumarke/
grep -rn "\.\./varumarke" modell/ dokument/ 2>/dev/null
```

Om kommandona ger träffar är det regelbrott som ska åtgärdas.
