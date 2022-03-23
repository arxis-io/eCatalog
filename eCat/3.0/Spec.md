# eCat 3.0 specification

Original: initeam  
Adjusted by: Gerjan Konterman

# Intro

Product management in furniture retail is quite often a big challenge. They have a lot of suppliers, with sometimes complex product structures. The shops want more and more to have all of the product information in their software (eMeubel in our case). So they can inform and serve their clients in a contemporary manner (in shop and on the web). And also all of their employees then have access to the product information.
For the exchange of simple, flat products, you can use Excel (CSV). For more complex products, this is not so straightforward. This is why we, for eMeubel, have defined an XML format, that can handle such complex structure.
A supplier that generates this XML out of there ERP, can send this file directly to the shops. The shop can import this file and obtain all the correct product information. The communication between supplier and vendor is directly, there is no EDI vendor in between.

For suppliers who are not in the possibility of building and exporting the XML from there ERP, we have 2 solutions:

- We make a conversion/integration tool based on the available data
- The supplier can input and manage the catalog manually in ‘eCat’, our software tool for building catalogues. (Retailers are also using this tool for building and maintaining catalogues of suppliers that don’t provide digital catalogues)
  Also if the supplier wants to check their catalog before sending it to the shop, or if they want to add something (eg multimedia), this can be done via eCat.

The retailer can send his purchase orders via e-mail or FTP to the supplier. The e-mail has 2 attachments: the order as a pdf and in XML format. The supplier can import this XML order in their ERP. (if this option is provided by the ERP of course.)

In this document we explain how the eCatXML catalogue is built.

# Structure of the XML catalog

## Base structure XML catalog

```xml
<catalog>
<General>…</General>
<OptionGTINDefinitions>…</OptionGTINDefinitions>
<ValueGTINDefinitions>…</ValueGTINDefinitions>
<Multimedia>…</Multimedia>
<Programs>…</Programs>
</catalog>
```

## General

Catalog contains general information such as the sender (= supplier), validity dates, ...
The IDs of both supplier and retailer must be unambiguous. If you want connect to an EDI platform, an official GLN (Global Location Number, 13 digits) is required and the number must be unique (you must be a member of GS1).
For suppliers or retailers who don’t need to work this 'official' way, we propose to make your own GLN. Take your telephone number (for example: 32 51 30 85 37), add prefix ’20…’ until you have 12 digits (for example: 203251308537). Calculate the check digit: https://www.gs1.org/check-digit-calculator. Our GLN results in 2032513085371

It is also possible to add data about the receiver (=customer) of the catalog. CustomerID is an internal ID field used by the issuer and not relevant for the receiving party.
In the Receivers tag, you can define the receivers of the catalog. This is done by adding the GLN of the receiving parties.
Currency = the currency codes of the catalog. These are ISO codes. Sales Price = retailers price excl. VAT. Retail Price = consumer price incl. VAT.
Default Language is the default language used in the catalog (ie the definitions of the products, option values, ...). This must be an ISO language code.

Example:

```xml
<General>
    <Sender>
        <ID>2003151308537</ID>
        <Name>Supplier name</Name>
        <GLN>2003251308537</GLN>
    </Sender>
    <CustomerID>XX</CustomerID>
    <Receivers>
        <GLN>2003251308538</GLN>
    </Receivers>
    <Version>Catalog v201106</Version>
    <Validfrom>20110101</Validfrom>
    <Validto>20111231</Validto>
    <Currency>
        <SalesPrice>EUR</SalesPrice>
        <RetailPrice>EUR</RetailPrice>
    </Currency>
    <Settings>
        <DefaultLanguage>NL</DefaultLanguage>
    </Settings>
</General>
```

## GTIN

Each item (option, value, product, ...) in the catalog has a unique number, called GTIN (Global Trade Item Number). It is necessary that this is a unique number within the entire catalog of a supplier.
EAN is one way to obtain such a unique number. If you want to be compatible with other platforms, it is necessary to purchase such a (EAN-13) number (and thus to join GS1).
If you only communicate with eMeubel, this not necessary. However, you must ensure that two different items in your catalog does not have the same GTIN!

## OptionGTINDefinitions

These are the definitions of all options (also called phrasing). Possible options are: 'Tissue', 'Color', 'Legs', 'Seating comfort', 'Height', ... The definition consists of a code (GTIN) and a description. Also descriptions in other languages can be specified. Later in the catalog, the code of the option is used to refer to this option (and not the description). In eMeubel this code has an important role: when the supplier changes the description, the products in eMeubel will also be adjusted (because the codes are the same – thus the GTIN codes are used for the mapping between catalog and products).
Next, we have the reference of the supplier. If it’s equal to the option code, this can be omitted.
Options are globally defined and have not yet been related to a product.
Options can have values. These values apply everywhere the option is used and should not be repeated at product level. These values can have a sort order and a value can be set as default. The sort order is a numerical value between 0 and 999. If no sorting is specified, the sorting is alphabetically.

> Note: it is not possible to define subvalues for these values (within this OptionDefinition).

Example:

```xml
<OptionGTINDefinitions>
    <OptionDefinition>
        <GTIN>5412345000001</GTIN>
        <Description>Comfort</Description>
        <Description language="FR">Comfort</Description>
        <Reference>5412345000001</Reference>
        <Values>
            <Value order="010"default="true">5412345000003</Value>
        <Valueorder="020">5412345000033</Value>
        </Values>
    </OptionDefinition>
    <OptionDefinition>
        <GTIN>5412345000004</GTIN>
        <Description>Stof/Kleur</Description>
        <Description language="FR">Tissue</Description>
        <Reference>5412345000004</Reference>
        </OptionDefinition>
    <OptionDefinition>
        <GTIN>5412345000021</GTIN>
        <Description>Breedte</Description>
        <Description language="FR">Largeur</Description>
        <Reference>5412345000021</Reference>
    </OptionDefinition>
    <OptionDefinition>
        <GTIN>5412345000022</GTIN>
        <Description>Lengte</Description>
        <Description language="FR">Longueur</Description>
        <Reference>5412345000022</Reference>
    </OptionDefinition>
</OptionGTINDefinitions>
```

## ValueGTINDefinitions

These are the definitions of all values (also called choices or specifications). Sample values are: 'Leather Class A', 'Oak 10 cm', 'Fixed', '200cm’, ... The definition exists of a code, the descriptions and reference (with the same remarks as above). You may also add a color number (hexadecimal) for the value. In this ValueDefinition you can define subvalues. These subvalues are valid everywhere the value is used. For example for the value ‘Leather Class A’ we could have subvalues: Red, Black, Brown. For these subvalues you can also specify the sort order and / or default value. This is the order in which the subvalues are displayed to the user of the catalog. If no sorting is specified, the sorting will be alphabetically.
All the values that are used in the catalog, are globally defined in this ValueGTINDefinitions tag.

We distinguish four types of values:

- VALUE: This is the most used type. This is a string, such as leather class A, blue, 200cm, Oak ...
- NUMERIC_RANGE_VALUE: the value must be selected from a given range with a certain step size. For example, width in cm between 80 and 100 with 2cm interval.
- FREE_VALUE: The value can be chosen freely by the end user. You can impose limits on its length.
- ARTICLE: The value is an article but used as a value.

> Note: a subvalue in this value definition, can’t contain subvalues.

Example:

```xml
<ValueDefinition type="Value">
    <GTIN>5412345000003</GTIN>
    <Description>Zacht</Description>
    <Description language="FR">Douce</Description>
    <Reference>5412345000003</Reference>
    <Color>#00FF00</Color>
</ValueDefinition>
<ValueDefinition type="Value">
    <GTIN>5412345000005</GTIN>
    <Description>A La Carte Klasse A</Description>
    <Description language="FR">A La Carte Classe A</Description>
    <Reference>5412345000005</Reference>
    <Color>#00FF00</Color>
    <Values>
        <Value order="010"default="true">5412345000008</Value>
        <Value order="020">5412345000009</Value>
    </Values>
</ValueDefinition>
<ValueDefinitiontype="Numeric_range_value">
    <GTIN>5412345000003</GTIN>
    <Reference>5412345000003</Reference>
    <From>80</From>
    <To>100</To>
    <StepSize>1</StepSize>
    <Default>90</Default>
</ValueDefinition>
<ValueDefinitiontype="Free_value">
    <GTIN>5412345000003</GTIN>
    <Reference>5412345000003</Reference>
    <MinLength>5</MinLength>
    <MaxLength>30</MaxLength>
</ValueDefinition>
    <ValueDefinitiontype="Article" useAsValue=”true”>
    <GTIN>5412345000003</GTIN>
    <Reference>5412345000003</Reference>
    <MinLength>5</MinLength>
    <MaxLength>30</MaxLength>
</ValueDefinition>
```

## Multimedia

This is the place to attach media (pictures, pdf, …). The number of items is unlimited. Media items can be attached to articles and choices. Every item is contained in a ‘MediaItem’ tag with the filename (Path) and the GTIN definition (GTINDefinition).

The following example contains 3 media items.

1. The first image “chair.jpg” is linked to the GTIN of an article
2. The second image ”pink.jpg” is linked to the GTIN of a choice regardless of the which article or option it is used in. This image could for example be shown for both a pink chair or a pink bed
3. The third image ”redchair.jpg” is linked to the GTIN of a choice for a specific article and option. The image could for example be shown for a red chair but not a red bed. The GTIN definition should have the following format: GTINArticle|GTINOption|GTINChoice (note the GTIN of the option must always be the top level option)

GTIN example:

```
1234500 Chair (article)
  1234501 Color (option)
    1234502 Pink (choice)
    1234503 Red (choice)
```

```xml
<Multimedia>
    <MediaItem>
        <Path>chair.jpg</Path>
        <GTINDefinition>1234500</GTINDefinition>
    </MediaItem>
    <MediaItem>
        <Path>pink.jpg</Path>
        <GTINDefinition>1234502</GTINDefinition>
    </MediaItem>
    <MediaItem>
        <Path>redchair.jpg</Path>
        <GTINDefinition>1234500|1234501|1234503</GTINDefinition>
    </MediaItem>
</Multimedia>
```
