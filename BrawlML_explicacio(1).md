# BrawlML: Brawler Markup Language

**Un llenguatge de marques per definir personatges (brawlers) del joc Brawl Stars**

> Pràctica 1 de LMSGI: Característiques dels llenguatges de marques

---

## 1\. Introducció als llenguatges de marques

### 1.1 Què és un llenguatge de marques?

Un **llenguatge de marques** (*markup language*) és un llenguatge que combina el **contingut** d'un document amb **marques** (etiquetes o anotacions) que n'indiquen l'estructura, el significat o la forma de presentació. Les marques s'escriuen dins del mateix text i permeten que un programa sàpiga com ha d'interpretar cada part.

No és un llenguatge de programació: no executa instruccions ni té lògica (bucles, condicions, etc.). Només **descriu** informació.

### 1.2 Orígens

| Any | Nota |
| :---- | :---- |
| Anys 60 | El terme *markup* ve del món de l'edició: els editors marcaven els manuscrits amb anotacions per indicar a la impremta com s'havien de composar. |
| 1969 | **GML** (Generalized Markup Language), creat per Charles Goldfarb, Edward Mosher i Raymond Lorie a IBM. |
| 1986 | **SGML** (Standard Generalized Markup Language), estàndard ISO 8879\. És el pare dels llenguatges de marques moderns. |
| 1991 | **HTML**, creat per Tim Berners-Lee al CERN per compartir documents a la web. |
| 1998 | **XML** (eXtensible Markup Language), recomanació del W3C: una versió simplificada de SGML. |
| Actualitat | Derivats i altres formats: XHTML, SVG, RSS, KML, Markdown, YAML (en un sentit més ampli), etc. |

1.3 Per a què es fan servir?

- **Estructurar la informació** (documents, dades, configuracions).  
- **Separar contingut i presentació**: les dades són una cosa i com es mostren és una altra.  
- **Intercanviar dades** entre aplicacions i sistemes diferents.  
- **Facilitar el processament automàtic** per part de programes.  
- **Representar dades de manera que les entenguin tant persones com màquines**.

### 1.4 Característiques comunes

1. Es basen en **text pla**, de manera que es poden editar amb qualsevol editor de text.  
2. Utilitzen **metadades**: informació sobre les dades (*dades sobre les dades*).  
3. Faciliten el **processament automàtic** de les dades.  
4. Faciliten la **representació o interpretació** de les dades emmagatzemades.

---

## 2\. Introducció a BrawlML i regles que hem definit

### 2.1 Què és BrawlML?

**BrawlML** (*Brawler Markup Language*) és un llenguatge de marques propi pensat per **descriure un brawler** (personatge) del joc *Brawl Stars*: les seves estadístiques, el seu atac, la seva súper, els gadgets, les star powers, etc.

Per fer-lo més proper al joc, **totes les etiquetes, els atributs i els valors del llenguatge estan en anglès**, igual que els termes que fa servir Brawl Stars (*brawler*, *super*, *gadget*, *star power*, *hypercharge*…).

Un fitxer BrawlML és un fitxer de **text pla** que un programa pot llegir per:

- validar que el personatge està ben definit,  
- generar una **fitxa visual** (carta, pàgina web, PDF),  
- comparar brawlers entre ells,  
- guardar-los en una base de dades.

### 2.2 Estructura general

```
<brawler atributs>
    <stats> ... </stats>
    <attack .../>
    <super .../>
    <gadget .../>          (opcional)
    <starpower .../>       (opcional)
    <hypercharge .../>     (opcional)
    <balance .../>         (opcional)
</brawler>
```

Cada fitxer descriu **un únic brawler**. L'element arrel és sempre `<brawler>`.

### 2.3 Regles de sintaxi

| \# | Regla |
| :---- | :---- |
| 1 | El fitxer és **text pla** codificat en UTF-8. |
| 2 | Tot s'escriu amb **etiquetes** entre `<` i `>`. |
| 3 | Les etiquetes d'obertura (`<health>`) han de tenir la seva etiqueta de tancament (`</health>`), excepte les etiquetes **autotancades**, que acaben en `/>` (per exemple `<attack .../>`). |
| 4 | Les etiquetes han d'estar **correctament** **obertes i tancades:** `<a><b></b></a>` és correcte; `<a><b></a></b>` no. |
| 5 | Els noms d'etiqueta i d'atribut s'escriuen en **anglès i en minúscules**, sense accents ni espais. El llenguatge distingeix majúscules i minúscules (*case sensitive*). |
| 6 | Els **atributs** s'escriuen com `name="value"` i el valor sempre va entre **cometes dobles**. |
| 7 | Els **comentaris** són línies que comencen per `#`. El programa les ignora. |
| 8 | Hi ha **un únic** element `<brawler>` per fitxer. |
| 9 | Els espais i salts de línia entre etiquetes no tenen significat (la sagnia és només per a llegibilitat). |

2.4 Elements del llenguatge

#### `<brawler>`: element arrel (obligatori)

Conté tot el personatge. **Els seus atributs són les metadades del document.**

| Atribut | Obligatori | Descripció | Valors |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nom del brawler | Text, 2 a 20 caràcters |
| `rarity` | Sí | Raritat | `Starting`, `Rare`, `Super Rare`, `Epic`, `Mythic`, `Legendary` |
| `class` | Sí | Rol al joc | `Damage Dealer`, `Tank`, `Marksman`, `Artillery`, `Controller`, `Assassin`, `Support` |
| `author` | No | Qui l'ha creat | Text |
| `version` | No | Versió de la definició | Format `número.número` (ex. `1.0`) |

#### `<stats>`: obligatori

Agrupa les característiques bàsiques. Conté elements amb **text com a contingut**.

| Element | Obligatori | Descripció | Rang |
| :---- | :---: | :---- | :---- |
| `<health>` | Sí | Punts de vida | 1 – 20000 |
| `<speed>` | Sí | Velocitat de moviment | 1 – 1000 |
| `<reload>` | No | Temps de recàrrega de munició, en ms | 100 – 10000 |

&nbsp;

```
<stats>
    <health>3200</health>
    <speed>720</speed>
</stats>
```

#### `<attack>`: obligatori (autotancat)

Defineix l'atac bàsic.

| Atribut | Obligatori | Descripció | Valors |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nom de l'atac | Text |
| `damage` | Sí | Dany per impacte | Enter, 1 – 10000 |
| `range` | Sí | Distància en rajoles | Enter, 1 – 20 |
| `type` | Sí | Forma de l'atac | `projectile`, `burst`, `melee`, `thrown` |

#### `<super>`: obligatori (autotancat)

Defineix l'habilitat especial.

| Atribut | Obligatori | Descripció | Valors |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nom de la súper | Text |
| `damage` | Sí | Dany de la súper | Enter, 0 – 20000 (0 si no fa dany) |
| `radius` | No | Radi de l’atac. | Enter, 1 – 10 |

`<gadget>`: opcional (màxim 2\)

| Atribut | Obligatori | Descripció |
| :---- | :---: | :---- |
| `name` | Sí | Nom del gadget |
| `effect` | Sí | Descripció de l'efecte |

#### `<starpower>`: opcional (màxim 2\)

Mateixos atributs que `<gadget>` (`name`, `effect`).

#### `<hypercharge>`: opcional (màxim 1\)

Mateixos atributs que `<gadget>` (`name`, `effect`).

#### `<balance>`: opcional (autotancat)

Puntuació orientativa de 1 a 10 per valorar el personatge.

| Atribut | Obligatori | Descripció | Valors |
| :---- | :---: | :---- | :---- |
| `damage` | Sí | Atac | Enter, 1 – 10 |
| `defense` | Sí | Resistència | Enter, 1 – 10 |
| `mobility` | Sí | Facilitat de moviment | Enter, 1 – 10 |

2.5 Regles semàntiques (de contingut)

1. Tots els valors numèrics han de ser **enters positius**, sense decimals ni signe.  
2. Cada valor numèric ha de ser dins del **rang** indicat.  
3. Els atributs `rarity`, `class` i `type` només admeten els **valors de la llista**.  
4. Les seccions obligatòries (`stats`, `attack`, `super`) han d'aparèixer **exactament una vegada**.  
5. No es poden repetir elements no permesos (per exemple, tres `<gadget>`).  
6. L'ordre dels elements dins de `<brawler>` és lliure.

### 2.6 Taula de traducció (per si us pregunten)

| Concepte | Etiqueta / atribut |
| :---- | :---- |
| Personatge | `brawler` |
| Estadístiques | `stats` |
| Vida | `health` |
| Velocitat | `speed` |
| Recàrrega | `reload` |
| Atac | `attack` |
| Dany | `damage` |
| Abast | `range` |
| Radi | `radius` |
| Hipercàrrega | `hypercharge` |
| Equilibri | `balance` |
| Defensa / Mobilitat | `defense` / `mobility` |
| Raritat / Classe | `rarity` / `class` |
| Autor / Versió | `author` / `version` |

---

## 3\. Classificació de BrawlML

Els llenguatges de marques es poden classificar segons el que expressen les seves marques:

| Tipus | Què descriu | Exemple |
| :---- | :---- | :---- |
| **Procedimental / de presentació** | *Com* s'ha de veure o tractar el contingut (negreta, mida, color…) | RTF, LaTeX bàsic, HTML antic (`<b>`, `<font>`) |
| **Descriptiu / semàntic** | *Què* és cada part del contingut, sense dir com es mostra | XML, HTML5 semàntic (`<article>`, `<nav>`) |

### Classificació: **descriptiu / semàntic**

#### Justificació

- Les etiquetes diuen **què és** cada dada: `<health>` és la vida, `<attack>` és l'atac, `<super>` és la súper. No diuen si s'han de mostrar en vermell, en negreta o a 14 punts.  
- El fitxer no conté **cap instrucció de presentació**. El mateix fitxer `.bml` es pot convertir en una carta de joc, una pàgina web, un PDF o una fila d'una base de dades, segons el programa que el llegeixi.  
- La **forma de mostrar** les dades és responsabilitat del programa (o d'una plantilla), no del document.  
- Això permet **reutilitzar** les mateixes dades en molts contextos diferents.

---

## 4\. Cas d'ús en què BrawlML és adequat

### Editor de brawlers de la comunitat (fan-made)

Imagina una web on els jugadors poden crear els seus propis brawlers i compartir-los.

**Com ajuda BrawlML:**

1. L'usuari escriu o genera el fitxer `.bml` del seu personatge.  
2. El programa **valida** el fitxer: comprova que té les seccions obligatòries i que els valors són dins dels rangs.  
3. El programa genera automàticament una **carta visual** amb el nom, les estadístiques i les habilitats.  
4. Els personatges es guarden en una base de dades i es poden **comparar** o ordenar per raritat, vida, dany, etc.

**Per què és adequat:**

- Les dades són **estructurades i previsibles**, de manera que un programa les processa fàcilment.  
- Cada brawler segueix el mateix format, així que és fàcil crear **llistes, rànquings i filtres**.  
- És **text pla**: es pot editar amb el Bloc de notes i compartir per xat o per correu.  
- Separa les dades del disseny: es pot canviar l'aspecte de les cartes sense tocar els fitxers.  
- Els noms en anglès coincideixen amb els del joc, així que qualsevol jugador entén el fitxer.

Altres usos adequats: documentar un joc propi, equilibrar personatges d'un mod, generar automàticament una guia o una wiki.

---

## 5\. Exemple d'ús

### 5.1 Fitxer `ember.bml`

```
# Definition of the brawler Ember
# Created by Group 3

<brawler name="Ember" rarity="Epic" class="Marksman" author="Group3" version="1.0">

    <stats>
        <health>3200</health>
        <speed>720</speed>
        <reload>1400</reload>
    </stats>

    <attack name="Cinder" damage="900" range="8" type="projectile"/>

    <super name="Eruption" damage="1500" radius="3"/>

    <gadget name="Thick Smoke" effect="Hides the brawler for 3 seconds"/>

    <starpower name="Eternal Flame" effect="Burns enemies for 200 damage per second"/>

    <balance damage="8" defense="4" mobility="6"/>

</brawler>
```

### 5.2 Explicació línia per línia

| Fragment | Explicació |
| :---- | :---- |
| `# Definition...` | Comentaris. El programa els ignora. |
| `<brawler name="Ember" rarity="Epic" ...>` | Obre l'element arrel. Els atributs són les **metadades**: nom, raritat, classe, autor i versió. |
| `<stats>...</stats>` | Secció amb tres elements de text: health 3200, speed 720 i reload 1400 ms. |
| `<attack name="Cinder" .../>` | Atac bàsic autotancat: fa 900 de dany, abast de 8 rajoles i és de tipus projectil. |
| `<super name="Eruption" .../>` | Súper que fa 1500 de dany en un radi de 3 rajoles. |
| `<gadget .../>` | Gadget opcional amb el seu nom i efecte. |
| `<starpower .../>` | Star power opcional. |
| `<balance .../>` | Valoració opcional de 1 a 10 en dany, defensa i mobilitat. |
| `</brawler>` | Tanca l'element arrel. |

### 5.3 Exemple d'errors

| Fitxer incorrecte | Error |
| :---- | :---- |
| `<health>3200` (sense tancar) | Etiqueta no tancada (viola la regla 3). |
| `<attack name=Cinder damage=900 .../>` | Atributs sense cometes dobles (regla 6). |
| `<brawler name="Ember" class="Mage" ...>` | `Mage` no és un valor permès per `class` (regla semàntica 3). |
| `<health>-500</health>` | Valor fora de rang i negatiu (regles semàntiques 1 i 2). |
| Fitxer sense `<super>` | Falta una secció obligatòria (regla semàntica 4). |
| `<Health>3200</Health>` | Majúscules en el nom d'etiqueta (regla 5). |

&nbsp;

---

## 6\. Cas d'ús en què BrawlML NO seria el més adequat.

### Descriure animacions, models 3D o comportament en temps real

BrawlML **no és adequat** per definir:

- **Animacions** del personatge (moviment de braços, efectes de partícules…).  
- **Models 3D, textures o sprites**.  
- **Lògica del joc**: com es comporta la IA, què passa en cas de col·lisió, com es calcula el dany segons condicions especials.  
- **Comportament dinàmic** (per exemple, "si la vida baixa del 30 %, augmenta la velocitat").

**Per què no és adequat:**

- BrawlML només **descriu dades estàtiques**: no té condicions, bucles ni variables.  
- No pot expressar **lògica ni càlculs**. Per això caldria un llenguatge de programació (Python, C\#, Lua…) o un motor de joc (Unity, Godot).  
- No pot representar **dades binàries** (imatges, models, sons) de manera eficient, perquè és text pla.  
- Per a **grans volums de dades** (milers de brawlers amb moltes estadístiques), un format com JSON o una base de dades seria més eficient.

---

## 7\. Justificació: per què BrawlML és un llenguatge de marques

Es compleixen els **quatre requisits** de l'enunciat:

| Requisit | Com ho compleix BrawlML |
| :---- | :---- |
| **Es basa en text pla** | Els fitxers `.bml` són text pla UTF-8 que es pot obrir i editar amb qualsevol editor de text (Bloc de notes, VS Code, etc.). No hi ha cap format binari. |
| **Fa ús de metadades** (no és metallenguatge) | Les etiquetes i els atributs són **dades sobre les dades**: `rarity="Epic"`, `class="Marksman"`, `author="Group3"` o `version="1.0"` expliquen què és i d'on ve el contingut. Això són metadades, no un metallenguatge (no s'utilitza BrawlML per definir altres llenguatges). |
| **Facilita el processament automàtic** | La sintaxi és estricta i previsible (etiquetes tancades, valors entre cometes, rangs definits), de manera que un programa pot llegir, validar i extreure les dades sense ambigüitats. |
| **Facilita la representació / interpretació** | Les etiquetes tenen noms comprensibles (`<health>`, `<attack>`, `<super>`), així que una persona les entén d'un cop d'ull. Un programa pot convertir les mateixes dades en una carta, una pàgina web o un PDF. |

A més, és un llenguatge **descriptiu**: marca el significat de cada part del contingut i separa clarament **les dades** de **la presentació**, que és l'essència dels llenguatges de marques.

---

## Resum per a la presentació

| \# | Apartat de l'enunciat | Contingut clau |
| :---- | :---- | :---- |
| 1 | Intro als llenguatges de marques | GML → SGML → HTML → XML. Serveixen per estructurar i intercanviar dades. |
| 2 | Intro a BrawlML i regles | Llenguatge per definir brawlers, amb etiquetes en anglès. Etiquetes, atributs, 9 regles de sintaxi i 6 de contingut. |
| 3 | Classificació | **Descriptiu/semàntic**: diu què és cada dada, no com es veu. |
| 4 | Cas d'ús adequat | Editor i galeria de brawlers de la comunitat. |
| 5 | Exemple | Fitxer `ember.bml` explicat per parts. |
| 6 | Cas d'ús no adequat | Animacions, models 3D o lògica de joc. |
| 7 | Justificació | Compleix els 4 requisits: text pla, metadades, processable i interpretable. |

&nbsp;