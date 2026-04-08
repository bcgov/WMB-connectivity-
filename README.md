# WMB-connectivity-
The goal of this project is to recreate a similar program to the Marxan tool.

## Methodology 
- Methodology follows Scenario5_Methodology_v3.0.docx

# input data
All spatail data was converted to geo parquets from various .gdbs
## spatial data
- AFLB = dissolved AFLB surface, with attribute aflb_fact (short/int), where aflb_fact=1, clip to priority WMBs
- rec_class_all = see below details on setting up layer 
- cia_1_p = merged CIA 1 and CIAP
- cia_2 = CIA 2 only 
- cultural_areas=merge of CIA1,2 and CIAP merged 
- cia_dissolve = cultural_areas dissolved into a single feature 
- priority_wmb = the 4 prioirty WMBs
- TSU_WMB = BFRN Traplines intersected with priority WMB, so each TSU is split by the WMBs
- hv1 = hv1 polygons (protection and development) clipped to the prioirty WMBs
- barriers = merge and dissolve of all PNG critical infrastrucutre and electrification infrastrucutures 
- cost_surface = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha
- hexagons_3ha = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- mineral_licks = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- headwaters = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- riparian = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- wetlands = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- moose = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- caribou_1 = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- caribou_2 = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha 
- fisher = provided layer from 2025_12_17_Marxan_Inputs_WMBs_3ha

## weights for MARXAN inputs
- dervided from 2026 01 22 Weighted Values Methods Overview.pdf

# tabular data
- brwmp_targets.csv


# rec_class_all set up
- merge all rec forest from RecruitmentForest_2025_01_14.gdb into one, and clip to priority WMB 
- calculte Rec_Cat_short (Rec_Cat_short is a typo that continued throughout the scripts Rec_Cat_short, should actually be For_Rep_Short but is hard coded intot to many places in the script)    
    ```
def get_short(FOR_REP):
    if FOR_REP =='High Productivity Coniferous Leading':
        return 'HPC'
    elif FOR_REP =='High Productivity Deciduous Leading':
        return 'HPD'
    elif FOR_REP =='High Productivity Mixedwood':
        return 'HPM'
    elif FOR_REP =='Low Productivity Coniferous Leading':
        return 'LPC'
    elif FOR_REP =='Low Productivity Deciduous Leading':
        return 'LPD'
    elif FOR_REP =='Low Productivity Mixedwood':
        return 'LPM'
    elif FOR_REP =='Medium Productivity Coniferous Leading':
        return 'MPC'
    elif FOR_REP == 'Medium Productivity Deciduous Leading':
        return 'MPD'
    elif FOR_REP == 'Medium Productivity Mixedwood':
        return 'MPM'
    ```
    - use identity to attach aflb_fact to rec_cat_all from AFLB layer 
    - recalculate aflb_area_ha
    - multipart to single part 
    - create field priority_2d (int)
    - select by location (within) all Cultural areas, calculate priority_2d to 3 
    - select by locaiton (within) all tsu, priority_2d field to 2 
    - intersect cultural areas and tsu, select by location (within) of the interesct and priority_2d field to 1
    - for processing speed remove all fields accept for SIFA, SIFA2, FOR_REP, NEW_AGE_CLASS, Orig_Age, WATER_MANAGMENT_BASIN_NAME, Area_ha, Rec_Cat, Rec_Cat_short, thlb_fact, aflb_fact, aflb_area_ha, priority_2d
<br>
<br>

## recreate recuirtment tables
- based on provided AFLB layer
- indenity on recuirtment forest all layer to add 'new' AFLB fact
- re calculate AFLB_area
- dissolve by WMB, Rec Cat, For Rep, stats for aflb_area_ha
- create pivot table for each WMB, cols= for rep, rows= rec cat, values= sum of aflb


## running step 2- scenario5_step2.ipynb
scenario5_step2.ipynb can be run entierly if the sections for stepc and step d are put back in, but they run extermely slow. 

**For Best Results**
Run the cells up to the end of step 2b<br>
Step 2c:
    - from the data created in 2b, select polygons containing cultural lands or TSU and not anything in HV1 DZ
    - save seelction as ret_class_2c_intermidate.parquet
    - continue in script <br>
Continue running script until the end of 2c<br>
Step 2d: 
    staring with rec_class_all
    - verify protection_2d is populated
    - remove stands that overlap with step 2b(ret_class_2b.parquet)
    - remove stands that overlap with HV1 DZ
    - save as step_2d_base_aflb.parquet<br>

Continue running the rest of the cells for scenario5_step2.ipynb
<br>
<br>

## running step 3- scenario5_step3.ipynb
Before running the script check the variables in the first cell **patche_area_3a** and **max_rings**
run cells up to the end of 3j
## manual steps for secondary reserves aspatial deferred old forest and conservations lands 
### Conservation lands
- merge hv1 protections, secondary reserves and Aspatially Deferred Old Forest inside TSUs
- remove any areas that overlap hv1 development and critical infrastructures 

for each layer
- if not already dissolved, dissolve it for a 'clean' slate of attributes 
- create new attribute 'Designation' (text), populate with layer name (conservation lands, etc)
- union dissolved layer with wmb, tsu(split by wmb), forest recruitment, thlb layer and AFLB
- select polygons that have the designation column populated
- erase HV1 dz and critical infrastructure and electrification infrastructure from dataset, so there are none overlapping  
- export data set with the following attributes: designation, BASIN_ID, NAME, TRAPLINE_AREA_IDENTIFIER,  SIFA, SIFA_2, For_REP, Rec_Cat, for_rep_short, thlb_fact, afhlb_fact, area_ha, thlb_area_ha, aflb_ha
- recalculate area_ha, thlb_area_ha (thlb_fact*area_ha) and aflb_ha(aflb_fact*area_ha) 

Visually verify secondary reserves ,Aspatially Deferred and conservation lands <br>


## running step 4- scenario5_step4_summary.ipynb
Before running the script check the variables in the first cell **scen5_run_number** and **scen5_run_folder**


# old forest outside of conservation lands and aspatially deferred old forest
- query vri for stands >= than 140
- clip vri to priority wmb
- union wmb, tus, and thlb to vri selection
- recalculate area_ha, thlb_area_ha
- erase conservation lands and aspatially deferred old from union layer 
- recalculate area_ha, thlb_area_ha
- summarize erased area by wmb 

