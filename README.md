# SplunkToLogstalgia
This Python script can be run to create and submit a Streaming Splunk query and format the output for Logstalgia. 



The Many Prerequisites: 

1) You will need Python 2.7.12 (later versions have not been tested) 
2) You will need to download and install the Splunk Python SDK  
	https://github.com/splunk/splunk-sdk-python
3) You will need patience grasshopper 
4) You will need to setup the Splunk CLI (or install Splunk Lite)
	https://www.splunk.com/en_us/download/splunk-light.html
5) Setup credentials for connecting to your Splunk server (or your hosted Splunk Cloud instance) 
6) Test your connection to Splunk by using the CLI to submit a query
	For example (for Mac OSX):
		cd /Applications/Splunk/bin
		./splunk list monitor
		(login if needed)
		ensure you see a listing of Monitored Directories and Files...or something relevant to prove your connected to your Splunk cluster. 
7) Change directories to the directory that you cloned/downloaded the Splunk Python SDK and enter the examples directory.  Then copy the SplunkToLogstalgia.py file to the examples directory.  
	For example(on Mac OSX): 
		cd /Users/myusername/code/splunk-sdk-python/examples     
		cp /Users/myusername/code/SplunkToLogstalgia/SplunkToLogstalgia.py /Users/myusername/code/splunk-sdk-python/examples/SplunkToLogstalgia.py
8) Find and use a search that returns the data you need for the Apache logs. Be sure you test this first with Splunk so you know it works.  
	For example:   
	search sourcetype="haproxy" client_ip="*" haproxy_server_name="*" haproxy_status_code="*" haproxy_wikiid="*" haproxy_accept_date="*" haproxy_http_request="*" haproxy_resp_content_length="*"
9) The python script will parse the fields in this example and populate a Logstalgia compatible log entry.  This is sent to STDOUT for logstalgia to read in, or can be redirected to a file.  
	For example: 
	python SplunkToLogstalgia.py "search sourcetype="haproxy" client_ip="*" haproxy_server_name="*" haproxy_status_code="*" haproxy_wikiid="*" haproxy_accept_date="*" haproxy_http_request="*" haproxy_resp_content_length="*"" | logstalgia
10) For course, make sure you installed Logstalgia
	https://github.com/acaudwell/Logstalgia
11) Adjust Logstalgia to display the data you need and be sure that jobs are canceled on Splunk after you stop the Streaming Splunk query. 



Note:  
  It is possible to connect to one of your servers running Splunk to send the queries to Splunk Cloud and recieve data on the appropriate port. 
  For Example: 
  	ssh -L 8089:mysplunkdomain.splunkcloud.com:8089  oneofmyservers.mydomain.com









Additional Command Line examples: 

ssh -v -i .ssh/privatekey -L 8089:jumpbox:8089  splunkserver.mydomain.com

python SplunkToLogstalgia.py "search sourcetype="haproxy" client_ip="*" haproxy_server_name="*" haproxy_status_code="*" haproxy_wikiid="*" haproxy_accept_date="*" haproxy_http_request="*" haproxy_resp_content_length="*" " | logstalgia --full-hostnames -g "MindTouch-API Client Error,CODE=^[4],30" -g "MindTouch-API Success,CODE=^[2],30" -g "MindTouch-API Redirects,CODE=^[3],30" -g "MindTouch-API Server Error,CODE=^[5],30"



