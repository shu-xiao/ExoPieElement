# Installation of ExoPieElement and dependencies
New README

## now the setup works both in SLC6 and centos 7, You can login to slc6 using USERNAME@lxplus6.cern.ch   and slc7 using USERNAME@lxplus.cern.ch

## it is recomended to use centos 7 

### setup CMSSW

export SCRAM_ARCH=slc7_amd64_gcc630     ## for slc 6 use export SCRAM_ARCH=slc6_amd64_gcc630 

cmsrel CMSSW_9_4_13

cd CMSSW_9_4_13/src

cmsenv

git cms-init

git cms-merge-topic cms-egamma:EgammaPostRecoTools #just adds in an extra file to have a setup function to make things easier


git cms-merge-topic cms-met:METFixEE2017_949_v2


git clone git@github.com:ExoPie/ExoPieElement.git

scram b -j 4 


##For jetToolBox

git clone git@github.com:cms-jet/JetToolbox.git JMEAnalysis/JetToolbox

cd JMEAnalysis/JetToolbox

git checkout jetToolbox_94X_v3

cd -

scram b -j 4


cd ExoPieElement/TreeMaker

config file to run is in test dir: named treeMaker_Summer17_cfg.py 

cd test 

## before cmsRun do set the proxy. 

cmsRun treeMaker_Summer17_cfg.py

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

for crab resubmit: 
python MultiCrab_2017MC.py --resubmit --crabdir=crab_MC_2017miniaodV2_V1

for crab kill: 
python MultiCrab_2017MC.py --kill --crabdir=crab_MC_2017miniaodV2_V1
