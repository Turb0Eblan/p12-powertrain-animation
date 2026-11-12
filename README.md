# P12 Hybrid Powertrain Animation

Animated technical infographic of the **Project 12** fuel cell + battery hybrid powertrain
(Overall Multidisciplinary Project, Grenoble INP – Ense3).

Fuel cell stack → DC/DC boost → 72 V DC bus → battery pack + motor/generator, driven through a
standard driving cycle scaled to a 1 kW peak. Two analog gauges (fuel cell power, battery SOC),
a changing formula, and a power-vs-time chart with a synchronised runner.

## Run

Open `P12_powertrain_animation.html` in any current browser. It is a single self-contained file
(inline SVG + JS, no dependencies).

Controls: play/pause (space), restart, driving cycle (FTP-75 / HWFET / US06 / custom CSV),
speed 1×–120× (`[` / `]`), time scrubber, PNG frame export, video recording (MP4 or WebM).

Custom profile: CSV with two columns, time [s] and power [W]. It is rescaled to a 1 kW peak and
regeneration is clipped at −300 W.

## Parameters

From the team's `FC_init.m`, `Battery_init.m`, `DCDC_init.m`, `Motor_init.m`:

| Subsystem | Values | Status |
|---|---|---|
| Fuel cell | 48 cells, 53 cm², i_lim ≈ 74 A; 52 V / 20.8 A / 1.09 kW at R = 2.5 Ω | validated in Simulink |
| Battery | 13S2P Samsung INR18650-20Q, E0 ≈ 47.2 V, R_pack = 162.5 mΩ, 4.0 Ah | hand-calc |
| DC/DC | averaged boost, η = 0.95, 52 V → 72 V (proposed bus) | hand-calc |
| Motor | P_peak ≈ 1 kW, P_regen ≈ 300 W, η_inv = 0.95 | hand-calc |

## Limitations

- Driving cycles are EPA speed schedules converted to traction power with a generic small-car
  road-load model, only to shape the trace before scaling. The project's official v1 profile is
  the FTP-75 fuel cell power trace from the reference model; load it as CSV to use it.
- The fuel cell / battery power split is a simple rule-based strategy written for this
  illustration (low-pass filtered demand with SOC correction). It is not part of the team model.
- Fuel cell current is shown as P / 52 V (no polarisation curve).
- Battery-side converter placement (T1–T4) is still open; the drawing shows the battery on the bus.

## Data sources

`profiles/` holds the EPA dynamometer driving schedules (speed in mph, 1 s steps):

- `uddscol.txt` – UDDS; FTP-75 = UDDS + first 505 s repeated (10-min soak omitted)
- `hwycol.txt` – HWFET
- `us06col.txt` – US06

Source: U.S. EPA, [Dynamometer Drive Schedules](https://www.epa.gov/vehicle-and-fuel-emissions-testing/dynamometer-drive-schedules).
