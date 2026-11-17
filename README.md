[![make.fun](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/mmr9759/gta6-twitch-ccu-forecast-01/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/mmr9759/gta6-twitch-ccu-forecast-01/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)

# GTA VI launch-day Twitch viewers — hourly forecast

Forecast how many people watch Grand Theft Auto VI live on Twitch in each
hour of its release day, 2026-11-19 (UTC), after the first hour. The entry
program (`main.ts`, Node 22, standard library only) reads a case file from
the input directory and writes five quantiles (0.05, 0.25, 0.5, 0.75, 0.95)
of each hour's concurrent-viewer count, hours 2 to 24, to the output
directory.

## Method

A big console launch follows the rolling midnight unlock: quiet in the
small UTC hours, then a climb as the US unlocks into a late-day peak. The
shape depends strongly on how big hour 1 already is, so the forecaster is
calibrated on the case's own comparable past launches: for each forecast
hour it takes every comparable launch's release-day ratio
viewers(h)/viewers(1), weights each comparable by how close its own hour-1
print is to this case's hour-1 print (Gaussian kernel on log hour-1,
bandwidth 1.0), and uses the weighted median log-ratio as the centre and
the weighted spread of log-ratios as the log-spread. The result is an
honest, calibrated set of quantiles — validated offline by leave-one-out
over the comparables (mean pinball loss 0.40 overall, 0.21 on launches
with hour-1 >= 50,000 viewers, against 0.93 and 0.25 for the house
baseline on the same sets).

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out node main.ts
```
