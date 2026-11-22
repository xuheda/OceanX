# Gallery capture notes

Updated 2026-11-22. The scene and sensor images in the public README were captured
from OceanX in November 2026. Each file is a byte-for-byte copy of its original
lossless PNG. Images have not been upscaled, sharpened, recolored, or generated.
Click a README image to inspect its original pixels.

## Native images

| Image | Native dimensions | Capture type |
|---|---|---|
| [surface-air-fleet.png](rendering/surface-air-fleet.png) | 1920 × 1080 | Surface and airborne fleet viewport |
| [underwater-fleet.png](rendering/underwater-fleet.png) | 1920 × 1080 | Underwater fleet viewport |
| [underwater-camera.png](rendering/underwater-camera.png) | 1280 × 720 | Published underwater camera frame |
| [surface-camera.png](rendering/surface-camera.png) | 1280 × 720 | Published surface camera frame |
| [air-camera.png](rendering/air-camera.png) | 1280 × 720 | Published airborne camera frame |
| [sonar-scene.png](rendering/sonar-scene.png) | 1920 × 1080 | Acoustic showcase viewport |
| [forward-looking-sonar.png](rendering/forward-looking-sonar.png) | 960 × 720 | Production forward-looking sonar panel |
| [mechanical-scanning-sonar.png](rendering/mechanical-scanning-sonar.png) | 960 × 720 | Production mechanical scanning sonar panel |
| [side-scan-sonar.png](rendering/side-scan-sonar.png) | 960 × 720 | Production side-scan sonar panel |

The 1920 × 1080 scene views are displayed at up to 960 pixels wide. Optical
frames retain 1280 × 720 pixels and are displayed at up to 640 pixels wide.
The 960 × 720 sonar panels were rendered by the application's production widgets
at double DPI and are displayed at up to 480 pixels wide. Original logo and
vehicle catalog assets are retained; SVG badges are vector graphics.

## What the captures show

- The surface/air and underwater fleet views show the same static multi-domain
  group in the authored IntegratedScene. They are external viewpoints, distinct
  from the onboard camera frames. World Terrain is described separately in the
  README and is not the source of these images.
- The underwater, surface, and airborne cameras independently sample their
  respective views at a configured 1280 × 720 and 20 Hz. These images preserve
  the published sensor pixels. Their exposure times differ; configured sample
  rate does not establish measured runtime throughput.
- The acoustic scene contains actual vehicle and obstacle geometry. The
  forward-looking and mechanical sonar panels observe stationary targets.
  The side-scan history was captured from a separate moving carrier. Range
  scales, status labels, and echo intensities are the original application
  output, and the three devices have different sampling times and patterns.
- These captures illustrate rendering and sensor observation. They do not
  establish that an autonomous coordination or landing task has completed.

## SHA-256

The following hashes identify the exact published PNGs:

```text
889a5fab9bfb5a5463ab06b2195f6468804d497537e7a15c1a9d63d12f8e3807  surface-air-fleet.png
ead20b2325d02f3e73e157f86fc6ff11db292309343a5affbc16147c9f097368  underwater-fleet.png
4083f5d8c4a530b2561be1b4b0d5efe86dee1dcb3f2304913e701d1b6d97e02b  underwater-camera.png
f802444f6e653a111c7b97282d169302e7320409310db4f626d98fe027928549  surface-camera.png
7e707b662a983357a3f3a01e747d915d0357164b8828cddb46d475afe54da97a  air-camera.png
ddef58d62b921db794897f6b32990966d3e433b4790755913e994fbd90d03175  sonar-scene.png
57fa7da62bcfc2e51a81609ec04bc06cc174e43b36656e4609a2e42b065e4af6  forward-looking-sonar.png
9fcd0adc07bc143426841707a3fc2193d3c9c34a50f420d55dfc694620f24261  mechanical-scanning-sonar.png
5b134a89ad12488f566a9a939d5610ce9863c7426396e55e2cac5dacb8a4fa58  side-scan-sonar.png
```
