# Supermarket Self Checkout
> [!CAUTION]
> Tempering with self checkout systems or otherwise interfering with standard and intended operation of security devices is illegal.
> I do not condone tempering with systems you do not have explicit permissions for.

## How the barcode to exit system works
Note: This may differ depending on your region, store, or other implemented (security) features.

The barcodes I have encountered thus far work on the GS1-128 or EAN-128 codex, which allows for upto 48 characters of data within the barcode [^1]. 
The receipts I found used barcodes ranging from 14 to 20 digits [^2].
Which can be split up in different chunks of data.
Typically these chunks are a variation of the following: first digit indicating the receipt can be used, the code of the shop, the terminal/counter, the transaction number, the date.
Note that the usage or position of these chunks seems to be flexible.

As such a barcode such as 61234001999903072025 ([Receipt digit: 6] [Shop code: 1234] [Terminal: 001] [Transaction: 9999] [Date MMDDYYYY: 03072025]) would be a valid code.
Note that the receipts I used to decode these barcodes had clear indicators if not literal labels on it describing what each chunk meant.

From my findings I feel it is within reason to assume the exit gate scanunit checks the receipts on most chunks to be possible, this could easily be done in a lightweight lookup table.
But in order to speed up the process and limit bandwidth ignore some chunks, such as the transaction number (which btw simply loops and does not reset daily).
One of the leading reasons for this conclusion is the differences in complexity and thus cost comparing a database integrated system with a simpler solution, see below.

## A simple gate scaning solution
While there are probably as many simple solutions to this scanning unit as there are roads to Rome, I will outline two solutions I feel would be the most logical option for a technician to implement.
From a pure simplicity (and thus cost saving) standpoint I would say a Hardcoded solution or a Semi dynamic solution would be the most plausible choice.

### Requirements
For both the Hardcoded and Semi dynamic solution a simple chipboard (or even docker VM running on an admin pc) would provide more than enough power to run smoothly.
Both solutions would have file sizes in the bytes, as most data could be simplified.
For software both would need to run a simple script comparing int32 integer chunks next to a scan unit (these might even have a decoding chip build in, meaning the processing/checkings unit would only need int32 capabilities).
It really doesn't get much simpler in software.

### Functionality
Given the Hardcoded and Semi dynamic solutions function slightly differently I will explain them individually.

**Hardcoded solution:**
Within the code that checks the validity of the barcode we hardcode (define them within the code, to avoid needing to look them up) as many chunks as possible.
It stands to reason that the first digit, the static Receipt digit, doesn't need to change, so this could be hardcoded.
Next to that we can also assume that units get placed in the shop by a technician who has access to the unit's software, as such we can hardcode the shop code as well.
Other chunks that needn't change can be stored as well.

We now have (as for the previous example): 6-1234-XXX-XXXX-XXXXXXXXX

Now the Terminal number is the main argument that differs the Hardcoded solution from the Semi dynamic solution.
Which also for the Hardcoded solution has a few options.
The most logical option would be to have one variable (int32) reserved for storing the amount of terminals (and thus the valid values).
Example: for 5 counters -> static var 4 (keep in mind that integers start at 0).
Now actually starting the var at 1, i.e. flagging number 0 (or 000 as found in actual barcodes) as fraudulent would be a valid method to check this chunks validity aside from values above specified.

One downside of this method is that counters that are taken out of service cannot be flagged as invalid without overhauling the software.
Obviously this could be solved by adding a second code snipped that specifies and checks out of service counters (that are manually coded in).

Just for fun we could theorize other options that stay hardcoded. Such we could make a script that specifically defines each counter option. 
Just for fun and for the sake of ruining code efficiency we can even convert this into an if-statement list. 
Or even worse, just so the poor lonely CPU Hertz have something to do, create a class function that seeks to define the counter number and links to hardcoded class functions that all have the same function with their respective number.
This thought experiment is going off the rails, but it may show how optimized a Hardcoded solution may.

From here we have: 6-1234-001-XXXX-XXXXXXXX

Lastly assuming the scan unit has a time crystal (as do most motherboards) the time can be gathered locally, else using network connectivity the current data can be gathered.
Fun fact, even if no network connection or crystal is present the unit would still be able to note the date by simply counting upwards from the point of installation.
This is because the unit doesn't need to be precise in the hours. In theory a unit with a daily timing error or 2 minutes per day could be service free for 270 days. 

<sub>Assuming the unit is installed at midnight and the shop opens at 9am: 2minutes over 9 hours (to assume not one second of error during the open hours), 9hrs/2min -> 540/2 = 270. I.e. nearly one year </sub>

This gives us: 6-1234-001-XXXX-03072025

The only thing missing is the transaction number, but we can simply ignore this chunk. If you would really like to you could define specific numbers to check for errors such as 0000, 9999, 1234, etc.
But realistically there isn't much point.
As noted before these numbers simply loop whenever the highest number fitting (9999 or other defined) is hit at the counter.
This is where the Database solution shines, with it's database connection it can check in reasonable time whether this specific transaction is valid, or even if this transaction has already exited the gate.

### Limitations

## A database integrated solution

### Requirements

### Functionality

### Limitations

[^1]: https://www.gs1us.org/upcs-barcodes-prefixes/gs1-128
[^2]: Plus and Jumbo supermarkets as found here (https://github.com/Aves-Frog/Flipper/new/main/Barcode/SupermarketSelfCheckout) 
