# ECAT 2.3


## Extended with:

- More details in General
- AdditionalDescription
- PromoPrices
- Color
- Brand

The General section has been expanded by Arxis to service the option to extend and enrich the catalog with Receiver & Sender identification, version-numbering en availibility information.

Next to this, we’ve agreed on a text-field tag “Additional Description” to have an extra slot for product-information. As we see a few more ‘flat’ article suppliers enter our eco-system the need for ‘promo-prices’ rose as well. A supplier can use this to communicate two different retail prices. 
Then we extend with the option to communicate the brand name and the color of the article in a separate tag as well. 


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

Below is an example article which has been enriched by all three new tags as described above.


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