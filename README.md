# Reverb.js &nbsp; ![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png) 

**Reverb.js** is a Web Audio API extension for creating reverb nodes and an accompanying impulse-response reverb library. You can quickly load any of the provided impulse responses or use your own.

The Web Audio API does not specifically include a reverb node type. Instead, it has something better: a *convolution node*! Convolution is a mathematical transformation that combines an impulse response and a dry signal to yield a wet signal. Reverberation is easy to model with impulse responses since most reverberation has a linear response (contains little distortion). Impulse responses for simulating reverberation are often generated by recording a percussive noise or frequency sweep in a space. Does this all sound convoluted? Well, technically, it is!

This project is dedicated to the public domain via the CC0 license. However, most of the impulse responses are copyright and come with their own licenses (many are Creative Commons), so if you end up redistributing these, please make sure you follow the applicable licenses mentioned in the attributions.

## Demo
Here is a [demo](http://reverbjs.org) of Reverb.js with each of the samples in the reverb library.

## Usage
Assuming ``audioContext`` was the name of the variable containing the reference to the AudioContext, first call:

```
reverbjs.extend(audioContext);
```

This will add three different functions for creating a reverb node.

```
audioContext.createReverbFromBase64(audioBase64, callback) -> AudioNode
audioContext.createReverbFromUrl(audioUrl, callback) -> AudioNode
audioContext.createReverbFromBase64Url(audioBase64, callback) -> AudioNode
```

These act like any of the other ``audioContext.create...()`` functions, and each immediately returns a new node (of the ConvolverNode type) and asynchronously attempts to fetch and/or decode the given impulse response into the convolver node's ``buffer``, calling the optional ``callback`` after it has successfully loaded.

The ``audioContext.createReverbFromBase64`` function accepts a Base64 string representation of the audio file (without newlines). The characters allowable by the Base64 decoder are A-Z, a-z, 0-9, +, /, and =. Base64 is useful for embedding the impulse response into your JavaScript (for local testing, resource consolidation, etc.).

Alternatively, you can use the ``audioContext.createReverbFromUrl`` to load the impulse response from any URL.

Finally, if only plain-text URLs are accessible from your site's origin (as is the case with the demo on GitHub), there is a ``audioContext.createReverbFromBase64Url`` function, which fetches the Base64-encoded contents of an audio file from a URL.

For the best cross-platform performance, use **32-bit float, 2 channel (stereo), 44.1KHz** **WAV** (uncompressed) or **M4A** (AAC compressed) impulse responses. As of late 2015, there is universal support for both WAV and M4A in all major browsers, so the choice to use WAV or M4A mostly depends on whether fidelity or size is most important. Keep in mind that audio codecs are primarily designed to compress sound, speech, and music—not impulse responses. Depending on the nature of the impulse response, compression may result in a noticeable change in the *color* of reverberation.

## Convenience Functions

The reverbjs.extend() function also adds three complementary functions for loading source samples. These work just like the createReverb... functions above except that they create source nodes using the loaded buffer.

```
audioContext.createSourceFromBase64(audioBase64, callback) -> AudioNode
audioContext.createSourceFromUrl(audioUrl, callback) -> AudioNode
audioContext.createSourceFromBase64Url(audioBase64, callback) -> AudioNode
```
