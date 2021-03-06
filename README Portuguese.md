# Push Button: MicrosoftFlow-LogicAppsAzure

## Automação de Processos com Microsoft Flow / Logic Apps Azure utilizando NodeMCU e Arduino IDE
O objetivo deste documento é guiar o desenvolvimento de um botão IoT integrado com a plataforma Microsoft Flow ou o serviço Logic Apps - Microsoft Azure. 

Nesse tutorial será desenvolvido um botão IoT aplicado ao cenário de manutenção de uma máquina de café utilizando o Microsoft Flow, todavia pode-se facilmente adaptadar para qualquer outro cenário ou aplicação.

## Itens Necessários

* Acesso ao Microsoft Flow ou Azure Logic Apps
* Arduino IDE
* Placa de desenvolvimento NodeMCU
* Push Button
* 1 Resistor de 330Ω
* 1 Resistor de 1MΩ
* Jumpers
* Protoboard
* Cabo micro USB

------------
Preparação do Ambiente Microsoft Flow 
------------

## 1) Portal Microsoft Flow

Acesse [Microsoft Flow](flow.microsoft.com), faça o Login e clique em "My Flows".

![Portal Microsoft Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/1.png)
## 2) Create from blank

Clique em "Create from blank" para iniciar o processo de criação de um novo Fluxo de Automação.

![Create from blank](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/2.png)
## 3) Request/Response

Preencha o campo "Flow name" como o nome que você deseja dar para o fluxo. Selecione o Trigger "Request/Response".

![Request/Response](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/3.png)
## 4) Method GET

Em "Advance Options" escolha o Method GET.
![Method GET](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/4.png)
## 5) Adicione uma nova ação

Clique em "Add an action" para adicionar uma ação a ser executada logo após a sensibilização do Trigger "Request/Response". 

![Adicione uma nova ação](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/5.png)
## 6) Send an email

Escolha a ação "Office 365 Outlook - Send an email".

![Send an email](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/6.png)
## 7) Create Flow

Configure o email como desejar e clique em "Create Flow".

![Create Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/7.png)
## 8) HTTP GET URL

Após a criação do Fluxo, reserve a HTTP GET URL gerada. Ela será na forma:

``https://prod-32.westus.logic.azure.com:443/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>``

![HTTP GET URL](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/8.png)


Preparação do Hardware 
------------
## 1) Montagem do Circuito

Monte o circuito, conforme a figura abaixo.

![Circuito](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/MicrosoftFlow-LogicApps-Button-Frittzing%20Project_bb.png)

Implementação do Software 
------------

A placa de desenvolvimento NodeMCU pode ser programa em Lua, ou em C++ utilizando a IDE Arduino. Nesse tutorial será utilizado a IDE Arduino.

## Preparação da IDE Arduino 

###### 1) Package ESP8266

Faça o download da [IDE](https://www.arduino.cc/en/Main/Software), e a instale. Abra a IDE e em File -> Preferences, no campo "Additional Boards Manager URLs" insira a URL ``http://arduino.esp8266.com/stable/package_esp8266com_index.json`` e clique em OK. Após esses passos será iniciado o Download dos itens necessários para a programação do NodeMCU. Quando finalizar o download, reinicie a IDE.

![NodeMCU_IDE](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/9.png)

###### 2) Inserção das Bibliotecas

Faça o Download do [arquivo Arduino-Master](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/Arduino-master.zip). E insira a biblioteca ESP8266WiFi.

![library](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/11.png)

## Software 

Faça o [download do código](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/MicrosoftFlow_IoT_JoaoLucindo.ino) completo. Substitua os campos:

* SSID pelo nome de sua rede Wifi
* PASSWORD pela senha da rede
* HOST pelos strings da HTTP GET URL antes de 443, obtida no item #8 (no caso: `` https://prod-32.westus.logic.azure.com `` )
* URL  pelos strings da HTTP GET URL após 443, obtida no item #8 (no caso ``/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>``)

Dessa forma, o código final seria:

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

Após essas modificações no arquivo .ino baixado, basta compilar e transferir o código para a placa de Desenvolvimento NodeMCU. O resultado, após apertar o push button é o email conforme a imagem a seguir:

![email](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/12.png)






