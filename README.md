# WMB-connectivity-
The goal of this project is to recreate a similar program to the Marxan tool.

# input data
- rec_class_all
- cia_dissolve = CIA_1, 2, and CIAP merged and dissolved 

- brwmp_targets.csv

# data prep
- clip BFRN AFLB layer
- use identity to join aflb_fact to rec_class_all
- re calculate AFLB_area

# recreate recuirtment tables
- based on provided AFLB layer
- indenity on recuirtment forest all layer to aff 'new' AFLB fact
- re calculate AFLB_area
- dissolve by WMB, Rec Cat, For Rep, stats for aflb_area_ha
- create pivot table for each WMB, cols= for rep, rows= rec cat, values= sum of aflb


# base data and addtional data needed for this analysis 
- merge all rec forest from RecruitmentForest_2025_01_14.gdb into one, and clip to priority WMB 
- for processing speed remove all fields accept for SIFA, SIFA2, FOR_REP, Rec_Cat 
- calculte for_rep_short    
    ```if FOR_REP =='High Productivity Coniferous Leading':
        return 'HPC'
    elif FOR_REP =='High Productivity Deciduous Leading':
        return 'HPD'
    elif FOR_REP =='High Productivity Mixedwood':
        return 'HPM'
    elif FOR_REP =='Low Productivity Coniferous Leading':
        return 'LPC'
    elif FOR_REP =='Low Productivity Deciduous Leading':
        return 'LPC'
    elif FOR_REP =='Low Productivity Mixedwood':
        return 'LPM'
    elif FOR_REP =='Medium Productivity Coniferous Leading':
        return 'MPC'
    elif FOR_REP == 'Medium Productivity Deciduous Leading':
        return 'MPD'
    elif FOR_REP == 'Medium Productivity Mixedwood':
        return 'MPM'
    ```
- Calculate 'recuirtment_poly_ha' area/10000
- AFLB recuirtment forest data treats aflb as 1:1, therfore all polygons from the rec data are considered to have an aflb of 1

- AFLB, merge AFLB from FAIB (AFLB_fact>0) with BFRN recruitment forest data and dissolve, calculate aflb_fact as 1 and calculate aflb_area



## running step 2
- the script can run step 2a and b
- step 2c can run from the script but it is quicker to do part of it manually 
    - from the data created in 2b, select polygons containing cultural lands or TSU an dnot anything in HV1 DZ
    - save seelction as ret_class_2c_intermidate.parquet
    - continue in script 

## step 3 script



## step 4 script



# old forest outside of conservation lands and aspatially deferred old forest
- query vri for stands >= than 140
- clip vri to priority wmb
- union wmb, tus, and thlb to vri selection
- recalculate area_ha, thlb_area_ha
- erase conservation lands and aspatially deferred old from union layer 
- recalculate area_ha, thlb_area_ha
- summarize erased area by wmb 

