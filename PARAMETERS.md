## **Parameters**

Dit script vereist **één verplichte parameter** en meerdere **optionele parameters**.  
Zonder de juiste parameters zal het script **geen destructieve acties uitvoeren**.

De parameters bepalen:
- **Binnen welke Azure subscription** het script wordt uitgevoerd
- **Hoe het script zich gedraagt** (audit of verwijderen)
- **Welke scope** wordt gebruikt om resources te selecteren en te verwijderen

---

### **SubscriptionId** (verplicht)

```powershell
-SubscriptionId "<subscription-id>"
Geeft aan binnen welke Azure subscription het script wordt uitgevoerd.

Altijd verplicht

Het script werkt per subscription

Zonder deze parameter stopt het script direct

Deze parameter is noodzakelijk omdat alle Azure resources altijd aan een subscription zijn gekoppeld.

Mode
powershell
Code kopiëren
-Mode Audit
-Mode Delete
Bepaalt het gedrag van het script.

Audit

Standaard modus

Er worden geen resources verwijderd

Alleen een inventaris (CSV en JSON) en logging worden aangemaakt

Geschikt om vooraf te controleren wat er geraakt zou worden

Delete

Alle resources binnen de opgegeven scope worden verwijderd

Er worden geen bevestigingsvragen gesteld

Deze modus voert onomkeerbare acties uit

Scope parameters
Verplicht bij gebruik van Mode Delete.

Om onbedoelde verwijderingen te voorkomen, moet bij Mode Delete altijd een scope worden opgegeven.
Zonder scope zal het script weigeren om resources te verwijderen.

ResourceGroupName
powershell
Code kopiëren
-ResourceGroupName "rg-customer-prod"
Verwijdert alle Azure resources binnen de opgegeven resource group.

Beperkt het script tot één resource group

Veel gebruikt bij het opruimen of beëindigen van klantomgevingen

TagName en TagValue
powershell
Code kopiëren
-TagName "DeleteMe"
-TagValue "True"
Verwijdert alle resources met de opgegeven tag.

TagValue is optioneel

Als alleen TagName wordt opgegeven, worden alle resources met deze tag verwijderd

Handig om resources vooraf te markeren voor verwijdering

AllowSubscriptionWideDelete
powershell
Code kopiëren
-AllowSubscriptionWideDelete
Maakt het mogelijk om alle Azure resources binnen de gehele subscription te verwijderen.

Standaard geblokkeerd

Moet expliciet worden opgegeven

Alleen gebruiken wanneer dit bewust en goedgekeurd is

Deze parameter voorkomt dat per ongeluk een volledige subscription wordt opgeschoond.

Optionele parameters
De onderstaande parameters zijn niet verplicht, maar beïnvloeden het gedrag van het script.

MaxDeletePasses
powershell
Code kopiëren
-MaxDeletePasses 5
Bepaalt het aantal verwijderrondes dat het script uitvoert.

Handig bij afhankelijkheden tussen resources

Het script probeert meerdere keren om achterblijvende resources te verwijderen

Standaardwaarde: 5

SecondsBetweenPasses
powershell
Code kopiëren
-SecondsBetweenPasses 15
Bepaalt de wachttijd in seconden tussen verwijderrondes.

Geeft Azure tijd om afhankelijkheden correct af te handelen

Standaardwaarde: 15 seconden

PurgeSoftDeleted
powershell
Code kopiëren
-PurgeSoftDeleted
Probeert soft-deleted resources permanent te verwijderen waar dit wordt ondersteund, zoals:

Azure Key Vaults

Azure Storage Accounts

Deze parameter wordt alleen toegepast na het verwijderen van actieve resources.

IncludeResourceGroupsInDelete
powershell
Code kopiëren
-IncludeResourceGroupsInDelete
Verwijdert ook de resource group zelf nadat alle resources zijn verwijderd.

Alleen van toepassing als ResourceGroupName is opgegeven

Zonder deze parameter blijft de resource group bestaan

Overzicht parameters
Parameter	Verplicht	Omschrijving
SubscriptionId	Ja	Doel subscription
Mode	Nee	Audit of Delete
ResourceGroupName	Nee	Scope op resource group
TagName	Nee	Scope op tag
TagValue	Nee	Optionele tagwaarde
AllowSubscriptionWideDelete	Nee	Subscription-breed verwijderen
MaxDeletePasses	Nee	Aantal verwijderrondes
SecondsBetweenPasses	Nee	Wachtijd tussen rondes
PurgeSoftDeleted	Nee	Opruimen soft-deleted resources
IncludeResourceGroupsInDelete	Nee	Verwijdert resource group zelf

Belangrijke opmerkingen
Het script werkt alleen op Azure resources binnen een subscription

Entra ID (Azure AD) objecten worden niet verwijderd

Verwijderacties zijn onomkeerbaar

Het wordt sterk aanbevolen om altijd eerst Audit mode te gebruiken

yaml
Code kopiëren

---

Dit is nu:
- volledig Markdown
- vetgedrukte tekst blijft vetgedrukt
- uitleg is duidelijk en logisch opgebouwd
- klaar voor GitHub zonder aanpassingen

Als je wilt, kan ik als volgende stap:
- de **README hier perfect op laten aansluiten**
- of een **korte Quick Start** toevoegen bovenaan
