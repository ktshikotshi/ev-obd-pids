## Summary
This PR adds a community candidate OBD profile for the Deepal S05.

## What is included
Validated / useful daily telemetry candidates:
- SOC direct via `22F22F` on header `7A1`
- Pack voltage via `22F228`
- Pack current via `22F229`
- Derived pack power
- SOH candidate via `22F27D`
- Cell max/min voltage via `22F250` / `22F251`
- Battery max/min temperature via `22F252` / `22F253`
- Charge temperature candidate via `22F255`

## What is NOT validated
The following are intentionally marked experimental and should not be treated as lifetime totals:
- `22F264` available energy / Et sum interpretations
- charged-energy lifetime total
- odometer
- lifetime EFC

## Notes
This profile is intended as a starter for community testing and review.
