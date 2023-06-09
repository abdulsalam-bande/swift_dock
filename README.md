# Swift Dock


In this study, various machine learning (ML) models were tested in an effort to forecast the docking scores of a ligand for a specific target protein, bypassing the need for detailed docking calculations. The primary objective is to identify a regression model capable of accurately determining the docking scores of ligands within a chemical library, relative to a target protein. This would be achieved using data derived from explicit docking of only a small selection of molecules.

Among the ML models utilized is a neural network model based on Long Short-Term Memory (LSTM). LSTMs are typically deployed in the handling of sequence data within Natural Language Processing (NLP) contexts, such as speech recognition. The specific LSTM model employed in this study is combined with an attention mechanism to enable the neural network model to more effectively distill useful characteristics from incoming ligand data. Pytorch, a widely-used Python ML framework, was utilized to implement the LSTM.

Additionally, several other models were also explored, including XGBoost, which was executed via the XGBoost Python library, as well as decision tree regression and stochastic gradient descent models drawn from the scikit-learn Python library.


# Setting up the environment

1. Make sure Python 3.7 is installed on your system
2. Create a virtual environment and run  pip install -r requirements.txt
## Training Using LSTM
### First build a model and get validation results 
 There are many options to train the lstm model depending on the target, descriptor and training_size of your choice, you can also use 5 fold cross validation. So lets you have a csv file named 'docking_scores.csv' and you want use the mac descriptor and using 50 molecules selected randomly with 5 fold cross validation. Use this steps.
   1. Put your target in the dataset folder. You should use the format of the sample test.csv target in the dataset folder.
   2. Example -  src/models run `python main_lstm.py --input docking_scores.csv --descriptors mac --training_sizes 50 --cross_validation True`. This will train a model using 50 molecules selected randomly from target 'docking_scores.csv' file using mac descriptor without 5 cross validation
4. Above code will produce validation_scores.txt file where different performance metrics (R-squared, mean absolute error). This will give you information about the prediction power of this model

Note that docking_scores.csv file should contain the following columns separated by comma.
-first column: docking score
-second column: smiles
### Build a model to predict docking scores of new molecules 
 Here we will apply the same command as above without using cross validation. Here you should use cross_validation False
 Also put a name for the model to built. ex: --modelname lstm_model.pt
 Example -  src/models run `python main_lstm.py --input docking_scores.csv --descriptors mac --training_sizes 100 --cross_validation False --modelname lstm_model.pt `. 
 This will built a model using 100 molecules selected randomly from target 'docking_scores.csv' file using mac descriptor without 5 cross validation
This code will produce lstm_model.pt file that will be used to predict new molecules
### Making Prediction for your target using LSTM
1. For the molecules you want to predict the docking scores of. Make sure your input csv has the same format at molecules_for_prediction.csv (containing smiles of molecules) in the dataset folder
2. Under src/models, run `python other_models_inference.py --input_file molecules_for_prediction.csv  --output_dir prediction_results --model_name lstm_model.pt`
3. The model_name is the path to the model of your choice that you get after training that is saved in the previous step. Example 'lstm_model.pt'

## Training Using other models (from scikit-learn)
1. To train the models using other models other than lstm, first create the dataset. So lets you have a target named 'my_target' and you want use the mac descriptor and a training size of 50 molecules with 5 fold cross validation. Use this steps.
2. Put your target under the datasets folder
3. Under src/utils, run `python create_fingerprint_data.py --targets my_target`. This will create the dataset for the 'my_target' target.
4. Next, under src/models run `python main_ml.py --targets my_target --descriptors mac --training_sizes 50 --regressor sgreg --cross_validate True`. This will train the sgreg model using 'my_target' target us for training size of 50 with cross_validation.

# Making Prediction for your target with other models (sgreg, xgboost, decision tree)
1. For the molecules you want to predict the docking scores of. Make sure your input csv has the same format at input.csv in the dataset folder
2. Under src/models, run `python other_models_inference.py --input_file --output_dir --model_id`
3. The model_id is the path to the model of your choice that you get after training that is saved under results/serialized_models. Example 'lstm_target_mac_50_model.pt'

