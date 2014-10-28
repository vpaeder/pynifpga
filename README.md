pynifpga
========

Python wrapper for the National Instruments FPGA C interface.

This wrapper is based on boost-python and is tested to run on Windows 7 with NI-SCOPE 4.1.
I used Microsoft Visual Studio Express 2013 to compile (project files included).

The wrapper is made to wrap as closely as possible the functions provided by the NI-FPGA C interface. The project is made of a C-to-C++ stage followed by a C++-to-Python stage (I find it more readable in this way).

Architecture
============

Here is a brief list of the functions and other things provided by pynifpga. An extended description can be found in the [NI-FPGA C Interface help files](http://zone.ni.com/reference/en-XX/help/372928G-01/), under *API Reference*.
The *enum* structures give a convenient way to access open/close/run attributes and IRQ assignments.
The functions that are not supposed to return any value will return True if they succeeded, and False otherwise.

**pynifpga**
*Error handling functions*
* int GetLastStatus()
* str GetErrorDescription(int status)

*Required functions*
* bool Initialize()
* bool Finalize()

*Session functions*
* bool Open(str bitfile, str signature, str resource, int attribute)
* bool Close(int attribute)

*Method functions*
* bool Run(int attribute)
* bool Abort()
* bool Reset()
* bool Download()

*Read functions*
* bool ReadBool(int indicator)
* int ReadI8(int indicator)
* int ReadU8(int indicator)
* int ReadI16(int indicator)
* int ReadU16(int indicator)
* int ReadI32(int indicator)
* int ReadU32(int indicator)
* int ReadI64(int indicator)
* int ReadU64(int indicator)

*Write functions*
* bool WriteBool(int control, bool value)
* bool WriteI8(int control, int value)
* bool WriteU8(int control, int value)
* bool WriteI16(int control, int value)
* bool WriteU16(int control, int value)
* bool WriteI32(int control, int value)
* bool WriteU32(int control, int value)
* bool WriteI64(int control, int value)
* bool WriteU64(int control, int value)

*ReadArray functions*
* list ReadArrayBool(int indicator, int size)
* list ReadArrayI8(int indicator, int size)
* list ReadArrayU8(int indicator, int size)
* list ReadArrayI16(int indicator, int size)
* list ReadArrayU16(int indicator, int size)
* list ReadArrayI32(int indicator, int size)
* list ReadArrayU32(int indicator, int size)
* list ReadArrayI64(int indicator, int size)
* list ReadArrayU64(int indicator, int size)

*WriteArray functions*
* bool WriteArrayBool(int control, list values)
* bool WriteArrayI8(int control, list values)
* bool WriteArrayU8(int control, list values)
* bool WriteArrayI16(int control, list values)
* bool WriteArrayU16(int control, list values)
* bool WriteArrayI32(int control, list values)
* bool WriteArrayU32(int control, list values)
* bool WriteArrayI64(int control, list values)
* bool WriteArrayU64(int control, list values)

*Interrupt functions*
* PyObject* ReserveIrqContext()
* bool UnreserveIrqContext(PyObject* context)
* bool WaitOnIrqs(PyObject* context, int irqs, int timeout)
* bool AcknowledgeIrqs(int irqs)

*FIFO Method functions*
* bool ConfigureFifo(int fifo, int requestedDepth)
* bool StartFifo(int fifo)
* bool StopFifo(int fifo)
* bool ReleaseFifoElements(int fifo, int elements)
* int GetPeerToPeerFifoEndpoint(int fifo)

*Read FIFO functions*
* list ReadFifoBool(int fifo, int numberOfElements, int timeout)
* list ReadFifoI8(int fifo, int numberOfElements, int timeout)
* list ReadFifoU8(int fifo, int numberOfElements, int timeout)
* list ReadFifoI16(int fifo, int numberOfElements, int timeout)
* list ReadFifoU16(int fifo, int numberOfElements, int timeout)
* list ReadFifoI32(int fifo, int numberOfElements, int timeout)
* list ReadFifoU32(int fifo, int numberOfElements, int timeout)
* list ReadFifoI64(int fifo, int numberOfElements, int timeout)
* list ReadFifoU64(int fifo, int numberOfElements, int timeout)

*Write FIFO functions*
* int WriteFifoBool(int fifo, list data, int timeout)
* int WriteFifoI8(int fifo, list data, int timeout)
* int WriteFifoU8(int fifo, list data, int timeout)
* int WriteFifoI16(int fifo, list data, int timeout)
* int WriteFifoU16(int fifo, list data, int timeout)
* int WriteFifoI32(int fifo, list data, int timeout)
* int WriteFifoU32(int fifo, list data, int timeout)
* int WriteFifoI64(int fifo, list data, int timeout)
* int WriteFifoU64(int fifo, list data, int timeout)

*Acquire FIFO Read Elements functions*
* list AcquireFifoReadElementsBool(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsI8(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsU8(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsI16(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsU16(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsI32(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsU32(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsI64(int fifo, int elementsRequested, int timeout)
* list AcquireFifoReadElementsU64(int fifo, int elementsRequested, int timeout)

*Acquire FIFO Write Elements functions*
* bool AcquireFifoWriteElementsBool(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsI8(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsU8(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsI16(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsU16(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsI32(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsU32(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsI64(int fifo, list elements, int elementsRequested, int timeout)
* bool AcquireFifoWriteElementsU64(int fifo, list elements, int elementsRequested, int timeout)

*OpenAttribute enum*
* NoRun

*CloseAttribute enum*
* NoResetIfLastSession

*RunAttribute enum*
* WaitUntilDone

*Irq enum*
* values from 0 to 31 corresponding to IRQ 0 to 31

Usage example
=============

	# FPGA settings
	res = "RIO0" # instrument address (resource name, for example RIO0)
	fname = "NiFpga_niScopeEXP2PInterleavedDataFPGA.lvbitx" # Bitfile name
	sig = "SignatureRegister node value found in LVBITX file"
	numPoints = 2000 # number of points to read
	timeout = 1500 # operation timeout in msec
	
	# loads FPGA drivers
	from pynifpga import pynifpga
	fpga = pynifpga()
	
	# loads file into the FPGA
	fpga.Open(fname, sig, res, 0)
	
	# writes some value in one register on the FPGA
	# the control number can be read from the Offset property of a Register node in the LVBITX file.
	fpga.WriteU32(0,207)
	
	# reads data from a FPGA DMA-to-host channel
	data = array(fpga.ReadFifoI16(1, numPoints, timeout))
	
	# add here what you want to do with the data
	
	fpga.Close(0) # closes FPGA connection
	fpga.Finalize() # cleans up FPGA driver
