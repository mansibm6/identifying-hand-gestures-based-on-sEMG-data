
# Identifying Hand Gestures based on sEMG Data

In this project, we used sEMG readings from human subjects to detect their hand gestures. 


## Authors

- [@mansibm6](https://www.github.com/mansibm6)


## Dataset

Dataset Link - [Ninapro DB1 Dataset](http://ninaweb.hevs.ch/node/3)

The first Ninapro database includes data from 27 intact subjects acquired with the acquisition protocol described into the papers: "Manfredo Atzori, Arjan Gijsberts, Ilja Kuzborskij, Simone Heynen, Anne-Gabrielle Mittaz Hager, Olivier Deriaz, Claudio Castellini, Henning Müller and Barbara Caputo. Characterization of a Benchmark Database for Myoelectric Movement Classification. IEEE Transactions on Neural Systems and Rehabilitation Engineering, 2014" (http://publications.hevs.ch/index.php/publications/show/1715)

and "Manfredo Atzori, Arjan Gijsberts, Claudio Castellini, Barbara Caputo, Anne-Gabrielle Mittaz Hager, Simone Elsig, Giorgio Giatsidis, Franco Bassetto & Henning Müller. Electromyography data for non-invasive naturally-controlled robotic hand prostheses. Scientific Data, 2014" (http://www.nature.com/articles/sdata201453).

The database includes 10 repetitions of 52 different movements of 27 intact subjects.
The subjects have to repeat several movement represented by movies that are shown on the screen of a laptop.
The experiment is divided in three exercises:
1. Basic movements of the fingers
2. Isometric, isotonic hand configurations and basic wrist movements
3. Grasping and functional movements
The sEMG data are acquired using 10 Otto Bock MyoBock 13E200 electrodes, while kinematic data are acquired using a Cyberglove 2 data glove.

For each exercise, for each subject, the database contains one matlab file with synchronized variables.
The variables included in the matlab files are:
- subject: subject number
- exercise: exercise number
- emg (10 columns): sEMG signal of the electrodes
- glove (22 columns): uncalibrated signal from the 22 sensors of the cyberglove
- stimulus (1 column): the movement repeated by the subject.
- restimulus (1 column): again the movement repeated by the subject. In this case the duration of the movement label is refined a-posteriori in order to correspond to the real movement. Please, use the paper Gijsberts et al., 2014 (http://publications.hevs.ch/index.php/publications/show/1629) for more details about relabelling procedure
- repetition (1 column): repetition of the stimulus
- rerepetition (1 column): repetition of restimulus

## Methodology

The DB1 database from Ninapro contained a total of 81 MatLab files. We accessed all of them using a Python script. We transformed the data from lists of arrays to a pandas dataframe with rows and columns. We removed the columns that seemed unnecessary to our goal and that would create problems for our machine learning models. In the end, we kept the 10 EMG columns, the exercise column, and the stimulus column. We converted the dataframe into a CSV file. This task was done using the csv_generator.py script.

The next part was done in Google Colab. We accessed the CSV file in Google Colab and converted it back into a Pandas dataframe. 
We wanted to use only one of the 3 exercises so we filtered out the rows with exercise value 1. Exercise 1 corresponds to Exercise A of "Manfredo Atzori, Arjan Gijsberts, Claudio Castellini, Barbara Caputo, Anne-Gabrielle Mittaz Hager, Simone Elsig, Giorgio Giatsidis, Franco Bassetto & Henning Müller. Electromyography data for non-invasive naturally-controlled robotic hand prostheses. Scientific Data, 2014" (http://www.nature.com/articles/sdata201453). Exercise A includes 12 hand gestures:
1.	Index flexion
2.	Index extension
3.	Middle flexion
4.	Middle extension
5.	Ring flexion
6.	Ring extension
7.	Little finger flexion
8.	Little finger extension
9.	Thumb adduction
10.	Thumb abduciton
11.	Thumb flexion
12.	Thumb extension
It also includes a rest state. This corresponds to the 13 classes of the stimulus column with 0 as the rest class.


We deleted the exercise column as it had a constant value of 1 throughout the dataset. The resultant dataset contained 10 EMG columns as X_labeles and one stimulus column as y_label. The next step was to split the dataset into train and test sets. We split the data such that 70% of it lay in the training set and 30% in the test set.
 
Our next step was to apply feature scaling.
We found that the Standardization process yields better results. We used the StandardScaler transformer for this. We applied this to the X_labels of both training and test sets. This step concluded our data preprocessing tasks. 

We trained and evaluated the model on KNN Classifier (neighbors = 7), Decision Tree Classifier, and Random Forest Classifier (estimators = 50).The accuracies from these models: 
- KNN Classifier: 71.8%
- Decision Tree Classifier: 63.5%
- Random Forest Classifier: 74.9%


## Conclusion

From the project, we concluded that the Random Forest Classifier (with 50 estimators) performed best. We are also working on adding new feature extractions steps to this project and train a custom neural network to get a model that perfoms even beteter. 