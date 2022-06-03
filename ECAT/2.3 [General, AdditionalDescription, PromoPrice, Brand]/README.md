# ECAT 2.3


## Extended with:

- More details in General
- AdditionalDescription
- PromoPrices
- Brand

The General section has been expanded by Arxis to service the option to extend and enrich the catalog with Receiver & Sender identification, version-numbering en availibility information.

Next to this, we’ve agreed on a text-field tag “Additional Description” to have an extra slot for product-information. As we see a few more ‘flat’ article suppliers enter our eco-system the need for ‘promo-prices’ rose as well. A supplier can use this to communicate two different retail prices. 
And the third addition to this version is to communicate the brand name of the article in a separate field as well. Especially in case of multi-supplier or multi-brand catalogs, this is a necessary addition.


### Example <General>

```xml
Catalog>
<General>
	<Receiver>
		<ArxisID>10000010</ArxisID> 
		<!-- [Arxis ID van Retailer] [Vult Arxis altijd]-->
		<GLN>8713782213835</GLN> 
		<!-- [GLN van Retailer] [Indien leeg: Arxis vult eventueel aan]-->
	</Receiver>
	<CustomerID>31_LS</CustomerID> 
	<!-- [Debiteurnummer retailer] [Leeg: Arxis vult eventueel aan met identificatienummer]-->
	<Sender>
		<ArxisID>10000019</ArxisID> 
		<!-- [Arxis ID van Leverancier] [Vult Arxis altijd]-->
		<Name>LS Bedding</Name> 
		<!-- [Naam van Leverancier] [Vult Arxis altijd]-->
		<GLN>5414662999971</GLN> 
		<!-- [GLN van Leverancier] [Vult Arxis altijd]-->
	</Sender>
	<Name>Catalogus Naam</Name> 
	<!-- [Titel van catalogus] [Vult Arxis indien leeg]-->
	<Version>Catalog v201106</Version> 
	<!-- [Versienummer van Leverancier]-->
	<ArxisCatalogNR>6</ArxisCatalogNR > 
	<!-- [Versie/Volgnummer van Arxis] [Vult Arxis altijd]-->	
	<Validfrom>20110101</Validfrom> 
	<!-- [Leeg: Arxis vult eventueel aan, met beschikbaarheidsdata van Arxis]-->
	<Validto>20111231</Validto> 
	<!-- [Leeg: Arxis vult eventueel aan, met beschikbaarheidsdata van Arxis]-->
	<Currency> 
		<SalesPrice>EUR</SalesPrice>
		<!-- [Of van Leverancier, of Default = EUR]-->
		<RetailPrice>EUR</RetailPrice> 
		<!-- [Of van Leverancier, of Default = EUR]-->
	</Currency>
	<Settings> 
		<DefaultLanguage>NL</DefaultLanguage> 
		<!-- [Of van Leverancier, of Default = NL]-->
	</Settings>
</General> 


#### Example AdditionDescription, PromoPrice and Brand

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
          <Color>white      4</Color>
          <Size>
            <Height>0.00</Height>
            <Width>0.00</Width>
            <Depth>0.00</Depth>
          </Size>
          <SupplierArticleDescription>white toiletmat 55x55</SupplierArticleDescription>
        </Article>

```

