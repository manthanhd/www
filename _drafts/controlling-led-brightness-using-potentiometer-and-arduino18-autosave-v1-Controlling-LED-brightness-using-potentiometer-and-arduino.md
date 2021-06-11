---
id: 109
title: Controlling LED brightness using potentiometer and arduino
date: 2015-08-24T19:43:02+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/18-autosave-v1/
permalink: /?p=109
---
I was doing experiments with my Arduino today and this was one of them. I have commented the code well so it should be easy to understand. Here you go:<br /><pre>/*<br />SETUP<br />Connect led to analog pin 10, shorter end to ground.<br />Potentiometer setup:<br />(From left to right)<br />1. +5V<br />2. Analog pin 0 (A0)<br />3. Ground<br />*/<br /><br />//Set LED pin to analog pin 10.<br />const int led_pin = 10;<br />//Set potentio meter pin to analog pin 0 (A0).<br />const int pot_met = 0;<br /><br />void setup(){<br />  //Set led_pin to be the output.<br />  pinMode(led_pin,OUTPUT);<br />  //Initiate serial (optional).<br />  Serial.begin(9600);<br />}<br /><br />void loop(){<br />  //Use the readPotentioMeterBrightness method to read brightness value.<br />  int sensor_val = readPotentioMeterBrightness();<br />  //Output the value to serial (optional).<br />  Serial.println(sensor_val);<br />  //Set LED brightness to value that we acquired from potentiometer.<br />  analogWrite(led_pin,sensor_val);<br />  //Pause for 250 ms.<br />  delay(250);<br />}<br /><br />int readPotentioMeterBrightness(){<br />  //Read analog input and store value in integer variable val<br />  int val = analogRead(pot_met);<br />  //Adjust the value to be in range 0 to 255.<br />  val = map(val,0,1023,0,255);<br />  //Ensure value is between 0 and 255. <br />  val = constrain(val,0,255);<br />  //Return value<br />  return val;<br />}<br /></pre><br />