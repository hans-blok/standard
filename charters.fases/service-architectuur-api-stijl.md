# Samenvatting Huidige Service Architectuur TrueLogicX

**Bron**: Confluence - TLX Architectuur Space  
**Pagina**: "Service architectuur & API-stijl"  
**Datum**: 23 november 2025

---

## Architectuur Aanpak

TrueLogicX hanteert een **hybride REST architectuur** met onderscheid tussen Resource APIs en Function APIs.

### Resource APIs (CRUD-oriented)

- **Naamgeving**: Noun-based (entiteiten)
- **HTTP Methods**: GET, POST, PATCH, PUT, DELETE
- **Idempotentie**: Altijd
- **Connectie**: Synchroon
- **Focus**: Objecten en hun state

### Function APIs (Action-oriented)

- **Naamgeving**: Werkwoord-based (acties)
- **HTTP Methods**: POST
- **Idempotentie**: Waar mogelijk (met compensatie/rollback mechanisme)
- **Connectie**: Synchroon én Asynchroon
- **Focus**: Commands, side-effects, processen

---

## TLX Service Types

| Type | Code | Resource API | Function API | Logica Type | Toelichting | REST Methods | Binnen Bounded Context |
|------|------|--------------|--------------|-------------|-------------|--------------|------------------------|
| **Entity** | E | +++ | + | Data | CRUD-acties op entiteiten | GET, PUT, DELETE | ✅ Ja |
| **Task** | T | ++ | ++ | Composition | Geen SQL! Orchestreren van Entity services. Cross-context toegestaan. Voorbeeld: "haal orders op → plan ritten → leg ritten vast" | POST, GET | ❌ Nee |
| **Orchestration Task** | O | + | +++ | Proces Composition | UI-specifieke orchestraties (codering komt overeen met UI). Vraag: zijn deze nodig of zit orchestration logic in de UI? | POST | ❌ Nee |
| **Rule** | R | +++ | - | Rules | Klant-specifieke configuratie en bedrijfsregels. Voorbeeld: aantal dagen voor herinnering bij niet-afgemelde container, boetebedragen. Gebaseerd op beleid en productbeschrijvingen. | GET | ✅ Ja |
| **Utility** | U | + | +++ | Divers | Technische services: logging, PDF generatie, authenticatie, etc. | GET, POST, PUT | ❌ Nee |

**Legend**: +++ = primair gebruik, ++ = regelmatig, + = beperkt, - = niet van toepassing

---

## Belangrijke Architectuur Principes

### 1. Combineren in Eén Service

**Resource APIs en Function APIs worden samengevoegd in dezelfde services.**

_Fysiek splitsen in aparte microservices doe je alleen als NFR's (schaalbaarheid, security, teamgrenzen) daar echt om vragen._

### 2. Waarom Onderscheid Resource vs Function?

1. **Duidelijker mental model voor clients**  
   - Resource APIs → objecten en hun state
   - Function APIs → acties en taken
   - Voorkomt complexe processen in geforceerde CRUD-vorm

2. **Beter gebruik van HTTP-semantiek**  
   - Resources: focus op idempotentie, caching en filtering
   - Functies: focus op command-semantiek, side-effects en (optioneel) asynchroniciteit

3. **Eenvoudiger versionering en evolutie**  
   - Resource-contracten veranderen relatief langzaam
   - Function APIs kunnen sneller evolueren (bijv. verschillende planningsalgoritmes per versie)

4. **Gerichte observability en governance**  
   Differentiatie per type in:
   - **Metrics**: latency vs. doorlooptijd
   - **Autorisatie**: fijne CRUD-rechten vs. expliciete actie-rechten
   - **Rate-limits**: veel reads op resources vs. beperkte, zware functies

### 3. Bounded Context Respect

- **Entity (E)** en **Rule (R)** services blijven binnen hun bounded context
- **Task (T)** en **Orchestration (O)** services mogen cross-context zijn
- **Utility (U)** services zijn context-agnostisch

### 4. Geen SQL in Task Services

- Task Services orchestreren Entity Services (geen directe database access)
- Entity Services voeren de daadwerkelijke CRUD uit

### 5. Views & Rapportages

#### Orchestration Task Services
Geen SQL → Views niet relevant

#### Entity Services
- Joins mogelijk over meerdere tabellen binnen dezelfde bounded context
- Views leiden tot extra afhankelijkheden → bij voorkeur vermijden

#### Rapportages (cross-domain)
- **Geen** Task Services voor rapportages
- **Wel** Views/Materialized Views (in overleg met DBA)
- Views houden SQL in code eenvoudig

---

## Basis: SOA Methodologie (Thomas Erl)

De service types zijn gebaseerd op [SOA-patronen van Arcitura](https://patterns.arcitura.com/soa-patterns):

|  | **Business** | **Non-business** |
|---|--------------|------------------|
| **Proces non-agnostisch** | [Task Services](https://patterns.arcitura.com/soa-patterns/basics/soamethodology/task_services) | - |
| **Proces agnostisch** | [Entity Services](https://patterns.arcitura.com/soa-patterns/basics/soamethodology/entity_services) | [Utility Services](https://patterns.arcitura.com/soa-patterns/basics/soamethodology/utility_services) |

**Referentie**: [Amazon SOA White Paper](https://softwarearchitecture.s3.amazonaws.com/ServiceDesignPrinciples.pdf)

---

## Visuele Representatie

```
                         API-stijl
         +----------------------------------+
         | Resource API    | Function API   |
+--------+----------------------------------+
| Entity | 1:1 CRUD op     | Acties op      |
| (E)    | entiteit        | entiteit,      |
|        |                 | side-effects & |
|        |                 | commands       |
+--------+-----------------+----------------+
| Task   | Zelden;         | Orkestraties,  |
| (T)    | soms read-model | processen en   |
|        |                 | business-taken |
+--------+-----------------+----------------+
```

---

## Conclusie

De huidige architectuur biedt:

✅ **Flexibiliteit**: Resource én Function APIs in één service  
✅ **DDD-alignment**: Bounded contexts via Entity en Rule services  
✅ **Duidelijke separatie**: Cross-context via Task services  
✅ **Observability**: Differentiatie per service type (E/T/O/R/U)  
✅ **Governance**: Type-specifieke autorisatie en rate limiting  

Deze basis vormt een solide fundament voor verdere uitbreiding met API Gateway en GraphQL federation.
