Class\_11
================

\#set the first column as the row names not a data column

``` r
pdb.data <- read.csv("Data Export Summary.csv", row.names = 1)
pdb.data
```

    ##                          X.ray   NMR   EM Multiple.methods Neutron Other  Total
    ## Protein (only)          142419 11807 6038              177      70    32 160543
    ## Protein/Oligosaccharide   8426    31  991                5       0     0   9453
    ## Protein/NA                7498   274 2000                3       0     0   9775
    ## Nucleic acid (only)       2368  1378   60                8       2     1   3817
    ## Other                      149    31    3                0       0     0    183
    ## Oligosaccharide (only)      11     6    0                1       0     4     22

\#Q1: What percentage of structures in the PDB are solved by X-Ray and
Electron Microscopy.

``` r
XR <- sum(pdb.data[,1])
XR
```

    ## [1] 160871

``` r
EM <- sum(pdb.data[,3])
EM
```

    ## [1] 9092

``` r
round(sum(XR)/sum(pdb.data[,7])*100,2)
```

    ## [1] 87.53

``` r
round(sum(EM)/sum(pdb.data[,7])*100,2)
```

    ## [1] 4.95

\#How about doing this over every method (i.e column in the little
table)

\#can use pdb.data$“name of the colum” instead of col position also

``` r
round((colSums(pdb.data)/sum(pdb.data$Total))*100,2)
```

    ##            X.ray              NMR               EM Multiple.methods 
    ##            87.53             7.36             4.95             0.11 
    ##          Neutron            Other            Total 
    ##             0.04             0.02           100.00

\#Q2: What proportion of structures in the PDB are protein?

``` r
pdb.data$Total[1]
```

    ## [1] 160543

``` r
round((pdb.data$Total[1]/sum(pdb.data$Total))*100,2)
```

    ## [1] 87.35

# \#Q3: Type HIV in the PDB website search box on the home page and determine how many HIV-1 protease structures are in the current PDB?

![](1hsg.png)

\#Do a normal mode analysis (NMA) a prediction of the conformational
variability and intrinsic dynamics of this protein

``` r
library(bio3d)

pdb <- read.pdb("1hel")
```

    ##   Note: Accessing on-line PDB file

``` r
pdb
```

    ## 
    ##  Call:  read.pdb(file = "1hel")
    ## 
    ##    Total Models#: 1
    ##      Total Atoms#: 1186,  XYZs#: 3558  Chains#: 1  (values: A)
    ## 
    ##      Protein Atoms#: 1001  (residues/Calpha atoms#: 129)
    ##      Nucleic acid Atoms#: 0  (residues/phosphate atoms#: 0)
    ## 
    ##      Non-protein/nucleic Atoms#: 185  (residues: 185)
    ##      Non-protein/nucleic resid values: [ HOH (185) ]
    ## 
    ##    Protein sequence:
    ##       KVFGRCELAAAMKRHGLDNYRGYSLGNWVCAAKFESNFNTQATNRNTDGSTDYGILQINS
    ##       RWWCNDGRTPGSRNLCNIPCSALLSSDITASVNCAKKIVSDGNGMNAWVAWRNRCKGTDV
    ##       QAWIRGCRL
    ## 
    ## + attr: atom, xyz, seqres, helix, sheet,
    ##         calpha, remark, call

``` r
m <- nma(pdb)
```

    ##  Building Hessian...     Done in 0.024 seconds.
    ##  Diagonalizing Hessian...    Done in 0.264 seconds.

``` r
plot(m)
```

![](Class_11_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

\#make a little movie(trajectory) for viewing in VMD.

``` r
mktrj(m, file="nma.pdb")
```

add
