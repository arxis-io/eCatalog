This is a simple example of an XML catalog with products without options. These products don’t need to be configured.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Catalog>
  <General>
    <Sender>
      <ID>2003151308537</ID>
      <Name>Leverancier</Name>
    </Sender>
    <Version>Catalog v201106</Version>
    <Validfrom>20110101</Validfrom>
    <Validto>20111231</Validto>
    <Currency>
      <SalesPrice>EUR</SalesPrice>
      <RetailPrice>EUR</RetailPrice>
    </Currency>
  </General>
  <Programs>
    <Program>
      <Description>Tafels</Description>
      <Articles>
        <Article>
          <GTIN>THR</GTIN>
          <Description>Tafel Hondo Rond</Description>
          <Prices>
            <Price>
              <SalesPrice>450,00</SalesPrice>
              <RetailPrice>900,00</RetailPrice>
            </Price>
          </Prices>
        </Article>
        <Article>
          <GTIN>THV</GTIN>
          <Description>Tafel Hondo Vierkant</Description>
          <Prices>
            <Price>
              <SalesPrice>475,00</SalesPrice>
              <RetailPrice>950,00</RetailPrice>
            </Price>
          </Prices>
        </Article>
        <Article>
          <GTIN>THR</GTIN>
          <Description>Tafel Mona</Description>
          <Prices>
            <Price>
              <SalesPrice>350,00</SalesPrice>
              <RetailPrice>700,00</RetailPrice>
            </Price>
          </Prices>
        </Article>
      </Articles>
    </Program>
    <Program>
      <Description>Stoelen</Description>
      <Articles>
        <Article>
          <GTIN>SH</GTIN>
          <Description>Stoel Hondo</Description>
          <Prices>
            <Price>
              <SalesPrice>75,00</SalesPrice>
              <RetailPrice>150,00</RetailPrice>
            </Price>
          </Prices>
        </Article>
        <Article>
          <GTIN>THR</GTIN>
          <Description>Stoel Mona</Description>
          <Prices>
            <Price>
              <SalesPrice>55,00</SalesPrice>
              <RetailPrice>110,00</RetailPrice>
            </Price>
          </Prices>
        </Article>
      </Articles>
    </Program>
  </Programs>
</Catalog>
```
