# SMOKE-MOVES
SMOKE-MOVES2014 Processing Scripts

This package assumes a base directory of `/opt/SMOKE-MOVES/`. Update the following input files with your installation location:

```
inputs/countyrep.in
inputs/06001/control.in
```

Also, set the location of your MOVES2014 installation in `inputs/06001/control.in`:

`MOVESHOME      = /opt/MOVES2014`

Generate import scripts and runspec files:

`> scripts/runspec_generator.pl inputs/06001/control.in inputs/countyrep.in -csh`

Import custom county data:

`> csh runspec_files/06001/06001_2011importer.csh`

Run MOVES2014:

`> csh runspec_files/06001/06001_2011runspec.csh`

After MOVES finishes running, generate emission factor files:

```
> mkdir efs
> mkdir efs/06001
> scripts/moves2smkEF.pl -r RPD \
    --formulas scripts/pollutant_formulas_AQ.txt \
    --proc_agg scripts/process_aggregation.csv \
    runspec_files/06001/06001_2011outputDBs.txt \
    scripts/pollutant_mapping_AQ_CB05.csv efs/06001/
> scripts/moves2smkEF.pl -r RPV \
    --formulas scripts/pollutant_formulas_AQ.txt \
    --proc_agg scripts/process_aggregation_rpv.csv \
    runspec_files/06001/06001_2011outputDBs.txt \
    scripts/pollutant_mapping_AQ_CB05.csv efs/06001/
> scripts/moves2smkEF.pl -r RPP \
    --formulas scripts/pollutant_formulas_AQ.txt \
    --proc_agg scripts/process_aggregation.csv \
    runspec_files/06001/06001_2011outputDBs.txt \
    scripts/pollutant_mapping_AQ_CB05.csv efs/06001/
> scripts/moves2smkEF.pl -r RPH \
    --formulas scripts/pollutant_formulas_AQ.txt \
    --proc_agg scripts/process_aggregation.csv \
    runspec_files/06001/06001_2011outputDBs.txt \
    scripts/pollutant_mapping_AQ_CB05.csv efs/06001/
```

## Data sources

### County-specific MOVES data

The CSV files in `inputs/06001/` are extracted from the representative county database `c06001y2011_20150301` available from the EPA at ftp://ftp.epa.gov/EmisInventory/2011v6/v2platform/2011emissions/onroad/2011RepCDBs-030115versions.zip

File|Database Table
----|---
agedistribution.csv|sourcetypeagedistribution
avgspeeddistribution.csv|avgspeeddistribution
dayvmtfraction.csv|dayvmtfraction
fuelavft.csv|avft
fuelformulation.csv|fuelformulation
fuelsupply.csv|fuelsupply
fuelusage.csv|fuelusagefraction
hourvmtfraction.csv|hourvmtfraction
hpmsvtypeyear.csv|hpmsvtypeyear
imcoverage.csv|imcoverage
monthvmtfraction.csv|monthvmtfraction
population.csv|sourcetypeyear
roadtypedistribution.csv|roadtypedistribution

### Meteorology files

```
inputs/MOVES_DAILY_12US2_2011001-2011365.txt
inputs/MOVES_RH_DAILY_12US2_2011001-2011365.txt
```

These [Met4moves](https://www.cmascenter.org/smoke/documentation/3.6.5/html/ch06s07.html) output files are extracted from the national versions available from the EPA at ftp://ftp.epa.gov/EmisInventory/2011v6/v2platform/2011emissions/onroad/2011NEIv2_SMOKE_MOVES2014_met.zip