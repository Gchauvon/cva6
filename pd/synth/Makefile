#
# Copyright 2021 Thales DIS design services SAS
#
# Licensed under the Solderpad Hardware Licence, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# SPDX-License-Identifier: Apache-2.0 WITH SHL-2.0
# You may obtain a copy of the License at https://solderpad.org/licenses/
#
# Original Author: Jean-Roch COULON (jean-roch.coulon@thalesgroup.com)
#

DESIGN_NAME           = ariane
PERIOD               ?= 17
FOUNDRY_PATH         ?= /shares/common/tools/techno/synopsys/bitcoin_v1.1_lib/lib/stdcell_rvt/db_nldm/
TECH_NAME            ?= saed32rvt_tt1p05v25c
TARGET_LIBRARY_FILES  = $(TECH_NAME).db
DC_SHELL_PATH         = /opt/synopsys/syn/Q-2019.12/bin/

EXPORT_LIST=SNPSLMD_QUEUE=TRUE TECH_NAME=$(TECH_NAME) DESIGN_NAME=$(DESIGN_NAME) TERM=vt100 PERIOD=$(PERIOD) FOUNDRY_PATH=$(FOUNDRY_PATH) TARGET_LIBRARY_FILES=$(TARGET_LIBRARY_FILES)

pre_cva6_synth:
	grep "CVA6_REPO_DIR\}" ../../core/Flist.cv64a6_imafdc_sv39|grep -v "instr_tracer"|grep -v "incdir"|grep -v "SyncSpRamBeNx64" > Flist.cva6_synth
	sed -i "s/^/analyze -f sverilog -define WT_DCACHE -lib ariane_lib /" Flist.cva6_synth
	echo "analyze -f sverilog -define WT_DCACHE -lib ariane_lib SyncSpRamBeNx64.sv" >> Flist.cva6_synth

cva6_synth: pre_cva6_synth
	@echo $(PERIOD)
	@export $(EXPORT_LIST); $(DC_SHELL_PATH)/dc_shell -f ./cva6_synth.tcl -output synthesis_batch.log
	python scripts/gateAnalysis.py '$(DESIGN_NAME)/reports/$(PERIOD)/$(DESIGN_NAME)_$(TECH_NAME)_synth_area.rpt' 1000

cva6_read:
	@export $(EXPORT_LIST); $(DC_SHELL_PATH)/dc_shell -f cva6_read.tcl -gui

clean:
	@rm -rf work alib* *_LIB *log *svf netlist reports ${DESIGN_NAME}
