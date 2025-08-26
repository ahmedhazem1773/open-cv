عارفين ان list و array ان البفرق ما بينهم ان array ان ele بتاعتها كلها من نفس datatype لكن list  ان ele بتاعتها عادي تكون مختلفة لكن هي بتشاركها عادي indexing و slicing
## 1. np.array()
هي func  بتاخد list or tuple جواها و بتحولها لarray
```python 
#example
mylist = [1 ,2 ,3]
newarray = np.array(mylist)
print(newarray , type(nwearray))
```
	Output : [1 2 3 ] <class 'numpy.ndarray'> 
	Note1 : nd refer to --> n-dimentinal 
	Note2 : if i create list that holds differnt datatypes , the code will run normally like above code 
## 2. np.arange()
1. هي func بتعمل array من من الرقم start الي هتباصيه لحد ما قبل رقم stop  و تقدر كمان تحدد step بتاعت الارقام كام 
2. formula is : new array= np.arange(start , stop , step default =0) 
3. هتلاحظ ان هي شبه range() buildin func
4. ان هي بتعمل ارقام int
5. بعرف عدد العناصر ان اطرح البداية من النهاية
```python
#example
newarray4 = np.arange(4,12)
print(newarray4)
#will change the step
newarray5 = np.arange(4,12 ,2)
print(newarray5)
```
	Output:
	[ 4  5  6  7  8  9 10 11]
	[ 4  6  8 10]
## 3. np.zeros() & np.ones()
1. دول مسئولين عن عمل مصفوفة كل عناصرها يا 1 او 0 
2. بتاخد parameters اول parameter ده اسمه shape  بتابصي فيه tuple of integers و عدد ele  الي فيها دي بتحدد dimentions بتاعتها
3. formula is : np.zeros((int , int , ...))
4. المصفوفة بتبقي من flout numbers 
```python 
#example
newarray6 = np.zeros((4,2))
#4=rows , 2=colomn]d
print(newarray6)
print("-------------------")
newarray7= np.zeros((2,2,2))
print(newarray7)
```
	Output:
	[[0. 0.]
	 [0. 0.]
	 [0. 0.]
	 [0. 0.]]
	-------------------
	[[[0. 0.]
	  [0. 0.]]
	
	 [[0. 0.]
	  [0. 0.]]]
## 4.np.random.randint() 
1. دي بتخليك تعمل array بارقام عشوائية  ما بين رقمين تحددهم انت و عدد ele الي عايزها انا 
2. formula is : np.random.randint( start , end , num of ele)
3. تقدر كمان زي randint الي عادية من مكتبة random انك تتحكم في seed بحيث تثبت الارقام العشوائية الي هتظهرلك عن طريق و طبعا بتتكتب هي الاول قبل randint زي المكتبة العادية
4. formula is : np.random.seed(int)
```python
#example 
newarray8 = np.random.randint(0,100 , 10)
print(newarray8)
np.random.seed(101)
newarray9 = np.random.randint(0,100 , 10)
print(newarray9)
```
	output of first run :
	[99 11 93  8 37 43  6 30  8 74]
	[95 11 81 70 63 87 75  9 77 40]
	output of second run :
	[90  3 67 33 48 27 63  6 28 53]
	[95 11 81 70 63 87 75  9 77 40]
## 5. .max() , .min() & .mean()
1. دي methodes بنطبقها علي numpyarray obj vari(variable) بتخلينا نعرف اكبر رقم موجود في array بتاعنا و اصغر رقم و متوسط بتاع الارقام الي في array
2. نقدر كمان نعرف index بتاعت elements دي عن طريق methodes زي 
   .argmax() & .argmin()
3. في كمان func  تجيبلك max , min  و الاماكن بتاعتهم علي الترتيب الاتي  و بتشتغل علي الابعاد الي زي 2d 
	1. min value
	2. max value
	3. min loc
	4. max loc
```python
newarray9 = np.random.randint(0,100 , 10)
print(cv2.minMaxLoc(newarray9))
print(newarray9)
print(newarray9.max())
print(newarray9.argmax())
print(newarray9.min())
print(newarray9.argmin())
print(newarray9.mean())
```
	Output:
	[95 11 81 70 63 87 75  9 77 40]
	(9.0, 95.0, (0, 7), (0, 0))
	95
	0
	9
	7
	60.8
## 6. .shape & .reshape()
1. دي methods ليها علاقة مع dimensions بتاعت array
2. الاولي بتخليك تعرف ابعاد الArray و عدد بتاع كل بعد و خلي بالك هي مش بنكتب فيها اقواس
3. التانية بتخليك بتعمل إعادة تشكيل للarray دي و بتاخد الابعاد الجديدة في شكل tuple المهم الي هحدده ميعيديش عدد ele  الي معايا او يقل عنه عشان هيدي ايرور و ده طبيعي هيملي الخانات الجديدة منين او lost data هيويدها فين ببساطة ممكن نعرف ده عن طريق نخلي ضرب الابعاد في بضعها يساوي نفس عدد العناصر $d1 x d2 ... =num of ele$ 
4. ممكن استخدم reshape مع arange عشان احدد ابعاد بتاعت الarray دي 
5. اعرف عدد الابعاد بتاعتها عن طريق `.ndim`
```python
#example
newarray10 = np.random.randint(0,100,10)
print(newarray10)
print(newarray10.reshape((2,5)))
#i show u that dosen't affect on the vari itself
print(newarray10.shape)
#now i affect on it
newarray10=newarray10.reshape(2,5)
print(newarray10)
newarray11 = np.arange(0,20).reshape(4,5)
print(newarray11)
print(newarray11.ndim)
```
	Output:
	[ 4 63 40 60 92 64  5 12 93 40]
	[[ 4 63 40 60 92]
	 [64  5 12 93 40]]
	(10,)
	[[ 4 63 40 60 92]
	 [64  5 12 93 40]]
	[[ 0  1  2  3  4]
	 [ 5  6  7  8  9]
	 [10 11 12 13 14]
	 [15 16 17 18 19]]
	 2
## 7. indexing & slicing 
1. زي ما احنا عارفين عشا نوصل لعنصر معين في array nd  كنا بنعمل `newarray [d1][d2]..... [dn]`  ونقدر نفس النظام نعمل كده في slicing ان كل بعد نعمله لوحده `newarray [1:3][:2]`
2. بدل ما نكتبهم كل بعد لوحده ممكن نكتبهم كلهم في brackets واحدة و نفصل ما بينهم بالفصلة `newarray [1:3,:2]`
3. تقدر كمان تسخمدم `.copy()` بتاعت الlist عادي
4. تقدر كمان من slicing تدي قيمة للعناصر الي واقعة في الفترة دي 
