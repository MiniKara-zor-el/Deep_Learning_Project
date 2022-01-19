# Image Classification using AWS SageMaker

Use AWS Sagemaker to train a pretrained model that can perform image classification by using the Sagemaker profiling, debugger, hyperparameter tuning and other good ML engineering practices. This can be done on either the provided dog breed classication data set or one of your choice.

## Project Set Up and Installation
Enter AWS through the gateway in the course and open SageMaker Studio. 
Download the starter files.
Download/Make the dataset available. 

## Dataset
The provided dataset is the dogbreed classification dataset which can be found in the classroom.
The project is designed to be dataset independent so if there is a dataset that is more interesting or relevant to your work, you are welcome to use it to complete the project.

### Access
Upload the data to an S3 bucket through the AWS Gateway so that SageMaker has access to the data. 

## Hyperparameter Tuning
What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search

I Deployed a hyperparameter tuning job on sagemaker. and wait for the combination of hyperparameters turn out with best metric. The hyperparameter searchspaces are learning-rate and batchsize. I used Resnet34 pretrained model and 3 fully connected layers to classify the breeds of dogs.

The Hook was set to record the Loss Criterion of the process in both training and validation/testing. The Plot of the Cross Entropy Loss is shown below. By adding more fully conneted layers we can remove ups and downs during validation phase.

Loss_plot.png


Remember that your README should:
- Include a screenshot of completed training jobs
- Logs metrics during the training process
- Tune at least two hyperparameters
- Retrieve the best best hyperparameters from all your training jobs

## Debugging and Profiling
**TODO**: Give an overview of how you performed model debugging and profiling in Sagemaker

### Results
**TODO**: What are the results/insights did you get by profiling/debugging your model?

**TODO** Remember to provide the profiler html/pdf file in your submission.
Result is pretty good, as I was using ml.g4dn.xlarge to utilize the GPU of the instance, both the hpo jobs and training job did't take too much time.

Results/Insights is named as : profiler-report.html

## Model Deployment
**TODO**: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.

**TODO** Remember to provide a screenshot of the deployed active endpoint in Sagemaker.

Model was deployed on the ml.g4dn.xlarge instance with best result. First version of my deployment didn't work, so I need to add new inference script call endpoint.py to set up and deploy a working endpoint.



## Standout Suggestions
**TODO (Optional):** This is where you can provide information about any standout suggestions that you have attempted

To use the endpoint, just pass a local image of dog, or the address of the jpg image to the endpoint. Every processing job is done in the inference script and user can enjoy the result easily. Here is an procedure of using the deployed Endpoint. we first open an image as file with open("./testdog.jpg", "rb") as f: payload = f.read() so the payload becomes a bytetype file and pass it to the endpoint with predictor.predict(payload) command, the endpoint.py script check it and convert it to image type using PIL module. After that, the transformation takes place by cropping and converting the image to tensors. The function then call the model, predict and return the prediction as array of the 133 breeds of dogs, and we take the value of max one by selecting with np.argmax.