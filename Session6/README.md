## Session 6: Text Generation with LSTM
Implementation of [Text Generation With LSTM Recurrent Neural Networks](https://machinelearningmastery.com/text-generation-lstm-recurrent-neural-networks-python-keras/) with additional optimization for improved results.
Training an LSTM model from scratch that takes a seed sentence and predects next few sentences.  

**Dataset**: [Alice’s Adventures in Wonderland by Lewis Carroll](https://www.gutenberg.org/ebooks/11)   

### Optimizations to improve results:
 - Limit predicted characters length to 500.
 - Improved data preparation and cleaning.
 - Replaced random sequences of characters with padded sequence. 
 - Added dropout to the input layer and removed it from the layer before dense layer. 
 - Changed dropout value to 0.1 
 - Trained the model for 100 epochs
