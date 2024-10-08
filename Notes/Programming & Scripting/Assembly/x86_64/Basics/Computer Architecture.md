
**Von Neumann Architecture:** 

Consists of these main elements: 

CPU, Memory, and I/O devices

-----------------------------------------


**CPU:** 

Consists of Control Unit (CU), Arithmetic Logic Unit (ALU), and Registers

-----------------------------------------


**Memory:** 

Consists of Cache and RAM


	Cache: 

		Exists to access upcoming instructions quicker than retireving them from RAM. It is much faster than RAM 


Cache Levels: 


| Level | Description                                                                                                    |
|-------|----------------------------------------------------------------------------------------------------------------|
| 1     | Usually in kilobytes, the fastest memory available, located in each CPU core. (Only registers are faster.)<br> |
| 2     | Usually in megabytes, extremely fast (but slower than L1), shared between all CPU cores.<br>                   |
| 3     | Usually in megabytes (larger than L2), faster than RAM but slower than L1/L2. (Not all CPUs use L3.)<br>       |


	RAM: 
		
		Much larger than cache and also much slower. When a program is run, all data & instructions are moved from the storage unit into RAM to be accessed when needed up the CPU. There are 4 segments of RAM. 


RAM Segments: 


| Segment | Description                                                                                                                                                                                          |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Stack   | Has a Last-in First-out (LIFO) design and is fixed in size. Data in it can only be accessed in a specific order by push-ing and pop-ing data.                                                        |
| Heap    | Has a hierarchical design and is therefore much larger and more versatile in storing data, as data can be stored and retrieved in any order. However, this makes the heap slower than the Stack.<br> |
| Data    | Has two parts: Data, which is used to hold variables, and .bss, which is used to hold unassigned variables (i.e., buffer memory for later allocation).<br>                                           |
| Text    | Main assembly instructions are loaded into this segment to be fetched and executed by the CPU.<br>                                                                                                   |





	