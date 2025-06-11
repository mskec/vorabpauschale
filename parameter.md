# Parameters of the Main Routine for Calculating the Vorabpauschale and the Assessment Base

The routine `berechne_vorabpauschale_und_bemessungsgrundlage` is located in `vorabutil.py`.

## Input Values

The routine is called with two parameters:

 * `para`: a dictionary with the values of the tax case
 * `basiszinsen_feld`: a dictionary with the base interest rates for the individual years

### basiszinsen_feld

Dictionary

 * Key: Integer, year number (e.g. 2019)
 * Value: Float, percentage value (e.g. 0.52 - corresponding to 0.52%)

### Tax Case Input Values

The key names in this dictionary are defined as constants (in capital letters throughout) in `vorabutil.py`.

They correspond to the key names in the INI files (however, they are written in lowercase throughout there).


| Constant                   | When
| -------------------------- | ---------------
| ABRECHNUNGSJAHR            | always
| KAUFDATUM                  | from purchase
| KAUFKURS                   | from purchase
| ANZAHL                     | from purchase
| FREISTELLUNGSSATZ          | from purchase
| KURS_JAHRESANFANG          | when holding
| KURS_JAHRESENDE            | when holding
| AUSSCHUETTUNGEN_IM_JAHR    | when holding, when selling
| VERKAUFSDATUM              | when selling
| VERKAUFSKURS               | when selling
| SUMME_ALTE_VORABPAUSCHALEN | when selling

### Results

These keys are also defined as constants.

| Constant                   | When
| -------------------------- | ---------------
| VORABPAUSCHALE             | always
| BEMESSUNGSGRUNDLAGE        | always
| BASISERTRAG                | when holding
| WERTSTEIGERUNG_JAHR        | when holding
| WERTSTEIGERUNG_GESAMT      | when selling
| UNTERJAHR_FAKTOR           | in the year of purchase
| FEHLERHINWEIS              | in special situations

#### FEHLERHINWEIS

One of the mentioned special situations to which this entry points is the estimation of the advance tax *during* the accounting year (see also [inidateien.md](inidateien.md)).

Normally, the result of the routine has no FEHLERHINWEIS entry.