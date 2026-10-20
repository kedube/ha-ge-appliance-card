# GE Appliance Card for Home Assistant

A washer and tumble dryer card for **GE Appliances** machines (**GE, GE Profile, Café**) that use the
[GE Home (SmartHQ)](https://github.com/simbaja/ha_gehome) integration.

The machine is drawn on the card with a progress ring round the door, the time left on its display, and the
laundry turning while it runs. When Remote Enable is on, you can start the cycle you chose on the machine from the card.

> **Beta.** The GE support was written by [@kedube](https://github.com/kedube), and the card is tested against
> simulated states. If you try it, please [open an issue](../../issues) with your model and what you see.
> That's how it gets out of beta.

Sister card of the [hOn washer & dryer card](https://github.com/spikey1965/ha-hon-appliance-card) (Haier, Candy, Hoover),
the [Candy card](https://github.com/spikey1965/ha-candy-appliance-card) and the [Miele card](https://github.com/spikey1965/ha-miele-appliance-card).

![Washer running](images/running.png)

## What it shows

- Status: Off, Ready, Scheduled (delayed start), Running with the phase (Filling, Washing, Rinse, Spin, Drying, Cooldown ...), Paused, Finished
- Time left on the machine's display, the end time, and a progress ring round the door
- The cycle, plus wash temperature and spin (washer) or heat and dryness (dryer)
- The door, the washer's door lock (if your machine has one) and Remote Enable
- A **Start** button for the cycle chosen on the machine. It asks before starting.

![Ready to start](images/ready.png)

**Limits that come from the integration:**
- GE Home can't pause, resume, stop or switch off a machine, or choose a cycle. Choose the cycle on the machine, press
  **Remote Enable**, then **Start** on the card.
- GE only reports the time left. The progress ring uses Home Assistant's history to find when the cycle started, so it
  stays right after a pause. History is on unless you've excluded the machine-state sensor from the recorder.
- No cycle count, because GE doesn't report one.

## Requirements

- Your washer and/or dryer in the **SmartHQ** app on your phone.
- **[GE Home (SmartHQ)](https://github.com/simbaja/ha_gehome)** version 2025.12 or newer, from HACS. Restart Home
  Assistant, then add **GE Home (SmartHQ)** under **Settings → Devices & services**.

## Install

**HACS:** HACS → ⋮ → Custom repositories → add `https://github.com/spikey1965/ha-ge-appliance-card`, type **Dashboard**.
Then find "GE Appliance Card", download it, and reload the browser. While it's a pre-release, open it in HACS
and switch on "Show beta versions" if it isn't offered.

**Manual:** copy `ge-appliance-card.js` to `/config/www/`, then add a dashboard resource
(Settings → Dashboards → ⋮ → Resources): `/local/ge-appliance-card.js?v=0.1.0`, type **JavaScript module**.

It can sit alongside the hOn card. They use different card names.

## Use

```yaml
type: custom:ge-appliance-card
appliance: washer
```

```yaml
type: custom:ge-appliance-card
appliance: dryer
```

The card finds your machine by itself: the first GE washer (or dryer, with `appliance: dryer`).

GE entity IDs start with the machine's serial number, for example `sensor.zl123456g_laundry_machine_state`, or with its
MAC address (`sensor.aa_bb_cc_dd_ee_ff_laundry_machine_state`), depending on the **Device identifier** in GE Home's
**Configure** options. If you have more than one GE washer or dryer, search **Settings → Entities** for
`laundry_machine_state` and set `entity` to the part before `_laundry_machine_state`:

```yaml
type: custom:ge-appliance-card
appliance: dryer
entity: gd987654
```

| Option | Default | What it does |
|---|---|---|
| `appliance` | `washer` | `washer` or `dryer` |
| `entity` | first GE machine | the serial number or MAC address at the start of the entity IDs |
| `name` | Washer / Dryer | card title |
| `colour` | blue / orange | accent colour, e.g. `"#4fb3ff"` |
| `names` | none | your own cycle names, e.g. `Normal Or Mixed Load: Everyday` (as GE Home reports it, or as the card shows it) |
| `style` | `drawn` | `photo` = a more detailed illustration |
| `finish` | `white` | machine colour: `white`, `graphite` or `black` |
| `max_width` | `440px` | |
| `images` | built in | a folder with your own `washer.svg`, `dryer.svg` and `laundry-drum.webp` |

![Dryer cooling down, photo style in graphite](images/dryer.png)

## Troubleshooting

- **No Start button:** press Remote Enable on the machine, and make sure it's on and showing Ready (Standby). There's
  no Start while a cycle is running, because GE Home can't send anything else.
- **Card shows Off and "—" everywhere:** the card didn't find a GE washer (or dryer). Set `entity` as shown above.
- **Something else looks wrong:** please open an issue with your model and a screenshot of Developer Tools → States
  filtered for `laundry`.

## Thanks

GE support written by **[@kedube](https://github.com/kedube)**. Thanks to **simbaja** for the GE Home integration.
Card by [My Media Build](https://mymediabuild.uk). If you find it useful, you can [buy me a coffee](https://buymeacoffee.com/bobgreggr) ☕ (no pressure).

## Licence

MIT. Not affiliated with or endorsed by GE Appliances, Haier or SmartHQ.
