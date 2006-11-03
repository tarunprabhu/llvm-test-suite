##===- Makefile --------------------------------------------*- Makefile -*-===##
#
# This makefile contains information for building the SPEC2006 soplex test.
#
##===----------------------------------------------------------------------===##

LEVEL = ../../../..

FP_ABSTOLERANCE = 1.0e-5

include ../../Makefile.spec2006

CPPFLAGS += -DNDEBUG

ifeq ($(RUN_TYPE),test)
  FP_TOLERANCE     = 0.0001
  RUN_OPTIONS      = -s1 -e -m10000 test.mps
  STDOUT_FILENAME := test.out
else
  FP_TOLERANCE     = 20
  RUN_OPTIONS      = -s1 -e -m1200 train.mps
  STDOUT_FILENAME := train.out
endif

LDFLAGS = -lstdc++ -lm
LIBS    = -lstdc++ -lm