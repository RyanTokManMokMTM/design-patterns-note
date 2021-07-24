## Design Patterns(設計模式)

---

### 什麼是設計模式

> 設計模式(Design Pattern) 是對軟體設計中普遍存在（反覆出現）的各種問題，所提出的解決方案。
>
> 設計模式是描述在各種不同的情況下，如何解決問題的一種方案。
>
> OOP設計模式通常以類別或者物件來描述關係和相互的作用(不涉及完成應用的特定object)。
>
> 設計模式能使不穩定依賴於相對穩定、具體依賴於相對抽象，避免會引起麻煩的緊耦合。

* **設計模式的類型**
  * 建立型模式
    * 工廠方法模式(Factory Method Pattern)
    * 
  * 結構型模式
  * 行為型模式

---

### 原則( Principe)

* 單一職責原則(Single Responsibility Principle)

  * > 定義:
    >
    > 一個class 只負責一個功能領域中的相應的職責。=> 只負責某一塊,只有1個原因會使他改變

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
    >
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

* 依賴導致原則(Dependence Inversion Principle)

  * >

---



## 工廠模式(Factory Method)

### 簡單工廠模式(Simple Factor)

* > Simple Factor 是一種管理Object Creation的模式,根據參數回傳不同的值。

