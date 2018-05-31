---
title: java基礎
date: 2018-05-31 09:34:08
tags: java
---
## java 基礎總結  
 
1. ####面向對象的特征有哪些？
   * 封裝 隱藏一切可隱藏的東西，只向外提供簡單的編程接口
   * 繼承
   * 多態：方法重載和重寫
   * 抽象：只關注有哪些行為和屬性，不關注細節 

2. ####public，private，protected以及default的區別？ 
  
    * 橫坐標：當前類>>同包>>子類>>其它包  
    
	* 縱坐標：public>>protected>>default>>private
3. ####java的數據類型
	* 基本類型 
		* int,double,byte,boolean,long,float,short,char
	* 引用類型
		* eg:String
	* 枚舉類型（java 5以後）  
	 
4. ####java的自動拆箱與裝箱機制
   
 	其實就是基本數據類型與引用類型的自動轉換  

5. ####IntegerCache的範圍  
 
	-128~127  

6. ####&和&&的區別
	 
 	&是按位于；&&是邏輯與；按位與的作用通常作用是對某些位清零或保留某些位。  
	

7. ####stack,heap和meathod area 的用法
	
	######首先明確堆和棧存放的東西是什麼。  

	* 棧：基本數據類型的變量，一個對象的引用，函數調用的現場保存
	* 堆：new 或構造器創建的對象，堆是垃圾收集器管理的主要區域
	* 方法區：方法區和堆都是各個線程共享的內存區域。用於存儲已經被jvm加載的類信息，常量，靜態變量登數據。字面量如"hello",100等是放在常量池中，jdk1.7以前常量池是方法區的一部分，1.7以後常量池在堆中。  
	
	######他們的特點是什麼？  

	  棧後進先出，空間小，操作起來速度快，大量的對象存在堆空間，他們的大小可通過jvm的啟動參數來調整，棧空間用光了，會引發StackOverflowError，而堆和常量池空間不足會引發OutofMemoryError 

8. ####java四捨五入的原理是什麼？ 

	加0.5然後向下取整。  

9. ####switch可以作用在哪些類型的變量上？  
   
	java5以前：byte,short,char,int.
	java5:引入枚舉類型
	java7:引入String類型  
  
10. ####==和equal的區別 
  
	==看看左右是不是一個東西，equal是看左右是否長的一樣。
    若x,y滿足x.equals(y)==true,則他們的hash code 一定相同，反過來不成立。  
	有關==和equals的更詳細總結，可參考[這裡](www.baidu.com)  

11. ####java到底是值傳遞還是引用傳遞？  
  
	值傳遞。首先明確值傳遞和引用傳遞的概念：  

	* 值傳遞：表示方法接收的是調用者提供的值
	* 引用傳遞：表示方法接收的是調用者提供的變量地址  
	  
	**方法得到是所有參數值得一個拷貝，方法不能修改任何傳遞給它的參數變量的內容**  
  
	因此，在java中，  
	* 一個方法不能修改一個基本數據類型的參數（針對參數為基本數據類型）
	* 一個方法不能讓對象參數引用一個新對象（針對參數為引用類型）    
	 
	下面的方法解釋為什麼java中不存在引用傳遞。   
       
	<pre>
	Employee a = new Employee("tom",20);  

	Employee b = new Employee("jack",19);  
 
	public static void swap(Employee x,Employee y){    
	 	Employee temp=x;  
	 	x=y;  
		y=temp;  
	}  	 
	</pre>  
	如果是引用傳遞，swap可以實現交換兩個對象值得效果，然而最終結果是swap并沒有改變存儲在a和b中的對象引用，x和y被初始化位兩個對象引用的拷貝。  
  
12. ####String、StringBuilder、StringBuffer的區別  
   
	String只讀。其餘兩種可對字符串進行修改，StringBuilder（1.5引入）和StringBuffer完全相同，區別在於StringBuilder線程不安全，效率比StringBuffer高。
  
13. ####重載和重寫的區別
	重載發生在同一個類中，重寫發生在子類與父類之間，前者是編譯時多態性，（編譯時就知道要調用哪一個方法），後者是運行時多態性。
14. ####JVM加載class文件的原理機制
	
	類的裝載有類加載器（ClassLoader)和它的子類實現。過程如下:  
	
	**加載**：把.class文件中的數據讀到內存中  
  
	&nbsp;&nbsp;|  
	&nbsp;&nbsp;|    

	**連接**：（驗證、準備、解析）為靜態變量分配內存，并設置默認的初始值，將符號引用替換為直接引用。  

	&nbsp;&nbsp;|  
	&nbsp;&nbsp;|  
	  
	**初始化**：
	* 如果類存在直接的父類且這個父類還未初始化，則先初始化父類
	* 如果類中存在初始化語句則依次執行  
	 
15. ####如何實現對象的克隆
	此處列舉深度克隆的方式。
	<pre>
	public static <T extends Serializable> T clone(T obj) throws Exception(){
	  //將對象寫入流中
	  ByteOutputStream bout = new ByteArrayOutputStream();
	  ObjectOutputStream oos = new ObjectOutputStram(bout);
	  oos.write(obj);  
	  //從流中讀取對象
	  ByteInputStream bin = new ByteArrayInputStream(bout.toByteArray());
	  ObjectInputStream ois = new ObjectInputStream(bin);
	  return (T)ois.readObject();  
	}
	</pre>
	參考 [實現對象克隆的兩種方式](https://www.cnblogs.com/Qian123/p/5710533.html)
  
16. ####常見的異常分類
	關於Exception更多詳細信息點擊[這裡](http://www.baidu.com)  
  
17. ####闡述ArrayList、Vector、LinkedList的存儲性能和特性
	* ArrayList和Vector都是使用數組的方式來存儲數據，查詢速度快，插入數據慢。  

	* Vector的方法有synchronized 修飾，性能上比ArrayList差。  
	
	* LinkedList使用雙向鏈錶實現存儲。插入速度快。
	  
	* Vector,Hashtable,Dictionary,BitSet,Stack,Properties是遺留容器，不推薦使用。  
	  
	* ArrayList和LinkedList都是非線程安全的，可用Collections中synchronizedList方法變為線程安全的
	  
18. ####Colledtion和Collections的區別
	Collection是Set,List登容器的父接口；Collections是一個工具類，提供靜態方法來輔助容器操作。
  
19. ####TreeMap和TreeSet在排序時如何比較元素？Collections工具類中的sort()方法如何比較元素？
	簡單記：  
	* 實現Comparable接口，重寫compareTo方法  
	  
	* 採用Collections中sort方法，如下 

		<pre>  
		//obj為要排序的對象，第二個參數為自定義的排序規則
		Collections.sort(obj,new Comparator<Object>(){  
			public int compare(Object o1,Object o2){  
				return o1.hashCode()-o2.hashCode();
			}
		}  
		</pre>	
  
20. ####Thread類的sleep()方法和對象的wait()方法都可以讓線程暫停執行，它們有什麼區別？
	它們的共同點是調用此方法會讓當前線程暫停執行制定的時間。
	* sleep方法是Thread類的靜態方法，但對象鎖依然保持。
	* wait()方法是Object類的方法，調用后會放棄當前對象的鎖，進入對象的等待池，只有調用對象的nodify()或notifYAll()才能喚醒等待池中的線程進入等鎖池，如果線程重新獲得對象的鎖就可以進入就緒狀態。
  
21. ####sleep方法和yield方法有什麼區別？
  
	* sleep給其它線程運行時會不考慮線程優先級，會給底優先級線程以運行的機會，yield只能把機會給相同優先級或更高優先級的線程
	* 線程執行sleep后進入阻塞狀態，執行yield方法后進入就緒狀態
	* sleep方法需要拋出InterruptedException
	* sleep方法比yield（跟操作系統的cpu有關）具有更好的移植性  
	    
22. ####請說出與線程同步以及線程調度相關的方法
	* wait():使一個線程處於阻塞狀態，並且釋放對象所持有的鎖；
	* sleep():讓線程進入睡眠狀態
	* notify():喚醒一個處於等待狀態的線程，但不能確切的喚醒某一個等待的線程，而是有JVM確定
	* notifyAll():喚醒所有處於等待狀態的線程，該方法並不是將對象的鎖給所有線程，而是讓他們去競爭，只有獲得鎖的線程才能進入就緒狀態。
  
23. ####編寫多線程有幾種實現方法
	java5以前有兩種：
	* 繼承Thread類
	* 實現Runnable接口
	兩種方式都需要通過重寫run()方法來定義線程的行為，後者更加靈活
  	java5以後還可以通過：實現Callable和ExecutorService等來創建（待補充）。
	
24. ####什麼是線程池？
	“池化技術”產生的原因：盡可能的減少對象創建和銷毀的次數，從而減少資源的消耗。如線程池，數據庫連接池。
	線程池就是事先創建若干個可執行的線程放入一個容器中，需要的時候從池中獲取而不需要自行創建，使用完放回池中。
	線程工具類Executors提供一些靜態工廠方法，生成一些常用的線程池。
	* newSingleThreadExecutor：單線程線程池，該線程池保證所有任務的執行順序按照任務的提交順序執行。一個不行了另一個接著上。
	* newFixedThreadPool:創建固定大小的線程池，線程池大小一旦達到最大值就保持不變。
	* newCachedThreadPool:可緩存的線程池。當線程池大小超過處理任務所需線程數，則回收空閒線程，若不夠用，則新建線程來補充	，最終線程池大小取決於jvm能夠創建的最大線程數
	* newScheduledThreadPool:創建一個大小無線的線程池。該線程池支持定時以及週期性執行任務的需求。     

25. ####線程的基本狀態以及狀態之間的關係  
	![](https://coding.net/u/sukianata/p/ImgRepository/git/raw/master/java/Thread.png)	
	說明：  
	<pre>
		 Blocked in Object's Wait Pool  對象等待池  
		 Blocked in Object's Lock Pool	等鎖池  
	 	 Runnable                       就緒狀態  
		 Running                        運行狀態
		 Blocked                        阻塞狀態
	</pre>  
26. ####synchronized和java.util.concurrent.locks.Lock的異同  
	Lock能完成synchronized所實現的所有功能。主要不同點：Lock有比synchronized更精確的線程語義和更好的性能，而且不強制性的要求一定要獲得鎖。synchronized會自動釋放鎖，而Lock一定要求程序員手工釋放。
  
27. java中有幾種類型的流
	字節流和字符流。字節流繼承于InputStream,OutputStream,字符流繼承與Reader,Writer。
	編程實現文件拷貝，此處列出NIO的形式。
	<pre>
	public static void fileCopyNIO(String soure,String target) throws IOException{
	  try(FileInputStram in =new FileInputStream(source)){
		try(FileOutputStream out = new FileOutputStream(target)){
			FileChannel inChannel = in.getChannel();
			FileChannel outChannel =out.getChannel();
			ByteBuffer buffer = ByteBuffer.allocate(4096);
			while(inChannel.read(buffer) !=-1){
				buffer.flip();
				outChannel.write(buffer);	
				buffer.clear();
			}
		 }
	  }
	}
	</pre>
	此處用到了java7的新特性try...with..resource格式。
