# ECAT 2.3

Download and use a Dummy Catalog in ECAT 2.3 format here: [Dummy Catalogus ECAT 2.3](https://github.com/Arxis-io/eCatalog/blob/main/ECAT/2.3%20%5BGeneral%2C%20AdditionalDescription%2C%20PromoPrice%2C%20Color%2C%20Brand%5D/Sample/Dummy%20Catalogus%20ECAT2.3%20.xml)

## Extended with:

- More details in General
- AdditionalDescription
- PromoPrices
- Color
- Brand
- Multi Supplier support

The General section has been expanded by Arxis to service the option to extend and enrich the catalog with Receiver & Sender identification, version-numbering en availibility information.

Next to this, we’ve agreed on a text-field tag “Additional Description” to have an extra slot for product-information. As we see a few more ‘flat’ article suppliers enter our eco-system the need for ‘promo-prices’ rose as well. A supplier can use this to communicate two different retail prices. 
Then we extend with the option to communicate the brand name and the color of the article in a separate tag as well. 
The Multi Supplier support has been added to support the possibility to bundle multiple suppliers and their articles into one catalog. Mainly used by RSO's like VME, Interring i.e.


### Example General

```xml
Catalog>
<General>
	<Receiver>
		<ArxisID>10000010</ArxisID> 
		<!-- [Arxis ID from Retailer] [Arxis always fills]-->
		<GLN>8713782213835</GLN> 
		<!-- [GLN from Retailer] [If empty: Arxis might fill]-->
    <CustomerID>31_LS</CustomerID> 
	  <!-- [Debtornumber retailer] [If empty: Arxis might fill with identification number]-->
	</Receiver>
	
	<Sender>
		<ArxisID>10000019</ArxisID> 
		<!-- [Arxis ID from Supplier] [Arxis always fills]-->
		<Name>LS Bedding</Name> 
		<!-- [Name Supplier] [Arxis always fills]-->
		<GLN>5414662999971</GLN> 
		<!-- [GLN from Supplier] [Arxis always fills, if added to Arxis]-->
	</Sender>
	<Name>Catalogus Naam</Name> 
	<!-- [Catalog title] [If empty: Arxis always fills]-->
	<Version>Catalog v201106</Version> 
	<!-- [Catalog version number from supplier]-->
	<ArxisCatalogNR>6</ArxisCatalogNR > 
	<!-- [Version, Follownumber from Arxis] [Arxis always fills]-->	
	<Validfrom>20110101</Validfrom> 
	<!-- [If empty: Arxis might fill, with availability dates on Arxis]-->
	<Validto>20111231</Validto> 
	<!-- [If empty: Arxis might fill, with availability dates on Arxis]-->
	<Currency> 
		<SalesPrice>EUR</SalesPrice>
		<!-- [From Supplier, or Default = EUR]-->
		<RetailPrice>EUR</RetailPrice> 
		<!-- [From Supplier, or Default = EUR]-->
	</Currency>
	<Settings> 
		<DefaultLanguage>NL</DefaultLanguage> 
		<!-- [From Supplier, or Default = NL]-->
	</Settings>
</General> 

```

### Example AdditionDescription, PromoPrice, Color and Brand

Below is an example article which has been enriched by all four new tags as described above.


```xml
  <Article>
          <AdditionalDescription>badmatten Vandyck SCALA LUXURY BATHMAT</AdditionalDescription>
 	  <GTIN>8718471010177</GTIN>
          <Reference>BAAM11203 4 5</Reference>
          <Weight>0.00</Weight>
          <Description language="NL">badmatten Vandyck SCALA LUXURY BATHMAT white toiletmat 55x55</Description>
          <Prices>
            <Price>
              <SalesPrice>0</SalesPrice>
              <RetailPrice>99.95</RetailPrice>
              <PromoPrices>
                <PromoPrice>
                  <Price>79.95</Price>
                </PromoPrice>
              </PromoPrices>
            </Price>
          </Prices>
          <CountryOrigin>Portugal</CountryOrigin>
          <Brand>Vandyck</Brand>
          <Color>white 4</Color>
          <Size>
            <Height>0.00</Height>
            <Width>0.00</Width>
            <Depth>0.00</Depth>
          </Size>
          <SupplierArticleDescription>white toiletmat 55x55</SupplierArticleDescription>
        </Article>

```

### Multi Supplier support

Multi Supplier support means 2 things:
1. Supplier Definitions
Describe and detail suppliers within the catalog XML with name, GTIN, address and contact information.
2. Supplier GTIN on article level
For every article, a Supplier GTIN is added, to communicate from which supplier (that is described under Supplier Definitions), the article is.

```xml
<SupplierDefinitions>
<Supplier>
<GTIN>9999990000019</GTIN>
<Name>Polipol International GmbH Bereich Hukla</Name>
<Street>Diepenauer Heide</Street>
<HouseNumber>1</HouseNumber>
<ZipCode>31603</ZipCode>
<City>Diepenau</City>
<CountryCode/>
<Fax/>
<Email>d.stork@polipol-international.com</Email>
</Supplier>
<Supplier>
<GTIN>9999990000071</GTIN>
<Name>Meubar (via V.D.M. Meubelagenturen bv)</Name>
<Street>Industriestraat</Street>
<HouseNumber>9</HouseNumber>
<ZipCode>B8211</ZipCode>
<City>Aartrijke</City>
<CountryCode/>
<Fax/>
<Email>made1000@planet.nl</Email>
</Supplier>

And an Article:

<Article>
<GTIN>9810013194044</GTIN>
<SupplierGTIN>9999990000019</SupplierGTIN> !This article belongs to the supplier, identified by this GTIN
<Description>Relaxfauteuil Anne</Description>
<SupplierArticleDescription/>
<Reference>HU-CA19032</Reference>
<Color/>
<Classification>000</Classification>
<Size unit="st">
<Height>000</Height>
<Width>000</Width>
<Length>000</Length>
</Size>
<Options>
<Option>

```

