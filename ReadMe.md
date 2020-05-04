# Using The LoRa Messenger Library


Table of contents:


#### [How to import the Lora Messenger library](#Import-the-Lora-Messenger-library) 

#### [How to add the encoding table to the Assets folder](#Add-the-encoding-table-to-the-Assets-folder)   

#### [How to create an Assets folder inside of your application](#Create-an-Assets-folder-inside-of-your-application)  

#### [How to read the encoding table from the Assets folder](#Read-the-encoding-table-from-the-Assets-folder)  

 
#### [The encodingMessage function](#The-encoding-message-function)  

#### [The forwardMessage function](#The-forward-message-function)  

#### [The sendLoraMessage function](#The-send-lora-message-function)  



# General Instructions

## Import the Lora Messenger library:
Ryan


## Add the encoding table to the Assets folder:
Ryan

## Create an Assets folder inside of your application:
In order to let your Android application read files, you need to have an assets folder. If you don’t have one you need to create it in order to add the encoding table file. This folder can be created by the following steps: 
1) Navigate to your application/project directory 
2) Right-click on your application folder 
3) From the drop-down list, choose New > Folder > Assets Folder

## Read the encoding table from the Assets folder:
After the encoding table file is added to the Assets folder, your application needs to read the file in order to encode the message. The following code snippet will let your application read the encoding table file from the assets folder.
```
val jsonString: String =
                application.assets.open("encoding_table.json").bufferedReader().use {
                    it.readText()
                }
```



# Functions

## The encoding message function 

#### What does it do: 
The encoded message function will look up the passed parameter in the encoding table and find the corresponding byte to that parameter and replace it with the parameter that was passed in the first place.

#### What does it take (parameters):
The parameters that the encoding message function needs to take are:
* ApiName: The name of the API that is chosen to execute.
* Parameter: The actual message components assigned to an array that is passed from the application to the Lora Messenger library in order to encode it.

#### What does it return:
It returns the encoded message in a byte code form.



## The forward message function 

#### What does it do: 
The forward message function is responsible for fragmenting the encoded message into smaller bytes and assigning the fragmented message to a packet stream. The function also assigns the packet stream to the device’s IP address which is where the messages will be received.

#### What does it take (parameters):
The only parameter that the forward message function takes is the encoded message after it got passed from the encoding message function.

#### How to initialize the socket’s IP address in the forwardMessage function:
The initialization of the IP address in the socket is very similar to assigning values in a regular array….(TO DO)

#### What does it return:
The forward message function returns the fragmented message and sets it to a specific IP address to be sent over the network.


##### Note: 
Prior to sending a packet stream, the forward Message function sends a 4 byte header which includes the total number of bytes expected to be in the message. Due to the constraints of LoRaWAN, messages are sent in 13 byte packets. The packets are constructed as follows:


* 2 bytes that form a unique id for the message as a whole
* 1 byte that is a combination of two nibbles that store information for the packet’s number and the total packets expected for the message. For example, if a message were to require 3 packets to be sent, the 3rd bytes of these 3 packets (in order) would be:
``` 
000 0011	0001 0011	0010 0011
3		19		35
```

* Up to 10 bytes of actual message data. This is a stream of the encoded parameters, taking up a number of bytes and in the order defined in the encoding table. 
  * The first byte of this stream is always a combination of two nibbles that store information on which app and api this message is for, as defined by the encoding table.

## The send lora message function
Ryan
  #### What does it do: 
  

  #### What does it take (parameters):
  

  #### What does it return:
  

