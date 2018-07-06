---
layout: post
title:  "Understanding Tensorflow's RNN model"
date:   2016-03-01
categories: research coding
tags: deep-learning tensorflow rnn
---

The RNN cell implementation in Tensorflow can be found at [here](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/rnn_cell.py). The RNN model can be found here.

One great LSTM RNN tutorial is [Colah’s Understanding LSTM Networks](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

# RNN Cell

The basic definition of RNN cell in Tensorflow is 

```python
def __call(self, inputs):
	return inputs
```

```python
def __call__(self, inputs, state, scope=None):

  """Run this RNN cell on inputs, starting from the given state.
      
   Args:
    inputs: 2D Tensor with shape [batch_size x self.input_size].
    state: 2D Tensor with shape [batch_size x self.state_size].
    scope: VariableScope for the created subgraph; defaults to class name.
  
   Returns:
    A pair containing:
    - Output: A 2D Tensor with shape [batch_size x self.output_size]
    - New state: A 2D Tensor with shape [batch_size x self.state_size].
  """
```

And an instance looks like

```python
def __call__(self, inputs, state, scope=None):
  """Most basic RNN: output = new_state = tanh(W * input + U * state + B).
  	with vs.variable_scope(scope or type(self).__name__):  # "BasicRNNCell"
    output = tanh(linear([inputs, state], self._num_units, True))
    return output, output
  """
```

As we can see from the code, a RNN cell needs two inputs, inputs, and state, and then calculate the score and then return the result. Inside the cell, the calcuation is performed by `tanh(linear([inputs, state], self._num_units, True))`, therefore we need to check the definition of `linear`, which is 

```python
def linear(args, output_size, bias, bias_start=0.0, scope=None):
  """Linear map: sum_i(args[i] * W[i]), where W[i] is a variable.
    Args:
      args: a 2D Tensor or a list of 2D, batch x n, Tensors.
      output_size: int, second dimension of W[i].
      bias: boolean, whether to add a bias term or not.
      bias_start: starting value to initialize the bias; 0 by default.
      scope: VariableScope for the created subgraph; defaults to "Linear".
    Returns:
      A 2D Tensor with shape [batch x output_size] equal to
      sum_i(args[i] * W[i]), where W[i]s are newly created matrices.
    Raises:
      ValueError: if some of the arguments has unspecified or wrong shape.
  """
  
  assert args
  if not isinstance(args, (list, tuple)):
    args = [args]

  # Calculate the total size of arguments on dimension 1.
  total_arg_size = 0
  shapes = [a.get_shape().as_list() for a in args]
  for shape in shapes:
    if len(shape) != 2:
      raise ValueError("Linear is expecting 2D arguments: %s" % str(shapes))
    if not shape[1]:
      raise ValueError("Linear expects shape[1] of arguments: %s" % str(shapes))
    else:
      total_arg_size += shape[1]

  # Now the computation.
  with vs.variable_scope(scope or "Linear"):
    matrix = vs.get_variable("Matrix", [total_arg_size, output_size])
    if len(args) == 1:
      res = math_ops.matmul(args[0], matrix)
    else:
      res = math_ops.matmul(array_ops.concat(1, args), matrix)
    if not bias:
      return res
    bias_term = vs.get_variable(
      "Bias", [output_size],
      initializer=init_ops.constant_initializer(bias_start))
  return res + bias_term
```