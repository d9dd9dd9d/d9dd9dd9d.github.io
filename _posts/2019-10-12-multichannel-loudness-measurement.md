---
layout: post
title:  "Multichannel Loudness Measurement Algorithm"
categories: Audio
tags: Loudness measurement
authors: Mark
---

* content
{:toc}

# Introduction

This Recommendation specifies audio measurement algorithms for purpose of determining subjective programme loudness, and true-peak signal level.

# Algorithm

## Objective multichannel loudness measurement algorithm

The algorithm consists of four stages:
1. "K" frequency weighting;
2. mean square calculation for each channel;
3. channel-weighted summation (surround channels have larger weights, and the LFE channel
is excluded);
4. gating of 400 ms blocks (overlapping by 75%), where two thresholds are used:
	1. the first at −70 LKFS;
	2. the second at −10 dB relative to the level measured after application of the first threshold.