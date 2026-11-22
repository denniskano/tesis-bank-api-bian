# APIs bancarias públicas — candidatos de corpus

Nota de trabajo. 21 de noviembre de 2026.

Open Finance Brasil es la parte ya corrida. Desde el borrador del 21 de noviembre de 2026, el corpus de la tesis es esta lista: los estándares que se bajan sin cuenta entran al mismo procedimiento. Solo I1–I6 tienen score. FDX y los PDF no entran al parser.

Las URLs de la primera tabla devolvieron HTTP 200 ese día. El detalle de versión y de licencia sale de la cabecera del archivo o del README del repositorio, no de una corrida del parser.

## Corpus que sí se bajan

| Dónde | Qué es | Dónde está | Formato | Qué cubre | Licencia | Idioma |
|-------|--------|------------|---------|-----------|----------|--------|
| Brasil (ya en el piloto) | Open Finance Brasil | https://github.com/OpenBanking-Brasil/openapi | OpenAPI, varios YAML | Cuentas, pagos, datos abiertos, y el resto de las fases | La del repositorio OFB | Portugués |
| Reino Unido | Open Banking Read/Write, Open Banking Limited, v4.0.1 en `master` | https://github.com/OpenBankingUK/read-write-api-specs | OpenAPI 3.0 YAML y JSON | Cuentas, iniciación de pagos, confirmación de fondos. En el mismo directorio hay VRP y eventos | `open-licence`: https://www.openbanking.org.uk/open-licence | Inglés |
| Europa | Berlin Group NextGenPSD2 XS2A, v1.3.16 (27 nov 2025) | https://gitlab.com/the-berlin-group/nextgenpsd2 | OpenAPI 3.0.1 YAML | Cuentas, pagos, confirmación de fondos. Carpeta extendida: valores, tarjetas, beneficiarios | OpenAPI en CC BY 4.0. Las guías PDF son CC BY-ND y son el texto normativo | Inglés |
| Europa | Berlin Group openFinance | https://gitlab.com/the-berlin-group/openfinance | OpenAPI 3.0.1 YAML | AIS, pagos, confirmación de fondos y extensiones. El YAML abierto de AIS dice 2.4.1 | CC BY 4.0 en el archivo AIS. El PDF puede ir por delante del YAML | Inglés |
| Francia | STET PSD2 API 1.6.3.2 | https://www.stet.eu/en/psd2/ y el JSON https://www.stet.eu/assets/files/site/PSD2/stetpsd2api-1.6.3.2.final-oas3.json | Swagger 2 YAML y OpenAPI 3 JSON | Cuentas, pagos, confirmación de fondos | CC BY 3.0 Francia, dentro del JSON | Inglés |
| Polonia | PolishAPI 3.0 y el archivo de cambios 3.0.1 | https://polishapi.org/en/documentation/ y https://docs.polishapi.org/ver3.0/PolishAPI-ver3_0.yaml | Swagger 2 YAML | Cuentas, pagos, confirmación de fondos, más un YAML de callback | CC BY 3.0 Polonia, en la página de documentación | Polaco e inglés en los mismos resúmenes |
| Chequia | Czech Open Banking Standard 9.0.0, Czech Banking Association | https://github.com/Czech-BA/COBS | OpenAPI 3.0.1 YAML | Cuentas, iniciación de pagos, consulta de saldo | El repositorio no declara licencia | Inglés |
| Australia | Consumer Data Right, Data Standards Body (Treasury), banking 1.36.0 | https://github.com/ConsumerDataStandardsAustralia/standards | OpenAPI 3.0.3 YAML y JSON. El de banca es `cds_banking.yaml` | Productos, cuentas, saldos, movimientos, débitos, pagos programados, beneficiarios. El mismo repo tiene energía y telecomunicaciones | MIT | Inglés |
| Nueva Zelanda | Payments NZ API Centre | https://github.com/PaymentsNZ/API-Account-Information y https://github.com/PaymentsNZ/API-Payments-Initiation | YAML y JSON. Cuentas v3.0.3 es OpenAPI 3.0.3; v2.3.5 sigue en Swagger 2 | Consentimiento de cuentas, cuentas, saldos, movimientos. Otro repo para iniciación de pagos | El README dice que la licencia no está cerrada. Sale del estándar del Reino Unido, no es una ley | Inglés |
| Baréin | CBB Open Banking Framework v1.0.0 (ago 2020) | https://github.com/Central-Bank-of-Bahrain/Bahrain-OBF | OpenAPI 3 YAML y JSON | Cuentas, movimientos, productos, iniciación de pagos | El YAML pone `license.name: TBC-licence`. No hay archivo LICENSE | Inglés |
| Emiratos | Open Finance de CBUAE, archivos en el operador Nebras | https://github.com/Nebras-Open-Finance/api-specs | OpenAPI 3 YAML. El de pagos dice `v2.1-errata3` | Cuentas, pagos, confirmación de beneficiario, seguros, productos, cajeros, FX | MIT en el archivo de GitHub, copyright 2026 Nebras. El sitio se declara comunitario, no el repositorio de CBUAE | Inglés |
| India | ReBIT Account Aggregator 2.1.0 | https://specifications.rebit.org.in/api_specifications/account_aggregator/AA_2_1_0.yaml | Swagger 2 YAML. También hay FIP | Consentimiento y flujo de datos del agregador. No es una API de cuentas de un banco | No figura en la cabecera | Inglés |

Archivos sueltos, de un banco, no de un estándar nacional:

- Eslovaquia, VÚB, implementación Berlin Group: https://www.vub.sk/document/documents/VUB/psd2/vub-psd2-api_1.3.12_2024-08-23.yaml
- Hungría, K&H: https://webelectra.sso.kh.hu/eibpublic_portal/yaml/khb_eleapi.yaml
- México, CNBV, solo cajeros: https://api.swaggerhub.com/apis/CNBV9/atm_open_data_specification/1.0.0 (Swagger 2, `GET /atms`)

## Existe el estándar, y no se baja sin cuenta

FDX (Financial Data Exchange) es el contrato de Estados Unidos y el que Canadá designó como estándar técnico. La propia FAQ dice OpenAPI 3.1, detrás de la licencia. El alta de observador es gratis y sigue pidiendo registro. No hay un GitHub público del YAML. No sustituir FDX por el archivo de un agregador.

Italia, CBI Globe: la wiki es pública (https://www.cbiglobe.com/Wiki/index.php/Main_Page). El OpenAPI está detrás del portal.

## Regla o PDF, sin corpus YAML

No encontramos un árbol de YAML nacional, sin registro, en estos sitios. Sirven como contexto. No entran al parser tal cual.

| Dónde | Qué hay | Dónde mirar |
|-------|---------|-------------|
| Eslovaquia, asociación | PDF v2.1 del Slovak Banking API Standard. El enlace al OpenAPI 2.0 del propio PDF no resolvió | https://www.sbaonline.sk/projekt/slovak-banking-api-standard/ |
| Chile, CMF | NCG 514 y NCG 569 piden OpenAPI 3.1. El portal es tablas HTML | https://www.cmfchile.cl/normativa/ncg_514_2024.pdf |
| Colombia, Superfinanciera | Circular y una hoja de ruta de febrero 2024 que anuncia Swagger. Esos archivos no aparecieron | https://www.superfinanciera.gov.co/publicaciones/10114733/innovasfcfinanzas-abiertasfinanzas-abiertas-colombiadesarrolladorespeguntas-frecuentes-10114733/ |
| Perú, SBS | Wiki de finanzas abiertas, sin YAML | https://sbsperu.atlassian.net/wiki/spaces/finanzasab/overview |
| Argentina, Uruguay | Decreto o anteproyecto. Sin OpenAPI de finanzas abiertas | — |
| Arabia Saudita, SAMA | Portal del Open Banking Lab. Pide contacto. Sin GitHub de SAMA | https://www.openbanking.sama.gov.sa/index-en.html |
| Nigeria | El repo de Open Banking Nigeria es Markdown y Java, no un OpenAPI. La guía del CBN (marzo 2023) es PDF | https://github.com/openbankingnigeria/api |
| Singapur | ABS–MAS API Playbook, PDF. Cada banco publica el suyo | https://www.mas.gov.sg/-/media/mas/smart-financial-centre/api/absmasapiplaybook.pdf |
| Hong Kong | Marco de HKMA y el PDF de fase III de HKAB. Cada banco publica su diccionario | https://www.hkma.gov.hk/media/eng/doc/key-information/press-release/2018/20180718e5a2.pdf |
| Japón | Estándar de mensajes de la Japanese Bankers Association, en PDF y en japonés | https://www.zenginkyo.or.jp/fileadmin/res/abstract/council/openapi/openapi_sp_1.pdf |
| Corea | FSC es datos públicos. KFTC es HTML y pide cuenta de desarrollador | https://developers.kftc.or.kr/dev/starter/starter |
| Indonesia, SNAP | Guías HTML de BI / ASPI. El sandbox pide login | https://www.bi.go.id/en/layanan/Standar/SNAP/default.aspx |
| Malasia, BNM | El borrador de 2018 apuntaba a un GitHub que hoy no tiene repositorios | https://github.com/BankNegaraMY |
| Sudáfrica | PDF de consulta del SARB (nov 2020). Nedbank publica un Swagger propio. No es el estándar nacional | https://www.resbank.co.za/content/dam/sarb/what-we-do/payments-and-settlements/regulation-oversight/Consultation%20Paper%20on%20open%20banking.pdf |

La EBA y el BCE no publican un OpenAPI de XS2A. PSD2 obliga a la interfaz. No fija el YAML.

## Qué no usar como fuente

Agregadores comerciales (Plaid, Tink, TrueLayer, Tarabut, Lean, Mono, OnePipe) no son el estándar. Un gist o un YAML viejo de Berlin Group en otro GitHub tampoco: el archivo vigente está en el GitLab del grupo. N26 publica su XS2A, que es el banco, no el estándar.

## Cómo se usarían, si se usan

El objeto sigue siendo el contrato OpenAPI bancario. OFB es la muestra que ya corrimos. Reino Unido, Berlin Group, STET, PolishAPI, Chequia y Australia son los más cercanos a ese tipo de archivo: un estándar sectorial, en YAML, sin registro.

Casi todos están en inglés. Eso cambia el piso léxico que vimos en OFB (portugués contra BIAN en inglés). No cambia la regla de la muestra: un estándar nacional no es el YAML de un banco emisor, y no autoriza a puntuar todo contra todo.

Antes de copiar un archivo al piloto hay que fijar tres cosas: el tag o la versión (en Reino Unido `master` se mueve; conviene `v4.0.1`), la licencia (Nueva Zelanda y Baréin no están cerradas; Emiratos está en un repo comunitario), y que el PDF normativo puede ir por delante del YAML (Berlin Group lo dice en el README).
