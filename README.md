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
 * Install all JVMs
 * Run the following commands
	```
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

 * Test cases added to Eclipse OpenJ9 test suite (link to pull request omitted because of blind review)
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

 * Detected Non-conformance Candidates
 
 
  | Id 	 | Method with Non-conformance Candidate | Test Cases | Failures | Oracle | OpenJDK | Eclipse OpenJ9 | IBM J9 |
  |:----------:|:---------------|---------------:|---------------:|:---------------:|:---------------:|:---------------:|:---------------:|
  | 1 | Class.getPackage | 473,436|471 | C | C | C | -
  | 2 | Class.getAnnotations | 937,860 | 3,602 | O | - | C | C
  | 3 | Class.getConstructor | 470,384 |59,934 | C | C | F | -
  | 4 | Class.getConstructors | 468,663 | 979 | C | C | F | -
  | 5 | Class.getDeclaredAnnotations | 939,368 | 3,490 | D | - | C | C
  | 6 | Class.getDeclaredConstructor | 469,624 | 15,389 | C | C | F | -
  | 7 | Class.getDeclaredConstructors | 466,646 | 1,013 | C | C | F | -
  | 8 | Class.getDeclaredField | 2,350,808 | 298 | C | C | F | -
  | 9 | Class.getDeclaredFields | 467,522 | 449 | C | C | F | -
  | 10 | Class.getDeclaredMethod | 8,184,703 | 480 | C | C | F | -
  | 11 | Class.getDeclaredMethods | 467,347 | 1,265 | C | C | F | -
  | 12 | Class.getField | 2,359,361 | 1,768 | C | C | F | -
  | 13 | Class.getFields | 470,437 | 880 | C | C | F | -
  | 14 | Class.getMethod | 8,205,417 | 40 | C | C | F | -
  | 15 | Class.getMethods | 467,821 | 160,213 | C | C | F | -
  | 16 | Class.getResource | 1,178,325 | 200 | A | - | F | -
  | 17 | Class.getResourceAsStream | 1,172,325 | 200 | A | - | F | -
  | 18 | Constructor.getAnnotatedParameterTypes | 356,884 | 7,657 | C | C | F | -
  | 19 | Executable.getAnnotatedParameterTypes | 88,702 | 435 | C | C | F | -
  | 20 | Executable.getAnnotations | 177,220 | 41 | O | - | C | C
  | 21 | Executable.getDeclaredAnnotations | 177,412 | 48 | O | - | C | C
  | 22 | Executable.getParameterAnnotations | 177,212 | 7 | O | - | C | C
  | 23 | Field.getAnnotations |603,028|120 | O | - | C | C
  | 24 | Field.getDeclaredAnnotations | 615,960 | 122 | O | - | C | C
  | 25 | Method.getAnnotations | 965,740 | 293 | O | - | C | C
  | 26 | Method.getDeclaredAnnotations | 976,304 | 338 | O | - | C | C
  | 27 | Method.getParameterAnnotations | 963,708 | 45 | O | - | C | C
  | 28 | Method.invoke | 1,468,228 | 240 | D | - | R | -
  | 29 | Package.getImplementationTitle | 59,014 | 117 | C | C | C | -
  | 30 | Parameter.getAnnotatedType | 88,806 | 1,135 | C | C | F | -
  | 31 | Parameter.getParameterizedType | 88,764 | 1,131 | C | C | F | -
  | 32 | Parameter.toString | 88,664 | 1,154 | C | C | F | -

**Table III. Detected Java Reflection API non-conformances. Test Cases: number of test cases executed by our technique calling the method. Failures: number of test cases exposing non-conformance candidate in the method. Status: – = Unreported bug; O = Bug Open; F = Fixed bug; A = Accepted bug; R = Rejected bug; C = Correct result; D = Duplicated bug.**
 
 * Our technique reported false positives for the following methods of Java Reflection API:
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
 * Reported Non-conformance Candidates ([see all](all-reported-non-conformances.md))

 
 | Method with Non-conformance | JVM | Report ID/Bug Tracker URL | Status |
 |:----------:|:---------------|---------------:|---------------:|
 |Class.getAnnotations |	Oracle |	9053675 |	Open |
 |Class.getConstructor |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1837 |	Fixed |
 |Class.getConstructors |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1838 |	Fixed |
 |Class.getDeclaredAnnotations |	Oracle |	https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202652 |	Duplicated |
 |Class.getDeclaredConstructor |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1839 |	Fixed |
 |Class.getDeclaredConstructors |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1840 |	Fixed |
 |Class.getDeclaredField |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1841 |	Fixed |
 |Class.getDeclaredFields |	Eclipse OpenJ9 |	Omitted* |	Fixed |
 |Class.getDeclaredMethod |	Eclipse OpenJ9 |	Omitted* |	Fixed |
 |Class.getDeclaredMethods |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1843 |	Fixed |
 |Class.getField |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1844 |	Fixed |
 |Class.getFields |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1845 |	Fixed |
 |Class.getMethod |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1846 |	Fixed |
 |Class.getMethods |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1847 |	Fixed |
 |Class.getResource |	Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202687 |	Accepted |
 |Class.getResource |	Eclipse OpenJ9 |	https://github.com/eclipse/openj9/issues/1848 |	Accepted |
 |Class.getResourceAsStream |	Oracle  |	https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202687 |	Accepted |
 |Class.getResourceAsStream |	Eclipse OpenJ9	| https://github.com/eclipse/openj9/issues/1849 |	Accepted |
 |Constructor.getAnnotatedParameterTypes |	Eclipse OpenJ9	| Omitted* |	Accepted |
 |Executable.getAnnotatedParameterTypes |	Eclipse OpenJ9	| Omitted* |	Accepted |
 |Executable.getAnnotations |	Oracle |	9053679 |	Open |
 |Executable.getDeclaredAnnotations |	Oracle |	9053680 |	Open |
 |Executable.getParameterAnnotations |	Oracle |	9053682 |	Open |
 |Field.getAnnotations |	Oracle |	9053683 |	Open |
 |Field.getDeclaredAnnotations |	Oracle |	9053684 |	Open |
 |Method.getAnnotations |	Oracle |	9053677 |	Open |
 |Method.getDeclaredAnnotations |	Oracle |	9053678 |	Open |
 |Method.getParameterAnnotations |	Oracle |	9053681 |	Open |
 |Method.invoke |	Oracle	| https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8202689 |	Duplicated |
 |Method.invoke |	Eclipse OpenJ9	| Omitted* |	Duplicated |
 |Parameter.getAnnotatedType |	Eclipse OpenJ9	| Omitted* |	Accepted |
 |Parameter.getParameterizedType |	Eclipse OpenJ9	| Omitted* |	Accepted |
 |Parameter.toString |	Eclipse OpenJ9	| Omitted* |	Accepted |

**Table IV. Reported non-conformance candidates (some links are ommited because of blind review).**
