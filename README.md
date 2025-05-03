**ISO-8583** is a standard used in the financial industry for **electronic funds transfer** messages, 
and jPOS is an open-source implementation that supports this standard along with ISO-20022, ANS X9.24, EMV, and others. 
It provides a Java-centric solution for payment gateways, acquirer, and issuer applications, making it useful for financial transactions globally.

**ISO 8583 messages** are used to transfer financial messages over the network. 
Typically between automated teller machines (ATM) or point of sale (POS) devices and banks transfer information through ISO 8583 messages.


![img.png](img.png)

### **Structure of the ISO 8583 message**
![img_1.png](img_1.png)

### Message Structure
ISO 8583 message basically has 4 parts.

1. Header
2. Message Type Identifier (MTI)
3. Bitmap
4. Data Fields

![img_2.png](img_2.png)

### Header 
Header can be any value and it’s not mandatory. But before transmitting the message, all involved parties should agree the length of the header.
So the header can be empty too. In this case, header length is 7.
![img_3.png](img_3.png)

### Message Type Identifier (MTI)
The MTI is a 4-digit number that indicates the type of transaction being performed.
![img_4.png](img_4.png)

### Bitmap
The bitmap is a 64-bit binary number that indicates which data fields are present in the message.
The bitmap is divided into two parts: the primary bitmap and the secondary bitmap.
The primary bitmap is the first 64 bits of the message, and it indicates which of the first 64 data fields are present.
The secondary bitmap is the next 64 bits of the message, and it indicates which of the next 64 data fields are present.
![img_5.png](img_5.png)
![img_6.png](img_6.png)

![img_7.png](img_7.png)
![img_8.png](img_8.png)

### Data Fields
This section contains the actual information of the transaction. Data fields definitions need to be defined beforehand to correctly parse data.
![img_9.png](img_9.png)
![img_10.png](img_10.png)

### Summary
![img_11.png](img_11.png)
![img_12.png](img_12.png)
![img_13.png](img_13.png)
![img_14.png](img_14.png)
![img_15.png](img_15.png)
![img_16.png](img_16.png)