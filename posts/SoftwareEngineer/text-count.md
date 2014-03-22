# 文本中的字符串计数问题


------------------------------------------

##问题回顾(Describle)：##

>写一个程序，分析一个文本文件中各个词出现的频率，并且把频率最高的10个词打印出来。文本文件大约是30KB~300KB大小。

-------------------------------------

##问题分析(Analysis)：##

>1.  可能涉及到得一些方法：
    * 文件读写
    * 字符串处理
    * 排序 
>2.  开发平台：
    * Java
>3.  测试工具：
    * VS

--------------------------------
##设计(Design)：##
>+ 第一次尝试
>  1.设计一个 *WordCount* 类

>                  public class WordCount implements Comparable{
>                       //实现Comparble接口
>
                        private String word;  
>                        
                        private int count;
>
                        public String getWord(){
                        >
                        }
                        >
                        public int getCount(){
                        >
                        }
                        >
                        public compareTo(WordCount wc){
                        >
                        }
                }
>   2.设计一个 *Scanner* 类       
>
        public class Scanner() implements{
        >
            //Scan the file and process it; 
        >
            private  WordCount[] wcs;
        >
            public WordCount[]  getWordList(){
        >
                return wcs;
            }
        >
            public void fileProcessor(String file){
            >
                //接受文件名并处理相关文件,将分开的单词保存到 wcs 里面
            >
            }
        } 
>
>   3.设计 *test* 函数调用
>
>   4.不足之处：
>           
>       * 在设计Scanner时,由于设及到对word的统计,每扫描一个单词都要先检查数组中是否已存在          该对象,并进行相应的处理.
>
>
>   
>       * 用数组进行统计不太方便,要么会浪费资源,要么要进行动态的构造
>
>>  _以上两点决定了该方法的效率低下._
        
>+ 第二次尝试：
>
>
>   1. 采用 **HashMap**来代替之前的**WordCount**采用**API**提供的接口的程序效率要更高一点


>   
    2. 用**List**类保存 **HashMap**的值,方便与最后对整个数组的排序
    
>               
            public class newScanner {
>
            public static List<> processInput(String path)throws IOException{
>        
>    	        //open file
>               File file = new File(path);
                FileInputStream fis = new FileInputStream(file);
                InputStreamReader isr = new InputStreamReader(fis);
>
                int thechar;		//hold the inputchar
>        
>               StringBuffer sb = new StringBuffer();	//Buffered the char to 
                                                        composite a word                >                                            
>       
>               HashMap<String, Integer> wordList = new HashMap<String, Integer>(); 
                                            // Hold the set of word pair
>        
>               while ((thechar = isr.read()) != -1) {
                char letter = (char) thechar;
                if ((letter >= 'a' && letter <= 'z')
                    || (letter >= 'A' && letter <= 'Z')) {
                sb.append(letter);
                } else if (sb.length() != 0) {
                String theword = new String(sb);
                if (wordList.containsKey(theword)) {
                    wordList.put(theword, wordList.get(theword) + 1);
                } else {
                    wordList.put(theword, 1);
                }
                sb.delete(0, sb.length());
                }
            }
            isr.close();
>            
>       
            List<Map.Entry<String, Integer>> words = new ArrayList<Map.Entry<String, Integer>>(
                wordList.entrySet());
>        
            Collections.sort(words, new Comparator<Map.Entry<String, Integer>>() {
>
            @Override
            public int compare(Entry<String, Integer> o1,
                    Entry<String, Integer> o2) {
                return -(o1.getValue() - o2.getValue());
            }
            });
            return words;
            }
>
>   3. 设计Test类进行测试
>
        >> _首先,在查阅了 **Java** 相关 **API** 以后,确定了本次的数据结构,明确了一些相_
            _关的接口.利用 **HashMap** 实现对单词的录入,利用 **Collection** 对统计值进_
            _行排序,最后输出_
            
---------------------------------------
+ 测试函数：

        public class test {
	        public static void main(String args[]) throws Exception{
	        
	        Scanner sc = new Scanner(System.in);
	
	    //Get path of file
	        System.out.println("Please Input The filename That you want to Analysis:");
	        String path;
	        String tmp;
	        tmp = sc.nextLine();
	        if(tmp=="\n"){
	       	path = tmp;
	        }else{
		    path ="test.txt";
	        }
	        sc.close();
>	
>	
	    //System.out.println(path); 
    	    List<Map.Entry<String,Integer>>ls = newScanner.processInput(path);
	        System.out.println("读取的文件中出现频率最多的十个单词是：");
            int i = 0;
            for (Map.Entry<String, Integer> node : ls) {
                if (i < 10) {
                    System.out.println(node.getKey() + " : " + node.getValue());
                } else {
                break;
                }
             i++;
            }
    	    }
         }
+ 测试结果：

![输出](https://raw.github.com/Booob/blog/master/sources/wordCount.jpg)


![Threads](https://raw.github.com/Booob/blog/master/sources/deamon_threads.png)

    守护线程和活动线程都在时间处发生了变化.守护线程在程序结束后又回归原来的状态,活动线程在达到定点后回归原来的变化状态.

![Heap_Usage](https://raw.github.com/Booob/blog/master/sources/heap_usage.png)

    堆栈使用情况:可以很明显的看出在 12:56:20  秒后堆栈使用增加,包括类·对象·以及文件的载入使用的堆栈.测试文件并不大 只有675.8K,由此来看堆栈的使用主要在于类的加载以及各对象的调用对堆栈的使用.在程序设计中有必要去有意识减少类的使用,不能滥用类,无限制的递归调用,这对系统性能也是一个瓶颈

![Load_Class](https://raw.github.com/Booob/blog/master/sources/load.png)

    因为本程序涉及得类并非很多,所以在  12:56:20s 左右类的数量并不是很明显

![Cpu_Usage](https://raw.github.com/Booob/blog/master/sources/cpu_usage.png)

    从分析工具得出的结果来看:Cpu利用率在12:56:20s时开始具体变化,短短的时间内,产生两次峰值.第一次峰值较低,应当是在做一些处理文件的操作,IO处理时CPU利用率下降,到开始进行文本串的处理时达到峰值.由此看出,一个程序的关键在于其核心算法的效率. 




-------------------------------------------------

测试文件地址:

[Click][http://yunpan.cn/Q47IQFaUSTE7D] To Get The Test File!

-------------------------

>   JokieZhang 2014.3.16. -----初稿
>              2014.3.21  -----修改