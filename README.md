# Delete-all-Resources

Azure Cloud Shell PowerShell script for controlled cleanup of customer environments.  
Removes Azure resources within a defined scope, runs without confirmation prompts, generates inventories and logs, performs retries, and verifies complete removal.

---

## Toelichting cleanup script voor het verwijderen van Azure resources

Dit PowerShell script is bedoeld om alle Azure resources van een klant **gecontroleerd en volledig te verwijderen** binnen een vooraf gedefinieerde scope.

Het script is geschikt om te draaien in **Azure Cloud Shell (PowerShell)** met een account dat minimaal **Owner** rechten heeft op de betreffende subscription. Global Admin is niet vereist, maar kan wel worden gebruikt.

---

## Wat doet het script

Het script voert de volgende stappen automatisch uit:

- Selecteert de opgegeven Azure subscription  
- Zoekt alle Azure Resource Manager resources binnen de opgegeven scope  
  - Een specifieke resource group  
  - Of resources met een specifieke tag  
  - Subscription breed alleen als dit expliciet wordt toegestaan  
- Maakt altijd eerst een inventaris (CSV en JSON) van alle gevonden resources  
- Verwijdert alle gevonden resources **zonder bevestigingsvragen**  
- Voert meerdere verwijderrondes uit om afhankelijkheden correct af te handelen  
- Controleert na afloop of er nog resources over zijn en probeert deze alsnog te verwijderen  
- Logt alle acties en eventuele fouten naar bestanden in Cloud Shell  

---

## Belangrijke eigenschappen

- Werkt per **subscription**  
- Niet interactief, geen confirm prompts  
- Veilig door ingebouwde guardrails  
- Altijd logging en inventaris  
- Geschikt voor partner en MSP gebruik  

---

## Wat het script wel verwijdert

- Azure Resource Manager resources zoals:
  - Virtual Machines  
  - Storage Accounts  
  - Virtual Networks  
  - App Services  
  - Databases  
  - Key Vaults  
  - Log Analytics Workspaces  

---

## Wat het script niet verwijdert

- Entra ID objecten (users, groups, app registrations, service principals)  
- Role assignments of RBAC configuratie  
- Subscriptions  
- Management groups  
- Tenant-level instellingen  

---

## Belangrijke opmerking

Dit script voert **onomkeerbare acties** uit.  
Verwijderde Azure resources kunnen niet worden hersteld, tenzij er vooraf back-ups zijn gemaakt.

Gebruik dit script alleen wanneer:
- De klant expliciet akkoord is  
- De scope vooraf is gecontroleerd  
- De audit mode eerst is uitgevoerd  

---

## Disclaimer

Dit script wordt geleverd **as-is**.  
Gebruik is volledig op eigen risico.
