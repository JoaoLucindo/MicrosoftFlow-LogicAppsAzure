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

Acesse [Microsoft Flow](flow.microsoft.com), faça o Login e clique em "My Flows"".

![Portal Microsoft Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/1.png)
## 2) Create from blank
![Create from blank](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/2.png)
## 3) Request/Response
![Request/Response](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/3.png)
## 4) Method GET
![Method GET](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/4.png)
## 5) Adicione uma nova ação
![Adicione uma nova ação](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/5.png)
## 6) Send an email
![Send an email](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/6.png)
## 7) Create Flow
![Create Flow](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/7.png)
## 8) HTTP GET URL
![HTTP GET URL](https://github.com/JoaoLucindo/MicrosoftFlow-LogicAppsAzure/blob/master/8.png)
