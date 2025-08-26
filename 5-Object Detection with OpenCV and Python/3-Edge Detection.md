1. هنتلكم عن اشهر خوارزميات في الحوار canny edge detection و هي من النوع الي فيها عدد من مراحل في عملية الprocess  بتعمل بلور الاول عشان الnoise  بعد كده sobel  بتجيب القيمة و الاتجاه و باستخدام الاتجاه تخفف سمك  الedge  الي ملهاش لازمة بعد كده مبدأ hysteresis thresholding 
2. هنستخدم جاوس فلتر عشان نشيل الnoise 
3. هتلاقيها بتاخد 2 threshold هي بعد ما تستخدم sobel  بتعمل فلتر للedges الضعيفة الي ملهاش لازمة بحيث بتعمل ايه اي قيمة فوق upper threshold  ده strong edge  و ده معانا اي حاجة تحته مش تبعنا بس لما لاحظنا ان هو في بعض الedges هي ضعيفة نوعا ما بس تبعنا و من الحاجات المهمة فا  هنا يجي دور lower threshold  ان اي حاجة تحته مش محسوبة edge لينا لكن اي قيمة ما بين upper&lower هي دي فيها احتمالية ان هي تكون مهمة لينا فا هي بنحسبها هي معانا لو كانت متصلة مع نقط اعلي من  upper , ترتيب الparameters  الصورة بعد كدخ الlower  بعد كده الupper
4. ممكن نضيف معادلة حسابية بتديك نقط threshold  كويسة تبدأ منها و انا بعد كده بالحس ازود ولا اقلل ان انت تجيب القيمة الي  تعتبر في النص ما بين القيم 
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Pasted image 20250820214135.png]]
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
img = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\sammy_face.jpg")
# blured_img = cv2.blur(img,ksize=(5,5))
blured_img= cv2.GaussianBlur(img, ksize=(5,5),sigmaX=8)
med_val = np.median(img)
# Upper bound is either 255 or 30% above the median value, whichever is lower
upper = int(min(255,1.3 * med_val))
# Lower bound is either 0 or 70% of the median value, whicever is higher
lower = int(max(0, 0.7* med_val))
edges = cv2.Canny(image=blured_img, threshold1=lower , threshold2=upper+56)
#by triying 70 , 120
plt.imshow(edges )
plt.show()
```
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_75.png]]
