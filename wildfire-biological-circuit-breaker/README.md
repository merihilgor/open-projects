Türkçe: [README.tr.md](README.tr.md)

# Cellular Biological Circuit Breaker Model for Wildfire Spread Prevention

*Stopping the domino effect: a biological circuit breaker model inspired by software architecture.*

| | |
|---|---|
| **Subject** | Fire-spread barriers and biological belting |
| **Focus region** | Aegean and Mediterranean forest ecosystems |
| **Source of inspiration** | Software architecture: the Circuit Breaker pattern |
| **Delivery capacity** | Pilot site and national-service workforce integration |

## Summary

The most destructive property of wildfires is that flames advance in a chain along continuous vegetation, producing an uncontrolled **domino effect (cascading failure)**. Traditional containment ideas, such as digging trenches or building artificial water channels, are impractical because of their high financial cost, their need for heavy machinery and the damage they do to natural topography.

This project applies the **Circuit Breaker** principle, which software architecture uses to prevent system lock-ups and cascading crashes, to the forest. Instead of costly trenches, forest land is divided into **separate cellular clusters (a honeycomb structure) bounded by fire-resistant native biological green belts**. If a fire starts in one cell, it is isolated there and cannot jump to neighbouring cells.

The model is to be validated on a 500-hectare pilot site in a high-risk zone and scaled up afterwards. The planting and maintenance workforce for scale-up comes from young people doing national service, under a cooperation protocol between the environment/forestry and defence authorities.

## Problem

- In recent years, severe wildfires across the Mediterranean and Aegean basins (and Southern Europe more widely) have exposed a critical weakness: flames driven by wind across continuous fuel beds spread quickly and uncontrollably, creating a domino effect.
- Conventional prevention and containment methods, such as excavating deep physical trenches or building artificial water channels and ponds:
  - carry very high financial costs (excavation, fuel, heavy machinery);
  - require heavy equipment;
  - damage soil structure and the natural landscape and raise erosion risk;
  - cannot stop fire from jumping over them through wind-borne embers and flying cones.
- For these reasons they do not offer a sustainable solution.

## Proposed Solution

The core idea is a direct analogy with software engineering. In software systems, "circuit breaker" mechanisms are placed between chained services so that one failure cannot cascade through the system. In a line of dominoes, taking out a few tiles stops the chain. In the same way, the proposal places biological **"green locks"** in the forest to cancel out the distance fire covers through wind and flying cones.

The proposal rests on three pillars:

1. **Cellular isolation (honeycomb structure).** Continuous forests are segmented into independent, protected hexagonal or cellular clusters. A fire that starts in one cell is contained within it.
2. **Biological green belts.** Cell boundaries are planted with layers of fire-resistant, high-moisture native flora instead of trenches or artificial barriers.
3. **Integrated workforce.** Conscripts and young people doing national service plant and maintain the belts, under an inter-agency protocol.

## How It Works

### Theoretical model: circuit breaker and cellular isolation

```
   ___       ___      |||||||      ___       ___
  /   \     /   \     |GREEN|     /   \     /   \
 | A   |---| B   |    |BELT |    | C   |---| D   |
  \___/     \___/     |||||||     \___/     \___/
  forest    FIRE      biological   protected cells
  cell      (cell B)  circuit
                      breaker
```

*Figure 1: Cellular honeycomb structure and the biological circuit breaker isolation model. A fire in Cell B is stopped by the green belt and does not reach Cells C and D.*

System principles:

- a barrier against the domino effect;
- a heat shield at both the crown and the ground level;
- isolation of separate clusters.

### Biological green belt flora suited to Turkey's ecosystems

Trenches are expensive, so the honeycomb boundaries are planned as layers of native, hard-to-ignite flora with high moisture capacity:

| Plant species (botanical and common name) | Layer role | Fire-resistance mechanism | Ecological fit |
|---|---|---|---|
| **Cupressus sempervirens var. horizontalis** (Mediterranean cypress, horizontal form) | Upper-layer tree barrier | Acts as a windbreak, and its cones do not burst. Holds a lot of moisture in the crown and traps airborne embers and flying cones. Tested as a fire barrier in Spain. | Dry areas of the Aegean and Mediterranean |
| **Ceratonia siliqua** (Carob) | Mid-layer tree barrier | Broad, fleshy leaves hold a lot of water and have a high ignition temperature. Its fallen leaves do not fuel ground fire. Absorbs flame energy. | Coastal and lower Mediterranean belt |
| **Nerium oleander and Laurus nobilis** (Oleander and Bay laurel) | Lower-layer shrub barrier | Stops ground fire from advancing. Lowers flame height and so keeps fire from climbing into tree crowns. | Across the Aegean, Mediterranean and Marmara |
| **Arbutus unedo** (Strawberry tree) | Soil-protecting windbreak | Builds up moisture on the vulnerable forest floor and absorbs flame energy through evaporation. | Marmara and Aegean cut-over (felling) areas |

These plants do not flare up when fire reaches them. Instead they evaporate their water and absorb the fire's energy, which makes it much harder for the fire to jump from one cell to the next.

### Using natural topography

The honeycomb does not need perfect hexagons. Geographic Information Systems (GIS) are used to fold existing rock faces, riverbeds, clearings and roads into the cell boundaries, so the hexagonal pattern adapts to the natural topography. Only the remaining gaps are planted, which brings costs close to zero.

## Implementation & Phasing

### Phase 1: Pilot site

A pilot covering about **500 hectares** is run in a high-fire-risk area under the Regional Forest Directorates of **Muğla (Marmaris/Datça) or İzmir (Urla/Seferihisar)**:

- **Geographic model:** GIS brings existing rock faces, riverbeds and roads into the honeycomb boundaries, and the hexagonal structure is adapted to the natural topography.
- **Field validation:** **5 separate cells** are created, and the moisture retention and windbreak performance of the biological belts is monitored.

### Phase 2: Gradual scale-up

Once the pilot has validated the model, it can be rolled out across other high-fire-risk regions.

### Workforce operating model (national service integration)

Building the biological belts, planting saplings, and preparing and maintaining the land all take substantial labour. To lower costs and use public resources efficiently, the proposal is that the **Ministry of National Defence and the Ministry of Agriculture and Forestry sign a cooperation protocol**. Under it, young people doing their national service, and military units (privates and reserve officers), would work at the planting sites. In the European version of the proposal, the equivalent is a protocol between environment ministries and defence/civil protection ministries that involves conscripts, civic corps or volunteer corps during national service. This approach brings labour costs close to zero and also raises public awareness.

## Stakeholders & Benefits

### Implementing stakeholders and partners

- **Ministry of Agriculture and Forestry, General Directorate of Forestry (OGM)** and its Regional Forest Directorates (pilot site owner and technical lead).
- **Ministry of National Defence** (national-service workforce under the joint protocol).
- **TEMA Foundation** (expert evaluation and development of the proposal).
- For wider European and international uptake: the European Commission (DG ECHO / EU Civil Protection Mechanism), UNCCD, IUCN, and European forestry authorities, as partners for evaluation and pilot validation.
- Environmental engineers, software architects and nature-conservation specialists, contributing across disciplines.

### Comparative analysis

| Parameter | Traditional trench / channel | Biological circuit breaker (proposed) |
|---|---|---|
| **Cost** | High (excavation, fuel, heavy machinery) | Very low (sapling production plus national-service workforce) |
| **Environmental impact** | Damages soil structure, raises erosion risk | Enriches the natural ecosystem and supports beekeeping |
| **Stopping fire jumps** | Cannot stop flying cones | Tall cypress crown barrier catches flying cones |

### Benefits

- Forests are protected "cell by cell" without the cost of trenches.
- Uses native species that fit Aegean and Mediterranean ecosystems and enrich them.
- Labour costs fall close to zero, and public awareness rises.
- The interdisciplinary approach (software plus ecology) can be transferred to other Mediterranean countries.

## Risks & Mitigations

| Risk | Mitigation (from the proposal) |
|---|---|
| The model has not yet been validated under local conditions | A 500-hectare, 5-cell pilot with monitoring of moisture retention and windbreak performance before any scale-up |
| High cost and ecological damage from artificial barriers | Biological green belts instead of trenches or channels, with existing natural barriers used as cell boundaries |
| Fire jumping across barriers through embers and flying cones | Tall, high-moisture cypress crowns as windbreaks that trap embers and cones |
| High labour needs for planting and maintenance | Inter-agency protocol that brings in the national-service workforce |

## References / Examples

- **Andilla wildfire, Valencia, Spain (2012):** during a wildfire that burned everything else in the area, a strip of Mediterranean cypress (*Cupressus sempervirens*) did not burn and stopped the fire. This is the empirical evidence behind the upper-layer barrier species.
- **Circuit Breaker pattern (software architecture):** a design principle for stopping cascading failures between chained services. It is the conceptual source of the model.

## Origin

Originally drafted by Merih İlgör as a public policy proposal (July 2026); published here as an open project idea.

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md).
