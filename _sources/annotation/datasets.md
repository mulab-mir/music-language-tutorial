# Music Description Datasets

## Overview
Below we review existing music datasets that contain natural language text accompanying music items. These can be music descriptions such as captions, or other types of text such as question-answer pairs and reviews. Note that in many cases the audio isn't directly distributed with the dataset and may be subject to copyright. 

Among the caption datasets, only three feature fully human-written descriptions: MusicCaps, the Song Describer Dataset and YouTube8M-MusicTextClips.

| Dataset | Description | Size | Audio | Audio Length | Audio License| Text | Text License
| ------- |  ------ |    ---- | ---- | ---- | ---- | ---- | ---- | 
| [MusicCaps](https://www.kaggle.com/datasets/googleai/musiccaps) | Captions (by musicians) |  5.5k | ❌ <br> (YT IDs from AudioSet)| 10 sec | | Human-written |  | CC BY-SA 4.0 |
| [Song Describer Dataset](https://zenodo.org/records/10072001) | Captions (crowdsourced) |   1.2k  |  ✅ <br> (MTG-Jamendo) | 2 min | | Human-written |  | CC BY-SA 4.0 |
| [YouTube8M-MusicTextClips](https://zenodo.org/records/8040754) | Captions (crowdsourced) | 3.2k | ❌ <br> (YT IDs from YouTube8M) | 10 sec | | Human-written | | CC BY-SA 4.0 |
| [LP-MusicCaps](https://github.com/seungheondoh/lp-music-caps) | Captions (generated from tags) |  2.2M / 88k / 22k  |  ❌ <br> (MusicCaps, MagnaTagATune, and Million Song Dataset ECALS) | 30s / 10s| Synthetic | | CC-BY-NC 4.0 |
| [MusicQA](https://huggingface.co/datasets/mu-llama/MusicQA) | Question-answer pairs (generated from tags/captions) |     | ❌ <br> (MusicCaps, MagnaTagATune) | YT IDs | | Synthetic| |
| [MusicInstruct](https://huggingface.co/datasets/m-a-p/Music-Instruct) | Question-answer pairs (generated from captions) |  28k / 33k |YT IDs | | Synthetic| |
| [MusicBench](https://paperswithcode.com/dataset/musicbench) | Captions (expanded via text templates) |  53k  | ✅ | 10s | ❌ <br> (from MusicCaps) | Human-written + text templates | CC | 
|[MuEdit]() | Album reviews |  | ❌ | ❌ |
|[MARD]() | Album reviews | 264k| ❌ | ❌ |

## Music Caption Datasets
Focusing on the caption datasets, let's now review these in more detail.

### MusicCaps (MC)
{cite}`agostinelli2023musiclm`

[TODO]

```{warning}
[TODO]
```

### The Song Describer Dataset (SDD)
{cite}`manco2023song`

[TODO]

### YouTube8M-MusicTextClips (YT8M-MTC)
{cite}`mckee2023language`

[TODO]

### Datasets with synthetic text 
The three human-written caption datasets we have just seen often consistute the basis for other derived datasets that use text templates to extend the original captions (MusicBench), or LLM-enabled augmentation to transform them into question-answer pairs (e.g. MusicQA, MusicInstruct)

[TODO]

* LP-MusicCaps: captions generated from ground-truth tags via OpenAI's GPT-3.5 Turbo, 2.2M (MSD), 88k (MTT), 22k (MC) captions, 
* MusicBench: captions from the MusicCaps dataset expanded with features extracted from audio (chords, beats, tempo, and key)
* MusicQA: [TODO]
* MusicInstruct: [TODO]

## References

```{bibliography}
:filter: docname in docnames
```
