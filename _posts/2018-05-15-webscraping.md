Steps:

* I copied the html/source code of http://www.perseus.tufts.edu/hopper/collection?collection=Perseus:collection:RichTimes into my texteditor.

![](../img/stage1.png)


* Using Regular Expressions, I filtred the links I needed to download the articles later on. 

![](../img/stage2.png)

* Then I inserted missing parts of the linku using Regular Expressions: `^ www.perseus.tufts.edu/hopper/dl`

* After collecting all the links in one text files and using the command line and wget I downloaded the the xmls of the articles.
