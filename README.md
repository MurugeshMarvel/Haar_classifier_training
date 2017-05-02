# Haar Cascade Classifier
##### This Project is an implementation of Haar like feature algorithm to train images with opencv framework
#### For more info please refer to this tutorial on Haar-like feature
[Haar-like Feature](https://singhgaganpreet.wordpress.com/tag/explaining-haar-cascade)
[Cascade Classifier Training-OpenCV](http://docs.opencv.org/2.4.13.2/doc/user_guide/ug_traincascade.html)
### Prerequisite
1. Linux OS
2. Python
3. ##### Positive Images
   Images which you want to detect. Crop exactly the image portion in the pictures. Better collect more than 50 images minimum in the same size not exceeding 100x100</p></H5></li>
4. ##### Negative Images
   Images that doesn't contain the image you want to detect, best collected the normal pictures of the surronding where you want to install this system. Collect more than 200 images of same size which would be more than 640x320

### Steps to be followed
1. ##### Install OpenCV
   [Guide to Install OpenCV](http://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html)
2. ##### Clone this repository
```bash
git clone https://github.com/MurugeshMarvel/Haar_classifier_training.git
```
3. Make two folders named positive_images and negative images inside the datasets folder
'''bash
cd datasets
mkdir positive_images
mkdir negative_images
'''
4. Drop all the cropped positives images in the datasets/positive_images folder
   *all the images should be in the same size and the size should not exceed 100x100, for unifying*
   *the size use the mogrify tool*
   '''bash
	mogrify -resize "width x height" *
   '''
5. Drop all the collected negative images in the datasets/negative_images folder
   *all the images should be in the same size and on the average make the size as 640x320 or more*
6. Create positive and negative images list by the following command inside the project home folder
   '''bash
   	find ./datasets/positive_images -iname "*.jpg" > positive_images.txt
	find ./datasets/negative_images -iname "*.jpg" > negative_images.txt
   '''
7. Create different type of sample images with respect to the collected positive and negative images
   using the following perl program and the resulted vectors of the sample images will get saved in
   the "sample_vectors" folder.
   '''bash
	perl codes/createsamples.pl positives_images.txt negative_images.txt samples 2000\
	"opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1\
	-maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 30 -h 30"
   '''
   *You can change the 2000 depending on the no. of sample images you want and change the width and*
   *height from 30x30 to you positive image size*
8. Then merge all the created sample vectors into a single unified vector by the following command
   '''
	python ./codes/mergevec.py -v sample_vectors/ -o unified_vector/unified_sample.vec
   '''
9. After proceeding all the steps without any error, start to train the classifier using the opencv   	 framework and save the results of the training of each stages to the classifier_results folder.
   '''
	opencv_traincascade -data classifier_results -vec unified_sample/unified_sample.vec -bg\
	negative_images.txt -numstages 10 -minHitRate 0.99 -maxFalseAlarmRate 0.5 -numPos 1500\
	-numNeg 800 -w 30 -h 30 -mode ALL -precalcValBufSize 1024 -precalcIdxBufSize 1024
   '''
10. The above process would take days to finish and the result will get saved in the                 	classifier_results folder as "cascade.xml".

