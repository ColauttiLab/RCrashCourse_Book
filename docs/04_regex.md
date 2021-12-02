# Regular Expressions

## Overview

**Regular Expressions**, also known as **regex** and **regexp** is a syntax that allows for coders to run complex find-and-replace functions. I didn't learn regular expressions until I was a postdoc working at Duke University, but I wish I had learned this a lot earlier! This remains one of the most useful programming tools I have ever used. It is absolutely essential for working with any kind of large text files or large datasets.

A lot of programming tools in biology use input text files that require very specific formatting (e.g. .txt, .csv, .fasta, .nex). Sometimes, you might need to reorganize or recode data in a large text file. This can be a big pain that can lead to errors if you try to do everything manually. But regular expressions can automate the process.

Here's one example. As a PhD student I co-founded a project called the **Global Garlic Mustard Field Survey (GGMFS)** with collaborator Dr. Oliver Bossdorf at the University of Tuebingen -- yes the same Dr. Bossdorf mentioned in the [qplot Tutorial](https://colauttilab.github.io/RCrashCourse/2_qplot.html). We were fortunate to have over 100 collaborators across Europe and North America who helped to collect samples for the project. Details of the project were published in the Journal **Neobiota**: https://neobiota.pensoft.net/article/1270/ but one BIG problem is the way that each of these 100+ collaborators entered their data online. For example, latitudes and longitudes were entered in a variety of different formats. Regular expressions allowed me to write a small program to automatically convert all of these different formats to a common, decimal format that we could use for the analysis. This saved a huge amount of time and prevented errors that could have been introduced if we tried to edit these values by hand.

Often when you work with large datasets, you will need to automate some of your error correction, and regular expressions can be a big help here. For example, imagine a simple field where people were simply asked a simple yes or no question. You might find a variety of inputs such as: "YES, Y, yes, and Yes". These all mean the same thing but if you try to analyze it, R will treat these as different categories. Here again, regular expressions can be used to quickly change all the different examples to a common "Y" or even a boolean variable `TRUE`.

One final example, is pattern searching. This is common for the analysis of DNA, proteins or other large strings of data. You may want to find a particular sequence of data, possibly with a few variable sites: e.g. TCTA or TCAA or TCGA. This is another area where regular expressions can help.

### Universal

Regular expressions are a universal language that extends to many other programming languages, including **C/C#/C++**, **Python**, **Unix/Linux**, and **Perl**. We focus here on R but most of the syntax is mantained across programming languages.

> WARNING!

There is a very steep learning curve here, and the only way to really learn this is to drown yourself in examples. There are lots of exercises you can do for practice online. You should also try to apply these whenever you can.

## Functions

There are four main functions that use regular expressions in R. 

`grep()` and `grepl()` are equivalent to 'find' in your favorite word processor

They have the general form: `grep("find this pattern", in.this.object)`

`grep()` outputs a vector with all the address locations (i.e. numbers) that match. Thus the output length is equal to the number of matches.

`grepl()` outputs a vector of `TRUE` (match) and `FALSE` (no match). Thus, the output length is equal to the length of the input object.

`sub()` and `gsub()` are equivalent to 'find and replace'

They have the general form: `grep("find this pattern", "and replace with this", in.this.object)`

`sub()` replaces only the first match, whereas `gsub()` replaces all of the matches.

Some specific examples are provided below to help you understand these similarities and differences.

There are two other more advanced functions in R. These aren't covered in this tutorial, but may be of use once you are more comortable with the above functions.

`regexpr()` provides more detailed info about the first match

`gregexpr()` provides more detailed results about all matches

### Examples

Some examples can help to understand the differences among the four main functions. Let's start with a simple data frame of species names.


```r
Species<-c("petiolata", "verticillatus", "salicaria", "minor")
print(Species)
```

```
## [1] "petiolata"     "verticillatus" "salicaria"     "minor"
```

### `grep()` 

This returns cell addresses matching query. Note the vector length compared to the input vector.


```r
grep("a",Species)
```

```
## [1] 1 2 3
```

### `grepl()` 

This returns a vector of `TRUE` (match) and `FALSE` (no match). Comapre this output with `grep()`.


```r
grepl("a",Species)
```

```
## [1]  TRUE  TRUE  TRUE FALSE
```

### `sub()` 

This replaces the first match (in each cell)


```r
sub("l","L",Species)
```

```
## [1] "petioLata"     "verticiLlatus" "saLicaria"     "minor"
```

### `gsub()`  

This replaces all matches (in each cell). Compere this output to `sub()`.


```r
gsub("l","L",Species)
```

```
## [1] "petioLata"     "verticiLLatus" "saLicaria"     "minor"
```


## Wildcards

### `\` escape character

The backslash is a special character. It's called the 'escape' character because it is used to 'escapes' the interpretation of the next character that occurs after it. This is easier to understand by example, as shown below.

### `\\` in R

In the introduction, we discussed the universality of **regular expressions** in the sense that a similar syntax is used by many different programming langagues. But now here is one exception. In R, the double-escape is usually needed, whereas other programming languages typically use just one. The reason is a bit meta -- it's because we are running regular expressions within R object. So the first `\` is used to escape special characters in R, applying it to the second `\`, which is itself the special character that needs to be escaped to pass through the function. The second slash is followed by the 'escaped' character. Some examples are provided below.

If that isn't clear. Just remember that you need two backslashes instead of one.

### `\\w`

Instead of finding the letter `w`, the `\\w` is a **wildcard** character that represents any letter or digits.


```r
gsub("w","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xhich 1-100 Xords get replaced?"
```

```r
gsub("\\w","X","...which 1-100 words get replaced?")
```

```
## [1] "...XXXXX X-XXX XXXXX XXX XXXXXXXX?"
```

### `\\W` 

This is the inverse of `\\w` find a character that is NOT a letter or number.


```r
gsub("\\W","X","...which 1-100  words get replaced?")
```

```
## [1] "XXXwhichX1X100XXwordsXgetXreplacedX"
```

### `\\s`

This represents a space


```r
gsub("\\s","X","...which 1-100  words get replaced?")
```

```
## [1] "...whichX1-100XXwordsXgetXreplaced?"
```

### `\\t` 

This is a tab character. A lot of data files stored as text are tab-delimited (.tsv) as well as comma-delimited (.csv)


```r
gsub("\\t","X","...which 1-100  words get replaced?")
```

```
## [1] "...which 1-100  words get replaced?"
```

### `\\d` 

Digit characters


```r
gsub("\\d","X","...which 1-100  words get replaced?")
```

```
## [1] "...which X-XXX  words get replaced?"
```

### `\\D` 

Non-digit characters


```r
gsub("\\D","X","...which 1-100  words get replaced?")
```

```
## [1] "XXXXXXXXX1X100XXXXXXXXXXXXXXXXXXXXX"
```

## New Lines

There are two special characters that indicate new lines in a text file. 

### `\\r`

This is the 'carriage return' special character

### `\\n` 

This is the 'newline' special character

### Big Problem

One or two of these is/are generated when you press the 'enter' key while writing a text file. These also add a source of headache and confusion when working with text files because:

> Macs/Unix and PC/Windows use different standards!

Unix/Mac files -- lines usually end with `\\n` only

Windows/DOS files -- lines usually end with `\\r\\n`

This can cause problems when moving text files between Windows/DOS and Mac/Unix machines, particularly with older operating systems or when working on remote computers that use very basic Linux software.


## Special characters

In addition to special characters that use the escape `\\`, there are a number of other special characters that don't use the escape, but have a special meaning.

Note that if you want to search for the characters below you would have to use the escape character. E.g. `\\.` if you wanted to search for a period `.`.

### `|`

This is sometimes called the **pipe** character, and it simply means 'or'. For example, we can search for `w or e`.


```r
gsub("w|e","X","...which 1-100  words get replaced?")
```

```
## [1] "...Xhich 1-100  Xords gXt rXplacXd?"
```

### `.` 

This means any character except new line. This includes all of the `\\w` characters but also other characters like puncutation marks.


```r
gsub(".","X","...which 1-100  words get replaced?")
```

```
## [1] "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
```

So how to search for a period `.`? As noted above, we have to use the escape character


```r
gsub("\\.","X","...which 1-100  words get replaced?")
```

```
## [1] "XXXwhich 1-100  words get replaced?"
```

### `*`, `?`, `+`, `{}` 

These special characters refer to details about the kind of search that we are trying to conduct. Look at these examples carefully, and remember that `sub` replaces the first match while `gsub` replaces all of the matches.


```r
sub("\\w","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xhich 1-100 words get replaced?"
```

```r
gsub("\\w","X","...which 1-100 words get replaced?")
```

```
## [1] "...XXXXX X-XXX XXXXX XXX XXXXXXXX?"
```

Now let's apply some of these special characters to see how they work.

#### `+`

Finds 1 or more matches


```r
sub("\\w+","X","...which 1-100 words get replaced?")
```

```
## [1] "...X 1-100 words get replaced?"
```

```r
gsub("\\w+","X","...which 1-100 words get replaced?")
```

```
## [1] "...X X-X X X X?"
```

Compare this match to the one above. Notice how we have replaced groups of letters instead of single letters. The algorithm works like this: 

  1. Start at the left and move to the right, one character at a time
  2. Check if the character is a letter or number (`\\w`). 
  3. If NO, move to the next character
  4. If YES, check the next character. If it is also a `\\w` then go to the next character. Repeat until the next character is not `\\w`, and replace the entire string of characters.

When run in the `sub` command, it does the above and then stops. When run with the `gsub` command, it continues to the next character, and then starts over.

### `*` 

This is a 'greedy' search (match the largest possible)


```r
sub("\\w*","X","...which 1-100 words get replaced?")
```

```
## [1] "X...which 1-100 words get replaced?"
```

```r
gsub("\\w*","X","...which 1-100 words get replaced?")
```

```
## [1] "X.X.X.X X-X X X X?X"
```

In the `sub` command, it detects a `.` as the first character, indicating no match. It replaces the 'null' or `0` match at the beginning, which has the effect of adding a character. In the `gsub` command it repeats this before each `.` until it finds the letter `w`. Then it finds a group of `\\w` matches, replacing with a single star. Then a space, which is skipped, then a `-`, which is another null match, prompting another insert.


### `?` 

This means 'match zero or one time'


```r
sub("\\w?","X","...which 1-100 words get replaced?")
```

```
## [1] "X...which 1-100 words get replaced?"
```

```r
gsub("\\w?","X","...which 1-100 words get replaced?")
```

```
## [1] "X.X.X.XXXXX X-XXX XXXXX XXX XXXXXXXX?X"
```
Compare this to the `*` above. It behaves in a similar way, except it is not 'greedy' -- in the second example, each letter is replaced instead of entire words.

###  `+?` 

This is the 'lazy' version of `+` -- note in particular the difference in `sub` which replaces on the the first letter here but the whole word when `+` is used alone. In the `gsub` example we end up replacing every letter instead of whole words. Remember, `sub` runs the algorithm once and then stops, while `gsub` repeats until it reaches the end of the line.


```r
sub("\\w+?","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xhich 1-100 words get replaced?"
```

```r
gsub("\\w+?","X","...which 1-100 words get replaced?")
```

```
## [1] "...XXXXX X-XXX XXXXX XXX XXXXXXXX?"
```

### `*?`

Similarly, we can combine these characters for the 'lazy' version of `*`


```r
sub("\\w*?","X","...which 1-100 words get replaced?")
```

```
## [1] "X...which 1-100 words get replaced?"
```

```r
gsub("\\w*?","X","...which 1-100 words get replaced?")
```

```
## [1] "X.X.X.XwXhXiXcXhX X1X-X1X0X0X XwXoXrXdXsX XgXeXtX XrXeXpXlXaXcXeXdX?X"
```

> Try using `+*`. Why do you get an error message? 

### `{}`

Curly brackets are used to specify a number of matches, expanding on the options even futher.

### `{n,m}` 

Find between $n$ to $m$ matches


```r
gsub("\\w{3,4}","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xh 1-X Xs X XX?"
```

### `{n}` 

Find exactly $n$ matches


```r
gsub("\\w{3}","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xch 1-X Xds X XXed?"
```

### `{n,}`

Fomd $n$ or more matches


```r
gsub("\\w{4,}","X","...which 1-100 words get replaced?")
```

```
## [1] "...X 1-100 X get X?"
```

### {}?

As above, we cau use `?` for the 'lazy' versions of these searches


```r
gsub("\\w{4,}?","X","...which 1-100 words get replaced?")
```

```
## [1] "...Xh 1-100 Xs get XX?"
```


## `[]`

Square brackets allow us to find 'any' of the values listed within them. We can also use the dash `-` to specify a range of numbers or letters.


```r
gsub("[aceihw-z]","X","...which 1-100 words get replaced?")
```

```
## [1] "...XXXXX 1-100 Xords gXt rXplXXXd?"
```

In the above example, we search for 1 of any of the listed letters: a, c, e, i h, w, x, y, z. Note that x and y are included in the `x-z` statement.

What if we want to find 1 or more of these characters in a row?


```r
gsub("[aceihw-z]+","X","...which 1-100 words get replaced?")
```

```
## [1] "...X 1-100 Xords gXt rXplXd?"
```

## `^` and `$`

Use these characters to specify searches at the start `^` or end `$` of the input string.

### `^`

Find species starting with the letter *s*


```r
grep("^s",Species)
```

```
## [1] 3
```

> IMPORTANT: `^` Also means 'negate' when used within `[]`

Find species containing any letter other than *s*


```r
grep("[^a]",Species)
```

```
## [1] 1 2 3 4
```

Replace every letter except *s*


```r
gsub("[^a]","X",Species)
```

```
## [1] "XXXXXXaXa"     "XXXXXXXXXaXXX" "XaXXXaXXa"     "XXXXX"
```

### `$`

Find species ending with *a*


```r
grep("a$",Species)
```

```
## [1] 1 3
```


## `()`

Regular parentheses are used to 'capture' text, which can then be specified in the replacement string using `\\1`. Or you can capture multiple pieces of text and reorganize them by using the corresponding number -- `\\1` for the first set of`()`, `\\2` for the second set of `()`, etc. Some examples should help. 

Replace each word with its first letter


```r
gsub("(\\w)\\w+","\\1","...which 1-100 words get replaced?")
```

```
## [1] "...w 1-1 w g r?"
```

Pull out only the numbers and reverse their order


```r
gsub(".*([0-9]+)-([0-9]+).*","\\2-\\1","...which 1-100 words get replaced?")
```

```
## [1] "100-1"
```

Reverse first two letters of each word


```r
gsub("(\\w)(\\w)(\\w+)","\\2\\1\\3","...which 1-100 words get replaced?")
```

```
## [1] "...hwich 1-010 owrds egt erplaced?"
```

## Scraping

Scraping is a method for collecting data from online sources. In R, we can use the functions `readLines` and `curl()`, both from the `curl` library, to 'scrape' data from websites. Websites with the `.html` extension are a special kind of text file. 

We can use regular expressions to pull out text from the website. Here's an example where we will scrape a record for the Green Fluorescent Protein (GFP) from the Protein Data Bank (PDB). Note that this is a file with the extension `.pdb` but this is a human-readable text file that can be opened in any text editor

First, we'll import the text into an R object.


```r
library(curl) 
```

```
## Using libcurl 7.64.1 with Schannel
```

You will have to use `install.packages("curl")` to download this package to your computer. You only need to do this once but you will have to use `library(curl)` whenever you want to use the commands, as explained in the [R Fundamentals Tutorial](https://colauttilab.github.io/RCrashCourse/1_fundamentals.html)

Now we can download a file to play with.


```r
Prot<-readLines(curl("http://www.rcsb.org/pdb/files/1ema.pdb"))
```

> HINT: Download this link to your computer and open with a text file. 

This hint is a simple trick to understand what kind of file(s) you are working with.

This is a tab-delimited file, which we could import as a data frame using `read.delim` but we'll keep it this way to see how we can use regular expressions.

The `Prot` object we have made is a simple vector of strings, with each cell corresponding to a different row of text:


```r
length(Prot)
```

```
## [1] 2363
```

```r
Prot[grep("TITLE",Prot)]
```

```
## [1] "TITLE     GREEN FLUORESCENT PROTEIN FROM AEQUOREA VICTORIA                      "
```

We can pull out the amino acid sequences, which are rows that start with the word 'ATOM'


```r
AAseq<-Prot[grep("^ATOM",Prot)]
length(AAseq)
```

```
## [1] 1717
```

```r
AAseq[1]
```

```
## [1] "ATOM      1  N   SER A   2      28.888   9.409  52.301  1.00 85.05           N  "
```
> Try to isolate the 3-letter amino acid code

There are lots of possibilities. Take the time to try a few.

Here's one good option, since we know it's a tab-delimited file with the amino acid in the 4th column:


```r
gsub("ATOM\\t\\w+\\t\\w+\\t(\\w+).*","\\1",AAseq[1])
```

```
## [1] "ATOM      1  N   SER A   2      28.888   9.409  52.301  1.00 85.05           N  "
```

That didn't work. Sometimes the 'tabs' are actually just multiple 'spaces'


```r
AAchain<-gsub("ATOM\\s+\\w+\\s+\\w+\\s+(\\w+).*","\\1",AAseq)
AAchain[1:100]
```

```
##   [1] "SER" "SER" "SER" "SER" "SER" "SER" "LYS" "LYS" "LYS" "LYS" "LYS" "LYS"
##  [13] "LYS" "LYS" "LYS" "GLY" "GLY" "GLY" "GLY" "GLU" "GLU" "GLU" "GLU" "GLU"
##  [25] "GLU" "GLU" "GLU" "GLU" "GLU" "GLU" "GLU" "GLU" "GLU" "GLU" "LEU" "LEU"
##  [37] "LEU" "LEU" "LEU" "LEU" "LEU" "LEU" "PHE" "PHE" "PHE" "PHE" "PHE" "PHE"
##  [49] "PHE" "PHE" "PHE" "PHE" "PHE" "THR" "THR" "THR" "THR" "THR" "THR" "THR"
##  [61] "GLY" "GLY" "GLY" "GLY" "VAL" "VAL" "VAL" "VAL" "VAL" "VAL" "VAL" "VAL"
##  [73] "VAL" "VAL" "VAL" "VAL" "VAL" "VAL" "PRO" "PRO" "PRO" "PRO" "PRO" "PRO"
##  [85] "PRO" "ILE" "ILE" "ILE" "ILE" "ILE" "ILE" "ILE" "ILE" "LEU" "LEU" "LEU"
##  [97] "LEU" "LEU" "LEU" "LEU"
```

## Examples 

Let's try practicing with a couple of examples

### Transect Data

Regular expressions are also useful with data objects

Imagine you have a repeated measures design. 3 transects (A-C) and 3 positions along each transect (1-3)


```r
Transect<-data.frame(Species=letters[1:20],A1=rnorm(20),A2=rnorm(20),A3=rnorm(20),B1=rnorm(20),B2=rnorm(20),B3=rnorm(20),C1=rnorm(20),C2=rnorm(20),C3=rnorm(20))
head(Transect)
```

```
##   Species          A1         A2          A3         B1         B2         B3
## 1       a  0.07139707 -0.7728001  0.81305648 -0.6098756  0.3799136  0.8451857
## 2       b -0.85109159 -0.5640395 -0.88588115 -0.7857094  1.5084828  0.5826571
## 3       c  1.08841353  1.3048859  0.05254901 -0.6883161 -1.0110300 -0.3930457
## 4       d -0.62425975 -0.4979739 -0.80039790  2.0192492  0.7369518  0.7150064
## 5       e -2.10014932 -1.1267024 -0.14952315  1.9066553 -0.7954435  1.5053513
## 6       f -1.96116164 -0.9376691 -1.00747944 -0.5799077 -3.2586303  0.4925379
##           C1         C2         C3
## 1 -2.6729185 -2.1552864 -2.0309665
## 2 -0.6006691 -0.7511939 -0.1027611
## 3  1.5251702  1.6607134 -0.9350379
## 4  0.1715294  3.4725070  0.2424075
## 5 -0.1584703 -1.6796146 -1.2852899
## 6  0.9523366  0.5827346  0.1407577
```

> TIP: the object `letters` contains lower-case letter, while `LETTERS` contains upper case.

#### Challenge 

Use your knowledged gained above with substting data outlined in the [R Fundamentals Tutorial](https://colauttilab.github.io/RCrashCourse/1_fundamentals.html) to do the following:

  1. Subset only the columns that have an "A" in their name
  2. Subset the data for species "D"

Take the time to do this on your own. It will take you a while and you will make a lot of mistakes. That's all part of the learning process. The longer you struglle, the faster you will learn. 

Now here is a more challenging example:


### Genbank


```r
Lythrum_18S<-scan("https://colauttilab.github.io/RCrashCourse/sequence.gb",what="character",sep="\n")
```

This is the sequence of the 18S subunit from the ribosome gene of *Lythrum salicaria* (from Genbank)


```r
print(Lythrum_18S)
```

```
##  [1] "LOCUS       AF206955                1740 bp    DNA     linear   PLN 18-APR-2003"
##  [2] "DEFINITION  Lythrum salicaria 18S ribosomal RNA gene, complete sequence."       
##  [3] "ACCESSION   AF206955"                                                           
##  [4] "VERSION     AF206955.1"                                                         
##  [5] "KEYWORDS    ."                                                                  
##  [6] "SOURCE      Lythrum salicaria"                                                  
##  [7] "  ORGANISM  Lythrum salicaria"                                                  
##  [8] "            Eukaryota; Viridiplantae; Streptophyta; Embryophyta; Tracheophyta;" 
##  [9] "            Spermatophyta; Magnoliopsida; eudicotyledons; Gunneridae;"          
## [10] "            Pentapetalae; rosids; malvids; Myrtales; Lythraceae; Lythrum."      
## [11] "REFERENCE   1  (bases 1 to 1740)"                                               
## [12] "  AUTHORS   Soltis,P.S., Soltis,D.E. and Chase,M.W."                            
## [13] "  TITLE     Direct Submission"                                                  
## [14] "  JOURNAL   Submitted (19-NOV-1999) School of Biological Sciences, Washington"  
## [15] "            State University, Pullman, WA 99164-4236, USA"                      
## [16] "FEATURES             Location/Qualifiers"                                       
## [17] "     source          1..1740"                                                   
## [18] "                     /organism=\"Lythrum salicaria\""                           
## [19] "                     /mol_type=\"genomic DNA\""                                 
## [20] "                     /db_xref=\"taxon:13129\""                                  
## [21] "                     /note=\"Lythrum salicaria L.\""                            
## [22] "     rRNA            1..1740"                                                   
## [23] "                     /product=\"18S ribosomal RNA\""                            
## [24] "ORIGIN      "                                                                   
## [25] "        1 gtcatatgct tgtctcaaag attaagccat gcatgtgtaa gtatgaacaa attcagactg"    
## [26] "       61 tgaaactgcg aatggctcat taaatcagtt atagtttgtt tgatggtatc tgctactcgg"    
## [27] "      121 ataaccgtag taattctaga gctaatacgt gcaacaaacc ccgacttctg gaagggacgc"    
## [28] "      181 atttattaga taaaaggtcg acgcgggctt tgcccgatgc tctgatgatt catgataact"    
## [29] "      241 tgacggatcg cacggccatc gtgccggcga cgcatcattc aaatttctgc cctatcaact"    
## [30] "      301 ttcgatggta ggatagtggc ctaccatggt gtttacgggt aacggagaat tagggttcga"    
## [31] "      361 ttccggagag ggagcctgag aaacggctac cacatccaag gaaggcagca ggcgcgcaaa"    
## [32] "      421 ttacccaatc ctgacacggg gaggtagtga caataaataa caatactggg ctctttgagt"    
## [33] "      481 ctggtaattg gaatgagtac aatctaaatc ccttaacgag gatccattgg agggcaagtc"    
## [34] "      541 tggtgccagc agccgcggta attccagctc caatagcgta tatttaagtt gttgcagtta"    
## [35] "      601 aaaagctcgt agttggacct tgggttgggt cgaccggtcc gcctttggtg tgcaccgatc"    
## [36] "      661 ggctcgtccc ttctaccggc gatgcgcgcc tggccttaat tggccgggtc gttcctccgg"    
## [37] "      721 tgctgttact ttgaagaaat tagagtgctc aaagcaagca ttagctatga atacattagc"    
## [38] "      781 atgggataac attataggat tccgatccta ttatgttggc cttcgggatc ggagtaatga"    
## [39] "      841 ttaacaggga cagtcggggg cattcgtatt tcatagtcag aggtgaaatt cttggattta"    
## [40] "      901 tgaaagacga acaactgcga aagcatttgc caaggatgtt ttcattaatc aagaacgaaa"    
## [41] "      961 gttgggggct cgaagacgat cagataccgt cctagtctca accataaacg atgccgacca"    
## [42] "     1021 gggatcagcg aatgttactt ttaggacttc gctggcacct tatgagaaat caaagttttt"    
## [43] "     1081 gggttccggg gggagtatgg tcgcaaggct gaaacttaaa ggaattgacg gaagggcacc"    
## [44] "     1141 accaggagtg gagcctgcgg cttaatttga ctcaacacgg ggaaacttac caggtccaga"    
## [45] "     1201 catagtaagg attgacagac tgagagctct ttcttgattc tatgggtggt ggtgcatggc"    
## [46] "     1261 cgttcttagt tggtggagcg atttgtctgg ttaattccgt taacgaacga gacctcagcc"    
## [47] "     1321 tgctaactag ctatgtggag gtacacctcc acggccagct tcttagaggg actatggccg"    
## [48] "     1381 cttaggccaa ggaagtttga ggcaataaca ggtctgtgat gcccttagat gttctgggcc"    
## [49] "     1441 gcacgcgcgc tacactgatg tattcaacga gtctatagcc ttggccgaca ggcccgggta"    
## [50] "     1501 atctttgaaa tttcatcgtg atggggatag atcattgcaa ttgttggtct tcaacgagga"    
## [51] "     1561 attcctagta agcgcgagtc atcagctcgc gttgactacg tccctgccct ttgtacacac"    
## [52] "     1621 cgcccgtcgc tcctaccgat tgaatggtcc ggtgaaatgt tcggatcgcg gcgacgtggg"    
## [53] "     1681 cgcttcgtcg ccgacgacgt cgcgagaagt ccattgaacc ttatcattta gaggaaggag"    
## [54] "//"
```

Notice that each line is read in as a separate cell in a vector, with sequences beginning with a number ending with 1. We can take advantage of this to extract just the sequence data

#### Challenge

**Before proceeding** try to do the following:

  1. Isolate only the rows containing DNA sequences. This should include
  
    a. Removing all of the characters that are not a, t, g, or c.
    b. Combining separate cells/lines into a single string. You can do this with using the `paste()` function with the `collapse=""` parameter 
    
  2. Convert lower-case to upper-case. To do this, you can use `gsub("([actg])","\\U\\1",Seq,perl=T)`. The `\\U\\\1` means 'paste brackets as upper-case, and is only available as a **Perl** command, which is accessible in gsub with the `perl=T` parameter.
  3. Replace start codons (ATG) with "-->START-->ATG"
  4. Replace stop codons (TAA or TAG or TGA) with TAA or TAG or TGA followed by ">--STOP--|"


Take the time to stuggle with this and try different combinations until you find a way through. The more you struggle, the faster you will learn.

A cool thing about regular expressions is that there is rarely a single right answer, especially for complicated problems. When you are ready, Continue on to see one possible solution.


## Solutions

### Transects

Imagine you have a repeated measures design. 3 transects (A-C) and 3 positions along each transect (1-3)


```r
Transect<-data.frame(Species=1:20,A1=rnorm(20),A2=rnorm(20),A3=rnorm(20),B1=rnorm(20),B2=rnorm(20),B3=rnorm(20),C1=rnorm(20),C2=rnorm(20),C3=rnorm(20))
head(Transect)
```

```
##   Species         A1         A2          A3        B1         B2          B3
## 1       1 -0.3551728  1.5574030  0.92831524 1.2809810 -0.1825205 -1.81648927
## 2       2 -0.5005998  1.8180958 -2.69323529 0.9405535 -0.3922514  0.06811127
## 3       3 -0.4393575 -0.4473246  0.02226106 0.6968660 -0.4779725 -0.11903021
## 4       4  1.9956463  0.8015969  0.06578602 1.7468116  0.7371601  0.14762524
## 5       5  1.2568587  1.2727379 -0.10217342 0.6973361  2.4661032  0.88846771
## 6       6  2.2362467 -0.4398664 -1.08099609 0.6642294 -0.2111424 -0.25869663
##           C1          C2         C3
## 1  0.3888588 -0.53862615 1.63044736
## 2 -0.7136629 -0.27674505 0.23562220
## 3  1.1601536 -0.64381623 1.20883465
## 4  1.8521279  0.17199073 0.61716530
## 5  0.3304053 -0.05247375 0.15312470
## 6  1.2250694 -1.26948339 0.07721129
```

You want to look at only transect A for the first 3 species


```r
Transect[1:3,grep("A",names(Transect))]
```

```
##           A1         A2          A3
## 1 -0.3551728  1.5574030  0.92831524
## 2 -0.5005998  1.8180958 -2.69323529
## 3 -0.4393575 -0.4473246  0.02226106
```

Subset the data for species "D"


```r
Transect[grep("1",names(Transect)),]
```

```
##   Species         A1         A2         A3         B1         B2         B3
## 2       2 -0.5005998  1.8180958 -2.6932353  0.9405535 -0.3922514 0.06811127
## 5       5  1.2568587  1.2727379 -0.1021734  0.6973361  2.4661032 0.88846771
## 8       8 -0.5934295 -0.3563004  0.6264754 -0.3666217  0.2624573 1.59699670
##           C1          C2         C3
## 2 -0.7136629 -0.27674505  0.2356222
## 5  0.3304053 -0.05247375  0.1531247
## 8  0.8995420 -1.49135884 -1.3148355
```

### Genbank

Use `.*` with `()` to delete everything before the DNA sequence


```r
Seq<-gsub(".*(1 [gatc])","",Lythrum_18S)
paste(Seq,collapse="")
```

```
## [1] "LOCUS       AF206955                1740 bp    DNA     linear   PLN 18-APR-2003DEFINITION  Lythrum salicaria 18S ribosomal RNA gene, complete sequence.ACCESSION   AF206955VERSION     AF206955.1KEYWORDS    .SOURCE      Lythrum salicaria  ORGANISM  Lythrum salicaria            Eukaryota; Viridiplantae; Streptophyta; Embryophyta; Tracheophyta;            Spermatophyta; Magnoliopsida; eudicotyledons; Gunneridae;            Pentapetalae; rosids; malvids; Myrtales; Lythraceae; Lythrum.o 1740)  AUTHORS   Soltis,P.S., Soltis,D.E. and Chase,M.W.  TITLE     Direct Submission  JOURNAL   Submitted (19-NOV-1999) School of Biological Sciences, Washington            State University, Pullman, WA 99164-4236, USAFEATURES             Location/Qualifiers     source          1..1740                     /organism=\"Lythrum salicaria\"                     /mol_type=\"genomic DNA\"                     /db_xref=\"taxon:13129\"                     /note=\"Lythrum salicaria L.\"     rRNA            1..1740                     /product=\"18S ribosomal RNA\"ORIGIN      tcatatgct tgtctcaaag attaagccat gcatgtgtaa gtatgaacaa attcagactggaaactgcg aatggctcat taaatcagtt atagtttgtt tgatggtatc tgctactcggtaaccgtag taattctaga gctaatacgt gcaacaaacc ccgacttctg gaagggacgctttattaga taaaaggtcg acgcgggctt tgcccgatgc tctgatgatt catgataactgacggatcg cacggccatc gtgccggcga cgcatcattc aaatttctgc cctatcaacttcgatggta ggatagtggc ctaccatggt gtttacgggt aacggagaat tagggttcgatccggagag ggagcctgag aaacggctac cacatccaag gaaggcagca ggcgcgcaaatacccaatc ctgacacggg gaggtagtga caataaataa caatactggg ctctttgagttggtaattg gaatgagtac aatctaaatc ccttaacgag gatccattgg agggcaagtcggtgccagc agccgcggta attccagctc caatagcgta tatttaagtt gttgcagttaaaagctcgt agttggacct tgggttgggt cgaccggtcc gcctttggtg tgcaccgatcgctcgtccc ttctaccggc gatgcgcgcc tggccttaat tggccgggtc gttcctccgggctgttact ttgaagaaat tagagtgctc aaagcaagca ttagctatga atacattagctgggataac attataggat tccgatccta ttatgttggc cttcgggatc ggagtaatgataacaggga cagtcggggg cattcgtatt tcatagtcag aggtgaaatt cttggatttagaaagacga acaactgcga aagcatttgc caaggatgtt ttcattaatc aagaacgaaattgggggct cgaagacgat cagataccgt cctagtctca accataaacg atgccgaccaggatcagcg aatgttactt ttaggacttc gctggcacct tatgagaaat caaagtttttggttccggg gggagtatgg tcgcaaggct gaaacttaaa ggaattgacg gaagggcaccccaggagtg gagcctgcgg cttaatttga ctcaacacgg ggaaacttac caggtccagaatagtaagg attgacagac tgagagctct ttcttgattc tatgggtggt ggtgcatggcgttcttagt tggtggagcg atttgtctgg ttaattccgt taacgaacga gacctcagccgctaactag ctatgtggag gtacacctcc acggccagct tcttagaggg actatggccgttaggccaa ggaagtttga ggcaataaca ggtctgtgat gcccttagat gttctgggcccacgcgcgc tacactgatg tattcaacga gtctatagcc ttggccgaca ggcccgggtatctttgaaa tttcatcgtg atggggatag atcattgcaa ttgttggtct tcaacgaggattcctagta agcgcgagtc atcagctcgc gttgactacg tccctgccct ttgtacacacgcccgtcgc tcctaccgat tgaatggtcc ggtgaaatgt tcggatcgcg gcgacgtggggcttcgtcg ccgacgacgt cgcgagaagt ccattgaacc ttatcattta gaggaaggag//"
```

Use the `.*` and space with `+` to eliminate all text before the sequence :


```r
Seq<-gsub(".*ORIGIN +","",paste(Seq,collapse=""))
Seq
```

```
## [1] "tcatatgct tgtctcaaag attaagccat gcatgtgtaa gtatgaacaa attcagactggaaactgcg aatggctcat taaatcagtt atagtttgtt tgatggtatc tgctactcggtaaccgtag taattctaga gctaatacgt gcaacaaacc ccgacttctg gaagggacgctttattaga taaaaggtcg acgcgggctt tgcccgatgc tctgatgatt catgataactgacggatcg cacggccatc gtgccggcga cgcatcattc aaatttctgc cctatcaacttcgatggta ggatagtggc ctaccatggt gtttacgggt aacggagaat tagggttcgatccggagag ggagcctgag aaacggctac cacatccaag gaaggcagca ggcgcgcaaatacccaatc ctgacacggg gaggtagtga caataaataa caatactggg ctctttgagttggtaattg gaatgagtac aatctaaatc ccttaacgag gatccattgg agggcaagtcggtgccagc agccgcggta attccagctc caatagcgta tatttaagtt gttgcagttaaaagctcgt agttggacct tgggttgggt cgaccggtcc gcctttggtg tgcaccgatcgctcgtccc ttctaccggc gatgcgcgcc tggccttaat tggccgggtc gttcctccgggctgttact ttgaagaaat tagagtgctc aaagcaagca ttagctatga atacattagctgggataac attataggat tccgatccta ttatgttggc cttcgggatc ggagtaatgataacaggga cagtcggggg cattcgtatt tcatagtcag aggtgaaatt cttggatttagaaagacga acaactgcga aagcatttgc caaggatgtt ttcattaatc aagaacgaaattgggggct cgaagacgat cagataccgt cctagtctca accataaacg atgccgaccaggatcagcg aatgttactt ttaggacttc gctggcacct tatgagaaat caaagtttttggttccggg gggagtatgg tcgcaaggct gaaacttaaa ggaattgacg gaagggcaccccaggagtg gagcctgcgg cttaatttga ctcaacacgg ggaaacttac caggtccagaatagtaagg attgacagac tgagagctct ttcttgattc tatgggtggt ggtgcatggcgttcttagt tggtggagcg atttgtctgg ttaattccgt taacgaacga gacctcagccgctaactag ctatgtggag gtacacctcc acggccagct tcttagaggg actatggccgttaggccaa ggaagtttga ggcaataaca ggtctgtgat gcccttagat gttctgggcccacgcgcgc tacactgatg tattcaacga gtctatagcc ttggccgaca ggcccgggtatctttgaaa tttcatcgtg atggggatag atcattgcaa ttgttggtct tcaacgaggattcctagta agcgcgagtc atcagctcgc gttgactacg tccctgccct ttgtacacacgcccgtcgc tcctaccgat tgaatggtcc ggtgaaatgt tcggatcgcg gcgacgtggggcttcgtcg ccgacgacgt cgcgagaagt ccattgaacc ttatcattta gaggaaggag//"
```

Elimminate spaces and the two `//` at the end


```r
Seq<-gsub(" |//","",Seq)
Seq
```

```
## [1] "tcatatgcttgtctcaaagattaagccatgcatgtgtaagtatgaacaaattcagactggaaactgcgaatggctcattaaatcagttatagtttgtttgatggtatctgctactcggtaaccgtagtaattctagagctaatacgtgcaacaaaccccgacttctggaagggacgctttattagataaaaggtcgacgcgggctttgcccgatgctctgatgattcatgataactgacggatcgcacggccatcgtgccggcgacgcatcattcaaatttctgccctatcaacttcgatggtaggatagtggcctaccatggtgtttacgggtaacggagaattagggttcgatccggagagggagcctgagaaacggctaccacatccaaggaaggcagcaggcgcgcaaatacccaatcctgacacggggaggtagtgacaataaataacaatactgggctctttgagttggtaattggaatgagtacaatctaaatcccttaacgaggatccattggagggcaagtcggtgccagcagccgcggtaattccagctccaatagcgtatatttaagttgttgcagttaaaagctcgtagttggaccttgggttgggtcgaccggtccgcctttggtgtgcaccgatcgctcgtcccttctaccggcgatgcgcgcctggccttaattggccgggtcgttcctccgggctgttactttgaagaaattagagtgctcaaagcaagcattagctatgaatacattagctgggataacattataggattccgatcctattatgttggccttcgggatcggagtaatgataacagggacagtcgggggcattcgtatttcatagtcagaggtgaaattcttggatttagaaagacgaacaactgcgaaagcatttgccaaggatgttttcattaatcaagaacgaaattgggggctcgaagacgatcagataccgtcctagtctcaaccataaacgatgccgaccaggatcagcgaatgttacttttaggacttcgctggcaccttatgagaaatcaaagtttttggttccggggggagtatggtcgcaaggctgaaacttaaaggaattgacggaagggcaccccaggagtggagcctgcggcttaatttgactcaacacggggaaacttaccaggtccagaatagtaaggattgacagactgagagctctttcttgattctatgggtggtggtgcatggcgttcttagttggtggagcgatttgtctggttaattccgttaacgaacgagacctcagccgctaactagctatgtggaggtacacctccacggccagcttcttagagggactatggccgttaggccaaggaagtttgaggcaataacaggtctgtgatgcccttagatgttctgggcccacgcgcgctacactgatgtattcaacgagtctatagccttggccgacaggcccgggtatctttgaaatttcatcgtgatggggatagatcattgcaattgttggtcttcaacgaggattcctagtaagcgcgagtcatcagctcgcgttgactacgtccctgccctttgtacacacgcccgtcgctcctaccgattgaatggtccggtgaaatgttcggatcgcggcgacgtggggcttcgtcgccgacgacgtcgcgagaagtccattgaaccttatcatttagaggaaggag"
```

Capital letters look nicer, but requires a PERL qualifier `\\U` that is not standard in R


```r
Seq<-gsub("([actg])","\\U\\1",Seq,perl=T)
print(Seq)
```

```
## [1] "TCATATGCTTGTCTCAAAGATTAAGCCATGCATGTGTAAGTATGAACAAATTCAGACTGGAAACTGCGAATGGCTCATTAAATCAGTTATAGTTTGTTTGATGGTATCTGCTACTCGGTAACCGTAGTAATTCTAGAGCTAATACGTGCAACAAACCCCGACTTCTGGAAGGGACGCTTTATTAGATAAAAGGTCGACGCGGGCTTTGCCCGATGCTCTGATGATTCATGATAACTGACGGATCGCACGGCCATCGTGCCGGCGACGCATCATTCAAATTTCTGCCCTATCAACTTCGATGGTAGGATAGTGGCCTACCATGGTGTTTACGGGTAACGGAGAATTAGGGTTCGATCCGGAGAGGGAGCCTGAGAAACGGCTACCACATCCAAGGAAGGCAGCAGGCGCGCAAATACCCAATCCTGACACGGGGAGGTAGTGACAATAAATAACAATACTGGGCTCTTTGAGTTGGTAATTGGAATGAGTACAATCTAAATCCCTTAACGAGGATCCATTGGAGGGCAAGTCGGTGCCAGCAGCCGCGGTAATTCCAGCTCCAATAGCGTATATTTAAGTTGTTGCAGTTAAAAGCTCGTAGTTGGACCTTGGGTTGGGTCGACCGGTCCGCCTTTGGTGTGCACCGATCGCTCGTCCCTTCTACCGGCGATGCGCGCCTGGCCTTAATTGGCCGGGTCGTTCCTCCGGGCTGTTACTTTGAAGAAATTAGAGTGCTCAAAGCAAGCATTAGCTATGAATACATTAGCTGGGATAACATTATAGGATTCCGATCCTATTATGTTGGCCTTCGGGATCGGAGTAATGATAACAGGGACAGTCGGGGGCATTCGTATTTCATAGTCAGAGGTGAAATTCTTGGATTTAGAAAGACGAACAACTGCGAAAGCATTTGCCAAGGATGTTTTCATTAATCAAGAACGAAATTGGGGGCTCGAAGACGATCAGATACCGTCCTAGTCTCAACCATAAACGATGCCGACCAGGATCAGCGAATGTTACTTTTAGGACTTCGCTGGCACCTTATGAGAAATCAAAGTTTTTGGTTCCGGGGGGAGTATGGTCGCAAGGCTGAAACTTAAAGGAATTGACGGAAGGGCACCCCAGGAGTGGAGCCTGCGGCTTAATTTGACTCAACACGGGGAAACTTACCAGGTCCAGAATAGTAAGGATTGACAGACTGAGAGCTCTTTCTTGATTCTATGGGTGGTGGTGCATGGCGTTCTTAGTTGGTGGAGCGATTTGTCTGGTTAATTCCGTTAACGAACGAGACCTCAGCCGCTAACTAGCTATGTGGAGGTACACCTCCACGGCCAGCTTCTTAGAGGGACTATGGCCGTTAGGCCAAGGAAGTTTGAGGCAATAACAGGTCTGTGATGCCCTTAGATGTTCTGGGCCCACGCGCGCTACACTGATGTATTCAACGAGTCTATAGCCTTGGCCGACAGGCCCGGGTATCTTTGAAATTTCATCGTGATGGGGATAGATCATTGCAATTGTTGGTCTTCAACGAGGATTCCTAGTAAGCGCGAGTCATCAGCTCGCGTTGACTACGTCCCTGCCCTTTGTACACACGCCCGTCGCTCCTACCGATTGAATGGTCCGGTGAAATGTTCGGATCGCGGCGACGTGGGGCTTCGTCGCCGACGACGTCGCGAGAAGTCCATTGAACCTTATCATTTAGAGGAAGGAG"
```

Look for start codons?


```r
gsub("ATG","-->START-->ATG",Seq)
```

```
## [1] "TCAT-->START-->ATGCTTGTCTCAAAGATTAAGCC-->START-->ATGC-->START-->ATGTGTAAGT-->START-->ATGAACAAATTCAGACTGGAAACTGCGA-->START-->ATGGCTCATTAAATCAGTTATAGTTTGTTTG-->START-->ATGGTATCTGCTACTCGGTAACCGTAGTAATTCTAGAGCTAATACGTGCAACAAACCCCGACTTCTGGAAGGGACGCTTTATTAGATAAAAGGTCGACGCGGGCTTTGCCCG-->START-->ATGCTCTG-->START-->ATGATTC-->START-->ATGATAACTGACGGATCGCACGGCCATCGTGCCGGCGACGCATCATTCAAATTTCTGCCCTATCAACTTCG-->START-->ATGGTAGGATAGTGGCCTACC-->START-->ATGGTGTTTACGGGTAACGGAGAATTAGGGTTCGATCCGGAGAGGGAGCCTGAGAAACGGCTACCACATCCAAGGAAGGCAGCAGGCGCGCAAATACCCAATCCTGACACGGGGAGGTAGTGACAATAAATAACAATACTGGGCTCTTTGAGTTGGTAATTGGA-->START-->ATGAGTACAATCTAAATCCCTTAACGAGGATCCATTGGAGGGCAAGTCGGTGCCAGCAGCCGCGGTAATTCCAGCTCCAATAGCGTATATTTAAGTTGTTGCAGTTAAAAGCTCGTAGTTGGACCTTGGGTTGGGTCGACCGGTCCGCCTTTGGTGTGCACCGATCGCTCGTCCCTTCTACCGGCG-->START-->ATGCGCGCCTGGCCTTAATTGGCCGGGTCGTTCCTCCGGGCTGTTACTTTGAAGAAATTAGAGTGCTCAAAGCAAGCATTAGCT-->START-->ATGAATACATTAGCTGGGATAACATTATAGGATTCCGATCCTATT-->START-->ATGTTGGCCTTCGGGATCGGAGTA-->START-->ATGATAACAGGGACAGTCGGGGGCATTCGTATTTCATAGTCAGAGGTGAAATTCTTGGATTTAGAAAGACGAACAACTGCGAAAGCATTTGCCAAGG-->START-->ATGTTTTCATTAATCAAGAACGAAATTGGGGGCTCGAAGACGATCAGATACCGTCCTAGTCTCAACCATAAACG-->START-->ATGCCGACCAGGATCAGCGA-->START-->ATGTTACTTTTAGGACTTCGCTGGCACCTT-->START-->ATGAGAAATCAAAGTTTTTGGTTCCGGGGGGAGT-->START-->ATGGTCGCAAGGCTGAAACTTAAAGGAATTGACGGAAGGGCACCCCAGGAGTGGAGCCTGCGGCTTAATTTGACTCAACACGGGGAAACTTACCAGGTCCAGAATAGTAAGGATTGACAGACTGAGAGCTCTTTCTTGATTCT-->START-->ATGGGTGGTGGTGC-->START-->ATGGCGTTCTTAGTTGGTGGAGCGATTTGTCTGGTTAATTCCGTTAACGAACGAGACCTCAGCCGCTAACTAGCT-->START-->ATGTGGAGGTACACCTCCACGGCCAGCTTCTTAGAGGGACT-->START-->ATGGCCGTTAGGCCAAGGAAGTTTGAGGCAATAACAGGTCTGTG-->START-->ATGCCCTTAG-->START-->ATGTTCTGGGCCCACGCGCGCTACACTG-->START-->ATGTATTCAACGAGTCTATAGCCTTGGCCGACAGGCCCGGGTATCTTTGAAATTTCATCGTG-->START-->ATGGGGATAGATCATTGCAATTGTTGGTCTTCAACGAGGATTCCTAGTAAGCGCGAGTCATCAGCTCGCGTTGACTACGTCCCTGCCCTTTGTACACACGCCCGTCGCTCCTACCGATTGA-->START-->ATGGTCCGGTGAA-->START-->ATGTTCGGATCGCGGCGACGTGGGGCTTCGTCGCCGACGACGTCGCGAGAAGTCCATTGAACCTTATCATTTAGAGGAAGGAG"
```

Look for stop codons? 


```r
gsub("(TAA|TAG|TGA)","\\1>--STOP--|",Seq)
```

```
## [1] "TCATATGCTTGTCTCAAAGATTAA>--STOP--|GCCATGCATGTGTAA>--STOP--|GTATGA>--STOP--|ACAAATTCAGACTGGAAACTGCGAATGGCTCATTAA>--STOP--|ATCAGTTATAG>--STOP--|TTTGTTTGA>--STOP--|TGGTATCTGCTACTCGGTAA>--STOP--|CCGTAG>--STOP--|TAA>--STOP--|TTCTAG>--STOP--|AGCTAA>--STOP--|TACGTGCAACAAACCCCGACTTCTGGAAGGGACGCTTTATTAG>--STOP--|ATAA>--STOP--|AAGGTCGACGCGGGCTTTGCCCGATGCTCTGA>--STOP--|TGA>--STOP--|TTCATGA>--STOP--|TAA>--STOP--|CTGA>--STOP--|CGGATCGCACGGCCATCGTGCCGGCGACGCATCATTCAAATTTCTGCCCTATCAACTTCGATGGTAG>--STOP--|GATAG>--STOP--|TGGCCTACCATGGTGTTTACGGGTAA>--STOP--|CGGAGAATTAG>--STOP--|GGTTCGATCCGGAGAGGGAGCCTGA>--STOP--|GAAACGGCTACCACATCCAAGGAAGGCAGCAGGCGCGCAAATACCCAATCCTGA>--STOP--|CACGGGGAGGTAG>--STOP--|TGA>--STOP--|CAATAA>--STOP--|ATAA>--STOP--|CAATACTGGGCTCTTTGA>--STOP--|GTTGGTAA>--STOP--|TTGGAATGA>--STOP--|GTACAATCTAA>--STOP--|ATCCCTTAA>--STOP--|CGAGGATCCATTGGAGGGCAAGTCGGTGCCAGCAGCCGCGGTAA>--STOP--|TTCCAGCTCCAATAG>--STOP--|CGTATATTTAA>--STOP--|GTTGTTGCAGTTAA>--STOP--|AAGCTCGTAG>--STOP--|TTGGACCTTGGGTTGGGTCGACCGGTCCGCCTTTGGTGTGCACCGATCGCTCGTCCCTTCTACCGGCGATGCGCGCCTGGCCTTAA>--STOP--|TTGGCCGGGTCGTTCCTCCGGGCTGTTACTTTGA>--STOP--|AGAAATTAG>--STOP--|AGTGCTCAAAGCAAGCATTAG>--STOP--|CTATGA>--STOP--|ATACATTAG>--STOP--|CTGGGATAA>--STOP--|CATTATAG>--STOP--|GATTCCGATCCTATTATGTTGGCCTTCGGGATCGGAGTAA>--STOP--|TGA>--STOP--|TAA>--STOP--|CAGGGACAGTCGGGGGCATTCGTATTTCATAG>--STOP--|TCAGAGGTGA>--STOP--|AATTCTTGGATTTAG>--STOP--|AAAGACGAACAACTGCGAAAGCATTTGCCAAGGATGTTTTCATTAA>--STOP--|TCAAGAACGAAATTGGGGGCTCGAAGACGATCAGATACCGTCCTAG>--STOP--|TCTCAACCATAA>--STOP--|ACGATGCCGACCAGGATCAGCGAATGTTACTTTTAG>--STOP--|GACTTCGCTGGCACCTTATGA>--STOP--|GAAATCAAAGTTTTTGGTTCCGGGGGGAGTATGGTCGCAAGGCTGA>--STOP--|AACTTAA>--STOP--|AGGAATTGA>--STOP--|CGGAAGGGCACCCCAGGAGTGGAGCCTGCGGCTTAA>--STOP--|TTTGA>--STOP--|CTCAACACGGGGAAACTTACCAGGTCCAGAATAG>--STOP--|TAA>--STOP--|GGATTGA>--STOP--|CAGACTGA>--STOP--|GAGCTCTTTCTTGA>--STOP--|TTCTATGGGTGGTGGTGCATGGCGTTCTTAG>--STOP--|TTGGTGGAGCGATTTGTCTGGTTAA>--STOP--|TTCCGTTAA>--STOP--|CGAACGAGACCTCAGCCGCTAA>--STOP--|CTAG>--STOP--|CTATGTGGAGGTACACCTCCACGGCCAGCTTCTTAG>--STOP--|AGGGACTATGGCCGTTAG>--STOP--|GCCAAGGAAGTTTGA>--STOP--|GGCAATAA>--STOP--|CAGGTCTGTGA>--STOP--|TGCCCTTAG>--STOP--|ATGTTCTGGGCCCACGCGCGCTACACTGA>--STOP--|TGTATTCAACGAGTCTATAG>--STOP--|CCTTGGCCGACAGGCCCGGGTATCTTTGA>--STOP--|AATTTCATCGTGA>--STOP--|TGGGGATAG>--STOP--|ATCATTGCAATTGTTGGTCTTCAACGAGGATTCCTAG>--STOP--|TAA>--STOP--|GCGCGAGTCATCAGCTCGCGTTGA>--STOP--|CTACGTCCCTGCCCTTTGTACACACGCCCGTCGCTCCTACCGATTGA>--STOP--|ATGGTCCGGTGA>--STOP--|AATGTTCGGATCGCGGCGACGTGGGGCTTCGTCGCCGACGACGTCGCGAGAAGTCCATTGA>--STOP--|ACCTTATCATTTAG>--STOP--|AGGAAGGAG"
```


## More Exercises

Here are some more exercises to practice your skils. No solutions are given for these, you will have to solve them on your own. Note that they may find their way onto a test, assignment or quiz.

### 1. Consider a vector of email addresses scraped from the internet:

  * robert 'dot' colautti 'at' queensu 'dot' ca
  * chris.eckert[at]queensu.ca
  * lonnie.aarssen at queensu.ca

Use regular expressions to convert all email addresses to the standard format: name@queensu.ca

### 2. Create a random sequence of DNA:


```r
My.Seq<-sample(c("A","T","G","C"),1000,replace=T)
```

    * Replace T with U
    * Find all start codons (AUG) and stop codons (UAA, UAG, UGA)
    * Find all open reading frames (hint: consider each sequence beginning with AUG and ending with a stop codon; how do you know if both sequences are in the same reading frame?)
    * Count the length of bp for all open reading frames

### 3. More online examples

[http://regex.sketchengine.co.uk/extra_regexps.html](http://regex.sketchengine.co.uk/extra_regexps.html)

### 4. Regex Golf

Have fun! [LINK](https://alf.nu/RegexGolf)

