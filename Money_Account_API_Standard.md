# Money Account API Standard version 0.0.1


## 1 Introduction
Money Account API act as a communication channel between the participants in the payment network. The API should be implemented on any server participating in Bitcoin Cheque payments, including cheque issuers and webshops.

The API at a bank can offer support for Payment Apps to draw Bitcoin Cheques and for the receivers of the verify and claim it. A webshop and use the API to receive cheques as payments. Moreover, the API offers support for users to withdraw their Bitcoins from their user account by requesting a Bitcoin transaction.

The API is accessed by using HTTP Get and Post requests. The rationale for using HTTP is to make it easy for any web site to implement it, for example, by creating PHP scripts. Otherwise, the implementation could require changes in server configuration, which may not be possible at many web hosts.
