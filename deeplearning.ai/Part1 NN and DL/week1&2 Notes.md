# Week 1 Introduction
## 15July 
1. Deep learning is used to train neural networks 

2. In a neural network, the input layer and the hidden layer(layer in the middle) is densely connected. Every input feature will be connected to the circles in the middle.

3. Examples of sequence data are 
  - audio
    - 1D time series
    - 1D temporal sequence
  - language(comes once at a time). 

4. As to **structured data**, each of the element has a well-defined meaning. While for **unstructured data**, features can be pixel value and individual word in a piece of text. 

5. Thanks to deep learning, now a lot of fancy applications are created such as speech recognition, image recognition, NLP.

6. Short-term economic value have been created using neural networks such as advertising systems, process giant database, profit recommendation.

7. **Increasing** the **training set size** generally does not hurt an algorithm’s performance, and it may help significantly.

8. **Increasing** the **size of a neural network** generally does not hurt an algorithm’s performance, and it may help significantly. 

9. RNN (Recurrent Neural Network) is not strictly more powerful than a Convolutional Neural Network (CNN). 

10. There are differnt type of neural networks:
- image->CNN
- sequence data-> RNN
  - audio: 
      - 1D time series
      - 1D temporal sequence 
  - language
- automatic driving->combination of CNN and other techniques.
11. Why is deep learning taking off?
- Being able to train a big enough neural network
- Huge amount of labeled data
12. Scale drives deep learning progress
- Data
- Computation(hardware such as GPU becomes better)
- Algorithm


# Week 2 Basics of Neural Network programming
## 16July 
1. This week goes over the basic of neural network programming. Some implemential techniques are very important. For example, in neural network, we want to process a training set of m examples without using an explicit for loop to stepping through all the examples. Another idea is that when organizing the computation of the neural networks, there is a term"forward propogation step" followed by a "backward propagation step/backward pass. So why the computation in learning an NN can be organized in this two processes? This week uses logistic regression to understand those problems.

2. Logistic regression->used for binary classification.

3. A 64x64 pixel image is composed of RGB, to turn the pixel value to feature vector, just unrow them to a long feature vector [64\*64\*3,1]. The total dimension is 64\*64\*3.

4. A useful convention of notation is to take the data(x, y and further other quantities) associated with different training examples and to stack them into different columns(like what we done for both X and Y)

5. When implementing logistic regression, we keep w and b separately. 

6. **Loss function** is used to measure how good y hat is when the truth is y. Loss function applies to a **single** training sample.We want it to be convex such that GD works well.

7. **Cost functio**n measures how well you are doing on the **entire** dataset. It is the average over sum of all the loss function applying to each of the training samples. It is the cost of the parameters. During training the neural networks, we try to find w and b that minimizes J overall.

8. Logistic regression can be viewed as a very small neural network.

9. alpha: learning rate, controls how big each step during learning(ways to choose alpha will be taught later)

10. Understanding of calculus helps to understand neural network.

## 18July
1. The computations of a neural network are all organized as a forward path or a forward propagation step in which we compute the output of the neural network, follow by a back complition step which we use to compute gradients/derivatives 

2. Computation graph explains the reason for 1.

3. When coding the derivative of the final output variable towards some variables, in Python just write dJ/dvar as dvar to represent the derivative of the final output variable with respect to the intermediate variables

4. Having explict for loops in the code when running deep learning will make the algorithm less efficiently

5. In order to scale to big dataset, it is important to avoid explict for loops in your code-> vectorization allows you to get rid of the explicit for loop, which becomes even more efficient in deep learning.

6. A lot of scaleable deep learning implementations are done on a GPU(graphics processing unit图像处理单元)。 Both GPU and CPU have parallelization instrutions, sometimes called SIMD(single instruction multiple data)->if you use built-in functions such as np.dot(x,y) or others that do not require you explicitly implementing a for loop, it enables Python numpy to take much better advantage of parallelism.

7. GPU are remakably good at these SIMD calculations

8. Rule of thumb: whenever possible, avoid using explicit for loops

9. Broadcasting turns out to be a technique that Python and numpy allows you to make the code more efficient

## 19July 
1. In python, sum **axis=0** means that we want Python to sum **vertically**,  while the horizontal is axis = 0

2. The reshape function in Python is O(1) computation, so it is a good way to make sure that our matrices are the size that we need.

3. Python can have some internal strange logic, for example, adding a column vector towards a row vector will lead to a matrix as a sum of a row vector and a column vector.

4. np.random.randn(5) will lead to a structure whose shape is (5,). This is **a rank 1 array in Python. It is neither a row vector nor a column vector**. And as a result, a.T will looks as the same as a, and np.dot(a,a.T) will only give a single value. So Andrew suggested to avoid these rank1-structure whose shape is (n,). Instead, we can write 
```Python
np.random.randn((5,1)) 
```
to assure that a is a column vector. Then np.dot(a,a.T) while result in a outer product.

5. The result of randn(5) will be [1,2,3,4,5], while randn(5,1) will be [[1,2,3,4,5]], two squares. **[[**  q,w,e,r,t  **]]** is really a 5x1 matrix, while [q,w,e,r,t] is a rank-1 array. 

6. When not sure what is the dimension of the vector, use assert() to make sure something. Like assert(a.shape==(5,1)).

7. When some times you do have a rank1 array, you can write it as a=a.reshape((5,1)).

8. Usually if you are training a learning algorithm you want to make the probabilites large whereas in logistic regression we need to minimize the loss function. So here minimize the loss corresponds to maximizing the log of the probability

9. By minimizing the cost function J(w,b), we are really carring out maxium likelihood estimation with the logistic regresion model's assumption that the training examples are IID

10. In python, a\*b will perform element-wise multiplication, while **np.dot(a,b) will perform matirx multiplication**. For example, we cannot do a\*b when a is (4,3) but b is (3,2).

## 21July 

1. The inputs of the"math" library are real numbers. But in deep learning we mostly use matrices and vectors. This is why numpy is more useful. 

2. Gradient descent converges faster after normalization.

3. After normalizing rows of x, each row of the input matrix x will be a vector of unit length(divide the original matrix x by the np.linalg.norm from axis=1 direction).

4. np.sum(..., axis = 1) will sum each rows of the input.

5. To remember: 
* np.exp(x) works for any np.array x and applies the exponential function to every coordinate; 
* the sigmoid function and its gradient; 
* image2vector is commonly used in deep learning;  
* np.reshape is widely used. 

6. Keeping your matrix/vector dimensions straight will go toward eliminating a lot of bugs. 
* dim(X) = (n<sub>x</sub> , m)
* dim(Y) = (1, m)

7. General principle for broadcasting:
* matrix (m,n) +-x÷ (1,n)-->(m,n)
* matrix (m,n) +-x÷ (m,1)-->(m,n)

8. A justification of the cost function for logistic regression:
* interpret a =p(y=1|x)
  - if y=1, p(y|x)=a
  - if y=0, p(y|x)=1-a
* To summarize the above two equations to a single equation, p(y|x)=a<sup>y</sup>(1-a)<sup>(1-y)</sup>, since log is a strictly monotonically increasing function, max{log p(y|x)}=max{p(y|x)}
  -log p(y|x)=yloga +(1-y)log(1-a)=-loss function
    - The reson for "negative" is that when training algorithm, we want to make the probability p(y|x) large, whereas we want to make the loss function to decrease.
    
## Coding 
1. np.dot towards two **1-D** vector will result in their inner product.

2. np.dot towards two **2-D** matrices will result in **matrix product**.

3. Vectorization is very important in deep learning since it provides computational efficiency and clarity. 

4. Usage of np.multiply: same with \*, but can add more optional arguments to make it more versatile. 

5. Why the following code can print cat? 
```Python
classes[np.squeeze(train_set_y[:, index])].decode("utf-8")
```
* Here train_set_y[:, index] will return [0] or [1]，np.squeeze will throw [] away. The result of np.squeeze(train_set_y[:, index])will e 0 or 1. And the classes is [b'non-cat' b'cat'] with type bytes, we need to decode it using utf-8, then it will display cat or non-cat.

6. A trick when you want to **flatten a matrix X of shape (a,b,c,d) to a matrix X_flatten of shape (b ∗ c ∗ d, a)** is to use: 
```Python
X_flatten = X.reshape(X.shape[0], -1).T; Note here X.T is the transpose of X
```
**The "-1" makes reshape flatten the remaining dimensions**

7. One common preprocessing step in machine learning is to center and standardize your dataset, meaning that you substract the mean of the whole numpy array from each example, and then divide each example by the standard deviation of the whole numpy array. 

8. For picture datasets, it is simpler and more convenient and works almost as well to just divide every row of the dataset by 255 (the maximum value of a pixel channel).

9. Common steps for pre-processing a new dataset are:
* Figure out the dimensions and shapes of the problem (m<sub>train</sub> , m<sub>test</sub>, num<sub>px</sub>, ...)
* Reshape the datasets such that each example is now a vector of size (num<sub>px</sub> * num<sub>px</sub> * 3, 1)
* "Standardize" the data

10. The main steps for building a Neural Network are:
- Define the model structure (such as number of input features)
- Initialize the model's parameters
- Loop:
  - Calculate current loss (forward propagation)
  - Calculate current gradient (backward propagation)
  - Update parameters (gradient descent)

11.  np.squeeze: Remove single-dimensional entries from the shape of an array.
# Plot learning curve (with costs)
```Python
costs = np.squeeze(d['costs'])
plt.plot(costs)
plt.ylabel('cost')
plt.xlabel('iterations (per hundreds)')
plt.title("Learning rate =" + str(d["learning_rate"]))
plt.show()
```
12. Error may occur if write /m directly, as opposed to write (**1.0**/m)

13. The **learning rate** alpha determines how **rapidly** we update the parameters. If the learning rate is too large we may "overshoot" the optimal value. Similarly, if it is too small we will need too many iterations to converge to the best values. That's why it is crucial to use a well-tuned learning rate.

14. A lower cost doesn't mean a better model. You have to check if there is possibly **overfitting**. It **happens when the training accuracy is a lot higher than the test accuracy**.

15. In deep learning, we usually recommend that you:
* Choose the learning rate that better minimizes the cost function. 
* If your model overfits, use other techniques to reduce overfitting.

16. When initializing the parameters, using **assert** to assure the dimension:
```Python
def initialize_with_zeros(dim):
    """
    This function creates a vector of zeros of shape (dim, 1) for w and initializes b to 0.
    
    Argument:
    dim -- size of the w vector we want (or number of parameters in this case)
    
    Returns:
    w -- initialized vector of shape (dim, 1)
    b -- initialized scalar (corresponds to the bias)
    """
    
    ### START CODE HERE ### (≈ 1 line of code)
    w = np.zeros((dim, 1))
    b = 0
    ### END CODE HERE ###

    assert(w.shape == (dim, 1))
    assert(isinstance(b, float) or isinstance(b, int))
    
    return w, b
```

17. Several useful code from homework:
* Use dictionary to store the return value 
```Python
def propagate(w, b, X, Y):
    #.......Just omit other code 
    assert(dw.shape == w.shape)
    assert(db.dtype == float)
    cost = np.squeeze(cost)
    assert(cost.shape == ())
    
    grads = {"dw": dw,
             "db": db}
    
    return grads, cost

```
* Record the cost function every 10 iteration
```Python
        # Record the costs
        if i % 100 == 0:
            costs.append(cost)
        
        # Print the cost every 100 training iterations
        if print_cost and i % 100 == 0:
            print ("Cost after iteration %i: %f" %(i, cost))
```
18. To compare the result with different learning rate:
```Python
learning_rates = [0.01, 0.001, 0.0001]
models = {}
for i in learning_rates:
    print ("learning rate is: " + str(i))
    models[str(i)] = model(train_set_x, train_set_y, test_set_x, test_set_y, num_iterations = 1500, learning_rate = i, print_cost = False)
    print ('\n' + "-------------------------------------------------------" + '\n')

for i in learning_rates:
    plt.plot(np.squeeze(models[str(i)]["costs"]), label= str(models[str(i)]["learning_rate"]))

plt.ylabel('cost')
plt.xlabel('iterations (hundreds)')

legend = plt.legend(loc='upper center', shadow=True)
frame = legend.get_frame()
frame.set_facecolor('0.90')
plt.show()
```

19. Inspiration from this assignment:
* Preprocessing the dataset is important. 
* You implemented each function separately: initialize(), propagate(), optimize(). Then you built a model(). 
* Tuning the learning rate (which is an example of a "hyperparameter") can make a big difference to the algorithm. 
