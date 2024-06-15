# TSL2591
An improvement for TSL2591 sensor lux calculation
Sensor Used: Adafruit TSL2591( [https://cdn-learn.adafruit.com/assets/assets/000/078/658/original/TSL2591_DS000338_6-00.pdf?1564168468||Data Sheet] )

We can calculate Illuminance (lux) values as follows:

Illuminance (Lux) = \frac{Power \times Efficiency}{Area} = \frac{Luminous Flux (Lumens)}{Area}

(Power is in Watts and Efficiency is in lumen per watt.)

Area illuminated by a point light source like the sun at a distance R for example will be the surface area of a sphere with radius R.

Area=4\pi R^{2} .Since LED is not a point light source emitting at 360 degrees we take half this area. For an incandescent bulb we take full area since it emits light in all directions.

Therefore Area_{LED}=\frac{(4\pi R^{2})}{2} and Area_{Incandescent}=4\pi R^{2}

Now an incandescent bulb of 710 lumens is used and various distances and corresponding lux values are calculated using the above formula 1.This gives us expected values of lux from the incandescent bulb.

Sensor values for IR and Full spectrum (both IR and full spectrum receivers are available in TSL2591 as channel 1 and channel 0 respectively) values are noted at the same distances used for calculation in the previous step.These IR and Full spectrum values are then used for calculation of lux by the formula specified in adafruit library.

Now the equation for lux calculation is obtained from adafruit [https://github.com/adafruit/Adafruit_TSL2591_Library||TSL2591] library and then the values of IR and Visible is used for lux calculation. The formula for lux calculation used in the library is mentioned below:,Illuminance(Lux)=\frac{(ch0-ch1)*(1.0-\frac{ch1}{ch0})}{cpl}

ch0 is sensor reading of full spectrum .

ch1 is sensor reading from IR spectrum.

where, cpl=\frac{(integration_{-}time*gain)}{(TSL2591_{-}LUX_{-}DF)},

Note: integration_time, gain and TSL2591_LUX_DF are constants specified.

We now have a set of expected illuminance (lux) values from calculations using Equation 1, as well as illuminance (lux) values computed from the IR and full spectrum readings of the sensor using Formula 2.

Both curves are plotted below:



The next step involved correcting the formula for illuminance calculation (Equation 3) by fitting its curve to the expected illuminance values from Equation 2. To achieve this, a random constant ‘c’ was added to Equation 3. This constant was assigned values to adjust the calculated lux with the aim of aligning it with the expected values around the 5k lux range. Higher lux values (closer to the source) were not used for correction due to the \frac{1}{r^{2}} dependence, which results in significant lux variations with minor changes in distance, potentially leading to substantial fluctuations.

After curve fitting:



New equation for lux calculation is:

Illuminance(Lux)=\frac{((c*ch0)-ch1)*(1.0-\frac{ch1}{ch0})}{cpl}

Where c =0.8678.

Shown below is the same correct plot lux unit values converted to ppfd units (with lux to ppfd conversion factor 0.070 as approximation).



Possible improvements:

1) Use a calibrated light meter for generating the expected values than from calculating.

2) Use an optical bench to measure distances properly.
