v1

1.  Dataset: asthma_longit_visits_v4.csv, comorbidities
2.  recode of race according to nih
3.  Weekdays added
4.  atopic march for AD, AR, EoE
5.  Longitudinal comorbidities added:

-   G47.3 Sleep apnea
-   E66.0, E66.2, E66.3, E66.8, E66.9 Obesity
-   J30.x Allergic rhinitis
-   K20.0 EoE
-   K21.x Gastro-esophageal reflux disease
-   L20.x AD
-   Z91.01 Z91.02 Food/Food additives allergy status

1.  Readmission at 7 and 14 days.

v2

1.  age at visit\>18 years old

v3

week added for season as a spline sample size at each stage new
comorbidities added: \* E11.x Type 2 Diabetes \* I10.x Hypertension \*
I25.x CIHD \* J44.x COPD \* F32.x and F33.x Depression \* C00x-D49
Neoplasm (Cancer) \* N18.x Chronic Kidney Disease (CKD)

v4

1.  Readmission within 30 days for readmission analysis.
2.  Race and ethnicity collapsed.

v6 Make use of v6 visits version race/ethnicity already comes collapsed
into race2 field

v7 Drop NHPI \<20 participants

v8 Health insurance info collapsed and added

v9 code clean up

# Insurance

Insurance info comes from a survey instead of EHR. We need to
pre-process and join to each patient.

    ## # A tibble: 18,931 × 2
    ##    person_id     n
    ##        <int> <int>
    ##  1   1000109     1
    ##  2   1000151     2
    ##  3   1000185     2
    ##  4   1000234     1
    ##  5   1000396     2
    ##  6   1000612     1
    ##  7   1000709     1
    ##  8   1000724     2
    ##  9   1000843     1
    ## 10   1000955     1
    ## # ℹ 18,921 more rows

    ##                            
    ##                             Employer Or Union       Indian      Invalid
    ##                                  0.000000e+00 0.000000e+00 0.000000e+00
    ##   No                             0.000000e+00 0.000000e+00 0.000000e+00
    ##   PMI: Dont Know                 3.876136e-04 4.306818e-05 0.000000e+00
    ##   PMI: Prefer Not To Answer      5.168181e-04 0.000000e+00 0.000000e+00
    ##   PMI: Skip                      1.248977e-03 8.613635e-05 0.000000e+00
    ##   Yes                            1.989750e-01 5.598863e-04 2.971704e-03
    ##                            
    ##                                 Medi GAP     Medicaid     Medicare     Military
    ##                             0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
    ##   No                        0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
    ##   PMI: Dont Know            0.000000e+00 2.110341e-03 4.737499e-04 4.306818e-05
    ##   PMI: Prefer Not To Answer 0.000000e+00 2.110341e-03 3.876136e-04 0.000000e+00
    ##   PMI: Skip                 4.306818e-05 2.928636e-03 1.808863e-03 1.722727e-04
    ##   Yes                       2.497954e-03 3.210733e-01 2.588397e-01 9.905681e-03
    ##                            
    ##                              No Coverage         None Other Government
    ##                             0.000000e+00 0.000000e+00     0.000000e+00
    ##   No                        0.000000e+00 0.000000e+00     0.000000e+00
    ##   PMI: Dont Know            4.306818e-05 1.162841e-03     4.306818e-05
    ##   PMI: Prefer Not To Answer 0.000000e+00 9.044317e-04     0.000000e+00
    ##   PMI: Skip                 0.000000e+00 2.153409e-04     0.000000e+00
    ##   Yes                       4.306818e-05 1.938068e-03     3.488522e-03
    ##                            
    ##                             Other Health Plan PMI: Dont Know
    ##                                  0.000000e+00   0.000000e+00
    ##   No                             0.000000e+00   0.000000e+00
    ##   PMI: Dont Know                 3.876136e-04   8.613635e-05
    ##   PMI: Prefer Not To Answer      3.014772e-04   0.000000e+00
    ##   PMI: Skip                      3.014772e-04   0.000000e+00
    ##   Yes                            2.291227e-02   8.182954e-04
    ##                            
    ##                             PMI: Prefer Not To Answer    PMI: Skip      Private
    ##                                          0.000000e+00 4.306818e-05 8.613635e-05
    ##   No                                     0.000000e+00 3.014772e-02 0.000000e+00
    ##   PMI: Dont Know                         0.000000e+00 6.890908e-04 4.306818e-05
    ##   PMI: Prefer Not To Answer              1.722727e-04 2.584091e-03 4.306818e-05
    ##   PMI: Skip                              0.000000e+00 1.550454e-03 1.292045e-04
    ##   Yes                                    5.598863e-04 6.331022e-03 3.906284e-02
    ##                            
    ##                                Purchased        Schip Single Service
    ##                             0.000000e+00 0.000000e+00   0.000000e+00
    ##   No                        0.000000e+00 0.000000e+00   0.000000e+00
    ##   PMI: Dont Know            2.153409e-04 0.000000e+00   0.000000e+00
    ##   PMI: Prefer Not To Answer 2.153409e-04 0.000000e+00   4.306818e-05
    ##   PMI: Skip                 3.445454e-04 0.000000e+00   4.306818e-05
    ##   Yes                       5.129420e-02 1.292045e-04   4.220681e-03
    ##                            
    ##                             State Sponsored           VA
    ##                                4.306818e-05 0.000000e+00
    ##   No                           0.000000e+00 0.000000e+00
    ##   PMI: Dont Know               4.306818e-05 8.613635e-05
    ##   PMI: Prefer Not To Answer    4.306818e-05 4.306818e-05
    ##   PMI: Skip                    8.613635e-05 1.722727e-04
    ##   Yes                          6.632499e-03 1.507386e-02

    ##                                                  No            PMI: Dont Know 
    ##                         4                       700                       136 
    ## PMI: Prefer Not To Answer                 PMI: Skip                       Yes 
    ##                       171                       212                     21996

    ##   Yes 
    ## 21748

    ##  [1] "Employer Or Union" "Indian"            "Medi GAP"         
    ##  [4] "Medicaid"          "Medicare"          "Military"         
    ##  [7] "No Coverage"       "None"              "Other Government" 
    ## [10] "Other Health Plan" "Private"           "Purchased"        
    ## [13] "Schip"             "Single Service"    "State Sponsored"  
    ## [16] "VA"

![](01_asthmaaou_preprocessing_v9_files/figure-markdown_github/unnamed-chunk-4-1.png)![](01_asthmaaou_preprocessing_v9_files/figure-markdown_github/unnamed-chunk-4-2.png)![](01_asthmaaou_preprocessing_v9_files/figure-markdown_github/unnamed-chunk-4-3.png)

    ##                        
    ##                         level   Overall        
    ##   n                     ""      "17,501"       
    ##   Employer Or Union (%) "FALSE" "12881 (73.6)" 
    ##                         "TRUE"  "4620 (26.4)"  
    ##   Indian (%)            "FALSE" "17488 (99.9)" 
    ##                         "TRUE"  "13 (0.1)"     
    ##   Medi GAP (%)          "FALSE" "17443 (99.7)" 
    ##                         "TRUE"  "58 (0.3)"     
    ##   Medicaid (%)          "FALSE" "10046 (57.4)" 
    ##                         "TRUE"  "7455 (42.6)"  
    ##   Medicare (%)          "FALSE" "11491 (65.7)" 
    ##                         "TRUE"  "6010 (34.3)"  
    ##   Military (%)          "FALSE" "17271 (98.7)" 
    ##                         "TRUE"  "230 (1.3)"    
    ##   No Coverage (%)       "FALSE" "17500 (100.0)"
    ##                         "TRUE"  "1 (0.0)"      
    ##   None (%)              "FALSE" "17456 (99.7)" 
    ##                         "TRUE"  "45 (0.3)"     
    ##   Other Government (%)  "FALSE" "17420 (99.5)" 
    ##                         "TRUE"  "81 (0.5)"     
    ##   Other Health Plan (%) "FALSE" "16969 (97.0)" 
    ##                         "TRUE"  "532 (3.0)"    
    ##   Private (%)           "FALSE" "16594 (94.8)" 
    ##                         "TRUE"  "907 (5.2)"    
    ##   Purchased (%)         "FALSE" "16310 (93.2)" 
    ##                         "TRUE"  "1191 (6.8)"   
    ##   Schip (%)             "FALSE" "17498 (100.0)"
    ##                         "TRUE"  "3 (0.0)"      
    ##   Single Service (%)    "FALSE" "17403 (99.4)" 
    ##                         "TRUE"  "98 (0.6)"     
    ##   State Sponsored (%)   "FALSE" "17347 (99.1)" 
    ##                         "TRUE"  "154 (0.9)"    
    ##   VA (%)                "FALSE" "17151 (98.0)" 
    ##                         "TRUE"  "350 (2.0)"

|                       | level | Overall       |
|:----------------------|:------|:--------------|
| n                     |       | 17501         |
| Employer Or Union (%) | FALSE | 12881 ( 73.6) |
|                       | TRUE  | 4620 ( 26.4)  |
| Indian (%)            | FALSE | 17488 ( 99.9) |
|                       | TRUE  | 13 ( 0.1)     |
| Medi GAP (%)          | FALSE | 17443 ( 99.7) |
|                       | TRUE  | 58 ( 0.3)     |
| Medicaid (%)          | FALSE | 10046 ( 57.4) |
|                       | TRUE  | 7455 ( 42.6)  |
| Medicare (%)          | FALSE | 11491 ( 65.7) |
|                       | TRUE  | 6010 ( 34.3)  |
| Military (%)          | FALSE | 17271 ( 98.7) |
|                       | TRUE  | 230 ( 1.3)    |
| No Coverage (%)       | FALSE | 17500 (100.0) |
|                       | TRUE  | 1 ( 0.0)      |
| None (%)              | FALSE | 17456 ( 99.7) |
|                       | TRUE  | 45 ( 0.3)     |
| Other Government (%)  | FALSE | 17420 ( 99.5) |
|                       | TRUE  | 81 ( 0.5)     |
| Other Health Plan (%) | FALSE | 16969 ( 97.0) |
|                       | TRUE  | 532 ( 3.0)    |
| Private (%)           | FALSE | 16594 ( 94.8) |
|                       | TRUE  | 907 ( 5.2)    |
| Purchased (%)         | FALSE | 16310 ( 93.2) |
|                       | TRUE  | 1191 ( 6.8)   |
| Schip (%)             | FALSE | 17498 (100.0) |
|                       | TRUE  | 3 ( 0.0)      |
| Single Service (%)    | FALSE | 17403 ( 99.4) |
|                       | TRUE  | 98 ( 0.6)     |
| State Sponsored (%)   | FALSE | 17347 ( 99.1) |
|                       | TRUE  | 154 ( 0.9)    |
| VA (%)                | FALSE | 17151 ( 98.0) |
|                       | TRUE  | 350 ( 2.0)    |

    ##                        Stratified by hcType1
    ##                         level   Private        Public          p        test
    ##   n                     ""      "5,403"        "12,098"        ""       ""  
    ##   Employer Or Union (%) "FALSE" "1737 (32.1)"  "11144 (92.1)"  "<0.001" ""  
    ##                         "TRUE"  "3666 (67.9)"  "954 (7.9)"     ""       ""  
    ##   Indian (%)            "FALSE" "5403 (100.0)" "12085 (99.9)"  "0.035"  ""  
    ##                         "TRUE"  "0 (0.0)"      "13 (0.1)"      ""       ""  
    ##   Medi GAP (%)          "FALSE" "5403 (100.0)" "12040 (99.5)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "58 (0.5)"      ""       ""  
    ##   Medicaid (%)          "FALSE" "5398 (99.9)"  "4648 (38.4)"   "<0.001" ""  
    ##                         "TRUE"  "5 (0.1)"      "7450 (61.6)"   ""       ""  
    ##   Medicare (%)          "FALSE" "5370 (99.4)"  "6121 (50.6)"   "<0.001" ""  
    ##                         "TRUE"  "33 (0.6)"     "5977 (49.4)"   ""       ""  
    ##   Military (%)          "FALSE" "5403 (100.0)" "11868 (98.1)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "230 (1.9)"     ""       ""  
    ##   No Coverage (%)       "FALSE" "5403 (100.0)" "12097 (100.0)" "1.000"  ""  
    ##                         "TRUE"  "0 (0.0)"      "1 (0.0)"       ""       ""  
    ##   None (%)              "FALSE" "5403 (100.0)" "12053 (99.6)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "45 (0.4)"      ""       ""  
    ##   Other Government (%)  "FALSE" "5403 (100.0)" "12017 (99.3)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "81 (0.7)"      ""       ""  
    ##   Other Health Plan (%) "FALSE" "4871 (90.2)"  "12098 (100.0)" "<0.001" ""  
    ##                         "TRUE"  "532 (9.8)"    "0 (0.0)"       ""       ""  
    ##   Private (%)           "FALSE" "4709 (87.2)"  "11885 (98.2)"  "<0.001" ""  
    ##                         "TRUE"  "694 (12.8)"   "213 (1.8)"     ""       ""  
    ##   Purchased (%)         "FALSE" "4856 (89.9)"  "11454 (94.7)"  "<0.001" ""  
    ##                         "TRUE"  "547 (10.1)"   "644 (5.3)"     ""       ""  
    ##   Schip (%)             "FALSE" "5403 (100.0)" "12095 (100.0)" "0.594"  ""  
    ##                         "TRUE"  "0 (0.0)"      "3 (0.0)"       ""       ""  
    ##   Single Service (%)    "FALSE" "5342 (98.9)"  "12061 (99.7)"  "<0.001" ""  
    ##                         "TRUE"  "61 (1.1)"     "37 (0.3)"      ""       ""  
    ##   State Sponsored (%)   "FALSE" "5403 (100.0)" "11944 (98.7)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "154 (1.3)"     ""       ""  
    ##   VA (%)                "FALSE" "5403 (100.0)" "11748 (97.1)"  "<0.001" ""  
    ##                         "TRUE"  "0 (0.0)"      "350 (2.9)"     ""       ""

|                       | level | Private      | Public        | p       | test |
|:----------------------|:------|:-------------|:--------------|:--------|:-----|
| n                     |       | 5403         | 12098         |         |      |
| Employer Or Union (%) | FALSE | 1737 ( 32.1) | 11144 ( 92.1) | \<0.001 |      |
|                       | TRUE  | 3666 ( 67.9) | 954 ( 7.9)    |         |      |
| Indian (%)            | FALSE | 5403 (100.0) | 12085 ( 99.9) | 0.035   |      |
|                       | TRUE  | 0 ( 0.0)     | 13 ( 0.1)     |         |      |
| Medi GAP (%)          | FALSE | 5403 (100.0) | 12040 ( 99.5) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 58 ( 0.5)     |         |      |
| Medicaid (%)          | FALSE | 5398 ( 99.9) | 4648 ( 38.4)  | \<0.001 |      |
|                       | TRUE  | 5 ( 0.1)     | 7450 ( 61.6)  |         |      |
| Medicare (%)          | FALSE | 5370 ( 99.4) | 6121 ( 50.6)  | \<0.001 |      |
|                       | TRUE  | 33 ( 0.6)    | 5977 ( 49.4)  |         |      |
| Military (%)          | FALSE | 5403 (100.0) | 11868 ( 98.1) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 230 ( 1.9)    |         |      |
| No Coverage (%)       | FALSE | 5403 (100.0) | 12097 (100.0) | 1.000   |      |
|                       | TRUE  | 0 ( 0.0)     | 1 ( 0.0)      |         |      |
| None (%)              | FALSE | 5403 (100.0) | 12053 ( 99.6) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 45 ( 0.4)     |         |      |
| Other Government (%)  | FALSE | 5403 (100.0) | 12017 ( 99.3) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 81 ( 0.7)     |         |      |
| Other Health Plan (%) | FALSE | 4871 ( 90.2) | 12098 (100.0) | \<0.001 |      |
|                       | TRUE  | 532 ( 9.8)   | 0 ( 0.0)      |         |      |
| Private (%)           | FALSE | 4709 ( 87.2) | 11885 ( 98.2) | \<0.001 |      |
|                       | TRUE  | 694 ( 12.8)  | 213 ( 1.8)    |         |      |
| Purchased (%)         | FALSE | 4856 ( 89.9) | 11454 ( 94.7) | \<0.001 |      |
|                       | TRUE  | 547 ( 10.1)  | 644 ( 5.3)    |         |      |
| Schip (%)             | FALSE | 5403 (100.0) | 12095 (100.0) | 0.594   |      |
|                       | TRUE  | 0 ( 0.0)     | 3 ( 0.0)      |         |      |
| Single Service (%)    | FALSE | 5342 ( 98.9) | 12061 ( 99.7) | \<0.001 |      |
|                       | TRUE  | 61 ( 1.1)    | 37 ( 0.3)     |         |      |
| State Sponsored (%)   | FALSE | 5403 (100.0) | 11944 ( 98.7) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 154 ( 1.3)    |         |      |
| VA (%)                | FALSE | 5403 (100.0) | 11748 ( 97.1) | \<0.001 |      |
|                       | TRUE  | 0 ( 0.0)     | 350 ( 2.9)    |         |      |

Join to patients:

# Comorbidities

Let’s load the already processed from all of us:

    ## [1] "Emergency Room - Hospital"          "Emergency Room and Inpatient Visit"
    ## [3] "Emergency Room Visit"               "Hospital"                          
    ## [5] "Inpatient Hospital"                 "Inpatient Visit"                   
    ## [7] "Observation Room"

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##    0.000    0.000    1.000    2.066    2.000 2833.000

    ## [1] 13.23068

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   0.000   0.000   1.000   1.962   2.000 132.000

#Proportions checking

    ##                    V1          V2          V3          V4          V5
    ## race3        White_EA       Asian    Black_AA          HL        MENA
    ## n                8716         232        6084        3822          63
    ## prop      46.03845341  1.22543841 32.13606592 20.18804141  0.33276991
    ## prop2            46.0         1.2        32.1        20.2         0.3
    ## prop2text   8716 (46)   232 (1.2) 6084 (32.1) 3822 (20.2)    63 (0.3)
    ##                    V6
    ## race3            NHPI
    ## n                  15
    ## prop       0.07923093
    ## prop2             0.1
    ## prop2text    15 (0.1)

    ##                     V1         V2           V3           V4         V5
    ## race3         White_EA      Asian     Black_AA           HL       MENA
    ## n                32788        597        39349        19746        269
    ## prop        35.3132505  0.6429795   42.3795625   21.2667880  0.2897177
    ## prop2             35.3        0.6         42.4         21.3        0.3
    ## prop2text 32788 (35.3)  597 (0.6) 39349 (42.4) 19746 (21.3)  269 (0.3)
    ##                   V6
    ## race3           NHPI
    ## n                100
    ## prop       0.1077018
    ## prop2            0.1
    ## prop2text  100 (0.1)

    ##                    V1          V2          V3          V4          V5
    ## race3        White_EA       Asian    Black_AA          HL        MENA
    ## n                8278         216        5299        3397          56
    ## prop      47.95782400  1.25137593 30.69926424 19.68020393  0.32443080
    ## prop2            48.0         1.3        30.7        19.7         0.3
    ## prop2text   8278 (48)   216 (1.3) 5299 (30.7) 3397 (19.7)    56 (0.3)
    ##                    V6
    ## race3            NHPI
    ## n                  15
    ## prop       0.08690111
    ## prop2             0.1
    ## prop2text    15 (0.1)

    ##                     V1         V2           V3           V4         V5
    ## race3         White_EA      Asian     Black_AA           HL       MENA
    ## n                30775        535        33683        17070        242
    ## prop        33.1452143  0.5762044   36.2771812   18.3846891  0.2606382
    ## prop2             33.1        0.6         36.3         18.4        0.3
    ## prop2text 30775 (33.1)  535 (0.6) 33683 (36.3) 17070 (18.4)  242 (0.3)
    ##                   V6
    ## race3           NHPI
    ## n                100
    ## prop       0.1077018
    ## prop2            0.1
    ## prop2text  100 (0.1)
