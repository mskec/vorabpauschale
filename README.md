# Program for calculating the Vorabpauschale and the assessment base

> This program calculates a specific German tax. It may only be of interest to private German tax payers. Due to this the  documentation is in German only.

## Important Notice

> I am not a tax advisor. All results of this routine are non-binding and without guarantee. They may differ from the actual values.

## Usage

Example:
```
./vorabpauschale.py depot1_2018.ini depot2_2018.ini ...
```

## Description

The program is based on research on this topic found on the internet. However, differences in the interpretation of the legal text have also been shown.

This program implements an algorithm according to the majority of these sources. However, this is not a guarantee of correctness and I am grateful for hints about errors.

The main routine `berechne_vorabpauschale_und_bemessungsgrundlage` is located in the file `vorabutil.py`. It receives the data of individual tax cases as a dictionary and also outputs the results as a dictionary. It contains no further dependencies and can therefore also be called by other programs. The description of the parameters can be found in [parameter.md](parameter.md)

The rest of the program serves to read the data and output the results.

The exact description of the parameters and the algorithm can be found in [grundlagen.md](grundlagen.md).

[inidateien.md](inidateien.md) describes the parameters that are stored in the (own) INI files. There you will also learn which entries must be set when.

The inputs and outputs of the algorithm use the normal Python data formats (string, float, etc.).

The values in the INI files and the output of the results, however, use the decimal comma.

The file `basis.ini` contains the information about the base interest rate and the applicable capital gains tax rate.

The file `test.ini` contains the tax test cases for testing the algorithm.

INI files with your own test cases can have any name.
