# eCat 3.1

eCat 3.1 is the successor of eCat 3.0. Please check below to see what's added or changed. To see the 3.0 spec please check [eCat 3.0 Specification](../3.0/Spec.md).

## Changelog

- Added unit
- Added classification

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
