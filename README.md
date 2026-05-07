# TED Talk Rhetorical Strategies Annotated Dataset

A span-level annotated dataset of the rhetorical devices in TED talk transcripts. Each talk was chunked using the natural newline boundaries in the native TED transcription, and annotations mark spans within those chunks. Data is provided in JSON format for easy Label Studio upload.

Datasets were human and LLM-annotated with a team of 3 annotators, 3 student researchers, one Gemma-4-31B and one Gemma-3-12B.

## Repository Structure

```
dataset/
├── tier1-niche/
│   └── *.json
├── tier2-standard/
│   └── *.json
├── tier3-popular/
│   └── *.json
└── README.md
```

Each tier directory contains one JSON file per talk, organized by view count: **tier1-niche** (lower), **tier2-standard** (mid-range), and **tier3-popular** (high).

## Citation

If you use this dataset, please cite:

```bibtex
@misc{brezgis2026ted,
    title  = {TED Talks Rhetorical Strategies Annotated Dataset},
    author = {Brezgis, Anna and [Edith's last name] and [Amanda's last name]},
    year   = {2026},
    url    = {https://github.com/brezgis/TED-talks-rhetorical-strategies-annotated-dataset}
}
```

## Acknowledgments

This dataset was created as part of coursework for COSI 230B at Brandeis University.
