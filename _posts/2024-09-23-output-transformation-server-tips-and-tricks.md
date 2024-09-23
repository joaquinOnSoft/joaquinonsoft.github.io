---
title: "OpenText Output Transformation Server: tips and tricks"
header:
  image: /images/2024-09-23-output-transformation-server-tips-and-tricks/01-p-tag-used-in-header.png
  og_image: /images/2024-09-23-output-transformation-server-tips-and-tricks/01-p-tag-used-in-header.png
tags:
  - OpenText
  - Output Transformation Server
  - OTS
last_modified_at: 2024-09-23T08:34:18-04:00
---

This article explains how to solve some common challenges that you can face during a 
document remediation using OpenText Output Transformation Server.

> **OpenText Output Transformation Server (OTS)** automatically transform and present archived content 
> in a highly usable, navigable and accessible format, such as PDF/UA, for persons 
> requiring accessible technologies.

## `<P>` tag is used in a header  when Auto Detect is enabled

Follow this steps to set the tag that you want to use to auto-tag your document title:
 
 - Select the `PDF Parser component`

![PDF parser component](/images/2024-09-23-output-transformation-server-tips-and-tricks/02-pdf-parser-component.png)

 - Click on one character of your title

![Select one character from the title](/images/2024-09-23-output-transformation-server-tips-and-tricks/03-select-one-character-from-the-title.png)

 - Double click on the character on the left-hand side
 - Copy/remember the font `Name` and `Point Size` used

![xif text element](/images/2024-09-23-output-transformation-server-tips-and-tricks/04-xif-text-element.png)

 - Back to `XFT Field Technology component`
 - Edit the field that contains your title

![Edit field](/images/2024-09-23-output-transformation-server-tips-and-tricks/05-edit-field.png)

 - Click on `Auto Detect` tab
 - Click on `[…]` close to `Auto Detect Options`

![Auto detect options](/images/2024-09-23-output-transformation-server-tips-and-tricks/06-auto-detect-options.png)

 - Edit `Default` auto detect options

![Auto detect options: edit](/images/2024-09-23-output-transformation-server-tips-and-tricks/07-auto-detect-options-edit.png)

 - Right click on `TextToTag[0]`
 - Click on `+ Add`

![Auto detect options: text to tag](/images/2024-09-23-output-transformation-server-tips-and-tricks/08-auto-detect-options-text-to-tag.png)

 - Set the following values:
    - **Name**: H1
    - **TagType**: H1
    - **FonCompare**:
    - **Fontname**: CIDFont\+F1-Asc
    - **PtSizes**: 15-16

![Text to tag](/images/2024-09-23-output-transformation-server-tips-and-tricks/09-text-to-tag.png)


> **NOTE**: Original font name is “CIDFont+F1-Asc”. You must  scape `+` character adding a `\` before it.
>
> `PtSizes` admit exact values and ranges. We are using a range because the exact value is a decimal number.

 - Click `OK` button
 - Click `OK` at the `Auto Detect Options` popup
 - Click `OK` at the `XFT Container properties` popup

![XFT Container properties](/images/2024-09-23-output-transformation-server-tips-and-tricks/10-xft-container-properties.png)

Now the title is using a H1 tag in the DOM tree.

![Header using H1](/images/2024-09-23-output-transformation-server-tips-and-tricks/11-header-using-h1.png)

## A paragraph is split in multiple `<p>` tags when Auto Detect is enabled

A paragraph is split in multiple `<p>` tags when Auto Detect is enabled and you want to use just one `<p>` tag for the full paragraph. 
How you can do it?. Just follow these steps:

![A paragraph is split in multiple p tags when Auto Detect is enabled](/images/2024-09-23-output-transformation-server-tips-and-tricks/12-paragraph-is-split-in-multiple-p-tags-when-auto-detect-is-enabled.png)

 - Edit the field that contains your paragraph

![Edit P1](/images/2024-09-23-output-transformation-server-tips-and-tricks/13-edit-p1.png)

 - Click on `Auto Detect` tab
 - Click on `[…]` close to `Auto Detect Options`

![Edit P1](/images/2024-09-23-output-transformation-server-tips-and-tricks/13a-xft-container-properties.png)

 - Edit Default auto detect options

![Edit P1](/images/2024-09-23-output-transformation-server-tips-and-tricks/13b-auto-detect-options.png)

 - Click on `TextJoining` parameter
 - Edit `LineJoinUnderRation`
   - Set value to `1.5` 

![Auto Detect Options](/images/2024-09-23-output-transformation-server-tips-and-tricks/14-auto-detect-options-textjoininig.png)

 - Click OK at the `Auto Detect Options` popup
 - Click OK at the `XFT Container properties` popup

![Auto Detect Options](/images/2024-09-23-output-transformation-server-tips-and-tricks/14a-xft-container-properties.png)

Now the paragraph is merged, using only one <p> tag for the full paragraph

![Auto Detect Options](/images/2024-09-23-output-transformation-server-tips-and-tricks/15-paragraph-merged.png)

## `<TD>` used in table header row instead of `<TH>`

When you autodetect the columns in a table that contains a header row `<TD>` tag is used by default.
Ideally we must use `<TH>` in headers cells

![Table columns](/images/2024-09-23-output-transformation-server-tips-and-tricks/17-table-column.png)

Follow these steps to fix it:

 - Select you table
 - Right click on table
 - Click on `Edit`
 
![Edit table](/images/2024-09-23-output-transformation-server-tips-and-tricks/18-edit-table.png)
 
 - Click on `Cells` tab
  
![Cells tab in XFT table field properties](/images/2024-09-23-output-transformation-server-tips-and-tricks/19-cells-tab-in-xft-table-field-properties.png)

 - Right click on `RowHeaderColumns[0]`
 - Click on `+ Add`

![Add RowHeaderColumns - Cells tab](/images/2024-09-23-output-transformation-server-tips-and-tricks/20-add-RowHeaderColumns-cells-tab-in-xft-table-field-properties.png)

 - Click on Value cell at the `RowHeaderColumns[0] > [0]` row
 - Set value to `1`

![Add RowHeaderColumns - Cells tab](/images/2024-09-23-output-transformation-server-tips-and-tricks/21-add-RowHeaderColumns-cells-tab-in-xft-table-field-properties.png)


 - Click `OK`

If you check the DOM tree you can see that now `<TH>` is used in the header row

![TH used in table header](/images/2024-09-23-output-transformation-server-tips-and-tricks/22-th-used-in-table-header.png)

## The language of my document is not English

Sometimes you work in document remediation with documents written in a language other than English.

> **NOTE**: 
> You can check the language of your document in Acrobat Reader at Menu > Document properties.
> At Advanced tab in the Document properties pop-up you can see the language.
> This language will be used by the screen reader to choose the right voice.

![Reading options language](/images/2024-09-23-output-transformation-server-tips-and-tricks/25-reading-options-language.png)

Follow these steps to set the default language of a document in Output Transformation Server:

 - Right click on your `Document` at the `XFT Field Technology` menu
 - Click on `Edit`

![Edit document](/images/2024-09-23-output-transformation-server-tips-and-tricks/26-edit-document.png)

 - Click on `DOM properties` tab

![DOM properties](/images/2024-09-23-output-transformation-server-tips-and-tricks/27-xft-document-dom-properties.png)

 - Select the document Language at the `Language` drop-down list
 - Click `OK`

![DOM properties](/images/2024-09-23-output-transformation-server-tips-and-tricks/28-xft-document-dom-properties.png)

> **NOTE:**
> If the language of your document is not included in the drop-down list you can 
> use the ISO language code, i.e. you can use `ca_ES` for Catalan.
































