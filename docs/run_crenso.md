[⬅ Voltar](../README.md)

# Workflow

### Informações iniciais

Para rodar o CREST, você vai precisar.

- Ter o arquivo da molécula, que pode estar no formato XYZ, MOL, COORD ou SDF.
- Saber o solvente mais adequado para rodar o CREST e o CENSO para a fase líquida, o método usado no artigo do grupo do professor Stefan Grimme é escolher um solvente com uma constante dielétrica semelhante, você pode verificar os solventes disponíveis no CREST [aqui](https://xtb-docs.readthedocs.io/en/latest/gbsa.html), os solventes disponíveis no CENSO estão no arquivo [censo_solvents.json](https://xtb-docs.readthedocs.io/en/latest/CENSO_docs/censo_solvation.html), como muita gente não está acostumada com o formato JSON, irei apresentar o arquivo em formato tabular, lembrando que para versões futuras do CENSO, o arquivo censo_solvents.json pode estar diferente.
    
    
    | Solvente | DC | SMD |
    | --- | --- | --- |
    | hexane | 1.9 | [N-HEXANE, N-HEXANE] |
    | octane | 1.94 | [N-OCTANE, N-OCTANE] |
    | cyclohexane | 2.0 | [CYCLOHEXANE, CYCLOHEXANE] |
    | hexadecane | 2.1 | [N-HEXADECANE, N-HEXADECANE] |
    | ccl4 | 2.2 | [CARBON TETRACHLORIDE, CARBON TETRACHLORIDE] |
    | dioxane | 2.2 | [1,4-DIOXANE, 1,4-DIOXANE] |
    | benzene | 2.3 | [BENZENE, BENZENE] |
    | toluene | 2.4 | [TOLUENE, TOLUENE] |
    | cs2 | 2.6 | [CARBON DISULFIDE, CARBON DISULFIDE] |
    | furan | 3.0 | [None, THF] |
    | chbr3 | 4.25 | [BROMOFORM, BROMOFORM] |
    | diethylether | 4.4 | [DIETHYL ETHER, DIETHYL ETHER] |
    | chcl3 | 4.8 | [CHLOROFORM, CHLOROFORM] |
    | dibromoethane | 4.9 | [1,2-DIBROMOETHANE, 1,2-DIBROMOETHANE] |
    | ethylacetate | 5.9 | [ETHYL ETHANOATE, ETHYL ETHANOATE] |
    | aniline | 6.9 | [ANILINE, ANILINE] |
    | thf | 7.6 | [TETRAHYDROFURAN, TETRAHYDROFURAN] |
    | phenol | 8.0 | [None, THIOPHENOL] |
    | woctanol | 8.1 | [None, 1-OCTANOL] |
    | ch2cl2 | 9.1 | [DICHLOROMETHANE, DICHLOROMETHANE] |
    | octanol | 9.9 | [1-OCTANOL, 1-OCTANOL] |
    | dichloroethane | 10.125 | [1,2-DICHLOROETHANE, 1,2-DICHLOROETHANE] |
    | benzaldehyde | 18.2 | [BENZALDEHYDE, BENZALDEHYDE] |
    | acetone | 20.7 | [ACETONE, ACETONE] |
    | ethanol | 24.6 | [ETHANOL, ETHANOL] |
    | methanol | 32.7 | [METHANOL, METHANOL] |
    | acetonitrile | 36.6 | [ACETONITRILE, ACETONITRILE] |
    | nitromethane | 38.2 | [NITROMETHANE, NITROMETHANE] |
    | dmf | 38.3 | [N,N-DIMETHYLFORMAMIDE, N,N-DIMETHYLFORMAMIDE] |
    | dmso | 47.2 | [DIMETHYLSULFOXIDE, DIMETHYLSULFOXIDE] |
    | h2o | 80.1 | [WATER, WATER] |
- Basicamente, você vai precisar saber a constante dielétrica do seu composto, para assim selecionar um solvente com a constante dielétrica próxima. Cada modelo de solvatação possui diferentes solventes parametrizados, ou seja, a lista de solventes disponíveis com o ORCA e SMD é diferente dos solventes disponíveis com o COSMOtherm e COSMO-RS.

### Arquivo da molécula

O primeiro passo é obter o arquivo da molécula.

Pode ser feito com programas como o Avogadro, o programa mais simples é o [MolView](https://molview.org/), o arquivo exportado no formato .mol serve como arquivo de entrada para o CREST.

### CREST

O  [CREST](https://crest-lab.github.io/crest-docs/page/documentation/keywords.html#level-of-theory-options) (Conformer–Rotamer Ensemble Sampling Tool) é usado para gerar o conjunto conformacional, ou seja, a partir de um arquivo contendo a estrutura da molécula, o CREST vai gerar o conjunto conformacional nas condições que foram passadas.

Para gás, comando é mais simples

```bash
crest struc.xyz --gfn2//gfnff -T 6 > crest.out
```

Para líquido, você precisa passar a opção —alpb, conforme o artigo, você precisa selecionar um solvente que contém a constante dielétrica mais próxima da constante dielétrica do seu composto, e você precisa passar esse solvente para o CREST

```bash
crest struc.xyz --alpb seu_solvente --gfn2//gfnff -T 6 > crest.out
```

- **crest**: comando para chamar o crest.
- **struc.xyz**: arquivo onde estão as coordenadas, o arquivo pode ter os formatos .xyz, .mol, .coord e .sdf.
- **—alpb**: solvatação implícita.
- **—gfn2//gfnff**: informo que usaremos os métodos gfn2 e gfnff de forma combinada.
- **-T**: número de CPUs que serão usados no cálculo, no comando acima o valor é 6, mas você pode mudar para a quantidade de núcleos que quer usar nesse processamento.
- **> crest.out**: todo o output será salvo nesse arquivo.

### Configurar o CENSO

Para rodar o CENSO, o comando é mais simples.

```bash
censo > censo.out
```

- **censo**: comando para executar o CENSO.
- **> censo.out**: todo o output será salvo nesse arquivo.

O CENSO vai procurar por 2 arquivos.

- **crest_conformers.xyz:** arquivo contendo o conjunto conformacional, ou seja, o conjunto de estruturas possíveis que foi gerada pelo CREST.
- **.censorc:** arquivo de configuração do CENSO, que possui tudo que é necessário para a execução do programa.

### .censorc

O arquivo pode ser gerado com os comandos.

```bash
censo -newconfig
mv censorc_new .censorc
```

O arquivo gerado terá diversas opções, que são explicadas [aqui](https://xtb-docs.readthedocs.io/en/latest/CENSO_docs/censorc.html).

Deixarei aqui de exemplo, o arquivo .censorc usado para realizar cálculos da molécula X, na temperatura de 298.15K, usando o modelo de solvatação SMD.

### .censorc - exemplo para gas

```makefile
$CENSO global configuration file: .censorc
$VERSION:1.2.0 

ORCA: /home/taj/bin/orca_5_0_4
ORCA version: 5.0.4
GFN-xTB: /home/taj/miniconda3/envs/env/bin/xtb
CREST: /home/taj/bin/crest
mpshift: /path/including/binary/mpshift-binary # for NMR shielding calculations
escf: /path/including/binary/escf-binary # for NMR shielding calculations

#COSMO-RS
ctd = BP_TZVP_C30_1601.ctd cdir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES" ldir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES"
$ENDPROGRAMS

$CRE SORTING SETTINGS:
$GENERAL SETTINGS:
nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
charge: 0                        # ['number e.g. 0'] 
unpaired: 0                      # ['number e.g. 0'] 
solvent: gas                     # ['gas', 'acetone', 'acetonitrile', 'aniline', 'benzaldehyde', 'benzene', 'ccl4', '...'] 
prog_rrho: xtb                   # ['xtb'] 
temperature: 298.15              # ['temperature in K e.g. 298.15'] 
trange: [273.15, 378.15, 5]      # ['temperature range [start, end, step]'] 
multitemp: off                   # ['on', 'off'] 
evaluate_rrho: on                # ['on', 'off'] 
consider_sym: on                 # ['on', 'off'] 
bhess: on                        # ['on', 'off'] 
imagthr: automatic               # ['automatic or e.g., -100    # in cm-1'] 
sthr: automatic                  # ['automatic or e.g., 50     # in cm-1'] 
scale: automatic                 # ['automatic or e.g., 1.0 '] 
rmsdbias: off                    # ['on', 'off'] 
sm_rrho: alpb                    # ['alpb', 'gbsa'] 
progress: off                    # ['on', 'off'] 
check: on                        # ['on', 'off'] 
prog: orca                       # ['tm', 'orca']  
func: r2scan-3c                  # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basis: automatic                 # ['automatic', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
maxthreads: 6                    # ['number of threads e.g. 2'] 
omp: 1                           # ['number cores per thread e.g. 4'] 
balance: off                     # ['on', 'off'] 
cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...'] 

$PART0 - CHEAP-PRESCREENING - SETTINGS:
part0: on                        # ['on', 'off'] 
func0: b97-d3                    # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', '...'] 
basis0: def2-SV(P)               # ['automatic', 'def2-SV(P)', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part0_threshold: 4.0             # ['number e.g. 4.0'] 

$PART1 - PRESCREENING - SETTINGS:
# func and basis is set under GENERAL SETTINGS
part1: on                        # ['on', 'off'] 
smgsolv1: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part1_threshold: 3.5             # ['number e.g. 5.0'] 

$PART2 - OPTIMIZATION - SETTINGS:
# func and basis is set under GENERAL SETTINGS
part2: on                        # ['on', 'off'] 
prog2opt: prog                   # ['tm', 'orca', 'prog', 'automatic'] 
part2_threshold: 2.5             # ['number e.g. 4.0'] 
sm2: smd                         # ['cosmo', 'cpcm', 'dcosmors', 'default', 'smd'] modelos de solvatação implicita,
smgsolv2: smd                    # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] TROCAR PRA COSMORS
part2_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
ancopt: on                       # ['on'] 
hlow: 0.01                       # ['lowest force constant in ANC generation, e.g. 0.01'] 
opt_spearman: on                 # ['on', 'off'] VERIFICAR DEPOIS
part2_P_threshold: 99            # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 
optlevel2: automatic             # ['crude', 'sloppy', 'loose', 'lax', 'normal', 'tight', 'vtight', 'extreme', '...'] 
optcycles: 8                     # ['number e.g. 5 or 10'] 
spearmanthr: -4.0                # ['value between -1 and 1, if outside set automatically'] 
radsize: 10                      # ['number e.g. 8 or 10'] 
crestcheck: off                  # ['on', 'off'] 

$PART3 - REFINEMENT - SETTINGS:
part3: on                        # ['on', 'off'] 
prog3: prog                      # ['tm', 'orca', 'prog'] 
func3: pw6b95                    # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basis3: def2-TZVPD               # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
smgsolv3: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

$NMR PROPERTY SETTINGS:
$PART4 SETTINGS:
part4: off                       # ['on', 'off'] 
couplings: on                    # ['on', 'off'] 
progJ: prog                      # ['tm', 'orca', 'prog'] 
funcJ: pbe0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basisJ: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
sm4J: smd                        # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
shieldings: on                   # ['on', 'off'] 
progS: prog                      # ['tm', 'orca', 'prog'] 
funcS: pbe0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basisS: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
sm4S: smd                        # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
reference_1H: TMS                # ['TMS'] 
reference_13C: TMS               # ['TMS'] 
reference_19F: CFCl3             # ['CFCl3'] 
reference_29Si: TMS              # ['TMS'] 
reference_31P: TMP               # ['TMP', 'PH3'] 
1H_active: on                    # ['on', 'off'] 
13C_active: on                   # ['on', 'off'] 
19F_active: off                  # ['on', 'off'] 
29Si_active: off                 # ['on', 'off'] 
31P_active: off                  # ['on', 'off'] 
resonance_frequency: 300.0       # ['MHz number of your experimental spectrometer setup'] 

$OPTICAL ROTATION PROPERTY SETTINGS:
$PART5 SETTINGS:
optical_rotation: off            # ['on', 'off'] 
funcOR: pbe                      # ['functional for opt_rot e.g. pbe'] 
funcOR_SCF: r2scan-3c            # ['functional for SCF in opt_rot e.g. r2scan-3c'] 
basisOR: def2-SVPD               # ['basis set for opt_rot e.g. def2-SVPD'] 
frequency_optical_rot: [589.0]   # ['list of freq in nm to evaluate opt rot at e.g. [589, 700]'] 
$END CENSORC
```

### .censorc - exemplo para líquido

```makefile
$CENSO global configuration file: .censorc
$VERSION:1.2.0 

ORCA: /home/taj/bin/orca_5_0_4
ORCA version: 5.0.4
GFN-xTB: /home/taj/miniconda3/envs/tcc/bin/xtb
CREST: /home/taj/bin/crest
mpshift: /path/including/binary/mpshift-binary # for NMR shielding calculations
escf: /path/including/binary/escf-binary # for NMR shielding calculations

#COSMO-RS
ctd = BP_TZVP_C30_1601.ctd cdir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES" ldir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES"
$ENDPROGRAMS

$CRE SORTING SETTINGS:
$GENERAL SETTINGS:
nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
charge: 0                        # ['number e.g. 0'] 
unpaired: 0                      # ['number e.g. 0'] 
solvent: benzene                 # ['gas', 'acetone', 'acetonitrile', 'aniline', 'benzaldehyde', 'benzene', 'ccl4', '...'] 
prog_rrho: xtb                   # ['xtb'] 
temperature: 298.15              # ['temperature in K e.g. 298.15'] 
trange: [273.15, 378.15, 5]      # ['temperature range [start, end, step]'] 
multitemp: off                   # ['on', 'off'] 
evaluate_rrho: on                # ['on', 'off'] 
consider_sym: on                 # ['on', 'off'] 
bhess: on                        # ['on', 'off'] 
imagthr: automatic               # ['automatic or e.g., -100    # in cm-1'] 
sthr: automatic                  # ['automatic or e.g., 50     # in cm-1'] 
scale: automatic                 # ['automatic or e.g., 1.0 '] 
rmsdbias: off                    # ['on', 'off'] 
sm_rrho: alpb                    # ['alpb', 'gbsa'] 
progress: off                    # ['on', 'off'] 
check: on                        # ['on', 'off'] 
prog: orca                       # ['tm', 'orca']  
func: r2scan-3c                  # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basis: automatic                 # ['automatic', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
maxthreads: 6                    # ['number of threads e.g. 2'] 
omp: 1                           # ['number cores per thread e.g. 4'] 
balance: off                     # ['on', 'off'] 
cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...'] 

$PART0 - CHEAP-PRESCREENING - SETTINGS:
part0: on                        # ['on', 'off'] 
func0: b97-d3                    # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', '...'] 
basis0: def2-SV(P)               # ['automatic', 'def2-SV(P)', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part0_threshold: 4.0             # ['number e.g. 4.0'] 

$PART1 - PRESCREENING - SETTINGS:
# func and basis is set under GENERAL SETTINGS
part1: on                        # ['on', 'off'] 
smgsolv1: smd_gsolv              # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part1_threshold: 3.5             # ['number e.g. 5.0'] 

$PART2 - OPTIMIZATION - SETTINGS:
# func and basis is set under GENERAL SETTINGS
part2: on                        # ['on', 'off'] 
prog2opt: prog                   # ['tm', 'orca', 'prog', 'automatic'] 
part2_threshold: 2.5             # ['number e.g. 4.0'] 
sm2: smd                         # ['cosmo', 'cpcm', 'dcosmors', 'default', 'smd'] modelos de solvatação implicita,
smgsolv2: smd_gsolv              # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] TROCAR PRA COSMORS
part2_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
ancopt: on                       # ['on'] 
hlow: 0.01                       # ['lowest force constant in ANC generation, e.g. 0.01'] 
opt_spearman: on                 # ['on', 'off'] VERIFICAR DEPOIS
part2_P_threshold: 99            # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 
optlevel2: automatic             # ['crude', 'sloppy', 'loose', 'lax', 'normal', 'tight', 'vtight', 'extreme', '...'] 
optcycles: 8                     # ['number e.g. 5 or 10'] 
spearmanthr: -4.0                # ['value between -1 and 1, if outside set automatically'] 
radsize: 10                      # ['number e.g. 8 or 10'] 
crestcheck: off                  # ['on', 'off'] 

$PART3 - REFINEMENT - SETTINGS:
part3: on                        # ['on', 'off'] 
prog3: prog                      # ['tm', 'orca', 'prog'] 
func3: pw6b95                    # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basis3: def2-TZVPD               # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
smgsolv3: smd_gsolv              # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

$NMR PROPERTY SETTINGS:
$PART4 SETTINGS:
part4: off                       # ['on', 'off'] 
couplings: on                    # ['on', 'off'] 
progJ: prog                      # ['tm', 'orca', 'prog'] 
funcJ: pbe0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basisJ: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
sm4J: smd                        # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
shieldings: on                   # ['on', 'off'] 
progS: prog                      # ['tm', 'orca', 'prog'] 
funcS: pbe0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
basisS: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
sm4S: smd                        # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
reference_1H: TMS                # ['TMS'] 
reference_13C: TMS               # ['TMS'] 
reference_19F: CFCl3             # ['CFCl3'] 
reference_29Si: TMS              # ['TMS'] 
reference_31P: TMP               # ['TMP', 'PH3'] 
1H_active: on                    # ['on', 'off'] 
13C_active: on                   # ['on', 'off'] 
19F_active: off                  # ['on', 'off'] 
29Si_active: off                 # ['on', 'off'] 
31P_active: off                  # ['on', 'off'] 
resonance_frequency: 300.0       # ['MHz number of your experimental spectrometer setup'] 

$OPTICAL ROTATION PROPERTY SETTINGS:
$PART5 SETTINGS:
optical_rotation: off            # ['on', 'off'] 
funcOR: pbe                      # ['functional for opt_rot e.g. pbe'] 
funcOR_SCF: r2scan-3c            # ['functional for SCF in opt_rot e.g. r2scan-3c'] 
basisOR: def2-SVPD               # ['basis set for opt_rot e.g. def2-SVPD'] 
frequency_optical_rot: [589.0]   # ['list of freq in nm to evaluate opt rot at e.g. [589, 700]'] 
$END CENSORC
```

Lembrando, que esse é um guia simplificado, feito para executar o CRENSO usando esses programas de uma determinada versão, os passos podem mudar em versões futuras e por isso a leitura da documentação é sempre recomendada, aqui estão alguns links úteis.

- [Artigo do grupo do professor Stefan Grimme, onde ele apresenta o CRENSO.](https://pubs.rsc.org/en/content/articlehtml/2022/em/d2em00271j)
- [Documentação do xTB.](https://xtb-docs.readthedocs.io/en/latest/setup.html#)
- [Documentação do CREST.](https://crest-lab.github.io/crest-docs/)
- [Documentação do CENSO.](https://xtb-docs.readthedocs.io/en/latest/CENSO_docs/censo.html)
- [Documentação do ORCA.](https://www.orcasoftware.de/tutorials_orca/first_steps/install.html#)

Após os calculos serem concluídos, você pode calcular a pressão de vapor, [aqui](calculate_vp.md) você encontra um guia de como isso por ser feito.