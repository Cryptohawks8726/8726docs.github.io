ismail - not proofread nor complete
# REV motors

The two REV motors we use are apart of the [REV ION Brushless](https://docs.revrobotics.com/brushless) package.

## NEO
NEOs are compact and lightweight brushless motors. 

## NEO Vortex
A NEO Vortex (Vortex for short) is essentially an upgraded version of a regular NEO, designed for high-power applications. 

NEO Vortexes are typically controlled using a [SparkFlex motor controller.](https://docs.revrobotics.com/brushless/spark-flex/overview) SparkFlexes are mounted directly onto Vortexes; they contain most of the key features found in [SparkMaxes](sparkmax.md) along with a few additions. 8726 does not use SparkFlexes, however. Instead, a special [Solo Adapter](https://www.revrobotics.com/rev-11-2828/) has been mounted in place for Sparkmaxes to connect to.

| Port         | Wire Type   | Current Draw  | Required | Where is it?            | Additional Info        |
| ------------ | ----------- | ------------- | -------- | ----------------------- | ---------------------- |
| Encoder port   | 22 gauge    |  amps | &check;  | Side connecting to motor controller  | Enables connection to the motor's encoder |
