# Delete-all-Resources
Azure Cloud Shell PowerShell script for controlled cleanup of customer environments. Removes Azure resources within a defined scope, runs without confirmation prompts, generates inventories and logs, performs retries, and verifies complete removal.

Toelichting cleanup script voor het verwijderen van Azure resources

Dit PowerShell script is bedoeld om alle Azure resources van een klant gecontroleerd en volledig te verwijderen binnen een vooraf gedefinieerde scope. Het script is geschikt om te draaien in Azure Cloud Shell (PowerShell) met een account dat minimaal Owner rechten heeft op de betreffende subscription (Global Admin is niet vereist, maar kan wel).

Wat doet het script

Het script voert de volgende stappen automatisch uit:

Selecteert de opgegeven Azure subscription

Zoekt alle Azure Resource Manager resources binnen de opgegeven scope

Een specifieke resource group

Of resources met een specifieke tag

Subscription breed alleen als dit expliciet wordt toegestaan

Maakt altijd eerst een inventaris (CSV en JSON) van alle gevonden resources

Verwijdert alle gevonden resources zonder bevestigingsvragen

Voert meerdere verwijderrondes uit om afhankelijkheden correct af te handelen

Controleert na afloop of er nog resources over zijn en probeert deze alsnog te verwijderen

Logt alle acties en eventuele fouten naar bestanden in Cloud Shell

Het resultaat is een volledig opgeschoonde Azure omgeving binnen de gekozen scope, inclusief een audit trail.

Wat het script nadrukkelijk wel en niet doet

Wel

Verwijdert Azure resources zoals VM’s, storage accounts, VNets, databases, App Services, Key Vaults enzovoort

Werkt zonder interactieve bevestigingen

Herhaalt verwijderacties totdat alles weg is of de maximale pogingen zijn bereikt

Optioneel: opruimen van soft deleted Key Vaults en Storage Accounts

Niet

Verwijdert Entra ID objecten (users, groups, app registrations, service principals)

Verwijdert geen role assignments of RBAC instellingen

Verwijdert geen subscriptions of management groups

Draait niet automatisch tenant breed zonder expliciete toestemming

Veiligheidsmechanismen

Om fouten te voorkomen bevat het script ingebouwde beveiligingen:

Verwijderen is alleen mogelijk als:

Een resource group is opgegeven

Of een tag is opgegeven

Of expliciet wordt bevestigd dat subscription breed verwijderen is toegestaan

Standaard draait het script in Audit mode

Elke run genereert een inventaris en logbestanden

Zonder expliciete parameters kan het script geen destructieve acties uitvoeren

Aanbevolen werkwijze

Stap 1 – Audit (verplicht aanbevolen)
Draai het script eerst in audit modus om te controleren welke resources geraakt worden.

.\cleanup.ps1 -SubscriptionId "<subscription-id>" -ResourceGroupName "<rg-naam>" -Mode Audit


Controleer het gegenereerde CSV bestand.

Stap 2 – Verwijderen
Als de scope correct is, draai het script opnieuw in delete modus.

.\cleanup.ps1 -SubscriptionId "<subscription-id>" -ResourceGroupName "<rg-naam>" -Mode Delete

Gebruik voor volledige klant-opschoning

Voor scenario’s waarbij een klant volledig stopt en alle resources moeten worden verwijderd, kan het script veilig worden gebruikt zolang:

De scope expliciet is opgegeven

De audit stap is uitgevoerd

De partner Owner rechten heeft op de subscription

Belangrijke opmerking

Dit script voert onomkeerbare acties uit. Verwijderde resources kunnen niet worden hersteld, tenzij er vooraf back-ups zijn gemaakt. Gebruik dit script alleen wanneer de klant expliciet akkoord is met het verwijderen van alle resources binnen de gekozen scope.

Als je wilt, kan ik deze toelichting ook:

Korter maken voor een e-mail

Formuleren als README.md bij het script

In het Engels herschrijven voor internationale partners

Zeg maar wat je nodig hebt.

Je zei:
Kun je hem in het engels schrijven en een README ik wil hem namelijk op Github zetten.
ChatGPT zei:

Zeker. Hieronder staat een GitHub-ready README.md in het Engels, geschreven voor partners en klanten.
De tekst is duidelijk, professioneel en geschikt voor public use.

Je kunt dit bestand 1 op 1 kopiëren naar README.md.

Azure Resource Cleanup Script

This PowerShell script is designed to safely and completely remove Azure resources within a clearly defined scope.
It is intended to be executed in Azure Cloud Shell (PowerShell) by partners who need to decommission or clean up customer environments in a controlled way.

The script includes built-in guardrails to prevent accidental tenant-wide deletion while still allowing fully automated, non-interactive cleanup when explicitly approved.

Purpose

This script helps partners:

Decommission customer environments

Clean up proof-of-concept or test subscriptions

Remove all resources from a specific resource group

Remove resources based on tags

Ensure no resources remain due to dependency issues

It performs multiple deletion passes and a final verification to make sure the environment is truly empty within the selected scope.

Key Features

Runs in Azure Cloud Shell using Az PowerShell

Non-interactive removal (no confirmation prompts)

Mandatory scope definition (resource group, tag, or explicit subscription-wide approval)

Automatic inventory export (CSV and JSON)

Detailed logging for auditing purposes

Multiple delete passes to handle dependencies

Final verification after cleanup

Optional purge of soft-deleted resources (Key Vaults, Storage Accounts)

What the Script Removes

Azure Resource Manager resources such as:

Virtual Machines

Storage Accounts

Virtual Networks

App Services

Databases

Key Vaults

Log Analytics Workspaces

And other ARM-based resources

What the Script Does NOT Remove

Entra ID objects (users, groups, app registrations, service principals)

Role assignments or RBAC configuration

Subscriptions

Management groups

Billing or tenant-level configuration

Permissions Required

Owner role on the target subscription

Global Administrator is not required, but can be used

Safety Guards

To prevent accidental data loss, the script enforces the following rules:

Destructive actions require an explicit scope:

Resource Group

Tag

Or explicit approval for subscription-wide deletion

Default mode is Audit

Inventory and logs are always generated

Subscription-wide deletion requires a dedicated parameter

The script cannot run destructive operations by accident.

Recommended Usage Flow
Step 1 – Audit Mode (recommended)

Always run the script in audit mode first to review the scope.

.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "<resource-group-name>" `
  -Mode Audit


Review the generated CSV inventory file.

Step 2 – Delete Mode

After validating the inventory, run the script in delete mode.

.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "<resource-group-name>" `
  -Mode Delete


The script will remove all resources without confirmation prompts and verify completion.

Usage Examples
Delete all resources in a resource group
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "rg-customer-prod" `
  -Mode Delete

Delete resources based on a tag
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -TagName "DeleteMe" `
  -TagValue "True" `
  -Mode Delete

Purge soft-deleted resources (optional)
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "rg-customer-prod" `
  -Mode Delete `
  -PurgeSoftDeleted

Subscription-wide cleanup (explicit approval required)
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -Mode Delete `
  -AllowSubscriptionWideDelete


⚠️ Use subscription-wide cleanup only when explicitly approved by the customer.

Output Files

Each run generates the following files in the Cloud Shell home directory:

Inventory (CSV)

Inventory (JSON)

Execution log

Failure reports (if applicable)

These files provide a full audit trail of the cleanup process.

Important Notice

This script performs irreversible actions.
Deleted Azure resources cannot be recovered unless backups exist.

Always ensure:

Customer approval is documented

Backups have been verified

Audit mode has been executed before delete mode

Disclaimer

This script is provided as-is.
Use at your own risk and only in environments where full deletion has been explicitly approved.
