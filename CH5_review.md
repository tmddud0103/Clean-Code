# CH5 Formatting



## Intro

질서정연하고 깔끔하며, 일관적인 코드를 본다면 전문가의 인상을 준다

코드가 더럽다면 전반적으로 무성의한 태도로 작성했다고 생각한다

프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야한다.



## 형식을 맞추는 목적


코드 형식은 의사소통의 일환이며, 의사소통은 전문 개발자의 일차적인 의무다.

오늘 구현한 기능이 다음 버전에서 바뀔 확률은 높지만,
 코드의 스타일과 가독성은 유지보수의 용이성에 영향을 미친다.

**코드는 사라져도 스타일과 규율은 사라지지 않는다!**



## 적절한 행 길이 유지 


500줄을 넘지 않고 대부분 200줄 정도인 파일로도 시스템은 돌아간다.

엄격한 규칙은 아니지만, 큰 파일보다는 작은 파일이 이해하기 쉽다.



#### 신문 기사 작성

좋은 신문 기사는 최상단에 표제, 
첫 문단에는 전체 기사 내용을 요약, 기사를 읽으며 내려갈 수록 세세한 사실이 조금씩 드러나며 세부사항이 나오게 된다.

소스파일 이름(표제)는 간단하면서도 설명이 가능하게, 소스파일의 첫 부분(요약 내용)은 고차원 개념과 알고리즘을 설명한다.



#### 개념은 빈 행으로 분리


생각 사이에는 빈 행을 넣어 분리

그렇지 않다면 코드 가독성이 현저히 떨어진다.

```c++
// 나쁜 예시
package fitnesse.wikitext.widgets;
import java.util.regex.*;
public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
		Pattern.MULTILINE + Pattern.DOTALL);
	public BoldWidget(ParentWidget parent, String text) throws Exception {
		super(parent);
		Matcher match = pattern.matcher(text); match.find(); 
		addChildWidgets(match.group(1));}
	public String render() throws Exception { 
		StringBuffer html = new StringBuffer("<b>"); 		
		html.append(childHtml()).append("</b>"); 
		return html.toString();
	} 
}

// 좋은 예시
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''", 
		Pattern.MULTILINE + Pattern.DOTALL
	);
	
	public BoldWidget(ParentWidget parent, String text) throws Exception { 
		super(parent);
		Matcher match = pattern.matcher(text);
		match.find();
		addChildWidgets(match.group(1)); 
	}
	
	public String render() throws Exception { 
		StringBuffer html = new StringBuffer("<b>"); 
		html.append(childHtml()).append("</b>"); 
		return html.toString();
	} 
}
```



#### 세로 밀집도

줄바꿈이 개념을 분리

세로 밀집도는 연관성을 의미

```c++
// 의미없는 주석으로 변수를 떨어뜨려 놓아서 한눈에 파악이 잘 안된다.

public class ReporterConfig {
	/**
	* The class name of the reporter listener 
	*/
	private String m_className;
	
	/**
	* The properties of the reporter listener 
	*/
	private List<Property> m_properties = new ArrayList<Property>();
	public void addProperty(Property property) { 
		m_properties.add(property);
	}
    
// 의미 없는 주석을 제거함으로써 코드가 한눈에 들어온다.
// 변수 2개에 메소드가 1개인 클래스라는 사실이 드러난다.

public class ReporterConfig {
	private String m_className;
	private List<Property> m_properties = new ArrayList<Property>();
	
	public void addProperty(Property property) { 
		m_properties.add(property);
	}
```



#### 수직 거리

서로 밀접한 개념은 세로로 가까이 둬야 한다.


##### 변수선언

변수는 사용하는 위치에서 최대한 가까이 선언한다.

우리가 만든 함수는 매우 짧으므로 (Chapter3 - 함수를 공부했다면 말이지)

```c++
// InputStream이 함수 맨 처음에 선언 되어있다.

private static void readPreferences() {
	InputStream is = null;
	try {
		is = new FileInputStream(getPreferencesFile()); 
		setPreferences(new Properties(getPreferences())); 
		getPreferences().load(is);
	} catch (IOException e) { 
		try {
			if (is != null) 
				is.close();
		} catch (IOException e1) {
		} 
	}
}
// 모두들 알다시피 루프 제어 변수는 Test each처럼 루프 문 내부에 선언

public int countTestCases() { 
	int count = 0;
	for (Test each : tests)
		count += each.countTestCases(); 
	return count;
}
// 드물지만, 긴 함수에서는 블록 상단 또는 루프 직전에 변수를 선언 할 수도 있다.
...
for (XmlTest test : m_suite.getTests()) {
	TestRunner tr = m_runnerFactory.newTestRunner(this, test);
	tr.addListener(m_textReporter); 
	m_testRunners.add(tr);

	invoker = tr.getInvoker();
	
	for (ITestNGMethod m : tr.getBeforeSuiteMethods()) { 
		beforeSuiteMethods.put(m.getMethod(), m);
	}

	for (ITestNGMethod m : tr.getAfterSuiteMethods()) { 
		afterSuiteMethods.put(m.getMethod(), m);
	} 
}
...
```



###### 인스턴스 변수

자바의 경우 인스턴스 변수는 클래스 맨 처음에 선언

C++의 경우에는 마지막에 선언

어느 곳이든 잘 알려진 위치에 인스턴스 변수를 모으는 것이 중요하다.

```C++
// 도중에 선언된 변수는 꽁꽁 숨겨놓은 보물 찾기와 같다. 십중 팔구 코드를 읽다가 우연히 발견한다. 발견해보시길.
// 요즘은 IDE가 잘 되어있어서 찾기야 어렵지 않겠지만, 더러운건 마찬가지

public class TestSuite implements Test {
	static public Test createTest(Class<? extends TestCase> theClass,
									String name) {
		... 
	}

	public static Constructor<? extends TestCase> 
	getTestConstructor(Class<? extends TestCase> theClass) 
	throws NoSuchMethodException {
		... 
	}

	public static Test warning(final String message) { 
		...
	}
	
	private static String exceptionToString(Throwable t) { 
		...
	}
	
	private String fName;

	private Vector<Test> fTests= new Vector<Test>(10);

	public TestSuite() { }
	
	public TestSuite(final Class<? extends TestCase> theClass) { 
		...
	}

	public TestSuite(Class<? extends TestCase> theClass, String name) { 
		...
	}
	
	... ... ... ... ...
}
```



##### 종속 함수

한 함수가 다른 함수를 호출한다면(종속 함수) 두 함수는 세로로 가까이 배치한다. 

```C++
/*첫째 함수에서 가장 먼저 호출하는 함수가 바로 아래 정의된다.
다음으로 호출하는 함수는 그 아래에 정의된다. 그러므로 호출되는 함수를 찾기가 쉬워지며
전체 가독성도 높아진다.*/
	
/*makeResponse 함수에서 호출하는 getPageNameOrDefault함수 안에서 "FrontPage" 상수를 사용하지 않고,
상수를 알아야 의미 전달이 쉬워지는 함수 위치에서 실제 사용하는 함수로 상수를 넘겨주는 방법이
가독성 관점에서 훨씬 더 좋다*/

public class WikiPageResponder implements SecureResponder { 
	protected WikiPage page;
	protected PageData pageData;
	protected String pageTitle;
	protected Request request; 
	protected PageCrawler crawler;
	
	public Response makeResponse(FitNesseContext context, Request request) throws Exception {
		String pageName = getPageNameOrDefault(request, "FrontPage");
		loadPage(pageName, context); 
		if (page == null)
			return notFoundResponse(context, request); 
		else
			return makePageResponse(context); 
		}

	private String getPageNameOrDefault(Request request, String defaultPageName) {
		String pageName = request.getResource(); 
		if (StringUtil.isBlank(pageName))
			pageName = defaultPageName;

		return pageName; 
	}
	
	protected void loadPage(String resource, FitNesseContext context)
		throws Exception {
		WikiPagePath path = PathParser.parse(resource);
		crawler = context.root.getPageCrawler();
		crawler.setDeadEndStrategy(new VirtualEnabledPageCrawler()); 
		page = crawler.getPage(context.root, path);
		if (page != null)
			pageData = page.getData();
	}
	
	private Response notFoundResponse(FitNesseContext context, Request request)
		throws Exception {
		return new NotFoundResponder().makeResponse(context, request);
	}
	
	private SimpleResponse makePageResponse(FitNesseContext context)
		throws Exception {
		pageTitle = PathParser.render(crawler.getFullPath(page)); 
		String html = makeHtml(context);
		SimpleResponse response = new SimpleResponse(); 
		response.setMaxAge(0); 
		response.setContent(html);
		return response;
	} 
...
```



##### 개념의 유사성

개념적인 친화도가 높을 수록 코드를 서로 가까이 배치한다.

```C++
// 같은 assert 관련된 동작들을 수행하며, 명명법이 똑같고 기본 기능이 유사한 함수들로써 개념적 친화도가 높다.
// 이런 경우에는 종속성은 오히려 부차적 요인이므로, 종속적인 관계가 없더라도 가까이 배치하면 좋다.

public class Assert {
	static public void assertTrue(String message, boolean condition) {
		if (!condition) 
			fail(message);
	}

	static public void assertTrue(boolean condition) { 
		assertTrue(null, condition);
	}

	static public void assertFalse(String message, boolean condition) { 
		assertTrue(message, !condition);
	}
	
	static public void assertFalse(boolean condition) { 
		assertFalse(null, condition);
	} 
...
```



#### 세로 순서

일반적으로 함수 호출 종속성은 아래방향으로 유지하므로, 호출되는 함수를 호출하는 함수보다 뒤에 배치한다.


가장 중요한 개념을 가장 먼저 표현하고, 세세한 사항은 마지막에 표현한다.



## 가로 형식 맞추기

프로그래머들은 명백히 짧은 행을 선호

Hollerith가 제안한 80자 제한은 다소 인위적이므로 조금 더 늘여도 좋다. 

하지만 120자 이상을 넘어간다면 주의 부족이니 개인적으로는 120자 정도로 길이를 제한한다.



#### 가로 공백과 밀집도

가로로는 공백을 사용해 밀접/느슨한 개념을 표현한다

```C++
private void measureLine(String line) { 
	lineCount++;
	
	// 흔히 볼 수 있는 코드인데, 할당 연산자 좌우로 공백을 주어 왼쪽,오른쪽 요소가 확실하게 구분된다.
	int lineSize = line.length();
	totalChars += lineSize; 
	
	// 반면 함수이름과 괄호 사이에는 공백을 없앰으로써 함수와 인수의 밀접함을 보여준다
	// 괄호 안의 인수끼리는 쉼표 뒤의 공백을 통해 인수가 별개라는 사실을 보여준다.
	lineWidthHistogram.addLine(lineSize, lineCount);
	recordWidestLine(lineSize);
}
```

#### 가로 정렬

```C++
public class FitNesseExpediter implements ResponseSender {
	private		Socket		 	 socket;
	private 	InputStream 	 input;
	private 	OutputStream 	 output;
	private 	Reques		  	 request; 		
	private 	Response 	  	 response;	
	private 	FitNesseContex	 context; 
	protected 	long		  	 requestParsingTimeLimit;
	private 	long		  	 requestProgress;
	private 	long		  	 requestParsingDeadline;
	private 	boolean		  	 hasError;
	
	... 
```

보기엔 깔끔해 보일지 모르나, 코드가 엉뚱한 부분을 강조해 변수 유형을 자연스레 무시하고 이름부터 읽게 된다.



#### 들여쓰기

들여쓰기를 잘 해놓으면 *- 물론 그러고 있겠지만! -* 구조가 한 눈에 들어온다.



##### 들여쓰기 무시하기

간단한 if문, while문, 짧은 함수에서 들여쓰기를 무시하고픈 유혹이 생긴다.(정말?)

하지만 들여쓰기로 제대로 범위를 표현한 코드가 가독성이 더 높다. 

```C++
// 이렇게 한행에 다 넣을 수 있다고 다 때려 박는 것이 멋있는 코드가 아니란 것!

public class CommentWidget extends TextWidget {
	public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
	
	public CommentWidget(ParentWidget parent, String text){super(parent, text);}
	public String render() throws Exception {return ""; } 
}


// 한줄이라도 정성스럽게 들여쓰기로 감싸주자. 가독성을 위해

public class CommentWidget extends TextWidget {
	public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
	
	public CommentWidget(ParentWidget parent, String text){
		super(parent, text);
	}
	
	public String render() throws Exception {
		return ""; 
	} 
}
```



#### 가짜 범위

빈 while문이나 for문의 경우 


빈 블록을 들여쓰고 괄호로 감싸라. 

**그렇지 않으면 찾을 수 없는 버그가 발생할 가능성이 높다**



## 팀 규칙

팀에 속해있다면,
가장 우선시 되어야 하고 선호해야 할 규칙은 팀 규칙이다.


**좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이뤄지고, 읽기 쉬운 문서는 스타일이 일관적이고 매끄러워야 한다.**





## 밥 아저씨의 형식 규칙

이 책의 저자가 사용하는 규칙이 드러나는 코드를 첨부하며 턴을 종료한다.

```C++
public class CodeAnalyzer implements JavaFileAnalysis { 
	private int lineCount;
	private int maxLineWidth;
	private int widestLineNumber;
	private LineWidthHistogram lineWidthHistogram; 
	private int totalChars;
	
	public CodeAnalyzer() {
		lineWidthHistogram = new LineWidthHistogram();
	}
	
	public static List<File> findJavaFiles(File parentDirectory) { 
		List<File> files = new ArrayList<File>(); 
		findJavaFiles(parentDirectory, files);
		return files;
	}
	
	private static void findJavaFiles(File parentDirectory, List<File> files) {
		for (File file : parentDirectory.listFiles()) {
			if (file.getName().endsWith(".java")) 
				files.add(file);
			else if (file.isDirectory()) 
				findJavaFiles(file, files);
		} 
	}
	
	public void analyzeFile(File javaFile) throws Exception { 
		BufferedReader br = new BufferedReader(new FileReader(javaFile)); 
		String line;
		while ((line = br.readLine()) != null)
			measureLine(line); 
	}
	
	private void measureLine(String line) { 
		lineCount++;
		int lineSize = line.length();
		totalChars += lineSize; 
		lineWidthHistogram.addLine(lineSize, lineCount);
		recordWidestLine(lineSize);
	}
	
	private void recordWidestLine(int lineSize) { 
		if (lineSize > maxLineWidth) {
			maxLineWidth = lineSize;
			widestLineNumber = lineCount; 
		}
	}

	public int getLineCount() { 
		return lineCount;
	}

	public int getMaxLineWidth() { 
		return maxLineWidth;
	}

	public int getWidestLineNumber() { 
		return widestLineNumber;
	}

	public LineWidthHistogram getLineWidthHistogram() {
		return lineWidthHistogram;
	}
	
	public double getMeanLineWidth() { 
		return (double)totalChars/lineCount;
	}

	public int getMedianLineWidth() {
		Integer[] sortedWidths = getSortedWidths(); 
		int cumulativeLineCount = 0;
		for (int width : sortedWidths) {
			cumulativeLineCount += lineCountForWidth(width); 
			if (cumulativeLineCount > lineCount/2)
				return width;
		}
		throw new Error("Cannot get here"); 
	}
	
	private int lineCountForWidth(int width) {
		return lineWidthHistogram.getLinesforWidth(width).size();
	}
	
	private Integer[] getSortedWidths() {
		Set<Integer> widths = lineWidthHistogram.getWidths(); 
		Integer[] sortedWidths = (widths.toArray(new Integer[0])); 
		Arrays.sort(sortedWidths);
		return sortedWidths;
	} 
}
```













참고문헌(베낌)

https://github.com/Yooii-Studios/Clean-Code/blob/master/Chapter%2005%20-%20%ED%98%95%EC%8B%9D%20%EB%A7%9E%EC%B6%94%EA%B8%B0.md



