# EXEBlock Knowledge Base : Copy of Functional Specifications- 50/50 Ticket Purchase

## Purpose <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-Purpose"></a>

The purpose of this document is to outline the steps required by a user to purchase a ticket for a valid lottery within the Peerplays / eXeBlock 50/50 DaPP.

## Scope <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-Scope"></a>

The functional requirement listed in this document will be limited to the ticket purchase portion of the eXeBlock 50/50 DaPP. This will outline the required steps that will be performed by the by the user to purchase a ticket for a valid lottery. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## Background <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-Background"></a>

The eXeBlock 50/50 DaPP will be build to utilize the Peerplays blockchain platform. The eXeBlock 50/50 DaPP will provide an easily accessible way for organization to hold 50/50 draws as a way to raise funds, in a provably fair and open platform that can be monitored by all participants and/or interested parties.

## Process Overview <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-ProcessOverview"></a>

The user will be able to purchase a ticket from an existing lottery within the eXeBlock 50/50 DaPP, The user will select a lottery from the 50/50 Dashboard, specify the quantity of tickets to be purchased, and submit the request. The crypto funds will be taken from the users wallet and the user will be credited with the ticket for the chosen lottery.

## Context <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-Context"></a>

In order for user to purchase a ticket within the eXeBlock 50/50 DaPP, they must have a valid user account and be authenticated into the system. The process of creating a valid lottery ensures accurate and complete information gathering. At which point the eXeBlock 50/50 DaPP will automatically manage the lottery until the point that the lottery is resolved. Any winnings awarded as a result of the purchased ticket will automatically be awarded.

## Flow Diagram <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-FlowDiagram"></a>

## Steps to Purchase a Ticket <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-StepstoPurchaseaTicket"></a>

##  <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-"></a>

The ticket purchase screen is accessed by successfully authenticating into the 5050 application, selecting an open lottery from the dashboard to initiate the side window displaying the draw details and the "Buy Tickets" button.

Clicking on the "Buy Tickets" button will reveal the "Quantity" field where the user can specify the number of tickets to purchase. Clicking on the Buy Tickets button again will award the user the purchased tickets providing the user has enough currency in their wallet.

* Quantity
  * Is a required field
  * Must be a whole number,
  * must be less than or equal to the remaining tickets available for purchase
  * user must have the fund available to purchase the specified tickets

## Terms and Conditions <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-TermsandConditions"></a>

The terms and conditions must be accepted before the ticket\(s\) purchase will complete. 

The terms and condition details page can also be accessed by using the navigation bar on the left side of the screen.

## Ticket Purchase Success <a id="CopyofFunctionalSpecifications-50/50TicketPurchase-TicketPurchaseSuccess"></a>

If all information is entered correctly and the ticket purchase process completes successfully, the user is presented with a success message.

