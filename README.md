### **File** **I/O**



### **File** **I/O** 실습

```java
package com.lgcns.test;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.nio.file.FileSystems;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;
import java.nio.file.StandardWatchEventKinds;
import java.nio.file.WatchEvent;
import java.nio.file.WatchKey;
import java.nio.file.WatchService;
import java.time.LocalDateTime;


public class RunManager {
	static String rootPath = "C:/Input";
	public static void main(String[] args) throws InterruptedException, IOException {
		 FileSearchAll(rootPath);
		 
		 //3번
		 
		 File f = new File("C:\\LogGenerator\\LOG.TXT");
			while (!f.exists() ) {//파일 존재 유무에 따라 얼어줌
				Thread.sleep(100);
				continue;
			}
			
			BufferedReader br = new BufferedReader(new FileReader("C:/LogGenerator//LOG.TXT"));
			while (true) {//이렇게 두면 업데이트해서 계속 읽나봄....
				String line = br.readLine();//한라인씩 읽기
				if (line == null) {
					Thread.sleep(1);
					continue;
				}
				//[2024-05-07 오전 11:07:57] - 19 20 읽어서 19+20 더해주는 코드

				String [] words = line.split(" - ");
				String [] strNums = words[1].split(" ");
				int sum = Integer.parseInt(strNums[0]) + Integer.parseInt(strNums[1]);	
				System.out.println(String.format("[%s] %d", LocalDateTime.now(), sum));
			}

	}
	
	static void FileSearchAll(String path){
		File directory = new File(path);
		File[] fList = directory.listFiles();
		for (File file : fList) {
				if (file.isDirectory()) {
					//System.out.print(file.getPath());
					FileSearchAll(file.getPath());
				}
				else {
		        	String partPath = path.substring(rootPath.length());
		            System.out.println("." + partPath + "\\" + file.getName());
					//System.out.println(file.getPath() +":"+file.length()+"bytes.");
					if(file.length()>3*1024) {
						MyCopyFile(file.getPath(),file.getName());
						/*
						try {
							Path sourcePath = Paths.get(file.getPath());
							Path destinationPath = Paths.get("C:/OUTPUT\\"+file.getName());
							Files.move(sourcePath, destinationPath, StandardCopyOption.REPLACE_EXISTING);
							System.out.println("File moved successfully!");
						} catch (Exception e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}
						*/
					}
				}
		}
	}

	private static void MyCopyFile(String partPath, String filename) {
		final int BUFFER_SIZE = 512;
		
		int readLen;
		try {
			// Create Folder
    		File destFolder = new File(".\\OUTPUT" + partPath);
    		if(!destFolder.exists()) {
    			destFolder.mkdirs(); 
    		}        	
        	
    		// Copy File
            InputStream inputStream = new FileInputStream(".\\INPUT"+partPath+"\\"+filename);
            OutputStream outputStream = new FileOutputStream(".\\OUTPUT"+partPath+"\\"+filename);
 
            byte[] buffer = new byte[BUFFER_SIZE];
 
            while ((readLen = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, readLen);
            }
            
            inputStream.close();
            outputStream.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }		
		
	}


}
```



### List

```java
public static void main(String[] args) {
	 Worker worker = new Worker(0);
	 Worker worker2 = new Worker(1);
	 List<String> store=new ArrayList<String>();  //ArrayList 클래스를 사용하여 String 저장하는 리스트 생성  -> [apple, banana, orange] 
    											  //store.add("banana") 와 같이 추가
    											  //store.remove("banaba")와 같이 삭제
    											  //store.clear() -> 초기화
    											  //store.get(int index) ->지정된 인덱스 위치의 요소 반환
       											  //store.indexOf(Object o) ->지정된 요소의 인덱스 위치 반환
       											  //store.lastindexOf(Object o) ->지정된 요소의 마지막 인덱스 위치 반환
       											  //store.subList(int fronindex,int toIndex) ->지정된 범위 내의 요소 반환
    											  //store.set(int index,E element) -> 지정된 인덱스 위치의 요소를 새로운 요소로 교체
	 List<String> store2=new ArrayList<String>();
	 String res;
	Scanner scanner = new Scanner(System.in);
	String input;
	String []arrays;
   while (true) {
       input = scanner.nextLine();
       arrays=input.split(" ");
       if(arrays[1].equalsIgnoreCase("0")) { //equalsIgnoreCase: 대소문자 무시하고 동일한지 확인
    	   
       	res=worker.run(Integer.parseInt(arrays[0]),arrays[2]); //Integer.parseInt(arrays[0]) : string to int
           													   // String.valueOf(1) : int to string
       	store.add(arrays[0]+"#"+arrays[2]);
       	//System.out.println(store);   출력
        worker.removeExpiredStoreItems(Long.parseLong(arrays[0]), store);
       }
       else {
       	res=worker2.run(Integer.parseInt(arrays[0]),arrays[2]);
       	store2.add(arrays[0]+"#"+arrays[2]);
       	worker2.removeExpiredStoreItems(Long.parseLong(arrays[0]), store2);
       }
       if(res!=null) {
       	System.out.println(res);
       }
   }
}

+)ArrayList 클래스
ArrayList<String> al = new String>(); //ArrayList 클래스를 사용하여 String 저장하는 리스트 생성  

//데이터 출력 방식1
for(String name:al){
    System.out.println(name);
}

//데이터 출력 방식2
for(int i=0; i<al.size(); i++){
    System.out.println(al.get(i));
}

//데이터 출력 방식3
Iterator<String> itr=al.iterator();
while(itr.hasNext()){
    System.out.println(itr.next());
}

//데이터 정렬하여 출력(오름차순)
Collections.sort(al);
for(String name:al){
    System.out.println();
}

//데이터 내림차순 정렬하여 출력
Comparator<String> co=new Comparator<String>(){
    public int compare(String o1,String o2){
        return(o2.compareTo(o1));
    }
}
Collections.sort(al,co); //위에서 정의한 내림차순정렬로 정렬됨

//정렬(람다식 사용)
Collections.sort(al,(g1,g2)->g1.compateTo(g2));
for(String name:al){
    System.out.println(name);
}

```



### Map

-저장되는 순서가 유지되지 않는 구조(순서보장x)

-key,value 형태

-key 중복 불가

```java
HashMap<String,String> m=new HashMap<String,String>();
		
m.put("aaaaa", "111");
m.put("bbbbb", "222");	
m.put("ccccc", "333");
		
for(String key: m.keySet()) {
	System.out.println(key + ":" + m.get(key));
}
		
m.remove("aaaaa"); //key 이용하여 데이터 삭제
		
m.replace("bbbbb","565"); //key 이용하여 value 변경
```



### 큐(Queue)

-선입선출(fifo) 형식

```java
public static void main(String[] args) throws IOException {
	
	Queue<String> numberQ=new LinkedList<>();
	
	numberQ.add("one");
	numberQ.add("two");
	numberQ.add("three");
	
	System.out.println(numberQ.size()); //큐의 크기  결과는 3
	for(String i:numberQ) {
		System.out.println(i); //큐 순차적 출력
	}
	
	System.out.println(numberQ.poll()); // 큐에서 가장 앞에 있는 요소를 제거하고 반환합니다. 큐가 비어있으면 null을 반환
	System.out.println(numberQ.peek()); //큐에서 가장 앞에 있는 요소를 반환합니다. 큐가 비어있으면 null을 반환
	System.out.println(numberQ.contains("three"));//boolean값 반환
    numberQ.clear();
}
	
```

​	

```java
List<String> numberQ=new ArrayList<>();

numberQ.add("one");
numberQ.add("two");
numberQ.add("three");

System.out.println("Queue Count= "+numberQ.size());
for(String number:numberQ){
    System.out.println(number);//큐 순차적 출력
}
System.out.println("Deque : " + numberQ.get(0)); numberQ.remove(0);// 큐에서 가장 앞에 있는 요소를 제거하고 반환합니다. 큐가 비어있으면 null을 반환
System.out.println("Peek : " + numberQ.get(0)); //큐에서 가장 앞에 있는 요소를 반환합니다. 큐가 비어있으면 null을 반환
System.out.println("Contains(\"three\") = " + numberQ.contains("three"));//boolean값 반환

```




```java
public static void main(String[] args) {
	// TODO Auto-generated method stub
	//큐에 저장
	Queue<String> Q=new LinkedList<>();
	Scanner scanner = new Scanner(System.in); //사용자로부터 콘솔 입력받기
	String input;
	String []arrays;
	
    while (true) {//무한반복입력
        input = scanner.nextLine();//입력받은 값
        arrays=input.split(" ");//0 VIEW_AD1 값 split
        
        if(arrays[0].equals("SEND")) {
        	Q.add(arrays[1]);
        }
        else if(arrays[0].equals("RECEIVE")) {
        	System.out.println(Q.poll());
        }
    }
	
}
```



### List 실습

```java
package com.lgcns.test;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Scanner;

public class RunManager {

	public static void main(String[] args) throws IOException {
		
		Scanner scanner = new Scanner(System.in);
		String input;
		File f = new File("C:/Users//84046//Downloads//data_structure//List_sample.txt");
		List<String[]> al = new ArrayList<>();
		BufferedReader reader = new BufferedReader(new FileReader("C:/Users//84046//Downloads//data_structure//List_sample.txt"));
		String str;
		while ((str = reader.readLine()) != null) { 
			//Kim	80	70	90
			al.add(str.split("\\s"));//공백기준분리
		}
		 while (true) {
		       input = scanner.nextLine();
		       if(input.equals("print")) {
		    	   Collections.sort(al, new Comparator<String[]>() {
		               public int compare(String[]a, String[]b) {
		                   return Integer.compare(Integer.parseInt(b[0]), Integer.parseInt(a[0]));
		               }
		           });			
		       }else if(input.equals("korean")) {
		    	   Collections.sort(al, new Comparator<String[]>() {
		               public int compare(String[]a, String[]b) {
		                   return Integer.compare(Integer.parseInt(b[1]), Integer.parseInt(a[1]));
		               }
		           });	
		       }else if(input.equals("english")) {
		    	   Collections.sort(al, new Comparator<String[]>() {
		               public int compare(String[]a, String[]b) {
		                   return Integer.compare(Integer.parseInt(b[2]), Integer.parseInt(a[2]));
		               }
		           });	
		       }else if(input.equals("math")) {
		    	   Collections.sort(al, new Comparator<String[]>() {
		               public int compare(String[]a, String[]b) {
		                   return Integer.compare(Integer.parseInt(b[3]), Integer.parseInt(a[3]));
		               }
		           });
		       }
		       else if(input.equals("quit")) {
		    	   break;
		       }
		       for(String[] name:al){
	    		   System.out.println(String.join("\t", name));
			}
		 }
		// TODO Auto-generated method stub

	

	}

}

```

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.Iterator;

public class ListPrac {
    public static void main(String[] args) throws IOException {
    	ArrayList<Grade> al = new ArrayList<Grade>(); 
    	
        try {
            ////////////////////////////////////////////////////////////////
            BufferedReader in = new BufferedReader(new FileReader("List_Sample.txt"));
            String str;

            while ((str = in.readLine()) != null) {
            	String words[] = str.split("\t");
            	Grade g = new Grade(words[0],Integer.parseInt(words[1]),Integer.parseInt(words[2]),Integer.parseInt(words[3]));
            	al.add(g);
            }
            in.close();
            ////////////////////////////////////////////////////////////////
        } catch (IOException e) {
            System.err.println(e); // 에러가 있다면 메시지 출력
            System.exit(1);
        }
        
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while (true)
        {
        	String strInput = br.readLine();
        	
        	switch(strInput) {
        	case "PRINT": // 이름 순 출력
        		Collections.sort(al, (g1, g2) -> g1.getName().compareTo(g2.getName()));
        		break;
        	case "KOREAN": // 국어 성적 순 출력 
        		// Lambda식
        		Collections.sort(al, (g1, g2) -> (g2.getKorean() - g1.getKorean()) == 0 ? g1.getName().compareTo(g2.getName()) : g2.getKorean() - g1.getKorean());        		
        		break;
        	case "ENGLISH": // 영어 성적 순 출력
        		// Comparator
        		Collections.sort(al, new Comparator<Grade>() {

					@Override
					public int compare(Grade x, Grade y) {
						if (y.getEnglish() - x.getEnglish() == 0)
						{
							return x.getName().compareTo(y.getName());
						}
						else
						{
							return y.getEnglish() - x.getEnglish();
						}
					}
        			
        		});        		
        		break;
        	case "MATH":
        		// Comparator 
        		Collections.sort(al, new sortByMath());        		
        		break;
        	case "QUIT":
        		return;
       		default:
        		break;
        	}
        	
        	for (Grade val : al)
        	{
        		System.out.println(String.format("%s\t%d\t%d\t%d",val.getName(), val.getKorean(), val.getEnglish(), val.getMath()));
        	}        	     
        }  
    }   
}

class Grade 
{
    private String strName;
    private int Korean;
    private int English;
    private int Math; 

    public Grade(String str, int k, int e, int m)
    {
        strName = str;
        Korean = k;
        English = e;
        Math = m;
    }

    public String getName()
    {
        return strName;
    }
    public void setName(String strName)
    {
        this.strName = strName;
    }
    public int getKorean()
    {
        return Korean;
    }
    public void setProjectA(int n)
    {
        Korean = n;
    }
    public int getEnglish()
    {
        return English;
    }
    public void setProjectB(int n)
    {
        English = n;
    }
    public int getMath()
    {
        return Math;
    }
    public void setMath(int n)
    {
        Math = n;
    }
}

class sortByMath implements Comparator<Grade>{
	public int compare(Grade x, Grade y) {
		if (y.getMath() - x.getMath() == 0)
		{
			return x.getName().compareTo(y.getName());
		}
		else
		{
			return y.getMath() - x.getMath();
		}
	}
}
```



### MAP 실습

```java
package com.lgcns.test;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashMap;
import java.util.List;
import java.util.Scanner;

public class RunManager {

	public static void main(String[] args) throws IOException {
		
		HashMap<String, Effort> m = new HashMap<String, Effort> ();
        BufferedReader br = new BufferedReader(new FileReader("C:\\Users\\84046\\Downloads\\data_structure\\DS_Sample2.csv"));
        String line;
        while ((line = br.readLine()) != null) {
            String[] words = line.split(",");//csv는 ,로 구분
            String key = words[1];

            if (!m.containsKey(key))
            {
                   Effort ef = new Effort(words[2], Double.parseDouble(words[3]), Double.parseDouble(words[4]), Double.parseDouble(words[5]));
                   m.put(key, ef);
            }
            else 
            {
                   m.get(key).AddA(Double.parseDouble(words[3]));
                   m.get(key).AddB(Double.parseDouble(words[4]));
                   m.get(key).AddC(Double.parseDouble(words[5]));
            }
         }
        if(br != null) 
        	br.close();
        
        for( String key : m.keySet() ){
        	double total = m.get(key).getdA() + m.get(key).getdB() + m.get(key).getdC();
            System.out.println( String.format("%s	%s	%.1f	%.1f	%.1f	->	%.1f", key, m.get(key).getStrName(), m.get(key).getdA(), m.get(key).getdB(), m.get(key).getdC(), total));
        }        
    }
}

class Effort {
	private String strName;
	private double dA;
	private double dB;
	private double dC;
	
	public Effort(String str, double a, double b, double c){
		strName = str;
		dA = a;
		dB = b;
		dC = c;
	}
	
	public String getStrName() {
		return strName;
	}
	public void setStrName(String strName) {
		this.strName = strName;
	}
	public double getdA() {
		return dA;
	}
	public void setdA(double dA) {
		this.dA = dA;
	}
	public double getdB() {
		return dB;
	}
	public void setdB(double dB) {
		this.dB = dB;
	}
	public double getdC() {
		return dC;
	}
	public void setdC(double dC) {
		this.dC = dC;
	}
	public void AddA(double d)
	{
		dA += d;
	}
	public void AddB(double d)
	{
		dB += d;
	}
	public void AddC(double d)
	{
		dC += d;
	}
}
```



### **Process** **&** **Thread**

![image-20240507143320572](C:\Users\84046\AppData\Roaming\Typora\typora-user-images\image-20240507143320572.png)

1) Process

\- 운영 체제가 바라보는 일의 단위

2) Thread

\- Process 내에서 다시 나누어지는 일의 단위

```java
//외부 프로세스 실행
public static String getProcessOutput(List<String> cmdList)
throws IOException, InterruptedException { //프로세스 실행 커맨드
    ProcessBuilder builder = new ProcessBuilder(cmdList);
    Process process = builder.start(); //프로세스 실행
    InputStream psout = process.getInputStream();//출력 가져오기
    byte[] buffer = new byte[1024]; 
    psout.read(buffer);
	return (new String(buffer));
}

public static void main(String[] args) throws IOException, InterruptedException {
    String output = getProcessOutput(Arrays.asList("add_2sec.exe","2","3"));
    System.out.println(output);
}

//Thread class 상속
class ThreadClass extends Thread {
	public void run() {
		System.out.print("Thread is running...");
		}
	}
public class ThreadSample {
	public static void main(String[] args) { 
        ThreadClass tc1 = new ThreadClass();
		tc1.start();
		try {
			tc1.join();
		} catch (InterruptedException e) { 
        	e.printStackTrace();
		}
	}
}

//Runnable Interface 이용
class ThreadClass implements Runnable {
	public void run(){
		System.out.println("thread is running...");
	}
}

public class ThreadRunnable {
	public static void main(String[] args) { 
        ThreadClass m1=new ThreadClass(); 
        Thread t1 =new Thread(m1); 
        t1.start();
		try {
		t1.join();
		} catch (InterruptedException e) {
            e.printStackTrace();
		}
    }
}
                                      


```



### **Thread** **실습**

Thread 2개를 만든 후, Main함수와 Thread 2개에서 동시에 0부터 9까지 출력하시오.

어디서 출력하였는지 구분할 수 있게 숫자 앞에 [Main], [Thread1], [Thread2] 표시하시오.

```java
package com.lgcns.test;

public class RunManager {

	public static void main(String[] args)  {
		Thread thread1=new Thread(()-> {
			for(int i=0; i<10; i++) {
				System.out.println("[Thread1]"+i);
			}
		});
		
		Thread thread2=new Thread(()-> {
			for(int i=0; i<10; i++) {
				System.out.println("[Thread2]"+i);
			}
		});
		
		thread1.start();
		thread2.start();
		
		for (int i=0; i<10; i++) {
			System.out.println("[Main]"+i);
		}
	}
}
```



### **Process** **&** **Thread** **실습**

NUM.TXT에 저장되어 있는 5쌍의 숫자들을 add_2sec.exe를 통해 덧셈을 실행시킨 후, 각각의 결과들을 모두 출력하시오.

[조건]

-전체 실행 시간은 5초 이내

-결과의 출력 순서는 상관없음

-실행 시작과 끝에 현재시각 출력



\* add_2sec.exe [num1] [num2] 실행하면, 2초 후 num1 + num2의 값을 출력함.

```java
package com.lgcns.test;

import java.io.BufferedReader;
import java.io.File;
import java.io.IOException;
import java.io.InputStreamReader;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Date;
import java.util.Scanner;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class RunManager {

	public static void main(String[] args) throws IOException, InterruptedException  {
		Scanner scanner =new Scanner(new File("C:\\Users\\84046\\Downloads\\thread\\NUM.txt"));
		 ExecutorService executor = Executors.newFixedThreadPool(5); // 5개의 스레드를 가진 풀을 생성합니다.
			//LocalDateTime now = LocalDateTime.now();
	        //DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
	        Date currentDate = new Date();
		 System.out.println("Start -" + currentDate);
		while(scanner.hasNext()) {
			int num1=scanner.nextInt();
			int num2=scanner.nextInt();
			executor.submit(()->{
				//add_2sec.exe 실행
				try {
					ProcessBuilder pb=new ProcessBuilder("C:\\Users\\84046\\Downloads\\thread\\add_2sec.exe",String.valueOf(num1),String.valueOf(num2));
					Process process= pb.start();
					BufferedReader reader=new BufferedReader(new InputStreamReader(process.getInputStream()));
					String line;
					while((line=reader.readLine())!=null) {
						System.out.println(num1+"+"+num2+"="+line);
					}
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			});
			
		}
		executor.shutdown();
		executor.awaitTermination(Long.MAX_VALUE, TimeUnit.MILLISECONDS);//모든 작업이 끝날때까지 대기
		//now = LocalDateTime.now();
		currentDate = new Date();
		System.out.println("End -" + currentDate);
		scanner.close();
	}
}
```



### JSON 실습

JSON Library를 활용하여 다음 데이터를 sample.json 파일로 저장하시오.

```java
package com.lgcns.test;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.lang.reflect.Type;
public class RunManager {

	public static void main(String[] args) throws IOException, InterruptedException  {
		Map<String,Object> jsonObject=new HashMap<>();
		jsonObject.put("name","spiderman");
		jsonObject.put("age",45);
		jsonObject.put("married",true);
		jsonObject.put("specialty",Arrays.asList("martial art","gun"));
		
		Map<String, Object> vaccine = new HashMap<>();
	    vaccine.put("1st", "done");
	    vaccine.put("2nd", "expected");
	    vaccine.put("3rd", null);
	    jsonObject.put("vaccine", vaccine);
		
        Map<String, Object> child1 = new HashMap<>();
        child1.put("name", "spiderboy");
        child1.put("age", 10);
        
        Map<String, Object> child2 = new HashMap<>();
        child2.put("name", "spidergirl");
        child2.put("age", 8);

        jsonObject.put("children", Arrays.asList(child1, child2));
        jsonObject.put("adress", null);
        
        // JSON 객체를 파일로 저장
        try (FileWriter file = new FileWriter("C:\\sample.json")) {
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            file.write(gson.toJson(jsonObject));
            System.out.println("Successfully Copied JSON Object to File...");
            System.out.println("\nJSON Object: " + jsonObject);
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        //파일 읽고 출력
        try (FileReader reader = new FileReader("C:\\sample.json")) {
            // JSON 파일 읽기
            Gson gson = new Gson();
            Type type = new TypeToken<Map<String, Object>>(){}.getType();
            Map<String, Object> jsonObject2 = gson.fromJson(reader, type);

            // 원하는 정보 출력
            System.out.println("name(age) : " + jsonObject2.get("name") + "(" + jsonObject2.get("age") + ")");

            List<Map<String, Object>> children = (List<Map<String, Object>>) jsonObject2.get("children");
            for (Map<String, Object> child : children) {
            	if(child.get("name").equals("spidergirl")) {
            		System.out.println("name(age) : " + child.get("name") + "(" + child.get("age") + ")");
            	}
            }
            
         // 각 키의 값의 타입 출력
            for (Map.Entry<String, Object> entry : jsonObject.entrySet()) {
            	String valueType = "null";
                if (entry.getValue() != null) {
                    if (entry.getValue() instanceof String) {
                        valueType = "String";
                    } else if (entry.getValue() instanceof Number) {
                        valueType = "Number";
                    } else if (entry.getValue() instanceof Boolean) {
                        valueType = "Boolean";
                    } else if (entry.getValue() instanceof List) {
                        valueType = "Array";
                    } else if (entry.getValue() instanceof Map) {
                        valueType = "Object";
                    }
                }
                System.out.println("Key : " + entry.getKey() + " / Value Type : " + valueType);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
	}
}

/*
 * {
"name":"spiderman", "age":45,
"married":true, "specialty":["martial art", "gun"],
"vaccine":{"1st":"done","2nd":"expected","3rd":null},
"children": [{"name":"spiderboy", "age":10}, {"name":"spidergirl", "age":8}],
"adress":null
}
*/
 
```





### **Http** **통신 실습**

\1. Client에서 Server에 “http://127.0.0.1:8088/requestDate”로 요청하고,

Server는 현재 날짜와 시각을 Client로 응답하게 하시오. (요청 Method는 ‘GET’ 사용)

```java
package test;

import org.eclipse.jetty.server.*;
import org.eclipse.jetty.servlet.ServletHandler;

public class MyServer {

	public static void main(String[] args) throws Exception {
		new MyServer().start();
	}
	//127.0.0.1:8088/requestDate
	public void start() throws Exception {
		Server server = new Server();
		ServerConnector http = new ServerConnector(server);
		http.setHost("127.0.0.1");
		http.setPort(8088);
		server.addConnector(http);

		ServletHandler servletHandler = new ServletHandler();
		servletHandler.addServletWithMapping(MyServlet.class, "/requestDate");
		server.setHandler(servletHandler);

		server.start();
		server.join();
	}
}

```

```java
package test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

import org.eclipse.jetty.client.HttpClient;
import org.eclipse.jetty.client.api.ContentResponse;
import org.eclipse.jetty.client.util.StringContentProvider;
import org.eclipse.jetty.http.HttpMethod;

public class MyClient {
	static String rootPath = "C:/Input";
	static Map<String,Object> jsonObject=new HashMap<>();
	static ArrayList<String> files = new ArrayList<>(); // ArrayList 생성
	public static void main(String[] args) throws Exception {
		jsonObject.put("Folder","Input");
		FileSearchAll(rootPath);
		System.out.println(jsonObject);
		HttpClient httpClient = new HttpClient();
		httpClient.start();
		ContentResponse contentRes = httpClient.newRequest("http://127.0.0.1:8088/requestDate").method(HttpMethod.POST)
				.header("Content-Type", "application/json")
				.content(new StringContentProvider(jsonObject.toString()),"application/json")
				.send();
		System.out.println(contentRes.getContentAsString());
	}
	
	
    public static void FileSearchAll(String path)
    {
    	File directory = new File(path);
    	File[] fList = directory.listFiles();
	    for (File file : fList) {
	        if (file.isDirectory()) {
	        	FileSearchAll(file.getPath());
	        }    	
	        else { 
	        	String partPath = path.substring(rootPath.length());
	            System.out.println(file.getName());
	            files.add(file.getName());
	            MyCopyFile(partPath, file.getName());
	        }
	    }
	    jsonObject.put("Files",files);
    }
    
    public static void MyCopyFile(String partPath, String filename)
	{
		final int BUFFER_SIZE = 512;
        int readLen;
        try {
    		// Create Folder
    		File destFolder = new File("C:/OUTPUT" + partPath);
    		if(!destFolder.exists()) {
    			destFolder.mkdirs(); 
    		}        	
        	
    		// Copy File
            InputStream inputStream = new FileInputStream("C:/INPUT"+partPath+"\\"+filename);
            OutputStream outputStream = new FileOutputStream("C:/OUTPUT"+partPath+"\\"+filename);
 
            byte[] buffer = new byte[BUFFER_SIZE];
 
            while ((readLen = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, readLen);
            }
            
            inputStream.close();
            outputStream.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }		
	}    
}

```

```java
package test;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.Date;
import java.util.stream.Collectors;

import javax.servlet.ServletException;
import javax.servlet.http.*;

public class MyServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		System.out.println("Request : "+ req.getRequestURL());
		
		res.setStatus(200);
		res.getWriter().write(new Date().toString());
	}
	
	protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		System.out.println("Request : "+ req.getRequestURL());
		//String json = req.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
		//String timestamp = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMddHHmmss"));
		//Files.write(Paths.get("C:/"+timestamp + ".json"), json.getBytes());
		
		File destFolder = new File("./OUTPUT");
		if(!destFolder.exists()) {
		    destFolder.mkdirs(); 
		}
		
		LocalTime currentTime = LocalTime.now();
		String fname = String.format("C:/OUTPUT/%02d%02d%02d.json", currentTime.getHour(), currentTime.getMinute(), currentTime.getSecond());
	    PrintWriter printWriter = new PrintWriter(new FileWriter(new File(fname)));
	    
        BufferedReader input = new BufferedReader(new InputStreamReader(req.getInputStream()));
        String buffer;
        while ((buffer = input.readLine()) != null) {
        	printWriter.print(buffer);
        }        
		input.close();		
		printWriter.close();
		
		res.setStatus(200);
		String response = "Current date and time: " + LocalDateTime.now();
		res.getWriter().write(response);
	}
}

```



### 2023-1 기출

```java
package com.lgcns.test;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

public class RunManager {
	private Map<String, String> commandMap;
	public static void main(String[] args) throws Exception  {
		RunManager solution = new RunManager();
		solution.run();

	}

	public void run() throws Exception {
		loadCommandInfo();
		
		try (Scanner scanner = new Scanner(System.in)) {//사용자로부터 콘솔 입력받기
			String[] requestArray = scanner.next().split("#");//CMD_002#DEVICE_083,DEVICE_015#ce3c39
			String command = requestArray[0];//CMD_002
			String[] targetDeviceArray = requestArray[1].split(",");//DEVICE_083,DEVICE_015
			String parameter = requestArray[2];//ce3c39
			List<String> resultList = new ArrayList<>();

			for (String targetDevice : targetDeviceArray) {
				String wname = "DEVICE/REQ_TO_"+targetDevice+".TXT";
				writeFile(wname,this.commandMap.get(command)+"#"+parameter);
				Thread.sleep(500);
				String rname = "DEVICE/RES_FROM_"+targetDevice+".TXT";
				System.out.println(rname);
				String result = readFile(rname);
				resultList.add(result);
			}
			System.out.println(String.join(",", resultList));
		}
	}
	
	private void loadCommandInfo() throws Exception {
		this.commandMap = new HashMap<>();

		try (Scanner scanner = new Scanner(new File("INFO/SERVER_COMMAND.TXT"))) {
			while (scanner.hasNext()) {
				String[] stringArray = scanner.next().split("#");
				this.commandMap.put(stringArray[0], stringArray[1]);
			}
		}
	}

	//RES_FROM_DEVICE_083.TXT , c8d4fc55
	private String readFile(String fileName) throws Exception {
		return Files.readAllLines(Paths.get(fileName)).get(0);//c8d4fc55 읽기
	}

	//REQ_TO_DEVICE_083.TXT , CMD_002_B#ce3c39
	private void writeFile(String fileName, String content) throws Exception {
		Files.write(Paths.get(fileName), (content + "\n").getBytes(), StandardOpenOption.CREATE);//파일생성
	}

}



```

```java
package com.lgcns.test;

import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.HashMap;
import java.util.Map;

import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.server.ServerConnector;
import org.eclipse.jetty.servlet.ServletContextHandler;
import org.eclipse.jetty.servlet.ServletHolder;

import com.google.gson.Gson;
import com.lgcns.test.DeviceInfoData.DeviceInfo;
import com.lgcns.test.ServerCommandInfoData.ServerCommandInfo;

public class Solution {

	private Gson gson = new Gson();
	private Map<String, ServerCommandInfo> serverCommandInfoMap;
	private Map<String, DeviceInfo> deviceInfoMap;

	public void run() throws Exception {

		loadServerCommandInfo();
		loadDeviceInfo();

		Server fromServer = createServer();
		fromServer.start();
	}

	private void loadServerCommandInfo() throws Exception {//CommandInfo로드
		ServerCommandInfoData commandInfoData = gson.fromJson(
				String.join("", Files.readAllLines(Paths.get("INFO/SERVER_COMMAND.JSON"))),
				ServerCommandInfoData.class);
		this.serverCommandInfoMap = new HashMap<>();
		for (ServerCommandInfo serverCommandInfo : commandInfoData.getServerCommandInfoList()) {
			this.serverCommandInfoMap.put(serverCommandInfo.getCommand(), serverCommandInfo);
		}
	}

	private void loadDeviceInfo() throws Exception {
		DeviceInfoData deviceInfoData = gson
				.fromJson(String.join("", Files.readAllLines(Paths.get("INFO/DEVICE.JSON"))), DeviceInfoData.class);
		this.deviceInfoMap = new HashMap<>();
		for (DeviceInfo deviceInfo : deviceInfoData.getDeviceInfoList()) {
			this.deviceInfoMap.put(deviceInfo.getDevice(), deviceInfo);
		}
	}

	private Server createServer() {
		Server server = new Server();
		ServerConnector http = new ServerConnector(server);
		http.setHost("127.0.0.1");
		http.setPort(8010);
		server.addConnector(http);

		ServletContextHandler context = new ServletContextHandler(ServletContextHandler.SESSIONS);
        context.setContextPath("/*");
        context.addServlet(new ServletHolder(new EdgeNodeServlet(serverCommandInfoMap, deviceInfoMap)), "/*");
        server.setHandler(context);

        return server;
	}
}

```

```java
package com.lgcns.test;

import java.util.List;

import com.google.gson.annotations.SerializedName;
//데이터 딕셔너리화하기
/*
{  "serverCommandInfo":[
      {"command":"CMD_001", "forwardCommand":"CMD_001_A"},
      {"command":"CMD_002", "forwardCommand":"CMD_002_B"}
   ]
}	
 */
public class ServerCommandInfoData {
    @SerializedName("serverCommandInfo")
    private List<ServerCommandInfo> serverCommandInfoList;

    public List<ServerCommandInfo> getServerCommandInfoList() {
        return serverCommandInfoList;
    }
    
    public class ServerCommandInfo {
    	private String command;
    	private String forwardCommand;

    	public String getCommand() {
			return command;
		}
		public String getForwardCommand() {
			return forwardCommand;
		}
    }
}

```



