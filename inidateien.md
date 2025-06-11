# Values in the INI Files

The program uses the following INI files:

 * `basis.ini`: contains the information about the base interest rate and applicable capital gains tax rate
 * `test.ini`: contains the test cases for testing the algorithm
 * own INI files with tax cases: the name is freely selectable

## basis.ini

Typical content:

```
[basiszins]
2016=1,1%
2018=0,87%
2019=0,52%

[default]
steuersatz=26,375%
```

The *basiszins* section contains the base interest rates published by the German Federal Bank for calculating the advance tax for the individual years.

The value for 2016 is only used for testing purposes (see test cases from zendepot).

Under *default* you will find the applicable tax rate. Currently, this is a maximum of 25% + 5.5% solidarity surcharge

25% * (1 + 5.5%) = 26.375%

If your personal tax rate is lower or you pay church tax, the value can be adjusted.


## test.ini and own INI files

These files contain the tax cases that are processed by the program.

Here is a typical empty entry

```
[XXXXXXXXXXXX]
abrechnungsjahr=20XX
freistellungssatz=XX%
kurs_jahresanfang=
kurs_jahresende=
anzahl=
kaufdatum=
kaufkurs=
verkaufsdatum=
verkaufskurs=
ausschuettungen_im_jahr=0
summe_alte_vorabpauschalen=
hinweis=
bemerkung=
warnung=
```

The name of the section (in square brackets) must be unique and should be meaningful, e.g. year + ISIN. It is displayed when outputting the results.

Which fields must be filled in depends on whether you sold the fund in the accounting year (V) or held it beyond December 31 (H).


| Key | Required | Content |
| --- |:--------:| ------- |
| abrechnungsjahr | always | Year for which the accounting should be created |
| freistellungssatz | always | Exemption rate, depending on the type of fund (30%, 15%, 0%) |
| kurs_jahresanfang | H | Price of the fund at the beginning of the year |
| kurs_jahresende | H | Price of the fund at the end of the year |
| anzahl | always | Number of shares -> Value = Number * Price |
| kaufdatum | always | Date of purchase |
| kaufkurs | V | Price at which the share was purchased |
| verkaufsdatum | V | Date on which the share was sold |
| verkaufskurs | V | Price at which the share was sold |
| ausschuettungen_im_jahr | always | Amount of distributions in the accounting year, can be 0 |
| summe_alte_vorabpauschalen | V | Sum of the Vorabpauschalen paid in recent years for this tranche |
| hinweis | - | Plain text without further function |
| bemerkung | - | Plain text without further function |
| vorabpauschale | Test | if set → test case |
| bemessungsgrundlage | Test | if set → test case |
| Warnung | Test | Note that is *always* displayed in the output |

The detailed definition of the parameters can be found in [grundlagen.md](grundlagen.md).

## When do I need to enter what?

### Purchase

The following entries *must* be set:

 * kaufdatum
 * kaufkurs
 * anzahl
 * freistellungssatz

You *should* already enter now, because these values are available:

 * abrechnungsjahr
 * kurs_jahresanfang

### Sale

The following entries *must* additionally be set:

 * abrechnungsjahr
 * verkaufsdatum
 * verkaufskurs
 * ausschuettungen_im_jahr
 * summe_alte_vorabpauschalen

### if the fund was not sold during the accounting year

 * abrechnungsjahr
 * kurs_jahresanfang
 * kurs_jahresende
 * ausschuettungen_im_jahr

## Generally for every calculation

The following entries *must* be set:

 * abrechnungsjahr
 * ausschuettungen_im_jahr

### Estimation *during* the accounting year

During the accounting year, the price at the end of the year is of course not yet known.

The program then calculates an estimate based on

 * abrechnungsjahr
 * kurs_jahresanfang
 * ausschuettungen_im_jahr

A note that this is an estimate appears in the info column

## Tests

If a value is entered for the key `vorabpauschale` or `bemessungsgrundlage`, this record is considered a test case and these values are compared with the calculated values. The result of the test appears in the output. A successful comparison allows for a deviation of 3 euro cents.

## Warning

The key `warnung` serves to display a note in the list of results.

For example, the note that the "calculation on the website is probably incorrect".