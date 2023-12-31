# https://www.spec.org/cpu2017/Docs/benchmarks/549.fotonik3d_r.html
include(../../SpecCPU2017.cmake)
if (NOT TEST_SUITE_SPEC2017_ROOT)
  return ()
endif ()

speccpu2017_benchmark(RATE)

## test ########################################################################

speccpu2017_run_test(
  RUN_TYPE test
)

## train #######################################################################

speccpu2017_run_test(
  RUN_TYPE train
)

## ref #########################################################################

speccpu2017_run_test(
  RUN_TYPE ref
)

################################################################################

# fotonik3d has both relative and absolute tolerances
speccpu2017_verify_output(RELATIVE_TOLERANCE 1.0e-10 ABSOLUTE_TOLERANCE 1.0e-27)

speccpu2017_add_executable(
  parameter.f90                                                                                                   
  globalvar.F90                                                                                                   
  readline.f90                                                                                                    
  power.F90                                                                                                       
  calcflops.F90                                                                                                   
  communicate.F90                                                                                                 
  huygens.F90                                                                                                     
  update.F90                                                                                                      
  material.F90                                                                                                    
  mur.F90                                                                                                         
  PlaneSource.F90                                                                                                 
  PEC.F90                                                                                                         
  init.F90                                                                                                        
  leapfrog.F90                                                                                                    
  timerRoutine.f90                                                                                                
  UPML.F90                                                                                                        
  yeemain.F90                                                                                                     
  writeout.F90                                                                                                    
  MPI_dummy.F90  
)

speccpu2017_prepare_rundir()

# Generate OBJ.dat from OBJ.dat.xyz for fotonik3d as input

# We do not require that the SPEC install script has been run.
# specxz is executed during the build process on the host after
# the build system is generated by CMake.
string(TOLOWER ${CMAKE_HOST_SYSTEM_NAME} CMAKE_HOST_SYSTEM_NAME_LC)
set(SPECXZ_BIN
  "${TEST_SUITE_SPEC2017_ROOT}/tools/bin/${CMAKE_HOST_SYSTEM_NAME_LC}-${CMAKE_HOST_SYSTEM_PROCESSOR}/specxz")

foreach (_run_type IN LISTS TEST_SUITE_RUN_TYPE)
  add_custom_command(
    OUTPUT "${RUN_${_run_type}_DIR}/OBJ.dat"
    DEPENDS "${INPUT_${_run_type}_DIR}/OBJ.dat.xz"
            "${SPECXZ_BIN}"
    COMMAND "${SPECXZ_BIN}"
            -dc "${INPUT_${_run_type}_DIR}/OBJ.dat.xz"
            > "${RUN_${_run_type}_DIR}/OBJ.dat"
    WORKING_DIRECTORY "${RUN_${_run_type}_DIR}"
    COMMENT "Generating OBJ.dat from ${INPUT_${_run_type}_DIR}/OBJ.dat.xz"
    VERBATIM
    )

  # ${PROG} must be included in custom target name in order
  # to allow re-use of code in both RATE and SPEED benchmarks
  add_custom_target(${PROG}_OBJ_dat_${_run_type}
    DEPENDS "${RUN_${_run_type}_DIR}/OBJ.dat"
  )
  add_dependencies(${PROG} ${PROG}_OBJ_dat_${_run_type})
endforeach ()
