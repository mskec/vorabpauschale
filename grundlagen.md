# Important Notice

> I am not a tax advisor. All results of this routine are non-binding and without guarantee. Liability is excluded. Nevertheless, I am grateful for hints about errors.

This document describes the fundamentals of the program and the individual parameters of the routine `berechne_vorabpauschale_und_bemessungsgrundlage` in `vorabutil.py`.

It calculates the Vorabpauschale and the assessment base for private investors for funds and ETFs according to the Investment Tax Act 2018.

# Definitions

## Basic Data

### Accounting Year

Year for which the accounting is done.

Together with the `Purchase Date` and the `Sale Date`, it determines:

* whether the shares were bought in the accounting year
* whether the shares were sold in the accounting year
* whether the shares were held beyond the accounting year
* whether the investor owned the shares at all in the accounting year

### Quantity

Number of shares in a fund/ETF. The value is calculated from the multiplication
of the quantity with the respective price.

### Purchase Date

Date on which the shares were purchased.

### Purchase Price

Price at which the shares were purchased

### Sale Date

Date on which the shares were sold.

### Sale Price

Price at which the shares were sold

### Price at Beginning of Year

Price of a share at the beginning of the year = last price of the previous year

### Price at End of Year

Price of a share at the end of the year = last price in the year

### Distributions in the Year

Distributions of the fund received in the accounting year

### Base Interest Rate

Set at the beginning of the year by the Federal Ministry of Finance.
The interest rate is given in percent.
(Stored in `basis.ini`)

### Applicable Tax Rate

This value in `basis.ini` refers to the applicable withholding tax.
Currently (2019) this is a maximum of 25% + 5.5% solidarity surcharge:

25% * (1 + 5.5%) = 26.375%

If you additionally pay church tax or your own income tax rate
is below 25%, you can also enter adjusted values here.

### Partial Exemption

The partial exemption is a percentage and depends on the type of fund.
For private investors these are:

| Fund Type     | Equity Ratio | Partial Exemption |
| ------------- |:------------:|:-----------------:|
| Equity Funds  |    ≥ 51%     | 30%               |
| Mixed Funds   |    ≥ 25%     | 15%               |
| Others        |    < 25%     | 0%                |

### Sum of Old Vorabpauschalen

Sum of the Vorabpauschalen from previous years that have already been taxed.

## Derived Values

### Base Return

`Base Return = Price at Beginning of Year x Quantity x Base Interest Rate x 0.7`


### Value Development

`(Price at End of Year - Price at Beginning of Year) * Quantity`


### Vorabpauschale

* Zero in the year of sale
* Proportional in the year of purchase: n/12
  * n = Number of started months in which the security was held
    * Purchase on 31.01. => n = 12
    * Purchase on  1.02. => n = 11
    * Purchase on 28.02. => n = 11
    * Purchase on  1.03. => n = 10
    * etc.
* Zero with negative value development in the accounting year
* Zero if distributions in the year >= base return

#### If value development + distributions in the year >= base return

`Vorabpauschale = Base Return - Distributions`

* if negative, then 0

## If value development + distributions in the year < base return

`Vorabpauschale = Value Development`

* a negative value development has already been sorted out in advance

## Assessment Base

The value used to calculate the tax.

### if the fund was *not* sold in the accounting year

`Assessment Base = (Vorabpauschale + Distributions in the Year) * (100% - Partial Exemption)`

### if the fund was sold in the accounting year


`Assessment Base = ((Sale Price - Purchase Price) * Number_of_Shares + Distributions in the Year - Sum of Old Vorabpauschalen) * (100% - Partial Exemption)`

## Tax to be Paid

`(Sum of all assessment bases possibly minus the saver's allowance) * applicable tax rate`


# Open Questions

* What about the sum of old Vorabpauschalen if no tax was paid for a previous Vorabpauschale because of the allowance?
  * Answer see (https://github.com/MStrecke/vorabpauschale/issues/1)

# Links

* https://www.justetf.com/de/news/etf/etf-und-steuern-das-neue-investmentsteuergesetz-ab-2018.html
* https://www.justetf.com/de/etf-steuerrechner.html
* https://www.justetf.com/de/news/geldanlage/der-sparerpauschbetrag.html
* https://www.vlh.de/wissen-service/steuer-abc/wie-funktioniert-die-abgeltungssteuer.html
* https://www.dasinvestment.com/fondsverband-bvi-die-wichtigsten-fragen-und-antworten-zur-investmentsteuerreform/?page=16
  - Beispiel für Summe alte Vorabpauschalen
* https://www.onvista-bank.de/vorabpauschale.html
  - Vorabpauschale bezieht sich immer auf das Vorjahr
  - Grundlage ist das Portefolio am 31.12
* https://zendepot.de/etf-fonds-steuern/#Die_neuen_Regelungen
* https://www.ffb.de/public/wissen/investmentsteuerreform.html#tabcontent01https://www.ffb.de/public/wissen/investmentsteuerreform.html#tabcontent01
* https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=4&ved=2ahUKEwi2x-_m-NDlAhXIGuwKHWkPC_wQFjADegQIAhAC&url=https%3A%2F%2Fwww.hypovereinsbank.de%2Fcontent%2Fdam%2Fhypovereinsbank%2Ffooter%2FHVB-Investmentsteuerreform-Infoblatt.pdf&usg=AOvVaw2lHk0yC6zyzGJPW0hGOmZ8
