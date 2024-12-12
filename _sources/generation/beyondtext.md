# Beyond Text-Based Interactions

To wrap up this tutorial, there's a sort of elephant in the room that has been cropping up more and more in TTM generation discussions: do we *even want text* as a control method for music generation? This is often paired with a well circulated and unattributed quote:

> *"Writing about Music is like Dancing about Architecture"*

To put this more formally, text's main failure as a control medium stems from its lack of specificity and inability to address fine-grained, musically salient details:

1. **Low Specificity**: as most text caption datasets are generally boosted from metadata, which itself contains only high level information like genre, mood, function, and at best instrument-level tags, the overall mapping from text-to-music becomes a strongly one-to-many function (as an exercise, one can imagine the number of songs that might fit the caption "exciting and upbeat rock music with drums and guitar"). This means that TTM's rarely learn to follow text for anything more than genre-level correlations, as within a given high level mode all the text captions are similar in terms of musical content.
2. **Inability to Address Fine-Grained Details**: Whiile text is great at describing high-level information and even instrument classes, it's overall resolution is quite coarse and fails at describing fine-grained features that change at high resolutions. This is particularly bad for music, as many musical controls (volume, melody, chords, rhythm) require fine temporal-resolutions to describe and control accurately.
3. **Mismatch with Musically Salient Use-Cases**: