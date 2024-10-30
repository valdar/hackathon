# Apache Camel F2F tooling hackathon ([go back](../README.md))

## Lab 2: Perform a data transformation

The goal of this lab is to perform a data transformation. We will use the Kaoto Data mapper component to showcase how we can easily achieve it using an UI and leveraging the Apache Camel functionality.

We're gonna start from [an existing Apache Camel Route (CreateOrder-template)](CreateOrder-template.camel.yaml) that simulates fetching information from external systems, namely [`Account`](xsd/Account.xsd) and [`Cart`](xsd/Cart.xsd), and then transforming and combining them into a new [`ShipOrder`](xsd/ShipOrder.xsd) format. For this purpose, appropriate XML schemas are provided in the `xsd` folder.

Here's an example of the [`Account`](xsd/Account.xsd) object:
```xml
    <kaoto:Account AccountId="acc001" xmlns:kaoto="kaoto.datamapper.test">
        <Name>Jane Doe</Name>
        <Address>Purkyňova 111, 612 00</Address>
        <City>Brno-Medlánky</City>
        <Country>Česká republika</Country>
    </kaoto:Account>
```

And the [`Cart`](xsd/Cart.xsd) object:
```xml
    <kaoto:Cart xmlns:kaoto="kaoto.datamapper.test">
        <Item>
            <Title>Shadowman T-shirts</Title>
            <Note>XL</Note>
            <Quantity>10</Quantity>
            <Price>25.00</Price>
        </Item>
        <Item>
            <Title>Kaoto T-shirts</Title>
            <Note>L</Note>
            <Quantity>5</Quantity>
            <Price>24.50</Price>
        </Item>
    </kaoto:Cart>
```

The desired output is the [`ShipOrder`](xsd/ShipOrder.xsd) object:
```xml
<ShipOrder xmlns="io.kaoto.datamapper.poc.test"
           xmlns:ns0="kaoto.datamapper.test"
           OrderId="ORDER-acc001-nnnn">
   <OrderPerson>acc001:Jane Doe</OrderPerson>
   <ShipTo xmlns="">
      <Name>Jane Doe</Name>
      <Address>Purkyňova 111, 612 00</Address>
      <City>Brno-Medlánky</City>
      <Country>Česká republika</Country>
   </ShipTo>
   <Item xmlns="">
      <Title>Shadowman T-shirts</Title>
      <Note>XL</Note>
      <Quantity>10</Quantity>
      <Price>25.00</Price>
   </Item>
   <Item xmlns="">
      <Title>Kaoto T-shirts</Title>
      <Note>L</Note>
      <Quantity>5</Quantity>
      <Price>24.50</Price>
   </Item>
</ShipOrder>
```

### Prerequisites

* Install [VS code](https://code.visualstudio.com/docs/setup/setup-overview).
* Install the [Extension pack for Apache Camel by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.apache-camel-extension-pack) in VS Code.
* [Install the Kaoto editor extension provided in the `vsix` folder](https://code.visualstudio.com/docs/editor/extension-marketplace#_install-from-a-vsix).
* Install [JBang CLI](https://www.jbang.dev/documentation/guide/latest/installation.html).

### Instructions


### DataMapper screenshots (to be used above)

![Append a DataMapper step](images/01.append-step.png)

![Select a DataMapper step in the catalog](images/02.select-datamapper-in-catalog.png)

![Click added DataMapper step](images/03.click-datamapper-step.png)

![Click DataMapper configure button](images/04.click-datamapper-configure-button.png)

![DataMapper canvas](images/05.datamapper-canvas.png)

![Click "Attach a schema" button for Target Body Document](images/06.click-attach-target-body-document-schema.png)

![Select "xsd/ShipOrder.xsd"](images/07.select-ShipOrder-schema.png)

![ShipOrder schema is attached to the Target Body Document](images/08.ShipOrder-attached-to-target-body.png)

![Click "Attach a schema" button for Source Body Document](images/09.click-attach-source-body-document-schema.png)

![Select "xsd/Card.xsd"](images/10.select-Cart-schema.png)

![Cart schema is attached to the Source Body Document](images/11.Cart-attached-to-source-body.png)

![Click "Add a parameter" button](images/12.click-add-parameter-button.png)

![Add a parameter "account"](images/13.add-parameter-account.png)

![Parameter "account" is added](images/14.parameter-account-added.png)

![Click "Attach a schema" button for the parameter "account"](images/15.click-attach-param-account-schema.png)

![Select "xsd/Account.xsd"](images/16.select-Account-schema.png)

![Account schema is attached to the parameter "account"](images/17.Account-attached-to-param-account.png)

![Click "Add a parameter" button](images/12.click-add-parameter-button.png)

![Add a parameter "orderSequence"](images/19.add-parameter-orderSequence.png)

![Parameter "orderSequence" added](images/20.parameter-orderSequence-added.png)

![Drag "/Account/Name" property of account parameter and Drop to Target "/ShipOrder/ShipTo/Name" ](images/21.dnd-name.png)

![Repeat Drag and Drop for "Address", "City" and "Country" ](images/22.repeat-dnd-address-city-country.png)

![Drag "/Account/@AccountId" of account parameter and Drop to Target "/ShipOrder/OrderPerson"](images/23.dnd-accountid-to-orderperson.png)

![Drag "/Account/Name" of account parameter and Drop to Target "/ShipOrder/OrderPerson"](images/24.dnd-name-to-orderperson.png)

![Click a pencil button of Target "/ShipOrder/OrderPerson" to open XPath Editor](images/25.click-pencil-orderperson.png)

![Click "Function" tab](images/26.xpath-editor-click-function-orderperson.png)

![Drag "Concatenate" and Drop onto right text editor](images/27.xpath-editor-drop-concat.png)

![The "concat()" function is applied in the XPath expression](images/28.xpath-editor-concat-applied.png)

![Edit the XPath expression and add colon ":" in between 2 fields](images/29.xpath-editor-add-colon.png)

![Close XPath Editor](images/30.xpath-editor-close.png)

![Drag "/Account/@AccountId" of account parameter and Drop to Target "/ShipOrder/@OrderId"](images/31.dnd-accountid-to-orderid.png)

![Drag "orderSequence" parameter and Drop to Target "/ShipOrder/@OrderId"](images/32.dnd-orderSequence-to-orderid.png)

![Click a pencil button of Target "/ShipOrder/@OrderId"](images/33.click-pencil-orderid.png)

![Click "Function" tab](images/34.xpath-editor-click-function-orderid.png)

![Drap "Concatenate" and Drop onto right text editor](images/35.xpath-editor-drop-concat-orderid.png)

![The "concat()" function is applied in the XPath expression](images/36.xpath-editor-concat-applied-orderid.png)

![Add "ORDER-" prefix as a string literal](images/37.xpath-editor-add-ORDER-prefix.png)

![Add "-" separator in between "AccountId" and "orderSequence"](images/38.xpath-editor-add-separator.png)

![Close XPath Editor](images/39.close-xpath-editor.png)

![Click 3-dot context menu of Target "/ShipOrder/Item"](images/40.click-context-menu-item.png)

![Select "Wrap with for-each"](images/41.select-for-each.png)

![Drag "/Cart/Item" of Source Body and Drop to Target "for-each"](images/42.dnd-item-for-each.png)

![Drag and Drop all children of "Item"](images/43.dnd-item-children.png)

![Click "Design" tab and go back to the Kaoto Canvas](images/44.click-design-tab.png)

![Kaoto Canvas](images/45.kaoto-canvas.png)
