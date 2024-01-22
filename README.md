JavaScript Hackathon

- We will try to build a deep learning model through which we can recognise face expressions.
- And finally after building the model, we will store it into a js file and load into another html file where we will do live testing of our model. For instance, user video will be captured through the webcam and will be displayed over a webpage, simultaneously we will also display the expression, as our model suggests.

- Dataset: CK+
  - Challenges:
    - The Javascript file of CK+ dataset is not available on CDN (like MNIST), so we have to build that and load it into our main script.
  - Solution:
    - Using open-cv (alternate for js, whatever) make such a file and store the data in it.
    - Or load data differently.
  - Done
- Data Imbalance
  - Will have to think about it.

- Transformations to be applied on the image.
  - In process (Madhav)
  - Done

- Models: CNN layers - To be decided ( can only be adjusted after checking the error for each combinations)
  - Challenges:
    - Correct js library (CDN) to make a CNN model. (Involves some research)
- Storing of model into another js file
- Execution and testing using HTML and finally building the project.
- So this was our plan and we succeeded in our plan. But there are some challenges:
  - Accuracy of Model
  - So this is the main issue and I have to further think about it because ML with js is really a great combo. It can be helpful in various applications like I am thinking about website( two way interaction types)..
  - If time permits, we can think of improving this model.
  - Finally the overall learning from this, is observing the real example of ML with js.
- Resources to look at..
  - Try to see tensorflow. It will be a great library for this purpose.
