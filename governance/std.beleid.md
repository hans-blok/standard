# Beleid — Standards Repository

**Repository**: standards  
**Versie**: 1.4.0  
**Status**: Active  
**Last Updated**: 2025-12-30  
**Eigenaar**: Architecture & AI Enablement

---

## 1. Context en Doel

De **standards** repository bevat alle governance-documenten, agent-charters, fase-charters en templates die de basis vormen voor gestructureerde en consistente AI-agent-ontwikkeling binnen het **Agent Eco-systeem**.

### Agent Eco-systeem Architectuur

Dit beleid is onderdeel van een **centraal agent eco-systeem** met de volgende architectuur:

```
┌─────────────────────────────────────────────────────────────┐
│  Agent Eco-systeem (Centraal)                               │
│  ├── standards/          — Governance, charters, templates  │
│  ├── agent-capabilities/ — PowerShell scripts, tooling      │
│  └── [andere centrale repos]                                │
└─────────────────────────────────────────────────────────────┘
                          ↓ aanroepen
┌─────────────────────────────────────────────────────────────┐
│  Project-Workspaces (Lokaal, "schoon")                      │
│  └── /artefacten/        — Alle gegenereerde artefacten     │
│      ├── a.trigger/                                          │
│      ├── b.architectuur/                                     │
│      ├── c.specificatie/                                     │
│      └── ...                                                 │
│  GEEN .github/agents/, GEEN .github/prompts/                │
└─────────────────────────────────────────────────────────────┘
```

**Kernprincipes**:
- **Centraal beheer**: Alle agents, charters en governance blijven in centrale repositories
- **Schone project-workspaces**: Project-repositories bevatten GEEN agents of prompts
- **Scheiding van verantwoordelijkheden**: Agent-definitie (centraal) vs. artefact-generatie (lokaal)

Dit beleid beschrijft de specifieke regels en werkwijzen die gelden binnen deze repository en die aanvullend zijn op de algemene constitutie.

---

## 2. Taalgebruik

**Alle documentatie, charters en communicatie binnen deze repository gebeuren in het Nederlands.**

- Technische termen mogen in het Engels blijven indien er geen gangbare Nederlandse vertaling bestaat
- Code, identifiers en bestandsnamen volgen internationale conventies (Engels)
- Agents communiceren in het Nederlands op taalniveau B1
- Bij twijfel geldt: helderheid en begrijpelijkheid boven formaliteit

---

## 3. Governance-structuur

Deze repository wordt beheerd volgens de principes en werkwijze zoals vastgelegd in:

**`agent.charters/std.agent.charter.moeder.md`**

De Moeder Agent is verantwoordelijk voor:
- Het ontwerpen en creëren van nieuwe agents
- Het valideren en verbeteren van agent-charters
- Het waarborgen van consistentie en kwaliteit in het agent-landschap
- Het voorkomen van scope-overlap en conflicten tussen agents

Alle agent-ontwikkeling, charter-wijzigingen en governance-besluiten volgen de principes en werkwijze zoals beschreven in het charter van de Moeder Agent.

---
### 3.1 Git Beperkingen
Agents hebben **strikte beperkingen** op Git-operaties:

- ❌ **Geen push naar GitHub**: Agents mogen NOOIT direct pushen naar GitHub repositories
- ✅ **Lokaal werken**: Agents mogen alleen lokale bestanden creëren en wijzigen
- ✅ **Git add/commit**: Agents mogen lokaal committen (optioneel, alleen indien expliciet gevraagd)
- ❌ **Geen remote operaties**: Geen push, pull, fetch zonder menselijke tussenkomst
- ✅ **Menselijke controle**: Alle wijzigingen moeten door een mens worden gereviewd en gepushed

**Rationale**: 
- Kwaliteitscontrole door menselijke review voordat code naar remote gaat
- Voorkomen van ongewenste of foutieve wijzigingen in gedeelde repositories
- Behoud van traceerbaarheid en verantwoordelijkheid

## 4. Documentstructuur

De repository hanteert de volgende standaard-structuur:

```
governance/           — Constitutie, beleid, kwaliteitseisen, frameworks
agent.charters/       — Volledige agent-charters
phase.charters/       — Fase-specifieke charters (SAFe)
templates/            — Herbruikbare templates voor charters
desc-agents/          — Beknopte agent-beschrijvingen (overzicht)
```

---

## 5. Artefact-creatie en Project-structuur

### 5.1 PowerShell Scripts Locatie
Alle PowerShell scripts voor het realiseren van artefacten bevinden zich in de repository **agent-capabilities**.

### 5.2 Artefact-locatie: Lokale Project-Workspaces
Alle artefacten die door agents worden gegenereerd, worden aangemaakt in de **lokale project-workspace**, niet in de standards repository.

**Project-Workspace Principes**:
- **Schone workspace**: Project-workspaces bevatten GEEN `.github/agents/` of `.github/prompts/` folders
- **Alleen artefacten**: Project-workspaces bevatten uitsluitend gegenereerde artefacten en applicatiecode
- **Centraal agent-beheer**: Alle agents blijven in het centrale agent eco-systeem (standards repository)
- **Input via project-pad**: Agents ontvangen als input het **project-pad** (lokale workspace waar gewerkt wordt)
- **Output naar project**: Artefacten worden gegenereerd in de project-specifieke locatie

### 5.3 Folder-structuur conform Delivery Framework
Alle artefacten worden in de **artefacten** folder geplaatst conform het **Delivery Framework** zoals beschreven in `governance/delivery-framework.md`.

**Fase-folders in projectrepositories**:
```
<project-root>/
└── artefacten/
    ├── a.trigger/          — Business cases, initiaties
    ├── b.architectuur/     — ADR's, architectuurpatronen
    ├── c.specificatie/     — Requirements, features, datamodellen
    ├── d.ontwerp/          — API designs, technisch ontwerp
    ├── e.bouw/             — Code, scripts, implementatie
    ├── f.validatie/        — Test rapporten, validatie
    ├── g.deployment/       — Release notes, deployment scripts
    └── u.utility/          — Ondersteunende tools en utilities
```

**Naamgevingsconventie**:
- Alle fase-folders volgen het patroon: `<fase letter lowercase>.<fase naam lowercase>`
- Voorbeelden: `a.trigger`, `b.architectuur`, `c.specificatie`, `d.ontwerp`

### 5.4 Automatische Folder-creatie
**Wanneer een folder niet bestaat in de projectrepository, wordt deze automatisch aangemaakt door de agent.**

**Werkwijze**:
1. Agent ontvangt project-pad als input
2. Agent bepaalt fase (a t/m g of u) op basis van eigen charter
3. Agent controleert of `artefacten/<fase>` folder bestaat
4. Indien niet: agent creëert folder-structuur `artefacten/<fase letter lowercase>.<fase naam>`
5. Agent genereert artefact in juiste folder

**Voorbeeld**:
- Agent `std.c.requirements-writer` werkt in fase C (Specificatie)
- Agent ontvangt input: project-pad = `C:\projects\myapp`
- Agent controleert of `C:\projects\myapp\artefacten\c.specificatie\` bestaat
- Indien niet: agent creëert folder-structuur `artefacten/c.specificatie/`
- Agent genereert requirements in `C:\projects\myapp\artefacten\c.specificatie\requirements.md`

---

## 6. Charter-eisen

Alle agents in dit ecosysteem:
- Hebben een volledig ingevuld charter conform `templates/agent.charter.template.md`
- Voldoen aan de principes uit de constitutie
- Hebben een eenduidige, niet-overlappende scope
- Documenteren expliciete inputs, outputs en samenwerkingspatronen
- Benoemen anti-patterns en verboden gedrag

---

## 7. Wijzigingsproces

### Governance-documenten
- **Constitutie**: Alleen inhoudelijke wijzigingen door mens; redactionele wijzigingen door Logos Agent
- **Beleid**: Wijzigingen via expliciet updateproces en versiebeheer
- **Kwaliteitseisen en frameworks**: Wijzigingen na review door architect

### Agent-charters
- Nieuwe charters via Moeder Agent en Charter Writer Agent
- Wijzigingen aan bestaande charters via gecontroleerd proces
- Alle wijzigingen worden gedocumenteerd in de Change Log van het charter

---

## 8. Kwaliteitsnormen

Alle artefacten in deze repository voldoen aan:
- **Volledigheid**: Geen ontbrekende secties of impliciete informatie
- **Consistentie**: Terminologie en structuur zijn uniform
- **Traceerbaarheid**: Beslissingen zijn herleidbaar naar bron
- **Helderheid**: Begrijpelijk op B1-niveau
- **Evolutie**: Versiebeheersing en change logs

---

## 9. Agent-gedrag binnen Agent Eco-systeem

### 9.1 Agents in Centrale Repository (standards)
Agents die binnen deze repository werken:
1. Lezen eerst alle governance-documenten (constitutie, beleid, relevante charters)
2. Handelen conform hun charter en binnen hun scope
3. Escaleren bij scope-overlap of conflicten
4. Documenteren aannames expliciet
5. Leveren alleen complete, gevalideerde outputs

### 9.2 Agents Werkend op Project-Workspaces
Agents die artefacten genereren in project-workspaces:
1. Worden aangeroepen vanuit de **centrale agent eco-systeem repository**
2. Ontvangen als input het **project-pad** van de lokale workspace
3. Genereren artefacten in fase-folders binnen de project-workspace
4. Creëren automatisch benodigde folder-structuur indien deze niet bestaat
5. Plaatsen **GEEN** agent-definities of prompts in de project-workspace
6. Houden project-workspaces "schoon" en gefocust op artefacten
7. **Pushen GEEN code naar GitHub repositories** — agents mogen wijzigingen doorvoeren in de lokale workspace, maar het pushen naar remote repositories (git push) is niet toegestaan

---

## 10. Relatie tot Constitutie

Dit beleid is ondergeschikt aan `governance/constitutie.md`.

Bij conflict tussen dit beleid en de constitutie geldt altijd de constitutie.

---

## 11. Change Log

| Datum | Versie | Wijziging | Auteur |
|------|--------|-----------|--------|
| 2025-12-14 | 1.0.0 | Initiële versie | Moeder Agent |
| 2025-12-30 | 1.1.0 | Toegevoegd: Artefact-creatie beleid (§5) — PowerShell scripts in agent-capabilities, artefacten in lokale repos, folder-structuur conform Delivery Framework, automatische folder-creatie | Moeder Agent |
| 2025-12-30 | 1.2.0 | Toegevoegd: Agent Eco-systeem architectuur (§1) — Centraal agent-beheer, schone project-workspaces zonder agents/prompts, scheiding verantwoordelijkheden; Uitgebreid: Agent-gedrag (§9.2) | Moeder Agent |
| 2025-12-30 | 1.3.0 | Toegevoegd: Verbod op git push door agents (§9.2.7) — agents mogen geen code pushen naar GitHub repositories | Human |
| 2025-12-30 | 1.4.0 | Gewijzigd: Folder-structuur (§5.3, §5.4) — alle artefacten in centrale "artefacten" folder met naamgevingsconventie `<fase letter lowercase>.<fase naam>` | Human |

