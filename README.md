# Facial Expression Classifier using Brain.js

## Project Overview
This project implements a facial expression classification model using **Brain.js**, a JavaScript-based neural network library. The model is trained to classify facial expressions from images, and it can also run in real-time using a webcam feed. The project covers dataset preprocessing, model training, evaluation, and real-time emotion detection.

## Dataset Preparation
### 1. Loading the Dataset
The dataset used in this project is a CSV file containing grayscale images of facial expressions. The pixel values and their corresponding emotion labels are extracted.

```js
load_file().then(csv => { data = [].from_csv(csv) });
```

### 2. Data Cleaning
Unnecessary rows are removed to ensure the dataset is properly formatted.

```js
data.shift(); // Remove headers
data.pop();   // Remove trailing empty rows
data.length;
```

### 3. Data Transformation
The image pixel values are converted from space-separated strings into integer arrays for further processing.

```js
transformStringToArray = function (inputString) {
    const stringArray = inputString.split(' ');
    return stringArray.map(Number);
};
```

### 4. Normalization
Pixel values are normalized to a range of 0-1 for better model convergence.

```js
function normalize(data) {
    return data.map(row => row.map(value => parseFloat(value) / 255));
}
```

### 5. Data Augmentation
Image transformations (rotation, flipping, brightness adjustments) are applied to increase dataset variability.

```js
// Example: Apply random rotation
scrib.loadScript("https://cdn.jsdelivr.net/npm/jimp@0.22.10/browser/lib/jimp.min.js");
Jimp.read(imageData).then(img => img.rotate(15).getBase64(Jimp.AUTO, console.log));
```


### 6. Data Imbalance
To address class imbalances in the dataset, oversampling and undersampling techniques were applied.

```js
// Example: Balance dataset by limiting each emotion to the least frequent count
function balanceDataset(data) {
    let minSamples = Math.min(...[0,1,2,3,4,5,6].map(label => cnt_emo(data, label)));
    let balancedData = [];
    let emotionCounts = Array(7).fill(0);
    
    data.forEach(sample => {
        let label = Maxprob(sample.output);
        if (emotionCounts[label] < minSamples) {
            emotionCounts[label]++;
            balancedData.push(sample);
        }
    });
    
    return shuffle(balancedData);
}
```

## Model Architecture
The model is a **Multi-Layer Perceptron (MLP)** built using Brain.js with two hidden layers.

```js
const net = new brain.NeuralNetwork({
    hiddenLayers: [1152, 144], // Two hidden layers
    activation: "sigmoid" // Activation function
});
```

## Model Training
### 1. Preparing Training Data
Each data entry is structured with input pixel values and one-hot encoded labels.

```js
function oneHotEncode(label, numClasses) {
    let encoding = Array(numClasses).fill(0);
    encoding[label] = 1;
    return encoding;
}
```

### 2. Training the Model
The model is trained using backpropagation with multiple iterations.

```js
function trainModel(trainingData) {
    net.train(trainingData, {
        iterations: 10,
        learningRate: 0.01,
        log: true,
        logPeriod: 100
    });
}
```

## Model Testing & Evaluation
### 1. Preparing Test Data
Test data undergoes the same preprocessing steps (normalization and encoding).

```js
function testModel(testData) {
    let correct = 0;
    testData.forEach(({ input, output }) => {
        let prediction = net.run(input);
        let predictedLabel = prediction.indexOf(Math.max(...prediction));
        let actualLabel = output.indexOf(1);
        if (predictedLabel === actualLabel) correct++;
    });
    let accuracy = (correct / testData.length) * 100;
    console.log(`Model Accuracy: ${accuracy.toFixed(2)}%`);
}
```

## Real-Time Emotion Detection
The model can classify emotions in real-time using a webcam feed. The captured frames are processed and passed through the trained model for classification.


```js

// Capture an image
document.getElementById('captureBtn').addEventListener('click', () => {
    ctx.drawImage(video, 240, 170, 240, 240, 0, 0, 240, 240);
});

// Process the captured image and classify expression
document.getElementById('processBtn').addEventListener('click', () => {
    let pixels = getPixels();
    let grayscalePixels = getGrayPixels(pixels);
    let imgdata = resizeImage(grayscalePixels, 240, 240, 48, 48);
    imgdata = normalize(imgdata);
    let expression = Maxprob(net.run(imgdata));
    let expressions_map = {0:"Angry", 1:"Disgust", 2:"Fear", 3:"Happy", 4:"Sad", 5:"Surprise", 6:"Neutral"};
    alert(`Expression: ${expressions_map[expression]}`);
});
```

Example Output:

![Example](images/example1.jpg)

## Results & Achievements
- Successfully trained a Brain.js model to classify facial expressions.
- Implemented dataset preprocessing, augmentation, and normalization.
- Achieved a high accuracy on test data through iterative training.
- Integrated real-time emotion detection using a webcam feed.

## Future Enhancements
- Improve model accuracy with advanced preprocessing techniques.
- Experiment with different activation functions and learning rates.
- Implement a confusion matrix for detailed error analysis.
- Optimize performance for real-time inference on lower-end devices.


