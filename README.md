PT-BR
Controle de acesso com Arduino via leitura de Tag RFID.

Esta versão possui um menu CLI escondido que é mostrado quando o Arduino receber o caracter 's' via comunicação serial (Baud 9600).
Através da comunicação sérial é possível cadastrar e excluir um ID ou cadastrar e excluir o ID do master
bastando para isso saber o número da Tag a ser cadastrado / excluído no formato hexadecimal.
Pode-se usar o próprio leitor para saber previamente o número hexadecimal da Tag ou usar outro leitor de RFID.

Usando um programa externo também é possível capturar os dados enviados pela comunicação serial e criar um arquivo de log
do sistema o qual pode registrar os ID lidos e identificá-los por horário.

EN
Access control with Arduino via RFID Tag reading.

This version has a hidden CLI menu which is shown when the Arduino receives the 's' character via serial communication (Baud 9600).
Through serial communication it is possible to register and delete an ID or register and delete the master ID
just knowing the Tag number to be registered/deleted in hexadecimal format.
You can use the reader itself to know in advance the hexadecimal number of the Tag or use another RFID reader.

Using an external program it is also possible to capture the data sent by serial communication and create a log file
system which can record the read IDs and identify them by time.

# arduino-access-control
   Project improved by Marco Barbosa ---> https://github.com/mrc6/arduino-access-control
   --------------------------------------------------------------------------------------------------------------------
   Example sketch/program showing An Arduino Door Access Control featuring RFID, EEPROM, Relay
   --------------------------------------------------------------------------------------------------------------------
   This is a MFRC522 library example; for further details and other examples see: https://github.com/miguelbalboa/rfid

   This example showing a complete Door Access Control System

  Simple Work Flow (not limited to) :
                                     +---------+
  +----------------------------------->READ TAGS+^------------------------------------------+
  |                              +--------------------+                                     |
  |                              |                    |                                     |
  |                              |                    |                                     |
  |                         +----v-----+        +-----v----+                                |
  |                         |MASTER TAG|        |OTHER TAGS|                                |
  |                         +--+-------+        ++-------------+                            |
  |                            |                 |             |                            |
  |                            |                 |             |                            |
  |                      +-----v---+        +----v----+   +----v------+                     |
  |         +------------+READ TAGS+---+    |KNOWN TAG|   |UNKNOWN TAG|                     |
  |         |            +-+-------+   |    +-----------+ +------------------+              |
  |         |              |           |                |                    |              |
  |    +----v-----+   +----v----+   +--v--------+     +-v----------+  +------v----+         |
  |    |MASTER TAG|   |KNOWN TAG|   |UNKNOWN TAG|     |GRANT ACCESS|  |DENY ACCESS|         |
  |    +----------+   +---+-----+   +-----+-----+     +-----+------+  +-----+-----+         |
  |                       |               |                 |               |               |
  |       +----+     +----v------+     +--v---+             |               +--------------->
  +-------+EXIT|     |DELETE FROM|     |ADD TO|             |                               |
          +----+     |  EEPROM   |     |EEPROM|             |                               |
                     +-----------+     +------+             +-------------------------------+


   Use a Master Card which is act as Programmer then you can able to choose card holders who will granted access or not

 * **Easy User Interface**

   Just one RFID tag needed whether Delete or Add Tags. You can choose to use Leds for output or Serial LCD module to inform users.

 * **Stores Information on EEPROM**

   Information stored on non volatile Arduino's EEPROM memory to preserve Users' tag and Master Card. No Information lost
   if power lost. EEPROM has unlimited Read cycle but roughly 100,000 limited Write cycle.

 * **Security**
   To keep it simple we are going to use Tag's Unique IDs. It's simple and not hacker proof.

   @license Released into the public domain.

   Pin Layout
The following table shows the typical pin layout used:

|| MFRC522 |Uno / 101|Mega|Nano v3|Leonardo / Micro|Pro Micro|Yun [4]|Due|
|--|--|--|--|--|--|--|--|--|
| Signal|Pin|Pin|Pin|Pin|Pin|Pin|Pin|Pin|
|RST/Reset|RST|9[1]|5[1]|D9|RESET / ICSP-5|RST|Pin9|22 [1]|

SPI SS	SDA [3]	10 [2]	53 [2]	D10	10	10	Pin10	23 [2]
SPI MOSI	MOSI	11 / ICSP-4	51	D11	ICSP-4	16	ICSP4	SPI-4
SPI MISO	MISO	12 / ICSP-1	50	D12	ICSP-1	14	ICSP1	SPI-1
SPI SCK	SCK	13 / ICSP-3	52	D13	ICSP-3	15	ICSP3	SPI-3
 	ESP8266	Teensy
Wemos D1 mini	2.0	++ 2.0	3.1
Signal	Pin	Pin	Pin	Pin
RST/Reset	D3	7	4	9
SPI SS	D8	0	20	10
SPI MOSI	D7	2	22	11
SPI MISO	D6	3	23	12
SPI SCK	D5	1	21	13
[1]	(1, 2, 3) Configurable, typically defined as RST_PIN in sketch/program.
[2]	(1, 2, 3) Configurable, typically defined as SS_PIN in sketch/program.
[3]	The SDA pin might be labeled SS on some/older MFRC522 boards.
[4]	Source: https://github.com/miguelbalboa/rfid/issues/111#issuecomment-420433658 .
Important: If your micro controller supports multiple SPI interfaces, the library only uses the default (first) SPI of the Arduino framework.
  
  CLI Menu
    s - show this CLI Menu
    c - register new ID
    e - delete ID
    l - list all registered IDs
    m - delete master ID
    n - register new master ID
    x - delete ALL registered IDs
