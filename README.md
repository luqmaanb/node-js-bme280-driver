# node-js-bme280-driver

This is a node.js module for the [BME280](https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/) developed by Bosch. The sensor reports ambient temperature, relative humidity and atmospheric pressure. The module portable accross linux platforms such as Raspberry Pi and BeagleBone

## Usage

This is not an official node.js module hence it will need to be installed from github.

1. Install the module
```npm install luqmaanb/node-js-bme280-driver```
2. Run the example ```sudo node example.js```

## Example
```js
// get the bme280 driver
const BME280 = require('bme280');

// configure the necessary i2c parameters
const i2cSettings = {
  i2cBusNo   : 1,   // i2c bus depending on linux platform
  i2cAddress : 0x76 // i2c address of bme280
};

// create new instance of bme280 driver with the above i2c settings
const bme280 = new BME280(i2cSettings);

// get the data
const readSensorData = () => {
  bme280.readSensorData()
    .then((data) => { // report data on console if successful
      console.log(`data = ${JSON.stringify(data, null, 2)}`);
      setTimeout(readSensorData, 2000);
    })
    .catch((err) => { // report error on console if unsuccessul
      console.log(`BME280 read error: ${err}`);
      setTimeout(readSensorData, 2000);
    });
};

// Initialize the BME280 sensor
bme280.init()
  .then(() => {
    console.log('BME280 initialization succeeded');
    readSensorData();
  })
  .catch((err) => console.error(`BME280 initialization failed: ${err} `));
  ```