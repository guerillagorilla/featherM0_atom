/*
  PubNub over WiFi Example using Adafruit Feather M0 WINC1500
*/
#include <Arduino.h>

#include <SPI.h>

#include <WiFi101.h>
#define PubNub_BASE_CLIENT WiFiClient
#include <PubNub.h>

static char ssid[] = "FiOS-RRG1C";   // your network SSID (name)
static char pass[] = "cut6428term74hover";            // your network password
int status = WL_IDLE_STATUS;                // the Wifi radio's status

const static char pubkey[] = "pub-c-a90d9db6-0462-4a8e-8ccf-686258ad2949";
const static char subkey[] = "sub-c-bd973b02-14f1-11e8-acae-aa071d12b3f5";
const static char channel[] = "hello_world";

const int pinAdc = A0;

void setup() {
  /* This is the only line of code that is Feather M0 WINC1500
  specific, the rest is the same as for the WiFi101 */
  WiFi.setPins(8,7,4,2);

  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Serial set up");

  // attempt to connect using WPA2 encryption:
  Serial.println("Attempting to connect to WPA network...");
  status = WiFi.begin(ssid, pass);

  // if you're not connected, stop here:
  if ( status != WL_CONNECTED) {
    Serial.println("Couldn't get a wifi connection");
    while (true);
  } else {
    Serial.print("WiFi connecting to SSID: ");
    Serial.println(ssid);

    PubNub.begin(pubkey, subkey);
    Serial.println("PubNub set up");
  }
}


void loop() {

  long sum = 0;
  for(int i=0; i<32; i++)
  {
      sum += analogRead(pinAdc);
  }

  sum >>= 5;

  Serial.println(sum);
  delay(10);

  /* Publish */

  char msg[] = "\"Hello world from Arduino for Adafruit Feather M0 WINC1500\"";
  WiFiClient *client = PubNub.publish(channel, msg);

  if (!client) {
    Serial.println("publishing error");
    delay(1000);
    return;
  }
  client->stop();

  /* Subscribe */

  PubSubClient *sclient  = PubNub.subscribe(channel);
  if (0 == sclient) {
    Serial.println("subscribe error");
    delay(1000);
    return;
  }

  while (sclient->wait_for_data()) {
    Serial.write(sclient->read());
  }
  sclient->stop();

  /* Wait a little before we go again */
  delay(1000);
}
