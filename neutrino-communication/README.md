Türkçe: [README.tr.md](README.tr.md)

# Neutrino Communication: Sending Signals Straight Through the Earth

## Summary

Every telecommunication method in use today relies on light or radio. Both are stopped by rock, deep water, ice and the planet itself, so signals must travel around obstacles: up to satellites, along cables on the seabed, or through relay stations. **Neutrinos** are elementary particles that barely interact with matter. They pass straight through mountains, oceans and the entire Earth almost unaffected. This project proposes developing **neutrino transmitters and receivers as a new telecommunication method**: a way to send messages where no other signal can reach, and to link two points on the planet along the shortest possible path, straight through it.

## Problem

- **Radio and light cannot penetrate deep water.** Submarines at depth can receive only a few characters per minute, or have to rise toward the surface to communicate, which compromises their safety and mission.
- **Underground spaces are cut off.** Mines, tunnels, deep shelters and collapsed buildings lose contact with the surface in exactly the situations where communication matters most, such as accidents and rescues.
- **Global links take the long way round.** A message between opposite sides of the world travels around the curve of the Earth through cables, routers and satellites. The straight line through the planet is much shorter but unusable with light or radio.
- **Signals can be blocked or jammed.** Radio links can be jammed, and cables and satellites can be cut or disabled.
- **Some places have no line of sight at all:** polar and ice stations, and in the long term spacecraft hidden behind a planet.

## Proposed Solution

Use **neutrino beams as a carrier of information**:

1. **Transmitter:** a source produces a stream of neutrinos that can be switched on and off, or varied, in a controlled pattern. That pattern carries the message.
2. **Straight path:** the beam is aimed at the receiver and travels in a straight line through whatever lies in between (rock, water, ice or the whole planet) without needing cables, relays or line of sight.
3. **Receiver:** a detector at the destination registers the rare neutrinos that interact with it and reconstructs the pattern, and therefore the message.
4. **New uses first, not replacement:** neutrino links are not meant to replace fiber or radio. They target the places and needs those methods cannot serve.

## How It Works

![Earth cross-section: a neutrino transmitter sends a straight beam through the planet to a receiver on the other side, a shorter path than the curved route via satellite or cable; a submarine at depth and an underground mine also receive the signal](assets/neutrino-link.svg)

| Situation | Today | With neutrino communication |
|---|---|---|
| Submarine at depth | Very slow, or must approach the surface | Messages reach it at depth, through the water |
| Mine, tunnel or rescue site | Cut off when cables and radio fail | A signal can reach it through the rock |
| Link between distant points on Earth | Long, curved path via cables and satellites | Shortest straight path through the planet |
| Jamming or cut cables | Link can be disrupted | Practically impossible to block or jam |

The principle has already been demonstrated in research. In 2012 a team at Fermilab in the United States sent the word "neutrino" with a pulsed neutrino beam over 1.035 km, including 240 m of solid rock, to a 170-ton detector, at 0.1 bit per second with a 1% bit error rate. The challenge now is to turn that proof of principle into a practical communication method.

## Technical Design (research level)

This section goes one level deeper: how a practical receiver and a matching transmitter could be built, based on published research. All figures come from the sources listed under References.

![The neutrino link as a chain: data bits gate a pulsed accelerator source; protons hit a target and the resulting particles are focused into a neutrino beam; the beam crosses rock, water or the Earth; a detector, listening only during the pulse windows, counts neutrino interactions and decodes the data](assets/neutrino-tx-rx-chain.svg)

### Receiver design

**The basic rule.** The number of neutrinos a receiver catches is set by three factors multiplied together:

> detected events = neutrino flux at the receiver × interaction cross-section × number of target particles in the detector

A compact receiver has to increase at least one of the three:

| Lever | How it helps |
|---|---|
| Dense, heavy-nucleus target | More target particles per unit volume |
| Higher neutrino energy | The interaction probability rises with energy, and high-energy beams are also more tightly focused |
| Coherent scattering on heavy nuclei | At low energies a neutrino can scatter off a whole nucleus at once; the probability grows with the square of the number of neutrons, so heavy nuclei give a large boost. This is what makes kilogram-scale detectors possible |
| Short distance to the source | Flux falls with the square of the distance from a non-directional source |
| Time-gating to the transmitter | The receiver only counts events inside the known pulse windows of the transmitter, which rejects almost all natural background. For communication this is the most powerful filter |
| Dual signals and shielding | Reading two kinds of light signal (scintillation and Cherenkov) at once and shielding against natural radiation separate real neutrino events from noise |

**The honest limit.** The weak force is weak by nature. In practice a detector of about a cubic meter or more is the realistic floor for most links, and very small detectors only work very close to an intense, pulsed source.

| Receiver tier | Typical size | Pairs with | Best use |
|---|---|---|---|
| Coherent-scattering crystal | A few kilograms (14.6 kg in the first observation) | Intense pulsed source tens of meters away | Near-field demonstrator, links through walls, rock or ground |
| Liquid detector | About a cubic meter and up | Pulsed or modulated source at short range | Short-range links, mines and tunnels |
| Instrumented water or ice, or a Cherenkov array on a submarine hull | Uses the surrounding water as the detector | High-energy, collimated beam | Long range, submarines at depth |
| Large tracking detector | 100-ton class (170 tons in the 2012 demonstration) | Accelerator neutrino beam | Fixed research links between facilities |

### Transmitter options (research)

| Source | Direction | Can it be modulated quickly? | Size | Best use | Status |
|---|---|---|---|---|---|
| Accelerator beam (proton beam on a target, magnetic focusing, decay tunnel) | Directional beam | Yes: the beam is pulsed and each pulse can carry a symbol | Large facility, hundreds of meters to kilometers | Long-range, through-Earth links | Used in the 2012 demonstration |
| Muon storage ring | Very tightly collimated, very intense | Yes | Very large | Highest data rates, submarine links | Proposed; estimated 1 to 100 bit/s to a submarine |
| Pulsed spallation / decay-at-rest source | Non-directional | Yes: sharp, short pulses | Large accelerator, but no decay tunnel | Short-range links with compact coherent-scattering receivers | Operating sources exist for research |
| Compact cyclotron with an isotope target | Non-directional | Slowly: the isotope decays over about a second | Compact accelerator | Detector research and calibration | Designed for physics research |
| Nuclear reactors, radioactive sources | Non-directional | No | Large or fixed | Not suitable as transmitters | — |

**Key principle.** The higher the energy, the narrower the beam (its opening angle shrinks roughly as 1/γ) and the more likely neutrinos are to interact. The signal at a distant receiver therefore grows strongly with energy, so **long-range links need high-energy, directional beams**, while **short-range links can use pulsed, non-directional low-energy sources with compact receivers**.

### Practical transmitter design proposal

A staged path from demonstration to useful links:

1. **Step 1: near-field demonstrator.** A compact pulsed proton accelerator source in which the data bits switch the beam pulses on and off or shift their timing. A kilogram-scale coherent-scattering receiver sits tens of meters away, behind rock or concrete. Goal: prove modulation, time synchronization and error-correcting codes with a small receiver.
2. **Step 2: directional long-range transmitter.** A high-energy proton beam hits a target, the resulting particles are focused by magnetic horns and decay in a tunnel into a neutrino beam. The beam is aimed along the straight line through the Earth to a large water/ice receiver or a hull-mounted array. Each beam pulse carries one symbol.
3. **Step 3: maximum performance.** A muon storage ring for the most intense and best-collimated beam, aimed at the highest data rates and at submarines.

**Encoding the data.**
- **Modulation:** on/off keying (pulse or no pulse) or pulse-position modulation (the timing of the pulse carries the bits).
- **Reliability:** forward error correction and repetition, so the message survives the small number of detected events.
- **Synchronization:** the transmitter and receiver share precise timing, so the receiver only "listens" during pulse windows.
- **Data rate:** grows with the number of neutrinos detected per second, so stronger beams and bigger receivers mean faster links.

| Known figures | Value |
|---|---|
| Demonstrated (2012, Fermilab) | 0.1 bit/s with 1% bit error rate over 1.035 km, including 240 m of rock, 170-ton detector |
| Theoretical (muon storage ring to a submarine) | 1 to 100 bit/s |
| Smallest detector to observe coherent scattering (2017) | 14.6 kg crystal |

## Implementation & Phasing

1. **Research demonstrations:** repeat and extend the existing proof of principle over longer distances and through more challenging paths, and improve reliability and data rate.
2. **Critical low-rate signaling:** very short, high-value messages where nothing else works, for example alerts to submarines at depth, emergency signals to underground sites, and precise time and clock synchronization.
3. **Fixed long-distance links:** permanent through-the-Earth links between large facilities, offering a shorter path and a channel that cannot be jammed.
4. **Smaller, cheaper receivers:** as research makes receivers smaller and more sensitive, expand to more sites and more applications, including polar stations and, in the long term, deep space.

## Stakeholders & Benefits

- **Maritime and defense organizations:** contact with submarines at depth without exposing them.
- **Mining, tunneling and civil protection:** a lifeline to underground workers and trapped people during accidents and disasters.
- **Telecom and finance:** a shorter physical path between distant points, valuable where every millisecond counts.
- **Critical infrastructure:** a backup channel that cannot be jammed or cut when cables or satellites fail.
- **Science and space agencies:** links to polar stations today and, in the long term, to spacecraft behind planets.
- **Research institutions and industry:** a new field of telecommunication with long-term scientific and economic value.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Neutrinos interact so rarely that today's transmitters need very large facilities and receivers are very large | Start with fixed, high-value links between large facilities; invest in research toward smaller, more sensitive receivers |
| Very low data rates | Focus first on short, critical messages and time synchronization, where even a few bits matter |
| Slow modulation and natural background noise | Use pulsed sources and time-gating so the receiver only counts events in the transmitter's pulse windows; add error-correcting codes |
| High cost | Share facilities between research and communication; prioritize uses where no alternative exists |
| A beam passes through everything in its path and cannot be shielded | Encrypt the content; in practice, receiving the signal requires a very large dedicated detector precisely in the beam's path, which makes casual interception extremely difficult |
| The idea is not new in principle | This project focuses on turning it into a practical telecom method with clear use cases and a roadmap, rather than claiming a first invention |

## References

- A. W. Sáenz et al., "Telecommunication with Neutrino Beams", *Science* 198, 295 (1977): an early proposal for communication with neutrino beams, including submarines.
- D. D. Stancil et al., "Demonstration of Communication using Neutrinos" (2012), [arXiv:1203.2847](https://arxiv.org/abs/1203.2847): the word "neutrino" sent with the NuMI beam at Fermilab to the MINERvA detector; 0.1 bit/s, 1% bit error rate, 1.035 km including 240 m of rock.
- P. Huber, "Submarine neutrino communication", *Physics Letters B* (2010), [arXiv:0909.4554](https://arxiv.org/abs/0909.4554): a muon-storage-ring beam with detectors on a submarine hull; estimated 1 to 100 bit/s.
- COHERENT Collaboration (2017): first observation of coherent elastic neutrino–nucleus scattering with a 14.6 kg crystal detector at a pulsed spallation source.
- IsoDAR: a compact-cyclotron isotope source of antineutrinos designed for physics research.
- "Feasibility of Neutrino Communication: A Modern Physics Reassessment", APS meeting (2026): compares accelerator and muon-decay sources and estimates energy per bit.
- Earlier patents on neutrino communication exist (for example US 4,205,268 and US 10,050,721). This project does not claim to be the first invention; it focuses on practical use cases, a staged roadmap and an adoption path.

**Keywords:** neutrino communication, telecommunications, through-Earth communication, submarine communication, underground communication, mine rescue, anti-jamming, low-latency links, particle physics, neutrino detector, neutrino beam, pulse-position modulation, frontier technology

## Origin

Originally proposed by Merih İlgör (October 2026); published here as an open project idea.

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md). Modified versions may not be sold or transferred to third parties as new ideas, and patent-like rights are reserved; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md#derivatives-improvements-and-idea-protection).
