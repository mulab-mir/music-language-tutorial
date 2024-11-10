# Diffusion Model-based Text-to-Music Generation

While we could dedicate an entire tutorial to discussing how diffusion works in the context of generative audio (and in fact, others have this year at ISMIR!), here we present a condensed review of how diffusion works before jumping into how text conditioning can be built into these models, using [Stable Audio Open](https://huggingface.co/stabilityai/stable-audio-open-1.0) as a case study.

## Diffusion: Continuous Generation through Iterative Refinement

```{figure} ../img/generation/diff1.png
---
name: Generating Continuous-Valued Data
---
Unlike with language models that operate over discrete tokens, how should design our models to generate *continuous* data?
```


Unlike in the LM-based case where we wish to generate *discrete* tokens $x \in \mathbb{N}$,  the goal of diffusion is to generate some *continuous*-valued data $x \in \mathbb{R}$ (which is identical to the more classical models of VAEs and GANs). 

Formally, if our data comes from some distribution $\mathbf{x} \sim p(\mathbf{x})$, then the goal is to learn some  model that allows us to sample from this distribution $p_\theta(\mathbf{x}) \approx p(\mathbf{x})$. Practically speaking, in order to sample from the data distribution, with VAEs/GANs we parameterize our model as some generator $G_\theta$ such that:

$$
\mathbf{x} = G_\theta(z), \quad z \sim \mathcal{N}(0, \boldsymbol{I}),
$$

i.e. that we learn some model that transforms isotropic gaussian noise into our target data.

One of the **main** reasons why diffusion models have been so succesful at many generative media tasks over these classical models {cite}`dhariwal2021diffusion` (and why they are more controllable) is their ability of **iterative refinement**. In the above equation, the entire generation process occurs in a single model call. While this is certainly efficient (and many diffusion models have been conceptually reinventing GANs to capitalize on their efficiency {cite}`Kim2023ConsistencyTM,Novack2024PrestoDS`), this is *a lot* of work to be done in a single pass of the model, especially for high dimensional data! 

What would be useful is if we had a way to generate **part** of $\mathbf{x}$ in a given model call. This way, we could then call the model multiple times to fully generate $\mathbf{x}$ (and if you're paying attention, this sounds eerily similar to autoregression).

In order to build this "multi-step generator", we have to first introduce the concept of *corrupting* our data into noise (note: while this step doesn't fit as cleanly in our condensed diffusion intro, we encourage readers to check out more complete diffusion writeups that motivate the paradigm through a wider lens {cite}`Song2020ScoreBasedGM`). To start, we'll first adapt our notation to model a *diffusion* process from clean data to noise notated by the *timestep* $0\rightarrow T$, where $\mathbf{x}_0 \sim p_0(\mathbf{x}_0)$ is our clean data (i.e. $\mathbf{x} \sim p(\mathbf{x})$ previously) and $\mathbf{x}_T \sim p_T(\mathbf{x}_T)$ is pure Gaussian noise (i.e. $z$ previously). Then, we can define a diffusion process that gradually turns our clean data $\mathbf{x}_0$ into gaussian noise $\mathbf{x}_T$ through the stochastic differential equation (SDE):

$$
\mathrm{d}\mathbf{x} = f(\mathbf{x}, t)\mathrm{d}t + g(t)\mathrm{d}\boldsymbol{w},
$$

where $\boldsymbol{w}$ is a standard Weiner process (i.e. additive Gaussian noise) $f(\mathbf{x}, t)$ is the *drift* coefficient of $\mathbf{x}_t$ and $g(t)$ is the *diffusion* coefficient. If SDEs seem hard to parse, the important thing to take away is that the above equation defines a process $0\rightarrow T$ that gradually adds noise to our data, until there is only noise left. And for clarity, we will use $p_t(\mathbf{x})$ to denote the marginal probability density of $\mathbf{x}_t$.

```{figure} ../img/generation/diff2.png
---
name: Forward Diffusion Process
---
To start with diffusion models, we define a forward noising process that gradually corrupts our data into Gaussian noise.
```

The reason this is relevant at all is that a clever result from {cite}`anderson1982reverse` allows us to define a *reverse* diffusion process that transforms gaussian noise back into data, given by:

$$
\mathrm{d}\mathbf{x} = [f(\mathbf{x}, t) - g(t)^2\nabla_{\mathbf{x}}\log p_t(\mathbf{x})]\mathrm{d}t + g(t)\mathrm{d}\bar{\boldsymbol{w}},
$$

where $\bar{\boldsymbol{w}}$ is the reverse-time Weiner proccess and notably, $\nabla_{\mathbf{x}}\log p_t(\mathbf{x})$ is the *score function* of the marginal probability distribution of $\mathbf{x}_t$. In words, the score function defines a direction pointing towards higher density regions of the data distribution, which you can imagine is something like getting the derivative of a 1-D curved path but in high-dimensional space.

As we now have a way to define the *process* of converting noise to data, we can see that implicitly VAEs/GANs seek to learn a generator the *integrates* the above reverse-time SDE from $T$ to $0$, and thus learn a direct mapping from noise to data. 
The strength of diffusion models, however, comes instead from learning a *score model* $s_\theta(\mathbf{x}, t) \approx \nabla_{\mathbf{x}}\log p_t(\mathbf{x})$. This way, diffusion models iteratively solve the reverse-time SDE in multiple steps, in a sense walking through the reverse diffusion process at some fixed step size and checking the derivative at each point to determine where we should step next. In this way, diffusion models are able to iteratively refine the model output, gradually removing more and more noise from the starting isotropic Gaussian until our data is clear!

```{figure} ../img/generation/diff3.png
---
name: Diffusion Models vs. VAEs/GANs
---
While VAEs/GANs model the direct mapping from noise to data, diffusion models model the *score* of the reverse diffusion process, which allows us to generate data by walking along the reverse diffusion path using any off-the-sheld SDE solver.
```

If this all sounds like some weird version of how LMs perform autoregression, you'd be thinking about right! Sander Dieleman has a fantastic [blog post](https://sander.ai/2024/09/02/spectral-autoregression.html) on this conceptual similarity, and how one can imagine diffusion being autoregression, but in the *spectral* domain.

This concludes our intro on diffusion models, and while there's a lot of math here, as long as you understand the core idea that diffusion models approximate the ***gradient*** of the path from noise to data (rather than learning the path itself), you should be fine proceeding through this tutorial!


## Representation

Unlike the autoregressive language model approach, the exact input representation for diffusion-based TTM generation has varied a great deal since diffusion hit the scene in 2021. Below we list them, in *rough* chronological order:

1. Direct waveform modeling: $\mathbf{x}_0 \in \mathbb{R}^{f_s T \times 1}$, where $f_s$ is the audio's sampling rate and $T$ is the overall time in seconds. In words, we directly perform the diffusion process on the raw audio signal. This input representation is generally ***not*** used, both because the size of raw audio signals can get quite large (just 30 seconds of 44.1 kHz audio is over 1M floats!), and that diffusion just doesn't work as well on raw audio signals (and theres [good reason](https://sander.ai/2024/09/02/spectral-autoregression.html) for this).
2. Direct (mel)-Spectrogram modeling {cite}`zhu2023edmsound, Novack2024Ditto, Novack2024DITTO2DD, wu2023music`:  $\mathbf{x}_0 \in \mathbb{R}^{H \times W \times C}$, where $H$ and $W$ are the height and width of the audio (mel)-Spectrogram and $C$ is the number of channels (normally this is just 1, but if using complex spectrograms this can be 2). In this way, TTM diffusion proceeds almost identically to non-latent image diffusion, as we simply treat the audio spectrograms as "images" and run diffusion on these now 2D signals. As we cannot directly convert mel spectrograms back to audio, these models generally train {cite}`Zhu2024MusicHiFiFH` or use an off-the-shelf {cite}`wu2023music` *vocoder* $V(\mathbf{x}_0) : \mathbb{R}^{H \times W \times C} \rightarrow \mathbb{R}^{f_s T \times 1}$ to translate from the generated mel spectrogram back to audio.
3. Latent (mel)-Spectrogram modeling {cite}`liu2023audioldm, liu2023audioldm2, chen2023musicldm, forsgren2022riffusion`: $\mathbf{x}_0 \in \mathbb{R}^{D_h \times D_w \times D_c}$, where $D_h, D_w, D_c$ are the ***latent*** height, width, and number of channels after passing the spectrogram through a 2D **autoencoder** (and in general, $D_h \ll H, D_w \ll W$ for efficiency while $D_c > C$). This is perhaps the first design to really break the scene of TTM generation, with {cite}`forsgren2022riffusion` using the existing Stable Diffusion autoencoder and finetuning SD on spectrograms. This thus requires training a separate VAE $\mathcal{D}, \mathcal{E}$ before training the TTM diffusion model. Once trained, sampling from the model involves generating the latent representation with diffusion, passing this through the decoder $\mathcal{D}(\mathbf{x}_0): \mathbb{R}^{D_h \times D_w \times D_c} \rightarrow  \mathbb{R}^{H \times W \times C} $ and *then* passing that output through the vocoder $V$.
4. DAC-Style Latent Audio modeling {cite}`stableaudio, evans2024open,Novack2024PrestoDS`: $\mathbf{x}_0 \in \mathbb{R}^{D_T \times 1 \times D_c}$, where $D_T$ is the length of the compressed audio signal, as here we circumvent the vocoder and spectrogram VAE and instead use a **raw-audio VAE** to directly compress the audio into a latent 1D (but multi-channel, as $D_c$ is normally 32/64/96) sequence. Practically, this ends up being nearly identical to the discrete LM codecs like Encodec{cite}`defossez2022highfi` or DAC{cite}`kumar2023high`, with the only difference being that the discrete vector-quantization is replaced with a standard VAE KL regularization, thus giving us **continuous-valued latents** rather than discrete tokens. In fact, much of the rest of the training process and architecture remains the same (i.e. fully convolutional 1D encoder/decoder with snake activations, Multi-Resolution STFT discriminators, etc.). Thus, to sample from the model, we generate the latent representation and directly pass it through the decoder $\mathcal{D}$ to get the audio output. For the rest of the tutorial, we'll focus on this one, as it is what Stable Audio Open uses.

## Architecture

Concerning architecture design, most diffusion models have followed 1 of 2 broad categories: U-Nets and Diffusion Transformers (DiTs). In this work, we focus on DiTs, both because most modern diffusion models are adopting this modeling paradigm {cite}`stableaudio,evans2024open,Novack2024PrestoDS` and that DiTs are *much* simpler in terms of code design. A TTM DiT, in general, looks something like this:

```{figure} ../img/generation/dit.png
---
name: DiT Architecture
---
DiTs involve an initial patchifying and embedding layer for the noisy input data, as well as embedding for the noise level and conditions, and then pass this through a series of bidirectional transformer blocks (where the conditioning will modulate the internal representation of the noisy latent) before projecting back out to predict the clean latent.
```

In words, after the input latent representation is converted to "patches" (i.e. downsampling it further) and the input conditions (i.e. text, more on that later) and timestep/noise level are converted to their corresponding embeddings, a DiT simply is a stack of bidirectional transformer encoder blocks (i.e. like BERT) operating on this latent representation  (with the conditioning providing some form of modulation), followed by a final linear and de-patchifying layer to get our prediction. DiTs have a number of nice scaling properties over U-Nets, are able to handle variable length sequences a bit better, and notably allow for much cleaner code given the lack of manual residual down/up-sampling blocks.

## Conditioning

The big question is now, how does the text conditioning actually do anything in the model? Before hitting the model, the text prompt $\mathbf{c}_{\textrm{text}}$ first has to be converted from a string to some numerical embedding, which we'll call $\mathbf{e}_{\textrm{text}} = \textrm{Emb}(\mathbf{c}_{\textrm{text}})$, where $\textrm{Emb}$ is some embedding extraction function. In many cases, $\textrm{Emb}$ uses a pre-trained text backbone (such as CLAP or T5), followed by 1 or more linear layers to project the embedding to the correct size. After embedding, $\mathbf{e}_{\textrm{text}}$ is either a global embedding $\mathbb{R}^{d}$ or sequence level embedding $\mathbb{R}^{d \times \ell}$  where $d$ is the hidden dimension of the DiT and where $\ell$ denotes the token length of the text embedding (i.e. the text embedding can be extracted per-token, as is the case with T5). 

There are now multiple ways $\mathbf{e}_{\textrm{text}}$  can hit the main diffusion latent inside the model (which can all be combined), so we'll go over a few of them:

1. **Time-Domain Concatenation** (aka In-Context Conditioning or Prefix Conditioning): Here, we simply append the text condition to the diffusion latent sequence to get some new latent $\hat{\mathbf{x}} = [\mathbf{x}, \mathbf{e}_{\textrm{text}}] \in \mathbb{R}^{(D_T + \ell) \times 1 \times d}$, where $[\cdot]$ is the concatenation operation along the *time* axis (i.e. the sequence gets longer), and remove this extra token(s) after all the DiT blocks. In this way, the text operates on the diffusion latent through the DiT's self-attention blocks only, and causes minimal to moderate slowdowns depending on the length of the text embedding.
2. **Channel-Wise Concatenation**: Here, $\mathbf{e}_{\textrm{text}}$ is first project to have the same *sequence* length as the main diffusion latent, and then is concatenated along the *channel dimension* $\hat{\mathbf{x}} = [\mathbf{x}, \textrm{Proj}(\mathbf{e}_{\textrm{text}})] \in \mathbb{R}^{(D_T) \times 1 \times 2d}$ (i.e. the sequence gets *deeper* in a sense). This is generally not used that much for text conditioning (but is great for other conditions), as it imbues the text with a sort of temporality that does not exist for global captions.
3. **Cross-Attention**: Here, we add additional cross-attention layers interleaved with the self-attention layers inside each DiT block, where the diffusion latent direct attents to $\mathbf{e}_{\textrm{text}}$. This perhaps offers the best control (and is what Stable Audio Open uses), at the cost of the most added compute given the quadratic cost of each cross-attention layer.
4. **Adaptive Layer-Norm (AdaLN)**: Here, the layer-norms in each DiT block are augmented with shift, scale, and gate parameters (one for each index of the hidden dimension) that are learned from $\mathbf{e}_{\textrm{text}}$ through a small MLP: $\gamma_{\textrm{shift}}, \gamma_{\textrm{scale}}, \gamma_{\textrm{gate}} = \textrm{MLP}(\mathbf{e}_{\textrm{text}})$. This adds the least computation to the model, and is what the original DiT works uses {cite}`peebles2023scalable`. Note that in spirit, this is practically identical to the Feature-wise Linear Modulation (FiLM) layers used in MusicLDM {cite}`chen2023musicldm`. These shift, scale, and gate parameters can also be zero-initialized such that each block is essentially initialized as the identity function, which is what "AdaLN-Zero" refers to.


```{figure} ../img/generation/conds.png
---
name: Types of DiT conditioning mechanism,s
---
DiTs can be conditioned in a number of ways, including adaptive layer norm, cross-attention, and in-context conditioning (time-domain concatenation).
```