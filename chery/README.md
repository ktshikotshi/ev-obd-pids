# Chery Q (QQ3 EV) OBD Data

Community-tested PIDs for the 2026 Chery Q EV (sold as QQ3 EV in China), reverse engineered in 2026 with a BLE ELM327 and a read-only UDS explorer. All values come from the BMS (request 7E5, response 7ED), service 0x22, protocol 6 (ISO 15765-4 CAN 11-bit 500k).

Starting point was the Omoda E5 BMS DID list (github.com/sl3per/OmodaE5Mod); several of those DIDs carry over, the SoC block (22441E) does not.

| Key | DID | Status |
| --- | --- | --- |
| soc | 221038 | matched dash (parked, driving, AC, DC) |
| soh | 221048 | reads 100 on a new car |
| voltage | 224404 | cross-checked with 220001 and charger display |
| current | 224406 | + discharge / − charge; matches 220001 and AC/DC charge power |
| power | 220001 | V (bytes A,B ×0.02) × I (bytes AA,AB, offset 30000) |
| batt_temp | 22440E | average module temperature |
| odometer | 224402 | matched dash |
| speed | 224401 | matched dash |
| is_dcfc | 220001 | DC inlet voltage (bytes E,F ×0.02) > 100 V; matched charger display |
| is_charging | 220001 | charge current > 1 A. Interim: may read 1 during strong regen |

Not found yet: ext_temp, is_parked.

## Licenses
This data is licensed by Apache 2.0
