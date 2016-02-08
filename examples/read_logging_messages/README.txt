===========================================
 Example Code -- Read Debugging Messages
===========================================

Example Description
===================
This example shows how to print debugging messages from the internal queue. It 
uses the "Read One Logged Message.vi" subVI in order to receive these messages
from the internal queue. This subVI has following pins:
  - Inputs
    - Debugging table in: debugging table where we want to append the new 
      sample if it exists.
    - Clear table?: indicates to clear the table. It is disabled by default.
    - Max number of rows: indicates a new maximum number of rows in the table.
      The max number of rows is 512 by default.
    - error in (no error): error input
  - Outputs
    - Debugging table out: "debugging table in" with a new message appended if 
	  it existed.
    - Print table?: indicates whether a new data was added to the table or the
      table has been cleared, so the table needs to be printed.
    - error out: error standard output.

VIs Description
===============
Since this subVI just reads one menssage per execution, we need to include it 
in a loop and use a shift register in order to keep the previous messages. 
After that, it appends the new read message at the beginning of "Debugging
table in". However, if no new messages can been read, the table should not be
updated. For performance reasons, the subVI returns a flag called "Print
table?" which indicates when the table control should be printed. So, a case
structure is used with this flag in order to update the Debugging table.
   
Running LabVIEW Example
=======================
This VI may need the modification of the Filter level to "Debugging" using the
"Set Configuration Parameters.vi" subVI as well as running a LabVIEW 
application which uses the RTI Connext DDS Toolkit for LabVIEW 1.3.2 and above.
