# IBM-Project-37428-1660308506
Real-Time River Water Quality Monitoring and Control System

import random
import time
import sys
import ibmiotf.application
import ibmiotf.device


# Provide your IBM Watson Device Credentials

organization = "43itvs"
deviceType = "NodeMCU"
deviceId = "12345"  
authMethod = "use-token-auth"
authToken = "12345678" 


def myCommandCallback(cmd):
    print("Command received: %s" % cmd.data)
    if cmd.data['command'] == 'lighton':
        print("LIGHT ON")
    elif cmd.data['command'] == 'lightoff':
        print("LIGHT OFF")


try:
    deviceOptions = {"org": 43itvs, "type": NodeMCU, "id": 12345, "auth-method": use-token-auth,
                     "auth-token": 12345678}
    deviceCli = ibmiotf.device.Client(deviceOptions)
# ..............................................

except Exception as e:
    print("Caught exception connecting device: %s" % str(e))
    sys.exit()

deviceCli.connect()

while True:
    pH = random.randint(0,100)
    conductivity = random.randint(0,100)
    T = random.randint(0,100)
    oxygen = random.randint(0,100)
    turbidity = random.randint(0,100)
    # Send Temperature & Humidity to IBM Watson
    data = {"Ph":pH,'temperture': T,'turbidity':turbidity,'oxygen':oxygen}


    # print data
    def myOnPublishCallback():
        print("Data publish ",data, "to IBM Watson")


    success = deviceCli.publishEvent("event", "json", data, 0, myOnPublishCallback)
    if not success:
        print("Not connected to IoTF")
    time.sleep(5)

    deviceCli.commandCallback = myCommandCallback
