# CSE 15L Lab Report 3 | Researching Commands 
## Author: Manu Bhat - A17337644 - mbhat@ucsd.edu

### Interesting Use Cases of `find`

I will be summarizing my research on four interesting options on the UNIX `find` command. I conducted research via the internet and the manual for find.

1. `-depth <d>`
    Summary: This option allows us to find files that are `d` directories below the calling directory. This is useful in scenarios where we have a structure where the intermediate files aren't of interest, but we only care about the absolute leaf files.

    A. In written_2, a find call with depth = 1 reveals only the first level of paths. In this case, they are all directories.
    ```
    manubhat@manus-laptop skill-demo1-data % find written_2 -depth 1
    written_2/non-fiction
    written_2/travel_guides
    ```
    B. In written_2, a find call with depth = 2 reveals only the second level of paths. In this case, they are again all directories. I also tries with depth=3, which just showed all of the txt files.
    '''
    manubhat@manus-laptop skill-demo1-data % find written_2 -depth 2
    written_2/non-fiction/OUP
    written_2/travel_guides/berlitz1
    written_2/travel_guides/berlitz2
    '''
2. `-delete` 
    Summary: This option allows us to delete the files and directories that are generated. This is more powerful than `rm`, since we can combine the other utilities of find to delete exactly the files we want
    
    A. In written_2, we can combine the vanilla find with the '-type f' option to delete only the files
    ```
    manubhat@manus-laptop skill-demo1-data % find written_2 -type f -delete
    manubhat@manus-laptop skill-demo1-data % find written_2 
    written_2
    written_2/non-fiction
    written_2/non-fiction/OUP
    written_2/non-fiction/OUP/Berk
    written_2/non-fiction/OUP/Abernathy
    written_2/non-fiction/OUP/Rybczynski
    written_2/non-fiction/OUP/Kauffman
    written_2/non-fiction/OUP/Fletcher
    written_2/non-fiction/OUP/Castro
    written_2/travel_guides
    written_2/travel_guides/berlitz1
    written_2/travel_guides/berlitz2
    ``` 
    B. In written_2, a plain call will just delete the entire directory! A resulting `ls` operation shows no remaining files.
    ```
    manubhat@manus-laptop skill-demo1-data % find written_2 -delete
    manubhat@manus-laptop skill-demo1-data % ls  
    ```
3. `-s`
    Summary: This option allows us to traverse each level in a sorted manner. This is useful so we get deterministic results.
    
    A. in written_2, we can output all txt files of a particular directory.
    ```
    manubhat@manus-laptop skill-demo1-data % echo SORTED && find -s written_2/non-fiction/OUP/Berk
    SORTED
    written_2/non-fiction/OUP/Berk
    written_2/non-fiction/OUP/Berk/CH4.txt
    written_2/non-fiction/OUP/Berk/ch1.txt
    written_2/non-fiction/OUP/Berk/ch2.txt
    written_2/non-fiction/OUP/Berk/ch7.txt
    manubhat@manus-laptop skill-demo1-data % echo UNSORTED && find written_2/non-fiction/OUP/Berk
    UNSORTED
    written_2/non-fiction/OUP/Berk
    written_2/non-fiction/OUP/Berk/ch2.txt
    written_2/non-fiction/OUP/Berk/ch1.txt
    written_2/non-fiction/OUP/Berk/CH4.txt
    written_2/non-fiction/OUP/Berk/ch7.txt
    ```
    B. We can also sort all files that match the name command. Note that it's not a pure sort, but instead sorts at each level, which is slightly different.
    ```
    manubhat@manus-laptop skill-demo1-data % find -s written_2 -name "*a.txt"
    written_2/travel_guides/berlitz1/HandRIbiza.txt
    written_2/travel_guides/berlitz1/HandRJamaica.txt
    written_2/travel_guides/berlitz1/HandRMadeira.txt
    written_2/travel_guides/berlitz1/HandRMallorca.txt
    written_2/travel_guides/berlitz1/HistoryIbiza.txt
    written_2/travel_guides/berlitz1/HistoryIndia.txt
    written_2/travel_guides/berlitz1/HistoryJamaica.txt
    written_2/travel_guides/berlitz1/HistoryMadeira.txt
    written_2/travel_guides/berlitz1/HistoryMalaysia.txt
    written_2/travel_guides/berlitz1/HistoryMallorca.txt
    written_2/travel_guides/berlitz1/IntroIbiza.txt
    written_2/travel_guides/berlitz1/IntroIndia.txt
    written_2/travel_guides/berlitz1/IntroJamaica.txt
    written_2/travel_guides/berlitz1/IntroMadeira.txt
    written_2/travel_guides/berlitz1/IntroMalaysia.txt
    written_2/travel_guides/berlitz1/IntroMallorca.txt
    written_2/travel_guides/berlitz1/JungleMalaysia.txt
    written_2/travel_guides/berlitz1/WhatToIbiza.txt
    written_2/travel_guides/berlitz1/WhatToIndia.txt
    written_2/travel_guides/berlitz1/WhatToJamaica.txt
    written_2/travel_guides/berlitz1/WhatToMadeira.txt
    written_2/travel_guides/berlitz1/WhatToMalaysia.txt
    written_2/travel_guides/berlitz1/WhatToMallorca.txt
    written_2/travel_guides/berlitz1/WhereToIbiza.txt
    written_2/travel_guides/berlitz1/WhereToIndia.txt
    written_2/travel_guides/berlitz1/WhereToMadeira.txt
    written_2/travel_guides/berlitz1/WhereToMalaysia.txt
    written_2/travel_guides/berlitz1/WhereToMallorca.txt
    ```
4. `-exec`
    Summary: This can be used as a primary to filter files, but also so that we can essentially iterate on all files matching other primaries 
    
    A: Here I used exec as a filter, to see which files I want to include. Specifically, if the file contains the word "Lucayan"
    ```
    manubhat@manus-laptop skill-demo1-data % find . -type f -exec grep -l "Lucayan" {} \;
    ./written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
    ./written_2/travel_guides/berlitz2/Bahamas-History.txt
    ```
    B: Here I used exec to go through every file that matches a name, and then print out the word count
    ```
    manubhat@manus-laptop skill-demo1-data % find . -name "*a.txt" -exec wc -w {} \;
    1180 ./written_2/travel_guides/berlitz1/IntroMalaysia.txt
    2476 ./written_2/travel_guides/berlitz1/HistoryJamaica.txt
    3167 ./written_2/travel_guides/berlitz1/HandRJamaica.txt
    7774 ./written_2/travel_guides/berlitz1/HistoryIndia.txt
    1420 ./written_2/travel_guides/berlitz1/IntroMadeira.txt
     903 ./written_2/travel_guides/berlitz1/IntroIbiza.txt
    5431 ./written_2/travel_guides/berlitz1/WhatToIbiza.txt
    1939 ./written_2/travel_guides/berlitz1/HistoryMallorca.txt
   25345 ./written_2/travel_guides/berlitz1/WhereToIndia.txt
     191 ./written_2/travel_guides/berlitz1/HandRMallorca.txt
     990 ./written_2/travel_guides/berlitz1/JungleMalaysia.txt
    4044 ./written_2/travel_guides/berlitz1/WhatToMadeira.txt
   26307 ./written_2/travel_guides/berlitz1/WhereToMalaysia.txt
    3325 ./written_2/travel_guides/berlitz1/WhatToMalaysia.txt
    1673 ./written_2/travel_guides/berlitz1/HistoryMadeira.txt
     188 ./written_2/travel_guides/berlitz1/HandRMadeira.txt
      86 ./written_2/travel_guides/berlitz1/HandRIbiza.txt
   10944 ./written_2/travel_guides/berlitz1/WhereToMallorca.txt
    2551 ./written_2/travel_guides/berlitz1/WhatToMallorca.txt
    1517 ./written_2/travel_guides/berlitz1/IntroJamaica.txt
    9690 ./written_2/travel_guides/berlitz1/WhereToMadeira.txt
    6175 ./written_2/travel_guides/berlitz1/WhereToIbiza.txt
    4925 ./written_2/travel_guides/berlitz1/HistoryMalaysia.txt
    2762 ./written_2/travel_guides/berlitz1/WhatToIndia.txt
    1764 ./written_2/travel_guides/berlitz1/HistoryIbiza.txt
    3758 ./written_2/travel_guides/berlitz1/IntroIndia.txt
   13099 ./written_2/travel_guides/berlitz1/WhatToJamaica.txt
    1268 ./written_2/travel_guides/berlitz1/IntroMallorca.txt
    ```
