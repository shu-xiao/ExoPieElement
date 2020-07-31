this branch is already good for 2017 analysis, 



# Installation of ExoPieElement and dependencies
 please make all changes for 2016 in the present branch such that this can be used for both years. 

 make all the new switches configurable via python file, **DO NOT MAKE C++ ONLY SWITCHES ANYMORE.**

now the setup works both in SLC6 and centos 7, You can login to slc6 using USERNAME@lxplus6.cern.ch   and slc7 using USERNAME@lxplus.cern.ch

**It is recomended to use centos 7**

### Setup CMSSW
```
Working on lxplus7

##SCRAM ARCH SET for CMSSW_10_2_X

export SCRAM_ARCH=slc7_amd64_gcc700
cmsrel CMSSW_10_2_18
cd CMSSW_10_2_18/src/

cmsenv
git cms-init
git cms-merge-topic cms-egamma:EgammaPostRecoTools
git cms-merge-topic cms-met:METFixEE2017_949_v2_backport_to_102X  
##  Twiki: https://twiki.cern.ch/twiki/bin/view/CMS/MissingETUncertaintyPrescription

git clone -b VBFHH_upgrade git@github.com:ExoPie/ExoPieElement.git

scram b -j 4

git clone git@github.com:cms-jet/JetToolbox.git JMEAnalysis/JetToolbox -b jetToolbox_102X

scram b -j 4

cd ExoPieElement/TreeMaker/test/

## before cmsRun do set the proxy. 
voms-proxy-init -voms cms 
cmsRun treeMaker_16_17_cfg.py
```
If the file doesn't work, instead of /tmp/khurana.... use filename, it will take some time to run via xrootd. 

you have to change the line 

fileNames = cms.untracked.vstring("file:/tmp/khurana/temp2017.root"),

by

fileNames = cms.untracked.vstring(testFile),


## for crab submission 

go the the directory: 

cd ExoPieElement/CrabUtilities

copy the latest .py file from test dir, do it via

source setup.sh


edit the crabConfig_2017_MC.py file, change following parameters as per your site of storage: 

config.Data.splitting = 'EventAwareLumiBased'
config.Data.unitsPerJob = 30000

config.Site.storageSite = "T3_TW_NCU"
config.Data.outLFNDirBase = '/store/group/phys_exotica/bbMET/ExoPieElementTuples/%s' %(workname)

you can make a list of samples in a text and use it to submit multiple jobs using MultiCrab_2017MC.py

you need to edit the .txt file name and run it using 

python MultiCrab_2017MC.py

 to check the status of all the jobs you just submitted you can add one function in the same file to do it. there is some example in 




for crab submit: 
python MultiCrab_2017MC.py --submit 

for crab status: 
python MultiCrab_2017MC.py --status --crabdir=crab_MC_2017miniaodV2_V1

for crab status with summary of all the dataset:  (ss refer to status summary)
python MultiCrab_2017MC.py --status --crabdir=crab_MC_2017miniaodV2_V1 --ss 

for crab resubmit: 
python MultiCrab_2017MC.py --resubmit --crabdir=crab_MC_2017miniaodV2_V1

for crab kill: 
python MultiCrab_2017MC.py --kill --crabdir=crab_MC_2017miniaodV2_V1



## before cmsRun set the proxy

##Then run the test file by

cmsRun treeMaker_2018_cfg.py
