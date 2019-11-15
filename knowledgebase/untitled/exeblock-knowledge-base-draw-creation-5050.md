# EXEBlock Knowledge Base : Draw Creation 5050

This document is meant to describe the process of creating a lottery within the eXeBlock 5050 DaPP. Both from a users perspective as well as outlining the process that go on behind the scenes.

## User Perspective. <a id="DrawCreation5050-UserPerspective."></a>

## Draw Creation Form <a id="DrawCreation5050-DrawCreationForm"></a>

The draw creation form is accessed by successfully authenticating into the 5050 application.

When creating a new draw the user must complete the form correctly. Here are the fields on the form and the validations for each field:

* Draw name
  * The Draw creation naming rules rules are found libraries/chain/protocol/asset\_ops.cpp:35 - is\_valid\_symbol\(\)
  * Required field
  * Must be unique to the blockchain. Open or closed draws.
    * The input of a draw name is used as an asset name for the user-issued asset on the blockchain. Therefore, blockchain protocol prevents the creation of an asset with the same name as a previously created asset.
  * Must be between 3 and 16 characters in length.
  * Draw name must be entered in “UPPERCASE” characters
  * The first three characters must not be 'BIT'
  * Numbers are allowed, but can not be the first or last character in the name
  * A single dot \(“.”\)  is allowed but can not be the first or last character  
* Total tickets
  * Required field
  * must be a number
  * maximum umber of tickets is 1 Quadrillion. 1,000,000,000,000,000
  * minimum number of tickets is 5
* Resolution conditions
  * There are three options,”all tckets sold”, “on resolution date”, “either one”
  * Defaults to “all tickets sold”
* Draw resolution date and time
  * Only needed when resolution conditions is set to “on resolution date”, “either one”.
  * Date selected from a date picker
  * Time is selected from a time picker in 1 hours increments.
* Ticket price
  * Required field
  * minimum ticket price is .00001 PPY
* Benefactor user
  * Required field
  * The user id specified must have valid Peerplays wallet.
* Benefactor percentage
  * Required field
  * Minimum percentage 0.01%
* Winner percentage
  * Required field
  * Minimum percentage 0.01%

_**There are 2 points where validation is done. At the time that the form field is receiving data, also at the time the terms and conditions are accepted. This happens specifically for the uniqueness of the draw name. I assume this is due to the technology and the check for uniqueness is done when the transaction is being built. Is it possible to do the uniqueness check when the form field is receiving data?**_

_**The maximum number of tickets as it is now 1 quadrillion, this will cause performance issues during the draw resolution process. \(see trello \(XS\) 5050-26 Maximum ticket amount for one draw\)**_

## Term and Conditions <a id="DrawCreation5050-TermandConditions"></a>

The terms and conditions must be accepted before the draw will be created. If the person creates the draw wishes to view the terms and conditions, the details can be accessed by clicking on the terms and conditions link. After the link is clicked a new window will be spawned. This will keep the user from loosing the draw information as entered.

The terms and condition details page can also be accessed by using the navigation bar on the left side of the screen.

## Draw Creation Success <a id="DrawCreation5050-DrawCreationSuccess"></a>

If all information is entered correctly and the draw creation process completes successfully, the user is presented with a success message.

## Draw Creation Processing Flow <a id="DrawCreation5050-DrawCreationProcessingFlow"></a>

## LotteryActions.js <a id="DrawCreation5050-LotteryActions.js"></a>

project: 5050 developmentclass LotteryActions

* passes the lottery parameters as submitted from the form and the state to the lotteryService. LotteryService.createNewLottery\(lotteryParams, state\)
* after the newly created lottery is added to the blockchain.
  * Hide the terms and conditions modal.
  * Display the successful draw creation modal.
  * Reset the values from the draw create form.

## LotteryService <a id="DrawCreation5050-LotteryService"></a>

project: 5050 developmentclass LotteryService

* formats the submitted data and passes to the LotteryRepository

## LotteryRepository <a id="DrawCreation5050-LotteryRepository"></a>

project: 5050 developmentclass LotteryRepository

* feeds the lottery Parameters into the transaction builder
* create an asset with the lottery Parameters
* set the required fees
* get and add the required signatures to the transaction
* broadcast the transaction

## TransactionBuilder <a id="DrawCreation5050-TransactionBuilder"></a>

project: Peerplaysjs-libclass TransactionBuilder

* the transaction builder will broadcast the populated transaction with callback.

## GrapheneApi.js <a id="DrawCreation5050-GrapheneApi.js"></a>

project: peerplysjs-ws-masterApis.instance\(\).network\_api\(\).exec\( "broadcast\_transaction\_with\_callback", \[ function\(res\) { return resolve\(res\); } ,tr\_objethect\]\).then\(function\(\)

## Blockchain <a id="DrawCreation5050-Blockchain"></a>

project: peerplays-master

If the draw name as entered, is a duplicate to a previous draw, either open or closed, an error will occur and the message will be passed back through the system to the Draw Creation form, indicating that the draw name as already been used.

If there are no errors:

* the witness will add the transaction to the next block.

