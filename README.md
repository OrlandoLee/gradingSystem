gradingSystem
=============

an automatic grading system

This system is based on OCRopus(https://code.google.com/p/ocropus/).
The usage is in its wiki(https://code.google.com/p/ocropus/w/list).
We did some improvment by using true value to guide this system in ocrolib/lstm.py translate_back function.

Usually steps.
TRAINING
1. convert -units PixelsPerInch ~/img/combine/$i.png -density 300  ~/img/combine/$i.png(rescale image To 300dpi)
2.$ ocropus-nlbin ~/img/combine/1.png ~/img/combine/2.png -o ~/ocropus/ocropy/data/spaceTrainData (preprocess the image for example adjust the gray white scale)
3.$ ocropus-gpageseg trainingSet/????.bin.png (preprocess the image split image into lines)
4. add .gt.txt file according to trainingset. For example, if the image 01000a.bin.png has 123456 on it, we should have 01000a.gt.txt with text 123456 in it.
5.$ ocropus-rtrain --load the_most_you_want_to_load training_set -o output_training_set -F 200(frequency)
example:  ~/ocropus/ocropy/ocropus-rtrain  --load $(ls random_number_0_9_space* | sort | tail -1)   ~/ocropus/ocropy/data/spaceTrainData/0001/*.png ~/ocropus/ocropy/data/spaceTrainData/0002/*.png -o random_number_0_9_space -F 200
You can ctrl-C if you want to stop training it.

Recognition
1. convert -units PixelsPerInch ~/img/combine/$i.png -density 300  ~/img/combine/$i.png(rescale image To 300dpi)
2.$ ocropus-nlbin -o mybook page1.png page2.png
3.$ ocropus-gpageseg mybook/????.bin.png
4.$ ocropus-rpred mybook/????/??????.bin.png -m trained_model_you_want to use. For example in our example:ocropus-rpred mybook/????/??????.bin.png -m models/random_number_0_9_no_space-00010400.pyrnn.gz
