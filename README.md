# Automated System for Solving CAPTCHAs
CAPTCHAs (Completely Automated Public Turing test to tell Computers and Humans Apart) are popular ways of preventing bots from attempting to log on to systems by extensively searching the password space. In its traditional form, an image is given which contains a few characters (sometimes with some obfuscation thrown in). The challenge is to identify what those characters are and in what order. In this project, we wish to crack these challenges.

##### So I haved developed a computer program to automatically solve traditional CAPTCHA challenges by identifying characters in obfuscated images using image processing and machine learning.
---

#### Method to find out what character are present in the image:
- Initial image

![alt text](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/1.Initial-img.png?raw=true)

1.	Find corner pixel with max frequency to get the background colour of the image 
2.	Change the background to white

![Image without background](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/2.Background-Extraction.png?raw=true)

3.	Now Dilate the image to remove the stray lines 

![Dilated Image](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/3.Dilated-image.png?raw=true)

4.	Convert the image to grayscale

![Grayscale Image](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/4.Grayscale.png?raw=true)

5.	Segment image into 3 characters 
6.	Iterate over the columns of the image
7.	Check the frequency of number of non white pixels in each column 
8.	This will help to get the start and end coordinates of the character 
9.	Here, We have used window size of 30 pixel to detect characters and ignore any remaining noise (small stray lines) after dilation
10.	Get the bounding box of the three characters

![First letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_1_150x150.png?raw=true)

![Second letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_2_150x150.png?raw=true)

![Third letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_3_150x150.png?raw=true)

11.	We found 37 such images where our method was not able to segment images in three characters out of 2000 images. 
12.	So we divide the image equally in three segment of size 150x150 by leaving margin of 15 pixel in the beginning and 10 pixel in the following two.
13.	Extracting each character from the image using the bounding box we get from the above approach.
14.	Now we resize the image into 30x30 pixel to ...

![First letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_1_30x30.png?raw=true)

![Second letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_2_30x30.png?raw=true)

![Third letter](https://github.com/saqeeb360/Automated-System-for-Solving-CAPTCHAs/blob/master/symbol_3_30x30.png?raw=true)

15.	Now we flatten the image to convert into 1D array
16.	This the the feature vector of one character

#### Character Label to Numeric Label
- Using dictionary we encode each character to a numeric label from 0 to 23
{'ALPHA' : 0,
 'BETA' : 1,
 'CHI' : 2,
 'DELTA' : 3,
 'EPSILON': 4,
 'ETA' : 5,
 'GAMMA' : 6,
 'IOTA' : 7,
 'KAPPA' : 8,
 'LAMDA': 9,
 'MU' :10,
 'NU' : 11,
 'OMEGA' : 12,
 'OMICRON':13,
 'PHI' : 14,
 'PI' : 15,
 'PSI' : 16,
 'RHO' : 17,
 'SIGMA' : 18,
 'TAU' : 19,
 'THETA' : 20,
 'UPSILON' : 21,
 'XI' : 22,
 'ZETA': 23}

#### Numeric Label to Character Label
- Using dictionary we decode each character from a numeric label.
{0 : 'ALPHA',
 1: 'BETA',
 2: 'CHI',
 3: 'DELTA',
 4: 'EPSILON',
 5: 'ETA',
 6: 'GAMMA',
 7: 'IOTA',
 8: 'KAPPA',
 9: 'LAMDA',
 10: 'MU',
 11: 'NU',
 12: 'OMEGA',
 13: 'OMICRON',
 14: 'PHI',
 15: 'PI',
 16: 'PSI',
 17: 'RHO',
 18: 'SIGMA',
 19: 'TAU',
 20: 'THETA',
 21: 'UPSILON',
 22: 'XI',
 23: 'ZETA'}

#### Training
- For trying we have used Logistics Regression with 5000 iterations.
