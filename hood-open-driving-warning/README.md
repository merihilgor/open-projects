Türkçe: [README.tr.md](README.tr.md)

# Hood-Open Driving Warning: Preventing Accidents Caused by an Unlatched Hood

## Summary

Vehicles already warn the driver when a door or the trunk is left open. The hood (bonnet) deserves the same treatment. This proposal makes a **hood-open warning** a standard safety feature: the vehicle tells the driver when the hood is not fully closed and locked, and the warning becomes more insistent once the vehicle starts moving. The aim is simple: a hood that was left open should never fly up while driving, block the driver's view, and cause an accident.

## Problem

- The hood is opened often: checking oil and coolant, topping up washer fluid, jump-starting, servicing, car washes. It is easy to close it only halfway, or to drop it without it fully locking.
- From the driver's seat, a hood that is not fully locked is **hard or impossible to notice**. Its edge sits low and looks normal while the car is parked.
- At driving speed, air pressure can lift an unlocked hood. If it flies up, it **suddenly blocks the entire windshield**. The driver loses visibility with no warning, which can lead to panic braking, loss of control and collisions, putting the occupants and other road users at risk.
- Most drivers are used to the door-open warning, but the hood is often left out of this protection, or the warning is missing on many vehicles.

## Proposed Solution

Make the hood part of the same "open closure" warning logic that already exists for doors:

1. **Detection:** the vehicle knows whether the hood is **fully closed and locked**. It also recognizes a half-closed state (resting on the safety catch but not fully locked) as "not closed".
2. **Warning when parked:** when the vehicle is started or the ignition is switched on with the hood not fully locked, a clear **dashboard warning (icon plus message)** appears, exactly like the door-open warning.
3. **Escalating warning while moving:** if the vehicle starts moving with the hood not fully locked, the warning **escalates**: an audible alert that continues and a more prominent message telling the driver to stop safely and close the hood.
4. **No sudden intervention:** the system warns but never brakes or stops the vehicle by itself, so it does not create a new hazard in traffic.

## How It Works

| Situation | What the driver experiences |
|---|---|
| Hood fully closed and locked | Nothing. Normal driving. |
| Ignition on, hood not fully locked | Dashboard icon and message: "Hood open" |
| Vehicle starts moving, hood not fully locked | Continuous audible alert plus a prominent message: "Hood open, stop safely and close the hood" |
| The detection itself fails | A separate fault indicator, so a broken warning is never mistaken for a closed hood |

The experience deliberately mirrors the door-open warning that drivers already understand, so no new habits or training are needed.

## Implementation & Phasing

1. **Voluntary adoption:** manufacturers add the hood-open warning to new models as a safety feature, the same way they present the door-open warning.
2. **Standard for new models:** the warning becomes a requirement in vehicle type approval for new models.
3. **All new vehicles:** the requirement extends to all newly registered vehicles.
4. **Existing vehicles and inspection:** a simple retrofit option for existing vehicles is encouraged, and periodic vehicle inspection checks that the warning works on vehicles that have it.

## Stakeholders & Benefits

- **Drivers and passengers:** protection against a sudden, total loss of forward visibility.
- **Other road users:** fewer accidents caused by a vehicle that suddenly brakes or swerves because its hood flew up.
- **Service stations, car washes and roadside assistance:** less liability risk after work that involves opening the hood.
- **Manufacturers:** a low-cost, easy-to-explain safety feature that extends a warning drivers already know.
- **Insurers:** fewer claims from a preventable accident type.
- **Regulators:** a simple, measurable addition to vehicle safety requirements.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| False "hood open" alarms annoy drivers | Reliable detection, with a separate fault indicator for detection problems |
| Drivers ignore the warning | The warning escalates when the vehicle moves and stays on until the hood is closed |
| Automatic intervention could itself cause danger | The system only warns; it never brakes or stops the vehicle |
| Cost for manufacturers and owners | Phased introduction, starting with new models |

## Origin

Originally proposed by Merih İlgör (September 2026); published here as an open project idea.

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md).
