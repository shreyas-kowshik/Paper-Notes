# Parrotron : End to End Speech Conversion Model

Link : https://arxiv.org/pdf/1904.04169.pdf

### Overvie

* Input Speech -> Encoder -> Three outputs : 1. Decoder -> Spectrogram 2. Decoder -> Phonemes
* "We demonstrate that this model
can be trained to normalize speech from any speaker regardless
of accent, prosody, and background noise, into the voice of a
single canonical target speaker with a fixed accent and consistent
articulation and prosody"

### Details

* Data required : Parallel corpus of speech to speech utterances. The first is from a variety of speakers. The second is the same speech utterance in a normalized/canonical speaker
* Input speech -> Extract log mel spectrogram features 80 dimensional.
Log Mel and Mel scale features link : https://medium.com/analytics-vidhya/understanding-the-mel-spectrogram-fca2afa2ce53
Mel Scale is suited for human hearing. Humans can distinguish two similar lower frequency sounds as compared to higher.
For some details on dimensions of input : https://towardsdatascience.com/musical-genre-classification-with-convolutional-neural-networks-ff04f9601a74

In general you can assume the image to be of say 80xT dimensions with 80 mel scales and T timesteps. In case RGB image as input then 80xTx3 for color channels. (Self) This is for one frame and similarly multiple times to get images for frames across time.

Convolutional LSTM : LSTM however instead of the linear weight layer it is a convolution. Basically spatio-temporal processing as in images across time are inputs.

* The network overall predicts the input spectrogram and also tries to do phoneme classification. Once overall trained, given an input speech it will produce a spectrogram and WaveRNN neural vocoder produces the speech.

* Decoder targets are  1025-dim STFT magnitudes, computed
with the same framing as the input features, and a 2048-point
FFT obtained from the normalized TTS generated outputs.