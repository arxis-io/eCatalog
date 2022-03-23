# eCat 3.0 specification

Original: initeam  
Adjusted by: Arxis

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

## Programs

This tag contains the rest of the catalog definition. On root level we have the programs (sometimes called models or groups) with below: products for that program, program specific options and values, prices, ...

A program contains a group of items (= configurable products) with common attributes (eg type of products such as chairs, dining room, ... or products in a particular style – called model, ...). The classification of the catalog in programs, ensures a clear and neat catalog for the end user. (so that a product can easily be found in the catalog).
Therefore, a program can also consist of sub-programs. In this way, a tree structure can be built up of programs (with infinite number of levels, but make sure it remains workable for the user). Only the programs on the lowest level can contain products.
The program includes a code or GTIN, descriptions and a reference. The code must be unique throughout the catalog. (Eg. you can’t use the same code for a program and a product).

The definition of the products in the program, is discussed in the next chapter.

Example:

```xml
<Programs>
    <Program>
        <GTIN>EETKAMER</GTIN>
        <Description>EETKAMER</Description>
        <Description language="FR">Salle à manger</Description>
        <Reference>EETKAMER</Reference>
        <Program>
            <GTIN>BOSSA</GTIN>
            <Description>BOSSA</Description>
            <Description language="FR">BOSSA</Description>
            <Articles>…</Articles>
            <Configurations>…</Configurations>
        </Program>
        <Program>
            <GTIN>MALU</GTIN>
            <Description>MALU</Description>
            <Description language="FR">MALU</Description>
            <Articles>…</Articles>
            <Configurations>…</Configurations>
        </Program>
        <Program>
            <GTIN>SALON</GTIN>
            <Description>SALON</Description>
            <Description language="FR">SOFA</Description>
            <Reference>SALON</Reference>
        <Program>
            <GTIN>OHIO</GTIN>
            <Description>OHIO</Description>
            <Description language="FR">OHIO</Description>
            <Articles>…</Articles>
            <Configurations>…</Configurations>
        <Program>
            <GTIN>RIU</GTIN>
            <Description>RIU</Description>
            <Description language="FR">RIU</Description>
            <Articles>…</Articles>
            <Configurations>…</Configurations>
        </Program>
    </Program>
</Programs>
```

## Product structure in the XML catalog

Products are defined within a program. With the product, we mean the base product. This base product must be further configured (via options and values), in order to have a marketable product (SKU). It is of course also possible that this base product is a SKU on its own. (and there is no further configuration needed/possible).
The base product consist of a code (GTIN), descriptions (ie description in the default language and additional descriptions in other languages) and an optional reference. Again the code (GTIN) must be a unique value for the entire catalog. If possible, the base product can have dimensions specified (height, width, depth, sitting depth, sitting height).
Important note for the determination of the product description: It is recommended to choose a product description, which completely identifies the product. So the user recognizes the product, without the knowledge of the overlying program. Eg. It’s not recommended to choose the name 2-seater for a product within the program OHIO. Another product in the 'TORONTO' program can have that same description. It’s wiser to name the product ‘2-seater OHIO’.
It is possible to allocate a classification. There is a standard classification for the furniture sector: the VIV classification. The exact origin is unknown to us. Retailers can use this classification in eMeubel (and in other software) for example in product searches and reporting. Classification in the catalog is handy for the retailers (they don’t have to assign this themselves), but it’s not obligated. (You’ll find an overview of the VIV classification in the appendix)
Products can have a sort order. This determines the order in which the items appear within a particular program.
Weight, Volume, AssemblyTime, Intrastat, CountryOrigin, WeightPackaging, AmountCollis can also be defined in the product.
Country specific environmental taxes like eco-mobilier code (FR) and Valumat (BE).
A product can also be used as value. If this is the case, then the useAsValue attribute will be set. (see 2.2.6)

Example:

```xml
<Articles>
<Article order="010" useAsValue=”true”>
<GTIN>1234567890123</GTIN>
<Description>OHIO 2 zit met relax</Description>
<Description language="FR">OHIO 2 places avec relax</Description>
<Reference>2ZR</Reference>
<EcoMobilier>9876543</EcoMobilier>
<Valumat>01020106</Valumat>
<Size unit="CM">
<Height>80.00</Height>
<Width>200.00</Width>
<Depth>100.00</Depth>
<SittingDepth>100.00</ SittingDepth >
<SittingHeight>100.00</ SittingHeight >
</Size>
<Weigth unit="KG">10</Weigth>
<WeigthPackaging unit="KG">10</ WeigthPackaging>
<Volume unit="M³">2,365</Volume>
<AmountCollis>5</ AmountCollis>
<AssemblyTime unit="MIN">2</AssemblyTime>
<VIV>
<VIV1>OVMM_VIV1281</VIV1>
</VIV>
<Intrastat>12345678</Intrastat>
<CountryOrigin>BE</ CountryOrigin>
<Options>…</Options>
<Configurations>…</Configurations>
<Properties>…</Properties>
<References>…</ References>
<Prices>…</Prices>
</Article>
<Article>
…
</Article>
</Articles>
```

### Options and Values

These are the possible options (attributes) that come with the base product. This is defined by specifying the GTIN codes of the option as created in OptionGTINDefinitions. As mentioned above, we refer to the code and not the description.

For each option, the possible values are defined. Again, this is done by means of the GTIN codes. Remember that in the OptionGTINDefinitions and ValueGTINDefinitions you could have underlying values. These values are automatically inherited, so they don’t have to be repeated in this product definition.
Later on (see Configurations) we will see how to define sub values or exclude some of these inherited values.
The 'definition' tag is not mandatory, and is for internal use. This value can be ‘Article’ or ‘Program’ and indicates if the option (or value) was originally defined on product or program level
Further we have, a sort order and default value. (the same way as we have seen before).

Example

```xml
<Options>
<Option order="010"definition="Program">
<GTIN>5412345000004</GTIN>
<Values>
<Value order="010"definition="Article">
<GTIN>5412345000005</GTIN>
<Values>
<Value order="010"definition="Article">5412345000008</Value>
<Value order="020"definition="Article">5412345000009</Value>
</Values>
</Value>
<Value order="020"definition="Article">5412345000006</Value>
<Value order="030"definition="Program">5412345000007</Value>
</Values>
</Option>
<Optionorder="020">
…
</Option>
</Options>
```

### Configurations

In the configurations we can specify the exception rules or extra features in the product attributes (option/values). This will only be used in a more complex catalog. For most of the catalogs this will not be necessary (Then you can skip this topic). Configuration rules can also be defined at program level. Then these rules will be valid for all products in the program.

Each configuration rule has two possible actions: Add or delete (Remove).
You must define the option on which the rule is applicable and the value that has to be added or removed. As to expect, this is done by referring to the GTINs of the option and value.

A first example of the use of such rule, is for the subvalue. In the ValueGTINDefinition you can define subvalues for a particular value. Each product that uses the value, inherit also these subvalues. But in some circumstances, this subvalue may not be a possible choice for that product. The you can add a removal rule.
In the tag 'OptionToConfigure' you define GTINs that contains the (sub)value that has to be removed. If this is a subvalue than OptionToConfigure is composed of the option followed by value GTIN, separated by the pipe sign (|). ValueToConfigure contains the actual value that has to be removed.

Example:  
The value ‘Red’ is not possible within a leather class for a certain product.

```xml
<Configurations>
<Configuration action="Remove">
<OptionToConfigure>Tissue|Leather class A</OptionToConfigure>
<ValueToConfigure>Red</ValueToConfigure>
</Configuration>
</Configurations>
```

Example:  
For the value ‘blue’ you can have extra choices (subvalues) for a particular product. Via a configuration add-rule you can define these subvalues.

```xml
<Configurations>
<Configuration action="Add"order="010">
<OptionToConfigure>Tissue|Leather A|Blue</OptionToConfigure>
<ValueToConfigure>Azure</ValueToConfigure>
</Configuration>
<Configuration action="Add"order="020">
<OptionToConfigure> Tissue|Leather A|Blue </OptionToConfigure>
<ValueToConfigure>Sky</ValueToConfigure>
</Configuration>
</Configurations>
```

> Note: for readability reasons we used descriptions instead of GTINs

Secondly, configuration rules can also be used to define dependencies. A certain value can only be chosen, depending on other, previous selected values. The first part of the definition is identical as above (OptionToConfigure and ValueToConfigure), but there is an extra conditional part, defined in the Condition tag. This is only possible with the action ‘Remove’. Means that first you’ll have to ‘add’ a value, and then you can define conditional ‘remove’ rules.

Example:  
Upholstery of the cushions is not possible in leather class B, if the sofa itself is in a leather class A or C upholstery and has a firm seating comfort.

```xml
<Configurations>
<Configuration action="Remove">
<OptionToConfigure>Upholstery cushions</OptionToConfigure>
<ValueToConfigure>leather class B</ValueToConfigure>
<Conditions type="IF CONDITIONS MET">
<Condition>
<Option>Upholstery</Option>
<Values>
<Value>Leather A</Value>
<Value>Leather C</Value>
<Value>Leather D|Black</Value>
<Value>Leather D|White</Value>
</Values>
</Condition>
<Condition>
<Option>Seating comfort</Option>
<Values>
<Value>Firm</Value>
</Values>
</Condition>
</Conditions>
</Configuration>
</Configurations>
```

> Note: for readability reasons we used descriptions instead of GTINs

Explanation of the example:

- Attribute action = Remove. This means that the value leather class B will be removed if the conditions are fulfilled. (the action ‘Add’ is not possible at this moment)
- The condition type = ‘IF CONDITIONS MET’. The value ‘‘IF CONDITIONS NOT MET’ is also possible. Which type to choose, depend on the situation: sometimes the one or the other will be clearer, more comprehensible, or resulting in less rules. All conditions must be valid before the rule will apply.
- In the condition values you can use sub values. Then you have to concatenate all the values with the pipe sign (|)
- The number of conditions is unlimited.

If you want to be compatible with CSA-PRICAT, then the sort order of the conditions and options are important. You must always apply a dependency rule on the last option/value in the dependency.
For eMeubel this is not obligated.

Conclusion  
The use of configuration rules is not obligated and can sometimes be avoided. It depends on how you define your product and option/values. The more you split up your product in base product en values, the more change you need configuration rules. The more options a product has, the more the end users has to click en select.
Less options means sometimes a lot of values to choose from. And this means the end users has to search more, it’s less clear and neat for him/her.

### Properties

For each value of an item, you can add specific properties. If, for example, a certain option has an additional weight or adds a few centimeters to the total height, you can indicate this in this part. To work, the properties must have an option/value combination. As for the items, it is possible to add dimension data (width, height, depth, sittingdepth, sittingheight). It is also possible to add the volume, the number of collis, the weight, the assembly time, the intrastat code.
Country specific environmental taxes like eco-mobilier code (FR) and Valumat (BE).
The different dimensions, the different weights, the assembly time, the volume and the number of collis will be added to those of the other values/options and the item. For example, if the width of the item width is 100 cm and the width of the value is 20 cm, the total width of the article during the creation will be 120 cm.

Example:

```xml
<Properties>
<Property>
<Option>1234567890123|21325454545454</ Option >
	<Value>1234567890321</ Value >
<Size unit="CM">
<Height>80.00</Height>
<Width>200.00</Width>
<Depth>100.00</Depth>
<SittingDepth>100.00</ SittingDepth >
<SittingHeight>100.00</ SittingHeight >
</Size>
<Intrastat>12345678</Intrastat>
<EcoMobilier>123456</EcoMobilier>
<Valumat>01020106</Valumat>
<Volume unit="M³">2,365</Volume>
<AmountCollis>3</ AmountCollis>
<AssemblyTime unit="MIN">2</AssemblyTime>
<Weigth unit="KG">10</Weigth>
<WeigthPackaging unit="KG">1</ WeigthPackaging>
</Property>
</Properties>
```

### References

On each product, option, value you can add your own reference number. But sometimes your reference number can be a combination of your product and a value, and is indivisible. Then you can use this tag. (reference number can be very important if you want to import the purchase orders from the retailer).

Note: the reference number can only be a combination of a product and one value. In eMeubel this reference is concatenated with the reference number of the base product (if one).

Example:

```xml
<References>
<Reference>
<Option>Upholstry</Option>
<Value>Blue</Value>
<ArticleReference>A123456</ ArticleReference>
</Reference>
<Reference>
<Option>Upholstry</Option>
<Value>Red</Value>
<ArticleReference>B33556</ArticleReference>
</Reference>
</References>
```

### Prices

Until this point, we only have spoken about configuration (options and values), but not yet about pricing. All prices must be defined within this tag.

We have 2 types:

- salesprices (sales price of the supplier, purchase price for the vendor). This is always VAT exclusive
- retailprice (price in shop, consumer end price). This is VAT inclusive
  You can define a price for the base product, prices for values or prices for a combination of values.

When there are several prices for a product (SKU) with selected values, then all prices are added in order to get the total price for the product.
You could also use a percentage. This percentage will be applied on the total price of the product. (this is not commonly used)

Example

```xml
<Prices>
<Price>
<SalesPrice>450,00</SalesPrice>
<RetailPrice>900,00</RetailPrice>
<SurchargePrecentage>0</SurchargePrecentage>
</Price>
<Price>
<SalesPrice>50,00</SalesPrice>
<RetailPrice>100,00</RetailPrice>
<SurchargePrecentage>0</SurchargePrecentage>
<Conditions>
<Condition>
<Option>Upholstery</Option>
<Value>Leather A</Value>
</Condition>
</Conditions>
</Price>
<Price>
<SalesPrice>150,00</SalesPrice>
<RetailPrice>300,00</RetailPrice>
<SurchargePrecentage>0</SurchargePrecentage>
<Conditions>
<Condition>
<Option> Upholstery|Leather A</Option>
<Value>Blue</Value>
</Condition>
<Condition>
<Option>Seating comfort</Option>
<Value>Firm</Value>
</Condition>
<Condition>
<Option>Upholstery cushions|leather A</Option>
<Value>Black</Value>
</Condition>
</Conditions>
</Price>
</Prices>
```

> Note: for readability reasons we used descriptions instead of GTINs

### Product used as value

A product GTIN can also be used as a value within an option. The only thing this does, is using the same GTIN code and descriptions of the product, for the value. This GTIN act as a normal value. This value doesn’t act as a sub-product configuration. If the product itself has a options and values, these are not inherent within the value of the other, main product.
Same for the prices. A price defined on the product, doesn’t count on the value. If this value has a price, then this is and must be defined like the other prices on the values.
This rule/attribute can’t be used to define a kind of ‘super configuration’. The product act as a normal value does.

To define that a product can also be used as a value, you’ll have to add the attribute ‘useAsValue’ within the ‘Article’ tag.

```xml
<Article order="010" useAsValue="true">
```

In the value definition, you’ll have to set the type to ‘Article’ and use the same attribute ‘useAsValue’.

```xml
<ValueDefinitiontype="Article" useAsValue="true">
```
