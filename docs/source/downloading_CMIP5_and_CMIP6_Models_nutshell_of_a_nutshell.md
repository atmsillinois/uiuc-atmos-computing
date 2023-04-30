# Downloading CMIP5/6 Models Datasets from ESGF in a Nutshell

This tutorial is applicable to downloading both CMIP5 and CMIP6 model output, and will cover on downloading from [Earth System Grid Federation (ESGF)](https://esgf-node.llnl.gov/search/cmip6/). 

This is aimed to serve as a quick guide to download global model output. For any other questions, please see the [detailed ESGF guide](https://esgf.github.io/esgf-user-support/user_guide.html?highlight=open%20id), or reach out to fellow folks working with CMIP output (at UIUC or the developers at LLNL). Please take any suggestions provided in this guide as a grain of salt. 


## Workflow of Dowloading CMIP model datasets from ESGF nodes 
![esgfdownloadworkflow](../images/esgfdownloadworkflow_whitebg.png)

1. **Login to ESGF-LLNL Node.**
    
    - CMIP5: https://esgf-node.llnl.gov/search/cmip5/
    - CMIP6: https://esgf-node.llnl.gov/search/cmip6/

2. **Identify and Apply Search Conditions**
3. **Identify Best-suited Download Method**
4. **Proceed Download to Keeling**
    

<br/><br/>

--------
## One-click Resources: 
1. [ESGF User Support Documentation](https://esgf.github.io/esgf-user-support/)
2. [IPCC Standard Output from Coupled Ocean-Atmosphere GCMs (Table of variable names)](https://pcmdi.llnl.gov/mips/cmip3/variableList.html)
3. [CMIP6 Participant Guidance for Modelers](https://pcmdi.llnl.gov/CMIP6/Guide/modelers.html)
4. [Earth System Documentation(ES-Doc)](https://view.es-doc.org/?renderMethod=id&project=cmip6&id=f83db5ce-af53-4c2e-8cf8-ff4f38e49c3d&version=1&client=esdoc-search)
5. [(CMIP6 Data Only - with Jupyter Notebook Example) Loading CMIP6 Data from Google Cloud by Ryan Abernathey (See 2.)](https://medium.com/pangeo/cmip6-in-the-cloud-five-ways-96b177abe396)
