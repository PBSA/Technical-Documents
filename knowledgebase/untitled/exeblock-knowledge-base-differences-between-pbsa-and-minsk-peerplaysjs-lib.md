# EXEBlock Knowledge Base : Differences between PBSA and Minsk PeerPlaysJS-Lib

**Revisions:**

February 28th, 2018: Created Document

**Synopsis:** 

This document represents the changes between the original PBSA Codebase and the Minsk codebase. Any changes which may not merge or may need to be reviewed are highlighted in red.

**Changes:** 

**Chain/src/ChainTypes.js - Merge Safe**  
Line 49: Added lottery balance and sweeps vesting balance to ChainTypes object  
Line 110: Added asset\_dividend\_distribution, tournament payout, tournament leave, ticket purchase, lottery reward, lottery end, and sweeps vesting claim to Chain Type Operations.

**Serializer/src/Operations.js - Merge safe with note RE: Line 894**  
Line 107: Added the lottery asset to the operation fee parameters, seems to be an account reference \(64 bit unsigned int\)  
Line 299: Added game\_move\_operation\_fee\_parameters, asset\_update\_dividend\_operation\_fee\_parameters, asset\_divident\_distribution\_operation\_Fee\_parameters, tournament payout, tournament leave, ticket purchase, lottery reward, lottery end, sweeps vesting claim  
               Added Fees to Parameters.  
Line 653: Added lottery asset options to extensions object  
Line 857: Added Sweeps Parameter Extension serializer  
Line 894: Removed Future Extensions, Added Sweeps Parameter Extensions - Although this represents a changed line, I _believe_ future\_extensions to only be a placeholder variable for data down the road, and as such this likely won't be a breaking merge.  
Line 1191: Added asset\_divident\_distribution serializer, payout\_type enumerator, tournament\_payout serializer, tournament\_leave serializer, ticket\_purchase serializer, lottery\_reward serializer, lottery end serializer, sweeps vesting claim serializer. 

