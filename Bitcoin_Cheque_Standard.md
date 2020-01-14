# Bitcoin Cheque Standard version 0.0.1


## 1 Introduction
This document defines the specification for a Payment Cheque. The Payment Cheque is a note promising that its issuer will pay a certain amount to a receiver at some predefined conditions. The Payment Cheque is constructed to be flexible and can hold any currencies. The cheque can be sent using Payment Protocol and it can be saved to a file and sent as an attachment in an e-mail.


### 1.1 Terminology
During this specification the following terminology has been utilized.

| Expression     | Meaning
|----------------|---------
| Claim          | 
| Collect        | 
| Monney Address | 


## 2 Data fields
This chapter defines the information in a Payment Cheque data fields. 

The cheque's information is organized in data structures, which again can contains other data structures and data fields.

Each data field and data structure are listed in tables in the following sections. The tables have the following columns with information:
- **Field Name** - When encoding the cheque in JSON, the Field Name shall be used as the JSON name.
- **Required** - This field indicates is this field is mandatory or optional. All mandatory fields must be be included in a Payment Cheque. 
-  **Data Type** - This column indicates the field's data type. Some data typs are structures. All data types are defined in this specification.
-  **Size** - This field indicates the size of the field. Interpretation of size number is given at it's data type definition. For sizes set to n/a, the actual size is determined by a structure or defined by the field's data type.
-  **Description** - This column contains a short description of the field/structure.


### 2.1 Data structures
### 2.1.1 Root structure
The Payment Cheque has the following data fields.

| Field Name    | Required  | Data Type     | Description
|---------------|-----------|---------------|-------------
| ChequeData    | Mandatory | ChequeData    | ChequeData structure, see description below.
| Hash          | Mandatory | Hash          | Hash of ChequeData, ses description below
| Signature     | Optional  | Signature     | Signature of Hash, see description below.


### 2.1.2 ChequeData data structure
The ChequeData field contains a JSON encoded string made of a strucutre.

The structure has the following fields:

| Field Name            | Required  | Data Type | Size | Description
|-----------------------|-----------|-----------|------|-------------
| MetaData              | Mandatory | MetaData  | n/a  | Meta data for this object.
| SerialNo              | Mandatory | Integer   | 8    | Unique number for the issuer identifying the cheque.
| Amount                | Mandatory | Integer   | 8    | Value denoted in the smallest divisible currency unit.
| Currency              | Mandatory | String    | 3    | Currency code as specified in ISO 4217.
| IssueDateTime         | Mandatory | DateTime  | n/a  | UTC date and time when this cheque was issued.
| ExpirationDateTime    | Mandatory | DateTime  | n/a  | UTC date and time when this latest can be claimed. After this time an unclaimed cheque is canceled and it will not be possible to claim the money any more.
| EscrowDateTime        | Mandatory | DateTime  | n/a  | UTC date and time when value hold by this cheque can first be collected.
| HoldDateTime          | Mandatory | DateTime  | n/a  | The maximum time the issuer will hold the money for a claimed cheque before it will be sent to a receiver.
| IssuerURL             | Mandatory | String    | 64   | URL of the issuer where a teh cheque can be claim and collect.
| AccessCode            | Mandatory | String    | 32   | Access Code for accessing the cheque.
| Condition             | Optional  | Condition | n/a  | Required condition to claim and collect the cheque's promised money.
| Reference             | Optional  | String    | 16   | Receivers reference.
| Memo                  | Optional  | String    | 32   | Information from the payer to the receiver.
| FixedFee              | Optional  | Integer   | 8    | A fixed fee that must be paid when receive the money.
| TransferFee           | Optional  | Integer   | 8    | A fee to cover the transaction cost.
| IssuerId              | Optional  | IdFields  | n/a  | Identification fields of the cheque issuer.
| PayerId               | Optional  | IdFields  | n/a  | Identification fields of the cheque issuer.
| ReceiverId            | Optional  | IdFields  | n/a  | Identification fields of the cheque issuer.


### 2.1.3 MetaData data structure
This structure contains various fields to identify the type of this cheque.

| Field Name    | Required  | Data Type | Size | Description
|---------------|-----------|-----------|------|-------------
| Specification | Mandatory | String    | 32   | Fixed string "Payment Cheque" indicating this specification.
| Version       | Mandatory | Integer   | 16   | The version of this specification.
| FilePrefix    | Optional  | String    | 64   | Prefix for the cheque file. See 3.2 for details.


### 2.1.7 Condition data structure
This data structure holds conditions that must be meet in order for the cheque
receiver to collect the amount value hold by the cheque.

| Field Name            | Required  | Data Type     | Size | Description
|-----------------------|-----------|---------------|------|-------------
| Wallet                | Optional  | String        | 64   | Wallet that the amount shall be paid to.
| MoneyAddress          | Optional  | String        | 256  | Receiver must prove it is the owner of the Money Address. See Payment Protocol for validation procedure.
| Email                 | Optional  | String        | 256  | Receiver must prove it is the owner of the e-mail address.


### 2.1.4 IdField data structure
A IdField data structure holds information that can be used to identifies the parties involved in the cheque payment.

The IdFields data type is a structure consisting of the following fields:

| Field Name            | Required  | Data Type | Size | Description
|-----------------------|-----------|-----------|------|-------------
| FirstName             | Optional  | String    | 16   | First name of the body.
| LastName              | Optional  | String    | 16   | Last name of the body.
| Address1              | Optional  | String    | 16   | First address line.
| Address2              | Optional  | String    | 16   | Second address line.
| City                  | Optional  | String    | 16   | City.
| ZIP                   | Optional  | String    | 16   | ZIP code.
| State                 | Optional  | String    | 16   | State.
| Country               | Optional  | String    | 16   | Country.
| Email                 | Optional  | String    | 32   | E-mail address.
| MoneyAddress          | Optional  | String    | 32   | Money address.
| Web                   | Optional  | String    | 32   | Web address.
| SocialSecurity        | Optional  | String    | 16   | Social security number
| BusinessNo            | Optional  | String    | 16   | Business/Organization number.


### 2.1.5 Hash data structure
The Hash data type holds the hash value of the ChequeData field.

The ChequeData shall be stored as JSON encoded string and the hash shall be calculated of this string.

| Field Name            | Required  | Data Type     | Size | Description
|-----------------------|-----------|---------------|------|-------------
| Algorithm             | Mandatory | HashAlgorithm | n/a  | Algorithm, see list of valid algorithms below.
| Hash                  | Mandatory | String        | 32   | Hash value using the algorithm.


### 2.1.6 Signature data structure
The Signature data structure holds digital signature of the hash.

| Field Name            | Required  | Data Type     | Size | Description
|-----------------------|-----------|---------------|------|-------------
| Algorithm             | Mandatory | SigAlgorithm  | n/a  | Algorithm, see list of valid algorithms below.
| Signature             | Mandatory | String        | n/a  | Hash value using the algorithm.


## 2.2 Data types
### 2.2.1 Integer
Integers are both positive and negative.


### 2.2.2 String
Strings shall be UTF-16 encoded.

The size indicates number of characters that the field must be able to store. As UTF-16 encoded characters use 2 bytes, the actual data storage needed in bytes is twice the size.


### 2.2.3 DateTime data type
This type holds a date and time in string format.

Format is YYYYMMDDhhmmss.

Example: 20160914214130


### 2.2.4 HashAlgorithm data type


### 2.2.5 SigAlgorithm data type


## 3 File format
### 3.1 Data encoding
The Root Structure including all its sub-structures and fields are first encoded to JSON string and then encoded using base64.

Integers are stored as integer number type in the JSON string.

Base64 encoding shall be according to RFC 4648.

The base64 encoded string shall be stored using UTF-8 character encoding when saved as a file.


### 3.2 File prefixing
The base64 encoded string may optionally be prefixed with a text string. The purpose of the file prefix is to include a human readable text that helps identifying the content of the file.

If a file prefix is included it is recommended that it starts with the text "PAYMENT_CHEQUE".

The file prefix can include additional issuer specific texts.

The prefix text must consist of UTF-8 encoded character in range from 0x21 to 0x7E. Space character is not permitted. It is recommended to use underscore character in the need to separate words.

When using letters, it is recommended to only use uppercase.

The file prefix text and the base64 encoded data part must be separated with a colon character (:). When processing a file, it shall be possible to use this colon character when searching for the encoded part.

If a file prefix is included, the data field FilePrefix in the MetaData structure must be included and populated with the same prefix text. (The colon character between the file prefix text and the encoded data part is not included in the FilePrefix field.)


### 3.3 File name extension
When saved as a file the Payment Cheque shall have ".pcf" as extension.


## 4 Description of usage
### 4.1 Issuance 
Issuance is the process of creating a Payment Cheque and giving it to somebody else. Once the cheque is given to somebody else, it is expected that the issuer will fulfill the promise of paying the cheque's face value to the first whoever meets its stated condition.

When the cheque is created, all data mandatory structures and data fields must be created and populated with valid data.

Optional data structures and data fields can be created if required by the purpose of the cheque. If a optional data structures or data field is created, it shall be populated with valid data.

It is highly recommended that the cheque's field does not change or fields created after issuance. Hence all field expected to be needed to fulfill the purpose of the cheque must be prepared and populated.


### 4.2 Payment condition
The condition structure field may contain one or more condition that decides where the claimed money will be put.

- **Wallet** - This field can be set with a required wallet address that the cheque's promised money will be paid to.
- **Money Address** - This field indicates a Money Address that cheque will be paid to. It will be up to the Money Address interface to decide what Wallet address to put the money in. (The Money Address interface is out of scope of this specification.)
- **Email** - This field indicates a e-mail address that the Cheque Receiver must be the owner of to be able to claim the money. The receiver must prove to be the owner of this e-mail to be allowed to claim the cheque. If a bank that has issued a cheque already has verified a receiver's e-mail address, it should not be expected that a second e-mail address to be required. It will be up to the verified e-mail owner to decided what address to put the money in.

If this structure contains more than one conditions, then all conditions must be meet to claim the cheque. Note that some combinations of conditions may be contradictory and impossible to meet. It will be up to the cheque issuer to ensure that no such contradictions are present.


### 4.3 Cheque claiming and money collecting
For a Cheque Receiver to get the promised money of a Cheque, two procedures will be needed.

First, the receiver must claim the cheque. The claim must be conducted within the cheque's Expire Time. If the cheque has any conditions field, all conditions must be verified within the the expiration time.

Once a receiver has claimed a cheque and all conditions has been meet, the cheque is claimed and can not be claimed or paid to other.

After the cheque has been claimd, its promised money can be collected. The collection must be conducted after the Escrow Time but before the Pay Time.

When claiming and collecting a cheque, its Access Code and Hash should be shown to the bank as evidence that the reciever actual has the cheque.

The Pay Time will give a cheque receiver the ability to bundle and transfer money from several cheques in one transaction. This will recieve transaction costs. However, the bank shall not be expected to hold the money for a longer pariod after the Hold Time. After Hold Time the bank can make the money transaction to the receiver's address without prior notice.

The claim and collect procedure can be done in one single operation. This will be for the bank to implement. In any case, the actual money transaction should not take place before the Escrow Time as passed.


### 4.4 Money back-guarantee
The Escrow filed indicates when the cheque's money can first collected, that is payd out the receiver.

Due to the Escrow Time, the cheque's promised money will be hold back by the bank for a certain time. This will give the customer (Cheque Payer) and the bank a reasonable time to evaluate if a merchant (Payment Receiver) has meet his part of a deal. In case the merchant has not meet the deal, the customer  can fill in a complain to the bank and request the cheque to be canceled.

The process of handling a complain and decide whatever a cheque will be canceled or not, or maybe only be paid partially, will be for the bank to decided. That process is out of scope of this specification.


### 4.5 Information to receiver
The cheque has two fields that can be used to give information to a receiver.

- **Reference** - This field is intended to be filled with a reference number/string that has previously been provided by the receiver as a reference to an payment request. The reference can be used to map the payment request to the cheque payment. If no such reference has been given, this field should be omitted.
- **Memo** - This field is up to the payer to fill in. If a payer is sending an cheque, this field can be used to indicate what the money is for.


### 4.6 Indentifications of parties
The cheque has tree data structure fields for identifying each of the parties involved in a cheque payment, the Cheque Issuer, the Cheque Payer and the Cheque Receiver.

The identification structures has field to identify a party by post/office address, web address, e-mail address.

Jurisdictions may require these field to be set. It will be the Cheque Issuer that will be responsible to ensure the correctness of this information.


### 4.7 Fees and transaction costs
Operating a payment system is not without costs and eventually somebody will have to pay for these services. 

The cheuqe has two fee fields that have the following purposes:
- **FixedFee** - This is a fixed fee the receiver must pay to receive the money. The fee can be taken the cheques amount at collection.
- **TransferFee** - This fee is intended to cover the transaction cost when collecting the money. If a receiver chose to bundle several cheques, the receiver should be only be charge one of the transfer fees. This will make an incentive to bundle cheques and reduce transaction load on a blockchain system.

Obviously, the cheque's face value must be large enough to cover the fixed fee. However, it does not be large enough to cover the transfer fee. 

The fees may also be paid partially or completely by the Cheque Payer. In the case where the Cheque Receiver will not be required to pay any fees, these fields can be omitted.

A model for cost coverage and fees will be up to the bank to decide.


### 4.8 Security
The cheque has several in-built security features to prevent cheque fraud and to prevent a cheques to get in the wrong hands and prevent misuse of the system. One or several of these security mechanism may be utilized, depending on the circumstances and the value of the cheque.


#### 4.8.1 Hash and signature
The Hash is calculated from the Cheque Data field. The signature is calculated from this hash. 

By having the bank's public signature, a Cheque Receiver can verify the cheque is correct and valid. It is recommended that a Cheque Receiver always fist verifys the integrity of the cheque before claiming it. This is to prevent the receiver to unawarely participate in a DoS attack, repeatedly claiming no-existing cheques sent by criminals.


#### 4.8.2 Access Code
For a Cheque Receiver to claim and collect the cheque, he must have the Access Code. In practice also the Cheque's Serial Number will also be needed provided. The Access Code should be of certain length to prevent brute force attack.

In the case where user is manually claiming a cheque at the banks webpage, a combination of Access Code together with other techniques like captcha, should make a system resistant enough against brute force attack.


#### 4.8.3 Conditions
Additional to the Access Code, one or more conditions may be required.

If no condition is required, than anybody having the cheque's Serial Number and Access Code can claim and collect the cheque. In some situations, this may be the purpose and then it is all fine to leave the condition structure empty.

By adding condition, this will effectively prevent a stolen cheque to be collected and have its money put in a criminals account.

If only the Wallet field is added, a stolen cheque can still be claimed and collected by the thief, but the money can still only be put into the predetermined wallet.


### 4.9 Distributing financial information
The cheque's hash can be verified by having the cheque's signature and its issuing bank's public key. This makes it possible to verify that a cheque exists without having the rest of the details.

Any of the parties involved in a cheque payment, can transmitt the cheque's hash, signature and the bank's address (so the bank's public key can be obtained) to a third party. Additional cheque information like serial number, amount and currencies can also be sent. To verify the additional information, the third party will need to contanct the cheque's issuer. The hash can act as a key to access the bank and verify the additional information. (The MoneyAddress interface supports such verifications.)

This system will allow two levels of access rights to a cheque. By having both the Hash and Access Code, a suer has full access to the cheque and can claim and collect the money. By having only the Hash, a user will only have limited access like verifying the additional sent information.

This feature makes it possible to safely distribute payment information to third parties. These could be  statiscical service, payment insurance, payment advisory etc.
