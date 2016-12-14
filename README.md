####CppUnit 1.5####

Last Revision: 12/15/99 - Michael Feathers (mfeathers@acm.org) - written in standard C++, tested under Microsoft Visual C++ 6.0




####Background####

CppUnit is a simple unit test framework for C++. It is a port from JUnit, a testing framework for Java, developed by Kent Beck and Erich Gamma. 

####Contents####
README.html                     this file
    
    test                        the source code
        framework               the testing framework
		extensions	some framework extension classes 
        textui                  a command line interface to run tests 
    ms                          code for a Microsoft specific TestRunner
    samples                     some example test cases and extensions to the framework
        multicaster             a sample illustrating a publish/subscribe 
				multicaster under test
    doc                         documentation

####Installation####

To use the test framework, create a makefile or load all files in test\framework into your IDE. In this incarnation of CppUnit, all includes assume the current directory first. A makefile or project can be used to resolve the dependencies. 

The directory test\textui contains a simple command line example that uses the framework.

####Documentation####

CppUnit comes with the following documentation: 

* a cookbook: doc\cookbook.htm 
* this file 

####Samples####

You can find several sample test cases in the samples directory: 

* ExampleTestCase - some simple tests 
* Multicaster - test cases for a sample publish/subscribe multicaster class 

Also, the wiki page http://c2.com/cgi/wiki?ClassHierarchyTestingInCppUnit shows how to automatically apply tests of classes to the classes' subclasses.

####Extensions####

You can find several classes that illustrate framework extensions in the extensions directory: 

* TestDecorator - A Decorator for Test. You can use it as the base class for decorators that extend test cases. 
* TestSetup - A Decorator that can be used to set up and tear down additional fixture state. Subclass TestSetup and insert it into your tests when you want to set up additional state once before the test is run. 
* Orthodox - a template class which can be used to verify operations on an arbitrary class.

####Notes####

Porting this framework has been fun. I've tried to maintain the spirit and utility of JUnit in a C++ environment. Naturally, the move from Java to standard C++ forces out several nice JUnit features:

1. Platform independent GUI.
2. Stack traces of test failures
3. Active (threaded) tests
4. Direct invocation of test cases via reflection
5. Run-time loading of new tests

In addition, the lack of garbage collection in C++ requires some careful use of the framework classes. In particular, TestSuites are composites that manage the lifetime of any tests added to them. Holding onto a TestResult past the lifetime of the tests which filled it is a bad idea. This is because TestResults hold TestFailures and TestFailures hold pointers to the Tests that generated them.

On the plus side, we can use the C++ macro preprocessor to get the exact line at which a failure occurs, along with the actual text inside the assert () call that detected the failure. The features of C++ that enable this are the `__LINE__` and `__FILE__` preprocessor definitions, along with the stringizing operator. If you find that generating this much literal text bulks up your test executables, you can use the CPP_UNIT_SOURCEANNOT define to disable that portion of the reporting.

Note: If you use the C++ macro "assert ()" in your code, or include assert.h, you may have a name clash with CppUnit's assert macro. This can be remedied by changing the name of the macro to "cu_assert ()" in TestCase.h.

I'd like to thank Kent Beck and Erich Gamma for the inspiration, design, and a wonderful cookbook that was easily/shamelessly mutated to describe CppUnit. Double thanks to Erich for thinking up a way to implement TestCaller. Additional thanks to Kent, Ward Cunningham, Ron Jeffries, Martin Fowler, and several other netizens of the WikiWikiWeb. I don't think any other bunch of people could have convinced me that rapid development with unit tests can be both effective and easy.

Thanks also to Fred Huls for mentioning the idea of template-based testing. The orthodox template class demonstrates only a small part of what can be done with templated test cases.

####History Of Changes####

1.2 -- Added the TestCaller template class. There is now no need to use the CPP_UNIT_TESTCASEDISPATCH macro unless you are using a C++ compiler which does not support templates well. CPP_UNIT_TESTCASEDISPATCH remains in TestCase.h for backward compatibility. I've also kept the use of the macro in the Multicaster sample to leave in an example.

1.3 -- Retired the CPP_UNIT_TESTCASEDISPATCH macro and cleaned up the include structure. Fixed bug in the textui version.

1.4 -- Removed using directives for std in CppUnit headers. Merged the old AssertionFailedError into CppUnitException. Fixed a memory leak in the TestRunner class of the MS GUI TestRunner. Removed CppUnit.h file. Now headers for each class must be included directly.

1.5 -- Upgraded projects from VC++ 5.0 to 6.0.
