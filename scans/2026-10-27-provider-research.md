# Externe bridges en interoperabiliteit — 27 oktober 2026

## Uitkomst

Onderzocht: **12 partijen, 30 publieke GitHub-repositories, 1.908 branchnamen, 500 recent bijgewerkte pull requests in vijf repositories, plus een lege PR-lijst bij een zesde repository, en zes publieke API-responses**.

Geen bevestigde NewL1-integratie, NewL1-RPC of NewL1-bridgeadres aangetroffen in deze selectie. Wel concrete plekken gevonden waar een nieuwe integratie vroeg zichtbaar kan worden. Dit is een nulmeting: zonder eerdere snapshot kunnen we een bestaande onbekende netwerknaam niet als nieuw bestempelen. Een integratie onder een onbekende codenaam blijft mogelijk.

De codezoekactie omvatte tracked tekst op de default branches, met uitsluiting van bestanden met `lock` in de naam, SVG en snapshots. Er zijn 20.602 tracked paden geïnventariseerd; dat is niet hetzelfde als 20.602 inhoudelijk gelezen bestanden. Submodules zijn niet recursief onderzocht. Branchnamen zijn doorzocht, niet de volledige inhoud van alle branches. Alleen de hieronder beschreven kernbestanden zijn nader inhoudelijk bekeken.

## Per partij: waar we voortaan gericht kijken

| Partij | Onderzochte repositories | Concrete monitorroute | Resultaat |
|---|---|---|---|
| Relay | relay-kit, relay-docs, relay-periphery, relay-settlement, relay-protocol-oracle | Oracle-configs voor dev/staging/productie, periphery-deployments, live mainnet/testnet chainlijsten | Geen NewL1-match |
| Stargate | stargate-v2, tutorial-api-transfer | Nieuwe deploymentmappen, chainsConfig, messagingconfig en metadata voor beide omgevingen | Geen NewL1-match |
| LayerZero | LayerZero-v2, devtools, lz-address-book, monorepo-external | Endpoint-ID's, deploymentmetadata, address-book en app peer-configuratie | Alleen ongerelateerde codevariabelen/tooling |
| deBridge | multisig-evm-transactions, debridge-contracts-v1, dln-contracts, debridge-launcher | Publieke multisigvoorstellen, chains_config_default.json, deployment/setup-scripts | Geen NewL1-match |
| Celer/cBridge | sgn-v2-contracts, contract_addresses, cBridge-typescript-client | Bridge/MessageBus-deployscripts, adresdocumentatie en clientconfig | Alleen Oasys-contractconstructor |
| Meson | meson-to | SDK/integratiebroncode en afhankelijkheden | Geen NewL1-match; beperkte dekking van de protocolbackend |
| Mayan | swap-sdk, docs | src/addresses.ts, chains-contracts.mdx en live init-respons | Geen NewL1-match |
| Across | constants, contracts, exclusive-relayer-configs | src/networks.ts, deploymentadressen, SpokePools en relayerconfig | Matches betreffen algemene L1-variabelen |
| Wormhole | wormhole-sdk-ts | Chain-ID-constanten en core/tokenBridge-contracttabellen | Geen NewL1-match |
| Hyperlane | hyperlane-registry | Nieuwe chains/*/metadata.yaml, addresses.yaml en route-artifacts | Geen NewL1-match |
| LI.FI | contracts, types | Netwerk-/facetconfig, deployments en chain-types | Alleen Arbitrum SDK-constructor |
| Axelar | axelar-contract-deployments, axelar-configs | mainnet/stagenet/devnet-configs en gatewayregistraties | Geen NewL1-match |

Alle repo-URL's, gecontroleerde commits, zoekpatroon en resultaten staan in [repositories.json](../data/providers/repositories.json). De geselecteerde org-inventarisatie gebruikte maximaal de eerste 100 repositories per organisatie; dit is geen claim dat alle bestaande bridgepartijen of alle repositories zijn doorzocht.

## Relay: belangrijkste concrete ingangen

1. [Oracle configs](https://github.com/relayprotocol/relay-protocol-oracle/tree/55b22de6358c212c22eebb48d2df5b793a16e863/configs): `chains.mainnets.dev.json`, `chains.mainnets.stag.json`, `chains.mainnets.prod.json`. Nieuwe entries kunnen een chain-ID, depository of RPC-configuratie aanduiden. Staging is een onderzoeksbron, geen bewijs dat een nieuwe chain publiek bruikbaar is.
2. [Periphery deployments](https://github.com/relayprotocol/relay-periphery/tree/f937c6c0747a4685cc23a57f80840c4ec9ffcccc/deployments): adressen en deploy-instructies.
3. [Relay Chains-component](https://github.com/relayprotocol/relay-docs/blob/a0019bb36b93de79c226f821adc2b5ca7e9cb1ed/snippets/Chains.mdx) haalt de lijst dynamisch op. Daarom is alleen GitHub doorzoeken onvoldoende: ook de API-baseline is opgeslagen.

Read-only gecontroleerd: `https://api.relay.link/chains` geeft 59 netwerkentries; `https://api.testnets.relay.link/chains` geeft 2. Geen herkenbare NewL1-entry. BSC heeft chain-ID 56.

De documentatie beschrijft een routerovergang naar v3.2 op 26 oktober 2026. De API geeft op BSC deze nieuwe router/approval-proxy terug. Dit is een bestaande Relay-versiewijziging, geen NewL1-signaal. Zie [contractreferentie](https://github.com/relayprotocol/relay-docs/blob/a0019bb36b93de79c226f821adc2b5ca7e9cb1ed/references/api/api_resources/contract-addresses.mdx).

Ook Relay's eigen hub/chain is niet automatisch BNB NewL1. Een naam als `relay-chain` of een Relay-hub-RPC krijgt pas een BNB-koppeling bij afzonderlijk bewijs.

## Stargate: deployments vóór het gebruikersmenu

Prioriteit hebben:

- [Deploymentmappen](https://github.com/stargate-protocol/stargate-v2/tree/7a0e7a6cfac1cb569b3f5b65203cb900326b9820/packages/stg-evm-v2/deployments): nieuwe map, `.chainId`, TokenMessaging, CreditMessaging, pools en OFTs.
- `packages/stg-evm-v2/devtools/config/mainnet/01/chainsConfig/` en de testnetconfiguratie: netwerktoevoegingen en routeconfiguratie.
- `packages/stg-evm-sdk-v2/src/config/chains.ts`: de SDK leidt beschikbare namen af uit deploymentmappen. Dit maakt nieuwe mappen extra relevant.

De metadata-API leverde 123 mainnet-v2 **asset/deploymententries**, en 16 testnetentries. Dit zijn geen aantallen chains. Geen NewL1-keywordmatch. Bestaande `bsc-sandbox-local`-bestanden zijn sandboxconfiguratie en vormen op zichzelf geen NewL1-lead.

Op BSC hebben TokenMessaging en CreditMessaging meer informatiewaarde voor nieuwe verbindingen dan alleen losse transfers naar een pool: nieuwe peerconfiguratie kan een bestemming zichtbaar maken. `PeerSet(uint32 eid,bytes32 peer)` staat in de [OApp-interface](https://github.com/LayerZero-Labs/devtools/blob/4973ba8bef7b0fdf7268469abea3ea50dbd4bbd8/packages/oapp-evm/contracts/oapp/interfaces/IOAppCore.sol). Controleer de daadwerkelijke deployment-ABI voordat events worden gevolgd. Deze scan heeft geen historische peer-events uitgelezen.

## LayerZero: EID + endpoint + verificatieconfig

De [address-bookgenerator](https://github.com/LayerZero-Labs/lz-address-book/blob/c7c7c8f1cde9612fee901d81bffd1c9cb7f724d3/scripts/lz-generate-addresses.py) verwijst naar de live deploymentmetadata en Stargate-metadata. Die bronnen zijn daadwerkelijk opgehaald.

De LayerZero-respons bevatte **487 netwerkomgevingskeys**, inclusief mainnets, testnets en sandboxes. Herkenbare BNB-keys zijn BSC-mainnet/testnet/sandbox en opBNB-mainnet/testnet; geen herkenbare NewL1-key. Niet alle metadata is actuele gemeten netwerkstaat: bijvoorbeeld een opgegeven gemiddelde blocktijd is geen eigen benchmark.

Volgende scans vergelijken nieuw toegevoegde namen, native chain-ID, EID, stage, EndpointV2, send/receive libraries en executor. Voor BSC is native chain-ID **56**, terwijl LayerZero-v2 EID **30102** is. Die twee ID-systemen nooit verwarren. Hetzelfde endpointadres kan op meerdere chains bestaan; een adres zonder netwerkcontext is onvoldoende.

## Contracten toegevoegd voor vervolgonderzoek

De [externe contractwatchlist](../data/providers/contract-watchlist.json) bevat acht adressen met bron en netwerk:

- Relay: receiver, actuele router en approval proxy op BSC.
- Stargate: TokenMessaging, CreditMessaging, USDC-pool en USDT-pool op BSC.
- LayerZero: EndpointV2 op BSC.

Dit zijn **bestaande BSC-adressen uit providerdata/deploymentbestanden**, geen gevonden NewL1-contracten. Er is in deze scan geen eth_getCode-/ABI-verificatie of log-backfill voor deze acht adressen gedaan. Ze zijn toegevoegd om later configuratie en nieuwe bestemmingen te kunnen volgen, niet om iedere gewone BSC-transfer als lead te melden.

## Ruis beoordeeld en bewaard

- LayerZero-v2: TON-variabele `new_l1` en minified Yarn-code met `new l1`.
- Across: `newL1Gas`, `newl1Gas`, `newL1Token` en commentaar over een nieuwe gaslimiet; veel herhalingen in compiler-inputs.
- Celer: `new L1StandardERC20(...)` voor Oasys.
- LI.FI: `new L1ToL2MessageGasEstimator(...)` voor Arbitrum.

Dit zijn geen BNB NewL1-leads. Ze blijven als beoordeelde zoekmatches in het register staan zodat latere scans niet opnieuw dezelfde ruis onderzoeken.

## PR's, branches en blinde vlekken

Per repository maximaal 100 meest recent bijgewerkte PR's, alle statussen: Relay kit/docs, Stargate-v2, LayerZero-v2/devtools/monorepo-external. Vijf leverden 100 op, monorepo-external nul. Titels, bodies en branchnamen zijn doorzocht op NewL1-technische termen plus BNB/Binance, zonder relevante match. PR-diffs zijn niet allemaal uitgelezen. [Bewaarde PR-metadata](../data/providers/pulls.json).

1.908 publieke branchnamen zijn gecontroleerd op technische zoektermen, zonder match. Een generieke branchnaam kan relevante ongepubliceerde code bevatten: die inhoud is niet volledig onderzocht. Bij monorepo-external is een Windows-padlengteprobleem opgelost; de tekstzoekactie is daarna op HEAD uitgevoerd.

Niet gedekt: private repositories, volledige git-historie, alle open/gesloten PR-diffs, alle remote-branchinhoud, dynamische backenddata van alle twaalf partijen, recursieve submodules en on-chain configuratiehistorie. Dit is gericht provideronderzoek, geen volledige twaalf-route dagelijkse scan.

## Wat de volgende scan vergelijkt

1. Repository-heads en nieuwe/verwijderde kandidaatbestanden, ook bij onbekende chainnamen. [7.105 paden met blob-ID's](../data/providers/file-baseline.json) vormen de nulmeting.
2. De genormaliseerde [netwerkbaseline](../data/providers/network-baseline.json): nieuwe namen/ID's, gewijzigde contracten en extra omgevingen.
3. PR-updates sinds de vorige succesvolle controle, relevante diffs en nieuw gevonden branches.
4. Nieuwe bestemmingen/peers pas aan NewL1 koppelen wanneer chain-ID, genesis of officiële bron dat ondersteunt. Zwakke aanwijzingen wel meteen bewaren.

De vorige contractscan is behouden. De nieuwe providerbronnen zijn aan het centrale scanregister toegevoegd; er draait geen automatische monitor.
