Script Parameters Guide

This document explains which parameters are required and optional to successfully run the Azure Resource Cleanup script in Azure Cloud Shell.

The script is designed to be safe by default and will not perform destructive actions unless the correct parameters are explicitly provided.

Required Parameter
-SubscriptionId (Required)
-SubscriptionId "<subscription-id>"


Specifies the Azure subscription in which the script will operate.

All Azure resources exist within a subscription

The script cannot run without this parameter

The script always works on one subscription at a time

If this parameter is missing, the script will stop immediately.

Mode Parameter
-Mode (Optional)
-Mode Audit   # Default
-Mode Delete


Defines how the script behaves.

Mode	Description
Audit	No resources are deleted. Inventory and logs are generated only.
Delete	Resources within the defined scope are deleted without confirmation prompts.

If not specified, the script runs in Audit mode.

Scope Parameters

(Required when using -Mode Delete)

To prevent accidental deletion, a scope must be explicitly defined when running in Delete mode.

Option 1: Resource Group Scope
-ResourceGroupName "rg-customer-prod"


Deletes all Azure resources within the specified resource group.

Option 2: Tag-Based Scope
-TagName "DeleteMe"
-TagValue "True"


Deletes all resources that contain the specified tag.

Notes:

-TagValue is optional

If only -TagName is provided, all resources with that tag are included regardless of value

Option 3: Subscription-Wide Scope (Explicit Approval Required)
-AllowSubscriptionWideDelete


Allows deletion of all Azure resources within the subscription.

⚠️ This option is blocked by default and must be explicitly enabled.

Optional Behavior Parameters
-MaxDeletePasses
-MaxDeletePasses 5


Number of deletion attempts performed by the script.

Useful for handling resource dependencies

Default value: 5

-SecondsBetweenPasses
-SecondsBetweenPasses 15


Time in seconds to wait between deletion passes.

Default value: 15 seconds

-PurgeSoftDeleted
-PurgeSoftDeleted


Attempts to permanently remove soft-deleted resources where supported, such as:

Azure Key Vaults

Azure Storage Accounts

-IncludeResourceGroupsInDelete
-IncludeResourceGroupsInDelete


Deletes the resource group itself after all contained resources have been removed.

Only applicable when -ResourceGroupName is specified.

Parameter Summary Table
Parameter	Required	Description
SubscriptionId	Yes	Target Azure subscription
Mode	No	Audit or Delete
ResourceGroupName	No	Scope limited to a resource group
TagName	No	Scope based on resource tag
TagValue	No	Optional tag value filter
AllowSubscriptionWideDelete	No	Enables subscription-wide deletion
MaxDeletePasses	No	Number of deletion retries
SecondsBetweenPasses	No	Delay between deletion passes
PurgeSoftDeleted	No	Purges soft-deleted resources
IncludeResourceGroupsInDelete	No	Deletes the resource group itself
Valid Usage Examples
Audit resources in a resource group
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "rg-test"

Delete resources in a resource group
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -ResourceGroupName "rg-test" `
  -Mode Delete

Delete resources by tag
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -TagName "DeleteMe" `
  -Mode Delete

Subscription-wide cleanup (explicit approval)
.\cleanup.ps1 `
  -SubscriptionId "<subscription-id>" `
  -Mode Delete `
  -AllowSubscriptionWideDelete

Important Notes

The script operates only at subscription level

It does not remove Entra ID (Azure AD) tenant objects

Destructive actions are irreversible

Always run in Audit mode first
