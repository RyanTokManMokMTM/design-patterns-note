## Design Patterns(設計模式)

---

* **設計模式的類型**
  * 建立型模式
    * `簡單工廠模式(Simple Factory Pattern) `: 不在23個設計模式內
      * 由一個工廠來生產全部產品:
        * 違反·單一職責原則·
        * 違反·開閉原則·
    * `工廠方法模式(Factory Method Pattern)`
      * 每個產品都又1個具體工廠產生
    * `抽象工廠模式(Abstract Factory Pattern)`
      * 相同產品族(相同約束的產品)都在同一個工廠生產
        * 對於新增產品族符合`開閉原則`,但是對於修改或者刪除`產品等級結構`違反
    * 單例模式(`Sigleton Pattern`)
      * 只提供靜態方法讓外部存取class(`getInstance()`),而且系統中只存在一個Instance
      * `constructor`對外隱藏,外部無法透過new的方法新增Instance
      * 內部建立Instance 有2種方法
        * 餓漢式:`static MyClass* myClass = new MyClass() `,在系統運作時就先new Instance
          * 問題:假設如果沒有使用到該Instance會浪費系統資源
        * 懶漢式:`static MyClass* myClass = nullptr;`,在`client` call `GetInstance()`的時候在function 裡面檢測 `static MyClass* myClass = nullptr` 是否為`nullptr` ,`nullptr`就new Instance，否則return Instance
          * 問題:多線程中，可能是有出錯的情況，多個線程同時new,產生不同的Instance
          * 解決方法1:加入`Mutux`以及使用`double-Check Locking`
          * 解決方法2:在class裡面加入`static class`,只有在call ``getInstance()``才會生成
        * 違反`SPP`，又是工廠又是Product
        * 難擴展，不是抽象/界面
    * 原形模式(Prototype pattern)-自己就是一個工廠(Factory)
      * 通過複製自己來克隆與自己一摸一樣的Class，而且記憶體不一樣
      * 包含
        * 淺克隆(`Shallow-Clone`)(只有value type 才會被複製，其他只會複製記憶體位置)
        * 深克隆(`Deep-Clone`)(所有類型都會被複製)
      * 可以透過`Prototype manager`來進行原形克隆，透過Hash的方式matching 要複製的Class
      * 違反`OCP`,如果要修改`Clone`的方法，必須修改代碼
    * 創建者模式:
      * 透過`Builder`定義具體的建立方法與配置,通過`Director`進行`Product`的創建過程(Call `Builder`的`buildPartX 方法`)，並返回完成後的`Product`
      * `Client`只會與`Director`互動，並且獲取配置好的`Product`
      * 使用`HookMethod`使`Director`更好的控制創建過程
      * 違反:如果簡化了`Director`合併到`Builder`，如果`Consturct(建立)方法`過於複雜，而且需要合拼與建立過多的Components,便會違反`SSP`
      * 不適合產品/創建過程不相似的Product

---

## 創建型模式

> *提供一種建立Object的同時隱藏logic的方式(不是使用直接使用new 直接建立)*

+ **`簡單工廠模式(Simple Factory Pattern)`**-*不在23個設計模式內*

  > 定義:建立一個接口,讓子類自己決定實現哪一個Factory
  >
  > 重點在於*工廠*，透過工廠的Static method 進行Product(繼承於同一Abstract)的Object
  >
  > **違背原則:Open-Close Principe(新增產品必須修改工廠)**
  >
  > **缺點:職責太重**
  >
  > ---
  >
  > ```c++
  > //簡單🌰
  > class Product{
  >  public:
  >  	virtual void someMethod() = 0
  > }
  > 
  > class ProductA : public Product{
  >  public:
  >  	void someMethod(){
  >          //TODO For ProductA
  >      }
  > }
  > 
  > class ProductB : Public Product{
  >  public:
  >  	void someMethod(){
  >          //TODO for ProductB
  >      }
  > }
  > 
  > class Factory{
  >  public:
  >  	static Product* getProduct(string type){
  >          if(type == "A"){
  >              return new ProductA();
  >          }
  >          else if(type == "B"){
  >              return new ProductB();
  >          }
  >          return null;
  >      }
  > }
  > 
  > int main(){
  >  Product* A = Factory::getProduct("A"); //get ProductA instance
  >  Product* B = Factory::getProduct("B"); //get ProductB instance
  > 
  > }
  > ```

+ **`工廠模式模式(Factory Pattern)`**

  > 定義:建立一個接口,讓子類自己決定實現哪一個Factory
  >
  > **重點**:繼承了Simple Factory Patterns的優點,同時解決了它的問題(新增產品需要修改Source)
  >
  > **優點:**
  >
  > * Client無需知道Object的設置的細節，也無需知道實質Object的名稱，只需通過工廠即可
  > * 工廠(Factory)以及產品(Product)都是透過Polymorphism(多態)來實現，也是工廠模式的關鍵，工廠可以自主的決定要生產什麼產品，在內部進行設置產品(封裝在內部)
  > * 加入新的Product時無需修改Abstract class 以及 具體的工廠和產品的類，只要擴展新增即可，符合Open-Close principle
  >
  > **缺點:**
  >
  > * 新增產品時，要加入新的產品類以及工廠類，增加了系統的複雜度，而且需要從新編譯，增加了開銷
  > * 為了擴展性,加入了抽象類，Client都使用Abstract class 進行編程，增加了系統的抽象性和理解難度(都是Abstract class 不知道是什麼~)
  >
  > **這個模式可以讓Object(product)的設置簡化，client無需知道/一直弄複雜的設置，而且透過多態性和使用了里氏替換原則(Lisko Subsititution princeple)，使系統更容易擴展**
  >
  > ---
  > ```mermaid
  > classDiagram
  > 	FileLoggerFactory ..> FileLogger : create
  > 	DatabaseLoggerFactory  ..> DatabaseLogger : create
  > 
  > 	FileLoggerFactory ..|> LoggerFactory
  > 	DatabaseLoggerFactory ..|> LoggerFactory
  > 
  > 	FileLogger ..|> Logger
  > 	DatabaseLogger ..|> Logger
  > 
  > 	class Logger{
  > 		<<abstruct>>
  > 		+void writeLog()
  > 	}
  > 
  > 	class FileLogger{
  > 		+void writeLog()
  > 	}
  > 
  > 	class DatabaseLogger{
  > 		+void writeLog()
  > 	}
  > 
  > 
  > 	class LoggerFactory{
  > 		<<abstruct>>
  > 		+Logger createLogger()
  > 	}
  > 
  > 	class FileLoggerFactory{
  > 		+Logger createLogger()
  > 	}
  > 
  > 	class DatabaseLoggerFactory{
  > 		+Logger createLogger()
  > 	}
  > 	
  > 
  > 	
  > 	Client ..> LoggerFactory
  > 	Client ..> Logger
  > 
  > ```
  >
  > 
  >
  > ```c++
  > //Factory 透過Abstract來繼承/實現
  > //logger 的🌰
  > 
  > //Abstract Logger
  > class Logger{
  >     public:
  >     	virtual void wirteLog() = 0;
  > }
  > 
  > //Implement
  > class DatabaseLogger: public Logger{
  >     pubic:
  >     	void wirteLog(){
  >             //Wirte DB log message
  >         }
  > }
  > 
  > class FileLogger : public Logger{
  >     public:
  >     	void wirteLog(){
  >             //Write File Log message
  >         }
  > }
  > 
  > //Abstruct Factory
  > class Factory{
  >     public:
  >     	virtual Logger* createLogger() = 0;
  > }
  > //不可以是Static靜態，因為子類是動態實現父類的函數(static 沒有this指標)
  > //Implement
  > class FileLoggerFactory : public Factory{
  >     public:
  >     	Logger* createLogger(){
  >             //init file logger 
  >             //TODO init etc
  > 
  >             //create file logger
  >             Logger* logger = new FileLogger();
  >             //TODO Create file etc
  > 
  >             return Logger;
  >         }
  > }
  > 
  > class DabatabaseFactory : public Factory{
  >     public:
  >         Logger* createLogger(){
  >            	//Connect to db
  >             //Create db logger etc...
  >             Logger* logger = new DatabaseLogger();
  >             //init this logger etc...
  >             return logger
  >         }
  > }
  > 
  > //Client to use
  > int main(){
  >     Factory *factory = new FileLoggerFactory();
  >     Logger *logger = factory->createLogger(); //return the Product that it is initialized
  >     Logger->wirteLog();
  >     return;
  > 
  >     //如果想讓系統有更多靈活性和課擴展性，可以透過xml來進行配置，不需透過修過client的代碼
  >     //只需更新xml中的設置，在代碼中新增新的Product 以及新增的Factory 並重新編譯便可
  > 
  >     //除了默認的設置方法外，還想透過傳入String的方法來自定義設計，例如連接要DB，文件的路徑等等
  >     //可以透過Override Abstruct class 的func來進行設置
  >     /*
  >     	class Factory{
  >             public:
  >             	virtual Logger* createLogger() = 0;
  >             	virtual Logger* createLogger(string config) = 0;
  >             	virtual Logger* createLogger(Object obj) = 0;//通過Obj裡面的成員進行設置等
  > 		}
  >     */
  > }
  > 
  > ```

+ **`抽象工廠模式(Abstract Factory Pattern)`**

  + > 定義:
    >
    > 提供一個創建系列相關或者互相依賴的Interface，而無需指定其具體的class
    >
    > Abstract Factory Pattern又稱為Kit模式
    >
    > **優點:**
    >
    > * 解決工廠模式中每個產品都使用一個工廠的生產的問題
    > * 新增產品很方便，擴展產品以及工廠即可(增加產品族)
    > * 工廠模式的優點
    >
    > **缺點:**
    >
    > * 如果要新增產品等級結構必須修過工廠抽象類，違反Open-Close 原則(增加產品等級結構)=>*開閉原則的傾斜性*（抽象工廠模式唯一缺點）
    >   * **何謂開閉原則的傾斜性呢?**
    >     * 就是在新增產品族的時候可以符合開閉原則,但是在新增產品等級結構時候(不同的產品類型/等級)必須修改抽象類(無法解決，只能避免(設計時,全面考慮!))
    >
    > ---
    >
    > **工廠模式跟抽象工廠的區別**
    >
    > * 工廠模式(Factory Pattern)
    >
    >   * **每1個產品對應1個具體工廠用於生成該Product**
    > * 抽象工廠模式(Factory Pattern)
    >   * 每一組相關的Product都由同一個具體工廠生產
    >   * 例如:電器(同牌子/同一產品族)/電器(同電器(如都是冰箱)/同一產品等級結構)
    >     * **產品等級結構: 產品的繼承結構**
    >       * **同一種類的產品(如:同一電器但不同牌子)**
    >     * **產品族:同一工廠生產的，位於不用產品等級結構的一組產品**
    >       * **同一族群的產品(如:同一牌子的不同電器)**
    > ---
    > **使用Abstract Factory Pattern的實際場合**
    >
    > **當一個工廠的可以創立出屬於不同產品的等級結構的一個產品族中的所有對象時,此時使用抽象工廠模式更有效率和簡單**
    >
    > * **系統不應該依賴具體的細節(如何創建,表達細節等...)，依賴與抽象(所有工廠模式都一樣)**
    > * **系統中多於一個產品族時，而每次只會使用到某一個產品族。可通過配置文件動態修改產品族**
    > * **約束:屬於同一產品族的產品將在一起使用，而這些產品可以沒有任何關係，但有相同約束**
    > * **產品等級結構穩定，在設計完成後不會在系統更改/刪除產品等級結構(開閉原則的傾斜性)**
    >
    > ---
    > ``` mermaid
    > classDiagram
    > 	class SkinFactory{
    > 		<<abstruct>>
    > 		+Button createButton()
    > 		+TextFidld createTextField()
    > 		+ComboBox createComboBox()
    > 	}
    > 	
    > 	class SpringSkinFactory{
    > 		+Button createButton()
    > 		+TextFidld createTextField()
    > 		+ComboBox createComboBox()
    > 	}
    > 	
    > 	class SummerSkinFactory{
    > 		+Button createButton()
    > 		+TextFidld createTextField()
    > 		+ComboBox createComboBox()
    > 	}
    > 	
    > 	SpringSkinFactory ..|>  SkinFactory
    > 	SummerSkinFactory ..|>  SkinFactory
    > 	
    > 	class Button{
    > 		<<abstruct>>
    > 		+void display()
    > 	}
    > 	
    > 	class TextField{
    > 		<<abstruct>>
    > 		+void display()
    > 	}
    > 	
    > 	class ComboBox{
    > 		<<abstruct>>
    > 		+void display()
    > 	}
    > 	
    > 	class SpringButton{
    > 		+void display()
    > 	}
    > 	
    > 	class SummerButton{
    > 		+void display()
    > 	}
    > 	
    > 	class SpringTexField{
    > 		+void display()
    > 	}
    > 	
    > 	class SummerTexField{
    > 		+void display()
    > 	}
    > 	
    > 	class SpringComboBox{
    > 		+void display()
    > 	}
    > 	
    > 	class SummerComboBox{
    > 		+void display()
    > 	}
    > 	
    > 	SpringButton ..|> Button
    > 	SummerButton ..|> Button
    > 	
    > 	SpringTexField ..|> TextField
    > 	SummerTexField ..|> TextField
    > 	
    > 	SpringComboBox ..|> ComboBox
    > 	SummerComboBox ..|> ComboBox
    > 	
    > 	SpringSkinFactory  ..> SpringButton : create
    > 	SpringSkinFactory  ..> SpringTexField : create
    > 	SpringSkinFactory  ..> SpringComboBox : create
    > 	
    > 	SummerSkinFactory  ..> SummerButton : create
    > 	SummerSkinFactory  ..> SummerTexField : create
    > 	SummerSkinFactory  ..> SummerComboBox : create
    > 	
    > 	client ..> SkinFactory : use
    > 	client ..> Button : use
    > 	client ..> TextField : use
    > 	client ..> SkinFactory : use
    > 	
    > ```
    >
    > 
    >
    > ```c++
    > //🌰UI
    > //Button TextField ComboBox
    > //Abstract Class UI
    > class Button{
    >    public:
    >     	virtual void display() = 0;
    > }
    > 
    > class TextField{
    >    public:
    >     	virtual void display() = 0;
    > }
    > 
    > class ComboBox{
    >    public:
    >     	virtual void display() = 0;
    > }
    > 
    > //Abstract Class Factory
    > //用於把不同類型的UI/主題Group 在一起
    > class UIFactory {
    >     public:
    >     	virtual Button* createButton() = 0;
    >     	virtual TextField* createTextField() = 0;
    >     	virtual ComboBox* createComboBox() = 0;
    > }
    > ```
    >
    > ```c++
    > //Implenent UI
    > //Spring
    > class SpringButton : public Button{
    >     public:
    >     	void Draw(){
    >             //Draw Spring style Button
    >         }
    > }
    > class SpringTextField : public TextField{
    >     public:
    >     	void Draw(){
    >             //Draw Spring stype TextField
    >         }
    > }
    > class SpringComboBox : public ComboBox{
    >     public:
    >     	void Draw(){
    >             //Draw Spring stype ComboBox
    >         }
    > }
    > 
    > //Summer
    > class SummerButton : public Button{
    >     public:
    >     	void Draw(){
    >             //Draw Summer style Button
    >         }
    > }
    > class SummerTextField : public TextField{
    >     public:
    >     	void Draw(){
    >             //Draw Summer style TextField
    >         }
    > }
    > class SummerComboBox : public ComboBox{
    >     public:
    >     	void Draw(){
    >             //Draw Summer style ComboBox
    >         }
    > }
    > ```
    >
    > ```c++
    > //Implement
    > //Factory
    > class SpringFactory : public UIFactory{
    >     public:
    >     	 Button* createButton(){
    >              //To init the button and some setting
    >              Button* button = new SpringButton();
    >              return button;
    >          };
    >     	 TextField* createTextField(){
    >              //To init the TextField and some setting
    >              TextField* textField = new SpringTextField();
    >              return textField;
    >          };
    >     	 ComboBox* createComboBox(){
    >              //To init the ComboBox and some setting
    >              ComboBox* comboBox = new SpringComboBox();
    > 			return ComboBox;
    >          };
    > }
    > 
    > //SummerFactory
    > class SummerFactory : public UIFactory{
    >     public:
    >        	 Button* createButton(){
    >              //To init the button and some setting
    >              Button* button = new SummerButton();
    >              return button;
    >          };
    >     	 TextField* createTextField(){
    >              //To init the TextField and some setting
    >              TextField* textField = new SummerTextField();
    >              return textField;
    >          };
    >     	 ComboBox* createComboBox(){
    >              //To init the ComboBox and some setting
    >              ComboBox* comboBox = new SummerComboBox();
    > 			return ComboBox;
    >          };
    > }
    > ```
    >
    > ```c++
    > //main
    > int main(){
    >     //就是透過工廠生成接口生成不同的工廠 
    >     //再透過不同的工廠調用其生產的product
    > 	UIFactory* factory = new SummerFactory();
    >     Button* button = factory->createButton();
    >     TextField* textField = factory->createTextField();
    >     ComboBox* comboBox = factory->createComboBox();
    > 
    >     button->draw();  //畫出Summer Style的Button
    >     textField->draw(); //畫出Summer Style的textField
    >     comboBox->draw(); //畫出Summer Style的comboBox
    > 
    >     //最好透過XML文件，修過XML的設置使用不用Style的UI
    >     return;
    > }
    > ```
    >

+ 單例模式(`Singleton Pattern`)

  + > 定義(就是一個類只有1個Instance)：
    >
    > 確保一個Class只有一個Instance,整個系統只提個這個Instance.
    >
    > *確保唯一性,能節省系統資源*
    >
    > ---
    >
    > **優點:**
    >
    > * 提供了唯一性的Instance訪問(系統中只提供一個Instance訪問)
    > * System只存在一個Instance,可以節省系統資源
    > * 在單例模式中，可以提供指定數目的實例(數個Instance)，能節省系統資源，同時也能解決單例對象分享過多,損性能的問題
    >
    > **缺點:**
    >
    > * 擴展十分困難(單例模式不是由Abstract Class實現的)
    > * 有點違反了Single-responsibility Principe，因為他又是工廠的角色(new Instance)，又是Product的角色(可以透過Static返回的Instance 做操作)
    >
    > ---
    > ```mermaid
    > classDiagram
    > 	class LoadBalancer{
    > 		-LoadBalancer instance
    > 		-List serverList
    > 		-LoadBalancer()
    > 		+LoadBalancer getiadBalancer()
    > 		+void addServer()
    > 		+void removeServer()
    > 		+string getServer()
    > 	}
    > 	LoadBalancer o-- LoadBalancer : Shared
    > ```
    >
    > 
    >
    > ```c++
    > //Task Manager 🌰
    > //Constructor 對外隱藏，並提供一個靜態Static 讓外部只透過這個存取
    > class TaskManager{
    > private:
    > 	TaskManager();
    > 	void displayProcess();
    > 	void displayServices();
    > 	static TaskManager* taskManager = nullptr; //保存唯一Instance
    > public:
    > 	//外部只能透過靜態Static 方法存取 並new一個TaskManger的Instance 如果是null的
    > 	//此方法為工廠
    > 	static TaskManager* getInstance(){
    > if(taskManager == nullptr){
    >     taskManager = new TaskManager();
    > }
    > return TaskManager;
    > }
    > //主要Static 方法只能存取Static 變數，其他在實例化前是不存在的
    > }
    > ```
    >
    > ```c++
    > //LoadBalance 🌰
    > //用於計算服務器負債，所以必須使用單例
    > LoadBalance::getInstance()
    > class LoadBalance{
    > public:
    > 	static LoadBalance* getInstance(){
    > if(loadBalance == nullptr){
    > 
    >     loadBalance = new LoadBalance();
    > }
    > return LoadBalance;
    > }
    > private:
    > 	static LoadBalance* loadBalance;
    > 	LinkedList* serverList = nullptr;
    > 
    > 	LoadBalance();
    > 	void addServer(std::string server){
    > serverList->add(server);//add a server
    > }
    > 
    > 	std::string getServer(){
    > int i = random()% serverList->length(); //random get server
    > return serverList->get(i);
    > }
    > 
    > }
    > ```
    >
    > ```c++
    > //client
    > int main(){
    > LoadBalance *balancer1,*balancer2,*balancer3,*balancer4;
    > balancer1 = LoadBalance::getInstance(); //獲取LoadBalance，這裡會先new 再return
    > balancer2 = LoadBalance::getInstance(); //獲取LoadBalance
    > balancer3 = LoadBalance::getInstance(); //獲取LoadBalance
    > balancer4 = LoadBalance::getInstance(); //獲取LoadBalance
    > 
    > //以上4個balancer 都應該為同一記憶體
    > if(balancer1 == balancer2 && balancer2 == balancer3 && balancer3 == balancer4)
    > printf("Balancer is Unquie0");
    > 
    > //add server
    > balancer1->addServer("server1");
    > balancer1->addServer("server2");
    > balancer1->addServer("server3");
    > balancer1->addServer("server4");
    > 
    > for(int i = 0;i<10;i++){
    > printf("server %d",balancer1->getServer());
    > }
    > return;
    > }
    > ```
    >
    > ---
    >
    > **Singleton Pattern的問題(如何initialize Instance呢？)**
    >
    > * **Multi-Threading 同時Call getInstance()**
    >   * 如果Thread A 跟 Thread B同時Instance 為null 的時候 call getInstance()，會同時進入if-condition，並都會進行new，也就違反了Singleton Pattern的目的，可能導致系統錯誤。
    >
    > * **2種解決方法**
    >   * *餓漢式Singleton*
    >
    >     * 就是在定義Static variable的時候就new Instance，所以在class 加載時候就已經被實例化了
    >     * 問題:無論系統是否使用這個,它都會實例化
    >
    >   * *懶漢式Singleton(Lazy load)*
    >
    >     * 就是在call ``getInstance()``時候，先檢測是否存在，不存在(nullptr)的時候，先new Instance再return Instance
    >
    >       ```c++
    >       static MyClass* getInstance(){
    >           if(myClass == nullptr) myClass = new MyClass();
    >           return myClass;
    >       }
    >       ```
    >
    >     * 問題1:在多線程(Multi-threading)的時候，會有線程安全(需要用到mutex保護)
    >
    >       ```c++
    >       //suppse mutex is a keywork in c++ for 
    >       mutux static MyClass* getInstance(){
    >           if(myClass == nullptr) myClass = new MyClass();
    >           return myClass;
    >       }
    >       //這個問題就是每次call getInstance 都會鎖，影響性能
    >       ```
    >
    >       ```c++
    >       //suppse mutex is a keywork in c++ for 
    >       static MyClass* getInstance(){
    >           if(myClass == nullptr){
    >               mutex funcPtr(){
    >                   myClass = new MyClass();
    >               }
    >           }
    >           return myClass;
    >       }
    >       //這個解決了上面的問題
    >       //但是 還是會出問題
    >       //因為線程還是會同時進去if condition
    >       //所以要在一個if 再進行判斷
    >       //稱為double-Check Locking
    >       ```
    >
    >       ```c++
    >       //double-Check Locking
    >       mutux static MyClass* getInstance(){
    >           if(myClass == nullptr){
    >               mutex funcPtr(){
    >                   if(myClass == nullptr){
    >                        myClass = new MyClass();
    >                   }
    >               }
    >           }
    >           return myClass;
    >       }
    >       //檢查2次
    >       ```
    >
    >  * 使用內部Static class 來進行實現
    >
    >     * 能克服``餓漢式``跟``懶漢式``
    >
    >       ```c++
    >       class Singleton {
    >           private:
    >       		 Singleton() {
    >       			}
    >       	static class HolderClass {
    >                   private:
    >               		static Singleton* instance = new Singleton();
    >       	}
    >       	public:
    >           	static Singleton getInstance() {
    >       	    	return HolderClass::instance
    >       		}
    >       	}
    >                               
    >       //就是靜態單例沒有作為Singleton的成員 所以加載時候不會被實例化
    >       //getInstance()被呼叫時候才會呼叫HolderClass 並實例化
    >       ```
    >

+ 原型模式(Prototype Pattern)

  + > 定義:
    >
    > 使用Prototype Instance指定建立的Object的類型，通過複製自己創建新的Instance
    >
    > (建立或者複製一個跟自己一樣的Instance，重點在於如果實現``Clone``方法)
    >
    > ----
    >
    > **優點:**
    >
    > * 可以簡化建立對象的過程，直接複製已有的Instance(模板)，提個創建的效率
    > * 擴展性好，Prototype 提供了抽象類，Client可對抽象類編程，對系統運作沒有任何影響(新增或減少)
    > * 無需工廠(自己就是工廠)，透過clone 方法進行建立的過程(複製自己來創建)
    > * 可以使用`Deep Copy`備份Prototype的state
    >
    > **缺點:**
    >
    > * 每個Prototype class 都必須具有Clone 方法，而且要修改Clone 必須修改source，違背`SPP`
    > * `Deep Copy`實現起來比較複雜，如果Object有多個Object 嵌套(embedded),都需要對每個Class進行`Deep Copy`，可能實現麻煩
    >
    > ---
    >
    > **違背原則:**
    >
    > * 如果修改Prototype 的Clone 方法會違背``OCP``
    >
    > ---
    > ```mermaid
    > classDiagram
    > 	class Object{
    > 		<<abstruct>>
    > 		+Object Clone()
    > 	}
    > 
    > 	class Log{
    > 		-string name
    > 		-string data
    > 		-string content
    > 		+void setName(string name)
    > 		+void setData(string date)
    > 		+void setContent(string content)
    > 		+string getName()
    > 		+string getData()
    > 		+strint getContnent()
    > 		+Object Clone()
    > 	}
    > 
    > 	Log --|> Object
    > 	Client ..> Log
    > ```
    >
    > 
    >
    > ```c++
    > //簡單的Prototype的🌰
    > //Prototype 可以是抽象(Abstruct)/Interface(接口)/甚至可以是實例(Instance)
    > class Prototype{
    >     public:
    >     	virtual Prototype* clone() = 0
    > }
    > 
    > class PrototypeA : public Prototype{
    >     private:
    >     	int attr;
    >     public:
    >     	int getNum(){
    >             return this->attr;
    >         }
    >     	void setAttr(int num){
    >             //TODO set attr
    >             this->attr = num;
    >         }
    >     	 Prototype* clone(){
    >           	//To clone self Object with new
    >              Prototype* cloneObj = new PrototypeA();
    >              cloneObj->setAttr(this->attr);
    >              return cloneObj;
    >          }
    > 
    > }
    > 
    > class PrototypeB : public Prototype{
    >     private:
    >     	int attr;
    > 
    >     public:
    >         int getNum(){
    >             return this->attr;
    >         }
    >     	void setAttr(int num){
    >             //TODO set attr
    >             this->attr = num;
    >         }
    >     	 Prototype* clone(){
    >           	//To clone current Object with new
    >              Prototype* cloneObj = new PrototypeB();
    >              cloneObj->setAttr(this->attr);
    >              return cloneObj
    >          }
    > }
    > 
    > int main(){
    >     //先new 一個Instance ，再透過call clone method 進行Clone
    > 
    >     //Clone A and store in CloneObjA
    >     //複製A的Prototype(模板)
    >     Prototype* objA = new PrototypeA();
    >     Prototype* CloneObjA = onjA->Clone();
    > 
    > 
    >     //Clone B and store in CloneObjB
    >     //複製B的Prototype(模板)
    >     Prototype* objB = new PrototypeB();
    >     Prototype* CloneObjB = onjB->Clone();
    >     return;
    > }
    > ```
    >
    > > **注意:Clone 不應該是相同的記憶體位置，``return this/return self``**
    >
    > ---
    >
    > **複製Prototype 的問題**
    >
    > * **2個複製的concept**
    >
    > * *淺克隆(Shallow Clone)*:只有``value``類型會被複製
    >
    >   * >clone的Prototype的value``的類型，``value``類型會複製一份給Clone Object
    >     >
    >     >clone的Prototype的``reference``類型，``reference``類型會複製自己的記憶體位置給Clone Object,也就是2個Object的``reference``類型都會pointer to 同一個記憶體位置.（值沒有被複製）
    >
    > * 深克隆(Deep Clone):所以成員都會被複製
    >
    >   * >clone的Prototype的``value``的類型，``value`` 類型會複製一份給Clone Object
    >     >
    >     >clone的Prototype的``reference``類型，也會複製一份clone object.(記憶體是不一樣)
    >     >
    >     >所以成員都會被複製
    >
    > ```c++
    > //ShallowClone and DeepClone in c++
    > //如果要複製pointer 就得使用DeepClone
    > //在c++中default使用ShallowClone
    > ```
    >
    > ---
    >
    > **透過Prototype Manager(原形管理器)來複製不同的Prototype**
    >
    > > Prototype Manager = 負責建立Prototype的Factory
    > >
    > > 透過PM裡面的Hash來獲取要複製的Prototype
    > ```mermaid
    > classDiagram
    > 	class Prototype{
    > 		<<absstruct>>
    > 		+Prototype clone()
    > 	}
    > 	
    > 	class ConcretePrototypeA{
    > 		+Prototype Clone()
    > 	}
    > 	
    > 	class ConcretePrototypeB{
    > 		+Prototype Clone()
    > 	}
    > 	
    > 	ConcretePrototypeA --|> Prototype
    > 	ConcretePrototypeB --|> Prototype
    > 	
    > 	class PrototypeManager{
    > 		-Map PrototypeMap
    > 		+void add(string,Prototype)
    > 		+Prototype get(string)
    > 	}
    > 	
    > 	client ..> PrototypeManager : create
    > 		client ..> Prototype
    > ```
    >
    > ```mermaid
    > classDiagram
    > 	class Document{
    > 		<<abstruct>>
    > 		+Document clone()
    > 		+void display()
    > 	}
    > 	
    > 	class FAR {
    > 		+Document clone()
    > 		+void display()
    > 	}
    > 	
    > 	class SRS{
    > 		+Document clone()
    > 		+void display()
    > 	}
    > 	
    > 	class DRS{
    > 		+Document clone()
    > 		+void display()
    > 	}
    > 	
    > 	FAR ..|> Document
    > 	SRS ..|> Document
    > 	DRS ..|> Document
    > 	
    > 	class PrototypeManager{
    > 		-prototypeMap Map 
    > 		-PrototypeManager static 
    > 		+PrototypeManager()
    > 		+AddDoc(string,Document) void 
    > 		+getDoc() Document 
    > 		+getPrototypeManager() static PrototypeManager
    > 	}
    > 	
    > 	client ..> PrototypeManager
    > 	client ..> Document
    > ```
    >
    > 
    >
    > ```c++
    > //生成不同文件模板的栗子
    > class OfficialDocument{
    >     public:
    >     	virtual OfficialDocument* clone() = 0;
    >         virtual void display() = 0;
    > }
    > 
    > //Feasibility Analysis Report
    > class FAR : OfficialDocument{
    >     public:
    >     	OfficialDocument* clone(){
    >             //ShallowCopy 淺複製
    >             //省略代碼...
    >         }
    >     	void display(){
    >             printf("FAR document");
    >         }
    > }
    > 
    > //Software Requirements Specification
    > class SRS  : OfficialDocument{
    >     public:
    >     	OfficialDocument* clone(){
    >             //ShallowCopy 淺複製
    >             //省略代碼...
    >         }
    >     	void display(){
    >             printf("FAR document");
    >         }
    > }
    > 
    > //Factory-using Singleton pattern 使用餓漢式
    > class PrototypeManager{
    >     private:
    >     	map<std::string,OfficialDocument*> hashMap;
    >     	static PrototypeManager* pm = new PrototypeManager();
    >     	PrototypeManager(){
    >             hashMap["far"] = new OfficialDocument();
    >             hashMap["srs"] = new OfficialDocument();
    >         }
    > 
    >     public:
    >     	void addOfficeDoc(std::string,OfficialDocument* doc){
    >             //add pair to hashMap and instance
    >         }
    > 
    >         OfficialDocument* getOfficeDoc(std::string key){
    >             //get OfficialDocument clone from map for the key
    >             //return the clone
    >         }
    > 
    >     	static PrototypeManager* getInstance(){
    >             return pm;
    >         }
    > }
    > 
    > int main(){
    >     PrototypeManager* pm = PrototypeManager::getInstance();
    >     //get Feasibility Analysis Report clone
    >     OfficialDocument doc1 = pm->getOfficeDoc("far");
    >     OfficialDocument doc2 = pm->getOfficeDoc("far");
    > 
    >     OfficialDocument doc3 = pm->getOfficeDoc("src");
    >     OfficialDocument doc4 = pm->getOfficeDoc("src");
    > 
    >     //透過key value patterns 獲取要clone的Instance
    > 
    >     return;
    > }
    > ```


* 創造者模式(Builder pattern)


  * > 定義(多個不同的components 組成複雜的Product):
    >
    > 將一個複製的Object的建構與它的表示(組件Component)分離，使得同樣的建構過程(Component的建構)可以創建不同的表示(同一個Component可以與不用的Components組合)
    >
    > > **與抽象工廠(Absturct)的區別**
    > >
    > > * 抽象工廠:就是生產某一個部件的工廠，而且是放回一些了相關的Product
    > > * 創建者模式:通過具體的Builder去指導Director去生成複雜的對象
    >
    > ---
    >
    > **優點**
    >
    > * Client無需知道建立/組成產品的細節，讓Client在相同的過程下(Builder不一樣)就可以建立出不用的Product
    > * 每一個具體的`Builder`都是互相獨立的，如要新增`Builder`的具體class，直進行擴展。而且`Director`是對`Absbstruct class`進行編程,所以無需修改代碼，符合`Open-Close Principe`,且系統擴展也方便
    > * 可以增加程序(`HookMethod`)來更精細得控制創建過程
    >
    > **缺點**
    >
    > * 每個產品的建立都有相似的部分，在這種情況下才適合用`Builder Patterns`,如那種很多Component都不一樣的，不適合使用，所以，使用受到限制
    > * 如果某個Product需要多個Builder來實現複雜的變化，會導致吸引變得龐大，會增加系統的理解難點和運行成本
    >
    > ---
    >
    > *概念*:
    >
    > > Builder:為抽象(Abstuct)/界面(Interface),提供創建各部分的Method 以及return的Product
    > >
    > > ConcreteBuilder:Implement Builder ，提供各個Component的建立方法以及裝備方法
    > >
    > > Product :包含多個Components的複雜Object
    > >
    > > Director : 負責安排Builder組裝的順序，Client會之間與Director 進行互動，都過setter等注入方式配置複雜Object的屬性
    >
    > ---
    > ```c++
    > //簡單的例子
    > //Product class
    > class Product{
    >     private:
    >     	//組成的Component
    >     	std::string partA;
    >     	std::string partB;
    >     	std::string partC;
    >     public:
    >     //各個Component的設置以及獲取方法
    >     //PartA,B,C的setter跟getter
    > }
    > ```
    >
    > ```c++
    > //Builder 的Abstract ：builder將會實現什麼方法
    > class Builder{
    >     protected Product* product = new Product(); //新建Product,透過setter getter 去改變
    >     pubilc:
    >     	virtual void buildPartA() = 0;
    >     	virtual void buildPartB() = 0;
    >     	virtual void buildPartC() = 0;
    > 
    >     	//工廠方法
    >     	Product *getResult(){
    >             return product
    >         }
    > }
    > ```
    >
    > ```c++
    > //Director :建立Product的順序，以及分離Client和Creation
    > class Director{
    >     private:
    >     	Builder* builder; //用什麼builder 去build Product
    >     public:
    >     	Director(Builder* builder){
    >             this->builder = builder
    >         }
    > 
    >     	//修改Product的建立的builder
    >     	void setBuilder(Builder* builder){
    >             this->builder = builder
    >         }
    > 
    >     	Product* productBuild(){
    >             //如何建立Product
    >             //假設順序A B C
    >             builder->buildPartA();
    >             builder->buildPartB();
    >             builder->buildPartC();
    >             return builder->getResult(); //完成後返回
    >         }
    > }
    > 
    > ```
    >
    > ```c++
    > //Client
    > int main(){
    >     Builder* builder = new ConcreteBuilder(); //使用此builder 建立Product
    >     Director* director = new Director(builder);
    >     Product* product = director->construct(); //返回 建立好的Product
    >     //...
    >     return
    > }
    > ```
    >
    > ---
    >
    > ```mermaid
    > classDiagram
    > 	ActorBuilder <|-- HeroBuilder
    > 	ActorBuilder <|-- AngelBuilder
    > 	ActorBuilder <|-- DevilBuilder
    > 	ActorBuilder *-- Actor
    > 	ActorController ..> ActorBuilder
    > 
    > 
    > 	class Actor{
    >         -type : stirng
    >         -sex : string
    >         -face : string
    >         -constume : string
    >         -hairstyle : string
    >         +void setType(string type)
    >         +void setSex(string type)
    >         +void setFace(string type) 
    >         +void setCostume(string type)
    >         +void setHairStyle(string type) 
    >         +string getType()
    >         +string getSex()
    >         +string getFace()
    >         +string getCostume()
    >         +string getHairStyle()
    > 	}
    > 
    > 	class ActorBuilder{
    > 		#actor:Actor
    > 		+void buildType()
    > 		+void buildSex()
    > 		+void buildFace()
    > 		+void buildCostume()
    > 		+void buildHairStyle()
    > 		+Actor createActor()
    > 	}
    > 
    > 	class HeroBuilder{
    > 		#actor:Actor
    > 		+void buildType()
    > 		+void buildSex()
    > 		+void buildFace()
    > 		+void buildCostume()
    > 		+void buildHairStyle()
    > 		+Actor createActor()
    > 	}
    > 
    > 	class AngelBuilder{
    > 		+void buildType()
    > 		+void buildSex()
    > 		+void buildFace()
    > 		+void buildCostume()
    > 		+void buildHairStyle()
    > 	}
    > 
    > 	class DevilBuilder{
    > 		+void buildType()
    > 		+void buildSex()
    > 		+void buildFace()
    > 		+void buildCostume()
    > 		+void buildHairStyle()
    > 	}
    > 
    > 	class ActorController{
    > 		+Actor construct(ActorBuilder)
    > 	}
    > ```
    > ```c++
    > //以上Class Diagram的實際例子
    > //Actor(Product)
    > class Actor{
    >     private:
    > 		std::string type;
    >     	std::string sex; 
    >     	std::string face
    >     	std::string costume;
    >     	std::string hairStyle;
    >     public:
    >     	void setType(std::string type){
    >             //set the type
    >         }
    > 
    >     	void setSex(std::string sex){
    >             //set the sex
    >         }
    > 
    >     	void setFace(std::string face){
    >             //set the face
    >         }
    > 
    >     	void setCostume(std::strig costume){
    >             //set the custume
    >         }
    > 
    >     	void setHairStyle(std::string HairStyle){
    >             //set the hairStyle
    >         }
    > 
    >     	std::string getType(){
    >             //return the type
    >         }
    > 
    >         std::string getSex(){
    >             //return the type
    >         }
    > 
    >         std::string getFace(){
    >             //return the type
    >         }
    > 
    >         std::string getCostume(){
    >             //return the type
    >         }
    > 
    >         std::string getHairStyle(){
    >             //return the type
    >         }
    > }
    > ```
    >
    > ```c++
    > //ActorBuilder：Builder(Abstruct)
    > class ActorBuilder{
    >     protected:
    >     	Actor* actor = new Actor();
    >     pubilc:
    >     	virtual void buildType() = 0;
    >     	virtual void buildSex() = 0;
    >         virtual void buildFace() = 0;
    >         virtual void buildCostume() = 0;
    >     	virtual void buildHairStyle() = 0;
    >         Actor* createActor(){
    >             return actor;
    >         };
    > }
    > ```
    >
    > ```c++
    > //Implement Builder
    > //Hero
    > class HeroActor{
    >     pubilc:
    >     	void buildType(){
    >             this->actor->setType("Hero");
    >         };
    >     	void buildSex(){
    >             this->actor->setSex("Male");
    >         };
    >        	void buildFace(){
    >             this->actor->setFace("Handsome");
    >         };
    >         void buildCostume(){
    >             this->actor->setCostume("Armor");
    >         };
    >     	void buildHairStyle(){
    >             this->actor->setHairStyle("Elegant");
    >         };
    > }
    > 
    > //Angle
    > class AngleActor{
    >     pubilc:
    >     	void buildType(){
    >             this->actor->setType("Angle");
    >         };
    >     	void buildSex(){
    >             this->actor->setSex("Female");
    >         };
    >        	void buildFace(){
    >              this->actor->setFace("Pretty");
    >         };
    >         void buildCostume(){
    >             this->actor->setCostume("White Skirt");
    >         };
    >     	void buildHairStyle(){
    >             this->actor->setHairStyle("Long Hair");
    >         };
    > }
    > 
    > //Devil
    > class DevilActor{
    >     pubilc:
    >     	void buildType(){
    >             this->actor->setType("Devil");
    >         };
    >     	void buildSex(){
    >             this->actor->setSex("Demon");
    >         };
    >        	void buildFace(){
    >             this->actor->setFace("gly");
    >         };
    >         void buildCostume(){
    >              this->actor->setCostume("Black Clothes");
    >         };
    >     	void buildHairStyle(){
    >             this->actor->setHairStyle("Bald");
    >         };
    > }
    > ```
    >
    > ```c++
    > //ActorContoller:Director
    > class ActorController{
    > 	public:
    >     	Actor* construct(ActorBuilder ab){
    >             //Actor的建構過程
    >             Actor* actor;
    >             ab->buildType();
    >             ab->buildSex();
    >             ab->buildFace();
    >             ab->buildCostume();
    >             ab->buildHairStyle();
    >             actor = ab->createActor();
    >             retur actor; //返回建構完成的Actor
    >         }
    > }
    > ```
    >
    > ```c++
    > //Client
    > int main(){
    >     //生成hero actor
    >     ActorBuilder* heroBuilder = new HeroBuilder();
    >     ActorController* acController = new ActorController();
    >     Actor* ac = acController(heroBuilder);
    > 
    >     //生成Angel actor
    >     ActorBuilder* angelBuilder = new angelBuilder();
    >     ActorController* acController2 = new ActorController();
    >     Actor* ac = acController2(angelBuilder);
    > 
    >     //生成Devil actor
    >     ActorBuilder* devilBuilder = new DevilBuilder();
    >     ActorController* acController3 = new ActorController();
    >     Actor* ac = acController3(devilBuilder);
    > }
    > ```
    >
    > > **可以簡化以上例子(如何建構的過程不複雜，可以把Director的``construct``寫到builder``static method``)**
    > >
    > > ```c++
    > > class ActorBuilder{
    > >  protected:
    > >  	static Actor* actor = new Actor();
    > >  pubilc:
    > >  	virtual void buildType() = 0;
    > >  	virtual void buildSex() = 0;
    > >      virtual void buildFace() = 0;
    > >      virtual void buildCostume() = 0;
    > >  	virtual void buildHairStyle() = 0;
    > >      Actor* createActor(){
    > >          return actor;
    > >      };
    > > 
    > >  	static Actor* consturct(ActorBuilder* ab){
    > >          ab->buildType();
    > >          ab->buildSex();
    > >          ab->buildFace();
    > >          ab->buildCostume();
    > >          ab->buildHairStyle();
    > >          return actor;
    > >      }
    > > 
    > >  	//或者是不需要參數,直接調用Builder method,更加簡化
    > >     	static Actor* consturct(){
    > >          this->buildType();
    > >          this->buildSex();
    > >          this->buildFace();
    > >          this->buildCostume();
    > >          this->buildHairStyle();
    > >          return actor;
    > >      }
    > > }
    > > 
    > > //如果透過以上方法簡化Director，簡化了系統的結構，但加重了Builder 的職責
    > > //只能是創建過程十分簡單的product/建立簡單的產品，否則過於複雜就違反了 SPP 單一職責
    > > ```
    > >
    > > > 可以在Builder 加入`HookMethod(鉤子)`讓`Director`更加精細的控制是否調用`buildPartX`方法，`HookMethod` 通常是`bool function()`,在`Absturct`提供默認實現。
    > >
    > > ```c++
    > > //假設檢查是否Bareheaded actor
    > > class ActorBuilder{
    > >  protected:
    > >  	static Actor* actor = new Actor();
    > >  pubilc:
    > >  	virtual void buildType() = 0;
    > >  	virtual void buildSex() = 0;
    > >      virtual void buildFace() = 0;
    > >      virtual void buildCostume() = 0;
    > >  	virtual void buildHairStyle() = 0;
    > >      Actor* createActor(){
    > >          return actor;
    > >      };
    > > 	
    > >     bool isBareheaded(){return false;}
    > > }
    > > //假設實現的ActorBuilder 是 Bareheaded 就Override return true
    > > ```
    > >
    > > ```c++
    > > //Director class :ActorController
    > > class ActorController{
    > > 	public:
    > >     	Actor* construct(ActorBuilder ab){
    > >             //Actor的建構過程
    > >             Actor* actor;
    > >             ab->buildType();
    > >             ab->buildSex();
    > >             ab->buildFace();
    > >             ab->buildCostume();
    > >             //先檢查是否Bareheaded 進行控制是否要進行buildHair
    > >             if(!ab->isBareheaded()){
    > >                 ab->buildHairStyle();
    > >             }
    > >             actor = ab->createActor();
    > >             retur actor; //返回建構完成的Actor
    > >         }
    > > }
    > > ```
    > >
    > > > `HookMethod`使`Director`有更好得控制是否需要執行`buildPartX`方法

---
