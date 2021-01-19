---
title: "Lab 4 Homework"
author: "Ibrahim Rizk"
date: "2021-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double()
## )
## ℹ Use `spec()` for the full column specifications.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
sapply(homerange, class)
```

```
##                      taxon                common.name 
##                "character"                "character" 
##                      class                      order 
##                "character"                "character" 
##                     family                      genus 
##                "character"                "character" 
##                    species              primarymethod 
##                "character"                "character" 
##                          N                mean.mass.g 
##                "character"                  "numeric" 
##                 log10.mass alternative.mass.reference 
##                  "numeric"                "character" 
##                mean.hra.m2                  log10.hra 
##                  "numeric"                  "numeric" 
##              hra.reference                      realm 
##                "character"                "character" 
##           thermoregulation                 locomotion 
##                "character"                "character" 
##              trophic.guild                  dimension 
##                "character"                "character" 
##                   preymass             log10.preymass 
##                  "numeric"                  "numeric" 
##                       PPMR        prey.size.reference 
##                  "numeric"                "character"
```


```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  


```r
y <- as.factor(homerange$taxon)
class(y)
```

```
## [1] "factor"
```

```r
levels(y)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```


```r
x <- as.factor(homerange$order)
class(x)
```

```
## [1] "factor"
```

```r
levels(x)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes\xa0" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- select(homerange, taxon, common.name, class, order, family, genus, species)
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
tcounts <- table(homerange$taxon)

tcounts
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```
**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  


```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivore <- data.frame(homerange$species, trophic.guild = "carnivore")

carnivore
```

```
##            homerange.species trophic.guild
## 1                   rostrata     carnivore
## 2                  poecilura     carnivore
## 3                   anomalum     carnivore
## 4                funduloides     carnivore
## 5                 cataractae     carnivore
## 6                masquinongy     carnivore
## 7                 pollachius     carnivore
## 8                     virens     carnivore
## 9                   lineatus     carnivore
## 10                 lituratus     carnivore
## 11                 unicornis     carnivore
## 12                atlanticus     carnivore
## 13                 ignobilis     carnivore
## 14                 rupestris     carnivore
## 15                  gibbosus     carnivore
## 16               macrochirus     carnivore
## 17                 megalotis     carnivore
## 18                  dolomieu     carnivore
## 19                 salmoides     carnivore
## 20                 annularis     carnivore
## 21                 baronessa     carnivore
## 22                 trichrous     carnivore
## 23              trifascialis     carnivore
## 24              trifasciatus     carnivore
## 25              unimaculatus     carnivore
## 26              spectrabilis     carnivore
## 27                     pinos     carnivore
## 28                     falco     carnivore
## 29                marmoratus     carnivore
## 30                  japonica     carnivore
## 31                     dalli     carnivore
## 32                  hipoliti     carnivore
## 33                 nicholsii     carnivore
## 34               longipinnis     carnivore
## 35                 sectatrix     carnivore
## 36                     rufus     carnivore
## 37                 undulatus     carnivore
## 38                     julis     carnivore
## 39                bivittatus     carnivore
## 40                   garnoti     carnivore
## 41               maculipinna     carnivore
## 42                     poeyi     carnivore
## 43                dimidiatus     carnivore
## 44                  bergylta     carnivore
## 45                lineolatus     carnivore
## 46                   pulcher     carnivore
## 47                 adspersus     carnivore
## 48               bifasciatum     carnivore
## 49                    lunare     carnivore
## 50                     harak     carnivore
## 51                    analis     carnivore
## 52                    apodus     carnivore
## 53                decussatus     carnivore
## 54                   griseus     carnivore
## 55                 chrysurus     carnivore
## 56                  princeps     carnivore
## 57                    labrax     carnivore
## 58             flavolineatus     carnivore
## 59                porphyreus     carnivore
## 60                flavescens     carnivore
## 61                   luridus     carnivore
## 62                      argi     carnivore
## 63                   chromis     carnivore
## 64                biocellata     carnivore
## 65                   aruanus     carnivore
## 66                     wardi     carnivore
## 67                  apicalis     carnivore
## 68                  partitus     carnivore
## 69                variabilis     carnivore
## 70               microrhinos     carnivore
## 71                     iseri     carnivore
## 72                 rivulatus     carnivore
## 73              aurofrenatum     carnivore
## 74              chrysopterum     carnivore
## 75                rubripinne     carnivore
## 76                    viride     carnivore
## 77                     argus     carnivore
## 78                 cruentata     carnivore
## 79               hemistiktos     carnivore
## 80                   miniata     carnivore
## 81                  guttatus     carnivore
## 82                marginatus     carnivore
## 83                     morio     carnivore
## 84                  striatus     carnivore
## 85                   tauvina     carnivore
## 86                    huntii     carnivore
## 87               maccullochi     carnivore
## 88                    bonaci     carnivore
## 89                clathratus     carnivore
## 90                 nebulifer     carnivore
## 91                 areolatus     carnivore
## 92                 leopardus     carnivore
## 93                  cabrilla     carnivore
## 94                    scriba     carnivore
## 95                     salpa     carnivore
## 96                   auratus     carnivore
## 97                    clarki     carnivore
## 98                     gilae     carnivore
## 99                    mykiss     carnivore
## 100                    salar     carnivore
## 101                   trutta     carnivore
## 102                   bairdi     carnivore
## 103                carolinae     carnivore
## 104                    gobio     carnivore
## 105                 caurinus     carnivore
## 106                  inermis     carnivore
## 107                  maliger     carnivore
## 108                 melanops     carnivore
## 109                 mustinus     carnivore
## 110                  natalis     carnivore
## 111               guttulatus     carnivore
## 112           lumbriciformis     carnivore
## 113                 rostrata     carnivore
## 114               chrysaetos     carnivore
## 115                    buteo     carnivore
## 116                 gallicus     carnivore
## 117                fasciatus     carnivore
## 118                 pennatus     carnivore
## 119             percnopterus     carnivore
## 120                 strepera     carnivore
## 121                australis     carnivore
## 122                europaeus     carnivore
## 123               ostralegus     carnivore
## 124                     inca     carnivore
## 125                 palumbus     carnivore
## 126                   turtur     carnivore
## 127                 garrulus     carnivore
## 128                    epops     carnivore
## 129               glandarius     carnivore
## 130                  canorus     carnivore
## 131            californianus     carnivore
## 132               radiolosus     carnivore
## 133                 cooperii     carnivore
## 134                 gentilis     carnivore
## 135                    nisus     carnivore
## 136                 striatus     carnivore
## 137              jamaicensis     carnivore
## 138                 lineatus     carnivore
## 139                swainsoni     carnivore
## 140                  cyaneus     carnivore
## 141                 pygargus     carnivore
## 142                   milvus     carnivore
## 143                 cheriway     carnivore
## 144               americanus     carnivore
## 145                biarmicus     carnivore
## 146                mexicanus     carnivore
## 147               peregrinus     carnivore
## 148               sparverius     carnivore
## 149              tinnunculus     carnivore
## 150                  bonasia     carnivore
## 151             urophasianus     carnivore
## 152                 obscurus     carnivore
## 153                  lagopus     carnivore
## 154                   perdix     carnivore
## 155                   tetrix     carnivore
## 156                urogallus     carnivore
## 157          cupido pinnatus     carnivore
## 158                    wolfi     carnivore
## 159                     crex     carnivore
## 160                  elegans     carnivore
## 161               polyglotta     carnivore
## 162                 caudatus     carnivore
## 163                  arborea     carnivore
## 164               fuscicauda     carnivore
## 165                   rubica     carnivore
## 166               familiaris     carnivore
## 167                 juncidis     carnivore
## 168                    corax     carnivore
## 169            caryocatactes     carnivore
## 170                raimondii     carnivore
## 171               savannarum     carnivore
## 172                   cyanea     carnivore
## 173                   aberti     carnivore
## 174                   fuscus     carnivore
## 175                  arborea     carnivore
## 176                passerina     carnivore
## 177                cannabina     carnivore
## 178                  coelebs     carnivore
## 179                  serinus     carnivore
## 180                    magna     carnivore
## 181                 neglecta     carnivore
## 182                   virens     carnivore
## 183                 collurio     carnivore
## 184             ludovicianus     carnivore
## 185                    minor     carnivore
## 186                  senator     carnivore
## 187              polyglottos     carnivore
## 188                     alba     carnivore
## 189                    flava     carnivore
## 190                  striata     carnivore
## 191                 oenanthe     carnivore
## 192              phoenicurus     carnivore
## 193                  rubetra     carnivore
## 194             atricapillus     carnivore
## 195             carolinensis     carnivore
## 196                inornatus     carnivore
## 197                palustris     carnivore
## 198             philadelphia     carnivore
## 199                  trichas     carnivore
## 200                   citrea     carnivore
## 201             aurocapillus     carnivore
## 202                    fusca     carnivore
## 203                kirtlandi     carnivore
## 204                 magnolia     carnivore
## 205             pensylvanica     carnivore
## 206                 petechia     carnivore
## 207                ruticilla     carnivore
## 208                   virens     carnivore
## 209               canadensis     carnivore
## 210                  bonelli     carnivore
## 211             dentirostris     carnivore
## 212             ignicapillus     carnivore
## 213                  regulus     carnivore
## 214                 europaea     carnivore
## 215                 fasciata     carnivore
## 216                    sarda     carnivore
## 217                   undata     carnivore
## 218                 bewickiI     carnivore
## 219             ludovicianus     carnivore
## 220                    aedon     carnivore
## 221              troglodytes     carnivore
## 222                   sialis     carnivore
## 223                   virens     carnivore
## 224                  minimus     carnivore
## 225                 wrightii     carnivore
## 226                 tyrannus     carnivore
## 227             atricapillus     carnivore
## 228                    belli     carnivore
## 229                  griseus     carnivore
## 230                olivaceus     carnivore
## 231                stellaris     carnivore
## 232                   exilis     carnivore
## 233                  martius     carnivore
## 234                torquilla     carnivore
## 235                 leucotos     carnivore
## 236                   medius     carnivore
## 237              tridactylus     carnivore
## 238                    canus     carnivore
## 239                  viridis     carnivore
## 240              habroptilus     carnivore
## 241                americana     carnivore
## 242                  pennata     carnivore
## 243                 funereus     carnivore
## 244                     otus     carnivore
## 245                   noctua     carnivore
## 246                     bubo     carnivore
## 247              virginianus     carnivore
## 248               passerinum     carnivore
## 249                scandiaca     carnivore
## 250                    aluco     carnivore
## 251                     alba     carnivore
## 252                  camelus     carnivore
## 253                   ornata     carnivore
## 254               trevelyani     carnivore
## 255                   granti     carnivore
## 256                americana     carnivore
## 257                 melampus     carnivore
## 258               buselaphus     carnivore
## 259                   lervia     carnivore
## 260                    bison     carnivore
## 261                  bonasus     carnivore
## 262                   hircus     carnivore
## 263                pyrenaica     carnivore
## 264               callipygus     carnivore
## 265                 dorsalis     carnivore
## 266                  gazella     carnivore
## 267                guentheri     carnivore
## 268               americanus     carnivore
## 269                    ammon     carnivore
## 270               canadensis     carnivore
## 271               campestris     carnivore
## 272                rupicapra     carnivore
## 273                     oryx     carnivore
## 274                 scriptus     carnivore
## 275             strepsiceros     carnivore
## 276                    alces     carnivore
## 277                     axis     carnivore
## 278                capreolus     carnivore
## 279                  elaphus     carnivore
## 280                   nippon     carnivore
## 281                     dama     carnivore
## 282                  reevesi     carnivore
## 283                 hemionus     carnivore
## 284              virginianus     carnivore
## 285              bezoarticus     carnivore
## 286                     puda     carnivore
## 287                 tarandus     carnivore
## 288           camelopardalis     carnivore
## 289                johnstoni     carnivore
## 290              aethiopicus     carnivore
## 291                  wagneri     carnivore
## 292                   tajacu     carnivore
## 293                   pecari     carnivore
## 294                aquaticus     carnivore
## 295                  fulgens     carnivore
## 296                  lagopus     carnivore
## 297                 simensis     carnivore
## 298                 culpaeus     carnivore
## 299                  griseus     carnivore
## 300                  macroti     carnivore
## 301                 ruppelli     carnivore
## 302                    velox     carnivore
## 303                    telum     carnivore
## 304                    ferox     carnivore
## 305                  jubatus     carnivore
## 306                  caracal     carnivore
## 307                    catus     carnivore
## 308               sylvestris     carnivore
## 309             yagouaroundi     carnivore
## 310                 pardalis     carnivore
## 311                   wiedii     carnivore
## 312                   serval     carnivore
## 313               canadensis     carnivore
## 314                     lynx     carnivore
## 315                 pardinus     carnivore
## 316                    rufus     carnivore
## 317                geoffroyi     carnivore
## 318                     onca     carnivore
## 319                   pardus     carnivore
## 320                   tigris     carnivore
## 321              bengalensis     carnivore
## 322                 concolor     carnivore
## 323                    uncia     carnivore
## 324              paludinosus     carnivore
## 325              penicillata     carnivore
## 326                  parvula     carnivore
## 327               inchneumon     carnivore
## 328                albicauda     carnivore
## 329                cristatus     carnivore
## 330                  barbata     carnivore
## 331                  vittata     carnivore
## 332                     gulo     carnivore
## 333                americana     carnivore
## 334                    foina     carnivore
## 335                   martes     carnivore
## 336                 pennanti     carnivore
## 337                  erminea     carnivore
## 338                  frenata     carnivore
## 339                     furo     carnivore
## 340                 lutreola     carnivore
## 341                 nigripes     carnivore
## 342                  nivalis     carnivore
## 343                 sibirica     carnivore
## 344                    taxus     carnivore
## 345                   flavus     carnivore
## 346              melanoleuca     carnivore
## 347                  ursinus     carnivore
## 348                  genetta     carnivore
## 349                  tigrina     carnivore
## 350                  zibetha     carnivore
## 351                geoffroii     carnivore
## 352                maculatus     carnivore
## 353                 leucopus     carnivore
## 354                 stuartii     carnivore
## 355                americana     carnivore
## 356                  elegans     carnivore
## 357                lumholtzi     carnivore
## 358              antilopinus     carnivore
## 359                 dorsalis     carnivore
## 360              fuliginosus     carnivore
## 361                giganteus     carnivore
## 362                 robustus     carnivore
## 363              rufogriseus     carnivore
## 364                    rufus     carnivore
## 365                assimilis     carnivore
## 366                 gaimardi     carnivore
## 367                 longipes     carnivore
## 368                   volans     carnivore
## 369                 fraenata     carnivore
## 370               stigmatica     carnivore
## 371                   thetis     carnivore
## 372                  bicolor     carnivore
## 373                vulpecula     carnivore
## 374                 krefftii     carnivore
## 375                  ursinus     carnivore
## 376                europaeus     carnivore
## 377                  auritus     carnivore
## 378               idahoensis     carnivore
## 379               americanus     carnivore
## 380                 arcticus     carnivore
## 381             californicus     carnivore
## 382                 capensis     carnivore
## 383                europaeus     carnivore
## 384              nigricollis     carnivore
## 385                  timidus     carnivore
## 386                cuniculus     carnivore
## 387                aquaticus     carnivore
## 388               floridanus     carnivore
## 389                palustris     carnivore
## 390                curzoniae     carnivore
## 391                 princeps     carnivore
## 392                rufescens     carnivore
## 393            tetradactylus     carnivore
## 394              chrysopygus     carnivore
## 395                splendens     carnivore
## 396                  auratus     carnivore
## 397                 obesulus     carnivore
## 398                 caballus     carnivore
## 399                    simum     carnivore
## 400                 bicornis     carnivore
## 401                torquatus     carnivore
## 402                  maximus     carnivore
## 403                 africana     carnivore
## 404                 torridus     carnivore
## 405                     rufa     carnivore
## 406                  suillus     carnivore
## 407               damarensis     carnivore
## 408              hottentotus     carnivore
## 409                 capensis     carnivore
## 410         argenteocinereus     carnivore
## 411                   glaber     carnivore
## 412                patagonus     carnivore
## 413                  maximus     carnivore
## 414             californicus     carnivore
## 415             megacephalus     carnivore
## 416                sibiricus     carnivore
## 417                 agrestis     carnivore
## 418             californicus     carnivore
## 419                 montanus     carnivore
## 420              ochrogaster     carnivore
## 421           pennsylvanicus     carnivore
## 422                pinetorum     carnivore
## 423              richardsoni     carnivore
## 424             schisticolor     carnivore
## 425                  cinerea     carnivore
## 426                 fuscipes     carnivore
## 427                   lepida     carnivore
## 428                 micropus     carnivore
## 429                zibethica     carnivore
## 430               gossypinus     carnivore
## 431              raviventris     carnivore
## 432                  cooperi     carnivore
## 433                  pumilio     carnivore
## 434                  cuvieri     carnivore
## 435             semispinosus     carnivore
## 436              prehensilis     carnivore
## 437                 dorsatum     carnivore
## 438                   bottae     carnivore
## 439                 ocularis     carnivore
## 440             avellanarius     carnivore
## 441                   ingens     carnivore
## 442                 merriami     carnivore
## 443                    ordii     carnivore
## 444              spectabilis     carnivore
## 445                stephensi     carnivore
## 446         africaeaustralis     carnivore
## 447                   indica     carnivore
## 448                africanus     carnivore
## 449              flavicollis     carnivore
## 450               sylvaticus     carnivore
## 451                hurrianae     carnivore
## 452                   fuscus     carnivore
## 453                 antimena     carnivore
## 454                audeberti     carnivore
## 455                    rufus     carnivore
## 456                   cyanus     carnivore
## 457                sibiricus     carnivore
## 458                 pennanti     carnivore
## 459                 sabrinus     carnivore
## 460                   volans     carnivore
## 461             flaviventris     carnivore
## 462                    monax     carnivore
## 463                palliatus     carnivore
## 464                   aberti     carnivore
## 465             carolinensis     carnivore
## 466                      lis     carnivore
## 467                    niger     carnivore
## 468                 vulgaris     carnivore
## 469                 beecheyi     carnivore
## 470              columbianus     carnivore
## 471               franklinii     carnivore
## 472                  parryii     carnivore
## 473                spilosoma     carnivore
## 474         tridecemlineatus     carnivore
## 475               variegatus     carnivore
## 476                  amoenus     carnivore
## 477                  minimus     carnivore
## 478           quadrivittatus     carnivore
## 479                 umbrinus     carnivore
## 480               hudsonicus     carnivore
## 481               erythropus     carnivore
## 482                  russula     carnivore
## 483                 arcticus     carnivore
## 484                 cinereus     carnivore
## 485                coronatus     carnivore
## 486              gracillimus     carnivore
## 487             unguiculatus     carnivore
## 488                 cristata     carnivore
## 489                aquaticus     carnivore
## 490                 europaea     carnivore
## 491                   romana     carnivore
## 492                aegyptius     carnivore
## 493                   vermis     carnivore
## 494                  viridis     carnivore
## 495              constrictor     carnivore
## 496 constrictor flaviventris     carnivore
## 497                punctatus     carnivore
## 498                  couperi     carnivore
## 499           guttata emoryi     carnivore
## 500                 obsoleta     carnivore
## 501              platirhinos     carnivore
## 502             viridiflavus     carnivore
## 503            getula getula     carnivore
## 504               triangulum     carnivore
## 505                flagellum     carnivore
## 506                   natrix     carnivore
## 507            erythrogaster     carnivore
## 508                 sipeodon     carnivore
## 509             rufodorsatus     carnivore
## 510                catenifer     carnivore
## 511             melanoleucus     carnivore
## 512                  butleri     carnivore
## 513                    gigal     carnivore
## 514              longissimus     carnivore
## 515              bungaroides     carnivore
## 516                 scutatus     carnivore
## 517             porphyriacus     carnivore
## 518                 pallidus     carnivore
## 519                  cyclura     carnivore
## 520                   lewisi     carnivore
## 521                  pinguis     carnivore
## 522                 hispidua     carnivore
## 523                   obesus     carnivore
## 524                 dorsalis     carnivore
## 525                  galloti     carnivore
## 526              flagellifer     carnivore
## 527        spilota imbricata     carnivore
## 528                    major     carnivore
## 529               contortrix     carnivore
## 530              piscivorous     carnivore
## 531               schneideri     carnivore
## 532                    asper     carnivore
## 533                    atrox     carnivore
## 534                 cerastes     carnivore
## 535                 horridus     carnivore
## 536                 molossus     carnivore
## 537        oreganus concolor     carnivore
## 538                   pricei     carnivore
## 539               scutulatus     carnivore
## 540                   tigris     carnivore
## 541              shedaoensis     carnivore
## 542                   raddei     carnivore
## 543                 latastei     carnivore
## 544              longicollis     carnivore
## 545                    dahli     carnivore
## 546               serpentina     carnivore
## 547          picta marginata     carnivore
## 548              reticularia     carnivore
## 549               blandingii     carnivore
## 550              orbicularis     carnivore
## 551            flavimaculata     carnivore
## 552                   ornata     carnivore
## 553                  leprosa     carnivore
## 554                rubrubrum     carnivore
## 555           minor peltifer     carnivore
## 556                 odoratus     carnivore
## 557               carbonaria     carnivore
## 558                agassizii     carnivore
## 559               polyphemus     carnivore
## 560             travancorica     carnivore
## 561                   spekii     carnivore
## 562                 impressa     carnivore
## 563                tentorius     carnivore
## 564                 pardalis     carnivore
## 565                   graeca     carnivore
## 566                 hermanii     carnivore
## 567               horsfieldi     carnivore
## 568               kleinmanni     carnivore
## 569                spinifera     carnivore
```


```r
herbivore <- data.frame(homerange$species, trophic.guild = "herbivore")

herbivore
```

```
##            homerange.species trophic.guild
## 1                   rostrata     herbivore
## 2                  poecilura     herbivore
## 3                   anomalum     herbivore
## 4                funduloides     herbivore
## 5                 cataractae     herbivore
## 6                masquinongy     herbivore
## 7                 pollachius     herbivore
## 8                     virens     herbivore
## 9                   lineatus     herbivore
## 10                 lituratus     herbivore
## 11                 unicornis     herbivore
## 12                atlanticus     herbivore
## 13                 ignobilis     herbivore
## 14                 rupestris     herbivore
## 15                  gibbosus     herbivore
## 16               macrochirus     herbivore
## 17                 megalotis     herbivore
## 18                  dolomieu     herbivore
## 19                 salmoides     herbivore
## 20                 annularis     herbivore
## 21                 baronessa     herbivore
## 22                 trichrous     herbivore
## 23              trifascialis     herbivore
## 24              trifasciatus     herbivore
## 25              unimaculatus     herbivore
## 26              spectrabilis     herbivore
## 27                     pinos     herbivore
## 28                     falco     herbivore
## 29                marmoratus     herbivore
## 30                  japonica     herbivore
## 31                     dalli     herbivore
## 32                  hipoliti     herbivore
## 33                 nicholsii     herbivore
## 34               longipinnis     herbivore
## 35                 sectatrix     herbivore
## 36                     rufus     herbivore
## 37                 undulatus     herbivore
## 38                     julis     herbivore
## 39                bivittatus     herbivore
## 40                   garnoti     herbivore
## 41               maculipinna     herbivore
## 42                     poeyi     herbivore
## 43                dimidiatus     herbivore
## 44                  bergylta     herbivore
## 45                lineolatus     herbivore
## 46                   pulcher     herbivore
## 47                 adspersus     herbivore
## 48               bifasciatum     herbivore
## 49                    lunare     herbivore
## 50                     harak     herbivore
## 51                    analis     herbivore
## 52                    apodus     herbivore
## 53                decussatus     herbivore
## 54                   griseus     herbivore
## 55                 chrysurus     herbivore
## 56                  princeps     herbivore
## 57                    labrax     herbivore
## 58             flavolineatus     herbivore
## 59                porphyreus     herbivore
## 60                flavescens     herbivore
## 61                   luridus     herbivore
## 62                      argi     herbivore
## 63                   chromis     herbivore
## 64                biocellata     herbivore
## 65                   aruanus     herbivore
## 66                     wardi     herbivore
## 67                  apicalis     herbivore
## 68                  partitus     herbivore
## 69                variabilis     herbivore
## 70               microrhinos     herbivore
## 71                     iseri     herbivore
## 72                 rivulatus     herbivore
## 73              aurofrenatum     herbivore
## 74              chrysopterum     herbivore
## 75                rubripinne     herbivore
## 76                    viride     herbivore
## 77                     argus     herbivore
## 78                 cruentata     herbivore
## 79               hemistiktos     herbivore
## 80                   miniata     herbivore
## 81                  guttatus     herbivore
## 82                marginatus     herbivore
## 83                     morio     herbivore
## 84                  striatus     herbivore
## 85                   tauvina     herbivore
## 86                    huntii     herbivore
## 87               maccullochi     herbivore
## 88                    bonaci     herbivore
## 89                clathratus     herbivore
## 90                 nebulifer     herbivore
## 91                 areolatus     herbivore
## 92                 leopardus     herbivore
## 93                  cabrilla     herbivore
## 94                    scriba     herbivore
## 95                     salpa     herbivore
## 96                   auratus     herbivore
## 97                    clarki     herbivore
## 98                     gilae     herbivore
## 99                    mykiss     herbivore
## 100                    salar     herbivore
## 101                   trutta     herbivore
## 102                   bairdi     herbivore
## 103                carolinae     herbivore
## 104                    gobio     herbivore
## 105                 caurinus     herbivore
## 106                  inermis     herbivore
## 107                  maliger     herbivore
## 108                 melanops     herbivore
## 109                 mustinus     herbivore
## 110                  natalis     herbivore
## 111               guttulatus     herbivore
## 112           lumbriciformis     herbivore
## 113                 rostrata     herbivore
## 114               chrysaetos     herbivore
## 115                    buteo     herbivore
## 116                 gallicus     herbivore
## 117                fasciatus     herbivore
## 118                 pennatus     herbivore
## 119             percnopterus     herbivore
## 120                 strepera     herbivore
## 121                australis     herbivore
## 122                europaeus     herbivore
## 123               ostralegus     herbivore
## 124                     inca     herbivore
## 125                 palumbus     herbivore
## 126                   turtur     herbivore
## 127                 garrulus     herbivore
## 128                    epops     herbivore
## 129               glandarius     herbivore
## 130                  canorus     herbivore
## 131            californianus     herbivore
## 132               radiolosus     herbivore
## 133                 cooperii     herbivore
## 134                 gentilis     herbivore
## 135                    nisus     herbivore
## 136                 striatus     herbivore
## 137              jamaicensis     herbivore
## 138                 lineatus     herbivore
## 139                swainsoni     herbivore
## 140                  cyaneus     herbivore
## 141                 pygargus     herbivore
## 142                   milvus     herbivore
## 143                 cheriway     herbivore
## 144               americanus     herbivore
## 145                biarmicus     herbivore
## 146                mexicanus     herbivore
## 147               peregrinus     herbivore
## 148               sparverius     herbivore
## 149              tinnunculus     herbivore
## 150                  bonasia     herbivore
## 151             urophasianus     herbivore
## 152                 obscurus     herbivore
## 153                  lagopus     herbivore
## 154                   perdix     herbivore
## 155                   tetrix     herbivore
## 156                urogallus     herbivore
## 157          cupido pinnatus     herbivore
## 158                    wolfi     herbivore
## 159                     crex     herbivore
## 160                  elegans     herbivore
## 161               polyglotta     herbivore
## 162                 caudatus     herbivore
## 163                  arborea     herbivore
## 164               fuscicauda     herbivore
## 165                   rubica     herbivore
## 166               familiaris     herbivore
## 167                 juncidis     herbivore
## 168                    corax     herbivore
## 169            caryocatactes     herbivore
## 170                raimondii     herbivore
## 171               savannarum     herbivore
## 172                   cyanea     herbivore
## 173                   aberti     herbivore
## 174                   fuscus     herbivore
## 175                  arborea     herbivore
## 176                passerina     herbivore
## 177                cannabina     herbivore
## 178                  coelebs     herbivore
## 179                  serinus     herbivore
## 180                    magna     herbivore
## 181                 neglecta     herbivore
## 182                   virens     herbivore
## 183                 collurio     herbivore
## 184             ludovicianus     herbivore
## 185                    minor     herbivore
## 186                  senator     herbivore
## 187              polyglottos     herbivore
## 188                     alba     herbivore
## 189                    flava     herbivore
## 190                  striata     herbivore
## 191                 oenanthe     herbivore
## 192              phoenicurus     herbivore
## 193                  rubetra     herbivore
## 194             atricapillus     herbivore
## 195             carolinensis     herbivore
## 196                inornatus     herbivore
## 197                palustris     herbivore
## 198             philadelphia     herbivore
## 199                  trichas     herbivore
## 200                   citrea     herbivore
## 201             aurocapillus     herbivore
## 202                    fusca     herbivore
## 203                kirtlandi     herbivore
## 204                 magnolia     herbivore
## 205             pensylvanica     herbivore
## 206                 petechia     herbivore
## 207                ruticilla     herbivore
## 208                   virens     herbivore
## 209               canadensis     herbivore
## 210                  bonelli     herbivore
## 211             dentirostris     herbivore
## 212             ignicapillus     herbivore
## 213                  regulus     herbivore
## 214                 europaea     herbivore
## 215                 fasciata     herbivore
## 216                    sarda     herbivore
## 217                   undata     herbivore
## 218                 bewickiI     herbivore
## 219             ludovicianus     herbivore
## 220                    aedon     herbivore
## 221              troglodytes     herbivore
## 222                   sialis     herbivore
## 223                   virens     herbivore
## 224                  minimus     herbivore
## 225                 wrightii     herbivore
## 226                 tyrannus     herbivore
## 227             atricapillus     herbivore
## 228                    belli     herbivore
## 229                  griseus     herbivore
## 230                olivaceus     herbivore
## 231                stellaris     herbivore
## 232                   exilis     herbivore
## 233                  martius     herbivore
## 234                torquilla     herbivore
## 235                 leucotos     herbivore
## 236                   medius     herbivore
## 237              tridactylus     herbivore
## 238                    canus     herbivore
## 239                  viridis     herbivore
## 240              habroptilus     herbivore
## 241                americana     herbivore
## 242                  pennata     herbivore
## 243                 funereus     herbivore
## 244                     otus     herbivore
## 245                   noctua     herbivore
## 246                     bubo     herbivore
## 247              virginianus     herbivore
## 248               passerinum     herbivore
## 249                scandiaca     herbivore
## 250                    aluco     herbivore
## 251                     alba     herbivore
## 252                  camelus     herbivore
## 253                   ornata     herbivore
## 254               trevelyani     herbivore
## 255                   granti     herbivore
## 256                americana     herbivore
## 257                 melampus     herbivore
## 258               buselaphus     herbivore
## 259                   lervia     herbivore
## 260                    bison     herbivore
## 261                  bonasus     herbivore
## 262                   hircus     herbivore
## 263                pyrenaica     herbivore
## 264               callipygus     herbivore
## 265                 dorsalis     herbivore
## 266                  gazella     herbivore
## 267                guentheri     herbivore
## 268               americanus     herbivore
## 269                    ammon     herbivore
## 270               canadensis     herbivore
## 271               campestris     herbivore
## 272                rupicapra     herbivore
## 273                     oryx     herbivore
## 274                 scriptus     herbivore
## 275             strepsiceros     herbivore
## 276                    alces     herbivore
## 277                     axis     herbivore
## 278                capreolus     herbivore
## 279                  elaphus     herbivore
## 280                   nippon     herbivore
## 281                     dama     herbivore
## 282                  reevesi     herbivore
## 283                 hemionus     herbivore
## 284              virginianus     herbivore
## 285              bezoarticus     herbivore
## 286                     puda     herbivore
## 287                 tarandus     herbivore
## 288           camelopardalis     herbivore
## 289                johnstoni     herbivore
## 290              aethiopicus     herbivore
## 291                  wagneri     herbivore
## 292                   tajacu     herbivore
## 293                   pecari     herbivore
## 294                aquaticus     herbivore
## 295                  fulgens     herbivore
## 296                  lagopus     herbivore
## 297                 simensis     herbivore
## 298                 culpaeus     herbivore
## 299                  griseus     herbivore
## 300                  macroti     herbivore
## 301                 ruppelli     herbivore
## 302                    velox     herbivore
## 303                    telum     herbivore
## 304                    ferox     herbivore
## 305                  jubatus     herbivore
## 306                  caracal     herbivore
## 307                    catus     herbivore
## 308               sylvestris     herbivore
## 309             yagouaroundi     herbivore
## 310                 pardalis     herbivore
## 311                   wiedii     herbivore
## 312                   serval     herbivore
## 313               canadensis     herbivore
## 314                     lynx     herbivore
## 315                 pardinus     herbivore
## 316                    rufus     herbivore
## 317                geoffroyi     herbivore
## 318                     onca     herbivore
## 319                   pardus     herbivore
## 320                   tigris     herbivore
## 321              bengalensis     herbivore
## 322                 concolor     herbivore
## 323                    uncia     herbivore
## 324              paludinosus     herbivore
## 325              penicillata     herbivore
## 326                  parvula     herbivore
## 327               inchneumon     herbivore
## 328                albicauda     herbivore
## 329                cristatus     herbivore
## 330                  barbata     herbivore
## 331                  vittata     herbivore
## 332                     gulo     herbivore
## 333                americana     herbivore
## 334                    foina     herbivore
## 335                   martes     herbivore
## 336                 pennanti     herbivore
## 337                  erminea     herbivore
## 338                  frenata     herbivore
## 339                     furo     herbivore
## 340                 lutreola     herbivore
## 341                 nigripes     herbivore
## 342                  nivalis     herbivore
## 343                 sibirica     herbivore
## 344                    taxus     herbivore
## 345                   flavus     herbivore
## 346              melanoleuca     herbivore
## 347                  ursinus     herbivore
## 348                  genetta     herbivore
## 349                  tigrina     herbivore
## 350                  zibetha     herbivore
## 351                geoffroii     herbivore
## 352                maculatus     herbivore
## 353                 leucopus     herbivore
## 354                 stuartii     herbivore
## 355                americana     herbivore
## 356                  elegans     herbivore
## 357                lumholtzi     herbivore
## 358              antilopinus     herbivore
## 359                 dorsalis     herbivore
## 360              fuliginosus     herbivore
## 361                giganteus     herbivore
## 362                 robustus     herbivore
## 363              rufogriseus     herbivore
## 364                    rufus     herbivore
## 365                assimilis     herbivore
## 366                 gaimardi     herbivore
## 367                 longipes     herbivore
## 368                   volans     herbivore
## 369                 fraenata     herbivore
## 370               stigmatica     herbivore
## 371                   thetis     herbivore
## 372                  bicolor     herbivore
## 373                vulpecula     herbivore
## 374                 krefftii     herbivore
## 375                  ursinus     herbivore
## 376                europaeus     herbivore
## 377                  auritus     herbivore
## 378               idahoensis     herbivore
## 379               americanus     herbivore
## 380                 arcticus     herbivore
## 381             californicus     herbivore
## 382                 capensis     herbivore
## 383                europaeus     herbivore
## 384              nigricollis     herbivore
## 385                  timidus     herbivore
## 386                cuniculus     herbivore
## 387                aquaticus     herbivore
## 388               floridanus     herbivore
## 389                palustris     herbivore
## 390                curzoniae     herbivore
## 391                 princeps     herbivore
## 392                rufescens     herbivore
## 393            tetradactylus     herbivore
## 394              chrysopygus     herbivore
## 395                splendens     herbivore
## 396                  auratus     herbivore
## 397                 obesulus     herbivore
## 398                 caballus     herbivore
## 399                    simum     herbivore
## 400                 bicornis     herbivore
## 401                torquatus     herbivore
## 402                  maximus     herbivore
## 403                 africana     herbivore
## 404                 torridus     herbivore
## 405                     rufa     herbivore
## 406                  suillus     herbivore
## 407               damarensis     herbivore
## 408              hottentotus     herbivore
## 409                 capensis     herbivore
## 410         argenteocinereus     herbivore
## 411                   glaber     herbivore
## 412                patagonus     herbivore
## 413                  maximus     herbivore
## 414             californicus     herbivore
## 415             megacephalus     herbivore
## 416                sibiricus     herbivore
## 417                 agrestis     herbivore
## 418             californicus     herbivore
## 419                 montanus     herbivore
## 420              ochrogaster     herbivore
## 421           pennsylvanicus     herbivore
## 422                pinetorum     herbivore
## 423              richardsoni     herbivore
## 424             schisticolor     herbivore
## 425                  cinerea     herbivore
## 426                 fuscipes     herbivore
## 427                   lepida     herbivore
## 428                 micropus     herbivore
## 429                zibethica     herbivore
## 430               gossypinus     herbivore
## 431              raviventris     herbivore
## 432                  cooperi     herbivore
## 433                  pumilio     herbivore
## 434                  cuvieri     herbivore
## 435             semispinosus     herbivore
## 436              prehensilis     herbivore
## 437                 dorsatum     herbivore
## 438                   bottae     herbivore
## 439                 ocularis     herbivore
## 440             avellanarius     herbivore
## 441                   ingens     herbivore
## 442                 merriami     herbivore
## 443                    ordii     herbivore
## 444              spectabilis     herbivore
## 445                stephensi     herbivore
## 446         africaeaustralis     herbivore
## 447                   indica     herbivore
## 448                africanus     herbivore
## 449              flavicollis     herbivore
## 450               sylvaticus     herbivore
## 451                hurrianae     herbivore
## 452                   fuscus     herbivore
## 453                 antimena     herbivore
## 454                audeberti     herbivore
## 455                    rufus     herbivore
## 456                   cyanus     herbivore
## 457                sibiricus     herbivore
## 458                 pennanti     herbivore
## 459                 sabrinus     herbivore
## 460                   volans     herbivore
## 461             flaviventris     herbivore
## 462                    monax     herbivore
## 463                palliatus     herbivore
## 464                   aberti     herbivore
## 465             carolinensis     herbivore
## 466                      lis     herbivore
## 467                    niger     herbivore
## 468                 vulgaris     herbivore
## 469                 beecheyi     herbivore
## 470              columbianus     herbivore
## 471               franklinii     herbivore
## 472                  parryii     herbivore
## 473                spilosoma     herbivore
## 474         tridecemlineatus     herbivore
## 475               variegatus     herbivore
## 476                  amoenus     herbivore
## 477                  minimus     herbivore
## 478           quadrivittatus     herbivore
## 479                 umbrinus     herbivore
## 480               hudsonicus     herbivore
## 481               erythropus     herbivore
## 482                  russula     herbivore
## 483                 arcticus     herbivore
## 484                 cinereus     herbivore
## 485                coronatus     herbivore
## 486              gracillimus     herbivore
## 487             unguiculatus     herbivore
## 488                 cristata     herbivore
## 489                aquaticus     herbivore
## 490                 europaea     herbivore
## 491                   romana     herbivore
## 492                aegyptius     herbivore
## 493                   vermis     herbivore
## 494                  viridis     herbivore
## 495              constrictor     herbivore
## 496 constrictor flaviventris     herbivore
## 497                punctatus     herbivore
## 498                  couperi     herbivore
## 499           guttata emoryi     herbivore
## 500                 obsoleta     herbivore
## 501              platirhinos     herbivore
## 502             viridiflavus     herbivore
## 503            getula getula     herbivore
## 504               triangulum     herbivore
## 505                flagellum     herbivore
## 506                   natrix     herbivore
## 507            erythrogaster     herbivore
## 508                 sipeodon     herbivore
## 509             rufodorsatus     herbivore
## 510                catenifer     herbivore
## 511             melanoleucus     herbivore
## 512                  butleri     herbivore
## 513                    gigal     herbivore
## 514              longissimus     herbivore
## 515              bungaroides     herbivore
## 516                 scutatus     herbivore
## 517             porphyriacus     herbivore
## 518                 pallidus     herbivore
## 519                  cyclura     herbivore
## 520                   lewisi     herbivore
## 521                  pinguis     herbivore
## 522                 hispidua     herbivore
## 523                   obesus     herbivore
## 524                 dorsalis     herbivore
## 525                  galloti     herbivore
## 526              flagellifer     herbivore
## 527        spilota imbricata     herbivore
## 528                    major     herbivore
## 529               contortrix     herbivore
## 530              piscivorous     herbivore
## 531               schneideri     herbivore
## 532                    asper     herbivore
## 533                    atrox     herbivore
## 534                 cerastes     herbivore
## 535                 horridus     herbivore
## 536                 molossus     herbivore
## 537        oreganus concolor     herbivore
## 538                   pricei     herbivore
## 539               scutulatus     herbivore
## 540                   tigris     herbivore
## 541              shedaoensis     herbivore
## 542                   raddei     herbivore
## 543                 latastei     herbivore
## 544              longicollis     herbivore
## 545                    dahli     herbivore
## 546               serpentina     herbivore
## 547          picta marginata     herbivore
## 548              reticularia     herbivore
## 549               blandingii     herbivore
## 550              orbicularis     herbivore
## 551            flavimaculata     herbivore
## 552                   ornata     herbivore
## 553                  leprosa     herbivore
## 554                rubrubrum     herbivore
## 555           minor peltifer     herbivore
## 556                 odoratus     herbivore
## 557               carbonaria     herbivore
## 558                agassizii     herbivore
## 559               polyphemus     herbivore
## 560             travancorica     herbivore
## 561                   spekii     herbivore
## 562                 impressa     herbivore
## 563                tentorius     herbivore
## 564                 pardalis     herbivore
## 565                   graeca     herbivore
## 566                 hermanii     herbivore
## 567               horsfieldi     herbivore
## 568               kleinmanni     herbivore
## 569                spinifera     herbivore
```


**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
summary(homerange, trophic.guild = "carnivore")
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

```r
summary(homerange, trophic.guild = "herbivore")
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```
**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
deer <- data.frame(family = "cervidae", homerange$mean.mass.g, homerange$log10.mass, homerange$family, homerange$genus, homerange$species)

deer
```

```
##       family homerange.mean.mass.g homerange.log10.mass  homerange.family
## 1   cervidae                887.00           2.94792362       anguillidae
## 2   cervidae                562.00           2.74973632      catostomidae
## 3   cervidae                 34.00           1.53147892        cyprinidae
## 4   cervidae                  4.00           0.60205999        cyprinidae
## 5   cervidae                  4.00           0.60205999        cyprinidae
## 6   cervidae               3525.00           3.54715912          esocidae
## 7   cervidae                737.36           2.86767957           gadidae
## 8   cervidae                448.61           2.65186895           gadidae
## 9   cervidae                109.04           2.03758584      acanthuridae
## 10  cervidae                772.16           2.88770730      acanthuridae
## 11  cervidae                151.84           2.18138619      acanthuridae
## 12  cervidae                  6.20           0.79239169         blennidae
## 13  cervidae                726.18           2.86104428        carangidae
## 14  cervidae                196.00           2.29225607     centrarchidae
## 15  cervidae                 82.00           1.91381385     centrarchidae
## 16  cervidae                 79.00           1.89762709     centrarchidae
## 17  cervidae                 16.00           1.20411998     centrarchidae
## 18  cervidae               1134.00           3.05461305     centrarchidae
## 19  cervidae               2125.00           3.32735893     centrarchidae
## 20  cervidae                423.00           2.62634037     centrarchidae
## 21  cervidae                 27.63           1.44138089    chaetodontidae
## 22  cervidae                 31.88           1.50351831    chaetodontidae
## 23  cervidae                 60.54           1.78204242    chaetodontidae
## 24  cervidae                 70.45           1.84788100    chaetodontidae
## 25  cervidae                 78.12           1.89276223    chaetodontidae
## 26  cervidae               1551.13           3.19064820  cheilodactylidae
## 27  cervidae                  0.45          -0.34678749       cirrhitidae
## 28  cervidae                 10.45           1.01911629       cirrhitidae
## 29  cervidae               1450.23           3.16143689          cottidae
## 30  cervidae                  2.70           0.43136376          gobiidae
## 31  cervidae                  0.31          -0.50863831          gobiidae
## 32  cervidae                  0.22          -0.65757732          gobiidae
## 33  cervidae                  4.00           0.60205999          gobiidae
## 34  cervidae                 71.23           1.85266294          gobiidae
## 35  cervidae               1086.71           3.03611366        kyphosidae
## 36  cervidae                133.38           2.12509071          labridae
## 37  cervidae               1644.90           3.21613950          labridae
## 38  cervidae                119.16           2.07613050          labridae
## 39  cervidae                  7.17           0.85551916          labridae
## 40  cervidae                  6.57           0.81756537          labridae
## 41  cervidae                  9.55           0.98000337          labridae
## 42  cervidae                  7.03           0.84695533          labridae
## 43  cervidae                  3.49           0.54282543          labridae
## 44  cervidae                362.30           2.55906833          labridae
## 45  cervidae                 95.54           1.98018524          labridae
## 46  cervidae               1484.14           3.17147487          labridae
## 47  cervidae                103.47           2.01481445          labridae
## 48  cervidae                  3.12           0.49415459          labridae
## 49  cervidae                 84.06           1.92458939          labridae
## 50  cervidae                236.76           2.37430833       lethrinidae
## 51  cervidae               2167.70           3.33599918        lutjanidae
## 52  cervidae                 56.04           1.74849813        lutjanidae
## 53  cervidae                 50.00           1.69897000        lutjanidae
## 54  cervidae                 56.27           1.75027691        lutjanidae
## 55  cervidae               1176.86           3.07072480        lutjanidae
## 56  cervidae               1940.27           3.28786217     malacanthidae
## 57  cervidae                 85.99           1.93444795         moronidae
## 58  cervidae                297.72           2.47380801          mullidae
## 59  cervidae                191.55           2.28228216          mullidae
## 60  cervidae                134.00           2.12710480          percidae
## 61  cervidae                 94.58           1.97579931     pomacanthidae
## 62  cervidae                  2.50           0.39794001     pomacanthidae
## 63  cervidae                 28.41           1.45347123     pomacentridae
## 64  cervidae                  9.19           0.96331551     pomacentridae
## 65  cervidae                  3.96           0.59769519     pomacentridae
## 66  cervidae                 10.49           1.02077549     pomacentridae
## 67  cervidae                 45.30           1.65609820     pomacentridae
## 68  cervidae                  6.64           0.82216808     pomacentridae
## 69  cervidae                 39.00           1.59106461     pomacentridae
## 70  cervidae               1668.11           3.22222469          scaridae
## 71  cervidae                171.42           2.23406149          scaridae
## 72  cervidae                289.40           2.46149853          scaridae
## 73  cervidae                250.64           2.39905038          scaridae
## 74  cervidae                388.84           2.58977093          scaridae
## 75  cervidae                523.80           2.71916549          scaridae
## 76  cervidae                521.16           2.71697108          scaridae
## 77  cervidae                697.00           2.84323278        serranidae
## 78  cervidae                436.24           2.63972549        serranidae
## 79  cervidae                119.00           2.07554696        serranidae
## 80  cervidae                476.00           2.67760695        serranidae
## 81  cervidae                312.30           2.49457198        serranidae
## 82  cervidae                398.51           2.60043922        serranidae
## 83  cervidae               2181.15           3.33868553        serranidae
## 84  cervidae               2362.06           3.37329093        serranidae
## 85  cervidae               3081.08           3.48870297        serranidae
## 86  cervidae                119.00           2.07554696        serranidae
## 87  cervidae                 24.87           1.39567578        serranidae
## 88  cervidae               3247.34           3.51152776        serranidae
## 89  cervidae                378.42           2.57797408        serranidae
## 90  cervidae               1527.64           3.18402102        serranidae
## 91  cervidae               1992.23           3.29933948        serranidae
## 92  cervidae               1753.40           3.24388100        serranidae
## 93  cervidae                 41.11           1.61394748        serranidae
## 94  cervidae                 75.93           1.88041340        serranidae
## 95  cervidae                731.58           2.86426182          sparidae
## 96  cervidae                934.20           2.97043986          sparidae
## 97  cervidae                 99.20           1.99651167        salmonidae
## 98  cervidae                 47.00           1.67209786        salmonidae
## 99  cervidae                109.00           2.03742650        salmonidae
## 100 cervidae                  2.00           0.30103000        salmonidae
## 101 cervidae                402.00           2.60422605        salmonidae
## 102 cervidae                  5.00           0.69897000          cottidae
## 103 cervidae                  3.00           0.47712126          cottidae
## 104 cervidae                  5.00           0.69897000          cottidae
## 105 cervidae               1100.33           3.04152295        sebastidae
## 106 cervidae                 99.25           1.99673052        sebastidae
## 107 cervidae               1341.25           3.12750973        sebastidae
## 108 cervidae                737.78           2.86792688        sebastidae
## 109 cervidae                780.54           2.89239516        sebastidae
## 110 cervidae                202.00           2.30535137       ictaluridae
## 111 cervidae                  9.55           0.98000337      syngnathidae
## 112 cervidae                  1.22           0.08635983      syngnathidae
## 113 cervidae                  8.96           0.95230801    tetraodontidae
## 114 cervidae               3000.00           3.47712126      accipitridae
## 115 cervidae                846.00           2.92737036      accipitridae
## 116 cervidae               1699.00           3.23019338      accipitridae
## 117 cervidae               2049.00           3.31154196      accipitridae
## 118 cervidae                975.00           2.98900462      accipitridae
## 119 cervidae               2203.00           3.34301450      accipitridae
## 120 cervidae                719.00           2.85672889          anatidae
## 121 cervidae               2320.00           3.36548799       apterygidae
## 122 cervidae                 48.00           1.68124124     caprimulgidae
## 123 cervidae                521.00           2.71683772    haematopodidae
## 124 cervidae                 47.70           1.67851838        columbidae
## 125 cervidae                150.00           2.17609126        columbidae
## 126 cervidae                140.33           2.14715052        columbidae
## 127 cervidae                103.00           2.01283723        coraciidae
## 128 cervidae                 67.00           1.82607480          upupidae
## 129 cervidae                151.50           2.18041263         cuculidae
## 130 cervidae                128.00           2.10720997         cuculidae
## 131 cervidae                300.00           2.47712126         cuculidae
## 132 cervidae                433.00           2.63648790         cuculidae
## 133 cervidae                469.00           2.67117284      accipitridae
## 134 cervidae                978.00           2.99033886      accipitridae
## 135 cervidae                807.00           2.90687353      accipitridae
## 136 cervidae                141.00           2.14921911      accipitridae
## 137 cervidae               1126.00           3.05153839      accipitridae
## 138 cervidae                626.00           2.79657433      accipitridae
## 139 cervidae                971.00           2.98721923      accipitridae
## 140 cervidae                521.00           2.71683772      accipitridae
## 141 cervidae                315.50           2.49899936      accipitridae
## 142 cervidae               1033.70           3.01439452      accipitridae
## 143 cervidae               1125.00           3.05115252        falconidae
## 144 cervidae                625.00           2.79588002        falconidae
## 145 cervidae                675.00           2.82930377        falconidae
## 146 cervidae                721.00           2.85793527        falconidae
## 147 cervidae                781.50           2.89292898        falconidae
## 148 cervidae                112.00           2.04921802        falconidae
## 149 cervidae                200.00           2.30103000        falconidae
## 150 cervidae                410.00           2.61278386       phasianidae
## 151 cervidae               1750.00           3.24303805       phasianidae
## 152 cervidae               1050.00           3.02118930       phasianidae
## 153 cervidae                620.00           2.79239169       phasianidae
## 154 cervidae                381.50           2.58149454       phasianidae
## 155 cervidae               1139.00           3.05652372       phasianidae
## 156 cervidae               2936.00           3.46775605       phasianidae
## 157 cervidae                900.00           2.95424251       phasianidae
## 158 cervidae                506.00           2.70415052          rallidae
## 159 cervidae                165.00           2.21748394          rallidae
## 160 cervidae                266.00           2.42488164          rallidae
## 161 cervidae                 11.00           1.04139268    acrocephalisae
## 162 cervidae                  8.00           0.90308999      aegithalidae
## 163 cervidae                 30.00           1.47712125         alaudidae
## 164 cervidae                 37.70           1.57634135      cardinalidae
## 165 cervidae                 32.80           1.51587384      cardinalidae
## 166 cervidae                  8.77           0.94299959         certhidae
## 167 cervidae                  9.80           0.99122608      cisticolidae
## 168 cervidae               1410.00           3.14921911          corvidae
## 169 cervidae                130.00           2.11394335          corvidae
## 170 cervidae                 42.00           1.62324929        cotingidae
## 171 cervidae                 16.70           1.22271647       emberizidae
## 172 cervidae                 14.30           1.15533604       emberizidae
## 173 cervidae                 46.30           1.66558099       emberizidae
## 174 cervidae                 44.70           1.65030752       emberizidae
## 175 cervidae                 18.10           1.25767857       emberizidae
## 176 cervidae                 12.20           1.08635983       emberizidae
## 177 cervidae               1550.00           3.19033170      fringillidae
## 178 cervidae                 23.25           1.36642296      fringillidae
## 179 cervidae                 10.70           1.02938378      fringillidae
## 180 cervidae                 89.00           1.94939001         icteridae
## 181 cervidae                 89.00           1.94939001         icteridae
## 182 cervidae                 27.00           1.43136376          incertae
## 183 cervidae                 30.00           1.47712125          laniidae
## 184 cervidae                 48.10           1.68214508          laniidae
## 185 cervidae                 44.22           1.64561874          laniidae
## 186 cervidae                 35.00           1.54406804          laniidae
## 187 cervidae                 50.10           1.69983773           mimidae
## 188 cervidae                 21.22           1.32674538      motacillidae
## 189 cervidae                 17.50           1.24303805      motacillidae
## 190 cervidae                 12.80           1.10720997      muscicapidae
## 191 cervidae                 25.20           1.40140054      muscicapidae
## 192 cervidae                 15.21           1.18212921      muscicapidae
## 193 cervidae                 16.48           1.21695721      muscicapidae
## 194 cervidae                 11.00           1.04139268           paridae
## 195 cervidae                 10.10           1.00432137           paridae
## 196 cervidae                 16.60           1.22010809           paridae
## 197 cervidae                 11.00           1.04139268           paridae
## 198 cervidae                 11.30           1.05307844         parulidae
## 199 cervidae                  9.80           0.99122608         parulidae
## 200 cervidae                 16.10           1.20682588         parulidae
## 201 cervidae                 18.90           1.27646180         parulidae
## 202 cervidae                  9.70           0.98677173         parulidae
## 203 cervidae                 14.00           1.14612804         parulidae
## 204 cervidae                  8.60           0.93449845         parulidae
## 205 cervidae                  9.60           0.98227123         parulidae
## 206 cervidae                  9.50           0.97772360         parulidae
## 207 cervidae                  9.00           0.95424251         parulidae
## 208 cervidae                  9.00           0.95424251         parulidae
## 209 cervidae                  9.30           0.96848295         parulidae
## 210 cervidae                  7.50           0.87506126    phylloscopidae
## 211 cervidae                158.00           2.19865709 ptilonorhynchidae
## 212 cervidae                  5.30           0.72427587         regulidae
## 213 cervidae                  5.15           0.71180723         regulidae
## 214 cervidae                 18.30           1.26245109         stittidae
## 215 cervidae                 14.80           1.17026172          sylvidae
## 216 cervidae                 10.50           1.02118930         sylviidae
## 217 cervidae                  8.80           0.94448267         sylviidae
## 218 cervidae                 11.00           1.04139268     troglodytidae
## 219 cervidae                 18.50           1.26717173     troglodytidae
## 220 cervidae                 11.20           1.04921802     troglodytidae
## 221 cervidae                  9.50           0.97772360     troglodytidae
## 222 cervidae                 30.80           1.48855072          turdidae
## 223 cervidae                 13.80           1.13987909        tyrannidae
## 224 cervidae                  9.90           0.99563519        tyrannidae
## 225 cervidae                 12.30           1.08990511        tyrannidae
## 226 cervidae                 40.40           1.60638137        tyrannidae
## 227 cervidae                  8.50           0.92941893        vireonidae
## 228 cervidae                 10.00           1.00000000        vireonidae
## 229 cervidae                 11.40           1.05690485        vireonidae
## 230 cervidae                 17.60           1.24551267        vireonidae
## 231 cervidae                900.00           2.95424251          ardeidae
## 232 cervidae                 67.00           1.82607480          ardeidae
## 233 cervidae                277.37           2.44305949           picidae
## 234 cervidae                 38.00           1.57978360           picidae
## 235 cervidae                109.25           2.03842145           picidae
## 236 cervidae                 59.00           1.77085201           picidae
## 237 cervidae                 65.65           1.81723473           picidae
## 238 cervidae                124.50           2.09516935           picidae
## 239 cervidae                186.67           2.27107453           picidae
## 240 cervidae               1941.00           3.28802554       strigopidae
## 241 cervidae              25000.00           4.39794001           rheidae
## 242 cervidae              15000.00           4.17609126           rheidae
## 243 cervidae                119.00           2.07554696         strigidae
## 244 cervidae                252.00           2.40140054         strigidae
## 245 cervidae                156.50           2.19451434         strigidae
## 246 cervidae               2191.00           3.34064238         strigidae
## 247 cervidae               1510.00           3.17897695         strigidae
## 248 cervidae                 61.32           1.78760215         strigidae
## 249 cervidae               1920.00           3.28330123         strigidae
## 250 cervidae                519.00           2.71516736         strigidae
## 251 cervidae                285.00           2.45484486         tytonidae
## 252 cervidae              88250.00           4.94571471     struthionidae
## 253 cervidae                622.00           2.79379038         tinamidae
## 254 cervidae                436.52           2.64000415   chrysochloridae
## 255 cervidae                 23.00           1.36172784   chrysochloridae
## 256 cervidae              46099.90           4.66369998    antilocapridae
## 257 cervidae              63503.84           4.80279999           bovidae
## 258 cervidae             136000.34           5.13353999           bovidae
## 259 cervidae             167498.14           5.22400999           bovidae
## 260 cervidae             629999.20           5.79934000           bovidae
## 261 cervidae             628000.52           5.79796000           bovidae
## 262 cervidae              50999.98           4.70757001           bovidae
## 263 cervidae              69499.23           4.84197999           bovidae
## 264 cervidae              18143.87           4.25872993           bovidae
## 265 cervidae              20411.74           4.30988003           bovidae
## 266 cervidae              21474.84           4.33192994           bovidae
## 267 cervidae               4600.02           3.66275972           bovidae
## 268 cervidae             629999.20           5.79934000           bovidae
## 269 cervidae             113998.73           5.05690001           bovidae
## 270 cervidae              90719.36           4.95769998           bovidae
## 271 cervidae              11300.04           4.05307998           bovidae
## 272 cervidae              30999.88           4.49136001           bovidae
## 273 cervidae             635038.42           5.80280000           bovidae
## 274 cervidae              54431.46           4.73584998           bovidae
## 275 cervidae             197314.95           5.29515999           bovidae
## 276 cervidae             307227.44           5.48746000          cervidae
## 277 cervidae              62823.19           4.79811998          cervidae
## 278 cervidae              24050.27           4.38111996          cervidae
## 279 cervidae             234757.78           5.37061999          cervidae
## 280 cervidae              29450.32           4.46909002          cervidae
## 281 cervidae              71449.63           4.85399998          cervidae
## 282 cervidae              13499.88           4.13032991          cervidae
## 283 cervidae              53864.17           4.73129997          cervidae
## 284 cervidae              87884.04           4.94391001          cervidae
## 285 cervidae              35000.16           4.54407003          cervidae
## 286 cervidae               7499.98           3.87506011          cervidae
## 287 cervidae             102058.69           5.00884999          cervidae
## 288 cervidae            1339985.19           6.12710000        giraffidae
## 289 cervidae             230001.15           5.36173001        giraffidae
## 290 cervidae              80513.74           4.90587000            suidae
## 291 cervidae              34700.04           4.54032997       tayassuidae
## 292 cervidae              23205.98           4.36559991       tayassuidae
## 293 cervidae              20250.23           4.30642996       tayassuidae
## 294 cervidae              10850.01           4.03543014        tragulidae
## 295 cervidae               5399.95           3.73238974         ailuridae
## 296 cervidae               4989.53           3.69805964           canidae
## 297 cervidae              27749.81           4.44326001           canidae
## 298 cervidae               7370.04           3.86746985           canidae
## 299 cervidae               3989.97           3.60096963           canidae
## 300 cervidae               4499.97           3.65320962           canidae
## 301 cervidae               3249.97           3.51187935           canidae
## 302 cervidae               2089.30           3.32000080           canidae
## 303 cervidae                 60.00           1.77815125         dipodidae
## 304 cervidae               9549.93           3.98000019        eupleridae
## 305 cervidae              50000.00           4.69897000           felidae
## 306 cervidae              12999.90           4.11394001           felidae
## 307 cervidae               3394.53           3.53077965           felidae
## 308 cervidae               4825.03           3.68350002           felidae
## 309 cervidae               6803.93           3.83275984           felidae
## 310 cervidae               9989.64           3.99954984           felidae
## 311 cervidae               3628.77           3.55975944           felidae
## 312 cervidae              11999.97           4.07918016           felidae
## 313 cervidae              10205.87           4.00885003           felidae
## 314 cervidae              29999.91           4.47711995           felidae
## 315 cervidae              11049.94           4.04335992           felidae
## 316 cervidae              11339.92           4.05460999           felidae
## 317 cervidae               3589.96           3.55508961           felidae
## 318 cervidae              84040.77           4.92449002           felidae
## 319 cervidae              48374.89           4.68461999           felidae
## 320 cervidae             112000.51           5.04922000           felidae
## 321 cervidae               2500.00           3.39794001           felidae
## 322 cervidae              89999.48           4.95424000           felidae
## 323 cervidae              30500.01           4.48429998           felidae
## 324 cervidae               3599.98           3.55630009       herpestidae
## 325 cervidae                620.00           2.79239169       herpestidae
## 326 cervidae                281.84           2.45000263       herpestidae
## 327 cervidae               2810.00           3.44870632       herpestidae
## 328 cervidae               3599.98           3.55630009       herpestidae
## 329 cervidae              10000.00           4.00000000          hyanidae
## 330 cervidae               4466.22           3.64994011        mustelidae
## 331 cervidae               1750.01           3.24304053        mustelidae
## 332 cervidae              21545.67           4.33336000        mustelidae
## 333 cervidae                883.49           2.94620164        mustelidae
## 334 cervidae               1799.99           3.25527009        mustelidae
## 335 cervidae               2475.03           3.39358047        mustelidae
## 336 cervidae               3175.19           3.50176972        mustelidae
## 337 cervidae                270.54           2.43223149        mustelidae
## 338 cervidae                150.19           2.17664102        mustelidae
## 339 cervidae                808.74           2.90780892        mustelidae
## 340 cervidae                562.34           2.74999898        mustelidae
## 341 cervidae                912.01           2.95999960        mustelidae
## 342 cervidae                 88.10           1.94497591        mustelidae
## 343 cervidae                524.81           2.72000210        mustelidae
## 344 cervidae               8618.27           3.93542010        mustelidae
## 345 cervidae               2887.62           3.46054004       procyonidae
## 346 cervidae              87500.39           4.94200999           ursidae
## 347 cervidae              97750.73           4.99012001           ursidae
## 348 cervidae               1750.01           3.24304053        viverridae
## 349 cervidae               2150.01           3.33244048        viverridae
## 350 cervidae               8000.00           3.90308999        viverridae
## 351 cervidae               1096.48           3.04000071        dasyuridae
## 352 cervidae               2810.00           3.44870632        dasyuridae
## 353 cervidae                 23.00           1.36172784        dasyuridae
## 354 cervidae                 27.50           1.43933269        dasyuridae
## 355 cervidae                 19.50           1.29003461       didelphidae
## 356 cervidae                 29.00           1.46239800       didelphidae
## 357 cervidae               6649.97           3.82281969      macropodidae
## 358 cervidae              27250.22           4.43537001      macropodidae
## 359 cervidae              11249.93           4.05114982      macropodidae
## 360 cervidae              22124.83           4.34487994      macropodidae
## 361 cervidae              10375.05           4.01599020      macropodidae
## 362 cervidae              21250.05           4.32735996      macropodidae
## 363 cervidae              16850.00           4.22659990      macropodidae
## 364 cervidae              46249.82           4.66511005      macropodidae
## 365 cervidae               4646.01           3.66708014      macropodidae
## 366 cervidae               1660.01           3.22011070        potoroidae
## 367 cervidae               1899.98           3.27874903        potoroidae
## 368 cervidae               1299.99           3.11394001   pseudocheiridae
## 369 cervidae               5000.00           3.69897000      macropodidae
## 370 cervidae               4649.97           3.66745015      macropodidae
## 371 cervidae               5399.95           3.73238974      macropodidae
## 372 cervidae              14999.96           4.17609010      macropodidae
## 373 cervidae               2875.01           3.45863936     phalangeridae
## 374 cervidae              25499.99           4.40654001        vombatidae
## 375 cervidae              25999.80           4.41497001        vombatidae
## 376 cervidae                800.00           2.90308999       erinaceidae
## 377 cervidae                296.00           2.47129171       erinaceidae
## 378 cervidae                340.20           2.53173431         leporidae
## 379 cervidae               1360.79           3.13379111         leporidae
## 380 cervidae               4050.05           3.60746038         leporidae
## 381 cervidae               2267.98           3.35563922         leporidae
## 382 cervidae               3549.11           3.55011946         leporidae
## 383 cervidae               5250.01           3.72016013         leporidae
## 384 cervidae               3129.97           3.49554017         leporidae
## 385 cervidae               2825.01           3.45101999         leporidae
## 386 cervidae               1600.00           3.20411998         leporidae
## 387 cervidae               2150.01           3.33244048         leporidae
## 388 cervidae               1360.79           3.13379111         leporidae
## 389 cervidae               1349.99           3.13033055         leporidae
## 390 cervidae                155.30           2.19117146       ochotonidae
## 391 cervidae                146.50           2.16583762       ochotonidae
## 392 cervidae                 58.00           1.76342799   macroscelididae
## 393 cervidae                201.00           2.30319606   macroscelididae
## 394 cervidae                535.20           2.72851611   macroscelididae
## 395 cervidae                257.00           2.40993312    tachyglossidae
## 396 cervidae                390.50           2.59162104       peramelidae
## 397 cervidae                775.00           2.88930170       peramelidae
## 398 cervidae             427996.29           5.63144000           equidae
## 399 cervidae            2249986.95           6.35218000    rhinocerotidae
## 400 cervidae            1050001.69           6.02119000    rhinocerotidae
## 401 cervidae               3899.96           3.59106015      bradypodidae
## 402 cervidae            4000000.08           6.60206000      elephantidae
## 403 cervidae            4000000.08           6.60206000      elephantidae
## 404 cervidae                 22.00           1.34242268        cricetidae
## 405 cervidae               1133.99           3.05460923     aplodontiidae
## 406 cervidae                625.00           2.79588002      bathyergidae
## 407 cervidae                148.00           2.17026172      bathyergidae
## 408 cervidae                 65.00           1.81291336      bathyergidae
## 409 cervidae                242.00           2.38381537      bathyergidae
## 410 cervidae                155.00           2.19033170      bathyergidae
## 411 cervidae                 39.00           1.59106461      bathyergidae
## 412 cervidae               8000.00           3.90308999          caviidae
## 413 cervidae               5190.03           3.71516987     chinchillidae
## 414 cervidae                 22.57           1.35353156        cricetidae
## 415 cervidae                 66.30           1.82151353        cricetidae
## 416 cervidae                 92.14           1.96444821        cricetidae
## 417 cervidae                 38.00           1.57978360        cricetidae
## 418 cervidae                 70.87           1.85046243        cricetidae
## 419 cervidae                 56.70           1.75358306        cricetidae
## 420 cervidae                 35.44           1.54949371        cricetidae
## 421 cervidae                 49.61           1.69556923        cricetidae
## 422 cervidae                 29.50           1.46982202        cricetidae
## 423 cervidae                 85.50           1.93196612        cricetidae
## 424 cervidae                 30.00           1.47712125        cricetidae
## 425 cervidae                395.48           2.59712452        cricetidae
## 426 cervidae                308.30           2.48897353        cricetidae
## 427 cervidae                132.29           2.12152702        cricetidae
## 428 cervidae                255.15           2.40679557        cricetidae
## 429 cervidae               1360.79           3.13379111        cricetidae
## 430 cervidae                 27.54           1.43996394        cricetidae
## 431 cervidae                 11.05           1.04336228        cricetidae
## 432 cervidae                 38.27           1.58285846        cricetidae
## 433 cervidae                 52.50           1.72015930         dipodidae
## 434 cervidae                350.00           2.54406804        echimyidae
## 435 cervidae                428.00           2.63144377        echimyidae
## 436 cervidae               4250.01           3.62838995    erethizontidae
## 437 cervidae               8618.27           3.93542010    erethizontidae
## 438 cervidae                160.18           2.20460829         geomyidae
## 439 cervidae                 68.80           1.83758844          gliridae
## 440 cervidae                 31.00           1.49136169          gliridae
## 441 cervidae                153.56           2.18627810      heteromyidae
## 442 cervidae                 41.82           1.62138403      heteromyidae
## 443 cervidae                 56.70           1.75358306      heteromyidae
## 444 cervidae                144.58           2.16010822      heteromyidae
## 445 cervidae                 76.20           1.88195497      heteromyidae
## 446 cervidae              17000.04           4.23044994       hystricidae
## 447 cervidae              14999.96           4.17609010       hystricidae
## 448 cervidae               2749.98           3.43932954       hystridicae
## 449 cervidae                 31.60           1.49968708           muridae
## 450 cervidae                 21.20           1.32633586           muridae
## 451 cervidae                 88.30           1.94596070           muridae
## 452 cervidae                122.00           2.08635983           muridae
## 453 cervidae               1185.00           3.07371835        nesomyidae
## 454 cervidae                215.60           2.33364876        nesomyidae
## 455 cervidae                165.45           2.21866677        nesomyidae
## 456 cervidae                100.86           2.00371896      octodontidae
## 457 cervidae                 95.80           1.98136551         sciuridae
## 458 cervidae                102.50           2.01072387         sciuridae
## 459 cervidae                148.84           2.17271966         sciuridae
## 460 cervidae                 64.50           1.80955972         sciuridae
## 461 cervidae               3401.97           3.53173048         sciuridae
## 462 cervidae               3401.97           3.53173048         sciuridae
## 463 cervidae                375.00           2.57403127         sciuridae
## 464 cervidae                793.80           2.89971109         sciuridae
## 465 cervidae                532.98           2.72671091         sciuridae
## 466 cervidae                264.30           2.42209716         sciuridae
## 467 cervidae                952.99           2.97908834         sciuridae
## 468 cervidae                327.50           2.51521130         sciuridae
## 469 cervidae                725.75           2.86078704         sciuridae
## 470 cervidae                578.34           2.76218323         sciuridae
## 471 cervidae                637.87           2.80473218         sciuridae
## 472 cervidae                794.49           2.90008843         sciuridae
## 473 cervidae                106.00           2.02530586         sciuridae
## 474 cervidae                198.45           2.29765110         sciuridae
## 475 cervidae                748.43           2.87415119         sciuridae
## 476 cervidae                 26.93           1.43023635         sciuridae
## 477 cervidae                 42.52           1.62859326         sciuridae
## 478 cervidae                 42.52           1.62859326         sciuridae
## 479 cervidae                 42.52           1.62859326         sciuridae
## 480 cervidae                223.96           2.35017046         sciuridae
## 481 cervidae                502.00           2.70070372         sciuridae
## 482 cervidae                 10.00           1.00000000         soricidae
## 483 cervidae                  8.13           0.91009055         soricidae
## 484 cervidae                  4.17           0.62013606         soricidae
## 485 cervidae                  9.33           0.96988164         soricidae
## 486 cervidae                  4.37           0.64048144         soricidae
## 487 cervidae                 14.13           1.15014216         soricidae
## 488 cervidae                 47.86           1.67997269          talpidae
## 489 cervidae                103.50           2.01494035          talpidae
## 490 cervidae                 96.50           1.98452731          talpidae
## 491 cervidae                 81.42           1.91073110          talpidae
## 492 cervidae               2500.00           3.39794001          agamidae
## 493 cervidae                  3.46           0.53907610        colubridae
## 494 cervidae                  3.65           0.56229286        colubridae
## 495 cervidae                556.15           2.74519194        colubridae
## 496 cervidae                144.50           2.15986785        colubridae
## 497 cervidae                  9.00           0.95424251        colubridae
## 498 cervidae                450.00           2.65321251        colubridae
## 499 cervidae                256.70           2.40942587        colubridae
## 500 cervidae                642.80           2.80807587        colubridae
## 501 cervidae                147.32           2.16826171        colubridae
## 502 cervidae                234.10           2.36940141        colubridae
## 503 cervidae                315.72           2.49930209        colubridae
## 504 cervidae                165.00           2.21748394        colubridae
## 505 cervidae                534.50           2.72794771        colubridae
## 506 cervidae                 78.50           1.89486966        colubridae
## 507 cervidae                313.24           2.49587722        colubridae
## 508 cervidae                190.55           2.28000895        colubridae
## 509 cervidae                 62.50           1.79588002        colubridae
## 510 cervidae                375.00           2.57403127        colubridae
## 511 cervidae               1004.00           3.00173371        colubridae
## 512 cervidae                 21.51           1.33264041        colubridae
## 513 cervidae                306.00           2.48572143        colubridae
## 514 cervidae                249.30           2.39672228        colubridae
## 515 cervidae                 48.79           1.68833082          elapidae
## 516 cervidae                330.00           2.51851394          elapidae
## 517 cervidae                479.00           2.68033551          elapidae
## 518 cervidae               7000.00           3.84509804         iguanidae
## 519 cervidae               3780.00           3.57749180         iguanidae
## 520 cervidae               3200.00           3.50514998         iguanidae
## 521 cervidae               4223.33           3.62565502         iguanidae
## 522 cervidae                927.00           2.96707973         iguanidae
## 523 cervidae                210.00           2.32221930         iguanidae
## 524 cervidae                 59.00           1.77085201        lacertilia
## 525 cervidae                 40.00           1.60205999        lacertilia
## 526 cervidae                 28.00           1.44715803       liolaemidae
## 527 cervidae               1226.85           3.08879147        pythonidae
## 528 cervidae                638.00           2.80482068         scincidae
## 529 cervidae                231.12           2.36383753         viperidae
## 530 cervidae                188.00           2.27415785         viperidae
## 531 cervidae                 16.95           1.22916970         viperidae
## 532 cervidae                826.23           2.91710096         viperidae
## 533 cervidae                319.90           2.50501424         viperidae
## 534 cervidae                106.70           2.02816442         viperidae
## 535 cervidae               1020.00           3.00860017         viperidae
## 536 cervidae                414.00           2.61700034         viperidae
## 537 cervidae                138.70           2.14207646         viperidae
## 538 cervidae                 67.20           1.82736927         viperidae
## 539 cervidae                280.30           2.44762310         viperidae
## 540 cervidae                234.70           2.37051309         viperidae
## 541 cervidae                196.81           2.29404716         viperidae
## 542 cervidae                162.14           2.20989017         viperidae
## 543 cervidae                 97.40           1.98855896         viperidae
## 544 cervidae                691.00           2.83947805          chelidae
## 545 cervidae                595.00           2.77451697          chelidae
## 546 cervidae               4250.00           3.62838893       chelydridae
## 547 cervidae                354.50           2.54961624          emydidae
## 548 cervidae                588.00           2.76937733          emydidae
## 549 cervidae               1294.00           3.11193428          emydidae
## 550 cervidae                462.00           2.66464198          emydidae
## 551 cervidae               1135.00           3.05499586          emydidae
## 552 cervidae                211.00           2.32428246          emydidae
## 553 cervidae                720.60           2.85769426       geoemydidae
## 554 cervidae                154.70           2.18949031     kinosternidae
## 555 cervidae                164.00           2.21484385     kinosternidae
## 556 cervidae                190.00           2.27875360     kinosternidae
## 557 cervidae               6166.60           3.79004578      testudinidae
## 558 cervidae               2000.00           3.30103000      testudinidae
## 559 cervidae                335.00           2.52504481      testudinidae
## 560 cervidae                232.00           2.36548799      testudinidae
## 561 cervidae                620.00           2.79239169      testudinidae
## 562 cervidae               3000.00           3.47712126      testudinidae
## 563 cervidae                500.00           2.69897000      testudinidae
## 564 cervidae              10600.00           4.02530586      testudinidae
## 565 cervidae                400.00           2.60205999      testudinidae
## 566 cervidae               1522.00           3.18241465      testudinidae
## 567 cervidae               1018.00           3.00774778      testudinidae
## 568 cervidae                222.00           2.34635297      testudinidae
## 569 cervidae               2982.33           3.47455570      trionychidae
##     homerange.genus        homerange.species
## 1          anguilla                 rostrata
## 2         moxostoma                poecilura
## 3        campostoma                 anomalum
## 4       clinostomus              funduloides
## 5       rhinichthys               cataractae
## 6              esox              masquinongy
## 7        pollachius               pollachius
## 8        pollachius                   virens
## 9        acanthurus                 lineatus
## 10             naso                lituratus
## 11             naso                unicornis
## 12    ophioblennius               atlanticus
## 13           caranx                ignobilis
## 14      ambloplites                rupestris
## 15          lepomis                 gibbosus
## 16          lepomis              macrochirus
## 17          lepomis                megalotis
## 18      micropterus                 dolomieu
## 19      micropterus                salmoides
## 20          pomoxis                annularis
## 21        chaetodon                baronessa
## 22        chaetodon                trichrous
## 23        chaetodon             trifascialis
## 24        chaetodon             trifasciatus
## 25        chaetodon             unimaculatus
## 26   cheilodactylus             spectrabilis
## 27   amblycirrhitus                    pinos
## 28   cirrhitichthys                    falco
## 29  scorpaenichthys               marmoratus
## 30    amblyeleotris                 japonica
## 31       lythrypnus                    dalli
## 32        priolepis                 hipoliti
## 33     rhinogobiops                nicholsii
## 34     valenciennea              longipinnis
## 35         kyphosus                sectatrix
## 36         bodianus                    rufus
## 37         chelinus                undulatus
## 38            coris                    julis
## 39      halichoeres               bivittatus
## 40      halichoeres                  garnoti
## 41      halichoeres              maculipinna
## 42      halichoeres                    poeyi
## 43        labroides               dimidiatus
## 44           labrus                 bergylta
## 45    opthalmolepis               lineolatus
## 46    semicossyphus                  pulcher
## 47    tautogolabrus                adspersus
## 48       thalassoma              bifasciatum
## 49       thalassoma                   lunare
## 50        lethrinus                    harak
## 51         lutjanus                   analis
## 52         lutjanus                   apodus
## 53         lutjanus               decussatus
## 54         lutjanus                  griseus
## 55          ocyurus                chrysurus
## 56     caulolatilus                 princeps
## 57    dicentrarchus                   labrax
## 58   mulloidichthys            flavolineatus
## 59       parupeneus               porphyreus
## 60            perca               flavescens
## 61        abudefduf                  luridus
## 62       centropyge                     argi
## 63          chromis                  chromis
## 64      chrysiptera               biocellata
## 65        dascyllus                  aruanus
## 66      pomacentrus                    wardi
## 67        stegastes                 apicalis
## 68        stegastes                 partitus
## 69        stegastes               variabilis
## 70        chlorurus              microrhinos
## 71           scarus                    iseri
## 72           scarus                rivulatus
## 73        sparisoma             aurofrenatum
## 74        sparisoma             chrysopterum
## 75        sparisoma               rubripinne
## 76        sparisoma                   viride
## 77    cephalopholis                    argus
## 78    cephalopholis                cruentata
## 79    cephalopholis              hemistiktos
## 80    cephalopholis                  miniata
## 81      epinephelus                 guttatus
## 82      epinephelus               marginatus
## 83      epinephelus                    morio
## 84      epinephelus                 striatus
## 85      epinephelus                  tauvina
## 86   hypoplectrodes                   huntii
## 87   hypoplectrodes              maccullochi
## 88     mycteroperca                   bonaci
## 89       paralabrax               clathratus
## 90       paralabrax                nebulifer
## 91     plectropomus                areolatus
## 92     plectropomus                leopardus
## 93         serranus                 cabrilla
## 94         serranus                   scriba
## 95            sarpa                    salpa
## 96           sparus                  auratus
## 97     oncorhynchus                   clarki
## 98     oncorhynchus                    gilae
## 99     oncorhynchus                   mykiss
## 100           salmo                    salar
## 101           salmo                   trutta
## 102          cottus                   bairdi
## 103          cottus                carolinae
## 104          cottus                    gobio
## 105        sebastes                 caurinus
## 106        sebastes                  inermis
## 107        sebastes                  maliger
## 108        sebastes                 melanops
## 109        sebastes                 mustinus
## 110       ictalurus                  natalis
## 111     hippocampus               guttulatus
## 112        nerophis           lumbriciformis
## 113    canthigaster                 rostrata
## 114          aquila               chrysaetos
## 115           buteo                    buteo
## 116       circaetus                 gallicus
## 117      hieraaetus                fasciatus
## 118      hieraaetus                 pennatus
## 119        neophron             percnopterus
## 120            anas                 strepera
## 121         apteryx                australis
## 122     caprimulgus                europaeus
## 123      haematopus               ostralegus
## 124     scardafella                     inca
## 125         columba                 palumbus
## 126    streptopelia                   turtur
## 127        coracias                 garrulus
## 128           upupa                    epops
## 129        clamator               glandarius
## 130         cuculus                  canorus
## 131       geococcyx            californianus
## 132     neopmorphus               radiolosus
## 133       accipiter                 cooperii
## 134       accipiter                 gentilis
## 135       accipiter                    nisus
## 136       accipiter                 striatus
## 137           buteo              jamaicensis
## 138           buteo                 lineatus
## 139           buteo                swainsoni
## 140          circus                  cyaneus
## 141          circus                 pygargus
## 142          milvus                   milvus
## 143        caracara                 cheriway
## 144        daptrius               americanus
## 145           falco                biarmicus
## 146           falco                mexicanus
## 147           falco               peregrinus
## 148           falco               sparverius
## 149           falco              tinnunculus
## 150          bonasa                  bonasia
## 151    centrocercus             urophasianus
## 152     dendragapus                 obscurus
## 153         lagopus                  lagopus
## 154          perdix                   perdix
## 155          tetrao                   tetrix
## 156          tetrao                urogallus
## 157     typmanuchus          cupido pinnatus
## 158        aramides                    wolfi
## 159            crex                     crex
## 160          rallus                  elegans
## 161       hippolais               polyglotta
## 162      aegithalos                 caudatus
## 163         lululla                  arborea
## 164           habia               fuscicauda
## 165           habia                   rubica
## 166         certhia               familiaris
## 167       cisticola                 juncidis
## 168          corvus                    corax
## 169       nucifraga            caryocatactes
## 170       phytotoma                raimondii
## 171      ammodramus               savannarum
## 172       passerina                   cyanea
## 173          pipilo                   aberti
## 174          pipilo                   fuscus
## 175        spizella                  arborea
## 176        spizella                passerina
## 177       carduelis                cannabina
## 178       fringilla                  coelebs
## 179         serinus                  serinus
## 180       sturnella                    magna
## 181       sturnella                 neglecta
## 182         icteria                   virens
## 183         laniuis                 collurio
## 184         laniuis             ludovicianus
## 185          lanius                    minor
## 186          lanius                  senator
## 187           mimus              polyglottos
## 188       motacilla                     alba
## 189       motacilla                    flava
## 190       muscicapa                  striata
## 191        oenanthe                 oenanthe
## 192     phoenicurus              phoenicurus
## 193        saxicola                  rubetra
## 194           parus             atricapillus
## 195           parus             carolinensis
## 196           parus                inornatus
## 197           parus                palustris
## 198  geothlypis\xa0             philadelphia
## 199      geothylpis                  trichas
## 200    protonotaria                   citrea
## 201         seiurus             aurocapillus
## 202       setophaga                    fusca
## 203       setophaga                kirtlandi
## 204       setophaga                 magnolia
## 205       setophaga             pensylvanica
## 206       setophaga                 petechia
## 207       setophaga                ruticilla
## 208       setophaga                   virens
## 209        wilsonia               canadensis
## 210    phylloscopus                  bonelli
## 211    scenopoeetes             dentirostris
## 212         regulus             ignicapillus
## 213         regulus                  regulus
## 214           sitta                 europaea
## 215         chamaea                 fasciata
## 216          sylvia                    sarda
## 217          sylvia                   undata
## 218      thryomanes                 bewickiI
## 219     thryothorus             ludovicianus
## 220     troglodytes                    aedon
## 221     troglodytes              troglodytes
## 222          sialia                   sialis
## 223        contopus                   virens
## 224       empidonax                  minimus
## 225       empidonax                 wrightii
## 226        tyrannus                 tyrannus
## 227           vireo             atricapillus
## 228           vireo                    belli
## 229           vireo                  griseus
## 230           vireo                olivaceus
## 231        botaurus                stellaris
## 232      ixobrychus                   exilis
## 233       dryocopus                  martius
## 234            jynx                torquilla
## 235        picoides                 leucotos
## 236        picoides                   medius
## 237        picoides              tridactylus
## 238           picus                    canus
## 239           picus                  viridis
## 240        strigops              habroptilus
## 241            rhea                americana
## 242            rhea                  pennata
## 243        aegolius                 funereus
## 244            asio                     otus
## 245          athene                   noctua
## 246            bubo                     bubo
## 247            bubo              virginianus
## 248      glaucidium               passerinum
## 249          nyctea                scandiaca
## 250           strix                    aluco
## 251            tyto                     alba
## 252        struthio                  camelus
## 253     nothoprocta                   ornata
## 254    chrysospalax               trevelyani
## 255      eremitalpa                   granti
## 256     antilocapra                americana
## 257       aepyceros                 melampus
## 258      alcelaphus               buselaphus
## 259      ammotragus                   lervia
## 260           bison                    bison
## 261           bison                  bonasus
## 262           capra                   hircus
## 263           capra                pyrenaica
## 264     cephalophus               callipygus
## 265     cephalophus                 dorsalis
## 266         gazella                  gazella
## 267         madoqua                guentheri
## 268        oreamnos               americanus
## 269            ovis                    ammon
## 270            ovis               canadensis
## 271      raphicerus               campestris
## 272       rupicapra                rupicapra
## 273     taurotragus                     oryx
## 274     tragelaphus                 scriptus
## 275     tragelaphus             strepsiceros
## 276           alces                    alces
## 277            axis                     axis
## 278       capreolus                capreolus
## 279          cervus                  elaphus
## 280          cervus                   nippon
## 281            dama                     dama
## 282       muntiacus                  reevesi
## 283      odocoileus                 hemionus
## 284      odocoileus              virginianus
## 285      ozotoceros              bezoarticus
## 286            pudu                     puda
## 287        rangifer                 tarandus
## 288         giraffa           camelopardalis
## 289          okapia                johnstoni
## 290    phacochoerus              aethiopicus
## 291       catagonus                  wagneri
## 292          pecari                   tajacu
## 293         tayassu                   pecari
## 294      hyemoschus                aquaticus
## 295         ailurus                  fulgens
## 296          alopex                  lagopus
## 297           canis                 simensis
## 298     pseudalopex                 culpaeus
## 299     pseudalopex                  griseus
## 300          vulpes                  macroti
## 301          vulpes                 ruppelli
## 302          vulpes                    velox
## 303      stylodipus                    telum
## 304    cryptoprocta                    ferox
## 305        acinonyx                  jubatus
## 306         caracal                  caracal
## 307           felis                    catus
## 308           felis               sylvestris
## 309     herpailurus             yagouaroundi
## 310       leopardus                 pardalis
## 311       leopardus                   wiedii
## 312     leptailurus                   serval
## 313            lynx               canadensis
## 314            lynx                     lynx
## 315            lynx                 pardinus
## 316            lynx                    rufus
## 317       oncifelis                geoffroyi
## 318        panthera                     onca
## 319        panthera                   pardus
## 320        panthera                   tigris
## 321    prionailurus              bengalensis
## 322            puma                 concolor
## 323           uncia                    uncia
## 324          atilax              paludinosus
## 325        cynictis              penicillata
## 326        helogale                  parvula
## 327       herpestes               inchneumon
## 328       ichneumia                albicauda
## 329        proteles                cristatus
## 330            eira                  barbata
## 331        galictis                  vittata
## 332            gulo                     gulo
## 333          martes                americana
## 334          martes                    foina
## 335          martes                   martes
## 336          martes                 pennanti
## 337         mustela                  erminea
## 338         mustela                  frenata
## 339         mustela                     furo
## 340         mustela                 lutreola
## 341         mustela                 nigripes
## 342         mustela                  nivalis
## 343         mustela                 sibirica
## 344         taxidea                    taxus
## 345           potos                   flavus
## 346      ailuropoda              melanoleuca
## 347        melursus                  ursinus
## 348         genetta                  genetta
## 349         genetta                  tigrina
## 350         viverra                  zibetha
## 351        dasyurus                geoffroii
## 352        dasyurus                maculatus
## 353     sminthopsis                 leucopus
## 354      antechinus                 stuartii
## 355     monodelphis                americana
## 356        thylamys                  elegans
## 357     dendrolagus                lumholtzi
## 358        macropus              antilopinus
## 359        macropus                 dorsalis
## 360        macropus              fuliginosus
## 361        macropus                giganteus
## 362        macropus                 robustus
## 363        macropus              rufogriseus
## 364        macropus                    rufus
## 365       petrogale                assimilis
## 366       bettongia                 gaimardi
## 367        potorous                 longipes
## 368     petauroides                   volans
## 369     onychogalea                 fraenata
## 370       thylogale               stigmatica
## 371       thylogale                   thetis
## 372        wallabia                  bicolor
## 373     trichosurus                vulpecula
## 374     lasiorhinus                 krefftii
## 375        vombatus                  ursinus
## 376       erinaceus                europaeus
## 377     hemiechinus                  auritus
## 378     brachylagus               idahoensis
## 379           lepus               americanus
## 380           lepus                 arcticus
## 381           lepus             californicus
## 382           lepus                 capensis
## 383           lepus                europaeus
## 384           lepus              nigricollis
## 385           lepus                  timidus
## 386     oryctolagus                cuniculus
## 387      sylvilagus                aquaticus
## 388      sylvilagus               floridanus
## 389      sylvilagus                palustris
## 390        ochotona                curzoniae
## 391        ochotona                 princeps
## 392    elephantulus                rufescens
## 393     petrodromus            tetradactylus
## 394     rhynchocyon              chrysopygus
## 395    tachyoryctes                splendens
## 396         isoodon                  auratus
## 397         isoodon                 obesulus
## 398           equus                 caballus
## 399   ceratotherium                    simum
## 400         diceros                 bicornis
## 401        bradypus                torquatus
## 402         elephas                  maximus
## 403       loxodonta                 africana
## 404       onychomys                 torridus
## 405      aplodontia                     rufa
## 406      bathyergus                  suillus
## 407       cryptomys               damarensis
## 408       cryptomys              hottentotus
## 409       georychus                 capensis
## 410    heliophobius         argenteocinereus
## 411  heterocephalus                   glaber
## 412      dolichotus                patagonus
## 413      lagostomus                  maximus
## 414   clethrionomys             californicus
## 415       hylaeamys             megacephalus
## 416          lemmus                sibiricus
## 417        microtus                 agrestis
## 418        microtus             californicus
## 419        microtus                 montanus
## 420        microtus              ochrogaster
## 421        microtus           pennsylvanicus
## 422        microtus                pinetorum
## 423        microtus              richardsoni
## 424          myopus             schisticolor
## 425         neotoma                  cinerea
## 426         neotoma                 fuscipes
## 427         neotoma                   lepida
## 428         neotoma                 micropus
## 429         ondatra                zibethica
## 430      peromyscus               gossypinus
## 431 reithrodontomys              raviventris
## 432      synaptomys                  cooperi
## 433      pygeretmus                  pumilio
## 434      proechimys                  cuvieri
## 435      proechimys             semispinosus
## 436         coendou              prehensilis
## 437       erethizon                 dorsatum
## 438        thomomys                   bottae
## 439      graphiurus                 ocularis
## 440     muscardinus             avellanarius
## 441       dipodomys                   ingens
## 442       dipodomys                 merriami
## 443       dipodomys                    ordii
## 444       dipodomys              spectabilis
## 445       dipodomys                stephensi
## 446         hystrix         africaeaustralis
## 447         hystrix                   indica
## 448       atherurus                africanus
## 449        apodemus              flavicollis
## 450        apodemus               sylvaticus
## 451        meriones                hurrianae
## 452       pseudomys                   fuscus
## 453      hypogeomys                 antimena
## 454         nesomys                audeberti
## 455         nesomys                    rufus
## 456      spalacopus                   cyanus
## 457        eutamias                sibiricus
## 458      funambulus                 pennanti
## 459       glaucomys                 sabrinus
## 460       glaucomys                   volans
## 461         marmota             flaviventris
## 462         marmota                    monax
## 463       paraxerus                palliatus
## 464         sciurus                   aberti
## 465         sciurus             carolinensis
## 466         sciurus                      lis
## 467         sciurus                    niger
## 468         sciurus                 vulgaris
## 469    spermophilus                 beecheyi
## 470    spermophilus              columbianus
## 471    spermophilus               franklinii
## 472    spermophilus                  parryii
## 473    spermophilus                spilosoma
## 474    spermophilus         tridecemlineatus
## 475    spermophilus               variegatus
## 476          tamias                  amoenus
## 477          tamias                  minimus
## 478          tamias           quadrivittatus
## 479          tamias                 umbrinus
## 480    tamiasciurus               hudsonicus
## 481           xerus               erythropus
## 482       crocidura                  russula
## 483           sorex                 arcticus
## 484           sorex                 cinereus
## 485           sorex                coronatus
## 486           sorex              gracillimus
## 487           sorex             unguiculatus
## 488       condylura                 cristata
## 489        scalopus                aquaticus
## 490           talpa                 europaea
## 491           talpa                   romana
## 492       uromastyx                aegyptius
## 493       carphopis                   vermis
## 494       carphopis                  viridis
## 495         coluber              constrictor
## 496         coluber constrictor flaviventris
## 497       diadophis                punctatus
## 498      drymarchon                  couperi
## 499          elaphe           guttata emoryi
## 500          elaphe                 obsoleta
## 501       heterodon              platirhinos
## 502       hierophis             viridiflavus
## 503    lampropeltis            getula getula
## 504    lampropeltis               triangulum
## 505     masticophis                flagellum
## 506          natrix                   natrix
## 507         nerodia            erythrogaster
## 508         nerodia                 sipeodon
## 509      oocatochus             rufodorsatus
## 510       pituophis                catenifer
## 511       pituophis             melanoleucus
## 512      thamnophis                  butleri
## 513      thamnophis                    gigal
## 514         zamenis              longissimus
## 515   hoplocephalus              bungaroides
## 516        notechis                 scutatus
## 517      pseudechis             porphyriacus
## 518      conolophus                 pallidus
## 519         cyclura                  cyclura
## 520         cyclura                   lewisi
## 521         cyclura                  pinguis
## 522      sauromalus                 hispidua
## 523      sauromalus                   obesus
## 524     dipsosaurus                 dorsalis
## 525        gallotia                  galloti
## 526      phymaturus              flagellifer
## 527         morelia        spilota imbricata
## 528         egernia                    major
## 529     agkistrodon               contortrix
## 530     agkistrodon              piscivorous
## 531           bitis               schneideri
## 532        bothrops                    asper
## 533        crotalus                    atrox
## 534        crotalus                 cerastes
## 535        crotalus                 horridus
## 536        crotalus                 molossus
## 537        crotalus        oreganus concolor
## 538        crotalus                   pricei
## 539        crotalus               scutulatus
## 540        crotalus                   tigris
## 541        gloydius              shedaoensis
## 542     montivipera                   raddei
## 543          vipera                 latastei
## 544       chelodina              longicollis
## 545     mesoclemmys                    dahli
## 546        chelydra               serpentina
## 547       chrysemys          picta marginata
## 548     deirochelys              reticularia
## 549       emydoidea               blandingii
## 550            emys              orbicularis
## 551       graptemys            flavimaculata
## 552       terrapene                   ornata
## 553        mauremys                  leprosa
## 554     kinosternon                rubrubrum
## 555    sternotherus           minor peltifer
## 556    sternotherus                 odoratus
## 557      geochelone               carbonaria
## 558        gopherus                agassizii
## 559        gopherus               polyphemus
## 560     indotestudo             travancorica
## 561         kinixys                   spekii
## 562        manouria                 impressa
## 563     psammobates                tentorius
## 564    stigmochelys                 pardalis
## 565         testudo                   graeca
## 566         testudo                 hermanii
## 567         testudo               horsfieldi
## 568         testudo               kleinmanni
## 569         apalone                spinifera
```

```r
descending <- deer[order(-homerange$log10.mass),]

descending
```

```
##       family homerange.mean.mass.g homerange.log10.mass  homerange.family
## 402 cervidae            4000000.08           6.60206000      elephantidae
## 403 cervidae            4000000.08           6.60206000      elephantidae
## 399 cervidae            2249986.95           6.35218000    rhinocerotidae
## 288 cervidae            1339985.19           6.12710000        giraffidae
## 400 cervidae            1050001.69           6.02119000    rhinocerotidae
## 273 cervidae             635038.42           5.80280000           bovidae
## 260 cervidae             629999.20           5.79934000           bovidae
## 268 cervidae             629999.20           5.79934000           bovidae
## 261 cervidae             628000.52           5.79796000           bovidae
## 398 cervidae             427996.29           5.63144000           equidae
## 276 cervidae             307227.44           5.48746000          cervidae
## 279 cervidae             234757.78           5.37061999          cervidae
## 289 cervidae             230001.15           5.36173001        giraffidae
## 275 cervidae             197314.95           5.29515999           bovidae
## 259 cervidae             167498.14           5.22400999           bovidae
## 258 cervidae             136000.34           5.13353999           bovidae
## 269 cervidae             113998.73           5.05690001           bovidae
## 320 cervidae             112000.51           5.04922000           felidae
## 287 cervidae             102058.69           5.00884999          cervidae
## 347 cervidae              97750.73           4.99012001           ursidae
## 270 cervidae              90719.36           4.95769998           bovidae
## 322 cervidae              89999.48           4.95424000           felidae
## 252 cervidae              88250.00           4.94571471     struthionidae
## 284 cervidae              87884.04           4.94391001          cervidae
## 346 cervidae              87500.39           4.94200999           ursidae
## 318 cervidae              84040.77           4.92449002           felidae
## 290 cervidae              80513.74           4.90587000            suidae
## 281 cervidae              71449.63           4.85399998          cervidae
## 263 cervidae              69499.23           4.84197999           bovidae
## 257 cervidae              63503.84           4.80279999           bovidae
## 277 cervidae              62823.19           4.79811998          cervidae
## 274 cervidae              54431.46           4.73584998           bovidae
## 283 cervidae              53864.17           4.73129997          cervidae
## 262 cervidae              50999.98           4.70757001           bovidae
## 305 cervidae              50000.00           4.69897000           felidae
## 319 cervidae              48374.89           4.68461999           felidae
## 364 cervidae              46249.82           4.66511005      macropodidae
## 256 cervidae              46099.90           4.66369998    antilocapridae
## 285 cervidae              35000.16           4.54407003          cervidae
## 291 cervidae              34700.04           4.54032997       tayassuidae
## 272 cervidae              30999.88           4.49136001           bovidae
## 323 cervidae              30500.01           4.48429998           felidae
## 314 cervidae              29999.91           4.47711995           felidae
## 280 cervidae              29450.32           4.46909002          cervidae
## 297 cervidae              27749.81           4.44326001           canidae
## 358 cervidae              27250.22           4.43537001      macropodidae
## 375 cervidae              25999.80           4.41497001        vombatidae
## 374 cervidae              25499.99           4.40654001        vombatidae
## 241 cervidae              25000.00           4.39794001           rheidae
## 278 cervidae              24050.27           4.38111996          cervidae
## 292 cervidae              23205.98           4.36559991       tayassuidae
## 360 cervidae              22124.83           4.34487994      macropodidae
## 332 cervidae              21545.67           4.33336000        mustelidae
## 266 cervidae              21474.84           4.33192994           bovidae
## 362 cervidae              21250.05           4.32735996      macropodidae
## 265 cervidae              20411.74           4.30988003           bovidae
## 293 cervidae              20250.23           4.30642996       tayassuidae
## 264 cervidae              18143.87           4.25872993           bovidae
## 446 cervidae              17000.04           4.23044994       hystricidae
## 363 cervidae              16850.00           4.22659990      macropodidae
## 242 cervidae              15000.00           4.17609126           rheidae
## 372 cervidae              14999.96           4.17609010      macropodidae
## 447 cervidae              14999.96           4.17609010       hystricidae
## 282 cervidae              13499.88           4.13032991          cervidae
## 306 cervidae              12999.90           4.11394001           felidae
## 312 cervidae              11999.97           4.07918016           felidae
## 316 cervidae              11339.92           4.05460999           felidae
## 271 cervidae              11300.04           4.05307998           bovidae
## 359 cervidae              11249.93           4.05114982      macropodidae
## 315 cervidae              11049.94           4.04335992           felidae
## 294 cervidae              10850.01           4.03543014        tragulidae
## 564 cervidae              10600.00           4.02530586      testudinidae
## 361 cervidae              10375.05           4.01599020      macropodidae
## 313 cervidae              10205.87           4.00885003           felidae
## 329 cervidae              10000.00           4.00000000          hyanidae
## 310 cervidae               9989.64           3.99954984           felidae
## 304 cervidae               9549.93           3.98000019        eupleridae
## 344 cervidae               8618.27           3.93542010        mustelidae
## 437 cervidae               8618.27           3.93542010    erethizontidae
## 350 cervidae               8000.00           3.90308999        viverridae
## 412 cervidae               8000.00           3.90308999          caviidae
## 286 cervidae               7499.98           3.87506011          cervidae
## 298 cervidae               7370.04           3.86746985           canidae
## 518 cervidae               7000.00           3.84509804         iguanidae
## 309 cervidae               6803.93           3.83275984           felidae
## 357 cervidae               6649.97           3.82281969      macropodidae
## 557 cervidae               6166.60           3.79004578      testudinidae
## 295 cervidae               5399.95           3.73238974         ailuridae
## 371 cervidae               5399.95           3.73238974      macropodidae
## 383 cervidae               5250.01           3.72016013         leporidae
## 413 cervidae               5190.03           3.71516987     chinchillidae
## 369 cervidae               5000.00           3.69897000      macropodidae
## 296 cervidae               4989.53           3.69805964           canidae
## 308 cervidae               4825.03           3.68350002           felidae
## 370 cervidae               4649.97           3.66745015      macropodidae
## 365 cervidae               4646.01           3.66708014      macropodidae
## 267 cervidae               4600.02           3.66275972           bovidae
## 300 cervidae               4499.97           3.65320962           canidae
## 330 cervidae               4466.22           3.64994011        mustelidae
## 436 cervidae               4250.01           3.62838995    erethizontidae
## 546 cervidae               4250.00           3.62838893       chelydridae
## 521 cervidae               4223.33           3.62565502         iguanidae
## 380 cervidae               4050.05           3.60746038         leporidae
## 299 cervidae               3989.97           3.60096963           canidae
## 401 cervidae               3899.96           3.59106015      bradypodidae
## 519 cervidae               3780.00           3.57749180         iguanidae
## 311 cervidae               3628.77           3.55975944           felidae
## 324 cervidae               3599.98           3.55630009       herpestidae
## 328 cervidae               3599.98           3.55630009       herpestidae
## 317 cervidae               3589.96           3.55508961           felidae
## 382 cervidae               3549.11           3.55011946         leporidae
## 6   cervidae               3525.00           3.54715912          esocidae
## 461 cervidae               3401.97           3.53173048         sciuridae
## 462 cervidae               3401.97           3.53173048         sciuridae
## 307 cervidae               3394.53           3.53077965           felidae
## 301 cervidae               3249.97           3.51187935           canidae
## 88  cervidae               3247.34           3.51152776        serranidae
## 520 cervidae               3200.00           3.50514998         iguanidae
## 336 cervidae               3175.19           3.50176972        mustelidae
## 384 cervidae               3129.97           3.49554017         leporidae
## 85  cervidae               3081.08           3.48870297        serranidae
## 114 cervidae               3000.00           3.47712126      accipitridae
## 562 cervidae               3000.00           3.47712126      testudinidae
## 569 cervidae               2982.33           3.47455570      trionychidae
## 156 cervidae               2936.00           3.46775605       phasianidae
## 345 cervidae               2887.62           3.46054004       procyonidae
## 373 cervidae               2875.01           3.45863936     phalangeridae
## 385 cervidae               2825.01           3.45101999         leporidae
## 327 cervidae               2810.00           3.44870632       herpestidae
## 352 cervidae               2810.00           3.44870632        dasyuridae
## 448 cervidae               2749.98           3.43932954       hystridicae
## 321 cervidae               2500.00           3.39794001           felidae
## 492 cervidae               2500.00           3.39794001          agamidae
## 335 cervidae               2475.03           3.39358047        mustelidae
## 84  cervidae               2362.06           3.37329093        serranidae
## 121 cervidae               2320.00           3.36548799       apterygidae
## 381 cervidae               2267.98           3.35563922         leporidae
## 119 cervidae               2203.00           3.34301450      accipitridae
## 246 cervidae               2191.00           3.34064238         strigidae
## 83  cervidae               2181.15           3.33868553        serranidae
## 51  cervidae               2167.70           3.33599918        lutjanidae
## 349 cervidae               2150.01           3.33244048        viverridae
## 387 cervidae               2150.01           3.33244048         leporidae
## 19  cervidae               2125.00           3.32735893     centrarchidae
## 302 cervidae               2089.30           3.32000080           canidae
## 117 cervidae               2049.00           3.31154196      accipitridae
## 558 cervidae               2000.00           3.30103000      testudinidae
## 91  cervidae               1992.23           3.29933948        serranidae
## 240 cervidae               1941.00           3.28802554       strigopidae
## 56  cervidae               1940.27           3.28786217     malacanthidae
## 249 cervidae               1920.00           3.28330123         strigidae
## 367 cervidae               1899.98           3.27874903        potoroidae
## 334 cervidae               1799.99           3.25527009        mustelidae
## 92  cervidae               1753.40           3.24388100        serranidae
## 331 cervidae               1750.01           3.24304053        mustelidae
## 348 cervidae               1750.01           3.24304053        viverridae
## 151 cervidae               1750.00           3.24303805       phasianidae
## 116 cervidae               1699.00           3.23019338      accipitridae
## 70  cervidae               1668.11           3.22222469          scaridae
## 366 cervidae               1660.01           3.22011070        potoroidae
## 37  cervidae               1644.90           3.21613950          labridae
## 386 cervidae               1600.00           3.20411998         leporidae
## 26  cervidae               1551.13           3.19064820  cheilodactylidae
## 177 cervidae               1550.00           3.19033170      fringillidae
## 90  cervidae               1527.64           3.18402102        serranidae
## 566 cervidae               1522.00           3.18241465      testudinidae
## 247 cervidae               1510.00           3.17897695         strigidae
## 46  cervidae               1484.14           3.17147487          labridae
## 29  cervidae               1450.23           3.16143689          cottidae
## 168 cervidae               1410.00           3.14921911          corvidae
## 379 cervidae               1360.79           3.13379111         leporidae
## 388 cervidae               1360.79           3.13379111         leporidae
## 429 cervidae               1360.79           3.13379111        cricetidae
## 389 cervidae               1349.99           3.13033055         leporidae
## 107 cervidae               1341.25           3.12750973        sebastidae
## 368 cervidae               1299.99           3.11394001   pseudocheiridae
## 549 cervidae               1294.00           3.11193428          emydidae
## 527 cervidae               1226.85           3.08879147        pythonidae
## 453 cervidae               1185.00           3.07371835        nesomyidae
## 55  cervidae               1176.86           3.07072480        lutjanidae
## 155 cervidae               1139.00           3.05652372       phasianidae
## 551 cervidae               1135.00           3.05499586          emydidae
## 18  cervidae               1134.00           3.05461305     centrarchidae
## 405 cervidae               1133.99           3.05460923     aplodontiidae
## 137 cervidae               1126.00           3.05153839      accipitridae
## 143 cervidae               1125.00           3.05115252        falconidae
## 105 cervidae               1100.33           3.04152295        sebastidae
## 351 cervidae               1096.48           3.04000071        dasyuridae
## 35  cervidae               1086.71           3.03611366        kyphosidae
## 152 cervidae               1050.00           3.02118930       phasianidae
## 142 cervidae               1033.70           3.01439452      accipitridae
## 535 cervidae               1020.00           3.00860017         viperidae
## 567 cervidae               1018.00           3.00774778      testudinidae
## 511 cervidae               1004.00           3.00173371        colubridae
## 134 cervidae                978.00           2.99033886      accipitridae
## 118 cervidae                975.00           2.98900462      accipitridae
## 139 cervidae                971.00           2.98721923      accipitridae
## 467 cervidae                952.99           2.97908834         sciuridae
## 96  cervidae                934.20           2.97043986          sparidae
## 522 cervidae                927.00           2.96707973         iguanidae
## 341 cervidae                912.01           2.95999960        mustelidae
## 157 cervidae                900.00           2.95424251       phasianidae
## 231 cervidae                900.00           2.95424251          ardeidae
## 1   cervidae                887.00           2.94792362       anguillidae
## 333 cervidae                883.49           2.94620164        mustelidae
## 115 cervidae                846.00           2.92737036      accipitridae
## 532 cervidae                826.23           2.91710096         viperidae
## 339 cervidae                808.74           2.90780892        mustelidae
## 135 cervidae                807.00           2.90687353      accipitridae
## 376 cervidae                800.00           2.90308999       erinaceidae
## 472 cervidae                794.49           2.90008843         sciuridae
## 464 cervidae                793.80           2.89971109         sciuridae
## 147 cervidae                781.50           2.89292898        falconidae
## 109 cervidae                780.54           2.89239516        sebastidae
## 397 cervidae                775.00           2.88930170       peramelidae
## 10  cervidae                772.16           2.88770730      acanthuridae
## 475 cervidae                748.43           2.87415119         sciuridae
## 108 cervidae                737.78           2.86792688        sebastidae
## 7   cervidae                737.36           2.86767957           gadidae
## 95  cervidae                731.58           2.86426182          sparidae
## 13  cervidae                726.18           2.86104428        carangidae
## 469 cervidae                725.75           2.86078704         sciuridae
## 146 cervidae                721.00           2.85793527        falconidae
## 553 cervidae                720.60           2.85769426       geoemydidae
## 120 cervidae                719.00           2.85672889          anatidae
## 77  cervidae                697.00           2.84323278        serranidae
## 544 cervidae                691.00           2.83947805          chelidae
## 145 cervidae                675.00           2.82930377        falconidae
## 500 cervidae                642.80           2.80807587        colubridae
## 528 cervidae                638.00           2.80482068         scincidae
## 471 cervidae                637.87           2.80473218         sciuridae
## 138 cervidae                626.00           2.79657433      accipitridae
## 144 cervidae                625.00           2.79588002        falconidae
## 406 cervidae                625.00           2.79588002      bathyergidae
## 253 cervidae                622.00           2.79379038         tinamidae
## 153 cervidae                620.00           2.79239169       phasianidae
## 325 cervidae                620.00           2.79239169       herpestidae
## 561 cervidae                620.00           2.79239169      testudinidae
## 545 cervidae                595.00           2.77451697          chelidae
## 548 cervidae                588.00           2.76937733          emydidae
## 470 cervidae                578.34           2.76218323         sciuridae
## 340 cervidae                562.34           2.74999898        mustelidae
## 2   cervidae                562.00           2.74973632      catostomidae
## 495 cervidae                556.15           2.74519194        colubridae
## 394 cervidae                535.20           2.72851611   macroscelididae
## 505 cervidae                534.50           2.72794771        colubridae
## 465 cervidae                532.98           2.72671091         sciuridae
## 343 cervidae                524.81           2.72000210        mustelidae
## 75  cervidae                523.80           2.71916549          scaridae
## 76  cervidae                521.16           2.71697108          scaridae
## 123 cervidae                521.00           2.71683772    haematopodidae
## 140 cervidae                521.00           2.71683772      accipitridae
## 250 cervidae                519.00           2.71516736         strigidae
## 158 cervidae                506.00           2.70415052          rallidae
## 481 cervidae                502.00           2.70070372         sciuridae
## 563 cervidae                500.00           2.69897000      testudinidae
## 517 cervidae                479.00           2.68033551          elapidae
## 80  cervidae                476.00           2.67760695        serranidae
## 133 cervidae                469.00           2.67117284      accipitridae
## 550 cervidae                462.00           2.66464198          emydidae
## 498 cervidae                450.00           2.65321251        colubridae
## 8   cervidae                448.61           2.65186895           gadidae
## 254 cervidae                436.52           2.64000415   chrysochloridae
## 78  cervidae                436.24           2.63972549        serranidae
## 132 cervidae                433.00           2.63648790         cuculidae
## 435 cervidae                428.00           2.63144377        echimyidae
## 20  cervidae                423.00           2.62634037     centrarchidae
## 536 cervidae                414.00           2.61700034         viperidae
## 150 cervidae                410.00           2.61278386       phasianidae
## 101 cervidae                402.00           2.60422605        salmonidae
## 565 cervidae                400.00           2.60205999      testudinidae
## 82  cervidae                398.51           2.60043922        serranidae
## 425 cervidae                395.48           2.59712452        cricetidae
## 396 cervidae                390.50           2.59162104       peramelidae
## 74  cervidae                388.84           2.58977093          scaridae
## 154 cervidae                381.50           2.58149454       phasianidae
## 89  cervidae                378.42           2.57797408        serranidae
## 463 cervidae                375.00           2.57403127         sciuridae
## 510 cervidae                375.00           2.57403127        colubridae
## 44  cervidae                362.30           2.55906833          labridae
## 547 cervidae                354.50           2.54961624          emydidae
## 434 cervidae                350.00           2.54406804        echimyidae
## 378 cervidae                340.20           2.53173431         leporidae
## 559 cervidae                335.00           2.52504481      testudinidae
## 516 cervidae                330.00           2.51851394          elapidae
## 468 cervidae                327.50           2.51521130         sciuridae
## 533 cervidae                319.90           2.50501424         viperidae
## 503 cervidae                315.72           2.49930209        colubridae
## 141 cervidae                315.50           2.49899936      accipitridae
## 507 cervidae                313.24           2.49587722        colubridae
## 81  cervidae                312.30           2.49457198        serranidae
## 426 cervidae                308.30           2.48897353        cricetidae
## 513 cervidae                306.00           2.48572143        colubridae
## 131 cervidae                300.00           2.47712126         cuculidae
## 58  cervidae                297.72           2.47380801          mullidae
## 377 cervidae                296.00           2.47129171       erinaceidae
## 72  cervidae                289.40           2.46149853          scaridae
## 251 cervidae                285.00           2.45484486         tytonidae
## 326 cervidae                281.84           2.45000263       herpestidae
## 539 cervidae                280.30           2.44762310         viperidae
## 233 cervidae                277.37           2.44305949           picidae
## 337 cervidae                270.54           2.43223149        mustelidae
## 160 cervidae                266.00           2.42488164          rallidae
## 466 cervidae                264.30           2.42209716         sciuridae
## 395 cervidae                257.00           2.40993312    tachyglossidae
## 499 cervidae                256.70           2.40942587        colubridae
## 428 cervidae                255.15           2.40679557        cricetidae
## 244 cervidae                252.00           2.40140054         strigidae
## 73  cervidae                250.64           2.39905038          scaridae
## 514 cervidae                249.30           2.39672228        colubridae
## 409 cervidae                242.00           2.38381537      bathyergidae
## 50  cervidae                236.76           2.37430833       lethrinidae
## 540 cervidae                234.70           2.37051309         viperidae
## 502 cervidae                234.10           2.36940141        colubridae
## 560 cervidae                232.00           2.36548799      testudinidae
## 529 cervidae                231.12           2.36383753         viperidae
## 480 cervidae                223.96           2.35017046         sciuridae
## 568 cervidae                222.00           2.34635297      testudinidae
## 454 cervidae                215.60           2.33364876        nesomyidae
## 552 cervidae                211.00           2.32428246          emydidae
## 523 cervidae                210.00           2.32221930         iguanidae
## 110 cervidae                202.00           2.30535137       ictaluridae
## 393 cervidae                201.00           2.30319606   macroscelididae
## 149 cervidae                200.00           2.30103000        falconidae
## 474 cervidae                198.45           2.29765110         sciuridae
## 541 cervidae                196.81           2.29404716         viperidae
## 14  cervidae                196.00           2.29225607     centrarchidae
## 59  cervidae                191.55           2.28228216          mullidae
## 508 cervidae                190.55           2.28000895        colubridae
## 556 cervidae                190.00           2.27875360     kinosternidae
## 530 cervidae                188.00           2.27415785         viperidae
## 239 cervidae                186.67           2.27107453           picidae
## 71  cervidae                171.42           2.23406149          scaridae
## 455 cervidae                165.45           2.21866677        nesomyidae
## 159 cervidae                165.00           2.21748394          rallidae
## 504 cervidae                165.00           2.21748394        colubridae
## 555 cervidae                164.00           2.21484385     kinosternidae
## 542 cervidae                162.14           2.20989017         viperidae
## 438 cervidae                160.18           2.20460829         geomyidae
## 211 cervidae                158.00           2.19865709 ptilonorhynchidae
## 245 cervidae                156.50           2.19451434         strigidae
## 390 cervidae                155.30           2.19117146       ochotonidae
## 410 cervidae                155.00           2.19033170      bathyergidae
## 554 cervidae                154.70           2.18949031     kinosternidae
## 441 cervidae                153.56           2.18627810      heteromyidae
## 11  cervidae                151.84           2.18138619      acanthuridae
## 129 cervidae                151.50           2.18041263         cuculidae
## 338 cervidae                150.19           2.17664102        mustelidae
## 125 cervidae                150.00           2.17609126        columbidae
## 459 cervidae                148.84           2.17271966         sciuridae
## 407 cervidae                148.00           2.17026172      bathyergidae
## 501 cervidae                147.32           2.16826171        colubridae
## 391 cervidae                146.50           2.16583762       ochotonidae
## 444 cervidae                144.58           2.16010822      heteromyidae
## 496 cervidae                144.50           2.15986785        colubridae
## 136 cervidae                141.00           2.14921911      accipitridae
## 126 cervidae                140.33           2.14715052        columbidae
## 537 cervidae                138.70           2.14207646         viperidae
## 60  cervidae                134.00           2.12710480          percidae
## 36  cervidae                133.38           2.12509071          labridae
## 427 cervidae                132.29           2.12152702        cricetidae
## 169 cervidae                130.00           2.11394335          corvidae
## 130 cervidae                128.00           2.10720997         cuculidae
## 238 cervidae                124.50           2.09516935           picidae
## 452 cervidae                122.00           2.08635983           muridae
## 38  cervidae                119.16           2.07613050          labridae
## 79  cervidae                119.00           2.07554696        serranidae
## 86  cervidae                119.00           2.07554696        serranidae
## 243 cervidae                119.00           2.07554696         strigidae
## 148 cervidae                112.00           2.04921802        falconidae
## 235 cervidae                109.25           2.03842145           picidae
## 9   cervidae                109.04           2.03758584      acanthuridae
## 99  cervidae                109.00           2.03742650        salmonidae
## 534 cervidae                106.70           2.02816442         viperidae
## 473 cervidae                106.00           2.02530586         sciuridae
## 489 cervidae                103.50           2.01494035          talpidae
## 47  cervidae                103.47           2.01481445          labridae
## 127 cervidae                103.00           2.01283723        coraciidae
## 458 cervidae                102.50           2.01072387         sciuridae
## 456 cervidae                100.86           2.00371896      octodontidae
## 106 cervidae                 99.25           1.99673052        sebastidae
## 97  cervidae                 99.20           1.99651167        salmonidae
## 543 cervidae                 97.40           1.98855896         viperidae
## 490 cervidae                 96.50           1.98452731          talpidae
## 457 cervidae                 95.80           1.98136551         sciuridae
## 45  cervidae                 95.54           1.98018524          labridae
## 61  cervidae                 94.58           1.97579931     pomacanthidae
## 416 cervidae                 92.14           1.96444821        cricetidae
## 180 cervidae                 89.00           1.94939001         icteridae
## 181 cervidae                 89.00           1.94939001         icteridae
## 451 cervidae                 88.30           1.94596070           muridae
## 342 cervidae                 88.10           1.94497591        mustelidae
## 57  cervidae                 85.99           1.93444795         moronidae
## 423 cervidae                 85.50           1.93196612        cricetidae
## 49  cervidae                 84.06           1.92458939          labridae
## 15  cervidae                 82.00           1.91381385     centrarchidae
## 491 cervidae                 81.42           1.91073110          talpidae
## 16  cervidae                 79.00           1.89762709     centrarchidae
## 506 cervidae                 78.50           1.89486966        colubridae
## 25  cervidae                 78.12           1.89276223    chaetodontidae
## 445 cervidae                 76.20           1.88195497      heteromyidae
## 94  cervidae                 75.93           1.88041340        serranidae
## 34  cervidae                 71.23           1.85266294          gobiidae
## 418 cervidae                 70.87           1.85046243        cricetidae
## 24  cervidae                 70.45           1.84788100    chaetodontidae
## 439 cervidae                 68.80           1.83758844          gliridae
## 538 cervidae                 67.20           1.82736927         viperidae
## 128 cervidae                 67.00           1.82607480          upupidae
## 232 cervidae                 67.00           1.82607480          ardeidae
## 415 cervidae                 66.30           1.82151353        cricetidae
## 237 cervidae                 65.65           1.81723473           picidae
## 408 cervidae                 65.00           1.81291336      bathyergidae
## 460 cervidae                 64.50           1.80955972         sciuridae
## 509 cervidae                 62.50           1.79588002        colubridae
## 248 cervidae                 61.32           1.78760215         strigidae
## 23  cervidae                 60.54           1.78204242    chaetodontidae
## 303 cervidae                 60.00           1.77815125         dipodidae
## 236 cervidae                 59.00           1.77085201           picidae
## 524 cervidae                 59.00           1.77085201        lacertilia
## 392 cervidae                 58.00           1.76342799   macroscelididae
## 419 cervidae                 56.70           1.75358306        cricetidae
## 443 cervidae                 56.70           1.75358306      heteromyidae
## 54  cervidae                 56.27           1.75027691        lutjanidae
## 52  cervidae                 56.04           1.74849813        lutjanidae
## 433 cervidae                 52.50           1.72015930         dipodidae
## 187 cervidae                 50.10           1.69983773           mimidae
## 53  cervidae                 50.00           1.69897000        lutjanidae
## 421 cervidae                 49.61           1.69556923        cricetidae
## 515 cervidae                 48.79           1.68833082          elapidae
## 184 cervidae                 48.10           1.68214508          laniidae
## 122 cervidae                 48.00           1.68124124     caprimulgidae
## 488 cervidae                 47.86           1.67997269          talpidae
## 124 cervidae                 47.70           1.67851838        columbidae
## 98  cervidae                 47.00           1.67209786        salmonidae
## 173 cervidae                 46.30           1.66558099       emberizidae
## 67  cervidae                 45.30           1.65609820     pomacentridae
## 174 cervidae                 44.70           1.65030752       emberizidae
## 185 cervidae                 44.22           1.64561874          laniidae
## 477 cervidae                 42.52           1.62859326         sciuridae
## 478 cervidae                 42.52           1.62859326         sciuridae
## 479 cervidae                 42.52           1.62859326         sciuridae
## 170 cervidae                 42.00           1.62324929        cotingidae
## 442 cervidae                 41.82           1.62138403      heteromyidae
## 93  cervidae                 41.11           1.61394748        serranidae
## 226 cervidae                 40.40           1.60638137        tyrannidae
## 525 cervidae                 40.00           1.60205999        lacertilia
## 69  cervidae                 39.00           1.59106461     pomacentridae
## 411 cervidae                 39.00           1.59106461      bathyergidae
## 432 cervidae                 38.27           1.58285846        cricetidae
## 234 cervidae                 38.00           1.57978360           picidae
## 417 cervidae                 38.00           1.57978360        cricetidae
## 164 cervidae                 37.70           1.57634135      cardinalidae
## 420 cervidae                 35.44           1.54949371        cricetidae
## 186 cervidae                 35.00           1.54406804          laniidae
## 3   cervidae                 34.00           1.53147892        cyprinidae
## 165 cervidae                 32.80           1.51587384      cardinalidae
## 22  cervidae                 31.88           1.50351831    chaetodontidae
## 449 cervidae                 31.60           1.49968708           muridae
## 440 cervidae                 31.00           1.49136169          gliridae
## 222 cervidae                 30.80           1.48855072          turdidae
## 163 cervidae                 30.00           1.47712125         alaudidae
## 183 cervidae                 30.00           1.47712125          laniidae
## 424 cervidae                 30.00           1.47712125        cricetidae
## 422 cervidae                 29.50           1.46982202        cricetidae
## 356 cervidae                 29.00           1.46239800       didelphidae
## 63  cervidae                 28.41           1.45347123     pomacentridae
## 526 cervidae                 28.00           1.44715803       liolaemidae
## 21  cervidae                 27.63           1.44138089    chaetodontidae
## 430 cervidae                 27.54           1.43996394        cricetidae
## 354 cervidae                 27.50           1.43933269        dasyuridae
## 182 cervidae                 27.00           1.43136376          incertae
## 476 cervidae                 26.93           1.43023635         sciuridae
## 191 cervidae                 25.20           1.40140054      muscicapidae
## 87  cervidae                 24.87           1.39567578        serranidae
## 178 cervidae                 23.25           1.36642296      fringillidae
## 255 cervidae                 23.00           1.36172784   chrysochloridae
## 353 cervidae                 23.00           1.36172784        dasyuridae
## 414 cervidae                 22.57           1.35353156        cricetidae
## 404 cervidae                 22.00           1.34242268        cricetidae
## 512 cervidae                 21.51           1.33264041        colubridae
## 188 cervidae                 21.22           1.32674538      motacillidae
## 450 cervidae                 21.20           1.32633586           muridae
## 355 cervidae                 19.50           1.29003461       didelphidae
## 201 cervidae                 18.90           1.27646180         parulidae
## 219 cervidae                 18.50           1.26717173     troglodytidae
## 214 cervidae                 18.30           1.26245109         stittidae
## 175 cervidae                 18.10           1.25767857       emberizidae
## 230 cervidae                 17.60           1.24551267        vireonidae
## 189 cervidae                 17.50           1.24303805      motacillidae
## 531 cervidae                 16.95           1.22916970         viperidae
## 171 cervidae                 16.70           1.22271647       emberizidae
## 196 cervidae                 16.60           1.22010809           paridae
## 193 cervidae                 16.48           1.21695721      muscicapidae
## 200 cervidae                 16.10           1.20682588         parulidae
## 17  cervidae                 16.00           1.20411998     centrarchidae
## 192 cervidae                 15.21           1.18212921      muscicapidae
## 215 cervidae                 14.80           1.17026172          sylvidae
## 172 cervidae                 14.30           1.15533604       emberizidae
## 487 cervidae                 14.13           1.15014216         soricidae
## 203 cervidae                 14.00           1.14612804         parulidae
## 223 cervidae                 13.80           1.13987909        tyrannidae
## 190 cervidae                 12.80           1.10720997      muscicapidae
## 225 cervidae                 12.30           1.08990511        tyrannidae
## 176 cervidae                 12.20           1.08635983       emberizidae
## 229 cervidae                 11.40           1.05690485        vireonidae
## 198 cervidae                 11.30           1.05307844         parulidae
## 220 cervidae                 11.20           1.04921802     troglodytidae
## 431 cervidae                 11.05           1.04336228        cricetidae
## 161 cervidae                 11.00           1.04139268    acrocephalisae
## 194 cervidae                 11.00           1.04139268           paridae
## 197 cervidae                 11.00           1.04139268           paridae
## 218 cervidae                 11.00           1.04139268     troglodytidae
## 179 cervidae                 10.70           1.02938378      fringillidae
## 216 cervidae                 10.50           1.02118930         sylviidae
## 66  cervidae                 10.49           1.02077549     pomacentridae
## 28  cervidae                 10.45           1.01911629       cirrhitidae
## 195 cervidae                 10.10           1.00432137           paridae
## 228 cervidae                 10.00           1.00000000        vireonidae
## 482 cervidae                 10.00           1.00000000         soricidae
## 224 cervidae                  9.90           0.99563519        tyrannidae
## 167 cervidae                  9.80           0.99122608      cisticolidae
## 199 cervidae                  9.80           0.99122608         parulidae
## 202 cervidae                  9.70           0.98677173         parulidae
## 205 cervidae                  9.60           0.98227123         parulidae
## 41  cervidae                  9.55           0.98000337          labridae
## 111 cervidae                  9.55           0.98000337      syngnathidae
## 206 cervidae                  9.50           0.97772360         parulidae
## 221 cervidae                  9.50           0.97772360     troglodytidae
## 485 cervidae                  9.33           0.96988164         soricidae
## 209 cervidae                  9.30           0.96848295         parulidae
## 64  cervidae                  9.19           0.96331551     pomacentridae
## 207 cervidae                  9.00           0.95424251         parulidae
## 208 cervidae                  9.00           0.95424251         parulidae
## 497 cervidae                  9.00           0.95424251        colubridae
## 113 cervidae                  8.96           0.95230801    tetraodontidae
## 217 cervidae                  8.80           0.94448267         sylviidae
## 166 cervidae                  8.77           0.94299959         certhidae
## 204 cervidae                  8.60           0.93449845         parulidae
## 227 cervidae                  8.50           0.92941893        vireonidae
## 483 cervidae                  8.13           0.91009055         soricidae
## 162 cervidae                  8.00           0.90308999      aegithalidae
## 210 cervidae                  7.50           0.87506126    phylloscopidae
## 39  cervidae                  7.17           0.85551916          labridae
## 42  cervidae                  7.03           0.84695533          labridae
## 68  cervidae                  6.64           0.82216808     pomacentridae
## 40  cervidae                  6.57           0.81756537          labridae
## 12  cervidae                  6.20           0.79239169         blennidae
## 212 cervidae                  5.30           0.72427587         regulidae
## 213 cervidae                  5.15           0.71180723         regulidae
## 102 cervidae                  5.00           0.69897000          cottidae
## 104 cervidae                  5.00           0.69897000          cottidae
## 486 cervidae                  4.37           0.64048144         soricidae
## 484 cervidae                  4.17           0.62013606         soricidae
## 4   cervidae                  4.00           0.60205999        cyprinidae
## 5   cervidae                  4.00           0.60205999        cyprinidae
## 33  cervidae                  4.00           0.60205999          gobiidae
## 65  cervidae                  3.96           0.59769519     pomacentridae
## 494 cervidae                  3.65           0.56229286        colubridae
## 43  cervidae                  3.49           0.54282543          labridae
## 493 cervidae                  3.46           0.53907610        colubridae
## 48  cervidae                  3.12           0.49415459          labridae
## 103 cervidae                  3.00           0.47712126          cottidae
## 30  cervidae                  2.70           0.43136376          gobiidae
## 62  cervidae                  2.50           0.39794001     pomacanthidae
## 100 cervidae                  2.00           0.30103000        salmonidae
## 112 cervidae                  1.22           0.08635983      syngnathidae
## 27  cervidae                  0.45          -0.34678749       cirrhitidae
## 31  cervidae                  0.31          -0.50863831          gobiidae
## 32  cervidae                  0.22          -0.65757732          gobiidae
##     homerange.genus        homerange.species
## 402         elephas                  maximus
## 403       loxodonta                 africana
## 399   ceratotherium                    simum
## 288         giraffa           camelopardalis
## 400         diceros                 bicornis
## 273     taurotragus                     oryx
## 260           bison                    bison
## 268        oreamnos               americanus
## 261           bison                  bonasus
## 398           equus                 caballus
## 276           alces                    alces
## 279          cervus                  elaphus
## 289          okapia                johnstoni
## 275     tragelaphus             strepsiceros
## 259      ammotragus                   lervia
## 258      alcelaphus               buselaphus
## 269            ovis                    ammon
## 320        panthera                   tigris
## 287        rangifer                 tarandus
## 347        melursus                  ursinus
## 270            ovis               canadensis
## 322            puma                 concolor
## 252        struthio                  camelus
## 284      odocoileus              virginianus
## 346      ailuropoda              melanoleuca
## 318        panthera                     onca
## 290    phacochoerus              aethiopicus
## 281            dama                     dama
## 263           capra                pyrenaica
## 257       aepyceros                 melampus
## 277            axis                     axis
## 274     tragelaphus                 scriptus
## 283      odocoileus                 hemionus
## 262           capra                   hircus
## 305        acinonyx                  jubatus
## 319        panthera                   pardus
## 364        macropus                    rufus
## 256     antilocapra                americana
## 285      ozotoceros              bezoarticus
## 291       catagonus                  wagneri
## 272       rupicapra                rupicapra
## 323           uncia                    uncia
## 314            lynx                     lynx
## 280          cervus                   nippon
## 297           canis                 simensis
## 358        macropus              antilopinus
## 375        vombatus                  ursinus
## 374     lasiorhinus                 krefftii
## 241            rhea                americana
## 278       capreolus                capreolus
## 292          pecari                   tajacu
## 360        macropus              fuliginosus
## 332            gulo                     gulo
## 266         gazella                  gazella
## 362        macropus                 robustus
## 265     cephalophus                 dorsalis
## 293         tayassu                   pecari
## 264     cephalophus               callipygus
## 446         hystrix         africaeaustralis
## 363        macropus              rufogriseus
## 242            rhea                  pennata
## 372        wallabia                  bicolor
## 447         hystrix                   indica
## 282       muntiacus                  reevesi
## 306         caracal                  caracal
## 312     leptailurus                   serval
## 316            lynx                    rufus
## 271      raphicerus               campestris
## 359        macropus                 dorsalis
## 315            lynx                 pardinus
## 294      hyemoschus                aquaticus
## 564    stigmochelys                 pardalis
## 361        macropus                giganteus
## 313            lynx               canadensis
## 329        proteles                cristatus
## 310       leopardus                 pardalis
## 304    cryptoprocta                    ferox
## 344         taxidea                    taxus
## 437       erethizon                 dorsatum
## 350         viverra                  zibetha
## 412      dolichotus                patagonus
## 286            pudu                     puda
## 298     pseudalopex                 culpaeus
## 518      conolophus                 pallidus
## 309     herpailurus             yagouaroundi
## 357     dendrolagus                lumholtzi
## 557      geochelone               carbonaria
## 295         ailurus                  fulgens
## 371       thylogale                   thetis
## 383           lepus                europaeus
## 413      lagostomus                  maximus
## 369     onychogalea                 fraenata
## 296          alopex                  lagopus
## 308           felis               sylvestris
## 370       thylogale               stigmatica
## 365       petrogale                assimilis
## 267         madoqua                guentheri
## 300          vulpes                  macroti
## 330            eira                  barbata
## 436         coendou              prehensilis
## 546        chelydra               serpentina
## 521         cyclura                  pinguis
## 380           lepus                 arcticus
## 299     pseudalopex                  griseus
## 401        bradypus                torquatus
## 519         cyclura                  cyclura
## 311       leopardus                   wiedii
## 324          atilax              paludinosus
## 328       ichneumia                albicauda
## 317       oncifelis                geoffroyi
## 382           lepus                 capensis
## 6              esox              masquinongy
## 461         marmota             flaviventris
## 462         marmota                    monax
## 307           felis                    catus
## 301          vulpes                 ruppelli
## 88     mycteroperca                   bonaci
## 520         cyclura                   lewisi
## 336          martes                 pennanti
## 384           lepus              nigricollis
## 85      epinephelus                  tauvina
## 114          aquila               chrysaetos
## 562        manouria                 impressa
## 569         apalone                spinifera
## 156          tetrao                urogallus
## 345           potos                   flavus
## 373     trichosurus                vulpecula
## 385           lepus                  timidus
## 327       herpestes               inchneumon
## 352        dasyurus                maculatus
## 448       atherurus                africanus
## 321    prionailurus              bengalensis
## 492       uromastyx                aegyptius
## 335          martes                   martes
## 84      epinephelus                 striatus
## 121         apteryx                australis
## 381           lepus             californicus
## 119        neophron             percnopterus
## 246            bubo                     bubo
## 83      epinephelus                    morio
## 51         lutjanus                   analis
## 349         genetta                  tigrina
## 387      sylvilagus                aquaticus
## 19      micropterus                salmoides
## 302          vulpes                    velox
## 117      hieraaetus                fasciatus
## 558        gopherus                agassizii
## 91     plectropomus                areolatus
## 240        strigops              habroptilus
## 56     caulolatilus                 princeps
## 249          nyctea                scandiaca
## 367        potorous                 longipes
## 334          martes                    foina
## 92     plectropomus                leopardus
## 331        galictis                  vittata
## 348         genetta                  genetta
## 151    centrocercus             urophasianus
## 116       circaetus                 gallicus
## 70        chlorurus              microrhinos
## 366       bettongia                 gaimardi
## 37         chelinus                undulatus
## 386     oryctolagus                cuniculus
## 26   cheilodactylus             spectrabilis
## 177       carduelis                cannabina
## 90       paralabrax                nebulifer
## 566         testudo                 hermanii
## 247            bubo              virginianus
## 46    semicossyphus                  pulcher
## 29  scorpaenichthys               marmoratus
## 168          corvus                    corax
## 379           lepus               americanus
## 388      sylvilagus               floridanus
## 429         ondatra                zibethica
## 389      sylvilagus                palustris
## 107        sebastes                  maliger
## 368     petauroides                   volans
## 549       emydoidea               blandingii
## 527         morelia        spilota imbricata
## 453      hypogeomys                 antimena
## 55          ocyurus                chrysurus
## 155          tetrao                   tetrix
## 551       graptemys            flavimaculata
## 18      micropterus                 dolomieu
## 405      aplodontia                     rufa
## 137           buteo              jamaicensis
## 143        caracara                 cheriway
## 105        sebastes                 caurinus
## 351        dasyurus                geoffroii
## 35         kyphosus                sectatrix
## 152     dendragapus                 obscurus
## 142          milvus                   milvus
## 535        crotalus                 horridus
## 567         testudo               horsfieldi
## 511       pituophis             melanoleucus
## 134       accipiter                 gentilis
## 118      hieraaetus                 pennatus
## 139           buteo                swainsoni
## 467         sciurus                    niger
## 96           sparus                  auratus
## 522      sauromalus                 hispidua
## 341         mustela                 nigripes
## 157     typmanuchus          cupido pinnatus
## 231        botaurus                stellaris
## 1          anguilla                 rostrata
## 333          martes                americana
## 115           buteo                    buteo
## 532        bothrops                    asper
## 339         mustela                     furo
## 135       accipiter                    nisus
## 376       erinaceus                europaeus
## 472    spermophilus                  parryii
## 464         sciurus                   aberti
## 147           falco               peregrinus
## 109        sebastes                 mustinus
## 397         isoodon                 obesulus
## 10             naso                lituratus
## 475    spermophilus               variegatus
## 108        sebastes                 melanops
## 7        pollachius               pollachius
## 95            sarpa                    salpa
## 13           caranx                ignobilis
## 469    spermophilus                 beecheyi
## 146           falco                mexicanus
## 553        mauremys                  leprosa
## 120            anas                 strepera
## 77    cephalopholis                    argus
## 544       chelodina              longicollis
## 145           falco                biarmicus
## 500          elaphe                 obsoleta
## 528         egernia                    major
## 471    spermophilus               franklinii
## 138           buteo                 lineatus
## 144        daptrius               americanus
## 406      bathyergus                  suillus
## 253     nothoprocta                   ornata
## 153         lagopus                  lagopus
## 325        cynictis              penicillata
## 561         kinixys                   spekii
## 545     mesoclemmys                    dahli
## 548     deirochelys              reticularia
## 470    spermophilus              columbianus
## 340         mustela                 lutreola
## 2         moxostoma                poecilura
## 495         coluber              constrictor
## 394     rhynchocyon              chrysopygus
## 505     masticophis                flagellum
## 465         sciurus             carolinensis
## 343         mustela                 sibirica
## 75        sparisoma               rubripinne
## 76        sparisoma                   viride
## 123      haematopus               ostralegus
## 140          circus                  cyaneus
## 250           strix                    aluco
## 158        aramides                    wolfi
## 481           xerus               erythropus
## 563     psammobates                tentorius
## 517      pseudechis             porphyriacus
## 80    cephalopholis                  miniata
## 133       accipiter                 cooperii
## 550            emys              orbicularis
## 498      drymarchon                  couperi
## 8        pollachius                   virens
## 254    chrysospalax               trevelyani
## 78    cephalopholis                cruentata
## 132     neopmorphus               radiolosus
## 435      proechimys             semispinosus
## 20          pomoxis                annularis
## 536        crotalus                 molossus
## 150          bonasa                  bonasia
## 101           salmo                   trutta
## 565         testudo                   graeca
## 82      epinephelus               marginatus
## 425         neotoma                  cinerea
## 396         isoodon                  auratus
## 74        sparisoma             chrysopterum
## 154          perdix                   perdix
## 89       paralabrax               clathratus
## 463       paraxerus                palliatus
## 510       pituophis                catenifer
## 44           labrus                 bergylta
## 547       chrysemys          picta marginata
## 434      proechimys                  cuvieri
## 378     brachylagus               idahoensis
## 559        gopherus               polyphemus
## 516        notechis                 scutatus
## 468         sciurus                 vulgaris
## 533        crotalus                    atrox
## 503    lampropeltis            getula getula
## 141          circus                 pygargus
## 507         nerodia            erythrogaster
## 81      epinephelus                 guttatus
## 426         neotoma                 fuscipes
## 513      thamnophis                    gigal
## 131       geococcyx            californianus
## 58   mulloidichthys            flavolineatus
## 377     hemiechinus                  auritus
## 72           scarus                rivulatus
## 251            tyto                     alba
## 326        helogale                  parvula
## 539        crotalus               scutulatus
## 233       dryocopus                  martius
## 337         mustela                  erminea
## 160          rallus                  elegans
## 466         sciurus                      lis
## 395    tachyoryctes                splendens
## 499          elaphe           guttata emoryi
## 428         neotoma                 micropus
## 244            asio                     otus
## 73        sparisoma             aurofrenatum
## 514         zamenis              longissimus
## 409       georychus                 capensis
## 50        lethrinus                    harak
## 540        crotalus                   tigris
## 502       hierophis             viridiflavus
## 560     indotestudo             travancorica
## 529     agkistrodon               contortrix
## 480    tamiasciurus               hudsonicus
## 568         testudo               kleinmanni
## 454         nesomys                audeberti
## 552       terrapene                   ornata
## 523      sauromalus                   obesus
## 110       ictalurus                  natalis
## 393     petrodromus            tetradactylus
## 149           falco              tinnunculus
## 474    spermophilus         tridecemlineatus
## 541        gloydius              shedaoensis
## 14      ambloplites                rupestris
## 59       parupeneus               porphyreus
## 508         nerodia                 sipeodon
## 556    sternotherus                 odoratus
## 530     agkistrodon              piscivorous
## 239           picus                  viridis
## 71           scarus                    iseri
## 455         nesomys                    rufus
## 159            crex                     crex
## 504    lampropeltis               triangulum
## 555    sternotherus           minor peltifer
## 542     montivipera                   raddei
## 438        thomomys                   bottae
## 211    scenopoeetes             dentirostris
## 245          athene                   noctua
## 390        ochotona                curzoniae
## 410    heliophobius         argenteocinereus
## 554     kinosternon                rubrubrum
## 441       dipodomys                   ingens
## 11             naso                unicornis
## 129        clamator               glandarius
## 338         mustela                  frenata
## 125         columba                 palumbus
## 459       glaucomys                 sabrinus
## 407       cryptomys               damarensis
## 501       heterodon              platirhinos
## 391        ochotona                 princeps
## 444       dipodomys              spectabilis
## 496         coluber constrictor flaviventris
## 136       accipiter                 striatus
## 126    streptopelia                   turtur
## 537        crotalus        oreganus concolor
## 60            perca               flavescens
## 36         bodianus                    rufus
## 427         neotoma                   lepida
## 169       nucifraga            caryocatactes
## 130         cuculus                  canorus
## 238           picus                    canus
## 452       pseudomys                   fuscus
## 38            coris                    julis
## 79    cephalopholis              hemistiktos
## 86   hypoplectrodes                   huntii
## 243        aegolius                 funereus
## 148           falco               sparverius
## 235        picoides                 leucotos
## 9        acanthurus                 lineatus
## 99     oncorhynchus                   mykiss
## 534        crotalus                 cerastes
## 473    spermophilus                spilosoma
## 489        scalopus                aquaticus
## 47    tautogolabrus                adspersus
## 127        coracias                 garrulus
## 458      funambulus                 pennanti
## 456      spalacopus                   cyanus
## 106        sebastes                  inermis
## 97     oncorhynchus                   clarki
## 543          vipera                 latastei
## 490           talpa                 europaea
## 457        eutamias                sibiricus
## 45    opthalmolepis               lineolatus
## 61        abudefduf                  luridus
## 416          lemmus                sibiricus
## 180       sturnella                    magna
## 181       sturnella                 neglecta
## 451        meriones                hurrianae
## 342         mustela                  nivalis
## 57    dicentrarchus                   labrax
## 423        microtus              richardsoni
## 49       thalassoma                   lunare
## 15          lepomis                 gibbosus
## 491           talpa                   romana
## 16          lepomis              macrochirus
## 506          natrix                   natrix
## 25        chaetodon             unimaculatus
## 445       dipodomys                stephensi
## 94         serranus                   scriba
## 34     valenciennea              longipinnis
## 418        microtus             californicus
## 24        chaetodon             trifasciatus
## 439      graphiurus                 ocularis
## 538        crotalus                   pricei
## 128           upupa                    epops
## 232      ixobrychus                   exilis
## 415       hylaeamys             megacephalus
## 237        picoides              tridactylus
## 408       cryptomys              hottentotus
## 460       glaucomys                   volans
## 509      oocatochus             rufodorsatus
## 248      glaucidium               passerinum
## 23        chaetodon             trifascialis
## 303      stylodipus                    telum
## 236        picoides                   medius
## 524     dipsosaurus                 dorsalis
## 392    elephantulus                rufescens
## 419        microtus                 montanus
## 443       dipodomys                    ordii
## 54         lutjanus                  griseus
## 52         lutjanus                   apodus
## 433      pygeretmus                  pumilio
## 187           mimus              polyglottos
## 53         lutjanus               decussatus
## 421        microtus           pennsylvanicus
## 515   hoplocephalus              bungaroides
## 184         laniuis             ludovicianus
## 122     caprimulgus                europaeus
## 488       condylura                 cristata
## 124     scardafella                     inca
## 98     oncorhynchus                    gilae
## 173          pipilo                   aberti
## 67        stegastes                 apicalis
## 174          pipilo                   fuscus
## 185          lanius                    minor
## 477          tamias                  minimus
## 478          tamias           quadrivittatus
## 479          tamias                 umbrinus
## 170       phytotoma                raimondii
## 442       dipodomys                 merriami
## 93         serranus                 cabrilla
## 226        tyrannus                 tyrannus
## 525        gallotia                  galloti
## 69        stegastes               variabilis
## 411  heterocephalus                   glaber
## 432      synaptomys                  cooperi
## 234            jynx                torquilla
## 417        microtus                 agrestis
## 164           habia               fuscicauda
## 420        microtus              ochrogaster
## 186          lanius                  senator
## 3        campostoma                 anomalum
## 165           habia                   rubica
## 22        chaetodon                trichrous
## 449        apodemus              flavicollis
## 440     muscardinus             avellanarius
## 222          sialia                   sialis
## 163         lululla                  arborea
## 183         laniuis                 collurio
## 424          myopus             schisticolor
## 422        microtus                pinetorum
## 356        thylamys                  elegans
## 63          chromis                  chromis
## 526      phymaturus              flagellifer
## 21        chaetodon                baronessa
## 430      peromyscus               gossypinus
## 354      antechinus                 stuartii
## 182         icteria                   virens
## 476          tamias                  amoenus
## 191        oenanthe                 oenanthe
## 87   hypoplectrodes              maccullochi
## 178       fringilla                  coelebs
## 255      eremitalpa                   granti
## 353     sminthopsis                 leucopus
## 414   clethrionomys             californicus
## 404       onychomys                 torridus
## 512      thamnophis                  butleri
## 188       motacilla                     alba
## 450        apodemus               sylvaticus
## 355     monodelphis                americana
## 201         seiurus             aurocapillus
## 219     thryothorus             ludovicianus
## 214           sitta                 europaea
## 175        spizella                  arborea
## 230           vireo                olivaceus
## 189       motacilla                    flava
## 531           bitis               schneideri
## 171      ammodramus               savannarum
## 196           parus                inornatus
## 193        saxicola                  rubetra
## 200    protonotaria                   citrea
## 17          lepomis                megalotis
## 192     phoenicurus              phoenicurus
## 215         chamaea                 fasciata
## 172       passerina                   cyanea
## 487           sorex             unguiculatus
## 203       setophaga                kirtlandi
## 223        contopus                   virens
## 190       muscicapa                  striata
## 225       empidonax                 wrightii
## 176        spizella                passerina
## 229           vireo                  griseus
## 198  geothlypis\xa0             philadelphia
## 220     troglodytes                    aedon
## 431 reithrodontomys              raviventris
## 161       hippolais               polyglotta
## 194           parus             atricapillus
## 197           parus                palustris
## 218      thryomanes                 bewickiI
## 179         serinus                  serinus
## 216          sylvia                    sarda
## 66      pomacentrus                    wardi
## 28   cirrhitichthys                    falco
## 195           parus             carolinensis
## 228           vireo                    belli
## 482       crocidura                  russula
## 224       empidonax                  minimus
## 167       cisticola                 juncidis
## 199      geothylpis                  trichas
## 202       setophaga                    fusca
## 205       setophaga             pensylvanica
## 41      halichoeres              maculipinna
## 111     hippocampus               guttulatus
## 206       setophaga                 petechia
## 221     troglodytes              troglodytes
## 485           sorex                coronatus
## 209        wilsonia               canadensis
## 64      chrysiptera               biocellata
## 207       setophaga                ruticilla
## 208       setophaga                   virens
## 497       diadophis                punctatus
## 113    canthigaster                 rostrata
## 217          sylvia                   undata
## 166         certhia               familiaris
## 204       setophaga                 magnolia
## 227           vireo             atricapillus
## 483           sorex                 arcticus
## 162      aegithalos                 caudatus
## 210    phylloscopus                  bonelli
## 39      halichoeres               bivittatus
## 42      halichoeres                    poeyi
## 68        stegastes                 partitus
## 40      halichoeres                  garnoti
## 12    ophioblennius               atlanticus
## 212         regulus             ignicapillus
## 213         regulus                  regulus
## 102          cottus                   bairdi
## 104          cottus                    gobio
## 486           sorex              gracillimus
## 484           sorex                 cinereus
## 4       clinostomus              funduloides
## 5       rhinichthys               cataractae
## 33     rhinogobiops                nicholsii
## 65        dascyllus                  aruanus
## 494       carphopis                  viridis
## 43        labroides               dimidiatus
## 493       carphopis                   vermis
## 48       thalassoma              bifasciatum
## 103          cottus                carolinae
## 30    amblyeleotris                 japonica
## 62       centropyge                     argi
## 100           salmo                    salar
## 112        nerophis           lumbriciformis
## 27   amblycirrhitus                    pinos
## 31       lythrypnus                    dalli
## 32        priolepis                 hipoliti
```
**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

```r
smallest <- subset(homerange, family == "reptilia")

smallest
```

```
## # A tibble: 0 x 24
## # … with 24 variables: taxon <chr>, common.name <chr>, class <chr>,
## #   order <chr>, family <chr>, genus <chr>, species <chr>, primarymethod <chr>,
## #   N <chr>, mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <chr>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
