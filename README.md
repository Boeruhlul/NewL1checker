# NewL1checker

Handmatig gestart onderzoek naar BNB Chain NewL1. Geef de opdracht: **Doe de NewL1-scan** en verwijs in een nieuwe chat naar deze repository.

GitHub is de centrale bron voor het plan, scanstatus en geschiedenis. Geen automatische planning.

- [Scanplan](SCANPLAN.md): bronnen, werkwijze en meldingscriteria.
- [Scanregister](data/state.json): vorige scan, bronstatus en leads.
- `scans/`: gedateerde scanverslagen, toegevoegd bij daadwerkelijke scans.
- `data/`: bronregistratie en relevante snapshots, toegevoegd tijdens scans.

## Huidige status

Op 27 oktober 2026 is een [gerichte contract- en bridgenulmeting](scans/2026-10-27-contract-research.md) uitgevoerd. De [watchlist](data/contract-watchlist.json) bevat 15 gedocumenteerde NewL1-systeemadressen; een [BSC-codebaseline](data/bsc-contract-baseline.json) legt vier BSC-adressen vast. De volledige dagelijkse nulmeting van alle bronroutes is nog niet uitgevoerd.

## Na iedere scan

Aanvullend uitgevoerd: [externe bridgepartijen](scans/2026-10-27-provider-research.md), met 12 partijen en 30 repositories. Zie de [providerbaseline](data/providers/repositories.json), [netwerkgegevens](data/providers/network-baseline.json) en [externe contractwatchlist](data/providers/contract-watchlist.json). Geen bevestigde NewL1-integratie gevonden binnen deze selectie.

Lees eerst de nieuwste repositoryversie, vergelijk met de vorige succesvolle controle per bron, bewaar nieuwe en gewijzigde leads, werk het register bij en commit de resultaten naar deze repository. Rapporteer onbereikbare en niet-gecontroleerde bronnen. Onbevestigde leads blijven zichtbaar.

Deze repository is openbaar: bewaar geen sleutels, tokens, privéberichten of vertrouwelijke toegangsgegevens. Lokale bestanden zijn hoogstens tijdelijke werkkopieën.
