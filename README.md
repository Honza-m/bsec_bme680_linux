# bsec_bme680_linux

Read the BME680 sensor with the BSEC library on Linux (e.g. Raspberry Pi)

## Intro

Working example of fully using the
[BME680 sensor](https://www.bosch-sensortec.com/en/bst/products/all_products/bme680)
on Linux (e.g. Raspberry Pi) with the precompiled
[BSEC library](https://www.bosch-sensortec.com/bst/products/all_products/bsec),
which allows to read calibrated environment values including an actual Indoor
Air Quality (IAQ) score.

It makes use of
[Bosch's provided driver](https://github.com/BoschSensortec/BME680_driver)
and can be configured in terms of it.
Readings will be directly output to stdout in a loop.

## Prerequisites

[Download the BSEC software package from Bosch](https://www.bosch-sensortec.com/bst/products/all_products/bsec)
and put it into `./src`, then unpack.

## Configure and Compile

Optionally make changes to make.config.

Depending on how your sensor is embedded it might be surrounded by other
components giving off heat. Use an offset in °C in `bsec_bme680.c` to
compensate. The default is 0 °C:
```
#define temp_offset (0.0f)
```

To compile: `./make.sh`

## Usage


Output will be similar to this:

```
$ ./bsec_bme680
Collecting data to sqlite... (ctrl+c to quit)
```

It can easily be modified in the `output_ready` function.

The BSEC library is supposed to create an internal state of calibration with
increasing accuracy over time. Each 10.000 samples it will save the internal
calibration state to `./bsec_iaq.state` (or wherever you specify the config
directory to be) so it can pick up where it was after interruption.

## Troubleshooting

### bsec_bme680 just quits without a message

Your bsec_iaq.state file might be corrupt or incompatible after an update of the
BSEC library. Try (re)moving it.

