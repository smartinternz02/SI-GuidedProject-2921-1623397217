import time
import sys
import ibmiotf.application
import ibmiotf.device
import random
import json

#Provide your IBM Watson Device Credentials
organization = "pentio"
deviceType = "iotdevices"
deviceId = "1008"
authMethod = "token"
authToken = "6304532723"


# Initialize the device client.
E=0
A=0
T=0


try:
	deviceOptions = {"org": organization, "type": deviceType, "id": deviceId, "auth-method": authMethod, "auth-token": authToken}
	deviceCli = ibmiotf.device.Client(deviceOptions)
	#..............................................
	
except Exception as e:
	print("Caught exception connecting device: %s" % str(e))
	sys.exit()

# Connect and send a datapoint "hello" with value "world" into the cloud as an event of type "greeting" 10 times
deviceCli.connect()

while True:
        E=1811
        A='canteen'
        T='2pm'
        #Send Temperature & Humidity to IBM Watson
        data = {"d":{ 'employeeid' : E, 'area': A ,'time': T}}
        #print data
        def myOnPublishCallback():
            print ("Published employeeid = %s %%" % E, "area = %s %%" % A, "time = %s %%" % T, "to IBM Watson")

        success = deviceCli.publishEvent("Data", "json", data, qos=0, on_publish=myOnPublishCallback)
        if not success:
            print("Not connected to IoTF")
        time.sleep(1)
        
        

# Disconnect the device and application from the cloud
deviceCli.disconnect()

