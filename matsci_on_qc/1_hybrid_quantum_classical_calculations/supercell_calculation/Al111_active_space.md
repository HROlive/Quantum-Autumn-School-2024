# CP2K Input Configuration

This file contains the CP2K input configuration for the hybrid quantum-classical calculation of an Al(111) surface with active space embedding.

## Full Implementation

```content:Al111_active_space.inp
&GLOBAL
  PROJECT Al111_4x4_active_space_adsorbate
  PRINT_LEVEL MEDIUM
  RUN_TYPE ENERGY
&END GLOBAL

&FORCE_EVAL
  METHOD QUICKSTEP
  &DFT
    BASIS_SET_FILE_NAME BASIS_MOLOPT
    POTENTIAL_FILE_NAME POTENTIAL
    &QS
      METHOD GPW
      EPS_DEFAULT 1.0E-10
      EXTRAPOLATION ASPC
      EXTRAPOLATION_ORDER 3
    &END QS
    &POISSON
      PERIODIC XYZ
    &END POISSON
    &MGRID
      CUTOFF 500
      REL_CUTOFF 60
      NGRIDS 5
    &END MGRID
    &SCF
      SCF_GUESS MOPAC
      EPS_SCF 1.0E-6
      MAX_SCF 500
      ADDED_MOS 100
      &DIAGONALIZATION
        ALGORITHM STANDARD
        EPS_ADAPT 0.01
      &END DIAGONALIZATION
      &MIXING
        METHOD BROYDEN_MIXING
        ALPHA 0.1
        BETA 1.5
        NBROYDEN 8
      &END MIXING
      &SMEAR
        METHOD FERMI_DIRAC
        ELECTRONIC_TEMPERATURE [K] 1000
      &END SMEAR
      &OUTER_SCF
        MAX_SCF 50
        EPS_SCF 1.0E-6
      &END OUTER_SCF
      &PRINT
        &RESTART ON
        &END
      &END PRINT
      IGNORE_CONVERGENCE_FAILURE
    &END SCF
    &XC
      &XC_FUNCTIONAL PBE
      &END XC_FUNCTIONAL
      &VDW_POTENTIAL
        DISPERSION_FUNCTIONAL PAIR_POTENTIAL
        &PAIR_POTENTIAL
          TYPE DFTD3
          PARAMETER_FILE_NAME dftd3.dat
          REFERENCE_FUNCTIONAL PBE
        &END PAIR_POTENTIAL
      &END VDW_POTENTIAL
    &END XC
    &PRINT
      &MO
        ENERGIES TRUE
        OCCUPATION_NUMBERS TRUE
        &EACH
          QS_SCF 0
        &END
      &END
    &END PRINT
    &ACTIVE_SPACE
      ACTIVE_ELECTRONS 2
      ACTIVE_ORBITALS 5
      SCF_EMBEDDING TRUE
      EPS_ITER 1E-6
      MAX_ITER 100
      AS_SOLVER QISKIT
      ORBITAL_SELECTION CANONICAL
      &ERI
        METHOD FULL_GPW
        PERIODICITY 1 1 1
        OPERATOR <1/r>
      &END ERI
      &ERI_GPW
        CUTOFF 500
        REL_CUTOFF 60
      &END ERI_GPW
      &FCIDUMP ON
      &END FCIDUMP
    &END ACTIVE_SPACE
  &END DFT
  &SUBSYS
    &CELL
      A 11.45512985522207 0.00000000000000 0.0000000000
      B 5.727564927611035 9.92043345827187 0.0000000000
      C 0.000000000000000 0.00000000000000 40.000000000
      PERIODIC XYZ
    &END CELL
    &TOPOLOGY
      COORD_FILE_NAME al_slab_with_triazole_4x4x6_v10.0.xyz
      COORD_FILE_FORMAT xyz
    &END TOPOLOGY
    &KIND Al
      BASIS_SET DZVP-MOLOPT-SR-GTH
      POTENTIAL GTH-PBE-q3
    &END KIND
    &KIND C
      BASIS_SET DZVP-MOLOPT-GTH
      POTENTIAL GTH-PBE-q4
    &END KIND
    &KIND N
      BASIS_SET DZVP-MOLOPT-GTH
      POTENTIAL GTH-PBE-q5
    &END KIND
    &KIND H
      BASIS_SET DZVP-MOLOPT-GTH
      POTENTIAL GTH-PBE-q1
    &END KIND            
  &END SUBSYS
&END FORCE_EVAL
```

## Key Components

1. **Global Settings**: Project name and calculation type configuration
2. **DFT Settings**: Basis sets, functionals, and convergence parameters
3. **Active Space Configuration**: Settings for quantum-classical embedding
4. **System Structure**: Cell parameters and atomic species definitions