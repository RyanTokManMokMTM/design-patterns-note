## 結構型模式

----

* **適配器模式(Adapter Pattern)**

   > 定義(就是加入一個Adapter的class 去調用其他接口):
   >
   > 將一個接口轉出Client希望或者想要的另外一個接口，使接口不兼容的class能一起工作，Adapter也稱為包裝器(Wrapper)
   >
   > 2種模式:
   >
   > * *類結構型(Class structrue mode)*(是用那個較多)
   >   * 與適配的Class 是 繼承或者實現的關係
   > * *對象結構型(Object structrue mode)*
   >   * 與適配的Class 是關聯的關係
   >
   > ---
   >
   > **優點:**
   >
   > * 能將目標class 與 adapter class 解耦。通過`Adapter class` 來重用`Adaptee class` 提供的方法等
   > * 增加了class的透明性與可複用性。
   >   * `Adapter class`內的實現,如何調用`Adaptee`對Client 是透明的
   >   * 如果同為一個`Adapter class` ，可以用在不同的System。(如這些系統都需使用一個API的`Adapater`)
   > * 高靈活性以及擴展性
   >   * 擴展性性: `class adapter mode` 可以通過xml等文件適配不同的`adapter`。(`class adapter mode`跟適配者是繼承關係)
   >   * 高靈活性: 
   >     * `Object adapter mode` 一個`Object adapter class` 可把不用的`Adapatee`適配到同一個`target`
   >       * `Object adapter mode`是 `inherit` 於`target` ，所以可以有多個不同的`Adapater`可以適配不同的`Adapatee`到一個target
   >     * 由於`Object adapter mode` 的`Adapter`與`Adapatee`是關聯的關係，所以`Adapatee`的sub-class 也可以通過這個`Adapater` 進行適配(liskov substitution principle): base class -> substitution  as  sub-calss
   >
   > **缺點:**
   >
   > * `class adapter mode`
   >   * `class adapter mode` 必須同時`inherit`於`target`以及`Adaptee` ，有些語言不支援
   >   * `Adapter`不同為 `final`類型(最終類,不能在編譯時進行修改)
   >   * 部分語言使用該`Adapter pattern`有一定限制(如必須為`interface`，不能是`class`等等)
   > * `object adapter mode`
   >   * 主要的缺點是如果要替換`Adaptee`的某些方法，只能透過建立`Adaptee`的子類,override 要轉換的方法，在把子類給`Adapater`進行適配的工作(比較麻煩)
   >
   > ---
   >
   > **使用場合:**
   >
   > 1. 目前System 使用一些現有的class(可能沒有源碼).而class 的接口(如method name)不符合系統的設計，甚至沒有Source code的情況。
   > 2. 建立一些可以重複使用的class(其他系統也能使用的類等)，主要用於彼此沒有太大關聯的class，可以使用`Adapater pattern`進行設計與使用
   >
   > ---
   >
   > **Object structure mode**
   >
   > ```mermaid
   > classDiagram
   > 	class Target{
   > 		<<Absturct>>
   > 		+request() void
   > 	}
   > 
   > 	class Adapter{
   > 		- adaptee : Adapetee
   > 
   > 		+request : void
   > 	}
   > 
   > 	class Adaptee{
   > 		+sepcificRequest() : void
   > 	}
   > 
   > 	client ..> Target
   > 	Adapter  ..|> Target
   > 	Adapter o.. Adaptee
   > 
   > ```
   >
   > * Target : Client 所需的接口，可抽象可具體
   > * Adapter : 用於調用另外一個接口，通過`繼承Target` 以及 `關聯適配者`
   > * Adaptee : 被適配/被Adapter調用接口的角色
   >
   > ```c++
   > class Target{
   >     public:
   >     	virtual void request() = 0
   > }
   > 
   > class Adator : public Target{
   >     public:
   >     	Adator(Adaptee* adaptee){this->adaptee = adaptee; }
   >     	void request(){
   >             //TODO call adaptee
   >             adaptee->specificRequest();
   >         }
   >     private
   >         Adaptee* adaptee = nullptr;
   > }
   > 
   > class Adaptee {
   >     public:
   >     	void specificRequest(){
   >             //TODO something
   >         }
   > }
   > ```
   >
   > ---
   >
   > #### Call Algorithms API 🌰
   >
   > ```mermaid
   > classDiagram
   > 	class ScoreOperation{
   > 		<<Abstruct>>
   > 		+sort(int[])  int[]
   > 		+search(int[],int)  int
   > 	}
   > 
   > 
   > 	class Adapter{
   > 		+sort(int[])  int[]
   > 		+search(int[],int)  int
   > 		+Adapter() 
   > 		-qSort QuickSort
   > 		-bSearch BinarySearch
   > 	}
   > 
   > 	class QuickSort{
   > 		+quickSort(int[]) int[]
   > 		+sort(int[],int,int) void
   > 		+partition(int,int,int) int
   > 		+swap(int,int,int)
   > 	}
   > 
   > 	class BinarySearch{
   > 		+binarySearch(int[],int) int
   > 	}
   > 
   > 	client ..> ScoreOperation
   > 	Adapter ..|> ScoreOperation
   > 	Adapter ..> QuickSort
   > 	Adapter ..> BinarySearch
   > ```
   >
   > ---
   >
   > **Class Structure mode(比較少用，需要多重繼承)**
   >
   > ```mermaid
   > classDiagram
   > 	class Target{
   > 		<<Absturct>>
   > 		+request() void
   > 	}
   > 
   > 	class Adapter{
   > 		- adaptee : Adapetee
   > 		+request : void
   > 	}
   > 
   > 	class Adaptee{
   > 		+sepcificRequest() : void
   > 	}
   > 
   > 	client ..> Target
   > 	Adapter  ..|> Target
   > 	Adapter --|> Adaptee
   > ```
   >
   > > 這個模式會`inherit` client的接口以及適配者接口
   >
   > ```c++
   > class Adapter : public Target,Adaptee{
   >     public:
   >     	void request(){
   >             specificRequest()
   >         }
   > }
   > ```
   >
   > ---
   >
   > > 另一種適配器模式(Adapter pattern)
   > >
   > > **缺省適配器模式(Default Adapter Pattern)**，就是定義一個很多方法的Interface，然後透過Abstract class 去實現並提供默認的實現(可能是空的)，然後讓該抽象類的Sub class 透過`override`的方式提供某些方法的實現。*在Interface與具體實現之間加入一個`Default Adapter Class`*
   > >
   > > 這個模式可以讓繼承的子類只實現自己想要的方法，其他方法都為默認實現(空方法)

* 橋接模式(`Bridge Pattern`)

> 定義(將多個維度分離成獨立的維度-解決多層繼承的互相影響的問題)通過組合不同維度:
>
> 將抽象部分與它的實現部分分離,使他們都可以獨立地變化(**分離維度**)，透過分離結構使用橋接/關聯的方式進行互動
>
> ---
>
> **優點**
>
> * 把繼承層次結構分離成`抽象`以及`具體`等多維度變成獨立的維度，解耦繼承層次關係
> * 取代了繼承結構，解決多層繼承違背`SRP`，以及避免生成多個子類
> * 提高了系統擴展性，擴展無需修改Source,符合`OCP`
>
> **缺點**
>
> * 增加系統的理解和設計難點
> * 必須識別出獨立維度
>
> ---
>
> **使用場地:**
>
> 1. 如system需要在`Abstract`與`instance`之間有更高的靈活性，避免2個層次之間建立靜態的inheritance relationship，可使用`bridge pattern` 建立關聯關係
> 2. 抽象化與實現要進行動態耦合，如抽象部分與實現部分獨立擴展不影響，並在運行時將各自的sub-class獨自進行組合。
> 3. 一個class 存在倆個或者以上的獨立維度的變化，將多個維度拆開成獨立的維度，進行獨立的擴展
> 4. 對於某些class因多層繼承導致系統的class數目極劇增加，可以使用`Bridge Pattern`解決
>
> ---
>
> > 多層繼承,多維度問題
> >
> > 每個OS的Class都需要做解析以及顯示的工作，違反SRP
> >
> > 而且新增新的OS或者Image都得新增多個具體的class
>
> ```mermaid
> classDiagram
> 	class Image{
> 		<<Abstruct>>
> 	}
> 
> 	BMP ..|> Image
> 	JPG ..|> Image
> 	GIF ..|> Image
> 	class BMP{
> 
> 	}
> 
> 	class JPG{
> 
> 	}
> 
> 	class GIF{
> 
> 	}
> 
> 	WindowBMP --|> BMP
> 	LinuxBMP --|> BMP
> 	UnixBMP --|> BMP
> 	class WindowBMP{
> 
> 	}
> 
> 	class LinuxBMP{
> 
> 	}
> 
> 	class UnixBMP{
> 
> 	}
> 
> 	WindowGIF --|> GIF
> 	LinuxGIF --|> GIF
> 	UnixGIF --|> GIF
> 	class WindowGIF{
> 
> 	}
> 
> 	class LinuxGIF{
> 
> 	}
> 
> 	class UnixGIF{
> 
> 	}
> 
> 	WindowJPG --|> JPG
> 	LinuxJPG --|> JPG
> 	UnixJPG --|> JPG
> 	class WindowJPG{
> 
> 	}
> 
> 	class LinuxJPG{
> 
> 	}
> 
> 	class UnixJPG{
> 
> 	}
> ```
>
> ---
>
> > 設計`Bridge Pattern`的重點
> >
> > * 拆成2個維度(關聯關係)
> >   * 抽象部分(Object的本身)
> >   * 實現部分(Object的動作(做什麼)
> >
> > > 毛筆為例:
> > >
> > > 毛筆本身為抽象類，而顏色則為實現的部分
> > >
> > > ```mermaid
> > > classDiagram
> > > 	class 毛筆{
> > > 		<<Abstruct>>
> > > 		#color 顏色
> > > 		+setColor(顏色) void
> > > 		+draw() void
> > > 	}
> > > 	class 顏色{
> > > 		<<Abstract>>
> > > 		+shader() void
> > > 	}
> > > 	顏色 --o 毛筆
> > > 	
> > > 	小號毛筆 ..|> 毛筆
> > > 	大號毛筆 ..|> 毛筆
> > > 	中號毛筆 ..|> 毛筆
> > > 	class 小號毛筆{
> > > 		+draw() void
> > > 	}
> > > 	class 大號毛筆{
> > > 		+draw() void
> > > 	}
> > > 	class 中號毛筆{
> > > 		+draw() void
> > > 	}
> > > 	
> > > 	紅色 ..|> 顏色
> > > 	綠色 ..|> 顏色
> > > 	藍色 ..|> 顏色
> > > 	class 紅色{
> > > 		+shader() void
> > > 	}
> > > 	class 綠色{
> > >         +shader() void
> > > 	}
> > > 	class 藍色{
> > >         +shader() void
> > > 	}
> > > 	
> > > ```
> > >
> > > 
>
> ---
>
> > OS與Image 分離結構
>
> ```mermaid
> classDiagram
> 	class Image{
>     	<<Abstruct>>
>     	#Imp ImageImplement
>     	+setImp(ImageImplement) void
>     	+parseFile(string) void
> 	}
> 	Image o-- ImageImplement
> 	PNG --|> Image
> 	JPG --|> Image
> 	BMP --|> Image
> 	GIF --|> Image
> 
> 	class PNG{
> 
> 	}
> 
> 	class JPG{
> 
> 	}
> 
> 	class BMP{
> 
> 	}
> 
> 	class GIF{
> 
> 	}
> 
> 	class ImgMat{
> 
> 	}
> 
> 	ImageImplement  ..> ImgMat
> 
> 	ImageImplement
> 	class ImageImplement{
> 		<<Interface>>
> 		+doPaint(ImgMat) void
> 	}
> 
>     WindownsImplement  ..|> ImageImplement
>     LinuxImplement  ..|> ImageImplement
>     UnixImplement  ..|> ImageImplement
> 
> 	class WindownsImplement{
> 		+doPaint(ImgMat) void
> 	}
> 
> 	class LinuxImplement{
> 		+doPaint(ImgMat) void
> 	}
> 
> 	class UnixImplement{
> 		+doPaint(ImgMat) void
> 	}
> ```
>
> ```c++
> class Image{
>  	protected:
>     	ImageImplement* imp;
>     public:
>     	void setImp(ImageImplement* imp){
>             this->imp = imp;
>         }
> 
>     	virtual void parseFile(string fileName)
> }
> 
> //For image
> class JPG : public Image{
>     public:
>          void parseFile(string fileName){
> 			//new Mat
>              //imp->doPaint(mat)
>              //to paint windown JPH
>          }
> }
> class BMP : public Image{
>    public:
>          void parseFile(string fileName){
> 			//new Mat
>              //imp->doPaint(mat)
>              //to paint windown BMP
>          }
> }
> class PNG : public Image{
>    public:
>          void parseFile(string fileName){
> 			//new Mat
>              //imp->doPaint(mat)
>              //to paint windown PNG
>          }
> }
> class GIF : public Image{
>    public:
>          void parseFile(string fileName){
> 			//new Mat
>              //imp->doPaint(mat)
>              //to paint windown GIF
>          }
> }
> 
> //For OS Parase
> class ImgMat{
>     //TODO OS Image Parse
> }
> 
> class ImageImplement{
>     public:
>     	virtual void doPaint(ImgMat OSmat) = 0
> }
> 
> class WindownsImplement : public ImageImplement{
>     public:
>     	void doPaint(ImgMat OSmat){
>             //Print Windown Img
>         }
> }
> class UnixImplement : public ImageImplement{
>    	public:
>     	void doPaint(ImgMat OSmat){
>             //Print Unix Img
>         }
> }
> class LinuxImplement : public ImageImplement{
>    public:
>     	void doPaint(ImgMat OSmat){
>             //Print Linux Img
>         }
> }
> ```
>
> ---
>
> > `Bridge Pattern`與 `Adapter Patter`可能會一起使用
> >
> > ```mermaid
> > classDiagram
> > 	class 顯示報表{
> > 		<<Absstruct>>
> > 	}
> > 	顯示報表 o-- 數據採集
> > 	顯示方法一 --|> 顯示報表
> > 	顯示方法二 --|> 顯示報表
> > 	class 顯示方法一{
> > 		
> > 	}
> > 	
> > 	class 顯示方法二{
> > 	
> > 	}
> > 	
> > 	class 數據採集{
> > 		<<Interface>>
> > 	}
> > 	
> > 	文本採集 --|> 數據採集
> > 	DB採集 --|> 數據採集
> > 	Excel採集 --> 數據採集
> > 	class 文本採集{
> > 	
> > 	}
> > 
> > 	class DB採集{
> > 	
> > 	}
> > 	
> > 	class Excel採集{
> > 	
> > 	}
> > 	ExceltAdapter *--  Excel採集
> > 	ExceltAdapter o-- ExcelAPI
> > 	class ExceltAdapter{
> > 	
> > 	}
> > 	
> > 	class ExcelAPI{
> > 	
> > 	}
> > 	
> > ```
> >
> > 

* 組合模式(composite Pattern)

  * >定義(稱為Part-whole模式):
    >
    >組合多個Object形成Tree表示具有`整體-部分`關係的Structure
    >
    >* 單個對象(Leaf)
    >* 組合對象(Container)
    >
    >對Leaf 和 Contrainer 的使用有一致性
    >
    >---
    >
    >`Leaf 和 Container `共用一個抽象(Abstruct) Class.
    >
    >`Leaf` implement Abstruct class 的 menthod 會提供處理異常的方式進行處理(`Leaf 沒有其他Node了`)
    >
    >`container` implement Abstruct class 的 menthod 會提供visit 以及 manage Node 的方法(可以包含`leaft`以及`container`)
    >
    >> 該模式的重點是`Abstract代表Leaf 或者是Container`
    >>
    >> 不論是`leaf` 或者是`container`都統一對待,處理方式不一樣.(`區別對待會更複雜`)
    >>
    >> > 安全模式: 抽象類只提供都有的方法其他方法都在具體類自己增加，但client不能是抽象類編程!
    >
    >---
    >
    >**優點**
    >
    >* 組合模式(`composite pattern`) 清楚地定義分層的複雜對象,對於整個Object(整個結構) 或者部分Object(部分結構),Client 可以ignore 複雜地層次機構，可以方便得進行控制。
    >* Client 可以一致的對象組合對象(`Composite Object`)或者單個對象(`Single Object`),無需關心處理的是哪個結構(整個或是單個等等)，簡化了代碼。
    >* 組合模式(`composite pattern`) 對於新增`container`以及`leaf`很方便(對抽象編程)，無需修過代碼，符合`OCP`。
    >* 組合模式(`composite pattern`) 為`Tree-structure system`提供一種靈活的解決方案，通過`leaf`以及`container`的遞歸組合(`recursive compoition`), 形成複雜的`Tree-structure` ,但卻能簡單控制。
    >
    >**缺點**
    >
    >* 組合模式(`composite pattern`)  很難對某個類型提供約束，因為都來自於同一個抽象類，要約束某些特定的object,需要檢測類型是什麼才能進行約束(使用反射?)，實現比較複雜
    >
    >---
    >
    >**使用場地:**
    >
    >1. 具有整體以及部分的層次結構中,希望client可以一致性對象整體以及部分的差異(`container`以及`leaf`)
    >2. 需要處理`tree-structure`的系統
    >3. 在系統中可以分離出`container`以及`leaf`，而且他們的類型不固定(可能需要新增新的類型)
    >
    >---
    >
    >> 簡單的組合模式的例子
    >
    >
    >
    >```mermaid
    >classDiagram
    >  class Component{
    >      <<Absturct>>
    >      +add(Component) void
    >      +remove(Component) void
    >      +getChild(int) Component
    >      +operation() void
    >  }
    >
    >  class Leaf{
    >  	+add(Component) void
    >      +remove(Component) void
    >      +getChild(int) Component
    >      +operation() void
    >  }
    >
    >  class Container {
    >  	+add(Component) void
    >      +remove(Component) void
    >      +getChild(int) Component
    >      +operation() void
    >  }
    >
    >  Leaf ..|> Component
    >  Container  ..|> Component
    >  Container  o-- Leaf
    >```
    >---
    >
    >> 殺毒軟件(文件結構例子)
    >
    >```mermaid
    >classDiagram
    >	class AbstractFile{
    >		<<Absturct>>
    >		+add(AbstractFile) void
    >		+remove(AbstractFile) void
    >		+getChild(i) : AbstractFile
    >		+killVirus() void
    >	}
    >
    >	class ImageFile{
    >		+add(AbstructFile) void
    >		+remove(AbstructFile) void
    >		+getChild(i) : AbstructFile
    >		+killVirus() void
    >	}
    >
    >	class VideoFile{
    >		+add(AbstructFile) void
    >		+remove(AbstructFile) void
    >		+getChild(i) : AbstructFile
    >		+killVirus() void
    >	}
    >
    >	class TextFile{
    >		+add(AbstructFile) void
    >		+remove(AbstructFile) void
    >		+getChild(i) : AbstructFile
    >		+killVirus() void
    >	}
    >
    >	class Folder{
    >		+add(AbstructFile) void
    >		+remove(AbstructFile) void
    >		+getChild(i) : AbstructFile
    >		+killVirus() void
    >	}
    >
    >	ImageFile ..|> AbstractFile
    >	VideoFile ..|> AbstractFile
    >	TextFile ..|> AbstractFile
    >	Folder ..|> AbstractFile
    >	Folder o-- AbstractFile
    >```
    >
    >```c++
    >class AbstractFile{
    >   public:
    >       virtual void add(AbstractFile file) = 0;
    >       virtual void remove(AbstractFile file) = 0;
    >       virtual AbstractFile get(int i) = 0;
    >   	   virtual void killVirus() = 0;
    >}
    >
    >//Leaf
    >class ImageFile : public AbstractFile{
    >   private:
    >   	std::string name;
    >   public:
    >   	ImageFile(std::string name){
    >       	this->name = name 
    >       }
    >
    >   	void add(AbstractFile file){
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       void remove(AbstractFile file){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       AbstractFile get(int i){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >   	void killVirus() {
    >           printf("對該圖形%s進行殺毒操作",this->name);
    >       };
    >}
    >class VideoFile : public AbstractFile{
    >   private:
    >   	std::string name;
    >   public:
    >   	VideoFile(std::string name){
    >           this->name = name;
    >       }
    >   	void add(AbstractFile file){
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       void remove(AbstractFile file){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       AbstractFile get(int i){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >   	void killVirus() {
    >           printf("對該影片%s進行殺毒操作",this->name);
    >       };
    >}
    >class TextFile : public AbstractFile{
    >   private:
    >   	std::string name;
    >   public:
    >       TextFile(std::string name){
    >           this->name = name
    >       }
    >
    >   	void add(AbstractFile file){
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       void remove(AbstractFile file){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >       AbstractFile get(int i){ 
    >           //沒有其他Node，而且不是folder
    >           printf("抱歉,此文件不支援")
    >       };
    >
    >   	void killVirus() {
    >           printf("對該文本%s進行殺毒操作",this->name);
    >       };
    >}
    >
    >//contrainer
    >class Folder : public AbstractFile{
    >   //因為是Folder是可以包含其他leaf的或者是其他Folder
    >   //使用一個list 來保存他的sub tree node
    >   private:
    >   	//假設有這個list 結構
    >   	list<AbstractFile> *filesList = new list<AbstractFile>();
    >   	std::string name;
    >   public:
    >   	Folder(std::string name){
    >           this->name = name
    >       }
    >
    >      	void add(AbstractFile file){
    >			filesList->add(file);
    >       };
    >
    >       void remove(AbstractFile file){ 
    >			filesList->remove(file);
    >       };
    >
    >       AbstractFile get(int i){ 
    >			return AbstractFile(filesList->get(i));
    >       };
    >
    >   	void killVirus() {
    >          //要對文件夾進行殺毒,就得對該文件夾下的所以文件以及內部的所有文件夾的文件進行殺毒
    >          //對保存在這個node 下的文件list 調用這些文件的 killVirus()
    >          for(auto file : filesList){
    >              file->killVirus(); //如果當前文件是個文件夾就會一直recursive
    >          }
    >       };
    >
    >
    >}
    >```
    >
    >```c++
    >//Client 只需對AbstractFile 繼續編程即可
    >//組合模式符合OCP
    >int main(){
    >   AbstractFile *file1,*file2,*file3,*file4,*file5,*folder1,*folder2,*folder3,*folder4;
    >
    >   //create folder
    >   folder1 = new Folder("Jackson data");
    >   folder2 = new Folder("Image Files");
    >   folder3 = new Folder("Video Files");
    >   folder4 = new Folder("Text Files");
    >
    >   file1 = new ImageFile("a.jpg");
    >   file2 = new ImageFile("b.jpg");
    >   file3 = new VideoFile("a.mp4");
    >   file4 = new VideoFile("b.mp4");
    >   file5 = new TextFile("x.txt");
    >
    >   folder2->add(file1);
    >	folder2->add(file2);
    >	folder3->add(file3);
    >	folder3->add(file4);
    >	folder4->add(file5);
    >	folder1->add(folder2);
    >	folder1->add(folder3);
    >   folder1->add(folder4);
    >
    >   //folder1 是root folder
    >   folder->killVirus();//會進行recusive 調用
    >}
    >```
    >
    >---
    >
    >> 以上有個問題
    >
    >> **就是Leaf不會有其他Node，也就是必須要實現Abstract中的方法包括Add Remove 的等等，對於Leaft而已就很多餘了**
    >
    >> 解決方法有2個
    >
    >> * 透明組合模式(以上例子的方法)-這個方法不安全，因為沒必要或者不應該存在的方法也實現了
    >> * 安全組合模式(就是Abstract 只Declared 有必要的方法，其他的就是實體類自行去實現。問題是client 就必要要對實體類進行編程，無法通過抽象類進行編程)-不夠透明(必須對實體類編程)

* 裝飾模式(Decorator Pattern)

  * > 定義:(該模式解決複用的問題，以關聯取代繼承) -> `裝飾類`給對象新增額外的職責(取代繼承)
    >
    > 動態地給一個對象新增額外的職責,就增加對象功能來說,裝飾模式比子類實現更加靈活(只需聯合需增加功能的對象)
    >
    > * 為了具有靈活以及擴展，定義一個抽象的裝飾類，具體讓子類實現
    >
    > ---
    >
    > **優點:**
    >
    > * 對於擴展對象的功能，比繼承更加靈活，而且不會使具體類極具增加(減低耦合)。
    > * 通過動態的方式擴展對象的功能，運行時選擇不同的具體裝飾類，實現不同的行為
    > * 可對一個對象進行多次的裝飾，透過不同組合，實現功能強大的對象
    > * 具體的構建以及具體的裝飾類可以獨立變化,用戶可更加需要新增，不用改變源碼，符合`OCP`
    >
    > **缺點:**
    >
    > * 會產生大量小的對象,而且每個小的對象的區別在於，對象之間的連接的方式的不同，而不是對象之間的屬性不一樣(都繼承於一個抽象類)，大量的小對象會佔用資源，影響一定的性能
    > * Decorator Pattern 比 Inheritance 更靈活,但會比較容易出錯,排錯會比較困難
    >
    > ---
    >
    > **使用場地:**
    >
    > 1. 需要在不影響其他對象下,動態的,透明的增加新的單一職責
    > 2. 當不能採用繼承的方式進行擴展或者繼承不利於維護
    >    * 不能採用繼承的2種情況
    >      * 系統中存在大量獨立擴展的類，擴展這些類/組合這些類會使子類個數暴漲
    >      * 類已經被定義為不可繼承
    >
    > ---
    >
    > *存在問題的例子*
    >
    > ```mermaid
    > classDiagram
    > 
    > class Component{
    > 	<<Abstract>>
    > 	+display() void
    > }
    > 
    > class Window{
    > 	+display() void
    > }
    > 
    > class TextBox{
    > 	+display() void
    > }
    > 
    > class ListBox{
    > 	+display() void
    > }
    > 
    > Window --|> Component
    > TextBox --|> Component
    > ListBox --|> Component
    > 
    > class ScrollBarWindow{
    > 	+display() void
    > 	+setScrollBar() void
    > }
    > 
    > class BlackBorderWindow{
    > 	+display() void
    > 	+setBlackBorder() void
    > }
    > 
    > ScrollBarWindow --|> Window
    > BlackBorderWindow --|> Window
    > 
    > class ScrollBarAndBlackBorderWindow{
    > 	+display() void
    > }
    > 
    > ScrollBarAndBlackBorderWindow --|> ScrollBarWindow
    > ScrollBarAndBlackBorderWindow --|> BlackBorderWindow
    > 
    > class ScrollBarTextBox{
    > 	+display() void
    > 	+setScrollBar() void
    > }
    > 
    > class BlackBorderTextBox{
    > 	+display() void
    > 	+setBlackBorder() void
    > }
    > 
    > ScrollBarTextBox --|> TextBox
    > BlackBorderTextBox --|> TextBox
    > 
    > class ScrollBarAndBlackBorderTextBox{
    > 	+display() void
    > }
    > 
    > ScrollBarAndBlackBorderTextBox --|> ScrollBarTextBox
    > ScrollBarAndBlackBorderTextBox --|> BlackBorderTextBox
    > 
    > class ScrollBarListBox{
    > 	+display() void
    > 	+setScrollBar() void
    > }
    > 
    > class BlackBorderListBox{
    > 	+display() void
    > 	+setBlackBorder() void
    > }
    > 
    > ScrollBarListBox --|> ListBox
    > BlackBorderListBox --|> ListBox
    > 
    > class ScrollBarAndBlackListBox{
    > 	+dispaly() void
    > }
    > 
    > ScrollBarAndBlackListBox --|> ScrollBarListBox
    > ScrollBarAndBlackListBox --|> BlackBorderListBox
    > 
    > ```
    >
    > > 這個例子的問題是複用性導致，而且新增一個新的都必須新增多個具體類
    >
    > ---
    >
    > **裝飾模式的對象設計**
    >
    > > Component: 具體類以及裝飾類的共同父類，讓client可以透明操作，一致處理操作
    > >
    > > Concrete Component :具體類，實現Component的抽象方法(實現構件)
    > >
    > > Decorator : 抽象的裝飾類,給具體的構件增加職責，其中包含一個reference的對象，該reference可以調用裝飾前的構建
    > >
    > > ​					而裝飾，新增新的職責在子類完成
    > >
    > > Concrete Decorator : 具體的裝飾類,負責為構件新增職責，而每個具體裝飾類都會定義不用的裝飾行為，通過調用抽象									類中定義的方法對構件進行擴展
    >
    > > 由於構件類以及裝飾類都是基於相同的接口實現，可把已新增職責的構件注入到其他裝飾類進行二次裝飾
    >
    > ```c++
    > //裝飾模式Decorator 簡單代碼
    > class Decorator : public Component{
    >     private:
    >     	Component* component = nullptr;
    >     pbulic:
    >     	Decorator(component *Component){
    >             //setter
    >             this->component = component;//透過注入 維護構建
    >         }
    >     	
    >     	//實現抽象類的方法
    >     	void operation(){
    >             //因為Decorator不是構建 所以調用原Component的方法
    >             //讓子類進行調用
    >             component->operation(); //調用構建的方法
    >         }
    > }
    > ```
    >
    > ```c++
    > class ConcreteDecorator : public Decorator{
    >     public:
    >     	ConcreteDecorator(component *Component){
    >             //設置注入到父類
    >             supuer(component);
    >         }
    >     
    >     	void operation(){
    >             //override 父類方法 
    >             //同時調用父類方法進行擴展
    >             super.operation();
    >             this->addBehaviour() //裝飾Component
    >         }
    >     
    >     	void addBehaviour(){
    >             //add some thing to the compoennt
    >         }
    > }
    > ```
    >
    > ---
    >
    > **使用裝飾模式解決例子(克服繼承)**
    >
    > ```mermaid
    > classDiagram
    > class Component{
    > 	<<Abstruct>>
    > 	+display() void
    > }
    > 
    > class Window{
    > 	+display() void
    > }
    > 
    > class TextBox{
    > 	+display() void
    > }
    > 
    > class ListBox{
    > 	+display() void
    > }
    > 
    > class Decorator{
    > 	-Component component
    > 	+Decorator(Component)
    > 	+display() void
    > }
    > 
    > Window --|> Component
    > TextBox --|> Component
    > ListBox --|> Component
    > Decorator --|> Component
    > 
    > class ScrollBarDecorator{
    > 	+dioplay() void
    > 	+setScrollBar() void
    > 	+ScrollBarDecorator(Component)
    > }
    > 
    > class BlackBorderDecrator{
    > 	+dioplay() void
    > 	+setScrollBar() void
    > 	+ScrollBarDecorator(Component)
    > }
    > 
    > ScrollBarDecorator --|> Decorator
    > BlackBorderDecrator --|> Decorator
    > ```
    >
    > ```c++
    > //簡單實現代碼
    > class Component{
    >     public:
    >     	virtual void display(){}
    > }
    > 
    > class Window : public Component{
    >     public:
    >     	void display(){
    >             printf("Window")
    >         }
    > }
    > 
    > class TextBox : public Component{
    >     public:
    >     	void display(){
    >             printf("Textbox")
    >         }
    > }
    > 
    > class ListBox : public Component{
    >     public:
    >     	void display(){
    >             printf("ListBox")
    >         }
    > }
    > 
    > class Decorator : public Component{
    >     private:
    >     	Component* component = nullptr;
    >     public:
    >     	Decorator(Component* component){
    >             this->component = component;
    >         }
    >     
    >     	void display(){
    >            	//just call component dispaly
    >             this->component->display();
    >         }
    > }
    > 
    > class ScrollBarDecorator : public Decorator{
    >     public:
    >     	ScrollBarDecorator(Component* component){
    >             super(component);
    >         }
    >     
    >     	void display(){
    >             this->setScrollBar();
    >             super->display();
    >         }
    >     
    >     	void setScrollBar(){
    >             prinf("設置ScrollBar")
    >         }
    > }
    > class BlackBorderDecorator : public Decorator{
    >     public:
    >         BlackBorderDecorator(Component* component){
    >             super(component);
    >         }
    >     
    >     	void display(){
    >             this->setBlackBorder();
    >             super->display();
    >         }
    >     
    >         void setBlackBorder(){
    >             prinf("設置Black Border")
    >         }
    > }
    > ```
    >
    > ```c++
    > int main(){
    > 	//對抽象類進行編程
    >     Component *component, *componentSB,*componentBB;
    >     component = new Window();
    >     componentSB = new ScrollBarDecorator(component); //對window 進行裝飾
    >     
    >     //decorator 也是Component
    >     componentBB = new BlackBorder(componentSB);//進行2次裝飾
    >    	componentBB->display();
    >     
    >     //BlackBorder call ScrollBarDecorator的display()
    >     //ScrollBarDecorator 會call window 的display()
    > }
    > ```
    >
    > ```c++
    > //結果
    > //設置BlackBorder
    > //設置ScrollBar
    > //Window
    > ```
    >
    > ---
    >
    > **裝飾模式的2種不同的模式**
    >
    > * 透明裝飾模式(Transparent Decorator pattern)
    >   * Client 完全使用抽象進行編程(一致對待構建類以及裝飾類）
    > * 半透明裝飾模式(Semi-Transparent Decorator pattern)
    >   * Client 對構建使用抽象編程,但是對於裝飾類則使用具體進行編程(為了分開調用新增的方法以及原構建的方法)
    >   * **問題:因為具體構建並沒有Override父類的方法,而且再也不是父類的類型，所以無法再進行2次裝飾，注入的對象是抽象類，而不能是具體類**

  * 

