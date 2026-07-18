# TED Talks Rhetorical Strategies Annotated Dataset

A span-level annotated dataset of rhetorical devices in TED talk transcripts. The corpus comprises **399 annotation files covering 392 talks**, with **61,705 labeled spans** in total. Each transcript was segmented at the natural newline boundaries of the native TED transcription, and annotations mark character spans within those paragraph-level chunks. Files are Label Studio-style JSON annotation exports and can be re-imported into Label Studio directly.

Annotations were produced by a team of six human annotators (student researchers) together with locally run Gemma-series LLM annotators: 18 files are human-annotated and 381 are machine-annotated.

## Repository structure

```
tier1-niche/       133 files  (lower view counts)
tier2-standard/    135 files  (mid-range view counts)
tier3-popular/     131 files  (high view counts)
```

Talks are stratified into three tiers by view count. Each JSON file holds the complete annotation of one talk by one annotator. Most files are named by the talk's TED slug; where a talk was annotated independently by more than one person during calibration, the files carry a `__<annotator>` suffix (four talks have multiple annotation files).

## Data format

Each file is a single JSON object with the following top-level fields:

| Field | Type | Description |
| --- | --- | --- |
| `task_id` | int | Label Studio task identifier |
| `title` | str | Talk title |
| `author` | str | Speaker name |
| `url` | str | TED.com talk page |
| `annotation_id` | int | Label Studio annotation identifier |
| `completed_by` | int | Numeric annotator ID (1–6: human annotators; 7: LLM annotator) |
| `created_at`, `updated_at` | str | ISO 8601 timestamps |
| `lead_time` | float | Annotation time in seconds |
| `project_id`, `project_label` | int, str | Label Studio project metadata (absent in a few early files) |
| `result` | list | Span annotations (see below) |

Each element of `result` is one labeled span:

```json
{
  "value": {
    "start": "1",
    "end": "1",
    "startOffset": 0,
    "endOffset": 117,
    "text": "I'm going to start by telling you the same piece of information twice, ...",
    "paragraphlabels": ["REP"]
  },
  "id": "ZMWRffoB-X",
  "from_name": "label",
  "to_name": "text",
  "type": "paragraphlabels",
  "origin": "manual"
}
```

- `start` / `end` — zero-based index of the transcript paragraph containing the span, encoded as a string. In the current data `start` always equals `end`; spans never cross paragraph boundaries.
- `startOffset` / `endOffset` — character offsets of the span within that paragraph.
- `text` — the surface text of the span.
- `paragraphlabels` — the assigned label (always a single-element list).

The raw transcripts are not distributed as separate files; the text of every annotated span is embedded in the `text` field. In the machine-annotated files, spans typically cover every paragraph of the talk (paragraphs without a rhetorical device are labeled `NONE`).

## Annotation scheme

| Label | Spans | Description |
| --- | ---: | --- |
| `NONE` | 38,384 | No rhetorical device present |
| `FIG` | 8,627 | Figurative language (metaphor, simile, and related figures) |
| `ENG` | 7,831 | Direct audience engagement (e.g., second-person address) |
| `REP` | 3,535 | Repetition |
| `INCOMPLETE` | 1,961 | Annotation-status meta-label |
| `HUMOR` | 1,310 | Humor |
| `UNCERTAIN` | 45 | Annotator uncertainty meta-label |

An additional 12 spans carry legacy labels from early calibration rounds (`SIM`, `MET`, `SIMILE`, `PARALLEL`, `HYPER`, `Rhetorical question`). A standalone annotation-guidelines document is not included in this repository; the glosses above are brief working definitions derived from the data.

## Loading the data

```python
import glob, json
import pandas as pd

rows = []
for path in glob.glob("tier*/*.json"):
    talk = json.load(open(path))
    for span in talk["result"]:
        v = span["value"]
        rows.append({
            "title": talk["title"],
            "author": talk["author"],
            "paragraph": int(v["start"]),
            "start": v["startOffset"],
            "end": v["endOffset"],
            "label": v["paragraphlabels"][0],
            "text": v["text"],
        })

df = pd.DataFrame(rows)
print(df["label"].value_counts())
```

## Transcript source and licensing

All transcripts originate from [TED.com](https://www.ted.com/); the source page for each talk is recorded in the file's `url` field. TED talk content is copyright TED Conferences LLC and is made available under the Creative Commons BY–NC–ND 4.0 license in accordance with the [TED Talks Usage Policy](https://www.ted.com/about/our-organization/our-policies-terms/ted-talks-usage-policy); the transcript excerpts embedded in this dataset remain subject to those terms, including the non-commercial restriction. The annotations themselves are released under the MIT License (see [LICENSE](LICENSE)).

## Citation

If you use this dataset, please cite:

```bibtex
@misc{brezgis2026ted,
    title  = {TED Talks Rhetorical Strategies Annotated Dataset},
    author = {Brezgis, Anna},
    year   = {2026},
    url    = {https://github.com/brezgis/TED-talks-rhetorical-strategies-annotated-dataset}
}
```

## Acknowledgments

This dataset was created as part of coursework for COSI 230B at Brandeis University. Thanks to the fellow student annotators who contributed annotations.
