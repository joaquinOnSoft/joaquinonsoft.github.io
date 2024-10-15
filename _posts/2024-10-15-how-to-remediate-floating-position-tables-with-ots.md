---
title: "How to remediate floating position tables with OTS?"
header:
  image: /images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/01-floating-position-table.png
  og_image: /images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/01-floating-position-table.png
tags:
  - OpenText
  - Output Transformation Server
  - OTS
last_modified_at: 2024-10-15T08:34:18-04:00
---

Sometimes we found bills, contracts... that **contains floating position tables**. How can we remediate these tables using Output Transformation Server?

In this example the table called “REPLIEGO IMPORTI” always appear on the right-hand side of the title *"IMPORTO"*.
Let’s define 3 locators do manage this floating table

![Floating position table](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/02-floating-position-table.png)

We are going a define 3 locators:

 - **importoLoc**: to detect if `the table is present in the current page`
 - **riepilogoImportiLoc**:  to identify the `first row` of the table
 - **totaleDocumentoLoc**:  to identify the `last row` of the table

![floating position on a page](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/01bis-locators.png)

## "Importo" Text locator 

Follow these steps to create the first locator:

 - Right click on `Field Technology > Locator`
 - Select Add Text Locator

![Add locator](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/03-add-locator.png)

 - Creates a rectangle around the area where the title “IMPORTO” can appear
 
![Add locator. Define area](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/04-add-locator-define-area.png) 
 
 - Provide the following information in the `XFT Text Locator Properties` pop-up:
   - **Name**: importoLoc
   - **Literal Text**: Importo
 - Click on `OK` button

![Importo text locator](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/05-importo-text-locator.png)

## "Repielogo importi" table header - Text locator 

Lets create the second locator:

 - Right click on `Field Technology > Locator`
 - Select `Add Text Locator`

![Add locator. Riepilogo importi](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/06-add-locator-riepilogo-importi.png)

 - Creates a rectangle around the area where the table header `"Repielogo importi"` can appear

![Add locator. Define area](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/07-add-locator-define-area.png)

 - Provide the following information in the `XFT Text Locator Properties` pop-up:
   - **Name**: riepilogoImportiLoc
   - **Literal Text**: RIEPILOGO IMPORTI
 - Click on `OK` button

![Riepilogo importo, text locator](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/08-riepilogo-importo-text-locator.png)

## "Totale Documento" table footer - Text locator 

And the last locator:

 - Right click on `Field Technology > Locator`
 - Select `Add Text Locator`

![Add locator](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/09-add-locator-totale-documento.png)

 - Creates a rectangle around the area where the table footer `"Totale Documento"` can appear

![Add locator "Totale documento". Define area](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/10-add-locator-totale-documento-define-area.png)

 - Provide the following information in the `XFT Text Locator Properties` pop-up:
   - **Name**: totaleDocumentoLoc
   - **Literal Text**: Totale Documento
 - Click on `OK` button

!["Totale documento" text locator](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/11-totale-documento-text-locator.png)

## Remediating the floating table

Now that we have created the *locators* we can complete the configuration to manage the floating table:

 - Right click on `Field Technology > Document`
 - Select `Add Document`

![Add document](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/12-add-document.png)

 - Provide a name in the `XFT Document pop-up`, i.e. *doc* - 

![SFT document](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/13-xft-document.png)

 - Click `OK`
 - Click `No` in the `Auto Detect Sections` pop-up
 
![Autodetect sections NO](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/14-autodetect-sections-no.png)

 - Right click on `Field Technology > Documen > doc`
 
 
 ![Add section](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/15-add-section.png)
 
 - Creates a rectangle around the document body

 ![Add section body](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/16-add-section-body.png)

 - Provide a name, i.e. `"body"` in the `XFT Section Properties` pop-up.

 ![SFT section properties](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/17-xft-section-properties.png)

 - Click `Ok`
 - Click `No` in the `"Auto detect fields?"` pop-up
 
 ![Auto detect fields](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/18-auto-detect-fields.png)
  
 - Right click on `Documents > doc > body`
 - Click on `Add Table`

 ![Add table](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/19-add-table.png)

 - Creates a rectangle around the area where the table “Riepilogo Importi” appears

 ![Add table](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/20-add-table.png)

 - Provide a name, i.e. `"riepilogoImportiTable"`in the `XFT Table Field Properties`
 - Check `"Enable Start Identification"`

 ![XFT table field properties](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/21-xft-table-field-properties.png)

 - Click on  `"Enable Start"` tab
 - Click on `+` icon
 - Select `importoLoc` in the `Locator Name` drop down list
 - Check `required`

 ![Start identification](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/22-start-identification.png)

 - Click on  `"General"` tab
 - Check `"Enable Dynamic Positioning"`

 ![Enable dynamic positioning](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/23-enable-dynamic-positioning.png)

 - Click on  `"General"` tab
 - Check `"Enable Dynamic Positioning"`
 - Click on  `"Positioning"` tab
 
 ![Enable dynamic positioning](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/23-enable-dynamic-positioning.png)
 
 - Click on  `+` icon
 - Select `"riepilogoImportiLoc"` in the `Locator Name` drop down list
 - Set `-20000` on `Offset Top` field
 
 - Click on  `+` icon
 - Select `"totaleDocumentoiLoc"` in the `Locator Name` drop down list
 - Set `20000` on `"Offset Bottom"` field

 - Check  `Top`
 - Select `Highest` in the `LocationToUse` drop down list
 - Check  `Bottom (Height)`
 - Select `Lowest` in the `LocationToUse` drop down list

 ![positioning](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/24-positioning.png)


## Defininig table headers (TH)

Lets define the first row as a header:

 - Click on  `Cells` tab
 - Richt click on `ColumnHeaderRows[0]`
 - Click on  `+` Add
 - Set value `1` in the `[0]` field

 ![table cells](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/25-cells.png)

 - Click `OK`

Now `TH` tag is used in the cells of the first row.

 ![table cells](/images/2023-10-15-how-to-remediate-floating-position-tables-with-ots/26-table-with-headers.png)











