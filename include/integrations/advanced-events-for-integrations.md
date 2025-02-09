* [Hits](../../../glossary-en.md#hit) detected except for:

    * Experimental hits detected based on the [custom regular expression](../../rules/regex-rule.md). Non-experimental hits trigger notifications.
    * Hits not saved in the [sample](../../events/analyze-attack.md#sampling-of-hits).
* System related:
    * [User](../../../user-guides/settings/users.md) changes (newly created, deleted, role change)
    * [Integration](integrations-intro.md) changes (disabled, deleted)
    * [Application](../../../user-guides/settings/applications.md) changes (newly created, deleted, name change)
* [Vulnerabilities](../../../glossary-en.md#vulnerability) detected, all by default or only for the selected risk level(s):
    * High risk
    * Medium risk
    * Low risk
* [Rules](../../../user-guides/rules/intro.md) and [triggers](../../../user-guides/triggers/triggers.md) changed (creating, updating, or deleting the rule or trigger)
* [Scope](../../scanner/check-scope.md) changed: updates in hosts, services, and domains
