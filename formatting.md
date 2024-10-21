---

copyright:
  years: 2015, 2023
lastupdated: "2024-05-09"

subcollection: speech-to-text

---

{{site.data.keyword.attribute-definition-list}}

# Response formatting and filtering
{: #formatting}

The {{site.data.keyword.speechtotextfull}} service provides three features that you can use to parse transcription results. You can format a final transcript to include more conventional representations of certain strings and to include punctuation. You can redact sensitive numeric information from a final transcript, and you can filter profanity from most transcription results. All of these features are beta functionality and are restricted to certain languages.
{: shortdesc}


## Smart formatting Version 2
{: #smart-formatting-version}

The new version of smart formatting feature is available for US English, Brazilian Portuguese, French, German, Castilian Spanish, Spanish Latin American, and French Canadian. It is also available for the en-WW_Medical_Telephony model when US English audio is recognized. 

The new version:
-	provides more flexibility in adding new languages and patterns compared to older smart formatting. 
-	uses a more sophisticated machine learning technique (Weighted Finite State Transducers) to identify entities in texts compared to the older version, which was rule-based approach. 
-	provides more accurate entity classification and formatting and also adds capability to define hierarchies using weights when same text can be identified as two different entity type.

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
-   Times are identified by keywords or suffix, for example, timezones (for example, `est`, `eastern`), `am`, `pm`, `hours`, `o'clock`, `minutes past hour`.
-   Phone numbers must be either `911` or a number that contains 10 digits and/or starts with the number `[+]1`.
-   Currency symbols are substituted for strings in appropriate contextsfor, for example, `dollar`, `cent`, `euro`, `yen`. `cent` is optional after `dollar`, for example, `twelve dollars twenty five` and `twelve dollars twenty five cents` formatted as `$12.25`.
-   Internet email addresses with common format (for example, `[alphanumeric+symbols]+ at [alphanumeric dot]+ domainname` ) are smart formatted.
-   Web URLs, both short and long form are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`).
-   Most large integers are formatted as numeric sequences. When large numbers (million, billions) are spoken with a single group integers, quantity word `million/billion` is not converted for readability, for example, `fifty nine million` -> `59 million` but when the number is more complex, it is formatted as numeric digits, for example, `fifty nine million and one` -> `59000001`.
-   Numbers less than 10 are not converted to digit to avoid odd formatting, for example, `You are one of them` -> `You are 1 of them`. But in other context like expressing currency, they are converted appropriately, for example, `Give me one dollar` -> `Give me $1`.
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken or dictated punctuation symbols for the keyword strings.
    - `comma` (`,`), `period` (`.`), `question mark` (`?`), `exclamation point` (`!`), `semicolon` (`;`), `hyphen` (`-`).

### Smart formatting Examples
{: #smart-formatting-examples}

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
-   Times are identified by keywords and prefix, for example, `às` `ao`, `à`, `da tarde` (`p.m.`), `da madrugada` (`a.m.`), `meia noite`, `meio dia`. Prefixes  `às` `ao`, `à` are optional.
-   Landline numbers must have 10 digits (2 digits country code and 8 digits number), mobile numbers are 9 digits with first digit as `9` with optional country code. Area codes are optional. Numbers are formatted as `+NN (NN) NNNN-NNNN` and `+NN (NN) 9NNNN-NNNN`.
-   Brazilian real currency symbol is `R$`. Other Currency symbols are substituted for strings in appropriate contexts, for example, `dollar`, `cent`, `euro`, `yen`. `centavos` is optional after `reais` for example, `setenta e cinco dólares e sessenta e três` and `setenta e cinco dólares e sessenta e três centavos` formatted as `R$75,63`
-   Internet email addresses with common format (for example, `[alphanumeric+symbols]+ arroba [alphanumeric ponto]+ domainname` ) are smart formatted.
-   Web URLs, both short and long form, are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`).
-   Most large integer are formatted as numeric sequences. When large numbers (milhões, bilhões, etc) are spoken with a single group integers, quantity word `milhões/bilhões` is not converted for readability for example, `doze milhões` -> `12 milhões` but when the number is more complex, it's formatted as numeric digits for example, `doze milhões e um` -> `12000001`.
-   Numbers less than 10 are not formatted to digits to avoid odd conversions, for example, `vivo em uma casa` --> `vivo em 1 casa`.
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `vírgula` (`,`), `ponto` (`.`), `ponto de interrogação` (`?`), `ponto de exclamação` (`!`), `ponto e vírgula` (`;`), `hífen` (`-`).

### Smart formatting Examples
{: #smart-formatting-examples}

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
-   Times are identified by keywords and prefix, for example, `heures`, `de l'après-midi` or `du soir`, `du matin`, `midi`. Times are formatted as 24H clock : `HH h MM`
-   Telephone numbers must have 9 or 10 digits (5 pairs of two digits). In cases where only one digit of the first pairing is admitted, assumes that the 0 was skipped. Numbers are formatted as `NN NN NN NN NN`.
-   When the `de` or `d'` preposition is used to express currency, the currency symbol is not used to format. This usually occurs with large round numbers, for example, `un milliard d'euro` formatted as `1 milliard d'euro`.
-   Internet email addresses with common format ( for example, `[alphanumeric+symbols]+ arobase [alphanumeric point]+ domainname` ) are smart formatted. `@` can be represented by any of these: `arobase`, `chez`, `at`, `à`.
-   Cardinals less than nine are not converted (in order to avoid `j'ai un pomme` -> `j'ai 1 pomme` and any other odd conversions.
-   For ordinals, 'siècles' are rendered in roman numerals when given an ordinal adjective. e `dix-neuvième siècle` -> `XIXᵉ siècle`.
-   Formatting of Fractions is supported. For example,`un onzième` -> `1/11`.
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `virgule` (`,`), `point` (`.`), `point d'interrogation` (`?`), `point d'exclamation` (`!`), `point-virgule` (`;`), `trait d'union` (`-`).

### Smart formatting Examples
{: #smart-formatting-examples}

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

### Smart formatting Examples
{: #smart-formatting-examples}

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

### Smart formatting Examples
{: #smart-formatting-examples}

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
-   Date formatting has support for both numeric and name for months (for example, `zweiter` is same as `februar`). The dates are formatted as `DD.MM.YYYY`. 
-   Times are identified by keywords, for example, `nach` `uhr`, `vor`, `minuten`. Time is formatted as 24-hr clock : `HH:MM:SS`.
-   Telephone numbers should have 3-4 digit area code starting with `0` followed by 8-digit number. Country code (+49) is optional. Area code should not start with `0` if country code is used. Numbers are formatted as `+49 [N]NN NNNNNNNN or 0[N]NN NNNNNNNN `.
-   Most Currency symbols are substituted for strings in appropriate contexts, for example, `dollar`, `cent`, `euro`, `yen`.
-   Internet email addresses with common format ( for example, `[alphanumeric+symbols]+ ät [alphanumeric punkt]+ domainname` ) are formatted.
-   Web URLs, both short and long form, are formatted. It includes protocol (`http/s`), subdomain (`www`), ports (`443,80`) and paths (`/help/abc`)
-   Cardinals less than nine are not converted in order to avoid odd or ambiguous conversions.
-   Ordinals and fractions formatting are supported.
-   Most of the punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes spoken/dictated punctuation symbols for the keyword strings.
    - `komma` (`,`), `punkt` (`.`), `fragezeichen` (`?`), `ausrufezeichen` (`!`), `semikolon` (`;`), `bindestrich` (`-`).

### Smart formatting Examples
{: #smart-formatting-examples}

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


## Smart formatting
{: #smart-formatting}

The smart formatting feature is beta functionality that is available for US English, Japanese, and Spanish (all dialects). It also also available for the `en-WW_Medical_Telephony` model when US English audio is recognized.
{: beta}

The `smart_formatting` parameter directs the service to convert the following strings into more conventional representations:

-   Dates
-   Times
-   Series of digits and numbers
-   Phone numbers
-   Currency values (for US English and Spanish)
-   Internet email and web addresses (for US English and Spanish)

You set the `smart_formatting` parameter to `true` to enable smart formatting. By default, the service does not perform smart formatting. The service applies smart formatting just before it returns the final results to the client, when text normalization is complete. The conversion makes the transcript more readable and promotes better post-processing of transcription results by representing these artifacts as they would normally be written.

### What results does smart formatting affect?
{: #smart-formatting-effects}

Smart formatting impacts some transcription results and not others:

-   Smart formatting affects only words in the `transcript` field of final results, those results for which the `final` field is `true`. It does not affect interim results, for which `final` is `false`.
-   Smart formatting does not affect words in other fields of the response. For example, smart formatting is not applied to response data in the `timestamps` or `alternatives` fields.
-  Speech hesitations, such as "uhm" and "uh", can adversely impact the conversion of phrases and strings by smart formatting for some languages. Previous-generation models produce hesitation markers to replace such hesitations in a transcript. Smart formatting has the following effect on hesitation markers for previous-generation models:

    -   *For US English,* smart formatting suppresses hesitation markers from the `transcript` field for final results.
    -   *For Japanese,* hesitation markers continue to appear in final results.
    -   *For both US English and Japanese,* hesitation markers continue to appear in interim results.
    -   *For Spanish,* the service does not produce hesitation markers for any results.

    Next-generation models do not produce hesitation markers. They instead include the actual hesitations in transcription results. Smart formatting has no effect on hesitations that are included by next-generation models. For more information, see [Speech hesitations and hesitation markers](/docs/speech-to-text?topic=speech-to-text-basic-response#response-hesitation).

### Language differences
{: #smart-formatting-differences}

Smart formatting is based on the presence of obvious keywords in the transcript. Because of differences among the supported languages, smart formatting works slightly differently for each language. The following sections describe the strings and content that trigger smart formatting changes for [US English and Spanish](#smart-formatting-english-spanish) and for [Japanese](#smart-formatting-japanese).

#### US English and Spanish
{: #smart-formatting-english-spanish}

-   Times are identified by keywords such as `AM`, `PM`, or `EST`.
-   Military times are converted if they are identified by the keyword `hours` (*US English*) or `horas` (*Spanish*).
-   Phone numbers must be either `911` or a number that contains 10 or 11 digits and starts with the number `1`.
-   Currency symbols are substituted for the following strings in appropriate contexts:
    -   *For US English,* dollar, cent, and euro.
    -   *For Spanish,* dolar, peso, peseta, libras esterlinas, libra, and euro.
-   Internet email addresses are converted in some cases. Specifically, the service converts email addresses if the input audio uses the phrasing `email address ... {address}`. The following examples show a correct conversion of spoken phrases:
    -   `My email address is j dot d o e at i b m dot com` becomes `My email address is j.doe@ibm.com`.
    -   `Mi correo electronico es j punto d o e arroba i b m punto com` becomes `Mi correo electronico es j.doe@ibm.com`.
-   Internet web addresses are converted in their short forms. Fully qualified web addresses are not converted. The following examples show complete conversions:
    -   `I saw the story on yahoo dot com` becomes `I saw the story on yahoo.com`.
    -   `Vi la historia en yahoo punto com` becomes `Vi la historia en yahoo.com`.

    The following examples show incomplete conversions:
    -   `I saw the story on w w w dot yahoo dot com` becomes `I saw the story on w w w .yahoo.com`.
    -   `Vi la historia en w w w punto yahoo punto com` becomes `Vi la historia en w w w .yahoo.com`.
-   Conversion of large numbers and currency values can be challenging. The service converts digits and many numbers well. But larger and more complex numbers and currency values work best with more precise phrasing. For example, the service correctly converts the following transcripts because of their precise wording:
    -   `sixty nine thousand five hundred sixty dollars and twenty five cents` becomes `$69560.25`,
    -   `sixty nine thousand five hundred sixty dollars point twenty five` becomes `$69560.25`.

    But the service cannot correctly convert the following transcripts due to their looser phrasing:
    -   `sixty nine thousand five sixty dollars and twenty five cents` becomes `60 9000 $560.25`.
    -   `sixty nine thousand five sixty dollars point twenty five` becomes `60 9000 $560.25`.

    To correctly convert a greater possible variety of complex numbers, you need to experiment with the results of smart formatting and customize your own post-processing utilities.
-   *For US English,* certain punctuation symbols are added for special keywords that occur in appropriate places. When you use smart formatting, the service substitutes punctuation symbols for the following keyword strings based on where it finds them in a transcript:

    -   `Comma` (`,`)
    -   `Period` (`.`)
    -   `Question mark` (`?`)
    -   `Exclamation point` (`!`)

    The service converts these keyword strings to symbols only in appropriate positions of a transcript. In the following example, the speaker says the word `period` at the end of the sentence:

    -   `the warranty period is short period` becomes `the warranty period is short.`

    The service correctly differentiates between the noun that appears earlier in the sentence and the concluding punctuation.

#### Japanese
{: #smart-formatting-japanese}

-   Phone numbers must be 10 or 11 digits and begin with valid prefixes for telephone numbers in Japan. For example, valid prefixes include `03` and `090`.
-   English words are converted to ASCII (*hankaku*) characters. For example, `ＩＢＭ` is converted to `IBM`.
-   Ambiguous terms might not be converted if sufficient context is unavailable. For example, it is unclear whether `一時` and `十分` refer to times.
-   Punctuation is handled in the same way with or without smart formatting. For example, based on probability calculations, one of `カンマ` or `,` is selected.
-   Strings that describe yen values are not replaced with the yen currency symbol.
-   Internet email and web addresses in any form are not converted.
-   The Japanese narrowband model (`ja-JP_NarrowbandModel`) includes some multigram word units for digits and decimal fractions. The service returns these multigram units regardless of whether you enable smart formatting. The following examples show the units that the service returns. The numbers in parentheses show the equivalent Arabic numeric expression for each unit.
    -   *Digits:* `〇一` (01), ..., `〇九` (09), `一〇` (10), ..., `九〇` (90)
    -   *Decimal fractions:* `〇・` (0.), `一・` (1.), ..., `十・` (10.)

    The smart formatting feature understands and returns the multigram units that the model generates. If you apply your own post-processing to transcription results, you need to handle these units appropriately.

### Smart formatting results
{: #smart-formatting-results}

The following table shows examples of final transcripts both with and without smart formatting. The transcripts are based on US English audio.

| Information | Without smart formatting | With smart formatting |
|-------------|--------------------------|-----------------------|
| Dates | I was born on ten oh six nineteen seventy | I was born on 10/6/1970 |
|       | I was born on the ninth of December nineteen hundred | I was born on 12/9/1900 |
|       | Today is June sixth | Today is June 6 |
| Times | The meeting starts at nine thirty AM | The meeting starts at 9:30 AM |
|       | I am available at seven EST | I am available at 7:00 EST |
|       | We meet at oh seven hundred hours | We meet at 0700 hours |
| Numbers | The quantity is one million one hundred and one | The quantity is 1000101 |
|         | One point five is between one and two | 1.5 is between 1 and 2 |
| Phone numbers | Call me at nine one four two three seven one thousand | Call me at 914-237-1000 |
|               | Call me at one nine one four nine oh nine twenty six forty five | Call me at 1-914-909-2645 |
| Currency values | You owe me three thousand two hundred two dollars and sixty six | You owe me $3202.66 |
|                 | The dollar rose to one hundred and nine point seven nine yen from one hundred and nine point seven two yen | The dollar rose to 109.79 yen from 109.72 yen |
| Internet email and web addresses | My email address is john dot doe at foo dot com | My email address is john.doe@foo.com |
|                                  | I saw the story on yahoo dot com | I saw the story on yahoo.com |
| Combinations | The code is zero two four eight one and the date of service is May fifth two thousand and one | The code is 02481 and the date of service is 5/5/2001 |
|              | There are forty seven links on Yahoo dot com now | There are 47 links on Yahoo.com now |
{: caption="Smart formatting example transcripts"}

### Smart formatting results for long pauses
{: #smart-formatting-long-pauses}

In cases where an utterance contains long enough pauses of silence, the service can split the transcript into two or more final results. This affects the contents of the response, as shown in the following examples.

| Audio speech | Formatted transcription results |
|--------------|---------------------------------|
| My phone number is nine one four five five seven three three nine two | "My phone number is 914-557-3392" |
| My phone number is nine one four ...*pause*... five five seven three three nine two | "My phone number is 914"  \n "5573392" |
{: caption="Smart formatting example transcripts for long pauses"}

For more information about specifying a pause interval that affects the service's response, see [End of phrase silence time](/docs/speech-to-text?topic=speech-to-text-parsing#silence-time).

### Smart formatting example
{: #smart-formatting-example}

The following example requests smart formatting with a recognition request by setting the `smart_formatting` parameter to `true`. The following sections show the effects of smart formatting on the results of a request.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?smart_formatting=true"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?smart_formatting=true"
```
{: pre}

## Numeric redaction
{: #numeric-redaction}

The numeric redaction feature is beta functionality that is available for US English, Japanese, and Korean.
{: beta}

The `redaction` parameter directs the service to redact, or mask, numeric data from final transcripts. The feature redacts any number that has three or more consecutive digits by replacing each digit with an `X` character. It is intended to redact sensitive numeric data, such as credit card numbers.

By default, the service does not redact numeric data. Set the `redaction` parameter to `true` to enable numeric redaction. When you enable redaction, the service automatically enables smart formatting by setting the `smart_formatting` parameter to `true`, regardless of whether you explicitly disable that feature. To ensure maximum security, the service also disables the following parameters when you enable redaction:

-   The service disables keyword spotting, regardless of whether you specify values for the `keywords` and `keywords_threshold` parameters.
-   The service disables maximum alternatives, regardless of whether you specify a value greater than 1 for the `max_alternatives` parameter. The service returns only a single final transcript.
-   The service disables interim results for the WebSocket interface, regardless of whether you set the `interim_results` parameter to `true`.

The design of the feature parallels the existing smart formatting feature. The service applies redaction only to the final transcript of a recognition request, just before it returns the results to the client and after text normalization is complete.

### Language differences
{: #numeric-redaction-differences}

The feature works exactly as described for US English models but has the following differences for Japanese and Korean models.

#### Japanese
{: #numeric-redaction-japanese}

Japanese redaction has the following differences:

-   In addition to masking strings of three or more consecutive digits, redaction also masks street addresses and numbers, regardless of whether they contain fewer than three digits.
-   Similarly, redaction also masks date information in Japanese-style birth dates. In Japanese, date information is usually presented in Common Era format but sometimes follows Japanese style, particularly for birth dates. In this case, the year and month are masked even though they contain just one or two digits.

    For example, a Japanese-style birth date without redaction is `平成 30年 2月`. With redaction, the date becomes `平成 XX年 X月`.

#### Korean
{: #numeric-redaction-korean}

Korean redaction has the following differences:

-   The smart formatting feature is not supported. The service still performs numeric redaction for Korean, but it performs no other smart formatting.
-   Isolated digit characters are reduced, but possible digit characters that are included as part of Korean phrases are not. For example, the character `이`in the following phrase is not replaced by an `X` because it is adjacent to the following character:

    `이입니다`

    If the `이` character were separated from the following character by a space, it would be replaced by an `X`, as described in [Numeric redaction results](#numeric-redaction-results).

### Numeric redaction results
{: #numeric-redaction-results}

The following table shows examples of final transcripts both with and without numeric redaction in each supported language.

| Language | Without redaction | With redaction |
|----------|-------------------|----------------|
| US English | my credit card number is four one four seven two | my credit card number is XXXXX |
| Japanese | 私 のクレジット カード 番号 は 四 一 四 七 二です | 私 のクレジット カード 番号 は XXXXX です |
| Korean | 내 신용 카드 번호는 사 일 사 칠 이 번입니다 | 내 신용 카드 번호는 XXXXX 번입니다 |
{: caption="Numeric redaction example transcripts"}

### Numeric redaction example
{: #numeric-redaction-example}

The following example requests numeric redaction with a recognition request by setting the `redaction` parameter to `true`. Because the request enables redaction, the service implicitly enables smart formatting with the request. The service effectively disables the other parameters of the request so that they have no effect: The service returns a single final transcript and recognizes no keywords.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?&redaction=true&max_alternatives=3&keywords=birth%2Cbirthday&keywords_threshold=0.5"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/wav" \
--data-binary @{path}audio-file.wav \
"{url}/v1/recognize?&redaction=true&max_alternatives=3&keywords=birth%2Cbirthday&keywords_threshold=0.5"
```
{: pre}

## Profanity filtering
{: #profanity-filtering}

The profanity filtering feature is generally available for US English and Japanese only.
{: note}

The `profanity_filter` parameter indicates whether the service is to censor profanity from its results. By default, the service obscures all profanity by replacing it with a series of asterisks in the transcript. Setting the parameter to `false` displays words in the output exactly as transcribed.

The service censors profanity from all final transcripts and from any alternative transcripts. It also censors profanity from results that are associated with word alternatives, word confidence, and word timestamps. The sole exception is keyword spotting, for which the service returns all words as specified by the user, regardless of whether `profanity_filter` is `true`.

### Profanity filtering example
{: #profanity-filtering-example}

The following example shows the results for a brief audio file that is transcribed with the default `true` value for the `profanity_filter` parameter. The request also sets the `word_alternatives_threshold` parameter to a relatively high value of `0.99` and the `word_confidence` and `timestamps` parameters to `true`.

[IBM Cloud]{: tag-ibm-cloud}

```bash
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

[IBM Cloud Pak for Data]{: tag-cp4d}

```bash
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary @{path}audio-file.flac \
"{url}/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

The service masks profanity from the response by replacing it with a series of asterisks:

```javascript
{
  "result_index": 0,
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ]
}
```
{: codeblock}
