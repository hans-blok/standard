---
description: Maak of update de feature specificatie vanuit een natuurlijke taalbeschrijving van de feature.
handoffs: 
  - label: Bouw Technisch Plan
    agent: speckit.plan
    prompt: Maak een plan voor de spec. Ik bouw met...
  - label: Verduidelijk Spec Vereisten
    agent: speckit.clarify
    prompt: Verduidelijk specificatie-eisen
    send: true
---

## Gebruikersinvoer
2. **Conceptueel model** - Ontwerp entiteiten en relaties
```text
$ARGUMENTS
```

Je **MOET** de gebruikersinvoer overwegen voordat je verdergaat (indien niet leeg).

## Overzicht

De tekst die de gebruiker typte na `/speckit.specify` in het activerende bericht **is** de feature-beschrijving. Ga ervan uit dat je deze altijd beschikbaar hebt in dit gesprek, zelfs als `$ARGUMENTS` letterlijk hieronder verschijnt. Vraag de gebruiker niet om deze te herhalen, tenzij ze een leeg commando gaven.

Doe het volgende met die feature-beschrijving:

1. **Genereer een beknopte korte naam** (2-4 woorden) voor de branch:
   - Analyseer de feature-beschrijving en haal de meest betekenisvolle kernwoorden eruit
   - Maak een 2-4 woorden korte naam die de essentie van de feature vastlegt
   - Gebruik actie-zelfstandig naamwoord formaat waar mogelijk (bijv. "add-user-auth", "fix-payment-bug")
   - Behoud technische termen en acroniemen (OAuth2, API, JWT, etc.)
   - Houd het beknopt maar beschrijvend genoeg om de feature in één oogopslag te begrijpen
   - Voorbeelden:
     - "Ik wil gebruikersauthenticatie toevoegen" → "user-auth"
     - "Implementeer OAuth2 integratie voor de API" → "oauth2-api-integration"
     - "Maak een dashboard voor analytics" → "analytics-dashboard"
     - "Repareer payment processing timeout bug" → "fix-payment-timeout"

2. **Controleer op bestaande branches voordat je een nieuwe maakt**:
   
   a. Haal eerst alle remote branches op om te zorgen dat we de laatste informatie hebben:
      ```bash
      git fetch --all --prune
      ```
   
   b. Vind het hoogste feature-nummer over alle bronnen voor de korte-naam:
      - Remote branches: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - Lokale branches: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - Specs directories: Controleer op directories die matchen met `specs/[0-9]+-<short-name>`
   
   c. Bepaal het volgende beschikbare nummer:
      - Extraheer alle nummers uit alle drie de bronnen
      - Vind het hoogste nummer N
      - Gebruik N+1 voor het nieuwe branch-nummer
   
   d. Voer het script uit `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS"` met het berekende nummer en korte-naam:
      - Geef `--number N+1` en `--short-name "jouw-korte-naam"` mee samen met de feature-beschrijving
      - Bash voorbeeld: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" --json --number 5 --short-name "user-auth" "Add user authentication"`
      - PowerShell voorbeeld: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" -Json -Number 5 -ShortName "user-auth" "Add user authentication"`
   
   **BELANGRIJK**:
   - Controleer alle drie de bronnen (remote branches, lokale branches, specs directories) om het hoogste nummer te vinden
   - Match alleen branches/directories met het exacte korte-naam patroon
   - Als geen bestaande branches/directories gevonden met deze korte-naam, start met nummer 1
   - Je mag dit script slechts één keer per feature uitvoeren
   - De JSON wordt in de terminal als output gegeven - verwijs daar altijd naar om de werkelijke content te krijgen die je zoekt
   - De JSON output bevat BRANCH_NAME en SPEC_FILE paden
   - Voor enkele quotes in args zoals "I'm Groot", gebruik escape syntax: bijv 'I'\''m Groot' (of dubbele quote indien mogelijk: "I'm Groot")

3. Laad `.specify/templates/spec-template.md` om de vereiste secties te begrijpen.

4. Volg deze uitvoeringsstroom:

    1. Parse gebruikersbeschrijving uit Input
       Indien leeg: ERROR "Geen feature-beschrijving gegeven"
    2. Extraheer kernconcepten uit beschrijving
       Identificeer: actoren, acties, data, beperkingen
    3. Voor onduidelijke aspecten:
       - Maak geïnformeerde gissingen gebaseerd op context en industrie-standaarden
       - Markeer alleen met [VERDUIDELIJKING NODIG: specifieke vraag] als:
         - De keuze de feature-scope of gebruikerservaring significant beïnvloedt
         - Meerdere redelijke interpretaties bestaan met verschillende implicaties
         - Geen redelijke standaard bestaat
       - **LIMIET: Maximum 3 [VERDUIDELIJKING NODIG] markeringen totaal**
       - Prioriteer verduidelijkingen op impact: scope > security/privacy > gebruikerservaring > technische details
    4. Vul User Scenarios & Testing sectie in
       Indien geen duidelijke gebruikersstroom: ERROR "Kan gebruikersscenario's niet bepalen"
    5. Genereer Functionele Eisen
       Elke eis moet testbaar zijn
       Gebruik redelijke standaarden voor niet-gespecificeerde details (documenteer aannames in Aannames sectie)
    6. Definieer Succescriteria
       Creëer meetbare, technologie-agnostische uitkomsten
       Include zowel kwantitatieve metrics (tijd, performance, volume) als kwalitatieve maten (gebruikerstevredenheid, taak-voltooiing)
       Elk criterium moet verifieerbaar zijn zonder implementatie-details
    7. Identificeer Kernentiteiten (indien data betrokken)
    8. Return: SUCCESS (spec klaar voor planning)

5. Schrijf de specificatie naar SPEC_FILE met behulp van de template-structuur, vervang placeholders met concrete details afgeleid van de feature-beschrijving (argumenten) terwijl je de sectie-volgorde en headers behoudt.

6. **Specificatie Kwaliteitsvalidatie**: Na het schrijven van de initiële spec, **hanteer de kwaliteitseisen** zoals beschreven in `governance/kwaliteitseisen-specificaties.md`:

   - Genereer de kwaliteit checklist
   - Valideer de spec tegen alle checklist-items
   - Behandel [VERDUIDELIJKING NODIG] markeringen (max 3)
   - Presenteer verduidelijkingsvragen aan gebruiker indien nodig
   - Update de spec op basis van gebruikersresponsen
   - Documenteer validatie resultaten

   Zie `governance/kwaliteitseisen-specificaties.md` voor volledige details over checklist-structuur, validatie proces, verduidelijking formaat, en redelijke standaarden.

7. Rapporteer voltooiing met branch-naam, spec bestandspad, checklist resultaten, en gereedheid voor de volgende fase (`/speckit.clarify` of `/speckit.plan`).

**OPMERKING:** Het script maakt de nieuwe branch aan, checkt deze uit en initialiseert het spec-bestand voordat het schrijft.

## Algemene Richtlijnen

Volg de richtlijnen uit `governance/kwaliteitseisen-specificaties.md`:
- Focus op **WAT** en **WAAROM**, niet HOE
- Geschreven voor zakelijke stakeholders
- Maak geïnformeerde gissingen met redelijke standaarden
- Beperk verduidelijkingen tot maximum 3
- Documenteer aannames
- Zorg voor testbare, ondubbelzinnige eisen
- Succescriteria moeten meetbaar en technologie-agnostisch zijn
