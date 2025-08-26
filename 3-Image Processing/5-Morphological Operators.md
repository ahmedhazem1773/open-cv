1.  هي عبارة عن مجموعات جاهزة من الkernels  متنوعة زي الي منها تقلل الnoise و في منها حاجات معينة بتبقي مفيدة تشيل النقط السودة الي تشيب منطقة لونها ابيض و العكس بردوا 
2. بتخليك تعمل حاجات زي Erosion & dilation الحاجات بتبقي واضحة اكتر في الصور الي فيها نصوص هي بتبقي زي بتشيل حواف او بتزود في عن طريق قدر معين من المساحة
3. هي بفصل الخلفية عن المقدمة و بتبقي شغال بكفاءة لما تكون الخلفية هيالاغمق و المقدمة الافتح 
4. في الانواع دي و هناخد بعض منها و ده لينك الموقع [Morphology](https://homepages.inf.ed.ac.uk/rbf/HIPR2/morops.htm)
![Pasted image 20250818170759](open%20cv/3-Image%20Processing/images%20&%20videos/Pasted%20image%2020250818170759.png)
## 1. Erosion
1.   هي بتشيل من الحواف و بتحدد عدد مرات iteration  العملية دي تتم و بتباصي طبعا الkernel  بتاعك
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
erosion1 = cv2.erode(blank_img,kernel,iterations = 4)
plt.imshow(erosion1 , cmap="gray")
plt.show()
```
![Figure_29](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_29.png)
![Figure_34](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_34.png)

## 2. dilation
1.  هو بيعمل العكس تمام بيزود حواف للصورة لكن بياخد نفس الparamters بتاعت erosion
```python
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
dil1=cv2.dilate(blank_img, kernel, iterations=2)
plt.imshow(dil1, cmap='gray')
plt.show()
```
![Figure_44](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_44.png)
2. ممكن حاجة زي دي استفاد منها زي مثلا شايف ان الحروف لازقة في بعض ممكن اعمل erosion لحد ما الوصلة ما الي ما بينها وبعد كده اعمل diltion  تاني حاجة زي كده 
```python
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
erosion1 = cv2.erode(blank_img,kernel,iterations = 4)
dil1=cv2.dilate(erosion1, kernel, iterations=3)
plt.imshow(dil1, cmap='gray')
plt.show()
```
![Figure_42](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_42.png)
## 3. Opening
1.  هي بتبقي عملية ان انت بتعملي dilation الاول و بعد كده erosion و dilation  ده عكس erosion  و بتفيد انك تشيل الnoise  الي في الخلفية 
2. هنعمل الاول noise  عبارة مصفوفة عشوائية من الصفر و الواحد و بعد كده هنجمعها علي الصورة 
3. بعد كده هنستخدم  func بتاعت الmorphing  و جواها هنباصي الصورة بتاعتك و بعد كده نوع الmorphing  الي هو  opening و الkernel 
```python
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
white_noise_on_black = np.random.randint(low=0,high=2,size=(600,600))*255
plt.imshow(white_noise_on_black , cmap='gray')
plt.show()
noised_img= (white_noise_on_black + blank_img)
plt.imshow(noised_img , cmap='gray')
plt.show()
opening = cv2.morphologyEx(noised_img, cv2.MORPH_OPEN, kernel)
plt.imshow(opening, cmap='gray')
plt.show()
```
![Figure_35](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_35.png)
![Figure_36](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_36.png)
![Figure_37](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_37.png)
## 4. Closing
1. بتشتغل ان هي تشيل الnoise  من ال forground 
2. احنا هنعمل نفس الي عملنها فوق لما نكون الnoise  بس هنضرب في -255  بحيث بعد كده في الصورة لما اجمع عليها الnoise  اي حاجة قيمتها -255 احولها لصفر بحيث كده الnoise  مأثر فقط علي المقدمة
```python
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
white_noise_on_black = np.random.randint(low=0,high=2,size=(600,600))*-255
plt.imshow(white_noise_on_black , cmap='gray')
plt.show()
noised_img= (white_noise_on_black + blank_img)
noised_img[noised_img==-255]=0
plt.imshow(noised_img , cmap='gray')
plt.show()
closing= cv2.morphologyEx(noised_img, cv2.MORPH_CLOSE, kernel)
plt.imshow(closing, cmap='gray')
plt.show()
```
![Figure_38](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_38.png)
![Figure_39](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_39.png)
## 5. Morphological Gradient
1. هي بتاخد الفرق ما بين Erosion & dilation  بس بتاع الصورة  و تعتبر احدي الطرق الي مستخدمة في  detection 
```python
blank_img =np.zeros((600,600))
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='ABCDE',org=(50,300), fontFace=font,fontScale= 5,color=(255,255,255),thickness=25,lineType=cv2.LINE_AA)
plt.imshow(blank_img , cmap="gray")
plt.show()
kernel = np.ones((5,5),np.uint8)
gradient= cv2.morphologyEx(blank_img, cv2.MORPH_GRADIENT, kernel)
plt.imshow(gradient, cmap='gray')
plt.show()
```
![Figure_41](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_41.png)
