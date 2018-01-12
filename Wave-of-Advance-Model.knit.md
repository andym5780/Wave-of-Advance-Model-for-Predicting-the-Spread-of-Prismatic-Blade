---
title: "Wave-of-Advance-Model"
author: "Andrew Mark"
date: "December 15, 2017"
output:
  pdf_document: default
  html_document: default
---


```r
library(curl)
library(knitr)
library(ggmap)
```

```
## Loading required package: ggplot2
```

```r
library(ggplot2)
```




```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Combined-dataset.csv")
blade.database <- read.csv(f, header = TRUE)
head(blade.database)
```

```
##     Country       Region                  Site                      Zone
## 1 Honduras  Copan Valley Copan(Las Sepulturas)             Patio A, 9N-8
## 2 Honduras  Copan Valley Copan(Las Sepulturas) Vacant Space(IV/82 Pit 2)
## 3 Honduras  Copan Valley Copan(Las Sepulturas) Vacant Space(IV/82 Pit 3)
## 4 Honduras  Copan Valley Copan(Las Sepulturas)     Patio A, 9N-8(IV/111)
## 5 Honduras  Copan Valley Copan(Las Sepulturas)      Patio A, 9N-8(*8,20)
## 6 Honduras  Copan Valley      Copan(El Bosque)            11L-15(IV/115)
##   Latitude..DD. Longitude..DD.           Period Phase Start.BP End.BP
## 1      14.83874      -89.13520 Early Formative   Rayo     3280   3400
## 2      14.84409      -89.13364 Middle Formative           2850   2250
## 3      14.84104      -89.13461 Middle Formative           2850   2250
## 4      14.83874      -89.13520 Middle Formative           2850   2250
## 5      14.83874      -89.13520 Middle Formative           2850   2250
## 6      14.83313      -89.14508 Middle Formative           2850   2250
##   Radiocarbon.Dates..BP.     Sourc.es. Table.s..Included
## 1                        Aoyama (1999)               3.1
## 2                        Aoyama (1999)          4.1,4.2.
## 3                        Aoyama (1999)          4.1,4.2.
## 4                        Aoyama (1999)          4.1,4.2.
## 5                        Aoyama (1999)          4.1,4.2.
## 6                        Aoyama (1999)          4.1,4.2.
##   Total.Chipped.Stone Total.Chert Total.Obsidian Total.Blade
## 1                  84           8             76           0
## 2                2733        1188           1545          24
## 3                  35          18             17           0
## 4                 205          66            139           3
## 5                 325          57            268          16
## 6                  17           2             15           4
##   Total.Obstdion.Total.Chipped.Stone
## 1                        0.904761905
## 2                        0.565312843
## 3                        0.485714286
## 4                         0.67804878
## 5                        0.824615385
## 6                        0.882352941
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         0
## 2                               0.015533981
## 3                                         0
## 4                               0.021582734
## 5                               0.059701493
## 6                               0.266666667
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone. P.G.Platform P.G.Ratio
## 1                                              0                       
## 2                                    0.008781559                       
## 3                                              0                       
## 4                                    0.014634146                       
## 5                                    0.049230769                       
## 6                                    0.235294118                       
##                              Notes                             Comments X
## 1 From the Vega Physiographic Zone Chronology based on ceramic typology  
## 2 From the Vega Physiographic Zone        DMS=degrees, minutes, seconds  
## 3 From the Vega Physiographic Zone                   DD=digital degrees  
## 4 From the Vega Physiographic Zone                                       
## 5 From the Vega Physiographic Zone                                       
## 6 From the Vega Physiographic Zone                                       
##   X.1 X.2 X.3 X.4 X.5 X.6 X.7 X.8 X.9
## 1      NA  NA  NA  NA  NA  NA  NA  NA
## 2      NA  NA  NA  NA  NA  NA  NA  NA
## 3      NA  NA  NA  NA  NA  NA  NA  NA
## 4      NA  NA  NA  NA  NA  NA  NA  NA
## 5      NA  NA  NA  NA  NA  NA  NA  NA
## 6      NA  NA  NA  NA  NA  NA  NA  NA
```


```r
long <- blade.database$Longitude..DD.
lati <- blade.database$Latitude..DD.
```




```r
size <- make_bbox(lon = long, lat = lati, f = .2)
size
```

```
##       left     bottom      right        top 
## -136.92989   13.58653  126.49725   20.67604
```



```r
sq_map <- get_map(location = size, maptype = "satellite", source = "google")
```

```
## Warning: bounding box given to google - spatial extent only approximate.
```

```
## converting bounding box to center/zoom specification. (experimental)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17.131284,-5.216321&zoom=6&size=640x640&scale=2&maptype=satellite&language=en-EN&sensor=false
```



```r
ggmap(sq_map) + geom_point(data = blade.database, mapping = aes(x = long, y = lati))
```

```
## Warning: Removed 994 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-6-1.pdf)<!-- --> 



```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```

```r
ggmap(myMap) + geom_point(aes(x = long, y = lati, color = "black"), data = blade.database, size = 3) + xlim(-100,-87) + ylim(14,20)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 550 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-7-1.pdf)<!-- --> 


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = blade.database$Total.Blade, color = blade.database$Total.Blade), data = blade.database) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Using size for a discrete variable is not advised.
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 550 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-9-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Using size for a discrete variable is not advised.
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 550 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-10-1.pdf)<!-- --> 
the relative size and color ledgent was too big when I first did this so I had to take the legend out. This is jsut one example of the maps R and make

#Before 8500

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/Before%208500%20BP.csv")
Before8500BP <- read.csv(f, header = TRUE)
head(Before8500BP)
```

```
##   Country          Region   Site  Zone Latitude..DD. Longitude..DD.
## 1  Mexico Tehuacan Valley  Tc 50 XXVII      18.44056      -97.21887
## 2  Mexico Tehuacan Valley  Tc 50  XXVI      18.44056      -97.21887
## 3  Mexico Tehuacan Valley  Tc 50   XXV      18.44056      -97.21887
## 4  Mexico Tehuacan Valley Tc 50   XXIV      18.44056      -97.21887
## 5  Mexico Tehuacan Valley  Tc 50 XXIII      18.44056      -97.21887
## 6  Mexico Tehuacan Valley Tc 35w     6      18.44109      -97.54070
##         Period     Phase Start.BP End.BP Radiocarbon.Dates..BP.  Sourc.es.
## 1 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
## 2 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
## 3 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
## 4 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
## 5 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
## 6 Paleo-Indian Ajuereado     8500   8500                    n/a Byers 1967
##   Table.s..Included Total.Chipped.Stone Total.Chert Total.Obsidian
## 1               1,6                   2           2              0
## 2             1,6,8                   3           3              0
## 3          1,6,8,12                   9           9              0
## 4       1,6,8,10,12                  32          31              0
## 5       1,6,8,10,12                  84          83              0
## 6       1,6,8,10,12                  26          22              0
##   Total.Blade Total.Obstdion.Total.Chipped.Stone
## 1           0                                  0
## 2           0                                  0
## 3           0                                  0
## 4           0                                  0
## 5           0                                  0
## 6           0                                  0
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         0
## 2                                         0
## 3                                         0
## 4                                         0
## 5                                         0
## 6                                         0
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                              0 NA  NA  NA  NA  NA  NA
## 2                                              0 NA  NA  NA  NA  NA  NA
## 3                                              0 NA  NA  NA  NA  NA  NA
## 4                                              0 NA  NA  NA  NA  NA  NA
## 5                                              0 NA  NA  NA  NA  NA  NA
## 6                                              0 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9
## 1  NA  NA  NA  NA
## 2  NA  NA  NA  NA
## 3  NA  NA  NA  NA
## 4  NA  NA  NA  NA
## 5  NA  NA  NA  NA
## 6  NA  NA  NA  NA
```


```r
long <- Before8500BP$Longitude..DD.
lati <- Before8500BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = Before8500BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = Before8500BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-14-1.pdf)<!-- --> 

```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-15-1.pdf)<!-- --> 
#8500-6800

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/8500-6800%20BP.csv")
From8500to6800BP <- read.csv(f, header = TRUE)
head(From8500to6800BP)
```

```
##   Country          Region   Site Zone Latitude..DD. Longitude..DD.  Period
## 1  Mexico Tehuacan Valley  Tc 50 XXII      18.44056      -97.21887 Archaic
## 2  Mexico Tehuacan Valley  Tc 50  XXI      18.44056      -97.21887 Archaic
## 3  Mexico Tehuacan Valley Tc 255    C      18.38074      -97.39076 Archaic
## 4  Mexico Tehuacan Valley  Tc 50   XX      18.44056      -97.21887 Archaic
## 5  Mexico Tehuacan Valley  Tc 50  XIX      18.44056      -97.21887 Archaic
## 6  Mexico Tehuacan Valley Tc 307    H      18.03543      -97.09124 Archaic
##      Phase Start.BP End.BP Radiocarbon.Dates..BP.  Sourc.es.
## 1 El Riego     8500   6800                    n/a Byers 1967
## 2 El Riego     8500   6800                    n/a Byers 1967
## 3 El Riego     8500   6800                    n/a Byers 1967
## 4 El Riego     8500   6800                    n/a Byers 1967
## 5 El Riego     8500   6800                    n/a Byers 1967
## 6 El Riego     8500   6800                    n/a Byers 1967
##   Table.s..Included Total.Chipped.Stone Total.Chert Total.Obsidian
## 1       1,6,8,10,12                  29          25              0
## 2         1,6,8,10,                  26          15              0
## 3          1,6,8,12                  57          57              0
## 4       1,6,8,10,12                  49          47              0
## 5       1,6,8,10,12                  76          71              0
## 6       1,6,8,10,12                  15          13              0
##   Total.Blade Total.Obstdion.Total.Chipped.Stone
## 1           0                                  0
## 2           0                                  0
## 3           0                                  0
## 4           0                                  0
## 5           0                                  0
## 6           0                                  0
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         0
## 2                                         0
## 3                                         0
## 4                                         0
## 5                                         0
## 6                                         0
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                              0 NA  NA  NA  NA  NA  NA
## 2                                              0 NA  NA  NA  NA  NA  NA
## 3                                              0 NA  NA  NA  NA  NA  NA
## 4                                              0 NA  NA  NA  NA  NA  NA
## 5                                              0 NA  NA  NA  NA  NA  NA
## 6                                              0 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From8500to6800BP$Longitude..DD.
lati <- From8500to6800BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From8500to6800BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From8500to6800BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-19-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-20-1.pdf)<!-- --> 

#6800-5500

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/6800-5500%20BP.csv")
From6800to5500BP <- read.csv(f, header = TRUE)
head(From6800to5500BP)
```

```
##   Country          Region   Site Zone Latitude..DD. Longitude..DD.  Period
## 1  Mexico Tehuacan Valley Tc 272    Q      18.03543      -97.21887 Archaic
## 2  Mexico Tehuacan Valley  Tc 50 XIII      18.44056      -97.21887 Archaic
## 3  Mexico Tehuacan Valley Tc 307    D      18.03543      -97.09124 Archaic
## 4  Mexico Tehuacan Valley  Tc 50  XII      18.44056      -97.21887 Archaic
## 5  Mexico Tehuacan Valley Tc 35w    4      18.44109      -97.54070 Archaic
## 6  Mexico Tehuacan Valley Tc 254    E      18.38074      -97.39076 Archaic
##       Phase Start.BP End.BP Radiocarbon.Dates..BP.  Sourc.es.
## 1 Coxcatlan     6800   5500                    n/a Byers 1967
## 2 Coxcatlan     6800   5500                    n/a Byers 1967
## 3 Coxcatlan     6800   5500                    n/a Byers 1967
## 4 Coxcatlan     6800   5500                    n/a Byers 1967
## 5 Coxcatlan     6800   5500                    n/a Byers 1967
## 6 Coxcatlan     6800   5500                    n/a Byers 1967
##   Table.s..Included Total.Chipped.Stone Total.Chert Total.Obsidian
## 1               6,8                   3           3              0
## 2       1,6,8,10,12                 180         154              0
## 3       1,6,8,10,12                  15           9              0
## 4       1,6,8,10,12                 166         129              1
## 5       1,6,8,10,12                 218         162              1
## 6          1,6,8,12                  11          11              0
##   Total.Blade Total.Obstdion.Total.Chipped.Stone
## 1           0                        0.000000000
## 2           0                        0.000000000
## 3           0                        0.000000000
## 4           0                        0.006024096
## 5           1                        0.004587156
## 6           0                        0.000000000
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         0
## 2                                         0
## 3                                         0
## 4                                         0
## 5                                         1
## 6                                         0
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                    0.000000000 NA  NA  NA  NA  NA  NA
## 2                                    0.000000000 NA  NA  NA  NA  NA  NA
## 3                                    0.000000000 NA  NA  NA  NA  NA  NA
## 4                                    0.000000000 NA  NA  NA  NA  NA  NA
## 5                                    0.004587156 NA  NA  NA  NA  NA  NA
## 6                                    0.000000000 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From6800to5500BP$Longitude..DD.
lati <- From6800to5500BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From6800to5500BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From6800to5500BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-24-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-25-1.pdf)<!-- --> 
#5500-4300

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/5500-4300%20BP.csv")
From5500to4300BP <- read.csv(f, header = TRUE)
head(From5500to4300BP)
```

```
##   Country          Region   Site Zone Latitude..DD. Longitude..DD.  Period
## 1  Mexico Tehuacan Valley  Tc 50    X      18.44056      -97.21887 Archaic
## 2  Mexico Tehuacan Valley Tc 254    D      18.38074      -97.39076 Archaic
## 3  Mexico Tehuacan Valley Tc 307    B      18.03543      -97.09124 Archaic
## 4  Mexico Tehuacan Valley Tc 272    N      18.03543      -97.21887 Archaic
## 5  Mexico Tehuacan Valley Tc 272  N^1      18.03543      -97.21887 Archaic
## 6  Mexico Tehuacan Valley  Ts 51    C      18.44056      -97.21887 Archaic
##    Phase Start.BP End.BP Radiocarbon.Dates..BP.  Sourc.es.
## 1 Abejas     5500   4300                    n/a Byers 1967
## 2 Abejas     5500   4300                    n/a Byers 1967
## 3 Abejas     5500   4300                    n/a Byers 1967
## 4 Abejas     5500   4300                    n/a Byers 1967
## 5 Abejas     5500   4300                    n/a Byers 1967
## 6 Abejas     5500   4300                    n/a Byers 1967
##   Table.s..Included Total.Chipped.Stone Total.Chert Total.Obsidian
## 1       1,6,8,10,12                 365         217              3
## 2       1,6,8,10,12                  27          24              2
## 3       1,6,8,10,12                  43          38              2
## 4            1,6,12                  19          18              1
## 5                 1                   1          NA              1
## 6       1,6,8,10,12                  41          34              1
##   Total.Blade Total.Obstdion.Total.Chipped.Stone
## 1           2                        0.008219178
## 2           3                        0.074074074
## 3           1                        0.046511628
## 4           1                        0.052631579
## 5           1                        1.000000000
## 6           1                        0.024390244
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                 0.6666667
## 2                                 1.5000000
## 3                                 0.5000000
## 4                                 1.0000000
## 5                                 1.0000000
## 6                                 1.0000000
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                    0.005479452 NA  NA  NA  NA  NA  NA
## 2                                    0.111111111 NA  NA  NA  NA  NA  NA
## 3                                    0.023255814 NA  NA  NA  NA  NA  NA
## 4                                    0.052631579 NA  NA  NA  NA  NA  NA
## 5                                    1.000000000 NA  NA  NA  NA  NA  NA
## 6                                    0.024390244 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From5500to4300BP$Longitude..DD.
lati <- From5500to4300BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From5500to4300BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From5500to4300BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-29-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-30-1.pdf)<!-- --> 

#4300-3500

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/4300-3500%20BP.csv")
From4300to3500BP <- read.csv(f, header = TRUE)
head(From4300to3500BP)
```

```
##   Country          Region                                  Site Zone
## 1  Mexico Tehuacan Valley                                Tc 272  K^1
## 2  Mexico Tehuacan Valley                                Tc 272    K
## 3  Mexico         Chiapas                            Cuauhtemoc  n/a
## 4  Mexico         Chiapas                      Paso de la Amada  n/a
## 5  Mexico         Chiapas Paso de la Amada (other Proveniences)  n/a
## 6  Mexico         Chiapas                            Cuauhtemoc  n/a
##   Latitude..DD. Longitude..DD.            Period  Phase Start.BP End.BP
## 1      18.03543      -97.21887   Early Formative Purron     4300   3500
## 2      18.03543      -97.21887   Early Formative Purron     4300   3500
## 3      14.59932      -92.22720 Initial Formative  Barra     3850   3650
## 4      14.87773      -92.49078 Initial Formative Locona     3650   3450
## 5      14.87773      -92.49078 Initial Formative Locona     3650   3450
## 6      14.59932      -92.22720 Initial Formative Locona     3650   3450
##   Radiocarbon.Dates..BP.    Sourc.es.              Table.s..Included
## 1                    n/a   Byers 1967                           1,12
## 2                    n/a   Byers 1967                            1,8
## 3                    N/a Stark (2016)  Table 2, Supplemental table 2
## 4                    N/a Stark (2016) Table 2, Supplemental table 2.
## 5                    N/a Stark (2016) Table 2, Supplemental table 2.
## 6                    N/a Stark (2016) Table 2, Supplemental table 2.
##   Total.Chipped.Stone Total.Chert Total.Obsidian Total.Blade
## 1                   5           2              3           3
## 2                   3           1              2           2
## 3                 752           0            752           0
## 4              16,755           0         16,755         n/a
## 5                5923           0           5923         n/a
## 6                2126           0           2126           0
##   Total.Obstdion.Total.Chipped.Stone
## 1                          0.6000000
## 2                          0.6666667
## 3                          1.0000000
## 4                          1.0000000
## 5                          1.0000000
## 6                          1.0000000
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         1
## 2                                         1
## 3                                         0
## 4                                       n/a
## 5                                       n/a
## 6                                         0
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                            0.6 NA  NA  NA  NA  NA  NA
## 2                                    0.666666667 NA  NA  NA  NA  NA  NA
## 3                                              0 NA  NA  NA  NA  NA  NA
## 4                                            n/a NA  NA  NA  NA  NA  NA
## 5                                            n/a NA  NA  NA  NA  NA  NA
## 6                                              0 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From4300to3500BP$Longitude..DD.
lati <- From4300to3500BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From4300to3500BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From4300to3500BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-34-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-35-1.pdf)<!-- --> 
#3500-2900

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/3500-2900%20BP.csv")
From3500to2900BP <- read.csv(f, header = TRUE)
head(From3500to2900BP)
```

```
##     Country          Region                  Site          Zone
## 1 Honduras     Copan Valley Copan(Las Sepulturas) Patio A, 9N-8
## 2    Mexico Tehuacan Valley                Ts 204             H
## 3    Mexico Tehuacan Valley                Tc 272             J
## 4    Mexico Tehuacan Valley                Ts 204             G
## 5    Mexico Tehuacan Valley               Ts 204D         sub E
## 6    Mexico Tehuacan Valley                Ts 204           G^1
##   Latitude..DD. Longitude..DD.                           Period   Phase
## 1      14.83874      -89.13520                 Early Formative     Rayo
## 2      18.60288      -97.40244 Early Formative-Middle Formative Ajalpan
## 3      18.03543      -97.21887 Early Formative-Middle Formative Ajalpan
## 4      18.60288      -97.40244 Early Formative-Middle Formative Ajalpan
## 5      18.60288      -97.40244 Early Formative-Middle Formative Ajalpan
## 6      18.60288      -97.40244 Early Formative-Middle Formative Ajalpan
##   Start.BP End.BP Radiocarbon.Dates..BP.     Sourc.es. Table.s..Included
## 1     3400   3280                        Aoyama (1999)               3.1
## 2     3500   2900                           Byers 1967       1,6,8,10,12
## 3     3500   2900                           Byers 1967          1,6,8,12
## 4     3500   2900                           Byers 1967       1,6,8,10,12
## 5     3500   2900                           Byers 1967            1,6,10
## 6     3500   2900                           Byers 1967       1,6,8,10,12
##   Total.Chipped.Stone Total.Chert Total.Obsidian Total.Blade
## 1                  84           8             76           0
## 2                  58          43              3           2
## 3                  14          10              4           4
## 4                  67          59              1           0
## 5                   9           2              2           2
## 6                 101          81              4           4
##   Total.Obstdion.Total.Chipped.Stone
## 1                         0.90476190
## 2                         0.05172414
## 3                         0.28571429
## 4                         0.01492537
## 5                         0.22222222
## 6                         0.03960396
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         0
## 2                               0.666666667
## 3                                         1
## 4                                         0
## 5                                         1
## 6                                         1
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                              0 NA  NA  NA  NA  NA  NA
## 2                                    0.034482759 NA  NA  NA  NA  NA  NA
## 3                                    0.285714286 NA  NA  NA  NA  NA  NA
## 4                                              0 NA  NA  NA  NA  NA  NA
## 5                                    0.222222222 NA  NA  NA  NA  NA  NA
## 6                                     0.03960396 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```

```r
From3500to2900BP
```

```
##      Country          Region                                  Site
## 1  Honduras     Copan Valley                 Copan(Las Sepulturas)
## 2     Mexico Tehuacan Valley                                Ts 204
## 3     Mexico Tehuacan Valley                                Tc 272
## 4     Mexico Tehuacan Valley                                Ts 204
## 5     Mexico Tehuacan Valley                               Ts 204D
## 6     Mexico Tehuacan Valley                                Ts 204
## 7     Mexico Tehuacan Valley                                Ts 204
## 8     Mexico Tehuacan Valley                               Ts 204D
## 9     Mexico Tehuacan Valley                                Ts 204
## 10    Mexico Tehuacan Valley                                Tc 254
## 11    Mexico Tehuacan Valley                                Ts 204
## 12    Mexico Tehuacan Valley                               Ts 204C
## 13    Mexico Tehuacan Valley                                Ts 368
## 14    Mexico Tehuacan Valley                               Ts 368e
## 15    Mexico Tehuacan Valley                                Ts 368
## 16    Mexico Tehuacan Valley                               Ts 368e
## 17    Mexico Tehuacan Valley                                Ts 368
## 18    Mexico Tehuacan Valley                               Ts 368e
## 19    Mexico Tehuacan Valley                                Ts 368
## 20    Mexico Tehuacan Valley                               Ts 368e
## 21    Mexico         Chiapas                            Cuauhtemoc
## 22    Mexico         Chiapas                      Paso de la Amada
## 23    Mexico         Chiapas Paso de la Amada (other Proveniences)
## 24    Mexico         Chiapas                      Paso de la Amada
## 25    Mexico         Chiapas Paso de la Amada (other Proveniences)
## 26    Mexico         Chiapas                            Cuauhtemoc
## 27    Mexico         Chiapas                            Cuauhtemoc
## 28    Mexico         Chiapas                            Cuauhtemoc
## 29    Mexico         Tabasco              Isla Alor (Huimanfuillo)
## 30    Mexico        Veracruz                   Paso de los Ortices
## 31    Mexico        Veracruz        San Lorenzo(D5-9, D5-31, B3-5)
##             Zone Latitude..DD. Longitude..DD.
## 1  Patio A, 9N-8      14.83874      -89.13520
## 2              H      18.60288      -97.40244
## 3              J      18.03543      -97.21887
## 4              G      18.60288      -97.40244
## 5          sub E      18.60288      -97.40244
## 6            G^1      18.60288      -97.40244
## 7              F      18.60288      -97.40244
## 8              E      18.60288      -97.40244
## 9            F^1      18.60288      -97.40244
## 10             C      18.38074      -97.39076
## 11             E      18.60288      -97.40244
## 12           pit      18.60288      -97.40244
## 13           K^3      18.08659      -97.12971
## 14           K^3      18.08659      -97.12971
## 15           K^2      18.08659      -97.12971
## 16           K^2      18.08659      -97.12971
## 17           K^1      18.08659      -97.12971
## 18           K^1      18.08659      -97.12971
## 19             J      18.08659      -97.12971
## 20             J      18.08659      -97.12971
## 21           N/a      14.59932      -92.22720
## 22           N/a      14.87773      -92.49078
## 23           N/a      14.87773      -92.49078
## 24           N/a      14.87773      -92.49078
## 25           N/a      14.87773      -92.49078
## 26           N/a      14.59932      -92.22720
## 27           N/a      14.59932      -92.22720
## 28           N/a      14.59932      -92.22720
## 29           N/a      18.13683      -94.04713
## 30           N/a      17.83310      -94.75510
## 31           N/a      15.02477      -91.77659
##                              Period        Phase Start.BP End.BP
## 1                  Early Formative          Rayo     3400   3280
## 2  Early Formative-Middle Formative      Ajalpan     3500   2900
## 3  Early Formative-Middle Formative      Ajalpan     3500   2900
## 4  Early Formative-Middle Formative      Ajalpan     3500   2900
## 5  Early Formative-Middle Formative      Ajalpan     3500   2900
## 6  Early Formative-Middle Formative      Ajalpan     3500   2900
## 7  Early Formative-Middle Formative      Ajalpan     3500   2900
## 8  Early Formative-Middle Formative      Ajalpan     3500   2900
## 9  Early Formative-Middle Formative      Ajalpan     3500   2900
## 10 Early Formative-Middle Formative      Ajalpan     3500   2900
## 11 Early Formative-Middle Formative      Ajalpan     3500   2900
## 12 Early Formative-Middle Formative      Ajalpan     3500   2900
## 13 Early Formative-Middle Formative      Ajalpan     3500   2900
## 14 Early Formative-Middle Formative      Ajalpan     3500   2900
## 15 Early Formative-Middle Formative      Ajalpan     3500   2900
## 16 Early Formative-Middle Formative      Ajalpan     3500   2900
## 17 Early Formative-Middle Formative      Ajalpan     3500   2900
## 18 Early Formative-Middle Formative      Ajalpan     3500   2900
## 19 Early Formative-Middle Formative      Ajalpan     3500   2900
## 20 Early Formative-Middle Formative      Ajalpan     3500   2900
## 21                  Early Formative         Ocos     3450   3350
## 22                  Early Formative         Ocos     3450   3350
## 23                  Early Formative         Ocos     3450   3350
## 24                  Early Formative       Cherla     3350   3250
## 25                  Early Formative       Cherla     3350   3250
## 26                  Early Formative       Cherla     3350   3250
## 27                  Early Formative      Cuadros     3150   2950
## 28                  Early Formative      Jocotal     2950   2900
## 29                  Early Formative Missing Data     3450   2850
## 30                  Early Formative Missing Data     3450   2850
## 31                  Early Formative Missing Data     3450   2850
##    Radiocarbon.Dates..BP.     Sourc.es.              Table.s..Included
## 1                         Aoyama (1999)                            3.1
## 2                            Byers 1967                    1,6,8,10,12
## 3                            Byers 1967                       1,6,8,12
## 4                            Byers 1967                    1,6,8,10,12
## 5                            Byers 1967                         1,6,10
## 6                            Byers 1967                    1,6,8,10,12
## 7                            Byers 1967                    1,6,8,10,12
## 8                            Byers 1967                      1,6,10,12
## 9                            Byers 1967                    1,6,8,10,12
## 10                           Byers 1967                       1,6,8,12
## 11                           Byers 1967                           6,12
## 12                           Byers 1967                    1,6,8,10,12
## 13                           Byers 1967                           6,12
## 14                           Byers 1967                             10
## 15                           Byers 1967                         1,6,12
## 16                           Byers 1967                           8,10
## 17                           Byers 1967                         1,6,12
## 18                           Byers 1967                           8,10
## 19                           Byers 1967                         1,6,12
## 20                           Byers 1967                             10
## 21                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 22                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 23                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 24                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 25                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 26                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 27                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 28                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 29                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 30                    N/a  Stark (2016) Table 2, Supplemental table 2.
## 31                    N/a  Stark (2016) Table 2, Supplemental table 2.
##    Total.Chipped.Stone  Total.Chert Total.Obsidian  Total.Blade
## 1                   84            8             76            0
## 2                   58           43              3            2
## 3                   14           10              4            4
## 4                   67           59              1            0
## 5                    9            2              2            2
## 6                  101           81              4            4
## 7                  115           94              2            1
## 8                   18            8              4            4
## 9                   46           31              3            2
## 10                  23           22              1            1
## 11                   5            3              2            0
## 12                  22            7              5            3
## 13                   9            9              0            0
## 14                   4                           0            0
## 15                  26           21              5            5
## 16                  11            4              0            0
## 17                  17           16              1            0
## 18                   8            3              0            0
## 19                   4            2              2            2
## 20                   6                           0            0
## 21                 702            0            702            0
## 22                9096            0           9096 Missing Dara
## 23               18314            0          18314 Missing Dara
## 24                6954            0           6954            1
## 25               49965            0          49965 Missing Data
## 26                 499            0            499            0
## 27                 168            0           1680            0
## 28                 239            0            239            0
## 29                  77            1             40            8
## 30                  19 Missing Data             19 Missing Data
## 31                 447 Missing Data            447 Missing Data
##    Total.Obstdion.Total.Chipped.Stone
## 1                          0.90476190
## 2                          0.05172414
## 3                          0.28571429
## 4                          0.01492537
## 5                          0.22222222
## 6                          0.03960396
## 7                          0.01739130
## 8                          0.22222222
## 9                          0.06521739
## 10                         0.04347826
## 11                         0.40000000
## 12                         0.22727273
## 13                         0.00000000
## 14                         0.00000000
## 15                         0.19230769
## 16                         0.00000000
## 17                         0.05882353
## 18                         0.00000000
## 19                         0.50000000
## 20                         0.00000000
## 21                         1.00000000
## 22                         1.00000000
## 23                         1.00000000
## 24                         1.00000000
## 25                         1.00000000
## 26                         1.00000000
## 27                        10.00000000
## 28                         1.00000000
## 29                         0.51948052
## 30                         1.00000000
## 31                         1.00000000
##    Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                          0
## 2                                0.666666667
## 3                                          1
## 4                                          0
## 5                                          1
## 6                                          1
## 7                                        0.5
## 8                                          1
## 9                                0.666666667
## 10                                         1
## 11                                         0
## 12                                       0.6
## 13                                         0
## 14                                         0
## 15                                         1
## 16                                         0
## 17                                         0
## 18                                         0
## 19                                         1
## 20                                         0
## 21                                         0
## 22                                       n/a
## 23                                       n/a
## 24                               0.000143802
## 25                                       n/a
## 26                                         0
## 27                                         0
## 28                                         0
## 29                                       0.2
## 30                                       n/a
## 31                                       n/a
##    Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                               0 NA  NA  NA  NA  NA  NA
## 2                                     0.034482759 NA  NA  NA  NA  NA  NA
## 3                                     0.285714286 NA  NA  NA  NA  NA  NA
## 4                                               0 NA  NA  NA  NA  NA  NA
## 5                                     0.222222222 NA  NA  NA  NA  NA  NA
## 6                                      0.03960396 NA  NA  NA  NA  NA  NA
## 7                                     0.008695652 NA  NA  NA  NA  NA  NA
## 8                                     0.222222222 NA  NA  NA  NA  NA  NA
## 9                                     0.043478261 NA  NA  NA  NA  NA  NA
## 10                                    0.043478261 NA  NA  NA  NA  NA  NA
## 11                                              0 NA  NA  NA  NA  NA  NA
## 12                                    0.136363636 NA  NA  NA  NA  NA  NA
## 13                                              0 NA  NA  NA  NA  NA  NA
## 14                                              0 NA  NA  NA  NA  NA  NA
## 15                                    0.192307692 NA  NA  NA  NA  NA  NA
## 16                                              0 NA  NA  NA  NA  NA  NA
## 17                                              0 NA  NA  NA  NA  NA  NA
## 18                                              0 NA  NA  NA  NA  NA  NA
## 19                                            0.5 NA  NA  NA  NA  NA  NA
## 20                                              0 NA  NA  NA  NA  NA  NA
## 21                                              0 NA  NA  NA  NA  NA  NA
## 22                                            n/a NA  NA  NA  NA  NA  NA
## 23                                            n/a NA  NA  NA  NA  NA  NA
## 24                                    0.000143802 NA  NA  NA  NA  NA  NA
## 25                                            n/a NA  NA  NA  NA  NA  NA
## 26                                              0 NA  NA  NA  NA  NA  NA
## 27                                              0 NA  NA  NA  NA  NA  NA
## 28                                              0 NA  NA  NA  NA  NA  NA
## 29                                    0.103896104 NA  NA  NA  NA  NA  NA
## 30                                            n/a NA  NA  NA  NA  NA  NA
## 31                                            n/a NA  NA  NA  NA  NA  NA
##    X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1   NA  NA  NA  NA   NA   NA   NA   NA
## 2   NA  NA  NA  NA   NA   NA   NA   NA
## 3   NA  NA  NA  NA   NA   NA   NA   NA
## 4   NA  NA  NA  NA   NA   NA   NA   NA
## 5   NA  NA  NA  NA   NA   NA   NA   NA
## 6   NA  NA  NA  NA   NA   NA   NA   NA
## 7   NA  NA  NA  NA   NA   NA   NA   NA
## 8   NA  NA  NA  NA   NA   NA   NA   NA
## 9   NA  NA  NA  NA   NA   NA   NA   NA
## 10  NA  NA  NA  NA   NA   NA   NA   NA
## 11  NA  NA  NA  NA   NA   NA   NA   NA
## 12  NA  NA  NA  NA   NA   NA   NA   NA
## 13  NA  NA  NA  NA   NA   NA   NA   NA
## 14  NA  NA  NA  NA   NA   NA   NA   NA
## 15  NA  NA  NA  NA   NA   NA   NA   NA
## 16  NA  NA  NA  NA   NA   NA   NA   NA
## 17  NA  NA  NA  NA   NA   NA   NA   NA
## 18  NA  NA  NA  NA   NA   NA   NA   NA
## 19  NA  NA  NA  NA   NA   NA   NA   NA
## 20  NA  NA  NA  NA   NA   NA   NA   NA
## 21  NA  NA  NA  NA   NA   NA   NA   NA
## 22  NA  NA  NA  NA   NA   NA   NA   NA
## 23  NA  NA  NA  NA   NA   NA   NA   NA
## 24  NA  NA  NA  NA   NA   NA   NA   NA
## 25  NA  NA  NA  NA   NA   NA   NA   NA
## 26  NA  NA  NA  NA   NA   NA   NA   NA
## 27  NA  NA  NA  NA   NA   NA   NA   NA
## 28  NA  NA  NA  NA   NA   NA   NA   NA
## 29  NA  NA  NA  NA   NA   NA   NA   NA
## 30  NA  NA  NA  NA   NA   NA   NA   NA
## 31  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From3500to2900BP$Longitude..DD.
lati <- From3500to2900BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From3500to2900BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From3500to2900BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-40-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-41-1.pdf)<!-- --> 

#2900-2200

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/2900-2200%20BP.csv")
From2900to2200BP <- read.csv(f, header = TRUE)
head(From2900to2200BP)
```

```
##   Country          Region    Site Zone Latitude..DD. Longitude..DD.
## 1  Mexico Tehuacan Valley  Ts 368    I      18.08659      -97.12971
## 2  Mexico Tehuacan Valley Ts 368e    I      18.08659      -97.12971
## 3  Mexico Tehuacan Valley  Ts 368    H      18.08659      -97.12971
## 4  Mexico Tehuacan Valley Ts 368e    H      18.08659      -97.12971
## 5  Mexico Tehuacan Valley  Ts 368    G      18.08659      -97.12971
## 6  Mexico Tehuacan Valley Ts 368e    G      18.08659      -97.12971
##                            Period       Phase Start.BP End.BP
## 1 Middle Formative-Late Formative Santa Maria     2900   2200
## 2 Middle Formative-Late Formative Santa Maria     2900   2200
## 3 Middle Formative-Late Formative Santa Maria     2900   2200
## 4 Middle Formative-Late Formative Santa Maria     2900   2200
## 5 Middle Formative-Late Formative Santa Maria     2900   2200
## 6 Middle Formative-Late Formative Santa Maria     2900   2200
##   Radiocarbon.Dates..BP.  Sourc.es. Table.s..Included Total.Chipped.Stone
## 1                    n/a Byers 1967                 1                   4
## 2                    n/a Byers 1967         6,8,10,12                  36
## 3                    n/a Byers 1967                 1                   7
## 4                    n/a Byers 1967         6,8,10,12                  23
## 5                    n/a Byers 1967                 1                   8
## 6                    n/a Byers 1967         6,8,10,12                  48
##   Total.Chert Total.Obsidian Total.Blade
## 1                          4           4
## 2          24              1           0
## 3           1              6           6
## 4          17              1            
## 5                          8           8
## 6          45              1           0
##   Total.Obstdion.Total.Chipped.Stone
## 1                                  1
## 2                        0.027777778
## 3                        0.857142857
## 4                        0.043478261
## 5                                  1
## 6                        0.020833333
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         1
## 2                                         0
## 3                                         1
## 4                                         0
## 5                                         1
## 6                                         0
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                              1 NA  NA  NA  NA  NA  NA
## 2                                              0 NA  NA  NA  NA  NA  NA
## 3                                    0.857142857 NA  NA  NA  NA  NA  NA
## 4                                              0 NA  NA  NA  NA  NA  NA
## 5                                              1 NA  NA  NA  NA  NA  NA
## 6                                              0 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From2900to2200BP$Longitude..DD.
lati <- From2900to2200BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From2900to2200BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From2900to2200BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-45-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-46-1.pdf)<!-- --> 

#2200-1300

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/2200-1300%20BP.csv")
From2200to1300BP <- read.csv(f, header = TRUE)
head(From2200to1300BP)
```

```
##   Country          Region      Site Zone Latitude..DD. Longitude..DD.
## 1  Mexico Tehuacan Valley    Tc 307    A      18.03543      -97.09124
## 2  Mexico Tehuacan Valley  Ts 218-6    A      18.07303      -97.14956
## 3  Mexico Tehuacan Valley Ts 218-10    A      18.07303      -97.14956
## 4  Mexico Tehuacan Valley Ts 218-11    A      18.07303      -97.14956
## 5  Mexico Tehuacan Valley    Tc 272    F      18.03543      -97.21887
## 6  Mexico Tehuacan Valley     Tc 50   VI      18.44056      -97.21887
##                   Period       Phase Start.BP End.BP
## 1 Late Formative-Classic Palo Blanco     2200   1300
## 2 Late Formative-Classic Palo Blanco     2200   1300
## 3 Late Formative-Classic Palo Blanco     2200   1300
## 4 Late Formative-Classic Palo Blanco     2200   1300
## 5 Late Formative-Classic Palo Blanco     2200   1300
## 6 Late Formative-Classic Palo Blanco     2200   1300
##   Radiocarbon.Dates..BP.  Sourc.es. Table.s..Included Total.Chipped.Stone
## 1                    n/a Byers 1967         1,8,10,12                   7
## 2                    n/a Byers 1967       1,6,8,10,12                 114
## 3                    n/a Byers 1967          1,6,8,12                 448
## 4                    n/a Byers 1967          1,6,8,12                  14
## 5                    n/a Byers 1967            6,8,10                   3
## 6                    n/a Byers 1967       1,6,8,10,12                  60
##   Total.Chert Total.Obsidian Total.Blade
## 1           5              1           1
## 2          40             72          72
## 3         306            162         152
## 4          11              3           3
## 5           2              0           0
## 6          49              3           4
##   Total.Obstdion.Total.Chipped.Stone
## 1                        0.142857143
## 2                        0.631578947
## 3                        0.361607143
## 4                        0.214285714
## 5                                  0
## 6                               0.05
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                         1
## 2                                         1
## 3                               0.938271605
## 4                                         1
## 5                                         0
## 6                               1.333333333
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                    0.142857143 NA  NA  NA  NA  NA  NA
## 2                                    0.631578947 NA  NA  NA  NA  NA  NA
## 3                                    0.339285714 NA  NA  NA  NA  NA  NA
## 4                                    0.214285714 NA  NA  NA  NA  NA  NA
## 5                                              0 NA  NA  NA  NA  NA  NA
## 6                                    0.066666667 NA  NA  NA  NA  NA  NA
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From2200to1300BP$Longitude..DD.
lati <- From2200to1300BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From2200to1300BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From2200to1300BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-50-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-51-1.pdf)<!-- --> 
#1300-460

```r
f <- curl("https://raw.githubusercontent.com/andym5780/Wave-of-Advance-Model-for-Predicting-the-Spread-of-Prismatic-Blade/master/Dates-Divided/1300-460%20BP.csv")
From1300to460BP <- read.csv(f, header = TRUE)
head(From1300to460BP)
```

```
##   Country          Region   Site Zone Latitude..DD. Longitude..DD.
## 1  Mexico Tehuacan Valley Tc 35e    C      18.44109      -97.54070
## 2  Mexico Tehuacan Valley Tc 255    A      18.38074      -97.39076
## 3  Mexico Tehuacan Valley  Tc 50  III      18.44056      -97.21887
## 4  Mexico Tehuacan Valley Tc 35e    B      18.44109      -97.54070
## 5  Mexico Tehuacan Valley Tc 35w    3      18.44109      -97.54070
## 6  Mexico Tehuacan Valley  Tc 50   II      18.44056      -97.21887
##                  Period        Phase Start.BP End.BP
## 1 Classical-Postclassic Venta Salada     1300    460
## 2 Classical-Postclassic Venta Salada     1300    460
## 3 Classical-Postclassic Venta Salada     1300    460
## 4 Classical-Postclassic Venta Salada     1300    460
## 5 Classical-Postclassic Venta Salada     1300    460
## 6 Classical-Postclassic Venta Salada     1300    460
##   Radiocarbon.Dates..BP.  Sourc.es. Table.s..Included Total.Chipped.Stone
## 1                    n/a Byers 1967       1,6,8,10,12                 100
## 2                    n/a Byers 1967          1,6,8,12                  56
## 3                    n/a Byers 1967       1,6,8,10,12                  17
## 4                    n/a Byers 1967       1,6,8,10,12                  47
## 5                    n/a Byers 1967       1,6,8,10,12                 180
## 6                    n/a Byers 1967       1,6,8,10,12                  76
##   Total.Chert Total.Obsidian Total.Blade
## 1          41             41          41
## 2          49              7           7
## 3           6              7           7
## 4          20             24          23
## 5          96             56          56
## 6          14             59          58
##   Total.Obstdion.Total.Chipped.Stone
## 1                               0.41
## 2                              0.125
## 3                        0.411764706
## 4                        0.510638298
## 5                        0.311111111
## 6                        0.776315789
##   Blade.Ratio..Total.Blades.Total.Obsidian.
## 1                                 1.0000000
## 2                                 1.0000000
## 3                                 1.0000000
## 4                                 0.9583333
## 5                                 1.0000000
## 6                                 0.9830508
##   Blade.Ratio..Total.Blades.Total.Chipped.Stone.  X X.1 X.2 X.3 X.4 X.5
## 1                                           0.41 NA  NA  NA  NA        
## 2                                          0.125 NA  NA  NA  NA        
## 3                                    0.411764706 NA  NA  NA  NA        
## 4                                    0.489361702 NA  NA  NA  NA        
## 5                                    0.311111111 NA  NA  NA  NA        
## 6                                    0.763157895 NA  NA  NA  NA        
##   X.6 X.7 X.8 X.9 X.10 X.11 X.12 X.13
## 1  NA  NA  NA  NA   NA   NA   NA   NA
## 2  NA  NA  NA  NA   NA   NA   NA   NA
## 3  NA  NA  NA  NA   NA   NA   NA   NA
## 4  NA  NA  NA  NA   NA   NA   NA   NA
## 5  NA  NA  NA  NA   NA   NA   NA   NA
## 6  NA  NA  NA  NA   NA   NA   NA   NA
```


```r
long <- From1300to460BP$Longitude..DD.
lati <- From1300to460BP$Latitude..DD.
```


```r
myMap <- get_map(location = c(lon = -93.5,lat = 17), source = "google", maptype = "terrain", color = "bw", crop=TRUE, zoom = 5)
```

```
## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=17,-93.5&zoom=5&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false
```


```r
map <- ggmap(myMap) + geom_point(aes(x = long, y = lati, size = 1, color = From1300to460BP$Blade.Ratio..Total.Blades.Total.Obsidian.), data = From1300to460BP) + xlim(-100,-87) + ylim(14,20) + guides(fill=FALSE)
```

```
## Scale for 'x' is already present. Adding another scale for 'x', which
## will replace the existing scale.
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which
## will replace the existing scale.
```

```r
map
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-55-1.pdf)<!-- --> 


```r
newmap <- map + theme(legend.position="none")
newmap
```

```
## Warning: Removed 1 rows containing missing values (geom_rect).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](Wave-of-Advance-Model_files/figure-latex/unnamed-chunk-56-1.pdf)<!-- --> 












