# connector.ttn

Homey app that bridges The Things Network (TTN) LoRaWAN uplinks to Homey flow automations — receives decoded payloads via webhook and exposes them as flow trigger cards.

## Key Technologies

- **Platform:** Homey SDK v3 (Node.js)
- **Protocol:** LoRaWAN via The Things Network V3 Stack
- **Integration:** TTN webhook → Homey cloud webhook → Homey flows
- **Published:** [Homey App Store](https://homey.app/en-us/app/com.connector.ttn/The-Things-Network/)

## How It Works

1. The app registers a Homey cloud webhook endpoint and generates a unique authentication header
2. Configure the webhook URL and header in the TTN application console
3. On each uplink, TTN's payload formatter decodes the binary LoRa frame and sends JSON to the webhook
4. The app extracts device ID, application ID, and up to 4 decoded payload fields (`state1`, `state2`, `value1`, `value2`)
5. Three flow trigger cards fire: any device, specific device ID, or specific application ID

## Getting Started

Install from the [Homey App Store](https://homey.app/en-us/app/com.connector.ttn/The-Things-Network/) or sideload from source:

```bash
homey app run
```

Copy the generated webhook URL and x-user-id header into your TTN application's webhook settings.

---

Built by Owen O'Hehir — embedded Linux, IoT, Matter & Rust consulting at [electronicsconsult.com](https://electronicsconsult.com). Available for contract and consulting work.
