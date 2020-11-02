2780 RJE
========

This is a simulator of an IBM 2780 Data Transmission Terminal for 
RJE (Remote Job Entry) use with HASP on IBM operating systems using
the point-to-point BSC (bisync) telecommunications protocol.  The
device submits card decks representing batch jobs to an operating
system and retrieves the printed (and punched) output when the
job is completed.  It attaches to a simulated CPU with an IBM 2703
Communications Controller connected to a leased communication line.
The connection is simulated using a TCP connection to a host at a
port provided by the simulated 2703; the CPU host and the 2780 need
not be the same system.

The simulator was developed to run on an IBM 360 simulator, simh
(see https://github.com/rcornwell/sims) but is intended to be
usable with 370 simulators as well, in particular, with Hercules.
Full 2703 BSC support is required, however, and in Hercules
(as of Nov. 2020 at least), the BSC implementation is incomplete.

Like the 2780, the simulator operates in either transmit or
receive mode.  In transmit mode, it reads a deck from standard
input (in ASCII), converts it to EBCDIC, logs on to HASP, sends
the deck, and logs off.  In receive mode, it logs on to HASP,
receives a print file, converts it to ASCII, and prints it to the
standard output.  If HASP has any punched card output, that can be
diverted to a separate file in raw EBCDIC 80 byte records.

HASP is a fairly general I/O spooling program used on various IBM
360 and 370 OSes but also with MTS (the Michigan Terminal System;
see https://en.wikipedia.org/wiki/Michigan_Terminal_System for a
general description).  The HASP login procedure is slightly
different for IBM OSes and may need to be modified for use with
one of them (the job input deck would be vastly different).  See
the technical references at the end for further information.

The printer is assumed to be 66 lines long.  The BSC printer
protocol includes carriage control characters with each output line,
and the 2780 had carriage control tape which must be emulated
properly for proper vertical spacing of printed output.  There is a
built-in carriage control tape description that may be extended with
straightforward changes to the code.


```

usage: 2780-rje {-r | -s} [-h] [-# <n>] [-p <file>] [{-cc | -d}] <port>

Simulates IBM 2780 RJE device attached to a simulated 2703 using
BSC protocol.  Assumes that HASP provides the RJE service.
Use -s to send the input deck, -r to retrieve printed output.
Assumes 66 line output page length.
Use -p <file> with -r to put punched card output (EBCDIC) into
the specified <file>.

<port> is the RJE TCP listener (usually specified as localhost:2780).

Use -# <n> to provide an explicit job number if multiple simultaneous
jobs are active.

-d and -cc are debug options to view the raw output byte stream and
carriage control in the print stream.

-h prints this help.

```

References:
-----------

*Hardware: [2780]http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/2780/GA27-3005-3-2780_Data_Terminal_Description_Aug71.pdf
*Hardware: [2703]http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/2703/GA27-2703-1_2703_Transmission_Ctl_Component_Descr_May67.pdf

*Protocol: [BSC]http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/datacomm/A27-3004-0_bisyncGenDescr.pdf

*HASP: [360 version]http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/360/hasp/HASP_II_Manual_Jun72.pdf
