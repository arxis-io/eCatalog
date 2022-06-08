# ECAT 2.2

Download and use a Dummy Catalog in ECAT 2.0 format here: [Dummy Catalogus ECAT 2.2](https://github.com/Arxis-io/eCatalog/blob/main/ECAT/2.2%20%5BValumat%5D/Dummy%20Catalogus%20ECAT2.2%20.%20xml)

## Extended with:

- Valumat 'Belgian eco tax for matrasses'

Similar to the already included <EcoMobilier> code that is widely used in France, the <Valumat> is supporting the ability to communicate the Belgian environmental tax information that belongs to the article included in the catalog.

```xml
<Article order="010" useAsValue=”true”>
	<GTIN>1234567890123</GTIN>
	<Description>OHIO 2 zit met relax</Description>
<Description language="FR">OHIO 2 places avec relax</Description>
<Reference>2ZR</Reference>
<EcoMobilier>9876543</EcoMobilier>			
### <Valumat>01020106</Valumat>
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
```
