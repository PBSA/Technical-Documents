# EXEBlock Knowledge Base : Differences Between PBSA and Minsk Peerplays

**Revisions**:

February 27th, 2018: Completed analysis of changes to App src Folder, some to Chain src folder. Current progress - about 20% of changes studied.  
February 28th, 2018: Completed analysis of changes to Chain src Folder  
February 28th, 2018: Completed analysis of all changes to date

**Synopsis**:

The purpose of this document is to study and summarize the changes between the PBSA original PeerPlays Client code and the Minsk revised PeerPlays Client code, highlighting any changes and possible difficulties with a merge operation. Comments will indicate a possible reason for the modification in the code base if one is apparent. Noteworthy is that the Minsk teams version of the code is 3 revisions behind current, whereas 

One concerning trend seems to be a pattern of commenting out tests that are failing. For example, the fee distribution test is commented out as 'failed' in the Minsk team. While they may not affect the actual merge, there is the possibility that these represent more significant problems. Changes which may need to be reviewed have been highlighted in red font. 

**Changes** \(as of February 28th, 2018\)

**App**/**Api.cpp - Likely Merge Safe**  
Line 128: Minsk has replaced an asynchronous function with a callback that accepts a VARIANT type. The parameters are similar:**PBSA**: fc::async\( \[capture\_this,this,id,block\_num,trx\_num,trx,callback\]\(\){ **Minsk:** callback\( fc::variant\(transaction\_confirmation{ id, block\_num, trx\_num, trx}\) \);

**App/Application.cpp - Likely Merge Safe, but potentially unwanted in final product**  
Line 166-182: Minsk has commented out all of the seed node URL's provided by pbsa.  
****

**App/Database\_API.cpp - Merge Safe**  
Line 102: Added a number of objects to the class for controlling lotteries.  
Line 173: Minsk removed a command iDump from the content of the if statement here.  
Line 547: Minsk removed a command iDump prior to the declaration of std::map results  
Line 905: Minsk added a section of methods pertaining to lottery assets. \(For example, functions to return sweeps vesting balance, lottery balance, etc\)

**App/Impacted.cpp - Merge Safe**  
Line 250: Minsk added a section pertaining to defining impacted accounts for lottery operations \(for instance, the impacted of a ticket buy is the buyer, the impacted of a ticket sweeps claim is the account, etc\).

**App/libraries/\*.hpp - Merge Safe**  
Various: Corresponding to each function mentioned above, the required function signatures were added to the header files. These are only inserts and should be merge safe.

**Chain/CMakeLists.txt - Merge Safe**  
Line 47: Minsk added lottery\_ops to cmakelist file  
Line 70: Minsk added lottery\_evaluator to cmakelist file

**Chain/Asset\_Evaluator.cpp - Likely Merge Safe**  
Line 24: Balance\_Object.hpp added to includes  
Line 75: Added one additional line break  
Line 95: Commented out core\_fee\_paid -= core\_fee\_paid.value / 2 line. **This may be so that the fee is paid by the winners, not by the person creating the lottery at time of creation.**  
Line 119: Added a branch in the code which tests to see whether it is looking at lottery options, then checks to see if:

* Lottery contains more than 5 tickets \(fail if false\)
* End Date is after current end of the block chain or the current time \(fail if false\)

Inserted two sections copied from the original bitshares, seemingly in response to a known issue and possibly to fix a bug, do\_apply and pay\_fee  
Line 170: Minsk added a branch if lottery options are found which sets the precision equal to zero, configures the lottery options with the creators account id, and creates a Lottery Balance Object  
Line 198: Minsk added a fail condition to see if a user was trying to create a lottery by hand - if the is\_lottery boolean is not set in the options extension, a forced exception is thrown that indicates "the lottery option can not be created by hand"

**Chain/Asset\_Object.cpp - Likely Merge Safe**  
Line 43: Difficult to say... I _believe_ what is happening here is they changed it from being an override of the default graphene implementation of update\_median\_fields to a local implementation \(leaving the original graphene code accessible if needed\). The two functions appear to be identical in function. I see no reason this is not merge safe.  
Line 89: Added a function to get the expiration date for a lottery  
Line 164: Added a whole swath of lottery support functions, getting the asset holders, distributing to benefactors, distributing to the winners, distributing to the sweeps holders, ending the lottery, adjusting the balance of a lottery account and the sweeps vesting account

**Chain/DB\_Balance.cpp - Merge Safe**  
Line 26: Added balance object to includes   
Line 46: Added a function to retrieve a lottery balance from the database  
Line 88: Added a function to modify the lottery balance in the database, as well as the sweeps vesting balance

**Chain/DB\_block.cpp - Needs Review**  
Line 395: Seemingly the only change here was a shift from the open and close brackets of the else statement being on their own lines to being on the same line, likely done by their chosen IDE to beautify the code as the content does not seem to have changed.  
Line 403: Including this for completeness. A diff indicated a change here, but the code is functionally identical. The only change is the elimination of substantial whitespace trailing each line. I consider this to be a false positive.  
Line 536: Deleted an empty line and added one additional check to the routine block creation to check for ending lotteries. Uncommented RNG scheduling algorithm, but added a check to only run it at HARDFORK\_SWEEPS\_TIME. Given this represents a fundamental change to the RNG and a comment states it was commented because it seemed to be broken, I am flagging this as 'Needs Review'.

**Chain/DB\_getter.cpp - Merge Safe**  
Line 30: Included ctime and algorithm standard libraries   
              Added a function to return the RNG seed based on the asset id and the current seed of the random number generator producing a salted hash. Returns a 'result' which is broken up into sub seeds.  
              Added a function to get the winning numbers based on the seeds, the number of 'winners', the number of members, throwing an exception if there are too many or too few winners

**Chain/DB\_Init.cpp - Needs Review**  
Line 52: Included the Lottery Evaluator class.  
Line 181: Registered the ticket purchase evaluator, lottery reward evaluator, lottery end evaluator, and sweeps vesting claim evaluator.  
Line 239: Added the Lottery Balance Index and Sweeps Vesting Index  
Line 790: Modified the intiial genesis block secret from a hash of the secret hash type to the secret hash type. The implications of this change are unknown. It should be merge safe \(since the hash would be functionally identical to the original hash\), however I am flagging this for review due to the nature of the change.

**Chain/db\_maint.cpp - Merge Safe**  
Line 775: Changed output of logging statement \(dlog, looks like a debug log\) to typecast balances to int64. 

**Chain/db\_management.cpp - Merge Safe**  
Line 126: Removed two idump statements.  
Line 185: Added function to handle ending lotteries, both by time and by supply exhaustion. 

**Chain/Hardfork.d/CORE-429.hf  
DOES NOT EXIST IN PBSA FILE**  
  
Seems to track the unix time for HARDFORK\_CORE\_429,  translates to **GMT**: Friday, November 10, 2017 1:20:00 PM. A comment indicates this had to do with a rounding issue when new assets were created.  


**Chain/hardfork.d/SWEEPS.hf  
Does not exist in the PBSA code**

Seems to track the unix time for the hardfork that created the sweeps. Translates to **GMT**: Wednesday, January 3, 2018 5:32:44 PM.

**Chain/Libraries/\*.hpp - Likely Merge Safe with exceptions**  
Various: Corresponding to each function mentioned above, the required function signatures were added to the header files. These are only inserts and should be merge safe - with one exception. The signature for asset\_dynamic\_data\_object has been changed to include a sweeps\_ticket\_sold parameter.  
**Added**: Lotter\_evaluator.hpp header file, Lottery Ops Header File

**Chain/include/graphene/chain/protocol/chain\_parameters.hpp - Needs Review**  
Changed typedef of static variant to include sweeps specific template and parameter extensions. Changes extensions\_type extensions; to parameter\_Extension extensions = sweeps\_parameters\_extension\(\);  
Line 98: Added fc\_reflect for sweeps\_parameters\_extension

**Chain/Include/Graphene/Chain/vesting\_balance\_object.hpp - Needs Review**  
  
Line 188: Changed member\_offsets to const\_mem\_fun  

**Chain/lottery\_evaluator.cpp - Merge Safe  
Added File**

**Chain/Asset\_Ops.cpp - Merge Safe**  
Line 78: Rewrote a switch statement to a branch which checks to see if it is a lottery asset first, then checks for the size of the symbol second to determine if a fee is needed. Despite the change in structure, this really only affects the lottery branch and should be merge safe.  
Line 244: Added Validation function for lottery

**chain/protocol/fee\_schedule.cpp - Merge Safe**  
Line 124: Replaced a commented idump line with ... a commented + line. Possibly trying to use + like a function...? I legitimately don't know what they were trying to achieve here. Might be some kind of arcane operator overloading but it shouldn't break anything.

**Lottery\_Ops.cpp  
Added File**

**Chain/Witness\_Evaluator.cpp - Possibly Merge Safe**  
Line 43: Modified the new\_witness\_object to include a previous secret and next\_secret\_hash.

**plugins/account\_history/account\_history\_plugin.cpp - Merge Safe**  
Line 102: Added a code branch to load the users impacted lotteries into account history.

**plugins/generate\_genesis/generate\_genesis.cpp - Merge Safe**  
Line 113: The return value to whether the value of account\_id.instance is less than 100 \(it was formerly just account\_id.instance\).

**plugins/generate\_uia\_sharedrop\_genesis/generate\_uia\_sharedrop\_genesis.cpp - Merge Safe**  
Line 122: The return value to whether the value of account\_id.instance is less than 100 \(it was formerly just account\_id.instance\).

**wallet/include/graphene/wallet/wallet.hpp - Merge Safe**  
Added some lottery specific functions, list\_assets, get\_lotteries, get\_account\_lotteries, create\_lottery, get\_lottery\_balance, buy ticket

**wallet/wallet.cpp - Likely Merge Safe**  
Line 1437: Added the create lottery function, buy ticket function  
Line 1762: Changed witness\_create\_op initial secret from secret\_hash\_type::hash to enc.result, added get lotteries function, get account lotteries, get lottery balance, get account history, create lottery, buy ticket

**The following are unit tests and as such will not impact a merge. The changes are included for completeness only.**

**tests/common/database\_fixture.cpp**  
Line 79: Commented out a superfluous assignment \(genesis\_state\_initial\_timestamp\)  
Line 159: Added asset index \(asst\_index\)  
Line 170: Added a check for assets to see if they are lottery objects and updates balance tally accordingly, as well as sweeps\_vesting balance, and total balance.  
Line 462: Added witness\_fed\_asset to common\_options.flags  
Line 676: Modified initial secret from secret\_hash\_type::hash of enc.result\(\) to enc.result\(\)

**tests/tests/authority\_tests.cpp**  
Line 466: Commented out a boost check with a note that it 'fails  
Line 478: Commented out a boost check and generate blocks function call.  
Line 536: Commented out a boost check

**tests/tests/basic\_tests.cpp**  
Line 53: Inverted test from '!is\_valid\_name to is\_valid\_name for 'a', aaa, a.a  
Line 99: Inverted test for 'xn--sandmnchen-p8a.de'  
****

**tests/tests/block\_tests.cpp**  
Line 842: Commented out boost check  
Line 863: Commented out boost checks  
Line 1084: Commented out entire boost\_fixture\_test\_case  
****

**tests/tests/fee\_tests.cpp**  
Line 31: Added proposal\_object to includes  
Line 79: Commented out boost\_auto\_test\_case  
Line 266: Commented out boost\_auto\_test\_case for fee distribution, removed a number of tests referring to referral amounts.  
Line 944: Added test case for defaults.hard fork testing,

**Tests/Tess/Lottery\_Tests.cpp  
Added Lottery tests file**

**Tests/Tests/Operation\_tests.cpp**  
Line 648: Commented out push\_tx test  
Line 1587: Commented out Witness Feeds Test  
  
****

**Tests/Tests/operation\_tests2.cpp**  
Line 425: Changed Nathan test account to have a null private key rather than the predefined private key  
Line 463: Adds a loop to cycle through witnesses and generate a block on each  
Line 476: Removed 'nathan key' from generate block  
Line 491: Removed Boost Check of witness\_participation rate  
  
**/tgenesis.json  
File Added to Minsk team**

