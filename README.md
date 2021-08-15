# trainer

- [x] Keras backend
- [ ] Pytorch backend
- [ ] Paddle backend

## Example 1: use `KerasTrainer` to train a DNN for `jina hello fashion`

1. use artificial pairwise data (generated by `fashion_match_doc_generator`) to train the `user_model` in a siamese manner: 

    ```python
   # build a simple dense network with bottleneck as 10-dim
   import tensorflow as tf
   
   user_model = tf.keras.Sequential(
       [
        tf.keras.layers.Flatten(input_shape=(28, 28)),
        tf.keras.layers.BatchNormalization(),
        tf.keras.layers.Dense(128, activation='relu'),
        tf.keras.layers.Dense(32),
       ]
   )
   
   # wrap the user model with our trainer
   from trainer.keras import KerasTrainer
   
   kt = KerasTrainer(user_model)
   
   # generate artificial positive & negative data 
   from tests.data_generator import fashion_match_doc_generator as fmdg
   
   # fit and save the checkpoint
   kt.fit(fmdg, epochs=3)
   kt.save('./examples/fashion/trained')
    ```

2. Test the embedding in the Jina pipeline:
    ```bash
    python examples/fashion/app.py
    ```