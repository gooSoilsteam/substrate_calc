## Description: 
I would like a small web application to calculate the savings by reusing substrate. The application will be used as promotion for our machines on an agri-tech stand. Our plan is that can input the price they pay for new substrate, how much substrate they use per year in cubic meters and they will then be given how much they can save by using our machine. It should also tell them aproximatly how long time they will have to use our machine for and how much power will be used.
The users will be accessing the web app from a QR-code.


## Technical stack:
- Should be hosted on Azure as a web app
- Should be set up for PWA
- Should be mobile first

## Requirements
- Inputs for the required values.
- Output to the screen of the resulting price
- Should be cheap to host, not much compute
- Should follow SoilSteams style sheet. See pictures in style folder, C:\Users\GustavOmbergOften-So\substrate_calculator\style
- Prices should be given in euro per cubic meters
- Energy should be showen in kWh and liters of diesel. It should be assumed that one liter of diesel is equivalent to 9.6kWh
- There should be a link to SoilSteams website at the bottom, soilsteam.com
- The visuals should be light, mostly white and soilsteams colors. 

## Inputs and formulas:
### Inputs:
- Price per cubic meter substrate.
- Total amount of cubic meters per year. 
- Percentage of reused substrate. Should be set to 90% by default
- Drop down list of substrate types. Currently just add it for the visuals, will figure out the usage later.
     Types:
     - Peat 
     - Coco
     - Rockwool
- Energy type, should choose between electricity and diesel. Standing on diesel by default. 
- Energy price. Should be set to 20 cent per kWh by befaulte or 2 euro per liter of diesel. 

### Outputs:
- Required amount of new substrate: total amount of substrate * (1-percentage of reused substrate)
- Energy usage: Total amount of substrate * percentage of reused substrate * 35kWh 
- Hours of machine running: Total amount of substrate * percentage of reused substrate / 3.5
- Savings, unit euro: Total amount per year * price per cubic meter - total amount per year * (1 - percent reused) * price per cubic - energy usage * energy price - hours of machine running * 25
  

