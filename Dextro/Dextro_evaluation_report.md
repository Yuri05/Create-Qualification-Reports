# Building and evaluation of a PBPK model for Dextromethorphan in adults with different CYP2D6 phenotypes

| Version                                         | main-OSP12.2                                                   |
| ----------------------------------------------- | ------------------------------------------------------------ |
| based on *Model Snapshot* and *Evaluation Plan* | https://github.com/Open-Systems-Pharmacology/Dextromethorphan-Model/releases/tag/vmain |
| OSP Version                                     | 12.2                                                         |
| Qualification Framework Version                 | 3.5                                                         |

This evaluation report and the corresponding PK-Sim project file are filed at:

https://github.com/Open-Systems-Pharmacology/OSP-PBPK-Model-Library/

# Table of Contents

 * [1 Introduction](#1)
 * [2 Methods](#2)
   * [2.1 Modeling strategy](#21)
   * [2.2 Data and parameters used](#22)
     * [2.2.1 In vitro / physico-chemical Data Dextromethorphan ](#invitro-and-physico-chemical-data-dextromethorphan)
     * [2.2.2 In vitro / physico-chemical Data Dextrorphan ](#invitro-and-physico-chemical-data-dextrorphan)
     * [2.2.3 In vitro / physico-chemical Data Dextrorphan O-glucuronide ](#invitro-and-physico-chemical-data-dextrorphan-o-glucuronide)
     * [2.2.4 Clinical Data  ](#clinical-data)
       * [2.2.4.1 Base Model Building ](#base-model-building)
       * [2.2.4.2 Base Model Verification ](#base-model-verification)
       * [2.2.4.3 DGI Model Building ](#dgi-model-building)
       * [2.2.4.4 DGI Model Verification ](#dgi-model-verification)
   * [2.3 Model Parameters and Assumptions](#23)
     * [2.3.1 Absorption ](#model-parameters-and-assumptions-absorption)
     * [2.3.2 Distribution ](#model-parameters-and-assumptions-distribution)
     * [2.3.3 Metabolism and Elimination ](#model-parameters-and-assumptions-metabolism-and-elimination)
     * [2.3.4 Automated Parameter Identification ](#parameter-identification)
 * [3 Results and Discussion](#3)
   * [3.1 Final input parameters](#final-input-parameters)
   * [3.2 Dextromethorphan and its metabolites Diagnostics Plots](#31)
   * [3.3 Concentration-Time Profiles](#32)
     * [3.3.1 Model Building](#321)
     * [3.3.2 Model Verification](#322)
 * [4 Conclusion](#4)
 * [5 References](#5)

# 1 Introduction<a id="1"></a>

Dextromethorphan is a widely used over-the-counter cough suppressant and a common ingredient of cold medicines marketed toward children and adults.

Dextromethorphan is typically administered as its hydrobromide salt, which is considered a Biopharmaceutics Drug Disposition Classification System (BDDCS) class I drug with high solubility and permeability. After oral administration, dextromethorphan is rapidly absorbed. Dextromethorphan undergoes an extensive first-pass metabolism, predominately mediated by CYP2D6, reducing the bioavailability to 1%–2% in CYP2D6 extensive metabolizers (EMs) and 80% in CYP2D6 poor metabolizers (PMs). 

The herein presented model building and evaluation report evaluates the performance of the PBPK model for Dextromethorphan in (healthy) adults published by Rüdesheim et al. 2025 ([Rüdesheim 2025](#5-References)). 
A whole-body PBPK model of dextromethorphan and its metabolites dextrorphan and dextrorphan O-glucuronide was developed and evaluated to predict drug plasma concentrations over a wide dosing range (5–80mg).
Dextromethorphan is mainly metabolized via CYP2D6 and to a lesser extent by CYP3A4. A CYP2D6 activity score-dependent metabolism of dextromethorphan was implemented to describe the effect of CYP2D6 drug-gene interaction (DGI) on the PK of the modeled compounds. 
Although dextromethorphan is rapidly absorbed from the intestine, time to reach peak plasma concentration Cmax (tmax) often occurs as late as 4h after oral administration. This phenomenon likely occurs due to lysosomal trapping of dextromethorphan in the intestinal mucosa. To emulate the effect of lysosomal trapping in the gastrointestinal mucosa, a surrogate protein binding partner was included in the model. 

Model features include:

Dextromethorphan:
- metabolism by CYP2D6 and formation of the primary metabolite dextrorphan
- metabolism by CYP3A4
- passive glomerular filtration
- lysosomal trapping mimicked by surrogate protein binding

Primary metabolite Dextrorphan:
- metabolism by UGT2B15 into Dextrorphan-O-glucuronide
- metabolism by CYP3A4

Secondary metabolite Dextrorphan-O-glucuronide:
- passive glomerular filtration 
- active secretion to urine

The presented Dextromethorphan PBPK model as well as the respective evaluation plan and evaluation report are provided open-source ([https://github.com/Open-Systems-Pharmacology/Dextromethorphan-Model](https://github.com/Open-Systems-Pharmacology/Dextromethorphan-Model)).

# 2 Methods<a id="2"></a>

## 2.1 Modeling strategy<a id="21"></a>

The general concept of building a PBPK model has previously been described by Kuepfer et al. ([Kuepfer 2016](#5-references)) Regarding the relevant anthropometric (height, weight) and physiological parameters (e.g. blood flows, organ volumes, binding protein concentrations, hematocrit, cardiac output) in adults was gathered from the literature and has been previously published ([PK-Sim Ontogeny Database Version 7.3](#5-references)). The information was incorporated into PK-Sim® and was used as default values for the simulations in adults.

The  applied activity and variability of plasma proteins and active processes that are integrated into PK-Sim® are described in the publicly available PK-Sim® Ontogeny Database Version 7.3 ([Schlender 2016](#5-references)) or otherwise referenced for the specific process.

The dextromethorphan PBPK model was developed using a total of 28 clinical studies where dextromethorphan was administered as an intravenous infusion (one study), orally in single (26 studies), or multiple doses (one study), alone (17 studies) or as part of a phenotyping cocktail (11 studies). Doses ranged between 5 and 80 mg of administered dextromethorphan.
The PBPK model was built based on data from healthy individuals, using the reported sex, ethnicity and mean values for age, weight and height from each study protocol. If no demographic information was provided, the following default values were substituted: male, European, 30 years of age, 73 kg body weight and 176 cm body height (characteristics from the PK-Sim® population database). CYP2D6 was implemented in accordance with literature, using the
PK-Sim® expression database to define their relative expression in the different organs of the body.
First a base model was built using clinical data from a study where dextromethorphan was administered as an intravenous infusion ([Duedahl 2004](#5-references)) and oral administration from 3 clinical studies. The base model was further verified using 12 additional clinical studies. Next, the model was further developed using a DGI training dataset consisting of four studies that reported CYP2D6 activity scores or genotypes of their respective study populations. To complement these studies, 24 individual plasma concentration-time profiles were included. Overall, activity scores in the DGI model training dataset ranged from 0 (poor metabolizer PM) to 3 (ultrarapid metabolizer UR) and covered a total of eight activity scores. This dataset was used to optimize population kcat values for the activity scores of the respective studies or individual profiles

Unknown parameters (see below) were identified using the Parameter Identification module provided in PK-Sim®. Structural model selection was mainly guided by visual inspection of the resulting description of data and biological plausibility.

Once the appropriate structural model was identified, additional parameters for tablet formulations were identified. 

The model was verified by simulating further clinical studies reporting pharmacokinetic concentration-time profiles after oral administration of dextromethorphan (verification datasets).

Details about input data (physicochemical, *in vitro* and clinical) can be found in  [Section 2.2](#methods-data).

Details about the structural model and its parameters can be found in  [Section 2.3](#model-parameters-and-assumptions).

## 2.2 Data and parameters used<a id="22"></a>

### 2.2.1 In vitro / physico-chemical Data Dextromethorphan <a id="invitro-and-physico-chemical-data-dextromethorphan"></a>

A literature search was performed to collect available information on physicochemical properties of dextromethorphan. The obtained information from literature is summarized in the table below and is used for model building. 

| **Parameter**| **Unit** | **Value** | **Source** | **Description**|
| :-----------------------------| -------- | ---------- | ------------------------------------------ | ----------------------------------------------- |
| MW                            | g/mol    |   271.41   | [Benet 2011](#main-references)             | Molecular weight                                |
| pK<sub>a</sub>                |          |   9.10     | [Spaggiari 2014](#main-references)         | Acid dissociation constant                      |
| Solubility (pH)               | g/L      |   15.00    | [Benet 2011](#main-references)             | Aqueous Solubility Hydrobromide                 |
| logP                          |          |   4.10     | [Spaggiari 2014](#main-references)        | Partition coefficient between octanol and water |
| fu                            | %        |   35.00    | [Lutz 2012](#main-references)              | Fraction unbound in plasma                      |
| CYP2D6 K<sub>m</sub> -> dxt   | µmol/L   |   4.65     | [Brown 2007](#main-references)| Michaelis Menten constant|
| CYP2D6 K<sub>cat</sub> -> dxt (PM) | 1/min |  0       | [Brown 2007](#main-references)              | Catalytic rate constant                        |
| CYP3A4 K<sub>m</sub>          | µmol/L   |   176.80   | [Lutz 2012](#main-references)              | Michaelis Menten constant                       |
  
### 2.2.2 In vitro / physico-chemical Data Dextrorphan <a id="invitro-and-physico-chemical-data-dextrorphan"></a>

A literature search was performed to collect available information on physicochemical properties of Dextrorphan. The obtained information from literature is summarized in the table below and is used for model building. 

| **Parameter**| **Unit** | **Value** | **Source** | **Description**|
| :-----------------------------| -------- | ---------- | ------------------------------------------ | ----------------------------------------------- |
| MW                            | g/mol    |   257.37   | [HMDB-a](#main-references),[Wishart 2018](#main-references)           | Molecular weight                                |
| logP|          |  1.38    | [Spaggiari 2014](#main-references)         | Partition coefficient between octanol and water|
| pK<sub>a</sub> strongest acidic|         |   10.10    | [Spaggiari 2014](#main-references)         | Acid dissociation constant                      |
| logP                          |          |   2.90     | [Kim 2019](#main-references)               | Partition coefficient between octanol and water |
| fu                            | %        |   42.00    | [Watanabe 2018](#main-references)          | Fraction unbound in plasma                      |
| UGT2B15 K<sub>m</sub> -> dxt-glu   | µmol/L | 184.8   | [Lutz 2012](#main-references)              | Michaelis Menten constant                       |
| CYP2D6 K<sub>cat</sub> -> dxt (PM) | 1/min    |  0    | [Brown 2007](#main-references)| Catalytic rate constant|
| CYP3A4 K<sub>m</sub>          | µmol/L   |   910.00   | [Lutz 2012](#main-references)              | Michaelis Menten constant                       |
| CYP3A4 K<sub>cat</sub>        | 1/min    |  7.41      | [Lutz 2012](#main-references)| Catalytic rate constant|

### 2.2.3 In vitro / physico-chemical Data Dextrorphan O-glucuronide <a id="invitro-and-physico-chemical-data-dextrorphan-o-glucuronide"></a>

A literature search was performed to collect available information on physicochemical properties of Dextrorphan O-glucuronide. The obtained information from literature is summarized in the table below and is used for model building. 

| **Parameter**| **Unit** | **Value** | **Source** | **Description**|
| :-----------------------------| -------- | ---------- | ------------------------------------------ | ----------------------------------------------- |
| MW                            | g/mol    |   433.5    | [Wishart 2018](#main-references)| Molecular weight|
| logP                          |          |   1.38     | [HMDB-b](#main-references), [Wishart 2018](#main-references) | Partition coefficient between octanol and water |
| pK<sub>a</sub> strongest basic|          |   9.82     | [Wishart 2018](#main-references)           | Acid dissociation constant                      |
| pK<sub>a</sub> strongest acidic|         |   2.85     | [Wishart 2018](#main-references)           | Acid dissociation constant                      |
| Solubility                    | g/L      |   1.20     | [Wishart 2018](#main-references)           | Solubility|
| fu                            | %        |   37.00    | Calculated [Watanabe 2018](#main-references)| Fraction unbound in plasma                     |

### 2.2.4 Clinical Data  <a id="clinical-data"></a>

A literature search was performed to collect available clinical data on Dextromethorphan in healthy adults.

#### 2.2.4.1 Base Model Building <a id="base-model-building"></a>

The following studies were used for model building (training data):

| **Publication** | **Study description**|
| :-------------------------- | :----------------------- |
| [Duedahl 2005](#main-references) | CYP2D6 EM Subjects with an single IV infusion of 0.5 mg/kg for 30 min  |
| [Schadel 1995](#main-references) | CYP2D6 EM and PM Subjects with a single PO dose of 30 mg |
| [Tennezé 1990](#main-references) | CYP2D6 EM subjects with a single PO dose of 80 mg | 

#### 2.2.4.2 Base Model Verification <a id="base-model-verification"></a>

The following studies were used for model verification:

| **Publication** | **Study description**|
| :-------------------------- | :----------------------- |
| [Feld 2013](#main-references) | CYP2D6 EM subjects with a single PO dose of 60 mg   |
| [AntecipBioventuresLLC](#main-references)| CYP2D6 EM subjects with twice daily PO doses of 60 mg bid|
| [Armani 2017](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|
| [Dumond 2010](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|              
| [Edwards 2017](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd| 
| [Ermer 2015](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd| 
| [Kakuda 2014](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|
| [Khalilieh 2018](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|
| [Nakashima 2007](#main-references)| CYP2D6 EM subjects with a single PO dose of 50 mg sd|
| [Nyunt 2008](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|
| [Sager 2014](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|
| [Stage 2018](#main-references)| CYP2D6 EM subjects with a single PO dose of 30 mg sd|

#### 2.2.4.3 DGI Model Building <a id="dgi-model-building"></a>

The following studies were used for model building (training data):

| **Publication** | **Study description**|
| :-------------------------- | :----------------------- |
| [Qiu 2016](`#main-references`) | CYP2D6 NM and IM Subjects with a single PO dose of 15 mg |

#### 2.2.4.4 DGI Model Verification <a id="dgi-model-verification"></a>

The following studies were used for model verification:

| **Publication** | **Study description**|
| :-------------------------- | :----------------------- |
| [Capon 1996](#main-references) | CYP2D6 EM and PM subjects with a single PO dose of 30 mg   |
| [Gazzaz 2018](#main-references)| CYP2D6 NM subjects with a single PO dose of 30 mg sd|
| [Gorski 2004](#main-references)| CYP2D6 EM and PM subjects with a single PO dose of 30 mg sd|              
| [Yamazaki 2017](#main-references)| CYP2D6 NM and IM subjects with a single PO dose of 30 mg sd| 
| [Storelli 2018](#main-references)| CYP2D6 EM subjects with a single PO dose of 5 mg sd| 
| [Zawertailo 2010](#main-references)| CYP2D6 NM subjects with a single PO dose of 3 mg/kg sd|

## 2.3 Model Parameters and Assumptions<a id="23"></a>

### 2.3.1 Absorption <a id="model-parameters-and-assumptions-absorption"></a>

The dissolution of tablets were implemented via an empirical Weibull dissolution capsule. The final Weibull shape parameters and Weibull time parameters (50% dissolved) for the cocktail capsule formulation used in the PBPK model are given in [Section 2.3.4](#parameter-identification).

Although dextromethorphan is rapidly absorbed from the intestine, time to reach peak plasma concentration Cmax (tmax) often occurs as late as 4h after oral administration. This phenomenon likely occurs due to lysosomal trapping of dextromethorphan in the intestinal mucosa. However, other processes, such as renal excretion may also be affected by lysosomal trapping in the respective tissue. In short, lipophilic amines (logP > 1, acid dissociation constant (pKa) > 6) accumulate in lysosomes due to rapid diffusion across the lysosomal membrane in unionized form. Subsequently, due to the acidic environment in lysosomes (pH 4–5), the amine is then ionized and thus unable to
permeate back into the cytosol. The information necessary to physiologically implement lysosomal trapping (i.e. relative abundances of lysosomes in relevant tissues and diffusion constants for permeation across lysosomal membranes) are not yet available in the literature. Hence, intestinal lysosomal trapping was implemented as follows: First, a surrogate protein binding partner was expressed in high abundances (500 μmol/L) in the relevant tissues (duodenum, upper jejunum, lower jejunum, upper ileum and lower ileum, each 100% of relative expression). Secondly, a corresponding protein binding process was implemented for dextromethorphan. Finally, the relevant parameters
for the binding process - dissociation rate constant (k<sub>off</sub>) and dissociation constant (K<sub>D</sub>) - were informed by parameter optimization. For a comprehensive explanation on the process of lysosomal trapping under physiological circumstances, please refer to [Kazmi 2013](#main-references). The final parameters for K<sub>D</sub> and k<sub>off</sub> are given in [Section 2.3.4](#parameter-identification).

### 2.3.2 Distribution <a id="model-parameters-and-assumptions-distribution"></a>

After testing the available organ-plasma partition coefficient and cell permeability calculation methods built in PK-Sim, observed Dextromethorphan and dextrorphan clinical data was best described by choosing the partition coefficient calculation by `Berezhkovskiy` and cellular permeability calculation by `PK-Sim Standard`. For the Dextrorphan-O-glucuronide clinical data was best described by choosing the partition coefficient calculation by `Berezhkovskiy` and cellular permeability calculation by `Charge dependent Schmitt`.

### 2.3.3 Metabolism and Elimination <a id="model-parameters-and-assumptions-metabolism-and-elimination"></a>

Dextromethorphan-O-demethylation via CYP2D6 leads to the formation of the major active metabolite dextrorphan. Dextrorphan subsequently undergoes rapid glucuronidation via uridine diphosphate-glucuronosyltransferases 2B (UGT2Bs), namely UGT2B15, or N-demethylation via CYP3A4. **Figure 1** shows the implemented metabolic pathways.

**Figure** **1: Implemented dextromethorphan metabolic pathways.**
<a id="figure-2-1"></a>

![Figure 1](images/Figure_1.PNG)

Alternatively, dextromethorphan is N-demethylated by CYP3A4, which was found to be the main pathway of dextromethorphan metabolism in CYP2D6 PMs. Depending on the CYP2D6 phenotype, up to 50% of orally administered dextromethorphan is excreted unchanged in urine. For dextromethorphan and Dextrorphan, a GFR fraction of 1 was assumed. Dextrorphan O-glucuronide is renally eliminated via passive glomerular filtration and active secretion to the urine, a GFR fraction > 1 was optimized and represents both processes [Section 2.3.4](#parameter-identification).

The principal pathway of dextromethorphan metabolism is the CYP2D6-mediated O-demethylation, leading to the formation of dextrorphan. This pathway was implemented using Michaelis-Menten kinetics. Cytochrome P450 2D6 (CYP2D6) was implemented in accordance with literature, using the PK-Sim® expression database to define their relative expression in the different organs of the body [PK-Sim Ontogeny Database Version 7.3](#main-references). The CYP2D6, CYP3A4 and UGT2B15 expression profiles are based on high-sensitive real-time RT-PCR [Nishimura 2013](#main-references). The t1/2 for CYP3A4 was decreased from the default 37h to 36h and the reference concentration for UGT2B15 was increased from 1 µmol/L to 2.48 µmol/L for a better fit of the clinical data.

Because the CYP2D6 gene is prone to genetic alterations, dextromethorphan pharmacokinetics is subject to considerable drug-gene interaction (DGI) effects. For DGI modeling, the CYP2D6 Michaelis-Menten constant(K<sub>M</sub>) values for the dextromethorphan O-demethylation were kept constant over the whole range of modeled activity scores [Rüdesheim 2020](#main-references). CYP2D6 k<sub>cat</sub> values were optimized separately for each activity score. CYP2D6 poor metabolizers (PMs) (activity score = 0) were assumed to show no CYP2D6 activity (0%), whereas populations with two wildtype alleles (activity score = 2) were assumed to possess normal CYP2D6 activity (100%). Activity scores were assigned according to Caudle et al. 2017 [Caudle 2017](#main-references), see also table below. IM = intermediate metabolizers, NM = normal metabolizers and UM = ultra-rapid metabolizer.

| Activity Score | Projected | k<sub>cat</sub> -> dxt (1/min)    | k<sub>cat</sub> percentage of Reference (AS = 2) (%)|
| --------------- | ---------- | ---------------- |-------|
| 0.00            | PM        | 0.0               | 0   |
| 0.25            |           | 5.3               | 2   |
| 0.50            | IM        | 32.9              | 14  |
| 1.00            |           | 96.6              | 40  |      
| 1.25            |           | 115.2             | 48  |
| 1.50            | NM        | 151.8             | 63  |
| 2.00            |           | 242.5             | 100 |
| 3.00            | UM        | 413.2             | 170 |

In addition, fraction of bile that was continuously released was assumed 1 (`EHC continuous fraction`) for Dextromethorphan and its metabolites.

### 2.3.4 Automated Parameter Identification <a id="parameter-identification"></a>
| Model Parameter               | 
| --------------- | 
| **Dextromethorphan** | 
| Weibull time parameter    |   
| Weibull shape parameter    | 
| Intestinal permeability    |    
| CYP2D6 K<sub>cat</sub> -> dxt (EM) | 
| CYP3A4 K<sub>cat</sub>        | 
| Lysosomal trapping K<sub>D</sub>  | 
| Lysosomal trapping k<sub>off</sub> | 
| **Dextrorphan** |
| UGT2B15 K<sub>cat</sub> -> dxt (EM) |  
| **Dextrorphan-O-glucuronide** |
| logP |
| GFR fraction |

# 3 Results and Discussion<a id="3"></a>

The PBPK model for Dextromethorphan, dextrorphan and dextrorphan O-glucuronide was developed and verified with clinical pharmacokinetic data.

The PBPK model for dextromethorphan was developed and verified with clinical pharmacokinetic data. The model was built and evaluated covering data from studies including in particular 
intravenous (0.5 mg/kg 30 min infusion ) 
oral administrations (dose range 5 to 80 mg).

The model quantifies metabolism of dextromethorphan via mainly CYP2D6, and includes the possibility to include K<sub>cat</sub> activity scores for CYP2D6 to simulate different CYP2D6 phenotypes.

The next sections show:

1. the overall goodness of fit: [Section 3.2](#diagnostics-plots).
2. simulated vs. observed concentration-time profiles for the clinical studies used for model building and for model verification: [Section 3.3](#ct-profiles).

## 3.1 Final input parameters<a id="final-input-parameters"></a>

The compound parameter values of the final PBPK model are illustrated below.

### Compound: Dextromethorphan

#### Parameters

Name                                             | Value           | Value Origin                                                | Alternative | Default
------------------------------------------------ | --------------- | ----------------------------------------------------------- | ----------- | -------
Solubility at reference pH                       | 15 mg/ml        | Publication-Other-Benet 2011                                | Measurement | True   
Reference pH                                     | 7.4             | Publication-Other-Benet 2011                                | Measurement | True   
Lipophilicity                                    | 4.1 Log Units   | Publication-Other-Spaggiari 2014                            | LogP        | True   
Fraction unbound (plasma, reference value)       | 0.35            | Publication-Other-Taylor 2016                               | Measurement | True   
Specific intestinal permeability (transcellular) | 2.48E-06 cm/min | Parameter Identification-Parameter Identification-Optimized | Optimized   | True   
Is small molecule                                | Yes             |                                                             |             |        
Molecular weight                                 | 271.4 g/mol     | Unknown-Sager 2014                                          |             |        
Plasma protein binding partner                   | Unknown         |                                                             |             |        

#### Calculation methods

Name                    | Value          
----------------------- | ---------------
Partition coefficients  | Berezhkovskiy  
Cellular permeabilities | PK-Sim Standard

#### Processes

##### Systemic Process: Glomerular Filtration-Assumption

Species: Human

###### Parameters

Name         | Value | Value Origin
------------ | -----:| ------------:
GFR fraction |     1 |             

##### Metabolizing Enzyme: CYP2D6-Lutz 2012 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                          | Value Origin                                               
------------------------------------------- | ------------------------------ | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0.134 nmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein        | Unknown                                                    
Km                                          | 4.65 µmol/l                    | Publication-In Vitro-Lutz 2012                             
kcat                                        | 90.89 1/min                    | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP3A4-Brown 2007 (sink)

Molecule: CYP3A4

###### Parameters

Name                               | Value                         | Value Origin                                               
---------------------------------- | ----------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes | 0.61 nmol/min/mg mic. protein |                                                            
Km                                 | 176.8 µmol/l                  | Publication-In Vitro-Brown 2007                            
kcat                               | 7.98 1/min                    | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP2D6-AS=0 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin
------------------------------------------- | -------------------------- | ------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |             
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown     
Km                                          | 4.65 µmol/l                |             

##### Metabolizing Enzyme: CYP2D6-AS=2 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin                                     
------------------------------------------- | -------------------------- | -------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                  
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                          
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                   
kcat                                        | 242.45 1/min               | Parameter Identification-Parameter Identification

##### Metabolizing Enzyme: CYP2D6-AS=1.25 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin                                               
------------------------------------------- | -------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                    
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                             
kcat                                        | 115.17 1/min               | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP2D6-AS=0.5 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin                                               
------------------------------------------- | -------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                    
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                             
kcat                                        | 32.91 1/min                | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP2D6-AS=1 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin                                               
------------------------------------------- | -------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                    
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                             
kcat                                        | 96.64 1/min                | Parameter Identification-Parameter Identification-Optimized

##### Specific Binding: lysosomal trapping mucosa-lysosomal trapping (Kazmi 2013)

Molecule: lysosomal trapping mucosa

###### Parameters

Name | Value           | Value Origin                                               
---- | --------------- | -----------------------------------------------------------
koff | 709939.97 1/min | Parameter Identification-Parameter Identification-Optimized
Kd   | 74.21 µmol/l    | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP2D6-AS=0.25 (dextrorphan)

Molecule: CYP2D6

Metabolite: Dextrorphan

###### Parameters

Name                                        | Value                      | Value Origin                                               
------------------------------------------- | -------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                    
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                             
kcat                                        | 5.3 1/min                  | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: CYP2D6-AS=1.5 (dextrorphan)

Molecule: CYP2D6

###### Parameters

Name                                        | Value                      | Value Origin                                                                                                         
------------------------------------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                                                                                      
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                                                                              
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                                                                                       
kcat                                        | 151.81 1/min               | Parameter Identification-Parameter Identification-Value updated from 'Parameter Identification 6' on 2021-01-19 09:07

##### Metabolizing Enzyme: CYP2D6-AS=3 (dextrorphan)

Molecule: CYP2D6

###### Parameters

Name                                        | Value                      | Value Origin                                               
------------------------------------------- | -------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 0 pmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 10 pmol/mg mic. protein    | Unknown                                                    
Km                                          | 4.65 µmol/l                | Publication-In Vitro-Lutz 2012                             
kcat                                        | 413.19 1/min               | Parameter Identification-Parameter Identification-Optimized

### Formulation: capsule/solution

Type: Dissolved

### Formulation: cocktail capsule (weibull)

Type: Weibull

#### Parameters

Name                             | Value     | Value Origin                                               
-------------------------------- | --------- | -----------------------------------------------------------
Dissolution time (50% dissolved) | 46.05 min | Parameter Identification-Parameter Identification-Optimized
Lag time                         | 0 min     |                                                            
Dissolution shape                | 1.05      | Parameter Identification-Parameter Identification-Optimized
Use as suspension                | Yes       |                                                            

### Compound: Dextrorphan

#### Parameters

Name                                       | Value         | Value Origin                       | Alternative | Default
------------------------------------------ | ------------- | ---------------------------------- | ----------- | -------
Solubility at reference pH                 | 0.17 mg/ml    | Database-Other-metabocard/ChemAxon | Measurement | True   
Reference pH                               | 7.4           | Database-Other-metabocard/ChemAxon | Measurement | True   
Lipophilicity                              | 2.9 Log Units | Database-Other-metabocard/ChemAxon | LogP        | True   
Fraction unbound (plasma, reference value) | 0.42          | Publication-Other-Watanabe 2008    | Measurement | True   
Is small molecule                          | Yes           |                                    |             |        
Molecular weight                           | 257.37 g/mol  | Database-Other-metabocard/ChemAxon |             |        
Plasma protein binding partner             | Unknown       |                                    |             |        

#### Calculation methods

Name                    | Value          
----------------------- | ---------------
Partition coefficients  | Berezhkovskiy  
Cellular permeabilities | PK-Sim Standard

#### Processes

##### Systemic Process: Glomerular Filtration-Assumption

Species: Human

###### Parameters

Name         | Value | Value Origin
------------ | -----:| ------------:
GFR fraction |     1 |             

##### Metabolizing Enzyme: CYP3A4-Lutz 2012 (sink)

Molecule: CYP3A4

###### Parameters

Name                               | Value                        | Value Origin                                               
---------------------------------- | ---------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes | 0.8 nmol/min/mg mic. protein |                                                            
Km                                 | 910 µmol/l                   | Publication-In Vitro-Lutz 2012                             
kcat                               | 7.41 1/min                   | Parameter Identification-Parameter Identification-Optimized

##### Metabolizing Enzyme: UGT2B15-Lutz 2012 (Dextrorphan-O-glucuronide)

Molecule: UGT2B15

Metabolite: Dextrorphan-O-glucuronide

###### Parameters

Name                                        | Value                        | Value Origin                                               
------------------------------------------- | ---------------------------- | -----------------------------------------------------------
In vitro Vmax for liver microsomes          | 2.3 nmol/min/mg mic. protein |                                                            
Content of CYP proteins in liver microsomes | 62.1 pmol/mg mic. protein    | Publication-In Vitro-Achour 2014                           
Km                                          | 184.8 µmol/l                 | Publication-In Vitro-Lutz 2012                             
kcat                                        | 1137.98 1/min                | Parameter Identification-Parameter Identification-Optimized

### Compound: Dextrorphan-O-glucuronide

#### Parameters

Name                                       | Value                  | Value Origin                                                                                                                | Alternative | Default
------------------------------------------ | ---------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------- | -------
Solubility at reference pH                 | 1200 mg/l              | Parameter Identification-Parameter Identification-Value updated from 'Parameter Identification 6 - sol' on 2021-02-26 11:12 | Measurement | True   
Reference pH                               | 7                      |                                                                                                                             | Measurement | True   
Lipophilicity                              | 0.2928540753 Log Units | Parameter Identification-Parameter Identification-Value updated from 'Parameter Identification 6 - sol' on 2021-02-26 11:12 | LogP        | True   
Fraction unbound (plasma, reference value) | 0.366                  | Parameter Identification-Parameter Identification-Value updated from 'Parameter Identification 5' on 2020-10-21 11:17       | Measurement | True   
Is small molecule                          | Yes                    |                                                                                                                             |             |        
Molecular weight                           | 433.5 g/mol            |                                                                                                                             |             |        
Plasma protein binding partner             | Albumin                |                                                                                                                             |             |        

#### Calculation methods

Name                    | Value                   
----------------------- | ------------------------
Partition coefficients  | Berezhkovskiy           
Cellular permeabilities | Charge dependent Schmitt

#### Processes

##### Systemic Process: Glomerular Filtration-Assumption

Species: Human

###### Parameters

Name         | Value | Value Origin                                               
------------ | -----:| -----------------------------------------------------------
GFR fraction |  4.92 | Parameter Identification-Parameter Identification-Optimized

## 3.2 Dextromethorphan and its metabolites Diagnostics Plots<a id="31"></a>

Below you find the goodness-of-fit visual diagnostic plots for the PBPK model performance of all data presented in [Section 2.2.2](#clinical-data).

The first plot shows observed versus simulated plasma concentration, the second weighted residuals versus time. 

<a id="table-3-1"></a>

**Table 3-1: GMFE for Goodness of fit plot for Dextromethorphan concentration in plasma**

|Group                                          |GMFE |
|:----------------------------------------------|:----|
|Dextromethorphan IV (Model building)           |1.40 |
|Dextromethorphan oral (Model building)         |1.36 |
|Dextromethorphan oral (Model verification)     |2.03 |
|Dextromethorphan oral DGI (Model building)     |1.46 |
|Dextromethorphan oral DGI (Model verification) |1.77 |
|All                                            |1.75 |

<br>
<br>

<a id="figure-3-1"></a>

![](images/006_section_3/008_section_31/2_gof_plot_predictedVsObserved.png)

**Figure 3-1: Goodness of fit plot for Dextromethorphan concentration in plasma**

<br>
<br>

<a id="figure-3-2"></a>

![](images/006_section_3/008_section_31/3_gof_plot_residualsOverTime.png)

**Figure 3-2: Goodness of fit plot for Dextromethorphan concentration in plasma**

<br>
<br>

<a id="table-3-2"></a>

**Table 3-2: GMFE for Goodness of fit plot for Dextromethorphan metabolites concentration in plasma**

|Group                                      |GMFE |
|:------------------------------------------|:----|
|Dextrorphan (Model building)               |1.60 |
|Dextrorphan (Model verification)           |3.09 |
|Dextrorphan DGI (Model building)           |1.20 |
|Dextrorphan DGI (Model verification)       |2.17 |
|Dextrorphan O-glucuronide (Model building) |1.55 |
|Dextrorphan total (Model development)      |1.72 |
|Dextrorphan total (Model verification)     |1.79 |
|Dextrorphan total DGI (Model development)  |1.18 |
|Dextrorphan total DGI (Model verification) |1.53 |
|All                                        |1.93 |

<br>
<br>

<a id="figure-3-3"></a>

![](images/006_section_3/008_section_31/5_gof_plot_predictedVsObserved.png)

**Figure 3-3: Goodness of fit plot for Dextromethorphan metabolites concentration in plasma**

<br>
<br>

<a id="figure-3-4"></a>

![](images/006_section_3/008_section_31/6_gof_plot_residualsOverTime.png)

**Figure 3-4: Goodness of fit plot for Dextromethorphan metabolites concentration in plasma**

<br>
<br>

## 3.3 Concentration-Time Profiles<a id="32"></a>

Simulated versus observed concentration-time profiles of all data listed in [Section 2.2.2](#clinical-data) are presented below.

### 3.3.1 Model Building<a id="321"></a>

<a id="figure-3-5"></a>

![](images/006_section_3/009_section_32/010_section_321/1_time_profile_plot_Dextromethorphan_Duedahl2005_EM_0_5mgkg_DexBase.png)

**Figure 3-5: Duedahl 2005 EM, 0.5 mg/kg dextromethorphan base (infusion), n=24**

<br>
<br>

<a id="figure-3-6"></a>

![](images/006_section_3/009_section_32/010_section_321/2_time_profile_plot_Dextromethorphan_Schadel1995_EM_30mg_DexHBr.png)

**Figure 3-6: Schadel 1995 EM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=5**

<br>
<br>

<a id="figure-3-7"></a>

![](images/006_section_3/009_section_32/010_section_321/3_time_profile_plot_Dextromethorphan_Schadel1995_PM_30mg_DexHBr.png)

**Figure 3-7: Schadel 1995 PM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=4**

<br>
<br>

<a id="figure-3-8"></a>

![](images/006_section_3/009_section_32/010_section_321/4_time_profile_plot_Dextromethorphan_Tenneze1999_EM_80mg_DexHBr.png)

**Figure 3-8: Tennezé 1999 EM, 80 mg dextromethorphan hydrobromide (capsule/solution), n=36**

<br>
<br>

<a id="figure-3-9"></a>

![](images/006_section_3/009_section_32/010_section_321/17_time_profile_plot_Dextromethorphan_Qiu2016_IM_15mg_DexHBR_AS_0_5.png)

**Figure 3-9: Qiu 2016 IM, 15 mg dextromethorphan hydrobromide (capsule/solution), n=6, AS=0.5**

<br>
<br>

<a id="figure-3-10"></a>

![](images/006_section_3/009_section_32/010_section_321/18_time_profile_plot_Dextromethorphan_Qiu2016_NM_15mg_DexHBr_AS_1_25.png)

**Figure 3-10: Qiu 2016 NM, 15 mg dextromethorphan hydrobromide (capsule/solution), n=6, AS=1.25**

<br>
<br>

<a id="figure-3-11"></a>

![](images/006_section_3/009_section_32/010_section_321/19_time_profile_plot_Dextromethorphan_Qiu2016_NM_15mg_DexHBr_AS_2.png)

**Figure 3-11: Qiu 2016 NM, 15 mg dextromethorphan hydrobromide (capsule/solution), n=6, AS=2**

<br>
<br>

<a id="figure-3-12"></a>

![](images/006_section_3/009_section_32/010_section_321/20_time_profile_plot_Dextromethorphan_Zawertailo2009_NM_3_mg_kg_DexHBr_AS_2.png)

**Figure 3-12: Zawertailo 2009 NM, 3 mg/kg dextromethorphan hydrobromide (capsule/solution), n=6, AS=2**

<br>
<br>

### 3.3.2 Model Verification<a id="322"></a>

<a id="figure-3-13"></a>

![](images/006_section_3/009_section_32/011_section_322/5_time_profile_plot_Dextromethorphan_AntecipBioventures_EM_60mg_DexHBr_QD.png)

**Figure 3-13: Antecip Bioventures EM, 60 mg dextromethorphan hydrobromide multiple dose (capsule/solution), n=10**

<br>
<br>

<a id="figure-3-14"></a>

![](images/006_section_3/009_section_32/011_section_322/6_time_profile_plot_Dextromethorphan_Armani2017_EM_30mg_DexHBr.png)

**Figure 3-14: Armani 2017 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=20**

<br>
<br>

<a id="figure-3-15"></a>

![](images/006_section_3/009_section_32/011_section_322/7_time_profile_plot_Dextromethorphan_Dumond2010_EM_30mg_DexHBr.png)

**Figure 3-15: Dumond 2010 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=23**

<br>
<br>

<a id="figure-3-16"></a>

![](images/006_section_3/009_section_32/011_section_322/8_time_profile_plot_Dextromethorphan_Edwards2017_EM__30mg_DexHBr.png)

**Figure 3-16: Edwards 2017 EM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=48**

<br>
<br>

<a id="figure-3-17"></a>

![](images/006_section_3/009_section_32/011_section_322/9_time_profile_plot_Dextromethorphan_Ermer2015_EM_30mg_DexHBr.png)

**Figure 3-17: Ermer 2015 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=30**

<br>
<br>

<a id="figure-3-18"></a>

![](images/006_section_3/009_section_32/011_section_322/10_time_profile_plot_Dextromethorphan_Feld2013_EM_60mg_DexHBr.png)

**Figure 3-18: Feld 2013 EM, 60 mg dextromethorphan hydrobromide (capsule/solution), n=17**

<br>
<br>

<a id="figure-3-19"></a>

![](images/006_section_3/009_section_32/011_section_322/11_time_profile_plot_Dextromethorphan_Kakuda2014_EM_30mg_DexHBr.png)

**Figure 3-19: Kakuda 2014 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=14**

<br>
<br>

<a id="figure-3-20"></a>

![](images/006_section_3/009_section_32/011_section_322/12_time_profile_plot_Dextromethorphan_Khalilieh2018_EM_30mg_DexHBr.png)

**Figure 3-20: Khalilieh 2018 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=20**

<br>
<br>

<a id="figure-3-21"></a>

![](images/006_section_3/009_section_32/011_section_322/13_time_profile_plot_Dextromethorphan_Nakashima2007_EM_30mg_DexHBr.png)

**Figure 3-21: Nakashima 2007 EM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=24**

<br>
<br>

<a id="figure-3-22"></a>

![](images/006_section_3/009_section_32/011_section_322/14_time_profile_plot_Dextromethorphan_Nyunt2008_EM_30mg_DexHBr.png)

**Figure 3-22: Nyunt 2008 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=12**

<br>
<br>

<a id="figure-3-23"></a>

![](images/006_section_3/009_section_32/011_section_322/15_time_profile_plot_Dextromethorphan_Sager2014_EM_30mg_DexHBr.png)

**Figure 3-23: Sager 2014 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=10**

<br>
<br>

<a id="figure-3-24"></a>

![](images/006_section_3/009_section_32/011_section_322/16_time_profile_plot_Dextromethorphan_Stage2018_EM_30mg_DexHBr.png)

**Figure 3-24: Stage 2018 EM, 30 mg dextromethorphan hydrobromide (cocktail), n=12**

<br>
<br>

<a id="figure-3-25"></a>

![](images/006_section_3/009_section_32/011_section_322/21_time_profile_plot_Dextromethorphan_Capon1996_EM_30mg_DexHBr.png)

**Figure 3-25: Capon 1996 EM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=6**

<br>
<br>

<a id="figure-3-26"></a>

![](images/006_section_3/009_section_32/011_section_322/22_time_profile_plot_Dextromethorphan_Capon1996_PM_30mg_DexHBr.png)

**Figure 3-26: Capon 1996 PM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=6**

<br>
<br>

<a id="figure-3-27"></a>

![](images/006_section_3/009_section_32/011_section_322/23_time_profile_plot_Dextromethorphan_Gazzaz2018_NM_30mg_DexHBr_AS_1_25.png)

**Figure 3-27: Gazzaz 2018 NM, 30 mg dextromethorphan hydrobromide (cocktail), n=30, AS=1.25**

<br>
<br>

<a id="figure-3-28"></a>

![](images/006_section_3/009_section_32/011_section_322/24_time_profile_plot_Dextromethorphan_Gorski2004_EM_30mg_DexHBr.png)

**Figure 3-28: Gorski 2004 EM, 30 mg dextromethorphan hydromide (capsule/solution), n=11**

<br>
<br>

<a id="figure-3-29"></a>

![](images/006_section_3/009_section_32/011_section_322/25_time_profile_plot_Dextromethorphan_Gorski2004_PM_30mg_DexHBr.png)

**Figure 3-29: Gorski 2004 PM, 30 mg dextromethorphan hydromide (capsule/solution), n=1**

<br>
<br>

<a id="figure-3-30"></a>

![](images/006_section_3/009_section_32/011_section_322/26_time_profile_plot_Dextromethorphan_Storelli2018_IM_5mg_DexBase.png)

**Figure 3-30: Storelli 2018 IM, 5 mg dextromethorphan base (capsule/solution), n=16**

<br>
<br>

<a id="figure-3-31"></a>

![](images/006_section_3/009_section_32/011_section_322/27_time_profile_plot_Dextromethorphan_Storelli2018_NM_5mg_DexBase_AS_2.png)

**Figure 3-31: Storelli 2018 NM, 5 mg dextromethorphan base (capsule/solution), n=17, AS=2**

<br>
<br>

<a id="figure-3-32"></a>

![](images/006_section_3/009_section_32/011_section_322/28_time_profile_plot_Dextromethorphan_Yamazaki2017_IM_30mg_DexHBr_AS_0_5.png)

**Figure 3-32: Yamazaki 2017 IM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=12, AS=0.5**

<br>
<br>

<a id="figure-3-33"></a>

![](images/006_section_3/009_section_32/011_section_322/29_time_profile_plot_Dextromethorphan_Yamazaki2017_NM_30mg_DexHBr_AS_2.png)

**Figure 3-33: Yamazaki 2017 NM, 30 mg dextromethorphan hydrobromide (capsule/solution), n=11, AS=2**

<br>
<br>

# 4 Conclusion<a id="4"></a>

The herein presented PBPK model adequately describes the pharmacokinetics of Dextromethorphan and its metabolites in adults with different CYP2D6 phenotypes.

In particular, it applies quantitative metabolism by CYP2D6 which can be adapted to a CYP2D6 phenotype using CYP2D6 k<sub>cat</sub> activity score scalars. Thus, the model is fit for purpose to be applied for the investigation of DDIs with regards to CYP2D6 metabolism in adults with different CYP2D6 phenotypes.

# 5 References<a id="5"></a>

**Armani 2017** Armani S, Ting L, Sauter N, et al. Drug Interaction Potential of Osilodrostat (LCI699) Based on Its Effect on the Pharmacokinetics of Probe Drugs of Cytochrome P450 Enzymes in Healthy Adults. Clin Drug Investig. 2017;37(5):465-472. doi:10.1007/s40261-017-0497-0

**Benet 2011** Benet LZ, Broccatelli F, Oprea TI. BDDCS applied to over 900 drugs. AAPS J. 2011;13(4):519-547. doi:10.1208/s12248-011-9290-9

**Berezhkovskiy 2004** 1. Berezhkovskiy LM. Volume of distribution at steady state for a linear pharmacokinetic system with peripheral elimination. J Pharm Sci. 2004;93(6):1628-1640. doi:10.1002/jps.20073

**Brown 2007** Brown HS, Griffin M, Houston JB. Evaluation of cryopreserved human hepatocytes as an alternative in vitro system to microsomes for the prediction of metabolic clearance. Drug Metab Dispos. 2007;35(2):293-301. doi:10.1124/dmd.106.011569

**Caudle 2017** Caudle KE, Sangkuhl K, Whirl-Carrillo M, et al. Standardizing CYP2D6 Genotype to Phenotype Translation: Consensus Recommendations from the Clinical Pharmacogenetics Implementation Consortium and Dutch  Pharmacogenetics Working Group. Clin Transl Sci. 2020;13(1):116-124. doi:10.1111/cts.12692

**Capon 1996** Capon DA, Bochner F, Kerry N, Mikus G, Danz C, Somogyi AA. The influence of CYP2D6 polymorphism and quinidine on the disposition and antitussive effect of dextromethorphan in humans. Clin Pharmacol Ther. 1996;60(3):295-307. doi:10.1016/S0009-9236(96)90056-9

**Duedahl 2004** Duedahl TH, Dirks J, Petersen KB, Romsing J, Larsen NE, Dahl JB. Intravenous dextromethorphan to human volunteers: relationship between pharmacokinetics and anti-hyperalgesic effect. Pain. 2005;113(3):360-368. doi:10.1016/j.pain.2004.11.015

**Dumond 2010** Dumond JB, Vourvahis M, Rezk NL, et al. A phenotype-genotype approach to predicting CYP450 and P-glycoprotein drug interactions with the mixed inhibitor/inducer tipranavir/ritonavir. Clin Pharmacol Ther. 2010;87(6):735-742. doi:10.1038/clpt.2009.253

**Edwards 2017** Edwards JE, Eliot L, Parkinson A, Karan S, MacConell L. Assessment of Pharmacokinetic Interactions Between Obeticholic Acid and Caffeine, Midazolam, Warfarin, Dextromethorphan, Omeprazole, Rosuvastatin, and Digoxin in  Phase 1 Studies in Healthy Subjects. Adv Ther. 2017;34(9):2120-2138. doi:10.1007/s12325-017-0601-0

**Ermer 2015** Ermer J, Corcoran M, Martin P. Lisdexamfetamine Dimesylate Effects on the Pharmacokinetics of Cytochrome P450 Substrates in Healthy Adults in an Open-Label, Randomized, Crossover Study. Drugs R D. 2015;15(2):175-185. doi:10.1007/s40268-015-0090-z

**Feld 2013** Feld R, Woo MM, Leighl N, et al. A clinical investigation of inhibitory effect of panobinostat on CYP2D6 substrate in patients with advanced cancer. Cancer Chemother Pharmacol. 2013;72(4):747-755. doi:10.1007/s00280-013-2237-3

**Frank 2009** Frank, D. Bewertung von pharmakokinetischen Parametern zur Phänotypisierung des menschlichen Cytochrom P450 Enzyms CYP2D6 mittels Dextromethorphan PhD thesis (Rheinische Friedrich-Wilhelms-Universität Bonn, 2009).

**Gazzaz 2018** Gazzaz M, Kinzig M, Schaeffeler E, et al. Drinking Ethanol Has Few Acute Effects on CYP2C9, CYP2C19, NAT2, and P-Glycoprotein Activities but Somewhat Inhibits CYP1A2, CYP2D6, and Intestinal  CYP3A: So What? Clin Pharmacol Ther. 2018;104(6):1249-1259. doi:10.1002/cpt.1083

**Gorski 2004** Gorski JC, Huang SM, Pinto A, et al. The effect of echinacea (Echinacea purpurea root) on cytochrome P450 activity in vivo. Clin Pharmacol Ther. 2004;75(1):89-100. doi:10.1016/j.clpt.2003.09.013

**HMDB-a** Human Metabolome Database: Showing metabocard for Dextrorphan (HMDB0060552)

**HMDB-b** Human Metabolome Database: Showing metabocard for Dextrorphan O-glucuronide (HMDB0010341)

**Kakuda 2014** Kakuda TN, Van Solingen-Ristea RM, Onkelinx J, et al. The effect of single- and multiple-dose etravirine on a drug cocktail of representative cytochrome P450 probes and digoxin in healthy subjects. J Clin Pharmacol. 2014;54(4):422-431. doi:10.1002/jcph.214

**Kazmi 2013** Kazmi F, Hensley T, Pope C, et al. Lysosomal sequestration (trapping) of lipophilic amine (cationic amphiphilic) drugs in immortalized human hepatocytes (Fa2N-4 cells). Drug Metab Dispos. 2013;41(4):897-905. doi:10.1124/dmd.112.050054

**Khalilieh 2018** Khalilieh S, Hussain A, Montgomery D, et al. Effect of tildrakizumab (MK-3222), a high affinity, selective anti-IL23p19 monoclonal antibody, on cytochrome P450 metabolism in subjects with moderate to  severe psoriasis. Br J Clin Pharmacol. 2018;84(10):2292-2302. doi:10.1111/bcp.13670

**Kim 2019** Kim S, Chen J, Cheng T, et al. PubChem 2019 update: improved access to chemical data. Nucleic Acids Res. 2019;47(D1):D1102-D1109. doi:10.1093/nar/gky1033

**Kuepfer 2016** Kuepfer L, Niederalt C, Wendl T, Schlender JF, Willmann S, Lippert J, Block M, Eissing T, Teutonico D. Applied Concepts in PBPK Modeling: How to Build a PBPK/PD Model. CPT Pharmacometrics Syst Pharmacol. 2016 Oct;5(10):516-531. doi: 10.1002/psp4.12134. Epub 2016 Oct 19. 	

**Lutz 2012** Lutz JD, Isoherranen N. Prediction of relative in vivo metabolite exposure from in vitro data using two model drugs: dextromethorphan and omeprazole. Drug Metab Dispos. 2012;40(1):159-168. doi:10.1124/dmd.111.042200

**Nakashima 2007** Nakashima D, Takama H, Ogasawara Y, et al. Effect of cinacalcet hydrochloride, a new calcimimetic agent, on the pharmacokinetics of dextromethorphan: in vitro and clinical studies. J Clin Pharmacol. 2007;47(10):1311-1319. doi:10.1177/0091270007304103

**Nishimura 2013** Nishimura M, Yaguti H, Yoshitsugu H, Naito S, Satoh T. Tissue distribution of mRNA expression of human cytochrome P450 isoforms assessed by high-sensitivity real-time reverse transcription PCR. Yakugaku Zasshi. 2003;123(5):369-375. doi:10.1248/yakushi.123.369

**Nyunt 2008** Nyunt MM, Becker S, MacFarland RT, et al. Pharmacokinetic effect of AMD070, an Oral CXCR4 antagonist, on CYP3A4 and CYP2D6 substrates midazolam and dextromethorphan in healthy volunteers. J Acquir Immune Defic Syndr. 2008;47(5):559-565. doi:10.1097/QAI.0b013e3181627566

**AntecipBioventuresLLC** Antecip Bioventures, LLC. US9370513B2 - Compositions and methods for increasing the metabolic lifetime of dextromethorphan and related pharmacodynamic effects 2016. https://patents.google.com/patent/US9370513B2/en

**PK-Sim Ontogeny Database Version 7.3** ([https://github.com/Open-Systems-Pharmacology/OSPSuite.Documentation/blob/38cf71b384cfc25cfa0ce4d2f3addfd32757e13b/PK-Sim%20Ontogeny%20Database%20Version%207.3.pdf](https://github.com/Open-Systems-Pharmacology/OSPSuite.Documentation/blob/38cf71b384cfc25cfa0ce4d2f3addfd32757e13b/PK-Sim%20Ontogeny%20Database%20Version%207.3.pdf))	

**Qiu 2016** Qiu F, Liu S, Miao P, et al. Effects of the Chinese herbal formula “Zuojin Pill” on the pharmacokinetics of dextromethorphan in healthy Chinese volunteers with CYP2D6*10 genotype. Eur J Clin Pharmacol. 2016;72(6):689-695. doi:10.1007/s00228-016-2048-7

**Rüdesheim 2020** Rüdesheim S, Wojtyniak JG, Selzer D, et al. Physiologically Based Pharmacokinetic Modeling of Metoprolol Enantiomers and α-Hydroxymetoprolol to Describe CYP2D6 Drug-Gene Interactions. Pharmaceutics. 2020;12(12). doi:10.3390/pharmaceutics12121200

**Rüdesheim 2025** Rüdesheim S, Loer HLH, Feick D, et al. A Comprehensive CYP2D6 Drug-Drug-Gene Interaction Network for Application in Precision Dosing and Drug Development. Clin Pharmacol Ther. 2025;117(6):1718-1731. doi:10.1002/cpt.3604

**Sager 2014** Sager JE, Lutz JD, Foti RS, Davis C, Kunze KL, Isoherranen N. Fluoxetine- and norfluoxetine-mediated complex drug-drug interactions: in vitro to in vivo correlation of effects on CYP2D6, CYP2C19, and CYP3A4. Clin Pharmacol Ther. 2014;95(6):653-662. doi:10.1038/clpt.2014.50

**Schadel 1995** Schadel M, Wu D, Otton SV, Kalow W, Sellers EM. Pharmacokinetics of dextromethorphan and metabolites in humans: influence of the CYP2D6 phenotype and quinidine inhibition. J Clin Psychopharmacol. 1995;15(4):263-269. doi:10.1097/00004714-199508000-00005

**Schlender 2016** Schlender JF, Meyer M, Thelen K, Krauss M, Willmann S, Eissing T, Jaehde U. Development of a Whole-Body Physiologically Based Pharmacokinetic Approach to Assess the Pharmacokinetics of Drugs in Elderly Individuals. Clin Pharmacokinet. 2016 Dec;55(12):1573-1589. 

**Schmitt 2008** Schmitt W. General approach for the calculation of tissue to plasma partition coefficients. Toxicol In Vitro. 2008;22(2):457-467. doi:10.1016/j.tiv.2007.09.010

**Spaggiari 2014** Spaggiari D, Mehl F, Desfontaine V, et al. Comparison of liquid chromatography and supercritical fluid chromatography coupled to compact single quadrupole mass spectrometer for targeted in vitro  metabolism assay. J Chromatogr A. 2014;1371:244-256. doi:10.1016/j.chroma.2014.10.055

**Stage 2018** Stage TB, Graff M, Wong S, et al. Dicloxacillin induces CYP2C19, CYP2C9 and CYP3A4 in vivo and in vitro. Br J Clin Pharmacol. 2018;84(3):510-519. doi:10.1111/bcp.13467

**Storelli 2018** Storelli F, Matthey A, Lenglet S, Thomas A, Desmeules J, Daali Y. Impact of CYP2D6 Functional Allelic Variations on Phenoconversion and Drug-Drug Interactions. Clin Pharmacol Ther. 2018;104(1):148-157. doi:10.1002/cpt.889

**Tennezé 1999** Tennezé L, Verstuyft C, Becquemont L, Poirier JM, Wilkinson GR, Funck-Brentano C. Assessment of CYP2D6 and CYP2C19 activity in vivo in humans: a cocktail study with dextromethorphan and chloroguanide alone and in combination. Clin Pharmacol Ther. 1999;66(6):582-588. doi:10.1053/cp.1999.v66.103401001

**Thelen 2011** Thelen K, Coboeken K, Willmann S, Burghaus R, Dressman JB, Lippert J. Evolution of a detailed physiological model to simulate the gastrointestinal transit and absorption process in humans, part 1: oral solutions. J Pharm Sci. 2011;100(12):5324-5345. doi:10.1002/jps.22726

**Watanabe 2018** Watanabe R, Esaki T, Kawashima H, et al. Predicting Fraction Unbound in Human Plasma from Chemical Structure: Improved Accuracy in the Low Value Ranges. Mol Pharm. 2018;15(11):5302-5311. doi:10.1021/acs.molpharmaceut.8b00785

**Wishart 2018** Wishart DS, Feunang YD, Marcu A, et al. HMDB 4.0: the human metabolome database for 2018. Nucleic Acids Res. 2018;46(D1):D608-D617. doi:10.1093/nar/gkx1089

**Yamazaki 2017** Yamazaki T, Desai A, Goldwater R, et al. Pharmacokinetic Effects of Isavuconazole Coadministration With the Cytochrome P450 Enzyme Substrates Bupropion, Repaglinide, Caffeine, Dextromethorphan, and  Methadone in Healthy Subjects. Clin Pharmacol Drug Dev. 2017;6(1):54-65. doi:10.1002/cpdd.281

**Zawertailo 2010** Zawertailo LA, Tyndale RF, Busto U, Sellers EM. Effect of metabolic blockade on the psychoactive effects of dextromethorphan. Hum Psychopharmacol. 2010;25(1):71-79. doi:10.1002/hup.1086
 

