# Java Reflection API: Revealing the Dark Side of the Mirror (Artifacts)

## Eclipse OpenJ9 and OpenJDK Test Suites Analysis
 
 * Analyzed commits:
   * [Eclipse OpenJ9](https://github.com/eclipse/openj9) - c2aa03488caa3ac99061c828ec347aeafc40c1e5
   * [OpenJDK](http://hg.openjdk.java.net/jdk8u) - 1d288ad17d9c947ac53f5a3031e39527309751ff
 * Test suites characteristics:


 | Feature 	 | Eclipse OpenJ9 | OpenJDK   |
 |:----------|---------------:|----------:|
 | Test cases | 19,827 | 5,896 |
 | Input programs size | 23 SLOC - 8 KSLOC | 2 SLOC - 32 KSLOC |
 | Classes | 2,362 | 15,406 |
 | Source files | 2,097 | 13,19 |
 | Keywords coverage | 96.2% | 96.2% |
 | Enum usage | 26 | 1,000 |         
 | Annotations usage | 55 | 1,025 |  
 | Inheritance usage | 923 | 8,200 | 
 | Static usage | 4,897 | 33,294 |
 | Inner class usage | 265 | 2,215 |

**Table I. Eclipse OpenJ9 and Oracle JVMs test suites characteristics.**

## Survey

 * [Questions](survey/questions.pdf)
 * [All responses](survey/responses.xlsx)

## Technique

### Source Code

 * [View source code](technique)
 * Clone repository:

 ```
 git clone https://github.com/non-conformances-research/fse2019.git
 cd fse2019/technique
 ```
 
## Evaluation

### How to run the automatic steps of our technique

 * Requirements
	* Linux
	* Maven 3.3+
	* Java Virtual Machines:
	  * [Oracle 1.8.0_151](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)
	  * [OpenJDK 1.8.0_141](http://hg.openjdk.java.net/jdk8u/jdk8u/archive/jdk8u141-b15.tar.bz2)
	  * [Eclipse OpenJ9 0.8.0](https://github.com/AdoptOpenJDK/openjdk8-openj9-releases/releases/download/jdk8u162-b12_openj9-0.8.0/OpenJDK8-OPENJ9_x64_Linux_jdk8u162-b12_openj9-0.8.0.tar.gz)
	  * [IBM J9 8.0.5.10](https://www-01.ibm.com/support/docview.wss?uid=swg24042430#80510)
	* Python 2.7
	* Pip
	* Vrtualenv
 * Install all JVMs
 * Run the following commands
	```
	cd classifier
	virtualenv env
	source env/bin/activate
	pip install -r requirements.txt
	deactivate
	cd ..
	export ORACLE_PATH=<oracle_jvm_installation_path>
	export OPENJDK_PATH=<openjdk_jvm_installation_path>
	export ECLIPSE_OPENJ9_PATH=<eclipse_openj9_jvm_installation_path>
	export IBM_J9_PATH=<ibm_j9_jvm_installation_path>
	cd technique
	sh scripts/run_fse_experiment.sh
	
	```

### Test Cases

 * Parameters
 

 | Type 	 | Values |
 |:----------|:---------------:|
 |*int*      | -2147483648, -1, 0, 1, and 2147483647 |
 |*long*     | 2<sup>63</sup>, -1, 0, 1, and (2<sup>63</sup>)-1 |
 |*double*   | 4.9x10<sup>-324</sup>, -1.0, 0, 1.0, and 1.7x10<sup>308</sup> |
 |*boolean*  | false and true |
 |*char*     | '' and 'a' |                   
 |*float*    | 1.4x10<sup>-45</sup>, -1.0f, 0.0f, 1.0f, 3.4028235x10<sup>38</sup> |  
 |*short*    | -32768, -1, 0, 1, 32767|
 |*byte*     | -128, -1, 0, 1, 127 |
 |*String*   | "", " ", "gEuOVmBvn1", "#A1", and null | 
 |*Class*    | Class.class and null |
 |*Class[]*  | new Class[][]{new Class[]{int.class}, new Class[]{long.class}, new Class[]{double.class}, new Class[]{boolean.class}, new Class[]{char.class}, new Class[]{float.class}, new Class[]{short.class}, new Class[]{String.class}, new Class[]{Class.class}, new Class[]{Class[].class}, new Class[]{Object.class}} |
 |Other      | new Object() and null |     

**Table II. Values for Java primitive and non-primitive types used to create test cases.**

 * [Input Programs](subjects.xlsx)

### Results

 * Test cases added to Eclipse OpenJ9 test suite (https://github.com/eclipse/openj9/pull/1792)
   * [Class.getConstructor](test-cases/org/openj9/test/reflect/GetConstructorTests.java)
   * [Class.getConstructors](test-cases/org/openj9/test/reflect/GetConstructorsTests.java)
   * [Class.getDeclaredConstructor](test-cases/org/openj9/test/reflect/GetDeclaredConstructorTests.java)
   * [Class.getDeclaredConstructors](test-cases/org/openj9/test/reflect/GetDeclaredConstructorsTests.java)
   * [Class.getDeclaredField](test-cases/org/openj9/test/reflect/GetDeclaredFieldTests.java)
   * [Class.getDeclaredFields](test-cases/org/openj9/test/reflect/GetDeclaredFieldsTests.java)
   * [Class.getDeclaredMethod](test-cases/org/openj9/test/reflect/GetDeclaredMethodTests.java)
   * [Class.getDeclaredMethods](test-cases/org/openj9/test/reflect/GetDeclaredMethodsTests.java)
   * [Class.getField](test-cases/org/openj9/test/reflect/GetFieldTests.java)
   * [Class.getFields](test-cases/org/openj9/test/reflect/GetFieldsTests.java)
   * [Class.getMethod](test-cases/org/openj9/test/reflect/GetMethodTests.java)
   * [Class.getMethods](test-cases/org/openj9/test/reflect/GetMethodsTests.java)
 
 * Our technique detects false positives for the following methods of Java Reflection API:
   * Class.hashCode
   * Class.newInstance
   * Constructor.isAccessible
   * Constructor.newInstance
   * Field.getInt
   * Field.getLong
   * Field.isAccessible
   * Method.isAccessible
   * Parameter.getDeclaringExecutable
   * Parameter.hashCode
 * Reported Candidates ([see all](all-reported-candidates.md))

 
 | Method 				| Target | Report ID/Bug Tracker URL | Status |
 |:----------:|:---------------|---------------:|---------------:|
 |Class.getAnnotations 			| Specification | 9053675 								|Open |
 |Class.getDeclaredAnnotations 		| Specification | https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202652	|Duplicated |
 |Executable.getAnnotations 		| Specification | 9053679 								|Open|
 |Executable.getDeclaredAnnotations 	| Specification | 9053680 								|Open|
 |Executable.getParameterAnnotations 	| Specification | 9053682  								|Open|
 |Field.getAnnotations 			| Specification | 9053683  								|Open|
 |Field.getDeclaredAnnotations 		| Specification | 9053684  								|Open|
 |Method.getAnnotations 		| Specification | 9053677  								|Open|
 |Method.getDeclaredAnnotations 	| Specification | 9053678  								|Open|
 |Method.getParameterAnnotations 	| Specification | 9053681  								|Open|
 |Class.getResource 			| Specification | https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202687 	|Accepted |
 |Class.getResourceAsStream 		| Specification | https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202687 	|Accepted |
 |Class.getConstructor 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1837				|Fixed |
 |Class.getConstructors 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1838				|Fixed |
 |Class.getDeclaredConstructor 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1839 			|Fixed |
 |Class.getDeclaredConstructors 	| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1840 			|Fixed |
 |Class.getDeclaredField 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1841 			|Fixed |
 |Class.getDeclaredFields 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1627 			|Fixed |
 |Class.getDeclaredMethod 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1842 			|Fixed |
 |Class.getDeclaredMethods 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1843 			|Fixed |
 |Class.getField 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1844 			|Fixed |
 |Class.getFields 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1845 			|Fixed |
 |Class.getMethod 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1846 			|Fixed |
 |Class.getMethods 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1847 			|Fixed |
 |Constructor.getAnnotatedParameterTypes| Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8225394	|Fixed |
 |Constructor.getAnnotatedParameterTypes| Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8225395 	|Fixed |
 |Constructor.getAnnotatedParameterTypes| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1851 			|Fixed |
 |Constructor.getAnnotatedParameterTypes| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/5994 			|Fixed |
 |Constructor.getAnnotatedParameterTypes| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/5995 			|Fixed |
 |Executable.getAnnotatedParameterTypes | Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8225396	|Fixed |
 |Executable.getAnnotatedParameterTypes | Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8225398	|Fixed |
 |Executable.getAnnotatedParameterTypes | Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1852 			|Fixed |
 |Executable.getAnnotatedParameterTypes | Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/5997			|Fixed |
 |Executable.getAnnotatedParameterTypes | Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/5998			|Fixed |
 |Method.invoke 			| Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202689 	|Duplicated |
 |Method.invoke 			| Oracle	| 9053687 	|Open |
 |Method.invoke 			| Oracle	| 9053686 	|Open |
 |Method.invoke 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1834 		|Rejected |
 |Method.invoke 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1836 		|Rejected |
 |Method.invoke 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1835 		|Rejected |
 |Parameter.getAnnotatedType 		| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1850 		|Fixed |
 |Parameter.getParameterizedType 	| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1854			|Fixed |
 |Parameter.toString 			| Eclipse OpenJ9| https://github.com/eclipse/openj9/issues/1853			|Fixed |

**Table III. Reported candidates.**
