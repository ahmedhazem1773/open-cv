1. من دلوقتي هنطبق اغلب الي اتعلمناه 
2. هنستخدم حاجة من 3 methodes الي جايين
![Pasted image 20250821142416](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Pasted%20image%2020250821142416.png)
3. هنا هتلاقي مش لازم الصورة تكون هي هي تماما بالظبط عشان اعرف ادور و كده 
4. هي العملية بتتم ازاي اصلا احنا بنستخد حاجات اسمها feature ectraction دي methodes  بتقعد بتدور في الصورة عموما عن features  زي edges , corners , etc و وصف بتاع feature  الزاوية بتاعتها و مكانه و هكذا و ده اسمه descriptors و بعد كده بعد ما نطبق ده عي صورتين بنعمل بعد كدهcomparing  ما بين الfeature في الصورتين بحيث نعمل matching
## 1. Brute Force Detection with ORB Descriptors

1. اول حاجة كدخ ان orb هي عبارة عن fusion ما بين حاجتين حاجة اسمها fast بترجع keypoints دي النقط و الاتجاه و بتاع بس بوينتر  و brief  بترجع descriptors دي الوجود فين 
2. هتلاحظ الصورة الي عايزين ندور ليها matching فيها جزء زيادة عن الصورة الي هندور فيها زيادة في label  مكتوب عليه حجم عائلي و الlabel  ده موجود في حاجات تانية
3. الاول نعملorb obj detector  و نخليه يدور في كل صورة و بيرجع key & des  و هو بياخد الصورة و ماسك لو عايز 
4. بعد كده نعمل brute force matcher obj  هنباصي جواه الmethode الي هيستخدمها و حاجة اسمها cross check  بتخليها true  عشان تطلعلك نتايج احسن 
5. بعد كده نستخدم func  بتاعت الobj ده نباصي فيها des1 , des2 عشان نعمل match
6. بعد كده نعمل اعادة ترتيب لافضل match (اقل distance) لحد اعلي  distence  الي هو اسوء عن طريق func sorted  و نعمل جواها زي func  تقارن الdistence  عشان نعرف نرتبهم عشان vari match  ده في كذا  attribute  فيها فا انا عايز اعملهم ترتيب حسب attribute  بتاع الdistance 
7. بعد كده هاخد هستخدم func  بتعمل رسم (خط او رابط) ما بين البوينتز الي فيها matching  و بنباصي فيها الاتي 
	1. الصورة الي بنعمل بيها بحث و بندور عليها في الصورة الكبيرة
	2. الkeypoints  بتاعتها 
	3. الصورة الي بندور فيها 
	4. الkeypoints  بتاعتها 
	5. بعد كده match obj  الي عملنا فيه اعادة ترتيب عموما ينفع نعمل فيه slicing  لو كم البيانات كتير الي هيتعرض و انت عايز افضل 40  match  مثلا 
	6. بعد كده الماسك و هنحطه none
	7. بعد كده flags  في methode  الي هي دي `cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS`  او انت حط رقم 2 هي مسئولة عن كيفية رسم و لو حطيت صفر هيرسم كل الfeature detection لكن هيربط الي فيهم matching 
8. هتلاقي النتيجة هنا سيئة عشان في حاجات كتير شبه الصورة و الحاجاة التانية في ختلاف نوعاما بين الي بندور عليه والصورة الي معانا و الحاجة التالتة الي بندور عليه محطوط علي جنب و مش باين اوي 
```python
import cv2

import numpy as np

import matplotlib.pyplot as plt
img1 = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\reeses_puffs.png",0)

img2 = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\many_cereals.jpg",0)
plt.imshow(img1)

plt.show()

plt.imshow(img2)

plt.show()

# Initiate ORB detector

orb = cv2.ORB_create()

print(type(orb)) # output  : <class 'cv2.ORB'>

# find the keypoints and descriptors with ORB

kp1, des1 = orb.detectAndCompute(img1,None)

kp2, des2 = orb.detectAndCompute(img2,None)

print(kp1[0]) # output : < cv2.KeyPoint 00000273BA359C50>
  
# create BFMatcher object

bf = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)

  

# Match descriptors.

matches = bf.match(des1,des2)

  

# Sort them in the order of their distance.

matches = sorted(matches, key = lambda x:x.distance)

  

# Draw first 25 matches.

reeses_matches = cv2.drawMatches(img1,kp1,img2,kp2,matches[:40],None,flags=2)

plt.imshow(reeses_matches)

plt.show()
```
![Figure_81](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_81.png)
![Figure_80](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_80.png)
![Figure_82 2.png](Figure_82%202.png)
## 2. Brute-Force Matching with SIFT Descriptors and Ratio Test
1. هي بتشتغل كويس مع الحاجات ال في منها scale  و بتاع 
2. و هنغير بس class  الي مستخدم في feature extraction 
3. هتلاقي ان احنا مستخدمين bf.knnMatch الفرق ان انت هنا بتحدد عدد الbest matches كام للنقطة الواحدة
4. هنستخدم حاجة كمان اسمها الratio  بحيث نقارن pairs of matches  لو قليل في الdistance  تمام مقبولة لا غير كده يبقي لا غير مقبولة 
5. لما نرسم بقي هنباصي الي عملنا ليهم فلترة 
6. هتلاقيني هنا نزلت مكتب
```python
img1 = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\reeses_puffs.png",0)

img2 = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\many_cereals.jpg",0)

sift = cv2.xfeatures2d.SIFT_create()
# find the keypoints and descriptors with SIFT

kp1, des1 = sift.detectAndCompute(img1,None)

kp2, des2 = sift.detectAndCompute(img2,None)

  

# BFMatcher with default params

bf = cv2.BFMatcher()

matches = bf.knnMatch(des1,des2, k=2)

  

# Apply ratio test

good = []

for match1,match2 in matches:

    if match1.distance < 0.75*match2.distance:

        good.append([match1])

  

# cv2.drawMatchesKnn expects list of lists as matches.

sift_matches = cv2.drawMatchesKnn(img1,kp1,img2,kp2,good,None,flags=2)

plt.imshow(sift_matches)

plt.show()
```
![Figure_82 2.png](Figure_82%202.png)
## 3. FLANN based Matcher
1. هتلاقي نفس الي عملناه في الي فات بس مختلف الmatcher obj 
2. هي اسرع من الي فات بس مش هتطلع نتايج افضل من الي فات زي نفس فكرة الفرق ما بين a*  و dijkstra ان هي مش بتشوف كل الاحتمالات الممكنة 
3.  ولما بتلعب في الparametrs  بتاعتها هتبطئ من سرعتها و هنكون بيها flann  obj 
```python 
# Initiate SIFT detector

sift = cv2.xfeatures2d.SIFT_create()
  
# find the keypoints and descriptors with SIFT

kp1, des1 = sift.detectAndCompute(img1,None)

kp2, des2 = sift.detectAndCompute(img2,None)


# FLANN parameters

FLANN_INDEX_KDTREE = 0

index_params = dict(algorithm = FLANN_INDEX_KDTREE, trees = 5)

search_params = dict(checks=50)  

  

flann = cv2.FlannBasedMatcher(index_params,search_params)

  

matches = flann.knnMatch(des1,des2,k=2)


good = []
  

# ratio test

for i,(match1,match2) in enumerate(matches):

    if match1.distance < 0.7*match2.distance:

        good.append([match1])


flann_matches = cv2.drawMatchesKnn(img1,kp1,img2,kp2,good,None,flags=0)

plt.imshow(flann_matches)

plt.show()
```
![Figure_84](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_84.png)
## 4. extra
1. هتلاقي ناس بتحب تعمل ايه بقي تلون الfeatures  الي مش معمولها matching  بلون تاني عن طريق الماسك و محتاج ارجع ليهم تاني عشان مكسل اكتبهم  