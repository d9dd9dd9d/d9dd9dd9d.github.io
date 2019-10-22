# Multichannel Loudness Measurement Algorithm


This is a summary of article [Loudness measurement](https://d9dd9dd9d.github.io/Docs/audio/loudness/R-REC-BS.1770-4-201510-I!!PDF-E.pdf).

## Introduction

Many audio DSP algorithms require information on loudness of current track. Here we present a standard audio measurement algorithm for determining audio loudness, and true-peak signal level.

## Algorithm

### Multichannel loudness measurement algorithm

![](https://d9dd9dd9d.github.io/Docs/audio/loudness/loudness.png)

The algorithm consists of four stages:
1. "K" frequency weighting (concatenation of the
stage 1 filter to compensate for the acoustics effects of the head, and the stage 2 filter, the RLB
weighting);
2. Mean square calculation for each channel;
3. Channel-weighted summation (surround channels have larger weights, and the LFE channel
is excluded);
4. Gating of 400 ms blocks (overlapping by 75%), where two thresholds are used:
	1. The first at −70 LKFS;
	2. The second at −10 dB relative to the level measured after application of the first threshold.

**The last step can be omitted for simplicity.**

### Accurate measurement of "true-peak" level

![](https://d9dd9dd9d.github.io/Docs/audio/loudness/true_peak.png)

The stages of processing are:
1. Attenuate: 12.04 dB attenuation (2 bit right shift)
2. Upsample to 192kHz (48kHz base samplerate)
3. Lowpass filter
4. Absolute value
5. Conversion to dB TP

Continuous tones which are close to low-integer factors of the sampling frequency will underread on peak-sample meters because the beat frequency (the difference between n.ftone and fs) is low compared to the reciprocal of the decay rate of the meter:
![](https://d9dd9dd9d.github.io/Docs/audio/loudness/mis_peak.png)

For individual transients, the higher the frequency content of the transient, the larger the potential under-read.

Because real sounds generally have a spectrum which falls off towards higher frequencies, and
because this does not change with increasing sampling frequency, peak-sample meter under-read is
less severe at higher original sampling frequencies. 

**The conventional peak-sample meter with a logarithmic decay for simplicity.**