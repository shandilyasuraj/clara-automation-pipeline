# Changelog for Ben's Electric Solutions (v1 -> v2)

* **Resolved Unknown:** Updated `business_hours.timezone` from `null` to `EST`.
* **Resolved Unknown:** Established `call_transfer_rules.timeouts` at `30 seconds`.
* **Resolved Unknown:** Added "Dispatch Service (External)" as the fallback for `emergency_routing_rules`.
* **Update:** Changed `emergency_routing_rules.primary_contact` from "Ben's Cell" to "On-Call Technician Phone Tree".
* **Addition:** Added "Generator installation" to `services_supported`.
* **Addition:** Added "Medical equipment power failure" to `emergency_definition`.
* **Constraint Added:** Added rule to `integration_constraints`: "Do not log residential lighting quotes into ServiceTrade."