# TSL2591
A Quick improvement for TSL2591 sensor lux calculation  
Sensor Used: Adafruit TSL2591( [https://cdn-learn.adafruit.com/assets/assets/000/078/658/original/TSL2591_DS000338_6-00.pdf?1564168468||Data Sheet] )  

We can calculate Illuminance (lux) values as follows:

![image](https://github.com/am0032/TSL2591/assets/123314532/cb681c80-f6f2-4aa6-9af3-5a090b2a9731)



(Power is in Watts and Efficiency is in lumen per watt  

Area illuminated by a point light source like the sun at a distance R for example will be the surface area of a sphere with radius R.  

Area=4\pi R^{2} .Since LED is not a point light source emitting at 360 degrees we take half this area. For an incandescent bulb we take full area since it emits light in all directions. 

Therefore ![image](https://github.com/am0032/TSL2591/assets/123314532/e50ba097-efd9-4a40-90ca-34b9f89b724a)


Now an incandescent bulb of 710 lumens is used and various distances and corresponding lux values are calculated using the above formula 1.This gives us expected values of lux from the incandescent bulb.  

Sensor values for IR and Full spectrum (both IR and full spectrum receivers are available in TSL2591 as channel 1 and channel 0 respectively) values are noted at the same distances used for calculation in the previous step.These IR and Full spectrum values are then used for calculation of lux by the formula specified in adafruit library.  

Now the equation for lux calculation is obtained from adafruit [https://github.com/adafruit/Adafruit_TSL2591_Library||TSL2591] library and then the values of IR and Visible is used for lux calculation. The formula for lux calculation used in the library is mentioned below:  

![image](https://github.com/am0032/TSL2591/assets/123314532/03617fbf-889b-4c14-92c1-531ff118983c)  


ch0 is sensor reading of full spectrum .  

ch1 is sensor reading from IR spectrum.  

![image](https://github.com/am0032/TSL2591/assets/123314532/46cf00ca-343a-47d7-be0b-92fe14c9b0f2)  


Note: integration_time, gain and TSL2591_LUX_DF are constants specified.  

We now have a set of expected illuminance (lux) values from calculations using Equation 1, as well as illuminance (lux) values computed from the IR and full spectrum readings of the sensor using Formula 2.  

Both curves are plotted below:  

![image](https://github.com/am0032/TSL2591/assets/123314532/e8a4ce5b-422a-4313-a8a0-0a2892152331)  




The next step involved correcting the formula for illuminance calculation (Equation 3) by fitting its curve to the expected illuminance values from Equation 2. To achieve this, a random constant ‘c’ was added to Equation 3. This constant was assigned values to adjust the calculated lux with the aim of aligning it with the expected values around the 5k lux range. Higher lux values (closer to the source) were not used for correction due to the \frac{1}{r^{2}} dependence, which results in significant lux variations with minor changes in distance, potentially leading to substantial fluctuations.  

After curve fitting:  

![image](https://github.com/am0032/TSL2591/assets/123314532/ed8cc051-b638-4148-8d83-7d5234a440db)  


New equation for lux calculation is:  

![image](https://github.com/am0032/TSL2591/assets/123314532/647b0bc8-fe0c-4833-973d-defe377789bc)


Where c =0.8678.  


Possible improvements:

1) Use a calibrated light meter for generating the expected values than from calculating.

2) Use an optical bench to measure distances properly.
