# Beyond Text-Based Interactions

To wrap up this tutorial, there's a sort of elephant in the room that has been cropping up more and more in TTM generation discussions: do we *even want text* as a control method for music generation? This is often paired with a well circulated and unattributed quote:

> *"Writing about Music is like Dancing about Architecture"*

To put this more formally, text's main failure as a control medium stems from its lack of specificity and inability to address fine-grained, musically salient details:

1. **Low Specificity**: As most text caption datasets are generally boosted from metadata, which itself contains only high level information like genre, mood, function, and at best instrument-level tags, the overall mapping from text-to-music becomes a strongly one-to-many function (as an exercise, one can imagine the number of songs that might fit the caption "exciting and upbeat rock music with drums and guitar"). This means that TTM's rarely learn to follow text for anything more than genre-level correlations, as within a given high level mode all the text captions are similar in terms of musical content.
2. **Inability to Address Fine-Grained Details**: Whiile text is great at describing high-level information and even instrument classes, it's overall resolution is quite coarse and fails at describing fine-grained features that change at high resolutions. This is particularly bad for music, as many musical controls (volume, melody, chords, rhythm) require fine temporal-resolutions to describe and control accurately.
3. **Mismatch with Musically Salient Use-Cases**: While text-to-music generation seeks to upend current creative workflows and create new avenues for artistic expression, the textual input doesn't particularly fit in with many musicians' workflows and use-cases, who tend to operate more directly at the the audio level (i.e. given some audio, compose some other audio) and may not want full text2song systems (in favor of loopers, sample generators, accompaniment systems, and things that can run in real-time).

So given all this, where do we go from here? Luckily, there's a growing body of work in the MIR community venturing into more bespoke controls for these state-of-teh-art audio-domain music generation systems. We'll talk about a few broad categories, and notably highlight how there are both *training-based* control methods, where we build new music generation systems, and *training-free* control methods, where we can repurpose TTM systems without training for additional controls.


## Audio2Audio Controls

Maybe the most intuitive (and computationally simplest) control is to simply have the music be conditional on other music! This can take a number of forms, such as:

- Music Accompaniment Generation (i.e. drums->drums + guitar)
- Vocal Accompaniment Generation
- Vocal Morphing / timbre transfer (i.e. modulating music with voice controls)
- Iterative editing (i.e. given existing music outputs, *change* it conditional on the original audio)

On the training-based side, many of accompaniment such as MSDM {cite}`mariani2023multi`, SingSong {cite}`donahue2023singsong`, StemGen {cite}`parker2024stemgen`, and Diff-a-Riff {cite}`nistal2024diff` train models from scratch that directly model the conditional audio to audio (or joint across stems) distribution, allowing the reference audio to influence the generation through many of the ways we've seen so far (in particular, cross-attention and channel-wise concatenation). Even the older RAVE model {cite}`caillon2021rave` is a good example a of training-based audio2audio model addressing practical considerations, as its hyper optimized design allows for real time vocal morphing and timbre transfer.

On the training-free side, a number of audio2audio works focus on training-free *editing* with TTM models, as existing TTMs can be used as reasonably strong generative priors. These can range from the more simple VampNet-style periodic inpainting {cite}`garcia2023vampnet` and Stable Audio Open {cite}`evans2024open` init audio style transfer, which simply mask out regions of the audio and regenerate them with the model, using the TTM systems prior as a way for creative editing. There also exist more bespoke guidance/gradient-based editing like DDPM Inversion {cite}`Manor2024ZeroShotUA`, MusicMagus {cite}`zhang2024musicmagus`, and DITTO-(1/2) {cite}`novack2024ditto,Novack2024DITTO2DD`, which leverage invertibality or differentiation through generation to change the outputs of the model. Note that this is where diffusion-based systems particularly pull ahead of LM-based approaches, as the discrete nature of LM-based generation makes these inference-time tricks quite unstable and unusable.


## Abstract Musical Controls

Even more interesting for musicians is truly breaking out of *any* particular audio or text based mold, allowing for fully custom control mechanisms that are aligned with musical practice but may be hard to describe in words. For example:

- Time-varying musical intensity (i.e. volume as well as harmonic and rhythmic density)
- Time-varying melody
- Time-varying chords
- Overall musical structure (e.g. verse, chorus)

As we've seen a number of times now, training-based methods such as Music ControlNet {cite}`wu2023music`, JASCO {cite}`Tal2024JointAA`, and Mustango {cite}`melechovsky2023mustango` can input controls by defining some embedding function to extract latent vectors for the controls and letting them modulate the audio generation through the standard means. Additionally, training-free methods like DITTO {cite}`novack2024ditto,Novack2024DITTO2DD`, SMITIN {cite}`koo2024smitin`, and Guidance Gradients {cite}`Levy2023ControllableMP` can leverage similar differentiability through sampling to guide the generation process. 

