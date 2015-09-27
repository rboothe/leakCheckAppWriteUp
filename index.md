---
title       : Coursera Assignment
subtitle    : Develop and Publish Shiny App - Leak Check App()
author      : 
job         : Coursera Student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap, shiny] # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Objectives/Rationale


The basic objective was to develop and deploy a prediction model that could form the framework for a range of applications : from water useage, to the mean time before failure (MTBF) of machine parts.

Prediction Models are applied in a wide range of fields - especially given the "explosion" of data being generated today.  Monitoring data is particularly useful here, as the number variables being electronically monitored, the accuracy, and the sampling rate lends itself to fairly accurate results - using established statistical techniques.

<div style='text-align: center;'>
    <img height='560' src='assets/img/utility1.jpg' />
</div>

---

## Objectives (cont'd...)

Implications for Unified Maintenance Planning in large-scale industries such as utilities are clear.  The mtbf of say conductor joints or steam boiler parts (disparate sectors of the utility) can be estimated on a central platform available to a range of professionals.  Established datasets with performance information under different conditions would be regularly sourced from teh websites of authorities such as US Department of Energy and the Electric Power Research Institute.

<div style='text-align: center;'>
    <img height='560' src='assets/img/utility2.jpg' />
</div>




The Potential Applications are Immense

---

##  Description of the App

### As originally intended :

The App accepts inputs by the user and uses a random forest based model to predict whether or not his/her premises has a leak. The model is built using a downloaded dataset of water useage from trusted site such as Data.gov

The first 4 inputs were found to be highly correlated to water consumption :  
* No. of persons in household
* Existence of a vegetable garden
* whether urban or rural
* Size of property  

The last entry is the observed consumption - which is compared with the expected value predicted by the model to render a verdict.

---

##  Description of the App (cont'd...)


### Shortcomings of the App

1. Problem: Appropriate Dataset to train the model was unavailable. Work Around: Sample dataset created using random number generator

2. Problem: Debugging the errors due to coercion was unending.  Work Around: __A threshold of 10__ was "hard-wired" into the code instead of generating a prediction using a random forest model

As a result the only entry affecting the outcome is the last (the monthly consumption - which renders "Leak" for values > 10).  See user interface below:


```r
slidifyUI(fluidPage(
    sidebarLayout(
        sidebarPanel(
            numericInput("ni.mem", label="Number of Persons in Household",
                         0, min=1, max=30, step=1),
            
            radioButtons("rb.gar", label="Do you have a Vegetable Garden ?",
                        c("Yes" = 1,
                         "No" = 0)),

            radioButtons("rb.rur", label="Do you live in the City ?",
                       c("Yes" = 1,
                        "No" = 0)),

            textInput("ti.acr", label="Size of Premises (in acres)"),
            
            textInput("ti.use", label="What is your Monthly water consumption (in gallons)"),
                       
            submitButton("Submit")
        ),
        
        mainPanel(
           br(),
           br(),
           br(),
           br(),
           h3("The Verdict :"),
           verbatimTextOutput("op1")
            
        )
    )
    
)
    
)
```

```
## <div class="row-fluid">
##   <div class="container-fluid">
##     <div class="row">
##       <div class="col-sm-4">
##         <form class="well">
##           <div class="form-group shiny-input-container">
##             <label for="ni.mem">Number of Persons in Household</label>
##             <input id="ni.mem" type="number" class="form-control" value="0" min="1" max="30" step="1"/>
##           </div>
##           <div id="rb.gar" class="form-group shiny-input-radiogroup shiny-input-container">
##             <label class="control-label" for="rb.gar">Do you have a Vegetable Garden ?</label>
##             <div class="shiny-options-group">
##               <div class="radio">
##                 <label>
##                   <input type="radio" name="rb.gar" value="1" checked="checked"/>
##                   <span>Yes</span>
##                 </label>
##               </div>
##               <div class="radio">
##                 <label>
##                   <input type="radio" name="rb.gar" value="0"/>
##                   <span>No</span>
##                 </label>
##               </div>
##             </div>
##           </div>
##           <div id="rb.rur" class="form-group shiny-input-radiogroup shiny-input-container">
##             <label class="control-label" for="rb.rur">Do you live in the City ?</label>
##             <div class="shiny-options-group">
##               <div class="radio">
##                 <label>
##                   <input type="radio" name="rb.rur" value="1" checked="checked"/>
##                   <span>Yes</span>
##                 </label>
##               </div>
##               <div class="radio">
##                 <label>
##                   <input type="radio" name="rb.rur" value="0"/>
##                   <span>No</span>
##                 </label>
##               </div>
##             </div>
##           </div>
##           <div class="form-group shiny-input-container">
##             <label for="ti.acr">Size of Premises (in acres)</label>
##             <input id="ti.acr" type="text" class="form-control" value=""/>
##           </div>
##           <div class="form-group shiny-input-container">
##             <label for="ti.use">What is your Monthly water consumption (in gallons)</label>
##             <input id="ti.use" type="text" class="form-control" value=""/>
##           </div>
##           <div>
##             <button type="submit" class="btn btn-primary">Submit</button>
##           </div>
##         </form>
##       </div>
##       <div class="col-sm-8">
##         <br/>
##         <br/>
##         <br/>
##         <br/>
##         <h3>The Verdict :</h3>
##         <pre id="op1" class="shiny-text-output"></pre>
##       </div>
##     </div>
##   </div>
## </div>
```
