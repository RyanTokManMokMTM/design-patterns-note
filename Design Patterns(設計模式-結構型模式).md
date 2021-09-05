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
    > > **缺省適配器模式(Default Adapter Pattern)**，就是定義一個很多方法的Interface，然後透過Abstract class 去實現並提供默認的實現(可能是空的)，然後讓該抽象類的Sub class 透過`override`的方式提供某些方法的實現
    > >
    > > 這個模式可以讓繼承的子類只實現自己想要的方法，其他方法都為默認實現(空方法)

* 橋接模式(`Bridge Pattern`)

> 定義(將多個維度分離成獨立的維度-解決多層繼承的互相影響的問題):
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
    >
    >---
    >
    >**優點**
    >
    >**缺點**
    >
    >---
    >
    >> 簡單的組合模式的例子
    >
    >
    >
    >```mermaid
    >classDiagram
    >   class Component{
    >       <<Absturct>>
    >       +add(Component) void
    >       +remove(Component) void
    >       +getChild(int) Component
    >       +operation() void
    >   }
    >
    >   class Leaf{
    >   	+add(Component) void
    >       +remove(Component) void
    >       +getChild(int) Component
    >       +operation() void
    >   }
    >
    >   class Container {
    >   	+add(Component) void
    >       +remove(Component) void
    >       +getChild(int) Component
    >       +operation() void
    >   }
    >
    >   Leaf ..|> Component
    >   Container  ..|> Component
    >   Container  o-- Leaf
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
    >    public:
    >    
    >        virtual void add(AbstractFile file) = 0;
    >        virtual void remove(AbstractFile file) = 0;
    >        virtual AbstractFile get(int i) = 0;
    >    	virtual void killVirus() = 0;
    >}
    >
    >//Leaf
    >class ImageFile : public AbstractFile{
    >    private:
    >    	std::string name;
    >    public:
    >    	ImageFile(std::string name){
    >        	this->name = name 
    >        }
    >    
    >    	void add(AbstractFile file){
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        void remove(AbstractFile file){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        AbstractFile get(int i){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >    	void killVirus() {
    >            printf("對該圖形%s進行殺毒操作",this->name);
    >        };
    >}
    >class VideoFile : public AbstractFile{
    >    private:
    >    	std::string name;
    >    public:
    >    	VideoFile(std::string name){
    >            this->name = name;
    >        }
    >    	void add(AbstractFile file){
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        void remove(AbstractFile file){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        AbstractFile get(int i){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >    	void killVirus() {
    >            printf("對該影片%s進行殺毒操作",this->name);
    >        };
    >}
    >class TextFile : public AbstractFile{
    >    private:
    >    	std::string name;
    >    public:
    >        TextFile(std::string name){
    >            this->name = name
    >        }
    >    
    >    	void add(AbstractFile file){
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        void remove(AbstractFile file){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >        AbstractFile get(int i){ 
    >            //沒有其他Node，而且不是folder
    >            printf("抱歉,此文件不支援")
    >        };
    >    
    >    	void killVirus() {
    >            printf("對該文本%s進行殺毒操作",this->name);
    >        };
    >}
    >
    >//contrainer
    >class Folder : public AbstractFile{
    >    //因為是Folder是可以包含其他leaf的或者是其他Folder
    >    //使用一個list 來保存他的sub tree node
    >    private:
    >    	//假設有這個list 結構
    >    	list<AbstractFile> *filesList = new list<AbstractFile>();
    >    	std::string name;
    >    public:
    >    	Folder(std::string name){
    >            this->name = name
    >        }
    >    
    >       	void add(AbstractFile file){
    >			filesList->add(file);
    >        };
    >    
    >        void remove(AbstractFile file){ 
    >			filesList->remove(file);
    >        };
    >    
    >        AbstractFile get(int i){ 
    >			return AbstractFile(filesList->get(i));
    >        };
    >    
    >    	void killVirus() {
    >           //要對文件夾進行殺毒,就得對該文件夾下的所以文件以及內部的所有文件夾的文件進行殺毒
    >           //對保存在這個node 下的文件list 調用這些文件的 killVirus()
    >           for(auto file : filesList){
    >               file->killVirus(); //如果當前文件是個文件夾就會一直recursive
    >           }
    >        };
    >    
    >    	
    >}
    >```
    >
    >```c++
    >//Client 只需對AbstractFile 繼續編程即可
    >//組合模式符合OCP
    >int main(){
    >    AbstractFile *file1,*file2,*file3,*file4,*file5,*folder1,*folder2,*folder3,*folder4;
    >    
    >    //create folder
    >    folder1 = new Folder("Jackson data");
    >    folder2 = new Folder("Image Files");
    >    folder3 = new Folder("Video Files");
    >    folder4 = new Folder("Text Files");
    >    
    >    file1 = new ImageFile("a.jpg");
    >    file2 = new ImageFile("b.jpg");
    >    file3 = new VideoFile("a.mp4");
    >    file4 = new VideoFile("b.mp4");
    >    file5 = new TextFile("x.txt");
    >    
    >    folder2->add(file1);
    >	folder2->add(file2);
    >	folder3->add(file3);
    >	folder3->add(file4);
    >	folder4->add(file5);
    >	folder1->add(folder2);
    >	folder1->add(folder3);
    >    folder1->add(folder4);
    >    
    >    //folder1 是root folder
    >    folder->killVirus();//會進行recusive 調用
    >}
    >```
    >
    >---
    >
    >> 以上有個問題
    >>
    >> **就是Leaf不會有其他Node，也就是必須要實現Abstract中的方法包括Add Remove 的等等，對於Leaft而已就很多餘了**
    >>
    >> 解決方法有2個
    >>
    >> * 透明組合模式(以上例子的方法)-這個方法不安全，因為沒必要或者不應該存在的方法也實現了
    >> * 安全組合模式(就是Abstract 只Declared 有必要的方法，其他的就是實體類自行去實現。問題是client 就必要要對實體類進行編程，無法通過抽象類進行編程)-不夠透明(必須對實體類編程)

* 

