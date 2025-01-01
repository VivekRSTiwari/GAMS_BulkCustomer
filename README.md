# The optimal capacity expansion planning for bulk Indian consumer

Simple overview of use/purpose.

## Description

<br> This study prioritizes both cost-effectiveness and minimizing emissions, with a particular emphasis on expanding solar and storage capacity. As shown in Figure 1, the Capacity Expansion Planning (CEP) will use forecasted solar energy generation and electrical consumption load as inputs. It will also consider technical and economic parameters associated with solar and storage options, including their flexibility. Additionally, the CEP employs a Mixed-Integer Linear Programming (MILP) model for optimization. The outputs of the CEP will be the total cost and emissions of the system, the installed capacity of solar and storage systems, and the power generation and flexibility of the grid.



**1.	Optimization model**
<br> The prime objective is to minimize the overall cost of the power system of the campus. This cost encompasses several components, namely the total costs associated with the grid, renewable energy sources (solar and wind), and storage systems (see Eq.  2-5). The formulation of the objective function is shown in Eq. 1.

```math
Total Cost_{sys} = min(TC_{grid}+ TC_{solar}+ TC_{sto})  ...eq(1)
```

**1. a) Total cost of electricity drawn from the grid**

```math
TC_{grid} = \displaystyle\sum_{y=1}^{Y} (C_{FC,grid,y} \times N_{load,grid,y}) + \left (M_d \times  \displaystyle\sum_{t=1}^{T} (C_{LC,grid,t} \times P_{grid,t} \times \Delta t) \right) 
```

**1. b) Total cost of electricity drawn from solar energy**

```math
TC_{solar} = (C_{inv,solar} \times N_{solar}) + \left (M_d \times  \displaystyle\sum_{y=1}^{Y} (C_{o\&m,solar} \times N_{solar}) \right)
```

**1. c) Total cost of electricity drawn from storage**

```math
TC_{sto} = (C_{inv,storage} \times N_{storage}) + \left (M_d \times  \displaystyle\sum_{y=1}^{Y} (C_{o\&m,storage} \times N_{solar}) \right)
```


## Getting Started

### Dependencies and installed software

* Windows 10
* Microsoft Excel
* GAMS Studio 1.15.5
* GAMS Distribution 44.4.0 (GAMS Release: 44.4.0 06604687 WEX-WEI x86 64bit/MS Windows) 
* CPLEX solver

### Executing program

* How to run the program
* Step-by-step bullets

```

*--------------------------------------------------------------

Set
   t 'hours'         / t1*t8760 /
   c 'load or CFS' / load,TNCUFW,TNCUFS,MHCUFW,MHCUFS,GJCUFW,GJCUFS,KACUFW,KACUFS,RJCUFW,RJCUFS,APCUFW,APCUFS,UPCUFS /
;
*--------------------------------------------------------------


*Parameter data to store load, CFS and CFW for every time step from external excell file
*--------------------------------------------------------------
Parameter data(t,c); 

*Read the indata file for parameter data (read XX:YY)
$call GDXXRW indataCase3IITK2040.xlsx trace=3 par=data rng=Sheet1!a1:o8761 rdim=1 cdim=1
$GDXIN indataCase3IITK2040.gdx
$LOAD  data
$GDXIN
Display data;
*--------------------------------------------------------------


*--------------------------------------------------------------
Variable
   cost         'Total cost of system'
   
   Pgrid(t)     'power delivered by grid at any time t'
   
   Ngrid        'Santioned grid power (MW)'
   Ns_AP        'AP Solar energy capacity (MW)'
   Nw_AP        'AP Wind energy capacity (MW)'
   Ns_GJ        'GJ Solar energy capacity (MW)'
   Nw_GJ        'GJ Wind energy capacity (MW)'
   Ns_TN        'TN Solar energy capacity (MW)'   
   Nw_TN        'Solar energy capacity (MW)'
   Ns_MH        'Solar energy capacity (MW)'   
   Nw_MH        'Solar energy capacity (MW)'
   Ns_KA        'Solar energy capacity (MW)'
   Nw_KA        'Solar energy capacity (MW)'
   Ns_RJ        'Solar energy capacity (MW)'
   Nw_RJ        'Solar energy capacity (MW)'
   Ns_UP        'Solar energy capacity (MW)'
   
   EBc_ESS      'ESS capacity (MWh)'
   EBc_PHS      'PHS capacity (MWh)'

   Ps_AP(t)     'power (MW) delivered by AP solar energy at any time t'
   Psc_AP(t)    'power (MW) curtained by AP solar energy at any time t'
   
   Pw_AP(t)     'power (MW) delivered by AP wind energy at any time t'
   Pwc_AP(t)    'power (MW) curtained by AP wind energy at any time t'

   Ps_GJ(t)     'power (MW) delivered by GJ solar energy at any time t'
   Psc_GJ(t)    'power (MW) curtained by GJ solar energy at any time t'

   Pw_GJ(t)     'power (MW) delivered by GJ wind energy at any time t'
   Pwc_GJ(t)    'power (MW) curtained by GJ wind energy at any time t'

   Ps_TN(t)     'power delivered by solar energy at any time t'
   Psc_TN(t)    'power curtained by solar energy at any time t'

   Pw_TN(t)     'power delivered by solar energy at any time t'
   Pwc_TN(t)    'power curtained by solar energy at any time t'

   Ps_MH(t)     'power delivered by solar energy at any time t'
   Psc_MH(t)    'power curtained by solar energy at any time t'

   Pw_MH(t)     'power delivered by solar energy at any time t'
   Pwc_MH(t)    'power curtained by solar energy at any time t'
  
   Ps_KA(t)     'power delivered by solar energy at any time t'
   Psc_KA(t)    'power curtained by solar energy at any time t'
   
   Pw_KA(t)     'power delivered by solar energy at any time t'
   Pwc_KA(t)    'power curtained by solar energy at any time t'
   
   Ps_RJ(t)     'power delivered by solar energy at any time t'
   Psc_RJ(t)    'power curtained by solar energy at any time t'

   Pw_RJ(t)     'power delivered by solar energy at any time t'
   Pwc_RJ(t)    'power curtained by solar energy at any time t'

   Ps_UP(t)     'power delivered by solar energy at any time t'
   Psc_UP(t)    'power curtained by solar energy at any time t'
   
   SOC0_ESS     'Initial SOC of ESS'
   SOC_ESS(t)   'State of charge of ESS (MWh) at any time t'
   Pd_ESS(t)    'Discharging power of ESS (MW) at any time t'
   Pc_ESS(t)    'Charging power of ESS (MW) at any time t'
   PBc_ESS      'power of ESS (MW)'

   SOC0_PHS     'Initial SOC of PHS'
   SOC_PHS(t)   'State of charge of PHS (MWh) at any time t'
   Pd_PHS(t)    'Discharging power of PHS (MW) at any time t'
   Pc_PHS(t)    'Charging power of PHS (MW) at any time t'
   PBc_PHS      'power of PHS (MW)'  
;
*--------------------------------------------------------------


*--------------------------------------------------------------
Scalar
    Msim       'Months of simulation (month)'                    / 12 /
    
    Cfcgrid    'Cost of Grid santioned load (Rs/MW/Month)'       / 682083 /
    Cvcgrid    'Variable Cost of grid power delivered (Rs/MWh)'  / 14210 /
    Ngridmax   'Maximum grid capacity (MW)'                      / 1000 /
       
    Cs         'cost of solar energy (Rs/MWh)'                   / 1840 /
    Csc        'cost of curtainment solar energy (Rs/MWh)'       / 1500 /
    Ns_AP_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_GJ_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_TN_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_MH_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_KA_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_RJ_max  'Maximum Solar energy capacity (MW)'              / 1000 /
    Ns_UP_max  'Maximum Solar energy capacity (MW)'              / 1000 /

    Cw         'cost of wind energy (Rs/MWh)'                    / 2595 /
    Cwc        'cost of curtainment wind energy (Rs/MWh)'        / 1500 /    
    Nw_AP_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    Nw_GJ_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    Nw_TN_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    Nw_MH_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    Nw_KA_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    Nw_RJ_max  'Maximum Wind energy capacity (MW)'               / 1000 /
    
    ISOCP_ESS  'State of chage of ESS at t = 0 (MWh)'            / 0.5 /
    eta_c_ESS  'Charging efficiency of ESS (-)'                  / 0.95 /
    eta_d_ESS  'Charging efficiency of ESS (-)'                  / 0.9 /
    Cfcsto_ESS 'cost of ESS Storage (Rs/MWh)'                    / 233168 /
    PBmax_ESS  'Maximum power delivered by ESS (MW)'             / 30000 /
    EBmax_ESS  'Maximum ESS capacity (MWh)'                      / 30000 /
       
    ISOCP_PHS  'State of chage of PHS at t = 0 (MWh)'            / 0.5 /
    eta_c_PHS  'Charging efficiency of PHS (-)'                  /0.88/  
    eta_d_PHS  'Discharging efficiency of PHS (-)'               /0.91/  
    Cfcsto_PHS 'Cost of PHS Storage (Rs/MWh)'                    / 183794 /
    PBmax_PHS  'Maximum power delivered by PHS (MW)'             / 0 /
    EBmax_PHS  'Maximum PHS capacity (MWh)'                      / 0 /
;
*--------------------------------------------------------------


*--------------------------------------------------------------
*Range for power supplied by grid
pgrid.up(t) = Ngridmax;
pgrid.lo(t) = 0;
*--------------------------------------------------------------


*--------------------------------------------------------------
*Range for TN Solar capacity
Ns_TN.up  = Ns_TN_max;
Ns_TN.lo  = 0;

*Range for MH Solar capacity
Ns_MH.up  = Ns_MH_max;
Ns_MH.lo  = 0;

*Range for GJ Solar capacity
Ns_GJ.up  = Ns_GJ_max;
Ns_GJ.lo  = 0;

*Range for KA Solar capacity
Ns_KA.up  = Ns_KA_max;
Ns_KA.lo  = 0;

*Range for RJ Solar capacity
Ns_RJ.up  = Ns_RJ_max;
Ns_RJ.lo  = 0;

*Range for AP Solar capacity
Ns_AP.up  = Ns_AP_max;
Ns_AP.lo  = 0;

*Range for UP Solar capacity
Ns_UP.up  = Ns_UP_max;
Ns_UP.lo  = 0;
*--------------------------------------------------------------


*--------------------------------------------------------------
*Range for TN Wind capacity
Nw_TN.up  = Nw_TN_max;
Nw_TN.lo  = 0;

*Range for MH wind capacity
Nw_MH.up  = Nw_MH_max;
Nw_MH.lo  = 0;

*Range for GJ Wind capacity
Nw_GJ.up  = Nw_GJ_max;
Nw_GJ.lo  = 0;

*Range for KA Wind Capacity
Nw_KA.up  = Nw_KA_max;
Nw_KA.lo  = 0;

*Range for RJ Wind capacity
Nw_RJ.up  = Nw_RJ_max;
Nw_RJ.lo  = 0;

*Range for AP Wind capacity
Nw_AP.up  = Nw_AP_max;
Nw_AP.lo  = 0;
*--------------------------------------------------------------


*Capacity and power range of Energy Storage System (ESS) 
*--------------------------------------------------------------
*Range for Storage capacity of ESS
EBc_ESS.up = EBmax_ESS;
EBc_ESS.lo = 0;

*Range for Storage capacity of ESS
PBc_ESS.up = PBmax_ESS;
PBc_ESS.lo = 0;
*--------------------------------------------------------------


*Capacity and power range of Pumped Hydro Storage (PHS)
*--------------------------------------------------------------
*Range for Storage capacity of PHS
EBc_PHS.up = EBmax_PHS;
EBc_PHS.lo = 0;

*Range for Storage capacity of PHS
PBc_PHS.up = PBmax_PHS;
PBc_PHS.lo = 0;
*--------------------------------------------------------------


Equation
    SystemCost                       'Total system cost(Rs)',
    
    balance                          'Energy balance',
    
    ESS_Initial_Stored_Energy        'Initial Storage Energy of ESS (MWh)',
    ESS_charge_Balance               'Charging of ESS',
    ESS_capacity_max_limit           'Maximum ESS capacity',
    ESS_capacity_min_limit           'Minimum ESS capacity',
    
    ESS_Charging_Power_max_limit     'Maximum ESS Charging Power',
    ESS_Charging_Power_min_limit     'Minimum ESS Charging Power',
    
    ESS_Discharging_Power_max_limit  'Maximum ESS Discharging Power',
    ESS_Discharging_Power_min_limit  'Minimum ESS Discharging Power',
    

    PHS_Initial_Stored_Energy        'Initial Storage Energy of PHS (MWh)',
    PHS_charge_Balance               'Charging of PHS',
    PHS_capacity_max_limit           'Maximum PHS capacity',
    PHS_capacity_min_limit           'Minimum PHS capacity',
    
    PHS_Charging_Power_max_limit     'Maximum PHS Charging Power',
    PHS_Charging_Power_min_limit     'Minimum PHS Charging Power',
    
    PHS_Discharging_Power_max_limit  'Maximum PHS Discharging Power',
    PHS_Discharging_Power_min_limit  'Minimum PHS Discharging Power',
    
    
    maxPgrid        'Maximum Grid capacity (MW',
    minPgrid        'Minimum Grid capacity (MW)',
    
    solarTN         'Solar balance for TN (MW)',
    solarMH         'Solar balance for MH (MW)',
    solarGJ         'Solar balance for GJ (MW)',
    solarKA         'Solar balance for KA (MW)',
    solarRJ         'Solar balance for RJ (MW)',
    solarAP         'Solar balance for AP (MW)',
    solarUP         'Solar balance for UP (MW)',
    windTN          'Wind balance for TN (MW)',
    windMH          'Wind balance for MH (MW)',
    windGJ          'Wind balance for GJ (MW)',
    windKA          'Wind balance for KA (MW)',
    windRJ          'Wind balance for RJ (MW)',
    windAP          'Wind balance for AP (MW)',
    
    minPscTN        'Minimum Solar curtainment for TN',
    minPscMH        'Minimum Solar curtainment for MH',
    minPscGJ        'Minimum Solar curtainment for GJ',
    minPscKA        'Minimum Solar curtainment for KA',
    minPscRJ        'Minimum Solar curtainment for RJ',
    minPscAP        'Minimum Solar curtainment for AP',
    minPscUP        'Minimum Solar curtainment for UP',
    
    maxPscTN        'Maximum Solar curtainment for TN',
    maxPscMH        'Maximum Solar curtainment for MH',
    maxPscGJ        'Maximum Solar curtainment for GJ',
    maxPscKA        'Maximum Solar curtainment for KA',
    maxPscRJ        'Maximum Solar curtainment for RJ',
    maxPscAP        'Maximum Solar curtainment for AP',
    maxPscUP        'Maximum Solar curtainment for UP',
    
    minPwcTN        'Minimum Wind curtainment for TN',
    minPwcMH        'Minimum Wind curtainment for MH',
    minPwcGJ        'Minimum Wind curtainment for GJ',
    minPwcKA        'Minimum Wind curtainment for KA',
    minPwcRJ        'Minimum Wind curtainment for RJ',
    minPwcAP        'Minimum Wind curtainment for AP',
    
    maxPwcTN        'Maximum Wind curtainment for TN',
    maxPwcMH        'Maximum Wind curtainment for MH',
    maxPwcGJ        'Maximum Wind curtainment for GJ',
    maxPwcKA        'Maximum Wind curtainment for KA',
    maxPwcRJ        'Maximum Wind curtainment for RJ',
    maxPwcAP        'Maximum Wind curtainment for AP'
    ;

*1-Objective equation
*--------------------------------------------------------------
SystemCost..   cost =e=          (Ngrid*Cfcgrid*Msim)          + sum((t),Pgrid(t)*Cvcgrid)
                           + sum((t),(Ps_TN(t)-Psc_TN(t))*Cs)  + sum((t),(Psc_TN(t))*Csc)
                           + sum((t),(Ps_MH(t)-Psc_MH(t))*Cs)  + sum((t),(Psc_MH(t))*Csc)
                           + sum((t),(Ps_GJ(t)-Psc_GJ(t))*Cs)  + sum((t),(Psc_GJ(t))*Csc)
                           + sum((t),(Ps_KA(t)-Psc_KA(t))*Cs)  + sum((t),(Psc_KA(t))*Csc)
                           + sum((t),(Ps_RJ(t)-Psc_RJ(t))*Cs)  + sum((t),(Psc_RJ(t))*Csc)
                           + sum((t),(Ps_AP(t)-Psc_AP(t))*Cs)  + sum((t),(Psc_AP(t))*Csc)
                           + sum((t),(Ps_UP(t)-Psc_UP(t))*Cs)  + sum((t),(Psc_UP(t))*Csc)
                          
                           + sum((t),(Pw_TN(t)-Pwc_TN(t))*Cw)  + sum((t),(Pwc_TN(t))*Cwc)
                           + sum((t),(Pw_MH(t)-Pwc_MH(t))*Cw)  + sum((t),(Pwc_MH(t))*Cwc)
                           + sum((t),(Pw_GJ(t)-Pwc_GJ(t))*Cw)  + sum((t),(Pwc_GJ(t))*Cwc)
                           + sum((t),(Pw_KA(t)-Pwc_KA(t))*Cw)  + sum((t),(Pwc_KA(t))*Cwc)
                           + sum((t),(Pw_RJ(t)-Pwc_RJ(t))*Cw)  + sum((t),(Pwc_RJ(t))*Cwc)
                           + sum((t),(Pw_AP(t)-Pwc_AP(t))*Cw)  + sum((t),(Pwc_AP(t))*Cwc)
                           + (EBc_ESS*Cfcsto_ESS*Msim)
                           + (EBc_PHS*Cfcsto_PHS*Msim);
*--------------------------------------------------------------



*2-Energy balance
*--------------------------------------------------------------
balance(t)..        Ps_TN(t) + Ps_MH(t) + Ps_GJ(t) + Ps_KA(t) + Ps_RJ(t) + Ps_AP(t) + Ps_UP(t)
                  + Pw_TN(t) + Pw_MH(t) + Pw_GJ(t) + Pw_KA(t) + Pw_RJ(t) + Pw_AP(t)
                  + Pgrid(t)+ Pd_ESS(t)+ Pd_PHS(t) =e= data(t,'load') + Pc_ESS(t)+ Pc_PHS(t)
                  + Psc_TN(t) + Psc_MH(t) + Psc_GJ(t) + Psc_KA(t) + Psc_RJ(t) + Psc_AP(t)+ Psc_UP(t)
                  + Pwc_TN(t) + Pwc_MH(t) + Pwc_GJ(t) + Pwc_KA(t) + Pwc_RJ(t) + Pwc_AP(t);
*--------------------------------------------------------------


*Constraints for Energy Storage System (ESS)
*--------------------------------------------------------------
ESS_Initial_Stored_Energy(t)..           SOC0_ESS =e=  ISOCP_ESS * EBc_ESS;

*3-Storage balance
ESS_charge_Balance(t).. SOC_ESS(t) =e= SOC0_ESS$(ord(t)=1) + SOC_ESS(t-1)$(ord(t)>1) + Pc_ESS(t)*eta_c_ESS - (Pd_ESS(t))/eta_d_ESS;

*6-Range for Storage capacity of ESS
ESS_capacity_max_limit(t)..            SOC_ESS(t) =l=  EBc_ESS;
ESS_capacity_min_limit(t)..            SOC_ESS(t) =g=  0.2*EBc_ESS;

*7-Range for charging power of ESS (revisit)
ESS_Charging_Power_max_limit(t)..      Pc_ESS(t) =l= PBc_ESS;
ESS_Charging_Power_min_limit(t)..      Pc_ESS(t) =g= 0;

*8-Range for discharging power of ESS
ESS_Discharging_Power_max_limit(t)..   Pd_ESS(t) =l= PBc_ESS;
ESS_Discharging_Power_min_limit(t)..   Pd_ESS(t) =g= 0;
*--------------------------------------------------------------


*Constraints for Pumped Hydro Storage (PHS)
*--------------------------------------------------------------
PHS_Initial_Stored_Energy(t)..           SOC0_PHS =e=  ISOCP_PHS * EBc_PHS;

PHS_charge_Balance(t).. SOC_PHS(t) =e= SOC0_PHS$(ord(t)=1) + SOC_PHS(t-1)$(ord(t)>1) + Pc_PHS(t)*eta_c_PHS - (Pd_PHS(t))/eta_d_PHS;

*6-Range for Storage capacity of ESS
PHS_capacity_max_limit(t)..            SOC_PHS(t) =l=  EBc_PHS;
PHS_capacity_min_limit(t)..            SOC_PHS(t) =g=  0.2*EBc_PHS;

*7-Range for charging power of ESS
PHS_Charging_Power_max_limit(t)..      Pc_PHS(t) =l= PBc_PHS;
PHS_Charging_Power_min_limit(t)..      Pc_PHS(t) =g= 0;

*8-Range for discharging power of ESS
PHS_Discharging_Power_max_limit(t)..   Pd_PHS(t) =l= PBc_PHS;
PHS_Discharging_Power_min_limit(t)..   Pd_PHS(t) =g= 0;
*----------------------------------------------------------------


*Range for grid power
*--------------------------------------------------------------
*4-Range of power delivered by Grid
maxPgrid(t)..       Pgrid(t) =l= Ngrid;
minPgrid(t)..       Pgrid(t) =g= 0;
*--------------------------------------------------------------


*Solar balance for state TN, MH, GJ, KA, RJ, AP and UP
*--------------------------------------------------------------
*4-TN Solar balance
solarTN(t)..          Ps_TN(t)  =e= Ns_TN * data(t,'TNCUFS');

*4-MH Solar balance
solarMH(t)..          Ps_MH(t)  =e= Ns_MH * data(t,'MHCUFS');

*4-GJ Solar balance
solarGJ(t)..          Ps_GJ(t)  =e= Ns_GJ * data(t,'GJCUFS');

*4-KA Solar balance
solarKA(t)..          Ps_KA(t)  =e= Ns_KA * data(t,'KACUFS');

*4-RJ Solar balance
solarRJ(t)..          Ps_RJ(t)  =e= Ns_RJ * data(t,'RJCUFS');

*4-AP Solar balance
solarAP(t)..          Ps_AP(t)  =e= Ns_AP * data(t,'APCUFS');

*4-UP Solar balance
solarUP(t)..          Ps_UP(t)  =e= Ns_UP * data(t,'UPCUFS');
*--------------------------------------------------------------


*Range for Solar curtainment for state TN, MH, GJ, KA, RJ, AP and UP
*--------------------------------------------------------------
*9-Range for TN solar curtainment
maxPscTN(t)..          Psc_TN(t) =l= Ps_TN(t);
minPscTN(t)..          Psc_TN(t) =g= 0;

*9-Range for MH solar curtainment
maxPscMH(t)..          Psc_MH(t) =l= Ps_MH(t);
minPscMH(t)..          Psc_MH(t) =g= 0;

*9-Range for GJ solar curtainment
maxPscGJ(t)..          Psc_GJ(t) =l= Ps_GJ(t);
minPscGJ(t)..          Psc_GJ(t) =g= 0;

*9-Range for KA solar curtainment
maxPscKA(t)..          Psc_KA(t) =l= Ps_KA(t);
minPscKA(t)..          Psc_KA(t) =g= 0;

*9-Range for RJ solar curtainment
maxPscRJ(t)..          Psc_RJ(t) =l= Ps_RJ(t);
minPscRJ(t)..          Psc_RJ(t) =g= 0;

*9-Range for AP solar curtainment
maxPscAP(t)..          Psc_AP(t) =l= Ps_AP(t);
minPscAP(t)..          Psc_AP(t) =g= 0;

*9-Range for UP solar curtainment
maxPscUP(t)..          Psc_UP(t) =l= Ps_UP(t);
minPscUP(t)..          Psc_UP(t) =g= 0;
*--------------------------------------------------------------


*Wind balance for state TN, MH, GJ, KA, RJ and AP
*--------------------------------------------------------------
*5-TN wind balance
windTN(t)..          Pw_TN(t)  =e= Nw_TN * data(t,'TNCUFW');

*5-MH wind balance
windMH(t)..          Pw_MH(t)  =e= Nw_MH * data(t,'MHCUFW');

*5-GJ wind balance
windGJ(t)..          Pw_GJ(t)  =e= Nw_GJ * data(t,'GJCUFW');

*5-KA wind balance
windKA(t)..          Pw_KA(t)  =e= Nw_KA * data(t,'KACUFW');

*5-RJ wind balance
windRJ(t)..          Pw_RJ(t)  =e= Nw_RJ * data(t,'RJCUFW');

*5-AP wind balance
windAP(t)..          Pw_AP(t)  =e= Nw_AP * data(t,'APCUFW');
*--------------------------------------------------------------


*Range for wind curtainment for state TN, MH, GJ, KA, RJ and AP
*--------------------------------------------------------------
*10-Range for Pwc
maxPwcTN(t)..          Pwc_TN(t) =l= Pw_TN(t);
minPwcTN(t)..          Pwc_TN(t) =g= 0;

*10-Range for Pwc
maxPwcMH(t)..          Pwc_MH(t) =l= Pw_MH(t);
minPwcMH(t)..          Pwc_MH(t) =g= 0;

*10-Range for Pwc
maxPwcGJ(t)..          Pwc_GJ(t) =l= Pw_GJ(t);
minPwcGJ(t)..          Pwc_GJ(t) =g= 0;

*10-Range for Pwc
maxPwcKA(t)..          Pwc_KA(t) =l= Pw_KA(t);
minPwcKA(t)..          Pwc_KA(t) =g= 0;

*10-Range for Pwc
maxPwcRJ(t)..          Pwc_RJ(t) =l= Pw_RJ(t);
minPwcRJ(t)..          Pwc_RJ(t) =g= 0;

*10-Range for Pwc
maxPwcAP(t)..          Pwc_AP(t) =l= Pw_AP(t);
minPwcAP(t)..          Pwc_AP(t) =g= 0;
*--------------------------------------------------------------


*Model and solver
*--------------------------------------------------------------
Model DEDESScostbased / all /;
solve DEDESScostbased using MIP minimizing cost;
*--------------------------------------------------------------



*Export the data to an external excel file
*--------------------------------------------------------------
Parameter rep(t,*);
rep(t,'Pgrid')    = pgrid.l(t);

rep(t,'SOC_ESS')  = SOC_ESS.l(t);
rep(t,'Pd_ESS')   = Pd_ESS.l(t);
rep(t,'Pc_ESS')   = Pc_ESS.l(t);

rep(t,'Ps_TN')    = Ps_TN.l(t);
rep(t,'Psc_TN')   = Psc_TN.l(t);

rep(t,'Ps_MH')    = Ps_MH.l(t);
rep(t,'Psc_MH')   = Psc_MH.l(t);

rep(t,'Ps_GJ')    = Ps_GJ.l(t);
rep(t,'Psc_GJ')   = Psc_GJ.l(t);

rep(t,'Ps_KA')    = Ps_KA.l(t);
rep(t,'Psc_KA')   = Psc_KA.l(t);

rep(t,'Ps_RJ')    = Ps_RJ.l(t);
rep(t,'Psc_RJ')   = Psc_RJ.l(t);

rep(t,'Ps_AP')    = Ps_AP.l(t);
rep(t,'Psc_AP')   = Psc_AP.l(t);

rep(t,'Ps_UP')    = Ps_UP.l(t);
rep(t,'Psc_UP')   = Psc_UP.l(t);

rep(t,'Pw_TN')    = Pw_TN.l(t);
rep(t,'Pwc_TN')   = Pwc_TN.l(t);

rep(t,'Pw_MH')    = Pw_MH.l(t);
rep(t,'Pwc_MH')   = Pwc_MH.l(t);

rep(t,'Pw_GJ')    = Pw_GJ.l(t);
rep(t,'Pwc_GJ')   = Pwc_GJ.l(t);

rep(t,'Pw_KA')    = Pw_KA.l(t);
rep(t,'Pwc_KA')   = Pwc_KA.l(t);

rep(t,'Pw_RJ')    = Pw_RJ.l(t);
rep(t,'Pwc_RJ')   = Pwc_RJ.l(t);

rep(t,'Pw_AP')    = Pw_AP.l(t);
rep(t,'Pwc_AP')   = Pwc_AP.l(t);

rep(t,'Load')     = data(t,'load');

embeddedCode Connect:
- GAMSReader:
    symbols: [ { name: pgrid }, { name: rep } ]
- Projection:
    name: pgrid.l(t)
    newName: pgrid_l(t)
- PandasExcelWriter:
    file: ResultCase3IITK2040.xlsx
    symbols:
      - { name: pgrid_l, range: Pthermal!A1 }
      - { name: rep, range: rep!A1 }
endEmbeddedCode
*--------------------------------------------------------------

```

## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Author

* [Vivek Tiwari](https://www.linkedin.com/in/vivekrstiwari)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release
