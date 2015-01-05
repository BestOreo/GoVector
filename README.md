GoVector
========

A ShiViz compatible Logging Library in Go

This is small library you can add to any Go project to create a ShiViz Compatible Log. 

Files Included:

Govec Folder : Contains the Library and all its dependencies 
TestFile : A small program to Test the Library 
exampleprocess-log : An example log generated by library

How to Use This Library
	
	Step 1:
	Create a Global Variable and Initilize it using like this = 
	Logger:= Initialize("MyProcess","ProcessId",ShouldYouSeeLoggingOnScreen,ShouldISendVectorClockonWire,Debug)
	ShouldYouSeeLoggingOnScreen prints whatever is logged by the Libary on Standard Output
	ShouldISendVectorClockonWire should be true if all involved Networked Program are also using this library and false if
	only you are logging 
	Debug prints extra information on Standard Output
	
	Step 2:
	When Ever You Decide to Send any []byte, call PrepareSend and send output. 
	So instead of:
	TCP.Send([]MessagePayload)
	call :
	TCP.Send(PrepareSend("Message Description", []YourPayload))

        Step 3:
	When Receiveing a []byte Message, Unpack it with UnpackRecieve Call. 
	So instead of:
	TCP.Receive([]ReceivedPayload)
	call :
	[]UnpackedMessage :=  UnpackReceive("Message Description" []ReceivedPayload)
	using UnpackedMessage for further processing.
	
	
