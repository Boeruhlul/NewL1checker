# NewL1 — handmatige dagelijkse scan

Afspraak vastgelegd op 27 oktober 2026 (Europe/Amsterdam).

## Start

De gebruiker start een scan met bijvoorbeeld: “Doe de NewL1-scan”. Geen automatische planning. Lees voor iedere scan dit plan en data/state.json, plus eerdere scanverslagen. GitHub-repository Boeruhlul/NewL1checker is de centrale bron. Lees altijd de nieuwste remote versie en commit/push scanresultaten na iedere scan; vertrouw niet alleen op chatgeheugen of een lokale kopie.

## Doel

Zo vroeg mogelijk netwerkparameters, RPC's, chain-ID's, genesisconfiguratie, clientbuilds, faucets, explorers, bridges en mogelijkheden voor vroege deelname vinden. Hoge tolerantie voor foutieve leads: onbevestigde informatie behouden en zichtbaar labelen, niet wegfilteren vanwege onzekerheid.

## Bronnen

1. BNB Chain GitHub: publieke repos, branches, commits, open en gesloten PR's, issues, tags, releases en beschikbare publieke buildconfiguratie.
2. Documentatiebron: bnb-chain/bnb-chain.github.io, vooral docs/newl1/ en mkdocs.yml; vergelijk inhoud, niet alleen wijzigingsdatums.
3. Publieke forks en technische activiteit van aantoonbaar betrokken ontwikkelaars.
4. NewL1-forum: onderwerpen, reacties en wijzigingen aan bestaande berichten.
5. Betrokken validators en builders: publieke configuraties, handleidingen en onboarding.
6. Chainregistraties, wallets en SDK's, waaronder ethereum-lists/chains en relevante viem-netwerkdefinities.
7. RPC-, explorer- en indexerproviders: netwerkcatalogi, changelogs en integraties.
8. Gerelateerde publieke package- en containerregistraties: npm, Cargo, Docker, GHCR.
9. Publieke certificaatlogs en DNS voor onderbouwd gerelateerde domeinen; geen naamgelijkenis als bewijs behandelen.
10. App-, bridge- en contractdeploymentconfiguraties van gerelateerde projecten.
11. Toegankelijke sociale/communitykanalen, waaronder BNB Chain en BNB Chain Developers; besloten bronnen alleen met beschikbare toegang.
12. Brede zoekronde: technische blogs, hackathons, demo's, video's, presentaties en nieuwe namen.

Zoektermen: NewL1, New L1, BNB L1, next-gen L1, TxStream, PriorityLane, Multi-Lane, LtHash, newl1_, TEE Gateway, devnet, testnet, genesis, chainId, bootnodes, rpc, faucet, validator onboarding. Breid zoektermen en bronnen uit bij nieuwe ontdekkingen.

## Uitvoering en geheugen

- Eerste volledige scan: nulmeting met terugblik op reeds bestaande relevante commits, PR's en forumdiscussies. De eerdere chatresearch is slechts vooronderzoek.
- Volgende scans: lees vorige status; controleer wijzigingen sinds laatste succesvolle controle PER BRON, met enige overlap. Een mislukte controle verschuift die succesvolle controledatum niet.
- Noteer per bron: URL, controletijd, laatste succesvolle controle, gebruikte zoekopdracht of methode, revisie/hash indien beschikbaar, bevindingen, toegankelijkheid en beperkingen.
- Bewaar relevante tekst/configuraties of inhoudshashes waar mogelijk zodat wijzigingen aantoonbaar zijn. Leg oorspronkelijke publicatietijd en eigen eerste waarneming apart vast.
- Houd elke unieke lead bij met stabiel ID, bron, concrete gegevens, eerste/laatste waarneming, bewijs, status en vervolgstap. Ontdubbel gekopieerde berichten zonder onafhankelijke bevestiging te suggereren.
- Labels: onbevestigd; technisch reagerend; sterk gekoppeld aan NewL1; bevestigd; weerlegd. Bewaar weerlegde leads.
- Controleer gevonden toegankelijke RPC's met beperkte alleen-lezen verzoeken: chain-ID, client, voortschrijdende blokhoogte, genesis-hash en blockparameters. Geen transacties, geldverplaatsingen, onbekende software uitvoeren of berichten versturen zonder aparte opdracht.
- Houd parameters bij: consensus, block interval, turn length, EVM-versie, base fee, fee model, per-transaction gas cap, block gas limit. Maak onderscheid tussen documentatie, configuratie, voorstel en live waarneming.
- Rapporteer ontbrekende toegang, fouten en niet gecontroleerde routes expliciet. Claim geen volledige dekking bij onvolledige scan.
- Bewaar interne snapshots en scanstatus in data/. Bewaar gebruikersverslagen in scans/ met datum en tijd in bestandsnaam.
- Werk status pas bij op basis van werkelijk uitgevoerde controles. Markeer gedeeltelijke scans als gedeeltelijk.

## Dagelijks antwoord

1. Nieuwe leads, inclusief zwakke of verdachte leads.
2. Veranderingen aan bestaande leads en netwerkparameters (oud → nieuw).
3. Belangrijkste open vragen en vervolgstappen.
4. Scanbereik: gecontroleerde routes, onbereikbare bronnen en controles die niet zijn uitgevoerd.
5. Link naar het opgeslagen verslag.

Als er niets nieuws is, meld dit beknopt met de daadwerkelijke dekking. Geen herhaling van de volledige achtergrondinformatie.

## Bevestigde startbronnen uit vooronderzoek

- https://github.com/bnb-chain/bnb-chain.github.io
- https://github.com/bnb-chain/bnb-chain.github.io/blob/main/mkdocs.yml
- https://docs.bnbchain.org/newl1/get-started/network-info/
- https://docs.bnbchain.org/newl1/developers/quick-guide/
- https://docs.bnbchain.org/newl1/developers/examples/
- https://forum.bnbchain.org/t/about-the-bnbchain-newl1-category/5437
- https://forum.bnbchain.org/t/introducing-newl1-mev-solution-tee-gateway/5589
- https://github.com/ethereum-lists/chains

Vooronderzoek meldde intern devnet, geen geverifieerde publieke RPC. Dit is geen actuele nulmeting en moet bij de eerste scan opnieuw worden gecontroleerd.

