GoVector
========

### Overview
This is a small library that you can add to your Go project to
generate a [ShiViz](http://bestchai.bitbucket.org/shiviz/)-compatible
vector-clock timestamped log of events in your distributed system.

PLEASE NOTE: GoVec is compatible with Go 1.4 + 

* govec/: Contains the Library and all its dependencies
* test.go: A small program to test the library
* example-Log.txt: An example log generated by the library

### Index

type GoLog
   * func Initialize(string ProcessName, string LogName) *GoLog
   * func InitializeMutipleExecutions(string ProcessName, string LogName) *GoLog
   * func PrepareSend(string LogMessage, byte[] b) (byte[] output)
   * func UnpackReceive(string LogMessage, byte[] b) (byte[] output)
   * func LogLocalEvent(string LogMessage)
   

### Usage

Add the govec folder to your project and import it with:

	"import ./govec"


####   type GoLog

	type GoLog struct{
		// contains filtered or unexported fields
	}

 The GoLog struct provides an interface to creating and maintaining vector timestamp entries in the generated log file
 
#####   func Initialize

	func Initialize(string ProcessName, string LogName) *GoLog

Returns a Go Log Struct taking in two arguments and truncates previous logs:
* MyProcessName (string): local process name; must be unique in your distributed system.
* LogFileName (string) : name of the log file that will store info. Any old log with the same name will be truncated


#####   func InitializeMutlipleExecutions
	
	func Initialize(string ProcessName, string LogName) *GoLog

Returns a Go Log Struct taking in two arguments without truncating previous log entry:
* MyProcessName (string): local process name; must be unique in your distributed system.
* LogFileName (string) : name of the log file that will store info. Each run will append to log file seperated by "=== Execution # ==="

#####   func PrepareSend
	
	func PrepareSend(string LogMessage, byte[] b) (byte[] output)

Logs LogMessage and vector timestamp into log file, and packs incoming byte[] b and vector timestamp as a GOB. Returns a byte[] output that can be sent on wire.

#####   func UnpackReceive
	
	func UnpackReceive(string LogMessage, byte[] b) (byte[] output)
	
Unpacks incoming byte[] b from GOB and logs LogMessage with received vector timestamp. Returns byte[] output which can be used for further processing by the program.

#####   func LogLocalMessage

	func LogLocalEvent(string LogMessage)
	
Increments current vector timestamp and logs it into Log File. No output is generated

###   Examples

The following is a basic example of how this library can be used 

	package main

	import "./govec"

	func main() {
		Logger := govec.Initialize("example", "example")
		
		//In Sending Process
		
		//Prepare a Message
		messagepayload := []byte("samplepayload")
		finalsend := Logger.PrepareSend("Sending Message", messagepayload)
		
		//send message
		//connection.Write(finalsend)

		//In Receiving Process
		
		//receive message
		recbuf := Logger.UnpackReceive("Receiving Message", finalsend)
		fmt.Print(string(recbuf[:]))

		//Can be called at any point 
		Logger.LogLocalEvent("Example Complete")
	}
