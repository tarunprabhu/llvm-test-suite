list(APPEND CPPFLAGS -DTORONTO)
if(SMALL_PROBLEM_SIZE)
  set(RUN_OPTIONS 20)
else()
  set(RUN_OPTIONS 22)
endif()
llvm_multisource(treeadd)
