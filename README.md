# kien
VGG19 with cifar10 
from keras.datasets import cifar10
import cv2
import random
import numpy as np
from keras.utils import to_categorical
from keras.models import Sequential
from keras.layers.core import Flatten, Dense, Dropout
from keras.layers.convolutional import Convolution2D, MaxPooling2D, ZeroPadding2D
from keras.optimizers import SGD
import cv2, numpy as np
import tensorflow as tf
#load data
(x_train,y_train),(x_test,y_test)=cifar10.load_data()
# random.sample(list(range(x_test.shape[0])), 2000)

ind_train = random.sample(list(range(x_train.shape[0])), 2000)
x_train = x_train[ind_train]
y_train = y_train[ind_train]
# test data
ind_test = random.sample(list(range(x_test.shape[0])), 2000)
x_test = x_test[ind_test]
y_test = y_test[ind_test]
#resize image
def resize_data(data):
    data_upscale=np.zeros((data.shape[0],224,224,3))
    for i,img in enumerate(data):
        l_img=cv2.resize(img,dsize=(224,224),interpolation=cv2.INTER_CUBIC)
        data_upscale[i]=l_img
    return data_upscale
# resize train and  test data
x_train_r = resize_data(x_train)
x_test_r = resize_data(x_test)
# make explained variable hot-encoded
train_y= to_categorical(y_train)
test_y = to_categorical(y_test)

def VGG_19(weights=None, include_top=True, classes=10,input_shape=(224,224,3)):
    model = Sequential()
    model.add(ZeroPadding2D((1,1),input_shape=(224,224,3)))
    model.add(Convolution2D(64, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(64, 3, 3, activation='relu'))
    model.add(MaxPooling2D((2,2), strides=(2,2)))

    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(128, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(128, 3, 3, activation='relu'))
    model.add(MaxPooling2D((2,2), strides=(2,2)))

    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(256, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(256, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(256, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(256, 3, 3, activation='relu'))
    model.add(MaxPooling2D((2,2), strides=(2,2)))

    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(MaxPooling2D((2,2), strides=(2,2)))

    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(ZeroPadding2D((1,1)))
    model.add(Convolution2D(512, 3, 3, activation='relu'))
    model.add(MaxPooling2D((2,2), strides=(2,2)))

    model.add(Flatten())
    model.add(Dense(4096, activation='relu'))
    model.add(Dropout(0.5))
    model.add(Dense(4096, activation='relu'))
    model.add(Dropout(0.5))
    model.add(Dense(10, activation='softmax'))
    return model
model = VGG_19(input_shape = (224, 224, 3), classes = 10)
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
history=model.fit(x_train_r , train_y, epochs=5, batch_size=20, validation_data=(x_test_r , test_y ))
model.summary()




