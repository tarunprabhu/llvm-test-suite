if(ENDIAN STREQUAL "little")
  list(APPEND CFLAGS -DTARGET_RT_LITTLE_ENDIAN=1)
  list(APPEND CXXFLAGS -DTARGET_RT_LITTLE_ENDIAN=1)
else()
  list(APPEND CFLAGS -DTARGET_RT_BIG_ENDIAN=1)
  list(APPEND CXXFLAGS -DTARGET_RT_BIG_ENDIAN=1)
endif()
set(RUN_OPTIONS - - < tune.wav)
llvm_multisource(alacconvert-encode
  EndianPortable.c
  ALACBitUtilities.c
  ALACDecoder.cpp
  ALACEncoder.cpp
  ag_dec.c
  ag_enc.c
  dp_dec.c
  dp_enc.c
  matrix_dec.c
  matrix_enc.c
  main.cpp
  CAFFileALAC.cpp
)
llvm_test_data(alacconvert-encode tune.wav)
