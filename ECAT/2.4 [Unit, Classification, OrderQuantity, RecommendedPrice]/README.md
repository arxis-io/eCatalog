# ECAT 2.4


## Extended with:

- Unit
- Classification
- OrderQuantity
- RecommendedPrice

### Unit

```xml
<Article order="0">
    <GTIN>9917003309734</GTIN>
    <Description>PVC 4501</Description>
    <Reference>997450119</Reference>
    <Unit>PAK</Unit>
...
</Article>
```

Unit is used to indicate what the basic unit is for this article. This can be used as purchase unit (so the retailer sends the order to the supplier) but also as sales unit (the retailer sells it to a customer)
There is no validation for the value because there is no good (ISO) standard for this yet.

### Classification

```xml
<Article order="0">
    <GTIN>9917003309734</GTIN>
    <Description>PVC 4501</Description>
    <Reference>997450119</Reference>
    <Classification>001</Classification>
...
</Article>
```

Classification can be used for multiple purposes. It some sort of extra way of categorizing or classifying an article. One example is that it can be used to communicate the sales group (omzetgroep) but the retailer is not obliged to respect this, its more extra, optional data than can be used.


### OrderQuantity

OrderQuantity can be used by a supplier to communicate the minimum/standard amount of articles delivered. When for instance ordering a certain type of chairs, when a supplier communicated an OrderQuantity of 4, every order will be handled in batches of 4.




### RecommendedPrice

A fourth price type is introduced (next to salesprice, retailprice & promoprice).
Specifically in ecommerce cases and dropshipment scenarios this is useful to automatically let the supplier fill certain "From, For" (Van, Voor) prices as widely used in our industry.
