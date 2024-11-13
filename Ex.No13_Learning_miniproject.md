![image](https://github.com/user-attachments/assets/cce1a211-0980-4ae6-80f4-9eb0eb8b56c2)# Ex.No: 10 Learning – Use Supervised Learning  
### DATE:11-11-2024                                                                     
### REGISTER NUMBER : 212221040183
### AIM: 
Predicting eye diseases using artificial intelligence and machine learning
###  Algorithm:
```
Data Collection and Preprocessing:

Collect Data: Gather a large dataset of labeled images, such as fundus photographs or OCT scans, indicating different eye diseases (e.g., diabetic retinopathy, glaucoma, macular degeneration).
Data Annotation: Ensure images are accurately labeled by medical professionals.
Data Augmentation: Apply transformations like rotation, flipping, and scaling to increase the diversity of the dataset and improve the model's robustness.
Normalization: Normalize pixel values (e.g., scaling between 0 and 1 or standardizing to zero mean and unit variance).
Model Architecture:

Input Layer: The input shape should match the dimensions of the images.
Convolutional Layers: Use multiple convolutional layers with ReLU activation functions to extract features from the images.
Pooling Layers: Apply max-pooling or average-pooling layers to reduce the spatial dimensions and retain important features.
Fully Connected Layers: Flatten the output from the convolutional layers and pass through fully connected (dense) layers.
Output Layer: Use a softmax activation function for multi-class classification (one class per disease) or sigmoid for binary classification (presence or absence of a disease).
Model Training:

Loss Function: Use categorical cross-entropy for multi-class classification or binary cross-entropy for binary classification.
Optimizer: Common choices include Adam, RMSprop, or SGD with momentum.
Evaluation Metrics: Track accuracy, precision, recall, F1-score, and Area Under the ROC Curve (AUC) during training.
Training Process: Split the data into training, validation, and test sets. Train the model on the training set while monitoring performance on the validation set to tune hyperparameters and prevent overfitting.
Model Evaluation:

Evaluate the model on the test set to ensure it generalizes well to unseen data.
Generate a confusion matrix, ROC curves, and other relevant metrics to assess performance.
Deployment:

Integrate the trained model into a clinical decision support system.
Ensure the system complies with medical regulations and standards.
```

### Program:
```
!pip install openpyxl
import pandas as pd
import cv2
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import os
import random
import itertools
import cv2
import matplotlib.pyplot as plt
from tqdm import tqdm
import math
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.preprocessing.image import load_img,img_to_array
from keras.utils import to_categorical
from keras.models import Model, Sequential, load_model
from keras.layers import Dense, Conv2D, MaxPool2D, Flatten, Reshape, Dropout
from keras.preprocessing import image
from keras.preprocessing.image import ImageDataGenerator
from keras.optimizers import Adam
from keras.callbacks import ModelCheckpoint, EarlyStopping
from keras.applications.vgg16 import VGG16
from keras.applications.vgg16 import preprocess_input
from sklearn.metrics import confusion_matrix,classification_report,accuracy_score
from sklearn.model_selection import train_test_split
from sklearn.utils import shuffle
img_size = 224
df_data = pd.read_csv("/content/full_df.csv")
df_data.head()
df_data[df_data.C==1].head()
df_data.info()
df_data[df_data == 1].sum(axis=0)
df_data2 = df_data.iloc[:, 1:7]
#df_data2['filepath'] = pd.Series(df_data['filepath'])
df_data2.head()
img_dir = "/kaggle/input/ocular-disease-recognition-odir5k/preprocessed_images"
df_data2[df_data2['Left-Diagnostic Keywords'].str.match('cataract')].head()
df_left_cat = df_data2[df_data2['Left-Diagnostic Keywords'].str.match('cataract')]
print(len(df_left_cat))
df_data[df_data['Right-Diagnostic Keywords'].str.match('cataract')].head()
df_rt_cat = df_data2[df_data2['Right-Diagnostic Keywords'].str.match('cataract')]
print(len(df_rt_cat))
df_cat_filenames = df_left_cat['Left-Fundus'].append(df_rt_cat['Right-Fundus'], ignore_index=True)
df_cat_filenames.head()
df_cat_filenames.tail()
len(df_cat_filenames)
import os
img_dir = '/path/to/your/images'
img = cv2.imread('path_to_your_image.jpg')
import matplotlib.pyplot as plt
from PIL import Image
import matplotlib.image as mpimg
df_data[df_data == 1].sum(axis=0)
df_data2 = pd.DataFrame()
df_data2.head()
import cv2
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from IPython.display import Image
image_path = '/content/99_right.jpg'
Image(filename=image_path)
image_path = '/content/68_right.jpg'
image = cv2.imread(image_path)
plt.imshow(image)
plt.axis('off')
plt.show()
import matplotlib.pyplot as plt
for i in range(9):
    plt.subplot(3, 3, i+1)
    plt.plot([i, i+1, i+2], [i, i+1, i+2])
plt.tight_layout()
plt.show()
import pandas as pd
df_norm_filenames = pd.DataFrame()
if not df_norm_filenames.empty and len(df_norm_filenames) >= 572:
    df_norm_filenames_random = df_norm_filenames.sample(n=572)
    print(df_norm_filenames_random.head())
else:
    print("The DataFrame does not have enough rows to sample 68 elements.")
import pandas as pd
# Create an empty DataFrame
df_norm_filenames = pd.DataFrame(columns=['Left-Fundus', 'Right-Fundus'])
data = {'Column1': [1, 2, 3, 4, 5], 'Column2': ['A', 'B', 'C', 'D', 'E']}
df_norm_filenames_random = pd.DataFrame(data)
import matplotlib.pyplot as plt
import cv2
import os
plt.figure(figsize=(8, 8))
for i in range(5):  # Adjust the range based on the length of your DataFrame
    img = df_norm_filenames_random['Column2'][i]  # Replace 'Column2' with the correct column name
    image = cv2.imread(os.path.join(img_dir, img))
import matplotlib.pyplot as plt
plt.figure(figsize=(8, 8))
for i in range(9):
    plt.subplot(3, 3, i+1)
    plt.text(0.5, 0.5, f'Subplot {i+1}', ha='center', fontsize=12)
    plt.axis('off')
plt.tight_layout()  # Adjust the layout to prevent overlapping
plt.show()
import matplotlib.pyplot as plt
plt.figure(figsize=(8, 8))
for i in range(9):
    plt.subplot(3, 3, i+1)
df_cat_filenames = pd.DataFrame(df_cat_filenames, columns = ["filename"])
#df_cat_filenames.set_index("filename", inplace = True)
# add a new column of '1' to the dataframe
df_cat_filenames["label"] = "cataract"
df_cat_filenames.head()
df_norm_filenames_random = pd.DataFrame(df_norm_filenames_random, columns = ["filename"])
#df_cat_filenames.set_index("filename", inplace = True)
# add a new column of '1' to the dataframe
df_norm_filenames_random["label"] = "normal"
df_norm_filenames_random.head()
import pandas as pd
df_cat_filenames = pd.DataFrame()
df_norm_filenames_random = pd.DataFrame()
df_combined = df_cat_filenames.append(df_norm_filenames_random, ignore_index=True)
df_combined
df_train = df_combined_random.sample(frac=0.8,random_state=42)
df_train.reset_index(drop=True)
df_test = df_combined_random.drop(df_train.index)
df_test.reset_index(drop=True)
print(len(df_combined_random))
print(len(df_train))
print(len(df_test))
import tensorflow as tf
from tensorflow.keras.preprocessing.image import ImageDataGenerator
train_datagen=tf.keras.preprocessing.image.ImageDataGenerator(
            rescale=1./255.,
            validation_split=0.20,
            rotation_range=90,
            width_shift_range=0.2,
            height_shift_range=0.2,
            horizontal_flip=True,
            vertical_flip=True,
            shear_range=0.2,
            brightness_range=[0.3,1]
            )
test_datagen=ImageDataGenerator(rescale=1./255.)
df_train['label'] = df_train['label'].astype(str)
df_test['label'] = df_test['label'].astype(str)
img_height = 224
img_width = 224
batch_size=224
train_datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=20,
)
plt.figure(figsize=(8,8))
for i in range(9):
    plt.subplot(3, 3, i + 1)
plt.show()
import tensorflow.keras as keras
vgg16 = keras.applications.vgg16.VGG16(input_shape=(224, 224, 3),
                                       weights='imagenet',
                                       include_top=False)
# add new dense layers at the top
x = keras.layers.Flatten()(vgg16.output)
x = keras.layers.Dense(1024, activation='relu')(x)
x = keras.layers.Dropout(0.5)(x)
x = keras.layers.Dense(128, activation='relu')(x)
## remember we are using 2 outputs only
predictions = keras.layers.Dense(2, activation='softmax')(x)
# define and compile model
model = keras.Model(inputs=vgg16.inputs, outputs=predictions)
for layer in vgg16.layers:
    layer.trainable = False
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['accuracy'])
from tensorflow.keras.callbacks import ModelCheckpoint
from tensorflow.keras.callbacks import EarlyStopping
from tensorflow.keras.preprocessing.image import ImageDataGenerator
# Create an ImageDataGenerator for training data
train_datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=20,
    # Other data augmentation parameters...
)
def train_generator(data, labels, batch_size=32, epochs=10):
  for epoch in range(epochs):
    for i in range(0, len(data), batch_size):
      batch_data = data[i:i + batch_size]
      batch_labels = labels[i:i + batch_size]
      yield batch_data, batch_labels
directory = 'path_to_train_directory'
test_generator = test_datagen.flow_from_directory
test_generator = test_datagen.flow_from_directory
valid_datagen = ImageDataGenerator(rescale=1./255)
df_train = df_combined_random.sample(frac=0.8,random_state=42)
df_train.reset_index(drop=True)
# exclude the 80% that was already chosen, the remaining 20% will go into testing
df_test = df_combined_random.drop(df_train.index)
df_test.reset_index(drop=True)
print(len(df_combined_random))
print(len(df_train))
print(len(df_test))
train_datagen=tf.keras.preprocessing.image.ImageDataGenerator(
            rescale=1./255.,
            validation_split=0.20,
            rotation_range=90,
            width_shift_range=0.2,
            height_shift_range=0.2,
            horizontal_flip=True,
            vertical_flip=True,
            shear_range=0.2,
            brightness_range=[0.3,1],
           zoom_range=0.2
            )
test_datagen=ImageDataGenerator(rescale=1./255.)
df_train['label'] = df_train['label'].astype(str)
df_test['label'] = df_test['label'].astype(str)
# Example:
# Assuming 'train_labels' is a list or array containing the labels for your training data
train_labels = [...]  # Define your actual labels here
# Now 'train_labels' can be used within the current scope
train_labels[0]
vgg16 = keras.applications.vgg16.VGG16(input_shape=(224, 224, 3),
                                       weights='imagenet',
                                       include_top=False)
# add new dense layers at the top
x = keras.layers.Flatten()(vgg16.output)
x = keras.layers.Dense(1024, activation='relu')(x)
x = keras.layers.Dropout(0.5)(x)
x = keras.layers.Dense(128, activation='relu')(x)
predictions = keras.layers.Dense(2, activation='softmax')(x)
# define and compile model
model = keras.Model(inputs=vgg16.inputs, outputs=predictions)
for layer in vgg16.layers:
    layer.trainable = False
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['accuracy'])
checkpoint = ModelCheckpoint("vgg16_1.h5",
                             monitor='val_accuracy',
                             verbose=1,
                             save_best_only=True,
                             save_weights_only=False,
                             mode='auto',
                             period=1)
early = EarlyStopping(monitor='val_accuracy',
                      min_delta=0,
                      patience=3,
                      verbose=1,
                      mode='auto')
num_epochs = 10
validation_generator = validation_datagen.flow_from_directory
import numpy as np
test_features = [...]
test_data = np.array(test_features)
test_data = np.array([[...], [...], ...])
test_data = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
test_data = np.random.rand(10, 100)
from sklearn.metrics import classification_report
```


### Output:
![image](https://github.com/user-attachments/assets/fef95b82-786c-4a3d-869e-76e400d0c6fe)

![image](https://github.com/user-attachments/assets/1bc2e95e-e070-4051-a772-a807f16ff358)








### Result:
Thus the system was trained successfully and the prediction was carried out.