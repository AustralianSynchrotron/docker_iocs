TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE
#=============================

#=============================
# Build the IOC application

PROD_IOC = generic_ioc
# generic_ioc.dbd will be created and installed
DBD += generic_ioc.dbd

# generic_ioc.dbd will be made up from these files:
generic_ioc_DBD += base.dbd

# Include dbd files from all support applications:
generic_ioc_DBD += aliveSupport.dbd
generic_ioc_DBD += asyn.dbd
generic_ioc_DBD += drvAsynSerialPort.dbd
generic_ioc_DBD += drvAsynIPPort.dbd
generic_ioc_DBD += asSupport.dbd
generic_ioc_DBD += busySupport.dbd
generic_ioc_DBD += calcSupport.dbd
generic_ioc_DBD += sscanSupport.dbd
generic_ioc_DBD += stdSupport.dbd
generic_ioc_DBD += stream.dbd
generic_ioc_DBD += streamSynApps.dbd

# Add all the support libraries needed by this IOC
generic_ioc_LIBS += alive
generic_ioc_LIBS += asyn
generic_ioc_LIBS += autosave
generic_ioc_LIBS += busy
generic_ioc_LIBS += calc
generic_ioc_LIBS += seq
generic_ioc_LIBS += sscan
generic_ioc_LIBS += std
generic_ioc_LIBS += stream
generic_ioc_LIBS += streamSynApps

# generic_ioc_registerRecordDeviceDriver.cpp derives from generic_ioc.dbd
generic_ioc_SRCS += generic_ioc_registerRecordDeviceDriver.cpp

# Build the main IOC entry point on workstation OSs.
generic_ioc_SRCS_DEFAULT += generic_iocMain.cpp
generic_ioc_SRCS_vxWorks += -nil-

# Add support from base/src/vxWorks if needed
#generic_ioc_OBJS_vxWorks += $(EPICS_BASE_BIN)/vxComLibrary

# Finally link to the EPICS Base libraries
generic_ioc_LIBS += $(EPICS_BASE_IOC_LIBS)

#===========================

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

