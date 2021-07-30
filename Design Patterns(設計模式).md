## Design Patterns(設計模式)

> 設計模式(Design Pattern) 是對軟體設計中普遍存在（反覆出現）的各種問題，所提出的解決方案。
>
> 設計模式是描述在各種不同的情況下，如何解決問題的一種方案。
>
> OOP設計模式通常以類別或者物件來描述關係和相互的作用(不涉及完成應用的特定object)。
>
> 設計模式能使不穩定依賴於相對穩定、具體依賴於相對抽象，避免會引起麻煩的緊耦合。

---

* **設計模式的類型**
  * 建立型模式
    * 工廠方法模式(Factory Method Pattern)
  * 結構型模式
  * 行為型模式

---

## 創建型模式

> *提供一種建立Object的同時隱藏logic的方式(不是使用直接使用new 直接建立)*

+ **工廠模式(Factory Pattern)**

  > 定義:建立一個接口,讓子類自己決定實現哪一個Factory
  >
  > 例子:需要一台手機，工廠直接給你貨，工廠的製作過程不用去管(實現方法)
  >
  > **優點:**
  >
  > **1.直接調用Factory便可建立對應的Object instance**
  >
  > **2.擴展性高，要新增產品，只要擴展Factory class**
  >
  > **3.無需知道具體實現**
  >
  > **缺點:**
  >
  > **新增一個產品都要1個具體類以及該Object實現的工廠(工廠類實現該產品類的logic),增加了系統複雜度以及對具體類的依賴**
  >
  > ---
  >
  > **使用Factory Patterns:**
  >
  > * **任何需要建立複雜Object的地方，對於建立簡單的Object就不需要(可以直接new)**
  >
  > ---
  >
  > ```c++
  > //Factory Pattern 🌰
  > //使用工廠建立不同圖形，圖形類都有draw 的方法
  > class Shape{
  >     public:
  >     	virtual void draw() = 0;
  > }
  > 
  > //3個具體圖形類
  > class Circle : public Shape{
  >     public:
  >     	void draw {
  >             //draw a Circle
  >             printf("draw a Circle");
  >         }
  > }
  > 
  > class Square : public Shape{
  >     public:
  >     	void draw {
  >             //draw a Square
  >             printf("draw a Square");
  >         }
  > }
  > 
  > class Rectangle : public Shape {
  >     public:
  >     	void draw {
  >             //draw a Rectangle
  >              printf("draw a Rectangle");
  >         }
  > }
  > 
  > 
  > //Factory class
  > class ShapeFactory {
  >     pulic:
  >     	Shape* getShape(string shape){
  >             switch(shape){
  >                 case "Circle":
  >                     return new Circle();
  >                     break;
  > 
  >                case "Square":
  >                     return new Square();
  >                     break;
  > 
  >                case "Rectangle":
  >                     return new Rectangle();
  >                     break;
  >                 default:
  >                     return null;
  >             }
  >         }
  > }
  > 
  > //使用工廠模式，透過工廠類
  > int main(){
  >     ShapeFactory* Factory = new ShapeFactory();
  >     //get Circle
  > 
  >     Shape* shape1 = Factory->getShape("Circle");
  >     Shape* shape2 = Factory->getShape("Square");
  >     Shape* shape3 = Factory->getShape("Rectangle");
  > 
  >     //call draw method
  >     shape1->draw(); //out: draw a Circle
  >     shape2->draw(); //out: draw a Square
  >     shape3->draw(); //out: draw a Rectangle
  > }
  > 
  > //缺點就是每次新增1個product,都必須要對工程類進行修改，而且是依賴與實體類
  > ```

+ 抽象工廠模式(Abstract Factory Pattern)

  + > 定義:
    >
    > 通過1個超級工廠建立其他工廠
    >
    > **優點:**
    >
    > **1個Product可能會有多個Object一起工作,這個模式能保證Client始終只使用同一個Product中的Object**
    >
    > **缺點:**
    >
    > **如果要新增產品，Abstract Object裡面要加,具體類也要加**
    >
    > ---
    >
    > ```c++
    > //🌰:shape 和 color ，Factory
    > 
    > //Abstract Class
    > class Shape{
    >  public:
    >  	virtual void draw() = 0;
    > }
    > 
    > class Color{
    >  public:
    >  	virtual void fill() = 0;
    > }
    > 
    > class Factory {
    >  public:
    >  	virtual Shape* getShape(string shape) = 0;
    >  	virtual Color* getColor(string color) = 0;
    > }
    > 
    > class FactoryProducer{
    >  public:
    >  	virtual Factory* getFactory(string type) = 0;
    > }
    > ```
    >
    > ```c++
    > //Implement
    > //Shape 
    > class Circle : public Shape{
    >  public:
    >  	void draw(){
    >          printf("Darw a circle");
    >      }
    > }
    > class Rectangle : public Shape{
    >  public:
    >  	void draw(){
    >          printf("Draw a Reactangle")
    >      }
    > }
    > class Square : public Shape{
    >  public:
    >  	void draw{
    >          printf("Draw a Square")
    >      }
    > }
    > 
    > //Color
    > class Red : public Color{
    >  public:
    >  	void fill(){
    >         	printf("Fill color red");
    >      }
    > }
    > 
    > class Green : public Color{
    >  public:
    >  	void fill(){
    >         	printf("Fill color Green");
    >      }
    > }
    > 
    > class Blue : public Color{
    >  public:
    >  	void fill(){
    >         	printf("Fill color Blue");
    >      }
    > }
    > ```
    >
    > ```c++
    > //Implement
    > //Factory
    > class ShapeFactoy : public Factoty{
    >  public:
    >  	Shape* getShape(string shape){
    >          switch(shape){
    >              case "Circle":
    >                  return new Cicle();
    >                  break;
    >              case "Retangle":
    >                  return new Retangle();
    >                  break;
    >              case "Square":
    >                  return new Square();
    >                  break;
    >              default:
    >                  return null;
    >                  break;
    >          }
    >      };
    >  	Color* getColor(string color){return null;}
    > }
    > class ColorFactory : public Factory{
    >  public:
    >  	Shape* getShape(string shape){return null;}
    >  	Color* getColor(string color){
    >          switch(color){
    >              case "Red":
    >                  return new Red();
    >                  break;
    >              case "Green":
    >                  return new Green();
    >                  break;
    >              case "Blue":
    >                  return new Blue();
    >                  break;
    >              default:
    >                  return null;
    >                  break;
    >          }
    >      }
    > }
    > ```
    >
    > ```c++
    > //implment
    > //factoryProducer
    > class Procucer : public FactoryProducer{
    >  public:
    >  	Factory* getFactory(string type){
    >          switch(type){
    >              case "Shape":
    >                  return new ShapeFactory();
    >                  break;
    >              case "Color":
    >                  return new ColorFactory();
    >                  break;
    >              default:
    >                  return null;
    >                  break;
    >          }
    >      }
    > }
    > ```
    >
    > ```c++
    > //main
    > int main(){
    >  //就是透過工廠生成接口生成不同的工廠 
    >  //再透過不同的工廠調用其生產的product
    > 
    >  FactoryProducer * producer = new Procucer();//生成producer object
    >  Factory* shapeF = producer->getFactory("Shape");//回傳Shape工廠
    >  Factory* ColorF = producer->Factory("Color");//回傳Shape工廠
    > 
    >  //透過不同工廠生成不同product
    >  //Shape
    >  Shape* circleShape = shapeF->getShape("Circle");
    >  Shape* RetangleShape = shapeF->getShape("Retangle");
    >  Shape* SquareShape = shapeF->getShape("Square");
    > 
    >  circleShape->Draw(); //Draw a circle
    >  RetangleShape->Draw();//Draw a Retangle
    >  SquareShape->Draw();//Draw a Square
    > 
    >  //Color
    >  Color* RedColor = ColorF->getColor("Red");
    >  Color* GreenColor = ColorF->getColor("Green");
    >  Color* BlueColor = ColorF->getColor("Blue");
    > 
    > 	RedColor->fill(); //fill red color
    >  GreenColor->fill(); //fill green color;
    >  BlueColor->fill(); //fill blue color
    > 
    >  //free all memory
    >  return;
    > }
    > ```
    >
    > > 如果要新增1個產品就要新增產品的抽象類，具體類，以及工廠類，並且要修改Factory的抽象類以及Producer的具體類，難以擴展

+ 5

+ 