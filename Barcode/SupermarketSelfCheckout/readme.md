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

### Requirements

### Functionality

### Limitations

## A database integrated solution

### Requirements

### Functionality

### Limitations

[^1]: https://www.gs1us.org/upcs-barcodes-prefixes/gs1-128
[^2]: Plus and Jumbo supermarkets as found here (https://github.com/Aves-Frog/Flipper/new/main/Barcode/SupermarketSelfCheckout) 
