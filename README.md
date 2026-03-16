# WMB-connectivity-
The goal of this project is to recreate a similar program to the Marxan tool.
# base data 
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
- join aflb_fact and thlb_fact from  tsa40
