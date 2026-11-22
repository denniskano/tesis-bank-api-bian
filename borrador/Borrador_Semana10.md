---
title: "Borrador 1 — Semana 10. Resultados preliminares"
author: "Alex Mancilla Antay y Dennis Jack Paitan Cano — Grupo 10"
date: "21 de noviembre de 2026"
lang: es
---

# Borrador 1 — Semana 10

**Tesis:** Alineación entre contratos OpenAPI bancarios y modelos de referencia BIAN mediante análisis estructural y similitud semántica.

**Autores:** Alex Mancilla Antay y Dennis Jack Paitan Cano (Grupo 10).  
**Asesor:** por designar.  
**Fecha de este borrador:** 21 de noviembre de 2026.

Este archivo es el primer entregable del borrador. No es la tesis cerrada. La Guía N.º 2 (maestría profesionalizante) pide el capítulo III como desarrollo de la solución: propuesta, datos, resultados, discusión e impacto. La sesión 10 pide comprobar que el método ya se ejecutó. Aquí va esa evidencia. Los capítulos I, II y IV se redactan en las semanas siguientes a partir del plan ya aprobado. No los reescribimos en esta entrega.

El código y los YAML están en [piloto-bank-api-bian](https://github.com/denniskano/piloto-bank-api-bian), commit `12c1c04`. Los números de esta página salen de la corrida del 6 de noviembre de 2026. Esa corrida quedó firmada en `results/matching/oe1_firma.json` (fecha `2026-11-06T21:00:02Z`). No los volvimos a calcular el 21 de noviembre: el repositorio publica esos archivos.

## 1. Qué ya corrimos, y qué no

El objetivo general es calcular SemanticScore (S), StructuralScore (E), CoverageScore (C) y AlignmentScore para un contrato OpenAPI bancario frente a un Service Domain BIAN.

| Objetivo | Qué quedó hecho el 6 nov 2026 | Qué falta |
|----------|-------------------------------|-----------|
| OE1. Normalizar | Parser sobre 296 YAML. Firma 296/296. | Nada de esta corrida. El parser no aplana `allOf`. |
| OE2. Estructural | Greedy uno a uno en I1–I6. Pares congelados. | Etiquetas de un lector. |
| OE3. Semántico | S léxico (Jaccard + TF-IDF) y S MiniLM sobre esos pares. | Fine-tuning. No lo vamos a hacer: no hay etiquetas para ajustar. |
| OE4. Integrar | AlignmentScore = (S + E + C) / 3 y nivel por instancia. | Precision, recall, F1 y kappa. Salen cuando cerremos el etiquetado. |

No corrimos las 38 APIs contra los 258 dominios. No hay YAML de un banco emisor. No entrenamos un clasificador.

## 2. Datos

| | Open Finance Brasil | BIAN v14.0.0 | Total |
|--|---------------------|--------------|-------|
| Contratos | 38 | 258 | 296 |
| Operaciones | 160 | 4 580 | 4 740 |

Los YAML de OFB vienen de [OpenBanking-Brasil/openapi](https://github.com/OpenBanking-Brasil/openapi). Los de BIAN, de `bian-official/public`, release 14.0.0. Están en `data/ofb/` y `data/bian/` del piloto. No hay datos personales. El parser lee OpenAPI 3.x y deja una fila por operación: ruta, verbo, `operationId`, esquema y texto.

La población es cualquier par contrato OpenAPI bancario × Service Domain BIAN. La adquisición de este piloto son los 296 archivos. La muestra de score son siete instancias. I1 con I2, e I3a con I3b, comparten YAML. No son siete observaciones independientes. OFB es la muestra pública. No redefine el objeto.

| ID | OFB | BIAN | Para qué está |
|----|-----|------|----------------|
| I1 | accounts 2.4.2 | Savings Account | Par de cuentas |
| I2 | el mismo YAML | Current Account | El mismo contrato, otro dominio |
| I3a | opendata-accounts 1.0.1 | Savings Account | Cuentas en datos abiertos |
| I3b | el mismo YAML | Current Account | El mismo contrato, otro dominio |
| I4 | payments 4.0.0 | Payment Order Initiation | Pagos |
| I5 | automatic-payments 2.0.0 | Payment Order Initiation | Pagos automáticos |
| I6 | enrollments 2.1.0 | Payment Order Initiation | Control negativo. FIDO, no una orden de pago |

Calidad: la firma OE1 compara operaciones y esquemas extraídos contra lo que el YAML declara. Meta 1,00. Los 296 quedaron firmados. Las propiedades se cuentan en `properties` directas. `allOf` no se aplana. No copiamos tags `CR` ni `BQ` al YAML de OFB.

## 3. Procedimiento que se ejecutó

Cuatro programas, en este orden. Cada uno lee la salida del anterior.

1. `python -m ofb_bian.procesar_openapi` lee `data/` y escribe `results/consolidado/` y `results/por_contrato/`. Esos CSV no van a GitHub. Se regeneran.
2. `python -m ofb_bian.matching_v0` arma los pares de I1–I6 y calcula E, S léxico y C.
3. `python -m ofb_bian.matching_s1` recalcula solo S, con MiniLM, sobre los pares que congeló el paso 2. E y C no se tocan.
4. `python -m ofb_bian.mapa_ofb_bian` es un catálogo extra (loans, cards, investments). No entra en n = 7.

### StructuralScore (E)

Un candidato entra si el Jaccard del nombre llega a 0,40, o la ruta toca el recurso del Service Domain, o el verbo cae en el mismo grupo y el Jaccard del nombre llega a 0,25.

`e_pair = 0,40·nombre + 0,25·ruta + 0,20·verbo + 0,15·profundidad`

El verbo vale 1 si es el mismo, 0,5 si ambos son de lectura o ambos de escritura, y 0 si cruzan. GET y PUT pueden emparejarse. El greedy asigna cada operación una sola vez. La operación OFB sin par entra como 0. E es la media de `e_pair` sobre todas las operaciones OFB.

### SemanticScore

S v0, solo en los pares: `(Jaccard de tokens + coseno TF-IDF) / 2`. Sin GPU. El texto de OFB está en portugués y el de BIAN en inglés. Por eso el S léxico se queda cerca de cero.

S v1 usa `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`, ya entrenado, sin fine-tuning. El vector se normaliza y el coseno es el producto punto. S v1 es la media de esos cosenos sobre los mismos pares. Los cortes 0,45 / 0,55 / 0,65 se guardan como sensibilidad. No recortan el S oficial.

### CoverageScore y AlignmentScore

C es la fracción de tags `CR` y `BQ` del Service Domain que recibieron al menos un par.

AlignmentScore oficial = (S + E + C) / 3. La sensibilidad, sin cambiar el número oficial, usa (0,50, 0,30, 0,20) y (0,20, 0,50, 0,30).

| Nivel | Corte |
|-------|--------|
| Alta | 0,80 o más |
| Media | 0,60–0,79 |
| Baja | 0,40–0,59 |
| Nula | < 0,40 |

Esos cortes son convención de diseño. No salen de etiquetas.

## 4. Resultados preliminares

S de la tabla es MiniLM. E y C son los del matching v0. El detalle está en `results/matching_s1/COMPARACION_V0_V1.md` del piloto.

| ID | S léxico | S MiniLM | E | C | Score v0 | Score v1 | Nivel |
|----|----------|----------|---|---|----------|----------|-------|
| I1 | 0,01 | 0,56 | 0,35 | 0,40 | 0,26 | 0,44 | Nula → Baja |
| I2 | 0,01 | 0,53 | 0,47 | 0,50 | 0,33 | 0,50 | Nula → Baja |
| I3a | 0,00 | 0,45 | 0,56 | 0,20 | 0,25 | 0,41 | Nula → Baja |
| I3b | 0,00 | 0,46 | 0,56 | 0,20 | 0,25 | 0,41 | Nula → Baja |
| I4 | 0,00 | 0,52 | 0,29 | 0,25 | 0,18 | 0,35 | Nula → Nula |
| I5 | 0,03 | 0,59 | 0,32 | 0,50 | 0,28 | 0,47 | Nula → Baja |
| I6 | 0,00 | 0,00 | 0,00 | 0,00 | 0,00 | 0,00 | Nula → Nula |

Pares que armó el greedy, sobre operaciones OFB: I1 4/6, I2 5/6, I3a 2/2, I3b 2/2, I4 3/6, I5 4/7, I6 0/8.

Nadie llega a Alta. Nadie llega a Media. Cinco instancias pasan de Nula a Baja cuando S deja de ser léxico. I4 se queda en Nula: S sube a 0,52, pero E (0,29) y C (0,25) bajan la media a 0,35. I6 se queda en cero. No hubo par. Es el control negativo, y el procedimiento no inventó una correspondencia.

## 5. Qué leemos en esos números

El S léxico entre 0,00 y 0,03 no dice que las cuentas no se parezcan a Savings Account. Dice que `conta` y `SavingsAccountFacility` no comparten tokens. MiniLM, entrenado en varios idiomas y sin ajuste nuestro, sube S a la banda 0,45–0,59 en I1–I5 y lo deja en 0 en I6. Esa diferencia es el motivo de tener dos SemanticScore. El léxico es el piso. MiniLM es el que usamos como S oficial de esta corrida.

E no pide el mismo verbo HTTP. Si lo pidiéramos, OFB (casi todo GET) contra BIAN (casi todo PUT de ciclo de vida) caería a cero por una convención de interfaz, no por el recurso de negocio. Aun así E queda bajo en pagos (I4 0,29, I5 0,32): de 6 y 7 operaciones OFB, el greedy solo sostiene 3 y 4 pares, y las que sobran entran como 0 en la media.

C mide otra cosa. Cuenta cuántos tags `CR` y `BQ` del dominio recibieron par. En opendata-accounts, C = 0,20: el YAML abierto no cubre el desglose de behaviors de Savings Account. No es un fallo del parser. Es el hallazgo de cobertura.

I1 e I2 no se pueden leer como dos bancos. Es el mismo `accounts` 2.4.2 contra dos dominios. Lo mismo I3a e I3b. La muestra de score es siete instancias y menos de siete contratos.

Los niveles no están calibrados. Hasta que Alex y Jack etiqueten match o no-match por recurso de negocio, y el asesor adjudique el desacuerdo, no hay kappa ni F1. Un verdadero positivo será un par del greedy que el lector confirme. Un falso positivo, un par que rechace. Un falso negativo, un par del lector que el greedy no armó. Esas tres cifras no están en esta tabla.

## 6. Bitácora

| Fecha | Qué pasó | Dónde queda |
|-------|----------|-------------|
| 6 nov 2026 | Parser, firma 296/296, matching v0 y MiniLM v1 sobre I1–I6. | `oe1_firma.json`, `results/matching/`, `results/matching_s1/`. El commit de esa corrida, cuando el piloto aún estaba dentro del repo del curso, es `ec2b2ad`. |
| 21 nov 2026 | El piloto pasa a su repositorio. Se quita el libreto. El README queda con la arquitectura y los modelos. | [piloto-bank-api-bian](https://github.com/denniskano/piloto-bank-api-bian) `12c1c04`. Los scores no se recalcularon. |
| 21 nov 2026 | Este borrador, en el repositorio de la tesis. | `tesis-bank-api-bian` |

Dificultades que ya vimos y no escondemos: el techo léxico portugués–inglés; el parser que no aplana `allOf`; I1/I2 e I3a/I3b que no son independientes; los umbrales sin etiquetas; el asesor todavía sin designar.

## 7. Cómo repetirlo

Otra persona clona el piloto, no este repositorio.

```bash
git clone https://github.com/denniskano/piloto-bank-api-bian.git
cd piloto-bank-api-bian
git checkout 12c1c04
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
python -m ofb_bian.procesar_openapi
python -m ofb_bian.matching_v0
python -m ofb_bian.matching_s1
```

Python 3.11 o superior. Dependencias fijadas por mínimo en `pyproject.toml`: PyYAML 6 o superior, sentence-transformers 3 o superior, numpy 1.26 o superior. La primera vez, `matching_s1` descarga MiniLM desde Hugging Face. No hace falta GPU para el S léxico. El resultado comparable es la tabla de `results/matching_s1/COMPARACION_V0_V1.md`.

El orden no se puede saltar. `matching_v0` lee los CSV del parser, y esos CSV no están en GitHub.

## 8. Qué no está en este borrador

- Capítulos I, II y IV en el formato largo de la Guía N.º 2. El plan aprobado ya tiene el planteamiento y el marco. Los pasamos a prosa de tesis en las semanas 12 a 16.
- Etiquetado, kappa y F1.
- Corrida 38 × 258 y YAML de un banco emisor.
- Nombre del asesor.
