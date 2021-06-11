---
id: 110
title: Controlling LED brightness using potentiometer and arduino
date: 2015-08-24T19:43:14+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/18-revision-v1/
permalink: /?p=110
---
I was doing experiments with my Arduino today and this was one of them. I have commented the code well so it should be easy to understand. Here you go:<!--more-->
<pre>/*
SETUP
Connect led to analog pin 10, shorter end to ground.
Potentiometer setup:
(From left to right)
1. +5V
2. Analog pin 0 (A0)
3. Ground
*/

//Set LED pin to analog pin 10.
const int led_pin = 10;
//Set potentio meter pin to analog pin 0 (A0).
const int pot_met = 0;

void setup(){
  //Set led_pin to be the output.
  pinMode(led_pin,OUTPUT);
  //Initiate serial (optional).
  Serial.begin(9600);
}

void loop(){
  //Use the readPotentioMeterBrightness method to read brightness value.
  int sensor_val = readPotentioMeterBrightness();
  //Output the value to serial (optional).
  Serial.println(sensor_val);
  //Set LED brightness to value that we acquired from potentiometer.
  analogWrite(led_pin,sensor_val);
  //Pause for 250 ms.
  delay(250);
}

int readPotentioMeterBrightness(){
  //Read analog input and store value in integer variable val
  int val = analogRead(pot_met);
  //Adjust the value to be in range 0 to 255.
  val = map(val,0,1023,0,255);
  //Ensure value is between 0 and 255. 
  val = constrain(val,0,255);
  //Return value
  return val;
}</pre>
&nbsp;