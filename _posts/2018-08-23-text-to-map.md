# Text to Map

Thanks to a colleague I was able to do a part of this assignment, as I wasn't present in much of this class.

Extraction of geographical data from the Richmond ‘Dispatch’:

<pre><code>import re, os

source = os.path.abspath('Perseus_Test_Texte')
target = os.path.abspath('Output_ordner')

lof = os.listdir('Perseus_Test_Texte')
counter = 0 # general counter to keep track of the progress

# list to include data in variable
list = []

for f in lof:
    if f.startswith("dltext"): # fileName test        
        with open(source + '/' + f, "r", encoding="utf8") as f1:
            text = f1.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]:
                c += 1
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header)
                except:
                    header = "NO HEADER"
                   # print("\nNo header found!\n")

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)

                # generating necessary bits 
                # fName = date+"_"+unitType+"_"+str(c)

                itemID = "#ID: " + date+"_"+unitType+"_"+str(c)
                dateVar   = "#DATE: " + date
                unitType = "#TYPE: " + unitType
                header = "#HEADER: " + header
                text = "#TEXT: " + text

                # creating a text variable with each entry on a new line
                var = "\t".join([itemID,dateVar,unitType,header,text]) + "\n"
                #input(var)
                # appending variable to list
                list.append(var)

                # saving


with open(target + "/" + "total_data_structured" +".tsv", "w", encoding="utf8") as f9:
    for item in list:
        f9.write(item)

        # count processed issues and print progress counter at every 100        
        counter += 1
        if counter % 100 == 0:
            print(counter)</code></pre>
