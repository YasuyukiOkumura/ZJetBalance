# ZJetBalance
Run II studies for Z+Jet Balancing including b-tagging

```
setupATLAS
rcSetup Base,2.3.15
git clone https://github.com/UCATLAS/xAODAnaHelpers
git clone https://github.com/UCATLAS/ZJetBalance
python xAODAnaHelpers/scripts/checkoutASGtags.py 2.3.15
<also we need to setup the JES_ResponseFitter here : see below>
rc find_packages
rc compile
```

Then to run an example file try:
```
runZBJetBalance -inFile /afs/cern.ch/user/g/gfacini/public/mc15_13TeV.361107.PowhegPythia8EvtGen_AZNLOCTEQ6L1_Zmumu.merge.DAOD_SUSY5.e3601_s2576_s2132_r6633_r6264_p2370_tid05768578_00/DAOD_SUSY5.05768578._000001.pool.root.1
```
The output tree is in submitDir/data-tree/.


Then, run analysis code over the output ntuple
```
runProcessZJetBalanceMiniTree  -inFile submitDir/data-tree/mc15_13TeV.361107.PowhegPythia8EvtGen_AZNLOCTEQ6L1_Zmumu.merge.DAOD_SUSY5.e3601_s2576_s2132_r6633_r6264_p2370_tid05768578_00.root -submitDir submitDir2
```
( it uses ProcessZJetBalanceMiniTree object implemented in 'Root/ProcessZJetBalanceMiniTree.cxx'. )

Then the output histogram will be fed into the final JES_Response_Fitter tool as following.
You may use submitDir/hist-mc15_13TeV.361107.PowhegPythia8EvtGen_AZNLOCTEQ6L1_Zmumu.merge.DAOD_SUSY5.e3601_s2576_s2132_r6633_r6264_p2370_tid05768578_00.root but use another example with enough statsitics. 
```
ZJetBalancePlotter -inFile ${ROOTCOREBIN}/data/ZJetBalance/PlotterExampleInput.root
```
Then you will see Zjet_DB_Gauss_fits.pdf as output.

* How to setup the JES_REesponseFitter
```
rc checkout_pkg atlasperf/CombPerf/JetETMiss/JetCalibrationTools/JES_ResponseFitter/trunk
```
edit 1) Root/JES_BalanceFitter.cxx, 2) cmt/Makefile.RootCore as following

1) add 'using namespace std;' in Root/JES_BalanceFitter.cxx

2) modify cmt/Makefile.RootCore as following
> before: PACKAGE_BINFLAGS = -lCintex -lReflex
> after: PACKAGE_BINFLAGS =


See TWiki for more information
