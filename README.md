# BelGrammarDB

## About the project
This project is concerned with the conversion of the existing [bnkorpus.info] database of Belarusian grammar into a more lightweight searchable format that is compatible with [Universal Dependencies](https://universaldependencies.org/) markup. When possible, the markup adheres to [Belarusian-HSE treebank](https://universaldependencies.org/treebanks/be_hse/index.html) format, but additional features were introduced based on parameters available in the source data and UD conventions. The data was collected from the source material without modifications except morphological tag conversion and exclusion of optional . The initial markup 

Data:
- BNCorpus repository with documentation: [GrammarDB on GitHub↗︎](https://github.com/Belarus/GrammarDB)
- Source files in XML: [RELEASE-202309 on GitHub↗︎](https://github.com/Belarus/GrammarDB/releases)
- Accompanying publication with markup description (BE): [Граматычная база беларускай мовы / уклад. Уладзімір Кошчанка, Алесь Булойчык↗︎](https://www.academia.edu/60297156/%D0%93%D1%80%D0%B0%D0%BC%D0%B0%D1%82%D1%8B%D1%87%D0%BD%D0%B0%D1%8F_%D0%B1%D0%B0%D0%B7%D0%B0_%D0%B1%D0%B5%D0%BB%D0%B0%D1%80%D1%83%D1%81%D0%BA%D0%B0%D0%B9_%D0%BC%D0%BE%D0%B2%D1%8B_Belarusian_Language_Grammar_Database)

Universal Dependencies Markup:
- [Universal features](https://universaldependencies.org/u/feat/)
- [UD for Belarusian](https://universaldependencies.org/be/index.html)
- [Belarusian-HSE treebank](https://universaldependencies.org/treebanks/be_hse/index.html)

## Database structure
![BDschema](BDschema.png "BDschema")

The database has three tables that correspond to the structure of original XML: 
- `Lemma` - The list of paradigms (words) identified by their lemma (initial, dictionary form) 
- `Variant` - The list of variants of a paradigm, if there are alternative spellings. Most lemmas have 1 variant, 1248 lemmas have 2 variants, 33 lemmas have 3 variants, 3 lemmas have 4 variants, 2 lemmas have 5 variants (numbers shown for the initial data exporation). All forms belong to variants.
- `Form` - The list of word forms.

### `Lemma` table

Columns:
- `ID INTEGER PRIMARY KEY` - Mandatory
- `RefID INTEGER` - The original ID in BNCorpus XML, mandatory
- `Lemma TEXT` - Mandatory
- `POS TEXT` - Part of speech, mandatory:
    - `ADJ` - adjective {Прыметнік}
    - `CCONJ` - conjunction {злучнік}
    - `PART` - particle {часціца}
    - `ADP` - preposition (adposition) {прыназоўнік}
    - `NUM` - numeral {лічэбнік}
    - `NOUN` - noun {агульны назоўнік}
    - `PROPN` - proper noun {уласны назоўнік}
    - `VERB` - verb {дзеяслоў}
    - `ADV` - adverb {прыслоўе} 
    - `PRON` - pronoun {займеннік}
   - `INTJ` - interjection {выклічнік}
- `Type TEXT` - {Тып} - Subtypes within a part of speech 
    - ADJ:
        - `Qlt` - qualitative {якасны}
        - `Rel` - relative {адносны} 
        - `Pssv` - possesive {прыналежны}
    - CCONJ:
        - `Crd`- coordinating {злучальны}
        - `Sub` - subordinating {падпарадкавальны}
    - NUM:
        - `Card` - cardinal {колькасны}
        - `Ord` - ordinal {парадкавы}
        - `Col` - collective {зборны}
        - `Frac` - fraction {дробавы}
    - PRON:
        - `Prs` - personal, reflexive, possessive {асабовы, зваротны, прыналежны}, also see `Poss` & `Reflex` features
        - `Rcp` - reciprocal/reflexive {зваротны}
        - `Dem` - demonstrative {указальны}
        - `Emp` - emphatic/intensive {азначальны}
        - `Int` - interrogative-relative {пытальна–адносны}
        - `Neg` - negative {адмоўны}
        - `Ind` - indefinite {няпэўны}
- `InflClass TEXT` - {Словазмяненне} - Inflection type specific to POS
    - ADJ:
        - `Ind` - indeclinable {нескланяльны}
    - NUM:
        - `Ntype` - noun-like {як у назоўніка} 
        - `Atype` - adjective-like {як у прыметніка}
        - `Ind` - indeclinable {нескланяльны}
    - NOUN {Скланенне}:
        - `1d` - 1 substantive declension {1 скланенне}
        - `2d` - 2 substantive declension {2 скланенне}
        - `3d` - 3 substantive declension {3 скланенне}
        - `Atype` - adjective-type {ад’ектыўны тып скланення}
        - `Ind` - indeclinable {нескланяльны}
        - `Com` - combined {рознаскланяльны}
        - `Mix` - mixed {змешаны тып скланення}
    - PRON:
        - `Ntype` - noun-like {як у назоўніка} 
        - `Atype` - adjective-like {як у прыметніка}
        - `Ind` - indeclinable {нескланяльны}
    - VERB {Спражэнне}:
        - `1c` - 1 conjugation type {1 спражэнне}
        - `2c` - 2 conjugation type {2 спражэнне}
        - `Com` - combined type {рознаспрагальны} 
- `Degree TEXT` - {Ступень параўнання} - Degree of comparison:
    - ADJ:
        - `Pos` - positive {станоўчая}
        - `Cmp` - comparative {вышэйшая}
        - `Sup` - superlative {найвышэйшая}
- `Person INTEGER` - {Асоба} - Person for PRON
    - `0` - impersonal
    - `1` - 1st person
    - `2` - 2nd person
    - `3` - 3rd person
- `Gender TEXT` - {Род} - Gender for NOUN
    - `Fem`- feminine {жаночы}
    - `Masc` - masculine {мужчынскі}
    - `Neut` - neutral {ніякі}
    - `Com` - common {агульны}
- `Voice TEXT` {Стан} VERB voice
    - `Act` - active {незалежны}
    - `Pass` - passive {залежны}
- `Tense TEXT` - {Час} - Tense for participle (VERB, VerbForm == Part)
    - `Past` - past {прошлы}
    - `Pres` - present {цяперашні}
- `Aspect TEXT` {Трыванне} VERB aspect
    - `Imp` - imperfect {незакончанае}
    - `Perf` - perfect {закончанае}
- `Animacy INTEGER` - {Адушаўлёнасць} - Animacy for NOUN
    - `1` - True
    - `0` - False
- `Abbr INTEGER` - {Скарачэнне} - Abbreviation for NOUN
    - `1` - True 
    - `0` - False
- `NumForm TEXT` {Форма} - NUM type by complexity, not in HSE treebank
    - `Sim` - simple {просты}
    - `Com` - complex {складаны}
- `VerbForm TEXT` {Форма} - VERB form
    - `Part` - {дзеепрыметнік} - participle
- `Personal TEXT` - {Асабовасць} - Personal/impersonal NOUN, not in HSE features
    - `Per` - personal {асабовы}
    - `Imp`- impersonal {неасабовы}
- `Origin TEXT` - {Спосаб утварэння} - ADV derivation pattern  - not in UD 
    - `Adj` - from adjective {ад прыметнікаў}
    - `Conv`- from converb {ад дзеепрыслоўяў}
    - `Num` - from numeral {ад лічэбнікаў}
    - `Noun` - from noun {ад назоўнікаў}
    - `Pron` - from pronoun {ад займеннікаў}
    - `Verb` - from verb {ад дзеясловаў}
- `Poss INTEGER` - {прыналежны} - possessive PRON
    - `1` - True
    - `0` - False
- `Reflex INTEGER` - {зваротны} - reflexive PRON, VERB
    - `1` - True
    - `0` - False
- `SubCat TEXT` - {Пераходнасць} - VERB transitivity 
    - `Intr` - intransitive {непераходны}
    - `Tran` - transitive {пераходны} 

### `Variant` table

Columns:
- `ID INTEGER PRIMARY KEY` 
- `LemID INTEGER` - Lemma ID, `FOREIGN KEY (LemID) REFERENCES Lemma (ID)`
- `RefID INTEGER` - Variant ID.  Variants are identified by their original alphabetical ID: `a`, `b`, etc.
- `Sources TEXT` - The list of dictionaries listed on the entry, comma separated (see [GrammarDB README](https://github.com/Belarus/GrammarDB)). My mapping (**not verified!**):
    - `tsbm1984` - «Тлумачальны слоўнік беларускай мовы. У 5 т.» (1984)
    - `biryla1987` - «Слоўнік беларускай мовы (пад. рэд. М.В. Бірылы)» (1987)
    - `nazounik2008` - «Граматычны слоўнік назоўніка» (2008)
    - `dzsl2007` - «Граматычны слоўнік дзеяслова» (2008)
    - `prym2009` - «Граматычны слоўнік прыметніка, займенніка, лічэбніка, прыслоўя» (2008)
    - `krapivabr2012` - «Руска-беларускі слоўнік. У 3 т.» (2011)
    - `krapivabr2012` - «Беларуска-рускі слоўнік. У 3 т.» (2012)
    - `piskunou2012` - «Вялікі слоўнік беларускай мовы: арфаграфія, акцэнтуацыя, парадыгматыка (каля 223 000 слоў)» (2012)
    - `sbm2012` - «Слоўнік беларускай мовы» (2012)
    - `nazounik2013` - «Граматычны слоўнік назоўніка» (2013)
    - `dzsl2013` - «Граматычны слоўнік дзеяслова» (2013)
    - `prym2013` - «Граматычны слоўнік прыметніка, займенніка, лічэбніка, прыслоўя» (2013)
    - `tsblm1996` - «Тлумачальны слоўнік беларускай літаратурнай мовы» (2016)
- `Ortho TEXT` - {правапіс} - The list of orthographic conventions, comma separated:
    - `A1957` - «Правілы беларускай арфаграфіі і пунктуацыі» (1959)
    - `A2008` - «Правілы беларускай арфаграфіі і пунктуацыі» (2008)

    
### `Form` table

Columns:
- `ID INTEGER PRIMARY KEY`
- `LemID INTEGER` - Lemma ID, `FOREIGN KEY (LemID) REFERENCES Lemma (ID)`
- `VarID INTEGER` - Variant ID, `FOREIGN KEY (VarID) REFERENCES Variant (ID)`
- `Form TEXT` - Form without word stress
- `Sources TEXT` - Same as in `Variant`
- `Ortho TEXT` - Same as in `Variant`
- `Accent TEXT` - Word stress position in string format. E.g. `Accent == 6` for `Form == кацяня` means that the 6th character of the form is stressed as in _кацян**я́**_
    - For simple words:
        - Accent mark is detected: One numeric value is provided.
        - Accent mark is not detected: `NULL`
    - For compound hyphenated words:
        - Accent marks of all parts are detected: Multiple numeric values are provided as a comma separated list corresponding to the number of parts. 
        - The accent marks of some parts are not detected: Multiple numeric values are provided as a comma separated list corresponding to the number of parts, with `0` values for the parts with undetected stress.
        - No accent marks were detected in any
         parts: `NULL`
- `Gender TEXT` - {Род} - Gender for ADJ, NUM, NOUN, PRON, VERB 
    - `Fem`- feminine {жаночы}
    - `Masc` - masculine {мужчынскі}
    - `Neut` - neutral {ніякі}
- `Person INTEGER` - {Асоба} - Person for VERB
    - `0` - impersonal
    - `1` - 1st person
    - `2` - 2nd person
    - `3` - 3rd person
- `Cas TEXT` - {Склон} - Case for ADJ, NUM, NOUN, PRON, participle (VERB, VerbForm == Part)
    - `Acc` - Accusative {Вінавальны}
    - `Dat` - Dative {Давальны}
    - `Gen` - Genitive {Родны}
    - `Ins` - Instrumental {Творны}
    - `Loc` - Locative {Месны}
    - `Nom` - Nominative {Назоўны}
- `Number TEXT` - {Лік} - Number for ADJ, NUM, NOUN, PRON, VERB 
    - `Sing` - singular {адзіночны}
    - `Plur` - plural {множны}
- `Degree TEXT` - {Ступень параўнання} - Degree for ADV
    - `Pos` - positive {станоўчая}, `tag = "P"`
    - `Cmp` - comparative {вышэйшая}, `tag = "C"`
    - `Sup` - superlative {найвышэйшая}, `tag = "S"`
- `Tense TEXT` - {Час} - Tense for VERB
    - `Past` - past {прошлы}
    - `Pres` - present {цяперашні}
    - `Fut` - future {будучы}
- `Mood TEXT` - {Лад} - Mood for VERB
    - `Imp` - imperative {загадны}
- `VerbForm TEXT` - {Форма} - VERB form
    - `Inf` - infinitive {Інфінітыў}
    - `Conv` - converb/transgressive {дзеепрыслоўе}
- `Animacy INTEGER` - {Адушаўлёнасць} - Animacy option for Accusative case for ADJ, NOUN, NUM, PRON, participle (VERB, VerbForm == Part)
    - `1` - True
    - `0` - False
- `Short INTEGER` - {Кароткая форма} - Short form of participle (VERB, VerbForm == Part)
    - `1` - True 
    - `0` - False