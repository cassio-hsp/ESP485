#include <ModbusRTU.h>               //Biblioteca do Emelianov
#include "DHT.h"

#define DHTPIN 4                     //Pino ao qual se conectou o sinal do DHT22
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);  

#define SLAVE_ID 2                   //Endereço modbus que desejaremos atribuir ao nosso slave.

ModbusRTU mb;

void setup()
{ 
  Serial.begin(115200, SERIAL_8N1);
  Serial2.begin(115200, SERIAL_8N1);
  mb.begin(&Serial2,5);
  dht.begin();
}

void loop()
{
  float h = dht.readHumidity();                    //Leituras dos dados do sensor.
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) 
  {
    Serial.println(F("Verificar sensor DHT22."));
    return;
  }

  Serial.print("Umidade: "); 
  Serial.print(h);                               //Printará no monitor serial "50.2", por exemplo.
  Serial.print("    Temperatura: ");
  Serial.println(t);

  h = h*100;                                     //Multiplicados h por 100 para obtermos um valor como "5020". No destino, voltaremos a dividir tal leitura por 100.
  t = t*100;

  mb.slave(SLAVE_ID);          //Cria um slave modbus com ID = SLAVE_ID.
  mb.addHreg(1);               //Adiciona ao slave um registro de endereço 1.
  mb.Hreg(1,h);                //Atribui ao registro 1 do slave o valor h.
  
  mb.addHreg(2);
  mb.Hreg(2,t);
  
  mb.task();                   //Necessário para rodar nossa tarefa de slave.
  yield();
}
