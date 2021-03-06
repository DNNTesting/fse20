﻿# Model Accuracy under Different Scenarios (Table 3)

---
## Quick Start Based on Our Data:

1. Download the dataset from google drive link: https://drive.google.com/drive/folders/1WXqnuBT0FISMyYuYGShbjGwOxK7nHdGK?usp=sharing and merge them to the 'data' folder. Then download other data from google drive link: https://drive.google.com/drive/folders/1-QppLvGt2LdTd9OJIRSUYz_wpcNg7Wcu?usp=sharing and merge them to the 'data' folder again and 'new_model' folder, respectively. For the datasets to be put in 'data' folder, there must be several files that have same names as the files from the first link. Please replace those from the first link. 

2. We have put [commands](https://github.com/DNNTesting/CovTesting/blob/59c3005040dae76c5531a224ce2ce45696b385d3/Model%20Accuracy%20under%20Different%20Scenarios/test_example.sh#L7-L16)  in 'test_example.sh' script for reference. To get the results of model accuracy under different scenarios for CIFAR VGG16 model and SVHN SADL-1 model, please run the following commands:

   ```
   python all.py  -dataset cifar  -model vgg16
   python all.py  -dataset svhn  -model svhn_model
   ```
   
   The results will be stored in 'result.txt'. 
   
   *(We only keep the original models and datasets we used to get the results of CIFAR VGG16 and SVHN SADL-1. But users can generate their own models and datasets to get the similar results according to the steps in the following '**Experiment on Your Own Data**' section. We will take the steps to generate the results of MNIST LeNet-1 as an example.)*
   
   To understand the meaning of the numbers in 'result.txt', we can take 'the result of cifar vgg16' as an example. The numbers after 'Benign-dh' is the numbers at the 8th row and the 3rd column in Table 3. The numbers after 'DH-dh' is the numbers at the 8th row and the 4th column. The numbers after 'PGD-dh' is the numbers at the 8th row and the 5th column. The numbers after 'DH-pgd' is the numbers at the 10th row and the 4th column. As for the numbers at the 10th row and the 3rd column and the numbers at the 10th row and the 5th column in Table 3, they can be gotten from the data used to generate Figure 4. 
   



## Experiment on Your Own Data (Use LeNet1 model as example):

1. Download the dataset from google drive link: https://drive.google.com/drive/folders/1WXqnuBT0FISMyYuYGShbjGwOxK7nHdGK?usp=sharing and merge them to the 'data' folder. Then download other data from google drive link: https://drive.google.com/drive/folders/1-QppLvGt2LdTd9OJIRSUYz_wpcNg7Wcu?usp=sharing and merge them to the 'data' folder again and 'new_model' folder, respectively. For the datasets to be put in 'data' folder, there must be several files that have same names as the files from the first link. Please replace those from the first link. 

2. Generate new training data to be added to the original training dataset using Deephunter:

   ```$ python fuzzing.py ```

   Please select the [order_number](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/fuzzing.py#L336) from 0 to 9 and repeat this step if you want to generate more new training data. All results will be stored at  'fuzzing/nc_index_{}.npy'.format(order_number). This file only shows how to fuzz LeNet1 model. If you want to do this for other models, please modify the [dataset, model_name and model_layer](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/fuzzing.py#L319-L321).

3. Use the new training datasets to retrain the model:

   ```$ python retrain_robustness.py ```

   You can modify the [codes here](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/retrain_robustness.py#L381-L394) to get the models retrained by the training dataset gotten from step 2. Please modify the number behind 'format' to use the corresponding training dataset. Here, if you want to use more training dataset,  please keep all the codes, because the new training datasets are concatenated to the original datasets one by one. The retrained model is saved at (''new_model/dp_{}.h5'.format(model_name)'). (We have put one well retrained LeNet1 model at that position. It's ok if you want to retrain your own model). This file only shows how to retrain LeNet1 model. If you want to do this for other models, please modify the [dataset and model_name](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/retrain_robustness.py#L368-L369).

4. Generate new testing dataset attacked by PGD attack:

   ```$ python pgd_attack.py ```

   The results will be stored at './data/' + dataset + '\_data/model/' + model_name + '_' + attack + '.npy'. This file only shows PGD attack to LeNet1 model. If you want to do this for other models, please modify the [dataset and model_name](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/pgd_attack.py#L266-L267).

5. Generate the new testing dataset generated by Deephunter:

   ```$ python deephunter_attack.py ```

   The results will be stored at './data/' + dataset + '\_data/model/' + 'deephunter_adv_test_{}.npy'.format(model_name)'. This file only shows Deephunter to LeNet1 model. If you want to do this for other models, please modify the [dataset and model_name](https://github.com/DNNTesting/CovTesting/blob/b4f006e28cf8d49c0d95ca71d4e52416d1466623/Table%203/deephunter_attack.py#L304-L305).

6. Get the results of model accuracy under different scenarios for MNIST LeNet1 model:

   ```$ python all.py  -dataset mnist  -model lenet1``` 

   The results will be stored in 'result.txt'.

   (The final results depend on the training parameters, the size of new training datasets, the attack parameters and some other hyper-parameters. If the new results are consistent with our conclusion, then the new results are acceptable.)

   

   