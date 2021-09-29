## 結構型模式

----

* **適配器模式(`Adapter Pattern`)**

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

* 裝飾模式(`Decorator Pattern`)

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
    > **裝飾模式的設計**
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

* 外觀模式(`Facade Pattern`)

  * > 定義:  => 通過一個外觀類(Facade)把子系統(Service)的內部與外部的通訊分割開, 不用與多個子系統打交道(複雜)
    >
    > 為子系統中的一組接口提供一個統一的入口(外部都通過該入口與內部通訊),這入口使子系統更加容易使用。
    >
    > => 能減少類與類之間的耦合,Client 無需跟Subsystem進行交換
    >
    > ---
    >
    > **優點:**
    >
    > * 對Client屏蔽子系統,減少Client處理的對象,使客戶端代碼更簡單
    > * 減少Client與子系統的耦合,子系統改變不會影響Client,調整facade即可
    > *  修改一個子系統對其他系統沒有影響，而且子系統改變不會影響到Facade
    >
    > **缺點:**
    >
    > * 客戶端必須通過Facade 才能與子系統溝通，可能會對客戶端的訪問做太多限制，影響可變性與靈活性
    > * 如果設計不好，新增子系統可能需要修改Facade 的源代碼，會違反`OCP`
    >
    > ---
    >
    > **使用場地:**
    >
    > 1. 當要為複雜的子系統提供單一的入口時，可用
    > 2. Client與子系統之間存在很大的依賴(與多個子系統做互動),可將Client與子系統解耦
    > 3. 在層次化結構中,可通過Facade定義每一層的入口，層與層之間不之間交流/聯繫,減低耦合
    >
    > ---
    >
    > **外觀模式的設計**
    >
    > * 包含2個角色
    >   * Facade(外觀角色): 被Client 調用，調用對應的子系統的功能/職責，從而與子系統溝通
    >   * SubSystem(子系統(內部)): 可以是1個或者多個子系統的Set，這是Set實現了子系統的功能，調用(Client/Facade)子系統的功能從而執行子系統的業務。
    >
    > ---
    >
    > **模式實現:**
    >
    > > 子系統:可以是類/Module/Sub-system/etc...
    >
    > ```mermaid
    > classDiagram
    > class Client{
    > 	
    > }
    > 
    > class Facade{
    > 	
    > }
    > 
    > class SubSystemA{
    > 		
    > }
    > 
    > class SubSystemB{
    > 	
    > }
    > 
    > class SubSystemC{
    > 		
    > }
    > 
    > 
    > Client ..> Facade
    > Facade o-- SubSystemA
    > Facade o-- SubSystemB
    > Facade o-- SubSystemC
    > 
    > ```
    >
    > ```c++
    > //Sub-System
    > class SubSystemA{
    >     public:
    >     	void methodA{}
    > }
    > 
    > class SubSystemB{
    >     public:
    >     	void methodB{}
    > }
    > 
    > class SubSystemC{
    >     public:
    >     	void methodC{}
    > }
    > ```
    >
    > ```c++
    > //Facade
    > class Facade{
    >     private:
    >         SubSystemA* systemA = new SubSystemA();
    >      	SubSystemA* systemB = new SubSystemB();
    >      	SubSystemA* systemC = new SubSystemC();
    > 
    >     public:
    >     	void callMethod(){
    >             systemA->methodA();
    >             systemB->methodB();
    >             systemC->methodC();
    >         }
    > }
    > ```
    >
    > ```c++
    > int main(){
    >     Facade* facede = new Facade();
    >     facede->callMethod()
    > }
    > ```
    >
    > ---
    >
    > **具體例子(讀取文件,加密,解密):**
    >
    > ```mermaid
    > classDiagram
    > class FileReader{
    > 	+Reader(string) string
    > }
    > 
    > class FileWriter{
    > 	+Write(string) void
    > }
    > 
    > class CipherMachine{
    > 	+Encrypt(string) string
    > }
    > 
    > class EncryptFacade{
    > 	-FileReader reader
    > 	-FileWriter writer
    > 	-CipherMachine cipher
    > 
    > 	+EncryptFacade()
    > 	+FileEncrypt(string,string) void
    > }
    > 
    > EncryptFacade o-- FileReader
    > EncryptFacade o-- FileWriter
    > EncryptFacade o-- CipherMachine
    > ```
    >
    > ```c++
    > //Sub system
    > class FileReader{
    >     public:
    >     	string Read(string fileNameSrc){
    >             //Read Plain Text
    >             //return the plain text
    >             return file.str();
    >         }
    > }
    > 
    > class FileWriter{
    >     public:
    >     	void Write(string encryptString,string FileNameDes){
    >             //save encryptString and to the file
    >             printf("saved!");
    >         }
    > }
    > class CipherMachine{
    >     public:
    >     	string Encrypt(string plainText){
    >             printf("encrypting string");
    >             return encryptedStr
    >         }
    > }
    > ```
    >
    > ```c++
    > //facade system
    > class EncryptFacade{
    >     private:
    >     	FileReader *reader = nullptr;
    >     	FileWriter *writer = nullptr;
    >     	CipherMachine *cipher = nullptr;
    >     public:
    >     	EncryptFacade(){
    >           	reader = new FileReader();
    >             writer = new FileWriter();
    >             cipher = new CipherMachine();
    >         }
    > 
    >     	void FileEncrypt(string fileNameScr,string fileNameDes){
    >             string plainText = reader->Read(fileNameScr);
    >             string encryptStr =  writer->Encrypt(plainText);
    >             writer->Write(encryptStr,fileNameDes);
    >         }
    > }
    > ```
    >
    > ```c++
    > //client use facade 
    > int main(){
    >     EncryptFacade * facade = new EncryptFacade();
    >     facade->FileEncrypt("plain.txt","des.txt");
    >     //read the encrypted text from des.txt
    >     //etc...
    > }
    > ```
    >
    > ---
    >
    > > 使用抽象Facade 解決需要修改Facade 違反`ocp`問題 ->一定程度上能解決
    > >
    > > 例子新增一個新的`CipherMachine`
    >
    > **對抽象外觀進行編程**
    >
    > ```mermaid
    > classDiagram
    > class AbstractEncryptFacade{
    > 	<<Abstract>>
    > 	+FileEncrpyt(string src,string dsc) void
    > }
    > 
    > class EncryptFacade{
    > 	-Reader reader
    > 	-Writer writer
    > 	-cipher CipherMachine
    > 
    > 	+EncryptFacade()
    >     +FileEncrpyt(string src,string dsc) void
    > }
    > 
    > EncryptFacade o-- Reader
    > EncryptFacade o-- Writer
    > EncryptFacade o-- CipherMachine
    > 
    > class NewEncryptFacade{
    > 	-Reader reader
    > 	-Writer writer
    > 	-cipher NewCipherMachine
    > 
    >     +FileEncrpyt(string src,string dsc) void
    > }
    > 
    > NewEncryptFacade o-- Reader
    > NewEncryptFacade o-- Writer
    > NewEncryptFacade o-- NewCipherMachine
    > 
    > 
    > EncryptFacade --|> AbstractEncryptFacade
    > NewEncryptFacade --|> AbstractEncryptFacade
    > 
    > class Reader{
    > 	+Read(string src) string
    > }
    > 
    > class Writer{
    > 	+Write(string str,dsc) void
    > }
    > 
    > class CipherMachine{
    > 	+Encrypt(string plainText) string
    > }
    > 
    > class NewCipherMachine{
    > 	+NewEncrypt(string plainText) string
    > }
    > 
    > 
    > 
    > ```

* 享元模式(`Flyweight Pattern`)

  * > 定義(就是分享資源:如果有多個類似或者相同的對象都分享同一個對象) ->具有相同內部狀態的對象對存在享元池(Flyweight Pool)中
    >
    > 2種狀態:
    >
    > * 不變的狀態(內部的狀態(Intrinsic)->共享的部分)：不會隨著環境而變化
    >   * 如:組成一個字符 AVA -> A這個對象是可分享,內部的狀態，都是A
    > * 可變的狀態(外部的狀態(Extrinsic)->不共享的部分) ：會隨著環境而變化
    >   * 如:組成一個字符 AVA，前面的A是紅色,5號字,後面的A是黑色,3號字等,外部狀態,透過注入來改變
    >
    > 運用共享技術有效得支持大量的顆粒度對象的複用。系統只使用少量的對象,使用的對象都很相似,狀態的變化很小,可以實現對象的多次複用。由於享元模式要求能夠共享的對象比較是`顆粒度的對象(很小的對象)`。享元模式又被稱為輕量級模式。
    >
    > ---
    >
    > **優點**
    >
    > * 可以極大的減少內存中的對象屬性,有相同或者相似的對象只有一份(同一的記憶體位置)，從而節省資源
    > * 享元模式對外邊的狀態(ExtrinsicState)是獨立的不會影響到內部的狀態，從而使對象可以在不用的環境下被共享
    >
    > **缺點**
    >
    > * 會使系統變得複雜,享元模式需要分離出內部狀態(Intrinsic`不變`)以及外邊(Extrinsic`因環境改變`),使程序logic變得複雜
    > * 享元模式為了共享對象，需要將部分狀態外部化(可以被改變的狀態)，因為被外部化，該部分的狀態透過注入改變享元對象的外邊狀態,而讀取外邊的狀態會使運行時間變長!
    >
    > ---
    >
    > **使用場地**
    >
    > 1. 如果系統有大量相同或者相似的對象，造成內存大量消耗,可使用享元對象
    > 2. 如果對象大部分的狀態可以被外部化(可以因環境改變而改變狀態),透過注入把狀態傳入到對象中
    > 3. 因為享元模式需要維護享元池,需要消耗系統的一定的資源,需多次使用的享元對象才值得使用享元模式
    >
    > ---
    >
    > **設計例子**
    >
    > ```mermaid
    > classDiagram
    > class FlyWeight{
    > 	<<Abstract>>
    > 	+operation(extrainsicState)
    > }
    > 
    > class ConcreteFlyWeight{
    > 	-intrinsicState
    > 	+operation(extrainsicState)
    > }
    > 
    > class UnsharedConcreteFlyWeight{
    > 	-allState
    > 	+operation(extrainsicState)
    > }
    > 
    > ConcreteFlyWeight ..|>  FlyWeight
    > UnsharedConcreteFlyWeight ..|>  FlyWeight
    > 
    > class FlyWeightFactory{
    > 	-FlyWeight HashMap
    > 	+getFlyWeight(string key) FlyWeight
    > }
    > 
    > FlyWeightFactory o-- FlyWeight
    > 
    > ```
    >
    > > * FlyWeight:抽象享元對象declared 具體享元對象公共的方法
    > >
    > > * ConcreteFlyWeight : 具體的享元對象,存儲了內部狀態(intrinsic state)，通常會結合單例模式共產同一對象
    > >
    > > * UnSharedConcreteFlyWeight : 不提供分享的享元對象，可以直接實例化(該類不提供分享)
    > >
    > > * FlyWeightFactory: 享元對象工廠，該工廠保存了一個HashMap作為享元池(FlyWeight Pool)，Client通過key存取享元對象，如果池子中沒有則新建新的對象, 加入池子放回給client。
    >
    > ```c++
    > class FlyWeightFactory{ //享元對象工廠例子
    >     //定義一個HashMap 作為key-value pattern 的享元池
    >     private:
    >     	HashMap<string,*FlyWeight> flyWeightPool;
    >     public:
    >     	FlyWeight getFlyWeight(string key){
    >             if(flyWeightPool.find(key) != flyWeightPool.end()){
    >                 return flyWeightPool.find(key)->second;
    >             }else{
    >                FlyWeight* fw = new FlyWeight();
    >                 flyWeightPool.insert(pair<string,*FlyWeight>(key,fw));
    >                 return fw;
    >             }
    >         }
    > }
    > ```
    >
    > > 享元模式的狀態(內部/外部)分開處理
    > >
    > > 內部:作為對象的成員
    > >
    > > 外邊:通過注入的方式添加到享元對象中
    >
    > ```c++
    > class FlyWeight{
    >     private: 
    >     	string intrinsicState; //同一個享元對象有相同的內部狀態
    >     public:
    >     	FlyWeight(string intrinsicState){
    >             this->intrinsicState = intrinsicState;
    >         }
    > 
    >     	//而外邊狀態不會保存在享元對象中
    >     	//在使用該對象時傳入不同的外邊狀態進行配置
    >     	void operation(string extrinsicState){
    >             //TODO 
    >             //setting Up....
    >             //....
    >         }
    > }
    > ```
    >
    > ---
    >
    > **具體例子(棋子)**
    >
    > ```mermaid
    > classDiagram
    > class IgoChess{
    > 	<<Abstract>>
    > 	+getColor() string
    > 	+display() void
    > }
    > 
    > class BlackChess{
    > 	+getColor() string
    > }
    > 
    > class WhiteChess{
    > 	+getColor() string
    > }
    > 
    > BlackChess --|> IgoChess
    > WhiteChess --|> IgoChess
    > 
    > class IgoChessFactory{
    > 	-IgoChessFactory instance
    > 	-HashMap chessPool
    >     +IgoChessFactory()
    >     +getInstance() IgoChessFactory
    >     +getIgoChess(string key)IgoChess
    > }
    > 
    > IgoChessFactory o-- IgoChess
    > IgoChessFactory <-- IgoChessFactory
    > 
    > 
    > ```
    >
    > ```c++
    > class IgoChess{
    >     public:
    >     	virutal string getColor() = 0;
    >     	void display(){
    >             printf("chess color is %s!",this->getColor());
    >         }
    > }
    > 
    > class BlackChess : public IgoChess{
    >     public:
    >     	string getColor(){
    >             return "black"
    >         }
    > }
    > 
    > class WhiteckChess : public IgoChess{
    >     public:
    >         string getColor(){
    >             return "white"
    >         }
    > }
    > ```
    >
    > ```c++
    > //Singleton pattern
    > class IgoChessFactory{
    >     private:
    >     	static HashMap<string,IgoChess> ht;
    >     	static IgoChessFactory* Ins = new IgoChessFactory();
    >     	IgoChessFactory(){
    >             //init the hash map
    >            	IgoChess* whiteChess = new WhiteckChess();
    >             IgoChess* blackChess = new BlackChess();
    > 
    >             ht.insert(pair<string,IgoChess>("Black",blackChess));
    >             ht.insert(pair<string,IgoChess>("White",whiteChess));
    >         }
    > 
    >     public:
    >     	static IgoChessFactory* getIgoChessFactoryInstace(){
    >             return this->Ins;
    >         }
    > 
    >     	static IgoChess* getIgoChess(string key){
    >             return (IgoChess)ht.find(key);
    >         }
    > }
    > ```
    >
    > ```c++
    > //client
    > int main(){
    >     IgoChess *black1,*black2,*white1,*white2;
    >     IgoChessFactory* factory;
    > 
    > 	factory = IgoChessFactory.getIgoChessFactoryInstace();
    > 
    >     black1 = factory->getIgoChess("black");
    >     black2 = factory->getIgoChess("black");
    >     //share same object
    >     cout << "is same : " << black1 == black2 << endl; //is same :  true
    > 
    >     white1 = factory->getIgoChess("white");
    >     white2 = factory->getIgoChess("white");
    >     //share same object
    >     cout << "is same : " << white1 == white2 << endl; //is same :  true
    > 
    >     black1->display();// chess color is black
    >     black2->display();// chess color is black
    >     white1->display();// chess color is white
    >     white2->display();// chess color is white
    > }
    > ```
    >
    > ---
    >
    > **以上例子只有共享,不能設置其他狀態**
    >
    > > 加入另外一個類,用於設置外部狀態
    >
    > ```c++
    > //add coordinates:
    > class Coordinates{
    >     private:
    >     	int x;
    >     	int y;
    >     public:
    > 		Coordinates(int x, int y){
    >             this->x = x;
    >             this->y = y;
    >         }
    > 
    >     	//setter
    >     	void setX(int X){
    >             this->x = X;
    >         }
    > 
    >     	void setY(int Y){
    >             this->y = Y;
    >         }
    > 
    >     	//getter
    >     	int getX(){
    >             return this->X;
    >         }
    > 
    >     	int getY(){
    >             return this->Y;
    >         }
    > }
    > 
    > //把該Object(外部狀態)注入到享元對象中,顯示狀態
    > class IgoChess{
    >     public:
    >     	virutal string getColor() = 0;
    >     	void display(Coordinates* coordinates){
    >             printf("chess color is %s! And its coordinates x is %d and y is %d",this->getColor(),coordinates->getX(),coordinates->getY());
    >         }
    > }
    > ```
    >
    > ```c++
    > //client
    > int main(){
    >     IgoChess *black1,*black2,*white1,*white2;
    >     IgoChessFactory* factory;
    > 
    > 	factory = IgoChessFactory.getIgoChessFactoryInstace();
    > 
    >     black1 = factory->getIgoChess("black");
    >     black2 = factory->getIgoChess("black");
    >     //share same object
    >     cout << "is same : " << black1 == black2 << endl; //is same :  true
    > 
    >     white1 = factory->getIgoChess("white");
    >     white2 = factory->getIgoChess("white");
    >     //share same object
    >     cout << "is same : " << white1 == white2 << endl; //is same :  true
    > 
    >     //一樣是位置相同,共享Object 當時狀態不一樣！
    >     black1->display(new Coordinates(1,2));// chess color is black!And its coordinates x is 1 and y is 2
    >     black2->display(new Coordinates(3,4));// chess color is black!And its coordinates x is 3 and y is 4
    >     white1->display(new Coordinates(4,3));// chess color is white!And its coordinates x is 4 and y is 3
    >     white2->display(new Coordinates(2,1));// chess color is white!And its coordinates x is 2 and y is 1
    > }
    > ```
    >
    > ---
    >
    > **復合享元模式**
    >
    > * 2種享元模式
    >
    >   * 單純享元模式
    >
    >     * 所有具體的享元類都是可以分享的，不包含不可分享的享元類
    >
    >     * ```mermaid
    >       classDiagram
    >       class FlyWeight{
    >       	<<Abstract>>
    >       	+operation(extrinsicState)
    >       }
    >                   
    >       class ConcreFlyWeight{
    >       	-intrinsicState
    >       	+opreation(extrinsicState)
    >       }
    >       ConcreFlyWeight --|> FlyWeight
    >                   
    >       class FlyWeightFactory {
    >       	-HashMap<FlyWeight> ht
    >       	+getFlyWeight(string key) FlyWeight
    >       }
    >                   
    >       FlyWeightFactory o-- FlyWeight
    >       ```
    >
    >   * 復合享元模式
    >
    >     * 復合享元類不能被分享(將單純的享元類使用組合模式組合,形成復合享元對象)
    >
    >     * 復合享元類一樣是抽象享元類(包含了多個單純享元類(每個享元類都具有不同的狀態，可以為他們設置相同的外部狀態))
    >
    >     * ```mermaid
    >       classDiagram
    >       class FlyWeight{
    >       	<<Abstract>>
    >       	+operation(extrinsicState)
    >       }
    >                         
    >       class ConcreFlyWeight{
    >       	-intrinsicState
    >       	+opreation(extrinsicState)
    >       }
    >       ConcreFlyWeight --|> FlyWeight
    >                         
    >       class FlyWeightFactory {
    >       	-HashMap<FlyWeight> ht
    >       	+getFlyWeight(string key) FlyWeight
    >       }
    >                         
    >       FlyWeightFactory o-- FlyWeight
    >       CompositeConcreteFlyWeight --|> FlyWeight
    >       CompositeConcreteFlyWeight o-- FlyWeight
    >                         
    >       class CompositeConcreteFlyWeight{
    >       	 -FlyWeightflyweights 
    >       	 +operation(extrinsicState)
    >       	 +add(FlyWeight)
    >       	 +remove(FlyWeight)
    >       }
    >                         
    >       ```
    >
    > ---
    >
    > **享元模式與其他模式連用**
    >
    > * 工廠模式:用於生產享元對象
    > * 單例模式: 一個系統通常只有1個享元工廠
    > * 組合模式： 享元模式結合組合模式可以形成復合享元模式,可以統一對多個不同內部狀態的享元模式設置外部狀態

* 代理模式(`Proxy pattern`)

  * > 定義:(Client 與 對象中插入另外一個對象)
    >
    > 給某一個對象提供一個代理或者占位符(一個代理的東西),並由代理對象控制原來對象的訪問
    >
    > `透過代理對象使Client 間接訪問真實的對象，起到中介的作用`
    >
    > > 讓Client 一致對待代理對象以及真實的對象，因此,可以使用抽象編程
    >
    > ---
    >
    > **優點:**
    >
    > * 能夠協調協調者(Client)和被協調者(業務對象),一定程度上減低系統的耦合度(2者不是直接進行溝通)
    > * Client可以針對抽象進行編程,對客戶端隱藏代碼對象以及業務對象，而且對於新增或者刪除,甚至是更換代碼對象都無需修改源碼(透過配置文件取代)，符合`ocp`,系統有更好的靈活性以及可擴展性
    > * 不同的代理模式有不同的優點
    >   * 遠程代理(作為代理不同記憶體位置的調用)
    >   * 虛擬代理(作為代理高消耗對象的暫時替代)
    >   * 緩衝代理(作為操作結果暫時的緩存)
    >   * 保護代理(作為控制對象的訪問)
    >   * 智能引用代理(作為對象應用)
    >
    > **缺點:**
    >
    > * 要在Client 與 真實對象之間加入一個代理對象，可能會讓處理變慢，如包含代理模式，會造成處理速度變慢
    > * 實現某些代碼模式比較複雜，而且實現代碼模式需要額外的工作，如`remote proxy`
    >
    > ---
    >
    > **使用場地:**
    >
    > > 對於不同情況可以使用不同的代理模式
    >
    > 1. Client 對象需要訪問遠端主角的對象 -> `Remote Proxy`
    > 2. 當對象需要消耗較多的對象時,需要一個消耗資源較小的資源時候,可以使用 -> `Virtual Proxy`
    > 3. 當某些操作被頻繁訪問,可以把結果進行暫存,相同的操作只需放回緩存中的結果即可 -> `Cache Proxy`
    > 4. 當需要操作一個對象的訪問權(需要驗證等等)，為不用用戶提供不同的訪問 -> `Protect Proxy`
    > 5. 當需要為一個對象的訪問(reference)提供一些額外的操作時，可以使用 -> `Smart Reference Proxy`
    >
    > ---
    >
    > **設計例子**
    >
    > ```mermaid
    > classDiagram
    > class Client{
    > 	
    > }
    > 
    > class Obejct{
    > 	+request() void
    > }
    > 
    > class Proxy {
    > 	-ProxyRealObject oj
    > 	+prequest() void
    > 	+request() void
    > 	+postRequest() void
    > }
    > 
    > class ProxyRealObject{
    > 	+request() void
    > }
    > 
    > ProxyRealObject ..|> Obejct
    > Proxy ..|> Obejct
    > 
    > Proxy o-- ProxyRealObject
    > Client --> Proxy
    > 
    > ```
    >
    > 
    >
    > > Description(真實設計比這複雜很多)
    > >
    > > * Client : 客戶端
    > >
    > > * Object : 抽象對象,Declared 了主題和代碼主題的共同接口，讓任何主題的地方都可以使用代理主題
    > >
    > > * Proxy : 代理角色,包含了對真實主題(需代碼的對象)的引用,從而可以對真實注意進行操作，而且可以在對真實主題前/後做其他操作。因為代理對象中提供與真實主題角色相同的接口，所以可以進行替換
    > >
    > > * ProxyRealObject:真實主題的角色,代表真實角色的對象,會實現真的操作業務，客戶端通過帶來代碼角色間接調用真實角色中定義的操作。
    >
    > ```c++
    > class Object{ //抽象類 代理對象以及業務對象 實現
    >     public:
    >     	virtual void request() = 0; 
    > }
    > ```
    >
    > ```c++
    > class ProxyRealObject : public Object{
    >     public:
    >     	override void request(){
    >             //to do for current object
    >         }
    > }
    > 
    > class Proxy : public Object{
    >     private:
    >     	ProxyRealObject* object = new ProxyRealObject();// reference
    >     public:
    >     	void preRequest(){
    >             //to do preRequst
    >         }
    >     
    >     	void postRequest(){
    >             //to do postRequst
    >         }
    >     
    >     	override void request(){
    >             preRequest(); //before call obejct
    >             //to do some service
    >             object->request();
    >             
    >             postRequest(); //before call obejct
    >         }
    > 
    > }
    > 
    > //client -> proxy -> proxtObject
    > ```
    >
    > ---
    >
    > **常用的代理模式**(設計很複雜~~)
    >
    > * 遠程代理(`Remote Proxy`) -> `Ambassador(大使)` ->作為不同記憶體位置對象代理的者
    >
    >   * > 為一個位於不同記憶體空間的對象提供一個本地的代理對象
    >     >
    >     > 不同的記憶體空間可以是同一台主機，也可以是另一台主機
    >
    > * 虛擬代理(`Virtual Proxy`) ->作為代理生成對象的代理者
    >
    >   * > 如果需要Create 一個資源消耗較大的對象,可以先就愛你了一個相對較小的對象表(代理者)，真正要被創建的對象,在用到的時候才會被真正的建立
    >
    > * 保護代理(`Protect Proxy`) ->作為控制訪問的代理者
    >
    >   * > 控制對一個對象的訪問(作為代理給不同用戶不用的訪問權限),可以給不同的用戶提供不同級別的使用權
    >
    > * 緩衝代理(`Cache Proxy`) ->作為提供臨時儲存空間的代理者
    >
    >   * > 為某一個目標操作的結果提供臨時的儲存空間(把操作結果暫存,如有相同的操作,透過代理者提供分享),以便多個客戶端可以共享這些結果
    >
    > * 智能應用代理(`Smart Reference Proxy`) ->作用對象引用的代理者
    >
    >   * > 當一個對象被引用時，提供一些額外的操作(如對象被調用的次數等等)
    >
    > ---
    >
    > **實際例子**(在原有業務上新增新的功能,驗證,log 等等)
    >
    > > 透過代理模式間接訪問真實對象,在代理模式中新增新的功能
    > >
    > > 代碼對象作為保護代理(驗證->才能訪問真實的對象)
    > >
    > > 代碼對象作為智能引用代理(調用引用對象錢做額外的操作)
    >
    > ```mermaid
    > classDiagram
    > class Searcher{
    > 	<<Abstract>>
    > 	+doSearch(string id,stirng keyword) string
    > }
    > 
    > class ProxySearcher{
    > 	-Logger log
    > 	-Validator vaildator
    > 	+doSearch(string id,stirng keyword) string
    > 	+log(string id) void
    > 	+validate(string id) bool
    > }
    > 
    > class RealSearcher{
    > 	+doSearch(string id,stirng keyword) string
    > }
    > 
    > ProxySearcher ..|> Searcher
    > RealSearcher ..|> Searcher
    > 
    > ProxySearcher o-- Validator
    > ProxySearcher o-- Logger
    > ProxySearcher o-- RealSearcher
    > 
    > class Validator{
    > 	+validata(string id) bool
    > }
    > 
    > class Logger{
    > 	+log(string id) void
    > }
    > ```
    >
    > ```c++
    > class Search{
    >     public:
    >     	virtual string doSearch(string id,string keyword) = 0
    > }
    > 
    > class RealSearch : public Search{
    >     public:
    >     	 string doSearch(string id,string keyword){
    >              //to do search
    >          }
    > }
    > 
    > class ProxySearch : public Search {
    >     private:
    >     	RealSearch *realObj = new RealSearch();// keep the reference
    >     	Validate *validator;
    >     	Logger *logger;
    >     public:
    >     	bool validate(string id){
    >             //to do validate
    >             validator = new Validator();
    >             return validator->Validate(id)
    >         }
    >     
    >     	void log(string keyword){
    >             //to write logger
    >             logger = new Logger();
    >             logger->log(id);
    >         }
    >     
    >     	string doSearch(string id,string keyword){
    >             //to do search with reference
    >             if(validate(id)){ //check is validated
    >                 //search
    >                 string result = realObj->doSeach(id,keyword);
    >                 this->log(id);
    >                 return result;
    >             }else{
    >                 return null;
    >             }
    >         }
    > }
    > ```
    >
    > ```c++
    > //validator and logger
    > class Validator {
    >     public:
    >     	bool Validate(string id){
    >             printf("Validating database with userId %s...",id);
    >             if(vaildateConditionPass){
    >                 printf("UserID %s passed the validation!Welcome!",id);
    >                 return true;
    >             }else{
    >                 return false;
    >             }
    >         }
    > }
    > 
    > class Logger{
    >     public:
    >     	void log(string id){
    >             printf("database updated with userID %s",id)
    >         }
    > }
    > ```
    >
    > ```c++
    > int main(){
    >     Searcher* searcher;
    >     seacher = new ProxySearch();
    >     
    >     //using proxy to validate and search with realObj
    >     string resulty = seacher->doSearch("id","keyword");
    >     printf("%s",result);
    > }
    > ```
    >
    > > 如果要新增新的機制，只需新增新的代理類，即可，符合`ocp`
    >
    > ---
    >
    > **常用的代理模式**
    >
    > * `Remote Proxy` :在本地端建立遠程代理對象，遠程代理對象會進行網絡相關的業務去調用遠端業務的對象,對客戶端而已是隱藏的，客戶端無需關心
    >
    > * `Virutal Proxy` :  對於佔用系統資源或者加載時間過長的對象，可以使用`Virtual Proxy`在真實Object創建完成之前，由`Virtual Proxy`作為替身,而當真實對象創建對象建立之後,再將`Virtual Proxy `再請求給真實的對象
    >
    >   * 使用Virtual Proxy的實際情況(可考慮使用) ->`可以提高系統的性能`
    >     * 對象`本身很複雜(建立時間很久)或者網絡等原因`使得加載時間很長，可以使用Virutal Proxy(加載時間較短) 來代替真正的對象。可使用多線程來處理(1個線程用於顯示代碼對象,其他線程用於加載)。->使用代理模式可以大大減少程序啟動時間(init 代理對象時間相對較短)->在使用代碼對象引用真實對象(如果已經加載完成)
    >     * 當一個`對象加載十分消耗系統資源`(建立需要佔用大量記憶體)時,可使用代理模式延遲建立(當要用到的時候才建立),而且使用代理對象,代理真實的對象 ,可讓重複的對象重複被利用，從而減少浪費資源，但是每次存取都需要檢測真實的對象是否已存在嗎,會需要一定的系統時間(以時間換取空間)。
    >
    > * `Cache Proxy`: 為操作結果提供暫時的緩存，讓這些結果可以共享,可避免重複執行，優化系統性能
    >
    >   * `Microsoft 範例` -> 新增`ProductDataProxy` 為 `product`業務提供緩存
    >
    >     * > 代理對象 提供跟真實對象的方法,對客戶端而已是沒有區別的
    >
    >     * ```mermaid
    >       classDiagram
    >       class Client{
    >       	<<Client>>
    >       }
    >       
    >       class ProductDataProxy{
    >       	-int productTimeout$
    >       	-bool cache$
    >       	+GetProductByCategory(string category) lList
    >       	+GetProductsBySearch(string text) lList
    >       	+GetProduct(string productId) ProductInfo
    >       }
    >       
    >       class Product{
    >       	+GetProductByCategory(string category) lList
    >       	+GetProductsBySearch(string text) lList
    >       	+GetProduct(string productId) ProductInfo
    >       }
    >       Client --> ProductDataProxy
    >       ProductDataProxy --> Product
    >       ```
    >
    >     * ```c++
    >       //簡單範例
    >       static class ProductDataProxy{
    >           private:
    >           	static int productTImeOut = ToConfigTheSetting();
    >           	static bool enableCache = ToConfigTheCache();
    >           
    >          	public:
    >           	static IList GetProductByCatrgory(string category){
    >                   Product *product = new Product();
    >                   
    >                   //如果cache 沒有啟用 之間調用objec
    >                   if(!enableCache){
    >                      return product->GetProductByCatrgory(category);
    >                   }
    >                   
    >                   //透過cache
    >                   string cacheKey = "cache" + category;
    >                   
    >                   //a simulated
    >                   IList* data = (*IList)cache.Get(cacheKey);
    >                   if(data == nullptr){
    >                       //data not in cache
    >                      data = product->GetProductByCatrgory(category);
    >                       //TODO 
    >                       //把建立好的存到cache
    >                       //...
    >                   }
    >                   return data;
    >               }
    >           	
    >           	//.....
    >       }
    >       ```
    >
    > ---
    >
    > 

