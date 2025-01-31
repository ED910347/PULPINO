# Variables
FSDB = /home/synopsys/tools/verdi/W-2024.09/share/PLI/VCS/LINUX64
IPS_FILELIST = $(PULP_PATH)/rtl/rtl_filelist.f
work = work # Library name
INC = +incdir+$(PULP_PATH)/rtl +incdir+$(PULP_PATH)/rtl/includes +incdir+$(PULP_PATH)/ips/adv_dbg_if/rtl +incdir+$(PULP_PATH)/ips/apb/apb_event_unit/include +incdir+$(PULP_PATH)/ips/apb/apb_i2c +incdir+$(PULP_PATH)/ips/riscv/include +incdir+$(PULP_PATH)/ips/axi/axi_node
TB_FILELIST = $(PULP_PATH)/tb/tb_filelist.f
TB_INC = +incdir+$(PULP_PATH)/tb
VSIMOPT = -vopt -voptargs=+acc
VSIMCOV = -cm line+cond+fsm+branch+tgl+assert
ERROR = +error+20

# Help target
help:
	@echo "============================================================================================================="
	@echo "! USAGE          --  make <target>                                                                 !"
	@echo "! clean          =>  Clean the earlier log and intermediate files.                                  !"
	@echo "! sv_cmp         =>  Create library and compile the code.                                          !"
	@echo "! run_testX      =>  Compile & run simulation for a specific test.                                  !"
	@echo "! regress        =>  Clean, compile, and run all test cases in batch mode.                          !"
	@echo "! report         =>  Merge coverage reports and generate HTML output.                               !"
	@echo "! fun_cov        =>  Open the functional coverage report in HTML format.                            !"
	@echo "! code_cov       =>  Open the code coverage report in Verdi.                                        !"
	@echo "! view_waveX     =>  View the waveform for a specific test.                                         !"
	@echo "============================================================================================================="

# Compilation target
sv_cmp:
	vcs -l vcs_compile.log +define+FPU -timescale=1ns/1ps -sverilog -ntb_opts uvm -debug_access+all -full64 -kdb -lca -f $(IPS_FILELIST) -f $(TB_FILELIST)  \
	    -P $(FSDB)/novas.tab $(INC) $(TB_INC) $(FSDB)/pli.a $(VSIMCOV) $(ERROR) +define+VERILATOR  

# Generic test run target
run_test:
	./simv -l vcs_test.log +UVM_TESTNAME=$(TESTNAME) +TESTCASE=$(TESTNAME) $(VSIMCOV) -cm_name $(TESTNAME)
	python3 regress_testcase.py vcs_test.log



# Clean target
clean:
	rm -rf simv* csrc* *.tmp *.vpd *.vdb *.key *.log *hdrs.h *.fsdb verdi* coverage_report coverage *.log DVE*
	clear

# Coverage reporting
generate_report:
	urg -dir ./coverage_compile.vdb -dbname merged_dir/merged

report: generate_report
	firefox coverage_report/dashboard.html &

# Open functional coverage report
fun_cov:
	firefox coverage_report/grp*.html &

# Open code coverage report
code_cov:
	verdi -cov &

