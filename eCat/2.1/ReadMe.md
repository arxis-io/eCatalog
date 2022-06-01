# ECAT 2.1

Extended with: 
‘useAsValue’


A product GTIN can also be used as a value within an option. The only thing this does, is using the same GTIN code and descriptions of the product, for the value. This GTIN act as a normal value. This value doesn’t act as a sub-product configuration. If the product itself has a options and values, these are not inherent within the value of the other, main product.

Same for the prices. A price defined on the product, doesn’t count on the value. If this value has a price, then this is and must be defined like the other prices on the values. This rule/attribute can’t be used to define a kind of ‘super configuration’. The product act as a normal value does.


To define that a product can also be used as a value, you’ll have to add the attribute ‘useAsValue’ within the ‘Article’ tag.

<Article order="010" useAsValue=”true”>
  
In the value definition, you’ll have to set the type to ‘Article’ and use the same attribute ‘useAsValue’.
<ValueDefinitiontype="Article" useAsValue=”true”>
