# Chery Q (QQ3 EV) OBD Data

Community-tested PIDs for the 2026 Chery Q EV (sold as QQ3 EV in China; Gotion LFP pack), reverse engineered in 2026 with a BLE ELM327 and a read-only UDS explorer. All reads are service 0x22 on protocol 6 (ISO 15765-4 CAN 11-bit 500k). Most fields come from the BMS (request 7E5, response 7ED); is_charging comes from the on-board charger / DC-DC unit (request 7E6, response 7EE).

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
| is_charging | 7E6 224314 | on-board charger flag: 1 while AC charging, 0 unplugged and plugged-not-charging (Car Scanner, default session). Not yet checked on DC; is_dcfc covers DC |

## Not found yet (help wanted)
- **ext_temp**: not in the BMS or the charger module. Probably on the climate panel or the body controller, whose addresses are unknown.
- **is_parked / gear**: no BMS or charger byte follows P/R/N/D. Likely on the VCU (7E0); its 22-DIDs were not swept yet.
- **soe, est_battery_range, capacity, tire_pressure, hvac_power**: not found.

## Notes for contributors
- Modules answering 11-bit physical requests: 7E0 VCU, 7E4 MCU, 7E5 BMS, 7E6 charger/DC-DC (OBC). 7E7 answers but returns NRC 0x11 to everything tried.
- Car Scanner's ECU list shows about 15 modules (IPB, EPS, e-shifter, FCM, BDM, climate panel, liftgate, radars, wireless charger, head unit, ...). Their request IDs were not found in 700-7FF or 29-bit 18DAxxF1, so they probably sit behind the gateway at other addresses.
- 220001 and 220002 are long (110/130-byte) BMS blocks; 220013 and 220014 (553-853 bytes) look like cell-voltage arrays (first word 0x0D02 = 3.33 V), but a BLE ELM327 garbles them. Use STmin 20 (ATFCSD300014) or a faster adapter.
- On the charger module, 224304 = AC input current (A), 224303 = DC output current ((Int16(A,B)-30000)/100, A; reads 0x0200 when idle), 22431C = HV voltage (/10). 224314 is the charging flag used here.
- The pack appears to be 128s LFP (220001 byte AW = 128), about 105 Ah, 42.7 kWh gross / 41.3 kWh usable.

## Licenses
This data is licensed by Apache 2.0
