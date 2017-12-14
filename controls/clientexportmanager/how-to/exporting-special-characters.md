---
title: Exporting Special Characters
page_title: Exporting Special Characters | RadClientExportManager for ASP.NET AJAX Documentation
description: Exporting Special Characters
slug: clientexportmanager/how-to/exporting-special-characters
tags: custom,font,ASCII,special,characters,pdf
published: True
position: 3
---

# Exporting Special Characters


This article demonstrates how you can use custom fonts to export non-ASCII characters to PDF.

## 

If you do not specify a font explicitly any non-ASCII characters the exported DOM element contains will not be exported to the PDF file.

**Example:** Consider the following `<div>` element, which contains Swedish and Japanese characters(Kanji):


````ASPNET
<div id="foo">Västergötland(West Gothia) 日本(Japan)</div>
````


````CSS
#foo {
    background-color: grey;
    color: white;
}
````

This is how it looks like in the browser:

![Special Characters](images/clientexportmanager-special-characters.png)

Exporting this `<div>` to PDF would result in the following:

![Special Characters Missing](images/clientexportmanager-special-characters-missing.png)

The special characters are missing in the exported file, because the standard fonts used in exporting to PDF do not support non-ASCII characters. In order to export them you have to use a custom font that supports them. The custom font file has to be included in the project and the font has to be added to the RadClientExportManager's **PdfSettings.Fonts** collection and set to the element you want to export (in this example the `<div>` "foo").

* We will include the Arial Unicode MS font in our project's "Fonts" folder and add it to the **PdfSettings.Fonts** collection. It is an extended version of the Arial font and supports Unicode characters.


````C#
protected void Page_Load(object sender, EventArgs e)
{
    RadClientExportManager1.PdfSettings.Fonts.Add("Arial Unicode MS", "Fonts/ArialUnicodeMS.ttf");
}
````
````VB.NET
Protected Sub Page_Load(sender As Object, e As EventArgs)
	RadClientExportManager1.PdfSettings.Fonts.Add("Arial Unicode MS", "ArialUnicodeMS.ttf")
End Sub
````


* Apply the font to the `<div>` "foo":


````CSS
#foo {
    font-family: 'Arial Unicode MS';
    background-color: grey;
    color: white;
}
````


* Export the `<div>`:


````ASPNET
<telerik:RadClientExportManager runat="server" ID="RadClientExportManager1">
    <pdfsettings filename="Myfile.pdf" />
</telerik:RadClientExportManager>
<input type="button" onclick="exportElement()" value="export" />

<script type="text/javascript">
    function exportElement() {
        var exp = $find("<%= RadClientExportManager1.ClientID %>");
        exp.exportPDF($telerik.$("#foo"));
    }
</script>
````


The special characters are correctly exported and visible in the PDF file:

![Special Characters Exported](images/clientexportmanager-special-characters-exported.png)

# See Also

 * [ClientExportManager - Export Special Characters](https://demos.telerik.com/aspnet-ajax/client-export-manager/applicationscenarios/export-special-characters/defaultcs.aspx) online demo

