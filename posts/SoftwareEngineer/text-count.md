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


    
>       * 用数组进行统计不太方便,要么会浪费资源,要么要进行动态的构造
>
        >> _以上两点决定了该方法的效率低下._
        
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
        <img src="/sources/wordCount.jpg"/>
        <img src="/sources/cpu_usage.PNG"/>
        <img src="/sources/deamon_threads.PNG"/>
        <img src="/sources/heap_usage.PNG"/>
        <img src="/sources/load.PNG"/>