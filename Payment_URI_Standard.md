# Payment URI Standard version 0.0.1


## 1 Introduction
Payment URI is a Uniform Resources Identifier, which connects to the user’s installed Payment App. The resource identifier scheme is “payment”.

The resource identifier can be put in anchor elements on web pages, which will create clickable payment links. When clicking the link, the Payment App will start, and the user can make the payment. The link can be coded like this:

The link given in the resource identifier’s request argument points to a Payment Request file. This file is read using the HTTP Get method. The Payment App can use the details in this file to draw a cheque according to the payment requested and pay with it.

On a web page selling several types of items, the payment link can point to different Payment Request files, one for each of the items.
