# ha-zen32-exhaust-fan-blueprint
A blueprint to use a [Zooz ZEN32 Scene Controller](https://www.getzooz.com/zooz-zen32-scene-controller/) to control an exhaust fan.

This gives four timers using the small buttons, and leave the big button for relay control (i.e. manual on and off)

[![Import Blueprint in Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fklovatt%2Fha-zen32-exhaust-fan-4-timers-plus-manual-blueprint%2Fmain%2Fzen32_exhaust_fan.yaml)

You can also see the discussion about the original version of this Blueprint [in the Home Assistant Community Blueprint Exchange](https://community.home-assistant.io/t/zooz-zen32-scene-controller-for-timed-exhaust-fan-control/452275). Perhaps a future discussion will open for this fork.

# Requirements
* Scene Controller device (Zooz Zen32 - either 700 or 800 series)
* Enable all 5 entities for LED Control - this allows for LED states to be read and only changed if required
* Enable entitity for relay LED Colour control - this allows the big button LED light colour to be read and only changed if required
* Fan Switch entity
* Virtual Button (helper) Entities for each of the four scene buttons - Allows for mockup of panel for testing via Lovelace GUI
* Fan timer (helper) entity
* Maximum fan-on duration (e.g. 30 minutes, 60 minutes) - Timers are calculated based on this value. Timer 1 is 1/6 of the value, Timer 2 is 1/3, TImer 3 is 1/2, Timer 4 is 1/1
* Debug level for logging - The blueprint allows for debug level to be set for the automation without affecting the default debug level for all of Home Assistant


* [Timer](https://www.home-assistant.io/integrations/timer/) entity to track time remaining for the exhaust fan 
* [Switch](https://www.home-assistant.io/integrations/switch/) entity to turn the fan on/off (can be the scene controller if wired that way)
* Device for the scene controller.

# Diagram

```
┌────────────────────────────────────┐
│                                    │
│ ┌─┐ Blue when off                  │
│ └─┘ Green when on w/ no time set   │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│                                    │
│ Manual on/off                      │
└────────────────────────────────────┘

┌────────────────┐  ┌────────────────┐
│     White w/   │  │     White w/   │
│ ┌─┐ time left  │  │ ┌─┐ time left  │
│ └─┘ <= Timer 1 │  │ └─┘ > Timer 1  │
│                │  │     <= Timer 2 │
│                │  │                │
│                │  │                │
│ 1/6 of max     │  │ 1/3 of max     │
└────────────────┘  └────────────────┘

┌────────────────┐  ┌────────────────┐
│     White w/   │  │     White w/   │
│ ┌─┐ time left  │  │ ┌─┐ time left  │
│ └─┘ > Timer 2  │  │ └─┘ > Timer 3  │
│     <= Timer 3 │  │     <= Timer 4 │
│                │  │                │
│                │  │                │
│ 1/2 max        │  │ max            │
└────────────────┘  └────────────────┘
```

# Contributing

## Setup

```
python3.10 -m venv .venv
source .venv/bin/activate

# Install Requirements
pip install -r requirements.txt

# One-Time Install of Commit Hooks
pre-commit install
```

## Testing

Tests are run with `pytest`.
