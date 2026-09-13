# Claude-Code-Konfiguration dieses Projekts

Alles in diesem Ordner gilt für **jede** Claude-Code-Session, die in diesem Repo
arbeitet — lokal, im Web und in Cloud-Sessions. Damit muss niemand mehr etwas
von Hand nachinstallieren.

Getrackt ist hier bewusst nur das Geteilte: `settings.json`, diese Datei und
`skills/`. Lokaler Kram gehört nicht ins Repo — wenn Claude Code hier
Session-Zustand, Worktrees oder eine `settings.local.json` anlegt, nimm sie in
die `.gitignore` auf.

## Was schon eingerichtet ist

`settings.json` registriert den Marketplace `obra/superpowers-marketplace` und
aktiviert das Plugin **Superpowers** (v6.3.0, 14 Skills: TDD, systematisches
Debugging, Brainstorming, Plan-Erstellung und -Ausführung, Code-Review,
Git-Worktrees).

## Weitere Skills ergänzen

Es gibt zwei Wege, je nachdem, was du heruntergeladen hast.

### 1. Ein Plugin aus einem Marketplace

Trag Marketplace und Plugin in `settings.json` ein:

```json
{
  "extraKnownMarketplaces": {
    "superpowers-marketplace": {
      "source": { "source": "github", "repo": "obra/superpowers-marketplace" }
    },
    "NEUER-MARKETPLACE": {
      "source": { "source": "github", "repo": "OWNER/REPO" }
    }
  },
  "enabledPlugins": {
    "superpowers@superpowers-marketplace": true,
    "PLUGIN@NEUER-MARKETPLACE": true
  }
}
```

Alternativ im laufenden Claude Code, was dasselbe Ergebnis schreibt:

```
/plugin marketplace add OWNER/REPO
/plugin install PLUGIN@MARKETPLACE
```

Die Skills melden sich danach mit dem Präfix des Plugins, z. B.
`superpowers:brainstorming`.

### 2. Ein einzelner Skill

Leg ihn als eigenen Ordner unter `skills/` ab:

```
.claude/skills/mein-skill/SKILL.md
```

`SKILL.md` braucht einen YAML-Kopf mit `name` und `description` — die
`description` entscheidet, wann der Skill greift, also gehören die
Auslösebegriffe hinein. Ein Eintrag in `settings.json` ist dafür **nicht**
nötig; Skills in diesem Ordner werden automatisch gefunden.

## Zwei Stolpersteine

**Skills und Plugins laden unterschiedlich schnell.** Ein Skill, den du unter
`skills/` ablegst, steht sofort zur Verfügung — die laufende Session findet ihn,
ohne Neustart. Ein **Plugin** dagegen wird beim Session-Start eingelesen: Was du
in `settings.json` einträgst, greift erst in der nächsten Session.

**Doppelte Skills vermeiden.** Skills können aus drei Quellen kommen: aus diesem
Repo, aus einem Plugin und aus den über dein Claude-Konto synchronisierten
Skills. Liegt derselbe Skill in zwei Quellen, erscheint er doppelt (etwa
`superpowers:brainstorming` neben `anthropic-skills:brainstorming`) — beide
Beschreibungen kosten Tokens und konkurrieren bei der Auswahl. Bevor du einen
Skill hier ablegst: prüfe, ob ein Plugin ihn schon mitbringt.
