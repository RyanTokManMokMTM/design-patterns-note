## Design Patterns(設計模式)

* **原則(Principe)**
  * 單一職責原則(Single Responsibility Principle)
    + 每個Object值負責一個某塊領域的職責
  * 開放封包原則(Open-Close Principle)
    * 對Object修改封閉(不能修改已經設計好的object),對Object擴展開發
  * 里氏替換原則(Lisiko Substitution Principe)
    * 編程時都定義成基類(父類)，在運行時以子類取代
  * 依賴導致原則(Dependence Inversion Principle)
    * 繼承與Interface/抽象,以抽象/界面編程
  * 接口隔離原則(Interface Segregation Principle,ISP)
    * 把大的interface分離成小的，讓實現的人幹該幹的事，不要多餘不該幹的事
  * 合成复用原則:
    * 以合成的方式取代繼承(因為繼承會將base class 暴露給子類，而且當父類修過也會影響到子類)
    * 以合成的方式(就是使用注入(setter，constructor等方式)，可以減少耦合/依賴，當要使用的類成員要新增功能時，可以直接擴展，並把子類作為該類的成員(以子類取代父類(Lisiko substitution Principe))
  * 迪米特法則(Law of Demeter)
    * 讓object 跟 object 之間的耦合變小，不是朋友的不要聊天
    * 要引用其他Object，使用中間類，讓類與類之間的耦合變小

---

### 原則( Principe)

* 單一職責原則(Single Responsibility Principle)

  * > 定義:
    >
    > 一個class 只負責一個功能領域中的相應的職責。=> 只負責某一塊,只有1個原因會使他改變
    >
    > 這個原則主要是實現高內聚(Object裡面的Code關聯性很強)以及低耦合(和其他Object的關聯性很低)
    >
    > ---
    >
    > ```c++
    > //不符合的例子
    > class StuScoreDataChart {
    >     public:
    >     	void getConnection(); //connection to db
    >     	void findStudents();//quert db
    >     	void createChart(); //create chart bar
    >     	void displayChart(); //display the chart bar
    > }
    > /*
    > 這個Object 一共做了4件事，連接到db,從db中query student表以及 建立和顯示chartBar
    > 這個object 包含了太多responsibility
    > 改變這個object的原因有很多，並非單一1個原因，可能是改變db連接，修改圖片等都會改變到這個object
    > 必須將這個responsibility 分成不同的class,使其有重用的可能以及只做1件事
    > */
    > ```
    >
    > ```c++
    > //符合Single Responsibility Principe
    > class StuScoreDataChart{ //只負責chartBar
    >     private:
    >     	studentQuery sqlQuery; 
    >     public:
    >     	void createChart(); //create chart bar
    >     	void displayChart(); //display the chart bar
    > }
    > 
    > class studentQuery{//只負責db查詢
    >     private:
    >     	DBUtil util; 
    >     public:
    >     	//dbQuerty method
    >     	List findStudents()
    > }
    > 
    > class DBUtil{ //只負責連接
    >     public:
    >     	Connetion getConnetion();
    > }
    > 
    > //Structure
    > //StuScoreDataChart -(Query)-> studentQuery -(connectDB)->DBDBUtil
    > ```
    >
    > 

* 開放封閉原則(Open-Closed Principle) => 只能擴展,不能修改

  * >定義:
    >
    >Software 應該是可以擴展,而不可修改。可以對其進行擴展(Open),但不能對其進行修改(Close)。
    >
    >開放(Open):對擴展開放是指有新需求/變化時,在不修改的情況下,對系統代碼進行擴展,以便適應新環境。
    >
    >​		=>通過inheritance 以及polymorphism的方式對不用Method進行Override固有的行為，實現擴展。
    >
    >封閉(Close):在系統設計完成後,能獨立運行或完成工作的情況下,不要對任何Object進行修改。、
    >
    >​		=>被class依賴的固定的類型(有固定的抽象行為)，不能被修改。
    >
    >---
    >
    >```c++
    >//Example :A Bank Staff(此例子為對 ‘修改’ 開放) ----違反原則
    >//每個staff 都會進行不同的業務
    >//如果新增bank process 就要對switch 做修改
    >//
    >class BusyBankStaff{
    >    //suppose there are some bacis bank process : Class BankProcess()
    >    private:
    >    	BankProcess* bankProc = new BankProcess();
    >    public:
    >    	void HadleProcess(Client client){
    >            switch(client.ClientType){
    >                    case: "Customer Deposit":
    >                    	//Bank process Deposit
    >                    	bankProc->Deposit();
    >                    	break;
    >                    case "Customer Transfer":
    >                    	//Bank process Transfer
    >                    	bankProc->Transfer();
    >                    	break;
    >                    case "Customer Draw":
    >                        //Bank process Transfer
    >                		bankProc->DrawMoney();
    >                    	break;    
    >            }
    >        }
    >}
    >```
    >
    >> 將最有可能的功能新增或者變化的部分分離出來，作為擴展的部分來實現(讓繼承的人來實現)。
    >>
    >> 把這部分變成一個Abstraction class,繼承於他人來實現這部分邏輯。
    >
    >```c++
    >//Abstraction class
    >//c++ using virtual function = 0
    >
    >//Abstraction class => pure virtual class
    >class BankProcess{
    >    public:
    >    	virtual void process() = 0; 
    >}
    >
    >//使用Abstraction class進行擴展
    >//假設有Deposit,Transfer,以及DrawMoney 3個業務
    >
    >//每個process都為單一的
    >class DepositProcess : public BankProcess{
    >    void process() override{
    >        //TODO Deposit Process
    >    }
    >}
    >class TransferProcess : public BankProcess{
    >    void process() override{
    >        //TODO Transfer Process
    >    }
    >}
    >class DrawMoneyProcess : public BankProcess{
    >    void process() override{
    >        //TODO Deposit DrawMoney
    >    }
    >}
    >
    >class EasuBankStaff{
    >    private:
    >    	BankProcess* bankProc = null;
    >    public:
    >    	void HandleProcess(Client* client){
    >            bankProc = client->CreateProcess(); //返回一個處理業務的process
    >            bankProc->process();
    >        }
    >}
    >
    >int main(int argc,char** argv){
    >    EasuBankStaff* bankStaff = new EasuBankStaff();
    >    bankStaff.HandleProcess(new Client("Deposit"));//處理存款業務
    >}
    >
    >```
    >
    >```c++
    >//這裡用戶還是需要加入新的邏輯->另外一個原則處理
    >class Client{
    >    private:
    >    	std::string ClientType;
    >    public:
    >    	Client(std::string clientType){
    >            ClientType = clientType;
    >        }
    >    
    >    	BankProcess* CreateProcess(){
    >            switch(ClientType){
    >                case "Deposit":
    >                    return new DepositProcess();
    >                    break;
    >                    
    >                case "Transfer":
    >                    return new TransferProcess();
    >                    break;
    >                    
    >              	case " DrawMoney":
    >                    return new DrawMoneyProcess();
    >                    break;
    >            }
    >        }
    >}
    >```

* 里氏替換原則(Liskov Substitution Principle)

  * > 定義(是實現Open-Close Principle 的重要方式之一):
    >
    > 所有應用Base Class(父類)的地方必須能透明的使用其子類的Object.(Sub Class => Base Class) 但(Base Class 不是 Sub Class)。透過子類來擴張。
    >
    > 簡單來說就是程式中都盡量用Base Class來進行定義,在運行時候再去確認其Sub Class，使用Sub Calss Object 來進行替換Base Class Object
    >
    > ---
    >
    > #### 使用Liskov Substitution 時候注意的問題
    >
    > * 子類的所以方法必須在父類中declare或者子類必須實現父類中所有declared的方法。
    > * 盡量把父類設計為抽象(abstraction)或者interface，讓子類繼承(inheritance)父類，實現父類declare的方法。在運行時，使用子類的instance 替換成父類instance(更方便的擴展),當擴展功能時無需修改子類代碼，可以通過新增一個新的子類來實現
    >
    > ---
    >
    > ```c++
    > //發郵件的🌰
    > //沒有可以重複利用之代碼
    > //每次新增都必須修改父類
    > class EmailSender{
    >     virtual void sned(Customeer customer){
    >        //SEND EMIAL TO Customeer
    >     } 
    >     //只要是繼承與Customer 的都可以發送(以上原則的定義)
    > }
    > 
    > class EmailSender {
    >     public:
    > 		void sendCommoneCustomer(CommoneCustomer customer);
    >    		void sendVipCustomer(VipCustomer customer);
    > 
    >     	//Adding new customer
    >     	//need to modfiy here~~
    >         void sendStaffCustomer(StaffCustomer customer)
    > }
    > 
    > //All customer method are same~~
    > class CommoneCustomer : public EmailSender{
    >     //TODO
    >         private:
    >     	string name;
    >     	string email;
    >     public:
    >     	string getName(){
    >             //Return customer name
    >         }
    > 		void setName(string name){
    >             //set customer name
    >         }
    > 
    >     	string getEmail(){
    >             //get customer email
    >         }
    > 
    >     	void setEmail(){
    >             //set customer email
    >         }
    > }
    > 
    > class VipCustomer : public EmailSender{
    >     //TODO
    >         private:
    >     	string name;
    >     	string email;
    >     public:
    >     	string getName(){
    >             //Return customer name
    >         }
    > 		void setName(string name){
    >             //set customer name
    >         }
    > 
    >     	string getEmail(){
    >             //get customer email
    >         }
    > 
    >     	void setEmail(){
    >             //set customer email
    >         }
    > }
    > 
    > //new customer type
    > class StaffCustomer : public EmailSender{
    >     //TODO
    >         private:
    >     	string name;
    >     	string email;
    >     public:
    >     	string getName(){
    >             //Return customer name
    >         }
    > 		void setName(string name){
    >             //set customer name
    >         }
    > 
    >     	string getEmail(){
    >             //get customer email
    >         }
    > 
    >     	void setEmail(){
    >             //set customer email
    >         }
    > }
    > 
    > ```
    >
    > 
    >
    > ```c++
    > //發郵件的🌰
    > //使用以上原則進行修改
    > class EmailSender{
    >     virtual void sned(Customeer customer){
    >        //SEND EMIAL TO Customeer
    >     } 
    >     //只要是繼承與Customer 的都可以發送(以上原則的定義)
    > }
    > 
    > class Customer : public EmailSender{
    >     private:
    >     	string name;
    >     	string email;
    >     public:
    >     	string getName(){
    >             //Return customer name
    >         }
    > 		void setName(string name){
    >             //set customer name
    >         }
    > 
    >     	string getEmail(){
    >             //get customer email
    >         }
    > 
    >     	void setEmail(){
    >             //set customer email
    >         }
    > }
    > 
    > //2 type of customer
    > //using send method allow to send to any customer,not only CommoneCustomer or VipCoustomer， even more new type Coustomer
    > class CommoneCustomer : public Customer{
    >     //TODO
    > }
    > 
    > class VipCustomer : public Customer{
    >     //TODO
    > }
    > 
    > //add a new type 
    > //不用修改base class 或者子類的任何代碼就可以實現擴展
    > class StaffCustomer : public Customer{
    >     //TODO
    > }
    > ```
    >
    > **Liskov Substitution Principle主要的意義**(擴展父類,但不能修改父類)
    >
    > > * *Sub Class 可以實現父類的abstract method,但不能override 父類的non-abstract method*
    > > * *Sub Class 可以增加自己特有的method*
    > > * 當Sub Class override 父類的method 時候，對於method 的argument比父類寬鬆(可以加入多個argument)
    > > * 當Sub Class override 父類的method 時候，對於method 的return type 比父類的嚴謹(不能改變return type)
    > >
    > > ---
    > >
    > > ```c++
    > > //不follow Liskov Substitution Principe的🌰
    > > //簡單的計算
    > > class A {
    > >     public:
    > >     	int func1(int a,int b){
    > >             return a - b;
    > >         }
    > > }
    > > 
    > > 
    > > int main(){
    > >     A* a = new A();
    > >     printf("%d",a->func1(100,50)); //100-50 = 50
    > >     printf("%d",a->func1(100,30)); //100-30 = 100-30 = 80
    > >     return;
    > > }
    > > ```
    > >
    > > ```c++
    > > //現在擴充計算功能
    > > //相加後 + 100
    > > //擴展A的功能
    > > 
    > > //不follow Liskov Substitution Principe的🌰
    > > //簡單的計算
    > > class A {
    > >     public:
    > >     	int func1(int a,int b){
    > >             return a - b;
    > >         }
    > > }
    > > 
    > > class B : public A{
    > >     public:
    > >     	int func1(int a, int b){
    > >             return a + b;
    > >         }
    > >     
    > >     	int func2(int a,int b){
    > >             return func1(a,b) + 100;
    > >         }
    > > }
    > > 
    > > 
    > > int main(){
    > >     B* b = new B();
    > >     printf("100 - 50:%d",b->func1(100,50)); //100-50 = 150(系統就錯誤了，本來的-變成+)
    > >     printf("100 - 30:%d",b->func1(100,30)); //100+30 = 130(系統就錯誤了，本來的-變成+)
    > >     printf("100 - 30:%d",b->func2(100,30)); // 230 (只有這個func 是正確的)
    > >     return;
    > > }
    > > ```
    > >
    > > > 以上的例子可見,子類Override了父類導致調用了子類的func1
    > > >
    > > > 使用Liskov Substitution Principe 可以大大減低系統錯誤
  
* 依賴導致原則(Dependence Inversion Principle)

  * >定義(主要是減低class 與 class 之間耦合):
    >
    >Abstraction 不應該依賴與detail,detail 應該依賴與Abstraction。就是說針對Abstraction(抽象)/interface(接口)編程，而不是針對細節(包含細節的Class)編程。
    >
    >也就是面向接口編程(interface oriented programming)
    >
    >---
    >
    >```c++
    >//舉個例子
    >//依賴與細節
    >class Book{
    >public:
    >	string getContent(){
    >       return "這本書要講的故事是有為少年.....";
    >   }
    >}
    >
    >class Mother {
    >public:
    >	void readingContent(Book* book){
    >       printf("%d",book->getConent())
    >   }
    >}
    >
    >int main(){
    >	Mother* mother = new Monther();
    >mother->readingContent(new Book()); //"這本書要講的故事是有為少年.....";
    >	return;
    >}
    >```
    >
    >> 如果現在要讀的不只是書呢？？？
    >
    >> 讀的可能是報紙？雜誌呢？
    >
    >> 那就要必須修改Mother這個class,只有就違反了 Open-Close Principe
    >
    >> **為什麼會出現這個問題呢？**
    >
    >> *英文mother class 是依賴與Book這個class(高耦合)*
    >
    >> 必須修改成依賴於接口/抽象(Interface/Abstract)，而非細節class
    >
    >```c++
    >//面向Abstract
    >class AReader{
    >   public:
    >   	virtual string getContent() = 0 //declared 而非細節，有子類來繼續具體細節
    >}
    >
    >//書
    >class Book ： AReader{
    >   public:
    >   	string getContent(){
    >            return "這本書要講的故事是有為少年.....";
    >       }
    >}
    >
    >//報紙
    >class Newspaper : AReader{
    >   public:
    >   	string getContent(){
    >           return "今天發生了某某某事.....";
    >       }
    >}
    >
    >class Magazine : AReader {
    >   public:
    >   	string getContent(){
    >           return "娛樂八卦誰誰誰跟誰誰黑...";
    >       }
    >}
    >
    >class Mother {
    >   public:
    >   	void reading(AReader*reader){ //Liskvo Substitution Principe:在編程時以子類取代父類
    >           reader->getContent();
    >       }
    >}
    >
    >int main(){
    >   Mother* mother = new Mother();
    >   mother->reading(new Book()); //讀書
    >   mother->reading(new Newspaper());//讀報紙
    >   mother->reading(new Magazine());//讀雜誌
    >   //要新增時,無需修改Mother Class,只需要新增子類即可
    >   //減低了Class 與 Class 之間的耦合
    >   //若要與某Class 有關係,使用依賴注入的方式:結構體(Constructor)，setter以及接口(以上的例子,使用把繼承與接口/抽象的class 已參數傳遞)
    >}
    >```
    >
    >---
    >
    >### 依賴注入（都是定義時使用抽象類，運行時再傳入具體的對象，以子類覆蓋子類）
    >
    >* constructor注入(透過constructor 把有依賴關係的具體對象傳入)
    >* setter 注入(透過setter 方法來傳入有依賴關係的具體對象)
    >* 接口注入:通過在接口中declare 方法來傳入具體對象(以上例子)
  
* 接口隔離原則(Interface Segregation Principle,ISP)

  * > 定義(讓接口僅提供需要的方法，單一原則)
    >
    > 使用多個專門的接口,而不是使用單一的總接口(把所以方法都用同1個interface來定義),客戶端不應該依賴哪些它需要的接口/方法(客戶端僅需要知道要做的事,不應該知道不需要干的活)。
    >
    > ---
    >
    > * **隔離原則**
    >   * 角色隔離原則
    >     * 把不同接口理解成為不同的角色，不同角色有不同功能，一個角色=一個接口
    >   * 定制服務
    >     * 為客戶端定制單獨的接口(僅需提供客戶端需要的行為/方法，不要的隱藏)。為客戶端提供盡量小的接口，而不是大的總接口
    >
    > *使用ISP時，**!注意!** 接口的大小，不能太小，也不能太大，太小會到之接口氾濫(到處都是)，太大就會違背ISP原則(不需要大的總接口),使用起來不方便*
    >
    > ---
    >
    > ```c++
    > //🌰
    > //提供了不需要的方法
    > class CustomerDataDisplay {
    >     public:
    >         virtual string dataRead() = 0;
    >         virtual bool transormToXML() = 0;
    >         virtual void createChart() = 0;
    >         virtual void displayChart() = 0;
    >         virtual void createReport() = 0;
    >      	virtual void displayReport() = 0;
    > }
    > 
    > //都必須實現以上方法
    > //有可能本來就是xml 無需實現，無需的方法也必須實現，實現為1個空的方法()
    > //有可能我只需要顯示和建立圖表,其他的客戶端都沒用等
    > //定義的方法太多，產生大量無用代碼
    > class ConcreteClass : CustomerDataDisplay{
    >     public:
    >          string dataRead() = 0;
    >          bool transormToXML() = 0;
    >          void createChart() = 0;
    >          void displayChart() = 0;
    >          void createReport() = 0;
    >      	 void displayReport() = 0;
    > }
    > ```
    >
    > ```c++
    > //符合ISP
    > //🌰
    > //提供了不需要的方法
    > 
    > //只做Data 處理
    > class DataHandle {
    >     public:
    >         virtual string dataRead() = 0;
    > }
    > 
    > //只做XML Transformer
    > class XMLTransformer {
    >     public:
    >         virtual bool transormToXML() = 0;
    > }
    > 
    > //只做Char 表
    > class CharHandler {
    >     public:
    >         virtual void createChart() = 0;
    >         virtual void displayChart() = 0;
    > }
    > 
    > //值做report 表
    > class ReportHandler {
    >     public:
    >         virtual void createReport() = 0;
    >      	virtual void displayReport() = 0;
    > }
    > 
    > //需要什麼接口直接實現，不會有多餘方面，客戶端不需要的方法等
    > //以下例子ConcreteClass client 只需做data 處理和 chart處理
    > class ConcreteClass : DataHandle,CharHandler{
    >     public:
    >          string dataRead() = 0;
    >          void createChart() = 0;
    >          void displayChart() = 0;
    > }
    > ```
  
* 迪米特法則(Law of Demeter)

  * > 定義(不要跟陌生人講話，只跟自己的朋友之間通訊):
    >
    > 實體與實體之間盡可能少的互相作用(沒有直接關係的盡可能少的直接通訊)
    >
    > ---
    >
    > **朋友的定義:**
    >
    > * *自己(this)*
    >
    > * *已argument傳入的object*
    >
    > * *當前object的member*
    >
    > * *如果當前Object的member是set,set當中的element也算是朋友*
    >
    > * *在當前Object所建立的Object*
    >
    > > **除了朋友關係以為不要之間跟其他人直接進行互動/有互動的作用**
    > >
    > > **如果Object有需要調用到其他非朋友的Object的一些方法，應該通過第三者(通過引入合理的第三者)調用，減低對象與對象之間的耦合**
    >
    > ---
    >
    > **使用*Law of Demeter*注意事項:**
    >
    > * 建立鬆耦合的類(Object 跟 Object 之間依賴性不強),耦合也低，有利於複用
    > * 鬆耦合的類如果被修改了不會對關聯的類有太大的波動/影響
    > * 在設計Object structure時候，每個Object對其member和mehod的訪問權限盡量降低
    > * 盡量把Object 設計成不變的類
    > * 一個類引用其他類盡可能降到最低
    >
    > ---
    >
    > ```c++
    > //使用UI界面來舉🌰
    > /*
    > 一個Button class 會與4個不同class 進行交互/相互作用，
    > 當要新增新的區塊/控件時，便要修改這些交換的控件的源代碼，難以擴展
    > Button -> Label
    > Button -> List
    > Button -> ComboBox
    > Button -> TextBox
    > */
    > ```
    >
    > ```c++
    > //透過中介人/中間類(Mediator)來進行控制控件
    > //減低了類與類的的耦合度
    > //若要新增或者刪除UI控件只要修改Midiator 便可，無需修改其他人/控件的代碼
    > 
    > /*
    > Button -> Mediator
    > */
    > ```
    >
    > 

---

