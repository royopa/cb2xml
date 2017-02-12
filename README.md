# A cb2xml 0.95.7 fork

This version of cb2xml contains fix for 88 levels on comp/comp-3 variables + new python example program.

*   [Uses of cb2xml](#cb2xmluses)
*   [cb2xml Xml File description and schemas](#xmlFormat)
*   [How to use cb2xml](#howto)
*   [Useful software for Cobol Data](#Useful)
*   [New Jaxb Jar](#jaxbjar)
*   [Change Summary](#changes)

## Warning for Existing users

In version 0.95.7,the format of the value tag has changed. Basically

<table>

<tbody>

<tr>

<td>**Cobol Value clause**</td>

<td>**Xml value tag**</td>

</tr>

<tr>

<td>value 121</td>

<td>value="121"</td>

</tr>

<tr>

<td>value zero</td>

<td>value="zero"</td>

</tr>

<tr>

<td>value 'some text'</td>

<td>value="'some text'"</td>

</tr>

<tr>

<td>value "some other text"</td>

<td>value=""some other text""</td>

</tr>

</tbody>

</table>

## <a id="cb2xmluses">Uses of cb2xml</a>

There are three main uses of the cb2xml -

*   Converting a **Cobol Copybook** to an equivalent **Xml-Copybook**. There are many ways of processing this Xml:
    *   A sample of how to process this Xml file using **jaxb** can be found in [Demo2.java](source/cb2xml_examples/src/net/sf/cb2xml/example/Demo2.java).
    *   The [JRecord](https://sourceforge.net/projects/jrecord/) project processes a _cb2xml Xml_ document [using a java program](http://jrecord.cvs.sourceforge.net/viewvc/jrecord/jrecord/src/net/sf/JRecord/External/XmlCopybookLoader.java?view=markup).
    [JRecord](https://sourceforge.net/p/jrecord/code/HEAD/tree/jrecord/Source/JRecord_Cbl2Xml/src/net/sf/JRecord/cbl2xml/impl/Cobol2GroupXml.java#l266)

    *   While the **cobol2j** project uses an [XSL transform](http://cobol2j.cvs.sourceforge.net/viewvc/cobol2j/cobol2j/src/main/xml/cb2xml2cobol2j.xsl?view=markup) to convert the cb2xml-file-description to its own Xml file-description-format.
    *   [CopybookUtils](http://www.rubydoc.info/gems/copybook_utils/) is a Ruby Gem for Ruby gem for reading Cobol Text files using cb2xml-Xml copybooks
    *   [cbgen](https://code.google.com/p/cbgen/source/browse/#svn%2Ftrunk%2Fsrc%2Fmain%2Fscala%2Fcom%2Fwasani%2Fcbgen) Written Scala, it uses it's own Copybook processor to extract Cobol details
*   Cobol-data-file to Xml file (Dat2Xml program). There is an enhance version in [JRecords Data2Xml](https://sourceforge.net/p/jrecord/wiki/Data2Xml%20and%20Xml2Data/)
*   Xml File to Cobol-data-file (Xml2Dat program). There is an enhance version in [JRecords Xml2Data](https://sourceforge.net/p/jrecord/wiki/Data2Xml%20and%20Xml2Data/)

**Note:** The Dat2Xml and Xml2Dat have the following limitations:

*   only handle text files.
*   Xml2Dat does not handle arrays !!!.
*   The size of the files they can handle is fairly limited because both the Cobol-Data file and Xml File (DOM format) are stored in memory during the conversion process

All these issues are resolved in [JRecords Data2Xml / Xml2Data](https://sourceforge.net/p/jrecord/wiki/Data2Xml%20and%20Xml2Data/).

## <a id="xmlFormat">cb2xml Xml File description and schemas</a>

[cb2xml.xsd](xmlSchema/cb2xml.xsd) and [cb2xml.dtd](xmlSchema/cb2xml.dtd) xml schemas can be found in the [xmlSchema](xmlSchema) directory.

Following is sample Cobol copybook :

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">1</font> </span>   <font color="#ff0000">03</font> f1                   <font color="#006699">**pic**</font> x(<font color="#ff0000">3</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">2</font> </span>   <font color="#ff0000">03</font> f2-comp              <font color="#006699">**pic**</font> s9(<font color="#ff0000">4</font>)    <font color="#006699">**comp**</font>.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">3</font> </span>   <font color="#ff0000">03</font> f3-comp-3            <font color="#006699">**pic**</font> s9(<font color="#ff0000">6</font>)V99 <font color="#006699">**comp-3**</font>.</font> </pre>

once the Cobol Copybook is processed by cb2xml:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">3</font> </span><font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">f1</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">x</font><font color="#ff00cc">(</font><font color="#ff00cc">3</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">4</font> </span><font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">f2</font><font color="#ff00cc">-</font><font color="#ff00cc">comp</font><font color="#ff00cc">"</font><font color="#0000ff">numeric</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">s9</font><font color="#ff00cc">(</font><font color="#ff00cc">4</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">signed</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">2</font><font color="#ff00cc">"</font><font color="#0000ff">usage</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">computational</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">5</font> </span><font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">8</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">f3</font><font color="#ff00cc">-</font><font color="#ff00cc">comp</font><font color="#ff00cc">-</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">numeric</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">s9</font><font color="#ff00cc">(</font><font color="#ff00cc">6</font><font color="#ff00cc">)</font><font color="#ff00cc">V99</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">6</font><font color="#ff00cc">"</font><font color="#0000ff">scale</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">2</font><font color="#ff00cc">"</font><font color="#0000ff">signed</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">5</font><font color="#ff00cc">"</font><font color="#0000ff">usage</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">computational</font><font color="#ff00cc">-</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font></font> </pre>

A more compilcated example:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">1</font> </span> <font color="#ff0000">01</font> Vendor.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">2</font> </span>    <font color="#ff0000">03</font> Brand               <font color="#006699">**Pic**</font> x(<font color="#ff0000">3</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">3</font> </span>    <font color="#ff0000">03</font> Location-details.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">4</font> </span>       <font color="#ff0000">05</font> Location-Number  <font color="#006699">**Pic**</font> <font color="#ff0000">9</font>(<font color="#ff0000">4</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">5</font> </span>       <font color="#ff0000">05</font> Location-Type    <font color="#006699">**Pic**</font> XX.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">6</font> </span>       <font color="#ff0000">05</font> Location-Name    <font color="#006699">**Pic**</font> X(<font color="#ff0000">35</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">7</font> </span>    <font color="#ff0000">03</font> Address-Details.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">8</font> </span>       <font color="#ff0000">05</font> actual-address.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">9</font> </span>          <font color="#ff0000">10</font> Address-1     <font color="#006699">**Pic**</font> X(<font color="#ff0000">40</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">10</font> </span>          <font color="#ff0000">10</font> Address-2     <font color="#006699">**Pic**</font> X(<font color="#ff0000">40</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">11</font> </span>          <font color="#ff0000">10</font> Address-3     <font color="#006699">**Pic**</font> X(<font color="#ff0000">35</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">12</font> </span>       <font color="#ff0000">05</font> Postcode         <font color="#006699">**Pic**</font> <font color="#ff0000">9</font>(<font color="#ff0000">4</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">13</font> </span>       <font color="#ff0000">05</font> Empty            <font color="#006699">**pic**</font> x(<font color="#ff0000">6</font>).
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">14</font> </span>       <font color="#ff0000">05</font> State            <font color="#006699">**Pic**</font> XXX.
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">15</font> </span>    <font color="#ff0000">03</font> Location-Active     <font color="#006699">**Pic**</font> X.</font> </pre>

This is converted into the following _Xml_ by cb2xml, please note _Nested Groups_ in the Cobol get converted to **Nested Items** in the Xml:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">1</font> </span><font color="#0000ff"><</font><font color="#0000ff">copybook</font><font color="#0000ff">filename</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">cbl2xml_Test110</font><font color="#ff00cc">.</font><font color="#ff00cc">cbl</font><font color="#ff00cc">"</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">2</font> </span>        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">173</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">01</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Ams</font><font color="#ff00cc">-</font><font color="#ff00cc">Vendor</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">173</font><font color="#ff00cc">"</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">3</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Brand</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">x</font><font color="#ff00cc">(</font><font color="#ff00cc">3</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">4</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">41</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Location</font><font color="#ff00cc">-</font><font color="#ff00cc">details</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">41</font><font color="#ff00cc">"</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">5</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Location</font><font color="#ff00cc">-</font><font color="#ff00cc">Number</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">9</font><font color="#ff00cc">(</font><font color="#ff00cc">4</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">numeric</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">6</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">2</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Location</font><font color="#ff00cc">-</font><font color="#ff00cc">Type</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">XX</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">8</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">2</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">7</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">35</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Location</font><font color="#ff00cc">-</font><font color="#ff00cc">Name</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">X</font><font color="#ff00cc">(</font><font color="#ff00cc">35</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">10</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">35</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">8</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">/</font><font color="#0000ff">item</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">9</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">128</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Address</font><font color="#ff00cc">-</font><font color="#ff00cc">Details</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">45</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">128</font><font color="#ff00cc">"</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">10</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">115</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">actual</font><font color="#ff00cc">-</font><font color="#ff00cc">address</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">45</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">115</font><font color="#ff00cc">"</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">11</font> </span>                          <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">40</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">10</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Address</font><font color="#ff00cc">-</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">X</font><font color="#ff00cc">(</font><font color="#ff00cc">40</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">45</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">40</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">12</font> </span>                          <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">40</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">10</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Address</font><font color="#ff00cc">-</font><font color="#ff00cc">2</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">X</font><font color="#ff00cc">(</font><font color="#ff00cc">40</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">85</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">40</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">13</font> </span>                          <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">35</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">10</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Address</font><font color="#ff00cc">-</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">X</font><font color="#ff00cc">(</font><font color="#ff00cc">35</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">125</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">35</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">14</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">/</font><font color="#0000ff">item</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">15</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Postcode</font><font color="#ff00cc">"</font><font color="#0000ff">numeric</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">true</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">9</font><font color="#ff00cc">(</font><font color="#ff00cc">4</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">160</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">4</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">16</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">6</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Empty</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">x</font><font color="#ff00cc">(</font><font color="#ff00cc">6</font><font color="#ff00cc">)</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">164</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">6</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">17</font> </span>                        <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">05</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">State</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">XXX</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">170</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">3</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">18</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">/</font><font color="#0000ff">item</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">19</font> </span>                <font color="#0000ff"><</font><font color="#0000ff">item</font><font color="#0000ff">display-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">level</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">03</font><font color="#ff00cc">"</font><font color="#0000ff">name</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">Location</font><font color="#ff00cc">-</font><font color="#ff00cc">Active</font><font color="#ff00cc">"</font><font color="#0000ff">picture</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">X</font><font color="#ff00cc">"</font><font color="#0000ff">position</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">173</font><font color="#ff00cc">"</font><font color="#0000ff">storage-length</font><font color="#0000ff">=</font><font color="#ff00cc">"</font><font color="#ff00cc">1</font><font color="#ff00cc">"</font><font color="#0000ff">/</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#990066">20</font> </span>        <font color="#0000ff"><</font><font color="#0000ff">/</font><font color="#0000ff">item</font><font color="#0000ff">></font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">21</font> </span><font color="#0000ff"><</font><font color="#0000ff">/</font><font color="#0000ff">copybook</font><font color="#0000ff">></font></font> </pre>

The attributes **name**, **numeric**, **picture** and **signed** are all fairly obvious. The other attributes include:

*   **position** - position in the record (1 is the first byte).
*   **storage-length** - length in bytes of the field.
*   **scale** - scaling factor, used when there is an assumed decimal point (Cobol V picture) or assumed zero (Cobol P picture).
    *   scale=-1 - multiply by 10 (e.g. Picture of S9(4)P)
    *   scale=1 - divide by 10 (e.g. Picture of S9(4)V9)
    *   scale=2 - divide by 100 (e.g. Picture of S9(4)V99)

    **Background information:** - Cobol uses an "assumed decimal" in representing numbers for expample a dollar amount could be represented in Cobol as **Pic s9(7)V99 comp** this is implemented as a binary integer but where binary 1 actually represent 0.01, i.e. integer values need to be **scaled**

*   **usage**, Cobol usage attribute; typically **computational** is Big-Endian-Binary, **computational-3** is packed decimal, **computational-5** is Machines-Binary-Integer (Little endian on Intel Hardware, big-endian on the Mainframe and Large-servers).
*   **editted-numeric** - Wether this field is an editted-numeric or not. In Cobol s9(4)V99 is a true numeric and can be used in numeric calculations while -(3)9.99 is called a **editted-numeric** but strictly speaking it is a charactger field and **can not** be used in calculations, only for formatting. In cb2xml both s9(4)V99 and -(3)9.99 are flagged as numeric but only -(3)9.99 is flagged as editted-numeric.
*   **inherited-usage** - Normally in cobol the usage is specified on each field:

## <a id="howto">How to use cb2xml</a>

The [examples directory](examples/) holds sample rexx, bat and shell scripts that call **cb2xml.jar** to:

*   Convert **Cobol Copybooks** to _Xml-File-Descriptions_ (cobol2xmlFileDescription.*)
*   Convert **Cobol Data** files to _Xml-Files_ (data2xml.*)
*   Convert **Xml-Files** to _Cobol Data_ files (xml2data.*)

Basically to convert a Cobol Copybook to Xml (bat / shell script):

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">7</font> </span>java -jar ../../lib/cb2xml.jar -cobol cbl2xml_Test102.cbl -indentXml  -xml cbl2xml_Test102_new3.cbl.xml
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">8</font> </span>java -jar ../../lib/cb2xml.jar -cobol cpyUtf8.cbl -indentXml  -font utf-8 -xml cpyUtf8_new4.cbl.xml</font> </pre>

The parameters to cb2xml are:

*   **-cobol** - input Cobol copybook
*   **-xml** - output Xml translation.
*   **-font** - Copybook font.
*   **-indentXml** - wether to indent (pretty print) the Xml.
*   **-debug** - wether

To load a Cobol Copybook in Java / JVM languages:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">43</font> </span>        Copybook copybook <font color="#000000">**=**</font> CobolParser.<font color="#9966ff">newParser</font><font color="#000000">**(**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">44</font> </span>                                .<font color="#9966ff">parseCobol</font><font color="#000000">**(**</font>copybookName<font color="#000000">**)**</font>;</font> </pre>

To create an Xml-Documnet in java:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">1</font> </span>          <font color="#ff8400">//</font><font color="#ff8400">If</font><font color="#ff8400">you</font><font color="#ff8400">want</font><font color="#ff8400">process</font><font color="#ff8400">a</font><font color="#ff8400">cobol</font><font color="#ff8400">copybook</font><font color="#ff8400">with</font><font color="#ff8400">column</font><font color="#ff8400">specified</font><font color="#ff8400">in</font><font color="#ff8400">the</font><font color="#ff8400">cb2xml.properties</font><font color="#ff8400">file</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">2</font> </span>     Document doc <font color="#000000">**=**</font> Cb2Xml.<font color="#9966ff">convertToXMLDOM</font><font color="#000000">**(**</font>cobolCopybookFile<font color="#000000">**)**</font>; 
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">3</font> </span>         <font color="#ff8400">//</font><font color="#ff8400">or</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">4</font> </span>     Document doc <font color="#000000">**=**</font> Cb2Xml2.<font color="#9966ff">convertToXMLDOM</font><font color="#000000">**(**</font>cobolCopybookFile, <font color="#cc00cc">false</font>, Cb2xmlConstants.USE_PROPERTIES_FILE<font color="#000000">**)**</font>;</font> </pre>

By default Cobol uses columns 6 --> 71 but some Cobol-Compilers allow other columns to be used. To cover for this cb2xml programs has a

<table border="1">

<tbody>

<tr>

<th align="LEFT" bgcolor="#DADADA" valign="TOP">Copybook Format</th>

<th align="LEFT" bgcolor="#DADADA" valign="TOP">Description</th>

</tr>

<tr>

<td>**Cb2xmlConstants.USE_PROPERTIES_FILE**</td>

<td>Get the line lengths from the cb2xml.properties file</td>

</tr>

<tr>

<td>**Cb2xmlConstants.USE_STANDARD_COLUMNS**</td>

<td>Standard Cobol Columns (6->71)</td>

</tr>

<tr>

<td>**Cb2xmlConstants.USE_COLS_6_TO_80**</td>

<td>Columns 6->80</td>

</tr>

<tr>

<td>**Cb2xmlConstants.USE_LONG_LINE**</td>

<td>Use columns 6 to whatever Columns (essentially 6->Max-Int)</td>

</tr>

<tr>

<td>**Cb2xmlConstants.FREE_FORMAT**</td>

<td>Free Formet (any column essentially 1->Max-Int)</td>

</tr>

</tbody>

</table>

**Groovy Code** to load and print Cobol copybook:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">9</font> </span>    <font color="#cc0000">//</font><font color="#cc0000">Parsing</font><font color="#cc0000">a</font><font color="#cc0000">Cobol</font><font color="#cc0000">copybook</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">10</font> </span>    <font color="#009966">**def**</font> cpy <font color="#000000">**=**</font> CobolParser.<font color="#9966ff">newParser</font><font color="#000000">**(**</font><font color="#000000">**)**</font>.<font color="#9966ff">parseCobol</font><font color="#000000">**(**</font><font color="#ff00cc">"</font><font color="#ff00cc">cbl2xml_Test110</font><font color="#ff00cc">.</font><font color="#ff00cc">cbl</font><font color="#ff00cc">"</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">11</font> </span>    
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">12</font> </span>    <font color="#cc0000">//</font><font color="#cc0000">Now</font><font color="#cc0000">lets</font><font color="#cc0000">print</font><font color="#cc0000">it</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">13</font> </span>    <font color="#66ccff">**print**</font><font color="#000000">**(**</font><font color="#ff00cc">"</font><font color="#ff00cc">"</font>, cpy.<font color="#9966ff">getItem</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">14</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">15</font> </span><font color="#cc0000">//</font><font color="#cc0000">Print</font><font color="#cc0000">Cobol</font><font color="#cc0000">item</font><font color="#cc0000">(and</font><font color="#cc0000">its</font><font color="#cc0000">child</font><font color="#cc0000">items</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">16</font> </span><font color="#009966">**def**</font> <font color="#66ccff">**print**</font><font color="#000000">**(**</font>String indent, List<Item> items<font color="#000000">**)**</font> {
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">17</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">18</font> </span>    <font color="#006699">**for**</font> <font color="#000000">**(**</font>Item item : items<font color="#000000">**)**</font> {
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">19</font> </span>       String s <font color="#000000">**=**</font> indent <font color="#000000">**+**</font> item.<font color="#9966ff">getName</font><font color="#000000">**(**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">"</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">20</font> </span>       s <font color="#000000">**=**</font> s.<font color="#9966ff">substring</font><font color="#000000">**(**</font><font color="#ff0000">0</font>, <font color="#ff0000">50</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">21</font> </span>       <font color="#66ccff">**println**</font> s <font color="#000000">**+**</font> item.<font color="#9966ff">getPosition</font><font color="#000000">**(**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">\t</font><font color="#ff00cc">"</font> <font color="#000000">**+**</font> item.<font color="#9966ff">getStorageLength</font><font color="#000000">**(**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">\t</font><font color="#ff00cc">"</font> <font color="#000000">**+**</font> item.<font color="#9966ff">getDisplayLength</font><font color="#000000">**(**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#9966ff">cn</font><font color="#000000">**(**</font>item.<font color="#9966ff">getPicture</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#9966ff">cn</font><font color="#000000">**(**</font>item.<font color="#9966ff">getUsage</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#9966ff">cn</font><font color="#000000">**(**</font>item.<font color="#9966ff">isNumeric</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font> <font color="#000000">**+**</font> <font color="#9966ff">cn</font><font color="#000000">**(**</font>item.<font color="#9966ff">isSigned</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">22</font> </span>      
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">23</font> </span>       <font color="#66ccff">**print**</font><font color="#000000">**(**</font>indent <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">"</font>, item.<font color="#9966ff">getItem</font><font color="#000000">**(**</font><font color="#000000">**)**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">24</font> </span>   }
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">25</font> </span>}
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">26</font> </span></font> </pre>

**Python Code** to load and print cb2xml - Xml

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">29</font> </span><font color="#006699">**def**</font> <font color="#9966ff">printItem</font><font color="#000000">**(**</font>indent, item<font color="#000000">**)**</font><font color="#006699">**:**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">30</font> </span>    n <font color="#000000">**=**</font> indent <font color="#000000">**+**</font> item.attrib[<font color="#ff00cc">'</font><font color="#ff00cc">level</font><font color="#ff00cc">'</font>] <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">"</font> <font color="#000000">**+**</font> item.attrib[<font color="#ff00cc">'</font><font color="#ff00cc">name</font><font color="#ff00cc">'</font>] <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">"</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">31</font> </span>    n <font color="#000000">**=**</font> n[<font color="#006699">**:**</font><font color="#ff0000">50</font>]
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">32</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">33</font> </span>    <font color="#006699">**print**</font> n, <font color="#ff00cc">'</font><font color="#ff00cc">\t</font><font color="#ff00cc">'</font>, item.attrib[<font color="#ff00cc">'</font><font color="#ff00cc">position</font><font color="#ff00cc">'</font>], <font color="#ff00cc">'</font><font color="#ff00cc">\t</font><font color="#ff00cc">'</font>, item.attrib[<font color="#ff00cc">'</font><font color="#ff00cc">storage</font><font color="#ff00cc">-</font><font color="#ff00cc">length</font><font color="#ff00cc">'</font>], <font color="#ff00cc">'</font><font color="#ff00cc">\t</font><font color="#ff00cc">'</font>, <font color="#9966ff">getAttr</font><font color="#000000">**(**</font>item, <font color="#ff00cc">'</font><font color="#ff00cc">display</font><font color="#ff00cc">-</font><font color="#ff00cc">length</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>, <font color="#9966ff">getAttr</font><font color="#000000">**(**</font>item, <font color="#ff00cc">'</font><font color="#ff00cc">picture</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>, <font color="#9966ff">getAttrId</font><font color="#000000">**(**</font>item, <font color="#ff00cc">'</font><font color="#ff00cc">usage</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>, <font color="#9966ff">getAttrId</font><font color="#000000">**(**</font>item, <font color="#ff00cc">'</font><font color="#ff00cc">numeric</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>, <font color="#9966ff">getAttrId</font><font color="#000000">**(**</font>item, <font color="#ff00cc">'</font><font color="#ff00cc">signed</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">34</font> </span>    <font color="#006699">**for**</font> child <font color="#006699">**in**</font> item.<font color="#9966ff">findall</font><font color="#000000">**(**</font><font color="#ff00cc">'</font><font color="#ff00cc">item</font><font color="#ff00cc">'</font><font color="#000000">**)**</font><font color="#006699">**:**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">35</font> </span>        <font color="#9966ff">printItem</font><font color="#000000">**(**</font>indent <font color="#000000">**+**</font> <font color="#ff00cc">"</font><font color="#ff00cc">"</font>, child<font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">36</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">37</font> </span><font color="#cc0000">#</font><font color="#cc0000">########################################################################</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">38</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">39</font> </span>tree <font color="#000000">**=**</font> ET.<font color="#9966ff">parse</font><font color="#000000">**(**</font><font color="#ff00cc">'</font><font color="#ff00cc">cbl2xml_Test110</font><font color="#ff00cc">.</font><font color="#ff00cc">cbl</font><font color="#ff00cc">.</font><font color="#ff00cc">xml</font><font color="#ff00cc">'</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">40</font> </span>root <font color="#000000">**=**</font> tree.<font color="#9966ff">getroot</font><font color="#000000">**(**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">41</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">42</font> </span><font color="#006699">**print**</font> <font color="#ff00cc">"</font><font color="#ff00cc">></font><font color="#ff00cc">></font><font color="#ff00cc">"</font>, root.tag, root.attrib[<font color="#ff00cc">'</font><font color="#ff00cc">filename</font><font color="#ff00cc">'</font>]
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">43</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">44</font> </span><font color="#006699">**for**</font> child <font color="#006699">**in**</font> root.<font color="#9966ff">findall</font><font color="#000000">**(**</font><font color="#ff00cc">'</font><font color="#ff00cc">item</font><font color="#ff00cc">'</font><font color="#000000">**)**</font><font color="#006699">**:**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">45</font> </span>    <font color="#9966ff">printItem</font><font color="#000000">**(**</font><font color="#ff00cc">"</font><font color="#ff00cc">"</font>, child<font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">46</font> </span></font> </pre>

**Ruby Code** to load and print cb2xml - Xml

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">10</font> </span><font color="#006699">**def**</font> printXml indent, tree
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">11</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">12</font> </span>    tree.each <font color="#006699">**do**</font> <font color="#000000">**|**</font>node<font color="#000000">**|**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">13</font> </span>         <font color="#006699">**puts**</font> <font color="#ff00cc">"</font><font color="#000000">**#{**</font>indent<font color="#000000">**}**</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">level</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">name</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#ff00cc">\t</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">position</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#ff00cc">\t</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">storage</font><font color="#ff00cc">-</font><font color="#ff00cc">length</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#ff00cc">\t</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">picture</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#ff00cc">\t</font><font color="#000000">**#{**</font>node<font color="#000000">**[**</font><font color="#ff00cc">"</font><font color="#ff00cc">usage</font><font color="#ff00cc">"</font><font color="#000000">**]**</font><font color="#000000">**}**</font><font color="#ff00cc">"</font>   
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">14</font> </span>         printXml <font color="#ff00cc">"</font><font color="#000000">**#{**</font>indent<font color="#000000">**}**</font><font color="#ff00cc">"</font>, node.children   
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">15</font> </span>    <font color="#006699">**end**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">16</font> </span><font color="#006699">**end**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">17</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">18</font> </span>   doc <font color="#000000">**=**</font> Nokogiri.XML<font color="#000000">**(**</font>File.open<font color="#000000">**(**</font><font color="#ff00cc">"</font><font color="#ff00cc">cb2xml_Output110</font><font color="#ff00cc">.</font><font color="#ff00cc">xml</font><font color="#ff00cc">"</font><font color="#000000">**)**</font><font color="#000000">**)**</font>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#000000">19</font> </span>   
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "><font color="#990066">20</font> </span>   printXml <font color="#ff00cc">"</font><font color="#ff00cc">"</font>, doc.root.children</font> </pre>

To convert a _Cobol-Data-file_ to a _DOM Document_:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">1</font> </span>        String sourceFileContents<font color="#000000">**=**</font> FileUtils.<font color="#9966ff">readFile</font><font color="#000000">**(**</font>dataFileName<font color="#000000">**)**</font>.<font color="#9966ff">toString</font><font color="#000000">**(**</font><font color="#000000">**)**</font>;
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">2</font> </span>        Document copyBookXml <font color="#000000">**=**</font> XmlUtils.<font color="#9966ff">fileToDom</font><font color="#000000">**(**</font>copybookFileName<font color="#000000">**)**</font>;
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">3</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">4</font> </span>        Document xmlDoc <font color="#006699">**new**</font> <font color="#9966ff">MainframeToXml</font><font color="#000000">**(**</font><font color="#000000">**)**</font>.<font color="#9966ff">convert</font><font color="#000000">**(**</font>sourceFileContents, copyBookXml<font color="#000000">**)**</font>;</font> </pre>

To convert _Xml_ file to _Cobol-Data_ file:

<pre><font color="#000000"><span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">1</font> </span>        Document sourceFileXml <font color="#000000">**=**</font> XmlUtils.<font color="#9966ff">fileToDom</font><font color="#000000">**(**</font>dataFileName<font color="#000000">**)**</font>;
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">2</font> </span>        Document copyBookXml <font color="#000000">**=**</font> XmlUtils.<font color="#9966ff">fileToDom</font><font color="#000000">**(**</font>copybookFileName<font color="#000000">**)**</font>;
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">3</font> </span>
<span style="background:#dbdbdb; border-right:solid 2px black; margin-right:5px; "> <font color="#000000">4</font> </span>        String fileData <font color="#000000">**=**</font> <font color="#006699">**new**</font> <font color="#9966ff">XmlToMainframe</font><font color="#000000">**(**</font><font color="#000000">**)**</font>.<font color="#9966ff">convert</font><font color="#000000">**(**</font>sourceFileXml, copyBookXml<font color="#000000">**)**</font>;</font> </pre>

Examples of processing the cb2xml generated DOM / Xml:

<table border="1">

<tbody>

<tr>

<th align="LEFT" bgcolor="#DADADA" valign="TOP">Example</th>

<th align="LEFT" bgcolor="#DADADA" valign="TOP">Description</th>

</tr>

<tr>

<td>[Examples](examples/)</td>

<td>There are very basic [python](examples/PythonExample/), [ruby](examples/RubyExample/), [groovy](examples/GroovyExample/), [jython](examples/JythonExample/) and [jruby](examples/JRubyExample/) programs to print a _cb2xml Xml_ file</td>

</tr>

<tr>

<td>[Demo.java](source/cb2xml_examples/src/net/sf/cb2xml/example/Demo.java)</td>

<td>A basic program that loads _cb2xml Xml_ via JAXB and then prints the copybook</td>

</tr>

<tr>

<td>[Demo2.java](source/cb2xml_examples/src/net/sf/cb2xml/example/Demo2.java)</td>

<td>A basic program that converts a _Cobol Copybook_ to a DOM document, loads this via JAXB then prints the copybook</td>

</tr>

<tr>

<td>[DemoCobolJTreeTable.java](source/cb2xml_examples/src/net/sf/cb2xml/example/DemoCobolJTreeTable.java)</td>

<td>A basic program to display a Cobol copybook in a TreeTable.</td>

</tr>

<tr>

<td>[source/cb2xml_tests/src/tests](source/cb2xml_tests/src/tests)</td>

<td>Holds examples of calling cb2xml from java. In particular _TstCb2xml02_ has examples of calling Cb2Xml to get get a _DOM Document_ and TstCblDataToXml01 call Dat2Xml and Xml2Dat.</td>

</tr>

<tr>

<td>[MainframeToXml.java](source/cb2xml/src/net/sf/cb2xml/convert/MainframeToXml.java)</td>

<td>Uses a **cb2xml-Xml** file to convert a _Cobol-Text_ file to Xml</td>

</tr>

<tr>

<td>[XmlToMainframe.java](source/cb2xml/src/net/sf/cb2xml/convert/XmlToMainframe.java)</td>

<td>Uses a **cb2xml-Xml** file to convert a _Xml_ file to a _Cobol-Text_ file.</td>

</tr>

<tr>

<td>[JRecord - XmlCopybookLoader](http://jrecord.cvs.sourceforge.net/viewvc/jrecord/jrecord/src/net/sf/JRecord/External/XmlCopybookLoader.java?view=markup)</td>

<td>JRecords code to process a _cb2xml Xml_ Document.</td>

</tr>

<tr>

<td>[XSL transform](http://cobol2j.cvs.sourceforge.net/viewvc/cobol2j/cobol2j/src/main/xml/cb2xml2cobol2j.xsl?view=markup)</td>

<td>Xsl Transform used in the cobol2j package to convert **cb2xml-Xml** ot its own Xml format. This method does not work for all possible Cobol Copybooks.</td>

</tr>

<tr>

<td>[Pretty Print](source/cb2xml_examples/src/net/sf/cb2xml/example/PrettyPrintCopybook.java)</td>

<td>uses a StAX parser to process the Xml.</td>

</tr>

</tbody>

</table>

## <a id="Useful">Useful software for Cobol Data</a>

If you need to work with Cobol files, you may find some of this software useful:

*   The [RecordEditor](https://sourceforge.net/projects/record-editor/) lets you view/edit Cobol-data files with a cobol copybook (you have to import the cobol copybook into the RecordEditor).
*   If you need to process binary Cobol files in Java consider [JRecord](https://sourceforge.net/projects/jrecord/), cobol2j or one of several other packages.
*   For Cobol Data to/from Csv, have a look at JRecords [JRecords Data2Xml / Xml2Data](https://sourceforge.net/p/jrecord/wiki/Data2Xml%20and%20Xml2Data/). [Cobol2Csv and Csv2Cobol](https://sourceforge.net/p/jrecord/wiki/Cobol2Xml%2C%20Xml2Cobol/) programs.
*   For Cobol to/from Xml look at JRecords
*   Another processing option is the [legstar](http://www.legsem.com/legstar/) packages.

## <a id="jaxbjar">New Jaxb Jar</a>

A new Jar cb2xml_Jaxb.jar has been crated. This jar holds Java Copybook and Item classes + code to create them from either a Cobol Copybook or a cb2xml-Xml file.

Longer term if there is enough interest in the jars contents, it will be included in the standard cb2xml.jar. The trouble with including it now is I will be stuck with it even hardly any one uses it.<a id="changes"></a>

## <a id="changes">Future Changes</a>

<a id="changes">

*   I am hoping to bring support for other Cobol Dialects (e.g. Gnu-Cobol) to cb2xml.
*   I will also add a tag for the actual field type (e.g packed-decimal Mainframe-Zoned-Decimal, Fujitsu-zoned decimal, Packed-decimal etc).
*   Option to return a **copyook** and **Item** class so you users of Cb2Xml do not need to pass the **Document**.

## Changes Version 0.95.7

*   Updated the value tag. Text values are now in quotes to distinguish them from numeric and cobol constants (like zero, spaces etc).

    <table>

    <tbody>

    <tr>

    <td>**Cobol Value clause**</td>

    <td>**Xml value tag**</td>

    </tr>

    <tr>

    <td>value 121</td>

    <td>value="121"</td>

    </tr>

    <tr>

    <td>value zero</td>

    <td>value="zero"</td>

    </tr>

    <tr>

    <td>value 'some text'</td>

    <td>value="'some text'"</td>

    </tr>

    <tr>

    <td>value "some other text"</td>

    <td>value=""some other text""</td>

    </tr>

    </tbody>

    </table>

*   Adding a justified tag (for when justified or justified right is used in the Cobol Copybook).

_Cobol move copybook_

## Changes Version 0.95.6

*   Fix for 88 levels on comp/comp-3 values.
*   Added **toTextCopybook.py** example program. It will create a _Text_ version of a copybook + _Cobol move copybook_ to move values from the Text-Copybook

## Changes Version 0.95.5

*   Fix for position after redefines.
*   Added **ObscureCobol** example program. It will "Obfuscate" a Cobol copybook.

## Changes Version 0.95.4

*   Added support for the use of Reader (instead of stream) in the library
*   Added -font and -indentXml options

## Changes Version 0.95.3

*   Fix for usage specified at the Group Level
*   Fix for dropping quotes in value tag
*   Added support continuation of value across multiple lines
*   New JAXB jar for accessing cb2xml - Xml.
*   Added examples in Ruby, JRuby, Jython and Groovey. Updated python example.
*   Added 2 new Xml tags: **editted-numeric** and **inherited-usage**.
*   Updated documentation.

## Changes Version 0.95.2

*   Fix for USE_SUPPLIED_COLUMNS option.

## Changes Version 0.95.1

*   Created a new class **Cb2Xml2** which is like the _Cb2Xml_ class except it does not trap errors !!
*   Minor improvements in the error messages from the Cobol Parser.
*   Created DTD and Xsd schemas for the Xml created by cb2xml.
*   Created Java Jaxb example programs for processing cb2xml Xml.
*   Created a basic python program to print cb2xml Xml.

## Changes Version 0.95

*   Improved support for P picture character, new assumed digits tag + Scale can be negative (due to P picture option).
*   Introduced constant class for the attributes used by Cb2xml. This allows other projects to integrate better with cb2xml
*   Most methods accept stream as well File (Feature request 1)
*   Support for Hex literals (patch 1)
*   Support for sync keyword
*   Fixed issue with fields starting with numerics
*   Fixed issue with comments at start of copybook in dat2xml and xml2dat
*   Fixed issue with “anonymous” REDEFINES (problem 12)
*   Fixed issue with sign seperate (problem 6)
*   Option to pass columns to CobolPreprocessor. This means when the calling program passes the start/end column the file cb2xml.properties is not needed anymore.
*   Added some basic Automated Tests, this will allow regular releases in the future.
*   dat2xml - trim numerics
*   dat2xml - Fixed issue with occurs (problem 4 and 10)
*   xml2dat - Changed to set field to spaces (when tag is absent in the Xml and no initialise is set). (problem 11).
*   xml2dat - fix for when incoming data is not the expected cobol length

</a>
