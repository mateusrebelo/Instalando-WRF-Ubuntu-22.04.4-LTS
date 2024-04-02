# Instalando-WRF-Ubuntu-22.04.4-LTS

O Modelo de Pesquisa e Previsão do Tempo (WRF) é um sistema numérico de previsão do tempo em mesoescala utilizado em todo o mundo para previsões operacionais e fins de pesquisa.

Aqui está a versão resumida da instalação do WRF 4.5.2 (ÚLTIMA VERSÃO ATÉ O PRESENTE MOMENTO) em um sistema Linux Ubuntu 22.04.4 LTS baseado em Intel I7 - 14700.

**- Instalando pré-requisitos:**

```
sudo apt install csh gfortran m4 mpich libhdf5-mpich-dev libpng-dev libnetcdff-dev netcdf-bin ncl-ncarg build-essential
```

**- Instale o Jasperlib:**

```
wget https://www.ece.uvic.ca/~frodo/jasper/software/jasper-1.900.29.tar.gz
```
```
tar xvf jasper-1.900.29.tar.gz 
```
```
cd jasper-1.900.29/
```
```
./configure --prefix=/opt/jasper-1.900.29
make
```
```
sudo make install
```

**- Obtendo o WRF:**

A versão mais recente do WRF você deve obter atráves do link do repositorio:
[AQUI!!!](https://github.com/wrf-model/WRF/releases/tag/v4.5.2)

_OBSERVAÇÃO!!!!: Se estiver baixando os arquivos do link acima, escolha o arquivo v4.5.2.tar.gz ou o arquivo v4.5.2.zip. **NÃO** escolha aqueles denominados "Código Fonte". Eles não incluem o submódulo NoahMP obrigatório - necessário para compilar o WRF._

Caso deseje baixar versões anteriores que tem inclusos no codigo fonte o submódulo NoahMP obrigatório, você pode usar o seguinte método:
```
wget https://github.com/wrf-model/WRF/archive/v4.2.1.tar.gz
```
```
tar xvf v4.2.1.tar.gz
```
```
cd WRF-4.2.1/
```

**- Instalando o WRF:**

```
export NETCDF=/usr
```
```
export NETCDF_classic=1
```
```
./configure
```
Agora realize a instalação configurando da forma que você preferir. Onde vai aparecer as seguintes opções:

```
checking for perl5... no
checking for perl... found /usr/bin/perl (perl)
Will use NETCDF in dir: /usr
HDF5 not set in environment. Will configure WRF for use without.
PHDF5 not set in environment. Will configure WRF for use without.
Will use 'time' to report timing information
$JASPERLIB or $JASPERINC not found in environment, configuring to build without grib2 I/O...
------------------------------------------------------------------------
Please select from among the following Linux x86_64 options:

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
 13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)
                                         17. (dm+sm)   INTEL (ifort/icc): Xeon Phi (MIC architecture)
 18. (serial)  19. (smpar)  20. (dmpar)  21. (dm+sm)   INTEL (ifort/icc): Xeon (SNB with AVX mods)
 22. (serial)  23. (smpar)  24. (dmpar)  25. (dm+sm)   INTEL (ifort/icc): SGI MPT
 26. (serial)  27. (smpar)  28. (dmpar)  29. (dm+sm)   INTEL (ifort/icc): IBM POE
 30. (serial)               31. (dmpar)                PATHSCALE (pathf90/pathcc)
 32. (serial)  33. (smpar)  34. (dmpar)  35. (dm+sm)   GNU (gfortran/gcc)
 36. (serial)  37. (smpar)  38. (dmpar)  39. (dm+sm)   IBM (xlf90_r/cc_r)
 40. (serial)  41. (smpar)  42. (dmpar)  43. (dm+sm)   PGI (ftn/gcc): Cray XC CLE
 44. (serial)  45. (smpar)  46. (dmpar)  47. (dm+sm)   CRAY CCE (ftn $(NOOMP)/cc): Cray XE and XC
 48. (serial)  49. (smpar)  50. (dmpar)  51. (dm+sm)   INTEL (ftn/icc): Cray XC
 52. (serial)  53. (smpar)  54. (dmpar)  55. (dm+sm)   PGI (pgf90/pgcc)
 56. (serial)  57. (smpar)  58. (dmpar)  59. (dm+sm)   PGI (pgf90/gcc): -f90=pgf90
 60. (serial)  61. (smpar)  62. (dmpar)  63. (dm+sm)   PGI (pgf90/pgcc): -f90=pgf90
 64. (serial)  65. (smpar)  66. (dmpar)  67. (dm+sm)   INTEL (ifort/icc): HSW/BDW
 68. (serial)  69. (smpar)  70. (dmpar)  71. (dm+sm)   INTEL (ifort/icc): KNL MIC
 72. (serial)  73. (smpar)  74. (dmpar)  75. (dm+sm)   FUJITSU (frtpx/fccpx): FX10/FX100 SPARC64 IXfx/Xlfx

Enter selection [1-75] : 35
------------------------------------------------------------------------
Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 

Configuration successful! 
------------------------------------------------------------------------
testing for fseeko and fseeko64
fseeko64 is supported
------------------------------------------------------------------------
...
```
Eu utilizei a 35, onde _35 é (dm+sm) GNU (gfortran/gcc)_. Fica a seu críterio qual utilizar.

**- Abra _"configure.wrf"_ e edite:**

Você pode usar interface gráfica ou linha de comando. Se você quiser usar a linha de comando, vá para gedit editor. Para editar use os seguintes comandos:
```
gedit configure.wrf
```
Encontre a linha _"LIB_EXTERNAL"_ e substitua pelo código à seguir:
```
 LIB_EXTERNAL    = \
                      -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/usr/lib -lnetcdff -lnetcdf     
```
Finalizou a edição da linha, salve o arquivo, feche e compile seguindo o proximo passo.

**- Agora, compile o código:**
```
./compile -j 2 em_real 2>&1 | tee compile.log
```
A saída será assim no final:
```
--->                  Executables successfully built                  <---
 
-rwxrwxr-x 1 wrf wrf 40691640 Jul 30 12:35 main/ndown.exe
-rwxrwxr-x 1 wrf wrf 40572760 Jul 30 12:35 main/real.exe
-rwxrwxr-x 1 wrf wrf 40048888 Jul 30 12:35 main/tc.exe
-rwxrwxr-x 1 wrf wrf 44609360 Jul 30 12:35 main/wrf.exe
 
==========================================================================
```

FIM!
Créditos ao [cidadão](https://pratiman-91.github.io/2020/07/28/Installing-WRF-4.2.1-on-Ubuntu-LTS-20.04.html) e cŕeditos ao [manual](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php#STEP4)
