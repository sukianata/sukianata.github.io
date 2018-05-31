---
title: javaweb
date: 2018-05-31 15:21:42
tags:
---
*這部分主要涉及在做java web開發過程中涉及到的知識。*
###java web
1. ###XML文檔定義和解析有幾種形式？它們之間有何本質區別
	約束有DTD和Schema兩種形式，Schema本身也是xml文件，約束能力更強。  
	解析xml主要有DOM，SAX,StAx(java6引入)。  
	其中DOM在處理大型文件時性能下降厲害，因為DOM樹結構佔用的內存較多，解析前必須將整個文檔裝入內存，適合對xml的隨機訪問。  
	SAX是事件驅動型解析方式，順序讀取，遇見開頭、結束或開頭標籤結束標籤時會觸發一個事件，用戶通過回調代碼來處理xml文件。
	    
2. ####簡述jdbc操作數據庫的步驟  
  2.1 加載驅動  
  
	```java  
	Class.forName("oracle.jdbc.driver.OracleDriver");
	```  
  2.2 創建連接  
  
	```Connection con =　DriverManager.getConnection("jdbc:oracle:thin@localhost:1521:orcl","root","root");```   
  2.3 創建語句
	<pre>
	 PreparedStatement ps =con.prePareStatement("select * from table_Name")
	//設置參數的方式：ps.setInt(index,value);index從1開始
	</pre>   
  2.4 執行語句  

	```ResultSet rs =ps.executeQuery();```  
  2.5 處理結果  
	<pre>
		while(rs.next()){
		  //do something here 
		  System.out.println(rs.getString("name"));
		}
	</pre>
  2.6 關閉資源  

	提示：關閉外部資源的順序和打開的順序相反。  
  
3. ####Statement和PrepareStatement區別。
	* 後者為預編譯，可增加安全性，且可帶參數。
	* 當批量處理SQL或頻繁執行相同的查詢時，後者有明顯性能上的優勢，因為數據庫可將編譯優化后的SQL語句緩存起來。
	* 調用存儲過程通過CallableStatement接口  
	
4. ####使用JDBC時，如何提升讀取和更新數據的性能？  
	* 讀取：rs.setFetchSize();指定每次抓取的記錄數。
	* 更新：使用PrepareStatement構建批量處理。

5. ####數據庫連接池的作用
	減少性能開銷。
	創建鏈接需要進行TCP的三次握手，釋放鏈接四次。
	有關TCP握手，點擊[這裡(待補充)](http://www.baidu.com)  
6. ####事物的ACID指什麼？  
	* Atiomic:原子性。事物中每個操作，要麼全做要麼全不做。  
	* Consistent:一致性。事物結束后系統狀態是一致的。
	* Isolated:隔離性。并發執行的事物無法看見對方的中間狀態。
	* Durable:持久性。事物完成后所做的改動都會被持久化。  
	
	補充幾個概念：
	* 髒讀：A事物讀取B事物尚未提交的數據，若B進行回滾，則A讀到的數據就是臟數據。
	* 不可重複讀：事物A重新讀取前面讀過的數據發現該數據已經被修改過了。
	* 幻讀：事物A重新執行一個查詢，發現其中插入了被B提交的事物。
	* 第1類丟失更新：事物A撤銷時，把已經提交事物B更新的數據覆蓋了。
	* 第2類丟失更新：事物A覆蓋事物B已經提交的數據，造成事物B所做的操作丟失。
	  
	數據庫通過鎖機制來解決數據並發訪問問題，且提供自動鎖機制，只需要用戶指定會話的事物隔離級別。
	  
7. ####JDBC能否處理Blod和Clob 
	能。補充：Blob為二進制大對象，Clob指大字符對象。  
8. ####獲得一個類的類對象有哪些方式
	* String.class
	* "hello".getClass()
	* Class.forName()
9. ####如何通過反射創建對象
	* 通過類對象調用newInstance(),eg: ```String.class.getNewInstance();```
	*  通過類對象的getConstructor()或getDeclaredContructor()方法獲得構造器對象并調用其newInstance()方法創建對象，eg: ```String.class.getConstructor(String.class).getNewInstance("hello");```
	  
10. 簡述面向對象的六原則一法則
	* 單一職責原則：一個類只做它該做的事，不涉及其它無關領域。
	* 開閉原則：類應該對擴展開放，對修改關係。例如增加新功能時，只需要從原來系統派生出新類，而不需要修改原有代碼
	* 依賴倒轉原則：面向接口編程。
	* 里氏替換原則：簡單的說，就是能用父類型的地方就一定能使用子類型。
	* 接口隔離原則：接口要小而專，不能大而全。
	* 合成聚合原則： 優先使用聚合或合成關係複用代碼，例如工具類可以拿來直接用，不需要繼承。
	* 迪米特法則：又稱最少知識原則，一個對象應當對其它對象有可能少的了解。

11. ####簡述你了解的的設計模式。
	有關於23種設計模式，點擊[這裡(待補充)](http://www.baidu.com)
	挑常用的兩三個記住並可以寫出demo來。
12. ####列舉一些java中常用的排序算法
	有關於常用的排序算法，點擊[這裡(待補充)](http://www.baidu.com)
	挑常用的兩個記住並能寫出來。例如冒泡排序。
13. ####列舉一些java中常用的查找算法  
	有關於常用的查找算法，點擊[這裡(待補充)](http://www.baidu.com)
	挑常用的兩個記住並能寫出來。例如二分查找。 
14. ####實現會話跟蹤的技術有哪些 
	* url重寫：將唯一的繪畫ID添加到url結尾
	* 設置隱藏表單域：將繪話相關的字段添加到隱式表單域中，這些信息提交表單會提交給服務器。
	* cookie:
		* 基於瀏覽器窗口，關閉后沒有了
		* 生成臨時文件
	  注意事項：不要存放敏感信息，不能存放過多內容
	*HttpSession:最強大，功能最多，當一個用戶第一次訪問某網站時會自動創建HttpSession,放在服務器內存中
15. ####Hibernate的一級緩存、二級緩存和查詢緩存
	* 一級緩存功能默認有效。當程序保存、修改持久化實體時，Session不會立即把改變提交到數據庫，而是緩存到當前的session中，除非顯示調用session的flush方法，或關閉session.通過一級緩存可減少程序與數據庫的交互。
	* SessionFactory的二級緩存是全局性的，所有session共享這個二級緩存，默認關閉。開啟時需要指定使用二級緩存的實體類，和二級緩存實現類，sessionFactory就會緩存訪問過該實體類的每個對象，直到緩存數據超出緩存空間。
	* 上面兩級緩存都是對整個實體進行緩存，查詢緩存可緩存普通屬性。將HQL或SQL語句及他們的查詢結果作為鍵值對進行緩存，對於同樣的查詢可以直接從緩存中獲取數據，默認關閉。

16. ####MyBatis中使用#和$書寫佔位符的區別
	'#'將傳入的參數當成字符串，會自動加上引號；  
     $將參數直接顯示在生成的sql中，不安全，盡量不用
	寫order by 時應該用$ 