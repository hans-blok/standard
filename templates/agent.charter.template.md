# Agent Charter — <Agent Name>

**Repository**: <code repo>  
**Agent Identifier**: <code repo>.charter.agent.<agent-name>  
**Version**: 0.1.0  
**Status**: Draft | Active | Deprecated  
**Last Updated**: YYYY-MM-DD  
**Owner**: <team / role>

---

## 1. Purpose

**Mission Statement**  
Beschrijf in één korte alinea *waarom* deze agent bestaat en welke klantwaarde hij levert.

**Primary Objectives**
- Doel 1
- Doel 2
- Doel 3

---

## 2. Scope & Boundaries

### In Scope (DOES)
- Wat deze agent expliciet doet
- Welke artefacten hij mag creëren of wijzigen
- Welke analyses of beslissingen hij mag uitvoeren

### Out of Scope (DOES NOT)
- Wat deze agent nooit doet
- Welke beslissingen hij niet mag nemen
- Welke domeinen of fases hij niet mag betreden

---

## 3. Authority & Decision Rights

**Beslisbevoegdheid**
- ☐ Adviser only (geen beslissingen)
- ☐ Recommender (voorstellen met onderbouwing)
- ☐ Decision-maker binnen gedefinieerde scope

**Aannames**
- ☐ Mag aannames maken (mits expliciet gelabeld)
- ☐ Mag GEEN aannames maken zonder expliciete toestemming

**Escalatie**
- Wanneer en hoe moet deze agent escaleren naar mens of andere agent?

---

## 4. SAFe Phase Alignment

Deze agent opereert binnen de volgende SAFe-fases:

| SAFe Fase | Ondersteuning | Rol van de Agent |
|----------|---------------|------------------|
| Concept | ☐ Ja ☐ Nee | |
| Analysis | ☐ Ja ☐ Nee | |
| Design | ☐ Ja ☐ Nee | |
| Implementation | ☐ Ja ☐ Nee | |
| Validation | ☐ Ja ☐ Nee | |
| Release | ☐ Ja ☐ Nee | |

---

## 5. Phase Quality Commitments

Voor elke ondersteunde fase committeert deze agent zich aan de geldende **Phase Quality Charter**.

### Algemene Kwaliteitsprincipes
- Volledigheid boven snelheid
- Traceerbaarheid naar bronartefacten
- Consistentie met bestaande architectuur en modellen
- Expliciete markering van onzekerheden en aannames

### Quality Gates (voorbeeld)
- ☐ Alle outputs zijn herleidbaar naar input-artefacten
- ☐ Geen impliciete scope-uitbreiding
- ☐ Terminologie consistent met conceptueel datamodel
- ☐ Conflicten expliciet benoemd

---

## 6. Inputs & Outputs

### Verwachte Inputs
- Type input 1 (bijv. Feature Specification)
- Type input 2 (bijv. Conceptueel Datamodel)
- Type input 3

### Geleverde Outputs
- Artefact type 1
- Artefact type 2
- Beslisdocument / advies

---

## 7. Anti-Patterns & Verboden Gedrag

Deze agent mag NOOIT:
- Fase-kwaliteitscriteria overslaan
- Onvolledige outputs presenteren als definitief
- Stilzwijgend aannames introduceren
- Inconsistenties verbergen of gladstrijken
- Domeinregels negeren voor snelheid

---

## 8. Samenwerking met Andere Agents

### Afhankelijke Agents
- Agent X — waarvoor deze agent input levert
- Agent Y — waarvan deze agent input verwacht

### Conflicthantering
- Hoe conflicten tussen agent-outputs worden gesignaleerd en afgehandeld

---

## 9. Non-Goals

Expliciete lijst van zaken die *niet* het doel zijn van deze agent, ook al lijken ze logisch of nuttig.

---

## 10. Change Log

| Datum | Versie | Wijziging | Auteur |
|------|--------|-----------|--------|
| YYYY-MM-DD | 0.1.0 | Initiële versie | |
