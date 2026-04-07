# Legalize EE

### Eesti õigusaktid Markdown-vormingus, versioonide juhtimisega Git'is.

Iga seadus on fail. Iga muudatus on commit.

**Ametlik allikas:** [Riigi Teataja](https://www.riigiteataja.ee) — Eesti Vabariigi ametlik väljaanne

Osa projektist [Legalize](https://github.com/legalize-dev/legalize) · [legalize.dev](https://legalize.dev)

> **Algusfaas** — See hoidla on aktiivses arenduses. Failistruktuur, commit'ide ajalugu ja sisu võivad oluliselt muutuda, sealhulgas täielikud uuesti genereerimised.

## Kiire algus

```bash
# Eesti õigusaktide kloonimine
git clone https://github.com/legalize-dev/legalize-ee.git

# Otsi põhiseaduse esimest paragrahvi
grep -A 3 "§ 1" ee/24304.md

# Vaata konkreetse seaduse muudatuste ajalugu
git log --oneline -- ee/22711.md

# Loe Eesti Vabariigi põhiseadust (kõige uuem versioon)
less ee/24304.md

# Vaata, mis muutus konkreetse reformiga
git show <commit-sha> -- ee/<seaduse-id>.md
```

## Struktuur

```
ee/
  {globaalID}.md         — iga konsolideeritud seadus üks fail
  ...
```

Failistruktuur on **lame** — üks kataloog riigi kohta, ilma alamkataloogideta. Dokumendi liik (`seadus`, `määrus`) on iga faili YAML-päises.

Failinimi on iga seadusgrupi (`terviktekstiGrupiID`) **kõige vanem `globaalID`**, mis tagab failinime stabiilsuse läbi kõigi reformide.

## Vorming

Iga fail on Markdown koos YAML-päisega:

```yaml
---
title: "Eesti Vabariigi põhiseadus"
identifier: "24304"
country: "ee"
rank: "seadus"
publication_date: "2002-06-01"
last_updated: "2026-04-07"
status: "in_force"
source: "https://www.riigiteataja.ee/akt/24304"
department: "Rahvahääletusel vastu võetud"
short_title: "PS"
text_type: "terviktekst"
adoption_date: "1992-06-28"
original_effective_date: "1992-07-03"
original_publication: "RT, 1992, 26, 349"
rt_section: "RT I"
group_id: "151381"
schema: "tyviseadus_1_10.02.2010.xsd"
---
# Eesti Vabariigi põhiseadus

Kõikumatus usus ja vankumatus tahtes...

### I. peatükk ÜLDSÄTTED

##### § 1.

(1) Eesti on iseseisev ja sõltumatu demokraatlik vabariik...
```

Hierarhia kaardistus:

| Eesti | Markdown |
|---|---|
| `osa` (osa) | `## N. osa PEALKIRI` |
| `peatykk` (peatükk) | `### N. peatükk Pealkiri` |
| `jagu` (jagu) | `#### N. jagu Pealkiri` |
| `paragrahv` (paragrahv §) | `##### § N. Pealkiri` |
| `loige` (lõige) | `(N) tekst` |
| `punkt` / `alampunkt` | `N) tekst` |

## Commit'ide ajalugu = õiguslik ajalugu

Iga commit vastab konkreetsele konsolideeritud versioonile, mis avaldati Riigi Teatajas. Commit'i kuupäev on `kehtivuseAlgus` — kuupäev, mil see versioon jõustus.

```bash
# Vaata seaduse 24304 täielikku ajalugu (Põhiseadus)
git log --format="%ai %s" -- ee/24304.md

# Vaata, mis muutus konkreetse reformiga
git show <commit-sha> -- ee/24304.md

# Vaata kahe versiooni vahelist erinevust
git diff <commit1>..<commit2> -- ee/24304.md
```

## Andmed

Kõik andmed pärinevad Riigi Teataja avaandmete portaalist (`avaandmed/ERT/`), mis avaldatakse iga päev XML-vormingus. Genereerimispipeline on saadaval [legalize-dev/legalize-pipeline](https://github.com/legalize-dev/legalize-pipeline).

**Eesti seadusandlus on autoriõigusega kaitsmata** Eesti Autoriõiguse seaduse alusel ja on avalikus omandis. Tasuta taaskasutus on lubatud.

## Litsents

MIT — vt [LICENSE](LICENSE)

Käesolevas hoidlas sisalduvad seadusetekstid ise on **avalik omand** Eesti seaduse järgi.
