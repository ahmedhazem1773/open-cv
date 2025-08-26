1. هي بتستخدم في ىان تظبط كاميرا مثلا علي  عربية بتتحرك بحيث تعرف تتبع فا انت بتلزق عليها زي  pattern  سواء qr code  مثلا او اي حاجة شبه رقعة الشطرنج و هكذا
2. خلي بالك هي مش بتشوف الحاجة الي علي حافة الصورة  زي الي حصل في corner detection 
3. هم اتنين func بنستخدمهم هنا و هنا هنتعامل مع الحاجات الي زي رقعة الشطرنج الاولي بتدور علي الكورنرز في الشكل ده و انت بتباصي فيها الصورة و مقاس بتاع الرقعة يعني زي 7x7 عشان هي مش بتعرف تقرا الحواف و عارفين الشطرنج 8x8 و بترجع اماكنهم بس و هل vari  هي لقت pattern  ولا لا
4. الfunc التانية بترسملك النقط دي الي لقتها علي الرقعة بتاعتك هنباصي فيها الصورة و بعد كده مقاس الpattern بعد كده  اماكن الكورنرز  و بعد كده vari بتاع هي لقت ولا لا  و خلي بالك مقاس الpattern الي احدده اهم حاجة ميزدش عن عدد الcorners 
```python
img2=cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\flat_chessboard.png")

found, corners = cv2.findChessboardCorners(img2,(7,7))
flat_chess_copy = img2.copy()
cv2.drawChessboardCorners(flat_chess_copy, (7, 7), corners, found)
plt.imshow(flat_chess_copy)
plt.show()
```
![Figure_76](Figure_76.png)
5. دلوقتي هنتكلم عن pattern عبارة عن نقط او كور هي هي نفس الي فات بس الفرق في الfunc الاولي بتاخد ايه نفس الparameters بس زيادة واحد ان انت تحدد الmethode الي هيمشي بيها
```python
dots = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\dot_grid.png")

found, corners = cv2.findCirclesGrid(dots, (10,10), cv2.CALIB_CB_SYMMETRIC_GRID)

dbg_image_circles = dots.copy()

cv2.drawChessboardCorners(dbg_image_circles, (10, 10), corners, found)

plt.imshow(dbg_image_circles)

plt.show()
```
![Figure_77](Figure_77.png)