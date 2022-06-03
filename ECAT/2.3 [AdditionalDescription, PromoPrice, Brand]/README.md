# ECAT 2.3


## Extended with:

- AdditionalDescription
- PromoPrices
- Brand

There was a urgent need for more room for article descriptions. We’ve agreed on a text-field tag “Additional Description” to have an extra slot for product-information. As we see a few more ‘flat’ article suppliers enter our eco-system the need for ‘promo-prices’ rose as well. A supplier can use this to communicate two different retail prices. 
And the third addition to this version is to communicate the brand name of the article in a separate field as well. Especially in case of multi-supplier or multi-brand catalogs, this is a necessary addition.


### Example

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

