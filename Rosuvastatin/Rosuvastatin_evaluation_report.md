# Building and evaluation of a PBPK model for Rosuvastatin in healthy adults

| Version                                         | master-OSP12.2                                                   |
| ----------------------------------------------- | ------------------------------------------------------------ |
| based on *Model Snapshot* and *Evaluation Plan* | https://github.com/Open-Systems-Pharmacology/Rosuvastatin-Model/releases/tag/vmaster |
| OSP Version                                     | 12.2                                                          |
| Qualification Framework Version                 | 3.5                                                          |

This evaluation report and the corresponding PK-Sim project file are filed at:

https://github.com/Open-Systems-Pharmacology/OSP-PBPK-Model-Library/

# Table of Contents

 * [1 Introduction](#1)
 * [2 Methods](#2)
   * [2.1 Modeling strategy](#21)
   * [2.2 Data used](#22)
   * [2.3 Model parameters and assumptions](#23)
 * [3 Results and Discussion](#3)
   * [3.1 Rosuvastatin final input parameters](#31)
   * [3.2 Rosuvastatin Diagnostics Plots](#32)
   * [3.3 Concentration-Time Profiles](#33)
     * [3.3.1 Model Building](#331)
     * [3.3.2 Model Verification](#332)
 * [4 Conclusion](#4)
 * [5 References](#5)

# 1 Introduction<a id="1"></a>

Rosuvastatin is an inhibitor of the liver 3-hydroxy-3-methyl-glutaryl-coenzyme A reductase (HMG-CoA reductase) and is prescribed to treat hyperlipidemia. Rosuvastatin is metabolized by CYP2C9 to a small extent, transported by OATP1B1 and OATP1B3 into the liver, and subsequently excreted into the bile by BCRP. The renal excretion of rosuvastatin is high, suggesting active tubular secretion (by the transporters OATP2B1, OAT3, and Pgp). The absolute bioavailability of rosuvastatin is about 20%, mainly due to poor absorption. The fraction excreted in feces and urine after an oral dose is about 90% and 10%, respectively, both mainly as parent compound. Rosuvastatin is a substrate of OATP1B1, OATPB1B3, and BCRP and can, therefore, serve as a victim for these transporters in DDI studies. 

The herein presented PBPK model of rosuvastatin has been developed using published pharmacokinetic clinical data by Billington et al. 2019 ([Billington 2019](#5-references)), Martin et al. 2003 ([Martin 2003c](#5-references)), Stopfer et al. 2016 ([Stopfer 2016](#5-references)), Stopfer et al. 2018 ([Stopfer 2018b](#5-references)), Wu et al. 2017 ([Wu 2017](#5-references)), Gidal et al. 2017 ([Gidal  2017])(#5-references)), Cooper et al. 2003 ([Cooper 2003a](#5-references)),  Lee et al. 2018 ([Lee 2018](#5-references)) and Gosai et al. 2008 ([Gosai 2008](#5-references)). 
The model has then been evaluated by comparing observed data to simulations of both intravenously and orally administered rosuvastatin covering a dose range of 0.002 mg to 80.0 mg. Both single dose and multiple dose administration are represented in the development and evaluation data sets. 

The presented model includes the following features:

- metabolism by CYP2C9,
- transport by OATP1B1 and OATP1B3 (lumped together for identifiability),
- transport by BCRP,
- transport by OATP2B1,
- transport by Pgp,
- transport by OAT3,
- renal clearance by glomerular filtration,
- oral absorption with dissolution rate described by a Weibull function.

# 2 Methods<a id="2"></a>

## 2.1 Modeling strategy<a id="21"></a>

The general concept of building a PBPK model has previously been described by Kuepfer et al. ([Kuepfer 2016](#5-references)). Relevant information on anthropometric (height, weight) and physiological parameters (e.g. blood flows, organ volumes, binding protein concentrations, hematocrit, cardiac output) in adults was gathered from the literature and has been previously published ([Willmann 2007](#5-references)). The information was incorporated into PK-Sim® and was used as default values for the simulations in adults.

The applied activity and variability of plasma proteins and active processes that are integrated into PK-Sim® are described in the publicly available PK-Sim® Ontogeny Database Version 7.3 ([PK-Sim Ontogeny Database Version 7.3](#5-references)) or otherwise referenced for the specific process.

A mean model was built based on clinical data from studies with intravenous (Billington et al. 2019 ([Billington 2019](#5-references)), Martin et al. 2003 ([Martin 2003c](#5-references))) and oral administration (Martin et al. 2003 ([Martin 2003c](#5-references)), Stopfer et al. 2016 ([Stopfer 2016](#5-references)), Stopfer et al. 2018 ([Stopfer 2018b](#5-references)), Wu et al. 2017 ([Wu 2017](#5-references)), Gidal et al. 2017 ([Gidal  2017](#5-references)), Cooper et al. 2003 ([Cooper 2003a](#5-references)),  Lee et al. 2018 ([Lee 2018](#5-references)) and Gosai et al. 2008 ([Gosai 2008](#5-references))) of rosuvastatin. The studies reported individual ([Billington 2019](#5-references)) or mean plasma concentrations of rosuvastatin. The study by Billington et al. 2019 also reported individual (n=1) bile fraction of unchanged rosuvastatin as well as tissue concentration in liver and kidney, and the studies by Martin et al. 2003, Stopfer et al. 2016 and Stopfer et al. 2018 reported fraction excreted in urine of unchanged rosuvastatin in addtion to plasma concentrations. The mean PBPK model was developed using a mean individual based on the demographic data for each study. If no demographic data were provided, the following values were used: male, European, 30 years of age, 73 kg body weight, and 176 cm body height. The relative tissue-specific expressions of the enzyme and transporter predominantly being involved in the metabolism/transport of rosuvastatin (CYP2C9, OATP1B1/, BCRP, Pgp and OAT3) were considered ([Meyer 2012](#5-references)). A Weibull function was used to describe the oral dissolution of rosuvastatin and the dose was applied as a split dose approach with lag time for the second dose to capture the late Tmax.  

A specific set of parameters (see below) was optimized to describe the disposition of rosuvastatin using the Parameter Identification module provided in PK-Sim®. Structural model selection was mainly guided by visual inspection, mean relative deviation and geometric mean fold error of all predicted
AUClast and Cmax values.

The model was then verified by simulating further clinical studies reporting pharmacokinetic concentration-time profiles after intravenous and oral administration of rosuvastatin.

Details about input data (physicochemical, *in vitro* and clinical) can be found in [Section 2.2](#22-data-used).

Details about the structural model and its parameters can be found in [Section 2.3](#23-model-parameters-and-assumptions).

## 2.2 Data used<a id="22"></a>

### 2.2.1 In vitro and physicochemical data

A literature search was performed to collect available information on physicochemical properties of rosuvastatin. The obtained information from literature is summarized in the table below, and is used for model building. Final model parameters are stated in [Section 3.1](#31-rosuvastatin-final-input-parameters).

| **Parameter**           | **Unit** | **Value** | Source                               | **Description**                                              |
| :---------------------- | -------- | --------- | ------------------------------------ | ------------------------------------------------------------ |
| MW                      | g/mol    | 481.54    | [Wishart 2006](#5-references)        | Molecular weight                                             |
| pK<sub>a</sub> (acid)   |          | 4.3       | [Riccardi 2019](#5-references)       | acid dissociation constant of conjugate acid                 |
| Solubility (water)      | g/L      | 7.8       | [TGA 2011](#5-references)            | Aqueous solubility                  |
| logP                    |          | -0.33     | [McTaggart 2001](#5-references) and [Jones 2012](#5-references)         | Partition coefficient between octanol and water              |
| fu                      | %        | 11.5      | [FDA 2003](#5-references)           | Fraction unbound in plasma                                   |
| K<sub>m</sub> BCRP      | µmol/L   | 2.02      | [Kitamura 2008](#5-references)            | BCRP Michaelis-Menten constant                             |
| K<sub>m</sub> OAT3      | µmol/L   | 7.40      | [Windass 2007](#5-references)                | OAT3 Michaelis-Menten constant                                 |
| K<sub>m</sub> OATP2B1   | µmol/L   | 6.42      | [Kitamura 2008](#5-references)        | OATP2B1 Michaelis-Menten constant                                   |
| K<sub>m</sub> OATP1B1/3 | µmol/L   | 0.80      | [Kitamura 2008](#5-references)          | OATP1B1/3 Michaelis-Menten connstant                                 |
| K<sub>m</sub> Pgp       | µmol/L   | 203       | [Goard 2010](#5-references)            | Pgp Michaelis-Menten constant                             |
| Weibull shape           | -        | 1.77      | [Seo 2019](#5-references)            | Dissolution profile shape                             |
| Weibull time            | min      | 6.25      | [Seo 2019](#5-references)            | Dissolution time (50% dissolved)                            |

### 2.2.2 Clinical data

A literature search was performed to collect available clinical data on rosuvastatin in adults. 

The following publications were found in adults for model building:

| Publication                   | Arm / Treatment / Information used for model building        |
| :---------------------------- | :----------------------------------------------------------- |
| [Billington 2019](#5-references)  | Individual arterial plasma and whole blood, and tissue liver and kidney PK profiles and fraction excreted to bile in healthy subjects after single intravenous administration of 0.00174 mg rosuvastatin 160 minutes after an oral dose of 5 mg rosuvastatin |
| [Martin 2003c](#5-references) | Plasma PK profiles and urine data in healthy subjects after single intravenous infusion (4 h) of 8 mg rosuvastatin and single oral administration of 40 mg rosuvastatin tablet|
| [Stopfer 2016](#5-references)    | Plasma PK profiles and urine data in healthy subjects after single oral administration of 10 mg rosuvastatin tablet |
| [Stopfer 2018b](#5-references)    | Plasma PK profiles and urine data in healthy subjects after single oral administration of 10 mg rosuvastatin tablet |
| [Wu 2017](#5-references)   | Plasma PK profiles in healthy subjects after single oral administration of 20 mg rosuvastatin tablet |
| [Gidal 2017](#5-references)   | Plasma PK profiles in healthy subjects after single oral administration of 40 mg rosuvastatin tablet |
| [Cooper 2003a](#5-references)    | Plasma PK profiles in healthy subjects after single oral administration of 80 mg rosuvastatin tablet |
| [Lee 2018](#5-references)    | Plasma PK profiles in healthy subjects after multiple oral administration of 20 mg rosuvastatin QD |
| [Gosai 2008](#5-references)    | Plasma PK profiles in healthy subjects after multiple oral administration of 40 mg rosuvastatin QD |

The following table shows the data from the excretion studies used for model building:

| Observer                                                     | Value |
| ------------------------------------------------------------ | ----- |
| Fraction excreted to bile of unchanged rosuvastatin at 0.46 h after iv administration of 0.00174 mg | 4.76%   |
| Fraction excreted to urine of unchanged rosuvastatin after iv infusion of 8 mg | 29.5%   |
| Fraction excreted to urine of unchanged rosuvastatin after oral administration 10 mg | 6.27% and 5.1% |
| Fraction excreted to urine of unchanged rosuvastatin after oral administration 40 mg | 5.09%  |

The following dosing scenarios were simulated and compared to respective data for model verification:

| Scenario                                                     | Data reference                       |
| ------------------------------------------------------------ | ------------------------------------ |
| po 0.05                         | [Prueksaritanont 2017](#5-references) |
| po 5                                  | [Prueksaritanont 2014](#5-references) |
| po 10 mg                                   | [Cooper 2003b](#5-references), [Csonka 2019](#5-references), [Huguet 2016](#5-references), [Martin 2003b](#5-references), [Stopfer 2018a](#5-references), [Wiebe 2020](#5-references) and [Coss 2016](#5-references) |
| po 20 mg                              | [Martin 2003d](#5-references), [FDA 2003](#5-references), [Edwards 2017](#5-references), [Martin 2003b](#5-references), [Birmingham 2015](#5-references), [Jones 2020](#5-references), [Lee 2018](#5-references), [Martin 2016](#5-references) and [Willis 2020](#5-references) |
| po 40 mg                              | [Martin 2002a ](#5-references), [Martin 2003b](#5-references) and [Lee 2005](#5-references)|
| po 80 mg                                   | [Cooper 2002](#5-references), [Cooper 2003b](#5-references), [Cooper 2003c](#5-references), [Martin 2003b](#5-references), [Schneck 2004](#5-references) |
| po 10 mg QD for 14 days                    | [Kosoglou 2004](#5-references) and [Martin 2002b](#5-references)|
| po 20 mg QD for 7 days                    | [FDA 2003](#5-references) and [FDA 2018](#5-references)|

## 2.3 Model parameters and assumptions<a id="23"></a>

### 2.3.1 Absorption

The parameter value for  `Specific intestinal permeability`  was optimized based on clinical oral data, see results of optimization in [Section 2.3.4](#234-automated-parameter-identification). The measured solubility in water was used in the model (see [Section 2.2.1](#221-in-vitro-and-physicochemical-data))

The dissolution of tablets was implemented via empirical Weibull dissolution extracted from literature. The dose was applied as a split dose approach with lag time for the second dose to capture the late Tmax as informed by popPK modeling. 

### 2.3.2 Distribution

Rosuvastatin is highly bound to plasma proteins (approx. 88.5 %) (see [Section 2.2.1](#221-in-vitro-and-physicochemical-data)). A value of 11.5% was used in this PBPK model for `Fraction unbound (plasma, reference value)`. The major binding partner was set to albumin (see [Section 2.2.1](#221-in-vitro-and-physicochemical-data)).

An important parameter influencing the resulting volume of distribution is lipophilicity. The reported experimental logP value of -0.33 was used in this model (see [Section 2.2.1](#221-in-vitro-and-physicochemical-data)). 

After testing the available organ-plasma partition coefficient and cell permeability calculation methods built in PK-Sim, observed clinical data was best described by choosing the partition coefficient calculation by `PK-Sim Standard` and cellular permeability calculation by `PK-Sim Standard`.

### 2.3.3 Metabolism and Elimination

One metabolic pathway was implement into the model via Michaelis-Menten kinetics 

* CYP2C9

The CYP2C9 expression profiles is based on high-sensitive real-time RT-PCR ([Nishimura 2003](#5-references)). Metabolic enzyme activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was taken from literature and the `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

The following transport proteins were implemented into the model with the Michaelis-Menten kinetics 

* OAT3

The OAT3 expression profiles is based on high-sensitive real-time RT-PCR ([Nishimura 2003](#5-references)). Transporter activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was set to 1 µmol/l and `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

* BCRP

The BCRP expression profiles is based on whole genome expression arrays from ArrayExpress ([Kolesnikov 2015](#5-references)). Transporter activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was taken from literature and `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

* Pgp

The Pgp expression profiles is based on high-sensitive real-time RT-PCR ([Nishimura 2003](#5-references)) with an intestinal mucosa of factor 3.57 ([Hanke 2018](#5-references)). Transporter activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was taken from literature and `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

* OATP1B1/3

For OATP1B1/3 the expression profiles was considered only for OATP1B1 and is based on high-sensitive real-time RT-PCR ([Nishimura 2003](#5-references)). The reference concentration for OATP1B1 was measured by liquid chromatography tandem mass spectroscopy ([Prasad 2014](#5-references)). Transporter activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was taken from literature and `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

* OATP2B1

The OATP2B1 expression profiles is based on whole genome expression arrays from ArrayExpress ([Kolesnikov 2015](#5-references)). Transporter activity was described as saturable process following Michaelis-Menten kinetics, were the `Km` was taken from literature and `kcat` was optimized based on clinical data (see [Section 2.3.4](#234-automated-parameter-identification)).

Additionally, passive renal clearance by glomerular filtration was implemented and the `GFR fraction` was set to 1. In addition, fraction of bile that was continously released was set to 1 (`EHC continuous fraction`)

### 2.3.4 Automated Parameter Identification

The following parameters were optimized by fitting the model to the data:

| Model Parameter                |
| ------------------------------ | 
| `kcat` (BCRP)                 |
| `kcat` (CYP2C9)             | 
| `kcat` (OATP1B1/3)            |
| `kcat` (Pgp)                    | 
| `kcat` (OAT3)                  | 
| `kcat` (OATP2B1)                  |
| `Specific intestinal permeability`| 
| `P(intracell->interstitial) small intestine`|
| `P(intracell->interstitial) small intestine`|

 

# 3 Results and Discussion<a id="3"></a>

The PBPK model for rosuvastatin was developed and verified with clinical pharmacokinetic data.

The model was built and evaluated covering data from studies including in particular

* single intravenous administration and both single and multiple oral administrations (tablets, capsules and solution).
* a dose range of 0.002 to 80.0 mg

The model quantifies excretion via urine and bile (by glomerular filtration and active transport) and metabolism via CYP2C9. The active transport is attributed to Pgp, BCRP, OATP1B1/3, OAT3 and OATP2B1.

The next sections show:

1. the final model input parameters for the building blocks: [Section 3.1](#31-probenecid-final-input-parameters).
2. the overall goodness of fit: [Section 3.2](#32-probenecid-diagnostics-plots).
3. simulated vs. observed concentration-time profiles for the clinical studies used for model building and for model verification: [Section 3.3](#33-concentration-time-profiles).

## 3.1 Rosuvastatin final input parameters<a id="31"></a>

The compound parameter values of the final PBPK model are illustrated below.

### Compound: Rosuvastatin

#### Parameters

Name                                             | Value                   | Value Origin                                                                                                                  | Alternative | Default
------------------------------------------------ | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ----------- | -------
Solubility at reference pH                       | 7800 mg/l               | Publication-In Vitro-TGA 2011                                                                                                 | Measurement | True   
Reference pH                                     | 7                       |                                                                                                                               | Measurement | True   
Lipophilicity                                    | -0.33 Log Units         | Publication-In Vitro-McTaggart 2001 and Jones 2012                                                                            | Measurement | True   
Fraction unbound (plasma, reference value)       | 0.115                   | Publication-In Vitro-FDA 2003                                                                                                 | Measurement | True   
Specific intestinal permeability (transcellular) | 1.8086332716E-06 cm/min | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:15 | Fit         | True   
F                                                | 1                       |                                                                                                                               |             |        
Is small molecule                                | Yes                     |                                                                                                                               |             |        
Molecular weight                                 | 481.538 g/mol           |                                                                                                                               |             |        
Plasma protein binding partner                   | Albumin                 |                                                                                                                               |             |        

#### Calculation methods

Name                    | Value          
----------------------- | ---------------
Partition coefficients  | PK-Sim Standard
Cellular permeabilities | PK-Sim Standard

#### Processes

##### Transport Protein: BCRP-Paper

Molecule: BCRP

###### Parameters

Name                      | Value                       | Value Origin                                                                                                                 
------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/transporter | 0 nmol/min/pmol transporter |                                                                                                                              
Km                        | 2.02 µmol/l                 | Publication-In Vitro-Kitamura 2008                                                                                           
kcat                      | 1.0072328564 1/min          | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:14

##### Systemic Process: Glomerular Filtration-GFR

Species: Human

###### Parameters

Name         | Value | Value Origin                                                                                                             
------------ | -----:| -------------------------------------------------------------------------------------------------------------------------
GFR fraction |     1 | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV' on 2020-10-27 16:22

##### Transport Protein: OAT3-Paper

Molecule: OAT3

###### Parameters

Name                      | Value                       | Value Origin                                                                                                                 
------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/transporter | 0 nmol/min/pmol transporter |                                                                                                                              
Km                        | 7.4 µmol/l                  | Publication-In Vitro-Windass 2007                                                                                            
kcat                      | 30999.6809549078 1/min      | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:15

##### Transport Protein: OATP1B1-Paper

Molecule: OATP1B1

###### Parameters

Name                      | Value                       | Value Origin                                                                                                                 
------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/transporter | 0 nmol/min/pmol transporter |                                                                                                                              
Km                        | 0.802 µmol/l                | Publication-In Vitro-Kitamura 2008                                                                                           
kcat                      | 805.2413910931 1/min        | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:15

##### Transport Protein: OATP2B1-Paper

Molecule: OATP2B1

###### Parameters

Name                      | Value                       | Value Origin                                                                                                                 
------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/transporter | 0 nmol/min/pmol transporter |                                                                                                                              
Km                        | 6.42 µmol/l                 | Publication-In Vitro-Kitamura 2008                                                                                           
kcat                      | 4.3108123152 1/min          | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:15

##### Transport Protein: P-gp-Paper

Molecule: P-gp

###### Parameters

Name                      | Value                       | Value Origin                                                                                                                 
------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/transporter | 0 nmol/min/pmol transporter |                                                                                                                              
Km                        | 203 µmol/l                  | Publication-In Vitro-Goard 2010                                                                                              
kcat                      | 46.0938220893 1/min         | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:14

##### Metabolizing Enzyme: CYP2C9-Hypothetical Km

Molecule: CYP2C9

###### Parameters

Name                             | Value                       | Value Origin                                                                                                                 
-------------------------------- | --------------------------- | -----------------------------------------------------------------------------------------------------------------------------
In vitro Vmax/recombinant enzyme | 0 nmol/min/pmol rec. enzyme |                                                                                                                              
Km                               | 1 µmol/l                    | Other-Assumption-Assumed                                                                                                     
kcat                             | 0.00097440861845 1/min      | Parameter Identification-Parameter Identification-Value updated from '29 - P large int, CYP2C9 - CoV - 5' on 2020-10-30 16:15

## 3.2 Rosuvastatin Diagnostics Plots<a id="32"></a>

Below you find the goodness-of-fit visual diagnostic plots for the PBPK model performance of all data used presented in [Section 2.2.2](#222-clinical-data).

The first plot shows observed versus simulated plasma concentration, the second weighted residuals versus time. 

<a id="table-3-1"></a>

**Table 3-1: GMFE for Goodness of fit plot for concentration in plasma.**

|Group                                                 |GMFE |
|:-----------------------------------------------------|:----|
|Rosuvastatin iv (model building)                      |1.49 |
|Rosuvastatin oral administration (model building)     |1.26 |
|Rosuvastatin oral administration (model verification) |1.34 |
|All                                                   |1.33 |

<br>
<br>

<a id="figure-3-1"></a>

![](images/006_section_3/008_section_32/2_gof_plot_predictedVsObserved.png)

**Figure 3-1: Goodness of fit plot for concentration in plasma.**

<br>
<br>

<a id="figure-3-2"></a>

![](images/006_section_3/008_section_32/3_gof_plot_residualsOverTime.png)

**Figure 3-2: Goodness of fit plot for concentration in plasma.**

<br>
<br>

## 3.3 Concentration-Time Profiles<a id="33"></a>

Simulated versus observed concentration-time profiles of all data listed in [Section 2.2.2](#222-clinical-data) are presented below.

### 3.3.1 Model Building<a id="331"></a>

<a id="figure-3-3"></a>

![](images/006_section_3/009_section_33/010_section_331/1_time_profile_plot_Rosuvastatin_iv__0_00174_mg__Billington_2019__bolus__n_1.png)

**Figure 3-3: Billington 2019 - IV bolus 0.00174 mg control**

<br>
<br>

<a id="figure-3-4"></a>

![](images/006_section_3/009_section_33/010_section_331/2_time_profile_plot_Rosuvastatin_iv__8_0_mg__Martin_2003c__4_h__n_10.png)

**Figure 3-4: Martin 2003c - IV 8 mg control**

<br>
<br>

<a id="figure-3-5"></a>

![](images/006_section_3/009_section_33/010_section_331/8_time_profile_plot_Rosuvastatin_po_10_mg__Stopfer_2016__tab__n_19.png)

**Figure 3-5: Stopfer 2016 - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-6"></a>

![](images/006_section_3/009_section_33/010_section_331/9_time_profile_plot_Rosuvastatin_po_10_mg__Stopfer_2018a__tab__n_13.png)

**Figure 3-6: po 10 mg, Stopfer 2018a, tab**

<br>
<br>

<a id="figure-3-7"></a>

![](images/006_section_3/009_section_33/010_section_331/13_time_profile_plot_Rosuvastatin_po_20_mg__FDA_2002__caps__n_6.png)

**Figure 3-7: FDA 2002 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-8"></a>

![](images/006_section_3/009_section_33/010_section_331/20_time_profile_plot_Rosuvastatin_po_40_mg__Gidal_2017__tab__n_33.png)

**Figure 3-8: Gidal 2017 - PO Rosuvastatin 40 mg control**

<br>
<br>

<a id="figure-3-9"></a>

![](images/006_section_3/009_section_33/010_section_331/25_time_profile_plot_Rosuvastatin_po_40_mg__Martin_2003c__tab__n_10.png)

**Figure 3-9: Martin 2003c - PO Rosuvastatin 40 mg control**

<br>
<br>

<a id="figure-3-10"></a>

![](images/006_section_3/009_section_33/010_section_331/27_time_profile_plot_Rosuvastatin_po_80_mg__Cooper_2003a__tab__n_13.png)

**Figure 3-10: Cooper 2003a - PO Rosuvastatin 80 mg control**

<br>
<br>

<a id="figure-3-11"></a>

![](images/006_section_3/009_section_33/010_section_331/35_time_profile_plot_Rosuvastatin_po_20_mg_QD__Lee_2018_____n_32.png)

**Figure 3-11: Lee 2018 - PO Rosuvastatin 20 mg QD control**

<br>
<br>

<a id="figure-3-12"></a>

![](images/006_section_3/009_section_33/010_section_331/36_time_profile_plot_Rosuvastatin_po_40_mg_QD__Gosai_2008_____n_42.png)

**Figure 3-12: Gosai 2008 - PO Rosuvastatin 40 mg QD control**

<br>
<br>

### 3.3.2 Model Verification<a id="332"></a>

<a id="figure-3-13"></a>

![](images/006_section_3/009_section_33/011_section_332/3_time_profile_plot_Rosuvastatin_po_10_mg__Cooper_2003b__caps__n_12.png)

**Figure 3-13: po 10 mg, Cooper 2003b, caps, n=12**

<br>
<br>

<a id="figure-3-14"></a>

![](images/006_section_3/009_section_33/011_section_332/4_time_profile_plot_Rosuvastatin_po_10_mg__Coss_2016_____n_49.png)

**Figure 3-14: Coss 2016 - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-15"></a>

![](images/006_section_3/009_section_33/011_section_332/5_time_profile_plot_Rosuvastatin_po_10_mg__Csonka_2019__tab__n_20.png)

**Figure 3-15: Csonka 2019 - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-16"></a>

![](images/006_section_3/009_section_33/011_section_332/6_time_profile_plot_Rosuvastatin_po_10_mg__Huguet_2016__tab__n_16.png)

**Figure 3-16: Huguet 2016 - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-17"></a>

![](images/006_section_3/009_section_33/011_section_332/7_time_profile_plot_Rosuvastatin_po_10_mg__Martin_2003b__tab__n_18.png)

**Figure 3-17: Martin 2003b - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-18"></a>

![](images/006_section_3/009_section_33/011_section_332/10_time_profile_plot_Rosuvastatin_po_10_mg__Stopfer_2018b__tab__n_25.png)

**Figure 3-18: Stopfer 2018b - PO Rosuvastatin 10 mg control**

<br>
<br>

<a id="figure-3-19"></a>

![](images/006_section_3/009_section_33/011_section_332/11_time_profile_plot_Rosuvastatin_po_20_mg__Birmingham_2015_____n_29.png)

**Figure 3-19: Birmingham 2015 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-20"></a>

![](images/006_section_3/009_section_33/011_section_332/12_time_profile_plot_Rosuvastatin_po_20_mg__Edwards_2017__tab__n_48.png)

**Figure 3-20: Edwards 2017 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-21"></a>

![](images/006_section_3/009_section_33/011_section_332/14_time_profile_plot_Rosuvastatin_po_20_mg__Jones_2020_____n_16.png)

**Figure 3-21: Jones 2020 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-22"></a>

![](images/006_section_3/009_section_33/011_section_332/15_time_profile_plot_Rosuvastatin_po_20_mg__Lee_2018_____n_35.png)

**Figure 3-22: Lee 2018 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-23"></a>

![](images/006_section_3/009_section_33/011_section_332/16_time_profile_plot_Rosuvastatin_po_20_mg__Martin_2003b__tab__n_9.png)

**Figure 3-23: Martin 2003b - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-24"></a>

![](images/006_section_3/009_section_33/011_section_332/17_time_profile_plot_Rosuvastatin_po_20_mg__Martin_2003d__sol__n_6.png)

**Figure 3-24: Martin 2003d - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-25"></a>

![](images/006_section_3/009_section_33/011_section_332/18_time_profile_plot_Rosuvastatin_po_20_mg__Martin_2016_____n_21.png)

**Figure 3-25: Martin 2016 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-26"></a>

![](images/006_section_3/009_section_33/011_section_332/19_time_profile_plot_Rosuvastatin_po_20_mg__Willis_2020_____n_31.png)

**Figure 3-26: Willis 2020 - PO Rosuvastatin 20 mg control**

<br>
<br>

<a id="figure-3-27"></a>

![](images/006_section_3/009_section_33/011_section_332/21_time_profile_plot_Rosuvastatin_po_40_mg__Lee_2005_____n_36.png)

**Figure 3-27: Lee 2005 - PO Rosuvastatin 40 mg control**

<br>
<br>

<a id="figure-3-28"></a>

![](images/006_section_3/009_section_33/011_section_332/22_time_profile_plot_Rosuvastatin_po_40_mg__Martin_2002a__caps__female__n_8.png)

**Figure 3-28: Martin 2002a - PO Rosuvastatin 40 mg control female**

<br>
<br>

<a id="figure-3-29"></a>

![](images/006_section_3/009_section_33/011_section_332/23_time_profile_plot_Rosuvastatin_po_40_mg__Martin_2002a__caps__male__n_8.png)

**Figure 3-29: Martin 2002a - PO Rosuvastatin 40 mg control male**

<br>
<br>

<a id="figure-3-30"></a>

![](images/006_section_3/009_section_33/011_section_332/24_time_profile_plot_Rosuvastatin_po_40_mg__Martin_2003b__tab__n_9.png)

**Figure 3-30: po 40 mg, Martin 2003b, tab**

<br>
<br>

<a id="figure-3-31"></a>

![](images/006_section_3/009_section_33/011_section_332/26_time_profile_plot_Rosuvastatin_po_80_mg__Cooper_2002__tab__n_14.png)

**Figure 3-31: Cooper 2002 - PO Rosuvastatin 80 mg control**

<br>
<br>

<a id="figure-3-32"></a>

![](images/006_section_3/009_section_33/011_section_332/28_time_profile_plot_Rosuvastatin_po_80_mg__Cooper_2003b__tab__n_14.png)

**Figure 3-32: Cooper 2003b - PO Rosuvastatin 80 mg control**

<br>
<br>

<a id="figure-3-33"></a>

![](images/006_section_3/009_section_33/011_section_332/29_time_profile_plot_Rosuvastatin_po_80_mg__Cooper_2003c__tab__n_14.png)

**Figure 3-33: Cooper 2003c - PO Rosuvastatin 80 mg control**

<br>
<br>

<a id="figure-3-34"></a>

![](images/006_section_3/009_section_33/011_section_332/30_time_profile_plot_Rosuvastatin_po_80_mg__Martin_2003b__tab__n_18.png)

**Figure 3-34: Martin 2003b - PO Rosuvastatin 80 mg control**

<br>
<br>

<a id="figure-3-35"></a>

![](images/006_section_3/009_section_33/011_section_332/31_time_profile_plot_Rosuvastatin_po_10_mg_QD__Kosoglou_2004_____n_12.png)

**Figure 3-35: Kosoglou 2004 - PO Rosuvastatin 10 mg QD control**

<br>
<br>

<a id="figure-3-36"></a>

![](images/006_section_3/009_section_33/011_section_332/32_time_profile_plot_Rosuvastatin_po_10_mg_QD__Martin_2002b_____n_21.png)

**Figure 3-36: Martin 2002b - PO Rosuvastatin 10 mg QD control**

<br>
<br>

<a id="figure-3-37"></a>

![](images/006_section_3/009_section_33/011_section_332/33_time_profile_plot_Rosuvastatin_po_20_mg_QD__FDA_2002__caps__n_6.png)

**Figure 3-37: FDA 2002 - PO Rosuvastatin 20 mg QD control**

<br>
<br>

<a id="figure-3-38"></a>

![](images/006_section_3/009_section_33/011_section_332/34_time_profile_plot_Rosuvastatin_po_20_mg_QD__FDA_2018_____n_10.png)

**Figure 3-38: FDA 2018 - PO Rosuvastatin 20 mg control**

<br>
<br>

# 4 Conclusion<a id="4"></a>

The presented PBPK model adequately describes the intravenous and oral pharmacokinetics of rosuvastatin in adults.

# 5 References<a id="5"></a>

**Billington 2019** Billington S, Shoner S, Lee S, Clark-Snustad K, Pennington M, Lewis D, Muzi M, Rene S,Lee J, Nguyen TB, Kumar V, Ishida K, Chen L, Chu X, Lai Y, Salphati L, Hop CECA, Xiao G, Liao M, Unadkat JD (2019) Positron Emission Tomography Imaging of [11 C]Rosuvastatin Hepatic Concentrations and Hepatobiliary Transport in Humans in the Absence and Presence of Cyclosporin A. *Clinical pharmacology and therapeutics* *106*(5):1056–1066 

**Birmingham 2015** Birmingham BK, Bujac SR, Elsby R, Azumaya CT, Wei C, Chen Y, Mosqueda-Garcia R, Ambrose HJ (2015) Impact of ABCG2 and SLCO1B1 polymorphisms on pharmacokinetics of rosuvastatin, atorvastatin and simvastatin acid in Caucasian and Asian subjects: a class effect? *European journal of clinical pharmacology* *71*(3):341–55

**Cooper 2002** Cooper KJ, Martin PD, Dane AL, Warwick MJ, Schneck DW, Cantarini MV (2002) The effect of fluconazole on the pharmacokinetics of rosuvastatin. *European journal of clinical pharmacology* *58*(8):527–31

**Cooper 2003a** Cooper KJ, Martin PD, Dane AL, Warwick MJ, Raza A, Schneck DW (2003) Lack of effect of ketoconazole on the pharmacokinetics of rosuvastatin in healthy subjects. *British journal of clinical pharmacology* *55*(1):94–9

**Cooper 2003b** Cooper KJ, Martin PD, Dane AL, Warwick MJ, Schneck DW, Cantarini MV (2003) Effect of itraconazole on the pharmacokinetics of rosuvastatin. *Clinical pharmacology and therapeutics* *73*(4):322–9

**Cooper 2003c** Cooper KJ, Martin PD, Dane AL, Warwick MJ, Raza A, Schneck DW (2003) The effect of erythromycin on the pharmacokinetics of rosuvastatin. *European journal of clinical pharmacology* *59*(1):51–6

**Coss 2016** Coss CC, Jones A, Dalton JT (2016) Pharmacokinetic drug interactions of the selective androgen receptor modulator GTx-024(Enobosarm) with itraconazole, rifampin, probenecid, celecoxib and rosuvastatin. *Investigational new drugs* *34*(4):458–67

**Csonka 2019** Csonka D, Bruderer S, Schultz A, Soergel M, Stepanova R, Sabattini G, Perez-Ruixo JJ (2019) Effect of Macitentan on the Pharmacokinetics of the Breast Cancer Resistance Protein Substrates, Rosuvastatin and Riociguat, in Healthy Male Subjects. *Clinical drug investigation* *39*(12):1223–1232

**Edwards 2017** Edwards JE, Eliot L, Parkinson A, Karan S, MacConell L (2017) Assessment of Pharmacokinetic Interactions Between Obeticholic Acid and Caffeine, Midazolam, Warfarin, Dextromethorphan, Omeprazole, Rosuvastatin, and Digoxin in Phase 1 Studies in Healthy Subjects. *Advances in therapy* *34*(9):2120–2138

**FDA 2003** US Food and Drug Administration (FDA) (2003) Approval Package for Application Number 21-366 (Crestor), *Clinical Pharmacology and Biopharmaceutics Review*

**FDA 2018** US Food and Drug Administration (FDA) (2018) Approval Package for NDA 210450 (Orilissa),*Multi-Disciplinary Review and Evaluation*

**Gidal 2017** Gidal BE, Mintzer S, Schwab M, Schutz R, Kharidia J, Blum D, Grinnell T, Sunkaraneni S (2017) Evidence for a pharmacokinetic interaction between eslicarbazepine and rosuvastatin: Potential effects on xenobiotic transporters. *Epilepsy research* 135:64–70

**Goard 2010** Goard CA, Mather RG, Vinepal B, Clendening JW, Martirosyan A, Boutros PC, Sharom FJ, Penn LZ (2010) Differential interactions between statins and P-glycoprotein: implications for exploiting statins as anticancer agents. *International journal of cancer* *127*(12):2936–48

**Gosai 2008** Gosai P, Liu J, Doyle RT, Johnson J, Carter R, Sica D, McKenney JM (2008) Effect of omega-3-acid ethyl esters on the steady-state plasma pharmacokinetics of rosuvastatin in healthy adults. *Expert opinion on pharmacotherapy* *9*(17):2947–53

**Hanke 2018** Hanke N, Frechen S, Moj D, Britz H, Eissing T, Wendl T, Lehr T (2018) PBPK models for CYP3A4 and P-gp DDI prediction: A modeling network of rifampicin, itraconazole, clarithromycin, midazolam, alfentanil, and digoxin. CPT: Pharmacometrics & Systems Pharmacology 7(10):647–659

**Huguet 2016** Huguet J, Lu J, Gaudette F, Chiasson JL, Hamet P, Michaud V, Turgeon J (2016) No effects of pantoprazole on the pharmacokinetics of rosuvastatin in healthy subjects. *European journal of clinical pharmacology* *72*(8):925–31

**Jones 2012** Jones HM, Barton HA, Lai Y, Bi YA, Kimoto E, Kempshall S, Tate SC, El-Kattan A, Houston JB, Galetin A, Fenner KS (2012) Mechanistic pharmacokinetic modeling for the prediction of transporter-mediated disposition in humans from sandwich culture human hepatocyte data. *Drug metabolism and disposition: the biological fate of chemicals* *40*(5):1007–17

**Jones 2020** Jones NS, Yoshida K, Salphati L, Kenny JR, Durk MR, Chinn LW (2020) Complex DDI by Fenebrutinib and the Use of Transporter Endogenous Biomarkers to Elucidate the Mechanism of DDI. *Clinical pharmacology and therapeutics* *107*(1):269–277

**Kitamura 2008** Kitamura S, Maeda K, Wang Y, Sugiyama Y (2008) Involvement of multiple transporters in the hepatobiliary transport of rosuvastatin. *Drug metabolism and disposition: the biological fate of chemicals* *36*(10):2014–23

**Kolesnikov 2015** Kolesnikov N, Hastings E, Keays M, Melnichuk O, Tang YA, Williams E, Dylag M, Kurbatova N, Brandizi M, Burdett T, Megy K, Pilicheva E, Rustici G, Tikhonov A, Parkinson H, Petryszak R, Sarkans U, Brazma A (2015) ArrayExpress update—simplifying data submissions. *Nucleic Acids Research* *43*(D1):D1113–D1116

**Kosoglou 2004** Kosoglou T, Statkevich P, Yang B, Suresh R, Zhu Y, Boutros T, Maxwell SE, Tiessen R, Cutler DL (2004) Pharmacodynamic interaction between ezetimibe and rosuvastatin. *Current medical research and opinion* *20*(8):1185–95

**Kuepfer 2016** Kuepfer L, Niederalt C, Wendl T, Schlender JF, Willmann S, Lippert J, Block M, Eissing T, Teutonico D. (2016). Applied Concepts in PBPK Modeling: How to Build a PBPK/PD Model. *CPT Pharmacometrics Syst Pharmacol*. Oct;5(10), 516-531.

**Lee 2005** Lee E, Ryan S, Birmingham B, Zalikowski J, March R, Ambrose H, Moore R, Lee C, Chen Y, Schneck D (2005) Rosuvastatin pharmacokinetics and pharmacogenetics in white and Asian subjects residing in the same environment. *Clinical pharmacology and therapeutics* *78*(4):330–41

**Lee 2018** Lee J, Rhee SJ, Lee S, Yu KS (2018) Evaluation of drug interactions between fimasartan and rosuvastatin after single and multiple doses in healthy Caucasians. *Drug design, development and therapy* 12:787–794

**Martin 2002a** Martin PD, Dane AL, Nwose OM, Schneck DW, Warwick MJ (2002) No effect of age or gender on the pharmacokinetics of rosuvastatin: a new HMG-CoA reductase inhibitor. *Journal of clinical pharmacology* *42*(10):1116–21

**Martin 2002b** Martin PD, Mitchell PD, Schneck DW (2002) Pharmacodynamic effects and pharmacokinetics of a new HMG-CoA reductase inhibitor, rosuvastatin, after morning or evening administration in healthy volunteers. *British journal of clinical pharmacology* *54*(5):472–7

**Martin 2003b** Martin PD, Warwick MJ, Dane AL, Cantarini MV (2003) A double-blind, randomized, incomplete crossover trial to assess the dose proportionality of rosuvastatin in healthy volunteers.*Clinical therapeutics* *25*(8):2215–24

**Martin 2003c** Martin PD, Warwick MJ, Dane AL, Brindley C, Short T (2003) Absolute oral bioavailability of rosuvastatin in healthy white adult male volunteers. *Clinical therapeutics* *25*(10):2553–63

**Martin 2003d** Martin PD, Warwick MJ, Dane AL, Hill SJ, Giles PB, Phillips PJ, Lenz E (2003) Metabolism, excretion, and pharmacokinetics of rosuvastatin in healthy adult male volunteers. *Clinical therapeutics* *25*(11):2822–35

**Martin 2016** Martin P, Gillen M, Ritter J, Mathews D, Brealey C, Surry D, Oliver S, Holmes V, Severin P, Elsby R (2016) Effects of Fostamatinib on the Pharmacokinetics of Oral Contraceptive, Warfarin, and the Statins Rosuvastatin and Simvastatin: Results From Phase I Clinical Studies. *Drugs in R&D* *16*(1):93–107

**McTaggart 2001** McTaggart F, Buckett L, Davidson R, Holdgate G, McCormick A, Schneck D, Smith G, Warwick M (2001) Preclinical and clinical pharmacology of Rosuvastatin, a new 3-hydroxy-3-methylglutaryl coenzyme A reductase inhibitor. *The American journal of cardiology* *87*(5A):28B–32B

**Meyer 2012** Meyer M, Schneckener S, Ludewig B, Kuepfer L, Lippert J. (2012). Using expression data for quantification of active processes in physiologically based pharmacokinetic modeling. *Drug Metab Dispos*. May;40(5), 892-901.

**Nishimura 2003** Nishimura M, Yaguti H, Yoshitsugu H, Naito S, Satoh T. (2003). Tissue distribution of mRNA expression of human cytochrome P450 isoforms assessed by high-sensitivity real-time reverse transcription PCR. *Yakugaku Zasshi.* May;123(5), 369-75.

**Prasad 2014** Prasad B, Evers R, Gupta A, Hop CECA, Salphati L, Shukla S, Ambudkar SV, Unadkat JD (2014) Interindividual variability in hepatic organic anion-transporting polypeptides and PGlycoprotein (ABCB1) protein expression: Quantification by liquid chromatography tandem mass spectroscopy and influence of genotype, age, and sex. Drug Metabolism and Disposition 42(1):78–88

**Prueksaritanont 2014**  Prueksaritanont T, Chu X, Evers R, Klopfer SO, Caro L, Kothare PA, Dempsey C, Rasmussen S, Houle R, Chan G, Cai X, Valesky R, Fraser IP, Stoch SA (2014) Pitavastatin is a more sensitive and selective organic anion-transporting polypeptide 1B clinical probe than rosuvastatin. *British journal of clinical pharmacology* *78*(3):587–98

**Prueksaritanont 2017** Prueksaritanont T, Tatosian DA, Chu X, Railkar R, Evers R, Chavez-Eng C, Lutz R, Zeng W, Yabut J, Chan GH, Cai X, Latham AH, Hehman J, Stypinski D, Brejda J, Zhou C, Thornton B, Bateman KP, Fraser I, Stoch SA (2017) Validation of a microdose probe drug cocktail for clinical drug interaction assessments for drug transporters and CYP3A. *Clinical pharmacology and therapeutics* *101*(4):519–530

**Riccardi 2019** Riccardi KA, Tess DA, Lin J, Patel R, Ryu S, Atkinson K, Di L, Li R (2019) A Novel Unified Approach to Predict Human Hepatic Clearance for Both Enzyme- and Transporter-Mediated Mechanisms Using Suspended Human Hepatocytes. *Drug metabolism and disposition: the biological fate of chemicals* *47*(5):484–492

**Schneck 2004** Schneck DW, Birmingham BK, Zalikowski JA, Mitchell PD, Wang Y, Martin PD, Lasseter KC, Brown CDA, Windass AS, Raza A (2004) The effect of gemfibrozil on the pharmacokinetics of rosuvastatin. *Clinical pharmacology and therapeutics* *75*(5):455–63

**Seo 2019** Seo KS, Han HK (2019) Multilayer-Coated Tablet of Clopidogrel and Rosuvastatin: Preparation and In Vitro/In vivo Characterization. *Pharmaceutics* *11*(7)

**Stopfer 2016** Stopfer P, Giessmann T, Hohl K, Sharma A, Ishiguro N, Taub ME, Zimdahl-Gelling H, Gansser D, Wein M, Ebner T, Müller F (2016) Pharmacokinetic Evaluation of a Drug Transporter Cocktail Consisting of Digoxin, Furosemide, Metformin, and Rosuvastatin. *Clinical pharmacology and therapeutics* *100*(3):259–67.

**Stopfer 2018a** Stopfer P, Giessmann T, Hohl K, Sharma A, Ishiguro N, Taub ME, Jungnik A, Gansser D, Ebner T, Müller F (2018) Effects of Metformin and Furosemide on Rosuvastatin Pharmacokinetics in Healthy Volunteers: Implications for Their Use as Probe Drugs in a Transporter Cocktail. *European journal of drug metabolism and pharmacokinetics* *43*(1):69–80

**Stopfer 2018b** Stopfer P, Giessmann T, Hohl K, Hutzel S, Schmidt S, Gansser D, Ishiguro N, Taub ME, Sharma A, Ebner T, Müller F (2018) Optimization of a drug transporter probe cocktail: potential screening tool for transporter-mediated drug-drug interactions. *British journal of clinical pharmacology* *84*(9):1941–1949

**TGA 2011** Australian Therapeutic Goods Administration (TGA) (2011) Crestor - Product Information

**Wiebe 2020** Wiebe ST, Giessmann T, Hohl K, Schmidt-Gerets S, Hauel E, Jambrecina A, Bader K, Ishiguro N, Taub ME, Sharma A, Ebner T, Mikus G, Fromm MF, Müller F, Stopfer P (2020) Validation of a Drug Transporter Probe Cocktail Using the Prototypical Inhibitors Rifampin, Probenecid, Verapamil, and Cimetidine. *Clinical pharmacokinetics*

**Willis 2020** Willis BA, Andersen SW, Ayan-Oshodi M, James DE, Liffick E, Hillgren K, Guo Y, Monk SA (2020) Assessment of Transporter Polymorphisms as a Factor in a BCRP Drug Interaction Study With Lanabecestat. *Journal of clinical pharmacology* *60*(1):107–116

**Willmann 2007** Willmann S, Höhn K, Edginton A, Sevestre M, Solodenko J, Weiss W, Lippert J, Schmitt W. (2007). Development of a physiology-based whole-body population model for assessing the influence of individual variability on the pharmacokinetics of drugs. *J Pharmacokinet Pharmacodyn.* 34(3), 401-431.

**Windass 2007** Windass AS, Lowes S, Wang Y, Brown CDA (2007) The contribution of organic anion transporters OAT1 and OAT3 to the renal uptake of rosuvastatin. *The Journal of pharmacology and experimental therapeutics* *322*(3):1221–7

**Wishart 2006** Wishart DS, Knox C, Guo AC, Shrivastava S, Hassanali M, Stothard P, Chang Z, Woolsey J (2006) DrugBank: a comprehensive resource for in silico drug discovery and exploration. *Nucleic Acids Research 34*(Supplement 1):D668–D672

**Wu 2017** Wu HF, Hristeva N, Chang J, Liang X, Li R, Frassetto L, Benet LZ (2017) Rosuvastatin Pharmacokinetics in Asian and White Subjects Wild Type for Both OATP1B1 and BCRP Under Control and Inhibited Conditions. *Journal of pharmaceutical sciences* *106*(9):2751–2757.

