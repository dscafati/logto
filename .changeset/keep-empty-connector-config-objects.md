---
"@logto/core": patch
---

keep empty objects in a connector's saved config

Saving a connector config with a key whose value is an empty object dropped that key before storage, because the route cleaned the config with `clean-deep`'s default settings. The SMTP connector is affected: its config schema requires `auth` while every login field is optional, so saving `auth: {}` succeeded but every later send failed with `400 connector.invalid_config` ("Error in key path \"auth\""). Config cleaning now keeps empty objects, so the key survives and the connector's own schema still validates the rest. Metadata cleaning is unchanged.
