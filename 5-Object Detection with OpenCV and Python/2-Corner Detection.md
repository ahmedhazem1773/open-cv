1.  الكورنر هو مكان التحام 2 edges 
## 1. Harris Corner Detection
1.  هي معتمدة علي مبدأ ان هي تدور علي التغير في الاتجاه
2. نحولهم لgray scale و نحولها لfloat  32  عشان هي مش بتشتغل غير علي النوع ده 
3. هي بتاخد paramters  الاتية
	1. الصورة
	2. دي المسئولة عن بلوك سايز بتاع الجيرة neighborhood (default 2)  الي هو الي حوالين النقطة بتاعت ال corner الي تم تحديدها
	3. مقاس الكيرنل بس بندخل  بعد واحد فقط (كيرنل الي هي sobel)
	4. ده parameter  تبع harris  (default 0.04~0.06) كل ما تزود القيمة الdetect بيكون اقوي بس اقل في عدد الdetections 
4. هتلاقي ان احنا عملنا dilate  عشان نعرف نشوف الكورنر بدل ما يعلم علي بيكسل واحد 
5. هتلاقي ان احنا محددين زي threshold  بحيث نعرف نعلم الكورنر 
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
flat_chess = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\flat_chessboard.png")

flat_chess = cv2.cvtColor(flat_chess,cv2.COLOR_BGR2RGB)

plt.imshow(flat_chess)

plt.show()

real_chess = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\real_chessboard.jpg")

real_chess = cv2.cvtColor(real_chess,cv2.COLOR_BGR2RGB)

plt.imshow(real_chess)

plt.show()

gray_flat_chess = cv2.cvtColor(flat_chess,cv2.COLOR_BGR2GRAY)

plt.imshow(gray_flat_chess,cmap='gray')

plt.show()

gray_real_chess = cv2.cvtColor(real_chess,cv2.COLOR_BGR2GRAY)

plt.imshow(gray_real_chess,cmap='gray')

plt.show()

gray1=np.float32(gray_flat_chess)

res1 = cv2.cornerHarris(gray1 , blockSize=2 , ksize=3 , k=0.04)

res1= cv2.dilate(res1 ,None)

plt.imshow(res1,cmap='gray')

plt.show()

flat_chess[res1 > 0.01* res1.max()] = [255, 0, 0]

plt.imshow(flat_chess)

plt.show()

gray2=np.float32(gray_real_chess)

res2 = cv2.cornerHarris(gray2 , blockSize=2, ksize=5 , k=0.04)

res2= cv2.dilate(res2 ,None)

plt.imshow(res2,cmap='gray')

plt.show()

real_chess[res2 > 0.01* res2.max()] = [255, 0, 0]

plt.imshow(real_chess)

plt.show()
```
![flat_chessboard](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/flat_chessboard.png)
![Figure_67](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_67.png)
![Figure_68](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_68.png)
![Figure_69](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_69.png)
![Figure_70](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_70.png)
![Figure_71](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_71.png)
![Figure_72](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_72.png)


## 2. ShiTomasi Detection
1. هي تعديل للmethode  الي فاتت و بتدي نتايج افضل 
2.  هي بتاخ الاتي
	1. الصورة
	2. عدد الكورنرز الي عايزه يشوفها لو حطيت قيمة سالبة هيشوف كل الكورنرز الي محتملة (ممكن يطلع ايرور ),لو حدد عدد هيرعض افضل كورنز عددهم العدد ده
	3. دي الي بتحدد بيها زي qaulity  الكورنر ان الرقم ده بيضرب في افضل كورنر و بعد كده اي كورنر جودته اقل من جودة دي مش هيتعرض
	4. اقل مسافة ممكنة ما بين اي كورنر 
3. هي بترجع اماكن الكورنرز بس مش بتعلم علي الصورة زي harris كده 
4. احد عيوبها ان هي ممكن تشتغل كويس علي الصور الي كونترست بتاعها قليل و هي حساسة للقيم
5. هتلاحظ ان احنا مستخدمين func  اسمه `ravel` دي بتعمل زي flat للarray بتخليها في بعد واحد 
6. هنحول القيم من float ل int 
7. لو خد بالك ان هي بترجع الابعاد في قوسين مش قوس واحد لما نعمل iteration فا هي ravel بتخليها قوس واحد مش اتنين  زي ما واضح ازاي الاحداثيات محطوطة في قوسين
![Pasted image 20250820204924](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Pasted%20image%2020250820204924.png)
```python
flat_chess = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\flat_chessboard.png")
flat_chess = cv2.cvtColor(flat_chess,cv2.COLOR_BGR2RGB)
real_chess = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\real_chessboard.jpg")
real_chess = cv2.cvtColor(real_chess,cv2.COLOR_BGR2RGB)
gray_flat_chess = cv2.cvtColor(flat_chess,cv2.COLOR_BGR2GRAY)
plt.imshow(gray_flat_chess,cmap='gray')
plt.show()
gray_real_chess = cv2.cvtColor(real_chess,cv2.COLOR_BGR2GRAY)
plt.imshow(gray_real_chess,cmap='gray')
plt.show()
corners1 = cv2.goodFeaturesToTrack(gray_flat_chess,5,0.01,10)
corners1 = np.intp(corners1)
for i in corners1:

    center= np.ravel(i)
    cv2.circle(flat_chess,center,3,(255,0,0),-1)

plt.imshow(flat_chess)
plt.show()
corners2 = cv2.goodFeaturesToTrack(gray_real_chess,100,0.01,10)
corners2 = np.intp(corners2)
for i in corners2:
    center= np.ravel(i)
    cv2.circle(real_chess,center,3,(255,0,0),-1)
plt.imshow(real_chess)
plt.show()
```
![Figure_67 1](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_67%201.png)
![Figure_68 1](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_68%201.png)


