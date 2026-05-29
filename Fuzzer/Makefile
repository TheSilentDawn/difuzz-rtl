# Verilator COCOTB Makefile

CWD=$(shell pwd)

SIM=verilator
CMD=verilator

PYTHONPATH := $(CWD)/src:$(CWD)/RTLSim/src:$(PYTHONPATH)

TOPLEVEL_LAGN ?= verilog

TOPLEVEL ?= $(VFILE)
VERILOG_SOURCES = $(CWD)/../Benchmarks/Verilog/$(VFILE).v
COCOTB_HDL_TIMEUNIT=1us
COCOTB_HDL_TIMEPRECISION=1us

OUT ?= output
NUM_ITER ?= 1
TEMPLATE ?= Template
RECORD ?= 0

ifdef DEBUG
    PRINT=$(DEBUG)
    PRINTF_COND=0
else
    PRINT=0
endif

ifdef MULTICORE
    MULTI=-DMULTICORE
else
    MULTI=
endif

ifeq ($(VFILE), RocketTile_VHarness)
    INCLUDE=-I $(CWD)/../Benchmarks/Verilog/RocketTile_mux.v
else
    INCLUDE=
endif

PRINTF_COND ?= 0
STOP_COND ?= 0

COMPILE_ARGS=$(MULTI) $(INCLUDE) -DPRINTF_COND=$(PRINTF_COND) -DSTOP_COND=$(STOP_COND) -Wno-PINMISSING
PLUSARGS=+TOPLEVEL=$(TOPLEVEL) +NUM_ITER=$(NUM_ITER) +TEMPLATE=$(TEMPLATE) +OUT=$(OUT) +RECORD=$(RECORD) +DEBUG=$(PRINT)

MODULE=DifuzzRTL

CUSTOM_SIM_DEPS=$(CWD)/Makefile

include $(shell cocotb-config --makefiles)/Makefile.inc
include $(shell cocotb-config --makefiles)/Makefile.sim

# Override cocotb's default c++11 - Verilator 5.x requires c++14+
SIM_BUILD_FLAGS := $(filter-out -std=c++11,$(SIM_BUILD_FLAGS)) -std=c++17

sim: $(MODULE).py
