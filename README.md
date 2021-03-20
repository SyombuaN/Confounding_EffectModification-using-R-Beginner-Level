# Stratified Analysis: Beginner Level 
## *Confounding vs Effect Modification*
 
## Stratified Analysis to study association between stroke and angina with BMI as a 3rd variable
   *Exposure=Angina, Outcome= Stroke*

framdata<-read.csv(file.choose(),header = T)
names(framdata)
attach(framdata)

## Categorize individuals into 2 groups of BMI : normal or over weight 
BMI_category<-ifelse(BMI >25,"Overweight","Normal Weight")
table(BMI_category)
### Output
 BMI_category
 Normal Weight    Overweight 
         1449          1785 
## *Install and load epiR Package*
install.packages("epiR")
library(epiR)

## Create contigency tables
epitables<-table(framdata$STROKE,framdata$ANGINA,BMI_category)
rownames(epitables)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(epitables)<-c("STROKE","NO STROKE") #OUTCOME 
epitables 
   ### Output
, , BMI_category = Normal Weight
              STROKE NO STROKE
  ANGINA      1166       167
  NO ANGINA     91        25

, , BMI_category = Overweight
              STROKE NO STROKE
  ANGINA      1343       280
  NO ANGINA    126        36


## Prepare the matrix table using the information from the output
## for normal weight people (BMI<25)
normal_wt<-matrix(c(1166,167,91,25),nrow = 2,byrow = T)
rownames(normal_wt)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(normal_wt)<-c("STROKE","NO STROKE") #OUTCOME 
normal_wt

## for overweight individuals (BMI >25)
over_wt<-matrix(c(1341,280,126,36),nrow = 2,byrow = T)
rownames(over_wt)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(over_wt)<-c("STROKE","NO STROKE") #OUTCOME 
over_wt

## Stratified Odds Ratios and Risk Ratios
epi.2by2(epitables,method = "cohort.count") #crude/unadjusted
epi.2by2(normal_wt,method = "cohort.count") #adjusted for normal weight individuals
epi.2by2(over_wt,method = "cohort.count") #adjusted for overweight individuals

### Output
> epi.2by2(epitables,method = "cohort.count") #crude/unadjusted
              Outcome +      Outcome -      Total
Exposed +         2509          447         2956
Exposed -          217           61          278
Total             2726          508         3234
                 Inc risk *        Odds
Exposed +              84.9        5.61
Exposed -              78.1        3.56
Total                  84.3        5.37


Point estimates and 95% CIs:
-------------------------------------------------------------------
Inc risk ratio (crude)                       1.09 (1.02, 1.16)
Inc risk ratio (M-H)                         1.09 (1.02, 1.16)
Inc risk ratio (crude:M-H)                   1.00
Odds ratio (crude)                           1.58 (1.17, 2.13)
Odds ratio (M-H)                             1.56 (1.15, 2.11)
Odds ratio (crude:M-H)                       1.01
Attrib risk (crude) *                        6.82 (1.79, 11.85)
Attrib risk (M-H) *                          6.67 (-6.54, 19.89)
Attrib risk (crude:M-H)                      1.02
-------------------------------------------------------------------
 M-H test of homogeneity of RRs: chi2(1) = 0.504 Pr>chi2 = 0.48
 M-H test of homogeneity of ORs: chi2(1) = 1.194 Pr>chi2 = 0.27
 Test that M-H adjusted OR = 1:  chi2(1) = 8.514 Pr>chi2 = 0.00
 Wald confidence limits
 M-H: Mantel-Haenszel; CI: confidence interval
 * Outcomes per 100 population units 

> epi.2by2(normal_wt,method = "cohort.count") #adjusted for normal weight individuals
             Outcome +    Outcome -      Total
Exposed +         1166          167       1333
Exposed -           91           25        116
Total             1257          192       1449
                 Inc risk *        Odds
Exposed +              87.5        6.98
Exposed -              78.4        3.64
Total                  86.7        6.55

Point estimates and 95% CIs:
-------------------------------------------------------------------
Inc risk ratio                               1.12 (1.01, 1.23)
Odds ratio                                   1.92 (1.20, 3.07)
Attrib risk *                                9.02 (1.33, 16.71)
Attrib risk in population *                  8.30 (0.62, 15.98)
Attrib fraction in exposed (%)               10.32 (1.13, 18.65)
Attrib fraction in population (%)            9.57 (1.01, 17.39)
-------------------------------------------------------------------
 Test that OR = 1: chi2(1) = 7.559 Pr>chi2 = 0.01
 Wald confidence limits
 CI: confidence interval
 * Outcomes per 100 population units 

> epi.2by2(over_wt,method = "cohort.count") #adjusted for overweight individuals
             Outcome +    Outcome -      Total
Exposed +         1341          280       1621
Exposed -          126           36        162
Total             1467          316       1783
                 Inc risk *        Odds
Exposed +              82.7        4.79
Exposed -              77.8        3.50
Total                  82.3        4.64

Point estimates and 95% CIs:
-------------------------------------------------------------------
Inc risk ratio                               1.06 (0.98, 1.16)
Odds ratio                                   1.37 (0.92, 2.03)
Attrib risk *                                4.95 (-1.71, 11.61)
Attrib risk in population *                  4.50 (-2.14, 11.14)
Attrib fraction in exposed (%)               5.98 (-2.39, 13.67)
Attrib fraction in population (%)            5.47 (-2.19, 12.56)
-------------------------------------------------------------------
 Test that OR = 1: chi2(1) = 2.474 Pr>chi2 = 0.12
 Wald confidence limits
 CI: confidence interval
 * Outcomes per 100 population units 

detach(framdata)
