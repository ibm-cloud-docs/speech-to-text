---

copyright:
  years: 2021, 2024
lastupdated: "2024-08-23"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

## Smart formatting Version 2
{: #smart-formatting}

The new version of smart formatting feature is available for US English, Brazilian Portuguese, French and German. It is also available for the `en-WW_Medical_Telephony` model when US English audio is recognized. The new version provides more flexibility in adding new languages and patterns compared to older smart formatting. The newer version uses a more sophisticated machine learning technique (Weighted Finite State Transducers) to identify entities in texts compared to the older version which was rule-based approach. The new approach provides more accurate entity classification and formatting and also adds capability to define hierarchies using weights when same text can be identified as two different entity type.

The `smart_formatting` feature directs the service to convert the following strings into more conventional representations:

-   Dates and times
-   Integers, decimals, ordinals
-   Alphanumeric sequences (of length > 2)
-   Phone numbers
-   Currency values
-   Measures (`/km²`, `kg`, `mph`, `m³`, etc)
-   Emails, URLs and IP addresses
-   Credit Card Numbers (formatted as groups of 4 digits)
-   Punctuations (as spoken in dictations)

To use the new smart formatting feature for US English, Brazilian Portuguese, French and German; set the parameter smart_formatting=true and smart_formatting_version=2.

### Entity Patterns and Examples
{: #smart-formatting-differences}

#### US English
{: #smart-formatting-english}

-   Different spoken forms of dates are accepted, including dates just as numbers or names of months and use of `the` and `of` (`the twenty fifth of july twenty twelve`). The dates are formatted as `m/d/yyyy`. 
-   Times are identified by keywords or suffix e.g. timezones (e.g. `est`, `eastern`), `am`, `pm`, `hours`, `o'clock`, `minutes past hour`.
-   Phone numbers must be either `911` or a number that contains 10 digits and/or starts with the number `[+]1`.
-   Currency symbols are substituted for strings in appropriate contexts e.g. `dollar`, `cent`, `euro`, `yen`. `cent` is optional after `dollar` e.g. `twelve dollars twenty five` and `twelve dollars twenty five cents` formatted as `$12.25`
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ at [alphanumeric dot]+ domainname` ) are smart formatted.
-   Web URLs, both short and long form, are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`)
-   Most large integer are formatted as numeric sequences. When large number (million, billions, etc) are spoken with a single group integers, quantity word `million/billion` is not converted for readability e.g. `fifty nine million` -> `59 million` but when the number is more complex, its formatted as numeric digits e.g. `fifty nine million and one` -> `59000001`
-   Numbers below 10 are not converted to digit to avoid odd formatting e.g. `You are one of them` -> `You are 1 of them`. But in other context like expressing currency, they are converted appropriately e.g. `Give me one dollar` -> `Give me $1`
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `comma` (`,`), `period` (`.`), `question mark` (`?`), `exclamation point` (`!`), `semicolon` (`;`), `hyphen` (`-`), etc.

### Smart formatting Examples
{: #smart-formatting-examples-en}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                                      | With smart formatting            |
|-----------------|-------------------------------------------------------------------------------|----------------------------------|
| Dates           | july twenty fifth two thousand twelve                                         | 7/25/2012                        |
|                 | the twenty fifth of july twenty twelve                                        | 7/25/2012                        |
|                 | january the thirty first two thousand                                         | 1/31/2000                        |
|                 | zero five zero five nineteen eighty three                                     | 6/20/2014                        |
|                 | second quarter of twenty twenty two                                           | Q2 2022                          |
| Times           | it is two eleven eastern                                                      | it is 02:11 est                  |
|                 | we begin at oh seven hundred hours                                            | we begin at 07:00                |
|                 | quarter past one                                                              | 01:15                            |
|                 | three o'clock                                                                 | 03:00                            |
| Numbers         | The quantity is one million one hundred and one                               | The quantity is 1000101          |
|                 | One point five is between one and two                                         | 1.5 is between 1 and 2           |
|                 | It would cost five point two million                                          | It would cost 5.2 million        |
|                 | Its one hundred twenty first trial                                            | Its 121st trial                  |
| Phone numbers   | nine one four five five six eight three three one                             | 914-556-8331                     |
|                 | plus one nine two three one two three five six seven eight                    | +1 923-123-5678                  |
| Currency values | You owe me four united states dollars and sixty nine cents                    | You owe me $4.69                 |
|                 | seventy five dollars sixty three                                              | $75.63                           |
|                 | Dollar rose to one hundred and nine point seven nine yen                      | Dollar rose to ¥109.79           |
| Email, URL, IP  | I saw the story on w w w dot yahoo dot com                                    | I saw the story on www.yahoo.com |
|                 | a b three hyphen s d d dash three at g mail dot com                           | ab3-sdd-3@gmail.com              |
|                 | h t t p colon slash slash w w w dot c o m d a i l y n e w s dot a b slash s m | http://www.comdailynews.ab/sm    |
|                 | two two five dot double five dot o dot forty five                             | 225.55.0.45                      |
| Measures        | two hundred kilometers per hour                                               | 200 km/h                         |
|                 | two kilo watt hours                                                           | 2 kWh                            |
| Sequences       | H F H nine nine three dot seven B                                             | HFH993.7B                        |
|                 | a ten eighty p display                                                        | a 1080p display                  |
{: caption="Smart formatting example transcripts"}

#### Brazilian-Portuguese
{: #smart-formatting-pt-br}

-   For dates, `do` and `de` in the transcript is used as separators for day, month and year. `primeiro` is considered as 1st of the month. The dates are formatted as `DD/MM/YYYY`. 
-   Times are identified by keywords and prefix e.g. `às` `ao`, `à`, `da tarde` (`p.m.`), `da madrugada` (`a.m.`), `meia noite`, `meio dia`. Prefixes  `às` `ao`, `à` are optional.
-   Landline numbers must have 10 digits (2 digits country code and 8 digits number), mobile numbers are 9 digits with first digit as `9` with optional country code. Area codes are optional. Numbers are formatted as `+NN (NN) NNNN-NNNN` and `+NN (NN) 9NNNN-NNNN`.
-   Brazilian real currency symbol is `R$`. Other Currency symbols are substituted for strings in appropriate contexts e.g. `dollar`, `cent`, `euro`, `yen`. `centavos` is optional after `reais` e.g. `setenta e cinco dólares e sessenta e três` and `setenta e cinco dólares e sessenta e três centavos` formatted as `R$75,63`
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ arroba [alphanumeric ponto]+ domainname` ) are smart formatted.
-   Web URLs, both short and long form, are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`)
-   Most large integer are formatted as numeric sequences. When large number (milhões, bilhões, etc) are spoken with a single group integers, quantity word `milhões/bilhões` is not converted for readability e.g. `doze milhões` -> `12 milhões` but when the number is more complex, it's formatted as numeric digits e.g. `doze milhões e um` -> `12000001`
-   Numbers below 10 are not formatted to digits to avoid odd conversions e.g. `vivo em uma casa` --> `vivo em 1 casa`
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `vírgula` (`,`), `ponto` (`.`), `ponto de interrogação` (`?`), `ponto de exclamação` (`!`), `ponto e vírgula` (`;`), `hífen` (`-`), etc.

### Smart formatting Examples for Brazilian-Portuguese
{: #smart-formatting-examples-pt-br}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                                 | With smart formatting |
|-----------------|--------------------------------------------------------------------------|-----------------------|
| Dates           | trinta e um de dezembro de mil novecentos e oitenta e oito               | 31/12/1988            |
|                 | um do um de mil novecentos e oitenta e sete                              | 01/01/1987            |
| Times           | quinze pro meio dia                                                      | 11:45                 |
|                 | meio dia e meia hora                                                     | 12:30                 |
|                 | ao meio dia e meio                                                       | ao 12:30              |
|                 | às dez pras duas da madrugada                                            | às 1:50 a.m.          |
|                 | às quinze para a meia noite                                              | às 23:45              |
| Numbers         | cento e quarenta e sete mil quatrocentos e cinquenta e um                | 147451                |
|                 | um vírgula vinte e seis                                                  | 1,26                  |
|                 | décimo primeiro                                                          | 11º                   |
| Phone numbers   | quatro cinco um dois três quatro cinco seis sete oito                    | (45) 1234-5678        |
|                 | onze nove nove oito meia cinco quinze zero dois                          | (11) 99865-1502       |
|                 | nove vinte e sete vinte e oito trinta e sete trinta e oito               | 92728-3738            |
|                 | mais cinco cinco onze nove meia nove zero meia zero um quatro meia       | +55 (11) 96906-0146   |
| Currency values | vinte e cinco centavos                                                   | R$ 0,25               |
|                 | vinte e nove dólares e cinquenta centavos                                | $ 29,50               |
|                 | vinte e cinco centavos                                                   | R$ 0,25               |
| Email, URL, IP  | a ponto b c arroba g mail ponto com                                      | a.bc@gmail.com        |
|                 | dáblio dáblio dáblio ponto a b c ponto es barra e f g                    | www.abc.es/efg        |
|                 | w w w ponto nvidia ponto com                                             | www.nvidia.com        |
|                 | noventa e oito ponto setenta e seis ponto noventa e oito ponto dezesseis | 98.76.98.16           |
| Measures        | duzentos e quarenta e cinco quilômetros por hora                         | 245 kph               |
|                 | duzentos e quarenta e cinco metros por segundo                           | 245 m/s               |
| Sequences       | d dezesseis três nove c hífen f noventa e oito                           | d1639c-f98            |
|                 | Modelo f t doze x                                                        | Modelo ft12x          |
{: caption="Smart formatting example transcripts"}

#### French
{: #smart-formatting-fr-fr}

-   In dates, the ordinal`premier` is considered 1st of the month. The dates are formatted as `DD/MM/YYYY`. 
-   Times are identified by keywords and prefix e.g. `heures`, `de l'après-midi` or `du soir`, `du matin`, `midi`. Times are formatted as 24H clock : `HH h MM`
-   Telephone numbers must have 9 or 10 digits (5 pairs of two digits). In cases where only one digit of the first pairing is admitted, assumes that the 0 was skipped. Numbers are formatted as `NN NN NN NN NN`
-   When the `de` or `d'` preposition is used to express currency, the currency symbol is not used to format. This usually occurs with large round numbers e.g. `un milliard d'euro` formatted as `1 milliard d'euro`.
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ arobase [alphanumeric point]+ domainname` ) are smart formatted. `@` can be represented by any of these: `arobase`, `chez`, `at`, `à`.
-   Cardinals below nine are not converted (in order to avoid `j'ai un pomme` -> `j'ai 1 pomme` and any other odd conversions.
-   For ordinals, 'siècles' are rendered in roman numerals when given an ordinal adjective. e.g. `dix-neuvième siècle` -> `XIXᵉ siècle`
-   Formatting of Fractions is supported. e.g.`un onzième` -> `1/11`
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `virgule` (`,`), `point` (`.`), `point d'interrogation` (`?`), `point d'exclamation` (`!`), `point-virgule` (`;`), `trait d'union` (`-`), etc.

### Smart formatting Examples for French
{: #smart-formatting-examples-fr-fr}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                     | With smart formatting     |
|-----------------|--------------------------------------------------------------|---------------------------|
| Dates           | vingt-quatre juillet deux-mille-treize                       | 24/7/2013                 |
|                 | dix-huit mai dix-neuf cent trente                            | 18/5/1930                 |
| Times           | huit heures du matin                                         | 8 h                       |
|                 | onze heures cinquante-sept                                   | 11 h 57                   |
|                 | deux heures de l'après-midi                                  | 14 h                      |
| Numbers         | cent quarante-sept mille quatre cent cinquante et une        | 147451                    |
|                 | moins vingt-cinq-mille-trente-sept                           | 25037                     |
|                 | vingt-troisièmes                                             | 23ᵉˢ                      |
|                 | quatre et deux quatrièmes                                    | 4 2/4                     |
| Phone numbers   | double neuf douze trente-deux trente trente                  | 99 12 32 30 30            |
|                 | deux douze trente-deux trente trente                         | 02 12 32 30 30            |
| Currency values | deux dollars vingt                                           | 2,20 $                    |
|                 | cinq euro et soixante                                        | 5,60 €                    |
|                 | quatre virgule quatre-vingt milliards d'euros                | 4,80 milliards d'euros    |
| Email, URL, IP  | a b trois point s d d point trois arobase g mail point com   | ab3.sdd.3@gmail.com       |
|                 | w w w point web point c o point f r                          | www.web.co.fr             |
|                 | double neuf dot trente-deux dot trente dot trente            | 99.32.30.30               |
| Measures        | quarante-deux-mille-deux-cent-cinquante-neuf par mètre carré | 42 259 /m²                |
|                 | deux cents kilomètres heure                                  | 200 km/h                  |
| Sequences       | le document numéro zéro deux trente-six vingt-quatre         | le document numéro 023624 |
|                 | r t x dix-huit t i                                           | rtx18ti                   |
{: caption="Smart formatting example transcripts"}

#### French-Canadian
{: #smart-formatting-fr-ca}

-   In dates, the ordinal`premier` is considered 1st of the month. The dates are formatted as `DD/MM/YYYY`. 
-   Times are identified by keywords and prefix e.g. `heures`, `de l'après-midi` or `du soir`, `du matin`, `midi`. Times are formatted as 24H clock : `HH h MM`
-   Phone numbers must be either `911` or a number that contains 10 digits and/or starts with the number `[+]1`.
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ arobase [alphanumeric point]+ domainname` ) are smart formatted. `@` can be represented by any of these: `arobase`, `chez`, `at`, `à`.
-   Cardinals below nine are not converted if they occur in midst of other text(in order to avoid `j'ai un pomme` -> `j'ai 1 pomme` and any other odd conversions). They are still formatted if they occur in isolation with no other text.
-   Formatting of Fractions is supported. e.g.`un onzième` -> `1/11`
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `virgule` (`,`), `point` (`.`), `point d'interrogation` (`?`), `point d'exclamation` (`!`), `point-virgule` (`;`), `trait d'union` (`-`), etc.

### Smart formatting Examples for French-Canadian
{: #smart-formatting-examples-fr-ca}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                     | With smart formatting     |
|-----------------|--------------------------------------------------------------|---------------------------|
| Dates           | vingt-quatre juillet deux-mille-treize                       | 24/7/2013                 |
|                 | dix-huit mai dix-neuf cent trente                            | 18/5/1930                 |
| Times           | huit heures du matin                                         | 8 h                       |
|                 | onze heures cinquante-sept                                   | 11 h 57                   |
|                 | deux heures de l'après-midi                                  | 14 h                      |
| Numbers         | cent quarante-sept mille quatre cent cinquante et une        | 147451                    |
|                 | moins vingt-cinq-mille-trente-sept                           | 25037                     |
|                 | vingt-troisièmes                                             | 23es                      |
|                 | quatre et deux quatrièmes                                    | 4 2/4                     |
| Phone numbers   | plus un cinq un quatre cinq cinq cinq un deux trois quatre   | +1 (514) 555-1234         |
|                 | cinq un quatre quatre six neuf deux un zéro zéro             | 02 12 32 30 30            |
| Currency values | deux dollars vingt                                           | 2,20 $                    |
|                 | vingt dollars cinq                                           | 20,05 $                    |
|                 | quatre virgule quatre-vingt milliards d'euros                | 4,80 milliards d'euros    |
| Email, URL, IP  | a b trois point s d d point trois arobase g mail point com   | ab3.sdd.3@gmail.com       |
|                 | w w w point web point c o point f r                          | www.web.co.fr             |
|                 | double neuf dot trente-deux dot trente dot trente            | (514) 469-210             |
| Measures        | quarante-deux-mille-deux-cent-cinquante-neuf par mètre carré | 42 259 /m²                |
|                 | deux cents kilomètres heure                                  | 200 km/h                  |
| Sequences       | le document numéro zéro deux trente-six vingt-quatre         | le document numéro 023624 |
|                 | r t x dix-huit t i                                           | rtx18ti                   |
{: caption="Smart formatting example transcripts"}

#### Spanish
{: #smart-formatting-es-es}

-   In dates, the ordinal`primero` is considered 1st of the month. The dates are formatted as `DD/MM/YYYY`. 
-   Times on the hour or time without an article followed by a suffix (indicating 'a.m.' or 'p.m.'), it will be converted .e.g `las dos pe eme`. Times are formatted as 24H clock : `HH h MM` or as 12H clock with a.m./p.m.
-   Telephone numbers must have 8, 9 or 10 digits. Numbers are formatted as `NNNN NNNN` or `NNN NNN NNN` or `NNN NNN NNNN`
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ arroba [alphanumeric punto]+ domainname` ) are smart formatted.
-   Cardinals below nine are not converted if they occur in midst of other text(in order to avoid `un gato en el camino` -> `1 gato en el camino` and any other odd conversions). They are still formatted if they occur in isolation with no other text.
-   Formatting of Fractions is supported. e.g.`un décimo` -> `1/10`
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `punto` (`.`), `interrogación` (`?`), `exclamación` (`!`), `punto y coma` (`;`), `guion medio` (`-`), etc.

### Smart formatting Examples for Spanish
{: #smart-formatting-examples-es-es}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                     | With smart formatting     |
|-----------------|--------------------------------------------------------------|---------------------------|
| Dates           | treinta y uno de diciembre de mil novecientos noventa y dos  | 31/12/1992                |
|                 | dieciséis de septiembre dos mil dieciocho                    | 16/09/2018                |
| Times           | las dieciséis cincuenta                                      | las 16:50                 |
|                 | las dos a eme                                                | las 2:00 a.m.             |
| Numbers         | mil novecientos cincuenta y ocho                             | 1958                      |
|                 | once mil novecientos cincuenta y ocho                        | 11958                     |
|                 | décima primera                                               | 11ª                       |
|                 | un cuarentiunavo                                             | 1/41                      |
| Phone numbers   | nueve uno cuatro cinco cinco seis ocho tres tres uno         | 914 556 8331              |
|                 | uno dos tres cuatro cinco seis siete ocho                    | 1234 5678                 |
| Currency values | dos euros noventa centavos                                   | €2,90                     |
|                 | doce euros y cinco centavos                                  | €12,05                    |
|                 | nueve punto cinco millones de pesos                          | $9.5 millones             |
| Email, URL      | a b c arroba g mail punto a b c                              | abc@gmail.abc             |
|                 | doble uve doble uve doble uve punto nvidia punto com         | www.nvidia.com            |
| Measures        | tres metros cúbicos                                          | 3 m³                      |
|                 | dos kilómetros por hora                                      | 2 kph                     |
| Sequences       | cero dos tres seis dos cuatro                                | 023624                    |
|                 | r t x cero dos tres w                                        | rtx023w                   |
{: caption="Smart formatting example transcripts"}

#### German
{: #smart-formatting-de-de}

-   Date formatting has support for both numeric and name for months ( e.g. `zweiter` is same as `februar`). The dates are formatted as `DD.MM.YYYY`. 
-   Times are identified by keywords e.g. `nach` `uhr`, `vor`, `minuten`. Time is formatted as 24-hr clock : `HH:MM:SS`
-   Telephone numbers should have 3-4 digit area code starting with `0` followed by 8-digit number. Country code (+49) is optional. Area code should not start with `0` if country code is used. Numbers are formatted as `+49 [N]NN NNNNNNNN` or `0[N]NN NNNNNNNN`.
-   Most Currency symbols are substituted for strings in appropriate contexts e.g. `dollar`, `cent`, `euro`, `yen`.
-   Internet email addresses with common format ( e.g. `[alphanumeric+symbols]+ ät [alphanumeric punkt]+ domainname` ) are formatted
-   Web URLs, both short and long form, are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`)
-   Cardinals below nine are not converted in order to avoid odd or ambiguous conversions.
-   Ordinals and fractions formatting are supported.
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `komma` (`,`), `punkt` (`.`), `fragezeichen` (`?`), `ausrufezeichen` (`!`), `semikolon` (`;`), `bindestrich` (`-`), etc.

### Smart formatting Examples for German
{: #smart-formatting-examples-de-de}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Entity Type     | Without smart formatting                                                     | With smart formatting   |
|-----------------|------------------------------------------------------------------------------|-------------------------|
| Dates           | vierundzwanzigster juli zwei tausend dreizehn                                | 24.07.2013              |
|                 | dreizehnter zweiter zwei tausend zwanzig                                     | 13.02.2020              |
| Times           | vierundzwanziguhrzweiundzwanzig                                              | 24:22 Uhr               |
|                 | acht uhr sieben                                                              | 08:07 Uhr               |
|                 | ein uhr eine minute eine sekunde                                             | 01:01:01 Uhr            |
| Numbers         | minus fünf und zwanzig tausend sieben und dreißig                            | -25037                  |
|                 | acht hundert achtzehn komma drei null drei                                   | 818,303                 |
|                 | fünfundzwanzigtausendeinhundertelftem                                        | 25111.                  |
|                 | drei zwei ein hundertstel                                                    | 3 2/100                 |
| Phone numbers   | null vier eins eins eins zwei drei vier eins zwei drei vier                  | 0411 12341234           |
|                 | plus vier neun vier eins eins eins zwei drei vier eins zwei drei vier        | +49 411 12341234        |
| Currency values | zwei komma null null null eins dollar                                        | 2,0001 $                |
|                 | zweiundzwanzig cent                                                          | 0,22 €                  |
| Email, URL, IP  | a b drei bindestrich s d d bindestrich drei ät g mail punkt com              | ab3-sdd-3@gmail.com     |
|                 | h t t p s doppelpunkt slash slash w w w punkt a b c punkt com slash a b      | https://www.abc.com/ab  |
|                 | drei fünf punkt eins drei fünf punkt zwei vier punkt zwei vier               | 35.135.24.24            |
| Measures        | zwei kilometer pro stunde                                                    | 2 km/h                  |
|                 | vier hundert vierzig milliliter                                              | 440 ml                  |
| Sequences       | c b vier drei bindestrich fünf drei fünf zwei vier zwei punkt vier drei fünf | cb43-535242.435         |
|                 | teilenummer f t strich zwölf p                                               | teilenummer ft-12p      |
{: caption="Smart formatting example transcripts"}

### Smart formatting V2 examples
{: #smart-formatting-example}

The following example requests smart formatting with a recognition request by setting the `smart_formatting` parameter to `true`. The following sections show the effects of smart formatting on the results of a request.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_Telephony&smart_formatting=true&smart_formatting_version=2"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?model=en-US_Telephony&smart_formatting=true&smart_formatting_version=2"
```
{: pre}
