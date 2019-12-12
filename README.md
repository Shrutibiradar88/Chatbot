# Chatbot
Sequence to Sequence Models

In sequence to sequence models to recurrent networks work together to transform  one sequence to another. Some a model can be used for machine translation of languages or answering question answers models.

Preprocess: The input sentences are tokenized and a word2index dictionary is made out of them.
I have chosen to keep only 10,000 question answer models .We use <start> and <end> tags to mark start and end of each sentence


The model consists of three parts:

1. Encoder : This blocks of lstm (200 in length) units in our case that takes an input of dimension 200 from the word embedding layer made from Stanford glove representation.
The  words  enter the lstm units in which each cell accepts single input of input sequence, collects information for that elements and propagates it forward.Only questions enter the encoder model.

2. Encoder Vector :This is the final hidden state produced from the encoder part of the model. It is calculated using the formula above.This vector aims to encapsulate the information for all input elements in order to help the decoder make accurate predictions.
These are represented by encoder_states = [ state_h , state_c ] in the code.



1. Decoder : A stack of several last units where each predicts an output y_t at a time step t.Each recurrent unit accepts a hidden state from the previous unit and produces and output as well as its own hidden state.
In the question-answering problem, the output sequence is a collection of all words from the answer. Each word is represented as y_i where i is the order of that word

Just as encoders, decoders also produce a hidden state.we are just using the previous hidden state to compute the next one.

We calculate the outputs using the hidden state at the current time step together with the respective weight W(S). Softmax is used to create a probability vector which will help us determine the final output (e.g. word in the question-answering problem)

Three important points in the code

* The return_state contructor argument, configuring a LSTM layer to return a list where the first entry is the outputs and the next entries are the internal RNN states. This is used to recover the states of the encoder.
* The inital_state call argument, specifying the initial state(s) of a RNN. This is used to pass the encoder states to the decoder as initial states.
* The return_sequences constructor argument, configuring a LSTM to return its full sequence of outputs (instead of just the last output, which the defaults behavior). This is used in the decoder.

Scope for future work....

Attention: The input to the decoder is a single vector which has to store all the information about the context. This becomes a problem with large sequences. Hence the attention mechanism is applied which allows the decoder to look at the input sequence selectively.
Beam Search: The highest probability word is selected as the output by the decoder. But this does not always yield the best results, because of the basic problem of greedy algorithms. Hence beam search is applied which suggests possible translations at each step. This is done making a tree of top k-results.
Bucketing: Variable-length sequences are possible in a seq2seq model because of the padding of 0’s which is done to both input and output. However, if the max length set by us is 100 and the sentence is just 3 words long it causes huge wastage of space. So we use the concept of bucketing. We make buckets of different sizes like (4, 8) (8, 15) and so on, where 4 is the max input length defined by us and 8 is the max output length defined.



