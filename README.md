# Push Button: MicrosoftFlow-LogicAppsAzure

## Process automation with Microsoft Flow / Logic Apps Azure using NodeMCU and Arduino IDE
The purpose of this document is to guide the development of a IoT button integrated with Microsoft Flow platform or Logic Apps - Microsoft Azure.

In this tutorial it will be developed an IoT button applied to the scenario of maintenance of a coffee machine using Microsoft Flow. However, it can be easily adapted to any other scenario or application.

## Requirements

* Access to Microsoft Flow or Azure Logic Apps
* Arduino IDE
* NodeMCU development board 
* Push Button
* 1 x 330 Ω resistor
* 1 x 1M Ω resistor
* Jumpers
* Breadboard
* Micro USB cable

------------
Setup Microsoft Flow  Environment 
------------

## 1) Microsoft Flow portal

Access [Microsoft Flow](flow.microsoft.com), Login and click "My Flows".

![Portal Microsoft Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/1.png)
## 2) Create from blank

Click "Create from blank" to create a new workflow.

![Create from blank](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/2.png)
## 3) Request/Response

Give a name to your Flow. Select the Trigger "Request/Response".

![Request/Response](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/3.png)
## 4) Method GET

In "Advance Options" choose  "Method GET".
![Method GET](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/4.png)
## 5) Add an action

Click "Add an action" to add a new action. 

![Adicione uma nova ação](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/5.png)
## 6) Send an email

Choose the action "Office 365 Outlook - Send an email".

![Send an email](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/6.png)
## 7) Create Flow

Complete all required fields (as you wish), and then click "Create Flow".

![Create Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/7.png)
## 8) HTTP GET URL

Then copy and save the HTTP GET URL:

``https://prod-32.westus.logic.azure.com:443/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>``

![HTTP GET URL](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/8.png)


Hardware Setup 
------------
## 1) Building a Circuit on Breadboard

Build the circuit like the one shown below.

![Circuito](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/MicrosoftFlow-LogicApps-Button-Frittzing%20Project_bb.png)

Software 
------------

The ESP8266 NodeMcu comes with a firmware that lets you program the chip with the Lua scripting language. But if you are already familiar with the Arduino way of doing things, you can also use the Arduino IDE to progam the ESP. In this tutorial we'll use the Arduino IDE. 

## IDE Arduino setup 

###### 1) Package ESP8266

Download the [IDE](https://www.arduino.cc/en/Main/Software), and install. Open the IDE; Choose File -> Preferences, in "Additional Boards Manager URLs" insert the URL ``http://arduino.esp8266.com/stable/package_esp8266com_index.json`` and than click "OK". After this steps your download will begin shortly and then restart the IDE.

![NodeMCU_IDE](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/9.png)

###### 2) Add libraries

Download the [file Arduino-Master](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/Arduino-master.zip). Add the library ESP8266WiFi.

![library](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/11.png)

## Software 

[Download the code](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/MicrosoftFlow_IoT_JoaoLucindo.ino) . 

Replace the values:

* SSID by your wireless network name
* PASSWORD by your wireless network password
* HOST by the strings of the HTTP GET URL before 443 (in this case: `` https://prod-32.westus.logic.azure.com `` )
* URL  by the strings of the HTTP GET URL after 443 (in this case ``/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>``)

In this way, the final code would be:
```
#include <ESP8266WiFi.h>

//static const uint8_t D0   = 16;
//static const uint8_t D1   = 5;
//static const uint8_t D2   = 4;
//static const uint8_t D3   = 0;
//static const uint8_t D4   = 2;
//static const uint8_t D5   = 14;
//static const uint8_t D6   = 12;
//static const uint8_t D7   = 13;
//static const uint8_t D8   = 15;
//static const uint8_t D9   = 3;
//static const uint8_t D10  = 1;

int inPin = 16;   // pushbutton connected to digital pin 0   
int val = 0;     // variable to store the read value
//Include the SSL client
#include <WiFiClientSecure.h>

char ssid[] = "<SSID>";       // your network SSID (name)
char password[] = "<PASSWORD>";  // your network key

//Add a SSL client
WiFiClientSecure client;


void setup() {

  pinMode(inPin, INPUT);      // sets the digital pin 1 as input

   Serial.begin(115200);

  // Set WiFi to station mode and disconnect from an AP if it was Previously
  // connected
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);

  // Attempt to connect to Wifi network:
  Serial.print("Connecting Wifi: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(500);
  }


Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  IPAddress ip = WiFi.localIP();
  Serial.println(ip);

}

String MicrosoftFlow() {
 
  char host[] = "prod-37.westus.logic.azure.com";

  if (client.connect(host, 443)) {
    Serial.println("connected");

    String URL = "/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>";

    Serial.println(URL);

    client.println("GET " + URL + " HTTP/1.1");
    client.print("Host: "); client.println(host);
    client.println("User-Agent: arduino/1.0");
    client.println("");
    }
}

void loop() {
  
  
  val = digitalRead(inPin);  // read input value
  delay(200);
  //Serial.println(val);

  if(val==HIGH){
    MicrosoftFlow();
    delay(1000);
    setup(); 
    }
  
}
```

Now you can compile and upload the code. You can see the result (after press the push button) in the picture below.

![email](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/12.png)






