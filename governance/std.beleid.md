# Beleid — Standards Repository

**Repository**: standards  
**Versie**: 1.0.0  
**Status**: Active  
**Last Updated**: 2025-12-14  
**Eigenaar**: Architecture & AI Enablement

---

## 1. Context en Doel

De **standards** repository bevat alle governance-documenten, agent-charters, fase-charters en templates die de basis vormen voor gestructureerde en consistente AI-agent-ontwikkeling binnen het ecosysteem.

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

## 5. Charter-eisen

Alle agents in dit ecosysteem:
- Hebben een volledig ingevuld charter conform `templates/agent.charter.template.md`
- Voldoen aan de principes uit de constitutie
- Hebben een eenduidige, niet-overlappende scope
- Documenteren expliciete inputs, outputs en samenwerkingspatronen
- Benoemen anti-patterns en verboden gedrag

---

## 6. Wijzigingsproces

### Governance-documenten
- **Constitutie**: Alleen inhoudelijke wijzigingen door mens; redactionele wijzigingen door Logos Agent
- **Beleid**: Wijzigingen via expliciet updateproces en versiebeheer
- **Kwaliteitseisen en frameworks**: Wijzigingen na review door architect

### Agent-charters
- Nieuwe charters via Moeder Agent en Charter Writer Agent
- Wijzigingen aan bestaande charters via gecontroleerd proces
- Alle wijzigingen worden gedocumenteerd in de Change Log van het charter

---

## 7. Kwaliteitsnormen

Alle artefacten in deze repository voldoen aan:
- **Volledigheid**: Geen ontbrekende secties of impliciete informatie
- **Consistentie**: Terminologie en structuur zijn uniform
- **Traceerbaarheid**: Beslissingen zijn herleidbaar naar bron
- **Helderheid**: Begrijpelijk op B1-niveau
- **Evolutie**: Versiebeheersing en change logs

---

## 8. Agent-gedrag binnen deze Repository

Agents die binnen deze repository werken:
1. Lezen eerst alle governance-documenten (constitutie, beleid, relevante charters)
2. Handelen conform hun charter en binnen hun scope
3. Escaleren bij scope-overlap of conflicten
4. Documenteren aannames expliciet
5. Leveren alleen complete, gevalideerde outputs

---

## 9. Relatie tot Constitutie

Dit beleid is ondergeschikt aan `governance/constitutie.md`.

Bij conflict tussen dit beleid en de constitutie geldt altijd de constitutie.

---

## 10. Change Log

| Datum | Versie | Wijziging | Auteur |
|------|--------|-----------|--------|
| 2025-12-14 | 1.0.0 | Initiële versie | Moeder Agent |

