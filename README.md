##TED Talk Rhetorical Strategies Annotated Dataset##

A span-level annotated dataset of the rhetorical devices in TED talk transcripts. Each talk was chunked using the natural newline boundaries in the native TED transcription, and annotations mark spans within those chunks. Data is provided in JSON format for easy label-studio upload.

Datasets were human and LLM-annotated with a team of 3 annotators, 3 student researchers, one Gemma-4-31B and one Gemma-3-12B. 

##Repository Structure##

dataset/
├── tier1-niche/          # Lower-view-count talks
│   └── *.json            # One JSON file per talk
├── tier2-standard/       # Mid-range talks
│   └── *.json
├── tier3-popular/        # High-view-count talks
│   └── *.json
└── README.md

# If you use this dataset, please cite #

@misc{brezgis2026ted,
    title  = {TED Talks Rhetorical Strategies Annotated Dataset},
    author = {Brezgis, Anna and [Edith's last name] and [Amanda's last name]},
    year   = {2026},
    url    = {https://github.com/brezgis/TED-talks-rhetorical-strategies-annotated-dataset}
}

# This datset was created as part of coursework for COSI 230B at Brandeis University*

