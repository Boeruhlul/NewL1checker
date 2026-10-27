# Instructies voor agents

- Lees README.md, SCANPLAN.md en data/state.json voor iedere NewL1-scan.
- Lees ook data/contract-watchlist.json en het contractonderzoek in scans/2026-10-27-contract-research.md. BSC-codebaseline is geen event-scan-cursor. Bewaar netwerkidentiteit per contract; 0x...3000 op BSC is geen NewL1 LaneRegistry en NewL1 CrossChain is volgens de nulmeting een placeholder.
- De nieuwste GitHub-versie is leidend. Haal remote wijzigingen op voor je schrijft; overschrijf geen wijzigingen van anderen en gebruik geen force push.
- Scans starten uitsluitend op verzoek. Geen scheduler installeren.
- Volg alle bronroutes uit SCANPLAN.md; leg niet-uitgevoerde en mislukte controles expliciet vast.
- De eerste echte scan is een nulmeting; maak geen fictieve scanresultaten op basis van het bestaan van dit plan.
- Bewaar verslagen in scans/ met datum en tijd; werk data/state.json bij met werkelijke controles en bevindingen. Bewaar relevante snapshots in data/.
- Houd onbevestigde en weerlegde leads bij en ontdubbel herpublicaties.
- Commit en push de scanresultaten naar deze repository als onderdeel van de scanopdracht. Controleer daarna dat de remote versie klopt. Als push mislukt, meld dat expliciet.
- Gebruik uitsluitend publieke informatie of informatie waarvoor toegang is gegeven. Deze repo is publiek: geen credentials, privéberichten of vertrouwelijke endpoints publiceren.
- Geen transacties, geldverplaatsingen, onbekende software uitvoeren of contact met derden zonder aparte opdracht.
