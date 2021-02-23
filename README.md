Jump Cloud QA Assignment Prepared by Alex Shleyfer

TC#1 
TC Name: 		    Initial application state check

Precondition:   	Unzip attached broken-hashserve.tgz
			        SET PORT=8088
			        Install Curl 
			        Install and configure POSTMAN tool for testing

Test Steps:	   	    launch C:\>broken-hashserve_win.exe in new Command Prompt window
Expected Results:  	Application is up and running
Actual Results:    	Application is up and running
Test Results:	   	Pass
Defect:		        None


TC#2
TC Name: 		    validate that STATS endpoint returns initial value of 0 requests processed

Precondition:   

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open second Command Prompt window and execute: C:\>curl http://127.0.0.1:8088/stats

Expected Results:  	Application is up and running
			        Validate that STATS endpoint returns initial value of 0 requests processed

Actual Results:    	Application is up and running  
	           	    {"TotalRequests":0,"AverageTime":0} 
			
Test Results:	   	Pass
Defect:	            None

TC#3 
TC Name: 		    Checking hash request and validate password using computed SHA512 hash

Precondition:  

Test Steps:	   	    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open second Command Prompt window and execute: C:\>curl -X POST -H ""application/json"" -d '{""password"":""angrymonkey""}' http://127.0.0.1:8088/hash 
			        Wait for 5 seconds
			        request hashed password value using integer value from previous step
			        C:\>curl -H ""application/json"" http://127.0.0.1:8088/hash/<insert integer value from previous step>
			        validate password value using computed SHA512 hash of the password from POST step above"

Expected Results:  	Application is up and running 	
			        {"TotalRequests":0,"AverageTime":0}
			        Password should be validated
				

Actual Results:    	Application is up and running
			        Malformed Input
			        Malformed Input

Test Results:	    Fail
Defect:			    Malformed Input errors appears after the following step: C:\>curl -X POST -H "application/json" -d '{"password":"angrymonkey"}' 
                    http://127.0.0.1:8088/hash


TC#4 
TC Name: 		    Check STATS gets updated after each POST execution

Precondition:   

Test Steps:	   	    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open second Command Prompt window and execute:C:\>curl http://127.0.0.1:8088/stats
			        Open third  Command Prompt window and execute:C:\>curl -X POST -H ""application/json"" -d '{""password"":""angrymonkey""}' http://127.0.0.1:8088/hash 
			        Repeat step#2 - C:\>curl http://127.0.0.1:8088/stats
			        Open third  Command Prompt window and execute:C:\>curl -X POST -H ""application/json"" -d '{""password"":""angrymonkey""}' http://127.0.0.1:8088/hash 
			        Repeat step#2 - C:\>curl http://127.0.0.1:8088/stats"

Expected Results:  	Total Reuests getting updated properly 
			
Actual Results:    	Application is up and running 
			        {"TotalRequests":0,"AverageTime":0
			        Malformed Input message appears
			        {"TotalRequests":1,"AverageTime":0}
			        Malformed Input message appears
			        {"TotalRequests":6,"AverageTime":0}" 

Test Results:	   	Pass
Defect:	            None

TC#5
TC Name: 		    Check correct calculation of AverageTime response parameter

Precondition:   	Additionally launch POSTMAN tool

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open POSTMAN and send POST request
			        Send GET request for STATS method In POSTMAN- http://127.0.0.1:8088/stats
			        in CMD window run this: C:\>curl http://127.0.0.1:8088/stats


Expected Results:  	Application is up and running 
			        POSTMAN response displays -Malformed Input message and response time appears in seconds +milliseconds
			        Make sure that 200 status message appears and response time is in milliseconds
			        The following message appears: {""TotalRequests"":7,""AverageTime"":0}"

Actual Results:    	Response time command prompt window displays 0 milliseconds even POSTMAN displays different time

Test Results:	   	Fail
Defect:			    AverageTime response in POSTMAN displays in milliseconds and in seconds in command line


TC#6
TC Name: 		    Check multiple connections

Precondition:   

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open POSTMAN and create multiple POST requests
			        Execute POST requests from POSTMAN

Expected Results:  	Application is up and running 
			    	Multiple POST requests executed simultaneously


Actual Results:    	Application is up and running
			        POST requests executed with Malformed Input messages

Test Results:	   	Pass
Defect:	            None

TC#7
TC Name: 		    Check  base64 encoded password

Precondition:   

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        run this command - curl -H ""application/json"" http://127.0.0.1:8088/hash/1

Expected Results:  	Get base 64 encoded password should appea

Actual Results:    	Application is up and running 
			        Hash not found message appears

Test Results:	   	Fail
Defect:			    Hash not found error appears for http://127.0.0.1:8088/hash/1 request



TC#8
TC Name: 	        Shutdown

Precondition:   

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open second Command Prompt window and execute: C:\>curl -X POST -H ""application/json"" -d '{""password"":""angrymonkey""}' http://127.0.0.1:8088/hash 
			        Execute the following: curl -X POST -d ‘shutdown’ http://127.0.0.1:8088/hash

Expected Results:  	Application should be stopped  

Actual Results:    	Application is up and running 
			        Malformed Input message appears
			        Nothing happens.. Script just hangs

Test Results:	   	Fail
Defect:			    curl -X POST -d ‘shutdown’ http://127.0.0.1:8088/hash is invalid

TC#9
TC Name: 		    Shutdown -Corrections
DESCRIPTION		    Looks like 'shutdown' appears in single quotes accidently or intentionally (change to double quotes)

Precondition:   

Test Steps:		    launch C:\>broken-hashserve_win.exe in new Command Prompt window
			        Open second Command Prompt window and execute: C:\>curl -X POST -H ""application/json"" -d '{""password"":""angrymonkey""}' http://127.0.0.1:8088/hash 
			        Execute the following: curl -X POST -d ""shutdown"" http://127.0.0.1:8088/hash

Expected Results:  	Application shutdown properly

Actual Results:    	Application is up and running 
			        Malformed Input message appears
			        Application shutdown properly

Test Results:	   	Fail
Defect:			    curl -X POST -d ‘shutdown’ http://127.0.0.1:8088/hash is invalid


TC#10
TC Name: 		    Shutdown inactive application

Precondition:   

Test Steps:		    make sure that application is NOT running
			        Execute the following: curl -X POST -d ""shutdown"" http://127.0.0.1:8088/hash

Expected Results:  	Application can not be shutdown since is not active

Actual Results:    	Make sure the following message appears: C:\Users\shley>curl -X POST -d ""shutdown"" http://127.0.0.1:8088/hash
			curl: (7) Failed to connect to 127.0.0.1 port 8088: Connection refused"	

Test Results:	   	Pass
Defect:			    None
