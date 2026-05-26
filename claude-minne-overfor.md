# Overføre Claude-minne til ny maskin eller katalog

Claude Code lagrer prosjektminne lokalt, knyttet til absolutt sti. Dette dokumentet forklarer hvordan du tar det med deg.

## Hva som må kopieres

| Hva | Fra | Inneholder |
|-----|-----|------------|
| Prosjektminne | `~/.claude/projects/<kodet-sti>/` | Auto-minne, innstillinger per prosjekt |
| Globale innstillinger | `~/.claude/settings.json` | Tillatelser, globale preferanser |

Stikodering: `/` erstattes med `-`. Eksempel:
- `~/prosjekter` → `-home-kjell-prosjekter`
- `~/dev/prosjekter` → `-home-kjell-dev-prosjekter`

## Samme sti på ny maskin

```bash
# På gammel maskin — pakk ned
tar -czf claude-prosjekter.tar.gz \
  ~/.claude/projects/-home-kjell-prosjekter \
  ~/.claude/settings.json

# Kopier til ny maskin, f.eks. med scp
scp claude-prosjekter.tar.gz nymaksin:~

# På ny maskin — pakk opp
tar -xzf ~/claude-prosjekter.tar.gz -C /
```

## Ny sti (f.eks. ~/dev/prosjekter i stedet for ~/prosjekter)

```bash
# Kopier minnemappen og gi den nytt navn
cp -r ~/.claude/projects/-home-kjell-prosjekter \
       ~/.claude/projects/-home-kjell-dev-prosjekter
```

## Sjekk hvilken sti Claude Code bruker nå

```bash
pwd  # kjør fra prosjektkatalogen — denne stien er det som kodes
```

## Hva som alltid følger med git (krever ikke manuell kopiering)

- `CLAUDE.md` — prosjektinstruksjoner og konvensjoner
- Alle `logg.md`, `todo.md`, `README.md` osv.

## Minne i dette repoet

Minnefilene for dette prosjektet ligger i:
```
~/.claude/projects/-home-kjell-prosjekter/memory/
├── MEMORY.md                     ← indeks
├── user_profile.md               ← brukerprofil
└── project_monorepo_strategi.md  ← repo-strategi
```
