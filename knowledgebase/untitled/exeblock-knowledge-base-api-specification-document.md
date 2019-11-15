# EXEBlock Knowledge Base : API Specification Document

50/50 eXeBlock

#### revision history <a id="APISpecificationDocument-revisionhistory"></a>

| version | Dare | Author | Description |
| :--- | :--- | :--- | :--- |
| 0.1 | 07 Feb 2018  | [Jeffrey Maxwell](https://peerplays.atlassian.net/wiki/people/5a7aea885c3dd85c7e58e08e?ref=confluence) | initial draft |
|  |  |  |  |
|  |  |  |  |

This is a living document and is currently under construction and in draft mode. At such time that the document is peer reviewed and accepted the version number will go from 0.x to 1.0.

## Introduction <a id="APISpecificationDocument-Introduction"></a>

eXeBlock Technology Corp. \(CSE:XBLK\) is a designer of custom, state-of-the-art blockchain based software applications that provide profitable, secure and efficient solutions to businesses and markets globally.

This document is meant to demonstrate the available functionality and proper use of the eXeBlock 50/50 API that sits on top of the Peerplays blockchain.

The eXeBlock 50/50 API enables the management of several types of lottery type games of chance, Not just 50/50 style games. All of the lottery type game are conducted in an open, auditable  and verifiable way that is inherent in the blockchain technology.

## Terms and Conditions <a id="APISpecificationDocument-TermsandConditions"></a>

Copyright \(c\) 2017, Peerplays Blockchain Standards Association

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files \(the "Software"\), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## API Operations <a id="APISpecificationDocument-APIOperations"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Service</th>
      <th style="text-align:left">Operations</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BuyTicketService</td>
      <td style="text-align:left">
        <p>buyTickets</p>
        <p>formValues</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ChainService</td>
      <td style="text-align:left">
        <p>getDateOfBlock</p>
        <p>getDateOfBlockSynch</p>
        <p>getCurrentLotterySupply</p>
        <p>getSoldTickets</p>
        <p>getLotterySymbol</p>
        <p>getLotterPrice</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">CryptoService</td>
      <td style="text-align:left">formEncryption</td>
    </tr>
    <tr>
      <td style="text-align:left">KeyGeneratorService</td>
      <td style="text-align:left">generateKeys</td>
    </tr>
    <tr>
      <td style="text-align:left">LotteryService</td>
      <td style="text-align:left">
        <p>createNewLottery</p>
        <p>formValues</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SignInService</td>
      <td style="text-align:left">
        <p>checkLoginAccount</p>
        <p>systemLogin</p>
        <p>systemLoginByPrivateKey</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">StorageService</td>
      <td style="text-align:left">
        <p>get</p>
        <p>set</p>
        <p>remove</p>
        <p>getEncryption</p>
        <p>setEncryption</p>
        <p>resetEncryption</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ValidationService</td>
      <td style="text-align:left">
        <p>validateLotteryName</p>
        <p>validateLotteryTicketAmount</p>
        <p>validateLotteryTicketPrice</p>
        <p>validatePercents</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WalletService</td>
      <td style="text-align:left">
        <p>createWalletAccount</p>
        <p>createWalletByPrivateKey</p>
        <p>resetDBTables</p>
        <p>saveKeysToDB</p>
        <p>saveWalletToDB</p>
        <p>setCurrentWalletNameToDB</p>
        <p>getDBWallet</p>
        <p>getDBKeys</p>
        <p>checkEnableWallet</p>
      </td>
    </tr>
  </tbody>
</table>## Methods <a id="APISpecificationDocument-Methods"></a>

### BuyTicketService <a id="APISpecificationDocument-BuyTicketService"></a>

#### buyTickets <a id="APISpecificationDocument-buyTickets"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### formValues <a id="APISpecificationDocument-formValues"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

### ChainService <a id="APISpecificationDocument-ChainService"></a>

#### getDateOfBlock <a id="APISpecificationDocument-getDateOfBlock"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getDateOfBlockSynch <a id="APISpecificationDocument-getDateOfBlockSynch"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getCurrentLotterySupply <a id="APISpecificationDocument-getCurrentLotterySupply"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getSoldTickets <a id="APISpecificationDocument-getSoldTickets"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getLotterySymbol <a id="APISpecificationDocument-getLotterySymbol"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getLotterPrice <a id="APISpecificationDocument-getLotterPrice"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

### CryptoService <a id="APISpecificationDocument-CryptoService"></a>

#### formEncryption <a id="APISpecificationDocument-formEncryption"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

### KeyGeneratorService <a id="APISpecificationDocument-KeyGeneratorService"></a>

#### generateKeys <a id="APISpecificationDocument-generateKeys"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

### LotteryService <a id="APISpecificationDocument-LotteryService"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### SignInService <a id="APISpecificationDocument-SignInService"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### StorageService <a id="APISpecificationDocument-StorageService"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### ValidationService <a id="APISpecificationDocument-ValidationService"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
        <p>Parameter</p>
        <p>Response</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### WalletService <a id="APISpecificationDocument-WalletService"></a>

#### createWalletAccount <a id="APISpecificationDocument-createWalletAccount"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### createWalletByPrivateKey <a id="APISpecificationDocument-createWalletByPrivateKey"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### resetDBTables <a id="APISpecificationDocument-resetDBTables"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### saveKeysToDB <a id="APISpecificationDocument-saveKeysToDB"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### saveWalletToDB <a id="APISpecificationDocument-saveWalletToDB"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### setCurrentWalletNameToDB <a id="APISpecificationDocument-setCurrentWalletNameToDB"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getDBWallet <a id="APISpecificationDocument-getDBWallet"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### getDBKeys <a id="APISpecificationDocument-getDBKeys"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

#### checkEnableWallet <a id="APISpecificationDocument-checkEnableWallet"></a>

Parameter

|  |  |
| :--- | :--- |
|  |  |

Response

|  |  |
| :--- | :--- |
|  |  |

## Screens <a id="APISpecificationDocument-Screens"></a>

### Login <a id="APISpecificationDocument-Login"></a>

Provides a way for an user who already has a Peerplays wallet to access the 50/50 DaPP.

#### checkLoginAccount <a id="APISpecificationDocument-checkLoginAccount.1"></a>

#### systemLogin <a id="APISpecificationDocument-systemLogin.1"></a>

#### systemLoginByPrivateKey <a id="APISpecificationDocument-systemLoginByPrivateKey.1"></a>

### Sign up <a id="APISpecificationDocument-Signup"></a>

Provide a way for a potential user to create a Peerplays account/wallet which can then be used to access Peerplays/eXeBlock's selection of DaPPs.

####  createWalletAccount <a id="APISpecificationDocument-createWalletAccount.1"></a>

#### createWalletByPrivateKey <a id="APISpecificationDocument-createWalletByPrivateKey.1"></a>

#### resetDBTables <a id="APISpecificationDocument-resetDBTables.1"></a>

#### saveKeysToDB <a id="APISpecificationDocument-saveKeysToDB.1"></a>

#### saveWalletToDB <a id="APISpecificationDocument-saveWalletToDB.1"></a>

#### setCurrentWalletNameToDB <a id="APISpecificationDocument-setCurrentWalletNameToDB.1"></a>

#### getDBWallet <a id="APISpecificationDocument-getDBWallet.1"></a>

#### getDBKeys <a id="APISpecificationDocument-getDBKeys.1"></a>

#### checkEnableWallet <a id="APISpecificationDocument-checkEnableWallet.1"></a>

### Create New Draw <a id="APISpecificationDocument-CreateNewDraw"></a>

Provides a way for an authenticated user to create a draw/lottery. This requires the user to enter draw specific information to allow the DaPP to properly administer the draw.

#### createNewLottery <a id="APISpecificationDocument-createNewLottery.1"></a>

#### formValues <a id="APISpecificationDocument-formValues.2"></a>

### My Draws <a id="APISpecificationDocument-MyDraws"></a>

This displays a summary of the draw that were created by the authenticated user.

#### validateLotteryName <a id="APISpecificationDocument-validateLotteryName.1"></a>

#### validateLotteryTicketAmount <a id="APISpecificationDocument-validateLotteryTicketAmount.1"></a>

#### validateLotteryTicketPrice <a id="APISpecificationDocument-validateLotteryTicketPrice.1"></a>

#### validatePercents <a id="APISpecificationDocument-validatePercents.1"></a>

### History <a id="APISpecificationDocument-History"></a>

This provides a list of all transaction completed by the authenticated user

### Terms and Conditions <a id="APISpecificationDocument-TermsandConditions.1"></a>

This displays terms of use as it pertains to the Draw creation and the purchase of tickets.

### Draw Resolution <a id="APISpecificationDocument-DrawResolution"></a>

This section describes how the Draw resolution is processed based on the conditions and information entered at the time the draw was created.

During the block generation process a witness with evaluate the lotteries to see if there is a lottery that have ended.

* The end point of a lottery is determined when a user creates the lottery. At lottery can either end when all of the tickets have been sold, or when a specific time and date has been reached.

When the witness discovers a lottery that has ended. The witness will generate a "secret" \(secret1\) and a hash value \(hash1\). This secret and hash are then published to the block. When the next block is generated, the witness will again publish the original "Secret" \(secret1\) and generate a new hash value \(hash2\).

At this point the new Hash value and the asset ID of the lottery that just ended are used by a hash function to generate a "seed" value. This generated seed value is then used by a random number generator to select the winner of the lottery.

Why is the Draw resolution process spread out over more than 1 block?

The reason of this is to ensure that the value output by the random number generator can not be altered in any way.

For example: If the seed value used by the RNG were to be derived from the asset ID of the lottery, and the hash value from the first block \(hash1\). It is theoretically possible for a witness to evaluate the winner generated by the RNG, and if the witness did not approve of the result, the witness could rearrange the transactions in the block, regenerate the hash value, which in turn would alter the seed value for the RNG and ultimately change the winner of the lottery.

By spreading the draw resolution over 2 blocks in the chain, We are taking advantage of the feature of the block chain that makes it secure and renders the data it contains as immutable.

\(I am not sure which of these diagrams is correct\)

