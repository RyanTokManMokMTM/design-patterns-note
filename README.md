## Design Patterns(設計模式)

> 設計模式(Design Pattern) 是對軟體設計中普遍存在（反覆出現）的各種問題，所提出的解決方案。
>
> 設計模式是描述在各種不同的情況下，如何解決問題的一種方案。
>
> OOP設計模式通常以類別或者物件來描述關係和相互的作用(不涉及完成應用的特定object)。
>
> 設計模式能使不穩定依賴於相對穩定、具體依賴於相對抽象，避免會引起麻煩的緊耦合。

---

| 原則(Principe) | 定義(Def) |
| :------------: | :------: |
|單一職責原則(Single Responsibility Principle) | 一個類只負責一個功能領域中的相應的原則|
|開閉原則(Open-Closed Principle) | 對擴展開發(可以繼承擴展)，對修改關閉(不能修改BaseClass)|
|里氏替換原則(Liskov Substitution Principe) | 所有引用於Base Class 的地方 都能夠透明的使用其子類的對象(以子類來替換父類)|
|依賴倒轉原則(Dependencie Inversion Principe) |細節應依賴於抽象(Abstract),抽象(Abstract)不應該依賴與細節。(也就是細節由子類來實現，父類負責定義)|
|接口隔離原則(Interface Segregation Principle) | 使用多個專用的接口(Interface),而不是使用單一的總接口.(能避免Client實現一些沒必要的功能) |
|合成複用原則(Composition Reuse Principle) | 盡量使用Object Composition(在要使用的Object的地方把用到的Object注入),而不是繼承能達到目的.(減少依賴)|
|迪米特法則(Law of Demeter) | 一個軟件實體應盡可能地與其他實體發生互相作用.(就是不用跟不是朋友(注入)的Object 直接通訊)|
---
### 創建式
| 模式(Patterns) | 簡單描述(Simple description) |
| :------------: | :-------------------------:
|簡單工廠模式(Simple Factory Pattern) |使用一個對象建立不同的對象,Client 傳入1個String 返回一個對象|
|工廠模式(Factory Pattern) | Comming soon... |
|抽象工廠模式(Abstract Factory Pattern) | Comming soon... |
|單例模式(Singleton Pattern) |Comming soon...  |
|原形模式(Prototype Pattern) |Comming soon...  |
|建立者模式(Builder Pattern) | Comming soon... |

### 結構型模式
| 模式(Patterns) |簡單描述(Simple description)|
| :------------: | :------------------------:|
|適配器模式(Adapter Pattern) | Comming soon... |
|橋接模式(Bridge Pattern) | Comming soon... |
|組合模式(Composite Pattern) | Comming soon... | 
|裝飾模式(Decorator Pattern) | Comming soon... |
|外觀模式(Facade Pattern) |Comming soon...  |
|享元模式(FlayWeight Pattern) | Comming soon... |
