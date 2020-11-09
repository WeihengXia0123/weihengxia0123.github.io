---
title: "Back-propagation from scratch (Python)"
date: 2020-11-09T21:27:01+01:00
draft: true
thumbnailImagePosition: bottom
thumbnailImage: https://res.cloudinary.com/dr5l034cf/image/upload/v1604958146/backpropagation/backprop_icon_ytjd7s.jpg
autoThumbnailImage: yes
coverSize: false
coverImage: img/plane_sky.jpg
---

## Let's start from the Formula

The first thing to implement an algorithm is to understand its mathematics, so I wrote it step by step here:

![formula](https://res.cloudinary.com/dr5l034cf/image/upload/v1604958154/backpropagation/Backpropagation_formula_marked_s7xhpl.jpg)

During my implementation, I found that it's super **important** to know the **shape** (or size) of each `W`, `b`, and `error (delta)` ! Otherwise, it would take you ages to debug and print every variable's shape. Believe me, it sounds very troublesome to write down the size before implementing, but you will find it quite handy later;)



The below summary of equations from Michael A. Nielsen[1] is very simple and useful, with only 4 main equations in back-propagation.

![summary](https://res.cloudinary.com/dr5l034cf/image/upload/v1604958146/backpropagation/backprop_summary_vxi44b.png)



Reference:

[1] Michael A. Nielsen, "Neural Networks and Deep Learning", Determination Press, 2015. [URL](http://neuralnetworksanddeeplearning.com/chap2.html)



## Let's get our hand dirty! (code)

Firstly, we want to construct a class for MLP, as follows:

```python
class MLP(Layer): # Multi Layer Perceptron
    def __init__(self, neurons_per_layer):
        ''' SHAPE of each variable
        W - n(i+1) x n(i), which means the shape of W is the number of neurons in layer(i+1) x number of neurons in layer(i)
        b - 1 x n(i+1)
        a - 1 x n(i+1)
        z - 1 x n(i+1)
        d_W - n(i+1) x n(i) (same with W, think for why)
        d_b - 1 x n(i+1)
        d_a - 1 x n(i+1)
        d_z - 1 x n(i+1)
        '''
        
        super().__init__()
        
        self.layer={}
        
        for i in range(0,len(neurons_per_layer)-1) :
            self.layer[i]=Layer()
            self.layer[i].W=(10**(-1))*np.random.randn(neurons_per_layer[i+1],neurons_per_layer[i])
            self.layer[i].b=np.zeros((1,neurons_per_layer[i+1]))
            self.layer[i].a=np.zeros((1,neurons_per_layer[i+1])) 
            self.layer[i].z=np.zeros((1,neurons_per_layer[i+1]))
            self.layer[i].d_W=np.zeros((neurons_per_layer[i+1],neurons_per_layer[i]))
            self.layer[i].d_b=np.zeros((1,neurons_per_layer[i+1])) 
            self.layer[i].d_a=np.zeros((1,neurons_per_layer[i+1]))
            self.layer[i].d_z=np.zeros((1,neurons_per_layer[i+1]))

        self.losses=[]
        self.accuracies=[]
```

Secondly, we need a sigmoid and derivative of sigmoid function for later use:

```python
def sigmoid(a):
    return np.array(logistic.cdf(a)) 

def d_sigmoid(a):
    return MLP.sigmoid(a)*(1-MLP.sigmoid(a))
```
Thirdly, we implement the forward-propagation:

```python
def forward(self, x) :
    if x.ndim == 1:
        # if input is 1D, we reshape it to D_input x N, where N is the number of samples in one batch.
        # For example, if you want to train more than 1 sample in one time, the input size can be N=1000
        x = x.reshape(x.shape[0],1)
    else:
        # if input is 2D, we also reshape it D_input x N
        x = x.T
        
    num_layer = len(self.layer.keys())
    
    for i in range(num_layer):
        if i == 0:
            '''
            - W: n(0) x n(input)
            - x: n(input) x N
            - b: 1 x n(0)
            '''
            self.layer[i].z = (self.layer[i].W) @ x + self.layer[i].b.T
        else:
            '''
            - W: n(i) x n(i-1)
            - a: n(i-1) x N
            - b: 1 x n(0)
            - z: n(i) x N
            '''
            self.layer[i].z = self.layer[i].W @ self.layer[i-1].a + self.layer[i].b.T
        self.layer[i].a = MLP.sigmoid(self.layer[i].z)
    
    y_hat = self.layer[num_layer-1].a.T

    return y_hat
```

Fourthly, we need to calculate MSE loss and accuracy:

```python
def loss(y_hat, y) :
    '''
    - y_hat : NxC
    - y : NxC
    - L : MSE loss
    '''
    
    L = np.mean((y_hat - y)**2)/2
    
    return L


def accuracy(y_hat,y) :
    n=0
    y_hat_chosen = np.zeros(y_hat.shape)
    
    for i in range(y.shape[0]):    
        argmax = np.argmax(y_hat[i])
        y_hat_chosen[i][argmax] = 1
        
        if (y_hat_chosen[i] == y[i]).all():
            n += 1
    
    acc = float(n/y.shape[0])

    return acc
```

```python
def backpropagation(self,x,y,y_hat,learning_rate) :
    '''
    - deltas[i]: 1 x [this_layer]  (N=1 is the number of samples, and D_0 is the number of neurons in hidden layer 1)
    - d_W: same with W, [this_layer] x [previous_layer]
    - x: 1 x [input_layer] (N=1 is the number of samples)
    - a: [this_layer] x 1 (N=1 is the number of samples)
    - y, y_hat: 1 x C
    - z: [this_layer] x 1
    - d_b: 1 x [this_layer]
    '''
    
    deltas = {}
    num_layer = len(self.layer.keys())
    
    for i in reversed(range(num_layer)):
        # step1: compute error: delta
        if i == num_layer-1:
            deltas[i] = (y_hat - y) * MLP.d_sigmoid(self.layer[i].z).T
        else:
            deltas[i] = deltas[i+1] @ self.layer[i+1].W * MLP.d_sigmoid(self.layer[i].z).T

        # step2: compute d_b
        self.layer[i].d_b = deltas[i]
        
        # step3: compute d_W
        if i == 0:
            self.layer[i].d_W = deltas[i].T * x
        else:
            self.layer[i].d_W = deltas[i].T * self.layer[i-1].a.T
    

	# update gradients
    for i in range(num_layer):
        self.layer[i].W = self.layer[i].W - learning_rate*self.layer[i].d_W
        self.layer[i].b = self.layer[i].b - learning_rate*self.layer[i].d_b
```

****

> **CONGRATUALTIONS!!**

We finished the MLP structure by Python with just these few blocks!

Now it's your time to try it out by implementing your own training function, which calls the `MLP()` class to utilize its forward and backward-propagation functions in your own code!









