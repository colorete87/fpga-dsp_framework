###############################################################################
#  Copyright and license header
# 
###############################################################################
#  File name: Makefile
###############################################################################
#  Description:
#    TODO
###############################################################################
#  History:
#    - 2022-04-01 - federico g. zacchigna - Original version.
#    - 2022-04-18 - federico g. zacchigna - Generics, wrappers and python lib added.
###############################################################################

# Sources
VHDL_SOURCES = $(addprefix $(PWD)/$(TEST_DIR)/wrappers/,$(VHDL_WRAPPERS)) \
	$(PWD)/rtl/pb/pb_v001.vhd \
	$(PWD)/rtl/pb/pb_v002.vhd \
	$(PWD)/rtl/pb/pb_v003.vhd \
	$(PWD)/rtl/pb/pb_pixo.vhd \
	$(PWD)/rtl/fb/fb_i1x1_o1xn.vhd \
	$(PWD)/rtl/fb/fb_imxn_o1xn.vhd \
	$(PWD)/rtl/fb/fb_imx1_omxn.vhd \
	$(PWD)/rtl/fb/fb_imxn_omx1.vhd \
	$(PWD)/rtl/fb/fb_imxn_opmxn.vhd \
	$(PWD)/rtl/fb/win_khxw_pwxi_rr.vhd \
	$(PWD)/rtl/fb/win_valid_i1xpi_khxw_opwx1_rr.vhd \
	$(PWD)/rtl/common_blocks/mac_ss.vhd \
	$(PWD)/rtl/common_blocks/shift_register_n.vhd \
	$(PWD)/rtl/common_blocks/reg.vhd \
	$(PWD)/rtl/common_blocks/sif_reg.vhd \
	$(PWD)/rtl/memory/ram.vhd \
	$(PWD)/rtl/test/sif_driver_test.vhd \
	$(PWD)/rtl/test/shift_register_test.vhd \
	$(PWD)/rtl/test/lenet_l00.vhd \
	$(PWD)/rtl/test/conv_test_0.vhd \

# Tests
TEST_DIR = test

# Waves format:
# - "vcd":
# - "fst":
# - "wave" (ghw):
WAVES_FORMAT=fst


###############################################################################
# DO NOT MODIFY THE FILE BELOW THIS LINE
###############################################################################

# Wrappers and generics
VHDL_WRAPPERS =$(notdir $(wildcard $(TEST_DIR)/wrappers/*))
GHDL_GENERICS =$(notdir $(wildcard $(TEST_DIR)/generics/*))

# Simulator
SIM ?= ghdl
TOPLEVEL_LANG ?= vhdl

# Other
WAVES=FALSE
TESTS_AUX=$(notdir $(wildcard $(TEST_DIR)/*))
TESTS=$(filter-out wrappers generics lib, $(TESTS_AUX))
TEST_RESULTS=$(addprefix $(TEST_DIR)/,$(addsuffix /results.xml,$(TESTS)))
LABEL=echo -e "\033[0;34m[Makefile]\033[0m"


.PHONY: all
all: usage list_tests


.PHONY: help
help: usage list_tests help1 list_generics list_wrappers list_sources


.PHONY: all_tests
all_tests: $(TESTS)
	@echo $$($(LABEL))" "
	@echo $$($(LABEL))" All tests done!"
	@echo $$($(LABEL))" TODO: Concatenate results!"
	@echo $$($(LABEL))" TODO: Existe un 'combine_results.py en ~/repos/cocotb/bin/"
	@echo $$($(LABEL))" "


.PHONY: all_tests-with_waves
all_tests-with_waves: $(addsuffix -with_waves,$(TESTS))
	@echo $$($(LABEL))" "
	@echo $$($(LABEL))" All tests done!"
	@echo $$($(LABEL))" TODO: Concatenate results!"
	@echo $$($(LABEL))" TODO: Existe un 'combine_results.py en ~/repos/cocotb/bin/"
	@echo $$($(LABEL))" "


# Print usage
.PHONY: usage
usage:
	@echo $$($(LABEL))" Usage: "
	@echo $$($(LABEL))"   make all_tests               Run all availabe tests."
	@echo $$($(LABEL))"   make all_tests-with_waves    Run all availabe tests and save waves."
	@echo $$($(LABEL))"   make <test_name>             Run a specific test."
	@echo $$($(LABEL))"   make <test_name>-with_waves  Run a specific test and save waves."
	@echo $$($(LABEL))"   make list_tests              Print available tests."
	@echo $$($(LABEL))"   make help                    Print some help."
	@echo $$($(LABEL))" "


# Print usage
.PHONY: help1
help1:
	@echo $$($(LABEL))" The configuration of the generic values of the top-level"
	@echo $$($(LABEL))" module may be performed in three different ways:"
	@echo $$($(LABEL))"   - Using the default generic values of the module"
	@echo $$($(LABEL))"   - Using generics definition files"
	@echo $$($(LABEL))"   - Using module wrappers"
	@echo $$($(LABEL))" "


# List all
.PHONY: list_all
list_all: usage list_tests list_wrappers list_generics list_sources


# List all available test
.PHONY: list_tests
list_tests:
	@echo $$($(LABEL))"   Available Test are:"
	@for test in $(addprefix "> ",$(TESTS)) ; do \
		echo $$($(LABEL))"    " $$test ; \
	done
	@echo $$($(LABEL))" "


# List all generics
.PHONY: list_generics
list_generics:
	@echo $$($(LABEL))"   Files defining generics are:"
	@for generic_file in $(addprefix "> ",$(GHDL_GENERICS)) ; do \
		echo $$($(LABEL))"    " $$generic_file ; \
	done
	@echo $$($(LABEL))" "


# List all wrappers
.PHONY: list_wrappers
list_wrappers:
	@echo $$($(LABEL))"   Wrappers are:"
	@for wrapper in $(addprefix "> ",$(VHDL_WRAPPERS)) ; do \
		echo $$($(LABEL))"    " $$wrapper ; \
	done
	@echo $$($(LABEL))" "


# List all VHDL sources
.PHONY: list_sources
list_sources:
	@echo $$($(LABEL))" Current source files are:"
	@for src_file in $(addprefix "- ",$(VHDL_SOURCES)) ; do \
		echo $$($(LABEL))"  " $$src_file ; \
	done
	@echo $$($(LABEL))" "


# Run a test set
.PHONY: $(TESTS)
$(TESTS): % : $(TEST_DIR)/%/results.xml
	@echo $<
	@echo $$($(LABEL))" TEST: $@ -> Done!"


# Run a test set With Waves
$(addsuffix -with_waves,$(TESTS)): WAVES=TRUE
$(addsuffix -with_waves,$(TESTS)): SIM_ARGS+=--$(WAVES_FORMAT)=waves.$(WAVES_FORMAT)
$(addsuffix -with_waves,$(TESTS)): %-with_waves : %


# Run a test set (results.xml)
$(TEST_RESULTS): TOPLEVEL=$(notdir $(shell echo $@ | sed -e "s/-.*//"))
$(TEST_RESULTS): TEST_NAME=$(subst /results.xml,,$(shell echo $@ | sed -e "s/.*-//"))
$(TEST_RESULTS): MODULE_AUX=$(basename $(notdir $(wildcard $(subst /results.xml,,$@)/*.py)))
$(TEST_RESULTS): MODULE=$(shell echo $(MODULE_AUX) | sed -e "s/ /,/g")
.SECONDEXPANSION:
$(TEST_RESULTS): %/results.xml : $(addprefix $(TEST_DIR)/wrappers/,$(VHDL_WRAPPERS)) $(addprefix $(TEST_DIR)/generics/,$(GHDL_GENERICS)) $(VHDL_SOURCES) %$(wildcard $$%/*py) $$(wildcard %/*.py)
	@# Create sim Makefile
	@echo "SIM           = $(SIM)"                                   > $(@:%/results.xml=%/Makefile)
	@echo "VHDL_SOURCES  = $(VHDL_SOURCES)"                         >> $(@:%/results.xml=%/Makefile)
	@echo "TOPLEVEL_LANG = $(TOPLEVEL_LANG)"                        >> $(@:%/results.xml=%/Makefile)
	@echo "TOPLEVEL      = $(TOPLEVEL)"                             >> $(@:%/results.xml=%/Makefile)
	@echo "MODULE        = $(MODULE)"                               >> $(@:%/results.xml=%/Makefile)
	@echo "SIM_ARGS      = $(SIM_ARGS)"                             >> $(@:%/results.xml=%/Makefile)
	@# Add generics
	@if [ -f $(@:%/results.xml=%/generics.txt) ]; then \
		cp $(@:%/results.xml=%/generics.txt) $(@:%/results.xml=%/generics.tmp)  ;\
		sed -i '/^\s*$$/d'        $(@:%/results.xml=%/generics.tmp) ;\
		sed -i '/^#/d'            $(@:%/results.xml=%/generics.tmp) ;\
		sed -i 's/ //g'           $(@:%/results.xml=%/generics.tmp) ;\
		sed -i 's/;//g'           $(@:%/results.xml=%/generics.tmp) ;\
		sed -i 's/,//g'           $(@:%/results.xml=%/generics.tmp) ;\
		sed -i 's/^/ -g/'         $(@:%/results.xml=%/generics.tmp) ;\
		sed -i 's/\n//g'          $(@:%/results.xml=%/generics.tmp) ;\
		cat                       $(@:%/results.xml=%/generics.tmp) | tr -d '\n' > $(@:%/results.xml=%/generics.aux) ;\
		mv                        $(@:%/results.xml=%/generics.aux) $(@:%/results.xml=%/generics.tmp)  ;\
		sed -i 's/^/SIM_ARGS +=/' $(@:%/results.xml=%/generics.tmp) ;\
		echo "" >>                $(@:%/results.xml=%/generics.tmp) ;\
		cat                       $(@:%/results.xml=%)/Makefile $(@:%/results.xml=%/generics.tmp) >> $(@:%/results.xml=%/Makefile2) ;\
		mv                        $(@:%/results.xml=%)/Makefile2 $(@:%/results.xml=%/Makefile) ;\
	fi
	@echo "include $(shell cocotb-config --makefiles)/Makefile.sim" >> $(@:%/results.xml=%/Makefile)
	@# Print info
	@echo $$($(LABEL))" SIM           = $(SIM)"
	@echo $$($(LABEL))" VHDL_SOURCES  = $(VHDL_SOURCES)"
	@echo $$($(LABEL))" TOPLEVEL_LANG = $(TOPLEVEL_LANG)"
	@echo $$($(LABEL))" TOPLEVEL      = $(TOPLEVEL)"
	@echo $$($(LABEL))" TEST_NAME     = $(TEST_NAME)"
	@echo $$($(LABEL))" MODULE_AUX    = $(MODULE_AUX)"
	@echo $$($(LABEL))" MODULE        = $(MODULE)"
	@echo $$($(LABEL))" WAVES         = $(WAVES)"
	@echo $$($(LABEL))" SIM_ARGS      = $(SIM_ARGS)"
	@if [ -f $(@:%/results.xml=%)"/generics.tmp" ]; then \
		echo $$($(LABEL))" INFO: Generics file found in test." ;\
		echo $$($(LABEL))" "$$(cat $(@:%/results.xml=%)/generics.tmp) ;\
	else \
		echo $$($(LABEL))" INFO: No generics file found in test. Using defaults." ;\
	fi
	@echo $$($(LABEL))""
	@# Run simulation
	$(MAKE) -C $(@:%/results.xml=%)/ sim
	-@if [[ $(WAVES_FORMAT) == "wave" && $(WAVES) == "TRUE" ]]; then \
		rename .wave .ghw $(@:%/results.xml=%)/waves* ; \
	fi
	@echo $$($(LABEL))" "


# Clean all tests
.PHONY: clean
clean:
	-@for test in $(TESTS) ; do \
	 	$(MAKE) -C $(TEST_DIR)/$$test clean ; \
	 	rm -rf $(TEST_DIR)/$$test/results.xml ; \
	 	rm -rf $(TEST_DIR)/$$test/*.o ; \
	 	rm -rf $(TEST_DIR)/$$test/__pycache__ ; \
		find $(TEST_DIR)/$$test/ -type f ! \( \
        -name '*.py' -o \
        -name '*.vhd' -o \
        -name '*.gtkw' -o \
        -name 'generics.txt' \
	  \) -delete ; \
	done

