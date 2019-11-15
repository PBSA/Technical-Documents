# EXEBlock Knowledge Base : Ticket Purchase 5050

## User Perspective <a id="TicketPurchase5050-UserPerspective"></a>

## Dashboard <a id="TicketPurchase5050-Dashboard"></a>

When the use identifies the desired 5050 draw, clicking on the draw row will cause the side window to open that will display the draw information.

## Side Window <a id="TicketPurchase5050-SideWindow"></a>

Clicking on the “Buy Tickets” button in this side window will display a “quantity” form field.

## Specifying the number of tickets to purchase. <a id="TicketPurchase5050-Specifyingthenumberofticketstopurchase."></a>

Clicking on the “Buy Tickets” button again will initiate the purchase of the ticket\(s\).

* Quantity can not exceed the number of available tickets.

## Terms and Conditions <a id="TicketPurchase5050-TermsandConditions"></a>

Accepting the terms and conditions will finalize the ticket purchase.

## Success Message <a id="TicketPurchase5050-SuccessMessage"></a>

Clicking on the OK button will remove the success message.

## Ticket Purchase Processing Flow. <a id="TicketPurchase5050-TicketPurchaseProcessingFlow."></a>

## BuyTicketAsideAction.js <a id="TicketPurchase5050-BuyTicketAsideAction.js"></a>

captures the quantity, the person buying the ticket and the draw for which the ticket belongs. Then passes this information to the LotteyRepository \(buyLotteryTickets\)

## LotteryRepository.js <a id="TicketPurchase5050-LotteryRepository.js"></a>

the lottery repository accepts the information from the buyTicketAsideAction and implements the transaction builder.

## TransactionBuilder.js <a id="TicketPurchase5050-TransactionBuilder.js"></a>

The transaction builder add additional information to the information that has been submitted from the ticket purchase form.

* add\_type\_operation\('ticket\_purchase', lotteryParams\)
* set\_required\_fees\(\)
* get\_required\_signatures\(pubKeys\)
* add\_signer\(\)

at this point the transaction builder will broadcast the transaction.

* broadcast\(\)
* \_broadcast\(\)
  * this does validations to ensure that the transaction is complete.

## GrapheneApi.js <a id="TicketPurchase5050-GrapheneApi.js"></a>

adds the transaction to the next block.

Apis.instance\(\).network\_api\(\).exec\(\)

