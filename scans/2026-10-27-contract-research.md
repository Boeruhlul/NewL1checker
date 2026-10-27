# Gerichte nulmeting: contracten en bridges

27 oktober 2026. Dit is contractonderzoek, niet de volledige dagelijkse scan van alle twaalf bronroutes. GitHub is de centrale opslag. Onderzochte bronversies en bestandshashes staan in [research-sources.json](../data/research-sources.json).

## Resultaat

- 15 gedocumenteerde NewL1-systeemadressen vastgelegd in [contract-watchlist.json](../data/contract-watchlist.json). Dit zijn adressen uit de devnetdocumentatie, geen onafhankelijk geverifieerde live deployments.
- Vier BSC-adressen read-only gecontroleerd op één vast blok: code aanwezig, bytecode-SHA256 vastgelegd in [bsc-contract-baseline.json](../data/bsc-contract-baseline.json). SHA256 is hier een vergelijkingsfingerprint, niet Ethereum EXTCODEHASH/Keccak.
- Geen bevestigd BSC-side NewL1-bridgecontract of officiële NewL1-deployer gevonden in de onderzochte bronnen. Er is dus geen onderbouwd deployeradres om nu al transacties van te volgen.
- Vier repositories inhoudelijk onderzocht: BNB-documentatie, canonical-bridge, bsc-genesis-contract en de onafhankelijke BUNKER-repository.
- Eén zwakke projectlead behouden; één zoektermverwarring vastgelegd.

## 1. Hoogste prioriteit zodra NewL1 bereikbaar wordt

Alle korte adressen hieronder zijn links met nul aangevuld tot 20 bytes; volledige adressen staan in de JSON-watchlist. De netwerkidentiteit moet altijd bestaan uit chain-ID én genesis-hash. Een gelijk adres op een andere chain betekent niet hetzelfde contract.

| Onderdeel | Adreseinde | Wat we volgen | Waarom relevant |
|---|---|---|---|
| LaneRegistry | 3000 | newl1_laneConfig: laneRoutes, laneDefs, version, inert, anchorHash | Route-adressen kunnen vroege toepassingen/infrastructuur onthullen |
| Governor | 2004 | Nieuwe voorstellen, targets en calldata | Parameters en nieuwe registraties kunnen vóór uitvoering zichtbaar worden |
| Timelock | 2006 | Geplande/uitgevoerde calls en doelen | Tijdige herkenning van komende wijzigingen |
| GovHub | 1007 | updateParam(key,value,target), uitvoering en foutmeldingen | Koppeling tussen voorstel en gewijzigd systeemcontract |
| StakeHub | 2002 | Registraties, operatoradressen en creditcontracten | Nieuwe validatorgerelateerde publieke adressen |
| ValidatorSet | 1000 | Actief rooster en validator schedule | Netwerkactiviteit en validatorwissels |
| ShieldedPool + authverifier | 5000 / 5003 | Codefingerprint en relevante poollogs | Controle van privacy-implementatie en activiteit |
| CrossChain | 2000 | Wijziging van code of beschreven functie | Momenteel placeholder; geen actieve bridge |

Bron: [officiële systeemcontracten](https://docs.bnbchain.org/newl1/governance/system-contracts/), [governance](https://docs.bnbchain.org/newl1/governance/overview/) en [Multi-Lane](https://docs.bnbchain.org/newl1/core-concepts/multi-lane/).

**LaneRegistry is een concrete ontdekkingsroute.** De configuratie bevat adres-naar-lane-routes. Zodra een bereikbare RPC beschikbaar is, kunnen we veranderingen daarvan vergelijken en nieuwe doeladressen apart onderzoeken. Dit bewijst geen samenwerking: een registratie is een technische waarneming. Het voorbeeld in de documentatie toont een lege, inactieve genesisconfiguratie; voorbeelddata is geen gemeten netwerkstaat.

**Precompiles apart behandelen.** AccountKeychain (4000) en SignatureVerifier (4001) zijn native functies. Lege eth_getCode-output bewijst daar niet dat ze ontbreken. Geen blinde transactietests uitvoeren; eerst de gedocumenteerde read-interface gebruiken.

## 2. Contracten die op BSC al bereikbaar zijn

Netwerk: BSC mainnet, chain-ID 56. RPC: https://bsc-dataseed.bnbchain.org. Bloknummer/hash en alle fingerprints staan in het baselinebestand. Geen transactie verstuurd.

| Contract | Adres | Codegrootte | Monitorfunctie |
|---|---|---:|---|
| Governor | 0x0000000000000000000000000000000000002004 | 28.490 bytes | Voorstellen en calldata onderzoeken op expliciete NewL1/bridge-verwijzingen |
| Timelock | 0x0000000000000000000000000000000000002006 | 9.607 bytes | CallScheduled/CallExecuted en hun targets volgen |
| GovHub | 0x0000000000000000000000000000000000001007 | 2.874 bytes | updateParam-doelen en resultaten volgen |
| TokenRecoverPortal | 0x0000000000000000000000000000000000003000 | 7.542 bytes | Negatieve controle: dit is op BSC geen NewL1 LaneRegistry |

De namen/adreskoppelingen volgen uit [System.sol](https://github.com/bnb-chain/bsc-genesis-contract/blob/84641812a013eda1593e058aeed4dc116c7746eb/contracts/System.sol). Codeaanwezigheid is via RPC bevestigd; de runtime is niet onafhankelijk gerecompileerd om volledige broncodegelijkheid te bewijzen.

De [Governor-interface](https://github.com/bnb-chain/bsc-genesis-contract/blob/84641812a013eda1593e058aeed4dc116c7746eb/test/utils/interface/IBSCGovernor.sol) bevat ProposalCreated met description, targets en calldatas. De [Timelock-interface](https://github.com/bnb-chain/bsc-genesis-contract/blob/84641812a013eda1593e058aeed4dc116c7746eb/test/utils/interface/IBSCTimelock.sol) beschrijft CallScheduled en CallExecuted. Bij NewL1 zijn dit kandidaat-ABI's, geen geverifieerde ABI's van een live netwerk.

Belangrijk uit [GovHub.sol](https://github.com/bnb-chain/bsc-genesis-contract/blob/84641812a013eda1593e058aeed4dc116c7746eb/contracts/GovHub.sol): het oude paramChange-event is deprecated. Alleen dat event volgen mist mogelijk wijzigingen. Ook een succesvolle transactie is niet automatisch een geslaagde parameterwijziging: de code kan fouten afvangen en fout-events uitsturen.

**Beperking:** er is geen bewijs dat NewL1-deployments via deze BSC-governance zullen lopen. Dit is een aanvullende, concrete monitorroute. Bestaande governancehistorie is in deze scan niet uitgelezen; er wordt geen claim gedaan dat daarin geen aanwijzingen staan. Het vastgelegde blok is een codebaseline, geen log-scan-cursor.

## 3. Native bridge versus bestaande bridge-aggregator

De [NewL1 netwerkdocumentatie](https://docs.bnbchain.org/newl1/get-started/network-info/) vermeldt een bridge in ontwikkeling. De [migratiehandleiding](https://docs.bnbchain.org/newl1/get-started/migrate-from-bsc/) beschrijft die als nog niet live. CrossChain op NewL1 is een placeholder, geen bewezen toekomstige definitieve bridgebestemming.

De afzonderlijke [canonical-bridge-repository](https://github.com/bnb-chain/canonical-bridge) is een aggregator met provideradapters. Gecontroleerde versie: `6f9f9c54bcc70ccf4ddc8a3907bf6a04c2fd1d50`.

Concrete bestanden voor volgende scans:

- `apps/canonical-bridge-server/src/common/constants/chains.ts`: netwerk-ID's, RPC's en explorers.
- `apps/canonical-bridge-ui/token-config/mainnet/chains.ts`: UI-netwerkconfiguratie.
- `packages/canonical-bridge-sdk/src/adapters/`: cBridge, deBridge, layerZero, mayan, meson en stargate.

Een gerichte zoekactie in tracked TS/TSX/JSON/Markdown gaf geen match voor `newl1|new.l1|laneregistry|txstream`. Dat sluit een integratie onder een andere naam, in een andere branch of via dynamische providerdata niet uit. De aanwezigheid van een adapter bewijst geen NewL1-ondersteuning door die provider.

Bij een echte bridgevondst leggen we vast: beide chain-ID's/genesis-hashes, beide contractadressen, creatietransactie en creator, implementatie/proxy indien van toepassing, publiek admin/timelock-adres en herkomst van de koppeling. Pas dan worden deployer en gerelateerde nieuwe contracten aparte watchlist-items. Geen Binance-hotwallets volgen op basis van naamassociatie alleen.

## 4. Zwakke lead: BUNKER

Een [publieke spiegel van @bunkeronbnb](https://www.sotwe.com/bunkeronbnb) bevat claims over belangstelling voor NewL1 en een mogelijke toekomstige migratiebridge. De code-link uit die berichten verwijst via t.co naar [imadegoodstuff/bunker](https://github.com/imadegoodstuff/bunker). Deze verwijzing is met een HTTP-redirect gecontroleerd.

De primaire repository op commit `b4517b2e67988e6cf2a8f9aeebd449268796c70e` is een BSC-privacyproject. De onderzochte checkout bevat geen NewL1/MigrateExit-match in de geselecteerde MD/SOL/JSON-bestanden en geen gecommitteerde `contracts/deployments/56.json`. `app/src/lib/config.ts` krijgt de pool- en launchpadadressen via buildomgevingsvariabelen; standaard chain-ID is 56. Daardoor kunnen we daaruit geen NewL1-contractadres afleiden.

Status: **onbevestigde onafhankelijke projectlead, geen officiële partner of werkende NewL1-bridge**. De publieke main-branch en recentere sociale claims lopen uiteen; dat blijft een open vraag. Volg nieuwe deploybestanden, branches, SDK-configuratie en een eventueel publiek contractadres. Een in marketing genoemd tokenadres wordt niet als bridgeadres opgenomen.

Direct X lezen gaf HTTP 403. De live docs waren via de webtool niet bereikbaar. Daardoor blijven sociale claims en live deploymentgegevens ongeverifieerd.

## 5. Zoekruis en vervolgstappen

- `NewL1` in Polygon/aggkit bridgesync is een generieke constructornaam, geen bewijs voor BNB NewL1. Bewaard als weerlegde match om dubbel werk te voorkomen.
- Publieke smartcontracts kunnen bij genesis al bestaan zonder normale deployertransactie. Voor NewL1-predeploys is het genesisbestand daarom belangrijker dan uitsluitend contractcreaties zoeken.
- Hoogste prioriteit: native bridgebroncode/deployment vinden, officiële client/genesis vinden, daarna RPC-identiteit en LaneRegistry controleren.
- BSC-governancehistorie en toekomstige events gericht onderzoeken; ABI-signatures uit geverifieerde interface gebruiken en logvensters met blokhash vastleggen. Niet doen alsof de codebaseline al eventdekking geeft.
- Relevante open PR's, andere branches en openbare deployerrelaties uitbreiden wanneer een concrete oorsprong is gevonden.

## Dekking

Gedaan: vier default-branch checkouts, gerichte zoekopdrachten, officiële contract/governance/netwerk/lane-documentatie, privacyforum, bridgeconfiguratie, BUNKER-verwijzing en read-only BSC-codecontrole. Geen software uit de onderzochte repos uitgevoerd.

Niet gedaan: volledige git-historie/alle branches/alle PR's, historische BSC-governancelogs, NewL1 on-chain controle (geen geverifieerde RPC), brede provider-API-scan, DNS/certificaatonderzoek, packagecatalogi en besloten communities. De algemene dagelijkse nulmeting blijft daarom open.
