# TARDIS

[TARDIS](https://github.com/SMLC-NYSBC/napari-tardis_em) — *Transformer-bAsed Rapid Dimensionless Instance Segmentation* — is a deep-learning framework that automatically and accurately annotates **filaments and membranes**. Its pretrained models perform **instance** segmentation (each filament gets its own label, not just a "filament/not-filament" mask) across modalities — cryo-ET, cryo-EM, and plastic-section ET, in both 2D and 3D — in a single framework **without retraining or tuning**. It's especially strong on microtubules and other cytoskeletal filaments, with large reported accuracy gains over older tools.

It ships both as a CLI (`tardis-em` on [PyPI](https://pypi.org/project/tardis-em/)) and as a [napari plugin](https://napari-hub.org/plugins/napari-tardis-em.html). Developed by [Robert Kiewisz](https://github.com/RRobert92) and colleagues at NYSBC. Paper: [Kiewisz et al., bioRxiv 2024](https://www.biorxiv.org/content/10.1101/2024.12.19.629196v1).

!!! warning "Confirm before the workshop — TODO"
    Confirm this year's env, whether we run the CLI or the napari plugin, and the correct pretrained model name for our data.

## When to use it

* You need **individual filaments** segmented and separated (instance segmentation), not just a binary mask — e.g. for counting, length/orientation measurement, or per-filament downstream analysis.
* Microtubules, actin, and other filaments; also a strong membrane segmenter.

## Setup

```bash
# TODO: confirm env + install
conda activate tardis        # placeholder
# CLI:    pip install tardis-em
# napari: pip install napari-tardis_em ; then: napari
```

## Running TARDIS

!!! note "TODO: fill in concrete commands from the workshop machines"

=== "CLI"

    ```bash
    # TODO: confirm subcommand + flags for current tardis-em
    # tardis_mt   --path <tomogram>.mrc ...   # microtubules / filaments
    # tardis_mem  --path <tomogram>.mrc ...   # membranes
    ```

=== "napari plugin"

    1. Launch `napari`.
    2. Open the TARDIS plugin from the Plugins menu.
    3. Load the tomogram, choose the pretrained model (filament vs membrane), and run.

## Outputs

* Instance-labeled filaments (each filament a distinct label) and/or membrane segmentations.
* Point-cloud / coordinate outputs for the segmented instances, suitable for downstream measurement.

## Tips

* TARDIS instance labels pair nicely with morphometrics when you want per-filament statistics.
* Pretrained models cover common cases without tuning — but as always, check the result against the raw tomogram.
