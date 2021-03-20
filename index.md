# Confounding_EffectModification: Beginner Level
 
## Stratified analysis to study association between stroke and angina with BMI as a 3rd variable
framdata<-read.csv(file.choose(),header = T)
names(framdata)
attach(framdata)

## Categorize individuals into 2 groups of BMI : normal or over weight 
BMI_category<-ifelse(BMI >25,"Overweight","Normal Weight")
table(BMI_category)

## *Install and load epiR Package*
library(epiR)
epitables<-table(framdata$STROKE,framdata$ANGINA,BMI_category)
rownames(epitables)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(epitables)<-c("STROKE","NO STROKE")   # OUTCOME 
epitables 

## Prepare the matrix table using the information and find the RRs and ORs
## for normal weight people (BMI<25)
normal_wt<-matrix(c(1166,167,91,25),nrow = 2,byrow = T)
rownames(normal_wt)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(normal_wt)<-c("STROKE","NO STROKE")  # OUTCOME 
normal_wt

## for overweight individuals (BMI >25)
over_wt<-matrix(c(1341,280,126,36),nrow = 2,byrow = T)
rownames(over_wt)<-c("ANGINA","NO ANGINA") #EXPOSURE
colnames(over_wt)<-c("STROKE","NO STROKE")   # OUTCOME 
over_wt

## Stratified Odds Ratios and Risk Ratios
epi.2by2(epitables,method = "cohort.count") #crude/unadjusted
epi.2by2(normal_wt,method = "cohort.count") #adjusted for normal weight individuals
epi.2by2(over_wt,method = "cohort.count") #adjusted for overweight individuals

detach(framdata)
