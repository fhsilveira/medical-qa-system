# Medical Question-Answering System

## Directory Structure

- **t5_fine_tuning.ipynb** - Notebook with implementation of data inspection, preprocessing, model training and inference with rudimentary experiment control.
- **t5_inference.ipynb** - Notebook comparing single inference using Flan-T5 base and fine-tuned models stored as experiments.
- **data** - Folder containing subfolders used by the system to store input dataset and experiment results (models and parameters saved after the model is trained).

## Analysis and Approaches

- The data in the medical dataset provided is structured in `question` and `answer` pairs. Flan-T5 was selected because it was pre-trained on several different tasks and despite its modest size it's able to perform well on new tasks. It's also able to handle QA pairs without the need of `context`. Another reason I considered was GPU limitations and task controlability.
- After inspection, I could observe duplicated `questions`, `NaN` values and analyse the distribution of the lenght of the `answers`, which is important to consider because of the model token truncation.
- Some of the data contains long detailed descriptions rather than answers.
- The fine-tuning was done based on experiments saved in the `data/experiments` folder for more control in the parameter tunning.
- Starting with recomended values and order of magnitude in a smaller subset of the dataset and later on adapting the values in a controlled manner using the `training loss` and `validation loss` as metrics.
- I tried to use `ROUGE` as a more robust metric to measure the similarity, but due to GPU memory allocation it was not possible to finish the training in this case.
- Controled experiments allows empiric observations on the inference results other than the stored experiments.
- Following the same logic, experiments 1 to 3 used Flan-T5-small and experiment 4 uses Flan-T5-base.

## Model's Performance and Evaluation

- I could observe performance improvements increasing the number of QA examples, and max tokens passed in the training phase. Also did some empirical parameter fine-tuning considering a later update for a larger model using the same Flan-T5 family.
- The performance up to experiment 4 is not satisfactory possibly because of model size used due to hardware limitations
- Controlabiliy, task specific approach and small size allows cost efficiency in large scale applications but accuracy, groundedness and context relevance are harmed.

### Experiment 1
 - Model: google/flan-t5-small
 - Number of QA examples: 1000
 - Max source tokens: 128
 - Max sample tokens: 128
 - Training Loss: 2.6047
 - Epochs: 3

### Experiment 2
 - Model: google/flan-t5-small
 - Number of QA examples: 14976
 - Max source tokens: 128
 - Max sample tokens: 128
 - Training Loss: 2.2298
 - Epochs: 3

### Experiment 3
 - Model: google/flan-t5-small
 - Number of QA examples: 14976
 - Max source tokens: 128
 - Max sample tokens: 256
 - Training Loss: 2.3603
 - Epochs: 3

### Experiment 4
 - Model: google/flan-t5-base
 - Number of QA examples: 14976
 - Max source tokens: 128
 - Max sample tokens: 128
 - Training Loss: 1.8207
 - Epochs: 3

<img src="https://github.com/fhsilveira/medical-qa-system/blob/main/experiment_4_plot_loss.png?raw=true">(Experiment 4 - Training vs Evaluation Loss)

## Examples

Q: What are the treatments for Idiopathic Pulmonary Fibrosis ?

A: How might idiopathic pulmonary fibrosis (IPF) be treated? Treatment of patients with IPF consists of controlling the rate of blood flow from the brain to the heart, or by increasing the amount of fluids in the lungs. Pulmonary fibrosis may be treated with treatment based on the cause of the condition. Treatment depends on the following: - Symptoms - Medications - Surgery - Arthralgia - Pulmonary transplant - Genetic testing - Surgery may be necessary to prevent the progression of IPF

Q: Do you have information about Coronary Artery Bypass Surgery

A: Summary : Coronary artery bypass surgery involves the surgery to close the heart muscle. The surgeon places a catheter or balloon at the center of the coronary artery, allowing it to travel through the heart. This will help your heart beat faster, easier, and prevent any complications. The surgery helps your body break down sugars and blood vessels. It helps to reduce the pressure in the heart. A heart attack means the disease can stop - Infections such as heart attacks, heart failure, stroke, and cardiomyopathy. Most heart surgeries 

Q: What is (are) galactosialidosis ?

A: Galactosialidosis is a disorder in which a sugar gland forms a small blood vessel to carry fats and other carbohydrates to the body. This vessel is called a glucuronis, which is found in many foods and in plants, including - potatoes, cabbage, broccoli, cabbage, lettuce, tomatoes and green beans - beans, red pepper, onions, and other vegetables - grapefruits, beets, onions and garlic - agar, garlic powder, tomatoes, peppers, onions, onions and garlic - fatty liver and kidney mal

## Potential improvements
 - Use a cloud service to run the fine-tuning pipeline using a larger model and the full sample size window of 512 tokens.
 - Use similarity to remove duplicates considering small question variations.
 - Clean punctuation, trailing characters and questions included in the answers.
 - Annotate long detailed descriptions again as answers.
 - Augment the dataset with other relevant medical and compatible dataset
 - Calibrate hyper-parameters again using a larger model from the same Flan-T5 family.
 - Train a more robust and state of the art LLM such as Llama-3.1. It's architecture allows better generalization yet giving control.
