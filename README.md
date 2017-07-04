# MicrosoftFlow-LogicAppsAzure

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

Monte o circuito, conforme a figura abaixo e alimente o módulo com o cabo USB mini.

![Circuito](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/MicrosoftFlow-LogicApps-Button-Frittzing%20Project_bb.png)

Implementação do Software 
------------
A placa de desenvolvimento NodeMCU pode ser programa em LUA, ou em C++ utilizando a IDE Arduino. Nesse tutorial será utilizado a IDE Arduino.

## Preparação da IDE Arduino 

###### 1) Package ESP8266

Faça o download da [IDE](https://www.arduino.cc/en/Main/Software), e a instale. Abra a IDE e em File -> Preferences, no campo "Additional Boards Manager URLs" insira a URL ``http://arduino.esp8266.com/stable/package_esp8266com_index.json`` e clique em OK. Após esses passos será iniciado o Download dos itens necessários para a programação do NodeMCU. Quando finilazir o download, reinicie a IDE.

![NodeMCU_IDE](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/9.png)

###### 2) Inserção das Bibliotecas

Faça o Download do [arquivo ESP8266wifi-master.zip](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/ESP8266wifi-master.zip). Indique o caminho do arquivo recém baixado e clique em "Open"

![library](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/11.png)

## Software 

Faça o download do código completo. Substitua os campos:

* <SSID> pelo nome de sua rede Wifi
* <PASSWORD> pela senha da rede
* <HOST> pelos strings antes de 443, obtida no item #8 [no caso: `` https://prod-32.westus.logic.azure.com `` ]
* <URL>  pelos strings após 443, obtida no item #8 [no caso ``/workflows/<ID>/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<ID>``]

