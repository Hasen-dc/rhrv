# rhrv

Matlab tools for calculating Heart-Rate Variability (HRV) metrics on ECG signals. Supports working with the [PhysioNet](https://physionet.org/) data format.

## Features
Currently this project is in a very early iteration and is intended for my personal research. Hopefully if might be useful to others at some point.

* WFDB wrappers: A small subset of the WFDB tools are wrapped with matlab functions.
  * `gqrs`: A QRS detection algorithm.
  * `rdsamp`: For reading PhysioNet signal data into matlab.
  * `rdann`: For reading PhysioNet annotation data into matlab.

* QRS detection: Finding the beats in ECG signals.
  * Detection of R-peaks in ECG signals (based on PhysioNet's `gqrs`). Configurable for use with both human and animal ECGs.
  * Comparison of QRS detections to reference annotations and calculation of quality indices like Sensitivity, PPV.

* ECG signal processing
   * Construction of NN time series (adjacent "normal" beats).
   * Filtering ECG to removing ectopic (out of place) beats.

* HRV Metrics: Calculating quantative measures that indicate the activity of the heart.
  * Time Domain: AVNN, SDNN, p50NN.
  * Frequency Domain:
    * Total and relative power in ULF, VLF, LF and HF bands.
    * Plotting the spectrum of the NN timeseries using either an Autoregressive model (Yule-Walker method) or a Lomb periodogram.
  * Nonlinear methods:
    * Detrended fluctuation analysis (DFA), with short- and long-term scaling exponents (alpha1, alpha2).
    * Spectral power-law scaling exponent (beta).
    * Multiscale sample entropy (MSE).

## Requirements
* The Pysionet WFDB tools.
* Matlab. Only tested on 2015b, but should work on any other version.
* Linux or OSX.

## Installation
* This repo contains a submodule as one of it's dependencies. After cloning, please run `git submodule update --init` to also clone the dependency into your local repo.
* Install the [Physionet WFDB tools](https://www.physionet.org/physiotools/wfdb.shtml). If you're on OSX, you can use [homebrew](http://brew.sh) to install it easily with `brew install homebrew/science/wfdb`. Otherwise, install WFDB from source using the instructions on their website.
* Edit [`cfg/rhrv_config.m`](https://github.com/avivrosenberg/rhrv/blob/master/cfg/rhrv_config.m). Make sure the `wfdb_path` variable points to the directory where you installed the WFDB binaries.

## Usage
Exaple of calculating HRV measures for a PhysioNet record (in this case from [`mitdb`](https://www.physionet.org/physiobank/database/mitdb/)):
```
>> rhrv('db/mitdb/111', 'window_minutes', 15, 'plot', true);
```
Will give you:
```
[0.000] >> rhrv: Reading ECG signal from record db/mitdb/111...
[0.990] >> rhrv: Signal duration: 30.066717 [min] (2115 samples)
[0.990] >> rhrv: Pre-processing signal...
[1.140] >> rhrv: 10 intervals were filtered out
[1.150] >> rhrv: Analyzing window 2 of 2...
[4.590] >> rhrv: Analyzing window 1 of 2...
[7.970] >> rhrv: Building output table...
             AVNN        SDNN       RMSSD       pNN50     LF_to_TOT    HF_to_TOT    LF_to_HF    alpha1     alpha2       beta        mse_a      mse_b 
            _______    ________    ________    _______    _________    _________    ________    _______    _______    ________    _________    ______
    1       0.85886    0.031027      0.0337    0.14313    0.066431     0.79515      0.083546    0.60966    0.88153    -0.51973    -0.016803    1.5393
    2       0.84181    0.040139    0.031761    0.12299    0.071112      0.6483       0.10969    0.67389     1.0589    -0.99082     0.019299    1.2907
    Avg.    0.85033    0.035583    0.032731    0.13306    0.068772     0.72172      0.096618    0.64178    0.97019    -0.75527    0.0012478     1.415
[8.550] >> rhrv: Finished processing record db/mitdb/111.

```
The `window_minutes` parameter allow splitting the signal into windows and calculating all metrics per window. You can pass in an empty array `[]` to disable spliting.

Example plots you can generate:
* NN interval time series (with filtering)
  ![Example NN spectrum](https://github.com/avivrosenberg/rhrv/blob/master/img/example_nn.png)
* Spectrum of NN interval time series
  ![Example NN spectrum](https://github.com/avivrosenberg/rhrv/blob/master/img/example_spectrum.png)
* HRV Metrics
  ![Example NN spectrum](https://github.com/avivrosenberg/rhrv/blob/master/img/example_hrv.png)

## Attribution
Some of the code in `lib/` was created by others, used here as dependencies. Original author attribution exists in the source files.

## Contributing
Feel free to send pull requests or open issues.