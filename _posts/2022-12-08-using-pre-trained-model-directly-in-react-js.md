---
layout: post
title:  "Using a Pre-trained Model in React.js Project"
date:   2022-12-08
excerpt: "Have you worked with website that contains simple machine learning models to predict things. Do you create a python server or something like that which serves one/two endpoints and basically doing the job of a simple function? Well if that so, then this article is for you."
---

TensorFlow.js is a JavaScript library for training and deploying machine learning models in the browser and on the server. 
In this article, we will discuss how to use TensorFlow.js to incorporate pre-trained machine learning models into your React project.
We will go over the basics of TensorFlow.js, including how to install and import the library, 
and how to load a pre-trained model from Python and use it to make predictions in your React project. 
By the end of this article, you should have a good understanding of how to use TensorFlow.js to integrate pre-trained machine learning models into your React project.

![machine learning](/assets/images/machine-learning.png)

Now, to start with asimple machine learning project, I am choosing the [Salary_Data.csv](https://raw.githubusercontent.com/sudip-mondal-2002/personal-blog/tensorflow-js/assets/datasets/Salary_Data.csv) file.
We'll build a artificial neural network model in python and later use it in React, without creating any backend server.

## Building the model in python

#### Importing the libraries

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
import tensorflowjs as tfjs
```

#### Preparing the data

```py
data = pd.read_csv("Salary_Data.csv")
X = data.iloc[:,0].values
Y = data.iloc[:,1].values
```

#### Scaling the data

As we're using deep-learning model, it's required to scale the data. I am using the logic of min-max scaling. 
Note that, the scaling will be required in the later stages to be used in the website.
```py
X_scales = [max(X), min(X)]
X_data = (X-X_scales[1])/(X_scales[0]-X_scales[1])
Y_scales = [max(Y), min(Y)]
Y_data = (Y-Y_scales[1])/(Y_scales[0]-Y_scales[1])
```

#### Build the model

You may build the model your way.
```py
regressor = Sequential()
regressor.add(Dense(units = 10, input_shape=(1,), activation="relu"))
regressor.add(Dense(units = 50, activation="relu"))
regressor.add(Dense(units = 500, activation="relu"))
regressor.add(Dense(units = 50, activation="relu"))
regressor.add(Dense(units = 10, activation="relu"))
regressor.add(Dense(units = 1))
regressor.compile(optimizer = 'adam', loss = 'mean_squared_error')
regressor.summary()
```

#### Fitting the model

```py
history = regressor.fit(X_data, Y_data, epochs = 30, validation_split=0.1)
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epochs')
plt.plot(history.history['loss'])
plt.legend(['Train'])
plt.show()
```

![loss](https://user-images.githubusercontent.com/74463091/206439610-032d5e66-871c-4864-9c7d-a6f71cd83d6b.jpg)

#### Plotting the prediction

```py
predicted_y_data = regressor.predict(X_data)
predicted_y = predicted_y_data*(Y_scales[0]-Y_scales[1]) + Y_scales[1]
plt.scatter(X,Y)
plt.plot(X,predicted_y.reshape(-1,1))
```

![prediction](https://user-images.githubusercontent.com/74463091/206439841-6e42d226-10d2-4d7b-be3f-220eb10f2b88.png)

#### Save the model and print the scales

```py
tfjs.converters.save_keras_model(regressor, "model")
print(X_scales, Y_scales)
```
```sh
[10.5, 1.1] [122391.0, 37731.0]
```

There will be a folder has been created named `model` containing a `.json` and a `.bin` file. Save those for use in the React project.

## Using the model in React project

Create a react app using `create-react-app` or any other way you like.

```sh
$ npx create-react-app salary-predictor
$ cd salary-predictor
```
Now upload the `.json` and `.bin` file in the public directory. Also create a custom hook named `useSalaryModel.js` to use the model.

![image](https://user-images.githubusercontent.com/74463091/206441643-0f23d050-41e1-429c-8b7a-bf19f947b1e8.png)

#### useSalaryModel.js

```jsx
import { useEffect, useState } from "react";
import * as tf from "@tensorflow/tfjs";
export default function useSalaryModel() {
  const [model, setModel] = useState(null);
  const loadModel = async () => {
    const model = await tf.loadLayersModel("/model.json");
    setModel(model);
  };
  const predict = async (experience) => {
    if (!model) return 0;
    const X_scales = [10.5, 1.1];
    const Y_scales = [122391.0, 37731.0];
    const exp_scaled = (experience - X_scales[1]) / (X_scales[0] - X_scales[1]);
    const exp_scaled_tensor = tf.tensor1d([exp_scaled]);
    const sal_scaled = await model.predict(exp_scaled_tensor).array();
    const salary = sal_scaled * (Y_scales[0] - Y_scales[1]) + Y_scales[1];
    return salary;
  };
  useEffect(() => {
    loadModel();
  }, []);
  return { model, predict };
}
```

#### App.js
```jsx
import { useEffect, useState } from "react";
import "./styles.css";
import useSalaryModel from "./useSalaryModel";

export default function App() {
  const { model, predict } = useSalaryModel();
  const [givenExp, setGivenExp] = useState(0);
  const [expectedSal, setExpectedSal] = useState(0);
  useEffect(() => {
    if (!model) return;
    predict(givenExp).then((res) => {
      setExpectedSal(res);
    });
  }, [model, predict, givenExp]);
  return (
    <div className="App">
      {!model && <>Loading the model...</>}
      {model && (
        <div>
          <div>
            <input
              type="number"
              value={givenExp}
              onChange={(e) => {
                setGivenExp(e.target.value);
              }}
            />
          </div>
          <div>{expectedSal}</div>
        </div>
      )}
    </div>
  );
}
```

The whole react project can be found in the [sandbox](https://codesandbox.io/s/unruffled-cherry-zh3fkt).

Here are some screenshots of the website.

<img src="https://user-images.githubusercontent.com/74463091/206442562-fd638b0a-3a12-4c04-9190-8a81781663f5.png" width="32%" align="right"/>
<img src="https://user-images.githubusercontent.com/74463091/206442547-4780a4e1-588b-442a-a49f-d3e663470b68.png" width="32%" align="left"/>
<img src="https://user-images.githubusercontent.com/74463091/206442556-1958ff63-2c44-4d4b-9de1-5313071cd775.png" width="32%" align="center"/>





